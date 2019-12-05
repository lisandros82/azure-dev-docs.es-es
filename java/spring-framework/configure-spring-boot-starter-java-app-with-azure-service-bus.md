---
title: Cómo usar Spring Boot Starter para JMS en Azure Service Bus
description: En este artículo se muestra cómo usar Spring JMS Starter para enviar y recibir mensajes de Azure Service Bus.
author: seanli1988
manager: kyliel
ms.author: seal
ms.date: 08/21/2019
ms.topic: article
ms.openlocfilehash: b64095bc2971bf9d9a7308bebdb91617538796c4
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812122"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-service-bus-jms"></a>Cómo usar Spring Boot Starter para JMS en Azure Service Bus

[!INCLUDE [spring-boot-20-note.md](../includes/spring-boot-20-note.md)]

Azure proporciona una plataforma de mensajería asincrónica denominada [Azure Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) ("Service Bus") que se basa en el estándar [Advanced Message Queueing Protocol 1.0](http://www.amqp.org/) (AMQP 1.0). Service Bus puede usarse en el intervalo de plataformas de Azure admitidas.

Spring Boot Starter para JMS en Azure Service Bus proporciona integración de Spring con Service Bus.

En este artículo se muestra cómo usar Spring Boot Starter para JMS en Azure Service Bus para enviar y recibir mensajes de `queues` y `topics` de Service Bus.

## <a name="prerequisites"></a>Requisitos previos

Se necesitan los siguientes requisitos previos para este artículo:

1. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) o registrarse para obtener una [cuenta gratuita](https://azure.microsoft.comfree/).

1. Un kit de desarrollo de Java (JDK) compatible (versión 8 o posterior). Para más información sobre los JDK disponibles para desarrollar en Azure, consulte <https://aka.ms/azure-jdks>.

1. Apache [Maven](http://maven.apache.org/) (versión 3.2 o posterior).

1. Si ya tiene una cola o un tema de Service Bus configurado, asegúrese de que el espacio de nombres de Service Bus cumple los requisitos siguientes:

    1. Permite el acceso desde todas las redes
    1. Es Premium (o superior)
    1. Tiene una directiva de acceso con acceso de lectura y escritura para la cola y el tema

1. Si no tiene una cola o un tema de Service Bus configurado, use Azure Portal para [crear una cola de Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quickstart-portal) o [un tema de Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal). Asegúrese de que el espacio de nombres cumple los requisitos especificados en el paso anterior. Además, tome nota de la cadena de conexión del espacio de nombres, ya que la necesitará para la aplicación de prueba de este tutorial.

1. Si no tiene ninguna aplicación de Spring Boot, [cree un proyecto de **Maven** con Spring Initializr](https://start.spring.io/). No olvide seleccionar **Proyecto de Maven** y, en **Dependencias**, agregue la dependencia **Web**.

## <a name="use-the-azure-service-bus-jms-starter"></a>Usar JMS Starter en Azure Service Bus

1. Busque el archivo *pom.xml* en el directorio principal de la aplicación; por ejemplo:

    `C:\SpringBoot\servicebus\pom.xml`

    O bien

    `/users/example/home/servicebus/pom.xml`

1. Abra el archivo *pom.xml* en un editor de texto.

1. Agregue JMS Starter en Azure Service Bus de Spring Boot a la lista de `<dependencies>`:

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus-jms-spring-boot-starter</artifactId>
        <version>2.1.7</version>
    </dependency>
    ```

    ![Agregue la sección de dependencia al archivo pom.xml.](./media/configure-spring-boot-starter-java-app-with-azure-service-bus/add-dependency-section-new.png)

1. Guarde y cierre el archivo *pom.xml*.

## <a name="configure-the-app-for-your-service-bus"></a>Configuración de la aplicación para Service Bus 

En esta sección, verá cómo configurar la aplicación para usar una cola o un tema de Service Bus.

### <a name="use-a-service-bus-queue"></a>Usar una cola de Service Bus

1. Busque el archivo *application.properties* en el directorio *resources* de la aplicación, por ejemplo:

    `C:\SpringBoot\servicebus\application.properties`

    O bien

    `/users/example/home/servicebus/application.properties`

1. Abra el archivo *application.properties* en un editor de texto.

1. Anexe el código siguiente al final del archivo *application.properties*. Reemplace los valores de ejemplo por los valores adecuados para Service Bus:

    ```yml
    spring.jms.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.jms.servicebus.idle-timeout=<IdleTimeout>
    ```

    **Descripciones de los campos**

    | Campo                                     | DESCRIPCIÓN                                                                                     |
    |-------------------------------------------|-------------------------------------------------------------------------------------------------|
    | `spring.jms.servicebus.connection-string` | Especifique la cadena de conexión que obtuvo en el espacio de nombres de Service Bus desde Azure Portal. |
    | `spring.jms.servicebus.idle-timeout`      | Especifique el tiempo de expiración de inactividad en milisegundos. El valor recomendado para este tutorial es 1800000.   |

1. Guarde y cierre el archivo *application.properties*.

### <a name="use-service-bus-topic"></a>Usar un tema de Service Bus

1. Busque el archivo *application.properties* en el directorio *resources* de la aplicación, por ejemplo:

    `C:\SpringBoot\servicebus\application.properties`

    O bien

    `/users/example/home/servicebus/application.properties`

1. Abra el archivo *application.properties* en un editor de texto.

1. Anexe el código siguiente al final del archivo *application.properties*. Reemplace los valores de ejemplo por los valores adecuados para Service Bus:

    ```yml
    spring.jms.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.jms.servicebus.topic-client-id=<ServiceBusTopicClientId>
    spring.jms.servicebus.idle-timeout=<IdleTimeout>
    ```

    **Descripciones de los campos**

    | Campo                                     | DESCRIPCIÓN                                                                                       |
    |-------------------------------------------|---------------------------------------------------------------------------------------------------|
    | `spring.jms.servicebus.connection-string` | Especifique la cadena de conexión que obtuvo en el espacio de nombres de Service Bus desde Azure Portal.   |
    | `spring.jms.servicebus.topic-client-id`   | Especifique el identificador de cliente de JMS si usa un tema de Azure Service Bus con una suscripción durable. |
    | `spring.jms.servicebus.idle-timeout`      | Especifique el tiempo de expiración de inactividad en milisegundos. El valor recomendado para este tutorial es 1800000.     |

1. Guarde y cierre el archivo *application.properties*.

## <a name="implement-basic-service-bus-functionality"></a>Implementar la funcionalidad básica de Service Bus

En esta sección, creará las clases Java necesarias para enviar mensajes a la cola o el tema de Service Bus y recibirá los mensajes de la suscripción correspondiente del tema o la cola.

### <a name="modify-the-main-application-class"></a>Modificación de la clase de aplicación principal

1. Busque el archivo de Java de la aplicación principal en el directorio del paquete de la aplicación; por ejemplo:

    `C:\SpringBoot\servicebus\src\main\java\com\wingtiptoys\servicebus\ServiceBusJmsStarterApplication.java`

    O bien

    `/users/example/home/servicebus/src/main/java/com/wingtiptoys/servicebus/ServiceBusJmsStarterApplication.java`

1. Abra el archivo Java de la aplicación principal en un editor de texto.

1. Agregue el siguiente código al archivo:

   ```java
    package com.wingtiptoys.servicebus;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusJmsStarterApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusJmsStarterApplication.class, args);
        }
    }
    ```

1. Guarde y cierre el archivo.

### <a name="define-a-test-java-class"></a>Definir una clase de Java de prueba

1. Con un editor de texto, cree un archivo Java denominado *User.java* en el directorio del paquete de la aplicación.

1. Defina una clase de usuario genérica que almacene y recupere el nombre del usuario:

    ```java
    package com.wingtiptoys.servicebus;

    import java.io.Serializable;

    // Define a generic User class.
    public class User implements Serializable {

        private static final long serialVersionUID = -295422703255886286L;

        private String name;

        public User() {
        }

        public User(String name) {
            setName(name);
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

    }
    ```

    `Serializable` se implementa para usar el método `send` en `JmsTemplate` en el marco Spring. De lo contrario, se debe definir un bean `MessageConverter` personalizado para serializar el contenido en JSON en formato de texto. Para obtener más información sobre `MessageConverter`, consulte el [Spring JMS Starter](https://spring.io/guides/gs/messaging-jms/).

1. Guarde y cierre el archivo *User.java*.

### <a name="create-a-new-class-for-the-message-send-controller"></a>Crear una nueva clase para el controlador de envío de mensajes

1. Con un editor de texto, cree un archivo Java denominado *SendController.java* en el directorio del paquete de la aplicación.

1. Agregue el siguiente código al archivo nuevo:

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.jms.core.JmsTemplate;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class SendController {

        private static final String DESTINATION_NAME = "<DestinationName>";

        private static final Logger logger = LoggerFactory.getLogger(SendController.class);

        @Autowired
        private JmsTemplate jmsTemplate;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            logger.info("Sending message");
            jmsTemplate.convertAndSend(DESTINATION_NAME, new User(message));
            return message;
        }
    }
    ```

    > [!NOTE]
    > Reemplace `<DestinationName>` por el nombre de la cola o el nombre del tema configurado en el espacio de nombres de Service Bus.

1. Guarde y cierre *SendController.java*.

### <a name="create-a-class-for-the-message-receive-controller"></a>Crear una clase para el controlador de recepción de mensajes

#### <a name="receive-messages-from-a-service-bus-queue"></a>Recibir mensajes de una cola de Service Bus

1. Con un editor de texto, cree un archivo Java denominado *QueueReceiveController.java* en el directorio del paquete de la aplicación.

1. Agregue el siguiente código al archivo nuevo:

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.jms.annotation.JmsListener;
    import org.springframework.stereotype.Component;

    @Component
    public class QueueReceiveController {

        private static final String QUEUE_NAME = "<ServiceBusQueueName>";

        private final Logger logger = LoggerFactory.getLogger(QueueReceiveController.class);

        @JmsListener(destination = QUEUE_NAME, containerFactory = "jmsListenerContainerFactory")
        public void receiveMessage(User user) {
            logger.info("Received message: {}", user.getName());
        }
    }
    ```

    > [!NOTE]
    > Reemplace `<ServiceBusQueueName>` por el nombre de la cola configurado en el espacio de nombres de Service Bus.

1. Guarde y cierre el archivo *QueueReceiveController.java*.

#### <a name="receive-messages-from-a-service-bus-subscription"></a>Recibir mensajes de una suscripción de Service Bus

1. Con un editor de texto, cree un archivo Java denominado *SendController.java* en el directorio del paquete de la aplicación. 

1. Agregue el siguiente código al archivo nuevo. Reemplace el marcador de posición `<ServiceBusTopicName>` por el nombre de su propio tema configurado en el espacio de nombres de Service Bus. Reemplace el marcador de posición `<ServiceBusSubscriptionName>` por el nombre de su propia suscripción para el tema de Service Bus.

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.jms.annotation.JmsListener;
    import org.springframework.stereotype.Component;

    @Component
    public class TopicReceiveController {

        private static final String TOPIC_NAME = "<ServiceBusTopicName>";

        private static final String SUBSCRIPTION_NAME = "<ServiceBusSubscriptionName>";

        private final Logger logger = LoggerFactory.getLogger(TopicReceiveController.class);

        @JmsListener(destination = TOPIC_NAME, containerFactory = "topicJmsListenerContainerFactory",
                subscription = SUBSCRIPTION_NAME)
        public void receiveMessage(User user) {
            logger.info("Received message: {}", user.getName());
        }
    }
    ```

1. Guarde y cierre el archivo *TopicReceiveController.java*.

## <a name="build-and-test-your-application"></a>Compilación y prueba de la aplicación

1. Abra un símbolo del sistema y cambie el directorio a la ubicación del archivo *pom.xml*; por ejemplo:

    `cd C:\SpringBoot\servicebus`

    O bien

    `cd cd /users/example/home/servicebus`

1. Compile su aplicación de Spring Boot con Maven y ejecútela:

    ```shell
    mvn clean spring-boot:run
    ```

1. Una vez que se esté ejecutando la aplicación, puede usar *curl* para probarla:

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    Debería ver "Sending message" y "hello" en el registro de la aplicación:

    ```shell
    [nio-8080-exec-1] com.wingtiptoys.servicebus.SendController : Sending message
    [enerContainer-1] com.wingtiptoys.servicebus.ReceiveController : Received message: hello
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no lo necesite, use [Azure Portal](https://portal.azure.com/) para eliminar los recursos creados en este artículo y evitar cargos inesperados.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cómo usar API de JMS con Service Bus y AMQP 1,0](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)