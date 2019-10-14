---
title: 'Tutorial: Implementación de una imagen de contenedor en Azure App Service con Visual Studio Code'
description: Paso 2 del tutorial, implementación de la imagen de Docker real en Azure App Service desde un registro de contenedor.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: a099e25134615fc05508cde3c0f128a3be813402
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172269"
---
# <a name="tutorial-deploy-a-container-image-to-azure-app-service"></a>Tutorial: Implementación de una imagen de contenedor en Azure App Service

[Paso anterior: requisitos previos](tutorial-deploy-containers-01.md)

Con una imagen de contenedor en un registro, puede usar la extensión Docker en VS Code para configurar fácilmente un servicio de aplicaciones que ejecute el contenedor.

1. En el explorador de **Docker**, expanda **Registros**, expanda el nodo de su registro (por ejemplo, **Azure**), y después expanda el nodo de su nombre de imagen hasta que vea la imagen con la etiqueta `:latest`.

    ![Localización de una imagen en el explorador de Docker](media/deploy-containers/deploy-find-image.png)

1. Haga clic con el botón derecho en una imagen y seleccione **Deploy Image to Azure App Service** (Implementar imagen en Azure App Service).

    ![Selección del comando de menú Implementar](media/deploy-containers/deploy-menu.png)

1. Siga las indicaciones para seleccionar una suscripción de Azure, seleccione o especifique un grupo de recursos, especifique una región, configure un plan de App Service (B1 es el menos caro) y especifique un nombre para el sitio. La animación siguiente ilustra el proceso.

    ![Creación e implementación de una animación](media/deploy-containers/deploy-to-app-service.gif)

    Un **grupo de recursos** es una colección con nombre de los distintos recursos que componen una aplicación. Asignar todos los recursos de la aplicación a un solo grupo permite administrar fácilmente esos recursos como una sola unidad. (Para más información, consulte [Introducción a Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) en la documentación de Azure).

    Un **plan de App Service** define los recursos físicos (una máquina virtual subyacente) que hospeda el contenedor en ejecución. En este tutorial, B1 es el plan menos costoso que admite contenedores de Docker. (Para más información, consulte la [introducción al plan de App Service](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview) en la documentación de Azure).

    El nombre del servicio de aplicaciones debe ser único en todo Azure, por lo que normalmente se usa un nombre personal o de empresa. Para los sitios de producción, el servicio de aplicaciones normalmente se configura con un nombre de dominio registrado por separado.

1. La creación del servicio de aplicaciones tarda unos minutos y verá el progreso en el panel de salida de VS Code.

1. Una vez completado, **tiene que** agregar una configuración llamada `WEBSITES_PORT` (observe el plural "WEBSITES") al servicio de aplicaciones para especificar el puerto en el que el contenedor está escuchando. (Si usa una imagen del tutorial [Creación de un contenedor de Python en VS code](https://code.visualstudio.com/docs/python/tutorial-create-container), por ejemplo, el puerto es 5000 para Flask y 8000 para Django). Para establecer `WEBSITES_PORT`, cambie al explorador de **Azure: App Service**, expanda el nodo del nuevo servicio de aplicaciones (actualice si es necesario), haga clic con el botón derecho en **Configuración de la aplicación** y seleccione **Agregar nueva configuración**. En las indicaciones, escriba `WEBSITES_PORT` como clave y número de puerto del valor.

    ![Comando de menú contextual en un servicio de aplicaciones para Agregar nueva configuración](media/deploy-containers/add-app-service-setting.png)

1. El servicio de aplicaciones se reinicia automáticamente al cambiar la configuración. También puede hacer clic con el botón derecho en el servicio de aplicaciones y seleccionar **Reiniciar** en cualquier momento.

1. Una vez reiniciado el servicio, examine el sitio en `http://<name>.azurewebsites.net`. Puede usar **Ctrl**+clic (o **Cmd**+clic en MacOS) en la dirección URL del panel de salida, o hacer clic con el botón derecho en el servicio de aplicaciones en el explorador de **Azure: App Service** y seleccione **Examinar sitio web**.

> [!div class="nextstepaction"]
> [He implementado la imagen](tutorial-deploy-containers-03.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=02-deploy-container)
