---
title: Reimplementación de un contenedor en Azure App Service después de realizar cambios en Visual Studio Code
description: Paso 3 del tutorial, pasos sencillos para recompilar y reimplementar una imagen de contenedor.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 30b0d0863900c36232b69c23db0eae1c70a34396
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019503"
---
# <a name="make-changes-and-redeploy"></a>Cambios y reimplementación

[Paso anterior: implementación de la imagen en Azure](tutorial-deploy-containers-02.md)

Como es inevitable realizar cambios en la aplicación, es necesario recompilar y reimplementar el contenedor muchas veces. Por suerte, el proceso es sencillo:

1. Realice los cambios en la aplicación y pruébela localmente. (Este paso y los dos siguientes se explican en el tutorial [Creación de un contenedor de Python en VS Code](https://code.visualstudio.com/docs/python/tutorial-create-container)).

1. Recompile la imagen de Docker. Si solo cambia el código de la aplicación, la compilación tardará solo unos segundos.

1. Inserte la imagen de Docker en el Registro. De nuevo, si solo ha cambiado el código de la aplicación, solo hay que insertar esa capa pequeña y el proceso se completa normalmente en unos segundos.

1. En el área **Azure: App Service**, haga clic con el botón derecho en el servicio de aplicaciones correspondiente y seleccione **Reiniciar**. Al reiniciar un servicio de aplicaciones, se extrae automáticamente la imagen de contenedor más reciente del Registro.

1. Después de unos 15-20 segundos, visite la dirección URL del servicio de aplicaciones para comprobar las actualizaciones.

> [!div class="nextstepaction"]
> [He realizado cambios y he vuelto a implementar](tutorial-deploy-containers-04.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=03-make-changes-redeploy)
