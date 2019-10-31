---
title: Ejecución de la aplicación de Azure Functions localmente en Visual Studio Code
description: Parte 3 del tutorial, ejecutar la aplicación localmente para probarla.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 0a2b74d754e8a55afb88f6ef628c5da466e7b2df
ms.sourcegitcommit: 38fc0daead4f6ef0cf16d9f4762ad24f4dc4c3e9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "71686031"
---
# <a name="test-the-function-locally"></a>Prueba local de la función

[Paso anterior: Creación de la aplicación de funciones](tutorial-vscode-serverless-node-02.md)

En este paso, ejecutará el proyecto de Azure Functions localmente para probarlo antes de implementarlo en Azure.

Cuando creó la aplicación de funciones, la extensión de Azure Functions agregó automáticamente una configuración de inicio de VS Code al proyecto, que se encuentra en el archivo *.vscode/launch.json*. Esta configuración usa el mismo tiempo de ejecución que se ejecuta en Azure, por lo que puede estar seguro de que el código fuente funciona antes de implementarlo en la nube.

1. En Visual Studio Code, presione **F5** (o use el comando **Depurar** > **Iniciar depuración**) para iniciar el depurador y adjuntarlo al host de Azure Functions. (Este comando usa automáticamente la configuración de depuración única que Azure Functions ha creado).

1. La salida de Functions Core Tools aparece en el panel **Terminal** de VS Code. Una vez iniciado el host, pulse **Ctrl** y haga clic en la dirección URL local que se muestra en la salida para abrir el explorador y ejecutar la función:

    ![Salida que se muestra en el panel Terminal de VS Code al depurar localmente](media/functions-extension/local-test-output.png)

1. El código creado por la plantilla de desencadenador HTTP predeterminado analiza un parámetro de consulta `name` para personalizar la respuesta. En el explorador, agregue `?name=<yourname>` a la dirección URL para ver la salida de la respuesta correctamente:

    ![Función de desencadenador HTTP analizando los parámetros de URL](media/functions-extension/local-test-browser.png)

1. Cuando la función se ejecuta localmente, puede establecer puntos de interrupción en distintas partes del código. (Para más información sobre los puntos de interrupción y la depuración en VS Code, consulte [Depuración](https://code.visualstudio.com/docs/editor/debugging)). Abra *index.js* y haga clic en el margen situado a la izquierda de la línea 11, en la ventana del editor. Aparece un punto rojo pequeño para indicar un punto de interrupción. Ahora, quite el argumento `?name=` de la dirección URL en el explorador. Cuando el explorador realiza esa solicitud, VS Code detiene el código de función en ese punto de interrupción:

    ![VS Code detenido en un punto de interrupción](media/functions-extension/debugging-breakpoint.png)

> [!div class="nextstepaction"]
> [He ejecutado la aplicación de funciones localmente](tutorial-vscode-serverless-node-04.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=run-app)
