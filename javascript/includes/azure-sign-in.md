---
ms.openlocfilehash: ce5cdbebca22aa198a3e8b6cf883a36dfd4e4a57
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686055"
---
Una vez instalada la extensión de Azure, inicie sesión en su cuenta de Azure; para ello, vaya al explorador de **Azure**, seleccione **Sign in to Azure** (Iniciar sesión en Azure) y siga las indicaciones. (Si tiene varias extensiones de Azure instaladas, seleccione una para el área en la que está trabajando, como App Service, Functions, etc.)

![Iniciar sesión en Azure mediante VS Code](../media/deploy-azure/azure-sign-in.png)

Después de iniciar sesión, compruebe que la dirección de correo electrónico de la cuenta de Azure (o cuenta "Sesión iniciada") aparece en la barra de estado y que las suscripciones aparecen en el explorador de **Azure**:

![Barra de estado de VS Code que muestra la cuenta de Azure](../media/deploy-azure/azure-account-status-bar.png)

![Explorador de Azure en VS Code que muestra las suscripciones](../media/deploy-azure/azure-subscription-view.png)

> [!NOTE]
> Si ve el error **"No se encuentra la suscripción con el nombre [identificador de suscripción]"** , puede deberse a que está detrás de un servidor proxy y no puede acceder a la API de Azure. Configure las variables de entorno `HTTP_PROXY` y `HTTPS_PROXY` con la información del servidor proxy en el terminal:
>
> ```bash
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
> ```
>
> ```powershell
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
