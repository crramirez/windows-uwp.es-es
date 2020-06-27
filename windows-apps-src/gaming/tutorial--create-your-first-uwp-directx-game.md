---
title: Creación de un juego de DirectX Plataforma universal de Windows (UWP)
description: En este conjunto de tutoriales, aprenderá a usar DirectX y [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) para crear el juego de ejemplo básico plataforma universal de Windows (UWP) denominado **Simple3DGameDX**.
ms.assetid: 9edc5868-38cf-58cc-1fb3-8fb85a7ab2c9
keywords: Juego de ejemplo de DirectX, juego de ejemplo, Plataforma universal de Windows (UWP), juego Direct3D 11
ms.date: 06/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2e3007cd79546cba8961000cb2aae44b0b0536fe
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409574"
---
# <a name="create-a-simple-universal-windows-platform-uwp-game-with-directx"></a>Crear un juego de Plataforma universal de Windows (UWP) simple con DirectX

En este conjunto de tutoriales, aprenderá a usar DirectX y [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) para crear el juego de ejemplo básico plataforma universal de Windows (UWP) denominado **Simple3DGameDX**. El juego tiene lugar en una sencilla Galería de Disparos 3D de primera persona.

> [!NOTE]
> El vínculo desde el que puede descargar el juego de ejemplo **Simple3DGameDX** es un [juego de ejemplo de Direct3D](/samples/microsoft/windows-universal-samples/simple3dgamedx/). El código fuente de C++/WinRT se encuentra en la carpeta denominada `cppwinrt` . Para obtener información sobre otras aplicaciones de ejemplo de UWP, consulte [obtener ejemplos de aplicaciones para UWP](/windows/uwp/get-started/get-uwp-app-samples).

Estos tutoriales cubren todas las partes principales de un juego, incluidos los procesos de carga de recursos como artes y mallas, la creación de un bucle principal del juego, la implementación de una canalización de representación simple y la adición de sonido y controles.

También verá las técnicas y las consideraciones sobre el desarrollo de juegos de UWP. Nos centraremos en los conceptos clave de desarrollo de juegos de DirectX de UWP y llamaremos a las consideraciones específicas del tiempo de ejecución de Windows en relación con estos conceptos.

## <a name="objective"></a>Objetivo

Para obtener información sobre los conceptos básicos y los componentes de un juego DirectX de UWP, y para familiarizarse con el diseño de juegos UWP con DirectX.

## <a name="what-you-need-to-know"></a>Aspectos que debe saber

Para este tutorial, debe estar familiarizado con estos temas.

- [/WinRT de C++](/windows/uwp/cpp-and-winrt-apis/). C++/WinRT es una proyección moderna estándar de lenguaje C++ 17 para API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivos de encabezado y diseñada para proporcionarle acceso de primera clase a las API modernas de Windows.
- Álgebra lineal básica y conceptos de física newtoniana.
- Terminología básica de programación de gráficos.
- Conceptos básicos de programación en Windows.
- Conocimiento básico de las API [Direct2D](/windows/desktop/Direct2D/direct2d-portal) y [Direct3D 11](/windows/desktop/direct3d11/how-to-use-direct3d-11).

##  <a name="direct3d-uwp-shooting-gallery-sample"></a>Ejemplo de galería de disparos de UWP de Direct3D

El juego de ejemplo **Simple3DGameDX** implementa una sencilla Galería de Disparos 3D de primera persona, donde el jugador activa las bolas en los objetivos móviles. Acertar a cada objetivo concede un determinado número de puntos, y el jugador puede avanzar a través de 6 niveles de dificultad creciente. Al final de los niveles, se cuentan los puntos y el jugador obtiene una puntuación final.

En el ejemplo se muestran estos conceptos de juegos.

- Interoperación entre DirectX 11.1 y Windows Runtime
- Una cámara y perspectiva en 3D en primera persona
- Efectos 3D estereoscópicos
- Detección de colisiones entre objetos en 3D
- Controlar la entrada del reproductor para los controles del mouse, el toque y la controladora Xbox
- Mezcla y reproducción de audio
- Estado básico de un juego: equipo

![el juego de ejemplo en acción](images/simple-dx-game-overview.png)

|Tema|Descripción|
|-------|-------------|
|[Configurar el proyecto de juego](tutorial--setting-up-the-games-infrastructure.md)|El primer paso para desarrollar el juego consiste en configurar un proyecto en Microsoft Visual Studio. Después de configurar un proyecto específicamente para el desarrollo de juegos, puede volver a usarlo como un tipo de plantilla.|
|[Definir el marco de la aplicación para UWP del juego](tutorial--building-the-games-uwp-app-framework.md)|El primer paso para codificar un juego de Plataforma universal de Windows (UWP) es crear el marco de trabajo que permite que el objeto de aplicación interactúe con Windows.|
|[Administración de flujo de juegos](tutorial-game-flow-management.md)|Defina el equipo de estado de alto nivel para habilitar la interacción del reproductor y del sistema. Obtenga información sobre cómo interactúa la interfaz de usuario con el equipo de estado general del juego y cómo crear controladores de eventos para juegos UWP.|
|[Definir el objeto principal del juego](tutorial--defining-the-main-game-loop.md)|Ahora, veremos los detalles del objeto principal del juego de ejemplo y cómo las reglas que implementa se traducen en interacciones con el mundo de juegos.|
|[Marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md)|Obtenga información sobre cómo desarrollar la canalización de representación para mostrar los gráficos. Introducción a la representación.|
|[Plataforma de representación II: representación de juego](tutorial-game-rendering.md)|Obtenga información sobre cómo ensamblar la canalización de representación para mostrar los gráficos. Reproducción de juegos, configurar y preparar datos.|
|[Agregar una interfaz de usuario](tutorial--adding-a-user-interface.md)|Obtenga información acerca de cómo agregar una superposición de interfaz de usuario 2D a un juego de DirectX UWP.|
|[Agregar controles](tutorial--adding-controls.md)|Ahora, echemos un vistazo a cómo el juego de ejemplo implementa los controles de movimiento y desplazamiento en un juego 3D, y cómo desarrollar controles táctiles básicos, del mouse y del controlador de juegos.|
|[Agregar sonido](tutorial--adding-sound.md)|Desarrolle un motor de sonido sencillo con las API de XAudio2 para reproducir música y efectos sonoros de juegos.|
|[Extender el juego de ejemplo](tutorial-resources.md)|Obtenga información sobre cómo implementar una superposición de XAML para un juego DirectX de UWP.|
