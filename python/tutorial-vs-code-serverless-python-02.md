---
title: 'Tutorial: Creación de una función de Python en Azure Functions con Visual Studio Code'
description: Paso 2 del tutorial en el que se muestra el uso de la extensión Azure Functions para VS Code.
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 691e64ae9b407ba4277ddde2a62a583623e53484
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172481"
---
# <a name="tutorial-create-a-python-function-for-azure-functions"></a>Tutorial: Creación de una función de Python en Azure Functions

[Paso anterior: requisitos previos](tutorial-vs-code-serverless-python-01.md)

1. El código de Azure Functions se administra dentro de un _proyecto_ de Functions, que se crea antes de crear el código. En el explorador de **Azure: Functions** (se abre con el icono de Azure en el lado izquierdo), seleccione el icono del comando **Nuevo proyecto** o abra la paleta de comandos (F1 ) y seleccione **Azure Functions: Crear nuevo proyecto**.

    ![Botón Crear nuevo proyecto en el explorador de Functions](media/tutorial-vs-code-serverless-python/project-create-new.png)

1. En las peticiones siguientes:

    | Prompt | Valor | DESCRIPCIÓN |
    | --- | --- | --- |
    | Especifique una carpeta para el proyecto | Carpeta abierta actual | Carpeta en la que se va a crear el proyecto. Puede que desee crear el proyecto en una subcarpeta. |
    | Seleccionar el lenguaje para el proyecto de la aplicación de funciones | **Python** | Lenguaje que se va a usar para la función, lo que determina la plantilla que se usa para el código. |
    | Seleccionar una plantilla para la primera función de su proyecto | **desencadenador HTTP** | Una función que usa un desencadenador HTTP se ejecuta cuando se realiza una solicitud HTTP al punto de conexión de la función. (Existen otros desencadenadores para Azure Functions. Para más información, consulte [¿Qué puedo hacer con las funciones?](/azure/azure-functions/functions-overview#what-can-i-do-with-functions)). |
    | Proporcionar un nombre de función | HttpExample | El nombre se usa para una subcarpeta que contiene el código de la función junto con los datos de configuración y también define el nombre del punto de conexión HTTP. Use "HttpExample" en lugar de aceptar el valor predeterminado "HTTPTrigger" para distinguir la propia función del desencadenador. |
    | Nivel de autorización | **Anónimo** | La autorización anónima hace que la función sea accesible públicamente para cualquier usuario. |
    | Seleccionar cómo desea que se abra el proyecto | **Abrir en la ventana actual** | Abre el proyecto en la ventana de Visual Studio Code actual. |

1. Tras un breve período de tiempo, se muestra un mensaje para indicar que se ha creado el nuevo proyecto. En el **Explorador** está la subcarpeta creada para la función y Visual Studio Code abre el archivo *\_\_init\_\_.py*, que contiene el código predeterminado de la función:

    [![Resultado de la creación de un nuevo proyecto de funciones de Python](media/tutorial-vs-code-serverless-python/project-create-results.png)](media/tutorial-vs-code-serverless-python/project-create-results.png)

    > [!NOTE]
    > Si Visual Studio Code le indica que no tiene un intérprete de Python seleccionado cuando abre *\_\_init\_\_.py*, abra la paleta de comandos (**F1**) y seleccione el comando **Python: seleccionar intérprete** y, a continuación, seleccione el entorno virtual en la carpeta `.env` local (que se creó como parte del proyecto). El entorno debe basarse en Python 3.6x específicamente, como se indicó en el artículo anterior, en la sección [Requisitos previos](tutorial-vs-code-serverless-python-01.md#prerequisites).
    >
    > ![Selección del entorno virtual creado con el proyecto](media/tutorial-vs-code-serverless-python/select-venv-interpreter.png)

> [!TIP]
> Cuando desee crear otra función en el mismo proyecto, use el comando **Crear función** en el explorador de **Azure: Functions** o abra la paleta de comandos (**F1**) y seleccione el comando **Azure Functions: Create Function** (Crear función). Ambos comandos solicitan un nombre de función (que es el nombre del punto de conexión) y, a continuación, se crea una subcarpeta con los archivos predeterminados.
>
> ![Comando Nueva función en el explorador de Azure: Functions](media/tutorial-vs-code-serverless-python/function-create-new.png)

> [!div class="nextstepaction"]
> [He creado la función](tutorial-vs-code-serverless-python-03.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=02-create-function)
