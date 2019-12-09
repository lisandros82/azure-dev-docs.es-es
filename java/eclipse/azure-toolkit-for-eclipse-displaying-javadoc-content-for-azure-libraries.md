---
title: Presentación del contenido de Javadoc en Eclipse
titleSuffix: Azure Libraries for Java
description: Cómo mostrar el contenido de Javadoc para las bibliotecas de Azure en Eclipse
documentationcenter: java
ms.assetid: 30f8b6a1-1d76-4d1c-861b-1db478c46e6b
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: ecd491031522fdf2a9e06ff4104f7191f4efc59e
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74811550"
---
# <a name="displaying-javadoc-content-in-eclipse-for-the-azure-libraries-package-for-java"></a>Visualización del contenido de Javadoc en Eclipse para el paquete de bibliotecas de Azure para Java

El contenido de Javadoc de las bibliotecas de Azure para Java se puede ver en el entorno de Eclipse asociando el contenido de Javadoc a las bibliotecas de Azure para Java. En los pasos siguientes se muestra cómo usar esta funcionalidad en Eclipse.

Este procedimiento supone que ya ha agregado la biblioteca de Azure para Java a su ruta de acceso de compilación.

## <a name="to-display-javadoc-content-in-eclipse-for-the-azure-libraries-for-java"></a>Para mostrar el contenido de Javadoc en Eclipse para las bibliotecas de Azure para Java

1. En el Explorador de proyectos de Eclipse, en la sección del proyecto **Bibliotecas de referencia** , abra el menú contextual de la biblioteca de Azure para JAR de Java. Por ejemplo, **microsoft-windowsazure-api-0.1.0.jar** (el número de versión puede ser diferente, en función de la versión que haya instalado).

1. Haga clic en **Propiedades**.

1. En el cuadro de diálogo **Properties** (Propiedades), en el panel izquierdo, haga clic en **Javadoc Location** (Ubicación de Javadoc). Aparecerá el cuadro de diálogo **Ubicación de Javadoc** .

1. Puede especificar una **Javadoc URL** (URL de Javadoc) o un **Javadoc in archive** (Javadoc en archivo).

   * Si opta por especificar una **dirección URL de Javadoc**, use direcciones URL como **https://dl.windowsazure.com/javadoc** o **https://dl.windowsazure.com/storage/javadoc** .

   * Si decide usar **Javadoc en archivo**, puede especificar un archivo externo o un archivo de área de trabajo.

   Realice su elección y examine o valide según sea necesario. En el ejemplo siguiente se asocian las bibliotecas de Azure para Java con el JAR de Javadoc correspondiente que se ha descargado localmente en una carpeta denominada **c:\MyAzureJARs**.

   ![Muestre el contenido de Javadoc.][ic553487]

1. *Paso opcional*: Haga clic en **Validar**. Aquí podrían mostrarse posibles problemas con el JAR de Javadoc.

1. Haga clic en **OK**.

Una vez asociado a la biblioteca, el contenido de Javadoc debe mostrarse dentro del IDE de Eclipse. Por ejemplo, si `blob` se define del tipo `CloudBlockBlob` dentro de su código, el siguiente es un ejemplo del contenido de Javadoc que aparece cuando escribe `blob.acquireLease` en el código:

![Muestre el contenido de Javadoc.][ic553488]

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh698319.aspx -->

<!-- IMG List -->

[ic553487]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553487.png
[ic553488]: media/azure-toolkit-for-eclipse-displaying-javadoc-content-for-azure-libraries/ic553488.png
