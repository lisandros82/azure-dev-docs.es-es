---
title: 'Tutorial: Reimplementación de un contenedor en Azure App Service después de realizar cambios en Visual Studio Code'
description: Paso 3 del tutorial, pasos sencillos para recompilar y reimplementar una imagen de contenedor.
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 7f6c8f742029533fa54bad2c4492397a0fe17d70
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466039"
---
# <a name="tutorial-redeploy-a-container-to-azure-app-service-after-making-changes"></a>Tutorial: Reimplementación de un contenedor en Azure App Service después de realizar cambios

[Paso anterior: implementación de la imagen en Azure](tutorial-deploy-containers-02.md)

En este artículo se explica cómo reimplementar un contenedor en Azure App Service después de realizar cambios en Visual Studio Code.

Como es inevitable realizar cambios en la aplicación, es necesario recompilar y reimplementar el contenedor muchas veces. Por suerte, el proceso es sencillo:

1. Realice los cambios en la aplicación y pruébela localmente. (Este paso y los dos siguientes se explican en el tutorial [Creación de un contenedor de Python en VS Code](https://code.visualstudio.com/docs/python/tutorial-create-container)).

1. Recompile la imagen de Docker. Si solo cambia el código de la aplicación, la compilación tardará solo unos segundos.

1. Inserte la imagen de Docker en el Registro. De nuevo, si solo ha cambiado el código de la aplicación, solo hay que insertar esa capa pequeña y el proceso se completa normalmente en unos segundos.

1. En el área **Azure: App Service**, haga clic con el botón derecho en el servicio de aplicaciones correspondiente y seleccione **Reiniciar**. Al reiniciar un servicio de aplicaciones, se extrae automáticamente la imagen de contenedor más reciente del Registro.

1. Después de unos 15-20 segundos, visite la dirección URL del servicio de aplicaciones para comprobar las actualizaciones.

> [!div class="nextstepaction"]
> [He realizado cambios y he vuelto a implementar](tutorial-deploy-containers-04.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=03-make-changes-redeploy)
