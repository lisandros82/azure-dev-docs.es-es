---
title: Uso de Spring Data JDBC con Azure Database for MySQL
description: Aprenda a usar Spring Data JDBC con una base de datos de Azure Database for MySQL.
documentationcenter: java
ms.date: 01/07/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.topic: conceptual
ms.openlocfilehash: 7a6550be633b29d97d55b8db2f50b2c57d0ba30d
ms.sourcegitcommit: 2ad3f7ce8c87331f8aff759ac2a3dc1b29581866
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2020
ms.locfileid: "76022077"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-mysql"></a>Uso de Spring Data JDBC con MySQL de Azure

## <a name="overview"></a>Información general

En este artículo se explica cómo crear una aplicación de ejemplo que utiliza [Spring Data] para almacenar y recuperar información en una base de datos de [Azure Database for MySQL](/azure/mysql/) mediante [Java Database Connectivity (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/).

## <a name="prerequisites"></a>Prerequisites

Los siguientes requisitos previos son necesarios para seguir los pasos descritos en este artículo:

* Una suscripción de Azure. Si todavía no la tiene, puede activar sus [ventajas como suscriptor de MSDN] o registrarse para obtener una [cuenta de Azure gratuita].
* Un kit de desarrollo de Java (JDK) admitido Para más información sobre los JDK disponibles para desarrollar en Azure, consulte <https://aka.ms/azure-jdks>.
* [Apache Maven](http://maven.apache.org/), versión 3.0 o posterior.
* [Curl ](https://curl.haxx.se/) o una utilidad HTTP similar para probar la funcionalidad.
* Utilidad de línea de comandos [mysql](https://dev.mysql.com/downloads/).
* Un cliente [Git](https://git-scm.com/downloads).

## <a name="create-an-azure-database-for-mysql"></a>Creación de una instancia de Azure Database for MySQL 

### <a name="create-a-server-using-the-azure-portal"></a>Creación de un servidor mediante Azure Portal

> [!NOTE]
> 
> Puede leer información más detallada sobre la creación de bases de datos MySQL en [Creación de un servidor de Azure Database for MySQL mediante Azure Portal](/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).

1. Vaya a Azure Portal en <https://portal.azure.com/> e inicie sesión.

1. Haga clic sucesivamente en **+Crear un recurso**, después en **Bases de datos** y, finalmente en **Azure Database for MySQL**.

   ![Creación de una base de datos MySQL][MYSQL01]

1. Escriba la siguiente información:

   - **Suscripción**: especifique la suscripción de Azure que se va a usar.
   - **Grupo de recursos**: especifique si desea crear un nuevo grupo de recursos o elija uno existente.
   - **Nombre del servidor**: Elija un nombre único para el servidor MySQL; se utilizará para crear un nombre de dominio completo como *wingtiptoysmysql.mysql.database.azure.com*.
   - **Origen de datos**: para este tutorial, seleccione `Blank` para crear una nueva base de datos.
   - **Nombre de usuario de administrador**: especifique el nombre del administrador de base de datos.
   - **Contraseña** y **Confirmar contraseña**: especifique la contraseña para el administrador de base de datos.
   - **Ubicación**: especifique la región geográfica más cercana a la base de datos.
   - **Versión**: especifique la versión de la base de datos más reciente.

   ![Creación de propiedades de la base de datos MySQL][MYSQL02]

1. Cuando haya especificado toda la información anterior, haga clic en **Revisar + crear**.

1. Revise las especificaciones y haga clic en **Crear**.

### <a name="configure-a-firewall-rule-for-your-server-using-the-azure-portal"></a>Configuración de una regla de firewall para el servidor mediante Azure Portal

1. Vaya a Azure Portal en <https://portal.azure.com/> e inicie sesión.

1. Haga clic en **Todos los recursos** y, a continuación, haga clic en el recurso de Azure Database for MySQL que acaba de crear.

1. Haga clic en **Seguridad de la conexión** y, en las **reglas de firewall**, cree una nueva regla mediante la especificación de un nombre único para la regla, escriba el intervalo de direcciones IP que necesitará para acceder a la base de datos y, después, haga clic en **Guardar**. (Para este ejercicio, la dirección IP es la del equipo de desarrollo, que es el cliente.  Puede usarlo tanto para **Dirección IP inicial** como para **Dirección IP final**).

   ![Configuración de la seguridad de la conexión][MYSQL04]

### <a name="retrieve-the-connection-string-for-your-server-using-the-azure-portal"></a>Recuperación de la cadena de conexión para el servidor mediante Azure Portal

1. Vaya a Azure Portal en <https://portal.azure.com/> e inicie sesión.

1. Haga clic en **Todos los recursos** y seleccione el recurso de Azure Database for MySQL que acaba de crear.

1. Haga clic en **Cadenas de conexión** y copie el valor en el campo de texto **JDBC**.

   ![Recuperación de la cadena de conexión JDBC][MYSQL05]

### <a name="create-a-database-using-the-mysql-command-line-utility"></a>Creación de una base de datos mediante la utilidad de línea de comandos `mysql`

1. Abra un shell de comandos y conéctese al servidor MySQL mediante la escritura de un `mysql` comando similar al ejemplo siguiente:

   ```shell
   mysql --host wingtiptoysmysql.mysql.database.azure.com --user wingtiptoysuser@wingtiptoysmysql -p
   ```
   Donde:

   | Parámetro | Descripción |
   |---|---|
   | `host` | Especifica el nombre completo del servidor MySQL que se mencionó anteriormente en este artículo. |
   | `user` | Especifica el administrador de MySQL y el nombre de servidor abreviado que se mencionaron anteriormente en este artículo. |
   | `p` | Especifica que se debe esperar hasta que se pida una contraseña. |

   El servidor MySQL debería responder con una pantalla similar al ejemplo siguiente:

   ```shell
   Welcome to the MySQL monitor.  Commands end with ; or \g.
   Your MySQL connection id is 64552
   Server version: 5.6.39.0 MySQL Community Server (GPL)
   
   Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.
   
   Oracle is a registered trademark of Oracle Corporation and/or its
   affiliates. Other names may be trademarks of their respective
   owners.
   
   Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
   
   mysql>
   ```
   > Nota: Si recibe un error que indica que el servidor no reconoce esta dirección IP, se mostrará la dirección IP que usa el cliente.  Vuelva y asígnela tal y como se describió anteriormente: *Configure una regla de firewall para el servidor mediante Azure Portal*.

1. Cree una base de datos denominada *mysqldb* mediante la escritura de un comando `mysql` similar al ejemplo siguiente:

   ```SQL
   CREATE DATABASE mysqldb;
   ```

   El servidor MySQL debería responder con una pantalla similar al ejemplo siguiente:

   ```shell
   Query OK, 1 row affected (0.30 sec)
   ```

1. OPCIONAL: Puede comprobar que se creó la base de datos mediante la escritura de un comando `mysql` similar al ejemplo siguiente:

   ```SQL
   SHOW DATABASES;
   ```

   El servidor MySQL debería responder con una pantalla similar al ejemplo siguiente:

   ```shell
   +--------------------+
   | Database           |
   +--------------------+
   | information_schema |
   | mysql              |
   | mysqldb            |
   | performance_schema |
   | sys                |
   +--------------------+
   ```

1. Escriba `\q` para salir de la utilidad `mysql`.

## <a name="configure-the-sample-application"></a>Configurar la aplicación de ejemplo

1. Abra un shell de comandos y clone el proyecto de ejemplo con un comando git como el siguiente ejemplo:

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. Busque el archivo *application.properties* en el directorio *resources* del proyecto de ejemplo, o cree el archivo si todavía no existe.

1. Abra el archivo *application.properties* en un editor de texto y agréguele o configure las siguientes líneas; luego, sustituya los valores de ejemplo por los valores adecuados que se mencionaron anteriormente:

   ```yaml
   spring.datasource.url=jdbc:mysql://wingtiptoysmysql.mysql.database.azure.com:3306/mysqldb?useSSL=true&requireSSL=false&serverTimezone=UTC
   spring.datasource.username=wingtiptoysuser@wingtiptoysmysql
   spring.datasource.password=********
    ```
   Donde:

   | Parámetro | Descripción |
   |---|---|
   | `spring.datasource.url` | Especifica la cadena de JDBC de MySQL que se mencionó anteriormente en este artículo, con la zona horaria agregada. |
   | `spring.datasource.username` | Especifica el nombre del administrador de MySQL que se mencionó anteriormente en este artículo, con el nombre abreviado del servidor anexado. |
   | `spring.datasource.password` | Especifica la contraseña de administrador de MySQL que se mencionó anteriormente en este artículo. |

1. Guarde y cierre el archivo *application.properties*.

## <a name="package-and-test-the-sample-application"></a>Empaquetado y prueba de la aplicación de ejemplo 

1. Compile la aplicación de ejemplo con Maven; por ejemplo:

   ```shell
   mvn clean package -P mysql
   ```

1. Inicie la aplicación de ejemplo; por ejemplo:

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. Cree nuevos registros con `curl` desde un símbolo del sistema como en los ejemplos siguientes:

   ```shell
   curl -s -d "{\"name\":\"dog\",\"species\":\"canine\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d "{\"name\":\"cat\",\"species\":\"feline\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   La aplicación debe devolver valores similares a los siguientes:

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. Recupere todos los registros existentes con `curl` desde un símbolo del sistema como los siguientes ejemplos:

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   La aplicación debe devolver valores similares a los siguientes:

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>Resumen

En este tutorial ha creado una aplicación Java de ejemplo que Spring Data para almacenar y recuperar información de una base de datos Azure Database for MySQL mediante JDBC.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Spring y Azure, vaya al centro de documentación de Azure.

> [!div class="nextstepaction"]
> [Spring en Azure](/azure/java/spring-framework)

### <a name="additional-resources"></a>Recursos adicionales

Para más información sobre el uso de Azure con Java, consulte [Azure para desarrolladores de Java] y [Working with Azure DevOps and Java] (Trabajo con Azure DevOps y Java).

<!-- URL List -->

[Azure para desarrolladores de Java]: /azure/java/
[cuenta de Azure gratuita]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: /azure/devops/ (Trabajo con Azure DevOps y Java)
[ventajas como suscriptor de MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[MYSQL01]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png
[MYSQL02]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png
[MYSQL04]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-04.png
[MYSQL05]: media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-05.png
