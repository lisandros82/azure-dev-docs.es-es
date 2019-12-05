---
title: Cómo usar el iniciador de Spring Boot para Apache Kafka con Azure Event Hubs
description: Aprenda a configurar una aplicación creada con Spring Boot Initializer para usar Apache Kafka con Azure Event Hubs.
services: event-hubs
documentationcenter: java
ms.date: 12/19/2018
ms.service: event-hubs
ms.topic: article
ms.openlocfilehash: 6e6bee70c07538d06859254543886685308a0302
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812061"
---
# <a name="how-to-use-the-spring-boot-starter-for-apache-kafka-with-azure-event-hubs"></a>Cómo usar el iniciador de Spring Boot para Apache Kafka con Azure Event Hubs

En este artículo se muestra cómo configurar una aplicación de Spring Cloud Stream Binder basada en Java creada con Spring Boot Initializer para usar [Apache Kafka] con Azure Event Hubs.

## <a name="prerequisites"></a>Requisitos previos

Los siguientes requisitos previos son necesarios para seguir los pasos descritos en este artículo:

* Una suscripción de Azure. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN] o registrarse para obtener una [cuenta de Azure gratuita].
* Un kit de desarrollo de Java (JDK) admitido Para más información sobre los JDK disponibles para desarrollar en Azure, consulte <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versión 3.0 o posterior.

> [!IMPORTANT]
>
> Se necesita Spring Boot versión 2.0 o posteriores para completar los pasos descritos en este artículo.
>

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a>Creación de un centro de eventos de Azure mediante Azure Portal

### <a name="create-an-azure-event-hub-namespace"></a>Creación de un espacio de nombres del centro de eventos de Azure

1. Vaya a Azure Portal en <https://portal.azure.com/> e inicie sesión.

1. Haga clic en **+ Crear un recurso**, luego en **Internet de las cosas** y, a continuación, busque *Event Hubs**.

1. Haga clic en **Create**(Crear).

   ![Creación de un espacio de nombres del centro de eventos de Azure][IMG01]

1. En la página **Crear espacio de nombres**, escriba la información siguiente:

   * Escriba un **Nombre** único, que pasará a formar parte del identificador URI del espacio de nombres del centro de eventos. Por ejemplo: si escribió **wingtiptoys** para el **Nombre**, el identificador URI sería *wingtiptoys.servicebus.windows.net*.
   * Plan de tarifa.
   * Especifique **Habilitar Kafka** para el espacio de nombres.
   * Elija la **Suscripción** que quiere usar para el espacio de nombres.
   * Especifique si quiere crear un nuevo **Grupo de recursos** para el espacio de nombres o elija un grupo de recursos existente.
   * Especifique la **Ubicación** del espacio de nombres del centro de eventos.
   * También puede especificar las **Unidades de procesamiento** para el espacio de nombres.

   ![Especificación de las opciones del espacio de nombres del centro de eventos de Azure][IMG02]

1. Cuando haya especificado las opciones enumeradas anteriormente, haga clic en **Crear** para crear el espacio de nombres.

### <a name="create-an-azure-event-hub-in-your-namespace"></a>Creación de un centro de eventos de Azure en el espacio de nombres

Una vez implementado el espacio de nombres, puede crear un centro de eventos en el espacio de nombres.

1. Vaya al espacio de nombres creado en el paso anterior.

1. Haga clic en **+ Centro de eventos** en la barra de menús superior.

1. Asigne un nombre al centro de eventos.

