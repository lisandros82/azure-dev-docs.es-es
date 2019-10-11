---
title: Creación de una cuenta de Azure Storage para un sitio web estático de Node.js desde Visual Studio Code
description: Parte 3 del tutorial, creación de una cuenta de Azure Storage
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 44e6479379fff3ddf1012cdb61cf73440cad346e
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685970"
---
# <a name="create-an-azure-storage-account"></a>Creación de una cuenta de Azure Storage

[Paso anterior: Creación de la aplicación](tutorial-vscode-static-website-node-02.md)

En este paso, puede crear una cuenta de Azure Storage, que actúa como un almacén de archivos simple (o CDN) con un servidor web integrado. Este servidor integrado hace de Azure Storage una excelente opción para hospedar sitios estáticos rápidamente.

1. Desde la carpeta `my-react-app` creada en el paso anterior, inicie Visual Studio Code para que abra automáticamente esa carpeta:

    ```bash
    code .
    ```

1. En VS Code, seleccione el logotipo de Azure para que se abra el explorador de **Azure**. En **Azure Storage**, haga clic con el botón derecho en la suscripción de Azure y seleccione **Create Storage Account** (Crear cuenta de almacenamiento):

    ![Creación de una cuenta de almacenamiento en VS Code](media/static-website/create-storage-account.png)

1. En el indicador, en "Enter the name of the new storage account" (Escriba el nombre de la nueva cuenta de almacenamiento), escriba un nombre único global para la cuenta de almacenamiento y presione Entrar. Los caracteres válidos para un nombre de aplicación son "a-z" y "0-9".

1. En el indicador, en "Select a resource group" (Seleccionar un grupo de recursos), seleccione **Create a new Resource Group** (Crear un nuevo grupo de recursos) y acepte el nombre predeterminado.

1. En el indicador, en "Select a location" (Seleccionar una ubicación), elija una [región](https://azure.microsoft.com/regions/) que esté cerca de usted.

1. Mientras se crea la cuenta de almacenamiento, el progreso aparece en el panel **Salida** de VS Code:

1. Una vez completada la creación de la cuenta de almacenamiento, haga clic con el botón derecho en esa cuenta y seleccione **Configure Static Website** (Configurar sitio web estático). Habilitar el hospedaje de sitios web estáticos implica que Azure Storage proporciona automáticamente el documento de índice y todos los demás recursos estáticos.

    ![Crear cuenta de almacenamiento](media/static-website/configure-static-website.png)

1. Cuando se le solicite, escriba *index.html* para el nombre del documento de índice y el nombre del documento del error 404. Se usa *index.html* para el documento de error porque las aplicaciones modernas de una sola página como React, Angular y Vue, controlan los errores en el cliente. En el caso de sitios web estáticos clásicos, use una página personalizada de error 404.

> [!div class="nextstepaction"]
> [He creado un contenedor de almacenamiento](tutorial-vscode-static-website-node-04.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
