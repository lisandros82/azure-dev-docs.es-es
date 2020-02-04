---
title: Migración de aplicaciones de Java SE a Java SE en Azure App Service
description: En esta guía se describe lo que hay que tener en cuenta para migrar una aplicación de Spring Boot existente u otra aplicación web independiente a Azure App Service mediante Java SE.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: 5a9f6838e516b6168be40c83ea1ff4329676e6e3
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830713"
---
# <a name="migrate-executable-jar-web-applications-to-java-se-on-azure-app-service"></a>Migración de aplicaciones web JAR ejecutables a Java SE en Azure App Service

En esta guía se describe lo que hay que tener en cuenta para migrar una aplicación de Spring Boot existente u otra aplicación web insertada a Azure App Service mediante Java SE.

## <a name="before-you-start"></a>Antes de comenzar

Si no puede cumplir alguno de los requisitos previos a la migración, consulte las siguientes guías de migración complementarias:

* Migración de aplicaciones JAR ejecutables a contenedores en Azure Kubernetes Service (planeada)
* Migración de aplicaciones JAR ejecutables a Azure Virtual Machines (planeada)

## <a name="pre-migration-steps"></a>Pasos previos a la migración

### <a name="switch-to-a-supported-platform"></a>Cambio a una plataforma compatible

App Service ofrece versiones específicas de Java SE. Para garantizar la compatibilidad, migre la aplicación a una de las versiones admitidas de su entorno actual antes de continuar con cualquiera de los pasos restantes. Asegúrese de probar por completo la configuración resultante. Use la versión estable más reciente de la distribución de Linux en dichas pruebas.

[!INCLUDE [note-obtain-your-current-java-version](includes/migration/note-obtain-your-current-java-version.md)]

### <a name="inventory-external-resources"></a>Recursos externos de inventario

Identifique los recursos externos, tales como los orígenes de datos, los agentes de mensajes JMS y las direcciones URL de otros servicios. En las aplicaciones de Spring Boot, normalmente encontrará la configuración de estos recursos en *src/main/directory*, en un archivo llamado normalmente *application.properties* o *application.yml*. Además, compruebe las variables de entorno de la implementación de producción para obtener las opciones de configuración pertinentes.

#### <a name="databases"></a>Bases de datos

Identifique la cadena de conexión de todas las bases de datos SQL.

En el caso de una aplicación de Spring Boot, las cadenas de conexión suelen aparecer en los archivos de configuración. 

A continuación se muestra un ejemplo de un archivo *application.properties*:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mysql_db
spring.datasource.username=dbuser
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

A continuación se muestra un ejemplo de un archivo *application.yaml*:

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://mongouser:deepsecret@mongoserver.contoso.com:27017
```

#### <a name="jms-message-brokers"></a>Agentes de mensajes de JMS

Identifique qué agentes se están usando. Para ello, examine las dependencias pertinentes en el manifiesto de compilación (normalmente *pom.xml* o *build.gradle*).

Por ejemplo, una aplicación de Spring Boot con ActiveMQ normalmente contendría esta dependencia en *pom.xml*:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
```

Las aplicaciones de Spring Boot que usan agentes exclusivos suelen contener las dependencias directamente en las bibliotecas de controladores de JMS de los agentes. A continuación se muestra un ejemplo de un archivo *build.gradle*:

```json
    dependencies {
      ...
      compile("com.ibm.mq:com.ibm.mq.allclient:9.0.4.0")
      ...
    }
```

Una vez identificados los agentes que se están usando, busque los valores correspondientes, que suelen encontrarse en los archivos *application.properties* y *application.yml* de Spring Boot.

A continuación se muestra un ejemplo de un archivo *application.properties*:

```properties
spring.activemq.brokerurl=broker:(tcp://localhost:61616,network:static:tcp://remotehost:61616)?persistent=false&useJmx=true
spring.activemq.user=admin
spring.activemq.password=tryandguess
```

A continuación se muestra un ejemplo de un archivo *application.yaml*:

```yaml
ibm:
  mq:
    queueManager: qm1
    channel: dev.ORDERS
    connName: localhost(14)
    user: admin
    password: big$ecr3t
```

#### <a name="all-other-external-resources"></a>Todos los demás recursos externos

No es factible documentar todas las dependencias externas posibles en esta guía. Es responsabilidad del equipo comprobar que puede cumplir todas las dependencias externas de la aplicación después de la migración de un servicio de aplicación.

### <a name="inventory-secrets"></a>Secretos de inventario

