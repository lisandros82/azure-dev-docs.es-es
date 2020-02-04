---
title: Migración de aplicaciones de Tomcat a Tomcat en Azure App Service
description: En esta guía se describe lo que hay que tener en cuenta para migrar una aplicación de Tomcat existente para que se ejecute en Azure App Service mediante Tomcat.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: f9611415264ce0c00a077d8988ef0fc9f7d97f66
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825904"
---
# <a name="migrate-tomcat-applications-to-tomcat-on-azure-app-service"></a>Migración de aplicaciones de Tomcat a Tomcat en Azure App Service

En esta guía se describe lo que hay que tener en cuenta para migrar una aplicación de Tomcat existente para que se ejecute en Azure App Service mediante Tomcat 8.5 o 9.0.

## <a name="before-you-start"></a>Antes de comenzar

Si no puede cumplir alguno de los requisitos previos a la migración, consulte las siguientes guías de migración complementarias:

* [Migración de aplicaciones de Tomcat a contenedores en Azure Kubernetes Service](migrate-tomcat-to-containers-on-azure-kubernetes-service.md)
* Migración de aplicaciones de Tomcat a Azure Virtual Machines (planeada)

## <a name="pre-migration-steps"></a>Pasos previos a la migración

### <a name="switch-to-a-supported-platform"></a>Cambio a una plataforma compatible

App Service ofrece versiones específicas de Tomcat en versiones específicas de Java. Para garantizar la compatibilidad, migre la aplicación a una de las versiones de Tomcat y Java admitidas en su entorno actual antes de continuar con los pasos restantes. Asegúrese de probar por completo la configuración resultante. Use la versión estable más reciente de la distribución de Linux en dichas pruebas.

[!INCLUDE [note-obtain-your-current-java-version](includes/migration/note-obtain-your-current-java-version.md)]

Para obtener la versión actual de Tomcat, inicie sesión en el servidor de producción y ejecute el siguiente comando:

```bash
${CATALINA_HOME}/bin/version.sh
```

