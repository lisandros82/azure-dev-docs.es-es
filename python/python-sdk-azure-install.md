---
title: Instalación de Azure SDK para Python
description: Cómo instalar Azure SDK para Python con PIP o GitHub. Azure SDK se puede instalar como bibliotecas individuales o como un paquete completo.
ms.date: 10/31/2019
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: 39de0959f3d73306412c39b32a4e13766d1500e9
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466403"
---
# <a name="install-the-azure-sdk-for-python"></a>Instalación de Azure SDK para Python

Azure SDK para Python proporciona una API a través de la cual puede interactuar con Azure desde el código Python. Puede instalar las bibliotecas individuales del SDK que necesite, o bien puede instalar el conjunto completo de bibliotecas.

Azure SDK para Python se ha probado y es compatible con las versiones 2.7 y 3.5.3 y posteriores de CPython, y con PyPy 5.4 y versiones posteriores. Los desarrolladores también usan el SDK con otros intérpretes como IronPython y Jython, pero podría tener problemas aislados e incompatibilidades. Si necesita un intérprete de Python, instale la versión más reciente desde [python.org/downloads](https://www.python.org/downloads).

## <a name="install-sdk-libraries-using-pip"></a>Instalación de bibliotecas del SDK con PIP

Azure SDK para Python se compone de una serie de bibliotecas individuales que aprovisionan o funcionan con servicios específicos de Azure. Puede instalar cada una de ellos mediante `pip install <library>` con los nombres que se muestran en la [lista de bibliotecas del SDK](https://github.com/Azure/azure-sdk-for-python/blob/master/packages.md). (Esta lista proporciona vínculos a archivos Léame útiles para cada biblioteca).

Por ejemplo, si utiliza Azure Storage, puede instalar la biblioteca `azure-storage-file`, `azure-storage-blob` o `azure-storage-queue`. Si usa tablas de Azure Cosmos DB, instale `azure-cosmosdb-table`. Azure Functions se admite mediante la biblioteca `azure-functions`, y así sucesivamente. Las bibliotecas que comienzan por `azure-mgmt-` proporcionan la API para aprovisionar los recursos de Azure.

### <a name="install-specific-library-versions"></a>Instalación de versiones de biblioteca específicas

Si necesita instalar una versión específica de una biblioteca, especifique la versión en la línea de comandos:

```bash
pip install azure-storage-blob==12.0.0
```

### <a name="install-preview-packages"></a>Instalación de paquetes en versión preliminar

Microsoft publica regularmente versiones preliminares de las bibliotecas del SDK, que admiten las próximas características. Para instalar la versión preliminar más reciente de una biblioteca, incluya la marca `--pre` en la línea de comandos. 

```bash
# Install all preview versions of the Azure SDK for Python
pip install --pre azure

# Install the preview version for azure-storage-blob only.
pip install --pre azure-storage-blob
```

## <a name="verify-sdk-installation-details-with-pip"></a>Comprobación de la información de la instalación del SDK con PIP

Use el comando `pip show <library>` para comprobar que se ha instalado una biblioteca. Si la biblioteca está instalada, el comando muestra la versión y un resumen con otra información. Si la biblioteca no está instalada, el comando no muestra nada.

```bash
# Check installation of the Azure SDK for Python
pip show azure

# Check installation of a specific library
pip show azure-storage-blob
```

También puede usar `pip freeze` o `pip list` para ver todas las bibliotecas instaladas en el entorno de Python actual.

## <a name="uninstall-azure-sdk-for-python-libraries"></a>Desinstalación de las bibliotecas de Azure SDK para Python

Para desinstalar una biblioteca individual, use `pip uninstall <library>`.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprenda a utilizar el SDK](python-sdk-azure-get-started.yml)
