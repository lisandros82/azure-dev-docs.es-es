---
title: Creación de una aplicación estática de Node.js en Visual Studio Code
description: Parte 2 del tutorial, creación de una aplicación de ejemplo
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 566d166a69bbbee59726b8e381bee4a24077d8c6
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685979"
---
# <a name="create-the-app"></a>Creación de la aplicación

[Paso anterior: Introducción y requisitos previos](tutorial-vscode-static-website-node-01.md)

En este paso, va a usar la CLI de la utilidad React, [create-react-app](https://github.com/facebook/create-react-app), para crear una aplicación sencilla de React que se puede implementar en Azure. Como alternativa, puede usar Angular, Vue, otra plataforma o cualquier carpeta que contenga varios archivos HTML. Si ya tiene una aplicación lista para su implementación, puede ir directamente a [Creación de una cuenta de Azure Storage](tutorial-vscode-static-website-node-03.md).

1. Utilice la herramienta create-react-app para aplicar la técnica de scaffolding a una nueva aplicación de React llamada `my-react-app`. Para ello, ejecute el comando:

    ```bash
    npm create react-app my-react-app
    ```

1. Compile la aplicación. Para ello, cambie a la nueva carpeta y ejecute `npm run build`:

    ```bash
    cd my-react-app
    npm run build
    ```

1. Ahora, debería tener una carpeta *build* en la carpeta *my-react-app*. La carpeta *build* contiene los archivos HTML, CSS y JavaScript que ha implementado en Azure Storage.

1. Ejecute la aplicación mediante el comando siguiente:

    ```bash
    npm start
    ```

1. Abra [http://localhost:3000](http://localhost:3000) en un explorador para comprobar que la aplicación está funcionando:

    ![La aplicación de React de ejemplo en funcionamiento](media/static-website/local-app.png)

1. Detenga el servidor presionando **Ctrl**+**C** en el terminal o en el símbolo del sistema.

> [!div class="nextstepaction"]
> [He creado la aplicación](tutorial-vscode-static-website-node-03.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
