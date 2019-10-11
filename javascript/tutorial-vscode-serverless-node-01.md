---
title: Implementación de Azure Functions en Node.js con Visual Studio Code
description: Parte 1 del tutorial, introducción y requisitos previos.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 5dd41d710df629565699cff3bfd8e4bca7677087
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686088"
---
# <a name="deploy-azure-functions-from-visual-studio-code"></a>Implementación de Azure Functions con Visual Studio Code

En este tutorial, usará Visual Studio Code y la extensión de Azure Functions para crear e implementar una aplicación de Azure Functions escrita con JavaScript. 

## <a name="prerequisites"></a>Requisitos previos

- Una [suscripción de Azure](#azure-subscription).
- [Visual Studio Code](https://code.visualstudio.com/)
- La [extensión de Azure Functions](vscode:extension/ms-azuretools.vscode-azurefunctions)
- [Node.js y npm](https://nodejs.org/en/download), el administrador de paquetes de Node.js.

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azurefunctions">Instalación de la extensión de Azure Functions</a>

### <a name="azure-subscription"></a>Suscripción de Azure

Si no tiene una suscripción de Azure, [regístrese ahora](https://azure.microsoft.com/en-us/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension) para obtener una cuenta gratuita con 200 USD en crédito de Azure para probar cualquier combinación de servicios.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="install-the-azure-functions-core-tools"></a>Instalación de Azure Functions Core Tools

Para habilitar la depuración local, debe instalar [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools), lo cual se puede hacer directamente en Visual Studio Code.

1. Inicie Visual Studio Code.

1. Abra la **paleta de comandos** (**F1**), escriba **Azure Functions: Install or Update Azure Functions Core Tools** (Instalar o actualizar Azure Functions Core Tools) y presione **ENTRAR** para ejecutar el comando.

1. Para comprobar la instalación, seleccione el comando de menú **Terminal** > **Nuevo terminal** en VS Code y, después, ejecute el comando `func`. El comando debe mostrar una salida similar a la siguiente (junto con la información de uso).

    ```output
                      %%%%%%
                     %%%%%%
                @   %%%%%%    @
              @@   %%%%%%      @@
           @@@    %%%%%%%%%%%    @@@
         @@      %%%%%%%%%%        @@
           @@         %%%%       @@
             @@      %%%       @@
               @@    %%      @@
                    %%
                    %

    Azure Functions Core Tools (2.4.419 Commit hash: c9c1724d002bd90b2e6b41393915ea3a26bcf0ce)
    Function Runtime Version: 2.0.12332.0
    ```

> [!div class="nextstepaction"]
> [He instalado los requisitos previos](tutorial-vscode-serverless-node-02.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=getting-started)
