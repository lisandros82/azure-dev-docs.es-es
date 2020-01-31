---
title: Herramientas para los desarrolladores que utilizan el SDK de Azure para Go
description: Herramientas para trabajar con el SDK de Azure para Go y los servicios de Azure
ms.date: 09/05/2018
ms.topic: conceptual
ms.openlocfilehash: b08a578c3e5ceb7381cc1643b2967405b292376e
ms.sourcegitcommit: 4cf22356d6d4817421b551bd53fcba76bdb44cc1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76872047"
---
# <a name="tools-for-developers-using-the-azure-sdk-for-go"></a>Herramientas para los desarrolladores que utilizan el SDK de Azure para Go

Estas son algunas herramientas recomendadas para escribir código de Go eficiente y hacer que funcione sin problemas con los servicios de Azure.

## <a name="azure-cli"></a>Azure CLI

La CLI de Azure proporciona una interfaz de la línea de comandos para crear y configurar recursos de Azure en las suscripciones. La CLI le permitirá empezar a crear recursos de Azure comunes y compartidos rápidamente, para que pueda centrarse en el uso más complejo de los servicios. La CLI tiene características de consulta y filtrado, por lo que puede canalizar la salida directamente a sus herramientas de la línea de comandos favoritas. La CLI está disponible para su instalación en el sistema local como una imagen de Docker o a través de [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

> [!div class="nextstepaction"]
> [Instalación de la CLI de Azure](/cli/azure/install-azure-cli)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code es un editor ligero que ofrece compatibilidad para Go. Esta extensión incluye compatibilidad con características como autocompletar, plantillas de `impl`, refactorización y depuración. Visual Studio Code también permite el acceso en el editor para el control de código fuente, así como extensiones para trabajar con servicios de Azure.

* [Instalación de Visual Studio Code](https://code.visualstudio.com/Download)
* [Obtención de la extensión de Visual Studio Code para Go](https://code.visualstudio.com/docs/languages/go)
* [Obtención de la extensión de Visual Studio Code Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)

## <a name="cicd-with-azure-devops-project"></a>Integración continua y entrega continua con Azure DevOps Project

Canalizaciones de Azure DevOps Projects le permiten configurar un sistema de integración continua para las aplicaciones de Go. Solo se necesita un repositorio de Git y puede implementar y probar directamente en Azure.

> [!div class="nextstepaction"]
> [Aprenda cómo crear una canalización de CI/CD con Azure DevOps Projects](/azure/devops-project/azure-devops-project-go)

## <a name="dependency-management-with-dep"></a>Administración de dependencias con dep

Azure SDK para Go usa dep para la administración de dependencias. El comando dep permite incorporar los cambios y preparar la aplicación de Go, evitar conflictos de versión y asegurarse de que el proyecto funcionará correctamente.

> [!div class="nextstepaction"]
> [Obtención del administrador de dependencias dep](https://github.com/golang/dep)
