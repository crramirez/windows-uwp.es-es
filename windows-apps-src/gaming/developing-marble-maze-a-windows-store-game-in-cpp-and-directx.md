---
title: Desarrollo de un juego de *Marble Maze* &mdash; a plataforma universal de Windows (UWP) creado con C++ para DirectX
description: En esta sección de la documentación se describe cómo usar DirectX y C++ para crear un juego de Plataforma universal de Windows 3D (UWP).
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.date: 08/10/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, ejemplo, DirectX, 3D
ms.localizationpriority: medium
ms.openlocfilehash: d2c0c630090c178a54a0452ab3cc430ffee4a176
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409504"
---
# <a name="developing-marble-mazemdasha-universal-windows-platform-uwp-game-built-with-c-for-directx"></a>Desarrollo de un juego de *Marble Maze* &mdash; a plataforma universal de Windows (UWP) creado con C++ para DirectX

En este tema se describe cómo utilizar DirectX y C++ para crear un juego de Plataforma universal de Windows 3D (UWP). El juego, llamado *Marble Maze*, adopta varios factores de forma, como tabletas, equipos de escritorio tradicionales y equipos portátiles.

> [!NOTE]
> Para descargar el código fuente de *Marble Maze* , consulte el [ejemplo en github](https://github.com/microsoft/Windows-appsample-marble-maze).

> [!IMPORTANT]
> *Marble Maze* muestra los patrones de diseño que consideramos prácticas recomendadas para la creación de juegos para UWP. Puedes modificar muchos de los detalles de implementación para que se adapten a tus procedimientos y a los requisitos únicos del juego que desarrolles. No dudes en usar diferentes técnicas o bibliotecas si se adaptan mejor a lo que necesitas. (Sin embargo, asegúrese siempre de que el código pasa el kit para la [certificación de aplicaciones de Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)). Cuando consideramos que una implementación se usa aquí para ser esencial para el desarrollo de juegos correctamente, lo resaltamos en esta documentación.

## <a name="introducing-marble-maze"></a>Presentación de *Marble Maze*

Hemos elegido *Marble Maze* porque es relativamente básico, pero sigue mostrando el alcance de las características que se encuentran en la mayoría de los juegos. Muestra cómo usar gráficos, gestión de entradas y audio. También demuestra mecánicas de juego, como reglas y objetivos.

*Marble Maze* se parece al Juego de Labyrinth de la tabla principal que se crea normalmente a partir de un cuadro que contiene huecos y una canica de acero o de vidrio. El objetivo de *Marble Maze* es el mismo que el de la versión superior de la tabla: Incline el laberinto para guiar la canica desde el principio hasta el final del laberinto en el menor tiempo posible, sin dejar que la canica quede en ninguno de los huecos. *Marble Maze* agrega el concepto de puntos de control. Si la canica cae en un agujero, el juego se reinicia desde el último punto de control por el que pasó la canica.

*Marble Maze* ofrece varias maneras de que un usuario interactúe con el tablero de juego. Si tu dispositivo es táctil o tiene acelerómetro, puedes usarlo para mover el tablero. También puede usar un controlador de Xbox One o un mouse para controlar la reproducción de juegos.

![Captura de pantalla del juego Marble Maze.](images/marblemaze-2.png)

## <a name="prerequisites"></a>Requisitos previos

-   Windows 10 Creators Update
-   [Microsoft Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
-   Conocimientos de programación de C++
-   Familiaridad con DirectX y terminología de DirectX
-   Conocimientos básicos de COM

## <a name="who-should-read-this"></a>¿A quiénes está dirigido este documento?

Si está interesado en crear juegos 3D u otras aplicaciones con muchos gráficos para Windows 10, esto es así. Esperamos que uses los principios y procedimientos que se exponen aquí para crear tu propio juego para UWP. Tener experiencia o un gran interés en la programación con C++ y DirectX te ayudará a obtener el máximo provecho de esta documentación. Si no tiene experiencia con DirectX, puede seguir disfrutando si tiene experiencia con entornos de programación de gráficos 3D similares.

En el documento [Tutorial: creación de un juego de UWP sencillo con DirectX](tutorial--create-your-first-uwp-directx-game.md) se describe otro ejemplo que implementa un juego básico de Disparos 3D mediante DirectX y C++.

## <a name="what-this-documentation-covers"></a>Temas incluidos en esta documentación

Esta documentación enseña a:

-   Usar la API de Windows Runtime y DirectX para crear un juego para UWP.
-   Use [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) y [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal) para trabajar con contenido visual como modelos, texturas, sombreadores de vértices y píxeles y superposiciones 2D.
-   Integre mecanismos de entrada como Touch, acelerómetro y el controlador Xbox One.
-   Usar [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal) para incorporar música y efectos de sonido.

## <a name="what-this-documentation-does-not-cover"></a>Temas no incluidos en esta documentación

Esta documentación no contempla los siguientes aspectos del desarrollo de juegos. Estos aspectos se tratan en los recursos adicionales que se incluyen.

-   principios de diseño de juegos 3D.
-   Programación básica con C++ o DirectX.
-   Diseñar recursos como texturas, modelos o audio.
-   Solucionar problemas de comportamiento o rendimiento de un juego.
-   Preparar un juego para usarlo en otras partes del mundo.
-   Cómo certificar y publicar el juego en el Microsoft Store.

*Marble Maze* también usa la biblioteca de [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal) para trabajar con geometría 3D y realizar cálculos físicos, como colisiones. DirectXMath no se detalla en esta sección. Para obtener más información sobre cómo *Marble Maze* usa DirectXMath, consulte el código fuente.

Aunque *Marble Maze* proporciona muchos componentes reutilizables, no es un marco de desarrollo de juegos completo. Cuando consideramos que un componente de *Marble Maze* se puede volver a usar en el juego, lo resaltamos en la documentación.

## <a name="next-steps"></a>Pasos siguientes

Se recomienda comenzar con los [aspectos básicos del ejemplo de Marble Maze](marble-maze-sample-fundamentals.md) para obtener información sobre la estructura de *Marble Maze* y algunas de las instrucciones de codificación y estilo que sigue el código fuente de *Marble Maze* . La siguiente tabla describe los documentos de esta sección para que te resulte más fácil consultarlos.

## <a name="in-this-section"></a>En esta sección

| Title                                                                                                                    | Descripción                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Conceptos básicos sobre la muestra de Marble Maze](marble-maze-sample-fundamentals.md)                                                   | Proporciona una introducción a la estructura del juego y algunas directrices de codificación y estilo que sigue el código fuente.                                                                                                                                 |
| [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md)                                               | Describe cómo se estructura el código de aplicación de *Marble Maze* y el modo en que la estructura de una aplicación DirectX UWP es diferente de la de una aplicación de escritorio tradicional.                                                                                    |
| [Agregar contenido visual a la muestra de Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)                   | Describe algunos de los procedimientos clave a tener en cuenta cuando trabajas con Direct3D y Direct2D. También describe cómo *Marble Maze* aplica estos procedimientos para el contenido visual.                                                                           |
| [Agregar métodos de entrada e interactividad en la muestra de Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md) | Describe cómo funciona *Marble Maze* con las entradas de controlador de acelerómetro, táctil y Xbox One para permitir que los usuarios naveguen por los menús e interactúen con el tablero de juego. También describe algunos de los procedimientos recomendados a tener en cuenta cuando trabajas con la entrada. |
| [Agregar audio a la muestra de Marble Maze](adding-audio-to-the-marble-maze-sample.md)                                     | Describe cómo funciona *Marble Maze* con audio para agregar música y efectos sonoros a la experiencia del juego.                                                                                                                                                  |
