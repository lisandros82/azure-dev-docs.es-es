---
title: Cómo usar el iniciador de Spring Boot para Azure Key Vault
description: Aprenda a configurar una aplicación de Spring Boot Initializer con el iniciador de Azure Key Vault.
services: key-vault
documentationcenter: java
ms.date: 10/29/2019
ms.service: key-vault
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: identity
ms.openlocfilehash: 13ca873a838d3097484343a28d1f12300550ded8
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812130"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-key-vault"></a>Cómo usar el iniciador de Spring Boot para Azure Key Vault

En este artículo se muestra cómo crear una aplicación con **[Spring Initializr]** que usa la funcionalidad Spring Boot Starter para Azure Key Vault para recuperar una cadena de conexión almacenada como un secreto en un almacén de claves.

## <a name="prerequisites"></a>Requisitos previos

Los siguientes requisitos previos son necesarios para seguir los pasos descritos en este artículo:

* Una suscripción de Azure. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN] o registrarse para obtener una [cuenta de Azure gratuita].
* Un kit de desarrollo de Java (JDK) admitido Para más información sobre los JDK disponibles para desarrollar en Azure, consulte <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versión 3.0 o posterior.

## <a name="create-an-app-using-spring-initializr"></a>Creación de una aplicación con Spring Initialzr

En el procedimiento siguiente se crea la aplicación mediante Spring Initializr.

1. Vaya a <https://start.spring.io/>.

1. Especifique que quiere generar un proyecto de **Maven** con **Java**.  

1. Escriba los nombres de **Group** (Grupo) y **Artifact** (Artefacto) de la aplicación.

1. En la sección **Dependencies** (Dependencias), escriba **Azure Key Vault**.

1. Desplácese a la parte inferior de la página y haga clic en **Generate** (Generar).

   ![Generación del proyecto de Spring Boot][secrets-01]

1. Cuando se le solicite, descargue el proyecto en una ruta de acceso del equipo local.

## <a name="sign-into-azure"></a>Inicio de sesión en Azure

En el siguiente procedimiento se autentica al usuario en la CLI de Azure.

1. Abra el símbolo del sistema.

1. Inicie sesión en la cuenta de Azure mediante la CLI de Azure:

   ```azurecli
   az login
   ```

Siga las instrucciones para completar el proceso de inicio de sesión.

1. Muestre las suscripciones:

   ```azurecli
   az account list
   ```

   Azure devolverá la lista de sus suscripciones, y tendrá que copiar el GUID de la suscripción que desea usar; por ejemplo:

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "ssssssss-ssss-ssss-ssss-ssssssssssss",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "tttttttt-tttt-tttt-tttt-tttttttttttt",
       "user": {
         "name": "contoso@microsoft.com",
         "type": "user"
       }
     }
   ]
   ```

1. Especifique el identificador GUID de la cuenta que desea usar con Azure; por ejemplo:

   ```azurecli
   az account set -s ssssssss-ssss-ssss-ssss-ssssssssssss
   ```

## <a name="create-a-new-azure-key-vault"></a>Creación de un nuevo almacén de claves de Azure

En el siguiente procedimiento se crea e inicializa el almacén de claves.

1. Cree un grupo de recursos para los recursos de Azure que se usarán para el almacén de claves; por ejemplo:

   ```azurecli
   az group create --name vged-rg2 --location westus
   ```

   Donde:

   | Parámetro | DESCRIPCIÓN |
   |---|---|
   | `name` | Especifica un nombre único para el grupo de recursos. |
   | `location` | Especifica la [región de Azure](https://azure.microsoft.com/regions/) donde se hospedará el grupo de recursos. |

   La CLI de Azure mostrará los resultados de la creación del grupo de recursos, por ejemplo:  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/resourceGroups/vged-rg2",
     "location": "westus",
     "managedBy": null,
     "name": "vged-rg2",
     "properties": {
       "provisioningState": "Succeeded"
     },
     "tags": null
   }
   ```

2. Cree una entidad de servicio de Azure desde el registro de su aplicación; por ejemplo:
   ```shell
   az ad sp create-for-rbac --name "vgeduser"
   ```
   Donde:

   | Parámetro | DESCRIPCIÓN |
   |---|---|
   | `name` | Especifica el nombre de la entidad de servicio de Azure. |

   La CLI de Azure devolverá un mensaje de estado JSON que contiene los valores de *appId* y *password*, que usará más adelante como identificador de cliente y contraseña de cliente; por ejemplo:

   ```json
   {
     "appId": "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii",
     "displayName": "vgeduser",
     "name": "http://vgeduser",
     "password": "pppppppp-pppp-pppp-pppp-pppppppppppp",
     "tenant": "tttttttt-tttt-tttt-tttt-tttttttttttt"
   }
   ```

