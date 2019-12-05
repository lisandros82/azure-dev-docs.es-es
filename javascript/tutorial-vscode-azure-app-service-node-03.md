---
title: Implementación de aplicaciones de Node.js en Azure App Service desde Visual Studio Code
description: Tutorial, parte 3, implementación del sitio web
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 937eb54e9885e3b5b9fa7be54f551945a54572cd
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467202"
---
# <a name="deploy-the-website"></a>Implementación del sitio web

[Paso anterior: Creación de la aplicación](tutorial-vscode-azure-app-service-node-02.md)

En este paso, implementará un sitio web de Node.js con Visual Studio Code y la extensión de Azure App Service. En este tutorial se usa el modelo de implementación más básico, donde la aplicación se comprime e implementa en Azure App Service en Linux.

1. Antes de implementar la aplicación, asegúrese de que está escuchando en el puerto proporcionado por la variable de entorno `PORT`: `process.env.PORT`.

1. En la carpeta de la aplicación (como `myExpressApp` en el paso anterior), inicie VS Code en esa carpeta con el siguiente comando:

    ```bash
    code .
    ```

1. En VS Code, seleccione el icono de Azure para abrir el explorador de **Azure App Service** y, luego, seleccione el icono de flecha azul hacia arriba para implementar la aplicación en Azure:

    ![Implementación en Web App](media/deploy-azure/deploy.png)

    > [!TIP]
    > Como alternativa, abra la **paleta de comandos** (**F1**), escriba "deploy to web app" (implementar en la aplicación web) y seleccione el comando **Azure App Service: implementar en Web App**.

1. En los indicadores, escriba la siguiente información:

    a. Seleccione la carpeta actual de la aplicación (`myExpressApp` en este tutorial).

    a. Seleccione **Create new Web App** (Crear nueva aplicación web).

    a. Escriba un nombre único global para la aplicación y presione **Entrar**. Los caracteres válidos para un nombre de aplicación son “a-z”, “0-9” y “-”.

    a. Elija una ubicación en una región de Azure (como se explica en [Regiones](https://azure.microsoft.com/regions/)) que esté cerca de usted o de otros servicios a los que tenga que acceder.

    a. Seleccione su **versión de Node.js** (le recomendamos que use LTS).

1. A medida que la extensión crea la aplicación, el progreso aparece en la ventana **Salida** en VS Code (puede que tenga que seleccionar la salida **Azure App Service**).

    ![Ventana de salida de Visual Studio Code para Azure App Service](media/deploy-azure/output-window.png)

1. Seleccione **Sí** cuando se le pida que actualice la configuración para ejecutar `npm install` en el servidor de destino. Después, se implementará la aplicación.

    ![Implementación configurada](media/deploy-azure/server-build.png)

1. Cuando se inicie la implementación, se le pedirá que actualice el área de trabajo para que las implementaciones posteriores usen como destino automáticamente la misma aplicación web de App Service. Seleccione **Sí** para asegurarse de que los cambios se implementen en la aplicación correcta.

    ![Implementación configurada](media/deploy-azure/save-configuration.png)

1. Una vez finalizada la implementación, seleccione **Browse Website** (Examinar sitio web) en el mensaje para ver el sitio web recién implementado.

## <a name="troubleshooting"></a>solución de problemas

Si ve el error "You do not have permission to view this directory or page" (No tiene permiso para ver este directorio o esta página), puede que la aplicación no se haya iniciado correctamente. La visualización de los registros, como se describe en el [paso siguiente](tutorial-vscode-azure-app-service-node-04.md) puede ayudarle a diagnosticar y corregir el problema. Si no puede corregirlo, haga clic en el botón siguiente **Tengo un problema** para ponerse en contacto con nosotros. Estaremos encantados de ayudarle.

## <a name="updating-the-website"></a>Actualización del sitio web

Para implementar los cambios en esta aplicación, siga el mismo proceso y seleccione la aplicación existente, en lugar de crear otra.

----

> [!div class="nextstepaction"]
> [Mi sitio se encuentra en Azure](tutorial-vscode-azure-app-service-node-04.md) [He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=deploy-app)
