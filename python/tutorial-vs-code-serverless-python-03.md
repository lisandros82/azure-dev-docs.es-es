---
title: 'Tutorial: Examen de los archivos de código de Python para Azure Functions en VS Code'
description: Paso 3 del tutorial, descripción de la plantilla de código de Python proporcionada por Azure Functions.
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 253bc4384f896c9114f2f1113cdf0ee2f290819d
ms.sourcegitcommit: 68a4044b9fa3291c9e7e2f68ae0049328f9c01bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2019
ms.locfileid: "74992496"
---
# <a name="tutorial-examine-the-python-code-files-in-visual-studio-code"></a>Tutorial: Examen de los archivos de código de Python en Visual Studio Code

[Paso anterior: creación de la función](tutorial-vs-code-serverless-python-02.md)

Examine los archivos de Python en la carpeta de la función mediante Visual Studio Code.

En la subcarpeta de la función recién creada hay tres archivos: *\_\_init\_\_.py* contiene el código de la función, *function.json* describe la función para Azure Functions y *sample.dat* es un archivo de datos de ejemplo. Puede eliminar *sample.dat* si lo desea, ya que solo existe para mostrar que puede agregar otros archivos en la subcarpeta.

Echemos un vistazo en primer lugar a *function.json* y luego al código de *\_\_init\_\_.py*.

## <a name="functionjson"></a>function.json

El archivo function.json proporciona la información de configuración necesaria para el punto de conexión de Azure Functions:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

La propiedad `scriptFile` identifica el archivo de inicio del código y ese código debe contener una función de Python llamada `main`. Puede dividir el código en varios archivos, siempre y cuando el archivo especificado aquí contenga una función `main`.

El elemento `bindings` contiene dos objetos, uno para describir las solicitudes entrantes y otro para describir la respuesta HTTP. En el caso de las solicitudes entrantes (`"direction": "in"`), la función responde a las solicitudes HTTP GET o POST y no requiere autenticación. La respuesta (`"direction": "out"`) es una respuesta HTTP que devuelve el valor que se devuelve desde la función de Python `main`.

## <a name="__init__py"></a>\_\_init\_\_.py

Cuando se crea una nueva función, Azure Functions proporciona el código de Python predeterminado en *\_\_init\_\_.py*:

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

Las partes importantes del código son las siguientes:

- Debe importar el módulo `azure.functions`; la importación del módulo de registro es opcional, pero se recomienda.
- La función de Python obligatoria `main` recibe un objeto `func.HttpRequest` llamado `req` y devuelve un valor de tipo `func.HttpResponse`. Puede encontrar más información sobre las funcionalidades de estos objetos en las referencias de [func.HttpRequest](/python/api/azure-functions/azure.functions.httprequest?view=azure-python) y [func.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse?view=azure-python).
- Después, el cuerpo de `main` procesa la solicitud y genera una respuesta. En este caso, el código busca un parámetro `name` en la dirección URL. Si no es así, comprueba si el cuerpo de la solicitud contiene un elemento JSON (mediante `func.HttpRequest.get_json`) y si el elemento JSON contiene un valor `name` (mediante el método `get` del objeto JSON devuelto por `get_json`).
- Si se encuentra un nombre, el código devuelve la cadena "Hello" con el nombre anexado; en caso contrario, devuelve un mensaje de error.

> [!div class="nextstepaction"]
> [He examinado los archivos de código](tutorial-vs-code-serverless-python-04.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=03-examine-code-files)
