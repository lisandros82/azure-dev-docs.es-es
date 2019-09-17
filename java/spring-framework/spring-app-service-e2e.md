---
title: Implementación de una aplicación Spring/Tomcat en App Service con Azure Database for MySQL
description: Tutorial completo para Java App Service con MySQL
author: KarlErickson
manager: barbkess
ms.author: karler
ms.date: 08/38/2019
ms.service: app-service
ms.devlang: java
ms.topic: article
ms.openlocfilehash: 0a35692d44b4c0a3d587c11e8781f3a9a3d141c9
ms.sourcegitcommit: 8d9b68a214a4b92b722df9e71afca5bc6827ce9b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383840"
---
# <a name="deploy-a-spring-app-to-app-service-with-mysql"></a>Implementación de una aplicación Spring en App Service con MySQL

Este tutorial le guiará en el proceso de creación, configuración, implementación, solución de problemas y escalado de aplicaciones web Java en App Service en Linux.

Este tutorial se basa en la popular aplicación de ejemplo Spring PetClinic. En este tema, probará una versión HSQLDB de la aplicación localmente y, a continuación, la implementará en [Azure App Service](/azure/app-service/containers). Después, configurará e implementará una versión que usa [Azure Database for MySQL](/azure/mysql). Por último, aprenderá a acceder a los registros de la aplicación y a escalar horizontalmente aumentando el número de trabajadores que ejecutan la aplicación.

## <a name="prerequisites"></a>Requisitos previos

