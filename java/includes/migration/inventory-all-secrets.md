---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a771d0689e83e0b61bf9b8c31e0cbf36949fdc20
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830763"
---
### <a name="inventory-all-secrets"></a>Inventario de todos los secretos

Antes de la llegada de las tecnologías de "configuración como servicio", como Azure Key Vault, no había un concepto bien definido de "secretos". En su lugar, hay un conjunto dispar de opciones de configuración que funcionaban de forma eficaz como lo que ahora llamamos "secretos". Con los servidores de aplicaciones como WebLogic Server, estos secretos se encuentran en muchos archivos de configuración y almacenes de configuración diferentes. Compruebe los secretos y las contraseñas en todas las propiedades y los archivos de configuración de los servidores de producción. Asegúrese de comprobar *weblogic.xml* en sus WAR. Los archivos de configuración que contienen contraseñas o credenciales también se pueden encontrar dentro de la aplicación. Para más información, consulte [Conceptos básicos de Azure Key Vault](/azure/key-vault/basic-concepts).
