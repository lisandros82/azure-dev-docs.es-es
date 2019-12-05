---
title: Revisión de los datos con Java Flight Recorder y Mission Control
description: Guía para el uso de Java Flight Recorder y Mission Control para recopilar y revisar los datos de la aplicación.
ms.date: 04/09/2019
ms.topic: conceptual
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 14084d2fb89c8adc78a0455b8bdd2cc0b5b6ec4c
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812253"
---
# <a name="monitor-and-manage-java-workloads-with-java-flight-recorder-jfr-and-zulu-mission-control"></a>Supervisión y administración de cargas de trabajo de Java con Java Flight Recorder (JFR) y Zulú Mission Control

Estos artículos muestran cómo supervisar y administrar cargas de trabajo de Java con Java Flight Recorder (JFR) y Zulu Mission Control.

Zulu Mission Control es una compilación completamente probada de JDK Mission Control, liberado como código abierto por Oracle en 2018 y que se administra como un proyecto bajo el paraguas de OpenJDK. Junto con Flight Recorder, Mission Control ofrece supervisión interactiva de baja sobrecarga y funcionalidades de administración para las cargas de trabajo de Java.

Zulu Mission Control es compatible con los siguientes JDK y JRE:

* Zulu 12.1 y versiones posteriores
* Zulu 11.0 y versiones posteriores
* Zulu 8u202 (8.36) y versiones posteriores
* Oracle OpenJDK 11+15 y versiones posteriores
* Oracle Java 11.0 y versiones posteriores
* Oracle Java 8.0 y versiones posteriores

Siga los pasos que aparecen a continuación para instalar Zulu Mission Control, conectarse a una máquina Virtual Java (JVM) y ganar visibilidad en tiempo real en todos los aspectos de una aplicación en ejecución.

1.  [Instale un JDK/JRE compatible con Zulu Mission Control](java-jdk-install.md).

2.  Descargue Zulu Mission Control desde [el sitio de descarga de Azul](https://www.azul.com/products/zulu-mission-control/), elija la versión adecuada para su sistema, guárdelo localmente y cambie a ese directorio.

3.  Expanda el archivo descargado.

    **Linux:**

    ```cli
    tar -xzvf zmc7.0.0-EA-linux_x64.tar.gz
    ```

    **Windows:**

    ```cli
    unzip -zxvf zmc7.0.0-EA-win_x64.zip 
    ```

    **MacOS:**

    ```cli
    tar -xzvf zmc7.0.0-EA-macosx_x64.tar.gz
    ```

4.  Inicie la aplicación de Java mediante uno de los JDK compatibles. Por ejemplo:

    ```cli
    $JAVA_HOME/bin/java -jar MyApplication.jar
    ```

5.  Inicio de Zulu Mission Control

    **Linux:**

    ```cli
    zmc7.0.0-EA-linux_x64/zmc
    ```

    **Windows:**

    ```cli
    zmc7.0.0-EA-win_x64\zmc.exe 
    ```

    **MacOS:**

    ```cli
    zmc7.0.0-EA-macosx_x64/Zulu\ Mission\ Control.app/Contents/MacOS/zmc
    ```

6.  Cambio de la instalación de JVM para Mission Control (opcional)

    En Windows, *zmc.exe* usará la instalación de JVM predeterminada configurada en el registro. Zulu Mission Control se debe iniciar desde un JDK completo para poder detectar automáticamente las instancias locales de JVM. Si se trata de una versión de JRE, verá la siguiente advertencia:

    > [!div class="mx-imgBorder"]
    ![Advertencia si la instalación JDK es solo JRE](../media/jdk/jfr-jre-warning-message.png)

    Para cambiar la versión de JVM utilizada por Mission Control, siga estos pasos: 
    1.  Abra el archivo de configuración *zmc.ini*, ubicado en el mismo directorio que el archivo *zmc.exe*
    2.  Antes de la línea `-vmargs`, agregue dos líneas:
        * En la primera línea, escriba `–vm`
        * En la segunda línea, escriba la ruta de acceso a la instalación de JDK. (Por ejemplo, `C:\Program Files\Java\jdk1.8.0_212\bin\javaw.exe`).

7.  Búsqueda de la versión de JVM que ejecuta la aplicación
    1.  En el panel superior izquierdo de la ventana de Zulu Mission Control, seleccione la pestaña **JVM Browser** (Explorador de JVM).
    2.  Seleccione y expanda el elemento de la lista de la parte superior izquierda de la instancia de JVM que ejecuta la aplicación.

    > [!div class="mx-imgBorder"]
    ![Expansión del elemento de la lista de la parte superior izquierda de la instancia de JVM](../media/jdk/jfr-jvm-instance-dashboard.png)


8.  Inicie una operación Flight Recording, si es necesario
    1.  Si Flight Recorder muestra "No recordings" (No hay grabaciones), inicie una haciendo clic con el botón derecho en la línea de Flight Recorder, en la pestaña del explorador de JVM, y seleccione **Start Flight Recording** (Iniciar grabación del vuelo).
    2.  Seleccione una grabación de duración fija o una grabación continua y una configuración de generación de perfiles (específica) o una configuración continua (con menor sobrecarga) y, a continuación, seleccione **Finish** (Finalizar).

    > [!div class="mx-imgBorder"]
    ![Inicio de una operación Flight Recording](../media/jdk/jfr-start-flight-recording.png)

9.  Volcado de la operación Flight Recording
    1.  Debería aparecer un elemento Flight Recording debajo de la línea de Flight Recorder en el explorador de JVM. Haga clic con el botón derecho en la línea que representa la operación Flight Recording y seleccione **Dump whole recording** (Volcar la grabación completa).
    2.  Aparecerá una nueva pestaña en el panel grande del lado derecho de la ventana de Zulu Mission Control. Este panel representa la grabación de Flight Recording recién volcada desde la instancia de JVM que ejecuta la aplicación.

10. Examen de la grabación de Flight Recording con Zulu Mission Control
    1.  Si aún no está activada, seleccione la pestaña etiquetada **Outline** (Esquema) en el panel izquierdo de la ventana de Zulu Mission Control. Esta pestaña contiene vistas diferentes de los datos recopilados en la operación Flight Recording.
 
    > [!div class="mx-imgBorder"]
    ![Revisión del registro del vuelo](../media/jdk/jfr-zulu-mission-control-data.png)

## <a name="resources"></a>Recursos

También hemos preparado un [vídeo de demostración](https://www.azul.com/presentation/azul-webinar-open-source-flight-recorder-and-mission-control-managing-and-measuring-openjdk-8-performance/) con comentarios de Simon Ritter, CTO de Azul Systems. El vídeo le guiará en la instalación y configuración de Flight Recorder y Zulu Mission Control. El análisis de Flight Recorder comienza en el minuto 31:30.

