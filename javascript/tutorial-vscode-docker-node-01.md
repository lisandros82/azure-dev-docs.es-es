---
title: Implementación de contenedores de Docker en Azure App Service desde Visual Studio Code
description: Parte 1 del tutorial, introducción y requisitos previos.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: f37e049ab3c6dd0a01726aa9204746658540110b
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686114"
---
# <a name="deploy-containers-to-azure-app-service"></a>Implementación de contenedores en Azure App Service

En este tutorial, usará Visual Studio Code para crear una aplicación de Node.js en contenedor con Docker, insertar la imagen de contenedor en un registro y, a continuación, implementar la imagen en Azure App Service.

## <a name="prerequisites"></a>Requisitos previos

- Una [suscripción de Azure](#azure-subscription).
- [Visual Studio Code](https://code.visualstudio.com/)
- La [extensión de Docker](vscode:extension/ms-azuretools.vscode-docker).
- La [extensión de Azure App Service](vscode:extension/ms-azuretools.vscode-azureappservice).
- [Node js y npm](https://nodejs.org/en/download), el administrador de paquetes de Node.js.
- [Docker](https://www.docker.com/community-edition).

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-docker">Instalación de la extensión de Docker</a>

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azureappservice">Instalación de la extensión de Azure App Service</a>

### <a name="azure-subscription"></a>Suscripción de Azure

Si no tiene una suscripción de Azure, [regístrese ahora](https://azure.microsoft.com/en-us/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension) para obtener una cuenta gratuita con 200 USD en crédito de Azure para probar cualquier combinación de servicios.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="verify-docker-install"></a>Comprobación de la instalación de Docker

Ejecute el siguiente comando en un símbolo del sistema o un terminal para comprobar que tiene instalado Docker:

```bash
docker --version
```

La salida debería ser similar a la siguiente:

```output
Docker Version 17.12.0-ce, build c97c6d6
```

> [!div class="nextstepaction"]
> [He instalado la extensión de Docker](tutorial-vscode-docker-node-02.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=getting-started)
