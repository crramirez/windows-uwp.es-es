---
title: Comparar código de EGL con DXGI y Direct3D
description: DirectX Graphics Interface (DXGI) y varias API de Direct3D cumplen el mismo rol que EGL. Este tema ayuda a comprender DXGI y Direct3D 11 desde la perspectiva de EGL.
ms.assetid: 90f5ecf1-dd5d-fea3-bed8-57a228898d2a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, EGL, DXGI, Direct3D
ms.localizationpriority: medium
ms.openlocfilehash: 3a93f78cdc0d716f9b421fdc493317208f9d17b2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368368"
---
# <a name="compare-egl-code-to-dxgi-and-direct3d"></a>Comparar código de EGL con DXGI y Direct3D




**API importantes**

-   [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)
-   [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)
-   [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)

DirectX Graphics Interface (DXGI) y varias API de Direct3D cumplen el mismo rol que EGL. Este tema ayuda a comprender DXGI y Direct3D 11 desde la perspectiva de EGL.

DXGI y Direct3D, al igual que EGL, proporcionan métodos para configurar recursos gráficos, obtener un contexto de representación en el que tus sombreadores puedan dibujar y para mostrar los resultados en una ventana. No obstante, DXGI y Direct3D tienen algunas opciones más y demandan más esfuerzo para configurarse correctamente cuando se portan desde EGL.

