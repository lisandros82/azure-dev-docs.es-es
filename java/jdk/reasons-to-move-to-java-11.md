---
title: Razones para pasarse a Java 11
description: Documento de resumen diseñado para los responsables de la toma de decisiones que sopesan las ventajas de pasar de Java 8 a Java 11.
author: dsgrieve
manager: maverberg
tags: java
ms.topic: article
ms.date: 11/19/2019
ms.author: dagrieve
ms.openlocfilehash: 7daf058c2abebbf2cca85dadc4f9ffe3e8771fa1
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812219"
---
# <a name="reasons-to-move-to-java-11"></a>Razones para pasarse a Java 11

La pregunta no es *si* debe pasarse a Java 11, sino *cuándo*. En los próximos años, no se admitirá Java 8 y los usuarios tendrán que pasarse a Java 11. Nosotros sostenemos que pasarse a Java 11 tiene ventajas y animamos a los equipos a hacerlo lo antes posible.

Desde Java 8, se han agregado nuevas características y se han realizado mejoras. Hay adiciones y modificaciones perceptibles en la API y hay mejoras en el inicio, el rendimiento y el uso de la memoria.

## <a name="transitioning-to-java-11"></a>Transición a Java 11

La transición a Java 11 se puede realizar de una manera escalonada. *No* es necesario que el código use módulos de Java para ejecutarse en Java 11. Java 11 se puede usar para ejecutar código desarrollado y creado con JDK 8.
Pero pueden surgir algunos problemas, principalmente relacionados con la API en desuso, los cargadores de clases y la reflexión.

