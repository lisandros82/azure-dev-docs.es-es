---
title: Cómo usar Spring Cloud Azure Stream Binder para Azure Service Bus
description: En este artículo se muestra cómo usar Spring Cloud Stream Binder para enviar y recibir mensajes de Azure Service Bus.
author: seanli1988
manager: kyliel
ms.author: seal
ms.date: 08/21/2019
ms.topic: article
ms.openlocfilehash: 8c62a68ff2a9912d88361adc6ef3b8dc2ea29c98
ms.sourcegitcommit: 6fa28ea675ae17ffb9ac825415e2e26a3dfe7107
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "77002294"
---
# <a name="how-to-use-spring-cloud-azure-stream-binder-for-azure-service-bus"></a>Cómo usar Spring Cloud Azure Stream Binder para Azure Service Bus

[!INCLUDE [spring-boot-20-note.md](../includes/spring-boot-20-note.md)]

Azure proporciona una plataforma de mensajería asincrónica denominada [Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) ("Service Bus") que se basa en el estándar [Advanced Message Queueing Protocol 1.0](http://www.amqp.org/) (AMQP 1.0). Service Bus puede usarse en el intervalo de plataformas de Azure admitidas.

En este artículo se muestra cómo usar Spring Cloud Stream Binder para enviar y recibir mensajes de `queues` y `topics` de Azure Service Bus.

## <a name="prerequisites"></a>Prerequisites

Se necesitan los siguientes requisitos previos para este artículo:

1. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) o registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/).

1. Un kit de desarrollo de Java (JDK) compatible (versión 8 o posterior). Para más información sobre los JDK disponibles para desarrollar en Azure, consulte <https://aka.ms/azure-jdks>.

