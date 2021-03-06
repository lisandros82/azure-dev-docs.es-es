---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: affabacec95b8f1c4c7ea654ff9a765056220c76
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825838"
---
### <a name="inventory-external-resources"></a>Recursos externos de inventario

Los recursos externos, tales como los orígenes de datos, los agentes de mensajes JMS y otros, se insertan a través de la interfaz de directorio y nomenclatura de Java (JNDI). Algunos de estos recursos pueden requerir una migración o reconfiguración.

#### <a name="inside-your-application"></a>Dentro de la aplicación

Inspeccione el archivo *META-INF/context.xml*. Busque elementos `<Resource>` dentro del elemento `<Context>`.

#### <a name="on-the-application-servers"></a>En los servidores de aplicaciones

Inspeccione los archivos *$CATALINA_BASE/conf/context.xml* y *$CATALINA_BASE/conf/server.xml*, así como los archivos *.xml* que se encuentran en los directorios *$CATALINA_BASE/conf/[nombreDelMotor]/[nombreDelHost]* .

En los archivos *context.xml*, los recursos de JNDI se describen con los elementos `<Resource>` que están dentro del elemento `<Context>` de nivel superior.

En los archivos *server.xml*, los recursos de JNDI se describen con los elementos `<Resource>` que están dentro del elemento `<GlobalNamingResources>`.

#### <a name="datasources"></a>Orígenes de datos

Los orígenes de datos son recursos de JNDI con el atributo `type` establecido en `javax.sql.DataSource`. Para cada origen de datos, documente la siguiente información:

* ¿Cuál es el nombre del origen de datos?
* ¿Cuál es la configuración del grupo de conexiones?
* ¿Dónde puedo encontrar el archivo JAR del controlador JDBC?

#### <a name="all-other-external-resources"></a>Todos los demás recursos externos

No es factible documentar todas las dependencias externas posibles en esta guía. Es responsabilidad del equipo comprobar que puede cumplir todas las dependencias externas de la aplicación después de la migración.
