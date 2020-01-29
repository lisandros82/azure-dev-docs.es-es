---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 23289c7dc4b608c6fe8bb75af479ea877a2000d9
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288624"
---
### <a name="inventory-persistence-usage"></a>Uso de la persistencia de inventario

Para usar el sistema de archivos en el servidor de aplicaciones será necesario cambiar la configuración o, en raras ocasiones, la arquitectura. Los módulos de Tomcat o el código de aplicación pueden usar el sistema de archivos. Puede identificar algunos o todos los escenarios siguientes.

#### <a name="read-only-static-content"></a>Contenido estático de solo lectura

Si su aplicación actualmente sirve contenido estático (por ejemplo, mediante una integración de Apache), necesitará una ubicación alternativa para ese contenido estático. Quizás quiera considerar la posibilidad de mover el contenido estático a Azure Blob Storage y agregar Azure CDN para tener descargas de alta velocidad globalmente. Para más información, consulte [Hospedaje de sitios web estáticos en Azure Storage](/azure/storage/blobs/storage-blob-static-website) y [Habilitación de Azure CDN para la cuenta de almacenamiento](/azure/cdn/cdn-create-a-storage-account-with-cdn#enable-azure-cdn-for-the-storage-account).

#### <a name="dynamically-published-static-content"></a>Contenido estático publicado dinámicamente

Si su aplicación permite que haya contenido estático que la aplicación carga o produce, pero que es inmutable una vez creado, puede usar Azure Blob Storage y Azure CDN con una función de Azure para controlar las cargas y la actualización de la red CDN. Hemos proporcionado una implementación de ejemplo para su uso en [Cargar y carga previa en CDN de contenido estático con Azure Functions](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn).
