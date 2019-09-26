---
title: Depuración local del código de Python en Azure Functions con Visual Studio Code
description: Paso 4 del tutorial, ejecución local del depurador de VS Code para comprobar el código de Python.
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.openlocfilehash: 28df4c9a8a8b3a6ab6308449e9ae2e1ebd2cc6e4
ms.sourcegitcommit: d6575ac86449380b5a9c6c66aa722cb33ed53438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186144"
---
# <a name="debug-the-function-code-locally"></a>Depuración local del código de función

[Paso anterior: examen de los archivos de código](tutorial-vs-code-serverless-python-03.md)

1. Al crear el proyecto de Functions, la extensión de Visual Studio Code también crea una configuración de inicio en `.vscode/launch.json` que contiene una configuración única llamada **Conectar a funciones de Python**. Esta configuración significa que solo tiene que presionar F5 o usar el explorador de depuración para iniciar el proyecto:

    ![Explorador de depuración que muestra la configuración de inicio de Functions](media/tutorial-vs-code-serverless-python/launch-configuration.png)

1. Al iniciar el depurador, se abre un terminal que muestra la salida de Azure Functions, incluido un resumen de los puntos de conexión disponibles. La dirección URL puede ser diferente si ha usado un nombre distinto de "HttpExample":

    ```output
    Http Functions:

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. Use **Ctrl+clic** o **Cmd+clic** en la dirección URL de la ventana **Salida** de Visual Studio Code para abrir un explorador en esa dirección o bien inicie un explorador y pegue la misma dirección URL. En cualquier caso, el punto de conexión es `api/<function_name>`, en este caso será `api/HttpExample`. Sin embargo, dado que esa dirección URL no incluye un parámetro name, la ventana del explorador solo debe mostrar "Pase un nombre en la cadena de consulta o en el cuerpo de la solicitud" según corresponda para esa ruta en el código.

1. Ahora intente agregar un parámetro name, como `http://localhost:7071/api/HttpExample?name=VS%20Code` y la ventana del explorador debería mostrar el mensaje "Hello Visual Studio Code!", en el que se muestra que se ha ejecutado esa ruta de código.

1. Para pasar el valor de name en un cuerpo de solicitud JSON, puede usar una herramienta como curl con el elemento JSON insertado:

    ```bash
    # Mac OS/Linux: modify the URL if you're using a different function name
    curl --header "Content-Type: application/json" --request POST \
        --data {"name":"Visual Studio Code"} http://localhost:7071/api/HttpExample
    ```

    ```ps
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    Como alternativa, puede crear un archivo como *data.json* que contenga `{"name":"Visual Studio Code"}` y usar el comando `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample`.

1. Para probar la depuración de la función, establezca un punto de interrupción en la línea que lee `name = req.params.get('name')` y vuelva a realizar una solicitud a la dirección URL. El depurador de Visual Studio Code debe detenerse en esa línea, lo que le permite examinar las variables y recorrer el código. (Para un breve tutorial de depuración básica, consulte [Tutorial de Visual Studio Code: configurar y ejecutar el depurador](https://code.visualstudio.com/docs/python/python-tutorial#configure-and-run-the-debugger)).

1. Cuando esté seguro de que ha probado exhaustivamente la función de forma local, detenga el depurador (con el comando de menú **Depurar** > **Detener depuración** o el comando **Desconectar** de la barra de herramientas de depuración).

> [!div class="nextstepaction"]
> [He ejecutado el depurador localmente](tutorial-vs-code-serverless-python-05.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=04-test-debug)
