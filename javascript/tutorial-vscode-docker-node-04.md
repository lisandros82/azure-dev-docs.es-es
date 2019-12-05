---
title: Implementación de una imagen de contenedor para una aplicación de Node.js desde Visual Studio Code
description: Parte 4 del tutorial, implementación de la imagen en Azure App Service
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: b43f93ba97950c84302db2e18f69b506e44dc84e
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466542"
---
# <a name="deploy-the-image-to-azure-app-service"></a>Implementación de la imagen en Azure App Service

[Paso anterior: Creación de la imagen de aplicación](tutorial-vscode-docker-node-03.md)

En este paso, implementará la imagen que insertó en un registro en [Azure App Service](https://azure.microsoft.com/services/app-service/) directamente desde Visual Studio Code.

1. En el explorador de **DOCKER**, expanda los nodos de la imagen en **Registries** (Registros), haga clic con el botón derecho en `:latest` y seleccione **Deploy Image to Azure App Service** (Implementar imagen en Azure App Service).

    ![Implementación desde el explorador](media/deploy-containers/deploy-image-command.png)

1. Cuando se le solicite, proporcione los valores del servicio de aplicaciones:

    - El nombre debe ser único en Azure.
    - Seleccione un grupo de recursos existente o cree uno nuevo. (Un **grupo de recursos** es una colección con nombre de los recursos de una aplicación en Azure).
    - Seleccione un plan de App Service ya existente o cree uno nuevo. (Un **plan de App Service** define los recursos físicos que hospedan el sitio web. Puede usar un nivel de plan básico o gratuito para este tutorial).

1. Una vez completada la implementación, Visual Studio Code muestra una notificación con la dirección URL del sitio web:

    ![Mensaje de implementación correcta](media/deploy-containers/deploy-successful.png)

1. También puede ver los resultados en el panel **Salida** de Visual Studio Code, en la sección **Docker**:

    ![Salida de implementación correcta](media/deploy-containers/deploy-output.png)

1. Para examinar el sitio web implementado, haga **Ctrl**+**Clic** en la dirección URL, en el panel **Salida**. La nueva instancia de App Service también aparece en el explorador de **AZURE** en Visual Studio Code, en la sección **APP SERVICE**. Ahí puede hacer clic con el botón derecho en el sitio web y seleccionar **Browse Website** (Examinar sitio web).

> [!div class="nextstepaction"]
> [Mi sitio se encuentra en Azure](tutorial-vscode-docker-node-05.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=deploy-app)
