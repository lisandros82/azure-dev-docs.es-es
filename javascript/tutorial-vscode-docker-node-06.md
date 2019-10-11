---
title: Transmisión de registros desde una aplicación de Node.js en contenedores desde Visual Studio Code
description: Parte 5 del tutorial, transmisión de los registros a Visual Studio Code
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 9df23cb6aac013006cf0f21871f16eededdcb816
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685990"
---
# <a name="stream-logs-into-visual-studio-code"></a>Transmisión de registros a Visual Studio Code

[Paso anterior: Cambios y reimplementación](tutorial-vscode-docker-node-05.md)

En este paso, aprenderá a ver o consultar el final de archivo de cualquier salida que el sitio web en ejecución genera mediante llamadas a `console.log`. Esta salida aparece en la ventana **Salida** de Visual Studio Code.

1. En el explorador de **Azure App Service**, haga clic con el botón derecho en el nodo de la aplicación y elija **Start Streaming Logs** (Iniciar transmisión de registros).

    ![Visualización de los registros de streaming](media/deploy-containers/stream-logs-command.png)

1. Cuando se le pida, seleccione la opción para habilitar el registro y reinicie la aplicación.

    ![Indicador para habilitar el registro y reinicio](media/deploy-azure/enable-restart.png)

1. Una vez que se reinicia la aplicación, se abre el panel **Salida** de Visual Studio Code con una conexión al flujo de registros, empezando por el mensaje `Starting Live Log Stream`.

> [!div class="nextstepaction"]
> [Veo los registros](tutorial-vscode-docker-node-07.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=tailing-logs)
