---
title: Creación de una aplicación de Node.js en Azure mediante la CLI de Azure
description: Parte 2 del tutorial, creación del código de la aplicación.
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: dba089c58cf6413263348b7bdeb975852c2e6a2f
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467181"
---
# <a name="create-the-app-code-using-express"></a>Creación del código de la aplicación mediante Express

[Paso anterior: Introducción y requisitos previos](tutorial-vscode-azure-cli-node-01.md)

En este paso, puede crear una aplicación sencilla de Node.js con [Express](https://www.expressjs.com) mediante [Express Generator](https://expressjs.com/en/starter/generator.html).

1. Use el siguiente comando para ejecutar el generador de Express y aplicar la técnica de scaffolding a una nueva aplicación de Express denominada "myExpressApp". (Los parámetros `--view pug --git` indican al generador que use el motor de plantillas [pug](https://pugjs.org/api/getting-started.html), antes conocido como Jade, y que cree un archivo *.gitignore*.

    ```bash
    npx express-generator myExpressApp --view pug –git
    ```

1. Vaya a la carpeta de la aplicación e instale las dependencias de esta mediante la ejecución de los siguientes comandos:

    ```bash
    cd myExpressApp
    npm install
    ```

1. Inicie el servidor de aplicaciones ejecutando el siguiente comando:

    ```bash
    npm start
    ```

1. Abra [http://localhost:3000](http://localhost:3000) en un explorador para ver la aplicación en ejecución:

    ![Ejecución local de la aplicación de Express](media/azure-cli/local-app.png)

1. Cuando haya terminado de probar la aplicación, detenga el servidor presionando **Ctrl**+**C** en el terminal donde ejecutó `npm start`.

> [!div class="nextstepaction"]
> [He creado la aplicación](tutorial-vscode-azure-cli-node-03.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=express)
