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
ms.openlocfilehash: a60c8fd0202e935960f14a9ab5570f86a78fab6e
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278920"
---
# <a name="tutorial-stream-logs-from-azure-app-service-into-visual-studio-code"></a>Tutorial: Transmisión de registros desde Azure App Service a Visual Studio Code

[Paso anterior: implementación de la aplicación](tutorial-deploy-app-service-on-linux-05.md)

Use este procedimiento para transmitir los registros desde un servicio de aplicaciones de Azure a Visual Studio Code.

1. En Visual Studio Code, abra el explorador **Azure: App Service**, haga clic con el botón derecho en el servicio de aplicaciones y seleccione **Iniciar transmisión de registros**:

   ![Inicio de la transmisión de registros desde el explorador de App Service](media/deploy-azure/start-streaming-logs-in-visual-studio-code.png)

1. Cuando se le pida que habilite el registro de archivos y reinicie la aplicación web, seleccione **Sí**. Mientras se reinicia la aplicación, la ventana **Salida** de VS Code muestra el progreso. Habilitar el registro es un proceso que se realiza una sola vez.

1. Una vez habilitado el registro, haga clic con el botón derecho en el servicio de aplicaciones y vuelva a seleccionar **Iniciar transmisión de registros**. La ventana **Salida** de VS Code muestra "Inicio de la transmisión de registros en directo" y comienza a aparecer la salida de los registros. Intente actualizar la aplicación web en el explorador para generar más información de registro.

1. Para detener la transmisión de los registros (sin deshabilitar el registro), haga clic con el botón derecho en el explorador de **Azure: App Service** y seleccione **Detener transmisión de registros**.

> [!div class="nextstepaction"]
> [Veo los registros](tutorial-deploy-app-service-on-linux-07.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=06-stream-logs)
