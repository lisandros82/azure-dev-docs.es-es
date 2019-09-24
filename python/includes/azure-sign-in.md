---
ms.openlocfilehash: d64a5c908915b5d020f27e1c501aee852f04ae38
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019663"
---
Una vez instalada la extensión de Azure, inicie sesión en su cuenta de Azure; para ello, vaya al explorador de **Azure: App Service**, seleccione **Iniciar sesión en Azure** y siga las indicaciones.

![Iniciar sesión en Azure mediante VS Code](../media/deploy-azure/azure-sign-in.png)

Después de iniciar sesión, compruebe que la dirección de correo electrónico de la suscripción de Azure aparece en la barra de estado y que las suscripciones aparecen en el explorador de **Azure: App Service**:

![Barra de estado de VS Code que muestra la cuenta de Azure](../media/deploy-azure/azure-account-status-bar.png)

![Explorador de Azure App Service de VS Code que muestra las suscripciones](../media/deploy-azure/azure-subscription-view.png)

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
