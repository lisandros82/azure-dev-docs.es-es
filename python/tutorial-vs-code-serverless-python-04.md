---
title: 'Tutorial: Depuración local del código de Python para Azure Functions con VS Code'
description: Paso 4 del tutorial, ejecución local del depurador de VS Code para comprobar el código de Python.
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: ffd5d433166c44edd8c021fd29cb7e43395df7ff
ms.sourcegitcommit: ac68fb174d606c7af2bfa79fe32b8ca7b73c86a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946685"
---
# <a name="tutorial-debug-the-azure-functions-python-code-locally"></a>Tutorial: Depuración local del código de Python en Azure Functions

[Paso anterior: examen de los archivos de código](tutorial-vs-code-serverless-python-03.md)

Puede depurar localmente el código de Python en Azure Functions con Visual Studio Code.

1. Al crear el proyecto de Functions, la extensión de Visual Studio Code también crea una configuración de inicio en `.vscode/launch.json` que contiene una configuración única llamada **Conectar a funciones de Python**. Esta configuración significa que solo tiene que presionar F5 o usar el explorador de depuración para iniciar el proyecto:

    ![Configuración del explorador de depuración para iniciar un proyecto de Python](media/tutorial-vs-code-serverless-python/configuration-to-start-a-python-project-for-debugging.png)

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
        --data '{"name":"Visual Studio Code"}' http://localhost:7071/api/HttpExample
    ```

    ```ps
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    En PowerShell, también puede usar el cmdlet [Invoke-WebRequest](/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-6).

    ---

    Como alternativa, puede crear un archivo como *data.json* que contenga `{"name":"Visual Studio Code"}` y usar el comando `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample`.

1. Para probar la depuración de la función, establezca un punto de interrupción en la línea que lee `name = req.params.get('name')` y vuelva a realizar una solicitud a la dirección URL. El depurador de Visual Studio Code debe detenerse en esa línea, lo que le permite examinar las variables y recorrer el código. (Para un breve tutorial de depuración básica, consulte [Tutorial de Visual Studio Code: configurar y ejecutar el depurador](https://code.visualstudio.com/docs/python/python-tutorial#configure-and-run-the-debugger)).

1. Cuando esté seguro de que ha probado exhaustivamente la función de forma local, detenga el depurador (con el comando de menú **Depurar** > **Detener depuración** o el comando **Desconectar** de la barra de herramientas de depuración).

> [!div class="nextstepaction"]
> [He ejecutado el depurador localmente](tutorial-vs-code-serverless-python-05.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=04-test-debug)
