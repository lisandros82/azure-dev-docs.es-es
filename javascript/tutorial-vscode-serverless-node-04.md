---
title: Implementación de la aplicación de Azure Functions desde Visual Studio Code
description: Parte 4 del tutorial, implementación de la aplicación de Functions en la nube.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 53d0dd11567084d42de71a0f737cf8b9f5fc5249
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685930"
---
# <a name="deploy-the-functions-app"></a>Implementación de la aplicación de Functions

[Paso anterior: Prueba local de la función](tutorial-vscode-serverless-node-03.md)

1. In VS Code, seleccione el logotipo de Azure para que se abra **Azure Explorer** y, después, en **Functions**, seleccione la flecha azul hacia arriba para implementar la aplicación:

    ![Implementación en un comando de Azure Functions](media/functions-extension/deploy-app.png)

    De forma alternativa, puede realizar la implementación abriendo la **paleta de comandos** (**F1**), escribiendo "deploy to function app" (implementar en aplicación de funciones) y ejecutando el comando **Azure Functions: Implementar en aplicación de funciones**.

1. En el indicador, en **Select Function App in Azure** (Seleccionar aplicación de funciones en Azure), elija **Create new Function app in Azure** (Crear una aplicación de funciones en Azure).

1. En el siguiente indicador, escriba un nombre único global para la aplicación de funciones y presione **ENTRAR**. Los siguientes son caracteres válidos para un nombre de aplicación de funciones: "a-z", "0-9" y "-".

1. En el siguiente indicador, seleccione una [región](https://azure.microsoft.com/en-us/regions/) de Azure que esté cerca de usted.

1. En el panel **Salida** de VS Code para **Azure Functions** aparece el progreso:

    ![Panel Salida de VS Code que muestra el progreso de la implementación](media/functions-extension/deploy-progress.png)

1. Una vez completada la implementación, vaya al explorador de **Azure Functions**, expanda el nodo de la suscripción de Azure, expanda el nodo de la aplicación de Functions y, a continuación, expanda **Functions (read only)** [Functions (solo lectura)]. Haga clic con el botón derecho en el nombre de la función y seleccione **Copy Function Url** (Copiar la dirección URL de la función):

    ![Comando Copy Function URL (Copiar la dirección URL de la función)](media/functions-extension/copy-function-url-command.png)

1. Pegue la dirección URL en un explorador y anexe un argumento `?name=<yourname>`. Después, el explorador debe mostrar la función que se ejecuta en la nube:

    ![Función en ejecución en la nube](media/functions-extension/remote-test-browser.png)

1. Si lo desea, realice algunos cambios en el código de la función en *index.js* o agregue funciones adicionales con otros desencadenadores. Después de realizar pruebas localmente, implemente el código de nuevo como en los pasos anteriores para probar esos cambios en la nube.

    > [!TIP]
    > Al implementar, toda la aplicación de Functions se implementa por lo que los cambios en todas las funciones individuales se implementarán al mismo tiempo.

> [!div class="nextstepaction"]
> [He implementado la aplicación de funciones](tutorial-vscode-serverless-node-05.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=deploy-app)
