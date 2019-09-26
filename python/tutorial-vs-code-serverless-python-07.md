---
title: Adición de un enlace de almacenamiento para Azure Functions en Python con Visual Studio Code
description: Paso 7 del tutorial, agregar un enlace en Python para escribir mensajes en Azure Storage.
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.openlocfilehash: 4595b6a60aa83d3818b41ddd1e4f06a44bb1eec1
ms.sourcegitcommit: d6575ac86449380b5a9c6c66aa722cb33ed53438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186123"
---
# <a name="add-a-binding-to-write-messages-to-azure-storage"></a>Adición de un enlace para escribir mensajes en Azure Storage

[Paso anterior: implementación de una segunda función](tutorial-vs-code-serverless-python-06.md)

Un _enlace_ le permite conectar el código de la función a los recursos, como Azure Storage, sin escribir ningún código de acceso a datos. Un enlace se define en el archivo *function.json* y puede representar tanto la entrada como la salida. Una función puede utilizar varios enlaces de entrada y de salida, pero solo un desencadenador. Para más información, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](/azure/azure-functions/functions-triggers-bindings).

En esta sección, agregará un enlace de almacenamiento a la función HttpExample que creó anteriormente en este tutorial. La función utiliza este enlace para escribir mensajes en el almacenamiento con cada solicitud. El almacenamiento en cuestión usa la misma cuenta de almacenamiento predeterminada que la aplicación de funciones. Sin embargo, si planea hacer un uso intensivo del almacenamiento, debería considerar la posibilidad de crear una cuenta independiente.

1. Sincronice la configuración remota del proyecto de Azure Functions con el archivo *local.settings.json*; para ello, abra la paleta de comandos y seleccione **Azure Functions: Descargar configuración remota**. Abra *local.settings.json* y compruebe que contiene un valor para `AzureWebJobsStorage`. Ese valor es la cadena de conexión de la cuenta de almacenamiento.

1. En la carpeta `HttpExample`, haga clic con el botón derecho en *function.json* y seleccione **Agregar enlace**:

    ![Comando Agregar enlace en el explorador de Visual Studio Code](media/tutorial-vs-code-serverless-python/add-binding-command.png)

1. En los mensajes que siguen en Visual Studio Code, seleccione o proporcione los siguientes valores:

    | Prompt | Valor que se va a proporcionar |
    | --- | --- |
    | Establecer dirección de enlace | out |
    | Seleccionar enlace con dirección de salida | Azure Queue Storage |
    | Nombre utilizado para identificar este enlace en el código | msg |
    | Cola a la que se enviará el mensaje | outqueue |
    | Seleccione la configuración de *local.settings.json* (para la conexión de almacenamiento) | AzureWebJobsStorage |

1. Después de hacer estas selecciones, compruebe que se agrega el siguiente enlace al archivo *function.json*:

    ```json
        {
          "type": "queue",
          "direction": "out",
          "name": "msg",
          "queueName": "outqueue",
          "connection": "AzureWebJobsStorage"
        }
    ```

1. Ahora que ha configurado el enlace, puede usarlo en el código de la función. De nuevo, el enlace recién definido aparece en el código como un argumento de la función `main` de *\_\_init\_\_.py*. Por ejemplo, puede modificar el archivo *\_\_init\_\_.py* de HttpExample para que coincida con lo siguiente, que muestra cómo usar el argumento `msg` para escribir un mensaje con marca de tiempo con el nombre usado en la solicitud. Los comentarios explican los cambios específicos:

    ```python
    import logging
    import datetime  # MODIFICATION: added import
    import azure.functions as func

    # MODIFICATION: the added binding appears as an argument; func.Out[func.QueueMessage]
    # is the appropriate type for an output binding with "type": "queue" (in function.json).
    def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> func.HttpResponse:
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
            # MODIFICATION: write the a message to the message queue, using msg.set
            msg.set(f"Request made for {name} at {datetime.datetime.now()}")

            return func.HttpResponse(f"Hello {name}!")
        else:
            return func.HttpResponse(
                 "Please pass a name on the query string or in the request body",
                 status_code=400
            )
    ```

1. Para probar estos cambios localmente, vuelva a iniciar el depurador en Visual Studio Code; para ello, presione F5 o seleccione el comando de menú **Depurar** > **Iniciar depuración**. Como antes, la ventana **Salida** ahora debería mostrar los puntos de conexión del proyecto.

1. En un explorador, visite la dirección URL `http://localhost:7071/api/HttpExample?name=VS%20Code` para crear una solicitud al punto de conexión de HttpExample, que también debe escribir un mensaje en la cola.

1. Para comprobar que el mensaje se ha escrito en la cola "outqueue" (como se llama en el enlace), puede usar uno de los tres métodos siguientes:

    1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya al grupo de recursos que contiene el proyecto de Functions. Dentro de ese grupo de recursos, busque y vaya a la cuenta de almacenamiento del proyecto y, a continuación, vaya a **Colas**. En esa página, vaya a "outqueue", que debería mostrar todos los mensajes registrados.

    1. Examine la cola con el Explorador de Azure Storage, que se integra con Visual Studio, tal y como se describe en [Conectar Functions a Azure Storage con Visual Studio Code](/azure/azure-functions/functions-add-output-binding-storage-queue-vs-code), especialmente la sección [Examen de la cola de salida](/azure/azure-functions/functions-add-output-binding-storage-queue-vs-code#examine-the-output-queue).

    1. Use la CLI de Azure para consultar la cola de almacenamiento, tal como se describe en [Consulta de la cola de almacenamiento](/azure/azure-functions/functions-add-output-binding-storage-queue-python#query-the-storage-queue).

1. Para probar en a nube, vuelva a implementar el código mediante la opción **Implementar en aplicación de funciones** en el explorador de **Azure: Functions**. Si se le solicita, seleccione la aplicación de funciones creada previamente. Una vez finalizada la implementación (tarda unos minutos), la ventana **Salida** muestra de nuevo los puntos de conexión públicos con los que puede repetir las pruebas.

> [!div class="nextstepaction"]
> [He agregado un enlace de almacenamiento](tutorial-vs-code-serverless-python-08.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=07-storage-binding)
