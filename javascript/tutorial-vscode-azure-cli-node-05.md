---
title: Transmisión de registros desde Azure App Service
description: Parte 5 del tutorial, visualización de registros
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 8a173bbb7f53de2189e0ecb99b851d77704ff92d
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466763"
---
# <a name="stream-logs-from-app-service"></a>Transmisión de registros desde App Service

[Paso anterior: Implementación de la aplicación](tutorial-vscode-azure-cli-node-04.md)

En este paso, verá (o podrá "consultar el final de") los registros de App Service en ejecución. Todas las llamadas a `console.log` en el código del sitio aparecen en el terminal.

1. Ejecute el siguiente comando para iniciar el registro, reemplazando `<your_app_name>` por el nombre de la instancia de App Service:

    ```bash
    az webapp log tail --name <your_app_name>
    ```

1. Después de unos segundos, aparecerá un mensaje donde se indica que está conectado al servicio de streaming de registros.

    ```bash
    2019-09-25T13:39:23  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours. Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    ```

1. Actualice la página varias veces en el explorador para generar salidas adicionales:

    ```bash
    GET / 304 2.327 ms - -
    GET / 304 0.957 ms - -
    GET / 304 2.435 ms - -
    ```

1. Presione **Ctrl**+**C** para finalizar la sesión de registro.

> [!div class="nextstepaction"]
> [Veo los registros](tutorial-vscode-azure-cli-node-06.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=tailing-logs)
