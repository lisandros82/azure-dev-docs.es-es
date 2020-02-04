---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 9403614c7ae2990a35fcbcce6d2e104f87724da5
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830923"
---
### <a name="determine-the-network-topology"></a>Determinación de la topología de red

El conjunto actual de ofertas de Marketplace es un punto de partida para la migración. Si la oferta no cubre los aspectos de la arquitectura que necesita migrar, deberá capturar la topología de red de la implementación existente y reproducirla en Azure, incluso después de poner en marcha la oferta básica con una de las plantillas de solución.

Este es un tema muy amplio, pero las referencias siguientes pueden ayudar a dirigir los esfuerzos de migración por el camino correcto:

* Esta referencia enumera los temas generales relacionados con la migración de la topología de red a Azure: [Guía de implementación de Fast Track](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/deploying.html#GUID-E0BE4A3E-44CD-4C95-9540-7A850BF02F6A).
* En esta referencia se describen aspectos importantes relacionados con la agrupación en clústeres, que afectan a la topología de red: [Agrupación en clústeres de servidores de WebLogic](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/clustering.html#GUID-E39A18C2-B990-485F-BFB1-0549250FABFE).
* Dado que los orígenes de datos son servidores independientes en un sistema WebLogic, debe incluirlos en el análisis de la topología de red. [Orígenes de datos de servidores de WebLogic](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jdbc.html#GUID-9FD5F552-B2E4-4FEC-8C10-503A08764B52).
* Los orígenes de mensajería también son servidores independientes. [Mensajería de servidor de WebLogic](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jms.html#GUID-3B5F647D-E001-413B-AC6A-1E103BDBA93F).
* El equilibrio de carga es un requisito fundamental. Esta referencia cubre el lado de WebLogic Server del equilibrio de carga: [Equilibrio de carga en un clúster](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/load_balancing.html#GUID-B8F6DE4B-1AAC-428B-878B-BFDCE161C054).
