---
title: Limpieza de los recursos de Azure
description: Paso 8 del tutorial, limpieza de recursos de Azure para evitar incurrir en cargos continuos.
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 6996121fc8ecba4489e2ec920de35574f6d1c5d8
ms.sourcegitcommit: d6575ac86449380b5a9c6c66aa722cb33ed53438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186175"
---
# <a name="clean-up-resources"></a>Limpieza de recursos

[Paso anterior: adición de un enlace de almacenamiento](tutorial-vs-code-serverless-python-07.md)

La aplicación de funciones que ha creado incluye recursos que pueden incurrir en costos mínimos (consulte [Precios de Functions](https://azure.microsoft.com/pricing/details/functions/)). Para limpiar los recursos, haga clic con el botón derecho en el explorador de **Azure: Functions** y seleccione **Eliminar aplicación de funciones**.

También puede visitar [Azure Portal](https://portal.azure.com), seleccionar **Grupos de recursos** en el panel de navegación izquierdo, seleccionar el grupo de recursos que se creó en el proceso de este tutorial y, a continuación, usar el comando **Eliminar grupo de recursos**.

## <a name="next-steps"></a>Pasos siguientes

Enhorabuena por completar este tutorial de implementación de código de Python en Azure Functions. Ahora está listo para crear muchas más funciones sin servidor.

Como se indicó anteriormente, encontrará más información sobre la extensión de Functions en el repositorio de GitHub [vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions). Los problemas detectados y las contribuciones también son bienvenidos.

Consulte la [Introducción a Azure Functions](/azure/azure-functions/functions-overview) para explorar los distintos desencadenadores que puede usar.

Para más información sobre los servicios de Azure que puede usar desde Python, incluido el almacenamiento de datos, junto con la inteligencia artificial y Machine Learning Service, visite el [Centro para desarrolladores de Python de Azure](/azure/python/?view=azure-python).

También hay otras extensiones de Azure para Visual Studio Code que pueden resultarle útiles. Solo tiene que buscar "Azure" en el explorador de extensiones:

![Extensiones de Azure para Visual Studio Code](media/tutorial-vs-code-serverless-python/azure-extensions.png)

Algunas extensiones populares son:

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Consulte el [tutorial de implementación en Azure App Service](tutorial-deploy-app-service-on-linux-01.md).
- [Herramientas de la CLI de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Herramientas de Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [Ya he terminado](https://docs.microsoft.com/python/azure/?view=azure-python)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)
