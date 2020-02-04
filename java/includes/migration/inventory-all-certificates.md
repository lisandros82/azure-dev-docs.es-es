---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 3427cc445333c9851a760a607c402e689c7b6ba4
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830803"
---
### <a name="inventory-all-certificates"></a>Inventario de todos los certificados

Documente todos los certificados usados para los puntos de conexión SSL públicos. Para ver todos los certificados de los servidores de producción, ejecute el siguiente comando:

```bash
keytool -list -v -keystore <path to keystore>
```
