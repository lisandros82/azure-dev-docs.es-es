---
title: Implementación de una aplicación web de Python en Azure App Service en Linux mediante VS Code
description: Paso 5 del tutorial, implementación del código de la aplicación web
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: e9b85928bd2b1308ab57747d7b0c32c085274cde
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019683"
---
# <a name="deploy-your-app"></a>Implementación de la aplicación

[Paso anterior: configuración de un archivo de inicio personalizado](tutorial-deploy-app-service-on-linux-04.md)

1. En Visual Studio Code, abra el explorador **Azure: App Service** y seleccione la flecha azul hacia arriba:

   ![Comando Implementar en aplicación web](media/deploy-azure/deploy-to-web-app-command.png)

    También puede hacer clic con el botón derecho en el nombre del servicio de aplicaciones y seleccionar el comando **Implementar en aplicación web**.

1. En los mensajes que siguen, proporcione los detalles siguientes:

    - En "Select the folder to deploy" (Seleccionar la carpeta para implementar), seleccione la carpeta de la aplicación actual.
    - En "Select Web App" (Seleccionar aplicación web), elija el servicio de aplicaciones que creó en el paso anterior.

1. Mientras se está llevando a cabo el proceso de implementación, puede ver el progreso en la ventana **Salida** de VS Code.

    ![Progreso de la implementación en la ventana Salida de VS Code](media/deploy-azure/deployment-progress.png)

1. Una vez completada la implementación después de unos minutos (dependiendo del número de dependencias que se deban instalar), aparece el mensaje siguiente. Seleccione el botón **Examinar sitio web** para ver el sitio en ejecución.

    ![Mensaje de finalización de la implementación](media/deploy-azure/deployment-complete.png)

    ![La aplicación se ejecuta correctamente en App Service](media/deploy-azure/running-app.png)

1. Para comprobar que los archivos están implementados, expanda el servicio de aplicaciones en el explorador de **Azure: App Service** y expanda **Archivos**:

    ![Comprobación de la implementación de los archivos mediante el explorador de App Service](media/deploy-azure/expand-files-node.png)

    La carpeta *antenv* es donde App Service crea un entorno virtual con sus dependencias. Si expande este nodo, puede comprobar que los paquetes a los que asignó nombre en *requirements.txt* están instalados en *antenv/lib/python3.7/site-packages*.

> [!div class="nextstepaction"]
> [He implementado mi aplicación](tutorial-deploy-app-service-on-linux-06.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=05-deploy-app)