> **Tenga en cuenta**    esta guía se basa en especificaciones de open Khronos del grupo de EGL 1.4, encontrar aquí: [Interfaz gráfica de plataforma nativa Khronos (EGL versión 1.4 - 6 de abril de 2011) \[PDF\]](https://www.khronos.org/registry/egl/specs/eglspec.1.4.20110406.pdf). Las diferencias en la sintaxis específica de otras plataformas y lenguajes de desarrollo no se analizan en esta guía.

 

## <a name="how-does-dxgi-and-direct3d-compare"></a>¿En qué aspectos se comparan DXGI y Direct3D?


La gran ventaja de EGL sobre DXGI y Direct3D es que es relativamente sencillo comenzar a dibujar en una superficie de ventana. Esto se debe a que OpenGL ES 2.0, y por ende EGL, es una especificación implementada por varios proveedores de plataformas, mientras que DXGI y Direct3D constituyen una única referencia que deben cumplir los controladores de los proveedores de hardware. Esto significa que Microsoft debe implementar un conjunto de API que permita el conjunto más amplio posible de características de proveedor, en lugar de centrarse en un subconjunto funcional ofrecido por un proveedor específico, o mediante la combinación de comandos de configuración específicos del proveedor en API más sencillas. Por otro lado, Direct3D proporciona un único conjunto de API que cubre un rango muy amplio de plataformas de hardware gráfico y niveles de característica, y ofrece más flexibilidad para desarrolladores con experiencia con la plataforma.

Como EGL, DXGI y Direct3D proporcionan API para los siguientes comportamientos:

-   Obtener y leer y escribir en un búfer de cuadros (llamado "cadena de intercambio" en DXGI).
-   Asociar el búfer de cuadros con una ventana de la interfaz de usuario.
-   Obtener y configurar contextos de representación en los cuales dibujar.
-   Emitir comandos para la canalización de gráficos para un contexto de representación específico.
-   Crear y administrar recursos de sombreador y asociarlos con contenido de representación.
-   Representar en destinos de representación específicos (como texturas).
-   Actualizar la superficie de presentación de la ventana con los resultados de representar con los recursos gráficos.

Para ver el proceso básico de Direct3D para configurar la canalización de gráficos, consulte la plantilla de aplicación de DirectX 11 (Windows Universal) en Microsoft Visual Studio 2015. La clase de representación de base que puedes encontrar en ella proporciona un buen punto de referencia para configurar tanto la infraestructura gráfica de Direct3D 11 como sus recursos básicos, así como para admitir características de aplicación de la Plataforma universal de Windows (UWP) tales como la rotación de la pantalla.

EGL tiene muy pocas API relacionadas con Direct3D 11, y navegar en este último puede ser un desafío si no estás familiarizado con la nomenclatura y la terminología particular de la plataforma. He aquí una sencilla introducción que te ayudará a orientarte.

Primero, revisa el objeto de EGL básico para la asignación de interfaz de Direct3D:

| Abstracción de EGL | Representación de Direct3D similar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EGLDisplay**  | En Direct3D (para aplicaciones para UWP), se obtiene el identificador de presentación a través de la API [**Windows::UI::CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) (o la interfaz de **ICoreWindowInterop** que expone el HWND). La configuración de hardware y adaptador se establece con las interfaces COM [**IDXGIAdapter**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) y [**IDXGIDevice1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2) respectivamente.                                                                                                                                                                                                                                                           |
| **EGLSurface**  | En Direct3D, los búferes y demás recursos de ventana (bien sean visibles o estén fuera de pantalla) se crean y configuran mediante interfaces de DXGI específicas; entre ellas, [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2), que es una implementación de patrón de fábrica usada para adquirir recursos de DXGI como [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (búferes de presentación). La interfaz [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) que representa el dispositivo gráfico y sus recursos, se adquiere mediante la función [**D3D11Device::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Para destinos de representación, usa la interfaz [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview). |
| **EGLContext**  | En Direct3D, puedes configurar y emitir comandos para la canalización de gráficos con la interfaz [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **EGLConfig**   | En Direct3D 11, puedes crear y configurar recursos gráficos como un búferes, texturas, galerías de símbolos y sombreadores gracias a los métodos de la interfaz [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1).                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

 

A continuación, te describimos el modo más básico de configurar una pantalla de gráficos sencilla, así como los recursos y el contexto en DXGI y Direct3D, de una aplicación para UWP.

1.  Llama a [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) para obtener un identificador del objeto [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) para el subproceso de la interfaz de usuario principal de la aplicación.
2.  En el caso de las aplicaciones para UWP, obtén una cadena de intercambio de [**IDXGIAdapter2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiadapter2) con [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow), y pásala a la referencia de la clase [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) que obtuviste en el paso 1. Obtendrás a cambio una instancia [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1). Define su ámbito en el objeto de representador y su subproceso de representación.
3.  Llama al método [**D3D11Device::CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) para obtener las instancias [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) y [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). Define sus ámbitos también en el objeto de representador.
4.  Crea sombreadores, texturas y otros recursos mediante los métodos del objeto [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) del representador.
5.  Define búferes, ejecuta los sombreadores y administra los estados de canalización con los métodos del objeto [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) del representador.
6.  Cuando la canalización se haya ejecutado y se haya dibujado un fotograma en el búfer de reserva, muéstralo en pantalla con [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).

Echa un vistazo a [Getting Started with DirectX Graphics (Introducción a los gráficos DirectX)](https://docs.microsoft.com/windows/desktop/getting-started-with-directx-graphics) para estudiar este proceso con mayor detenimiento. En lo que queda de artículo se abordarán muchos de los pasos básicos habituales para configurar y administrar la canalización de gráficos.
> **Tenga en cuenta**    las aplicaciones de escritorio de Windows tienen diferentes API para obtener una cadena de intercambio de Direct3D, tales como [ **D3D11Device::CreateDeviceAndSwapChain**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdeviceandswapchain)y no use un [ **CoreWindow** ](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) objeto.

 

## <a name="obtaining-a-window-for-display"></a>Obtener una ventana de presentación


En este ejemplo, se pasa un HWND al elemento eglGetDisplay de un recurso de ventana específico de la plataforma Microsoft Windows. Otras plataformas, como iOS de Apple (Cocoa) y Android de Google, tienen diferentes controladores o referencias a recursos de ventana, y pueden tener una sintaxis de llamada totalmente diferente. Después de obtener una presentación, la inicializas, estableces los parámetros de configuración preferidos y creas una superficie con un búfer de reserva en el que puedes dibujar.

Obtener una presentación y configurarla con EGL.

``` syntax
// Obtain an EGL display object.
EGLDisplay display = eglGetDisplay(GetDC(hWnd));
if (display == EGL_NO_DISPLAY)
{
  return EGL_FALSE;
}

// Initialize the display
if (!eglInitialize(display, &majorVersion, &minorVersion))
{
  return EGL_FALSE;
}

// Obtain the display configs
if (!eglGetConfigs(display, NULL, 0, &numConfigs))
{
  return EGL_FALSE;
}

// Choose the display config
if (!eglChooseConfig(display, attribList, &config, 1, &numConfigs))
{
  return EGL_FALSE;
}

// Create a surface
surface = eglCreateWindowSurface(display, config, (EGLNativeWindowType)hWnd, NULL);
if (surface == EGL_NO_SURFACE)
{
  return EGL_FALSE;
}
```

En Direct3D, la ventana principal de una aplicación para UWP se representa mediante el objeto [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow), que puede obtenerse del objeto de la aplicación llamando a [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread), como parte del proceso de inicialización del "suministrador de vistas" que construyes para Direct3D. (Si está utilizando la interoperabilidad de Direct3D XAML, usar proveedor de vistas del marco de trabajo XAML). Se trata el proceso de creación de un proveedor de vistas de Direct3D en [cómo configurar la aplicación para mostrar una vista](https://docs.microsoft.com/previous-versions/windows/apps/hh465077(v=win.10)).

Obtener CoreWindow para Direct3D.

``` syntax
CoreWindow::GetForCurrentThread();
```

Cuando se obtiene la referencia [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow), debe activarse la ventana; dicha acción ejecuta el método **Run** del objeto principal y comienza el procesamiento de eventos de la ventana. Después, crea un [ **ID3D11Device1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) y un [ **ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)y usarlos para obtener subyacente [ **IDXGIDevice1** ](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgidevice1) y [ **IDXGIAdapter** ](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) para que pueda obtener un [ **IDXGIFactory2** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) objeto para crear un recurso de cadena de intercambio basado en su [ **DXGI\_intercambio\_cadena\_DESC1** ](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) configuración.

Configurar y establecer la cadena de intercambio de DXGI en CoreWindow para Direct3D.

``` syntax
// Called when the CoreWindow object is created (or re-created).
void SimpleDirect3DApp::SetWindow(CoreWindow^ window)
{
  // Register event handlers with the CoreWindow object.
  // ...

  // Obtain your ID3D11Device1 and ID3D11DeviceContext1 objects
  // In this example, m_d3dDevice contains the scoped ID3D11Device1 object
  // ...

  ComPtr<IDXGIDevice1>  dxgiDevice;
  // Get the underlying DXGI device of the Direct3D device.
  m_d3dDevice.As(&dxgiDevice);

  ComPtr<IDXGIAdapter> dxgiAdapter;
  dxgiDevice->GetAdapter(&dxgiAdapter);

  ComPtr<IDXGIFactory2> dxgiFactory;
  dxgiAdapter->GetParent(
    __uuidof(IDXGIFactory2), 
    &dxgiFactory);

  DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
  swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
  swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
  swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
  swapChainDesc.Stereo = false;
  swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
  swapChainDesc.SampleDesc.Quality = 0;
  swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
  swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
  swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
  swapChainDesc.Flags = 0;

  // ...

  Windows::UI::Core::CoreWindow^ window = m_window.Get();
  dxgiFactory->CreateSwapChainForCoreWindow(
    m_d3dDevice.Get(),
    reinterpret_cast<IUnknown*>(window),
    &swapChainDesc,
    nullptr, // Allow on all displays.
    &m_swapChainCoreWindow);
}
```

Llama al método [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) después de preparar un cuadro con el fin de presentarlo.

Ten en cuenta que en Direct3D 11, no existe una abstracción igual a EGLSurface. (Hay [ **IDXGISurface1**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1), pero se usa de forma diferente.) La aproximación conceptual más cercana es la [ **ID3D11RenderTargetView** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) objeto que se usa para asignar una textura ([**ID3D11Texture2D** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)) como el búfer de reserva que nuestra canalización de sombreador que atraigan a.

Configurar el búfer de reserva para la cadena de intercambio en Direct3D 11

``` syntax
ComPtr<ID3D11RenderTargetView>    m_d3dRenderTargetViewWin; // scoped to renderer object

// ...

ComPtr<ID3D11Texture2D> backBuffer2;
    
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&backBuffer2));

m_d3dDevice->CreateRenderTargetView(
  backBuffer2.Get(),
  nullptr,
    &m_d3dRenderTargetViewWin);
```

Un procedimiento recomendado es llamar a este código siempre que se cree la ventana o se cambie su tamaño. Durante la representación, debes establecer la vista de destino de representación con [**ID3D11DeviceContext1::OMSetRenderTargets**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) antes de configurar cualquier otro subrecurso como sombreadores o búferes de vértices.

``` syntax
// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

## <a name="creating-a-rendering-context"></a>Crear un contexto de representación


En EGL 1.4, una "presentación" representa un conjunto de recursos de ventana. Generalmente, puedes configurar una "superficie" para la presentación proporcionando un conjunto de atributos al objeto de presentación y obteniendo a cambio una superficie. Creas un contexto para mostrar el contenido de la superficie creándolo y vinculándolo con la superficie y la presentación.

El flujo de la llamada generalmente es similar a este:

-   Llama a eglGetDisplay con el controlador para un recurso de ventana o presentación, y obtén un objeto de presentación.
-   Inicializa la presentación con eglInitialize.
-   Obtén la configuración de presentación disponible y selecciona una con eglGetConfigs y eglChooseConfig.
-   Crea una superficie de ventana con eglCreateWindowSurface.
-   Crea un contexto de presentación para dibujar con eglCreateContext.
-   Vincula el contexto de presentación a la presentación y la superficie con eglMakeCurrent.

En la sección anterior, creamos EGLDisplay y EGLSurface, y ahora usamos EGLDisplay para crear un contexto y asociarlo con la presentación, usando la EGLSurface configurada para parametrizar la salida.

Obtener un contexto de representación con EGL 1.4

```cpp
// Configure your EGLDisplay and obtain an EGLSurface here ...
// ...

// Create a drawing context from the EGLDisplay
context = eglCreateContext(display, config, EGL_NO_CONTEXT, contextAttribs);
if (context == EGL_NO_CONTEXT)
{
  return EGL_FALSE;
}   
   
// Make the context current
if (!eglMakeCurrent(display, surface, surface, context))
{
  return EGL_FALSE;
}
```

Un contexto de representación en Direct3D 11 se representa mediante un objeto [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), el cual representa el adaptador y te permite crear recursos de Direct3D como búferes y sombreadores; además, el objeto [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1), te permite administrar la canalización de gráficos y ejecutar los sombreadores.

Ten en cuenta los niveles de característica de Direct3D. Estos se usan para admitir plataformas de hardware de Direct3D anteriores, desde DirectX 9.1 a DirectX 11. Muchas plataformas que usan hardware gráfico de bajo consumo (como las tabletas), solamente tienen acceso a características de DirectX 9.1 y el hardware gráfico más antiguo que se admite oscila entre las versiones 9.1 y 11.

Crear un contexto de representación con DXGI y Direct3D

```cpp

// ... 

UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;
ComPtr<IDXGIDevice> dxgiDevice;

D3D_FEATURE_LEVEL featureLevels[] = 
{
        D3D_FEATURE_LEVEL_11_1,
        D3D_FEATURE_LEVEL_11_0,
        D3D_FEATURE_LEVEL_10_1,
        D3D_FEATURE_LEVEL_10_0,
        D3D_FEATURE_LEVEL_9_3,
        D3D_FEATURE_LEVEL_9_2,
        D3D_FEATURE_LEVEL_9_1
};

// Create the Direct3D 11 API device object and a corresponding context.
ComPtr<ID3D11Device> device;
ComPtr<ID3D11DeviceContext> d3dContext;

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &d3dContext // Returns the device immediate context.
);
```

## <a name="drawing-into-a-texture-or-pixmap-resource"></a>Dibujar en un recurso de textura o mapa de píxeles


Para dibujar en una textura con OpenGL ES 2.0, configura un búfer de píxeles, o PBuffer. Después de que le hayas configurado y creado una EGLSurface correctamente, puedes proporcionarle un contexto de representación y ejecutar la canalización de sombreador para dibujar en la textura.

Dibujar en un búfer de píxeles con OpenGL ES 2.0

``` syntax
// Create a pixel buffer surface to draw into
EGLConfig pBufConfig;
EGLint totalpBufAttrs;

const EGLint pBufConfigAttrs[] =
{
    // Configure the pBuffer here...
};
 
eglChooseConfig(eglDsplay, pBufConfigAttrs, &pBufConfig, 1, &totalpBufAttrs);
EGLSurface pBuffer = eglCreatePbufferSurface(eglDisplay, pBufConfig, EGL_TEXTURE_RGBA); 
```

En Direct3D 11, debes crear un recurso [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) y convertirlo en un destino de representación. Configurar el destino de representación con [ **D3D11\_REPRESENTAR\_destino\_vista\_DESC**](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc). Cuando se llama a la [ **ID3D11DeviceContext::Draw** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) método (o un dibujo similar\* operación en el contexto de dispositivo) con este destino de representación, los resultados se dibujan en una textura.

Dibujar en una textura con Direct3D 11

``` syntax
ComPtr<ID3D11Texture2D> renderTarget1;

D3D11_RENDER_TARGET_VIEW_DESC renderTargetDesc = {0};
// Configure renderTargetDesc here ...

m_d3dDevice->CreateRenderTargetView(
  renderTarget1.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);

// Later, in your render loop...

// Set the render target for the draw operation.
m_d3dContext->OMSetRenderTargets(
        1,
        d3dRenderTargetView.GetAddressOf(),
        nullptr);
```

Esta textura puede pasarse a un sombreador si está asociada a una interfaz [**ID3D11ShaderResourceView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview).

## <a name="drawing-to-the-screen"></a>Dibujar en la pantalla


Cuando hayas usado el elemento EGLContext para configurar los búferes y actualizar tus datos, ejecuta los sombreadores que tenga vinculados y lleva los resultados al búfer de reserva mediante glDrawElements. Muestras el búfer de reserva llamando a eglSwapBuffers.

OpenGL ES 2.0: Dibuja en la pantalla.

``` syntax
glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
```

En Direct3D 11, debes configurar los búferes y vincular los sombreadores con el método [**IDXGISwapChain::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1). A continuación, llame a uno de los [ **ID3D11DeviceContext1::Draw** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) \* métodos para ejecutar los sombreadores y dibujar los resultados en un destino de representación configurado como el búfer de reserva para la cadena de intercambio. Una vez hecho esto, solo debes llevar el búfer de reserva a la presentación llamando al método **IDXGISwapChain::Present1**.

Direct3D 11: Dibuja en la pantalla.

``` syntax

m_d3dContext->DrawIndexed(
        m_indexCount,
        0,
        0);

// ...

m_swapChainCoreWindow->Present1(1, 0, &parameters);
```

## <a name="releasing-graphics-resources"></a>Liberar recursos gráficos


En EGL, liberas los recursos de ventana pasando EGLDisplay a eglTerminate.

Finalizar una presentación con EGL 1.4

```cpp
EGLBoolean eglTerminate(eglDisplay);
```

En una aplicación para UWP, puedes cerrar CoreWindow con [**CoreWindow::Close**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.close), aunque esto solo puede usarse para ventanas de interfaz de usuario secundarias. El subproceso de interfaz de usuario principal y su elemento CoreWindow asociado no pueden cerrarse. En cambio, el sistema operativo define el momento de su expiración. No obstante, cuando se cierra un elemento CoreWindow secundario, se genera el evento [**CoreWindow::Closed**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.closed).

## <a name="api-reference-mapping-for-egl-to-direct3d-11"></a>Asignación de referencia de la API de EGL a Direct3D 11


| API de EGL                          | Comportamiento o API de Direct3D 11 similar                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eglBindAPI                       | N/D.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglBindTexImage                  | Llama a [**ID3D11Device::CreateTexture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) para establecer una textura 2D.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglChooseConfig                  | Direct3D no proporciona un conjunto de configuraciones de búfer de cuadros predeterminada. Configuración de la cadena de intercambio                                                                                                                                                                                                                                                                                                                                                                                           |
| eglCopyBuffers                   | Para copiar los datos de un búfer, llama a [**ID3D11DeviceContext::CopyStructureCount**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copystructurecount). Para copiar un recurso, llama a [**ID3DDeviceCOntext::CopyResource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource).                                                                                                                                                                                                                                                      |
| eglCreateContext                 | Crea el contexto de un dispositivo de Direct3D llamando a [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice); esto devolverá un identificador a un dispositivo de Direct3D y un contexto inmediato de Direct3D (objeto [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)). También puedes crear un contexto diferido de Direct3D llamando a [**ID3D11Device2::CreateDeferredContext**](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nf-d3d11_2-id3d11device2-createdeferredcontext2) en el objeto [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) devuelto. |
| eglCreatePbufferFromClientBuffer | Todos los búferes se leen y escriben como un subrecurso de Direct3D como, por ejemplo, [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d). Copia contenido de un tipo de subrecurso compatible a otro, mediante métodos como [**ID3D11DeviceContext1:CopyResource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-copyresource).                                                                                                                                                                                                     |
| eglCreatePbufferSurface          | Para crear un dispositivo de Direct3D sin una cadena de intercambio, llama al método estático [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Para obtener una vista de destino de representación de Direct3D, llama a [**ID3D11Device::CreateRenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview).                                                                                                                                                                                                                               |
| eglCreatePixmapSurface           | Para crear un dispositivo de Direct3D sin una cadena de intercambio, llama al método estático [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Para obtener una vista de destino de representación de Direct3D, llama a [**ID3D11Device::CreateRenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview).                                                                                                                                                                                                                               |
| eglCreateWindowSurface           | Obtén una interfaz [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1) (para los búferes de presentación) y una interfaz [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) (es una interfaz virtual para el dispositivo gráfico y sus recursos). Usa la interfaz **ID3D11Device1** para definir una interfaz [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) que puedas usar para crear el búfer de cuadros que proporcionaste a **IDXGISwapChain1**.                                                                                         |
| eglDestroyContext                | N/D. Usa [**ID3D11DeviceContext::DiscardView1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-discardview1) para deshacerte de una vista de destino de representación. Para cerrar la interfaz [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) principal, debes establecer la instancia en NULL y esperar a que la plataforma reclame sus recursos. No puedes destruir el contexto de dispositivo directamente.                                                                                                                                                |
| eglDestroySurface                | N/D. Los recursos gráficos se borrarán cuando la plataforma cierre el elemento CoreWindow de la aplicación para UWP.                                                                                                                                                                                                                                                                                                                                                                                                 |
| eglGetCurrentDisplay             | Llama a [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) para obtener una referencia a la ventana actual de la aplicación principal.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetCurrentSurface             | Esta es la interfaz [**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) actual. Generalmente, se asigna al objeto del procesador.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetError                      | Se obtienen errores como HRESULT, que devuelven la mayoría de los métodos en las interfaces de DirectX. Si el método no devuelve un error de tipo HRESULT, llama a [**GetLastError**](https://docs.microsoft.com/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror). Para convertir un error del sistema en un valor HRESULT, utilice el [**HRESULT\_FROM\_WIN32**](https://docs.microsoft.com/windows/desktop/api/winerror/nf-winerror-hresult_from_win32) macro.                                                                                                                                                                                                  |
| eglInitialize                    | Llama a [**CoreWindow::GetForCurrentThread**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.getforcurrentthread) para obtener una referencia a la ventana actual de la aplicación principal.                                                                                                                                                                                                                                                                                                                                                         |
| eglMakeCurrent                   | Establece un destino de representación para dibujar en el contexto actual con el método [**ID3D11DeviceContext1::OMSetRenderTargets**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets).                                                                                                                                                                                                                                                                                                                                  |
| eglQueryContext                  | N/D. No obstante, puedes adquirir destinos de representación desde una instancia [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1), así como ciertos datos de configuración. (Consulta el vínculo para obtener la lista de los métodos disponibles).                                                                                                                                                                                                                                                                                           |
| eglQuerySurface                  | N/D. No obstante, puedes adquirir datos acerca de las ventanillas y el hardware gráfico actual de los métodos de una instancia [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1). (Consulta el vínculo para obtener la lista de los métodos disponibles).                                                                                                                                                                                                                                                                               |
| eglReleaseTexImage               | N/D.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglReleaseThread                 | Para realizar una operación general de multithreading de GPU, consulta el artículo [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                                                              |
| eglSurfaceAttrib                 | Use [ **D3D11\_REPRESENTAR\_destino\_vista\_DESC** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_view_desc) para configurar una vista de destino de representación de Direct3D,                                                                                                                                                                                                                                                                                                                                                               |
| eglSwapBuffers                   | Usa [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).                                                                                                                                                                                                                                                                                                                                                                                                                     |
| eglSwapInterval                  | Consulta [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1).                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| eglTerminate                     | El sistema operativo administra el elemento CoreWindow que se usa para mostrar la salida de la canalización de gráficos.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglWaitClient                    | Para superficies compartidas, usa IDXGIKeyedMutex. Para realizar una operación general de multithreading de GPU, consulta el artículo [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitGL                        | Para superficies compartidas, usa IDXGIKeyedMutex. Para realizar una operación general de multithreading de GPU, consulta el artículo [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitNative                    | Para superficies compartidas, usa IDXGIKeyedMutex. Para realizar una operación general de multithreading de GPU, consulta el artículo [Multithreading](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-render-multi-thread-intro).                                                                                                                                                                                                                                                                                                                                    |

 

 

 




