---
title: Implementación de cambios en un sitio web de Node.js estático desde Visual Studio Code
description: Parte 5 del tutorial, realización de cambios y reimplementación
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: 0db773cdea2e288dc461479c3753b94c9a286e82
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466643"
---
# <a name="make-changes-and-redeploy"></a>Cambios y reimplementación

[Paso anterior: Implementación en Azure Storage](tutorial-vscode-static-website-node-04.md)

En este paso, realizará un cambio sencillo en el código fuente de la aplicación y reimplementará el sitio para experimentar el flujo de trabajo de implementación de un extremo a otro.

# <a name="angulartabangular"></a>[Angular](#tab/angular)

1. En Visual Studio Code, abra el archivo _src/app/app.component.html_ y cambie la línea 305 para que coincida con lo siguiente:

    ```html
    <span>Welcome To Azure</span>
    ```

1. En un terminal o símbolo del sistema, ejecute `npm run build`.

1. En VS Code, haga clic con el botón derecho en la carpeta _dist/my-static-site_ actualizada y elija de nuevo **Deploy to Static Website** (Implementar en sitio web estático). Elija su cuenta de almacenamiento y confirme que desea implementar los cambios. (La extensión de Azure eliminará automáticamente los archivos antiguos antes de implementar los cambios para evitar problemas de almacenamiento en caché).

1. Una vez finalizada la implementación, actualice el sitio en el explorador para observar los cambios:

    ![Cambios en la aplicación después de la reimplementación](media/static-website/updated-azure-app-angular.png)

# <a name="reacttabreact"></a>[React](#tab/react)

1. En Visual Studio Code, abra el archivo _src/app.js_ y cambie la línea 11 para que coincida con lo siguiente:

    ```html
    <h1 className="App-title">Welcome to Azure!</h1>
    ```

1. En un terminal o símbolo del sistema, ejecute `npm run build`.

1. En VS Code, haga clic con el botón derecho en la carpeta _build_ actualizada y elija de nuevo **Deploy to Static Website** (Implementar en sitio web estático). Elija su cuenta de almacenamiento y confirme que desea implementar los cambios. (La extensión de Azure eliminará automáticamente los archivos antiguos antes de implementar los cambios para evitar problemas de almacenamiento en caché).

1. Una vez finalizada la implementación, actualice el sitio en el explorador para observar los cambios:

    ![Cambios en la aplicación después de la reimplementación](media/static-website/updated-azure-app-react.png)

# <a name="vuetabvue"></a>[Vue](#tab/vue)

1. En Visual Studio Code, abra el archivo _src/App.vue_ y cambie la línea 11 para que coincida con lo siguiente:

    ```html
    <HelloWorld msg="Welcome to Azure!" />
    ```

1. En un terminal o símbolo del sistema, ejecute `npm run build`.

1. En VS Code, haga clic con el botón derecho en la carpeta _dist_ actualizada y elija de nuevo **Deploy to Static Website** (Implementar en sitio web estático). Elija su cuenta de almacenamiento y confirme que desea implementar los cambios. (La extensión de Azure eliminará automáticamente los archivos antiguos antes de implementar los cambios para evitar problemas de almacenamiento en caché).

1. Una vez finalizada la implementación, actualice el sitio en el explorador para observar los cambios:

    ![Cambios en la aplicación después de la reimplementación](media/static-website/updated-azure-app-vue.png)

---

> [!div class="nextstepaction"]
> [He implementado los cambios](tutorial-vscode-static-website-node-06.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=code-change)
