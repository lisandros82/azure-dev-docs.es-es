---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: e37d0337361ce742ce7f7994ab634e63bc4b2ead
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825839"
---
### <a name="inventory-secrets"></a>Secretos de inventario

#### <a name="passwords-and-secure-strings"></a>Contraseñas y cadenas seguras

Compruebe las cadenas secretas y las contraseñas en todas las propiedades y los archivos de configuración de los servidores de producción. Asegúrese de comprobar los archivos *server.xml* y *context.xml* en *$CATALINA_BASE/conf*. También puede encontrar archivos de configuración que contengan contraseñas o credenciales dentro de la aplicación. Estos pueden ser archivos *META-INF/context.xml* y, para las aplicaciones de Spring Boot, archivos *application.properties* o *application.yml*.
