---
title: Implementación del código de la aplicación en Azure App Service mediante la CLI de Azure
description: Parte 4 del tutorial, implementación del sitio web
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: fbf8c9d77876e5617aee5ed5b27257a4aaca2f41
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686163"
---
# <a name="deploy-the-app-to-app-service"></a>Implementación de la aplicación en App Service

[Paso anterior: Creación del servicio de aplicaciones](tutorial-vscode-azure-cli-node-03.md)

En este paso, implementará el código de la aplicación de Node.js en Azure App Service mediante un proceso básico de inserción en el repositorio de Git local en Azure.

1. En un símbolo del sistema o terminal, ejecute los comandos siguientes para inicializar un repositorio de Git local y realizar una confirmación inicial. (Se omite la carpeta *node_modules* porque se especifica en el archivo *.gitignore* creado cuando se ejecutó anteriormente el generador de Express).

    ```bash
    git init
    git add -A
    git commit -m "Initial Commit"
    ```

1. Ejecute los siguientes comandos para configurar las credenciales de implementación en Azure, reemplazando `username` y `pPassword` por sus credenciales. El comando muestra la salida JSON cuando finaliza correctamente.

    ```bash
    az webapp deployment user set --user-name <username> --password <password>
    ```

1. Ejecute el siguiente comando para recuperar el punto de conexión de Git en el que se va a insertar el código de la aplicación, reemplazando `<your_app_name>` por el nombre que usó al crear el servicio de aplicaciones en el paso anterior:

    ```bash
    az webapp deployment source config-local-git --name <your_app_name>
    ```

    La salida del comando es similar a la siguiente:

    ```output
    {
      "url": "https://username@msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git"
    }
    ```

1. Ejecute el siguiente comando para establecer un nuevo repositorio remoto en Git llamado `azure`, mediante la dirección URL del paso anterior y *omitiendo el nombre de usuario*. Con el ejemplo del paso anterior, el comando sería el siguiente:

    ```bash
    git remote add azure https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
    ```

1. Ejecute el siguiente comando para implementar el código de la aplicación desde el repositorio de Git en el servicio de aplicaciones. El comando le pide las credenciales:

    ```bash
    git push azure master
    ```

1. A medida que se ejecuta el comando, se muestra una serie de mensajes del host remoto. Una vez completado el proceso, actualice el explorador en el que tiene abierta la dirección URL de la aplicación para ver el código en ejecución:

    ![Código de aplicación en ejecución en Azure](media/azure-cli/remote-app.png)

> [!TIP]
> Si se produce el error `Object #<eventemitter> has no method 'hrtime'`, es probable que tenga que establecer la versión del entorno de ejecución del nodo en el sitio. El comando siguiente indica al sitio que use la versión del nodo `6.9.1`. Si su sitio requiere una versión diferente o posterior del nodo, especifique la versión semántica completa `major.minor.patch`.
>
> ```bash
> az webapp config appsettings set --name <your_app_name> --settings
> ```

> [!div class="nextstepaction"]
> [He implementado la aplicación](tutorial-vscode-azure-cli-node-05.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=deploy-website)
