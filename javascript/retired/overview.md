---
title: Módulos de Azure para JavaScript
description: Introducción a los módulos de servicio y de administración de Azure para JavaScript
ms.date: 06/17/2017
ms.topic: article
ms.openlocfilehash: 54405e8406dffec7333ef22d5f837de0db9037b5
ms.sourcegitcommit: 4cf22356d6d4817421b551bd53fcba76bdb44cc1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76872120"
---
# <a name="azure-modules-for-javascript"></a>Módulos de Azure para JavaScript

Administre los recursos de Azure y conecte con los servicios desde sus aplicaciones JavaScript con los módulos de Azure para JavaScript. El código está disponible como [módulos npm](../node-sdk-azure-install.md) para su uso en los proyectos. 

## <a name="manage-azure-resources"></a>Administración de recursos de Azure

Los módulos de administración se utilizan para crear y consultar los recursos de las aplicaciones o para crear las propias herramientas de automatización de Azure. 

Por ejemplo, para crear una máquina virtual Linux usando una interfaz de red existente, debe escribir el código siguiente:

```javascript
const msRestAzure = require('ms-rest-azure');
const ComputeManagementClient = require('azure-arm-compute');

// read in service principal values from env variables
const clientId = process.env['CLIENT_ID'];
const domain = process.env['DOMAIN'];
const secret = process.env['APPLICATION_SECRET'];
const subscriptionId = process.env['AZURE_SUBSCRIPTION_ID'];

msRestAzure.loginWithServicePrincipalSecret(clientId, secret, domain, function (err, credentials, subscriptions) {
    if (err) return console.log(err);
    const computeClient = new ComputeManagementClient(credentials, subscriptionId);
    // customize the VM 
    const vmParameters = {
        location: "eastus",
        osProfile: {
            computerName: "newLinuxVM",
            adminUsername: adminUsername,
            adminPassword: adminPassword
        },
        linuxConfiguration: {
            ssh: {
                publicKeys: [mySshKey]
            }
        },
        hardwareProfile: {
            vmSize: 'Basic_A1'
        },
        networkProfile: {
            networkInterfaces: [
                {
                    id: myNetworkInterfaceId,
                    primary: true
                }
            ]
        },
        storageProfile: {
            imageReference: {
                publisher: 'Canonical',
                offer: 'UbuntuServer',
                sku: '16.04-LTS',
                version: 'latest'
            },
        }
    };
 
    // create the VM
    computeClient.virtualMachines.createOrUpdate("myResourceGroup", "newLinuxVM", vmParameters, function (err, data) {
        if (err) return console.log(err);
    });

});
```

Revise las [instrucciones de instalación](../node-sdk-azure-install.md) para obtener una lista completa de los módulos y el [artículo de introducción](../node-sdk-azure-get-started.md) para configurar la autenticación y ejecutar código de ejemplo para crear y actualizar los recursos en su propia suscripción de Azure. 

## <a name="connect-to-azure-services"></a>Conexión a los servicios de Azure

Además de utilizar los módulos de Azure para crear y administrar recursos en Azure, también puede utilizar paquetes para conectarse a los servicios en la nube de Azure en las aplicaciones y a utilizarlos. Por ejemplo, puede actualizar una tabla de SQL Database o cargar archivos en Azure Storage. Seleccione el paquete que necesita para un servicio determinado en la [lista completa](../node-sdk-azure-install.md) y visite el [Centro para desarrolladores de JavaScript](https://azure.microsoft.com/develop/nodejs/) para ver tutoriales y código de ejemplo para aprender a usar los módulos en las aplicaciones.

Por ejemplo, para imprimir el contenido de cada blob en un contenedor de Azure Storage:

```javascript
var azure = require('azure-storage');
var blobService = azure.createBlobService(storageConnectionString);
blobService.listBlobsSegmented('testcontainer', null, function(error, result, response) {
   if (err) return console.log(err);
   console.log(result);
});
```

## <a name="sample-code-and-reference"></a>Código de ejemplo y referencia

Los ejemplos siguientes incluyen las tareas comunes con los módulos de administración de Azure y tienen código listo para usar en sus propias aplicaciones:

- [Máquinas virtuales](../node-samples-services-compute.md)
- [Aplicaciones web](../node-samples-services-web-and-mobile.md)
- [SQL Database](../node-samples-services-database.md)
   
Hay disponible una [referencia](/javascript/api) para todos los módulos tanto de los módulos de servicio como de las bibliotecas. Las nuevas características, cambios importantes e instrucciones de migración desde versiones anteriores están disponibles en las [notas de la versión](https://github.com/Azure/azure-sdk-for-node/releases).
