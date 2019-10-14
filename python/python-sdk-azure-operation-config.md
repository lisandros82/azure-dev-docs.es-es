---
title: 'Parámetros para la configuración de la operación: SDK de Azure para Python'
description: C iniciado por el SDK de Azure para Python
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/07/2018
ms.topic: conceptual
ms.devlang: python
ms.custom: seo-python-october2019
ms.openlocfilehash: ca69b72789f28445c4654e635e641e2954890a38
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172371"
---
# <a name="parameters-for-operation-configuration"></a>Parámetros para la configuración de las operaciones

Los métodos en operaciones tienen parámetros adicionales que se pueden proporcionar en elementos `kwargs`. Esto se denomina operation_config.

Las opciones de configuración de operaciones son:

|Nombre de parámetro|type|Role|
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
