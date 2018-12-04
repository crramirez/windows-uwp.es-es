---
title: Comparar código de EGL con DXGI y Direct3D
description: DirectX Graphics Interface (DXGI) y varias API de Direct3D cumplen el mismo rol que EGL. Este tema te ayudará a comprender DXGI y Direct3D 11 desde la perspectiva de EGL.
ms.assetid: 90f5ecf1-dd5d-fea3-bed8-57a228898d2a
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, EGL, DXGI, Direct3D
ms.localizationpriority: medium
ms.openlocfilehash: 1279d5100aa00e1b94d7d56b472a0574d22c3416
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8486302"
---
# <a name="compare-egl-code-to-dxgi-and-direct3d"></a>Comparar código EGL con DXGI y Direct3D




**API importantes**

-   [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)
-   [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)
-   [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)

DirectX Graphics Interface (DXGI) y varias API de Direct3D cumplen el mismo rol que EGL. Este tema ayuda a comprender DXGI y Direct3D 11 desde la perspectiva de EGL.

DXGI y Direct3D, al igual que EGL, proporcionan métodos para configurar recursos gráficos, obtener un contexto de representación en el que tus sombreadores puedan dibujar y para mostrar los resultados en una ventana. No obstante, DXGI y Direct3D tienen algunas opciones más y demandan más esfuerzo para configurarse correctamente cuando se portan desde EGL.

