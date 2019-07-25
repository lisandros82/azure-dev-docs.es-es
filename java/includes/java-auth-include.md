---
ms.openlocfilehash: 0a9b06932baa51c3cf003a4485a3a25261ffe91d
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68284266"
---
Cree un [archivo de autenticación](../java-sdk-azure-authenticate.md#mgmt-file) y exporte una variable de entorno `AZURE_AUTH_LOCATION` en la línea de comandos con la ruta de acceso completa al archivo.

```bash
export AZURE_AUTH_LOCATION=/Users/raisa/azure.auth
```

El archivo de autenticación se usa para configurar el objeto `Azure` del punto de entrada utilizado por las bibliotecas de administración para definir, crear y configurar los recursos de Azure.

```java
// pull in the location of the security file from the environment 
final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));

Azure azure = Azure
        .configure()
        .withLogLevel(LogLevel.NONE)
        .authenticate(credFile)
        .withDefaultSubscription();
```

[Más información](../java-sdk-azure-authenticate.md#mgmt-auth) acerca de las opciones de autenticación al utilizar las bibliotecas de administración de Azure para Java.