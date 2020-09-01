---
title: Planear la migración de OpenGL ES 2.0 a Direct3D
description: Si vas a migrar un juego de plataformas iOs o Android, probablemente has hecho una gran inversión en OpenGL ES 2.0.
ms.assetid: a31b8c5a-5577-4142-fc60-53217302ec3a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, OpenGL, Direct3D
ms.localizationpriority: medium
ms.openlocfilehash: dddbf3a1009ccbaeb7fd0695d995cd17ba6182d3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165349"
---
# <a name="plan-your-port-from-opengl-es-20-to-direct3d"></a>Planear la migración de OpenGL ES 2.0 a Direct3D




**API importantes**

-   [Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
-   [Visual C++](/previous-versions/60k1461a(v=vs.140))

Si vas a migrar un juego de plataformas iOs o Android, probablemente has hecho una gran inversión en OpenGL ES 2.0. Cuando te preparas para trasladar el código base de la canalización de gráficos a Direct3D 11 y Windows Runtime, debes considerar algunas cosas antes de empezar.

La mayor parte de la tarea de migración consiste en dar los primeros pasos por el código base y la asignación de patrones y API comunes entre los dos modelos. Encontrarás que este proceso es bastante fácil si te tomas algo de tiempo para leer y revisar este tema.

Estas son algunas de las cosas que debes tener en cuenta para migrar gráficos de OpenGL ES 2.0 a Direct3D 11.

## <a name="notes-on-specific-opengl-es-20-providers"></a>Notas sobre proveedores específicos de OpenGL ES 2.0


Los temas de migración de esta sección hacen referencia a la implementación de Windows de la especificación de OpenGL ES 2.0 creada por Khronos Group. Todas las muestras del código de OpenGL ES 2.0 se desarrollaron con Visual Studio 2012 y la sintaxis básica de Windows C. Si partes de un código base de Objective-C (iOS) o Java (Android), ten en cuenta que las muestras de código de Open GL ES 2.0 que se proporcionan pueden no usar parámetros o sintaxis de llamadas API similares. Esta orientación intenta no involucrarte con las plataformas en la medida que sea posible.

La documentación solo usa API de la especificación 2.0 para el código de OpenGL ES y la referencia. Si vas a migrar de OpenGL ES 1.1 o 3.0, este contenido puede serte útil, aunque algunos de los ejemplos del código de OpenGL ES 2.0 y el contexto pueden resultarte desconocidos.

Las muestras de Direct3D 11 de estos temas usan Microsoft Windows C++ con extensiones de componentes (CX). Para obtener más información sobre esta versión de la sintaxis de C++, lea [Visual C++](/previous-versions/60k1461a(v=vs.140)), [extensiones de componentes para plataformas en tiempo de ejecución](/cpp/windows/component-extensions-for-runtime-platforms)y [referencia rápida (C++ \\ CX)](/cpp/cppcx/quick-reference-c-cx).

## <a name="understand-your-hardware-requirements-and-resources"></a>Comprender los requisitos y recursos de hardware


El conjunto de características para procesar gráficos en OpenGL ES 2.0 se asigna en términos generales a las características proporcionadas en Direct3D 9.1. Si quieres sacar provecho de las características más avanzadas de Direct3D 11, revisa la documentación de [Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11) cuando planees una migración o revisa los temas [Migrar de DirectX 9 a la Plataforma universal de Windows (UWP)](porting-your-directx-9-game-to-windows-store.md) cuando ya hayas realizado el esfuerzo inicial.

Para simplificar tu primera migración, empieza con la plantilla Direct3D de Visual Studio. Esta proporciona un representador básico ya configurado y admite características de aplicaciones para UWP, como recrear recursos en cambios de Windows y niveles de características de Direct3D.

## <a name="understand-direct3d-feature-levels"></a>Comprender los niveles de característica de Direct3D 


Direct3D 11 proporciona compatibilidad con los niveles de características de hardware de 9 \_ 1 (Direct3D 9,1) para 11 \_ 1. Estos niveles de característica indican la disponibilidad de ciertos recursos y características de gráficos. Normalmente, la mayoría de las plataformas de OpenGL ES 2,0 admiten un conjunto de características de Direct3D 9,1 (nivel de característica 9 \_ 1).

## <a name="review-directx-graphics-features-and-apis"></a>Revisar API y características de gráficos de DirectX


| Familia de API                                                | Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)                     | La Infraestructura de gráficos de Microsoft DirectX (DXGI) proporciona una interfaz entre el hardware de gráficos y Direct3D. Establece la configuración del hardware y adaptador de dispositivo con las interfaces COM [**IDXGIAdapter**](/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) y [**IDXGIDevice1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2). Úsala para crear y configurar búferes y otros recursos de Windows. De manera notable, el modelo predeterminado [**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) se usa para adquirir recursos de gráficos, incluida la cadena de intercambio (un conjunto de búferes de trama). Dado que DXGI tiene su propia cadena de intercambio, la interfaz [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) se usa para presentar marcos en la pantalla. |
| [Direct3D](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)       | Direct3D es el conjunto de API que proporciona una representación virtual de la interfaz de gráficos y permite dibujar gráficos con ella. La versión 11 es comparable a grandes rasgos en características a OpenGL 4.3. (OpenGL ES 2.0, por otro lado, es similar a DirectX9 y OpenGL 2.0 en sus características, pero con la canalización de sombreador unificada de OpenGL 3.0). La mayoría del trabajo pesado se realiza con las interfaces ID3D11Device1 y ID3D11DeviceContext1, que proporcionan acceso a recursos individuales y subrecursos, y el contexto de representación, respectivamente.                                                                                                                                          |
| [Direct2D](/windows/desktop/Direct2D/direct2d-portal)                      | Direct2D proporciona un conjunto de API para la representación 2D con aceleración por GPU. Puede considerarse similar en su propósito a OpenVG.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal)            | DirectWrite proporciona un conjunto de API para la representación de fuente, de alta calidad, con aceleración por GPU.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| [DirectXMath](/windows/desktop/dxmath/directxmath-portal)                  | DirectXMath proporciona un conjunto de API y macros para controlar tipos trigonométricos y de álgebra lineal comunes, así como valores y funciones. Estos tipos y funciones están diseñados para funcionar con Direct3D y sus operaciones de sombreador.                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| [DirectX HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core) | Es la sintaxis actual de HLSL que usan los sombreadores de Direct3D. Implementa el modelo de sombreador 5.0 de Direct3D.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

 

