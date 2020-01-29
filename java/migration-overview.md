---
title: Migración de aplicaciones de Java a Azure
description: En este tema se proporciona una introducción a las estrategias recomendadas para migrar aplicaciones de Java a Azure.
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: fbf1faabbefcb987cf398a45005eb480ec16b27d
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288594"
---
# <a name="migrate-java-applications-to-azure"></a>Migración de aplicaciones de Java a Azure

En este tema se proporciona una introducción a las estrategias recomendadas para migrar aplicaciones de Java a Azure.

## <a name="identifying-application-type"></a>Identificación del tipo de aplicación

Antes de seleccionar un destino en la nube para la aplicación de Java, deberá identificar su tipo de aplicación. La mayoría de las aplicaciones de Java son de uno de los siguientes tipos:

* [Aplicaciones de Spring Boot/JAR](#spring-boot--jar-applications)
* [Spring Cloud/microservicios](#spring-cloud--microservices)
* [Aplicaciones web](#web-applications)
* [Aplicaciones de Java EE](#java-ee-applications)
* [Trabajos por lotes o programados](#batch--scheduled-jobs)

Estos tipos se describen en las secciones siguientes.

### <a name="spring-boot--jar-applications"></a>Aplicaciones de Spring Boot/JAR

Muchas aplicaciones más recientes se invocan directamente desde la línea de comandos. Estas aplicaciones todavía controlan las solicitudes web pero, en lugar de depender de un servidor de aplicaciones para proporcionar el control de solicitudes HTTP, incorporan la comunicación HTTP y todas las demás dependencias directamente en el paquete de aplicación. Estas aplicaciones se suelen compilar con marcos como Spring Boot, Dropwizard, Micronaut, MicroProfile, Vert.x y otros.

Estas aplicaciones se empaquetan en archivos con la extensión *.jar* (archivos JAR).

### <a name="spring-cloud--microservices"></a>Spring Cloud/microservicios

Con el estilo arquitectónico de los microservicios, se desarrolla una sola aplicación como un conjunto de servicios pequeños, cada uno de los cuales se ejecuta en su propio proceso y se comunica mediante mecanismos ligeros, a normalmente API de recursos HTTP. Estos servicios se crean en torno a las funcionalidades empresariales y se pueden implementar de forma independiente con maquinaria de implementación totalmente automatizada. Estos servicios tienen un mínimo de administración centralizada, que puede estar escrita en distintos lenguajes de programación y usar diferentes tecnologías de almacenamiento de datos. Estos servicios suelen compilarse con marcos como Spring Cloud.

Estos servicios se empaquetan en varias aplicaciones con la extensión *.jar* (archivos JAR).

### <a name="web-applications"></a>Aplicaciones web

Las aplicaciones web se ejecutan en un contenedor de [Servlet](https://en.wikipedia.org/wiki/Java_servlet). Algunas usan las API de Servlet directamente, mientras que otras usan marcos adicionales que encapsulan las API de Servlet, como Apache, Spring MVC, JavaServer Faces (JSF), etc.

Las aplicaciones web se empaquetan en archivos con la extensión *.war* (archivos WAR).

### <a name="java-ee-applications"></a>Aplicaciones de Java EE

Las aplicaciones de Java EE (también conocidas como aplicaciones J2EE o, más recientemente, aplicaciones de Yakarta) pueden contener algunos, todos o ninguno de los elementos de las aplicaciones web. También pueden contener y consumir muchos componentes adicionales, tal y como se define en la especificación [Java EE](https://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition).

Las aplicaciones de Java EE se pueden empaquetar como archivos con la extensión *.ear* (archivos EAR) o como archivos con la extensión *.war* (archivos WAR).

Las aplicaciones de Java EE se deben implementar en servidores de aplicaciones compatibles con Java EE (como WebLogic, WebSphere, WildFly, GlassFish, Payara y otros).

Las aplicaciones que solo se basan en las características proporcionadas por la especificación de Java EE (es decir, las aplicaciones independientes de la aplicación) se pueden migrar desde un servidor de aplicaciones compatible a otro. Si su aplicación depende de un servidor de aplicaciones específico (dependiente del servidor de aplicaciones), es posible que tenga que seleccionar un destino de servicio de Azure que le permita hospedar ese servidor de aplicaciones.

### <a name="batch--scheduled-jobs"></a>Trabajos por lotes o programados

Algunas aplicaciones están diseñadas para ejecutarse brevemente, ejecutar una carga de trabajo concreta y después salir, sin esperar solicitudes ni los datos proporcionados por el usuario. A veces, estos trabajos deben ejecutarse una vez o a intervalos regulares programados. En el entorno local, estos trabajos se suelen invocar desde el archivo crontab de un servidor.

Estas aplicaciones se empaquetan en archivos con la extensión *.jar* (archivos JAR).

> [!NOTE]
> Si la aplicación usa un programador (como Spring Batch o Quartz) para ejecutar tareas programadas, es muy recomendable factorizar estas tareas para que se ejecuten fuera de la aplicación. Si la aplicación se escala en varias instancias en la nube, el mismo trabajo se ejecutará más de una vez. Además, si el mecanismo de programación utiliza la zona horaria local del host, el comportamiento podría no ser el esperado si la aplicación se escala en regiones diferentes.

## <a name="selecting-the-target-azure-service-destination"></a>Selección del destino de servicio de Azure

En las secciones siguientes se muestran los destinos de servicio que cumplen los requisitos de la aplicación y qué responsabilidades conllevan.

### <a name="feature-grid"></a>Tabla de características

Use la tabla siguiente para identificar los destinos que admiten los tipos de aplicaciones y las características que necesita.

|   |Aplicación<br>Servicio<br>Java SE|Aplicación<br>Servicio<br>Tomcat|Aplicación<br>Servicio<br>WildFly|Azure<br>Spring<br>Nube|AKS|Virtual Machines|
|---|---|---|---|---|---|---|
| Aplicaciones de Spring Boot/JAR                                    |&#x2714;|        |        |        |&#x2714;|&#x2714;|
| Spring Cloud/microservicios                                      |        |        |        |&#x2714;|&#x2714;|&#x2714;|
| Aplicaciones web                                                  |        |&#x2714;|&#x2714;|        |&#x2714;|&#x2714;|
| Aplicaciones de Java EE                                              |        |        |&#x2714;|        |&#x2714;|&#x2714;|
| Servidores de aplicaciones comerciales<br>(como WebLogic o WebSphere) |        |        |        |        |&#x2714;|&#x2714;|
| Persistencia a largo plazo en el sistema de archivos local                         |&#x2714;|&#x2714;|&#x2714;|        |&#x2714;|&#x2714;|
| Agrupación en clústeres en el nivel del servidor de aplicaciones                               |        |        |        |        |&#x2714;|&#x2714;|
| Trabajos por lotes o programados                                            |        |        |        |&#x2714;|&#x2714;|&#x2714;|

### <a name="ongoing-responsibility-grid"></a>Tabla de responsabilidad en curso

Use la tabla siguiente para comprender la responsabilidad que cada destino conlleva para su equipo después de la migración.

El equipo será responsable de forma continua de las tareas indicadas con "&#x1F449;". Se recomienda implementar un proceso sólido y muy automatizado para cumplir todas estas responsabilidades. 

> [!NOTE]
> Esta no es una lista exhaustiva de todas las responsabilidades.

|   | App Service | Azure Spring Cloud | AKS | Virtual Machines |
|---|---|---|---|---|
| Actualización de bibliotecas<br>(incluida la corrección de vulnerabilidades)                 | &#x1F449;   | &#x1F449;   | &#x1F449;   | &#x1F449; |
| Actualización del servidor de aplicaciones<br>(incluida la corrección de vulnerabilidades)    | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| Actualización del tiempo de ejecución de Java<br>(incluida la corrección de vulnerabilidades)          | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| Desencadenamiento de actualizaciones de Kubernetes<br>(lo realiza Azure con un desencadenador manual) | N/D         | N/D         | &#x1F449;   | N/D       |
| Conciliación de los cambios de la API de Kubernetes no compatibles con versiones anteriores                  | N/D         | N/D         | &#x1F449;   | N/D       |
| Actualización de la imagen base del contenedor<br>(incluida la corrección de vulnerabilidades)      | N/D         | N/D         | &#x1F449;   | N/D       |
| Activación del sistema operativo<br>(incluida la corrección de vulnerabilidades)      | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| Detectación y reinicio de instancias con errores                                   | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| Implementación de purga y reinicio gradual de las actualizaciones                       | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| Administración de la infraestructura                                                   | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| Supervisión y administración de alertas                                             | &#x1F449;   | &#x1F449;   | &#x1F449;   | &#x1F449; |

Si implementa el contenedor de Servlet (como Spring Boot) como parte de la aplicación, se considerará una biblioteca y, como tal, será siempre su responsabilidad.

## <a name="ensuring-on-premises-connectivity"></a>Garantizar la conectividad local

Si su aplicación necesita acceder a cualquiera de los servicios locales, deberá aprovisionar uno de los servicios de conectividad de Azure. Para obtener más información, consulte. [Elección de una solución para conectar una red local a Azure](/azure/architecture/reference-architectures/hybrid-networking/). También tendrá que refactorizar la aplicación para que use las API disponibles públicamente que exponen los recursos locales.

Debe completar este trabajo antes de iniciar cualquier migración.

## <a name="inventory-current-capacity-and-resource-usage"></a>Capacidad actual de inventario y uso de los recursos

Documente el hardware de los servidores de producción actuales, además del número medio y máximo de solicitudes y el uso de los recursos. Necesitará esta información para aprovisionar los recursos en el destino del servicio.

## <a name="migration-guidance"></a>Guía de migración

Use las tablas siguientes para encontrar la guía de migración por tipo de aplicación y destino de servicio de Azure.

**Aplicaciones de Java**

Busque en las filas el tipo de aplicación de Java y, en las columnas, el destino de servicio de Azure que hospedará la aplicación.

|Destino&nbsp;→<br><br>Tipo de&nbsp;aplicación&nbsp;↓|Aplicación<br>Servicio<br>Java SE|Aplicación<br>Servicio<br>Tomcat|Aplicación<br>Servicio<br>WildFly|Azure<br>Spring<br>Nube|AKS|Virtual Machines|
|---|---|---|---|---|---|---|
| Spring Boot/<br>aplicaciones JAR | planeado | planeado        | planeado | planeado | planeado        | planeado |
| Spring Cloud/<br>microservicios   | N/D     | N/D            | N/D     | planeado | planeado        | planeado |
| Aplicaciones web<br>en Tomcat     | N/D     | [disponible][2] | N/D     | N/D     | [disponible][3] | planeado |

**Aplicaciones de Java EE**

Busque en las filas el tipo de aplicación de Java EE que se ejecuta en un servidor de aplicaciones específico. Busque en las columnas el destino del servicio de Azure que hospedará la aplicación.

|Destino&nbsp;→<br><br>Servidor de aplicaciones&nbsp;↓|Aplicación<br>Servicio<br>Java SE|Aplicación<br>Servicio<br>Tomcat|Aplicación<br>Servicio<br>WildFly|Azure<br>Spring<br>Nube|AKS|Virtual Machines|
|---|---|---|---|---|---|---|
| WildFly /<br>JBoss AS | N/D | N/D | planeado | N/D | planeado | planeado |
| WebLogic              | N/D | N/D | planeado | N/D | planeado | planeado |
| WebSphere             | N/D | N/D | planeado | N/D | planeado | planeado |
| JBoss EAP             | N/D | N/D | planeado | N/D | N/D     | planeado |

<!-- reference links, for use with tables -->
[1]: media/migration-overview/logo_azure.svg
[2]: migrate-tomcat-to-tomcat-app-service.md
[3]: migrate-tomcat-to-containers-on-azure-kubernetes-service.md