1. Apache [Maven](http://maven.apache.org/) (versión 3.2 o posterior).

1. Si ya tiene una cola o un tema de Service Bus configurado, asegúrese de que el espacio de nombres de Service Bus cumple los requisitos siguientes:

    1. Permite el acceso desde todas las redes
    1. Es Premium (o superior)
    1. Tiene una directiva de acceso con acceso de lectura y escritura para la cola y el tema

1. Si no tiene una cola o un tema de Service Bus configurado, use Azure Portal para [crear una cola de Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quickstart-portal) o [un tema de Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal). Asegúrese de que el espacio de nombres cumple los requisitos especificados en el paso anterior. Además, tome nota de la cadena de conexión del espacio de nombres, ya que la necesitará para la aplicación de prueba de este tutorial.

1. Si no tiene ninguna aplicación de Spring Boot, [cree un proyecto de **Maven** con Spring Initializr](https://start.spring.io/). No olvide seleccionar **Proyecto de Maven** y, en **Dependencias**, agregue la dependencia **Web**.

## <a name="use-the-spring-cloud-stream-binder-starter"></a>Uso de Spring Cloud Stream Binder Starter

1. Busque el archivo *pom.xml* en el directorio principal de la aplicación; por ejemplo:

    `C:\SpringBoot\servicebus\pom.xml`

    O bien

    `/users/example/home/servicebus/pom.xml`

1. Abra el archivo *pom.xml* en un editor de texto.

1. Agregue el siguiente bloque de código en el elemento **&lt;dependencies>** , en función de si está usando una cola o un tema de Service Bus:

    **cola de Service Bus**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-servicebus-queue-stream-binder</artifactId>
        <version>1.1.0.RC5</version>
    </dependency>
    ```

    ![Edite el archivo pom.xml para la cola de Service Bus.](./media/configure-spring-cloud-stream-binder-java-app-with-service-bus/add-stream-binder-starter-pom-file-dependency-for-service-bus-queue.png)

    **Tema de Service Bus**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-servicebus-topic-stream-binder</artifactId>
        <version>1.1.0.RC5</version>
    </dependency>
    ```

    ![Edite el archivo pom.xml para el tema de Service Bus.](./media/configure-spring-cloud-stream-binder-java-app-with-service-bus/add-stream-binder-starter-pom-file-dependency-for-service-bus-topic.png)

1. Guarde y cierre el archivo *pom.xml*.

## <a name="configure-the-app-for-your-service-bus"></a>Configuración de la aplicación para Service Bus

Puede configurar la aplicación en función de la cadena de conexión o un archivo de credenciales. En este tutorial se usa una cadena de conexión. Para obtener más información sobre el uso de archivos de credenciales, consulte el [ejemplo de código de Spring Cloud Azure Stream Binder para una cola de Service Bus](https://github.com/microsoft/spring-cloud-azure/tree/release/1.1.0.RC4/spring-cloud-azure-samples/servicebus-queue-binder-sample#credential-file-based-usage
) y el [ejemplo de código de Spring Cloud Azure Stream Binder para un tema de Service Bus](https://github.com/microsoft/spring-cloud-azure/tree/release/1.1.0.RC4/spring-cloud-azure-samples/servicebus-topic-binder-sample#credential-file-based-usage).

1. Busque el archivo *application.properties* en el directorio *resources* de su aplicación; por ejemplo:

   `C:\SpringBoot\servicebus\src\main\resources\application.properties`

   O bien

   `/users/example/home/servicebus/src/main/resources/application.properties`

1. Abra el archivo *application.properties* en un editor de texto.

1. Anexe el código adecuado al final del archivo *application.properties*, en función de si usa una cola o un tema de Service Bus. Use la [tabla de descripciones de campos](#fd) para reemplazar los valores de ejemplo por las propiedades adecuadas para Service Bus.

    **cola de Service Bus**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=examplequeue
    spring.cloud.stream.bindings.output.destination=examplequeue
    spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **Tema de Service Bus**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=exampletopic
    spring.cloud.stream.bindings.input.group=examplesubscription
    spring.cloud.stream.bindings.output.destination=exampletopic
    spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **<a name="fd">Descripciones de los campos</a>**

    |                                        Campo                                   |                                                                                   Descripción                                                                                    |
    |--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |               `spring.cloud.azure.servicebus.connection-string`                |                                        Especifique la cadena de conexión que obtuvo en el espacio de nombres de Service Bus desde Azure Portal.                                   |
    |               `spring.cloud.stream.bindings.input.destination`                 |                            Especifique la cola o el tema de Service Bus que usó en este tutorial.                         |
    |                  `spring.cloud.stream.bindings.input.group`                    |                                            Si ha usado un tema de Service Bus, especifique la suscripción del tema.                                |
    |               `spring.cloud.stream.bindings.output.destination`                |                               Especifique el mismo valor que se usó para el destino de entrada.                        |
    | `spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode` |                                                       Especifique `MANUAL`.                                                   |
    | `spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode` |                                                       Especifique `MANUAL`.                                                   |

1. Guarde y cierre el archivo *application.properties*.

## <a name="implement-basic-service-bus-functionality"></a>Implementar la funcionalidad básica de Service Bus

En esta sección, se crean las clases Java necesarias para enviar mensajes a Service Bus.

### <a name="modify-the-main-application-class"></a>Modificación de la clase de aplicación principal

1. Busque el archivo de Java de la aplicación principal en el directorio del paquete de la aplicación; por ejemplo:

    `C:\SpringBoot\servicebus\src\main\java\com\example\ServiceBusBinderApplication.java`

   O bien

   `/users/example/home/servicebus/src/main/java/com/example/ServiceBusBinderApplication.java`

1. Abra el archivo Java de la aplicación principal en un editor de texto.

1. Agregue el siguiente código al archivo:

    ```java
    package com.example;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusBinderApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusBinderApplication.class, args);
        }
    }
    ```

1. Guarde y cierre el archivo.

### <a name="create-a-new-class-for-the-source-connector"></a>Creación de una nueva clase para el conector de origen

1. Con un editor de texto, cree un archivo Java denominado *StreamBinderSource.java* en el directorio del paquete de la aplicación.

1. Agregue el siguiente código al archivo nuevo:

    ```java
    package com.example;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.messaging.Source;
    import org.springframework.messaging.support.GenericMessage;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @EnableBinding(Source.class)
    @RestController
    public class StreamBinderSource {

        @Autowired
        private Source source;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            this.source.output().send(new GenericMessage<>(message));
            return message;
        }
    }
    ```

1. Guarde y cierre el archivo *StreamBinderSources.java*.

### <a name="create-a-new-class-for-the-sink-connector"></a>Creación de una nueva clase para el conector receptor

1. Con un editor de texto, cree un archivo Java denominado *StreamBinderSink.java* en el directorio del paquete de la aplicación.

1. Agregue las líneas de código siguientes al archivo nuevo:

    ```java
    package com.example;

    import com.microsoft.azure.spring.integration.core.AzureHeaders;
    import com.microsoft.azure.spring.integration.core.api.Checkpointer;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.annotation.StreamListener;
    import org.springframework.cloud.stream.messaging.Sink;
    import org.springframework.messaging.handler.annotation.Header;

    @EnableBinding(Sink.class)
    public class StreamBinderSink {

        @StreamListener(Sink.INPUT)
        public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
            System.out.println(String.format("New message received: '%s'", message));
            checkpointer.success().handle((r, ex) -> {
                if (ex == null) {
                    System.out.println(String.format("Message '%s' successfully checkpointed", message));
                }
                return null;
            });
        }
    }
    ```

1. Guarde y cierre el archivo *StreamBinderSink.java*.

## <a name="build-and-test-your-application"></a>Compilación y prueba de la aplicación

1. Abra un símbolo del sistema.

1. Cambie el directorio a la ubicación del archivo *pom.xml*; por ejemplo:

    `cd C:\SpringBoot\servicebus`

    O bien

    `cd /users/example/home/servicebus`

2. Compile su aplicación de Spring Boot con Maven y ejecútela:

    ```shell
    mvn clean spring-boot:run
    ```

3. Una vez que se esté ejecutando la aplicación, puede usar *curl* para probarla:

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    Debería ver el mensaje "hello" en el registro de la aplicación:

    ```shell
    New message received: 'hello'
    Message 'hello' successfully checkpointed
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no lo necesite, use [Azure Portal](https://portal.azure.com/) para eliminar los recursos creados en este artículo y evitar cargos inesperados.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Spring en Azure](/java/azure/spring-framework)