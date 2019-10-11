---
title: 'Tutorial: Transmisión de registros desde Azure App Service a VS Code'
description: Paso 6 del tutorial, transmisión de los registros de aplicación a Visual Studio Code
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: f4aac132e9c01a0c428e243e06e811357defc2aa
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172234"
---
# <a name="tutorial-stream-logs-from-azure-app-service-into-visual-studio-code"></a>Tutorial: Transmisión de registros desde Azure App Service a Visual Studio Code

[Paso anterior: implementación de la aplicación](tutorial-deploy-app-service-on-linux-05.md)

1. En Visual Studio Code, abra el explorador **Azure: App Service**, haga clic con el botón derecho en el servicio de aplicaciones y seleccione **Iniciar transmisión de registros**:

   ![Comando Iniciar transmisión de registros](media/deploy-azure/start-streaming-logs-command.png)

1. Cuando se le pida que habilite el registro de archivos y reinicie la aplicación web, seleccione **Sí**. Mientras se reinicia la aplicación, la ventana **Salida** de VS Code muestra el progreso. Habilitar el registro es un proceso que se realiza una sola vez.

1. Una vez habilitado el registro, haga clic con el botón derecho en el servicio de aplicaciones y vuelva a seleccionar **Iniciar transmisión de registros**. La ventana **Salida** de VS Code muestra "Inicio de la transmisión de registros en directo" y comienza a aparecer la salida de los registros. Intente actualizar la aplicación web en el explorador para generar más información de registro.

1. Para detener la transmisión de los registros (sin deshabilitar el registro), haga clic con el botón derecho en el explorador de **Azure: App Service** y seleccione **Detener transmisión de registros**.

> [!div class="nextstepaction"]
> [Veo los registros](tutorial-deploy-app-service-on-linux-07.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=06-stream-logs)
