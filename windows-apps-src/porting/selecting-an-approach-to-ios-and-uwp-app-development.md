---
author: mcleblanc
description: ¿Cuáles son las opciones al desarrollar aplicaciones multiplataforma?
title: Selección de un enfoque para iOS y desarrollo de aplicaciones para UWP
ms.assetid: 5CDAB313-07B7-4A32-A49B-026361DCC853
---

# Selección de un enfoque para iOS y desarrollo de aplicaciones para UWP

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

¿Cuáles son las opciones al desarrollar aplicaciones multiplataforma?

## ¿Cuál es la mejor forma de compatibilidad de iOS y Windows?

Windows e iOS pueden parecer muy diferentes, pero un creciente número de herramientas y técnicas pueden ayudarte enormemente si tienes que escribir aplicaciones que admitan ambas plataformas (y Android también). La mejor solución depende del tipo de aplicación que escribas, además de si empiezas desde cero o migras un proyecto existente.

## Escribir una nueva aplicación

Con una tableta táctil vacía, tienes muchas opciones a tu disposición, incluidos:

-   [Xamarin](http://go.microsoft.com/fwlink/p/?LinkID=320484)

    Con Xamarin, puedes escribir tu aplicación en C#, ejecutarla en Windows y crear aplicaciones de iOS nativo también. La compatibilidad con Xamarin está integrada en Visual Studio; solo tienes que seleccionar el tipo de proyecto correcto.

-   [Apache Cordova](http://go.microsoft.com/fwlink/p/?LinkID=400439)

    Si eres más de Javascript y HTML, Apache Cordova (también conocido como PhoneGap) te ayudará a crear aplicaciones multiplataforma para iOS, Windows y Android. Este tipo de proyecto también está integrado en Visual Studio.

-   Motores de juegos

    Con herramientas como [Unity3D](http://go.microsoft.com/fwlink/p/?LinkID=320479) y [Unreal Engine](http://go.microsoft.com/fwlink/p/?LinkID=394062) a tu disposición, puedes escribir juegos de calidad AAA para Windows y muchas otras plataformas, como iOS. Unity admite scripting de C#; Unreal usa C++.

-   [MonoGame](http://go.microsoft.com/fwlink/p/?LinkID=320483)

    El sucesor espiritual de XNA. Ahora es un marco multiplataforma de código abierto, lo que significa que puedes escribir aplicaciones en C# para muchas plataformas con compatibilidad para motores físicos, así como para gráficos 2D y 3D.

## Adaptación de una aplicación existente

Con una aplicación de iOS existente, las opciones son un poco más limitadas. Sin embargo, no todo está perdido.

-   [Puente de Windows para iOS](https://go.microsoft.com/fwlink/p/?LinkId=619014)

    También conocido como Project Islandwood, es una herramienta aún en desarrollo que puede importar proyectos Xcode directamente a Visual Studio. El código de Objective-C se puede compilar y depurar desde Visual Studio. Si el proyecto hace uso de bibliotecas como Cocos para elementos gráficos, puedes encontrar que es una manera útil de portar rápidamente la aplicación.

-   Reusa el código de C++.

    Si la lógica de negocios principal está escrita en C++, en lugar de Objective-C o Swift, a menudo se puede usar este código con solo cambios menores en el proyecto. Después puede usar XAML para definir la interfaz de usuario, como con otras aplicaciones de Windows y llamar al código de C++ cuando sea necesario.

-   [Uso de ANGLE para ejecutar OpenGL ES en Windows](http://go.microsoft.com/fwlink/p/?linkid=618387)

    Un paso intermedio para migrar el proyecto de OpenGL ES 2.0 es usar ANGLE. ANGLE te permite ejecutar contenido de OpenGL ES en Windows mediante la conversión de llamadas a la API de OpenGL ES en llamadas a la API de DirectX 11.

## Otras herramientas de creación multiplataforma

-   [GameSalad](http://go.microsoft.com/fwlink/p/?LinkID=320480)

    Un entorno de creación de juegos.

-   [Construct 2]( http://go.microsoft.com/fwlink/p/?LinkID=320481)

    Un entorno de creación de juegos.

-   [Titanium Studio](http://go.microsoft.com/fwlink/p/?LinkID=320482)

    Un entorno de creación de multiplataforma.

-   [Cocos2D-x](http://go.microsoft.com/fwlink/p/?LinkID=320485)

    Una biblioteca de código multiplataforma para la manipulación de sprites y el modelado físico.

-   [Impact.js](http://go.microsoft.com/fwlink/p/?LinkID=320486)

    Una biblioteca de juegos basados en HTML.

-   [Marmalade](http://go.microsoft.com/fwlink/p/?LinkID=320487)

    Un SDK multiplataforma.

-   [OpenFL](http://go.microsoft.com/fwlink/p/?LinkID=320488)

    Una herramienta de desarrollo multiplataforma.

-   [GameMaker](http://go.microsoft.com/fwlink/p/?LinkID=320490)

    Un entorno de creación específico para juegos.

-   [PlayCanvas](http://go.microsoft.com/fwlink/p/?LinkID=394061)

    Una herramienta para desarrollo de juegos basados en HTML.



<!--HONumber=May16_HO2-->


