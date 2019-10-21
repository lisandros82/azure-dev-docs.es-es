---
title: 'Tutorial: Implementación de Azure Functions en Python con Visual Studio Code'
description: Paso 5 del tutorial, implementación del código de función de Python en Azure y descripción de cómo transmitir registros y sincronizar la configuración entre un proyecto local y Azure.
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 9acf6e4c0ecc5357580957e2ea9295cd89f441f0
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278549"
---
# <a name="tutorial-deploy-azure-functions-in-python"></a>Tutorial: Implementación de Azure Functions en Python

[Paso anterior: depuración local](tutorial-vs-code-serverless-python-04.md)

En este artículo, se usa la extensión de Azure Functions para crear una aplicación de funciones en Azure, junto con otros recursos de Azure necesarios. Una Function App permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos.

Una aplicación de funciones también necesita una cuenta de Azure Storage para los datos y un [plan de hospedaje](/azure/azure-functions/functions-scale#hosting-plan-support). Todos estos recursos están organizados dentro de un único grupo de recursos.

1. En el área **Azure: Functions**, seleccione el comando **Implementar en aplicación de funciones** o abra la paleta de comandos (**F1**) y seleccione el comando **Azure Functions: Implementar en aplicación de funciones**. De nuevo, la aplicación de funciones es donde se ejecuta el proyecto de Python en Azure.

    ![Implementación de la función de Python en una aplicación de funciones de Azure](media/tutorial-vs-code-serverless-python/deploy-a-python-fuction-to-azure-function-app.png)

1. Cuando se le solicite, seleccione **Crear nueva aplicación de funciones en Azure** y proporcione un nombre que sea único en Azure (normalmente con el nombre personal o de la compañía junto con otros identificadores únicos; puede usar letras, números y guiones). Si creó anteriormente una aplicación de funciones, su nombre aparecerá en esta lista de opciones.

1. La extensión realiza las siguientes acciones, que puede observar en los mensajes emergentes de Visual Studio Code y la ventana **Salida** (el proceso tarda unos minutos):

    - Cree un grupo de recursos con el nombre que asignó (quitando los guiones).
    - En ese grupo de recursos, cree la cuenta de almacenamiento, el plan de hospedaje y la aplicación de funciones. De forma predeterminada, se crea un [Plan de consumo](/azure/azure-functions/functions-scale#consumption-plan). Para ejecutar las funciones en un plan dedicado, debe [habilitar la publicación con opciones avanzadas de creación](/azure/azure-functions/functions-develop-vs-code).
    - Implemente el código en la aplicación de funciones.

    El explorador de **Azure: Functions** también muestra el progreso:

    ![Indicador de progreso de la implementación en el explorador de Azure: Functions](media/tutorial-vs-code-serverless-python/deployment-progress-indicator-in-azure-function-explorer.png)

1. Una vez completada la implementación, la extensión de Azure Functions muestra un mensaje con botones para tres acciones adicionales:

    ![Mensaje que indica una implementación correcta con acciones adicionales](media/tutorial-vs-code-serverless-python/azure-functions-deployment-success-with-additional-actions.png)

    Para **Transmitir registros** y **Cargar configuración**, consulte las secciones siguientes. Para **Ver la salida**, consulte el paso 5 siguiente.

1. Después de la implementación, la ventana **Salida** también muestra el punto de conexión público en Azure:

    ```output
    HTTP Trigger Urls:
      HttpExample: https://vscode-azure-functions.azurewebsites.net/api/HttpExample
    ```

    Use este punto de conexión para ejecutar las mismas pruebas que realizó localmente, con los parámetros de la dirección URL y/o solicitudes con datos JSON en el cuerpo de la solicitud. Los resultados del punto de conexión público deben coincidir con los resultados del punto de conexión local que probó anteriormente en la [parte 4](tutorial-vs-code-serverless-python-04.md).

## <a name="stream-logs"></a>Transmisión de registros

La compatibilidad con la transmisión de registros está actualmente en desarrollo, como se describe en la documentación del [Problema 589](https://github.com/microsoft/vscode-azurefunctions/issues/589) de la extensión de Azure Functions. El botón **Transmitir registros** del mensaje emergente de la implementación conectará finalmente la salida del registro en Azure a Visual Studio Code. También podrá iniciar y detener la transmisión de registros en el explorador de **Azure Functions**; para ello, haga clic con el botón derecho en el proyecto de Functions y seleccione **Iniciar transmisión de registros**o**Detener transmisión de registros**.

En la actualidad, sin embargo, estos comandos todavía no están operativos. En su lugar, la transmisión de registros está disponible desde un explorador; para ello, ejecute el comando siguiente, reemplazando `<app_name>` por el nombre de la aplicación de funciones en Azure:

```bash
# Replace <app_name> with the name of your Functions app on Azure
func azure functionapp logstream <app_name> --browser
```

## <a name="sync-local-settings-to-azure"></a>Sincronización de la configuración local con Azure

El botón **Cargar configuración** del mensaje emergente de la implementación aplica los cambios realizados en el archivo *local.settings.json* en Azure. También puede invocar el comando en el explorador de **Azure Functions**; para ello, expanda el nodo del proyecto de Functions, haga clic con el botón derecho en **Configuración de la aplicación** y seleccione **Cargar configuración local**. También puede usar la paleta de comandos para seleccionar el comando **Azure Functions: Cargar configuración local**.

Al cargar la configuración, se actualizan los valores existentes y se agregan las nuevas configuraciones definidas en *local.settings.json*. La carga no elimina ninguna configuración de Azure que no aparezca en el archivo local. Para eliminar esas configuraciones, expanda el nodo **Configuración de la aplicación** en el explorador de **Azure Functions**, haga clic con el botón derecho en la configuración y seleccione **Eliminar configuración**. También puede editar la configuración directamente en Azure Portal.

Para aplicar los cambios que realice mediante el portal o el **explorador de Azure** al archivo *local.settings.json*, haga clic con el botón derecho en el nodo **Configuración de la aplicación** y seleccione el comando **Descargar configuración remota**. También puede usar la paleta de comandos para seleccionar el comando **Azure Functions: Descargar configuración remota**.

> [!div class="nextstepaction"]
> [He implementado las funciones](tutorial-vs-code-serverless-python-06.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=05-deploy)
