---
title: 'Tutorial: Creación e implementación de Azure Functions sin servidor en Python con Visual Studio Code'
description: Paso 1 del tutorial, introducción y requisitos previos.
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 81121490f435c7ff2e2fe8ab397fa18bab7113a3
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278820"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>Tutorial: Creación e implementación de Azure Functions sin servidor en Python con Visual Studio Code

En este artículo, usará Visual Studio Code y la extensión de Azure Functions para crear un punto de conexión HTTP sin servidor con Python y agregar también una conexión (o "enlace") al almacenamiento.

Azure Functions ejecuta el código en un entorno sin servidor sin necesidad de aprovisionar una máquina virtual ni publicar una aplicación web. La extensión de Azure Functions para Visual Studio Code simplifica considerablemente el proceso de uso de Functions mediante la administración automática de muchos trabajos de configuración.

Si tiene problemas con cualquiera de los pasos de este tutorial, nos encantaría conocer los detalles. Use el botón **He tenido un problema** al final de cada artículo para enviar sus comentarios.

## <a name="prerequisites"></a>Requisitos previos

- Una [suscripción de Azure](#azure-subscription).
- [Visual Studio Code con la extensión de Azure Functions](#visual-studio-code-python-and-the-azure-functions-extension).
- [Azure Functions Core Tools](#azure-functions-core-tools).

### <a name="azure-subscription"></a>Suscripción de Azure

Si no tiene una suscripción de Azure, [regístrese ahora](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension) para obtener una cuenta gratuita de 30 días con 200 $ en créditos de Azure para probar cualquier combinación de servicios.

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code, Python y la extensión de Azure Functions

Instale el siguiente software:

- Python 3.6.x tal como requiere Azure Functions. [Python 3.6.8](https://www.python.org/downloads/release/python-368/) es la versión 3.6.x más reciente.
- [Visual Studio Code](https://code.visualstudio.com/)
- La [extensión de Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python), como se describe en [Tutorial de Python en Visual Studio Code: requisitos previos](https://code.visualstudio.com/docs/python/python-tutorial).
- La [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions). Para información general, visite [el repositorio de GitHub vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions).

> [!NOTE]
> La extensión de Azure Functions se incluye con el [paquete de extensiones de herramientas de Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack).

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Siga las instrucciones para su sistema operativo en [Trabajo con Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2). Las herramientas están escritas en .NET Core y el paquete Core Tools se instala mejor con el administrador de paquetes de Node.js, npm, que es el motivo por el que necesita instalar .NET Core y Node.js en la actualidad, incluso para el código de Python. Sin embargo, puede omitir el requisito de .NET Core con el uso de "conjuntos de extensión", tal y como se describe en la documentación mencionada anteriormente. Sea cual sea el caso, debe instalar estos componentes una sola vez, después de lo cual Visual Studio Code le pedirá automáticamente que instale las actualizaciones.

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>Verificar los requisitos previos

Para comprobar que están instaladas todas las herramientas de Azure Functions, abra la paleta de comandos de Visual Studio Code (**F1**), seleccione el comando **Terminal: crear nuevo terminal integrado** y, una vez que se abre el terminal, ejecute el comando `func`:

![Comprobación de las herramientas básicas necesarias para Azure Functions](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

La salida que empieza con el logotipo de Azure Functions (debe desplazarse hacia arriba en la salida) indica que Azure Functions Core Tools está presente.

Si no se reconoce el comando `func`, compruebe que la carpeta en la que instaló Azure Functions Core Tools está incluida en la variable de entorno PATH.

> [!div class="nextstepaction"]
> [He iniciado sesión en Azure](tutorial-vs-code-serverless-python-02.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=01-verify-prerequisites)