1. Haga clic en **Create**(Crear).

   ![Crear centro de eventos][IMG05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Creación de una aplicación sencilla de Spring Boot con Spring Initializr

1. Vaya a <https://start.spring.io/>.

1. Especifique las opciones siguientes:

   * Genere un proyecto de **Maven** con **Java**.
   * Especifique una versión de **Spring Boot** igual o superior a la 2.0.
   * Especifique los nombres de **Group** (Grupo) y **Artifact** (Artefacto) de la aplicación.
   * Agregue la dependencia **Web**.

      ![Opciones básicas de Spring Initializr][SI01]

   > [!NOTE]
   >
   > Spring Initializr usa los nombres de **Group** (Grupo) y **Artifact** (Artefacto) para crear el nombre del paquete, por ejemplo: *com.wingtiptoys.kafka*.
   >

1. Cuando haya especificado las opciones enumeradas anteriormente, haga clic en **Generate Project** (Generar proyecto).

1. Cuando se le solicite, descargue el proyecto en una ruta de acceso del equipo local.

1. Después de extraer los archivos en el sistema local, la aplicación sencilla de Spring Boot estará lista para editarla.

## <a name="configure-your-spring-boot-app-to-use-the-spring-cloud-kafka-stream-and-azure-event-hub-starters"></a>Configuración de la aplicación de Spring Boot para usar los iniciadores Spring Cloud Kafka Stream y Azure Event Hub

1. Busque el archivo *pom.xml* en el directorio raíz de la aplicación, por ejemplo:

   `C:\SpringBoot\kafka\pom.xml`

   O bien

   `/users/example/home/kafka/pom.xml`

1. Abra el archivo *pom.xml* en un editor de texto y agregue los iniciadores Spring Cloud Kafka Stream y Azure Event Hub a la lista `<dependencies>`:

   ```xml
   <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-stream-kafka</artifactId>
      <version>2.0.1.RELEASE</version>
   </dependency>
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-starter-eventhub</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![Edición del archivo pom.xml][SI03]

1. Guarde y cierre el archivo *pom.xml*.

## <a name="create-an-azure-credential-file"></a>Creación de un archivo de credenciales de Azure

1. Abra el símbolo del sistema.

1. Vaya al directorio *resources* de la aplicación de Spring Boot, por ejemplo:

   ```shell
   cd C:\SpringBoot\eventhub\src\main\resources
   ```

   O bien

   ```shell
   cd /users/example/home/eventhub/src/main/resources
   ```

1. Inicio de sesión en la cuenta de Azure

   ```azurecli
   az login
   ```

1. Muestre las suscripciones:

   ```azurecli
   az account list
   ```
   Azure devolverá la lista de sus suscripciones, y tendrá que copiar el GUID de la suscripción que desea usar; por ejemplo:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "gena.soto@wingtiptoys.com",
         "type": "user"
       }
     }
   ]
   ```
   
1. Especifique el identificador GUID de la suscripción que desea usar con Azure; por ejemplo:

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. Cree el archivo de credenciales de Azure:

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   Este comando crea un archivo *my.azureauth* en el directorio *resources* con un contenido similar al del siguiente ejemplo:

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a>Configuración de la aplicación de Spring Boot para usar el centro de eventos de Azure

1. Busque el archivo *application.properties* en el directorio *resources* de la aplicación, por ejemplo:

   `C:\SpringBoot\eventhub\src\main\resources\application.properties`

   O bien

   `/users/example/home/eventhub/src/main/resources/application.properties`

2. Abra el archivo *application.properties* en un editor de texto, agregue las siguientes líneas y, a continuación, sustituya los valores de ejemplo por las propiedades adecuadas del centro de eventos:

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoys

   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   ```
   Donde:

   |                       Campo                       |                                                                                   DESCRIPCIÓN                                                                                    |
   |---------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |     `spring.cloud.azure.credential-file-path`     |                                                    Especifica el archivo de credenciales de Azure que creó anteriormente en este tutorial.                                                    |
   |        `spring.cloud.azure.resource-group`        |                                                      Especifica el grupo de recursos de Azure que contiene el centro de eventos de Azure.                                                      |
   |            `spring.cloud.azure.region`            |                                           Especifica la región geográfica que seleccionó cuando creó el centro de eventos de Azure.                                            |
   |      `spring.cloud.azure.eventhub.namespace`      |                                          Especifica el nombre único que proporcionó cuando creó el espacio de nombres del centro de eventos de Azure.                                           |
   | `spring.cloud.stream.bindings.input.destination`  |                            Especifica el centro de eventos de Azure del destino de entrada, que para este tutorial es el centro que creó anteriormente en este tutorial.                            |
   |    `spring.cloud.stream.bindings.input.group `    | Especifica un grupo de consumidores del centro de eventos de Azure, que se puede establecer en "$Default" para poder usar el grupo de consumidores básico que se creó cuando creó el centro de eventos de Azure. |
   | `spring.cloud.stream.bindings.output.destination` |                               Especifica el centro de eventos de Azure del destino de salida, que en este tutorial será el mismo que el destino de entrada.                               |


3. Guarde y cierre el archivo *application.properties*.

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a>Adición de código de ejemplo para implementar la funcionalidad básica del centro de eventos

En esta sección se crean las clases de Java necesarias para enviar eventos al centro de eventos.

### <a name="modify-the-main-application-class"></a>Modificación de la clase de aplicación principal

1. Busque el archivo de Java de la aplicación principal en el directorio del paquete de la aplicación; por ejemplo:

   `C:\SpringBoot\kafka\src\main\java\com\wingtiptoys\kafka\KafkaApplication.java`

   O bien

   `/users/example/home/kafka/src/main/java/com/wingtiptoys/kafka/KafkaApplication.java`

1. Abra el archivo de Java de la aplicación principal en un editor de texto y agregue las siguientes líneas al archivo:

   ```java
   package com.wingtiptoys.kafka;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class KafkaApplication {
      public static void main(String[] args) {
         SpringApplication.run(KafkaApplication.class, args);
      }
   }
   ```

1. Guarde y cierre el archivo de Java de la aplicación principal.


### <a name="create-a-new-class-for-the-source-connector"></a>Creación de una nueva clase para el conector de origen

1. Cree un archivo de Java nuevo llamado *KafkaSource.java* en el directorio del paquete de la aplicación y, a continuación, abra el archivo en un editor de texto y agregue las siguientes líneas:

   ```java
   package com.wingtiptoys.kafka;

   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.messaging.Source;
   import org.springframework.messaging.support.GenericMessage;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RequestParam;
   import org.springframework.web.bind.annotation.RestController;

   @EnableBinding(Source.class)
   @RestController
   public class KafkaSource {
      @Autowired
      private Source source;

      @PostMapping("/messages")
      public String sendMessage(@RequestBody String message) {
         this.source.output().send(new GenericMessage<>(message));
         return message;
      }
   }
   ```

1. Guarde y cierre el archivo *KafkaSource.java*.

### <a name="create-a-new-class-for-the-sink-connector"></a>Creación de una nueva clase para el conector receptor

1. Cree un archivo de Java nuevo llamado *KafkaSink.java* en el directorio del paquete de la aplicación y, a continuación, abra el archivo en un editor de texto y agregue las siguientes líneas:

   ```java
   package com.wingtiptoys.kafka;

   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;

   @EnableBinding(Sink.class)
   public class KafkaSink {
      private static final Logger LOGGER = LoggerFactory.getLogger(KafkaSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message) {
         LOGGER.info("New message received: " + message);
      }
   }
   ```

1. Guarde y cierre el archivo *KafkaSink.java*.

## <a name="build-and-test-your-application"></a>Compilación y prueba de la aplicación

1. Abra un símbolo del sistema y cambie el directorio a la carpeta donde se encuentra el archivo *pom.xml*; por ejemplo:

   `cd C:\SpringBoot\kafka`

   O bien

   `cd /users/example/home/kafka`

1. Compile la aplicación de Spring Boot con Maven y ejecútela; por ejemplo:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Una vez que se está ejecutando la aplicación, puede usar *curl* para probar la aplicación, por ejemplo:

   ```shell
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   Debería ver el mensaje "hello" en los registros de la aplicación. Por ejemplo:

   ```shell
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka version : 1.0.2
   [http-nio-8080-exec-2] INFO org.apache.kafka.common.utils.AppInfoParser - Kafka commitId : 2a121f7b1d402825
   [wingtiptoyshub.container-0-C-1] INFO com.wingtiptoys.kafka.KafkaSink - New message received: hello
   ```


> [!NOTE]
> 
> Con fines de prueba, puede modificar el archivo *KafkaSource.java* para que contenga un formulario HTML simple similar al ejemplo siguiente:
> 
> ```java
> package com.wingtiptoys.kafka;
>    
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.cloud.stream.annotation.EnableBinding;
> import org.springframework.cloud.stream.messaging.Source;
> import org.springframework.messaging.support.GenericMessage;
> import org.springframework.web.bind.annotation.GetMapping;
> import org.springframework.web.bind.annotation.PostMapping;
> import org.springframework.web.bind.annotation.RequestBody;
> import org.springframework.web.bind.annotation.RequestParam;
> import org.springframework.web.bind.annotation.RestController;
> 
> @EnableBinding(Source.class)
> @RestController
> public class KafkaSource {
>   @Autowired
>   private Source source;
> 
>   @GetMapping("/")
>   public String sendForm() {
>     return "<html><body>" +
>       "<form action=\"/messages\" method=\"post\">" +
>       "<input type=\"text\" name=\"text\">" +
>       "<input type=\"submit\">" +
>       "</form></body><html>";
>     }
> 
>   @PostMapping("/messages")
>   public String sendMessage(@RequestBody String message) {
>     this.source.output().send(new GenericMessage<>(message));
>     return message;
>   }
> }
> ```
> 
> Esto le permitirá utilizar un explorador web para probar la aplicación:
> 
> ![Prueba de la aplicación mediante un explorador web][TB01]
> 
> Cuando se envía el formulario, la aplicación mostrará los resultados:
> 
> ![Respuesta de la aplicación en un explorador web][TB02]
> 

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Spring y Azure, vaya al centro de documentación de Azure.

> [!div class="nextstepaction"]
> [Spring en Azure](/azure/java/spring-framework)

### <a name="additional-resources"></a>Recursos adicionales

Consulte los siguientes artículos para más información sobre la compatibilidad de Azure para Event Hub Stream Binder y Apache Kafka:

* [¿Qué es Azure Event Hubs?](/azure/event-hubs/event-hubs-about)

* [Azure Event Hubs para Apache Kafka](/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview)

* [Creación de un espacio de nombres de Event Hubs y un centro de eventos con Azure Portal](/azure/event-hubs/event-hubs-create)

* [Creación de centros de eventos habilitados para Apache Kafka](/azure/event-hubs/event-hubs-create-kafka-enabled)

Para más información sobre el uso de Azure con Java, consulte [Azure para desarrolladores de Java] y [Working with Azure DevOps and Java] (Trabajo con Azure DevOps y Java).

**[Spring Framework]** es una solución de código abierto que ayuda a los desarrolladores de Java a crear aplicaciones de nivel empresarial. Uno de los proyectos más populares que se basa en esa plataforma es [Spring Boot], que proporciona un enfoque simplificado para crear aplicaciones de Java independientes. Para ayudar a los desarrolladores a empezar con Spring Boot, puede encontrar varios paquetes de ejemplo de Spring Boot en <https://github.com/spring-guides/>. Además de elegir de la lista de proyectos básicos de Spring Boot, el **[Spring Initializr]** ayuda a los desarrolladores en los primeros pasos para crear aplicaciones de Spring Boot personalizadas.

<!-- URL List -->

[Apache Kafka]: http://kafka.apache.org
[cuenta de Azure gratuita]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/ (Trabajo con Azure DevOps y Java)
[ventajas como suscriptor de MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[IMG01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-01.png
[IMG02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-02.png
[IMG03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-03.png
[IMG04]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-04.png
[IMG05]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-05.png
[IMG06]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-06.png
[IMG07]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-07.png
[IMG08]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-kafka-event-hub-08.png

[SI01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-01.png
[SI02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-02.png
[SI03]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/create-project-03.png

[TB01]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-01.png
[TB02]: ./media/configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub/test-browser-02.png