* [CLI de Azure](http://docs.microsoft.com/cli/azure/overview)
* [Java 8](http://java.oracle.com/)
* [Maven 3](http://maven.apache.org/)
* [Git](https://github.com/)
* [Tomcat](https://tomcat.apache.org/download-80.cgi)
* [CLI para MySQL](http://dev.mysql.com/downloads/mysql/)

## <a name="get-the-sample"></a>Obtener la aplicación de ejemplo

Para empezar a trabajar con la aplicación de ejemplo, clone y prepare el repositorio de código fuente con los siguientes comandos.

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux.git
cd e2e-java-experience-in-app-service-linux
yes | cp -rf .prep/* .
```

## <a name="build-and-run-the-hsqldb-sample-locally"></a>Compilar y ejecutar el ejemplo HSQLDB localmente

En primer lugar, probaremos el ejemplo localmente usando HSQLDB como base de datos.

Vaya a la versión HSQLDB del ejemplo y compílelo.

```bash
cd initial-hsqldb/spring-framework-petclinic
mvn package
```

A continuación, establezca la variable de entorno TOMCAT_HOME en la ubicación de su instalación de Tomcat.

```bash
export TOMCAT_HOME=<Tomcat install directory>
```

Después, actualice el archivo *pom.xml* para configurar Maven para una implementación de archivos WAR de Tomcat. Agregue el siguiente código XML como nodo secundario del elemento `<plugins>`. Si es necesario, cambie `1.7.7` a la versión actual del [Complemento Cargo Maven 2](https://mvnrepository.com/artifact/org.codehaus.cargo/cargo-maven2-plugin).

```xml
<plugin>
    <groupId>org.codehaus.cargo</groupId>
    <artifactId>cargo-maven2-plugin</artifactId>
    <version>1.7.7</version>
    <configuration>
        <container>
            <containerId>tomcat8x</containerId>
            <type>installed</type>
            <home>${TOMCAT_HOME}</home>
        </container>
        <configuration>
            <type>existing</type>
            <home>${TOMCAT_HOME}</home>
        </configuration>
        <deployables>
            <deployable>
                <groupId>${project.groupId}</groupId>
                <artifactId>${project.artifactId}</artifactId>
                <type>war</type>
                <properties>
                    <context>/</context>
                </properties>
            </deployable>
        </deployables>
    </configuration>
</plugin>
```

Una vez establecida esta configuración, puede implementar la aplicación localmente en Tomcat.

```bash
mvn cargo:deploy
```

A continuación, inicie Tomcat.

```bash
${TOMCAT_HOME}/bin/catalina.sh run
```

Use el explorador para ir a [http://localhost:8080](http://localhost:8080), para ver la aplicación en ejecución y hacerse una idea de cómo funciona. Cuando haya terminado, seleccione Ctrl+C en el símbolo del sistema de Bash para detener Tomcat.

## <a name="deploy-to-azure-app-service"></a>Implementación en Azure App Service

Ahora que la ha visto ejecutándose localmente, implementaremos la aplicación en Azure.

Primero, agregue estas variables de entorno:

```bash
export RESOURCEGROUP_NAME=<resource group>
export WEBAPP_NAME=<web app>
export WEBAPP_PLAN_NAME=${WEBAPP_NAME}-appservice-plan
export REGION=<region>
```

Maven usará estos valores para crear los recursos de Azure con los nombres que proporcione. Los secretos de la cuenta se pueden mantener fuera de los archivos del proyecto con variables de entorno.

Después, actualice el archivo *pom.xml* para configurar Maven para una implementación de Azure. Agregue el siguiente código XML después del elemento `<plugin>` que agregó anteriormente. Si es necesario, cambie `1.7.0` a la versión actual del [Complemento Maven para Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme).

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.7.0</version>
    <configuration>
        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>
        <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>
    </configuration>
</plugin>
```

Inicie de sesión en Azure.

```azurecli
az login
```

Después, implemente la aplicación en App Service en Linux.

```bash
mvn azure-webapp:deploy
```

Ahora puede ir a `https://<app-name>.azurewebsites.net` (después de reemplazar `<app-name>`) para ver la aplicación en ejecución.

## <a name="set-up-azure-database-for-mysql"></a>Configuración de Azure Database for MySQL

A continuación, vamos a usar MySQL en lugar de HSQLDB. Vamos a crear una instancia del servidor de MySQL en Azure y agregaremos una base de datos. Luego actualizaremos la configuración de la aplicación con la nueva información de conexión de base de datos.

Primero, agregue estas variables de entorno que usaremos más adelante:

```bash
export MYSQL_SERVER_NAME=<server>
export MYSQL_SERVER_FULL_NAME=${MYSQL_SERVER_NAME}.mysql.database.azure.com
export MYSQL_SERVER_ADMIN_LOGIN_NAME=<admin>
export MYSQL_SERVER_ADMIN_PASSWORD=<password>
export MYSQL_DATABASE_NAME=<database>
export DOLLAR=\$
```

A continuación, cree e inicialice el servidor de base de datos. Use [az mysql up](/cli/azure/ext/db-up/mysql?view=azure-cli-latest#ext-db-up-az-mysql-up) para la configuración inicial. Use [az mysql server configuration set](/cli/azure/mysql/server/configuration?view=azure-cli-latest#az-mysql-server-configuration-set) para aumentar el tiempo de espera de la conexión y establecer la zona horaria del servidor.

```azurecli
az mysql up \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server-name ${MYSQL_SERVER_NAME} \
    --database-name ${MYSQL_DATABASE_NAME} \
    --admin-user ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
    --admin-password ${MYSQL_SERVER_ADMIN_PASSWORD}

az mysql server configuration set --name wait_timeout \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value 2147483

az mysql server configuration set --name time_zone \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value -8:00
```

Use la CLI de MySQL para crear la base de datos.

```bash
mysql -u ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
 -h ${MYSQL_SERVER_FULL_NAME} -P 3306 -p
```

En el símbolo del sistema de la CLI de MySQL, ejecute el siguiente comando, reemplazando `<database name>` por el mismo valor que especificó anteriormente para la variable de entorno `MYSQL_DATABASE_NAME`.

```console
CREATE DATABASE <database name>;
```

MySQL ya está listo para su uso.

## <a name="configure-the-app-for-mysql"></a>Configuración de la aplicación para MySQL

A continuación, agregaremos la información de conexión a la versión de MySQL de la aplicación y la implementaremos en App Service.

En primer lugar, vaya a la carpeta correcta en el símbolo del sistema de Bash.

```bash
cd ../../initial-mysql/spring-framework-petclinic
```

A continuación, actualice el archivo *pom.xml* para que MySQL sea la configuración activa. Quite el elemento `<activation>` del perfil de HSQLDB y colóquelo en el perfil de MySQL, como se muestra aquí. El resto del fragmento de código muestra la configuración existente. Observe cómo Maven usa las variables de entorno establecidas anteriormente para configurar el acceso de MySQL.

```xml
<profile>
    <id>MySQL</id>
    <activation>
        <activeByDefault>true</activeByDefault>
    </activation>
    <properties>
        <db.script>mysql</db.script>
        <jpa.database>MYSQL</jpa.database>
        <jdbc.driverClassName>com.mysql.jdbc.Driver</jdbc.driverClassName>
        <jdbc.url>jdbc:mysql://${DOLLAR}{MYSQL_SERVER_FULL_NAME}:3306/${DOLLAR}{MYSQL_DATABASE_NAME}?useUnicode=true</jdbc.url>
        <jdbc.username>${DOLLAR}{MYSQL_SERVER_ADMIN_LOGIN_NAME}</jdbc.username>
        <jdbc.password>${DOLLAR}{MYSQL_SERVER_ADMIN_PASSWORD}</jdbc.password>
    </properties>
    ...
</profile>
```

Después, actualice el archivo *pom.xml* para configurar Maven para una implementación de Azure y para MySQL. Agregue el siguiente código XML después del elemento `<plugin>` que agregó anteriormente. Si es necesario, cambie `1.7.0` a la versión actual del [Complemento Maven para Azure App Service](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme).

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.7.0</version>
    <configuration>

        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>

        <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>

        <appSettings>
            <property>
                <name>MYSQL_SERVER_FULL_NAME</name>
                <value>${MYSQL_SERVER_FULL_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_LOGIN_NAME</name>
                <value>${MYSQL_SERVER_ADMIN_LOGIN_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_PASSWORD</name>
                <value>${MYSQL_SERVER_ADMIN_PASSWORD}</value>
            </property>
            <property>
                <name>MYSQL_DATABASE_NAME</name>
                <value>${MYSQL_DATABASE_NAME}</value>
            </property>
        </appSettings>

    </configuration>
</plugin>
```

Después, compile la aplicación, impleméntela y ejecútela con Tomcat para probarla.

```bash
mvn package
mvn cargo:deploy
${TOMCAT_HOME}/bin/catalina.sh run
```

Ahora puede ver la aplicación localmente en [http://localhost:8080](http://localhost:8080). La aplicación tendrá el mismo aspecto que antes, pero usará Azure Database for MySQL en lugar de HSQLDB. Cuando haya terminado, seleccione Ctrl+C en el símbolo del sistema de Bash para detener Tomcat.

Por último, implemente la aplicación en App Service.

```bash
mvn azure-webapp:deploy
```

Vaya a `https://<app-name>.azurewebsites.net` para ver la aplicación en ejecución con App Service y Azure Database for MySQL.

## <a name="access-the-app-logs"></a>Acceso a los registros de la aplicación

Si necesita solucionar algún problema, puede consultar los registros de la aplicación. Para abrir el flujo de registros remoto en el equipo local, use el siguiente comando.

```azurecli
az webapp log tail --name ${WEBAPP_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

Cuando haya terminado de ver los registros, seleccione Ctrl+C para detener el flujo.

El flujo de registros también está disponible en `https://<app-name>.scm.azurewebsites.net/api/logstream`.

## <a name="scale-out"></a>Escalado horizontal

Para admitir un aumento del tráfico hacia la aplicación, puede escalar horizontalmente a varias instancias mediante el siguiente comando.

```azurecli
az appservice plan update --number-of-workers 2 \
    --name ${WEBAPP_PLAN_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

Felicidades. Ha creado y escalado horizontalmente una aplicación web Java con Spring Framework, JSP, Spring Data, Hibernate, JDBC, App Service en Linux y Azure Database for MySQL.

## <a name="clean-up-resources"></a>Limpieza de recursos

En las secciones anteriores, creó recursos de Azure en un grupo de recursos. Si prevé que no necesitará estos recursos en el futuro, elimine el grupo de recursos ejecutando el siguiente comando.

```azurecli
az group delete --name ${RESOURCEGROUP_NAME}
```

## <a name="next-steps"></a>Pasos siguientes

Consulte las demás opciones de configuración y de CI/CD disponibles para Java con App Service.

> [!div class="nextstepaction"]
> [Configuración de una aplicación Java en Linux para Azure App Service](/azure/app-service/containers/configure-language-java)
> [!div class="nextstepaction"]
> [Creación e implementación de una aplicación web Java mediante Azure Pipelines](/azure/devops/pipelines/ecosystems/java-webapp?view=azure-devops&tabs=java-tomcat)
> [!div class="nextstepaction"]
> [Implementación en Azure App Service mediante el complemento de Jenkins](/azure/jenkins/deploy-jenkins-app-service-plugin)
