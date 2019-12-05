---
title: Creación de una instancia de Azure App Service desde Visual Studio Code
description: Parte 2 del tutorial, creación de la aplicación de Node.js
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 96c786b7cc8112c36c0aff06761417a97e30bf44
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466800"
---
# <a name="create-your-nodejs-application"></a>Creación de una aplicación de Node.js

[Paso anterior: Introducción y requisitos previos](tutorial-vscode-azure-app-service-node-01.md)

En este paso, creará una aplicación sencilla de Node.js con el generador de aplicaciones de Express que luego puede implementar en Azure.

También puede usar la aplicación del [tutorial de Node.js para Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial), en cuyo caso puede pasar directamente al paso [Implementación de la aplicación](tutorial-vscode-azure-app-service-node-03.md).

1. En un terminal o símbolo del sistema, use el siguiente comando para ejecutar el generador de Express y aplicar la técnica de scaffolding a una nueva aplicación de Express denominada "myExpressApp". (Los parámetros `--view pug --git` indican al generador que use el motor de plantillas [pug](https://pugjs.org/api/getting-started.html), antes conocido como Jade, y que cree un archivo *.gitignore*).

    ```bash
    npx express-generator myExpressApp --view pug -–git
    ```

1. Instale las dependencias de la aplicación ejecutando `npm install` en la carpeta de la aplicación:

    ```bash
    cd myExpressApp
    npm install
    ```

1. Inicie el servidor ejecutando `npm start`:

    ```bash
    npm start
    ```

1. Para probar la aplicación, abra [http://localhost:3000](http://localhost:3000) en un explorador. El sitio web debe tener el siguiente aspecto:

    ![Ejecución de una aplicación de Express](media/deploy-azure/express.png)

> [!div class="nextstepaction"]
> [He creado la aplicación de Node.js](tutorial-vscode-azure-app-service-node-03.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=create-app)
