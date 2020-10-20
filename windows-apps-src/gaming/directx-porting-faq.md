---
title: Preguntas más frecuentes sobre la migración a DirectX 11
description: Respuestas a las preguntas más frecuentes sobre la migración de juegos a la Plataforma universal de Windows (UWP).
ms.assetid: 79c3b4c0-86eb-5019-97bb-5feee5667a2d
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: bc6e1c2053b6ab3afe6eb42ed8b5223feb8ffb65
ms.sourcegitcommit: e39b569626804d2ce4246353ac2c03a916dc9737
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192995"
---
# <a name="directx-11-porting-faq"></a>Preguntas más frecuentes sobre la migración a DirectX 11




Respuestas a las preguntas más frecuentes sobre la migración de juegos a la Plataforma universal de Windows (UWP).

## <a name="is-porting-my-game-going-to-be-a-set-of-search-and-replace-operations-on-api-methods-or-do-i-need-to-plan-for-a-more-thoughtful-porting-process"></a>¿La migración del juego implicará un conjunto de operaciones de buscar y reemplazar en métodos de API o debo pensar en un proceso más complejo?


Direct3D 11 es una actualización significativa de Direct3D 9. Hay varios cambios de paradigma, entre ellos distintas API para el adaptador de gráficos virtualizados y su contexto, así como un nuevo nivel de polimorfismo para recursos de dispositivo. Tu juego aún puede usar hardware de gráficos esencialmente de la misma manera, pero tendrás que aprender sobre la nueva arquitectura de API que presenta Direct3D 11 y actualizar cada parte del código de gráficos para usar los componentes correctos de API. Consulta [Conceptos y consideraciones de migración](porting-considerations.md).

## <a name="what-is-the-new-device-context-for-am-i-supposed-to-replace-my-direct3d-9-device-with-the-direct3d-11-device-the-device-context-or-both"></a>¿Para qué sirve el nuevo contexto de dispositivo? ¿Qué se supone que debo reemplazar, el dispositivo Direct3D 9 por el dispositivo Direct3D 11, su contexto o ambos?


El dispositivo Direct3D ahora se usa para crear recursos en memoria de vídeo, mientras que el contexto de dispositivo se usa para establecer el estado de canalización y generar comandos de representación. Para obtener más información, consulta el artículo que te indicará [cuáles son los cambios más importantes desde Direct3D 9](understand-direct3d-11-1-concepts.md)

##  <a name="do-i-have-to-update-my-game-timer-for-uwp"></a>¿Tengo que actualizar el temporizador del juego para UWP?


[**QueryPerformanceCounter**](/windows/desktop/api/profileapi/nf-profileapi-queryperformancecounter), junto con [**QueryPerformanceFrequency**](/windows/desktop/api/profileapi/nf-profileapi-queryperformancefrequency), sigue siendo la mejor manera de implementar un temporizador para las aplicaciones para UWP.

Debes tener en cuenta un aspecto de los temporizadores y el ciclo de vida de una aplicación para UWP. La acción de suspensión y reanudación es diferente a cuando un jugador reinicia un juego de escritorio, ya que tu juego reanudará una instantánea de la última vez que estuvo activo. Si transcurre mucho tiempo, por ejemplo, algunas semanas, algunas implementaciones del temporizador del juego podrían tener errores leves. Puedes usar eventos de ciclo de vida de la aplicación para restablecer el temporizador cuando se reanuda el juego.

Los juegos que todavía usan la instrucción RDTSC deben actualizarse. Consulta [Procesadores de núcleo múltiple y tiempos de juego](/windows/desktop/DxTechArts/game-timing-and-multicore-processors).

## <a name="my-game-code-is-based-on-d3dx-and-dxut-is-there-anything-available-that-can-help-me-migrate-my-code"></a>El código de mi juego se basa en D3DX y DXUT. ¿Existe algo que pueda ayudarme a migrar el código?


