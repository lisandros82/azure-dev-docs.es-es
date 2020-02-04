---
author: yevster
ms.author: yebronsh
ms.date: 1/22/2020
ms.openlocfilehash: 1015c179c0f93485decd77bd89a3ceec8833652e
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825908"
---
### <a name="migrate-scheduled-jobs"></a>Migración de los trabajos programados

Para ejecutar trabajos programados en Azure, considere la posibilidad de usar un [desencadenador de temporizador para Azure Functions](/azure/azure-functions/functions-bindings-timer). No es necesario migrar el propio código de trabajo a una función. La función simplemente puede invocar una dirección URL en la aplicación para desencadenar el trabajo. Si estas ejecuciones de trabajos se deben invocar dinámicamente o hay que hacer un seguimiento centralizado, considere la posibilidad de usar [Spring Batch](https://spring.io/projects/spring-batch).

También puede crear una aplicación lógica con un desencadenador de periodicidad para invocar la dirección URL sin escribir ningún código fuera de la aplicación. Para más información, consulte [Introducción: ¿Qué es Azure Logic Apps?](/azure/logic-apps/logic-apps-overview) y [Creación, programación y ejecución de tareas y flujos de trabajo periódicos con el desencadenador de periodicidad en Azure Logic Apps](/azure/connectors/connectors-native-recurrence).

> [!NOTE]
> Para evitar el uso malintencionado, asegúrese de que el punto de conexión de invocación del trabajo requiera credenciales. En este caso, la función de desencadenador deberá proporcionar las credenciales.
