---
title: Migración de aplicaciones de Tomcat a contenedores en Azure Kubernetes Service
description: En esta guía se describe lo que debe tener en cuenta cuando quiera migrar una aplicación de Tomcat existente para que se ejecute en un contenedor de Azure Kubernetes Service.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: dbcf1f0989208f960f31fec13a65477d87b1a042
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825837"
---
# <a name="migrate-tomcat-applications-to-containers-on-azure-kubernetes-service"></a>Migración de aplicaciones de Tomcat a contenedores en Azure Kubernetes Service

En esta guía se describe lo que hay que tener en cuenta para migrar una aplicación de Tomcat existente a un contenedor de Azure Kubernetes Service (AKS).

## <a name="pre-migration-steps"></a>Pasos previos a la migración

[!INCLUDE [inventory-external-resources](includes/migration/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/migration/inventory-secrets.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

<!-- AKS-specific addendum to inventory-persistence-usage -->
#### <a name="dynamic-or-internal-content"></a>Contenido dinámico o interno

En el caso de los archivos que la aplicación lee y escribe con frecuencia (por ejemplo, los archivos de datos temporales) o los archivos estáticos que solo son visibles para la aplicación, puede montar recursos compartidos de Azure Storage como volúmenes persistentes. Para más información, consulte [Creación dinámica y uso de un volumen persistente con Azure Files en Azure Kubernetes Service](/azure/aks/azure-files-dynamic-pv).

### <a name="identify-session-persistence-mechanism"></a>Identificación del mecanismo de persistencia de sesión

Para identificar el administrador de persistencia de sesión que se está usando, inspeccione los archivos *context.xml* de la aplicación y la configuración de Tomcat. Busque el elemento `<Manager>` y, a continuación, anote el valor del atributo `className`.

Las implementaciones integradas de [PersistentManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html) de Tomcat, como [StandardManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Standard_Implementation) o [FileStore](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Nested_Components) no están diseñadas para usarse con una plataforma distribuida y escalada como Kubernetes. AKS puede equilibrar la carga entre varios pods y reiniciar de forma transparente cualquier pod en cualquier momento, por lo que no se recomienda conservar el estado mutable en un sistema de archivos.

Si se necesita la persistencia de la sesión, deberá usar una implementación de `PersistentManager` alternativa que escribirá en un almacén de datos externo, como Pivotal Session Manager con Redis Cache. Para más información, consulte [Uso de Redis como caché de sesión con Tomcat](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat).

### <a name="special-cases"></a>Casos especiales

Algunos escenarios de producción pueden requerir otros cambios o imponer limitaciones adicionales. Aunque estos escenarios no son frecuentes, es importante asegurarse de que no son aplicables a la aplicación o que se han resuelto correctamente.

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>Determinación de si la aplicación se basa en trabajos programados

Los trabajos programados, como las tareas del programador de Quartz o los trabajos de cron, no se pueden usar con implementaciones de Tomcat en contenedor. Si la aplicación se escala horizontalmente, un trabajo programado se podría ejecutar más de una vez por cada período programado. Esta situación puede tener consecuencias no deseadas.

Haga un inventario de todos los trabajos programados, dentro o fuera del servidor de aplicaciones.

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>Determinación de si la aplicación contiene código específico del sistema operativo

Si la aplicación contiene código adaptado al sistema operativo en el que se ejecuta la aplicación, es necesario refactorizar la aplicación para que no dependa del sistema operativo subyacente. Por ejemplo, los usos de `/` o `\` en las rutas de acceso del sistema de archivos tendrán que reemplazarse por [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) o [`Path.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-).

#### <a name="determine-whether-memoryrealm-is-used"></a>Determinación de si se usa MemoryRealm

[MemoryRealm](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/realm/MemoryRealm.html) requiere un archivo XML persistente. En Kubernetes, este archivo deberá agregarse a la imagen de contenedor o cargarse en un [almacenamiento compartido que esté disponible para los contenedores](#identify-session-persistence-mechanism). El parámetro `pathName` tendrá que modificarse según corresponda.

Para determinar si `MemoryRealm` se usa actualmente, inspeccione los archivos *server.xml* y *context.xml* y busque elementos `<Realm>` en los que el atributo `className` esté establecido en `org.apache.catalina.realm.MemoryRealm`.

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>Determinación de si se utiliza el seguimiento de sesión SSL

En las implementaciones en contenedor, lo habitual es que las sesiones SSL se descarguen fuera del contenedor de la aplicación, y normalmente lo hace el controlador de entrada. Si su aplicación requiere [seguimiento de sesión SSL](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL), asegúrese de que el tráfico SSL pase directamente al contenedor de la aplicación.

#### <a name="determine-whether-accesslogvalve-is-used"></a>Determinación de si se usa AccessLogValve

Si se utiliza [AccessLogValve](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/valves/AccessLogValve.html), el parámetro `directory` debe establecerse en un [recurso compartido de Azure Files montado](/azure/aks/azure-files-dynamic-pv) o en uno de sus subdirectorios.

### <a name="in-place-testing"></a>Pruebas en contexto

Antes de crear imágenes de contenedor, migre la aplicación al JDK y Tomcat que piensa usar en AKS. Pruebe la aplicación exhaustivamente para garantizar su compatibilidad y rendimiento.

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

## <a name="migration"></a>Migración

A excepción del primer paso ("Aprovisionamiento del registro de contenedor y AKS"), le recomendamos que siga los pasos que se indican a continuación de forma individual para cada aplicación (archivo WAR) que quiera migrar.

> [!NOTE]
> Algunas implementaciones de Tomcat pueden tener varias aplicaciones que se ejecutan en un solo servidor de Tomcat. Si este es el caso de su implementación, es muy recomendable ejecutar cada aplicación en un pod independiente. Esto le permite optimizar el uso de recursos para cada aplicación a la vez que minimiza la complejidad y el acoplamiento.

### <a name="provision-container-registry-and-aks"></a>Aprovisionamiento del registro de contenedor y AKS

Cree un registro de contenedor y un clúster de Azure Kubernetes cuya entidad de servicio tenga el rol de lector en el registro. Asegúrese de [elegir el modelo de red adecuado](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model) para los requisitos de red del clúster.

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```

### <a name="prepare-the-deployment-artifacts"></a>Preparación de los artefactos de implementación

Clone el [repositorio de GitHub Tomcat on Containers Quickstart](https://github.com/Azure/tomcat-container-quickstart). Contiene un archivo Dockerfile y archivos de configuración de Tomcat con varias optimizaciones recomendadas. En los pasos siguientes, se describen las modificaciones que probablemente deba realizar en estos archivos antes de compilar la imagen de contenedor e implementarla en AKS.

#### <a name="open-ports-for-clustering-if-needed"></a>Apertura de los puertos para la agrupación en clústeres, si es necesario

Si tiene previsto usar la [agrupación en clústeres de Tomcat](https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html) en AKS, asegúrese de que los intervalos de puertos necesarios estén expuestos en el archivo Dockerfile. Para especificar la dirección IP del servidor en *server.xml*, asegúrese de usar un valor de una variable que se inicializa durante el inicio del contenedor en la dirección IP del pod.

Como alternativa, el estado de sesión se puede [conservar en una ubicación alternativa](#identify-session-persistence-mechanism) para que esté disponible para todas las réplicas.

Para determinar si la aplicación usa la agrupación en clústeres, busque el elemento `<Cluster>` dentro de los elementos `<Host>` o `<Engine>` del archivo *server.xml*.

#### <a name="add-jndi-resources"></a>Adición de recursos de JNDI

Edite *server.xml* para agregar los recursos que preparó en los pasos previos a la migración, como los orígenes de datos.

Por ejemplo:

```xml
<!-- Global JNDI resources
      Documentation at /docs/jndi-resources-howto.html
-->
<GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml"
               />

    <!-- Migrated datasources here: -->
    <Resource
        name="jdbc/dbconnection"
        type="javax.sql.DataSource"
        url="${postgresdb.connectionString}"
        driverClassName="org.postgresql.Driver"
        username="${postgresdb.username}"
        password="${postgresdb.password}"
    />
    <!-- End of migrated datasources -->
</GlobalNamingResources>
```

### <a name="build-and-push-the-image"></a>Compilación e inserción de la imagen

El comando `az acr build` es la manera más sencilla de compilar y cargar la imagen en Azure Container Registry (ACR) para que AKS pueda usarla. Este comando no requiere que Docker esté instalado en el equipo. Por ejemplo, si tiene el archivo Dockerfile anterior y el paquete de aplicación *petclinic.war* en el directorio actual, puede compilar la imagen de contenedor en ACR con un solo paso:

```bash
az acr build -t "${acrName}.azurecr.io/petclinic:{{.Run.ID}}" -r $acrName --build-arg APP_FILE=petclinic.war --build-arg=prod.server.xml .
```

Puede omitir el parámetro `--build-arg APP_FILE...` si el archivo WAR se llama *ROOT.war*. Puede omitir el parámetro `--build-arg SERVER_XML...` si el archivo XML del servidor se llama *server.xml*. Ambos archivos deben estar en el mismo directorio que el archivo *Dockerfile*.

Como alternativa, puede usar la CLI de Docker para compilar la imagen localmente. Este enfoque puede simplificar las pruebas y perfeccionar la imagen antes de la implementación inicial en ACR. Sin embargo, requiere instalar la CLI de Docker y el demonio de Docker para ejecutarse.

```bash
# Build the image locally
sudo docker build . --build-arg APP_FILE=petclinic.war -t "${acrName}.azurecr.io/petclinic:1"

# Run the image locally
sudo docker run -d -p 8080:8080 "${acrName}.azurecr.io/petclinic:1"

# Your application can now be accessed with a browser at http://localhost:8080.

# Log into ACR
sudo az acr login -n $acrName

# Push the image to ACR
sudo docker push "${acrName}.azurecr.io/petclinic:1"
```

Para más información detallada sobre la creación y el almacenamiento de imágenes de contenedor en Azure, consulte el [curso de Microsoft Learn correspondiente](/learn/modules/build-and-store-container-images/).

### <a name="provision-a-public-ip-address"></a>Aprovisionamiento de una dirección IP pública

Si la aplicación va a ser accesible desde fuera de las redes internas o virtuales, necesitará una dirección IP estática pública. Esta dirección IP debe aprovisionarse dentro del grupo de recursos del nodo del clúster.

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```

### <a name="deploy-to-aks"></a>Implementación en AKS

[Cree y aplique los archivos YAML de Kubernetes](/azure/aks/kubernetes-walkthrough#run-the-application). Si va a crear un equilibrador de carga externo (ya sea para la aplicación o para un controlador de entrada), asegúrese de proporcionar la dirección IP aprovisionada en la sección anterior como `LoadBalancerIP`.

Incluya los [parámetros externos como variables de entorno](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/). No incluya secretos (por ejemplo, contraseñas, claves de API y cadenas de conexión de JDBC). Los secretos se describen en la sección [Configuración de un volumen flexible de KeyVault](#configure-keyvault-flexvolume).

### <a name="configure-persistent-storage"></a>Configuración de almacenamiento persistente

Si su aplicación requiere almacenamiento no volátil, configure uno o más [volúmenes persistentes](/azure/aks/azure-disks-dynamic-pv).

Quizás quiera crear un volumen persistente con Azure Files, montado en el directorio de registros de Tomcat ( */tomcat_logs*) para conservar los registros de forma centralizada. Para más información, consulte [Creación dinámica y uso de un volumen persistente con Azure Files en Azure Kubernetes Service (AKS)](/azure/aks/azure-files-dynamic-pv).

### <a name="configure-keyvault-flexvolume"></a>Configuración de un volumen flexible de KeyVault

[Cree un almacén de claves de Azure](/azure/key-vault/quick-create-cli) y rellene todos los secretos necesarios. Después, configure un [volumen flexible de KeyVault](https://github.com/Azure/kubernetes-keyvault-flexvol/blob/master/README.md) para que esos secretos sean accesibles para los pods.

Tendrá que modificar el script de inicio (*startup.sh* en el repositorio [Tomcat on Containers](https://github.com/Azure/tomcat-container-quickstart) de GitHub) para importar los certificados en el almacén de claves local del contenedor.

### <a name="migrate-scheduled-jobs"></a>Migración de los trabajos programados

Para ejecutar trabajos programados en el clúster de AKS, defina los [trabajos de cron](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/) que sean necesarios.

## <a name="post-migration-steps"></a>Pasos posteriores a la migración

Ahora que ha migrado la aplicación a AKS, debe comprobar que funciona como se espera. Una vez hecho esto, tenemos algunas recomendaciones que pueden hacer que su aplicación sea más nativa de la nube.

1. Considere la posibilidad de [agregar un nombre DNS](/azure/aks/ingress-static-ip#configure-a-dns-name) a la dirección IP asignada al controlador de entrada o al equilibrador de carga de la aplicación.

1. Considere la posibilidad de [agregar gráficos de Helm para la aplicación](https://helm.sh/docs/topics/charts/). Un gráfico de Helm permite parametrizar la implementación de la aplicación para que un conjunto de clientes más diverso pueda usarla y personalizarla.

1. Diseñe e implemente una estrategia de DevOps. Para mantener la confiabilidad y, al mismo tiempo, aumentar la velocidad de desarrollo, considere la posibilidad de [automatizar las implementaciones y pruebas con Azure Pipelines](/azure/devops/pipelines/ecosystems/kubernetes/aks-template).

1. Habilite la [supervisión de Azure en el clúster](/azure/azure-monitor/insights/container-insights-enable-existing-clusters) para poder recopilar los registros de contenedor o realizar un seguimiento del uso, entre otras tareas.

1. Considere la posibilidad de exponer métricas específicas de la aplicación mediante Prometheus. Prometheus es un marco de métricas de código abierto que se ha adoptado ampliamente en la comunidad de Kubernetes. Puede configurar el [barrido de métricas de Prometheus en Azure Monitor](/azure/azure-monitor/insights/container-insights-prometheus-integration) en lugar de hospedar su propio servidor de Prometheus para habilitar la agregación de métricas de sus aplicaciones y automatizar la respuesta o el escalado en caso de condiciones anómalas.

1. Diseñe e implemente una estrategia de continuidad empresarial y recuperación ante desastres. En el caso de las aplicaciones críticas, considere la posibilidad de usar una [arquitectura de implementación de varias regiones](/azure/aks/operator-best-practices-multi-region).

1. Consulte la [directiva de compatibilidad de versiones de Kubernetes](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy). Es su responsabilidad mantener [actualizado el clúster de AKS](/azure/aks/upgrade-cluster) para asegurarse de que siempre ejecuta una versión compatible.

1. Haga que todos los miembros del equipo responsables del desarrollo de aplicaciones y la administración del clúster revisen los [procedimientos recomendados de AKS](/azure/aks/best-practices).

1. Evalúe los elementos del archivo *logging.properties*. Considere la posibilidad de eliminar o reducir parte de la salida del registro para mejorar el rendimiento.

1. Considere la posibilidad de [supervisar el tamaño de la caché de código](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm) y de agregar los parámetros `-XX:InitialCodeCacheSize` y `-XX:ReservedCodeCacheSize` a la variable `JAVA_OPTS` en Dockerfile para optimizar aún más el rendimiento.
