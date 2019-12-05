---
title: 'Tutorial: Creación del servicio de aplicaciones desde Visual Studio Code'
description: Paso 3 del tutorial, creación del servicio de aplicaciones en la extensión para VS Code.
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: b64adc1604698de74f4f318b805dd8c289c8fff8
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466223"
---
# <a name="tutorial-create-the-app-service-from-visual-studio-code"></a>Tutorial: Creación del servicio de aplicaciones desde Visual Studio Code

[Paso anterior: preparación de la aplicación](tutorial-deploy-app-service-on-linux-02.md)

En este paso se creará la instancia de Azure App Service en la que implementará su aplicación.

Realice este paso antes de implementar el código para que pueda configurar un archivo de inicio personalizado si es necesario en el paso siguiente.

1. En el área **Azure: App Service**, seleccione el comando **+** para crear un servicio de aplicaciones nuevo, o abra la paleta de comandos (**F1**) y seleccione **Azure App Service: Crear una nueva aplicación web**. (En la terminología de App Service, una "aplicación web" es un **host** para el código de la aplicación web, no el código de la aplicación en sí).

    ![Crear un nuevo servicio de aplicaciones en el explorador de App Service](media/deploy-azure/create-new-app-service-in-app-service-explorer.png)

1. En las peticiones siguientes:

    - Escriba un nombre para la aplicación, que debe ser único en todo App Service; normalmente, se usa un nombre personal o de empresa seguido del nombre de la aplicación.
    - Seleccione **Python 3.7** como entorno de ejecución.

1. Cuando aparezca un mensaje que indica que se ha creado el nuevo servicio de aplicaciones, seleccione **Ver salida** para cambiar a la ventana **Salida** en VS Code. La salida muestra el nombre del grupo de recursos de Azure y del plan de App Service que se crearon, junto con la dirección URL del servicio de aplicaciones.

    ![Dirección URL, grupo de recursos y plan de App Service para su servicio de aplicaciones](media/deploy-azure/url-for-your-new-app-service-and-resource-group-and-plan.png)

1. Para confirmar que el servicio de aplicaciones se está ejecutando correctamente, expanda su suscripción en el explorador de **Azure: App Service**, haga clic con el botón derecho en el nombre del servicio de aplicaciones y seleccione **Examinar sitio web**:

    ![Comando Examinar sitio web en un servicio de aplicaciones en el explorador de App Service](media/deploy-azure/select-command-to-browse-website-in-app-service.png)

1. Como aún no ha implementado su propio código en el servicio de aplicaciones (se hace en el paso siguiente), solo aparece una aplicación predeterminada:

    ![Aplicación de Python predeterminada en App Service en Linux](media/deploy-azure/default-python-app-on-app-service-on-linux.png)

## <a name="optional-upload-an-environment-variable-definitions-file"></a>(Opcional) Carga de un archivo de definiciones de variables de entorno

Si tiene un archivo de definiciones de variables de entorno, puede usarlo para configurar también el entorno de App Service. (Para más información sobre estos archivos, que normalmente tienen la extensión *.env*, consulte [Entornos de Visual Studio Code: Python](https://code.visualstudio.com/docs/python/environments#environment-variable-definitions-file)).

1. En el área **Azure: App Service**, expanda el nodo del servicio de aplicaciones deseado, haga clic con el botón derecho en el nodo **Configuración de la aplicación** y seleccione **Cargar configuración local**.

1. VS Code solicita la ubicación del archivo *.env* y, a continuación, lo carga en el servicio de aplicaciones.

1. Una vez completada la carga, puede expandir el nodo **Configuración de la aplicación** para ver los valores individuales. También puede verlos en Azure Portal; para ello, vaya a hasta el servicio de aplicaciones y seleccione **Configuración**.

1. Si crea una configuración directamente en Azure Portal, puede guardarla en un archivo de definiciones; para ello, haga clic con el botón derecho en el nodo **Configuración de la aplicación** y seleccione **Descargar configuración remota**. Este proceso garantiza que la configuración se encuentra en el repositorio y no solo en el portal.

> [!div class="nextstepaction"]
> [He creado el servicio de aplicaciones](tutorial-deploy-app-service-on-linux-04.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=03-create-app-service)
