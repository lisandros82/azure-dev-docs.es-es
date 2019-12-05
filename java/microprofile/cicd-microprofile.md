---
title: CI/CD para aplicaciones de MicroProfile con Azure Pipelines
description: Obtenga información sobre cómo configurar un ciclo de versión de CI/CD para implementar una aplicación de MicroProfile en una instancia de Azure Web App for Containers con Azure Pipelines.
services: devops
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
ms.author: ruyakubu
ms.date: 07/31/2019
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: cdd704626b51105f93c19378511f4a267cb56649
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812240"
---
# <a name="cicd-for-microprofile-apps-using-azure-pipelines"></a>CI/CD para aplicaciones de MicroProfile con Azure Pipelines

En este tutorial se muestra cómo configurar de manera sencilla un ciclo de versión de integración continua e implementación continua (CI/CD) de Azure Pipelines para implementar una aplicación de Java EE [MicroProfile](http://microprofile.io) en una instancia de Azure Web App for Containers. La aplicación MicroProfile de este tutorial usa una imagen base [Payara Micro](https://www.payara.fish/payara_micro) para crear un archivo WAR. 

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
Puede iniciar el proceso de inclusión en contenedores de Azure Pipelines con la creación de una imagen de Docker y la inserción de la imagen de contenedor en una instancia de Azure Container Registry (ACR). El proceso se completa con la creación de una canalización de versión de Azure Pipelines y la implementación de la imagen de contenedor en una aplicación web.

## <a name="prerequisites"></a>Requisitos previos

1. En [Azure Portal](https://portal.azure.com), cree una instancia de [Azure Container Registry](https://azure.microsoft.com/services/container-registry).
   
1. En Azure Portal, cree una instancia de [Azure Web App for Containers](https://azure.microsoft.com/services/app-service/containers/). Seleccione **Linux** como el **SO** y, en **Configurar contenedor**, seleccione **Inicio rápido** como el **Origen de imagen**.  
   
1. Copie y guarde la dirección URL de clonación del repositorio GitHub de ejemplo en [https://github.com/Azure-Samples/microprofile-hello-azure](https://github.com/Azure-Samples/microprofile-hello-azure).
   
1. Regístrese o inicie sesión en su organización de [Azure DevOps](https://dev.azure.com) y cree un [proyecto](/vsts/organizations/projects/create-project) nuevo. 
   
1. Importe el repositorio GitHub de ejemplo a Azure Repos:
   
   1. En la página del proyecto de Azure DevOps, seleccione **Repositorios** en el panel de navegación de la izquierda.
   1. En **o importar un repositorio**, seleccione **Importar**. 
   1. En **Dirección URL de clonación**, escriba la URL de clonación de Git que guardó y seleccione **Importar**.
  
## <a name="create-a-build-pipeline"></a>Creación de una canalización de compilación

La canalización de compilación de la integración continua en Azure Pipelines ejecuta automáticamente todas las tareas de compilación cada vez que hay una confirmación en la aplicación de origen de Java EE. En este ejemplo, Azure Pipelines usa Maven para compilar el proyecto de MicroProfile de Java.

1. En la página del proyecto de Azure DevOps, seleccione **Canalizaciones** > **Compilaciones** en el panel de navegación de la izquierda. 
   
1. Seleccione **Nueva canalización**.
   
1. Seleccione **Use the classic editor to create a pipeline without YAML** (Use el editor clásico para crear una canalización sin YAML). 
   
1. Asegúrese de que el nombre del proyecto y el repositorio GitHub importado aparezcan en los campos y seleccione **Continuar**.
   
1. Seleccione **Maven** en la lista de plantillas y, luego, seleccione **Aplicar**.
   
1. En el panel de la derecha, asegúrese de que **Hosted Ubuntu 1604** aparezca en el menú desplegable **Grupo de agentes**.
   
   > [!NOTE]
   > Esta configuración permite que Azure Pipelines sepa qué servidor de compilación usar.  También puede usar un servidor de compilación personalizado privado.
   
1. Para configurar la canalización para la integración continua, seleccione la pestaña **Desencadenadores** en el panel de navegación de la izquierda y, luego, active la casilla junto a **Habilitar la integración continua**.  
   
1. En la parte superior de la página, seleccione el menú desplegable junto a **Guardar y poner en cola** y seleccione **Guardar**. 

   ![Habilitar la integración continua](media/cicd-microprofile/continuous-integration.png)

## <a name="create-a-docker-build-image"></a>Creación de una imagen de compilación de Docker

Azure Pipelines usa un Dockerfile con una imagen base de Payara Micro para crear una imagen de Docker.  

1. Seleccione la pestaña **Tareas** y, luego, seleccione el signo más **+** junto a **Trabajo del agente 1** para agregar una tarea.
   
   ![Agregar una tarea nueva](media/cicd-microprofile/add-task.png)
   
1. En el panel de la derecha, seleccione **Docker** en la lista de plantillas y, luego, seleccione **Agregar**. 
   
1. Seleccione **buildAndPush** en el panel de la izquierda y, en el panel de la derecha, escriba una descripción en el campo **Nombre para mostrar**.
   
1. En **Container Repositorio** (Repositorio de contenedor), seleccione **Nuevo** junto al campo **Registro de Container**. 
   
1. Rellene el cuadro de diálogo **Add a Docker Registry service connection** (Agregar una conexión de servicio de Registro de Docker) como se indica a continuación:
   
   |Campo|Valor|
   |---|---|
   |**Tipo de Registro**|Seleccione **Azure Container Registry**.|
   |**Nombre de la conexión**|Escriba un nombre para la conexión.|
   |**Suscripción de Azure**|Seleccione su suscripción de Azure en el menú desplegable y, si es necesario, seleccione **Autorizar**.|
   |**Azure Container Registry**|Seleccione el nombre de la instancia de Azure Container Registry en el menú desplegable.| 
   
1. Seleccione **Aceptar**.
   
   ![Agregar una conexión de servicio de Registro de Docker](media/cicd-microprofile/dockerconnection.png)
   
   > [!NOTE]
   > Si usa Docker Hub u otro registro, seleccione **Docker Hub** u **Otros** en lugar de **Azure Container Registry** junto a **Tipo de Registro**. Luego, proporcione las credenciales y la información de conexión para el registro de contenedor.
   
1. En **Comandos**, seleccione **build** en el menú desplegable **Comando**.
   
1. Seleccione los puntos suspensivos **…** junto al campo **Dockerfile**, busque y seleccione el **Dockerfile** del repositorio GitHub y, luego, seleccione **Aceptar**. 
   
   ![Seleccionar el Dockerfile](media/cicd-microprofile/selectdockerfile.png)
   
1. En **Etiquetas**, escriba *latest* (más reciente) en una línea nueva. 
   
1. En la parte superior de la página, seleccione el menú desplegable junto a **Guardar y poner en cola** y seleccione **Guardar**. 

## <a name="push-the-docker-image-to-acr"></a>Inserción de la imagen de Docker en ACR

Azure Pipelines inserta la imagen de Docker en la instancia de Azure Container Registry y la usa para ejecutar la aplicación de API de MicroProfile como una aplicación web de Java en contenedores.

1. Como usa Docker en Azure Pipelines, cree otra plantilla de Docker repitiendo los pasos de [Creación de una imagen de compilación de Docker](#create-a-docker-build-image). Esta vez, seleccione **push** en el menú desplegable **Comando**.
   
1. Seleccione el menú desplegable junto a **Guardar y poner en cola** y, luego, seleccione **Guardar y poner en cola**. 
   
1. En el menú emergente **Ejecutar canalización**, asegúrese de que la opción **Hosted Ubuntu 1604** esté seleccionada en **Grupo de agentes** y seleccione **Guardar y ejecutar**. 
   
1. Una vez finalizada la compilación, puede seleccionar el hipervínculo que aparece en la página **Compilación** para comprobar que la compilación se realizó correctamente y consultar otros detalles.
   
   ![Seleccionar el hipervínculo de compilación](media/cicd-microprofile/checkbuild.png)

## <a name="create-a-release-pipeline"></a>Creación de una canalización de versión

Una canalización de versión continua de Azure Pipelines desencadena automáticamente la implementación en un entorno de destino como Azure en cuanto una compilación se realiza correctamente. Puede crear canalizaciones de versión para entornos como desarrollo, prueba, ensayo o producción.

1. En la página del proyecto de Azure DevOps, seleccione **Canalizaciones** > **Versiones** en el menú de navegación de la izquierda. 
   
1. Seleccione **Nueva canalización**.
   
1. Seleccione **Implementar una aplicación Java en Azure App Service** en la lista de plantillas y, luego, seleccione **Aplicar**. 
   
   ![Seleccione la plantilla Implementar una aplicación Java en Azure App Service](media/cicd-microprofile/selectreleasetemplate.png)
   
1. En la ventana emergente, cambie **Fase 1** a un nombre de fase como *Dev* (Desarrollo), *Test* (Prueba), *Staging* (Ensayo) o *Production* (Producción) y cierre la ventana. 
   
1. En **Artefactos** en el panel de la izquierda, seleccione **Agregar** para vincular artefactos desde la canalización de compilación a la canalización de versión. 
   
1. En el panel de la derecha, seleccione la canalización de compilación en el menú desplegable en **Origen (canalización de compilación)** y, luego, seleccione **Agregar**.
   
   ![Adición de un artefacto de compilación](media/cicd-microprofile/addbuildartifact.png)
   
1. Seleccione el hipervínculo en la fase **Producción** para **ver las tareas de la fase**.
   
   ![Seleccionar el nombre de la fase](media/cicd-microprofile/viewstagetasks.png)
   
1. En el panel de la derecha, rellene el formulario como se indica a continuación:
   
   |Campo|Valor|
   |---|---|
   |**Suscripción de Azure**|Seleccione su suscripción de Azure en el menú desplegable.|
   |**Tipo de aplicación**|Seleccione **Web App for Containers (Linux)** en el menú desplegable.|
   |**Nombre de App Service**|Seleccione la instancia de ACR en el menú desplegable.|
   |**Registro o espacio de nombres**|Escriba el nombre de ACR en el campo. Por ejemplo, escriba *miRegistrodemicroprofile.azure.io*.
   |**Repositorio**|Escriba el repositorio que incluye la imagen de Docker.| 
   
   ![Configuración de las tareas de la fase](media/cicd-microprofile/configurestage.png)
   
1. En el panel de la izquierda, seleccione **Deploy War to Azure App Service** (Implementar War en Azure App Service) y, en el panel de la derecha, escriba la etiqueta *latest* (más reciente) en el campo **Tag** (Etiqueta). 
   
1. En el panel de la izquierda, seleccione **Ejecutar en el agente** y, en el panel de la derecha, seleccione **Hosted Ubuntu 1604** en el menú desplegable **Grupo de agentes**. 

## <a name="set-up-environment-variables"></a>Configurar las variables de entorno

Agregue y defina variables de entorno para conectarse al Registro de contenedor durante la implementación.

1. Seleccione la pestaña **Variables** y, luego, seleccione **Agregar** para agregar las variables siguientes para la dirección URL del Registro de contenedor, el nombre de usuario y la contraseña. 
   
   |NOMBRE|Valor|
   |---|---|
   |*registry.url*|Escriba la dirección URL del Registro de contenedor. Por ejemplo: *https:\//miRegistrodemicroprofile.azure.io*.|
   |*registry.username*|Escriba el nombre de usuario del Registro.|
   |*registry.password*|Escriba la contraseña del Registro. Por seguridad, seleccione el icono de candado para mantener oculto el valor de la contraseña.|
   
   ![Agregar variables](media/cicd-microprofile/addvariables.png)
   
1. En la pestaña **Tareas**, seleccione **Deploy War to Azure App Service** (Implementar War en Azure App Service) en el panel de la izquierda. 
   
1. En el panel de la derecha, expanda **Configuración y opciones de la aplicación** y, luego, seleccione los puntos suspensivos **…** junto al campo **Configuración de aplicación**.
   
1. En el menú emergente **Configuración de aplicación**, seleccione **Agregar** para definir y asignar las variables de configuración de la aplicación:
   
   |NOMBRE|Valor|
   |---|---|
   |*DOCKER_REGISTRY_SERVER_URL*|*$(registry.url)*|
   |*DOCKER_REGISTRY_SERVER_USERNAME*|*$(registry.username)*|
   |*DOCKER_REGISTRY_SERVER_PASSWORD*|*$(registry.password)*|
   
1. Seleccione **Aceptar**.
   
   ![Agregar y establecer variables](media/cicd-microprofile/appsettings.png)
   
## <a name="set-up-continuous-deployment"></a>Configurar la implementación continua 

Para habilitar la implementación continua: 

1. En la pestaña **Canalización**, en **Artefactos**, seleccione el icono de rayo en el artefacto de compilación. 
   
1. En el panel de la derecha, establezca el **Desencadenador de implementación continua** en **Habilitado**.
   
1. Seleccione **Guardar** en la esquina superior derecha y, luego, seleccione **Guardar** nuevamente. 
   
   ![Habilitar el desencadenador de implementación continua](media/cicd-microprofile/setcontinuousdeployment.png)
   
## <a name="deploy-the-java-app"></a>Implementación de la aplicación Java

Ahora que habilitó CI/CD, modificar el código fuente crea y ejecuta compilaciones y versiones de manera automática. También puede crear y ejecutar versiones de manera manual, tal como se indica a continuación:

1. En la esquina superior derecha de la página de canalización de versión, seleccione **Crear versión**.
   
1. En la página **Crear una nueva versión**, seleccione el nombre de la fase en **Fases para un cambio de desencadenador de automatizado a manual**. 
   
1. Seleccione **Crear**. 
   
1. Seleccione el nombre de la versión, mantenga el puntero sobre la fase o selecciónela y, luego, seleccione **Implementar**. 
   
## <a name="test-the-java-web-app"></a>Prueba de la aplicación web de Java

Una vez que la implementación se completa de manera correcta, pruebe la aplicación web. 

1. Copie la dirección URL de la aplicación web desde Azure Portal.
   
   ![Aplicación de App Service en Azure Portal](media/cicd-microprofile/portalurl.png)
   
1. Escriba la dirección URL en el explorador web para ejecutar la aplicación. En la página web debe aparecer **Hello Azure!**
   
   ![Página de la aplicación web de Java](media/cicd-microprofile/webapp.png)

