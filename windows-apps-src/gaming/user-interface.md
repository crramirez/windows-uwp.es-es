---
title: Plantillas de proyecto de juegos DirectX
description: Obtén información acerca de las plantillas para crear un juego de DirectX y la Plataforma universal de Windows (UWP).
ms.assetid: 41b6cd76-5c9a-e2b7-ef6f-bfbf6ef7331d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP, games, directx, plantillas
ms.localizationpriority: medium
ms.openlocfilehash: 9a4491fe9a3bb97a73652c40a2968f2f53c377b5
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8750746"
---
# <a name="directx-game-project-templates"></a>Plantillas de proyectos de juegos DirectX



Las plantillas de DirectX y UWP (Plataforma universal de Windows) te permiten crear rápidamente un proyecto como punto de partida para tu juego.

## <a name="prerequisites"></a>Requisitos previos


Para crear el proyecto, debes:

-   [Descargar Microsoft Visual Studio2015](https://www.visualstudio.com/vs-2015-product-editions). Visual Studio2015 tiene herramientas para programación, como herramientas de depuración de gráficos. Para obtener información general sobre las herramientas y las funciones de juegos y elementos gráficos de DirectX, consulta [Herramientas de Visual Studio para programación de juegos DirectX](set-up-visual-studio-for-game-development.md).

## <a name="choosing-a-template"></a>Elección de una plantilla


Studio2015 Visual incluye tres plantillas de DirectX y UWP:

-   DirectX11 App (Universal Windows): la plantilla DirectX11 App (Universal Windows) crea un proyecto de UWP, que representa directamente en una ventana de aplicación con DirectX11.
-   DirectX12 App (Universal Windows): la plantilla DirectX12 App (Universal Windows) crea un proyecto de UWP, que representa directamente en una ventana de aplicación con DirectX12.
-   DirectX11 and XAML App (Universal Windows): la plantilla DirectX11 and XAML App (Universal Windows) crea un proyecto de UWP, que representa en un control XAML con DirectX11. Esta plantilla usa un [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834), para que puedas usar los controles de la interfaz de usuario XAML. Esto puede facilitar la adición de elementos de interfaz de usuario pero el uso de la plantilla XAML puede reducir el rendimiento.

La plantilla que elijas dependerá del rendimiento y de las tecnologías que quieras usar.

## <a name="template-structure"></a>Estructura de la plantilla


Las plantillas de DirectX de Windows universal contienen los siguientes archivos:

-   pch.h y pch.cpp: compatibilidad con encabezado precompilado.
-   Package.appxmanifest: propiedades del paquete de implementación de la aplicación.
-   \*.pfx: certificados de la aplicación.
-   Dependencias externas: vínculos a archivos externos que usa el proyecto.
-   *Main.h y *Main.cpp: métodos para administrar activos de la aplicación, actualizar el estado de la aplicación y representar el cuadro.
-   App.h y App.cpp: punto de entrada principal de la aplicación. Conecta la aplicación con el shell de Windows y controla los eventos del ciclo de vida de la aplicación. Estos archivos solo aparecen en las plantillas DirectX11 App (aplicación universal de Windows) y DirectX12 App (aplicación universal de Windows).
-   App.xaml, App.xaml.cpp y App.xaml.h: punto de entrada principal de la aplicación. Conecta la aplicación con el shell de Windows y controla los eventos del ciclo de vida de la aplicación. Estos archivos solo aparecen en la plantilla DirectX11 and XAML App (Universal Windows).
-   DirectXPage.xaml, DirectXPage.xaml.cpp y DirectXPage.xaml.h: una página que hospeda un SwapChainPanel de DirectX. Estos archivos solo aparecen en la plantilla DirectX11 and XAML App (Universal Windows).
-   Contenido
    -   Sample3DSceneRenderer.h y Sample3DSceneRenderer.cpp: un representador de muestra que crea una instancia de una canalización de representación básica.
    -   SampleFpsTextRenderer.h y SampleFpsTextRenderer.cpp: representa el valor FPS actual en la parte inferior derecha de la pantalla con Direct2D y DirectWrite. Estos archivos solo aparecen en las plantillas DirectX11 App (aplicación universal de Windows) y DirectX11 and XAML App (aplicación universal de Windows).
    -   SamplePixelShader.hlsl: un sombreador de píxeles de ejemplo sencillo.
    -   SampleVertexShader.hlsl: un sombreador de vértices de ejemplo sencillo.
    -   ShaderStructures.h: estructuras que se usan para enviar los datos al sombreador de vértices de ejemplo.
-   Común
    -   StepTimer.h: una clase auxiliar para la animación y los intervalos de simulación.
    -   DirectXHelper.h: funciones auxiliares diversas.
    -   DeviceResources.h y Device Resources.cpp: proporciona una interfaz para una aplicación que posee DeviceResources que deben recibir una notificación cuando se pierde o se crea el dispositivo.
    -   d3dx12.h: contiene la biblioteca de utilidades D3DX12. Este archivo solo aparece en la plantilla DirectX12 App (Universal Windows).
-   Assets: imágenes de logotipo y pantalla de bienvenida que usa la aplicación.

## <a name="next-steps"></a>Pasos siguientes


Ahora que ya tienes un punto de partida, puedes ir adquiriendo más conocimientos para desarrollar tu juego e ir mejorando tus capacidades de desarrollo de juegos de Microsoft Store.

Si estás haciendo la portabilidad a un juego existente, consulta los siguientes temas.

-   [Migrar desde OpenGL ES 2.0 a Direct3D 11.1](port-from-opengl-es-2-0-to-directx-11-1.md)
-   [Migrar de DirectX9 a la Plataforma universal de Windows](porting-your-directx-9-game-to-windows-store.md)

Si estás creando un nuevo juego DirectX, consulta los siguientes temas.

-   [Crear un juego para UWP sencillo con DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Desarrollo de Marble Maze, un juego para la Plataforma universal de Windows en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)