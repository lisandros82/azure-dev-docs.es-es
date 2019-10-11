---
title: Creación de una imagen de contenedor para una aplicación de Node.js desde Visual Studio Code
description: Parte 3 del tutorial, creación de una imagen de aplicación de Node.js
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 1b79f84bd69853578796b4485ca669be98f41006
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686097"
---
# <a name="create-your-nodejs-application-image"></a>Creación de la imagen de aplicación de Node.js

[Paso anterior: Uso de un registro de contenedor](tutorial-vscode-docker-node-02.md)

En este paso, usará la extensión de Docker en Visual Studio Code para agregar los archivos necesarios para crear una imagen para la aplicación, compilarla e insertarla en un registro.

Si aún no tiene una aplicación para este tutorial, use la aplicación del [tutorial de Node.js para Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="add-docker-files"></a>Incorporación de archivos de Docker

1. En Visual Studio Code, abra la **paleta de comandos** (**F1**), escriba `add docker files to workspace` y, a continuación, seleccione el comando **Docker: Add Docker files to workspace** (Agregar archivos de Docker al área de trabajo).

1. Cuando se le solicite, seleccione **Node.js** para el tipo de aplicación y, a continuación, seleccione la publicación en la que escucha la aplicación.

1. El comando crea un archivo `Dockerfile` junto con algunos archivos de configuración para Docker Compose y un archivo `.dockerignore`.

    > [!TIP]
    > VS Code tiene una gran compatibilidad con los archivos de Docker. Consulte [Uso de Docker](https://code.visualstudio.com/docs/azure/docker) en la documentación de VS Code para más información sobre las características de lenguaje enriquecidas, como las sugerencias inteligentes, las finalizaciones y la detección de errores.

## <a name="build-a-docker-image"></a>Compilación de una imagen de Docker

En el archivo `Dockerfile` se describe el entorno de la aplicación, incluida la ubicación de los archivos de origen y el comando para iniciar la aplicación dentro de un contenedor.

> [!TIP]
> Contenedores en comparación con imágenes: Un contenedor es una instancia de una imagen.

1. Abra la **paleta de comandos** (**F1**) y ejecute **Docker Images: Build Image** (Imágenes de Docker: Compilar imagen) para compilar la imagen.

1. Cuando se le solicite, seleccione el archivo `Dockerfile` que se acaba de crear y, a continuación, asigne un nombre a la imagen. El nombre debe incluir el registro de destino o la cuenta de Docker Hub:

    `[registry or username]/[image name]:[tag]`

    En este tutorial, que utiliza Azure Container Registry, el nombre de la imagen es el siguiente:

    `msdocsvscodereg.azurecr.io/myexpressapp:latest`

    Si usa Docker Hub, utilice el nombre de usuario de este. Por ejemplo:

    `fiveisprime/myexpressapp:latest`

1. Una vez completado, se abre el panel **Terminal** de Visual Studio Code para ejecutar el comando `docker build`. La salida también muestra cada paso, o capa, que conforma el entorno de la aplicación.

1. Una vez compilada, la imagen aparece en el explorador de **DOCKER** en **Imágenes**.

    ![Lista de imágenes de Docker en Visual Studio Code](media/deploy-containers/image-list.png)

## <a name="push-the-image-to-a-registry"></a>Inserte la imagen en un registro

1. En Visual Studio Code, abra la **paleta de comandos** (**F1**) y ejecute **Docker Images: Push** (Imágenes de Docker: Insertar) y elija la imagen que acaba de compilar. En el panel **Terminal** aparecen los comandos `docker push` utilizados para esta operación.

1. Una vez completada, expanda el nodo **Images** (Imágenes) en el explorador de extensiones de Docker para ver la imagen.

    ![Imagen insertada que aparece en Azure Container Registry](media/deploy-containers/image-in-acr.png)

> [!div class="nextstepaction"]
> [He creado una imagen para mi aplicación](tutorial-vscode-docker-node-04.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=containerize-app)
