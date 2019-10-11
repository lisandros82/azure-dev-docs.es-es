---
title: Limpieza de recursos después de implementar una aplicación de Node.js en Azure mediante la CLI de Azure
description: Parte 7 del tutorial, limpieza de recursos
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 6d0b9c671c4ffd0fb5d521e6247e9964d6d93980
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686112"
---
# <a name="clean-up-resources"></a>Limpieza de recursos

[Paso anterior: Cambios y reimplementación](tutorial-vscode-docker-node-06.md)

El servicio de aplicaciones que ha creado incluye un plan de App Service de respaldo que puede incurrir en costos. Para limpiar los recursos, ejecute el siguiente comando en un terminal o símbolo del sistema:

```bash
az group delete --name myResourceGroup
```

También puede visitar [Azure Portal](https://portal.azure.com), seleccionar **Grupos de recursos** en el panel de navegación izquierdo, seleccionar el grupo de recursos que se creó en el proceso de este tutorial y, a continuación, usar el comando **Eliminar grupo de recursos**.

> [!div class="nextstepaction"]
> [He terminado](node-howto-deploy-web-app.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)
