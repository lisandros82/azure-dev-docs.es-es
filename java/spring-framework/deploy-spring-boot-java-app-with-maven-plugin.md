---
title: Implementación de una aplicación de archivo JAR de Spring Boot en Azure con Maven
description: Obtenga información acerca de cómo implementar una aplicación de Spring Boot en la nube con el complemento de Maven para Azure Web Apps para Linux.
services: app-service
documentationcenter: java
author: bmitchell287
manager: douge
editor: brborges
ms.author: brendm
ms.date: 12/19/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: f093e7f23a15420a60b6725e0f13d8457478ab5c
ms.sourcegitcommit: ad1b12d9ebb6113991ce48255f5b491364490079
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842231"
---
# <a name="deploy-a-spring-boot-jar-file-app-to-azure-app-service-with-maven-and-azure-on-linux"></a>Implementación de una aplicación de archivo JAR de Spring Boot en Azure App Service con Maven y Azure en Linux

En este inicio rápido se muestra cómo usar el [complemento de Maven para Azure App Service Web Apps](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme) para implementar una aplicación de Spring Boot empaquetada como un archivo JAR de Java SE en [Azure App Service en Linux](/azure/app-service/containers/). Elegirá la implementación de Java SE en lugar de [Tomcat y archivos WAR](/azure/app-service/containers/quickstart-java) cuando desee consolidar las dependencias, el entorno de ejecución y la configuración de la aplicación en un único artefacto implementable.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para realizar los pasos de este tutorial, necesitará tener instalado y configurado lo siguiente:

