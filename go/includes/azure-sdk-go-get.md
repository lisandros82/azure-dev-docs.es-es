---
ms.date: 09/05/2018
ms.technology: azure-cli
ms.openlocfilehash: af03f2efbb74e55dfcd14b6a2ac894a74eba321f
ms.sourcegitcommit: 4cf22356d6d4817421b551bd53fcba76bdb44cc1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76871897"
---
[Azure SDK para Go](https://github.com/Azure/azure-sdk-for-go) es compatible con las versiones de Go 1.8 y posteriores. Para entornos con [perfiles de Azure Stack](/azure/azure-stack/user/azure-stack-version-profiles-go), la versión 1.9 de Go es el requisito mínimo.
Si tiene que instalar Go, siga [las instrucciones de instalación de Go](https://golang.org/doc/install).

Puede descargar Azure SDK para Go y sus dependencias en `go get`.

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> Asegúrese de escribir en mayúsculas `Azure` en la dirección URL. De lo contrario puede causar problemas de importación relacionados con las mayúsculas y minúsculas cuando se trabaja con el SDK. También debe escribir en mayúscula `Azure` en las instrucciones de importación.
