---
title: Introducción a Logz.io para aplicaciones de Java que se ejecutan en Azure
description: En este tutorial se muestra cómo integrar y configurar Logz.io para aplicaciones de Java que se ejecutan en Azure.
author: jdubois
manager: bborges
ms.topic: tutorial
ms.date: 11/05/2019
ms.author: judubois
ms.openlocfilehash: d7f90939701bfbcdcd895b8baf3cabcae4d5efa9
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812450"
---
# <a name="tutorial-getting-started-with-monitoring-and-logging-using-logzio-for-java-apps-running-on-azure"></a>Tutorial: Introducción a la supervisión y el registro con Logz.io para aplicaciones de Java que se ejecutan en Azure

En este tutorial se muestra cómo configurar una aplicación de Java clásica para enviar registros al servicio [Logz.io](https://logz.io/) para su ingesta y análisis. Logz.io proporciona una solución de supervisión completa basada en Elasticsearch, Logstash, Kibana (ELK) y Grafana.

En el tutorial se da por hecho que está usando Log4J o Logback. Estas bibliotecas son las dos más utilizadas para el registro en Java, por lo que el tutorial debería funcionar en la mayoría de las aplicaciones que se ejecutan en Azure. Si ya está usando Elastic Stack para supervisar la aplicación de Java, en este tutorial se muestra cómo modificar la configuración para que el destino sea el punto de conexión de Logz.io.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Enviar registros desde una aplicación de Java existente a Logz.io.
> * Enviar los registros de diagnóstico y las métricas de los servicios de Azure a Logz.io.

## <a name="prerequisites"></a>Requisitos previos

* [Kit para desarrolladores de Java](https://aka.ms/azure-jdks) versión 8 o posteriores
* Una cuenta de Logz.io de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/logz.logzio-elk-as-a-service-pro)
* Una aplicación de Java existente que usa Log4J o Logback

## <a name="send-java-application-logs-to-logzio"></a>Envío de registros de aplicación de Java a Logz.io

En primer lugar, aprenderá a configurar la aplicación de Java con un token que le da acceso a su cuenta de Logz.io.

### <a name="get-your-logzio-access-token"></a>Obtención del token de acceso de Logz.io

Para obtener el token, inicie sesión en su cuenta de Logz.io, seleccione el icono de engranaje en la esquina derecha y, a continuación, seleccione **Settings > General** (Configuración > General). Copie el token de acceso que se muestra en la configuración de la cuenta para poder usarlo más adelante.

### <a name="install-and-configure-the-logzio-library-for-log4j-or-logback"></a>Instalación y configuración de la biblioteca de Logz.io para Log4J o Logback

La biblioteca de Java de Logz.io está disponible en Maven Central, por lo que puede agregarla como una dependencia a la configuración de la aplicación. Compruebe el número de versión en Maven central y use la versión más reciente en las siguientes opciones de configuración.

Si utiliza Maven, agregue la siguiente dependencia al archivo `pom.xml`:

**Log4J:**

```xml
<dependency>
    <groupId>io.logz.log4j2</groupId>
    <artifactId>logzio-log4j2-appender</artifactId>
    <version>1.0.11</version>
</dependency>
```

**Logback:**

```xml
<dependency>
    <groupId>io.logz.logback</groupId>
    <artifactId>logzio-logback-appender</artifactId>
    <version>1.0.22</version>
</dependency>
```

Si utiliza Gradle, agregue la siguiente dependencia al archivo de compilación:

**Log4J:**

```
implementation 'io.logz.log4j:logzio-log4j-appender:1.0.11'
```

**Logback:**

```
implementation 'io.logz.logback:logzio-logback-appender:1.0.22'
```

A continuación, actualice el archivo de configuración de Log4J o Logback:

**Log4J:**

```xml
<Appenders>
    <LogzioAppender name="Logzio">
        <logzioToken><your-logz-io-token></logzioToken>
        <logzioType>java-application</logzioType>
        <logzioUrl>https://<your-logz-io-listener-host>:8071</logzioUrl>
    </LogzioAppender>
</Appenders>

<Loggers>
    <Root level="info">
        <AppenderRef ref="Logzio"/>
    </Root>
</Loggers>
```

**Logback:**

```xml
<configuration>
    <!-- Use shutdownHook so that we can close gracefully and finish the log drain -->
    <shutdownHook class="ch.qos.logback.core.hook.DelayingShutdownHook"/>
    <appender name="LogzioLogbackAppender" class="io.logz.logback.LogzioLogbackAppender">
        <token><your-logz-io-token></token>
        <logzioUrl>https://<your-logz-io-listener-host>:8071</logzioUrl>
        <logzioType>java-application</logzioType>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>INFO</level>
        </filter>
    </appender>

    <root level="debug">
        <appender-ref ref="LogzioLogbackAppender"/>
    </root>
</configuration>
```

Reemplace el marcador de posición `<your-logz-io-token>` por el token de acceso y el marcador de posición `<your-logz-io-listener-host>` por el host del agente de escucha de la región (por ejemplo, listener.logz.io). Para más información sobre cómo encontrar la región de la cuenta, consulte [Región de la cuenta](https://docs.logz.io/user-guide/accounts/account-region.html).

El elemento `logzioType` hace referencia a un campo lógico de Elasticsearch que se usa para separar los distintos documentos entre sí. Es fundamental configurar este parámetro correctamente para sacar el máximo partido de Logz.io.

En Logz.io, "Type" es el formato del registro (por ejemplo: Apache, NGinx, MySQL) y no el origen (por ejemplo: servidor1, servidor2, servidor3). En este tutorial, vamos a llamar al tipo `java-application` porque estamos configurando aplicaciones de Java y esperamos que estas aplicaciones tengan el mismo formato.

Para un uso avanzado, puede agrupar las aplicaciones de Java en tipos diferentes, que tienen su propio formato de registro específico (configurable con Log4J y Logback). Por ejemplo, podría tener un tipo "spring-boot-monolith" y un tipo "spring-boot-microservice".

### <a name="test-your-configuration-and-log-analysis-on-logzio"></a>Prueba de la configuración y el análisis de registros en Logz.io

Una vez configurada la biblioteca Logz.io, la aplicación le enviará registros directamente. Para comprobar que todo funciona correctamente, vaya a la consola de Logz.io, seleccione la pestaña **Live tail** (Cola activa) y, después, seleccione **Run** (Ejecutar). Verá un mensaje similar al siguiente, que indica que la conexión funciona:

```output
Requesting Live Tail access...
Access granted. Opening connection...
Connected. Tailing...
````

A continuación, inicie la aplicación o úsela para generar algunos registros. Los registros aparecerán directamente en la pantalla. Por ejemplo, estos son los primeros mensajes de inicio de una aplicación de Spring Boot:

```output
2019-09-19 12:54:40.685Z Starting JavaApp on javaapp-default-9-5cfcb8797f-dfp46 with PID 1 (/workspace/BOOT-INF/classes started by cnb in /workspace)
2019-09-19 12:54:40.686Z The following profiles are active: prod
2019-09-19 12:54:42.052Z Bootstrapping Spring Data repositories in DEFAULT mode.
2019-09-19 12:54:42.169Z Finished Spring Data repository scanning in 103ms. Found 6 repository interfaces.
2019-09-19 12:54:43.426Z Bean 'spring.task.execution-org.springframework.boot.autoconfigure.task.TaskExecutionProperties' of type [org.springframework.boot.autoconfigure.task.TaskExecutionProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
```

Ahora que Logz.io procesa los registros, puede aprovechar todos los servicios de la plataforma.

## <a name="send-azure-services-data-to-logzio"></a>Envío de datos de los servicios de Azure a Logz.io

A continuación, aprenderá a enviar registros y métricas de sus recursos de Azure a Logz.io.

### <a name="deploy-the-template"></a>Implementación de la plantilla

El primer paso es implementar la plantilla de integración de Azure y Logz.io. La integración se basa en una plantilla de implementación de Azure lista para usar que configura todos los bloques de creación necesarios de la canalización. La plantilla crea un espacio de nombres de Event Hub, un centro de eventos, dos blobs de almacenamiento y todos los permisos y conexiones correctos necesarios. Los recursos configurados por la implementación automatizada pueden recopilar los datos de una sola región de Azure y enviarlos a Logz.io.

Busque el botón **Deploy to Azure** (Implementar en Azure) que se muestra en el [primer paso del archivo Léame del repositorio](https://github.com/logzio/logzio-azure-serverless).

Al seleccionar **Deploy to Azure** (Implementar en Azure), se mostrará la página **Custom Deployment** (Implementación personalizada) en Azure Portal, con una lista de campos rellenados previamente.

Puede dejar la mayoría de los campos tal cual, pero asegúrese de especificar la siguiente configuración:

* **Grupo de recursos**: Seleccione un grupo existente o cree uno nuevo.
* **Logzio Logs/Metrics Host** (Registros de Logzio/Host de métricas): indique la dirección URL del agente de escucha de Logz.io. Si no está seguro de cuál es la dirección URL, compruebe la dirección URL de inicio de sesión. Si es app.logz.io, use listener.logz.io (que es la configuración predeterminada). Si es app-eu.logz.io, use listener-eu.logz.io.
* **Logzio Logs/Metrics Token** (Registros de Logzio/Token de métricas): escriba el token de la cuenta de Logz.io a la que desea enviar los registros o las métricas de Azure. Encontrará este token en la página de la cuenta, en la interfaz de usuario de Logz.io.

Acepte los términos que se encuentra en la parte inferior de la página y seleccione **Purchase** (Comprar). Después, Azure implementará la plantilla, lo que puede tardar uno o dos minutos. Finalmente verá el mensaje "Deployment succeeded" (Implementación se realizó correctamente) en la parte superior del portal.

Puede visitar el grupo de recursos definido para revisar los recursos implementados.

Para más información sobre cómo configurar logzio-azure-serverless para realizar copias de seguridad de los datos en Azure Blob Storage, consulte [Envío de los registros de actividad de Azure](https://docs.logz.io/shipping/log-sources/azure-activity-logs.html).

### <a name="stream-azure-logs-and-metrics-to-logzio"></a>Transmisión de registros y métricas de Azure a Logz.io

Ahora que ha implementado la plantilla de integración, deberá configurar Azure para transmitir los datos de diagnóstico al centro de eventos que acaba de implementar. Cuando los datos entran en el centro de eventos, la aplicación de función los reenvía a Logz.io.

1. En la barra de búsqueda, escriba "diagnóstico" y, a continuación, seleccione **Configuración de diagnóstico**.

2. Elija un recurso de la lista de recursos y, a continuación, seleccione **Agregar configuración de diagnóstico** para abrir el panel **Configuración de diagnóstico** para ese recurso.

    ![Panel Configuración de diagnóstico](media/java-get-started-with-logzio/diagnostics-settings.png)

3. En **Nombre**, asigne un nombre a la configuración de diagnóstico.

4. Seleccione **Trasmitir a un centro de eventos** y, después, seleccione **Configurar** para abrir el panel **Seleccionar centro de eventos**.

5. Elija su centro de eventos:

    * **Seleccionar el espacio de nombres del Centro de eventos**: Elija el espacio de nombres que comienza por **Logzio** (`LogzioNS6nvkqdcci10p`, por ejemplo).
    * **Seleccionar nombre de centro de eventos**: En el caso de los registros, elija **insights-operational-logs** y para las métricas, elija **insights-operational-metrics**.
    * **Seleccionar el nombre de directiva del Centro de eventos**: Elija **LogzioSharedAccessKey**.

6. Seleccione **Aceptar** para volver a la página **Configuración de diagnóstico**.

7. En la sección Registro, seleccione los datos que desea transmitir y, a continuación, seleccione **Guardar**.

Los datos seleccionados se transmitirán ahora al centro de eventos.

### <a name="visualize-your-data"></a>Visualización de los datos

A continuación, dé tiempo para que los datos lleguen desde su sistema a Logz.io y, a continuación, abra Kibana. Verá los datos (con el tipo _eventhub_) llenando los paneles. Para más información sobre cómo crear paneles, consulte [Creación del panel de Kibana perfecto](https://logz.io/blog/perfect-kibana-dashboard/).

Desde allí, puede consultar datos específicos en la pestaña **Detectar** o puede crear objetos Kibana para visualizar los datos en la pestaña **Visualizar**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con los recursos de Azure que creó en este tutorial, puede eliminarlos con el siguiente comando:

```azurecli-interactive
az group delete --name <resource group>
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a configurar la aplicación Java y los servicios de Azure para enviar registros y métricas a Logz.io.

A continuación, obtenga más información sobre el uso del centro de eventos para supervisar la aplicación:

> [!div class="nextstepaction"]
> [Flujo de datos de supervisión de Azure a un centro de eventos para que lo consuma una herramienta externa](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)
