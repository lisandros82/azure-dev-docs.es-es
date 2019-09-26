---
title: Implementación de aplicaciones de Python en Azure App Service en Linux desde Visual Studio Code
description: Paso 1 del tutorial, introducción, requisitos previos e inicio de sesión en Azure.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 1b1e3d7fa1daa408584e4caf22c553d7f47bccea
ms.sourcegitcommit: d6575ac86449380b5a9c6c66aa722cb33ed53438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186191"
---
# <a name="deploy-to-azure-app-service-on-linux"></a>Implementación en App Service en Linux

Este tutorial le guía por el uso de Visual Studio Code para implementar una aplicación de Python en Azure App Service en Linux con la extensión [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).

Si tiene problemas con cualquiera de los pasos de este tutorial, nos encantaría conocer los detalles. Use el vínculo **He tenido un problema** al final de cada artículo para enviar sus comentarios.

> [!TIP]
> [Azure App Service en Linux](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro), actualmente en versión preliminar para Python, ejecuta el código fuente en un contenedor de Docker predefinido. Ese contenedor ejecuta aplicaciones con Python 3.7 mediante el servidor web [Gunicorn](https://gunicorn.org). Las características de este contenedor se describen en [Configuración de aplicaciones de Python para App Service en Linux](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python). La propia definición de contenedor se encuentra en [github.com/Azure-App-Service/python](https://github.com/Azure-App-Service/python/tree/master/3.7).

## <a name="prerequisites"></a>Requisitos previos

- Una [suscripción de Azure](#azure-subscription).
- [Visual Studio Code con la extensión Azure App Service](#visual-studio-code-python-and-the-azure-app-service-extension).
- Un entorno de Python

### <a name="azure-subscription"></a>Suscripción de Azure

Si no tiene una suscripción de Azure, [regístrese ahora](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension) para obtener una cuenta gratuita con 200 USD en crédito de Azure para probar cualquier combinación de servicios.

### <a name="visual-studio-code-python-and-the-azure-app-service-extension"></a>Visual Studio Code, Python y la extensión Azure App Service

Instale el siguiente software:

- [Visual Studio Code](https://code.visualstudio.com/)
- Python y la extensión [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python), tal y como se describe en [Tutorial de Python en Visual Studio Code: requisitos previos](https://code.visualstudio.com/docs/python/python-tutorial).
- La extensión [Azure App Service](vscode:extension/ms-azuretools.vscode-azureappservice), que proporciona interacción con Azure App Service desde VS Code. Para más información, consulte el [tutorial de la extensión App Service](https://code.visualstudio.com/tutorials/app-service-extension/getting-started) y visite el [repositorio de GitHub vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice).

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [He iniciado sesión en Azure](tutorial-deploy-app-service-on-linux-02.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=01-verify-prerequisites)
