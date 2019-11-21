---
description: ¿Cuáles son las opciones al desarrollar aplicaciones multiplataforma?
title: Selección de un enfoque para iOS y desarrollo de aplicaciones para UWP
ms.assetid: 5CDAB313-07B7-4A32-A49B-026361DCC853
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a76b451a3d268e418ae24998afdd29d32bb43ed6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260135"
---
# <a name="selecting-an-approach-to-ios-and-uwp-app-development"></a>Selección de un enfoque para iOS y desarrollo de aplicaciones para UWP


¿Cuáles son las opciones al desarrollar aplicaciones multiplataforma?

## <a name="whats-the-best-way-to-support-both-ios-and-windows"></a>¿Cuál es la mejor forma de compatibilidad de iOS y Windows?

Windows e iOS pueden parecer muy diferentes, pero un creciente número de herramientas y técnicas pueden ayudarte enormemente si tienes que escribir aplicaciones que admitan ambas plataformas (y Android también). La mejor solución depende del tipo de aplicación que escribas, además de si empiezas desde cero o migras un proyecto existente.

## <a name="writing-a-new-app"></a>Escribir una nueva aplicación

Con una tableta táctil vacía, tienes muchas opciones a tu disposición, incluidos:

-   [Xamarin](https://xamarin.com/)

    Con Xamarin, puedes escribir tu aplicación en C#, ejecutarla en Windows y crear aplicaciones de iOS nativo también. La compatibilidad con Xamarin está integrada en Visual Studio; solo tienes que seleccionar el tipo de proyecto correcto.

-   [Apache Cordova](https://www.microsoft.com/?ref=go)

    Si eres más de Javascript y HTML, Apache Cordova (también conocido como PhoneGap) te ayudará a crear aplicaciones multiplataforma para iOS, Windows y Android. Este tipo de proyecto también está integrado en Visual Studio.

-   Motores de juegos

    Con herramientas como [Unity3D](https://www.unity3d.com/) y [Unreal Engine](https://www.unrealengine.com/en-US/) a tu disposición, puedes escribir juegos de calidad AAA para Windows y muchas otras plataformas, como iOS. Unity admite scripting de C#; Unreal usa C++.

-   [MonoGame](http://www.monogame.net/)

    El sucesor espiritual de XNA. Ahora es un marco multiplataforma de código abierto, lo que significa que puedes escribir aplicaciones en C# para muchas plataformas con compatibilidad para motores físicos, así como para gráficos 2D y 3D.

## <a name="adapting-an-existing-app"></a>Adaptación de una aplicación existente

Con una aplicación de iOS existente, las opciones son un poco más limitadas. Sin embargo, no todo está perdido.

-   [Puente de Windows para iOS](https://github.com/Microsoft/WinObjC)

    También conocido como Project Islandwood, es una herramienta aún en desarrollo que puede importar proyectos Xcode directamente a Visual Studio. El código de Objective-C se puede compilar y depurar desde Visual Studio. Si el proyecto hace uso de bibliotecas como Cocos para elementos gráficos, puedes encontrar que es una manera útil de portar rápidamente la aplicación.

-   Reusa el código de C++.

    Si la lógica de negocios principal está escrita en C++, en lugar de Objective-C o Swift, a menudo se puede usar este código con solo cambios menores en el proyecto. Después puede usar XAML para definir la interfaz de usuario, como con otras aplicaciones de Windows y llamar al código de C++ cuando sea necesario.

-   [Usar el ángulo para ejecutar OpenGL ES en Windows](https://github.com/microsoft/angle/wiki)

    Un paso intermedio para migrar el proyecto de OpenGL ES 2.0 es usar ANGLE. ANGLE te permite ejecutar contenido de OpenGL ES en Windows mediante la conversión de llamadas a la API de OpenGL ES en llamadas a la API de DirectX 11.

## <a name="other-cross-platform-authoring-tools"></a>Otras herramientas de creación multiplataforma

-   [GameSalad](https://gamesalad.com/)

    Un entorno de creación de juegos.

-   [Construcción 2]( https://go.microsoft.com/fwlink/p/?LinkID=320481)

    Un entorno de creación de juegos.

-   [Titanium Studio](https://www.appcelerator.com/platform/titanium-studio/)

    Un entorno de creación de multiplataforma.

-   [Cocos2D-x](https://www.cocos2d-x.org/)

    Una biblioteca de código multiplataforma para la manipulación de sprites y el modelado físico.

-   [Impact. js](https://impactjs.com/)

    Una biblioteca de juegos basados en HTML.

-   [Mermelada](http://madewithmarmalade.com/)

    Un SDK multiplataforma.

-   [OpenFL](https://www.openfl.org/)

    Una herramienta de desarrollo multiplataforma.

-   [GameMaker](https://www.yoyogames.com/gamemaker/studio)

    Un entorno de creación específico para juegos.

-   [PlayCanvas](https://playcanvas.com/)

    Una herramienta para desarrollo de juegos basados en HTML.

