---
title: Cómo usar Spring y Cosmos DB con App Service en Linux
description: Este artículo le guiará en el proceso de creación, configuración, implementación, solución de problemas y escalado de aplicaciones web de Java en Azure App Service en Linux.
documentationcenter: java
author: bmitchell287
ms.author: brendm
ms.reviewer: joshuapa
ms.date: 4/24/2019
ms.devlang: java
ms.service: cosmos-db
ms.topic: article
ms.openlocfilehash: 7fcd23ad45a591614516fec97e312c71773ce657
ms.sourcegitcommit: 54d34557bb83f52a215bf9020263cb9f9782b41d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74118039"
---
# <a name="how-to-use-spring-and-cosmos-db-with-app-service-on-linux"></a>Cómo usar Spring y Cosmos DB con App Service en Linux

Este artículo le guiará en el proceso de creación, configuración, implementación, solución de problemas y escalado de aplicaciones web de Java en Azure App Service en Linux.

Se mostrará el uso de los siguientes componentes:

- [Uso de Spring Boot Starter con SQL API de Azure Cosmos DB](configure-spring-boot-starter-java-app-with-cosmos-db.md)
- [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction)
- [App Service en Linux](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro)

## <a name="prerequisites"></a>Requisitos previos

Los siguientes requisitos previos son necesarios para seguir los pasos descritos en este artículo:

