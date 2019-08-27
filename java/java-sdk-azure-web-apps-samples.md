---
title: Ejemplos de aplicaciones web de las bibliotecas de administración de Azure para Java
description: Obtenga código de ejemplo para la creación y actualización de aplicaciones web hospedadas en Azure App Service mediante las bibliotecas de administración de Azure para Java.
keywords: Azure, Java, SDK, API, Maven, Gradle, aplicaciones web, servicio de aplicaciones
author: rloutlaw
ms.author: brendm
manager: douge
ms.date: 04/16/2017
ms.topic: article
ms.devlang: java
ms.service: multiple
ms.assetid: 43633e5c-9fb1-4807-ba63-e24c126754e2
ms.custom: seo-java-august2019
ms.openlocfilehash: 1a21ca6e11cf66a603509bd5414d1b5caf9e8a30
ms.sourcegitcommit: f519a1ee8017850b2fa37049af3bac1ea5ca5516
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69889856"
---
# <a name="azure-management-libraries-for-java---web-app-samples"></a>Bibliotecas de administración de Azure para Java: ejemplos de aplicaciones web 

| **Creación de una aplicación** ||
|---|---|
| [Creación de una aplicación web e implementación desde FTP o GitHub][1] | Implemente aplicaciones web desde Git local o FTP e integración continua desde GitHub. |
| [Creación de una aplicación web y administración de los espacios de implementación][2] | Cree una aplicación web, implemente en espacios de ensayo y, después, intercambie las implementaciones entre los espacios. |
| **Configuración de la aplicación** ||
| [Creación de una aplicación web y configuración de un dominio personalizado][3] | Cree una aplicación web con un dominio personalizado y un certificado SSL autofirmado. |
| **Escalado de aplicaciones** ||
| [Escalado de una aplicación web con alta disponibilidad en varias regiones][4] | Escale una aplicación web en tres regiones geográficas diferentes y haga que estén disponibles a través de un punto de conexión único mediante Azure Traffic Manager. | 
| **Conexión de la aplicación a recursos** ||
| [Conexión de una aplicación web a una cuenta de almacenamiento][5] | Cree una cuenta de Azure Storage y agregue la cadena de conexión de la cuenta a la configuración de la aplicación. |
| [Conexión de una aplicación web a SQL Database][6] | Cree una aplicación web y una base de datos SQL y agregue la cadena de conexión de la base de datos SQL a la configuración de la aplicación. |

[1]: java-sdk-configure-webapp-sources.md
[2]: https://azure.microsoft.com/resources/samples/app-service-java-manage-staging-and-production-slots-for-web-apps/
[3]: https://azure.microsoft.com/resources/samples/app-service-java-manage-web-apps-with-custom-domains/
[4]: https://azure.microsoft.com/resources/samples/app-service-java-scale-web-apps-on-linux/
[5]: https://azure.microsoft.com/resources/samples/app-service-java-manage-storage-connections-for-web-apps/
[6]: https://azure.microsoft.com/resources/samples/app-service-java-manage-data-connections-for-web-apps/