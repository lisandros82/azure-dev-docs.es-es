---
title: Escritura de código Node.js sin servidor con Azure Functions
description: Instrucciones sobre cómo usar Azure Functions para crear e implementar código sin servidor mediante Azure Functions.
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/19/2019
ms.author: kraigb
ms.openlocfilehash: a5ed7d5d99009593845966969217f8a5081da4c4
ms.sourcegitcommit: 9cd460ee16b637e701aa30078932878c0d0a7945
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70181944"
---
# <a name="how-to-write-serverless-nodejs-code-on-azure"></a>Cómo escribir código de Node.js sin servidor en Azure

El código sin servidor le permite crear puntos de conexión a petición y receptivos en Internet sin tener que preocuparse por el aprovisionamiento o la administración de la infraestructura. El código sin servidor se compone de scripts u otros fragmentos de código que se ejecutan en respuesta a varios eventos. En Azure, la oferta sin servidor se denomina Azure Functions.

Para empezar, consulte:

- [Creación de la primera función mediante Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code). En este artículo se presenta Azure Functions en el contexto de Visual Studio Code, lo que simplifica muchos detalles.

A continuación, revise los artículos siguientes para ampliar sus conocimientos sobre lo que puede hacer Azure Functions:

- [Introducción a Azure Functions](/azure/azure-functions/functions-overview), que describe las ventajas del desarrollo sin servidor, los costos y los distintos desencadenadores que se pueden usar para ejecutar código sin servidor.

- [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](/azure/azure-functions/functions-triggers-bindings)

- [Guía para desarrolladores de Azure Functions](/azure/azure-functions/functions-reference), seguida de la [Guía para el desarrollador de JavaScript para Azure Functions](/azure/azure-functions/functions-reference-node)

- Si está interesado en escribir funciones *con estado* en un entorno sin servidor, consulte [¿Qué es Durable Functions?](/azure/azure-functions/durable/durable-functions-overview) y [Creación de su primera función durable en JavaScript](/azure/azure-functions/durable/quickstart-js-vscode).

Desde aquí, puede disfrutar de otros muchos recursos que le ayudarán a explorar en profundidad el código sin servidor:

- Módulo de Microsoft Learn: [Habilitar las actualizaciones automáticas en una aplicación web mediante Azure Functions y SignalR Service](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr/)

- Obtenga información sobre el uso de varios desencadenadores para ejecutar código sin servidor:

  - [Ejecutar código con un temporizador](/azure/azure-functions/functions-create-scheduled-function)
  - [Ejecutar código cuando se cargan o se actualizan archivos en Azure Blob Storage](/azure/storage/blobs/storage-upload-process-images?tabs=nodejsv10)
  - [Ejecutar código cuando se escribe un mensaje en Azure Queue Storage](/azure/azure-functions/functions-create-storage-queue-triggered-function)

- [Almacenamiento de datos no estructurados mediante Azure Functions y Cosmos DB](/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md?tabs=javascript). Para obtener información sobre otras bases de datos, consulte [How to integrate Azure databases in Node.js code](node-howto-integrate-databases.md) (Cómo integrar bases de datos de Azure en código Node.js).

- [Codificación y comprobación de Azure Functions en un entorno local](/azure/azure-functions/functions-develop-local)

- [Estrategias para probar el código en Azure Functions](/azure/azure-functions/functions-test-a-function) y [Control de errores](/azure/azure-functions/functions-bindings-error-pages)

- [Configurar la autenticación con Azure Active Directory](/azure/app-service/configure-authentication-provider-aad.md?toc=%2fazure%2fazure-functions%2ftoc.json)

- [Configuración de la implementación continua en Azure Pipelines](/azure/azure-functions/functions-how-to-azure-devops)

- [Ejemplos de Node.js y Azure Functions](/samples/browse/?languages=javascript%2Cnodejs&products=azure-functions)
