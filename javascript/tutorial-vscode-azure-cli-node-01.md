---
title: Implementación de aplicaciones de Node.js en Azure App Service mediante la CLI de Azure
description: Parte 1 del tutorial, introducción y requisitos previos.
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: b249084e6c22491bd05dbb3df2544f8570dadad0
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466794"
---
# <a name="deploy-to-azure-app-service-using-the-azure-cli"></a>Implementación en Azure App Service mediante la CLI de Azure

En este tutorial, va a implementar una aplicación de Node.js en Azure App Service mediante la [interfaz de la línea de comandos (CLI) de Azure](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest), que se ejecuta en todos los sistemas operativos. Con la CLI puede crear recursos de Azure, configurar una canalización de implementación entre un repositorio de Git y Azure, y ver la salida `console.log` de la aplicación.

## <a name="prerequisites"></a>Requisitos previos

- Una [suscripción de Azure](#azure-subscription).
- [Node js y npm 6.x o posterior](https://nodejs.org/en/download), el administrador de paquetes de Node.js.
- [Git](https://git-scm.com/downloads), después del cual el comando `git --version` debe mostrar un número de versión.
- La[CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

De forma alternativa, puede usar la [extensión de la CLI de Azure para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli), que proporciona una coloración de sintaxis, IntelliSense (finalizaciones) y fragmentos de código al escribir scripts de la CLI de Azure.

Una segunda alternativa consiste en utilizar [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview), que puede usar en Visual Studio Code mediante la [extensión de la cuenta de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

### <a name="azure-subscription"></a>Suscripción de Azure

Si no tiene una suscripción de Azure, [regístrese ahora](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-node-git&mktingSource=vscode-tutorial-node-git) para obtener una cuenta gratuita con 200 USD en crédito de Azure para probar cualquier combinación de servicios.

### <a name="sign-in-to-the-azure-cli"></a>Inicio de sesión en la CLI de Azure

Una vez instalada la CLI de Azure, ejecute el siguiente comando desde un símbolo del sistema o un terminal:

```bash
az login
```

El comando abre una ventana del explorador en la que se le pide que inicie sesión en Azure. Una vez que haya iniciado sesión, la ventana del terminal muestra la salida JSON con los detalles de la suscripción.

## <a name="check-npm-version"></a>Comprobación de la versión de npm

Si ya tiene Node.js instalado, ejecute el comando `node -v` y compruebe que la versión es 10.x u otra posterior. Si tiene una versión más antigua, [actualice](https://nodejs.org/en/download/) a la versión LTS (con soporte técnico a largo plazo) más reciente.

> [!div class="nextstepaction"]
> [He instalado los requisitos previos](tutorial-vscode-azure-cli-node-02.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=getting-started)
