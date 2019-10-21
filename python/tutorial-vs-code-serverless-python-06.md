---
title: 'Tutorial: Adición de una segunda función de Python en Azure Functions con Visual Studio Code'
description: Paso 6 del tutorial, adición de una segunda función para expandir un proyecto de Azure Functions.
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: f247ff7730a9f1bca8cc7ed6255ed52966d94d6d
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278504"
---
# <a name="tutorial-add-a-second-python-function-to-azure-functions"></a>Tutorial: Adición de una segunda función de Python en Azure Functions

[Paso anterior: implementación en Azure](tutorial-vs-code-serverless-python-05.md)

Después de la primera implementación, puede realizar cambios en el código, como agregar funciones adicionales de Python y volver a implementar en la misma aplicación de funciones de Azure.

1. En el área **Azure: Functions**, seleccione el comando **Crear función** o utilice **Azure Functions: Crear función** desde la paleta de comandos. Especifique los siguientes detalles para la función:

    - Plantilla: Desencadenador HTTP
    - Nombre: "DigitsOfPi"
    - Nivel de autorización: Anónimas

1. En el explorador de archivos de Visual Studio Code hay una subcarpeta con el nombre de la función que, de nuevo, contiene los archivos llamados *\_\_init\_\_.py*, *function.json* y *sample.dat*.

1. Reemplace el contenido de *\_\_init\_\_.py* para que coincida con el código siguiente, que genera una cadena que contiene el valor de PI en los dígitos especificados en la dirección URL (este código solo usa un parámetro de dirección URL).

    ```python
    import logging

    import azure.functions as func

    """ Adapted from the second, shorter solution at http://www.codecodex.com/wiki/Calculate_digits_of_pi#Python
    """

    def pi_digits_Python(digits):
        scale = 10000
        maxarr = int((digits / 4) * 14)
        arrinit = 2000
        carry = 0
        arr = [arrinit] * (maxarr + 1)
        output = ""

        for i in range(maxarr, 1, -14):
            total = 0
            for j in range(i, 0, -1):
                total = (total * j) + (scale * arr[j])
                arr[j] = total % ((j * 2) - 1)
                total = total / ((j * 2) - 1)

            output += "%04d" % (carry + (total / scale))
            carry = total % scale

        return output;

    def main(req: func.HttpRequest) -> func.HttpResponse:
        logging.info('DigitsOfPi HTTP trigger function processed a request.')

        digits_param = req.params.get('digits')

        if digits_param is not None:
            try:
                digits = int(digits_param)
            except ValueError:
                digits = 10   # A default

            if digits > 0:
                digit_string = pi_digits_Python(digits)

                # Insert a decimal point in the return value
                return func.HttpResponse(digit_string[:1] + '.' + digit_string[1:])

        return func.HttpResponse(
             "Please pass the URL parameter ?digits= to specify a positive number of digits.",
             status_code=400
        )
    ```

1. Dado que el código solo admite HTTP GET, modifique *function.json* para que la colección `"methods"` contenga solo `"get"` (es decir, elimine `"post"`). El archivo completo debe ser similar al siguiente:

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
            "get"
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

1. Inicie el depurador; para ello, presione F5 o seleccione el comando de menú **Depurar** > **Iniciar depuración**. La ventana **Salida** ahora debería mostrar ambos puntos de conexión en el proyecto:

    ```output
    Http Functions:

            DigitsOfPi: [GET] http://localhost:7071/api/DigitsOfPi

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. Desde un explorador o con curl, haga una solicitud a `http://localhost:7071/api/DigitsOfPi?digits=125` y observe la salida. (Es posible que observe que el algoritmo del código no es totalmente preciso, pero dejaremos las mejoras). Detenga el depurador cuando haya terminado.

1. Vuelva a implementar el código mediante la opción **Implementar en aplicación de funciones** en el explorador de **Azure: Functions**. Si se le solicita, seleccione la aplicación de funciones creada previamente.

1. Una vez finalizada la implementación (tarda unos minutos), la ventana **Salida** muestra los puntos de conexión públicos con los que puede repetir las pruebas.

> [!div class="nextstepaction"]
> [He agregado una segunda función](tutorial-vs-code-serverless-python-07.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=06-second-function)
