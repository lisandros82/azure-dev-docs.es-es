---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: e36fae7adfda8c05e222151872b137fa600cdd97
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830773"
---
### <a name="determine-whether-jms-queues-or-topics-are-in-use"></a>Determinación de si las colas o los temas de JMS están en uso

Si la aplicación utiliza colas o temas de JMS, deberá migrarlos a un servidor de JMS hospedado externamente. Azure Service Bus y Advanced Message Queuing Protocol pueden ser una estrategia de migración excelente para los usuarios que usan JMS. Para más información, consulte [Uso de Java Message Service (JMS) con Service Bus y AMQP 1.0](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp).

Si se han configurado almacenes persistentes de JMS, debe capturar su configuración y aplicarla después de la migración.

Si usa Oracle, puede migrar este software a Azure Virtual Machines y usarlo tal cual.
