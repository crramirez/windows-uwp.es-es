---
author: eliotcowley
title: Desarrollo de Marble Maze, un juego para UWP en C++ y DirectX
description: "Esta sección de la documentación describe cómo usar DirectX y Visual C++ para crear un juego 3D de la Plataforma universal de Windows (UWP)."
ms.assetid: 43f1977a-7e1d-614c-696e-7669dd8a9cc7
ms.author: elcowle
ms.date: 08/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, juegos, games, ejemplo, sample, DirectX, 3d
ms.openlocfilehash: dbfd6e6ef249ed8e0f3793068728db359d199a7b
ms.sourcegitcommit: ae20971c4c8276034cd22fd7e10b0e3ddfddf480
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="developing-marble-maze-a-uwp-game-in-c-and-directx"></a>Desarrollo de Marble Maze, un juego para UWP en C++ y DirectX


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este tema describe cómo usar DirectX y Visual C++ para crear un juego 3D de la Plataforma universal de Windows (UWP). El juego, llamado Marble Maze, comprende múltiples factores de forma, como tabletas, así como los tradicionales PC de escritorio y portátiles.

> [!NOTE]
> Para descargar el código fuente de Marble Maze, consulta la [muestra en GitHub](http://go.microsoft.com/fwlink/?LinkId=624011).

> [!IMPORTANT]
> Los patrones de diseño de Marble Maze son un ejemplo de lo que consideramos procedimientos recomendados para crear juegos para UWP. Puedes modificar muchos de los detalles de implementación para que se adapten a tus procedimientos y a los requisitos únicos del juego que desarrolles. No dudes en usar diferentes técnicas o bibliotecas si se adaptan mejor a lo que necesitas. (Sin embargo, asegúrate siempre de que el código pase el [Kit para la certificación de aplicaciones en Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit)). Cuando consideramos que una implementación usada aquí es esencial para el desarrollo correcto de juegos, lo resaltamos en esta documentación.

 

## <a name="introducing-marble-maze"></a>Presentación de Marble Maze


Hemos elegido Marble Maze porque, si bien es relativamente básico, demuestra la gran variedad de características presentes en la mayoría de los juegos. Muestra cómo usar gráficos, gestión de entradas y audio. También demuestra mecánicas de juego, como reglas y objetivos.

Marble Maze se asemeja al juego de tablero de laberinto que se suele construir a partir de una caja con agujeros y una canica de acero o vidrio. El objetivo en Marble Maze es igual que en la versión de tablero: inclinar el laberinto para conducir la canica desde la salida hasta el final del laberinto en el menor tiempo posible, evitando que caiga dentro de uno de los agujeros. Marble Maze añade el concepto de puntos de control. Si la canica cae en un agujero, el juego se reinicia desde el último punto de control por el que pasó la canica.

Marble Maze permite que el usuario interacciones con el tablero del juego de varias formas. Si tu dispositivo es táctil o tiene acelerómetro, puedes usarlo para mover el tablero. También puedes usar un controlador de Xbox One o un ratón para controlar el juego.

![Captura de pantalla del juego Marble Maze.](images/marblemaze-2.png)

## <a name="prerequisites"></a>Requisitos previos


-   Windows 10 Creators Update
-   [Microsoft Visual Studio 2017](https://www.visualstudio.com/downloads/)
-   Conocimientos de programación de C++
-   Familiaridad con DirectX y terminología de DirectX
-   Conocimientos básicos de COM

## <a name="who-should-read-this"></a>¿A quién está dirigida esta documentación?


Si estás interesado en crear juegos 3D u otras aplicaciones con uso intensivo de gráficos para Windows10, esta documentación es para ti. Esperamos que uses los principios y procedimientos que se exponen aquí para crear tu propio juego para UWP. Tener experiencia o un gran interés en la programación con C++ y DirectX te ayudará a obtener el máximo provecho de esta documentación. Si no tienes experiencia con DirectX, también te ayudará si tienes experiencia con entornos de programación de gráficos 3D similares.

El documento [Tutorial: crear un juego simple para UWP con DirectX](tutorial--create-your-first-metro-style-directx-game.md) describe otra muestra que implementa un juego de disparos 3D con DirectX y C++.

## <a name="what-this-documentation-covers"></a>Temas incluidos en esta documentación


Esta documentación enseña a:

-   Usar la API de Windows Runtime y DirectX para crear un juego para UWP.
-   Usar [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476080) y [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) para trabajar con contenido visual, como modelos, texturas, sombreadores de vértices y píxeles, y superposiciones 2D.
-   Integrar mecanismos de entrada, como la entrada táctil, el acelerómetro y el controlador de Xbox One.
-   Usar [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) para incorporar música y efectos de sonido.

## <a name="what-this-documentation-does-not-cover"></a>Temas no incluidos en esta documentación


Esta documentación no contempla los siguientes aspectos del desarrollo de juegos. Estos aspectos se tratan en los recursos adicionales que se incluyen.

-   Principios de diseño de juegos 3D.
-   Programación básica con C++ o DirectX.
-   Diseñar recursos como texturas, modelos o audio.
-   Solucionar problemas de comportamiento o rendimiento de un juego.
-   Preparar un juego para usarlo en otras partes del mundo.
-   Certificar y publicar un juego en la Tienda Windows.

Marble Maze también usa la biblioteca [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) para trabajar con la geometría 3D y realizar cálculos físicos, como colisiones. DirectXMath no se detalla en esta sección. Para obtener más detalles sobre cómo Marble Maze usa DirectXMath, consulta el código fuente.

Aunque Marble Maze proporciona muchos componentes reutilizables, no es un marco completo de desarrollo de juegos. Cuando consideramos que un componente de Marble Maze se puede reusar en tu juego, hacemos hincapié en ello en la documentación.

## <a name="next-steps"></a>Pasos siguientes


Te recomendamos que empieces con los [aspectos básicos de la muestra de Marble Maze](marble-maze-sample-fundamentals.md) para conocer la estructura de Marble Maze y algunas de las directrices de codificación y estilo que sigue el código fuente de Marble Maze. La siguiente tabla describe los documentos de esta sección para que te resulte más fácil consultarlos.

## <a name="in-this-section"></a>En esta sección


| Título                                                                                                                    | Descripción                                                                                                                                                                                                                                        |
|--------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Conceptos básicos sobre la muestra de Marble Maze](marble-maze-sample-fundamentals.md)                                                   | Proporciona una introducción a la estructura del juego y algunas directrices de codificación y estilo que sigue el código fuente.                                                                                                                                 |
| [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md)                                               | Describe cómo se estructura el código de la aplicación Marble Maze y las diferencias entre la estructura de una aplicación DirectX para UWP y la de una aplicación de escritorio tradicional.                                                                                    |
| [Agregar contenido visual a la muestra de Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)                   | Describe algunos de los procedimientos clave a tener en cuenta cuando trabajas con Direct3D y Direct2D. También describe cómo Marble Maze aplica estos procedimientos al contenido visual.                                                                           |
| [Agregar métodos de entrada e interactividad en la muestra de Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md) | Describe cómo funciona Marble Maze con acelerómetros, funciones táctiles y entradas de controladores de Xbox One para permitir a los usuarios navegar por los menús e interactuar con el tablero de juego. También describe algunos de los procedimientos recomendados a tener en cuenta cuando trabajas con la entrada. |
| [Agregar audio a la muestra de Marble Maze](adding-audio-to-the-marble-maze-sample.md)                                     | Describe cómo funciona Marble Maze con audio para agregar música y efectos de sonido a la experiencia de juego.                                                                                                                                                  |

 

 

 




