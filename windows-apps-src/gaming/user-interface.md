---
title: Plantillas de proyectos de juegos DirectX
description: Obtén información acerca de las plantillas para crear un juego de DirectX y la Plataforma universal de Windows (UWP).
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX, plantillas
ms.localizationpriority: medium
ms.openlocfilehash: 99d68be96858249fdd314857e1fe6d3020ee56e6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159039"
---
# <a name="directx-game-project-templates"></a>Plantillas de proyectos de juegos DirectX



Las plantillas de DirectX y UWP (Plataforma universal de Windows) te permiten crear rápidamente un proyecto como punto de partida para tu juego.

## <a name="prerequisites"></a>Requisitos previos


Para crear el proyecto, debes:

-   [Descargar Microsoft Visual Studio 2015](https://visualstudio.microsoft.com/vs/). Visual Studio 2015 tiene herramientas para programación de gráficos, como herramientas de depuración. Para obtener información general sobre las herramientas y las funciones de juegos y elementos gráficos de DirectX, consulta [Herramientas de Visual Studio para programación de juegos DirectX](set-up-visual-studio-for-game-development.md).

## <a name="choosing-a-template"></a>Elección de una plantilla


Visual Studio 2015 incluye tres plantillas de DirectX y UWP:

-   DirectX 11 App (Universal Windows): la plantilla DirectX 11 App (Universal Windows) crea un proyecto de UWP, que representa directamente en una ventana de aplicación con DirectX 11.
-   DirectX 12 App (Universal Windows): la plantilla DirectX 12 App (Universal Windows) crea un proyecto de UWP, que representa directamente en una ventana de aplicación con DirectX 12.
-   DirectX 11 and XAML App (Universal Windows): la plantilla DirectX 11 and XAML App (Universal Windows) crea un proyecto de UWP, que representa en un control XAML con DirectX 11. Esta plantilla usa un [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel), para que puedas usar los controles de la interfaz de usuario XAML. Esto puede facilitar la adición de elementos de interfaz de usuario pero el uso de la plantilla XAML puede reducir el rendimiento.

La plantilla que elijas dependerá del rendimiento y de las tecnologías que quieras usar.

## <a name="template-structure"></a>Estructura de plantilla


Las plantillas de DirectX de Windows universal contienen los siguientes archivos:

-   pch.h y pch.cpp: compatibilidad con encabezado precompilado.
-   Package.appxmanifest: propiedades del paquete de implementación de la aplicación.
-   \*. pfx: certificados para la aplicación.
-   Dependencias externas: vínculos a archivos externos que usa el proyecto.
-   \*Main. h y \* Main. cpp: métodos para administrar los recursos de la aplicación, actualizar el estado de la aplicación y representar el marco.
-   App.h y App.cpp: punto de entrada principal de la aplicación. Conecta la aplicación con el shell de Windows y controla los eventos del ciclo de vida de la aplicación. Estos archivos solo aparecen en las plantillas DirectX 11 App (aplicación universal de Windows) y DirectX 12 App (aplicación universal de Windows).
-   App.xaml, App.xaml.cpp y App.xaml.h: punto de entrada principal de la aplicación. Conecta la aplicación con el shell de Windows y controla los eventos del ciclo de vida de la aplicación. Estos archivos solo aparecen en la plantilla DirectX 11 and XAML App (Universal Windows).
-   DirectXPage.xaml, DirectXPage.xaml.cpp y DirectXPage.xaml.h: una página que hospeda un SwapChainPanel de DirectX. Estos archivos solo aparecen en la plantilla DirectX 11 and XAML App (Universal Windows).
-   Contenido
    -   Sample3DSceneRenderer.h y Sample3DSceneRenderer.cpp: un representador de muestra que crea una instancia de una canalización de representación básica.
    -   SampleFpsTextRenderer.h y SampleFpsTextRenderer.cpp: representa el valor FPS actual en la parte inferior derecha de la pantalla con Direct2D y DirectWrite. Estos archivos solo aparecen en las plantillas DirectX 11 App (aplicación universal de Windows) y DirectX 11 and XAML App (aplicación universal de Windows).
    -   SamplePixelShader.hlsl: un sombreador de píxeles de ejemplo sencillo.
    -   SampleVertexShader.hlsl: un sombreador de vértices de ejemplo sencillo.
    -   ShaderStructures.h: estructuras que se usan para enviar los datos al sombreador de vértices de ejemplo.
-   Comunes
    -   StepTimer.h: una clase auxiliar para la animación y los intervalos de simulación.
    -   DirectXHelper.h: funciones auxiliares diversas.
    -   DeviceResources.h y Device Resources.cpp: proporciona una interfaz para una aplicación que posee DeviceResources que deben recibir una notificación cuando se pierde o se crea el dispositivo.
    -   d3dx12.h: contiene la biblioteca de utilidades D3DX12. Este archivo solo aparece en la plantilla DirectX 12 App (Universal Windows).
-   Assets: imágenes de logotipo y pantalla de bienvenida que usa la aplicación.

## <a name="next-steps"></a>Pasos siguientes


Ahora que tiene un punto de partida, agréguelo a él para compilar los conocimientos de desarrollo de juegos y Microsoft Store habilidades de desarrollo de juegos.

Si estás migrando un juego existente, consulta los siguientes temas.

-   [Portar desde OpenGL ES 2.0 a Direct3D 11.1](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [Portar de DirectX 9 a la Plataforma universal de Windows](porting-your-directx-9-game-to-windows-store.md)

Si estás creando un nuevo juego DirectX, consulta los siguientes temas.

-   [Crear un juego para UWP sencillo con DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Desarrollo de Marble Maze, un juego para la Plataforma universal de Windows en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)