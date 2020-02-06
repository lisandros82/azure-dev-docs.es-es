---
ms.openlocfilehash: 8f757c030849cb89eea36d74b55867dcc2ff98a6
ms.sourcegitcommit: 6fa28ea675ae17ffb9ac825415e2e26a3dfe7107
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "77013740"
---
Puede eliminar el grupo de recursos con [Azure Portal](https://portal.azure.com) o la CLI de Azure:

- En el portal, seleccione **Grupos de recursos** en el panel de navegación izquierdo, seleccione el grupo de recursos que se creó en el proceso de este tutorial y, a continuación, use el comando **Eliminar grupo de recursos**.

- Ejecute el siguiente comando de la CLI de Azure (localmente o con [Cloud Shell](/azure/cloud-shell/overview)), reemplazando `<resource_group>` por el nombre del grupo que se usa en este tutorial:

    ```azurecli
    az group delete --name <resource_group>
    ```
