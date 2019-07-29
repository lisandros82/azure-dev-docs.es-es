---
title: Instrucciones de inicio de sesión del kit de herramientas de Azure para IntelliJ
description: Obtenga información sobre cómo iniciar sesión en Microsoft Azure con el kit de herramientas de Azure para IntelliJ.
services: ''
documentationcenter: java
author: bmitchell287
manager: douge
editor: ''
ms.assetid: ''
ms.author: brendm
ms.date: 02/01/2018
ms.devlang: Java
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: 7c2cec950c8a8bd51a7e1c6f9e5de390e1799c15
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "68278866"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>Instrucciones de inicio de sesión del kit de herramientas de Azure para IntelliJ

Una vez [instalado](https://www.jetbrains.com/help/idea/managing-plugins.html), [Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) proporciona dos métodos para iniciar sesión en su cuenta de Azure:

  - [Iniciar sesión en su cuenta de Azure mediante el inicio de sesión de dispositivo](#sign-in-to-your-azure-account-by-device-login)
  - [Iniciar sesión en su cuenta de Azure mediante entidad de servicio](#sign-in-to-your-azure-account-by-service-principal)

También se proporcionan métodos de [**Cierre de sesión**](#sign-out-of-your-azure-account).

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>Inicio de sesión en su cuenta de Azure mediante el inicio de sesión de dispositivo

Para iniciar sesión en Azure mediante el inicio de sesión de dispositivo, haga lo siguiente:

1. Abra el proyecto con IntelliJ IDEA.

2. Abra la barra lateral **Explorador de Azure** y, a continuación, haga clic en el icono **Inicio de sesión de Azure** en la barra de la parte superior (o en el menú IDEA **Herramientas/Azure/Inicio de sesión de Azure**).

   ![El comando de inicio de sesión en Azure IntelliJ][I01]

3. En la ventana **Inicio de sesión de Azure**, seleccione **Inicio de sesión de dispositivo** y, luego, haga clic en **Iniciar sesión**.

   ![Ventana Inicio de sesión de Azure con el inicio de sesión de dispositivo seleccionado][I02]

4. Haga clic en **Copiar y abrir** en el cuadro de diálogo **Inicio de sesión de dispositivo de Azure**.

   ![El cuadro de diálogo Inicio de sesión en Microsoft Azure][I03]

5. En el explorador, pegue el código del dispositivo (que se ha copiado al hacer clic en **Copiar y abrir** en el último paso) y, a continuación, haga clic en **Siguiente**.

   ![Explorador de inicio de sesión de dispositivo][I04]

6. En el cuadro de diálogo **Seleccionar suscripciones**, elija las suscripciones que desea usar y, luego, haga clic en **Aceptar**.

   ![Cuadro de diálogo Seleccionar suscripciones][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>Inicio de sesión en su cuenta de Azure mediante entidad de servicio

Los siguientes pasos lo guiarán por el proceso de creación de un archivo de credenciales que contiene los datos de entidades de servicio. Cuando finalice este proceso, IntelliJ usará el archivo de credenciales para iniciar sesión automáticamente en Azure cuando abra el proyecto.

1. Abra el proyecto con IntelliJ IDEA.

1. Abra la barra lateral **Explorador de Azure** y, a continuación, haga clic en el icono **Inicio de sesión de Azure** en la barra de la parte superior (o en el menú IDEA **Herramientas/Azure/Inicio de sesión de Azure**).
   ![El comando de inicio de sesión en Azure IntelliJ][A01]

1. En la ventana **Iniciar sesión de Azure**, seleccione **Entidad de servicio** y haga clic en **Nuevo**.

   ![Ventana de inicio de sesión de Azure con la entidad de servicio seleccionada][A02]

1. Haga clic en **Copiar y abrir** en el cuadro de diálogo **Inicio de sesión de dispositivo de Azure**.

   ![El cuadro de diálogo Inicio de sesión en Microsoft Azure][A03]

1. En el explorador, pegue el código del dispositivo (que se ha copiado al hacer clic en **Copiar y abrir** en el último paso) y, a continuación, haga clic en **Siguiente**.

   ![Explorador de inicio de sesión de dispositivo][A04]

1. En la ventana **Create authentication files** (Crear archivos de autenticación), seleccione las suscripciones que desea utilizar, elija el directorio de destino y, luego, haga clic en **Iniciar**.

   ![La ventana Create authentication files (Crear archivos de autenticación)][A05]

1. En el cuadro de diálogo **Estado de creación de entidades de servicio**, una vez que se hayan creado correctamente los archivos, haga clic en **Aceptar**.

   ![El cuadro de diálogo Service Principal Creation Status (Estado de creación de entidades de servicio)][A06]

1. En la ventana **Inicio de sesión en Microsoft Azure**, haga clic en **Iniciar sesión en**. 

   ![Cuadro de diálogo Inicio de sesión en Microsoft Azure][A07]

1. En el cuadro de diálogo **Seleccionar suscripciones**, elija las suscripciones que desea usar y, luego, haga clic en **Aceptar**.

   ![Cuadro de diálogo Seleccionar suscripciones][A08]

> Una vez creado el archivo de autenticación de la entidad de servicio, puede comenzar en el paso 8, elegir el archivo de autenticación e iniciar sesión.

## <a name="sign-out-of-your-azure-account"></a>Cierre de sesión en la cuenta de Azure

Después de haber configurado la cuenta con los pasos anteriores, iniciará sesión automáticamente cada vez que inicie IntelliJ. Sin embargo, si desea cerrar sesión de su cuenta de Azure, haga lo siguiente.

* En IntelliJ IDEA, abra la barra lateral Explorador de Azure, haga clic en el icono **Cerrar sesión de Azure** y confirme (o en el menú IDEA, **Herramientas/Azure/Cerrar sesión de Azure**).

   ![El comando de cierre de sesión en Azure IntelliJ][L01]

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-intellij-sign-in-instructions/I05.png

[A01]: media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-intellij-sign-in-instructions/A08.png
[A09]: media/azure-toolkit-for-intellij-sign-in-instructions/A09.png

[L01]: media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