El proyecto del [kit de herramientas de DirectX (DirectXTK)](https://github.com/Microsoft/DirectXTK) para la comunidad ofrece clases auxiliares para usar con Direct3D 11.

##  <a name="how-do-i-maintain-code-paths-for-the-desktop-and-the-microsoft-store"></a>Cómo mantener las rutas de acceso del código para el escritorio y el Microsoft Store?


La serie de artículos de Chuck Walbourn titulada [técnicas de codificación de doble uso para juegos](https://blogs.msdn.com/b/chuckw/archive/2012/09/17/dual-use-coding-techniques-for-games.aspx) ofrece orientación sobre el uso compartido de código entre el escritorio y las rutas de acceso del código Microsoft Store.

##  <a name="how-do-i-load-image-resources-in-my-directx-uwp-app"></a>¿Cómo cargo recursos de imagen en mi aplicación DirectX para UWP?


Hay dos rutas de acceso a API para cargar imágenes:

-   La canalización de contenido convierte imágenes en archivos DDS usados como recursos de textura en Direct3D. Consulta [Usar activos 3D en el juego o aplicación](/visualstudio/designers/using-3-d-assets-in-your-game-or-app).
-   [Windows Imaging Component](/windows/desktop/wic/-wic-lh) puede usarse para cargar imágenes en varios formatos, además de usarse para mapas de bits de Direct2D y recursos de textura de Direct3D.

También puedes usar DDSTextureLoader y WICTextureLoader, de [DirectXTK](https://github.com/Microsoft/DirectXTK) o [DirectXTex](https://github.com/Microsoft/DirectXTex).

## <a name="where-is-the-directx-sdk"></a>¿Dónde está el SDK de DirectX?


El SDK de DirectX se incluye como parte de Windows SDK. El SDK de DirectX más reciente que se separó del Windows SDK fue el de junio de 2010. Las muestras de Direct3D están en la Galería de código junto con el resto de las muestras de aplicaciones de la Tienda Windows.

## <a name="what-about-directx-redistributables"></a>¿Qué sucede con los redistribuibles de DirectX?


La mayoría de los componentes del Windows SDK ya están incluidos en versiones compatibles del sistema operativo o no tienen ningún componente DLL (como DirectXMath). Todos los componentes de API de Direct3D que las aplicaciones para UWP pueden usar ya están disponibles para tu juego. No es necesario redistribuirlos.

Las aplicaciones de escritorio Win32 todavía usan DirectSetup, por lo tanto, si también estás actualizando la versión de escritorio de tu juego, consulta [Implementación de Direct3D 11 para desarrolladores de juegos](/windows/desktop/direct3darticles/direct3d11-deployment).

## <a name="is-there-any-way-i-can-update-my-desktop-code-to-directx-11-before-moving-away-from-effects"></a>¿Cómo puedo actualizar el código de escritorio a DirectX 11 antes de salir de Efectos?


Consulta [Efectos para la actualización a Direct3D 11](https://github.com/Microsoft/FX11). Efectos 11 ayuda a quitar dependencias en encabezados heredados del SDK de DirectX. Está pensado para que se use como ayuda de migración solo en aplicaciones de escritorio.

##  <a name="is-there-a-path-for-porting-my-directx-8-game-to-uwp"></a>¿Hay alguna ruta trazada para migrar un juego de DirectX 8 a UWP?


Yes:

-   Lee [Converting to Direct3D 9 (Convertir a Direct3D 9)](/windows/desktop/direct3d9/converting-to-directx-9).
-   Asegúrate de que tu juego no tenga remanentes de la canalización fija. Consulta [Características desusadas](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated).
-   Luego sigue la ruta de migración de DirectX 9: [Migrar de D3D 9 a UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

##  <a name="can-i-port-my-directx-10-or-11-game-to-uwp"></a>¿Puedo migrar mi juego DirectX 10 u 11 a UWP?


Los juegos de escritorio de DirectX 10.x y 11 son fáciles de migrar a UWP. Consulta [Migrar a Direct3D 11](/windows/desktop/direct3d11/d3d11-programming-guide-migrating).

## <a name="how-do-i-choose-the-right-display-device-in-a-multi-monitor-system"></a>¿Cómo elijo el dispositivo de pantalla correcto en un sistema de varios monitores?


El usuario selecciona en qué monitor aparecerá tu aplicación. Deja que Windows proporcione el adaptador correcto llamando a [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) con el primer parámetro establecido en **nullptr**. A continuación, obtén el elemento [**IDXGIDevice interface**](/windows/desktop/api/dxgi/nn-dxgi-idxgidevice) del dispositivo, llama a [**GetAdapter**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter) y usa el adaptador DXGI para crear la cadena de intercambio.

## <a name="how-do-i-turn-on-antialiasing"></a>¿Cómo activo el suavizado de contorno?


El suavizado de contorno (muestreo múltiple) se habilita cuando creas el dispositivo Direct3D. Enumere la compatibilidad con multimuestreo llamando a [**CheckMultisampleQualityLevels**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkmultisamplequalitylevels)y, a continuación, establezca las opciones de Multimuestra en la [**estructura de \_ \_ DESC de ejemplo de DXGI**](/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc) al llamar a [**CreateSurface**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-createsurface).

## <a name="my-game-renders-using-multithreading-andor-deferred-rendering-what-do-i-need-to-know-for-direct3d-11"></a>Mi juego usa multithreading o representación diferida para representar. ¿Qué debo saber para Direct3D 11?


Para empezar, visita [Introducción a multithreading en Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro). Para obtener una lista de las diferencias principales, consulta [Diferencias de subprocesos entre versiones de Direct3D](/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-differences). Ten en cuenta que la representación diferida usa un *contexto diferido* de dispositivo en lugar de un *contexto inmediato*.

## <a name="where-can-i-read-more-about-the-programmable-pipeline-since-direct3d-9"></a>¿Dónde puedo encontrar más información sobre la canalización programable desde Direct3D 9?


Consulta estos temas:

-   [Guía de programación para HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-pguide)
-   [Preguntas más frecuentes sobre Direct3D 10](/windows/desktop/DxTechArts/direct3d10-frequently-asked-questions)

## <a name="what-should-i-use-instead-of-the-x-file-format-for-my-models"></a>¿Qué debo usar en lugar del formato de archivo .x para mis modelos?


Aunque no tenemos un sustituto oficial para el formato de archivo. x, muchos de los ejemplos usan el formato SDKMesh. Visual Studio también tiene una [canalización de contenido](/visualstudio/designers/using-3-d-assets-in-your-game-or-app) que compila varios formatos populares en archivos CMO. Estos archivos pueden cargarse con código mediante el kit de inicio de Visual Studio 3D o con [DirectXTK](https://github.com/Microsoft/DirectXTK).

## <a name="how-do-i-debug-my-shaders"></a>¿Cómo depuro mis sombreadores?


Microsoft Visual Studio incluye herramientas de diagnóstico para gráficos de DirectX. Consulta [Depurar gráficos de DirectX](/visualstudio/debugger/visual-studio-graphics-diagnostics).

##  <a name="what-is-the-direct3d-11-equivalent-for-x-function"></a>¿Cuál es el equivalente de la función *x* en Direct3D 11?


Consulta la información acerca de la [asignación de funciones](feature-mapping.md#function-mapping) proporcionada en el artículo Asignar características de DirectX 9 a las API de DirectX 11.

##  <a name="what-is-the-dxgi_format-equivalent-of-y-surface-format"></a>¿Cuál es el \_ equivalente de formato de DXGI del formato de superficie *y* ?


Consulta la información acerca de la [asignación de formatos de superficie](feature-mapping.md#surface-format-mapping) proporcionada en el artículo Asignar características de DirectX 9 a las API de DirectX 11.

 

 