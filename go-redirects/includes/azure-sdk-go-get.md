---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: a51c8667c74a2611ae8769aa42fd1a94f9253bc8
ms.sourcegitcommit: 4eee7d9a484e35eb695248c011a63b27603d354b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69642949"
---
[Azure SDK para Go](https://github.com/Azure/azure-sdk-for-go) es compatible con las versiones de Go 1.8 y posteriores. Para entornos con [perfiles de Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-go), la versión 1.9 de Go es el requisito mínimo.
Si tiene que instalar Go, siga [las instrucciones de instalación de Go](https://golang.org/doc/install).

Puede descargar Azure SDK para Go y sus dependencias en `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Asegúrese de escribir en mayúsculas `Azure` en la dirección URL. De lo contrario puede causar problemas de importación relacionados con las mayúsculas y minúsculas cuando se trabaja con el SDK. También debe escribir en mayúscula `Azure` en las instrucciones de importación.
