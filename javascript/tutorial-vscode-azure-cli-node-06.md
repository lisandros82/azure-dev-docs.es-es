---
title: Cambios en el código de la aplicación y reimplementación en Azure
description: Parte 6 del tutorial, realización de cambios y reimplementación
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 9702f10795893004965631fa99dfbfab181f2292
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467160"
---
# <a name="make-changes-and-redeploy"></a>Cambios y reimplementación

[Paso anterior: Transmisión de registros](tutorial-vscode-azure-cli-node-05.md)

En este paso, realizará cambios en el código de la aplicación, los confirmará en el repositorio de Git local y, posteriormente, volverá a implementar el sitio insertándolo en Azure.

1. En la carpeta `myExpressApp`, abra el archivo *views/index.pug* y cambie el mensaje de la línea 5 a `p Welcome to Azure!`.

    ![Editar el archivo index.pug](media/azure-cli/editpugfile.png)

1. Guarde el archivo.

1. En un símbolo del sistema o terminal, confirme los cambios en Git ejecutando el comando siguiente:

    ```bash
    git commit -a -m "Edited message"
    ```

1. Inserte los cambios en el repositorio remoto de Git llamado Azure que hemos creado anteriormente:

    ```bash
    git push azure master
    ```

1. Dado que App Service ya está conectado al repositorio de Git, la salida del comando muestra que los cambios se han publicado automáticamente en Azure: 

    ```output
    Enumerating objects: 7, done.
    Counting objects: 100% (7/7), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 405 bytes | 202.00 KiB/s, done.
    Total 4 (delta 2), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id '9fd89a25b6'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling node.js deployment.
    remote: Creating app_offline.htm
    remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
    remote: Copying file: 'views\index.pug'
    remote: Deleting app_offline.htm
    remote: Using start-up script bin/www from package.json.
    remote: Generated web.config.
    remote: The package.json file does not specify node.js engine version constraints.
    remote: The node.js application will run with the default node.js version 6.9.5.
    remote: Selected npm version 3.10.10
    remote: ..
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
       a98bad8..9fd89a2  master -> master
    ```

1. Actualice la aplicación en el explorador para ver los cambios:

    ![Cambios publicados visibles en el explorador](media/azure-cli/remote-app-changes.png)

> [!div class="nextstepaction"]
> [Veo mis cambios](tutorial-vscode-azure-cli-node-07.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=publishing-changes)
