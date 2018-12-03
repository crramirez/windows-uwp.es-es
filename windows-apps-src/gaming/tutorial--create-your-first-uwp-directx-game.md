---
title: Crear un juego sencillo para la Plataforma universal de Windows (UWP) con DirectX
description: En este conjunto de tutoriales, aprenderás a crear un juego básico para la Plataforma universal de Windows (UWP) con DirectX y C++.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: Ejemplo de juego DirectX, ejemplo de juego, plataforma universal de Windows (UWP), juego Direct3D 11
ms.date: 12/01/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dc602e2dd29231c1e6554d7ef55e9666a373fa31
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8461903"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>Crear un juego sencillo para la plataforma universal de Windows (UWP) con DirectX

En este conjunto de tutoriales, aprenderás a crear un juego básico para la plataforma universal de Windows (UWP) con DirectX y C++. Se tratan las partes principales de un juego, incluidos los procesos para cargar activos, como imágenes y mallas, cómo crear un bucle de juego principal, implementar una canalización de representación simple y agregar sonido y controles.

Te enseñaremos las técnicas y consideraciones para desarrollar juegos de UWP. No proporcionamos un juego completo de principio a fin, sino que nos centramos en conceptos clave del desarrollo de juegos DirectX de UWP y tenemos en cuenta consideraciones específicas de Windows Runtime en relación con esos conceptos.

## <a name="objective"></a>Objetivo

Usar los conceptos y componentes básicos de un juego DirectX de UWP y familiarizarse con el diseño de juegos de UWP con DirectX.

## <a name="what-you-need-to-know-before-starting"></a>Lo que debes saber antes de empezar


Antes de empezar este tutorial, necesitas estar familiarizado con estos temas.

-   Microsoft C++ con extensiones de idioma para Windows Runtime (C++/CX). Se trata de una actualización de Microsoft C++ que incorpora el recuento automático de referencias y es el lenguaje de desarrollo de juegos de UWP con DirectX11.1 o versiones posteriores.
-   Álgebra lineal básica y conceptos de física newtoniana.
-   Terminología básica de programación de gráficos.
-   Conceptos básicos de programación en Windows.
-   Conocimiento básico de las API [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx) y [Direct3D11](https://msdn.microsoft.com/library/windows/desktop/hh404569).

##  <a name="direct3d-uwp-shooting-game-sample"></a>Juego de disparos de ejemplo Direct3D (UWP)


Esta muestra implementa una sencilla galería de disparos en primera persona en la que el jugador dispara bolas a objetivos en movimiento. Acertar a cada objetivo concede un determinado número de puntos, y el jugador puede avanzar a través de 6 niveles de dificultad creciente. Al final de los niveles, se cuentan los puntos y el jugador obtiene una puntuación final.

La muestra hace una demostración de los siguientes conceptos de juego:

-   Interoperación entre DirectX 11.1 y Windows Runtime
-   Una cámara y perspectiva en 3D en primera persona
-   Efectos 3D estereoscópicos
-   Detección de colisiones entre objetos en 3D
-   Control de la entrada del jugador desde controles de mouse, gestos táctiles y mando de Xbox
-   Mezcla y reproducción de audio
-   Máquina de estado básica del juego

![muestra de juego en acción](images/simple-dx-game-overview.png)

| Tema | Description |
|-------|-------------|
|[Configurar el proyecto de juego](tutorial--setting-up-the-games-infrastructure.md) | El primer paso para ensamblar el juego es configurar un proyecto en Microsoft Visual Studio de tal forma que se reduzca al mínimo la cantidad de trabajo necesaria en la infraestructura de código. Puedes ahorrarte mucho tiempo y complicaciones si usas la plantilla adecuada y configuras el proyecto específicamente para desarrollar un juego. Vamos a guiarte por los pasos de configuración de un proyecto de juego sencillo. |
| [Definir el marco de la aplicación para UWP del juego](tutorial--building-the-games-uwp-app-framework.md) | Crea un marco que permita que el objeto del juego DirectX de UWP interactúe con Windows. Esto incluye las propiedades de Windows Runtime como suspender o reanudar el control de eventos, el foco de ventanas y el acoplamiento.  |
| [Administración del flujo de los juegos](tutorial-game-flow-management.md) | Define la máquina de estado de alto nivel para habilitar al jugador y la interacción del sistema. Obtén información sobre cómo interactúa la interfaz de usuario con la máquina de estado del juego general y sobre cómo crear controladores de eventos para juegos UWP. |
| [Define el objeto principal del juego](tutorial--defining-the-main-game-loop.md) | Define cómo se juega creando reglas. |
| [Marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md) | Ensambla un marco de representación para mostrar gráficos. Esta sección se divide en dos partes. Introducción a la representación explica cómo presentar los objetos de escena para mostrarlos en pantalla. |
| [Marco de representación II: Representación de juego](tutorial-game-rendering.md) | En la segunda parte del tema de la representación, aprenderás a preparar los datos necesarios antes de la representación. |
| [Agregar una interfaz de usuario](tutorial--adding-a-user-interface.md) | Agregar sencillas opciones de menú y componentes de la pantalla de visualización frontal, proporcionando comentarios al jugador. |
| [Agregar controles](tutorial--adding-controls.md) | Agregar controles de movimiento y vista en el juego &mdash; controles táctiles básicos, de mouse y de dispositivos de juego. |
| [Agregar sonido](tutorial--adding-sound.md) | Aprende a crear los sonidos de juego usando API [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813). |
| [Ampliar el ejemplo de juego](tutorial-resources.md) | Recursos para ampliar tu conocimiento del desarrollo de juegos DirectX, incluyendo el uso de XAML para crear superposiciones. |