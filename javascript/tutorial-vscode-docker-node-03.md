---
title: Creación de una imagen de contenedor para una aplicación de Node.js desde Visual Studio Code
description: Parte 3 del tutorial, creación de una imagen de aplicación de Node.js
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: ae39d6604f3ffe49915f6b311953cd6829ed9369
ms.sourcegitcommit: fc3408b6e153c847dd90026161c4c498aa06e2fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2019
ms.locfileid: "75191080"
---
# <a name="create-your-nodejs-application-image"></a>Creación de la imagen de aplicación de Node.js

[Paso anterior: Uso de un registro de contenedor](tutorial-vscode-docker-node-02.md)

En este paso, usará la extensión de Docker en Visual Studio Code para agregar los archivos necesarios para crear una imagen para la aplicación, compilarla e insertarla en un registro.

Si aún no tiene una aplicación para este tutorial, use la aplicación del [tutorial de Node.js para Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="add-docker-files"></a>Incorporación de archivos de Docker

1. En Visual Studio Code, abra la **paleta de comandos** (**F1**), escriba `add docker files to workspace` y, a continuación, seleccione el comando **Docker: Add Docker files to workspace** (Agregar archivos de Docker al área de trabajo).

1. Cuando se le pida, seleccione **Node.js** como tipo de aplicación, responda **No** a los archivos compuestos de Docker y, después, seleccione el puerto donde la aplicación escuchará.

1. El comando crea un archivo `Dockerfile` junto con algunos archivos de configuración para Docker Compose y un archivo `.dockerignore`.

    > [!TIP]
    > VS Code tiene una gran compatibilidad con los archivos de Docker. Consulte [Uso de Docker](https://code.visualstudio.com/docs/azure/docker) en la documentación de VS Code para más información sobre las características de lenguaje enriquecidas, como las sugerencias inteligentes, las finalizaciones y la detección de errores.

## <a name="build-a-docker-image"></a>Compilación de una imagen de Docker

En el archivo `Dockerfile` se describe el entorno de la aplicación, incluida la ubicación de los archivos de origen y el comando para iniciar la aplicación dentro de un contenedor.

> [!TIP]
> Contenedores en comparación con imágenes: Un contenedor es una instancia de una imagen.

1. Abra la **paleta de comandos** (**F1**) y ejecute **Docker Images: Build Image** (Imágenes de Docker: Compilar imagen) para compilar la imagen. VS Code usa Dockerfile en la carpeta actual y asigna a la imagen el mismo nombre que la carpeta actual.

1. Una vez completado, se abre el panel **Terminal** de Visual Studio Code para ejecutar el comando `docker build`. La salida también muestra cada paso, o capa, que conforma el entorno de la aplicación.

1. Una vez compilada, la imagen aparece en el explorador de **DOCKER** en **Imágenes**.

    ![Lista de imágenes de Docker en Visual Studio Code](media/deploy-containers/image-list.png)

## <a name="push-the-image-to-a-registry"></a>Inserte la imagen en un registro

1. Para insertar la imagen en un registro, primero debe etiquetarla con el nombre del registro. En el explorador de **DOCKER**, haga clic con el botón derecho en **latest** para elegir la imagen más reciente.

    ![Comando de etiquetado de imagen en Visual Studio Code](media/deploy-containers/tag-command.png)

1. En el símbolo del sistema siguiente, complete las etiquetas y presione **Entrar**.

    Por convención, el etiquetado usa el siguiente formato:

    `[registry or username]/[image name]:[tag]`

    Si utiliza Azure Container Registry, el nombre de la imagen será similar al siguiente:

    `msdocsvscodereg.azurecr.io/myexpressapp:latest`

    Si usa Docker Hub, utilice el nombre de usuario de este. Por ejemplo:

    `fiveisprime/myexpressapp:latest`

1. La imagen recién etiquetada aparece ahora en un nodo de **Images** (Imágenes) que incluye el nombre del registro. Expanda ese nodo, haga clic con el botón derecho en **latest** (más recientes) y seleccione **Push** (Insertar).

    ![Comando de inserción de imagen en Visual Studio Code](media/deploy-containers/push-command.png)

1. En el panel **Terminal** aparecen los comandos `docker push` utilizados para esta operación. El registro de destino viene determinado por el registro especificado en el nombre de la imagen.

1. Cuanto termine, expanda el nodo **Registries** (Registros) en el explorador de extensiones de Docker para ver la imagen en el registro.

    ![Imagen insertada que aparece en Azure Container Registry](media/deploy-containers/image-in-acr.png)

> [!div class="nextstepaction"]
> [He creado una imagen para mi aplicación](tutorial-vscode-docker-node-04.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=containerize-app)
