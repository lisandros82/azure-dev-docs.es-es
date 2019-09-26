---
title: Preparación de una aplicación para su implementación en Azure App Service en Linux desde Visual Studio Code
description: Paso 2 del tutorial, configuración de la aplicación
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: ab1609d6d0940172d61a61a31f4dbfabc868c023
ms.sourcegitcommit: d6575ac86449380b5a9c6c66aa722cb33ed53438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186130"
---
# <a name="prepare-your-app"></a>Preparación de la aplicación

[Paso anterior: requisitos previos](tutorial-deploy-app-service-on-linux-01.md)

Si ya tiene una aplicación con la que le gustaría trabajar, asegúrese de que tiene un archivo *requirements.txt* que describe las dependencias, incluidas las plataformas como Flask o Django.

Si aún no tiene una aplicación, use una de las opciones siguientes. Asegúrese de comprobar que la aplicación se ejecuta localmente.

## <a name="minimal-flask-app"></a>Aplicación Flask mínima

En esta sección se describe la aplicación de Flask mínima que se usa en este tutorial.

1. Cree una nueva carpeta, ábrala en VS Code y agregue un archivo llamado *Hello.py* con el contenido siguiente. El objeto de aplicación se llama `myapp` de forma intencionada para demostrar cómo se usan los nombres en el comando de inicio de App Service, como veremos más adelante.

    ```python
    from flask import Flask
    myapp = Flask(__name__)

    @myapp.route("/")
    def hello():
        return "Hello Flask, on Azure App Service for Linux"
    ```

1. Cree un archivo llamado *requirements.txt* con el siguiente contenido:

    ```text
    Flask==1.1.1
    ```

1. Siga las instrucciones del [tutorial de Flask: creación de un entorno de proyecto para Flask](https://code.visualstudio.com/docs/python/tutorial-flask#create-a-project-environment-for-flask) con el fin de crear un entorno virtual con Flask instalado, en el que puede ejecutar la aplicación localmente.

1. Para ejecutar esta aplicación, use los siguientes comandos (según el sistema operativo). La variable de entorno FLASK_APP indica a Flask dónde encontrar el objeto de aplicación.

    ```ps
    set FLASK_APP=hello:myapp
    flask run
    ```

    ```bash
    export FLASK_APP=hello:myapp
    flask run
    ```

    Después, puede abrir la aplicación en un explorador con la dirección URL `http://127.0.0.1:5000/`.

## <a name="vs-code-flask-tutorial-sample"></a>Ejemplo de tutorial de Flask para VS Code

Descargue o clone [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial), que es el resultado de seguir el [tutorial de Flask](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="vs-code-django-tutorial-sample"></a>Ejemplo de tutorial de Django para VS Code

Descargue o clone [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial), que es el resultado de seguir el [tutorial de Django](https://code.visualstudio.com/docs/python/tutorial-django).

Si su aplicación de Django usa una base de datos SQLite local como la del ejemplo, debe incluir una copia preinicializada y previamente rellenada del archivo *db.sqlite3* en el repositorio. La razón es que, en la actualidad, App Service para Linux no tiene un medio para ejecutar el comando `migrate` de Django como parte de la implementación, por lo que debe implementar una base de datos predefinida. Incluso así, la base de datos es realmente de solo lectura, por lo que escribir en la base de datos también provoca errores.

La mejor opción en cualquier caso es usar una base de datos distinta que se implemente e inicialice de forma independiente del código de la aplicación.

> [!div class="nextstepaction"]
> [Mi aplicación está lista](tutorial-deploy-app-service-on-linux-03.md)

[He tenido un problema](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=02-prepare-app)
