---
ms.openlocfilehash: 2f54e918e7126123ada68b696ea96f4d5f3dbb83
ms.sourcegitcommit: a8073315f751631ab983618fa9f812eb95d8b2dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125247"
---
Puede eliminar el grupo de recursos con [Azure Portal](https://portal.azure.com) o la CLI de Azure:

- En el portal, seleccione **Grupos de recursos** en el panel de navegación izquierdo, seleccione el grupo de recursos que se creó en el proceso de este tutorial y, a continuación, use el comando **Eliminar grupo de recursos**.

- Ejecute el siguiente comando de la CLI de Azure (localmente o con [Cloud Shell](/cloud-shell/overview)), reemplazando `<resource_group>` por el nombre del grupo que se usa en este tutorial:

    ```azurecli
    az group delete --name <resource_group>
    ```
