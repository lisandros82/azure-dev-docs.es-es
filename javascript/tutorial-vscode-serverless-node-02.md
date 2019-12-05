---
title: Creación de la aplicación de Azure Functions desde Visual Studio Code
description: Parte 2 del tutorial, creación de la aplicación de Azure Functions
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 5b2e46cde8740020cc2ad7a1b50ac9b4687d17d3
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467130"
---
# <a name="create-the-local-functions-app"></a>Creación de la aplicación de funciones local

[Paso anterior: Introducción y requisitos previos](tutorial-vscode-serverless-node-01.md)

En este paso, creará una aplicación de Azure Functions local que contiene una función que usa un desencadenador HTTP. Una aplicación de Azure Functions puede contener muchas funciones con distintos desencadenadores. El desencadenador HTTP controla específicamente el tráfico HTTP entrante.

1. Desde un terminal o un símbolo del sistema, ejecute Visual Studio Code desde una carpeta adecuada para el proyecto:

    ```bash
    # Create and navigate to a project folder

    # Run VS Code in that folder
    code .
    ```

1. En VS Code, seleccione el logotipo de Azure para abrir el explorador de **Azure Functions** y seleccione el comando **Create Project** (Crear proyecto):

    ![Creación de una aplicación de funciones local en VS Code](media/functions-extension/create-function-app-project.png)

1. En los dos primeros mensajes, seleccione la carpeta actual y, después, seleccione **JavaScript** como lenguaje.

1. En el símbolo del sistema, **Select a template for your project's first function** (Seleccione una plantilla para la primera función de su proyecto, seleccione **HTTP Trigger** (Desencadenador HTTP):

    ![Seleccione el desencadenador para la función](media/functions-extension/create-function-choose-template.png)

1. En el símbolo del sistema, **Provide a function name** (Asigne un nombre a la función), escriba **HttpExample**. (Evite usar el nombre predeterminado "HttpTrigger" porque es el mismo que el desencadenador y puede resultar confuso).

    ![Escribir un nombre de función](media/functions-extension/create-function-name.png)

1. En el símbolo del sistema, **Nivel de autorización**, seleccione **Anonymous** (Anónimo):

    ![Escribir un nombre de función](media/functions-extension/create-function-anonymous-auth.png)

1. Transcurridos unos instantes, VS Code completa la creación del proyecto. Ahora tiene una carpeta con el nombre de la función, *HttpExample*, en la que hay tres archivos:

    | Nombre de archivo | DESCRIPCIÓN |
    | --- | --- |
    | *index.js* |  El código fuente que responde a la solicitud HTTP. |
    | *functions.json* | La [configuración de enlace](/azure/azure-functions/functions-triggers-bindings) del desencadenador HTTP. |
    | *sample.dat* | Un archivo de datos de marcador de posición para mostrar que puede tener otros archivos en la carpeta. Puede eliminar este archivo si lo desea, porque no se usa en este tutorial. |

    ![Resultado de la creación de una aplicación de funciones](media/functions-extension/create-function-app-results.png)

> [!div class="nextstepaction"]
> [He creado la aplicación de funciones](tutorial-vscode-serverless-node-03.md) [he tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=create-app)
