---
title: Managed Disks
description: Cree, cambie el tamaño y actualice un disco administrado.
ms.topic: conceptual
ms.date: 6/15/2017
ms.openlocfilehash: c65e07dc4a56ef0376785df4f55d3a9fc9f129ac
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467023"
---
# <a name="managed-disks"></a>Managed Disks

Azure Managed Disks proporciona una administración de discos simplificada, mejor escalabilidad y mayor seguridad. Elimina la noción de cuenta de almacenamiento para los discos, lo que permite a los clientes escalar sin tener que preocuparse por las limitaciones asociadas con las cuentas de almacenamiento. Esta publicación proporciona una introducción rápida y referencia para utilizar el servicio desde Python.

Desde la perspectiva del desarrollador, la experiencia con los discos administrados en la CLI de Azure dependerá de la experiencia con la CLI en otras herramientas multiplataforma. Puede usar [Azure SDK para Python](https://azure.microsoft.com/develop/python/) y el [paquete azure-mgmt-compute 0.33.0](https://pypi.python.org/pypi/azure-mgmt-compute) para administrar discos administrados. Puede crear un cliente de proceso mediante este [tutorial](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python).

## <a name="standalone-managed-disks"></a>Discos administrados independientes

Hay varias formas de crear discos administrados independientes fácilmente.

### <a name="create-an-empty-managed-disk"></a>Crear un disco administrado vacío

```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'disk_size_gb': 20,
        'creation_data': {
            'create_option': DiskCreateOption.empty
        }
    }
)
disk_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-blob-storage"></a>Crear un disco administrado a partir de Blob Storage

```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.import_enum,
            'source_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd'
        }
    }
)
disk_resource = async_creation.result()
```

### <a name="create-an-image-from-blob-storage"></a>Crear una imagen a partir de un almacenamiento de blobs

```python
from azure.mgmt.compute.models import DiskCreateOption

async_creation = compute_client.images.create_or_update(
    'my_resource_group',
    'my_image_name',
    {
        'location': 'eastus',
        'storage_profile': {
           'os_disk': {
              'os_type': 'Linux',
              'os_state': "Generalized",
              'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
              'caching': "ReadWrite",
           }
        }        
    }
)
image_resource = async_creation.result()
```

### <a name="create-a-managed-disk-from-your-own-image"></a>Crear un disco administrado a partir de una imagen de su propiedad

```python
from azure.mgmt.compute.models import DiskCreateOption

# If you don't know the id, do a 'get' like this to obtain it
managed_disk = compute_client.disks.get(self.group_name, 'myImageDisk')
async_creation = compute_client.disks.create_or_update(
    'my_resource_group',
    'my_disk_name',
    {
        'location': 'eastus',
        'creation_data': {
            'create_option': DiskCreateOption.copy,
            'source_resource_id': managed_disk.id
        }
    }
)

