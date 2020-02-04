---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 4c20feba0a91d10846963f8697457735e5932c20
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830733"
---
### <a name="determine-whether-session-replication-is-used"></a>Determinación de si se usa la replicación de sesión

Si la aplicación utiliza replicación de sesión, con o sin Oracle Coherence*Web, tiene tres opciones:

1. Coherence*Web puede ejecutarse junto con un servidor de WebLogic en las máquinas virtuales de Azure, pero debe configurar esta opción manualmente después de aprovisionar la oferta. Si usa la versión independiente de Coherence, también puede ejecutarla en una máquina virtual de Azure, pero debe configurar esta opción manualmente después de aprovisionar la oferta.
2. Refactorice la aplicación para utilizar una base de datos para la administración de sesiones.
3. Refactorice la aplicación para externalizar la sesión en el servicio Azure Redis. Para más información, consulte [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-overview).

Para todas estas opciones, es una buena idea dominar cómo WebLogic realiza la replicación del estado de sesión HTTP. Para más información, consulte [Replicación del estado de sesión HTTP](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/failover.html#GUID-E13D8142-66BA-46A1-854F-4FC6F82992DD) en la documentación de Oracle.
