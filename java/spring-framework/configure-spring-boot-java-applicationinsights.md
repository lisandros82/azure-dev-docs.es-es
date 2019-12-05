---
title: Configuración de una aplicación de Spring Boot Initializr para que use Azure Application Insights SpringBoot Starter
description: Configure una aplicación de Spring Boot creada con Spring Initializr para que use Application Insights Spring Boot Starter.
services: Application-Insights
documentationcenter: java
author: dhaval24
ms.author: dhdoshi
ms.date: 11/29/2019
ms.service: azure-monitor
ms.tgt_pltfrm: application-insights
ms.topic: article
ms.openlocfilehash: 083abdf87d2298c99b9898db3b17e1c0e5e64bd8
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812151"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-application-insights"></a>Configuración de una aplicación de Spring Boot Initializr para que use Application Insights

Este artículo le guiará a través de la creación de una aplicación de Spring Boot con **[Spring Initializr]** . Usa Spring Boot Starter de Azure Application Insights para la supervisión integral de las aplicaciones Java en la nube.

## <a name="prerequisites"></a>Requisitos previos

Los siguientes requisitos previos son necesarios para seguir los pasos descritos en este artículo:

* Una suscripción de Azure. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN] o registrarse para obtener una [cuenta de Azure gratuita].
* Un kit de desarrollo de Java (JDK) admitido Para más información sobre los JDK disponibles para desarrollar en Azure, consulte <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versión 3.0 o posterior.
* FluxWeb API y Netty API **no se admiten actualmente** con el iniciador Spring Boot de Application Insights.

## <a name="create-a-custom-application-using-spring-initializr"></a>Creación de una aplicación personalizada mediante Spring Initializr

Cree una aplicación con el procedimiento siguiente.

