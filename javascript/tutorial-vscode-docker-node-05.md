---
title: Reimplementación de un contenedor en Azure App Service después de realizar cambios en Visual Studio Code
description: Paso 5 del tutorial, pasos sencillos para recompilar y reimplementar una imagen de contenedor.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 7b0f95bf0d478a742386060d05ebec20f60b4aeb
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686037"
---
# <a name="make-changes-and-redeploy"></a>Cambios y reimplementación

[Paso anterior: Implementación de la imagen de aplicación](tutorial-vscode-docker-node-04.md)

Como es inevitable realizar cambios en la aplicación, es necesario recompilar y reimplementar el contenedor muchas veces. Por suerte, el proceso es sencillo:

1. Realice los cambios en la aplicación y pruébela localmente.

1. En Visual Studio Code, abra la **paleta de comandos** (**F1**) y ejecute **Docker Images: Build Image** (Imágenes de Docker: compilar imagen) para recompilar la imagen. Si solo cambia el código de la aplicación, la compilación tardará solo unos segundos.

1. Para enviar la imagen al registro, abra de nuevo la **paleta de comandos** (**F1**), ejecute **Docker Images: Push** (Imágenes de Docker: insertar) y elija la imagen que acaba de compilar. De nuevo, como el cambio en el código de la aplicación es pequeño, solo hay que insertar esa capa y el proceso se completa normalmente en unos segundos.

1. En el área **Azure: App Service**, haga clic con el botón derecho en el servicio de aplicaciones correspondiente y seleccione **Reiniciar**. Al reiniciar un servicio de aplicaciones, se extrae automáticamente la imagen de contenedor más reciente del Registro.

1. Después de unos 15-20 segundos, visite la dirección URL del servicio de aplicaciones para comprobar las actualizaciones.

> [Veo los cambios](tutorial-vscode-docker-node-06.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=deploy-changes)