#### <a name="passwords-and-secure-strings"></a>Contraseñas y cadenas seguras

Compruebe si hay cadenas secretas y contraseñas en todas las propiedades, los archivos de configuración y las variables de entorno de las implementaciones de producción. En una aplicación de Spring Boot, es probable que estas cadenas se encuentren en *application.properties* o *application.yml*.

[!INCLUDE [inventory-certificates](includes/migration/inventory-certificates.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

### <a name="special-cases"></a>Casos especiales

Algunos escenarios de producción pueden requerir otros cambios o imponer limitaciones adicionales. Aunque estos escenarios no son frecuentes, es importante asegurarse de que no son aplicables a la aplicación o que se han resuelto correctamente.

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>Determinación de si la aplicación se basa en trabajos programados

Los trabajos programados, como las tareas del programador de Quartz o los trabajos de cron, no se pueden usar con App Service. App Service no le impedirá implementar internamente una aplicación que contenga tareas programadas. Sin embargo, si la aplicación se escala horizontalmente, un mismo trabajo programado se podría ejecutar más de una vez por cada período programado. Esta situación puede tener consecuencias no deseadas.

Haga un inventario de todos los trabajos programados, dentro o fuera del proceso de las aplicaciones.

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>Determinación de si la aplicación contiene código específico del sistema operativo

Si la aplicación contiene código adaptado al sistema operativo en el que se ejecuta la aplicación, es necesario refactorizar la aplicación para que no dependa del sistema operativo subyacente. Por ejemplo, los usos de `/` o `\` en las rutas de acceso del sistema de archivos tendrán que reemplazarse por [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) o [`Path.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-).

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>Identificación de todos los procesos externos o demonios en ejecución en los servidores de producción

Tendrá que migrar a otra parte o eliminar los procesos que se ejecuten fuera del servidor de aplicaciones, como los demonios de supervisión.

#### <a name="identify-handling-of-non-http-requests-or-multiple-ports"></a>Identificación del control de solicitudes que no son HTTP o de varios puertos

App Service solo admite un punto de conexión HTTP en un solo puerto. Si la aplicación escucha en varios puertos o acepta solicitudes mediante protocolos distintos de HTTP, no utilice Azure App Service.

## <a name="migration"></a>Migración

### <a name="parameterize-the-configuration"></a>Parametrización de la configuración

Asegúrese de que todas las coordenadas de recursos externos (como las cadenas de conexión de base de datos) y otras configuraciones personalizables se pueden leer desde las variables de entorno. Si va a migrar una aplicación de Spring Boot, toda la configuración debe ser ya [externalizable](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config).

Este es un ejemplo que hace referencia a una variable de entorno de `SERVICEBUS_CONNECTION_STRING` desde un archivo *application.properties*:

```properties
spring.jms.servicebus.connection-string=${SERVICEBUS_CONNECTION_STRING}
spring.jms.servicebus.topic-client-id=contoso1
spring.jms.servicebus.idle-timeout=10000
```

### <a name="provision-an-app-service-plan"></a>Aprovisionamiento de un plan de App Service

En la [lista de planes de servicio disponibles](https://azure.microsoft.com/pricing/details/app-service/linux/), seleccione el plan cuyas especificaciones sean igual o superiores a las del hardware de producción actual.

> [!NOTE]
> Si planea ejecutar implementaciones de ensayo o controladas, o usar [espacios de implementación](/azure/app-service/deploy-staging-slots), el plan de App Service debe incluir esa funcionalidad adicional. Se recomienda usar planes Premium o superiores para las aplicaciones de Java.

[Cree el plan de App Service](/azure/app-service/app-service-plan-manage#create-an-app-service-plan).

### <a name="create-and-deploy-web-apps"></a>Creación e implementación de aplicaciones web

Tendrá que crear una aplicación web en el plan de App Service (con "Java SE" como pila en tiempo de ejecución) para cada archivo JAR ejecutable que quiera ejecutar.

#### <a name="maven-applications"></a>Aplicaciones de Maven

Si la aplicación se ha creado a partir de un archivo POM de Maven, [usaeel complemento Webapp para Maven](/azure/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin#configure-maven-plugin-for-azure-app-service) para crear la aplicación web e implementarla.

#### <a name="non-maven-applications"></a>Aplicaciones que no son de Maven

Si no puede usar el complemento de Maven, debe aprovisionar la aplicación web con otros mecanismos, como:

* [Azure Portal](https://portal.azure.com/#create/Microsoft.WebSite)
* [CLI de Azure](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

Una vez creada la aplicación web, use uno de los mecanismos de implementación [disponibles](/azure/app-service/deploy-ftp) para implementar la aplicación. Si es posible, la aplicación debe cargarse en */home/site/wwwroot/app.jar*. Si no quiere cambiar el nombre de archivo JAR a app.jar, puede cargar un script de shell con el comando para ejecutar el archivo JAR. A continuación, pegue la ruta de acceso completa a este script en el cuadro de texto[Archivo de inicio](/azure/app-service/containers/app-service-linux-faq#built-in-images) de la sección Configuración del portal. El script de inicio no se ejecuta desde el directorio en el que se encuentra. Por lo tanto, use siempre rutas de acceso absolutas para hacer referencia a los archivos del script de inicio (por ejemplo: `java -jar /home/myapp/myapp.jar`).

### <a name="migrate-jvm-runtime-options"></a>Migración de las opciones de tiempo de ejecución de JVM

Si la aplicación requiere opciones de tiempo de ejecución específicas, [use el mecanismo más adecuado para especificarlas](/azure/app-service/containers/configure-language-java#set-java-runtime-options).

[!INCLUDE [configure-custom-domain-and-ssl](includes/migration/configure-custom-domain-and-ssl.md)]

[!INCLUDE [import-backend-certificates](includes/migration/import-backend-certificates.md)]

### <a name="migrate-external-resource-coordinates-and-other-settings"></a>Migración de las coordenadas de recursos externos y otras opciones

Siga [estos pasos para migrar las cadenas de conexión y otras opciones de configuración](/azure/app-service/containers/configure-language-java#spring-boot-1).

> [!NOTE]
> En el caso de las opciones de configuración de la aplicación de Spring Boot con variables en la sección [Parametrización de la configuración](#parameterize-the-configuration), esas variables de entorno deben definirse en la configuración de la aplicación. La configuración de la aplicación de Spring Boot no parametrizada explícitamente con variables de entorno se pueden invalidar con la configuración de la aplicación. Por ejemplo:

  ```properties
  spring.jms.servicebus.connection-string=${CUSTOMCONNSTR_SERVICE_BUS}
  spring.jms.servicebus.topic-client-id=contoso1
  spring.jms.servicebus.idle-timeout=1800000
  ```

![Configuración de aplicaciones de App Service](media/migrate-java-se-to-java-se-app-service/app-service-parameterized-spring-boot-app-settings.png)

[!INCLUDE [migrate-scheduled-jobs](includes/migration/migrate-scheduled-jobs.md)]

### <a name="restart-and-smoke-test"></a>Reinicio y prueba de humo

Por último, tendrá que reiniciar la aplicación web para aplicar todos los cambios de la configuración. Tras completar el reinicio, compruebe que la aplicación se está ejecutando correctamente.

## <a name="post-migration-steps"></a>Pasos posteriores a la migración

Ahora que ha migrado la aplicación a Azure App Service, debe comprobar que funciona como se espera. Una vez hecho esto, tenemos algunas recomendaciones que pueden hacer que su aplicación sea más nativa de la nube.

### <a name="recommendations"></a>Recomendaciones

* Si ha optado por usar el directorio */home* para el almacenamiento de archivos, considere la posibilidad de [reemplazarlo por Azure Storage](/azure/app-service/containers/how-to-serve-content-from-azure-storage).

* Si tiene una configuración en el directorio */home* que contiene cadenas de conexión, claves SSL y otra información secreta, considere la posibilidad de usar [Azure Key Vault](/azure/app-service/app-service-key-vault-references) y una [inyección de parámetros con la configuración de la aplicación](/azure/app-service/configure-common#configure-app-settings) siempre que sea posible.

* Considere la posibilidad de [usar espacios de implementación](/azure/app-service/deploy-staging-slots) para realizar implementaciones confiables sin tiempo de inactividad.

* Diseñe e implemente una estrategia de DevOps. Para mantener la confiabilidad y, al mismo tiempo, aumentar la velocidad de desarrollo, considere la posibilidad de [automatizar las implementaciones y pruebas con Azure Pipelines](/azure/devops/pipelines/ecosystems/java-webapp). Si usa espacios de implementación, puede [automatizar la implementación en un espacio](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot) y cambiar al siguiente espacio.

* Diseñe e implemente una estrategia de continuidad empresarial y recuperación ante desastres. En el caso de las aplicaciones críticas, considere la posibilidad de usar una [arquitectura de implementación de varias regiones](/azure/architecture/reference-architectures/app-service-web-app/multi-region).
