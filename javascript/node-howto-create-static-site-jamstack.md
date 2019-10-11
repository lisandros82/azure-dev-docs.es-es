---
title: Creación de sitios estáticos en Azure con Node.js, API y marcado
description: Cómo usar Azure para compilar una aplicación JAMstack (JavaScript, API y marcado)
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/20/2019
ms.author: kraigb
ms.custom: seo-javascript-september2019
ms.openlocfilehash: 5fae0fb9e7d76d33e39ec85a27c46c339b4b38f4
ms.sourcegitcommit: 945e92dae2fa4521eebdc049c65273ae6b5470ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813706"
---
# <a name="build-jamstack-static-site-web-apps-on-azure-with-nodejs"></a>Compilación de aplicaciones web de JAMstack (sitio estático) en Azure con Node.js

Unas excelentes aplicaciones web se pueden compilar y mantener de forma productiva mediante una combinación de front-end *JavaScript*, *API* (API de terceros o personalizadas compiladas como código sin servidor) y *marcado* (HTML y CSS) basado en modelo que se proporciona en forma de páginas estáticas. Con esta combinación, también conocida como JAMstack, evita escribir un código de back-end complicado para ofrecer páginas web. En su lugar, el sistema solo ofrece páginas estáticas (HTML, CSS y JavaScript), y esas páginas llaman a las API para el trabajo del lado servidor. Dado que las API se pueden escribir con tecnologías sin servidor de escalado automático, se evitan por completo los problemas de costo y seguridad derivados del uso de hosts web o servidores AlwaysOn típicos. Para obtener más información, consulte [jamstack.org](https://jamstack.org/).

Para implementar un sitio estático o JAMstack en Azure, se emplea una variedad de herramientas y servicios:

- Configure una base de datos según sea necesario.
- Implemente código de API sin servidor en Azure Functions. Estas API suelen usar la base de datos.
- Elija las bibliotecas que quiera para el desarrollo del front-end, como Angular. A continuación, cargue estos archivos HTML, CSS y JavaScript estáticos en Azure Blob Storage, que proporciona un servidor web integrado.
- Cree un proxy inverso para que todo el tráfico pase a través de un dominio de dirección URL.

Puede ver una demostración del proceso con la sesión de //build 2019: [Productive front-end development with JavaScript, Visual Studio Code, and Azure](https://mybuild.techcommunity.microsoft.com/sessions/77038?source=sessions#top-anchor) (Desarrollo de front-end productivo con JavaScript, Visual Studio y Azure).

> [!VIDEO https://medius.studios.ms/Embed/Video-nc/B19-BRK3021?latestplayer=true]

Encontrará un tutorial paso a paso en [Implementación de un sitio web estático en Azure](tutorial-vscode-static-website-node-01.md).

En los siguientes artículos también se explican más detalles:

- **Bases de datos**: puede usar cualquier base de datos que quiera, como los diferentes servicios de base de datos de Azure que se describen en [How to integrate Azure databases in Node.js apps](node-howto-integrate-databases.md) (Cómo integrar bases de datos de Azure en aplicaciones Node.js).
  
- **API sin servidor**:

  - Comience con [Implementación de funciones de Azure desde Visual Studio Code](tutorial-vscode-serverless-node-01.md), que presenta Azure Functions en el contexto de Visual Studio Code y que simplifica muchos de los detalles.
  - Al completar el artículo, tendrá un proyecto de Azure Functions (una carpeta) que contiene una subcarpeta denominada para la función, que es la misma que su punto de conexión HTTP. La carpeta de función contiene un archivo *index.js* con el código.
  - Puede modificar esa función según sea necesario, así como agregar más funciones al proyecto y, a continuación, implementarlas de nuevo en Azure, donde están disponibles públicamente.
  - Para obtener recursos adicionales sobre el desarrollo sin servidor, consulte [How to write serverless Node.js code on Azure](node-howto-write-serverless-code.md) (Cómo escribir código de Node.js sin servidor en Azure).

- **Implementar el front-end en Azure Storage**: con las API a mano, ya puede escribir el código de front-end para usar esas API con el marco que quiera. Cuando esté listo, siga el artículo [Tutorial: Hospedaje de un sitio web estático en Blob Storage](/azure/storage/blobs/storage-blob-static-website-host) para cargar esos archivos en Azure y activar el hospedaje de sitios web estáticos.

- **Crear un proxy inverso**: un proxy inverso, como se describe en [Uso de Azure Functions Proxies](/azure/azure-functions/functions-proxies) le permite dirigir fácilmente determinadas solicitudes a distintas direcciones URL. En este caso, quiere dirigir las solicitudes de los archivos front-end a la dirección URL de Azure Storage, donde implementó esos archivos, y las solicitudes de API a la dirección URL de Azure Functions.

  - Para crear estos proxies, edite el archivo *proxies.json* en el proyecto de Functions para que aparezca como se muestra a continuación, sustituyendo las direcciones URL por `<api_url>` y `<storage_url>`:
  
    ```json
    {
      "$schema": "http://json.schemastore.org/proxies",
      "proxies": {
        "Static frontend on Azure Storage": {
          "matchCondition": {
            "route": "/{*restOfPath}"
          },
          "backendUri": "<storage_url>/{restOfPath}"
        },
        "Azure Functions API": {
          "matchCondition": {
            "route": "/api/{*restOfPath}"
          },
          "backendUri": "<api_url>/api/{restOfPath}"
        }
      }
    }
    ```