## <a name="review-the-windows-runtime-apis-and-template-library"></a>Revisar la biblioteca de plantillas y API de Windows Runtime


Las API de Windows Runtime proporcionan la infraestructura general para aplicaciones para UWP. Revísalas [aquí](/uwp/api/).

Estas son las API esenciales de Windows Runtime que se usan en la migración de la canalización de gráficos:

-   [**Windows:: UI:: Core:: CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)
-   [**Windows:: UI:: Core:: CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher)
-   [**Windows::ApplicationModel::Core::IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView)
-   [**Windows::ApplicationModel::Core::CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)

Además, la Biblioteca de plantillas C++ de Windows Runtime (WRL) es una biblioteca que proporciona un modo de bajo nivel para crear y usar componentes de Windows Runtime. Las API de Direct3D 11 para aplicaciones para UWP se usan mejor en conjunto con las interfaces y tipos de esta biblioteca, como los punteros inteligentes ([ComPtr](/cpp/windows/comptr-class)). Para más información sobre la WRL, lee [Biblioteca de plantillas C++ de Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl).

## <a name="change-your-coordinate-system"></a>Cambiar el sistema de coordenadas


Una diferencia que a veces confunde en los primeros pasos de migración es el cambio del sistema de coordenadas tradicional a la derecha de OpenGL al sistema de coordenadas predeterminado a la izquierda de Direct3D. Esta cambio en los modelos de coordenadas afecta muchas partes del juego, desde la configuración de búferes de vértices a varias de las funciones matemáticas de la matriz. Los dos cambios más importantes para hacer son:

-   Invierte el orden de los vértices del triángulo, de modo tal que Direct3D los recorra en el sentido de las agujas del reloj desde el frente. Por ejemplo, si los vértices se indizan como 0, 1 y 2 en tu canalización de OpenGL, pásalos a Direct3D como 0, 2 y 1.
-   Usa la matriz de vista para escalar el espacio entre palabras en -1.0f en la dirección z, invirtiendo efectivamente las coordenadas del eje z. Para ello, invierte el signo de valores en las posiciones M31, M32 y M33 de la matriz de vista (cuando realices la migración al tipo [**Matrix**](/windows/desktop/direct3d9/matrix4x4)). Si M34 no es 0, invierte este signo también.