* La [CLI de Azure](/cli/azure/), ya sea localmente o con [Azure Cloud Shell](https://shell.azure.com).
* Un kit de desarrollo de Java (JDK) admitido Para más información sobre los JDK disponibles para desarrollar en Azure, consulte <https://aka.ms/azure-jdks>.
* [Maven](https://maven.apache.org/) de Apache, versión 3.
* Un cliente [Git](https://git-scm.com/downloads).

## <a name="install-and-sign-in-to-azure-cli"></a>Instalación e inicio de sesión en la CLI de Azure

La manera más sencilla y fácil de obtener el complemento Maven para implementar la aplicación de Spring Boot es con la [CLI de Azure](/cli/azure/).

Inicie sesión en la cuenta de Azure mediante la CLI de Azure:
   
   ```shell
   az login
   ```
   
Siga las instrucciones para completar el proceso de inicio de sesión.

## <a name="clone-the-sample-app"></a>Clonación de la aplicación de ejemplo

En esta sección, va a clonar una aplicación de Spring Boot terminada y a probarla de forma local.

1. Abra el símbolo del sistema o una ventana de terminal, cree un directorio local para alojar la aplicación de Spring Boot y cambie a dicho directorio, por ejemplo:
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- o --
   ```shell
   mkdir ~/SpringBoot
   cd ~/SpringBoot
   ```

1. Clone el proyecto de ejemplo [Primeros pasos de Spring Boot] en el directorio que creó, por ejemplo:
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. Cambie de directorio al del proyecto finalizado, por ejemplo:
   ```shell
   cd gs-spring-boot/complete
   ```

1. Compile el archivo JAR con Maven, por ejemplo:
   ```shell
   mvn clean package
   ```

1. Cuándo se haya creado la aplicación web, iníciela con Maven. Por ejemplo:
   ```shell
   mvn spring-boot:run
   ```

1. Pruebe la aplicación web. Para ello, navegue a ella localmente mediante un explorador web. Por ejemplo, puede utilizar el siguiente comando si dispone de curl:
   ```shell
   curl http://localhost:8080
   ```

1. Debería ver el mensaje siguiente mostrado: **Greetings from Spring Boot!**

## <a name="configure-maven-plugin-for-azure-app-service"></a>Configuración del complemento de Maven para Azure App Service

En esta sección, configurará el proyecto de Spring Boot `pom.xml` para que Maven pueda implementar la aplicación en Azure App Service en Linux.

1. Abra `pom.xml` en un editor de código.

2. En la sección `<build>` del archivo pom.xml, agregue la siguiente entrada `<plugin>` dentro de la etiqueta `<plugins>`.

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.8.0</version>
   </plugin>
   ```

3. Después, puede configurar la implementación, ejecutar el siguiente comando de Maven en el símbolo del sistema y usar el **número** para elegir estas opciones en el símbolo del sistema:
    * **OS**: linux  
    * **javaVersion**: Java 8    
    
```cmd
mvn azure-webapp:config
```

Cuando llegue al mensaje **Confirm (Y/N)** [Confirmar (S/N)], presione **y** y la configuración se realiza.

```cmd
~@Azure:~/gs-spring-boot/complete$ mvn azure-webapp:config
[INFO] Scanning for projects...
[INFO]
[INFO] -----------------< org.springframework:gs-spring-boot >-----------------
[INFO] Building gs-spring-boot 0.1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
[INFO] --- azure-webapp-maven-plugin:1.8.0:config (default-cli) @ gs-spring-boot ---
[WARNING] The plugin may not work if you change the os of an existing webapp.
Define value for OS(Default: Linux):
1. linux [*]
2. windows
3. docker
Enter index to use:
Define value for javaVersion(Default: Java 8):
1. Java 11
2. Java 8 [*]
Enter index to use:
Please confirm webapp properties
AppName : gs-spring-boot-1559091271202
ResourceGroup : gs-spring-boot-1559091271202-rg
Region : westeurope
PricingTier : Premium_P1V2
OS : Linux
RuntimeStack : JAVA 8-jre8
Deploy to slot : false
Confirm (Y/N)? : Y
```

4. Agregue la sección `<appSettings>` a la sección `<configuration>` de `<azure-webapp-maven-plugin>` para escuchar en el puerto *80*.

    ```xml
   <plugin>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.8.0</version>
       <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>gs-spring-boot-1559091271202-rg</resourceGroup>
          <appName>gs-spring-boot-1559091271202</appName>
          <region>westeurope</region>
          <pricingTier>P1V2</pricingTier>
          <runtime>
            <os>linux</os>
            <javaVersion>jre8</javaVersion>
            <webContainer>jre8</webContainer>
          </runtime>
          <!-- Begin of App Settings  -->
          <appSettings>
             <property>
                   <name>JAVA_OPTS</name>
                   <value>-Dserver.port=80</value>
             </property>
          </appSettings>
          <!-- End of App Settings  -->
          <deployment>
            <resources>
              <resource>
                <directory>${project.basedir}/target</directory>
                <includes>
                  <include>*.jar</include>
                </includes>
              </resource>
            </resources>
          </deployment>
         </configuration>
   </plugin>
   ```

## <a name="deploy-the-app-to-azure"></a>Implementación de la aplicación en Azure

Una vez que haya configurado todas las opciones en las secciones anteriores de este artículo, estará listo para implementar la aplicación web en Azure. Para ello, siga estos pasos:

1. En el símbolo del sistema o la ventana de terminal que usó anteriormente, vuelva a compilar el archivo JAR con Maven si ha realizado algún cambio en el archivo *pom.xml*; por ejemplo:
   ```shell
   mvn clean package
   ```

1. Implemente la aplicación web en Azure mediante Maven; por ejemplo:
   ```shell
   mvn azure-webapp:deploy
   ```

Maven implementará la aplicación web en Azure; si la aplicación web o el plan de la aplicación web no existen, se crearán.

Cuando se haya implementado la web, podrá administrarla mediante [Azure Portal].

* La aplicación web aparecerá en **App Services**:

   ![Aplicación web en la lista de App Services de Azure Portal][AP01]

* Y la dirección URL de la aplicación web se mostrará en la sección de **información general** de la aplicación web:

   ![Busque la dirección URL de la aplicación web en Azure Portal App Services][AP02]

Compruebe que la implementación se realizó correctamente mediante el mismo comando de cURL utilizado antes, utilizando la dirección URL de la aplicación web del portal en lugar de `localhost`. Debería ver el mensaje siguiente mostrado: **Greetings from Spring Boot!** 

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

- En Azure Portal, seleccione el grupo de recursos en el menú de la izquierda.
- Escriba **gs-spring-boot-** en el campo **Filtrar por nombre**; el grupo de recursos creado en este tutorial debe tener este prefijo.
- Seleccione el grupo de recursos que creó en este tutorial.
- Seleccione Eliminar grupo de recursos en el menú superior.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Spring y Azure, vaya al centro de documentación de Azure.

> [!div class="nextstepaction"]
> [Spring en Azure](/azure/java/spring-framework)

### <a name="additional-resources"></a>Recursos adicionales

Para más información acerca de las diferentes tecnologías que se tratan en este artículo, consulte los artículos siguientes:

* [Complemento Maven de Azure Web Apps]

* [Uso del complemento Maven de Azure Web Apps para implementar una aplicación de Spring Boot en contenedor en Azure](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [Creación de una entidad de servicio de Azure con la CLI de Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Referencia de configuración de Maven](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /azure/java/
[Azure Portal]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Primeros pasos de Spring Boot]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[Complemento Maven de Azure Web Apps]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->


[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/web-app-listed-azure-portal.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/determine-web-app-url.png
