---
title: Notas de la versión de las bibliotecas de administración de Azure para Java | Microsoft Docs
description: Vea las novedades y los cambios importantes en las bibliotecas de administración de Azure para Java
keywords: Azure, Java, API, referencia, notas, actualizaciones, en desuso
ms.assetid: 1f128cf9-f747-4344-84e1-f9964709deb8
ms.topic: reference
ms.date: 3/06/2016
ms.openlocfilehash: 2ef32414106be2ebfe917e1a5b434ba49d5c86ba
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812368"
---
# <a name="release-notes"></a>Notas de la versión 

## <a name="october-5-2017---130"></a>5 de octubre de 2017: 1.3.0 

La versión 1.3.0 es compatible con versiones anteriores de servicios y características que alcanzaron la etapa de disponibilidad general (estable) en versiones anteriores.

Los principales cambios con respecto a las versiones preliminares de esos servicios están marcados con la anotación @Beta.

Si va a migrar código a la versión 1.3.0, puede usar [estas notas](https://github.com/Azure/azure-sdk-for-java/blob/master/notes/prepare-for-1.3.0.md) para preparar el código existente para la versión 1.3.

### <a name="generally-availabile-in-v13"></a>Disponibilidad general en la versión V1.3

Algunas de las API que estaban todavía en la versión beta en versiones anteriores ahora están disponibles de forma general, en concreto:

- métodos asincrónicos
- todos los métodos de la red CDN que anteriormente se encontraban en la versión beta
- todos los métodos y las interfaces de Application Gateway que anteriormente se encontraban en la versión beta

  Algunas partes de la biblioteca aún están en versión preliminar. Consulte la tabla siguiente sobre el estado actual de las bibliotecas:

Servicio o característica | Disponible de forma general | Disponible como versión preliminar 
---------|---------|---------|-
Proceso  | Máquinas virtuales y extensiones de máquinas virtuales, conjuntos de escalado de máquinas virtuales, discos administrados   | Azure container service, Azure container registry 
Storage   |  Cuentas de almacenamiento       |    Cifrado     
SQL Database  | Bases de datos, firewalls, grupos elásticos              
Redes    |  Redes virtuales, interfaces de red, direcciones IP, tablas de enrutamiento, grupos de seguridad de red, DNS, administradores de tráfico, puertas de enlace de aplicaciones  |    Equilibradores de carga, emparejamiento de redes, puerta de enlace de red virtual, Network Watcher 
Más servicios    |  Resource Manager, Key Vault, Redis, CDN, Batch       |  Web Apps, Function App, Service Bus, Graph RBAC, Cosmos DB, Search  
Aspectos básicos     |   Autenticación: principal, métodos asincrónicos, identidad de servicio administrada      |      |

> Las características de la versión preliminar están marcadas con una anotación `@Beta` en el nivel de clase o interfaz o método de las bibliotecas. Estas características están sujetas a cambios. Puede que se modifiquen de cualquier manera (o incluso se eliminen) en el futuro.

### <a name="import-with-maven"></a>Importación con Maven

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure</artifactId>
    <version>1.3.0</version>
</dependency>
```

### <a name="get-help-and-give-feedback"></a>Obtención de ayuda y ofrecimiento de comentarios

Revisar la comunidad de [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-java-sdk) para obtener ayuda con las bibliotecas en su propio código. Si encuentra errores o tiene sugerencias para mejorar estas bibliotecas, puede enviarlas a través de [GitHub](https://github.com/Azure/azure-sdk-for-java/issues).