disk_resource = async_creation.result()
```

## <a name="virtual-machine-with-managed-disks"></a>Máquina virtual con discos administrados

Puede crear una máquina virtual con un disco administrado implícito para una imagen de disco específica. La creación se simplifica con la creación implícita de discos administrados sin especificar todos los detalles del disco. No tiene que preocuparse por crear y administrar cuentas de almacenamiento.

Un disco administrado se crea implícitamente al crear la máquina virtual a partir de una imagen de sistema operativo en Azure. En el parámetro ``storage_profile``, ``os_disk`` ahora es opcional y no es necesario crear una cuenta de almacenamiento como condición previa necesaria para crear una máquina virtual.

```python
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        publisher='Canonical',
        offer='UbuntuServer',
        sku='16.04-LTS',
        version='latest'
    )
)
```

Este parámetro ``storage_profile`` ahora es válido. Para ve un ejemplo completo de cómo crear una máquina virtual en Python (incluida la red, etc.), consulte el [tutorial completo sobre máquinas virtuales en Python](https://github.com/Azure-Samples/virtual-machines-python-manage).

También puede crear una ``storage_profile`` con sus propias imágenes.

```python
# If you don't know the id, do a 'get' like this to obtain it
image = compute_client.images.get(self.group_name, 'myImageDisk')
storage_profile = azure.mgmt.compute.models.StorageProfile(
    image_reference = azure.mgmt.compute.models.ImageReference(
        id = image.id
    )
)
```

Puede asociar fácilmente un disco administrado aprovisionado anteriormente.

```python
vm = compute.virtual_machines.get(
    'my_resource_group',
    'my_vm'
)
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
vm.storage_profile.data_disks.append({
    'lun': 12, # You choose the value, depending of what is available for you
    'name': managed_disk.name,
    'create_option': DiskCreateOptionTypes.attach,
    'managed_disk': {
        'id': managed_disk.id
    }
})
async_update = compute_client.virtual_machines.create_or_update(
    'my_resource_group',
    vm.name,
    vm,
)
async_update.wait()
```

## <a name="virtual-machine-scale-sets-with-managed-disks"></a>Conjuntos de escalado de máquinas virtuales con discos administrados

Antes de los discos administrados, había que crear manualmente una cuenta de almacenamiento para todas las máquinas virtuales que quería dentro del conjunto de escalado y, a continuación, usar el parámetro de lista ``vhd_containers`` para proporcionar el nombre de todas las cuentas de almacenamiento a la API de REST del conjunto de escalado. La guía de transición oficial está disponible en este artículo `<https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-convert-template-to-md>`.

Ahora, con los discos administrados, no es necesario administrar ninguna cuenta de almacenamiento. Si usa el SDK de VMSS para Python, ``storage_profile`` ahora puede ser exactamente el mismo perfil de almacenamiento que usó al crear las máquinas virtuales:

```python
'storage_profile': {
    'image_reference': {
        "publisher": "Canonical",
        "offer": "UbuntuServer",
        "sku": "16.04-LTS",
        "version": "latest"
    }
},
```

El ejemplo completo es este:

```python
naming_infix = "PyTestInfix"
vmss_parameters = {
    'location': self.region,
    "overprovision": True,
    "upgrade_policy": {
        "mode": "Manual"
    },
    'sku': {
        'name': 'Standard_A1',
        'tier': 'Standard',
        'capacity': 5
    },
    'virtual_machine_profile': {
        'storage_profile': {
            'image_reference': {
                "publisher": "Canonical",
                "offer": "UbuntuServer",
                "sku": "16.04-LTS",
                "version": "latest"
            }
        },
        'os_profile': {
            'computer_name_prefix': naming_infix,
            'admin_username': 'Foo12',
            'admin_password': 'BaR@123!!!!',
        },
        'network_profile': {
            'network_interface_configurations' : [{
                'name': naming_infix + 'nic',
                "primary": True,
                'ip_configurations': [{
                    'name': naming_infix + 'ipconfig',
                    'subnet': {
                        'id': subnet.id
                    } 
                }]
            }]
        }
    }
}

# Create VMSS test
result_create = compute_client.virtual_machine_scale_sets.create_or_update(
    'my_resource_group',
    'my_scale_set',
    vmss_parameters,
)
vmss_result = result_create.result()
```

## <a name="other-operations-with-managed-disks"></a>Otras operaciones con discos administrados

### <a name="resizing-a-managed-disk"></a>Cambio del tamaño de un disco administrado

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.disk_size_gb = 25
async_update = self.compute_client.disks.create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="update-the-storage-account-type-of-the-managed-disks"></a>Actualización del tipo de cuenta de almacenamiento de los discos administrados

```python
from azure.mgmt.compute.models import StorageAccountTypes

managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
managed_disk.account_type = StorageAccountTypes.standard_lrs
async_update = self.compute_client.disks.create_or_update(
    'my_resource_group',
    'myDisk',
    managed_disk
)
async_update.wait()
```

### <a name="create-an-image-from-nlob-storage"></a>Creación de una imagen a partir de Blob Storage

```python
async_create_image = compute_client.images.create_or_update(
    'my_resource_group',
    'myImage',
    {
        'location': 'westus',
        'storage_profile': {
            'os_disk': {
                'os_type': 'Linux',
                'os_state': "Generalized",
                'blob_uri': 'https://bg09.blob.core.windows.net/vm-images/non-existent.vhd',
                'caching': "ReadWrite",
            }
        }
    }
)
image = async_create_image.result()
```

### <a name="create-a-snapshot-of-a-managed-disk-that-is-currently-attached-to-a-virtual-machine"></a>Creación de una instantánea de un disco administrado que esté asociado actualmente a una máquina virtual

```python
managed_disk = compute_client.disks.get('my_resource_group', 'myDisk')
async_snapshot_creation = self.compute_client.snapshots.create_or_update(
        'my_resource_group',
        'mySnapshot',
        {
            'location': 'westus',
            'creation_data': {
                'create_option': 'Copy',
                'source_uri': managed_disk.id
            }
        }
    )
snapshot = async_snapshot_creation.result()
```
