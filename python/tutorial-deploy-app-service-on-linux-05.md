---
title: 'Tutorial: Implementación de una aplicación web de Python en Azure App Service en Linux mediante VS Code'
description: Paso 5 del tutorial, implementación del código de la aplicación web
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: b87271573f21dfc696c4f68b234780feb724ddd1
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278976"
---
# <a name="tutorial-deploy-your-python-web-app-to-azure-app-service-on-linux"></a>Tutorial: Implementación de una aplicación web de Python en Azure App Service en Linux

[Paso anterior: configuración de un archivo de inicio personalizado](tutorial-deploy-app-service-on-linux-04.md)

Use este procedimiento para implementar la aplicación de Python en un servicio de aplicaciones de Azure.

1. En Visual Studio Code, abra el explorador **Azure: App Service** y seleccione la flecha azul hacia arriba:

   ![Implementación de la aplicación web en un servicio de aplicaciones en el explorador de App Service](media/deploy-azure/deploy-web-app-to-app-service-in-app-service-explorer.png)

    También puede hacer clic con el botón derecho en el nombre del servicio de aplicaciones y seleccionar el comando **Implementar en aplicación web**.

1. En los mensajes que siguen, proporcione los detalles siguientes:

    - En "Select the folder to deploy" (Seleccionar la carpeta para implementar), seleccione la carpeta de la aplicación actual.
    - En "Select Web App" (Seleccionar aplicación web), elija el servicio de aplicaciones que creó en el paso anterior.

1. Mientras se está llevando a cabo el proceso de implementación, puede ver el progreso en la ventana **Salida** de VS Code.

    ![Progreso de la implementación en la ventana Salida de VS Code](media/deploy-azure/view-deployment-progress-in-visual-studio-code-output.png)

1. Una vez completada la implementación después de unos minutos (dependiendo del número de dependencias que se deban instalar), aparece el mensaje siguiente. Seleccione el botón **Examinar sitio web** para ver el sitio en ejecución.

    ![Implementación completada con el botón Examinar sitio web](media/deploy-azure/web-app-deployment-complete-with-browse-website-button.png)

    ![La aplicación se ejecuta correctamente en App Service](media/deploy-azure/web-app-running-successfully-on-app-service.png)

1. Para comprobar que los archivos están implementados, expanda el servicio de aplicaciones en el explorador de **Azure: App Service** y expanda **Archivos**:

    ![Comprobación de la implementación de los archivos mediante el explorador de App Service](media/deploy-azure/expand-files-node-to-check-deployment-of-web-app-files.png)

    La carpeta *antenv* es donde App Service crea un entorno virtual con sus dependencias. Si expande este nodo, puede comprobar que los paquetes a los que asignó nombre en *requirements.txt* están instalados en *antenv/lib/python3.7/site-packages*.

> [!div class="nextstepaction"]
> [He implementado mi aplicación](tutorial-deploy-app-service-on-linux-06.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=05-deploy-app)
