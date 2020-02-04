---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 2dccfba5cfe61ac862aeabe7b928e121502093b7
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830883"
---
### <a name="determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries"></a>Determinación de si usa sus propias bibliotecas de Java EE compartidas personalizadas

Si utiliza la característica de biblioteca de Java EE compartida, tiene dos opciones:

1. Refactorizar el código de la aplicación para quitar todas las dependencias de las bibliotecas y, en su lugar, incorpore la funcionalidad directamente a la aplicación.
2. Agregar las bibliotecas a la ruta de clases del servidor.
