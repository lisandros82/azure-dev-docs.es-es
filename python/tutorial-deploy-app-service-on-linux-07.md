---
title: 'Tutorial: Limpieza de recursos después de la implementación en Azure App Service en Linux desde Visual Studio Code'
description: Paso 7 del tutorial, limpieza de recursos de Azure
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 8145b33ae52427d55c9b3de9fcf6fb20467b7ba9
ms.sourcegitcommit: a8073315f751631ab983618fa9f812eb95d8b2dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125268"
---
# <a name="tutorial-clean-up-resources-after-deploying-to-azure-app-service-on-linux-from-visual-studio-code"></a>Tutorial: Limpieza de recursos después de la implementación en Azure App Service en Linux desde Visual Studio Code

[Paso anterior: transmisión de registros](tutorial-deploy-app-service-on-linux-06.md)

El servicio de aplicaciones de Azure que ha creado incluye un plan de App Service de respaldo que puede incurrir en costos. Para evitar estos costos, elimine el grupo de recursos que contiene todos los recursos.

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

Enhorabuena, ha completado este tutorial de implementación de código de Python en App Service en Linux.

Como se indicó anteriormente, encontrará más información sobre la extensión App Service en el repositorio de GitHub [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice). Los problemas detectados y las contribuciones también son bienvenidos.

Para más información sobre los servicios de Azure que puede usar desde Python, incluido el almacenamiento de datos, junto con la inteligencia artificial y Machine Learning Service, visite el [Centro para desarrolladores de Python de Azure](https://docs.microsoft.com/python/azure/?view=azure-python).

También hay otras extensiones de Azure para VS Code que pueden resultarle útiles. Solo tiene que buscar "Azure" en el explorador de extensiones:

![Extensiones de Azure para Visual Studio Code](media/deploy-containers/azure-extensions-for-visual-studio-code.png)

Algunas extensiones populares son:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Funciones de Azure](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Herramientas de la CLI de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Herramientas de Azure Resource Manager (ARM)](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Ya he terminado](https://docs.microsoft.com/python/azure/?view=azure-python) 

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=07-clean-up-resources)
