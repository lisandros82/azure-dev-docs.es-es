---
title: Creación de una instancia de Azure App Service desde la CLI de Azure para hospedar la aplicación
description: Parte 3 del tutorial, creación de una instancia de App Service
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: f8349d352d275abcc8126bb9dd1794d408045b29
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686135"
---
# <a name="create-the-app-service"></a>Creación del servicio de aplicaciones

[Paso anterior: Creación de la aplicación](tutorial-vscode-azure-cli-node-02.md)

En este paso, usará la CLI de Azure para crear una instancia de Azure App Service para hospedar el código de la aplicación.

1. En un símbolo del sistema o terminal, use el siguiente comando para crear un **grupo de recursos** para la instancia de App Service. Un grupo de recursos es una colección con nombre de los recursos de una aplicación en Azure, como un sitio web, una base de datos, Azure Functions, etc.

    ```bash
    az group create --name myResourceGroup --location westus
    ```

    El comando anterior crea un grupo de recursos llamado `myResourceGroup` en el centro de datos `westus`. Puede cambiar estos valores como desee.

    Una vez que el comando se ejecuta correctamente, aparece la salida JSON con los detalles del grupo de recursos.

1. Ejecute el siguiente comando para establecer el grupo de recursos y la región predeterminados para los comandos siguientes. Con ello evitará la necesidad de especificar esos valores cada vez. (Este comando no muestra ninguna salida en caso de finalización correcta).

    ```bash
    az configure --defaults group=myResourceGroup location=westus
    ```

1. Ejecute el siguiente comando para crear un **plan de App Service** que defina la máquina virtual subyacente que utiliza App Service:

    ```bash
    az appservice plan create --name myPlan --sku F1
    ```

    El comando anterior especifica un plan de hospedaje gratuito (`--sku F1`), que usa una máquina virtual compartida y asigna al plan el nombre `myPlan`. De nuevo, el comando mostrará la salida JSON en caso de finalización correcta.

1. Ejecute el siguiente comando para crear el servicio de aplicaciones, reemplazando `<your_app_name>` por un nombre único que se convierte en la dirección URL, `http://<your_app_name>.azurewebsites.net`. Tenga en cuenta que el comando de PowerShell es ligeramente diferente. El argumento `--runtime "node|6.9"` le indica a Azure que use la versión de Node 6.9.x en el servidor.

    ```bash
    az webapp create --name <your_app_name> --plan myPlan --runtime "node|6.9"
    ```

    > [!TIP]
    > También puede indicar la versión de nodo deseada en el archivo `package.json`. Azure aplicará esta configuración durante la implementación. Por ejemplo, la siguiente entrada `package.json` le indica a Azure que use como mínimo Node 7.0.0:
    >
    > ``` json
    > "engines": {
    >     "node": ">7.0.0"
    > },
    > ```

1. Ejecute el siguiente comando para abrir un explorador en el servicio de aplicaciones recién creado; para ello, sustituya `<your_app_name>` por el nombre que usó:

    ```bash
    az webapp browse --name <your_app_name>
    ```

1. Como no ha implementado ningún código personalizado para la aplicación, aparecerá una página predeterminada en el explorador:

    ![Página predeterminada del servicio de aplicaciones](media/azure-cli/azure-default-page.png)

> [!div class="nextstepaction"]
> [He creado el servicio de aplicaciones](tutorial-vscode-azure-cli-node-04.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=create-website)
