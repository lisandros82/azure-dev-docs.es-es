---
ms.openlocfilehash: fcbce1c0f17721c7f3fd90e8d396baba6ae100c4
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278844"
---
Una vez instalada la extensión de Azure, inicie sesión en su cuenta de Azure; para ello, vaya al explorador de **Azure**, seleccione **Sign in to Azure** (Iniciar sesión en Azure) y siga las indicaciones. (Si tiene varias extensiones de Azure instaladas, seleccione una para el área en la que está trabajando, como App Service, Functions, etc.)

![Iniciar sesión en Azure mediante VS Code](../media/deploy-azure/sign-in-to-azure-through-visual-studio-code.png)

Después de iniciar sesión, compruebe que la dirección de correo electrónico de la cuenta de Azure (o cuenta "Sesión iniciada") aparece en la barra de estado y que las suscripciones aparecen en el explorador de **Azure**:

![Barra de estado de Visual Studio Code que muestra la cuenta de Azure](../media/deploy-azure/azure-account-status-bar-in-visual-studio-code.png)

![Explorador de Azure App Service de Visual Studio Code que muestra las suscripciones](../media/deploy-azure/view-azure-subscription-in-visual-studio-code-app-service-explorer.png)

> [!NOTE]
> Si ve el error **"No se encuentra la suscripción con el nombre [identificador de suscripción]"** , puede deberse a que está detrás de un servidor proxy y no puede acceder a la API de Azure. Configure las variables de entorno `HTTP_PROXY` y `HTTPS_PROXY` con la información del servidor proxy en el terminal:
>
> ```sh
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
>
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
