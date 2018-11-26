---
description: Si eres un desarrollador con una aplicación WindowsPhone Silverlight, a continuación, puedes hacer un uso inigualable de tus conocimientos y código fuente al migrar a Windows 10.
title: Migrar de WindowsPhone Silverlight a UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 17d107e7886838071567a1368c6b542ae5ecfad0
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7719038"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>Migrar de WindowsPhone Silverlight a UWP


Si eres un desarrollador con una aplicación WindowsPhone Silverlight, a continuación, puedes hacer un uso inigualable de tus conocimientos y código fuente al migrar a Windows 10. Con Windows 10, puedes crear una aplicación de plataforma Universal de Windows (UWP), que es un paquete de aplicación única que los clientes pueden instalar en todos los tipos de dispositivo. Para obtener más información sobre Windows 10, las aplicaciones para UWP y los conceptos de código adaptable e interfaz de usuario adaptable que mencionaremos en esta guía de migración, consulta la [Guía de aplicaciones de la plataforma Universal de Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/dn894631).

Cuando se migra la aplicación WindowsPhone Silverlight a una aplicación de Windows 10, podrás ponerse al día sobre las características de dispositivos móviles que se [introdujo en Windows Phone 8.1](https://msdn.microsoft.com/library/windows/apps/dn632424)y que van más allá de uso cuyo modelo de aplicaciones y el marco de la interfaz de usuario en la plataforma Universal de Windows (UWP) son universales a través de todos los dispositivos de Windows 10. Esto hace posible admitir PC, tabletas, teléfonos y un gran número de otros tipos de dispositivos, con un código base y un paquete de la aplicación. Y multiplicará el público potencial de la aplicación, además de crear nuevas posibilidades con datos compartidos, consumibles comprados, etcétera. Para obtener más información sobre las nuevas características, consulte [Novedades para desarrolladores en Windows 10](https://dev.windows.com/getstarted/whats-new-windows-10).

Si lo deseas, la versión de WindowsPhone Silverlight de la aplicación y la versión de Windows 10 de ambos puede disponibles para los clientes al mismo tiempo.

**Nota**esta guía está diseñada para ayudarte a portar manualmente la aplicación WindowsPhone Silverlight para Windows 10. Además de usar la información de esta guía para migrar la aplicación, puedes probar la vista previa de desarrollador de **Puente Mobilize.NET Silverlight** para facilitar la automatización del proceso de migración. Esta herramienta analiza el código fuente de la aplicación y convierte las referencias en controles WindowsPhone Silverlight y las API en sus equivalentes UWP. Dado que esta herramienta todavía está en la vista previa de desarrollador, aún no procesa todos los escenarios de conversión. Sin embargo, la mayoría de los desarrolladores deberían poder ahorrar tiempo y esfuerzo si empiezan con esta herramienta. Para probar la vista previa de desarrollador, visita el [sitio web de Mobilize.NET](http://go.microsoft.com/fwlink/p/?LinkId=624546).

## <a name="xaml-and-net-or-html"></a>¿XAML y .NET o HTML?

WindowsPhone Silverlight tiene un marco de la UI de XAML basado en Silverlight 4.0 y se programa en una versión de .NET Framework y un pequeño subconjunto de las API de UWP. Puesto que usaste el lenguaje de marcado de aplicaciones Extensible (XAML) en la aplicación WindowsPhone Silverlight, es probable que el XAML sea tu elección para tu versión de Windows 10 porque se transferirá la mayor parte de tus conocimientos y experiencia, así como gran parte del código fuente y los patrones de software que usas. Incluso el marcado y el diseño de interfaz de usuario se pueden migrar inmediatamente. Encontrarás las API administradas, el marcado XAML, el marco de trabajo de la interfaz de usuario y las herramientas muy familiares; además puedes usar C++, C# o Visual Basic con XAML en una aplicación para UWP. Es posible que te sorprendas de lo relativamente fácil que es el proceso, incluso si te encuentras con algún desafío sobre la marcha.

Consulta [Guía básica para aplicaciones para la Plataforma universal de Windows con C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/br229583).

**Nota**Windows 10 admite mucho más de .NET Framework que una aplicación de Windows Phone Store. Por ejemplo, Windows 10 tiene varios espacios de nombres System.ServiceModel.\* como System.Net System.Net.NetworkInformation y System.Net.Sockets. Por lo tanto, ahora es un buen momento para migrar de WindowsPhone Silverlight y el código .NET compilar y usar en la nueva plataforma. Consulta [Asignaciones de espacios de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md).
Otro buen motivo para volver a compilar el código fuente de .NET existente en una aplicación de Windows 10 es que te beneficiarás de .NET Native, que una tecnología de compilación anticipada que convierte MSIL en código de máquina ejecutable de forma nativa. Las aplicaciones de .NET Native se inician más rápido, usan menos memoria y usan menos batería que sus equivalentes MSIL.

Esta guía de migración se centrará en XAML, pero, como alternativa, puedes compilar una aplicación funcionalmente equivalente (que llame a muchas de las mismas API de UWP) con JavaScript, Hojas de estilo CSS y HTML5 junto con la Biblioteca de Windows para JavaScript. Aunque los marcos de trabajo de la interfaz de usuario de Windows en tiempo de ejecución de XAML y HTML son diferentes entre sí, el que elijas funcionará universalmente en toda la gama de dispositivos Windows.

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>Objetivo la familia de dispositivos universales o móviles

Una opción que tienes es migrar la aplicación a una aplicación que tiene como objetivo la familia de dispositivos universales. En este caso, la aplicación puede instalarse en la gama más amplia de dispositivos. Si la aplicación llama a las API implementadas solamente en la familia de dispositivos móviles, puedes proteger esas llamadas con código adaptable. Como alternativa, puedes migrar la aplicación a una aplicación que tenga como objetivo la familia de dispositivos móviles, en cuyo caso no es necesario escribir código adaptable.

## <a name="adapting-your-app-to-multiple-form-factors"></a>Adaptar la aplicación a distintos factores de forma

La opción que elijas de la sección anterior determinará la gama de dispositivos en que se ejecutarán tus aplicaciones, la cual puede ser muy amplia. Incluso limitando la aplicación a la familia de dispositivos móviles, dispones de una amplia variedad de tamaños de pantalla compatibles. Por lo tanto, como la aplicación se ejecuta en factores de forma que antes no se admitían, prueba la interfaz de usuario en esos factores de forma y realiza los cambios necesarios para que la interfaz de usuario se adapte correctamente a cada uno. Piensa que se trata de una tarea posterior a la migración, o un objetivo de ampliación de la migración, y hay un ejemplo de ella en el caso de estudio [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md).

## <a name="approaching-porting-layer-by-layer"></a>Enfoque de la migración capa a capa

-   **Vista**. La vista (junto con el modelo de vista) conforma la interfaz de usuario de la aplicación. Lo ideal es que la vista conste de marcado enlazado a propiedades observables de un modelo de vista. Otro patrón (común y conveniente, pero solo a corto plazo) es para el código imperativo en un archivo de código subyacente para manipular directamente elementos de la interfaz de usuario. En cualquier caso, gran parte del marcado y diseño de interfaz de usuario (incluso el código imperativo que manipula los elementos de la interfaz de usuario) serán sencillos de migrar.
-   **Modelos de vista y de datos**. Aunque no adoptes formalmente patrones de separación de cuestiones (por ejemplo, MVVM), inevitablemente hay código presente en la aplicación que realiza la función de modelo de vista y modelo de datos. El código del modelo de vista usa tipos en los espacios de nombres del marco de trabajo de la interfaz de usuario. Tanto el modelo de vista como el modelo de datos usan además un sistema operativo no visual y las API de .NET (incluidas las API para el acceso a datos). La mayoría de ellas están [disponibles para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/br211369), por lo que posiblemente podrás migrar gran parte de este código sin cambios. Pero recuerda: un modelo de vista es un modelo, o *abstracción*, de una vista. Un modelo de vista proporciona el estado y el comportamiento de la interfaz de usuario, mientras que la vista en sí proporciona los elementos visuales. Por este motivo, cualquier interfaz de usuario que adaptes a los diferentes factores de forma en los que UWP te permite ejecutar probablemente necesitará los cambios de modelo de vista correspondientes. Para las redes y las llamadas a los servicios en la nube, normalmente puedes elegir entre usar las API de UWP o .NET. Para conocer los factores que implica tomar esta decisión, consulta [Servicios en la nube, redes y bases de datos](wpsl-to-uwp-business-and-data.md).
-   **Servicios en la nube**. Es probable que parte de la aplicación (quizás una gran parte) se ejecute en la nube en forma de servicios. La parte de la aplicación que se ejecuta en el dispositivo cliente se conecta a ellos. Esta es la parte de una aplicación distribuida que probablemente no se modificará al migrar la parte del cliente. Si aún no tienes uno, una buena opción de servicios en la nube para tu aplicación para UWP es [Servicios móviles de Microsoft Azure](http://azure.microsoft.com/services/mobile-services/), que proporciona componentes back-end eficaces a los que las aplicaciones universales de Windows pueden llamar para servicios que van desde sencillas notificaciones de actualizaciones de iconos dinámicos hasta la clase de escalabilidad de tareas difíciles que puede proporcionar una granja de servidores.

Antes o durante la migración, considera la posibilidad de si la aplicación podría mejorarse mediante la refactorización de modo que el código con un propósito similar se agrupe en capas y no se disperse arbitrariamente. La factorización de la aplicación para UWP en capas, como las descritas anteriormente, facilita corregir la aplicación, probarla y posteriormente leerla y mantenerla. Puedes hacer más reutilizable las funciones (y evitar algunos problemas de diferencias de API de interfaz de usuario entre plataformas) siguiendo el patrón Model-View-ViewModel ([MVVM](http://msdn.microsoft.com/magazine/dd419663.aspx)). Este patrón mantiene separadas entre sí las partes de la aplicación correspondientes a los datos, al negocio y a la interfaz de usuario. Incluso dentro de la interfaz de usuario, puede mantener separados el estado y el comportamiento, además de poderse probar por separado, desde los elementos visuales. Con MVVM, puedes escribir una vez la lógica de datos y de negocio, y usarla en todos los dispositivos, independientemente de la interfaz de usuario. Es probable que también puedas volver a usar la mayor parte del modelo de vista y los elementos de vista entre dispositivos.

## <a name="one-or-two-exceptions-to-the-rule"></a>Una o dos excepciones a la regla

Cuando leas esta guía de migración, puedes consultar [Asignaciones de espacios de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md). Una asignación bastante sencilla es la regla general; la tabla de asignaciones de espacios de nombres y clases describe las excepciones.

En el nivel de función, la buena noticia es que hay muy poco que no sea compatible con UWP. La mayoría de tus conocimientos y código fuente se traslada muy bien a las aplicaciones para UWP, como verás en el resto de esta guía de migración. Sin embargo, estas son las características de WindowsPhone Silverlight algunas que quizá hayas usado para que no hay ningún equivalente UWP.

| Función para la que no hay equivalente en UWP | Documentación de WindowsPhone Silverlight para la característica |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA. En general, [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) con C++ sirve de sustitución. Consulta [Desarrollo de juegos](https://msdn.microsoft.com/library/windows/apps/hh452744) e [Interoperabilidad de DirectX y XAML](https://msdn.microsoft.com/library/windows/apps/hh825871). | [Biblioteca de clases de XNA Framework](http://msdn.microsoft.com/library/bb203940.aspx) | 
|Aplicaciones de modos | [Modos para Windows Phone8](http://msdn.microsoft.com/library/windows/apps/jj206990.aspx) |

&nbsp;

| Tema| Descripción|
|------|------------| 
| [Asignaciones de espacios de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md) | Este tema proporciona una asignación completa de Windows Phone Silverlight APIs a sus equivalentes de UWP. |
| [Migración del proyecto](wpsl-to-uwp-porting-to-a-uwp-project.md) | Comenzar el proceso de migración creando un nuevo proyecto de Windows 10 en Visual Studio y copiando los archivos en él. |
| [Solución de problemas](wpsl-to-uwp-troubleshooting.md) | Se recomienda leer hasta el final esta guía de migración, aunque somos conscientes de que estás deseando seguir avanzando y llegar a la fase de compilación y ejecución de tu proyecto. Con esto en mente, puedes avanzar temporalmente si comentas o quitas cualquier código que no sea esencial y más tarde regresas allí para restaurar lo que has quitado. La tabla de solución de problemas de síntomas y soluciones de este tema puede resultarte útil en esta etapa, aunque no sustituye la lectura de los siguientes temas. Siempre puedes consultar la tabla a medida que avances a través de los temas posteriores. |
| [Migración de XAML y la interfaz de usuario](wpsl-to-uwp-porting-xaml-and-ui.md) | La práctica para definir la interfaz de usuario en forma de marcado XAML declarativo se traslada muy bien desde WindowsPhone Silverlight a las aplicaciones para UWP. Encontrarás que grandes secciones del marcado son compatibles, una vez hayas actualizado las referencias de clave de recurso del sistema, cambiado algunos nombres de tipos de elementos y cambiado "clr-namespace" por "using". |
| [Migración de modelo de E/S, dispositivos y aplicaciones](wpsl-to-uwp-input-and-sensors.md) | El código que se integra con el dispositivo y sus sensores implica la entrada del usuario y la salida de este. También puede implicar el procesamiento de datos. No obstante, este código no se considera generalmente como la capa de interfaz de usuario o la capa de datos. Este código incluye la integración con el controlador de vibración, el acelerómetro, el giroscopio, el micrófono y los altavoces (que se relacionan con el reconocimiento y la síntesis de voz), la localización (geográfica) y las modalidades de entrada, como, por ejemplo, entrada táctil, mouse, teclado y lápiz. |
| [Migración de capas de negocio y de datos](wpsl-to-uwp-business-and-data.md) | Detrás de la interfaz de usuario se encuentran las capas de negocio y de datos. El código de estas capas llama a las API del sistema operativo y de .NET Framework (por ejemplo, proceso en segundo plano, ubicación, cámara, sistema de archivos, red y acceso a otros datos). La mayoría de ellas están [disponibles para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/br211369), por lo que posiblemente podrás migrar gran parte de este código sin cambios. |
| [Migración para factor de forma y experiencia del usuario](wpsl-to-uwp-form-factors-and-ux.md) | Las aplicaciones de Windows comparten una apariencia común entre PC, dispositivos móviles y muchos otros tipos de dispositivos. La interfaz de usuario, las entradas y los patrones de interacción son muy similares, y un usuario que se mueve entre dispositivos agradecerá la experiencia familiar.|
|[Caso práctico: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | Este tema presenta un caso práctico de migración de una aplicación WindowsPhone Silverlight muy simple a una aplicación Windows10UWP. Con Windows 10, puedes crear un paquete de la aplicación único que los clientes pueden instalar en una amplia gama de dispositivos, y eso es lo que haremos en este caso práctico. |
| [Caso práctico: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | Este caso práctico, que se basa en la información proporcionada en [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md), comienza con una aplicación de WindowsPhone Silverlight que muestra datos agrupados en un **LongListSelector**. En el modelo de vista, cada instancia de la clase **Author** representa el grupo de los libros que ha escrito ese autor y, en **LongListSelector**, podemos ver la lista de libros agrupados por autor, o bien podemos alejar la vista para ver una lista de accesos directos a autores. |

## <a name="related-topics"></a>Temas relacionados

**Documentación**
* [Novedades para desarrolladores de Windows 10](https://dev.windows.com/getstarted/whats-new-windows-10)
* [Guía de aplicaciones para la Plataforma universal de Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/dn894631)
* [Guía básica para aplicaciones universales de Windows (UWP) con C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/br229583)
* [Siguientes pasos para los desarrolladores de Windows Phone 8](https://msdn.microsoft.com/library/windows/apps/xaml/dn655121.aspx)

**Artículos de revista**
* [Revista de VisualStudio: Windows Phone8.1, un paso de gigante hacia la convergencia](http://go.microsoft.com/fwlink/p/?LinkID=398541)

**Presentaciones**
* [La historia de traer la música de Nokia de Windows Phone a Windows8](http://go.microsoft.com/fwlink/p/?LinkId=321521)
 

