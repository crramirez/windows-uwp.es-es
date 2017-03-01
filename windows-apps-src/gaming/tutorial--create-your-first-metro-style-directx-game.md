---
author: mtoepke
title: Crear un juego de Plataforma universal de Windows (UWP) simple con DirectX
description: "En este conjunto de tutoriales, aprenderás a crear un juego básico para la Plataforma universal de Windows (UWP) con DirectX y C++."
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords:
- Muestra de juego DirectX
- muestra de juego, Plataforma universal de Windows (UWP)
- juego Direct3D 11
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3ffce29c3ad7088dd24b848cb159b85a4db158e3
ms.lasthandoff: 02/07/2017

---

# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>Crear un juego de Plataforma universal de Windows (UWP) simple con DirectX


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este conjunto de tutoriales, aprenderás a crear un juego de Plataforma universal de Windows (UWP) básico con DirectX y C++. Se tratan las partes principales de un juego, incluidos los procesos para cargar activos, como imágenes y mallas, cómo crear un bucle de juego principal, implementar una canalización de representación simple y agregar sonido y controles.

Te enseñaremos las técnicas y consideraciones para desarrollar juegos de UWP. No proporcionamos un juego completo de principio a fin, sino que nos centramos en conceptos clave del desarrollo de juegos DirectX de UWP y tenemos en cuenta consideraciones específicas de Windows Runtime en relación con esos conceptos.

## <a name="objective"></a>Objetivo


-   Usar los conceptos y componentes básicos de un juego DirectX de UWP y familiarizarse con el diseño de juegos de UWP con DirectX.

## <a name="what-you-need-to-know-before-starting"></a>Lo que debes saber antes de empezar


Antes de empezar este tutorial, necesitas estar familiarizado con estos temas.

-   Microsoft C++ con Component Extensions (C++/CX). Se trata de una actualización de Microsoft C++ que incorpora el recuento automático de referencias y es el lenguaje de desarrollo de juegos de UWP con DirectX 11.1 o versiones posteriores.
-   Álgebra lineal básica y conceptos de física newtoniana.
-   Terminología básica de programación de gráficos.
-   Conceptos básicos de programación en Windows.
-   Conocimiento básico de las API [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) y [Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh404569).

##  <a name="the-windows-store-direct3d-shooting-game-sample"></a>La muestra de juego de disparos Direct3D de la Tienda Windows


Esta muestra implementa una sencilla galería de disparos en primera persona en la que el jugador dispara bolas a objetivos en movimiento. Acertar a cada objetivo concede un determinado número de puntos, y el jugador puede avanzar a través de 6 niveles de dificultad creciente. Al final de los niveles, se cuentan los puntos y el jugador obtiene una puntuación final.

La muestra hace una demostración de los siguientes conceptos de juego:

-   Interoperación entre DirectX 11.1 y Windows Runtime
-   Una cámara y perspectiva en 3D en primera persona
-   Efectos 3D estereoscópicos
-   Detección de colisiones entre objetos en 3D
-   Administración de la entrada del jugador para controles de mouse, táctiles y mando de la Xbox 360
-   Mezcla y reproducción de audio
-   Máquina de estado básica del juego

![muestra de juego en acción](images/simple3dgame-display.png)


| Tema | Description |
|---------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Configurar el proyecto de juego](tutorial--setting-up-the-games-infrastructure.md) | El primer paso para ensamblar el juego es configurar un proyecto en Microsoft Visual Studio de tal forma que se reduzca al mínimo la cantidad de trabajo necesaria en la infraestructura de código. Puedes ahorrarte mucho tiempo y complicaciones si usas la plantilla adecuada y configuras el proyecto específicamente para desarrollar un juego. Vamos a guiarte por los pasos de configuración de un proyecto de juego sencillo. |
| [Definir el marco de la aplicación para UWP del juego](tutorial--building-the-games-metro-style-app-framework.md) | La primera parte de codificar un juego de UWP con DirectX es crear el marco que permite al objeto de juego interactuar con Windows. Esto incluye propiedades de Windows Runtime como la suspensión o reanudación del control de eventos, el foco de la ventana y el acoplamiento, además de los eventos, interacciones y transiciones para la interfaz de usuario. Examinaremos cómo está estructurado el juego de muestra y cómo define la máquina de estado general para la interacción entre el jugador y el sistema. |
| [Definir el objeto principal del juego](tutorial--defining-the-main-game-loop.md) | Vamos a ver los detalles del objeto principal de la muestra de juego y cómo las reglas que implementa se traducen en interacciones con el mundo del juego. |
| [Ensamblar el marco de representación](tutorial--assembling-the-rendering-pipeline.md) | Ahora vamos a ver cómo el juego de muestra usa esa estructura y ese estado para mostrar sus gráficos. Aquí echaremos un vistazo a la implementación de un marco de representación, empezando por la inicialización del dispositivo gráfico hasta la presentación de los objetos gráficos para mostrar. |
| [Agregar una interfaz de usuario](tutorial--adding-a-user-interface.md) | Has visto cómo el juego de muestra implementa el objeto principal del juego, así como el marco de representación básico. Ahora, veamos cómo el juego de muestra ofrece comentarios sobre el estado de la partida al jugador. Aquí aprenderás a agregar sencillas opciones de menú y componentes de pantalla de visualización frontal encima de la salida de la canalización de gráficos 3-D. |
| [Agregar controles](tutorial--adding-controls.md) | Echemos un vistazo ahora al modo en que la muestra de juego implementa los controles de movimiento y vista en un juego en 3-D, y a cómo desarrollar controles básicos para mandos, toque y mouse. |
| [Agregar sonido](tutorial--adding-sound.md) | En este paso, examinaremos cómo la muestra de juego de disparos crea un objeto para la reproducción de sonido usando las API de [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) . |
| [Extender la muestra de juego](tutorial-resources.md) | ¡Enhorabuena! Llegados a este punto, comprendes los componentes clave de un juego UWP 3D DirectX. Puedes establecer el marco para un juego, incluida la canalización de representación y el proveedor de vista, e implementar un bucle de juego básico. También puedes crear una superposición de interfaz de usuario básica e incorporar sonidos y controles. Ya estás preparado para crear un juego por ti mismo, y a continuación te mostramos algunos recursos para ampliar tu conocimiento sobre el desarrollo de juegos DirectX. |
 

 

 





