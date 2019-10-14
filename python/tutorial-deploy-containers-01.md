---
title: 'Tutorial: Implementación de contenedores de Docker en Azure App Service con Visual Studio Code'
description: Paso 1 del tutorial, introducción y requisitos previos.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: f6cdd345fddf0123cb26549ddbc498f156737799
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172298"
---
# <a name="tutorial-deploy-docker-containers-to-azure-app-service-with-visual-studio-code"></a>Tutorial: Implementación de contenedores de Docker en Azure App Service con Visual Studio Code

En este artículo se describe cómo usar Visual Studio Code para implementar una imagen de contenedor desde un registro de contenedor en [Azure App Service](https://azure.microsoft.com/services/app-service/containers/), todo dentro de Visual Studio Code.

Si tiene problemas con cualquiera de los pasos de este tutorial, nos encantaría conocer los detalles. Use el vínculo **He tenido un problema** al final de cada artículo para enviar sus comentarios.

## <a name="prerequisites"></a>Requisitos previos

- Una [cuenta de Azure](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- Un contenedor adecuado cargado en un registro de contenedor. Encontrará información sobre cómo crear un contenedor con una aplicación web de Python y agregarlo a un registro en [Creación de un contenedor](https://code.visualstudio.com/docs/python/tutorial-create-containers).
- La [extensión de Azure App Service para VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).
- La [extensión de Docker para VS Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [He iniciado sesión en Azure](tutorial-deploy-containers-02.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=01-verify-prerequisites)
