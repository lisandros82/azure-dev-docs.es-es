---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: 478059f598b9c39609a85647945ebef3df3cb7ec
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825836"
---
### <a name="inventory-persistence-usage"></a>Uso de la persistencia de inventario

Para usar el sistema de archivos en el servidor de aplicaciones será necesario cambiar la configuración o, en raras ocasiones, la arquitectura. Puede identificar algunos o todos los escenarios siguientes.

#### <a name="read-only-static-content"></a>Contenido estático de solo lectura

Si su aplicación actualmente sirve contenido estático (por ejemplo, mediante una integración de Apache), necesitará una ubicación alternativa para ese contenido estático. Quizás quiera considerar la posibilidad de mover el contenido estático a Azure Blob Storage y agregar Azure CDN para tener descargas de alta velocidad globalmente. Para más información, consulte [Hospedaje de sitios web estáticos en Azure Storage](/azure/storage/blobs/storage-blob-static-website) e [Inicio rápido: Integración de una cuenta de una instancia de Azure Storage con Azure CDN](/azure/cdn/cdn-create-a-storage-account-with-cdn#enable-azure-cdn-for-the-storage-account).

#### <a name="dynamically-published-static-content"></a>Contenido estático publicado dinámicamente

Si su aplicación permite que haya contenido estático que la aplicación carga o produce, pero que es inmutable una vez creado, puede usar Azure Blob Storage y Azure CDN con una función de Azure para controlar las cargas y la actualización de la red CDN. Hemos proporcionado una implementación de ejemplo para su uso en [Cargar y carga previa en CDN de contenido estático con Azure Functions](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn).
