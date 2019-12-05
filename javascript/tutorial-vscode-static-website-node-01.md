---
title: Implementación de un sitio web de Node.js estático en Azure desde Visual Studio Code
description: Parte 1 del tutorial, introducción y requisitos previos.
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 8b19b32933c2405082e2d1e2aa46c7a06af1f9d1
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467092"
---
# <a name="deploy-a-static-website-to-azure-from-visual-studio-code"></a>Implementación de un sitio web estático en Azure desde Visual Studio Code

En este tutorial, creará e implementará un sitio web estático en Azure mediante [Azure Storage](https://docs.microsoft.com/azure/storage). Un sitio web estático se compone de HTML, CSS, JavaScript y otros archivos estáticos, como imágenes o fuentes. Normalmente, un sitio estático es una aplicación de una sola página (o [SPA](https://en.wikipedia.org/wiki/Single-page_application)) escrita con Angular, React o Vue. Sin embargo, la aplicación se diseña y los archivos se hospedan directamente desde el _almacenamiento_ en lugar de usar un servidor web. El hospedaje en el almacenamiento es más sencillo y menos costoso que el mantenimiento de un servidor web.

> [!NOTE]
> Si tiene su propio código de servidor, como una aplicación de Node.js/Express, siga el [tutorial de App Service](tutorial-vscode-azure-app-service-node-01.md) en su lugar.

## <a name="prerequisites"></a>Requisitos previos

- Una [suscripción de Azure](#azure-subscription).
- [Visual Studio Code](https://code.visualstudio.com/)
- La [extensión de Azure Storage](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage).
- [Node js y npm](https://nodejs.org/en/download), el administrador de paquetes de Node.js. (Este requisito se usa únicamente para generar un proyecto de ejemplo. No es necesario instalar Node.js si ya tiene código de aplicación).

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azurestorage">Instalación de la extensión de Azure Storage</a>

### <a name="azure-subscription"></a>Suscripción de Azure

Si no tiene una suscripción de Azure, [regístrese ahora](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-static-website&mktingSource=vscode-tutorial-static-website) para obtener una cuenta gratuita con 200 USD en crédito de Azure para probar cualquier combinación de servicios.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [He instalado los requisitos previos](tutorial-vscode-static-website-node-02.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=getting-started)
