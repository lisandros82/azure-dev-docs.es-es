---
title: Administración de cuentas de almacenamiento con Azure Explorer para Eclipse
description: Obtenga información sobre cómo administrar las cuentas de almacenamiento de Azure mediante Azure Explorer para Eclipse.
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 2eee5bf3546712c5f43056a9d4a2b170951ee810
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74811088"
---
# <a name="manage-storage-accounts-by-using-the-azure-explorer-for-eclipse"></a>Administración de cuentas de almacenamiento mediante Azure Explorer para Eclipse

Azure Explorer, que forma parte del kit de herramientas de Azure para Eclipse, proporciona a los desarrolladores de Java una solución fácil de usar para la administración de cuentas de almacenamiento en su cuenta de Azure desde el entorno de desarrollo integrado de Eclipse (IDE).

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-storage-account-in-eclipse"></a>Creación de una cuenta de almacenamiento en Eclipse

Para crear una cuenta de almacenamiento con Azure Explorer, haga lo siguiente:

1. Inicie sesión en su cuenta de Azure siguiendo los pasos del artículo [Instrucciones de inicio de sesión del kit de herramientas de Azure para Eclipse](https://docs.microsoft.com/azure/java/eclipse/azure-toolkit-for-eclipse-sign-in-instructions).

1. En la vista de **Azure Explorer**, expanda el nodo **Azure**, haga clic con el botón derecho en **Cuentas de almacenamiento** y, después, haga clic en **Crear cuenta de almacenamiento**.

   ![Comando Crear cuenta de almacenamiento][CS01]

1. En el cuadro de diálogo**Crear cuenta de almacenamiento**, especifique las siguientes opciones:

   ![Cuadro de diálogo Crear nueva cuenta de almacenamiento][CS02]

   * **Nombre**: especifica el nombre para la nueva cuenta de almacenamiento.

   * **Suscripción**: especifica la suscripción de Azure que quiere usar para la nueva cuenta de almacenamiento.

   * **Grupo de recursos**: especifica el grupo de recursos para la máquina virtual. Seleccione una de las siguientes opciones:
      * **Crear nuevo**: especifica que quiere crear un nuevo grupo de recursos.
      * **Usar existente**: especifica que se va a elegir de una lista de grupos de recursos asociados a la cuenta de Azure.

   * **Región**: especifica la ubicación donde se creará la cuenta de almacenamiento, por ejemplo, "Oeste de EE. UU.".

   * **Tipo de cuenta**: especifica el tipo de cuenta de almacenamiento que se va a crear, por ejemplo "Blob Storage". Para más información, consulte [Acerca de las cuentas de almacenamiento de Azure].

   * **Rendimiento**: especifica la oferta de cuenta de almacenamiento que quiere usar del publicador seleccionado; por ejemplo, "Premium". Para obtener más información, consulte [Objetivos de escalabilidad y rendimiento de Azure Storage].

   * **Replication** (Replicación): especifica la replicación para la cuenta de almacenamiento; por ejemplo, en "Con redundancia de zona". Para más información, consulte el artículo sobre la [replicación de Azure Storage].

1. Cuando haya especificado todas las opciones anteriores, haga clic en **Crear**.

## <a name="create-a-storage-container-in-eclipse"></a>Creación de un contenedor de almacenamiento en Eclipse

Para crear un contenedor de almacenamiento con Azure Explorer, haga lo siguiente:

1. En la vista de **Azure Explorer**, haga clic con el botón derecho en la cuenta de almacenamiento donde quiera crear un contenedor y, después, haga clic en **Crear contenedor de blobs**.

   ![Comando Crear contenedor de blobs][CC01]

1. En el cuadro de diálogo **Crear contenedor de blobs**, especifique el nombre para el contenedor y, después, haga clic en **Aceptar**. Para más información sobre la nomenclatura de contenedores de almacenamiento, consulte [Asignar nombres y hacer referencia a contenedores, blobs y metadatos].

   ![Cuadro de diálogo Crear contenedor de blobs][CC02]

## <a name="delete-a-storage-container-in-eclipse"></a>Eliminación de un contenedor de almacenamiento en Eclipse

Para eliminar un contenedor de almacenamiento mediante Azure Explorer, siga estos pasos:

1. En la vista de **Azure Explorer**, haga clic con el botón derecho en el contenedor de almacenamiento y, después, haga clic en **Eliminar**.

   ![Comando Eliminar contenedor de almacenamiento][DC01]

1. En la ventana confirmación, haga clic en **Aceptar**.

   ![Ventana de confirmación Eliminar contenedor de almacenamiento][DC02]

## <a name="delete-a-storage-account-in-eclipse"></a>Eliminación de una cuenta de almacenamiento en Eclipse

Para eliminar una cuenta de almacenamiento con Azure Explorer, haga lo siguiente:

1. En la vista de **Azure Explorer**, haga clic con el botón derecho en la cuenta de almacenamiento y haga clic en **Eliminar**.

   ![Comando Eliminar cuenta de almacenamiento][DS01]

1. En la ventana confirmación, haga clic en **Aceptar**.

   ![Ventana de confirmación Eliminar cuenta de almacenamiento][DS02]

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las cuentas de Azure Storage, tamaños y precios, vea los siguientes recursos:

* [Introducción a Microsoft Azure Storage]
* [Acerca de las cuentas de almacenamiento de Azure]
* Tamaños de cuentas de Azure Storage
  * [Tamaños de las cuentas de almacenamiento de Windows en Azure]
  * [Tamaños de las cuentas de almacenamiento de Linux en Azure]
* Precios de las cuentas de Azure Storage
  * [Precios de cuentas de almacenamiento de Windows]
  * [Precios de cuentas de almacenamiento de Linux]

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Introducción a Microsoft Azure Storage]: /azure/storage/storage-introduction
[Acerca de las cuentas de almacenamiento de Azure]: /azure/storage/storage-create-storage-account
[replicación de Azure Storage]: /azure/storage/storage-redundancy
[Objetivos de escalabilidad y rendimiento de Azure Storage]: /azure/storage/storage-scalability-targets
[Asignar nombres y hacer referencia a contenedores, blobs y metadatos]: https://go.microsoft.com/fwlink/?LinkId=255555

[Tamaños de las cuentas de almacenamiento de Windows en Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Tamaños de las cuentas de almacenamiento de Linux en Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Precios de cuentas de almacenamiento de Windows]: https://azure.microsoft.com/pricing/details/virtual-machines/windows/
[Precios de cuentas de almacenamiento de Linux]: https://azure.microsoft.com/pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC02.png
