---
title: Managing Redis Caches using the Azure Explorer for Eclipse (Administración de Redis Cache mediante Azure Explorer para Eclipse)
description: Obtenga información acerca de cómo administrar Azure Redis Cache mediante Azure Explorer para Eclipse.
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: e1c7ac18067cd11a0d878da5d3e464ddd2ad6079
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74811660"
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-eclipse"></a>Managing Redis Caches using the Azure Explorer for Eclipse (Administración de Redis Cache mediante Azure Explorer para Eclipse)

Azure Explorer, que forma parte del kit de herramientas de Azure para Eclipse, proporciona a los desarrolladores de Java una solución fácil de usar para la administración de Redis Cache en su cuenta de Azure desde el IDE de Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-eclipse"></a>Creación de Redis Cache mediante una Eclipse

Los siguientes pasos le guían en la creación de instancias de Redis Cache mediante Azure Explorer.

1. Inicie sesión en su cuenta de Azure siguiendo los pasos del artículo [Instrucciones de inicio de sesión del kit de herramientas de Azure para Eclipse].

1. En la ventana de la herramienta **Azure Explorer**, expanda el nodo **Azure**, haga clic con el botón derecho en **Cachés en Redis** y luego haga clic en **Crear Redis Cache**.

   ![Creación de un menú de Redis Cache][CR01]

1. Cuando aparece el cuadro de diálogo **Nueva caché en Redis**, especifique las opciones siguientes:

   ![Creación del cuadro de diálogo Nueva caché en Redis][CR02]

   a. **Nombre DNS**: especifica el subdominio DNS para el nuevo Redis Cache, que se antepone a ".redis.cache.windows.net", por ejemplo *wingtiptoys.redis.cache.windows.net*.

   b. **Suscripción**: especifica la suscripción de Azure que desea usar en la nueva instancia de Redis Cache.

   c. **Grupo de recursos**: especifica el grupo de recursos para la instancia de Redis Cache. Debe elegir una de las siguientes opciones:
      * **Crear nuevo**: especifica que quiere crear un nuevo grupo de recursos.
      * **Usar existente**: especifica que se va a elegir entre una lista de grupos de recursos asociados a la cuenta de Azure.

   d. **Ubicación**: especifica la ubicación en la que se crea la instancia de Redis Cache, por ejemplo, *Oeste de EE. UU.*

   e. **Plan de tarifa**: especifica el plan de tarifa que usa Redis Cache. Este valor determina el número de conexiones del cliente. (Para más información, consulte [Precios de Redis Cache]).

   f. **Puerto no SSL**: especifica si Redis Cache permite conexiones no SSL. De forma predeterminada, se permiten solo las conexiones SSL.

1. Cuando haya especificado toda la configuración de Redis Cache, haga clic en **Aceptar**.

Una vez creada la instancia de Redis Cache, se mostrará en Azure Explorer.

   ![Redis Cache en Azure Explorer][CR03]

> [!NOTE]
>
> Para más información acerca de cómo configurar Azure Redis Cache, consulte [Configuración de Azure Redis Cache].
>

## <a name="display-the-properties-for-your-redis-cache-in-eclipse"></a>Mostrar las propiedades de Redis Cache en Eclipse

1. En Azure Explorer, haga clic con el botón derecho en Redis Cache y, después, en **Mostrar propiedades**.

   ![Menú contextual de Azure Explorer para mostrar las propiedades de Redis Cache][SP01]

1. Azure Explorer muestra las propiedades de Redis Cache.

   ![Propiedades de caché en Redis][SP02]

## <a name="delete-your-redis-cache-by-using-eclipse"></a>Eliminación de Redis Cache con Eclipse

1. En Azure Explorer, haga clic con el botón derecho en Redis Cache y, después, en **Eliminar**.

   ![Menú contextual de Azure Explorer para eliminar una instancia de Redis Cache][DE01]

1. Cuando se le pida que confirme la eliminación de Redis Cache, haga clic en **Aceptar**.

   ![Eliminar una instancia de Redis Cache][DE02]

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Azure Redis Cache, opciones de configuración y precios, consulte los vínculos siguientes:

* [Azure Redis Cache]
* [Documentación de Redis Cache]
* [Precios de Redis Cache]
* [Configuración de Azure Redis Cache]

[!INCLUDE [azure-toolkit-for-eclipse-additional-resources](../includes/azure-toolkit-for-eclipse-additional-resources.md)]

<!-- URL List -->

[Precios de Redis Cache]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Documentación de Redis Cache]: /azure/redis-cache/
[Configuración de Azure Redis Cache]: /azure/redis-cache/cache-configure

<!-- IMG List -->

[CR01]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: media/azure-toolkit-for-eclipse-managing-redis-caches-using-azure-explorer/DE02.png
