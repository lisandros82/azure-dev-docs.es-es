---
title: Cómo usar el iniciador de Spring Boot para Azure Storage
description: Aprenda a configurar una aplicación de Spring Boot Initializer con el iniciador de Azure Storage.
services: storage
documentationcenter: java
author: bmitchell287
ms.author: brendm
ms.date: 12/19/2018
ms.devlang: java
ms.service: storage
ms.topic: article
ms.workload: storage
ms.openlocfilehash: fe6f2869d775961cf69e7f109fe788d3dfad8b28
ms.sourcegitcommit: 54d34557bb83f52a215bf9020263cb9f9782b41d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74118063"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-storage"></a>Cómo usar el iniciador de Spring Boot para Azure Storage

Este artículo explica cómo crear una aplicación personalizada con **Spring Initializr** y, a continuación, agregar el iniciador de almacenamiento de Azure a la aplicación y usar la aplicación para cargar un blob en la cuenta de Azure Storage.

## <a name="prerequisites"></a>Requisitos previos

Los siguientes requisitos previos son necesarios para seguir los pasos descritos en este artículo:

* Una suscripción de Azure. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) o registrarse para obtener una [cuenta de Azure gratuita](https://azure.microsoft.com/pricing/free-trial/).
* La [Interfaz de la línea de comandos (CLI) de Azure](https://docs.microsoft.com/cli/azure/index).
* Un kit de desarrollo de Java (JDK) admitido Para más información sobre los JDK disponibles para desarrollar en Azure, consulte <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versión 3.0 o posterior.

> [!IMPORTANT]
>
> Se necesita Spring Boot versión 2.0 o posteriores para completar los pasos descritos en este artículo.
>

## <a name="create-an-azure-storage-account-and-blob-container-for-your-application"></a>Creación de una cuenta de Azure Storage y un contenedor de blobs para la aplicación

En el procedimiento siguiente se crea una cuenta de Azure Storage y un contenedor.

1. Vaya a Azure Portal en <https://portal.azure.com/> e inicie sesión.

1. Haga clic en **+Crear un recurso**, en **Almacenamiento** y en **Cuenta de almacenamiento**.

   ![Creación de una cuenta de Azure Storage][IMG01]

1. En la página **Crear cuenta de almacenamiento**, escriba la siguiente información:

   * Seleccione**Suscripción**.
   * Seleccione un **Grupo de recursos** o cree uno nuevo.
   * Escriba un **Nombre de cuenta de almacenamiento** único, que pasará a formar parte del identificador URI de la cuenta de almacenamiento. Por ejemplo: si escribió **wingtiptoysstorage** para el **Nombre**, el identificador URI sería *wingtiptoysstorage.core.windows.net*.
   * Especifique la **Ubicación** de la cuenta de almacenamiento.
1. Cuando haya especificado las opciones enumeradas anteriormente, haga clic en **Revisar y crear**. 
1. Revise las especificaciones y, a continuación, haga clic en **Crear** para crear la cuenta de almacenamiento.
1. Cuando la implementación finalice, haga clic en **Ir al recurso**.
1. Haga clic en **Containers**(Contenedores).
1. Haga clic en **+ Contenedor**.
   * Nombre del contenedor.
   * Seleccione *Blob* en la lista desplegable.

   ![Creación de un contenedor de blobs][IMG02]

1. Azure Portal enumerará el contenedor de blobs una vez se haya creado.

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Creación de una aplicación sencilla de Spring Boot con Spring Initializr

En el procedimiento siguiente se crea la aplicación de Spring Boot.

1. Vaya a <https://start.spring.io/>.

1. Especifique las opciones siguientes:

   * Genere un proyecto de **Maven**.
   * Especifique **Java**.
   * Especifique una versión de **Spring Boot** igual o superior a la 2.0.
   * Especifique los nombres de **Group** (Grupo) y **Artifact** (Artefacto) de la aplicación.
   * Agregue la dependencia **Web**.

      ![Opciones básicas de Spring Initializr][SI01]

   > [!NOTE]
   >
   > Spring Initializr usa los nombres de **Group** (Grupo) y **Artifact** (Artefacto) para crear el nombre del paquete, por ejemplo: *com.wingtiptoys.storage*.
   >

1. Cuando haya especificado las opciones enumeradas anteriormente, haga clic en **Generate** (Generar).

1. Cuando se le solicite, descargue el proyecto en una ruta de acceso del equipo local.

1. Después de extraer los archivos en el sistema local, la aplicación sencilla de Spring Boot estará lista para editarla.

## <a name="configure-your-spring-boot-app-to-use-the-azure-storage-starter"></a>Configuración de la aplicación de Spring Boot para usar el iniciador de Azure Storage

El siguiente procedimiento configura la aplicación de Spring Boot para que use Azure Storage.

1. Busque el archivo *pom.xml* en el directorio raíz de la aplicación, por ejemplo:

   `C:\SpringBoot\storage\pom.xml`

   O bien

   `/users/example/home/storage/pom.xml`

1. Abra el archivo *pom.xml* en un editor de texto y agregue el iniciador Spring Cloud Azure Storage a la lista `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-azure-starter-storage</artifactId>
      <version>1.0.0.M2</version>
   </dependency>
   ```

   ![Edición del archivo pom.xml][SI03]

1. Guarde y cierre el archivo *pom.xml*.

## <a name="create-an-azure-credential-file"></a>Creación de un archivo de credenciales de Azure

En el procedimiento siguiente se crea el archivo de credenciales de Azure.

1. Abra el símbolo del sistema.

1. Vaya al directorio *resources* de la aplicación de Spring Boot, por ejemplo:

   ```shell
   cd C:\SpringBoot\storage\src\main\resources
   ```

   O bien

   ```shell
   cd /users/example/home/storage/src/main/resources
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

## <a name="configure-your-spring-boot-app-to-use-your-azure-storage-account"></a>Configuración de la aplicación de Spring Boot para usar la cuenta de Azure Storage

En el siguiente procedimiento se configura la aplicación de Spring Boot para que use la cuenta de Azure Storage.

1. Busque el archivo *application.properties* en el directorio *resources* de la aplicación, por ejemplo:

   `C:\SpringBoot\storage\src\main\resources\application.properties`

   O bien

   `/users/example/home/storage/src/main/resources/application.properties`

2. Abra el archivo *application.properties* en un editor de texto, agregue las siguientes líneas y, a continuación, sustituya los valores de ejemplo por las propiedades adecuadas de la cuenta de almacenamiento:

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.storage.account=wingtiptoysstorage
   ```
   Donde:

   |                   Campo                   |                                            DESCRIPCIÓN                                            |
   |-------------------------------------------|---------------------------------------------------------------------------------------------------|
   | `spring.cloud.azure.credential-file-path` |            Especifica el archivo de credenciales de Azure que creó anteriormente en este tutorial.             |
   |    `spring.cloud.azure.resource-group`    |           Especifica el grupo de recursos de Azure que contiene la cuenta de Azure Storage.            |
   |        `spring.cloud.azure.region`        | Especifica la región geográfica que seleccionó cuando creó la cuenta de Azure Storage. |
   |   `spring.cloud.azure.storage.account`    |            Especifica la cuenta de Azure Storage que creó anteriormente en este tutorial.             |


3. Guarde y cierre el archivo *application.properties*.

## <a name="add-sample-code-to-implement-basic-azure-storage-functionality"></a>Adición de código de ejemplo para implementar la funcionalidad básica de Azure Storage

En esta sección, creará las clases de Java necesarias para almacenar un blob en la cuenta de Azure Storage.

### <a name="modify-the-main-application-class"></a>Modificación de la clase de aplicación principal

1. Busque el archivo de Java de la aplicación principal en el directorio del paquete de la aplicación; por ejemplo:

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\StorageApplication.java`

   O bien

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/StorageApplication.java`

1. Abra el archivo de Java de la aplicación principal en un editor de texto y agregue las siguientes líneas al archivo. Reemplace wingtiptoys por los valores siguientes:

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   public class StorageApplication {
      public static void main(String[] args) {
         SpringApplication.run(StorageApplication.class, args);
      }
   }
   ```

1. Guarde y cierre el archivo de Java de la aplicación principal.

### <a name="add-a-web-controller-class"></a>Adición de una clase de controlador web

1. Cree un archivo Java nuevo llamado *WebController.java* en el directorio del paquete de la aplicación, por ejemplo:

   `C:\SpringBoot\storage\src\main\java\com\wingtiptoys\storage\WebController.java`

   O bien

   `/users/example/home/storage/src/main/java/com/wingtiptoys/storage/WebController.java`

1. Abra el archivo de Java del controlador web en un editor de texto y agregue las siguientes líneas al archivo.  Cambie *wingtiptoys* por su grupo de recursos y *storage* por el nombre del artefacto.

   ```java
   package com.wingtiptoys.storage;

   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.core.io.Resource;
   import org.springframework.core.io.WritableResource;
   import org.springframework.util.StreamUtils;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RequestBody;
   import org.springframework.web.bind.annotation.RestController;

   import java.io.IOException;
   import java.io.OutputStream;
   import java.nio.charset.Charset;

   @RestController
   public class WebController {

      @Value("blob://test/myfile.txt")
      private Resource blobFile;

      @GetMapping(value = "/")
      public String readBlobFile() throws IOException {
         return StreamUtils.copyToString(
            this.blobFile.getInputStream(),
            Charset.defaultCharset()) + "\n";
      }

      @PostMapping(value = "/")
      public String writeBlobFile(@RequestBody String data) throws IOException {
         try (OutputStream os = ((WritableResource) this.blobFile).getOutputStream()) {
            os.write(data.getBytes());
         }
         return "File was updated.\n";
      }
   }
   ```

   Donde la sintaxis `@Value("blob://[container]/[blob]")` define respectivamente los nombres del contenedor y el blob en los que desea almacenar los datos.

1. Guarde y cierre el archivo de Java del controlador web.

1. Abra un símbolo del sistema y cambie el directorio a la carpeta donde se encuentra el archivo *pom.xml*; por ejemplo:

   `cd C:\SpringBoot\storage`

   O bien

   `cd /users/example/home/storage`

1. Compile la aplicación de Spring Boot con Maven y ejecútela; por ejemplo:

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. Una vez que se está ejecutando la aplicación, puede usar *curl* para probar la aplicación, por ejemplo:

   a. Envíe una solicitud POST para actualizar el contenido de un archivo:

      ```shell
      curl -X POST -H "Content-Type: text/plain" -d "Hello World" http://localhost:8080/
      ```

      Debería ver una respuesta que indica que se actualizó el archivo.

   b. Envíe una solicitud GET para comprobar el contenido del archivo:

      ```shell
      curl -X GET http://localhost:8080/
      ```

     Debería ver el texto "Hello World" que se envió.

## <a name="summary"></a>Resumen

En este tutorial ha creado una nueva aplicación Java con **[Spring Initializr]** , ha agregado el iniciador de Azure Storage a la aplicación y, finalmente, ha configurado la aplicación para cargar un blob en la cuenta de Azure Storage.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Spring y Azure, vaya al centro de documentación de Azure.

> [!div class="nextstepaction"]
> [Spring en Azure](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-azure-key-vault?view=azure-java-stable)

### <a name="additional-resources"></a>Recursos adicionales

Para más información acerca de otros iniciadores de Spring Boot disponibles para Microsoft Azure, consulte [Iniciadores de Spring Boot para Azure](spring-boot-starters-for-azure.md).

Para información detallada acerca de otras API de almacenamiento de Azure que puede llamar desde aplicaciones de Spring Boot, consulte los artículos siguientes:
* [Uso de Azure Blob Storage en Java](/azure/storage/blobs/storage-java-how-to-use-blob-storage)
* [Uso de Azure Queue Storage en Java](/azure/storage/queues/storage-java-how-to-use-queue-storage)
* [Uso de Azure Table Storage en Java](/azure/cosmos-db/table-storage-how-to-use-java)
* [Uso de Azure File Storage en Java](/azure/storage/files/storage-java-how-to-use-file-storage)

<!-- IMG List -->

[IMG01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-01.png
[IMG02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-02.png
[IMG03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-03.png
[IMG04]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-04.png
[IMG05]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-storage-account-05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-01.png
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-02.png
[SI03]: ./media/configure-spring-boot-starter-java-app-with-azure-storage/create-project-03.png
