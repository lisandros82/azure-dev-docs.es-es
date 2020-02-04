---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 33146c309d8fdaf21dc218c11054460cfc3dc8f4
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830983"
---
### <a name="domain-configuration"></a>Configuración del dominio

La unidad de configuración principal en WebLogic Server es el dominio. Como tal, el archivo *config.xml* contiene numerosas opciones de configuración que debe tener muy en cuenta para la migración. El archivo incluye referencias a archivos XML adicionales que se almacenan en subdirectorios. Oracle recomienda utilizar la **consola de administración** para configurar los objetos y servicios administrables de WebLogic Server, y dejar que WebLogic Server mantenga el archivo *config.xml*. Para más información, consulte [Archivos de configuración de dominio](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/domcf/config_files.html).

#### <a name="inside-your-application"></a>Dentro de la aplicación

Inspeccione el archivo *WEB-INF/weblogic.xml* y el archivo *WEB-INF/web.xml*.