En el grupo de ingeniería de Java de Microsoft proporcionará una guía completa de la transición de Java 8 a Java 11. Mientras tanto, hay muchas guías para realizar la transición de Java 8 a Java 9 que pueden servir de ayuda para comenzar. Por ejemplo, [Guía de migración de la plataforma Java Standard Edition a JDK 9 para Oracle](https://docs.oracle.com/javase/9/migrate/toc.htm) y [Estado del sistema de módulos: compatibilidad y migración](http://openjdk.java.net/projects/jigsaw/spec/sotms/#compatibility--migration).

## <a name="high-level-changes-between-java-8-and-11"></a>Cambios generales entre Java 8 y Java 11

En esta sección no se enumeran todos los cambios realizados en las versiones de Java 9 \[[1](#ref1)\], 10 \[[2](#ref2)\] y 11 \[[3](#ref3)\]. Se resaltan los cambios que afectan al rendimiento, el diagnóstico y la productividad.

### <a name="modules-4ref4"></a>Módulos \[[4](#ref4)\]

Los módulos solucionan problemas de configuración y encapsulación difíciles de administrar en aplicaciones a gran escala que se ejecutan en *classpath*. Un *módulo* es una colección autodescriptiva de clases e interfaces de Java y recursos relacionados.

Los módulos permiten personalizar configuraciones del entorno en tiempo de ejecución que contienen solo los componentes que requiere una aplicación. Esta personalización crea una superficie más pequeña y permite que una aplicación se vincule estáticamente, mediante [jlink](https://docs.oracle.com/en/java/javase/11/tools/jlink.html), en un entorno de ejecución personalizado para la implementación. Esta superficie más pequeña puede ser especialmente útil en una arquitectura de microservicios.

Internamente, la Máquina virtual Java es capaz de aprovechar las ventajas de los módulos de manera que la carga de clases sea más eficaz. El resultado es un entorno de ejecución más pequeño, más ligero y más rápido en iniciarse. Las técnicas de optimización utilizadas por la Máquina virtual Java para mejorar el rendimiento de la aplicación pueden ser más eficaces porque los módulos codifican qué componentes requiere una clase.

En el caso de los programadores, los módulos ayudan a forzar una encapsulación mayor al requerir una declaración explícita de los paquetes que exporta un módulo y los componentes que requiere, y mediante la restricción del acceso por reflexión.
Este nivel de encapsulación hace que una aplicación sea más segura y más fácil de mantener.

Una aplicación puede seguir usando *classpath* y no tiene que hacer una transición a los módulos como requisito para ejecutarse en Java 11.

### <a name="profiling-and-diagnostics"></a>Generación de perfiles y diagnóstico

#### <a name="java-flight-recorder-5ref5"></a>Java Flight Recorder \[[5](#ref5)\]

Java Flight Recorder (JFR) recopila datos de diagnóstico y generación de perfiles de una aplicación de Java en ejecución. JFR tiene poco impacto en una aplicación de Java en ejecución. Posteriormente, los datos recopilados se pueden analizar con Java Mission Control (JMC) y otras herramientas. Mientras que JFR y JMC eran características comerciales en Java 8, ambas son de código abierto en Java 11.

#### <a name="java-mission-control-6ref6"></a>Java Mission Control \[[6](#ref6)\]

Java Mission Control (JMC) proporciona una presentación gráfica de los datos recopilados por Java Flight Recorder (JFR) y es de código abierto en Java.
11. Además de la información general sobre la aplicación en ejecución, JMC permite al usuario profundizar en los datos. JFR y JMC se pueden usar para diagnosticar problemas en tiempo de ejecución, como fugas de memoria, sobrecarga del recolector de elementos no utilizados, métodos de acceso frecuente, cuellos de botella de subprocesos y bloqueos de E/S.

#### <a name="unified-logging-7ref7"></a>Registro unificado \[[7](#ref7)\]

Java 11 tiene un sistema de registro común para todos los componentes de JVM.
Este sistema de registro unificado permite al usuario definir qué componentes se van a registrar y a qué nivel. Este registro detallado es útil para realizar el análisis de la causa principal de los bloqueos de JVM y para diagnosticar problemas de rendimiento en un entorno de producción.

#### <a name="low-overhead-heap-profiling-8ref8"></a>Generación de perfiles de montones de sobrecarga baja \[[8](#ref8)\]

Se ha agregado una nueva API a la interfaz de herramientas de la Máquina virtual Java (JVMTI) para el muestreo de las asignaciones de montones de Java. El muestreo tiene poca sobrecarga y se puede habilitar de un modo continuado. Aunque la asignación de montones se puede supervisar con Java Flight Recorder (JFR), el método de muestreo de JFR solo trabaja con asignaciones. La implementación de JFR también puede perder algunas asignaciones. Por el contrario, el muestreo de montones de Java 11 puede proporcionar información sobre los objetos activos y los inactivos.

Los proveedores de soluciones de supervisión de rendimiento de aplicaciones (APM) comienzan a usar esta nueva característica y el grupo de ingeniería de Java investiga su posible uso con las herramientas de supervisión del rendimiento de Azure.

#### <a name="stackwalker-9ref9"></a>StackWalker \[[9](#ref9)\]

La obtención de una instantánea de la pila para el subproceso actual es algo común en el registro. El problema es cuánto seguimiento de la pila se va a registrar, si es que se registra. Por ejemplo, puede que desee ver el seguimiento de la pila solo para una excepción determinada de un método. La clase StackWalker (agregada en Java 9) proporciona una instantánea de la pila y proporciona métodos que dan al programador un control detallado sobre cómo consumir el seguimiento de la pila.

### <a name="garbage-collection-10ref10"></a>Recolección de elementos no utilizados \[[10](#ref10)\]

Los siguientes recolectores de elementos no utilizados están disponibles en Java 11: en serie, en paralelo, Garbage-First y Epsilon. El recolector de elementos no utilizados predeterminado en Java 11 es Garbage-First Garbage Collector (G1GC).

Los otros tres recolectores se mencionan por motivos de exhaustividad. El recolector de elementos no utilizados Z (ZGC) es un recolector simultáneo de baja latencia que intenta mantener los tiempos de pausa por debajo de los 10 ms. ZGC está disponible como una característica experimental de Java 11. Shenandoah es un recolector de baja pausa que, para reducir los tiempos de pausa, realiza más recolección de elementos no utilizados simultáneamente con el programa de Java en ejecución.
Shenandoah es una característica experimental de Java 12, pero hay modificaciones para compatibilidad con versiones anteriores para Java 11. El recolector Concurrent Mark and Sweep (CMS) está disponible pero está en desuso desde Java 9.

La Máquina virtual Java establece valores predeterminados de recolección de elementos no utilizados para el caso de uso medio. A menudo, estos valores predeterminados y otros valores de la recolección de elementos no utilizados se deben optimizar para lograr un rendimiento o una latencia óptimos, según los requisitos de la aplicación.
El ajuste correcto de la recolección de elementos no utilizados requiere un conocimiento profundo de la materia, una experiencia que proporciona el [Grupo de ingeniería de Java de Microsoft](mailto:javaplatformgroup@microsoft.com).

#### <a name="g1gc"></a>G1GC

El recolector de elementos no utilizados predeterminado en Java 11 es G1 (G1GC). El objetivo de G1GC es conseguir un equilibrio entre la latencia y el rendimiento. Para lograr un alto rendimiento, el recolector de elementos no utilizados G1 intenta satisfacer los objetivos de tiempo de pausa con una probabilidad alta. G1GC está diseñado para evitar recolecciones completas, pero cuando las recolecciones simultáneas no pueden reclamar memoria con suficiente rapidez, se producirá una recolección de elementos no utilizados completa de reserva. La recolección de elementos no utilizados completa utiliza el mismo número de subprocesos de trabajo paralelos que las recolecciones más recientes y mixtas.

#### <a name="parallel-gc"></a>Recolección de elementos no utilizados en paralelo

El recolector en paralelo es el predeterminado de Java 8. El recolector de elementos no utilizados en paralelo usa varios subprocesos para acelerar la recolección de elementos no utilizados.

#### <a name="epsilon-11ref11"></a>Epsilon \[[11](#ref11)\]

El recolector de elementos no utilizados Epsilon controla las asignaciones, pero no reclama memoria. Una vez agotado el montón, la Máquina virtual Java se apagará.
Epsilon es útil para servicios de corta duración y para aplicaciones que se sabe que no tienen elementos no utilizados.

#### <a name="improvements-for-docker-containers-12ref12"></a>Mejoras en los contenedores de Docker \[[12](#ref12)\]

Antes de Java 10, JVM no reconocía las restricciones de memoria y CPU establecidas en un contenedor. En Java 8, por ejemplo, JVM establecerá de forma predeterminada el tamaño máximo del montón en 1/4 de la memoria física del host subyacente. A partir de Java 10, JVM usa las restricciones establecidas por los grupos de control del contenedor para establecer los límites de memoria y CPU (consulte la nota a continuación).
Por ejemplo, el tamaño máximo predeterminado del montón es 1/4 del límite de memoria del contenedor (es decir, 500 MB para -m2G).

También se han agregado opciones a la Máquina virtual Java para proporcionar a los usuarios del contenedor de Docker un control detallado de la cantidad de memoria del sistema que se usará para el montón de Java.

Esta compatibilidad está habilitada de forma predeterminada y solo está disponible en plataformas basadas en Linux.

> [!NOTE]
> La mayor parte del trabajo de habilitación de grupos de control del contenedor se ha incorporado a Java 8 a partir de la versión jdk8u191. Las nuevas mejoras no se incorporarán necesariamente a la versión 8.

#### <a name="multi-release-jar-files-13ref13"></a>Archivos jar multiversión \[[13](#ref13)\]

En Java 11, es posible crear un archivo jar que contenga varias versiones de los archivos de clases específicas de la versión de Java. Los archivos jar multiversión permiten a los desarrolladores de bibliotecas admitir varias versiones de Java sin tener que enviar varias versiones de archivos jar. Para el consumidor de estas bibliotecas, los archivos jar multiversión solucionan el problema de tener que hacer coincidir determinados archivos jar con determinados objetivos del entorno de ejecución.

## <a name="miscellaneous-performance-improvements"></a>Mejoras de rendimiento varias

Los siguientes cambios en JVM tienen un impacto directo en el rendimiento.

-   **JEP 197: Memoria caché de código segmentada** \[[14](#ref14)\]: divide la memoria caché de código en segmentos distintos. Esta segmentación proporciona un mejor control de la superficie de memoria de JVM, acorta el tiempo de examen de los métodos compilados, reduce significativamente la fragmentación de la memoria caché de código y mejora el rendimiento.

-   **JEP 254: Cadenas compactas** \[[15](#ref15)\]: cambia la representación interna de una cadena de dos bytes por carácter a uno o dos bytes por carácter, en función de la codificación de caracteres. Dado que la mayoría de las cadenas contienen caracteres ISO-8859-1/Latin-1, este cambio divide entre dos la cantidad de espacio necesario para almacenar una cadena.

-   **JEP 310: Uso compartido de datos de clases de la aplicación** \[[16](#ref16)\]: el uso compartido de datos de clases reduce el tiempo de inicio, ya que permite que las clases archivadas se asignen a la memoria en tiempo de ejecución. El uso compartido de datos de clases de la aplicación extiende el uso compartido de datos de clases porque permite la colocación de las clases de aplicación en el archivo de CDS. Cuando varias JVM comparten el mismo archivo de almacenamiento, se ahorra memoria y se mejora el tiempo de respuesta total del sistema.

-   **JEP 312: Protocolos de enlace locales de subprocesos** \[[17](#ref17)\]: permiten ejecutar una devolución de llamada en subprocesos sin realizar un punto seguro de la máquina virtual global, lo que ayuda a la máquina virtual a lograr una latencia menor al reducir el número de puntos seguros globales.

-   **Asignación diferida de subprocesos del compilador** \[[18](#ref18)\]: en el modo de compilación en niveles, la máquina virtual inicia un gran número de subprocesos del compilador.
    Este modo es el valor predeterminado en sistemas con muchas CPU. Estos subprocesos se crean independientemente de la memoria disponible o el número de solicitudes de compilación. Los subprocesos consumen memoria incluso cuando están inactivos (que es casi todo el tiempo), lo que conduce a un uso ineficaz de los recursos. Para solucionar este problema, la implementación se ha cambiado para iniciar un solo subproceso del compilador de cada tipo durante el inicio. El inicio de subprocesos adicionales y el cierre de los subprocesos sin uso se controlan dinámicamente. 

Los cambios siguientes en las bibliotecas principales afectan al rendimiento del código nuevo o modificado.

-   **JEP 193: Manipuladores de variables** \[[19](#ref19)\]: define un medio estándar para invocar los equivalentes de varias operaciones de java.util.concurrent.atomic y sun.misc.Unsafe en campos de objeto y elementos de matriz, un conjunto estándar de operaciones de barrera para el control específico de la ordenación de la memoria, y una operación estándar de disponibilidad y barrera para asegurarse de que un objeto al que se hace referencia se mantiene con alta accesibilidad.

-   **JEP 269: Factory Method cómodos para colecciones** \[[20](#ref20)\]: define las API de biblioteca para que sea cómodo crear instancias de colecciones y asignaciones con un número pequeño de elementos. Factory Method estáticos de las interfaces de colección que crean instancias de colección compactas y no modificables. Estas instancias son intrínsecamente más eficaces. Las API crean colecciones que se representan de un modo compacto y no tienen una clase contenedora.

-   **JEP 285: Aviso de giro y espera** \[[21](#ref21)\]: proporciona una API que permite a Java avisar al sistema en tiempo de ejecución que se encuentra en un bucle de giro. Algunas plataformas de hardware se benefician del aviso del software de que un subproceso está en un estado de espera y ocupado.

-   **JEP 321: Cliente HTTP (estándar)** \[[22](#ref22)\]: proporciona una nueva API de cliente HTTP que implementa HTTP/2 y WebSocket y puede reemplazar la API HttpURLConnection heredada.

## <a name="references"></a>Referencias

<a id="ref1">\[1\]</a> Oracle Corporation, \"Notas de la versión de Java Development Kit 9\" (en línea). Disponible en: https://www.oracle.com/technetwork/java/javase/9u-relnotes-3704429.html.
(Último acceso el 13 de noviembre de 2019).

<a id="ref2">\[2\]</a> Oracle Corporation, \"Notas de la versión de Java Development Kit 10\" (en línea). Disponible en: https://www.oracle.com/technetwork/java/javase/10u-relnotes-4108739.html.
(Último acceso el 13 de noviembre de 2019).

<a id="ref3">\[3\]</a> Oracle Corporation, \"Notas de la versión de Java Development Kit 11\" (en línea). Disponible en: https://www.oracle.com/technetwork/java/javase/11u-relnotes-5093844.html.
(Último acceso el 13 de noviembre de 2019).

<a id="ref4">\[4\]</a> Oracle Corporation, \"Proyecto Jigsaw\", 22 de septiembre, 
2017. (en línea). Disponible en: http://openjdk.java.net/projects/jigsaw/.
(Último acceso el 13 de noviembre de 2019).

<a id="ref5">\[5\]</a> Oracle Corporation, \"JEP 328: Flight Recorder\", 9 de septiembre de 2018. (en línea). Disponible en: http://openjdk.java.net/jeps/328. (Último acceso el 13 de noviembre de 2019).

<a id="ref6">\[6\]</a> Oracle Corporation, \"Mission Control\", 25 de abril de 2019. (en línea). Disponible en: https://wiki.openjdk.java.net/display/jmc/Main. (Último acceso el 13 de noviembre de 2019).

<a id="ref7">\[7\]</a> Oracle Corporation, \"JEP 158: Registro de JVM unificado\", 14 de febrero de 2019. (en línea). Disponible en: http://openjdk.java.net/jeps/158.
(Último acceso el 13 de noviembre de 2019).

<a id="ref8">\[8\]</a> Oracle Corporation, \"JEP 331: Generación de perfiles de montones de sobrecarga baja\", 5 de septiembre de 2018. (en línea). Disponible en: http://openjdk.java.net/jeps/331. (Último acceso el 13 de noviembre de 2019).

<a id="ref9">\[9\]</a> Oracle Corporation, \"JEP 259: API Stack-Walking\", 18 de julio de 2017. (en línea).
Disponible en: http://openjdk.java.net/jeps/259. (Último acceso el 13 de noviembre de 2019).

<a id="ref10">\[10\]</a> Oracle Corporation, \"JEP 248: Establecimiento de G1 como el recolector de elementos no utilizados predeterminado\", 12 de septiembre de 2017. (en línea). Disponible en: http://openjdk.java.net/jeps/248. (Último acceso el 13 de noviembre de 2019).

<a id="ref11">\[11\]</a> Oracle Corporation, \"JEP 318: Epsilon: un recolector de elementos no utilizados sin operación\", 24 de septiembre de 2018.
(en línea). Disponible en: http://openjdk.java.net/jeps/318. (Último acceso el 13 de noviembre de 2019).

<a id="ref12">\[12\]</a> Oracle Corporation, \"JDK-8146115: Mejora de la detección de contenedores de Docker y del uso de la configuración de recursos\", 16 de septiembre de 2019.
(en línea). Disponible en: https://bugs.java.com/bugdatabase/view\_bug.do?bug\_id=JDK-8146115.
(Último acceso el 13 de noviembre de 2019).

<a id="ref13">\[13\]</a> Oracle Corporation, \"JEP 238: Archivos JAR multiversión\", 22 de junio de 2017. (en línea). Disponible en: http://openjdk.java.net/jeps/238. (Último acceso el 13 de noviembre de 2019).

<a id="ref14">\[14\]</a> Oracle Corporation, \"JEP 197: Memoria caché de código segmentada\", 28 de abril de 2017. (en línea).
Disponible en: http://openjdk.java.net/jeps/197. (Último acceso el 13 de noviembre de 2019).

<a id="ref15">\[15\]</a> Oracle Corporation, \"JEP 254: Cadenas compactas\", 18 de mayo de 2019. (en línea). Disponible en: http://openjdk.java.net/jeps/254.
(Último acceso el 13 de noviembre de 2019).

<a id="ref16">\[16\]</a> Oracle Corporation, \"JEP 310: Uso compartido de datos de clases de la aplicación\", 17 de agosto de 2018. (en línea). Disponible en: https://openjdk.java.net/jeps/310. (Último acceso el 13 de noviembre de 2019).

<a id="ref17">\[17\]</a> Oracle Corporation, \"JEP 312: Protocolos de enlace locales de subprocesos\", 21 de agosto de 2019.
(en línea). Disponible en: https://openjdk.java.net/jeps/312. (Último acceso el 13 de noviembre de 2019).

<a id="ref18">\[18\]</a> Oracle Corporation, \"JDK-8198756: Asignación diferida de subprocesos del compilador\", 29 de octubre de 2018. (en línea). Disponible en: https://bugs.java.com/bugdatabase/view\_bug.do?bug\_id=8198756.
(Último acceso el 13 de noviembre de 2019).

<a id="ref19">\[19\]</a> Oracle Corporation, \"JEP 193: Manipuladores de variables\", 17 de agosto de 2017. (en línea). Disponible en: https://openjdk.java.net/jeps/193. (Último acceso el 13 de noviembre de 2019).

<a id="ref20">\[20\]</a> Oracle Corporation, \"JEP 269: Factory Method cómodos para colecciones\", 26 de junio de 2017. (en línea). Disponible en: https://openjdk.java.net/jeps/269.
(Último acceso el 13 de noviembre de 2019).

<a id="ref21">\[21\]</a> Oracle Corporation, \"JEP 285: Avisos de giro y espera\", 20 de agosto de 2017. (en línea). Disponible en: https://openjdk.java.net/jeps/285. (Último acceso el 13 de noviembre de 2019).

<a id="ref22">\[22\]</a> Oracle Corporation, \"JEP 321: Cliente HTTP (estándar)\", 27 de septiembre de 2018. (en línea).
Disponible en: https://openjdk.java.net/jeps/321. (Último acceso el 13 de noviembre de 2019).