> **Nota**  esta guía se basa en especificación abierta del grupo Khronos para EGL 1.4, se encuentra aquí: [Khronos Native Platform Graphics Interface (EGL Version 1.4 - 6 de April de 2011) \[PDF\]](http://www.khronos.org/registry/egl/specs/eglspec.1.4.20110406.pdf). Las diferencias en la sintaxis específica de otras plataformas y lenguajes de desarrollo no se analizan en esta guía.

 

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

Para ver el proceso básico de Direct3D para configurar la canalización de gráficos, echa un vistazo a la plantilla DirectX 11 App (Universal Windows) en Microsoft Visual Studio2015. La clase de representación de base que puedes encontrar en ella proporciona un buen punto de referencia para configurar tanto la infraestructura gráfica de Direct3D 11 como sus recursos básicos, así como para admitir características de aplicación de la Plataforma universal de Windows (UWP) tales como la rotación de la pantalla.

EGL tiene muy pocas API relacionadas con Direct3D 11, y navegar en este último puede ser un desafío si no estás familiarizado con la nomenclatura y la terminología particular de la plataforma. He aquí una sencilla introducción que te ayudará a orientarte.

Primero, revisa el objeto de EGL básico para la asignación de interfaz de Direct3D:

| Abstracción de EGL | Representación de Direct3D similar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **EGLDisplay**  | En Direct3D (para aplicaciones para UWP), se obtiene el identificador de presentación a través de la API [**Windows::UI::CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) (o la interfaz de **ICoreWindowInterop** que expone el HWND). La configuración de hardware y adaptador se establece con las interfaces COM [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523) y [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/hh404543) respectivamente.                                                                                                                                                                                                                                                           |
| **EGLSurface**  | En Direct3D, los búferes y demás recursos de ventana (bien sean visibles o estén fuera de pantalla) se crean y configuran mediante interfaces de DXGI específicas; entre ellas, [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556), que es una implementación de patrón de fábrica usada para adquirir recursos de DXGI como [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) (búferes de presentación). La interfaz [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) que representa el dispositivo gráfico y sus recursos, se adquiere mediante la función [**D3D11Device::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Para destinos de representación, usa la interfaz [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582). |
| **EGLContext**  | En Direct3D, puedes configurar y emitir comandos para la canalización de gráficos con la interfaz [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **EGLConfig**   | En Direct3D 11, puedes crear y configurar recursos gráficos como un búferes, texturas, galerías de símbolos y sombreadores gracias a los métodos de la interfaz [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575).                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

 

A continuación, te describimos el modo más básico de configurar una pantalla de gráficos sencilla, así como los recursos y el contexto en DXGI y Direct3D, de una aplicación para UWP.

1.  Llama a [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) para obtener un identificador del objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) para el subproceso de la interfaz de usuario principal de la aplicación.
2.  En el caso de las aplicaciones para UWP, obtén una cadena de intercambio de [**IDXGIAdapter2**](https://msdn.microsoft.com/library/windows/desktop/hh404537) con [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559), y pásala a la referencia de la clase [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que obtuviste en el paso 1. Obtendrás a cambio una instancia [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631). Define su ámbito en el objeto de representador y su subproceso de representación.
3.  Llama al método [**D3D11Device::CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) para obtener las instancias [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) y [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598). Define sus ámbitos también en el objeto de representador.
4.  Crea sombreadores, texturas y otros recursos mediante los métodos del objeto [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) del representador.
5.  Define búferes, ejecuta los sombreadores y administra los estados de canalización con los métodos del objeto [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) del representador.
6.  Cuando la canalización se haya ejecutado y se haya dibujado un fotograma en el búfer de reserva, muéstralo en pantalla con [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797).

Echa un vistazo a [Getting Started with DirectX Graphics (Introducción a los gráficos DirectX)](https://msdn.microsoft.com/library/windows/desktop/hh309467) para estudiar este proceso con mayor detenimiento. En lo que queda de artículo se abordarán muchos de los pasos básicos habituales para configurar y administrar la canalización de gráficos.
> **Nota**  aplicaciones de escritorio de Windows tienen diferentes API para obtener una cadena de intercambio de Direct3D, como [**d3d11device:: createdeviceandswapchain**](https://msdn.microsoft.com/library/windows/desktop/ff476083)y no se usa un objeto de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) .

 

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

En Direct3D, la ventana principal de una aplicación para UWP se representa mediante el objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225), que puede obtenerse del objeto de la aplicación llamando a [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589), como parte del proceso de inicialización del "suministrador de vistas" que construyes para Direct3D. (Si estás usando la interoperabilidad de Direct3D-XAML, usa el proveedor de vistas del marco de XAML). El proceso para crear un suministrador de vistas de Direct3D se analiza en el tema acerca de [cómo configurar una aplicación para mostrar una vista](https://msdn.microsoft.com/library/windows/apps/hh465077).

Obtener CoreWindow para Direct3D.

``` syntax
CoreWindow::GetForCurrentThread();
```

Cuando se obtiene la referencia [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225), debe activarse la ventana; dicha acción ejecuta el método **Run** del objeto principal y comienza el procesamiento de eventos de la ventana. Después de hacer esto, crea una interfaz [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) y otra [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598), y úsalas para obtener las interfaces subyacentes [**IDXGIDevice1**](https://msdn.microsoft.com/library/windows/desktop/ff471331) y [**IDXGIAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174523); gracias a esto, podrás obtener un objeto [**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556) para poder crear una cadena de intercambio basada en la configuración [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528).

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

Llama al método [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797) después de preparar un cuadro con el fin de presentarlo.

Ten en cuenta que en Direct3D 11, no existe una abstracción igual a EGLSurface. (Existe [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343), pero se usa de manera diferente). La aproximación conceptual más cercana es el objeto [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) que usamos para asignar una textura ([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)) como el búfer de reserva en el que nuestra canalización de sombreador dibujará.

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

Un procedimiento recomendado es llamar a este código siempre que se cree la ventana o se cambie su tamaño. Durante la representación, debes establecer la vista de destino de representación con [**ID3D11DeviceContext1::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464) antes de configurar cualquier otro subrecurso como sombreadores o búferes de vértices.

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

Un contexto de representación en Direct3D 11 se representa mediante un objeto [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575), el cual representa el adaptador y te permite crear recursos de Direct3D como búferes y sombreadores; además, el objeto [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598), te permite administrar la canalización de gráficos y ejecutar los sombreadores.

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

En Direct3D 11, debes crear un recurso [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635) y convertirlo en un destino de representación. Configura el destino de representación mediante [**D3D11\_RENDER\_TARGET\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476201). Cuando llames al método [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407) (o a una operación Draw\* similar en el contexto del dispositivo) mediante este destino de representación, los resultados se dibujan en una textura.

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

Esta textura puede pasarse a un sombreador si está asociada a una interfaz [**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628).

## <a name="drawing-to-the-screen"></a>Dibujar en la pantalla


Cuando hayas usado el elemento EGLContext para configurar los búferes y actualizar tus datos, ejecuta los sombreadores que tenga vinculados y lleva los resultados al búfer de reserva mediante glDrawElements. Muestras el búfer de reserva llamando a eglSwapBuffers.

Open GL ES 2.0: dibujar en pantalla.

``` syntax
glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
```

En Direct3D 11, debes configurar los búferes y vincular los sombreadores con el método [**IDXGISwapChain::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797). A continuación, llama a uno de los métodos [**ID3D11DeviceContext1::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407)\* para ejecutar los sombreadores y dibujar los resultados en un destino de representación que se haya configurado como el búfer de reserva de la cadena de intercambio. Una vez hecho esto, solo debes llevar el búfer de reserva a la presentación llamando al método **IDXGISwapChain::Present1**.

Direct3D 11: dibujar en pantalla.

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

En una aplicación para UWP, puedes cerrar CoreWindow con [**CoreWindow::Close**](https://msdn.microsoft.com/library/windows/apps/br208260), aunque esto solo puede usarse para ventanas de interfaz de usuario secundarias. El subproceso de interfaz de usuario principal y su elemento CoreWindow asociado no pueden cerrarse. En cambio, el sistema operativo define el momento de su expiración. No obstante, cuando se cierra un elemento CoreWindow secundario, se genera el evento [**CoreWindow::Closed**](https://msdn.microsoft.com/library/windows/apps/br208261).

## <a name="api-reference-mapping-for-egl-to-direct3d-11"></a>Asignación de referencia de la API de EGL a Direct3D 11


| API de EGL                          | Comportamiento o API de Direct3D 11 similar                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| eglBindAPI                       | N/C.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglBindTexImage                  | Llama a [**ID3D11Device::CreateTexture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476521) para establecer una textura 2D.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglChooseConfig                  | Direct3D no proporciona un conjunto de configuraciones de búfer de cuadros predeterminada. Configuración de la cadena de intercambio                                                                                                                                                                                                                                                                                                                                                                                           |
| eglCopyBuffers                   | Para copiar los datos de un búfer, llama a [**ID3D11DeviceContext::CopyStructureCount**](https://msdn.microsoft.com/library/windows/desktop/ff476393). Para copiar un recurso, llama a [**ID3DDeviceCOntext::CopyResource**](https://msdn.microsoft.com/library/windows/desktop/ff476392).                                                                                                                                                                                                                                                      |
| eglCreateContext                 | Crea el contexto de un dispositivo de Direct3D llamando a [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082); esto devolverá un identificador a un dispositivo de Direct3D y un contexto inmediato de Direct3D (objeto [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)). También puedes crear un contexto diferido de Direct3D llamando a [**ID3D11Device2::CreateDeferredContext**](https://msdn.microsoft.com/library/windows/desktop/dn280495) en el objeto [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) devuelto. |
| eglCreatePbufferFromClientBuffer | Todos los búferes se leen y escriben como un subrecurso de Direct3D como, por ejemplo, [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635). Copia contenido de un tipo de subrecurso compatible a otro, mediante métodos como [**ID3D11DeviceContext1:CopyResource**](https://msdn.microsoft.com/library/windows/desktop/ff476392).                                                                                                                                                                                                     |
| eglCreatePbufferSurface          | Para crear un dispositivo de Direct3D sin una cadena de intercambio, llama al método estático [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Para obtener una vista de destino de representación de Direct3D, llama a [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517).                                                                                                                                                                                                                               |
| eglCreatePixmapSurface           | Para crear un dispositivo de Direct3D sin una cadena de intercambio, llama al método estático [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Para obtener una vista de destino de representación de Direct3D, llama a [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517).                                                                                                                                                                                                                               |
| eglCreateWindowSurface           | Obtén una interfaz [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) (para los búferes de presentación) y una interfaz [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) (es una interfaz virtual para el dispositivo gráfico y sus recursos). Usa la interfaz **ID3D11Device1** para definir una interfaz [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) que puedas usar para crear el búfer de cuadros que proporcionaste a **IDXGISwapChain1**.                                                                                         |
| eglDestroyContext                | N/C. Usa [**ID3D11DeviceContext::DiscardView1**](https://msdn.microsoft.com/library/windows/desktop/jj247573) para deshacerte de una vista de destino de representación. Para cerrar la interfaz [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) principal, debes establecer la instancia en NULL y esperar a que la plataforma reclame sus recursos. No puedes destruir el contexto de dispositivo directamente.                                                                                                                                                |
| eglDestroySurface                | N/C. Los recursos gráficos se borrarán cuando la plataforma cierre el elemento CoreWindow de la aplicación para UWP.                                                                                                                                                                                                                                                                                                                                                                                                 |
| eglGetCurrentDisplay             | Llama a [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) para obtener una referencia a la ventana actual de la aplicación principal.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetCurrentSurface             | Esta es la interfaz [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) actual. Generalmente, se asigna al objeto del procesador.                                                                                                                                                                                                                                                                                                                                                         |
| eglGetError                      | Se obtienen errores como HRESULT, que devuelven la mayoría de los métodos en las interfaces de DirectX. Si el método no devuelve un error de tipo HRESULT, llama a [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360). Para convertir un error del sistema en anHRESULTvalue, usa el[**HRESULT\_FROM\_WIN32**](https://msdn.microsoft.com/library/windows/desktop/ms680746)macro.                                                                                                                                                                                                  |
| eglInitialize                    | Llama a [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) para obtener una referencia a la ventana actual de la aplicación principal.                                                                                                                                                                                                                                                                                                                                                         |
| eglMakeCurrent                   | Establece un destino de representación para dibujar en el contexto actual con el método [**ID3D11DeviceContext1::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464).                                                                                                                                                                                                                                                                                                                                  |
| eglQueryContext                  | N/C. No obstante, puedes adquirir destinos de representación desde una instancia [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575), así como ciertos datos de configuración. (Consulta el vínculo para obtener la lista de los métodos disponibles).                                                                                                                                                                                                                                                                                           |
| eglQuerySurface                  | N/C. No obstante, puedes adquirir datos acerca de las ventanillas y el hardware gráfico actual de los métodos de una instancia [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575). (Consulta el vínculo para obtener la lista de los métodos disponibles).                                                                                                                                                                                                                                                                               |
| eglReleaseTexImage               | N/C.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| eglReleaseThread                 | Para realizar una operación general de multithreading de GPU, consulta el artículo [Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                                                              |
| eglSurfaceAttrib                 | Usa [**D3D11\_RENDER\_TARGET\_VIEW\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476201) para configurar una vista de destino de representación de Direct3D.                                                                                                                                                                                                                                                                                                                                                               |
| eglSwapBuffers                   | Usa [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797).                                                                                                                                                                                                                                                                                                                                                                                                                     |
| eglSwapInterval                  | Consulta [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631).                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| eglTerminate                     | El sistema operativo administra el elemento CoreWindow que se usa para mostrar la salida de la canalización de gráficos.                                                                                                                                                                                                                                                                                                                                                                                          |
| eglWaitClient                    | Para superficies compartidas, usa IDXGIKeyedMutex. Para un multithreading de GPU general, lee[ Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitGL                        | Para superficies compartidas, usa IDXGIKeyedMutex. Para un multithreading de GPU general, lee[ Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                    |
| eglWaitNative                    | Para superficies compartidas, usa IDXGIKeyedMutex. Para un multithreading de GPU general, lee[ Multithreading](https://msdn.microsoft.com/library/windows/desktop/ff476891).                                                                                                                                                                                                                                                                                                                                    |

 

 

 




