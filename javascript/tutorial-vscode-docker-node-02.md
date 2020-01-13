---
title: Uso de un registro de contenedor en Visual Studio Code
description: Parte 2 del tutorial, uso de un registro de contenedor
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: c5e9ff3cd803ef4d57408199682c71e4b57f2d77
ms.sourcegitcommit: fc3408b6e153c847dd90026161c4c498aa06e2fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2019
ms.locfileid: "75191040"
---
# <a name="use-a-container-registry"></a>Uso de un registro de contenedor

[Paso anterior: Introducción y requisitos previos](tutorial-vscode-docker-node-01.md)

En este paso, va a configurar un registro de contenedor adecuado para la imagen de la aplicación. Posteriormente, los servicios de hospedaje compatibles con contenedores como Azure App Service extraerán las imágenes del registro.

En este tutorial se usa [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR), un registro privado, seguro y hospedado para sus imágenes. Sin embargo, las herramientas y los procesos que aparecen aquí también funcionan con otros registros como [Docker Hub](https://hub.docker.com/).

## <a name="create-an-azure-container-registry"></a>Creación de una instancia de Azure Container Registry

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y seleccione **Crear un recurso**.

    ![Creación de un recurso nuevo en Azure Portal](media/deploy-containers/portal-01a.png)

1. En la página siguiente, seleccione **Contenedores** > **Container Registry**.

    ![Creación de un registro de contenedor en Azure Portal](media/deploy-containers/portal-01b.png)

1. En el formulario **Crear Registro de contenedor** que aparece, escriba los valores adecuados:

    - En **Nombre del registro**, el nombre debe ser único en Azure y contener entre 5 y 50 caracteres alfanuméricos.
    - Seleccione la suscripción en **Suscripción**.
    - En **Grupo de recursos**, el grupo de recursos solo tiene que ser único dentro de la suscripción.
    - En **Ubicación**, seleccione una región cercana a usted.
    - Establezca **Usuario administrador** en **Habilitar**.
    - Seleccione **Básico** en **SKU**.

    ![Valores del formulario del registro de contenedor](media/deploy-containers/portal-02.png)

1. Seleccione **Crear** para crear el registro.

1. Una vez creado el registro, abra las notificaciones en el portal y seleccione **Ir al recurso** para el registro:

    ![Abrir el recurso del registro recién creado](media/deploy-containers/portal-03.png)

1. En la página del registro, seleccione **Claves de acceso** y anote las credenciales del administrador:

    ![Credenciales del registro en Azure Portal](media/deploy-containers/portal-04.png)

1. En un símbolo del sistema o terminal, inicie sesión en Docker con el comando siguiente, sustituya `<registry_name>` por el nombre del registro y `<username>` y `<password>` con los valores que se muestran en Azure Portal para el usuario administrador:

    ```bash
    docker login <registry_name>.azurecr.io -u <username> -p <password>
    ```

    Para aumentar la seguridad, use `--password-stdin` en lugar de `-p <password>` y, después, pegue la contraseña cuando se le pida.

1. En Visual Studio Code, abra el explorador de **Docker** y asegúrese de que el punto de conexión del registro que acaba de configurar esté visible en **Registros**:

    ![Comprobación de que el registro aparece en el explorador de Docker](media/deploy-containers/registries.png)

> [!div class="nextstepaction"]
> [He creado un registro](tutorial-vscode-docker-node-03.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
