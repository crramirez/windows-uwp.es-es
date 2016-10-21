---
author: mtoepke
title: Juegos y DirectX
description: "La Plataforma universal de Windows (UWP) ofrece nuevas oportunidades para crear, distribuir y rentabilizar los juegos. Descubre cómo comenzar un nuevo juego o migrar uno existente."
ms.assetid: 4073b835-c900-4ff2-9fc5-da52f9432a1f
translationtype: Human Translation
ms.sourcegitcommit: adaff9abeb0ae8f9c9deb964b18e673c4d4c5f73
ms.openlocfilehash: fccc85584e84bf3364130fe80c61b75c6377c0c8

---

# Juegos y DirectX


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

La Plataforma universal de Windows (UWP) ofrece nuevas oportunidades para crear, distribuir y rentabilizar los juegos. Descubre cómo comenzar un nuevo juego o migrar uno existente.

| Tema | Descripción |
|---------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Guía de desarrollo de juegos para Windows 10](e2e.md) | Una guía completa sobre los recursos y la información que necesitas para el desarrollo de juegos para UWP. |
| [Tecnologías de juegos de aplicaciones para la Plataforma universal de Windows](game-development-platform-guide.md) | En esta guía, encontrarás información sobre las tecnologías disponibles para desarrollar juegos para UWP. |
| [Plantillas de proyectos y herramientas para juegos](prepare-your-dev-environment-for-windows-store-directx-game-development.md) | Se muestra lo que necesitas para empezar a programar juegos DirectX para UWP. |
| [Objeto de aplicación y DirectX](about-the-metro-style-user-interface-and-directx.md) | Los juegos con DirectX para la Plataforma universal de Windows no usan muchos de los elementos y objetos de la nueva interfaz de usuario de Windows. Como se ejecutan a un nivel más bajo en la pila de Windows Runtime, deben interoperar con el marco de la interfaz de usuario de una manera más básica, mediante el acceso y la interoperación con el objeto de aplicación directamente. Aprende cuándo y cómo se realiza esta interoperación y de qué manera puedes usar eficazmente, como desarrollador de DirectX, este modelo en el desarrollo de una aplicación para UWP. |
| [Inicio y reanudación de aplicaciones](launching-and-resuming-apps-directx-and-cpp.md) | Aprende a iniciar, suspender y reanudar una aplicación de DirectX para UWP. |
| [Elementos gráficos 2D para juegos DirectX](working-with-2d-graphics-in-your-directx-game.md) | Se analiza el uso de efectos y gráficos de mapas de bits 2D y cómo usarlos en un juego. |
| [Gráficos 3D básicos para juegos DirectX](an-introduction-to-3d-graphics-with-directx.md) | Se muestra cómo usar la programación DirectX para implementar los conceptos fundamentales de los gráficos 3D. |
| [Cargar recursos en un juego DirectX](load-a-game-asset.md) | La mayoría de los juegos, en algún momento, cargan recursos y activos (como sombreadores, texturas, mallas predefinidas y otros datos de gráficos) del almacenamiento local u otro flujo de datos. En este tema, se ofrece una guía por un panorama de alto nivel de lo que debes considerar al cargar estos archivos para un juego para UWP. |
| [Crear un juego para UWP sencillo con DirectX](tutorial--create-your-first-metro-style-directx-game.md) | En este conjunto de tutoriales, aprenderás a crear un juego de UWP básico con DirectX y C++. Se tratan las partes principales de un juego, incluidos los procesos para cargar activos, como imágenes y mallas, cómo crear un bucle de juego principal, implementar una canalización de representación simple y agregar sonido y controles. |
| [Desarrollo de Marble Maze, un juego para la Plataforma universal de Windows en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md) | En esta sección de la documentación se describe cómo usar DirectX y Visual C++ para crear un juego en 3-D para UWP. En esta documentación se muestra cómo crear un juego en 3-D llamado Marble Maze, que funciona tanto en los nuevos factores de forma, por ejemplo tabletas, como en equipos de escritorio y portátiles tradicionales. |
| [Compatibilidad con la orientación de pantalla](supporting-screen-rotation-directx-and-cpp.md) | Aquí se comentan los procedimientos recomendados para controlar la rotación de pantalla en una aplicación DirectX para UWP, de modo que el hardware de gráficos del dispositivo Windows 10 se pueda usar de manera eficaz. |
| [Audio para juegos](working-with-audio-in-your-directx-game.md) | Aprende a desarrollar e incorporar música y sonidos en un juego DirectX, y cómo procesar las señales de audio para crear sonidos dinámicos y posicionales. |
| [Controles táctiles para juegos](tutorial--adding-touch-controls-to-your-directx-game.md) | Aprende a agregar controles táctiles básicos a un juego en C++ para UWP con DirectX. Se muestra cómo agregar controles basados en el tacto para mover una cámara de plano fijo en un entorno de Direct3D, donde arrastrar con un dedo o un lápiz cambia la perspectiva de la cámara. |
| [Controles de movimiento y vista para juegos](tutorial--adding-move-look-controls-to-your-directx-game.md) | Aprende a agregar controles de movimiento y vista tradicionales de mouse y teclado (también conocidos como controles mouselook) a un juego DirectX. |
| [Movimiento de mouse relativo](relative-mouse-movement.md) | Aprende a agregar controles de mouse relativos, que no usan el cursor del sistema y no devuelven coordenadas de pantalla absolutas. En su lugar, siguen el delta de píxeles entre los diferentes movimientos del mouse. |
| [Optimizar el bucle de representación y de entrada](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) | La latencia de entrada puede influir considerablemente en la experiencia de un juego, por lo que al optimizarla puedes conseguir una experiencia más fluida. Además, una correcta optimización de los eventos de entrada contribuye a mejorar la vida de la batería. Aprende a elegir las opciones adecuadas de procesamiento de eventos de entrada de [CoreDispatcher](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md) para garantizar que el juego controle las entradas de la forma más fluida posible. |
| [Escalado de cadena de intercambio y superposiciones](multisampling--scaling--and-overlay-swap-chains.md) | Aprende a crear cadenas de intercambio con escala para que las representaciones en los dispositivos móviles sean más rápidas, y usa también cadenas de intercambio (si las hay) para mejorar la calidad visual. |
| [Reducir la latencia con cadenas de intercambio de DXGI 1.3](reduce-latency-with-dxgi-1-3-swap-chains.md) | Usa DXGI1.3 para reducir la latencia de fotogramas eficaz esperando a que la cadena de intercambio señale el momento adecuado para empezar a representar un nuevo fotograma. |
| [Muestreo múltiple en aplicaciones para UWP](multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md) | Aprende a usar el muestreo múltiple en aplicaciones para UWP compiladas con Direct3D. |
| [Controlar escenarios cuando se quitan dispositivos en Direct3D11](handling-device-lost-scenarios.md) | En este tema se explica cómo recrear la cadena de la interfaz de dispositivo de Direct3D y DXGI cuando se quita o reinicializa la tarjeta gráfica. |
| [Programación asincrónica para juegos](asynchronous-programming-directx-and-cpp.md) | En este tema se tratan diversos aspectos que deben considerarse cuando se usa la programación asincrónica y los subprocesos con DirectX. |
| [Conexión en red de juegos](work-with-networking-in-your-directx-game.md) | Aprende a desarrollar e incorporar características de red en un juego DirectX. |
| [Accesibilidad para juegos](accessibility-for-games.md) | Aprende a hacer que los juegos sean más accesibles. |
| [Nube para juegos](cloud-for-games.md) | Aprende a hacer uso de las tecnologías de nube para desarrollar juegos. |
| [Interoperabilidad de DirectX y XAML](directx-and-xaml-interop.md) | Puedes usar el lenguaje XAML y Microsoft DirectX juntos en un juego para UWP. |
| [Empaquetar un juego](package-your-windows-store-directx-game.md) | Los juegos para UWP grandes, especialmente aquellos que admiten varios idiomas con activos específicos de una región o activos de alta definición como característica opcional, pueden crecer hasta alcanzar tamaños considerables. En este tema, aprende cómo usar los paquetes de la aplicación y los lotes de aplicaciones para personalizar tu aplicación de modo que los clientes solamente reciban los recursos que de verdad necesitan. |
| [Aprobación de concepto](concept-approval.md) | Descubre cómo enviar el producto para la aprobación del concepto, lo que será necesario si tu producto se ejecuta en Xbox o usa Xbox Live. |
| [Guías para portar juegos](porting-guides.md) | Ofrece guías para portar los juegos existentes a Direct3D11, UWP y Windows10. |
| [Recursos de programación de juegos](additional-directx-game-programming-resources.md) | Para más información sobre la programación de juegos en Windows, consulta los siguientes recursos. |

 

> **Nota**  
Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows8.x o Windows Phone8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

Para aprovechar al máximo las explicaciones y los tutoriales sobre desarrollo de juegos, debes estar familiarizado con los siguientes temas:

-   Microsoft C++ con Component Extensions (C++/CX). Se trata de una actualización de Microsoft C++ que incorpora el recuento automático de referencias, y es el lenguaje de desarrollo para juegos de UWP con DirectX11.1 o versiones posteriores.
-   Terminología básica de programación de gráficos.
-   Conceptos básicos de programación en Windows.
-   Conocimiento básico de las API de Direct3D9 u11.

 

 







<!--HONumber=Aug16_HO5-->


