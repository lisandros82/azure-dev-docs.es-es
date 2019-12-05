---
title: Transmisión de registros desde Azure App Service a Visual Studio Code
description: Parte 4 del tutorial, visualización de los registros o consulta del final de los registros.
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: cc140d7751f9b014f1a16065fd4c65b481c7d1ae
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466809"
---
# <a name="stream-logs-from-azure-app-service"></a>Transmisión de registros desde Azure App Service

[Paso anterior: Implementación del sitio web](tutorial-vscode-azure-app-service-node-03.md)

En este paso, aprenderá a ver o consultar el final de archivo de cualquier salida que el sitio web en ejecución genera mediante llamadas a `console.log`. Esta salida aparece en la ventana **Salida** de Visual Studio Code.

1. En el explorador de **Azure App Service**, haga clic con el botón derecho en el nodo de la aplicación y elija **Start Streaming Logs** (Iniciar transmisión de registros).

    ![Visualización de los registros de streaming](media/deploy-azure/view-logs.png)

1. Cuando se le pida, seleccione la opción para habilitar el registro y reinicie la aplicación.

    ![Indicador para habilitar el registro y reinicio](media/deploy-azure/enable-restart.png)

1. Una vez reiniciada la aplicación, se abrirá la ventana **Salida** de VS Code con una conexión al flujo de registros que muestra la salida.

    ```bash
    Connecting to log-streaming service...
    2019-09-20 17:33:51.428 INFO  - Container msdocs-vscode-node_2 for site msdocs-vscode-node initialized successfully.
    2019-09-20 17:33:56.500 INFO  - Container logs
    ```

1. Actualice la página web varias veces en el explorador para ver otras salidas de registros.

> [!div class="nextstepaction"]
> [Veo los registros](tutorial-vscode-azure-app-service-node-05.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=tailing-logs)
