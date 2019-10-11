---
title: Implementación de archivos de aplicación de Node.js estáticos en Azure Storage desde Visual Studio Code
description: Parte 4 del tutorial, implementación de los archivos en Azure Storage
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: b1295fcb9a90ca26880715296e4214c2a1df6a07
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685945"
---
# <a name="deploy-the-website-to-azure-storage"></a>Implementación del sitio web en Azure Storage

[Paso anterior: Creación de una cuenta de Storage](tutorial-vscode-static-website-node-03.md)

En este paso, usará Visual Studio Code para implementar los archivos del sitio web estático creados en los pasos anteriores para Azure Storage que, a continuación, hospeda y da servicio a esos archivos.

1. En Visual Studio Code, vaya al explorador de **Azure Storage**, expanda su suscripción, expanda el nodo de la cuenta de Azure Storage que creó en el paso anterior y, a continuación, expanda el nodo **Blob Containers**. El contenedor `$web` es donde se implementa el código de la aplicación.

    ![Nodos de Azure Storage en el explorador de Azure Storage](media/static-website/storage-nodes.png)

1. Seleccione el explorador **Files** (Archivos), haga clic con el botón derecho en la carpeta *build* y elija **Deploy to Static Website** (Implementar en sitio web estático):

    ![Comando Deploy to Static Website (Implementar en sitio web estático)](media/static-website/deploy-build.png)

1. Cuando se le solicite, elija la cuenta de almacenamiento que creó anteriormente.

1. Una vez completada la implementación, aparece un mensaje con un botón **Browse to website** (Ir al sitio web). Seleccione ese botón para abrir el punto de conexión principal del código de aplicación implementado.

    ![Mensaje de implementación completada](media/static-website/deployment-complete.png)

    ![Sitio web estático en ejecución en Azure](media/static-website/azure-app.png)

> [!div class="nextstepaction"]
> [Mi sitio se encuentra en Azure](tutorial-vscode-static-website-node-05.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
