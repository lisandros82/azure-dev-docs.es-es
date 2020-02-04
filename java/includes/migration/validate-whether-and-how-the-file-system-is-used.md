---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 029277ac75c455041d26f160b1f0c16a5675368d
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830903"
---
### <a name="validate-whether-and-how-the-file-system-is-used"></a>Comprobación de si se usa el sistema de archivos y cómo

Los sistemas de archivos de las máquinas virtuales funcionan de la misma manera que los sistemas de archivos locales en cuanto a la persistencia, el inicio y el apagado. Sin embargo, es importante tener en cuenta las necesidades del sistema de archivos y asegurarse de que las máquinas virtuales tengan el tamaño y la capacidad de almacenamiento adecuados.

#### <a name="read-only-static-content"></a>Contenido estático de solo lectura

Si su aplicación actualmente sirve contenido estático, necesitará una ubicación alternativa para él. Quizás quiera considerar la posibilidad de mover el contenido estático a Azure Blob Storage y agregar Azure CDN para tener descargas de alta velocidad globalmente. Para más información, consulte [Hospedaje de sitios web estáticos en Azure Storage](/azure/storage/blobs/storage-blob-static-website) e [Inicio rápido: Integración de una cuenta de una instancia de Azure Storage con Azure CDN](/azure/cdn/cdn-create-a-storage-account-with-cdn).

#### <a name="dynamically-published-static-content"></a>Contenido estático publicado dinámicamente

Si su aplicación permite que haya contenido estático que la aplicación carga o produce, pero que es inmutable una vez creado, puede usar Azure Blob Storage y Azure CDN con una función de Azure para controlar las cargas y la actualización de la red CDN. Hemos proporcionado una implementación de ejemplo para su uso en [Cargar y carga previa en CDN de contenido estático con Azure Functions](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn).
