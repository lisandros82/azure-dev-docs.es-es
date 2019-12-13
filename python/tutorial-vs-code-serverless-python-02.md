---
title: 'Tutorial: Creación de una función de Python para Azure Functions con VS Code'
description: Paso 2 del tutorial en el que se muestra el uso de la extensión Azure Functions para VS Code.
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 49ab6b150f14268b6d52ac48524f66e6e520e547
ms.sourcegitcommit: 68a4044b9fa3291c9e7e2f68ae0049328f9c01bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74992519"
---
# <a name="tutorial-create-a-python-function-for-azure-functions"></a>Tutorial: Creación de una función de Python en Azure Functions

[Paso anterior: requisitos previos](tutorial-vs-code-serverless-python-01.md)

En este artículo, creará una función de Python para Azure Functions con Visual Studio Code. El código de Azure Functions se administra dentro de un _proyecto_ de Functions, que se crea antes de crear el código.

1. En el explorador de **Azure: Functions** (se abre con el icono de Azure en el lado izquierdo), seleccione el icono del comando **Nuevo proyecto** o abra la paleta de comandos (F1 ) y seleccione **Azure Functions: Crear nuevo proyecto**.

    ![Crear un nuevo proyecto en el explorador de Azure Functions](media/tutorial-vs-code-serverless-python/create-a-new-project-in-azure-functions-explorer.png)

1. En las peticiones siguientes:

    | Prompt | Valor | DESCRIPCIÓN |
    | --- | --- | --- |
    | Especifique una carpeta para el proyecto | Carpeta abierta actual | Carpeta en la que se va a crear el proyecto. Puede que desee crear el proyecto en una subcarpeta. |
    | Seleccionar el lenguaje para el proyecto de la aplicación de funciones | **Python** | Lenguaje que se va a usar para la función, lo que determina la plantilla que se usa para el código. |
    | Seleccionar una plantilla para la primera función de su proyecto | **desencadenador HTTP** | Una función que usa un desencadenador HTTP se ejecuta cuando se realiza una solicitud HTTP al punto de conexión de la función. (Existen otros desencadenadores para Azure Functions. Para más información, consulte [¿Qué puedo hacer con las funciones?](/azure/azure-functions/functions-overview#what-can-i-do-with-functions)). |
    | Proporcionar un nombre de función | HttpExample | El nombre se usa para una subcarpeta que contiene el código de la función junto con los datos de configuración y también define el nombre del punto de conexión HTTP. Use "HttpExample" en lugar de aceptar el valor predeterminado "HTTPTrigger" para distinguir la propia función del desencadenador. |
    | Nivel de autorización | **Anónimo** | La autorización anónima hace que la función sea accesible públicamente para cualquier usuario. |
    | Seleccionar cómo desea que se abra el proyecto | **Abrir en la ventana actual** | Abre el proyecto en la ventana de Visual Studio Code actual. |

    > [!NOTE]
    > Si tiene instalado Python 3.6 y 3.7, Visual Studio Code usa Python 3.6 de forma predeterminada para el proyecto de Azure Functions. Para usar Python 3.7 en la actualidad, cree y active primero un entorno de Python 3.7 y, a continuación, use el comando `func init` desde un terminal. A continuación, reinicie Visual Studio Code desde esa carpeta mediante el comando `code .`.

1. Tras un breve período de tiempo, se muestra un mensaje para indicar que se ha creado el nuevo proyecto. En el **Explorador** está la subcarpeta creada para la función y Visual Studio Code abre el archivo *\_\_init\_\_.py*, que contiene el código predeterminado de la función:

    ![Resultado de la creación de un nuevo proyecto de Python en Azure Functions](media/tutorial-vs-code-serverless-python/display-results-of-new-python-project-in-azure-functions.png)

    > [!NOTE]
    > Si Visual Studio Code le indica que no tiene un intérprete de Python seleccionado cuando abre *\_\_init\_\_.py*, abra la paleta de comandos (**F1**) y seleccione el comando **Python: seleccionar intérprete** y, a continuación, seleccione el entorno virtual en la carpeta `.env` local (que se creó como parte del proyecto). El entorno debe basarse en Python 3.6x específicamente, como se indicó en el artículo anterior, en la sección [Requisitos previos](tutorial-vs-code-serverless-python-01.md#prerequisites).
    >
    > ![Selección del entorno virtual creado con el proyecto de Python](media/tutorial-vs-code-serverless-python/select-virtual-environment-created-with-the-python-project.png)

> [!TIP]
> Cuando desee crear otra función en el mismo proyecto, use el comando **Crear función** en el explorador de **Azure: Functions** o abra la paleta de comandos (**F1**) y seleccione el comando **Azure Functions: Create Function** (Crear función). Ambos comandos solicitan un nombre de función (que es el nombre del punto de conexión) y, a continuación, se crea una subcarpeta con los archivos predeterminados.
>
> ![Creación de funciones con la función New en el explorador de Azure Functions](media/tutorial-vs-code-serverless-python/create-new-functions-in-azure-functions-explorer.png)

> [!div class="nextstepaction"]
> [He creado la función](tutorial-vs-code-serverless-python-03.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=02-create-function)