3. Cree un nuevo almacén de claves en el grupo de recursos; por ejemplo:

   ```azurecli
   az keyvault create --name vgedkeyvault --resource-group vged-rg2 --location westus --enabled-for-deployment true --enabled-for-disk-encryption true --enabled-for-template-deployment true --sku standard --query properties.vaultUri
   ```

   Donde:

   | Parámetro | DESCRIPCIÓN |
   |---|---|
   | `name` | Especifica un nombre único para el almacén de claves. |
   | `location` | Especifica la [región de Azure](https://azure.microsoft.com/regions/) donde se hospedará el grupo de recursos. |
   | `enabled-for-deployment` | Especifica la [opción de implementación del almacén de claves](/cli/azure/keyvault). |
   | `enabled-for-disk-encryption` | Especifica la [opción de cifrado del almacén de claves](/cli/azure/keyvault). |
   | `enabled-for-template-deployment` | Especifica la [opción de cifrado del almacén de claves](/cli/azure/keyvault). |
   | `sku` | Especifica la [opción de SKU del almacén de claves](/cli/azure/keyvault). |
   | `query` | Especifica el valor que se recuperará de la respuesta, que es el URI del almacén de claves que tendrá que completar en este tutorial. |

   La CLI de Azure mostrará el URI del almacén de claves, que usará más adelante; por ejemplo:  

   ```azurecli
   "https://vgedkeyvault.vault.azure.net"

   ```

4. Establezca la directiva de acceso de la entidad de servicio de Azure que creó anteriormente; por ejemplo:

   ```azurecli
   az keyvault set-policy --name vgedkeyvault --secret-permission set get list delete --spn "iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii"
   ```

   Donde:

   | Parámetro | DESCRIPCIÓN |
   |---|---|
   | `name` | Especifica el nombre del almacén de claves anterior. |
   | `secret-permission` | Especifica las [directivas de seguridad](/cli/azure/keyvault) para su almacén de claves. |
   | `spn` | Especifica el GUID del registro de la aplicación anterior. |

   La CLI de Azure mostrará los resultados de la creación de la directiva de seguridad, por ejemplo:  

   ```json
   {
     "id": "/subscriptions/ssssssss-ssss-ssss-ssss-ssssssssssss/...",
     "location": "westus",
     "name": "vgedkeyvault",
     "properties": {
       ...
       ... (A long list of values will be displayed here.)
       ...
     },
     "resourceGroup": "vged-rg2",
     "tags": {},
     "type": "Microsoft.KeyVault/vaults"
   }
   ```

5. Guarde un secreto en su nuevo almacén de claves; por ejemplo:

   ```azurecli
   az keyvault secret set --vault-name "vgedkeyvault" --name "connectionString" --value "jdbc:sqlserver://SERVER.database.windows.net:1433;database=DATABASE;"
   ```

   Donde:

   | Parámetro | DESCRIPCIÓN |
   |---|---|
   | `vault-name` | Especifica el nombre del almacén de claves anterior. |
   | `name` | Especifica el nombre del secreto. |
   | `value` | Especifica el valor del secreto. |

   La CLI de Azure mostrará los resultados de la creación del secreto; por ejemplo:  

   ```json
   {
     "attributes": {
       "created": "2017-12-01T09:00:16+00:00",
       "enabled": true,
       "expires": null,
       "notBefore": null,
       "recoveryLevel": "Purgeable",
       "updated": "2017-12-01T09:00:16+00:00"
     },
     "contentType": null,
     "id": "https://vgedkeyvault.vault.azure.net/secrets/connectionString/123456789abcdef123456789abcdef",
     "kid": null,
     "managed": null,
     "tags": {
       "file-encoding": "utf-8"
     },
     "value": "jdbc:sqlserver://.database.windows.net:1433;database=DATABASE;"
   }
   ```

## <a name="configure-and-compile-your-app"></a>Configuración y compilación de la aplicación

Use el procedimiento siguiente para configurar y compilar la aplicación.

1. Extraiga los archivos del archivo del proyecto de Spring Boot que descargó anteriormente en un directorio.

2. Vaya a la carpeta *src/main/resources* del proyecto y abra el archivo *application.properties* en un editor de texto.

3. Agregue los valores del almacén de claves que obtuvo en los pasos realizados anteriormente en este tutorial; por ejemplo:

   ```yaml
   azure.keyvault.uri=https://vgedkeyvault.vault.azure.net/
   azure.keyvault.client-id=iiiiiiii-iiii-iiii-iiii-iiiiiiiiiiii
   azure.keyvault.client-key=pppppppp-pppp-pppp-pppp-pppppppppppp
   ```

   Donde:

   |          Parámetro          |                                 DESCRIPCIÓN                                 |
   |-----------------------------|-----------------------------------------------------------------------------|
   |    `azure.keyvault.uri`     |           Especifica el URI obtenido cuando creó el almacén de claves.           |
   | `azure.keyvault.client-id`  |  Especifica el GUID de *appId* obtenido cuando creó la entidad de servicio.   |
   | `azure.keyvault.client-key` | Especifica el GUID de *password* obtenido cuando creó la entidad de servicio. |


4. Vaya al archivo de código fuente principal del proyecto; por ejemplo: */src/main/java/com/vged/secrets*.

5. Abra el archivo principal de Java de la aplicación en un archivo en un editor de texto, por ejemplo: *SecretsApplication.java* y agregue las siguientes líneas al archivo:

   ```java
   package com.vged.secrets;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class SecretsApplication implements CommandLineRunner {

      @Value("${connectionString}")
      private String connectionString;

      public static void main(String[] args) {
         SpringApplication.run(SecretsApplication.class, args);
      }

      public void run(String... varl) throws Exception {
         System.out.println(String.format("\nConnection String stored in Azure Key Vault:\n%s\n",connectionString));
      }
   }
   ```
   Este código de ejemplo recupera la cadena de conexión del almacén de claves y la muestra en la línea de comandos.

6. Guarde y cierre el archivo Java.

## <a name="build-and-test-your-app"></a>Compilación y prueba de la aplicación

Use el procedimiento siguiente para probar la aplicación.

1. Vaya al directorio donde está el archivo *pom.xml* de su aplicación de Spring Boot:

1. Compile la aplicación de Spring Boot con Maven; por ejemplo:

   ```bash
   mvn clean package
   ```

   Maven mostrará los resultados de la compilación.

   ![Estado de compilación de la aplicación de Spring Boot][build-application-01]

1. Ejecute la aplicación de Spring Boot con Maven; la aplicación mostrará la cadena de conexión del almacén de claves. Por ejemplo:

   ```bash
   mvn spring-boot:run
   ```

   ![Mensaje en tiempo de ejecución de Spring Boot][build-application-02]

## <a name="summary"></a>Resumen

En este tutorial, ha creado una nueva aplicación web de Java con **[Spring Initializr]** , ha creado una instancia de Azure Key Vault para almacenar información confidencial y, a continuación, ha configurado la aplicación para recuperar información del almacén de claves.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Spring y Azure, vaya al centro de documentación de Azure.

> [!div class="nextstepaction"]
> [Spring en Azure](/azure/java/spring-framework)

### <a name="additional-resources"></a>Recursos adicionales

Para más información sobre el uso de Azure Key Vault, consulte los siguientes artículos:

* [Documentación de Key Vault].

* [Introducción a Azure Key Vault]

Para obtener más información sobre el uso de aplicaciones de Spring Boot en Azure, consulte los siguientes artículos:

* [Implementación de una aplicación de Spring Boot en Azure App Service](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [Ejecución de una aplicación de Spring Boot en un clúster de Kubernetes en Azure Container Service](deploy-spring-boot-java-app-on-kubernetes.md)

Para más información sobre el uso de Azure con Java, consulte [Azure para desarrolladores de Java] y [Working with Azure DevOps and Java] (Trabajo con Azure DevOps y Java).

<!-- URL List -->

[Documentación de Key Vault]: /azure/key-vault/
[Introducción a Azure Key Vault]: /azure/key-vault/key-vault-get-started
[Azure para desarrolladores de Java]: /azure/java/
[cuenta de Azure gratuita]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/ (Trabajo con Azure DevOps y Java)
[ventajas como suscriptor de MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[secrets-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-01.png
[secrets-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-02.png
[secrets-03]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/secrets-03.png

[build-application-01]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-01.png
[build-application-02]: media/configure-spring-boot-starter-java-app-with-azure-key-vault/build-application-02.png
