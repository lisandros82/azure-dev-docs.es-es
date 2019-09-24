---
title: Limpieza de recursos después de la implementación en Azure App Service en Linux desde Visual Studio Code
description: Paso 7 del tutorial, limpieza de recursos de Azure
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: a86207477deb4652ef1d98c1130329bc3e0c91fc
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019613"
---
# <a name="clean-up-resources"></a>Limpieza de recursos

[Paso anterior: transmisión de registros](tutorial-deploy-app-service-on-linux-06.md)

El servicio de aplicaciones que ha creado incluye un plan de App Service de respaldo que puede incurrir en costos. Para limpiar los recursos, haga clic con el botón derecho en el servicio de aplicaciones, en el explorador de **Azure: App Service**, y seleccione **Eliminar**.

También puede visitar [Azure Portal](https://portal.azure.com), seleccionar **Grupos de recursos** en el panel de navegación izquierdo, seleccionar el grupo de recursos que se creó en el proceso de este tutorial y, a continuación, usar el comando **Eliminar grupo de recursos**.

## <a name="next-steps"></a>Pasos siguientes

Enhorabuena, ha completado este tutorial de implementación de código de Python en App Service en Linux.

Como se indicó anteriormente, encontrará más información sobre la extensión App Service en el repositorio de GitHub [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice). Los problemas detectados y las contribuciones también son bienvenidos.

Para más información sobre los servicios de Azure que puede usar desde Python, incluido el almacenamiento de datos, junto con la inteligencia artificial y Machine Learning Service, visite el [Centro para desarrolladores de Python de Azure](https://docs.microsoft.com/python/azure/?view=azure-python).

También hay otras extensiones de Azure para VS Code que pueden resultarle útiles. Solo tiene que buscar "Azure" en el explorador de extensiones:

![Extensiones de Azure para VS Code](media/deploy-containers/azure-extensions.png)

Algunas extensiones populares son:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Herramientas de la CLI de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Herramientas de Azure Resource Manager (ARM)](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Ya he terminado](https://docs.microsoft.com/python/azure/?view=azure-python) 

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=07-clean-up-resources)
