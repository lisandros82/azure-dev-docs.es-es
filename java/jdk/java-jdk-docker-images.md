---
title: Uso de imágenes de Docker con un JDK para el desarrollo de Java en Azure
description: ''
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 04/09/2019
ms.devlang: java
ms.topic: conceptual
ms.service: Azure
ms.openlocfilehash: 5de2866f1f303deccf40158ac7451dbd35de92f1
ms.sourcegitcommit: 4cc7f5e1e4601065bfcb4c2eeb7d47ad0bec61f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68430960"
---
# <a name="use-docker-with-a-jdk-for-azure"></a>Uso de Docker con un JDK para Azure 

Las imágenes predefinidas de Docker para Java 7, 8 y 11 están disponibles en [Docker Hub](https://hub.docker.com/_/microsoft-java-se).

Están disponibles en Docker Hub imágenes de contenedor de Docker certificadas para el JDK, JRE y JRE desatendido en varias imágenes de sistema operativo base:

* [JDK](https://hub.docker.com/_/microsoft-java-jdk)
* [JRE](https://hub.docker.com/_/microsoft-java-jre)
* [JRE desatendido](https://hub.docker.com/_/microsoft-java-jre-headless)

## <a name="running-a-docker-image"></a>Ejecución de una imagen de Docker

Se pueden ejecutar imágenes de Docker mediante la sintaxis `$ docker run mcr.microsoft.com/java/jdk:tag java` tal como se muestra en el ejemplo siguiente.

```cli
docker run mcr.microsoft.com/java/jdk:8u212-zulu-alpine java -version 
```

## <a name="creating-a-docker-image"></a>Creación de una imagen de Docker

Puede crear una imagen mediante imágenes de Docker Hub oficiales de Microsoft, como se muestra en los ejemplos siguientes.

### <a name="create-a-docker-file"></a>Creación de un archivo de Docker

```cli
FROM mcr.microsoft.com/java/jdk:8u212-zulu-alpine 
  
RUN echo $' \
  
public class HelloWorld { \
   public static void main(String[] args) { \
      // Prints "Hello, World" in the terminal window. \
      System.out.println("Hello, World - From Microsoft Azure !!!"); \
   } \
}' > HelloWorld.java
  
RUN javac HelloWorld.java
  
CMD ["java", "HelloWorld"]
```

### <a name="build-a-docker-image"></a>Compilación de una imagen de Docker

```cli
docker build -t hello-world
```

### <a name="run-the-new-image"></a>Ejecución de la nueva imagen

```cli
docker run hello-world
```

Verá la salida siguiente:

```output
Hello World - From Microsoft Azure !!!
```
