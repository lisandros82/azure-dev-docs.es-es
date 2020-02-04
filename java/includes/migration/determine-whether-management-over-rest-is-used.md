---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a67bdb749231c39e1b64d7b2a982ad4c0946cc25
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830743"
---
### <a name="determine-whether-management-over-rest-is-used"></a>Determinación de si se usa la administración mediante REST

Si el ciclo de vida de la aplicación incluye el uso de administración mediante REST, debe capturar qué puertos se usan para tener acceso a la API REST y determinar cómo se autentican y exponen. Después de la migración, tendrá que asegurarse de que se exponen estos mismos puertos y mecanismos de autenticación para que el ciclo de vida de la aplicación funcione de manera similar a como funcionaba antes de la migración. Para más información, consulte [Administración de Oracle WebLogic Server con servicios de administración RESTful](https://docs.oracle.com/middleware/12213/wls/WLRUR/title.htm).
