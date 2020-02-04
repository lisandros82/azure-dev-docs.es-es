---
author: yevster
ms.author: yebronsh
ms.date: 1/24/2020
ms.openlocfilehash: 3cf4edcf12d7fe72c0fa5a0216d1a8c97a2f2e59
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825909"
---
### <a name="inventory-certificates"></a>Certificados de inventario

Documente todos los certificados utilizados para los puntos de conexión SSL públicos o la comunicación con las bases de datos de back-end y otros sistemas. Para ver todos los certificados de los servidores de producción, ejecute el siguiente comando:

```bash
keytool -list -v -keystore <path to keystore>
```
