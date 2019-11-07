---
title: Autenticación con los módulos de administración de Azure para Node.js
description: Autenticación con una entidad de servicio en los módulos de administración de Azure para Node.js
author: kraigb
manager: barbkess
ms.author: kraigb
ms.date: 06/17/2017
ms.topic: article
ms.prod: azure
ms.devlang: nodejs
ms.openlocfilehash: 87a30973c8a295540924e41aee9c8e0af455b41f
ms.sourcegitcommit: 380300c283f3df8a87c7c02635eae3596732fb72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73661236"
---
# <a name="authenticate-with-the-azure-modules-for-nodejs"></a>Autenticación con los módulos de Azure para Node.js 

Todas las API de servicio requieren autenticación a través de un objeto `credentials` cuando se crea una instancia de él. Hay tres maneras de autenticar y crear las credenciales necesarias mediante el SDK de Azure para Node.js: 

- Autenticación básica
- Inicio de sesión interactivo
- Autenticación de entidad de servicio

## <a name="basic-authentication"></a>Autenticación básica

Para autenticar mediante programación mediante sus credenciales de la cuenta de Azure, use la función `loginWithUsernamePassword`. El siguiente fragmento de código de JavaScript muestra cómo utilizar la autenticación básica con credenciales almacenadas como variables de entorno. 

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.loginWithUsernamePassword(process.env.AZURE_USER, 
                                 process.env.AZURE_PASS, 
                                 (err, credentials) => {
  if (err) throw err;

  let storageClient = Azure.createARMStorageManagementClient(credentials, 
                                                             '<azure-subscription-id>');

  // ..use the client instance to manage service resources.
});
```

## <a name="interactive-login"></a>Inicio de sesión interactivo

El inicio de sesión interactivo proporciona un vínculo y un código que permita a los usuarios autenticarse desde un explorador. Utilice este método cuando el mismo script utiliza varias cuentas o cuando se prefiere la intervención del usuario.

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.interactiveLogin((err, credentials) => {
  if (err) throw err;

  let storageClient = Azure.createARMStorageManagementClient(credentials, '<azure-subscription-id>');

  // ..use the client instance to manage service resources.
});
```

## <a name="service-principal-authentication"></a>Autenticación de entidad de servicio

El [inicio de sesión interactivo](#interactive-login) es la manera más sencilla de autenticarse. Sin embargo, al usar el SDK de Node.js, puede que desee utilizar la autenticación de entidad de servicio en lugar de proporcionar sus credenciales de cuenta. En el tema [Creación de una entidad de servicio de Azure con Node.js](./node-sdk-azure-authenticate-principal.md) se explican varias técnicas para crear (y utilizar) una entidad de servicio. 