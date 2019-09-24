---
title: Transmisión de registros desde Azure App Service para un contenedor en Visual Studio Code
description: Parte 4 del tutorial, visualización de registros desde Azure App Service para supervisar su comportamiento.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: b886eee84e6e8daef772c2ba6e7290a604d18409
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019513"
---
# <a name="stream-logs-from-azure-app-service"></a>Transmisión de registros desde Azure App Service

[Paso anterior: cambios y reimplementación](tutorial-deploy-containers-03.md)

Desde VS Code, puede ver los registros del sitio en ejecución en Azure App Service, que captura cualquier salida en la consola desde instrucciones `print` y las enruta al panel **Salida** de VS Code.

1. Busque la aplicación en el explorador **Azure: App Service**, haga clic con el botón derecho en la aplicación y seleccione **Iniciar transmisión de registros**.

1. Cuando se le pida, responda **Sí** para habilitar el registro y reiniciar la aplicación. Después de reiniciar la aplicación, se abre el panel Salida de VS Code con una conexión a la transmisión de registros.

1. Después de unos segundos, verá un mensaje donde se indica que está conectado al servicio de transmisión de registros.

    ```bash
    Connecting to log stream...
    2018-09-27T20:14:26  Welcome, you are now connected to log-streaming service.

    2018-09-27 20:14:59.269 INFO  - Starting container for site

    2018-09-27 20:14:59.270 INFO  - docker run -d -p 24138:8000 --name vsdocs-django-sample-container_0 -e WEBSITES_PORT=8000 -e WEBSITE_SITE_NAME=vsdocs-django-sample-container -e WEBSITE_AUTH_ENABLED=False -e WEBSITE_ROLE_INSTANCE_ID=0 -e WEBSITE_INSTANCE_ID=02c705ae24eaf5f298e553a9c2724b9fe4485707c2d1c36137cd02931091e561 -e HTTP_LOGGING_ENABLED=1 vsdocsregistry.azurecr.io/python-sample-vscode-django-tutorial:latest

    2018-09-27 20:15:06.216 INFO  - Container vsdocs-django-sample-container_0 for site vsdocs-django-sample-container initialized successfully.
    ```

1. Busque en la aplicación una salida adicional para varias solicitudes HTTP.

> [!div class="nextstepaction"]
> [Veo los registros](tutorial-deploy-containers-05.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=04-stream-logs)
