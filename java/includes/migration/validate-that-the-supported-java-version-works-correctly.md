---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: cc634f89170f4db6f961da91e3eb3abe7393539d
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830993"
---
### <a name="validate-that-the-supported-java-version-works-correctly"></a>Comprobación de que la versión compatible de Java funciona correctamente

Todas las rutas de migración de WebLogic a Azure requieren una versión específica de Java, que varía para cada ruta de acceso. Deberá comprobar que la aplicación puede ejecutarse correctamente con esa versión compatible.

Para obtener la versión actual, inicie sesión en el servidor de producción y ejecute este comando:

```bash
java -version
```

> [!NOTE]
> Al migrar a WebLogic en máquinas virtuales de Azure, los requisitos para las versiones específicas de Java vienen determinados por el Java instalado previamente en las máquinas virtuales.
