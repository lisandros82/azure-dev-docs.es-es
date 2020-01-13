---
title: 'Tutorial: Creación e implementación de Azure Functions sin servidor en Python con VS Code'
description: Paso 1 del tutorial, introducción y requisitos previos.
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: a380a447150f29653a1f94a3fe1f6464dd495a81
ms.sourcegitcommit: fc3408b6e153c847dd90026161c4c498aa06e2fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2019
ms.locfileid: "75191002"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>Tutorial: Creación e implementación de Azure Functions sin servidor en Python con Visual Studio Code

En este artículo, usará Visual Studio Code y la extensión de Azure Functions para crear un punto de conexión HTTP sin servidor con Python y agregar también una conexión (o "enlace") al almacenamiento.

Azure Functions ejecuta el código en un entorno sin servidor sin necesidad de aprovisionar una máquina virtual ni publicar una aplicación web. La extensión de Azure Functions para Visual Studio Code simplifica considerablemente el proceso de uso de Functions mediante la administración automática de muchos trabajos de configuración.

Si tiene problemas con cualquiera de los pasos de este tutorial, nos encantaría conocer los detalles. Use el botón **He tenido un problema** al final de cada artículo para enviar sus comentarios.

## <a name="prerequisites"></a>Prerequisites

- Una [suscripción de Azure](#azure-subscription).
- [Azure Functions Core Tools](#azure-functions-core-tools).
- [Visual Studio Code con la extensión de Azure Functions](#visual-studio-code-python-and-the-azure-functions-extension).

### <a name="azure-subscription"></a>Suscripción de Azure

Si no tiene una suscripción de Azure, [regístrese ahora](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension) para obtener una cuenta gratuita de 30 días con 200 $ en créditos de Azure para probar cualquier combinación de servicios.

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

Siga las instrucciones correspondientes a su sistema operativo de [Trabajo con Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2) para instalar Azure Functions Core Tools. Ignore los comentarios en el artículo acerca del administrador de paquetes Chocolately, que no son necesarios para completar este tutorial.

Al instalar Node.js, use las opciones predeterminadas y *no* seleccione la opción de instalar automáticamente las herramientas necesarias.  Asegúrese también de usar la opción `-g` con los comandos `npm install` para que Core Tools estén disponibles para los comandos posteriores.

    > [!TIP]
    > The Core Tools are written in .NET Core, and the Core Tools package is best installed using the Node.js package manager, npm, which is why you need to install .NET Core and Node.js at present, even for working with Azure Functions in Python. You can, however bypass the .NET Core requirement using "extension bundles" as described in the aforementioned documentation. Whatever the case, you need install these components only once, after which Visual Studio Code automatically prompts you to install any updates.

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code, Python y la extensión de Azure Functions

Instale el siguiente software:

- Python 3.7 o Python 3.6 tal como requiere Azure Functions. [Python 3.7.5](https://www.python.org/downloads/release/python-375/) y [Python 3.6.8](https://www.python.org/downloads/release/python-368/) son las últimas versiones compatibles. Desplácese hacia abajo en estas páginas para encontrar los instaladores. Durante la instalación, seleccione **Add Python 3.x to PATH** (Agregar Python 3.x a PATH) y seleccione la opción **Install Now** (Instalar ahora) para usar las opciones predeterminadas. En Windows, seleccione también **Disable Path length limit** (Deshabilitar límite de longitud de ruta) al final del proceso.
- [Visual Studio Code](https://code.visualstudio.com/).
- La [extensión de Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python), como se describe en [Tutorial de Python en Visual Studio Code: requisitos previos](https://code.visualstudio.com/docs/python/python-tutorial).
- La [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions). Para información general, visite [el repositorio de GitHub vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions).

    > [!NOTE]
    > La extensión de Azure Functions se incluye con el [paquete de extensiones de herramientas de Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack).

### <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>Verificar los requisitos previos

Para comprobar que están instaladas todas las herramientas de Azure Functions, abra la paleta de comandos de Visual Studio Code (**F1**), seleccione el comando **Terminal: crear nuevo terminal integrado** y, una vez que se abre el terminal, ejecute el comando `func`:

![Comprobación de las herramientas básicas necesarias para Azure Functions](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

La salida que empieza con el logotipo de Azure Functions (debe desplazarse hacia arriba en la salida) indica que Azure Functions Core Tools está presente.

Si no se reconoce el comando `func`, vuelva a ejecutar `npm install -g azure-functions-core-tools` y compruebe que la instalación se realiza correctamente. Asegúrese también de utilizar el modificador `-g` con el comando de instalación; de lo contrario, npm instala el paquete solo en la carpeta actual.

El comando `func` funciona a través del archivo *func.cmd*, que se instala en la carpeta global de Node.js. Para ver la ubicación de esta carpeta, ejecute `npm -l` y examine la ubicación al final de la salida.

> [!div class="nextstepaction"]
> [He iniciado sesión en Azure](tutorial-vs-code-serverless-python-02.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=01-verify-prerequisites)
