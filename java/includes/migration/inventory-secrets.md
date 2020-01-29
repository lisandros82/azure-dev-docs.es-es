---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 4228d1db44ac16da1c05fe97d20a3491cc9c5f51
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288574"
---
### <a name="inventory-secrets"></a>Secretos de inventario

#### <a name="passwords-and-secure-strings"></a>Contraseñas y cadenas seguras

Compruebe las cadenas secretas y las contraseñas en todas las propiedades y los archivos de configuración de los servidores de producción. Asegúrese de comprobar los archivos *server.xml* y *context.xml* en $CATALINA_BASE/conf. También puede encontrar archivos de configuración que contengan contraseñas o credenciales dentro de la aplicación. Estos pueden ser archivos *META-INF/context.xml* y, para las aplicaciones de Spring Boot, archivos *application.properties* o *application.yml*.

#### <a name="certificates"></a>Certificados

Documente todos los certificados usados para los puntos de conexión SSL públicos. Para ver todos los certificados de los servidores de producción, ejecute el siguiente comando:

```bash
keytool -list -v -keystore <path to keystore>
```