- Para implementar una aplicación web de Java en la nube, necesita una suscripción de Azure. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o registrarse para obtener una [cuenta de Azure gratuita]((https://azure.microsoft.com/pricing/free-trial/)).
- [CLI de Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Java 8 JDK](https://docs.microsoft.com/azure/java/jdk/java-jdk-install)
- [Maven 3](http://maven.apache.org/)

## <a name="clone-the-sample-java-web-app-repository"></a>Clonación del repositorio de aplicaciones web de Java de ejemplo

Para este ejercicio va a usar la aplicación Todo de Spring, que es una aplicación de Java creada con [Spring Boot](https://spring.io/projects/spring-boot), [Spring Data para Cosmos DB](https://docs.microsoft.com/azure/java/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db) y [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/sql-api-introduction).
1. Clone la aplicación Todo de Spring y copie el contenido de la carpeta **.prep** para inicializar el proyecto:

    Para bash:
    ```bash
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    yes | cp -rf .prep/* .
    ```

    Para Windows:
    ```cmd
    git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
    cd e2e-java-experience-in-app-service-linux-part-2
    xcopy .prep /f /s /e /y
    ```

2. Cambie el directorio a la siguiente carpeta en el repositorio clonado:

   ```bash
   cd initial\spring-todo-app
   ```

## <a name="create-an-azure-cosmos-db-from-azure-cli"></a>Creación de una instancia de Azure Cosmos DB desde la CLI de Azure

En el procedimiento siguiente se crea una base de datos de Azure Cosmos mediante la CLI.

1. Inicie sesión en la CLI de Azure y establezca el identificador de suscripción.

    ```bash
    az login
    ```

2. Si es necesario, establezca el identificador de suscripción.

    ```bash
    az account set -s <your-subscription-id>
    ```

3. Cree un grupo de recursos de Azure y anote el nombre del grupo de recursos para su uso posterior.

    ```bash
    az group create -n <your-azure-group-name> \
    -l <your-resource-group-region>
    ```

4. Cree la instancia de Cosmos DB y especifique el tipo como GlobalDocumentDB.
Solo debe usar letras minúsculas en el nombre de Cosmos DB. Asegúrese de anotar el campo `documentEndpoint` de la respuesta. Lo necesitará más adelante.

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
     ```

5. Obtenga las claves de Azure Cosmos DB y anote el valor de `primaryMasterKey` para su uso posterior.

    ```bash
    az cosmosdb keys list -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="build-and-run-the-app-locally"></a>Compilación y ejecución de la aplicación en un entorno local

En el procedimiento siguiente se ejecuta la aplicación en el equipo de desarrollo.

1. En la consola de su elección, configure las variables de entorno que se muestran en las siguientes secciones de código con la información de conexión de Azure y Cosmos DB que recopiló anteriormente en este artículo. Deberá proporcionar un nombre único para **WEBAPP_NAME** y un valor para las variables **REGION**.

Para Linux (Bash):

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

Para Windows (símbolo del sistema):
```cmd
set COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
set COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
set COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>
set RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
set WEBAPP_NAME=<put-your-Webapp-name-here>
set REGION=<put-your-REGION-here>
```

> [!NOTE]
> Si desea aprovisionar estas variables con un script, hay una plantilla para Bash en el directorio .prep que puede copiar y usar como punto de partida.

2. Cambie el directorio al siguiente:

    ```bash
    cd initial/spring-todo-app
    ``` 
 
3. Ejecute la aplicación Todo de Spring localmente con el siguiente comando:

    ```bash
    mvn package spring-boot:run
    ```

4. Una vez se ha iniciado la aplicación, puede validar la implementación; para ello, acceda a la aplicación Todo de Spring aquí: [http://localhost:8080/](http://localhost:8080/).

 ![Aplicación de Spring que se ejecuta localmente][SCDB01]

## <a name="deploy-to-app-service-linux"></a>Implementación en App Service en Linux

En el procedimiento siguiente se implementa la aplicación en Linux en Azure.

1. Abra el archivo pom.xml que copió anteriormente en el directorio **initial/spring-todo-app** del repositorio. Asegúrese de que se incluye el [complemento de Maven para Azure App Service](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) como se muestra en el archivo pom.xml siguiente. Si la versión no está establecida en la **1.6.0**, actualice el valor.

```xml
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-webapp-maven-plugin</artifactId>
            <version>1.6.0</version>
            <configuration>
            <deploymentType>jar</deploymentType>
            
            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>
            
            <!-- Java Runtime Stack for Web App on Linux-->
            <linuxRuntime>jre8</linuxRuntime>
            
            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property>
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>
            
        </configuration>
    </plugin>            
    ...
</plugins>
```

2. Implementación en Java SE en App Service en Linux

    ```bash
    mvn azure-webapp:deploy
    ```

```bash
// Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.6.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Authenticate with Azure CLI 2.0
[INFO] Target Web App doesnt exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlan11111111-1111-1111'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2018-10-28T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

3. Vaya a la aplicación web que se ejecuta en Java SE en App Service en Linux:

    ```bash
    https://<WEBAPP_NAME>.azurewebsites.net
    ```

![Aplicación de Spring en ejecución en App Service en Linux][SCDB02]

## <a name="troubleshoot-spring-todo-app-on-azure-by-viewing-logs"></a>Solución de problemas de la aplicación Todo de Spring en Azure mediante la visualización de los registros

En el siguiente procedimiento se abren los archivos de registro en Azure.

1. Configure el registro de la aplicación web de Java implementada en Azure App Service en Linux:

    ```bash
    az webapp log config --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME} \
     --web-server-logging filesystem
    ```
2. Abra la secuencia de registro remoto de la aplicación web de Java desde un equipo local:

    ```bash
    az webapp log tail --name ${WEBAPP_NAME} \
     --resource-group ${RESOURCEGROUP_NAME}
     ```

```bash
bash-3.2$ az webapp log tail --name ${WEBAPP_NAME}  --resource-group ${RESOURCEGROUP_NAME}
2018-10-28T22:50:17  Welcome, you are now connected to log-streaming service.
2018-10-28T22:44:56.265890407Z   _____                               
2018-10-28T22:44:56.265930308Z   /  _  \ __________ _________   ____  
2018-10-28T22:44:56.265936008Z  /  /_\  \___   /  |  \_  __ \_/ __ \ 
2018-10-28T22:44:56.265940308Z /    |    \/    /|  |  /|  | \/\  ___/ 
2018-10-28T22:44:56.265944408Z \____|__  /_____ \____/ |__|    \___  >
2018-10-28T22:44:56.265948508Z         \/      \/                  \/ 
2018-10-28T22:44:56.265952508Z A P P   S E R V I C E   O N   L I N U X
2018-10-28T22:44:56.265956408Z Documentation: http://aka.ms/webapp-linux
2018-10-28T22:44:56.266260910Z Setup openrc ...
2018-10-28T22:44:57.396926506Z Service `hwdrivers needs non existent service dev
2018-10-28T22:44:57.397294409Z  * Caching service dependencies ... [ ok ]
2018-10-28T22:44:57.474152273Z Starting ssh service...
...
...
2018-10-28T22:46:13.432160734Z [INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
2018-10-28T22:46:13.744859424Z [INFO] TomcatWebServer - Tomcat started on port(s): 80 (http) with context path ''
2018-10-28T22:46:13.783230205Z [INFO] TodoApplication - Started TodoApplication in 57.209 seconds (JVM running for 70.815)
2018-10-28T22:46:14.887366993Z 2018-10-28 22:46:14.887  INFO 198 --- [p-nio-80-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring FrameworkServlet 'dispatcherServlet'
2018-10-28T22:46:14.887637695Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization started
2018-10-28T22:46:14.998479907Z [INFO] DispatcherServlet - FrameworkServlet 'dispatcherServlet': initialization completed in 111 ms

2018-10-28T22:49:20.572059062Z Sun Oct 28 22:49:20 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:25.850543080Z Sun Oct 28 22:49:25 GMT 2018 DELETE ======= /api/todolist/{4f41ab03-1b12-4131-a920-fe5dfec106ca} ======= 
2018-10-28T22:49:26.047126614Z Sun Oct 28 22:49:26 GMT 2018 GET ======= /api/todolist =======
2018-10-28T22:49:30.201740227Z Sun Oct 28 22:49:30 GMT 2018 POST ======= /api/todolist ======= Milk
2018-10-28T22:49:30.413468872Z Sun Oct 28 22:49:30 GMT 2018 GET ======= /api/todolist =======


```

3. Cuando haya terminado, puede comprobar los resultados con el código de [e2e-java-experience-in-app-service-linux-part-2/complete](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2/tree/master/complete).


## <a name="scale-out-the-spring-todo-app"></a>Escalado horizontal de la aplicación Todo de Spring

Use el procedimiento siguiente para escalar la aplicación.

1. Escale horizontalmente la aplicación web de Java mediante la CLI de Azure:

    ```bash
    az appservice plan update --number-of-workers 2 \
      --name ${WEBAPP_PLAN_NAME} \
      --resource-group ${RESOURCEGROUP_NAME}
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Guía para desarrolladores de Java para App Service en Linux](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-java)
- [Azure para desarrolladores de Java](https://docs.microsoft.com/azure/java/) Para más información acerca de Spring y Azure, vaya a la sección de Spring en el centro de documentación de Azure.

> [!div class="nextstepaction"]
> [Spring en Azure](/azure/java/spring-framework)

### <a name="additional-resources"></a>Recursos adicionales

Para obtener más información sobre el uso de aplicaciones de Spring Boot en Azure, consulte los siguientes artículos:

* [Implementación de una aplicación de Spring Boot en Azure App Service](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Ejecución de una aplicación de Spring Boot en un clúster de Kubernetes en Azure Container Service](deploy-spring-boot-java-app-on-kubernetes.md)

Para más información sobre el uso de Azure con Java, consulte [Azure para desarrolladores de Java] y [Working with Azure DevOps and Java] (Trabajo con Azure DevOps y Java).

**[Spring Framework]** es una solución de código abierto que ayuda a los desarrolladores de Java a crear aplicaciones de nivel empresarial. Uno de los proyectos más populares que se basa en esa plataforma es [Spring Boot], que proporciona un enfoque simplificado para crear aplicaciones de Java independientes. Para ayudar a los desarrolladores a empezar con Spring Boot, puede encontrar varios paquetes de ejemplo de Spring Boot en <https://github.com/spring-guides/>. Además de elegir de la lista de proyectos básicos de Spring Boot, el **[Spring Initializr]** ayuda a los desarrolladores en los primeros pasos para crear aplicaciones de Spring Boot personalizadas.

<!-- URL List -->

[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[Azure para desarrolladores de Java]: /azure/java/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Boot Document DB Starter for Azure]:https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: https://azure.microsoft.com/services/devops/java/ (Trabajo con Azure DevOps y Java)
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SCDB01]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB01.png
[SCDB02]: ./media/configure-spring-app-with-cosmos-db-on-app-service-linux/SCDB02.png