1. Vaya a [https://start.spring.io/](https://start.spring.io/).

1. Especifique que quiere generar un proyecto de **Maven** con **Java**, escriba los nombres de **Group** (Grupo) y **Artifact** (Artefacto) de su aplicación y seleccione la dependencia web en la sección de dependencias.

   ![Opciones básicas de Spring Initializr][SI01]

   > [!NOTE]
   >
   > Spring Initializr usa los nombres de **Group** (Grupo) y **Artifact** (Artefacto) para crear el nombre del paquete, por ejemplo: *com.vged.appinsights*.
   >

1. Haga clic en el botón **Generate** (Generar).

1. Cuando se le solicite, descargue el proyecto en una ruta de acceso del equipo local.

1. Después de extraer los archivos en el sistema local, la aplicación personalizada de Spring Boot estará lista para editarla.

## <a name="create-an-application-insights-resource-on-azure"></a>Creación de un recurso de Application Insights en Azure

Cree un recurso de Application Insights mediante el procedimiento siguiente.

1. Vaya a Azure en <https://portal.azure.com/> y haga clic en **+ Crear un nuevo recurso**.

1. Haga clic en **Herramientas de TI y administración** y, después haga clic en **Application Insights**.

1. En la página **Nuevo recurso de Application Insights**, escriba la siguiente información:

* Especifique la **Suscripción** y **Grupo de recursos**.
* Escriba el **nombre** del recurso de Application Insights.
* Seleccione la **Región**.

   Cuando haya especificado estas opciones, haga clic en **Revisar y crear**.

   ![Azure][AZ03]

* Revise las especificaciones y haga clic en **Crear**.

Después de crear el recurso, verá que aparece enumerado en el **Panel** de Azure, así como en las páginas **Todos los recursos**. Puede hacer clic en el recurso en cualquiera de esas ubicaciones para abrir la página de información general del recurso de Application Insights.

En la página de información general, copie la **clave de instrumentación**.

   ![Azure][AZ04]

## <a name="configure-your-downloaded-spring-boot-application-to-use-application-insights"></a>Configuración de la aplicación de Spring Boot descargada para que use Application Insights

Configure la aplicación mediante el procedimiento siguiente.

1. Busque el archivo *POM.xml* en el directorio raíz de la aplicación y agregue la siguiente dependencia en la sección de dependencias.

```XML
 <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>
```

1. Busque el archivo *application.properties* en el directorio *resources* de la aplicación, o cree el archivo si todavía no existe.

1. Abra el archivo *application.properties* en un editor de texto y agréguele las siguientes líneas; luego, sustituya los valores de ejemplo por las propiedades adecuadas con las credenciales correctas:

   ```yaml
   # Specify the instrumentation key of your Application Insights resource.
   azure.application-insights.instrumentation-key=[your ikey from the resource]
   # Specify the name of your springboot application. This can be any logical name you would like to give to your app.
   spring.application.name=[your app name]
   ```

   Para conocer más formas de ajustar Application Insights, consulte el [archivo léame de Spring Boot Starter de Application Insights](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).

   > [!NOTE]
   > 
   > Use diferentes claves de instrumentación de Application Insights (como recursos diferentes) para distintos perfiles, como PROD, DEV, etc. Consulte las [propiedades específicas de los perfiles de Spring Boot] para obtener más información. 

1. Guarde y cierre el archivo *application.properties*.

1. Cree una carpeta denominada *controller* en la carpeta de origen para el paquete; por ejemplo:

   `D:\Microsoft\demo\src\main\java\com\example\demo\controller`

   O bien

   `/users/example/home/demo/src/main/java/com/example/demo/controller`

1. Cree un archivo llamado *TestController.java* en la carpeta *controller*. Abra el archivo en un editor de texto y agréguele el siguiente código:

   ```java
    package com.example.demo;

    import com.microsoft.applicationinsights.TelemetryClient;
    import java.io.IOException;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    import com.microsoft.applicationinsights.telemetry.Duration;

    @RestController
    @RequestMapping("/sample")
    public class TestController {

        @Autowired
        TelemetryClient telemetryClient;

        @GetMapping("/hello")
        public String hello() {

            //track a custom event  
            telemetryClient.trackEvent("Sending a custom event...");

            //trace a custom trace
            telemetryClient.trackTrace("Sending a custom trace....");

            //track a custom metric
            telemetryClient.trackMetric("custom metric", 1.0);

            //track a custom dependency
            telemetryClient.trackDependency("SQL", "Insert", new Duration(0, 0, 1, 1, 1), true);

            return "hello";
        }
    }
   ```

   Necesitará reemplazar `com.example.demo` por el nombre del paquete de su proyecto.

1. Guarde y cierre el archivo *TestController.java*.

1. Compile la aplicación de Spring Boot con Maven y ejecútela. Por ejemplo:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Pruebe la aplicación web. Para ello, vaya a http://localhost:8080/sample/hello con un explorador web, o bien utilice una sintaxis como la del siguiente ejemplo si tiene **curl** disponible:

   ```shell
   curl http://localhost:8080/sample/hello
   ```

   Verá el mensaje "Hola" del controlador de ejemplo. Application Insights recopilará esta solicitud automáticamente y la enviará como un elemento de telemetría con la información asociada de evento personalizado, métrica personalizada, dependencia personalizada y seguimiento personalizado, tal y como se especifica en la lógica del controlador. 

   Después de unos segundos, verá los datos en Azure. 

   ![Azure][AZ05]

Haga clic en el icono **Mapa de aplicación** para ver los componentes generales y su interacción entre sí. Este es el lugar recomendado para ver la información general de toda la aplicación. Cada microservicio de Spring Boot se reconoce por el nombre de la aplicación de Spring. Recuerde que debe establecerlo.

   ![Azure][AZ08] 

## <a name="configure-springboot-application-to-send-log4j-logs-to-application-insights"></a>Configuración de la aplicación de Spring Boot para enviar registros log4j a Application Insights

Configure la aplicación para que envíe registros mediante el procedimiento siguiente.

1. Modifique el archivo POM.xml del proyecto y agregue o modifique la sección de dependencias con la siguiente información. 

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-log4j2</artifactId>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-spring-boot-starter</artifactId>
        <version>1.1.1</version>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-logging-log4j2</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```

2. Guarde y cierre el archivo *POM.xml*.

3. En la carpeta \src\main\resources, cree un nuevo archivo *log4j2.xml* y configúrelo. Por ejemplo:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Configuration packages="com.microsoft.applicationinsights.log4j.v2">
  <Properties>
    <Property name="LOG_PATTERN">
      %d{yyyy-MM-dd HH:mm:ss.SSS} %5p ${hostName} --- [%15.15t] %-40.40c{1.} : %m%n%ex
    </Property>
  </Properties>
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
    <ApplicationInsightsAppender name="aiAppender">
    </ApplicationInsightsAppender>
  </Appenders>
  <Loggers>
    <Root level="trace">
      <AppenderRef ref="Console"  />
      <AppenderRef ref="aiAppender"  />
    </Root>
  </Loggers>
</Configuration>
```

4. Vuelva a compilar y ejecutar la aplicación de Spring Boot como se indicó anteriormente.

En pocos segundos, verá que todos los registros de Spring están disponibles en Azure. Puede consultar los mensajes de registro detallados y realizar el análisis en el portal de Analytics.

![Azure][AZ07]

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Spring y Azure, vaya al centro de documentación de Azure.

> [!div class="nextstepaction"]
> [Spring en Azure](/azure/java/spring-framework)

### <a name="additional-resources"></a>Recursos adicionales

Para obtener más información sobre el uso de aplicaciones de Spring Boot en Azure, consulte los siguientes artículos:

* [Implementación de una aplicación de Spring Boot en Azure App Service](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Ejecución de una aplicación de Spring Boot en un clúster de Kubernetes en Azure Container Service](deploy-spring-boot-java-app-on-kubernetes.md)

Application Insights permite recopilar automáticamente las dependencias externas y correlacionarlas con las solicitudes entrantes. Actualmente se admite la recopilación automática de Oracle, MsSQL, MySQL y Redis. Para más información sobre cómo habilitar la recopilación automática, consulte el artículo sobre [cómo usar el agente de Application Insights para Java](/azure/application-insights/app-insights-java-agent).

Para más información sobre Azure Application Insights y sus funcionalidades de supervisión, consulte la página principal de **[Application Insights]** .

Para más información acerca de las opciones de configuración adicionales de Spring Boot Starter de Application Insights, consulte este [vínculo](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md).

Para solicitar características y notificar posibles errores, abra incidencias en nuestro repositorio de [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues).

Para más información sobre el uso de Azure con Java, consulte [Azure para desarrolladores de Java] y [Working with Azure DevOps and Java] (Trabajo con Azure DevOps y Java).

**[Spring Framework]** es una solución de código abierto que ayuda a los desarrolladores de Java a crear aplicaciones de nivel empresarial. Uno de los proyectos más populares que se basa en esa plataforma es [Spring Boot], que proporciona un enfoque simplificado para crear aplicaciones de Java independientes. Para ayudar a los desarrolladores a empezar con Spring Boot, hay varios paquetes de ejemplo de Spring Boot disponibles en [https://github.com/spring-guides/](https://github.com/spring-guides/). Además de elegir de la lista de proyectos básicos de Spring Boot, **[Spring Initializr]** ayuda a los desarrolladores en los primeros pasos para crear aplicaciones de Spring Boot personalizadas.

<!-- URL List -->

[Azure para desarrolladores de Java]: /azure/java/
[cuenta de Azure gratuita]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/ (Trabajo con Azure DevOps y Java)
[ventajas como suscriptor de MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Propiedades específicas de los perfiles de Spring Boot]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Application Insights]: /azure/application-insights/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_2.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_3.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Get_IKey.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/OverviewBladeResults.png
[AZ06]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Search_and_traces.png
[AZ07]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/traces_details.png
[AZ08]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/AppMap.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/spring_start.PNG
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/After_extract.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationproperties_loc.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationinsightsproperties.png