Si embargo Direct3D puede admitir un sistema de coordenadas a la derecha. DirectMath proporciona una serie de funciones que operan en sistemas de coordenadas a la derecha y a la izquierda. Pueden usarse para preservar parte de los datos originales de tu malla y procesamiento de matriz. Incluyen:

| Función de matriz DirectXMath                                                   | Descripción                                                                                                                 |
|-------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| [**XMMatrixLookAtLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatlh)                               | Crea una matriz de vista para un sistema de coordenadas a la izquierda usando una posición de cámara, una dirección hacia arriba y un punto focal.       |
| [**XMMatrixLookAtRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlookatrh)                               | Crea una matriz de vista para un sistema de coordenadas a la derecha usando una posición de cámara, una dirección hacia arriba y un punto focal.      |
| [**XMMatrixLookToLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktolh)                               | Crea una matriz de vista para un sistema de coordenadas a la izquierda usando una posición de cámara, una dirección hacia arriba y una dirección de cámara.  |
| [**XMMatrixLookToRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixlooktorh)                               | Crea una matriz de vista para un sistema de coordenadas a la derecha usando una posición de cámara, una dirección hacia arriba y una dirección de cámara. |
| [**XMMatrixOrthographicLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographiclh)                   | Crea una matriz de proyección ortogonal para un sistema de coordenadas a la izquierda.                                                 |
| [**XMMatrixOrthographicOffCenterLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterlh) | Crea una matriz de proyección ortogonal personalizada para un sistema de coordenadas a la izquierda.                                           |
| [**XMMatrixOrthographicOffCenterRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicoffcenterrh) | Crea una matriz de proyección ortogonal personalizada para un sistema de coordenadas a la derecha.                                          |
| [**XMMatrixOrthographicRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixorthographicrh)                   | Crea una matriz de proyección ortogonal para un sistema de coordenadas a la derecha.                                                |
| [**XMMatrixPerspectiveFovLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovlh)               | Crea una matriz de proyección de perspectiva a la izquierda basada en un campo visual.                                                |
| [**XMMatrixPerspectiveFovRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivefovrh)               | Crea una matriz de proyección de perspectiva a la derecha basada en un campo visual.                                               |
| [**XMMatrixPerspectiveLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectivelh)                     | Crea una matriz de proyección de perspectiva a la izquierda.                                                                         |
| [**XMMatrixPerspectiveOffCenterLH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterlh)   | Crea una versión personalizada de una matriz de proyección de perspectiva a la izquierda.                                                     |
| [**XMMatrixPerspectiveOffCenterRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiveoffcenterrh)   | Crea una versión personalizada de una matriz de proyección de perspectiva a la derecha.                                                    |
| [**XMMatrixPerspectiveRH**](/windows/desktop/api/directxmath/nf-directxmath-xmmatrixperspectiverh)                     | Crea una matriz de proyección de perspectiva a la derecha.                                                                        |

 

## <a name="opengl-es20-to-direct3d-11-porting-frequently-asked-questions"></a>Preguntas más frecuentes sobre la migración de OpenGL ES 2.0 a Direct3D 11


-   Pregunta: "En general, ¿puedo buscar ciertas cadenas o patrones en el código de OpenGL y reemplazarlos con los equivalentes de Direct3D?
-   Respuesta: No. OpenGL ES 2.0 y Direct3D 11 provienen de distintas generaciones de modelos de canalización de gráficos. Si bien hay algunas similitudes superficiales entre los conceptos y las API, como el contexto de representación y las instancias de sombreador, debes revisar esta orientación, así como la referencia de Direct3D 11, para asegurarte de tomar las mejores decisiones cuando recrees tu canalización en lugar de intentar una asignación 1 a 1. Sin embargo, su vas a migrar de GLSL a HLSL, crear un conjunto de alias comunes para las variables, intrínsecos y funciones de GLSL no solo puede facilitar la migración, sino que también te permite mantener un solo conjunto de archivos de código de sombreador.

 

 