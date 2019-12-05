---
title: 'Parámetros para la configuración de la operación: SDK de Azure para Python'
description: C iniciado por el SDK de Azure para Python
ms.date: 03/07/2018
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: b0aaaf5bcd51bce42ab38830d5dbce508db226b1
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466339"
---
# <a name="parameters-for-operation-configuration"></a>Parámetros para la configuración de las operaciones

Puede proporcionar parámetros adicionales para los métodos de las operaciones en Azure SDK para Python.

Se proporcionan parámetros adicionales en `kwargs`. Esta funcionalidad se llama *operation_config*.

Las opciones de configuración de operaciones son:

|Nombre de parámetro|Tipo|Role|
|----------------------|------|---------------|
| Comprobación |`bool`|Si se debe comprobar el certificado SSL. El valor predeterminado es True.|
|  cert |`str`| Ruta de acceso al certificado local para la comprobación del lado cliente.|
|  timeout |`int`| Tiempo de espera para establecer una conexión de servidor, en segundos.|
|  allow_redirects |`bool` | Si se permiten redirecciones.|
|  max_redirects  |`int`| Número máximo de redirecciones permitido.|
|  proxies  |`dict` |Configuración del servidor proxy.|
|  use_env_proxies |`bool` |Si se debe leer la configuración del servidor proxy de las variables de entorno locales.|
|  retries  |`int` | Número total de reintentos.|
|  enable_http_logger | `bool`| Habilitar los registros de HTTP en modo de depuración (False de forma predeterminada).|
