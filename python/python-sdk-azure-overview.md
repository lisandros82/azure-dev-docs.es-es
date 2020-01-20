---
title: SDK de Azure para Python
description: Introducción a las características y funcionalidades de Azure SDK para Python, que ayuda a los desarrolladores a ser más productivos en su trabajo con los servicios de Azure.
ms.date: 10/30/2019
ms.topic: conceptual
ms.openlocfilehash: 83a9541330f270e51ffa5ed8952a93dee8ff132f
ms.sourcegitcommit: ac68fb174d606c7af2bfa79fe32b8ca7b73c86a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946697"
---
# <a name="azure-sdk-for-python"></a>SDK de Azure para Python

Azure SDK para Python simplifica el uso y la administración de los recursos de Azure desde el código de aplicación de Python. El SDK admite Python 2.7 y Python 3.5.3 o versiones posteriores.

Para instalar el SDK, instale cualquiera de sus bibliotecas de componentes individuales mediante `pip install <library>`. Encontrará los nombres de las bibliotecas para los diferentes servicios en la [documentación de Azure SDK para Python](https://azure.github.io/azure-sdk-for-python/).

Para instrucciones más detalladas sobre cómo instalar bibliotecas e importarlas en proyectos, consulte [Instalación del SDK](python-sdk-azure-install.md). Después, lea el artículo [Introducción al SDK](python-sdk-azure-get-started.yml) para configurar la autenticación y ejecutar el código de ejemplo en su propia suscripción de Azure.

> [!TIP]
> Para obtener información sobre los cambios en el SDK, consulte las [notas de la versión del SDK](https://azure.github.io/azure-sdk/).

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="connect-and-use-azure-services"></a>Conexión y uso de los servicios de Azure

En el SDK hay varias *bibliotecas cliente* que le ayudan a conectarse a los recursos de Azure existentes y a usarlos en sus aplicaciones; por ejemplo, cargar archivos, acceder a los datos de tablas o trabajar con los distintos servicios de Azure Cognitive Services. Con el SDK, puede trabajar con esos recursos usando los paradigmas de programación con Python ya conocidos en lugar de tener que usar la API REST genérica del servicio.

Por ejemplo, supongamos que desea cargar un blob en una cuenta de Azure Storage que ha aprovisionado previamente. El primer paso es instalar la biblioteca correspondiente:

```bash
pip install azure-storage-blob
```

Después, importe la biblioteca en el código:

```python
from azure.storage.blob import BlobClient
```

Por último, use la API de la biblioteca para conectarse a los datos y cargarlos. En este ejemplo, la cadena de conexión y el nombre del contenedor ya se han aprovisionado en la cuenta de almacenamiento. El nombre del blob es el nombre que asigne a los datos cargados:

```python
blob = BlobClient.from_connection_string("my_connection_string", container="mycontainer", blob="my_blob")

with open("./SampleSource.txt", "rb") as data:
    blob.upload_blob(data)
```

Para más información sobre cómo trabajar con cada biblioteca específica, vea el archivo *README.md* o *README.rst* que se encuentra en la carpeta de proyecto de la biblioteca, en el [repositorio de GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk). Consulte también los [ejemplos de Azure](https://docs.microsoft.com/samples/browse/?languages=python).

También puede encontrar fragmentos de código adicionales en la [documentación de referencia](/python/api?view=azure-python).

### <a name="the-azure-core-library"></a>Biblioteca Core de Azure

Estamos actualizando las bibliotecas de cliente de Python para compartir la funcionalidad básica, como los reintentos, el registro, los protocolos de transporte, los protocolos de autenticación, etc. Esta funcionalidad compartida está contenida en la biblioteca [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core). Para más información sobre la biblioteca y sus instrucciones, consulte [Guía de Python: Introducción](https://azure.github.io/azure-sdk/python_introduction.html).

Las bibliotecas que actualmente funcionan con la biblioteca Core son las siguientes:

- `azure-storage-blob`
- `azure-storage-queue`
- `azure-keyvault-keys`
- `azure-keyvault-secrets`

## <a name="manage-azure-resources"></a>Administración de recursos de Azure

Azure SDK para Python también incluye muchas bibliotecas que le ayudarán a crear, aprovisionar y administrar los recursos de Azure. Nos referimos a ellas como *bibliotecas de administración*. Cada biblioteca de administración se llama `azure-mgmt-<service name>`. Con las bibliotecas de administración, puede escribir código de Python para llevar a cabo las mismas tareas que puede hacer con [Azure Portal](https://portal.azure.com) o la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli).

Por ejemplo, supongamos que desea crear una instancia de SQL Server. En primer lugar, instale la biblioteca de administración de correspondiente:

```bash
pip install azure-mgmt-sql
```

En el código de Python, importe la biblioteca:

```python
from azure.mgmt.sql import SqlManagementClient

```

A continuación, cree el objeto de cliente de administración usando sus credenciales y su identificador de suscripción de Azure:

```python
sql_client = SqlManagementClient(credentials, subscription_id)
```

Por último, use ese objeto de cliente para crear el recurso con un nombre de grupo de recursos, un nombre de servidor, una ubicación y las credenciales de administrador correspondientes:

```python
server = sql_client.servers.create_or_update(
    'myresourcegroup',
    'myservername',
    {
        'location': 'eastus',
        'version': '12.0', # Required for create
        'administrator_login': 'mysecretname', # Required for create
        'administrator_login_password': 'HusH_Sec4et' # Required for create
    }
)
```

Al igual que con las bibliotecas de cliente, encontrará más información sobre cómo trabajar con cada biblioteca de administración en los archivos *README.md* o *README.rst* que se encuentran en la carpeta de proyecto de la biblioteca, en nuestro [repositorio de GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk).

También puede encontrar fragmentos de código adicionales en la [documentación de referencia](/python/api?view=azure-python). 

## <a name="get-help-and-give-feedback"></a>Obtención de ayuda y ofrecimiento de comentarios

- Consulte la [documentación de Azure SDK para Python](https://aka.ms/python-docs).
- Plantee preguntas a la comunidad en [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python).
- Abra incidencias relativas al SDK en [GitHub](https://github.com/Azure/azure-sdk-for-python/issues).
- Envíe un tweet a [@AzureSDK](https://twitter.com/AzureSdk/).
