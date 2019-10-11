---
title: Implementar aplicaciones web de Node.js en Azure
description: Introducción a Azure App Service y otras opciones de hospedaje para aplicaciones web, incluidas las aplicaciones web progresivas (PWA)
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/20/2019
ms.author: kraigb
ms.custom: seo-javascript-september2019
ms.openlocfilehash: 1d9cfc4d58355f374877941747138d8e8f53c8ea
ms.sourcegitcommit: 945e92dae2fa4521eebdc049c65273ae6b5470ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813694"
---
# <a name="deploy-nodejs-web-apps-to-azure-app-service"></a>Implementación de aplicaciones web de Node.js en Azure App Service

En Azure, tiene varias opciones para hospedar aplicaciones web:

- La mejor opción de hospedaje para aplicaciones web es Azure App Service, una oferta de plataforma como servicio (PaaS). Para empezar, pruebe cualquiera de los siguientes recursos:

  - [Creación de una aplicación web de Node.js en Azure](/azure/app-service/app-service-web-get-started-nodejs)
  - [Probar Azure App Service: crear una aplicación rápidamente a partir de una plantilla](https://code.visualstudio.com/tryappservice/?utm_source=msftdocs&utm_medium=microsoft&utm_campaign=tryappservice)
  - [Hospedaje de una aplicación web con Azure App Service](/learn/modules/host-a-web-app-with-azure-app-service/index)
  - [Creación de una aplicación Node.js y MongoDB en Azure](/azure/app-service/app-service-web-tutorial-nodejs-mongodb-app)
  - [Ejemplos de App Service](/samples/browse/?languages=javascript%2Cnodejs&products=azure-app-service)

- Puede compilar sus propios contenedores e implementarlos en Azure mediante Azure Container Registry y Azure Kubernetes Service. Para obtener más información, consulte [How to deploy Node.js containers to Azure](node-howto-deploy-containers.md) (Cómo implementar contenedores de Node.js en Azure).

- Si quiere trabajar principalmente con código sin servidor, consulte [How to write serverless Node.js code on Azure](node-howto-write-serverless-code.md) (Cómo escribir código Node.js sin servidor en Azure).

- Para más información sobre cómo crear un sitio JAMstack (estático), consulte [How to build JAMstack (static site) web apps with Azure](node-howto-create-static-site-jamstack.md) (Cómo compilar aplicaciones web JAMstack [sitio estático] con Azure).

- Si quiere controlar la infraestructura, simplemente, puede usar una máquina virtual. Para empezar, siga la ruta de [Implementación de un sitio web con Azure Virtual Machines](/learn/paths/deploy-a-website-with-azure-virtual-machines/) en Microsoft Learn.

Para obtener información general completa de las distintas opciones de hospedaje, consulte [Decision tree for Azure compute services](/azure/architecture/guide/technology-choices/compute-decision-tree) (Árbol de decisión para los servicios de proceso de Azure), así como el módulo [Principales Cloud Services: Opciones de proceso de Azure](/learn/modules/intro-to-azure-compute/) de Microsoft Learn.
