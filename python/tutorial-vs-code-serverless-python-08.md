---
title: 'Tutorial: Limpie los recursos utilizados con código de Python en Azure Functions'
description: Paso 8 del tutorial, limpieza de recursos de Azure para evitar incurrir en cargos continuos.
ms.topic: conceptual
ms.date: 01/15/2020
ms.custom: seo-python-october2019
ms.openlocfilehash: 264c09a8d84c7115bb0a56d0455d576187695db0
ms.sourcegitcommit: a8073315f751631ab983618fa9f812eb95d8b2dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125258"
---
# <a name="tutorial-clean-up-azure-resources-for-azure-functions"></a>Tutorial: Limpieza de los recursos de Azure para Azure Functions

[Paso anterior: adición de un enlace de almacenamiento](tutorial-vs-code-serverless-python-07.md)

En este artículo se muestra cómo quitar los recursos de Azure creados en este tutorial. La aplicación de funciones de Azure que ha creado con Visual Studio Code incluye recursos que pueden incurrir en costos mínimos. Para más información, consulte los [precios de Azure Functions](https://azure.microsoft.com/pricing/details/functions/).

La mejor manera de limpiar los recursos es eliminar el grupo de recursos que contiene todos los recursos individuales que se usan en este tutorial. Entre los recursos están la aplicación de funciones, la cuenta de almacenamiento y el plan de App Service correspondiente.

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

En Visual Studio Code, en el menú contextual de la aplicación de funciones en el explorador **Azure: Functions**, es posible que vea un comando **Delete Function App** (Eliminar aplicación de funciones). El comando solo elimina la aplicación de funciones pero deja otros recursos, lo que puede incurrir en costos continuos.

## <a name="next-steps"></a>Pasos siguientes

Enhorabuena por completar este tutorial de implementación de código de Python en Azure Functions. Ahora está listo para crear muchas más funciones sin servidor.

Como se indicó anteriormente, encontrará más información sobre la extensión de Functions en el repositorio de GitHub [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions). Los problemas detectados y las contribuciones también son bienvenidos.

Consulte la [Introducción a Azure Functions](/azure/azure-functions/functions-overview) para explorar los distintos desencadenadores que puede usar.

Para más información sobre los servicios de Azure que puede usar desde Python, incluido el almacenamiento de datos, junto con la inteligencia artificial y Machine Learning Service, visite el [Centro para desarrolladores de Python de Azure](/azure/python/?view=azure-python).

También hay otras extensiones de Azure para Visual Studio Code que pueden resultarle útiles. Solo tiene que buscar "Azure" en el explorador de extensiones:

![Extensiones de Azure para Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-extensions-for-visual-studio-code.png)

Algunas extensiones populares son:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Consulte el [tutorial de implementación en Azure App Service](tutorial-deploy-app-service-on-linux-01.md).
- [Herramientas de la CLI de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Ya he terminado](https://docs.microsoft.com/python/azure/?view=azure-python)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)
