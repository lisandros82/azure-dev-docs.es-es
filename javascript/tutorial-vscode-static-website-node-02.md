---
title: Creación de una aplicación estática de Node.js en Visual Studio Code
description: Parte 2 del tutorial, creación de una aplicación de ejemplo
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: b57789ca26851c27a3a68c9d095f327ea64164cf
ms.sourcegitcommit: 2757d8bd0cc045b7d02f430d44de859f9de853f4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72587227"
---
# <a name="create-the-app"></a>Creación de la aplicación

[Paso anterior: Introducción y requisitos previos](tutorial-vscode-static-website-node-01.md)

En este paso, usará la interfaz de la línea de comandos (CLI) para [Angular](https://cli.angular.io/), [React](https://github.com/facebook/create-react-app) o [Vue](https://cli.vuejs.org/) para crear una aplicación sencilla que se pueda implementar en Azure. También puede usar cualquier otra plataforma de JavaScript que genere un conjunto de archivos estáticos o cualquier carpeta que contenga archivos HTML, CSS o JavaScript. Si ya tiene una aplicación lista para su implementación, puede ir directamente a [Creación de una cuenta de Azure Storage](tutorial-vscode-static-website-node-03.md).

# <a name="angulartabangular"></a>[Angular](#tab/angular)

1. Use la CLI para aplicar scaffolding a una nueva aplicación llamada "my-static-app" mediante la ejecución del siguiente comando:

    ```bash
    npx @angular/cli new my-static-app
    ```

    Cuando la CLI formule preguntas de configuración, presione Entrar para seleccionar las opciones predeterminadas.

1. Compile la aplicación. Para ello, cambie a la nueva carpeta y ejecute `npm run build`:

    ```bash
    cd my-static-app
    npm run build
    ```

1. Ahora, debería tener una carpeta _dist_ en la carpeta _my-static-app_. Dentro de esa carpeta _dist_, habrá una carpeta con el mismo nombre que el proyecto -_my-static-app_. La carpeta _build/my-static-app_ contiene los archivos HTML, CSS y JavaScript que ha implementado en Azure Storage.

1. Ejecute la aplicación mediante el comando siguiente:

    ```bash
    npm start
    ```

1. Abra [http://localhost:3000](http://localhost:3000) en un explorador para comprobar que la aplicación está funcionando:

    ![La aplicación de Angular de ejemplo en funcionamiento](media/static-website/local-app-angular.png)

1. Detenga el servidor presionando **Ctrl**+**C** en el terminal o en el símbolo del sistema.

# <a name="reacttabreact"></a>[React](#tab/react)

1. Use la CLI para aplicar scaffolding a una nueva aplicación llamada "my-static-app" mediante la ejecución del siguiente comando:

    ```bash
    npx create-react-app my-static-app
    ```

1. Compile la aplicación. Para ello, cambie a la nueva carpeta y ejecute `npm run build`:

    ```bash
    cd my-static-app
    npm run build
    ```

1. Ahora, debería tener una carpeta _build_ en la carpeta _my-static-app_. La carpeta _build_ contiene los archivos HTML, CSS y JavaScript que ha implementado en Azure Storage.

1. Ejecute la aplicación mediante el comando siguiente:

    ```bash
    npm start
    ```

1. Abra [http://localhost:3000](http://localhost:3000) en un explorador para comprobar que la aplicación está funcionando:

    ![La aplicación de React de ejemplo en funcionamiento](media/static-website/local-app-react.png)

1. Detenga el servidor presionando **Ctrl**+**C** en el terminal o en el símbolo del sistema.

# <a name="vuetabvue"></a>[Vue](#tab/vue)

1. Use la CLI para aplicar scaffolding a una nueva aplicación llamada "my-static-app" mediante la ejecución del siguiente comando:

    ```bash
    npx @vue/cli create my-static-app
    ```

Cuando la CLI formule preguntas de configuración, presione Entrar para seleccionar las opciones predeterminadas.

1. Compile la aplicación. Para ello, cambie a la nueva carpeta y ejecute `npm run build`:

    ```bash
    cd my-static-app
    npm run build
    ```

1. Ahora, debería tener una carpeta _dist_ en la carpeta _my-static-app_. La carpeta _dist_ contiene los archivos HTML, CSS y JavaScript que ha implementado en Azure Storage.

1. Ejecute la aplicación mediante el comando siguiente:

     ```bash
     npm run serve
     ```

1. Abra [http://localhost:8080](http://localhost:8080) en un explorador para comprobar que la aplicación está funcionando:

    ![La aplicación de Vue de ejemplo en funcionamiento](media/static-website/local-app-vue.png)

1. Detenga el servidor presionando **Ctrl**+**C** en el terminal o en el símbolo del sistema.

---

> [!div class="nextstepaction"]
> [He creado la aplicación](tutorial-vscode-static-website-node-03.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
