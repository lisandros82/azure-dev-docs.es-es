---
title: 'Tutorial: Configuración de un archivo de inicio personalizado para aplicaciones de Python en Azure App Service en Linux'
description: Paso 4 del tutorial, indicación a App Service de cómo iniciar la aplicación web.
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 7c3c863ed333528c675cda939f52b86f53bc8380
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278951"
---
# <a name="tutorial-configure-a-custom-startup-file-for-python-apps-on-azure-app-service"></a>Tutorial: Configuración de un archivo de inicio personalizado para aplicaciones de Python en Azure App Service

[Paso anterior: creación del servicio de aplicaciones](tutorial-deploy-app-service-on-linux-03.md)

En este artículo se muestra que tiene que configurar un archivo de inicio personalizado para una aplicación de Python en un servicio de aplicaciones de Azure.

En función de la estructura de la aplicación, puede que tenga que crear un archivo de comandos de inicio personalizado para la aplicación, tal y como se describe en [Configuración de aplicaciones de Python para App Service en Linux](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python), en la documentación de Azure.

Los casos de uso específicos de un comando de inicio personalizado son los siguientes:

- Tiene una aplicación de **Flask** cuyo archivo de inicio y objeto de aplicación tienen un nombre **distinto** de *application.py* y `app`, respectivamente. En otras palabras, a menos que tenga un archivo *application.py* en la carpeta raíz del proyecto *y* el objeto de aplicación de Flask se llame `app`, necesitará un comando de inicio personalizado.
- Inicie el servidor web Gunicorn con otros argumentos adicionales aparte de los valores predeterminados, que son `--bind=0.0.0.0 --timeout 600`.

## <a name="create-a-startup-file"></a>Creación de un archivo de inicio

Si necesita un archivo de inicio personalizado, siga estos pasos:

1. Cree un archivo en el proyecto llamado *startup.txt* (u otro nombre de su elección) que contenga el comando de inicio. Para Flask, vea [Comandos de inicio de Flask](#flask-startup-commands) en la sección siguiente. Las aplicaciones de Django normalmente no necesitan personalización.

1. Confirme el archivo en el repositorio de código para que se pueda implementar con el resto de la aplicación.

1. En el área **Azure: App Service**, expanda el servicio de aplicaciones, haga clic con el botón derecho en **Configuración de la aplicación** y seleccione **Abrir en el portal**:

    ![Abrir la configuración en el portal, en el explorador de App Service](media/deploy-azure/open-application-settings-in-portal-for-app-service.png)

1. En Azure Portal, inicie sesión si es necesario; después, en la página **Configuración**, seleccione **Configuración general**, escriba el nombre del archivo de inicio (como *startup.txt*) en **Configuración de la pila** > **Comando de inicio** y seleccione **Guardar**.

    ![Establecimiento del nombre del archivo de comando de inicio en Azure Portal](media/deploy-azure/enter-startup-file-for-app-service-in-the-azure-portal.png)

    > [!NOTE]
    > En lugar de usar un archivo de comandos de inicio, también puede colocar el comando de inicio directamente en el campo **Comando de inicio** en Azure Portal. Sin embargo, es preferible usar un archivo porque mantiene esa configuración en el repositorio, donde puede auditar los cambios y volver a implementar en una instancia de App Service diferente.

1. El servicio de aplicaciones se reinicia al guardar los cambios. No obstante, como aún no ha implementado el código de la aplicación, al visitar el sitio en este momento se muestra el mensaje "Error de aplicación". Este mensaje indica que el servidor de Gunicorn se inició pero no encontró la aplicación y, por tanto, no hay nada que responda a las solicitudes HTTP. El código de la aplicación se implementa en el siguiente paso.

## <a name="django-startup-commands"></a>Comandos de inicio de Django

De forma predeterminada, App Service busca automáticamente la carpeta que contiene el archivo *wsgi.py* e inicia Gunicorn con el siguiente comando:

```bash
# <module> is the path to the folder that contains wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Si desea cambiar cualquiera de los argumentos de Gunicorn, como el uso de `--timeout 1200`, cree un archivo de comandos con esas modificaciones.

## <a name="flask-startup-commands"></a>Comandos de inicio de Flask

De forma predeterminada, el contenedor de App Service en Linux da por hecho que el archivo de inicio de una aplicación de Flask se llama *application.py* y se encuentra en la carpeta raíz de la aplicación. Además, da por hecho que el objeto de aplicación de Flask definido en ese archivo se llama `app`. Si la aplicación no tiene esta estructura exacta, el comando de inicio personalizado debe identificar la ubicación del objeto de aplicación:

1. **Nombre de archivo o nombre de objeto de aplicación diferentes**: por ejemplo, si el archivo de inicio de la aplicación es *hello.py* y el objeto de aplicación se llama `myapp`, el comando de inicio es el siguiente:

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
    ```

1. **El archivo de inicio está en una subcarpeta**: por ejemplo, si el archivo de inicio es *myapp/website.py* y el objeto de aplicación es `app`, use el argumento `--chdir` de Gunicorn para especificar la carpeta y, a continuación, asigne un nombre al archivo de inicio y al objeto de aplicación como de costumbre:

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 --chdir myapp website:app
    ```

1. **El archivo de inicio está dentro de un módulo**: en el código [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial), el archivo de inicio *webapp.py* se encuentra en la carpeta *hello_app*, que es un módulo con un archivo *\_\_init\_\_.py*. El objeto de aplicación se llama `app` y se define en *\_\_init\_\_.py*, y *webapp.py* usa una importación relativa. Debido a esta organización, cuando Gunicorn apunta a `webapp:app`, se genera el error "Attempted relative import in non-package" (Intento de importación relativa en no paquete) y la aplicación no se inicia.

    En esta situación, cree un archivo de corrección de compatibilidad simple que importe el objeto de aplicación desde el módulo y, a continuación, haga que Gunicorn inicie la aplicación con la corrección de compatibilidad. El código [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial), por ejemplo, incluye *startup.py* con el siguiente contenido:

    ```python
    # startup.py shim
    from hello_app.webapp import app
    ```

    El comando de inicio es el siguiente:

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 startup:app
    ```

> [!div class="nextstepaction"]
> [He configurado mi archivo de inicio](tutorial-deploy-app-service-on-linux-05.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=04-startup-command)