Para obtener la versión actual que usa Azure App Service, descargue [Tomcat 8.5](https://tomcat.apache.org/download-80.cgi#8.5.50) o [Tomcat 9](https://tomcat.apache.org/download-90.cgi), en función de la versión que vaya a usar en Azure App Service.

[!INCLUDE [inventory-external-resources](includes/migration/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/migration/inventory-secrets.md)]

[!INCLUDE [inventory-certificates](includes/migration/inventory-certificates.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

<!-- App-Service-specific addendum to inventory-persistence-usage -->
#### <a name="dynamic-or-internal-content"></a>Contenido dinámico o interno

En el caso de los archivos que la aplicación lee y escribe con frecuencia (por ejemplo, los archivos de datos temporales) o los archivos estáticos que solo son visibles para la aplicación, puede montar recursos compartidos de Azure Storage en el sistema de archivos de App Service. Para obtener más información consulte [Servicio de contenido desde Azure Storage en App Service en Linux](/azure/app-service/containers/how-to-serve-content-from-azure-storage).

### <a name="identify-session-persistence-mechanism"></a>Identificación del mecanismo de persistencia de sesión

Para identificar el administrador de persistencia de sesión que se está usando, inspeccione los archivos *context.xml* de la aplicación y la configuración de Tomcat. Busque el elemento `<Manager>` y, a continuación, anote el valor del atributo `className`.

Las implementaciones integradas de [PersistentManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html) de Tomcat, como [StandardManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Standard_Implementation) o [FileStore](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Nested_Components) no están diseñadas para usarse con una plataforma distribuida y escalada como App Service. Como App Service puede equilibrar la carga entre varias instancias y reiniciar de forma transparente cualquier instancia en cualquier momento, por lo que no se recomienda conservar el estado mutable en un sistema de archivos.

Si se necesita la persistencia de la sesión, deberá usar una implementación de `PersistentManager` alternativa que escribirá en un almacén de datos externo, como Pivotal Session Manager con Redis Cache. Para más información, consulte [Uso de Redis como caché de sesión con Tomcat](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat).

### <a name="special-cases"></a>Casos especiales

Algunos escenarios de producción pueden requerir otros cambios o imponer limitaciones adicionales. Aunque estos escenarios no son frecuentes, es importante asegurarse de que no son aplicables a la aplicación o que se han resuelto correctamente.

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>Determinación de si la aplicación se basa en trabajos programados

Los trabajos programados, como las tareas del programador de Quartz o los trabajos de cron, no se pueden usar con App Service. App Service no le impedirá implementar internamente una aplicación que contenga tareas programadas. Sin embargo, si la aplicación se escala horizontalmente, un mismo trabajo programado se podría ejecutar más de una vez por cada período programado. Esta situación puede tener consecuencias no deseadas.

Haga un inventario de todos los trabajos programados, dentro o fuera del servidor de aplicaciones.

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>Determinación de si la aplicación contiene código específico del sistema operativo

Si la aplicación contiene código con dependencias en el sistema operativo host, deberá refactorizarla para quitar esas dependencias. Por ejemplo, puede que necesite reemplazar el uso de `/` o `\` en las rutas de acceso del sistema de archivos por [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) o [`Paths.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-).

#### <a name="determine-whether-tomcat-clustering-is-used"></a>Determinación de si se utiliza la agrupación en clústeres de Tomcat

Azure App Service no admite la [agrupación en clústeres de Tomcat](https://tomcat.apache.org/tomcat-8.5-doc/cluster-howto.html). En su lugar, puede configurar y administrar el equilibrio de carga y el escalado mediante Azure App Service sin la funcionalidad específica de Tomcat. Puede conservar el estado de la sesión en una ubicación alternativa para que esté disponible en todas las réplicas. Para más información, consulte [Identificación del mecanismo de persistencia de sesión](#identify-session-persistence-mechanism).

Para determinar si la aplicación usa la agrupación en clústeres, busque el elemento `<Cluster>` dentro de los elementos `<Host>` o `<Engine>` del archivo *server.xml*.

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>Identificación de todos los procesos externos o demonios en ejecución en los servidores de producción

Tendrá que migrar a otra parte o eliminar los procesos que se ejecuten fuera del servidor de aplicaciones, como los demonios de supervisión.

#### <a name="determine-whether-non-http-connectors-are-used"></a>Determinación de si se usan conectores que no son HTTP

App Service solo admite un conector HTTP. Si la aplicación requiere otros conectores, como el conector de AJP, no utilice App Service.

Para identificar los conectores HTTP que usa la aplicación, busque elementos `<Connector>` dentro del archivo *server.xml* en la configuración de Tomcat.

#### <a name="determine-whether-memoryrealm-is-used"></a>Determinación de si se usa MemoryRealm

[MemoryRealm](https://tomcat.apache.org/tomcat-8.5-doc/api/org/apache/catalina/realm/MemoryRealm.html) requiere un archivo XML persistente. En Azure AppService, tendrá que cargar este archivo en el directorio */home*, o en uno de sus subdirectorios, o en el almacenamiento montado. Tendrá que modificar el parámetro `pathName` como corresponda.

Para determinar si `MemoryRealm` se usa actualmente, inspeccione los archivos *server.xml* y *context.xml* y busque elementos `<Realm>` en los que el atributo `className` esté establecido en `org.apache.catalina.realm.MemoryRealm`.

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>Determinación de si se utiliza el seguimiento de sesión SSL

App Service realiza la descarga de la sesión fuera del tiempo de ejecución de Tomcat. Por lo tanto, no se puede usar el [seguimiento de sesión SSL](https://tomcat.apache.org/tomcat-8.5-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL). En su lugar, use otro modo de seguimiento de sesión (`COOKIE` o `URL`). Si necesita el seguimiento de sesión SSL, no utilice App Service.

#### <a name="determine-whether-accesslogvalve-is-used"></a>Determinación de si se usa AccessLogValve

Si usa [AccessLogValve](https://tomcat.apache.org/tomcat-8.5-doc/api/org/apache/catalina/valves/AccessLogValve.html), debe establecer el parámetro `directory` en `/home/LogFiles` o en uno de sus subdirectorios.

## <a name="migration"></a>Migración

### <a name="parameterize-the-configuration"></a>Parametrización de la configuración

En la migración previa, es probable que haya identificado secretos y dependencias externas, como orígenes de bits, en los archivos *server.xml* y *context.xml*. En cada elemento identificado, reemplace los nombres de usuario, contraseñas, cadenas de conexión o direcciones URL por una variable de entorno.

Por ejemplo, supongamos que el archivo *context.xml* contiene el siguiente elemento:

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="jdbc:postgresql://postgresdb.contoso.com/wickedsecret?ssl=true"
    driverClassName="org.postgresql.Driver"
    username="postgres"
    password="t00secure2gue$$"
/>
```

En este caso, podría cambiarlo como se muestra en el ejemplo siguiente:

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="${postgresdb.connectionString}"
    driverClassName="org.postgresql.Driver"
    username="${postgresdb.username}"
    password="${postgresdb.password}"
/>
```

### <a name="provision-an-app-service-plan"></a>Aprovisionamiento de un plan de App Service

En la lista de planes de servicio disponibles en [Precios de App Service](https://azure.microsoft.com/pricing/details/app-service/linux/), seleccione el plan cuyas especificaciones sean igual o superiores a las del hardware de producción actual.

> [!NOTE]
> Si planea ejecutar implementaciones de ensayo o controladas, o usar espacios de implementación, el plan de App Service debe incluir esa funcionalidad adicional. Se recomienda usar planes Premium o superiores para las aplicaciones de Java. Para más información, consulte [Configuración de entornos de ensayo en Azure App Service](/azure/app-service/deploy-staging-slots).

Después, cree un plan de App Service. Para más información, consulte [Cambio de una aplicación a otro plan de App Service](/azure/app-service/app-service-plan-manage).

### <a name="create-and-deploy-web-apps"></a>Creación e implementación de aplicaciones web

Tendrá que crear una aplicación web en su plan de App Service (con una versión de Tomcat como pila en tiempo de ejecución) para cada archivo WAR implementado en el servidor de Tomcat.

> [!NOTE]
> Aunque es posible implementar varios archivos WAR en una sola aplicación web, es muy poco recomendable. La implementación de varios archivos WAR en una sola aplicación web impide que cada aplicación escale según sus propias demandas de uso. También agrega complejidad a las posteriores canalizaciones de implementación. Si tiene que haber varias aplicaciones disponibles en una sola dirección URL, considere la posibilidad de usar una solución de enrutamiento como [Azure Application Gateway](/azure/application-gateway/).

#### <a name="maven-applications"></a>Aplicaciones de Maven

Si la aplicación se ha creado a partir de un archivo POM de Maven, [usaeel complemento Webapp para Maven](/azure/app-service/containers/quickstart-java#configure-the-maven-plugin) para crear la aplicación web e implementarla.

#### <a name="non-maven-applications"></a>Aplicaciones que no son de Maven

Si no puede usar el complemento de Maven, debe aprovisionar la aplicación web con otros mecanismos, como:

* [Azure Portal](https://portal.azure.com/#create/Microsoft.WebSite)
* [CLI de Azure](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

Una vez creada la aplicación web, use uno de los mecanismos de implementación [disponibles](/azure/app-service/deploy-zip) para implementar la aplicación.

### <a name="migrate-jvm-runtime-options"></a>Migración de las opciones de tiempo de ejecución de JVM

Si la aplicación requiere opciones de tiempo de ejecución específicas, [use el mecanismo más adecuado para especificarlas](/azure/app-service/containers/configure-language-java#set-java-runtime-options).

### <a name="populate-secrets"></a>Relleno de secretos

Use la configuración de la aplicación para almacenar los secretos específicos de la aplicación. Si piensa utilizar los mismos secretos en varias aplicaciones o requiere funcionalidades de auditoría y directivas de acceso específicas, [use Azure Key Vault](/azure/app-service/containers/configure-language-java#use-keyvault-references) en su lugar.

[!INCLUDE [configure-custom-domain-and-ssl](includes/migration/configure-custom-domain-and-ssl.md)]

[!INCLUDE [import-backend-certificates](includes/migration/import-backend-certificates.md)]

### <a name="migrate-data-sources-libraries-and-jndi-resources"></a>Migración de orígenes de datos, bibliotecas y recursos de JNDI

Siga [estos pasos para migrar orígenes de datos](/azure/app-service/containers/configure-language-java#tomcat).

Migre las demás dependencias de CLASSPATH en el nivel de servidor siguiendo [los mismos pasos que para los archivos JAR de origen de datos](/azure/app-service/containers/configure-language-java#finalize-configuration).

Migre otros [recursos de JDNI adicionales compartidos en el nivel de servidor](/azure/app-service/containers/configure-language-java#shared-server-level-resources).

> [!NOTE]
> Si está siguiendo la arquitectura recomendada de un archivo WAR por aplicación web, considere la posibilidad de migrar las bibliotecas de CLASSPATH en el nivel de servidor y los recursos de JNDI a su aplicación. Esto simplificará considerablemente la administración de cambios y la gobernanza de los componentes.

### <a name="migrate-remaining-configuration"></a>Migración de la configuración restante

Después de completar la sección anterior, tendrá la configuración de servidor personalizable en */home/tomcat/conf*.

Para completar la migración, copie otra configuración adicional (por ejemplo, [realms](https://tomcat.apache.org/tomcat-8.5-doc/config/realm.html) o [JASPIC](https://tomcat.apache.org/tomcat-8.5-doc/config/jaspic.html)).

[!INCLUDE [migrate-scheduled-jobs](includes/migration/migrate-scheduled-jobs.md)]

### <a name="restart-and-smoke-test"></a>Reinicio y prueba de humo

Por último, tendrá que reiniciar la aplicación web para aplicar todos los cambios de la configuración. Tras completar el reinicio, compruebe que la aplicación se está ejecutando correctamente.

## <a name="post-migration-steps"></a>Pasos posteriores a la migración

Ahora que ha migrado la aplicación a Azure App Service, debe comprobar que funciona como se espera. Una vez hecho esto, tenemos algunas recomendaciones que pueden hacer que su aplicación sea más nativa de la nube.

### <a name="recommendations"></a>Recomendaciones

1. Si ha optado por usar el directorio */home* para el almacenamiento de archivos, considere la posibilidad de [reemplazarlo por Azure Storage](/azure/app-service/containers/how-to-serve-content-from-azure-storage).

1. Si tiene una configuración en el directorio */home* que contiene cadenas de conexión, claves SSL y otra información secreta, considere la posibilidad de usar una combinación de [Azure Key Vault](/azure/app-service/app-service-key-vault-references) e [inyección de parámetros con la configuración de la aplicación](/azure/app-service/configure-common#configure-app-settings) siempre que sea posible.

1. Considere la posibilidad de [usar espacios de implementación](/azure/app-service/deploy-staging-slots) para realizar implementaciones confiables sin tiempo de inactividad.

1. Diseñe e implemente una estrategia de DevOps. Para mantener la confiabilidad y, al mismo tiempo, aumentar la velocidad de desarrollo, considere la posibilidad de [automatizar las implementaciones y pruebas con Azure Pipelines](/azure/devops/pipelines/ecosystems/java-webapp). Si usa espacios de implementación, puede [automatizar la implementación en un espacio](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot) y cambiar al siguiente espacio.

1. Diseñe e implemente una estrategia de continuidad empresarial y recuperación ante desastres. En el caso de las aplicaciones críticas, considere la posibilidad de usar una [arquitectura de implementación de varias regiones](/azure/architecture/reference-architectures/app-service-web-app/multi-region).
