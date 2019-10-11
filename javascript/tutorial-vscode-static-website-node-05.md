---
title: Implementación de cambios en un sitio web de Node.js estático desde Visual Studio Code
description: Parte 5 del tutorial, realización de cambios y reimplementación
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 986d2a0f8999d79dfd1d856ed20a053c495a3765
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685940"
---
# <a name="make-changes-and-redeploy"></a>Cambios y reimplementación

[Paso anterior: Implementación en Azure Storage](tutorial-vscode-static-website-node-04.md)

En este paso, realizará un cambio sencillo en el código fuente de la aplicación y reimplementará el sitio para experimentar el flujo de trabajo de implementación de un extremo a otro.

1. En Visual Studio Code, abra el archivo *src/app.js* y cambie la línea 11 para que coincida con lo siguiente:

    ```js
    <h1 className="App-title">Welcome to Azure!</h1>
    ```

1. En un terminal o símbolo del sistema, ejecute `npm run build`.

1. En VS Code, haga clic con el botón derecho en la carpeta *build* actualizada y elija de nuevo **Deploy to Static Website** (Implementar en sitio web estático). Elija su cuenta de almacenamiento y confirme que desea implementar los cambios. (La extensión de Azure eliminará automáticamente los archivos antiguos antes de implementar los cambios para evitar problemas de almacenamiento en caché).

1. Una vez finalizada la implementación, actualice el sitio en el explorador para observar los cambios:

    ![Cambios en la aplicación después de la reimplementación](media/static-website/updated-azure-app.png)

> [!div class="nextstepaction"]
> [He implementado los cambios](tutorial-vscode-static-website-node-06.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=code-change)
