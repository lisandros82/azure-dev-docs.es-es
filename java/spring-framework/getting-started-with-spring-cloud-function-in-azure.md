---
title: Introducción a Spring Cloud Function en Azure
description: Obtenga información sobre el uso de Spring Cloud Function en Azure.
services: ''
documentationcenter: java
author: judubois
manager: brborges
ms.assetid: ''
ms.author: judubois
ms.date: 07/17/2019
ms.devlang: Java
ms.service: azure-functions
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: fc62ce6d9416e2031fbefef92a4613fb0ff9cc43
ms.sourcegitcommit: 4cc7f5e1e4601065bfcb4c2eeb7d47ad0bec61f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68433165"
---
# <a name="getting-started-with-spring-cloud-function-in-azure"></a>Introducción a Spring Cloud Function en Azure

Este artículo le explica cómo usar [Spring Cloud Function](https://spring.io/projects/spring-cloud-function) para desarrollar una función de Java y publicarla en Azure Functions. Cuando haya terminado, el código de la función se ejecuta en el [Plan de consumo](/azure/azure-functions/functions-scale#consumption-plan) en Azure y puede activarse mediante una solicitud HTTP.

[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Requisitos previos

Para desarrollar funciones con Java, debe tener instalado lo siguiente:

- [Kit para desarrolladores de Java](https://aka.ms/azure-jdks), versión 8
- [Apache Maven](https://maven.apache.org), versión 3.0 o posterior
- [CLI de Azure](https://docs.microsoft.com/cli/azure)
- [Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2) versión 2.7.1158 u otra posterior.

> [!IMPORTANT]
> La variable de entorno JAVA_HOME se debe establecer en la ubicación de instalación del JDK para completar esta guía de inicio rápido.

## <a name="what-we-are-going-to-build"></a>Qué vamos a crear

Vamos a crear una función clásica "Hello, World", que se ejecuta en Azure Functions y se configura con Spring Cloud Function.

Recibirá un objeto JSON `User` simple, que contiene un nombre de usuario, y devolverá un objeto `Greeting`, que contiene el mensaje de bienvenida para ese usuario.

El proyecto que se creamos aquí está disponible en [https://github.com/Azure-Samples/hello-spring-function-azure ](https://github.com/Azure-Samples/hello-spring-function-azure), por lo que puede usar ese repositorio de ejemplo directamente si desea ver el trabajo final que se detalla en este inicio rápido.

## <a name="create-a-new-maven-project"></a>Creación de un nuevo proyecto de Maven

Vamos a crear un proyecto de Maven vacío y a configurarlo con Spring Cloud Function y Azure Functions.

En una carpeta vacía, cree un nuevo *archivo pom. XML* y copie y pegue el contenido del proyecto de ejemplo en [https://github.com/Azure-Samples/hello-spring-function-azure/blob/master/pom.xml](https://github.com/Azure-Samples/hello-spring-function-azure/blob/master/pom.xml).

> [!NOTE]
> Este archivo usa las dependencias de Maven tanto de Spring Boot como de Spring Cloud Function, y configura los complementos de Maven para Spring Boot y Azure Functions.

Es necesario personalizar algunas propiedades de la aplicación:

- `<functionAppName>` es el nombre de la función de Azure
- `<functionAppRegion>` es el nombre de la región de Azure donde se implementa la función.
- `<functionResourceGroup>` es el nombre del grupo de recursos de Azure que se usa.

Debe cambiar esas propiedades directamente cerca de la parte superior del archivo *pom.xml*:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <azure.functions.maven.plugin.version>1.3.2</azure.functions.maven.plugin.version>
    <azure.functions.java.library.version>1.3.0</azure.functions.java.library.version>
    <functionAppName>my-spring-function</functionAppName>
    <functionAppRegion>westus</functionAppRegion>
    <stagingDirectory>${project.build.directory}/azure-functions/${functionAppName}</stagingDirectory>
    <functionResourceGroup>my-resource-group</functionResourceGroup>
    <start-class>com.example.HelloFunction</start-class>
    <wrapper.version>1.0.22.RELEASE</wrapper.version>
</properties>
```

## <a name="create-azure-configuration-files"></a>Creación de archivos de configuración de Azure

Cree una carpeta *src/main/azure* y agregue los siguientes archivos de configuración de Azure Functions.

*host.json*:

```json
{
  "version": "2.0",
  "functionTimeout": "00:10:00"
}
```

*local.settings.json*:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "java",
    "AzureWebJobsDashboard": ""
  }
}
```

## <a name="create-domain-objects"></a>Creación de objetos de dominio

Azure Functions puede recibir y enviar objetos en formato JSON.
Ahora vamos a crear nuestros objetos `User` y `Greeting`, que representan nuestro modelo de dominio.
Puede crear objetos más complejos, con más propiedades, si desea personalizar este inicio rápido y hacerlo más interesante para su caso.

Cree una carpeta*src/main/java/com/example/model* y agregue los dos archivos siguientes:

*User.java*:

```java
package com.example.model;

public class User {

    public User() {
    }

    public User(String name) {
        this.name = name;
    }

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

*Greeting.java*:

```java
package com.example.model;

public class Greeting {

    public Greeting() {
    }

    public Greeting(String message) {
        this.message = message;
    }

    private String message;

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}
```

## <a name="create-the-spring-boot-application"></a>Creación de una aplicación de Spring Boot

Esta aplicación administrará toda la lógica de negocios y tendrá acceso a todo el ecosistema de Spring boot.
Esto ofrece dos ventajas principales en comparación con una función estándar de Azure Functions:

- No depende de las API de Azure Functions, por lo que se puede migrar fácilmente a otros sistemas. Por ejemplo, podría reutilizarse en una aplicación de Spring Boot normal.
- Puede usar todas las anotaciones `@Enable` de Spring Boot para agregar fácilmente características nuevas y eficaces.

En la carpeta *src/main/java/com/example*, cree el siguiente archivo, que es una aplicación de Spring Boot normal:

*HelloFunction.java*:

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

import java.util.function.Function;

@SpringBootApplication
public class HelloFunction {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(HelloFunction.class, args);
    }

    @Bean
    public Function<User, Greeting> hello() {
        return user -> new Greeting("Welcome, " + user.getName());
    }
}
```

> [!NOTE] 
> La función `hello()` es bastante específica:
> 
> - Devuelve `java.util.function.Function`, que es la función que se utilizará en este inicio rápido. Contiene la lógica de negocios y usa una API de Java estándar para transformar un objeto en otro.
> - Como tiene la anotación `@Bean`, es un bean de Spring y, de forma predeterminada, su nombre es el del método, `hello`. Esto es importante si desea crear otras funciones en la aplicación, ya que este nombre debe coincidir con el nombre de la función de Azure que crearemos en la sección siguiente.

## <a name="create-the-azure-function"></a>Creación de la función de Azure

Para aprovechar las ventajas de la API de Azure Functions, vamos a codificar una clase específica, que es una función de Azure que delegará su ejecución en la función de Spring Cloud Function que hemos creado en el paso anterior.

En la carpeta *src/main/java/com/example*, cree la siguiente función de Azure:

*HelloHandler.java*:

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import com.microsoft.azure.functions.ExecutionContext;
import com.microsoft.azure.functions.HttpMethod;
import com.microsoft.azure.functions.HttpRequestMessage;
import com.microsoft.azure.functions.annotation.AuthorizationLevel;
import com.microsoft.azure.functions.annotation.FunctionName;
import com.microsoft.azure.functions.annotation.HttpTrigger;
import org.springframework.cloud.function.adapter.azure.AzureSpringBootRequestHandler;

import java.util.Optional;

public class HelloHandler extends AzureSpringBootRequestHandler<User, Greeting> {

    @FunctionName("hello")
    public Greeting execute(
            @HttpTrigger(name = "request", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<User>> request,
            ExecutionContext context) {

        context.getLogger().info("Greeting user name: " + request.getBody().get().getName());
        return handleRequest(request.getBody().get(), context);
    }
}
```

Esta clase de Java es una función de Azure, con las siguientes características interesantes:

- Extiende `AzureSpringBootRequestHandler`, que crea el vínculo entre Azure Functions y Spring Cloud Function. Esto es lo que proporciona el método `handleRequest()` que se utiliza en su método `execute()`.
- El nombre de la función, tal como se define en la anotación `@FunctionName("hello")`, es el mismo que el bean de Spring que hemos configurado en el paso anterior, `hello`.
- Es una función real de Azure, por lo que puede usar toda la API de Azure Functions aquí.

## <a name="add-unit-tests"></a>Adición de pruebas unitarias

Por supuesto, este paso es opcional pero, como buenos desarrolladores, debe agregar pruebas unitarias para validar que la aplicación funciona correctamente.

Cree una carpeta*src/test/java/com/example* y agregue las siguientes pruebas JUnit:

*HelloFunctionTest.java*:

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import org.junit.Test;
import org.springframework.cloud.function.adapter.azure.AzureSpringBootRequestHandler;

import static org.assertj.core.api.Assertions.assertThat;

public class HelloFunctionTest {

    @Test
    public void test() {
        Greeting result = new HelloFunction().hello().apply(new User("foo"));
        assertThat(result.getMessage()).isEqualTo("Welcome, foo");
    }

    @Test
    public void start() throws Exception {
        AzureSpringBootRequestHandler<User, Greeting> handler = new AzureSpringBootRequestHandler<>(
                HelloFunction.class);
        Greeting result = handler.handleRequest(new User("foo"), null);
        handler.close();
        assertThat(result.getMessage()).isEqualTo("Welcome, foo");
    }
}
```

Ahora puede probar la función de Azure con Maven:

```bash
mvn clean test
```

## <a name="run-the-function-locally"></a>Ejecución de la función de forma local

Antes de implementar la aplicación en Azure Functions, vamos a probarla de forma local.

Primero debe empaquetar la aplicación en un archivo Jar:

```bash
mvn package
```

Ahora que la aplicación está empaquetada, puede ejecutarla mediante con el complemento `azure-functions` de Maven:

```bash
mvn azure-functions:run
```

La función de Azure estará disponible en el host local, a través del puerto 7071. Para probar la función, envíe una solicitud POST con un objeto `User` en formato JSON. Por ejemplo, con cURL:

```bash
curl http://localhost:7071/api/hello -d "{\"name\":\"Azure\"}"
```

La función responderá con un objeto `Greeting`, aún en formato JSON:

```Output
{
  "message": "Welcome, Azure"
}
```

Esta es una captura de pantalla de la solicitud cURL en la parte superior de la pantalla y la función local de Azure en la parte inferior:

 ![Función de Azure ejecutándose localmente][RFL01]

## <a name="deploy-the-function-to-azure-functions"></a>Implementación de la función en Azure Functions

Ahora va a publicar la función de Azure en el entorno de producción. Recuerde que las propiedades `<functionAppName>`, `<functionAppRegion>` y `<functionResourceGroup>` que ha definido en el archivo *pom.xml* se usarán para configurar la función.

Ejecute Maven para implementar la función automáticamente:

```bash
mvn azure-functions:deploy
```

Ahora, vaya a [Azure Portal](https://portal.azure.com) para buscar el `Function App` que se ha creado.

Haga clic en la función:

- En la información general de la función, anote la dirección URL de la función.
- Seleccione la pestaña **Características de la plataforma** para buscar el servicio **Secuencias de registro** y seleccione este servicio para comprobar la función en ejecución.

Ahora, igual que hizo en la sección anterior, use cURL para tener acceso a la función en ejecución. Reemplace `your-function-name` por el nombre real de la función:

```bash
curl https:/your-function-name.azurewebsites.net/api/hello -d "{\"name\":\"Azure\"}"
```

Igual que en la sección anterior, la función responderá con un objeto `Greeting`, aún en formato JSON:

```Output
{
  "message": "Welcome, Azure"
}
```

Enhorabuena, ya tiene una función de Spring Cloud Function en ejecución en Azure Functions.

<!-- IMG List -->

[RFL01]: ./media/getting-started-with-spring-cloud-function-in-azure/RFL01.png
