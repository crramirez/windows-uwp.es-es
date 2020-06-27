---
title: Extender el juego de ejemplo
description: Obtenga información sobre cómo implementar una superposición de XAML para un juego DirectX de UWP.
keywords: DirectX, XAML
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06b52e5b6fdba1db83c941e770cd49360085accf
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409554"
---
# <a name="extend-the-sample-game"></a>Extender el juego de ejemplo

> [!NOTE]
> Este tema forma parte de la serie de tutoriales de [creación de una plataforma universal de Windows sencilla (UWP) con DirectX](tutorial--create-your-first-uwp-directx-game.md) . El tema de ese vínculo establece el contexto de la serie.

En este momento, hemos tratado los componentes clave de un juego de DirectX 3D básico de Plataforma universal de Windows (UWP). Puede configurar el marco de trabajo para un juego, incluidos el proveedor de vistas y la canalización de representación, e implementar un bucle de juego básico. También puede crear una superposición de la interfaz de usuario básica, incorporar sonidos e implementar controles. Está a su modo de crear un juego propio, pero si necesita más ayuda e información, consulte estos recursos.

-   [Gráficos y juegos de DirectX](/windows/desktop/directx)
-   [Introducción a Direct3D 11](/windows/desktop/direct3d11/dx-graphics-overviews)
-   [Referencia de Direct3D 11](/windows/desktop/direct3d11/d3d11-graphics-reference)

## <a name="using-xaml-for-the-overlay"></a>Usar XAML para la superposición

Una alternativa que no analizamos en profundidad es el uso de XAML en lugar de [Direct2D](/windows/desktop/Direct2D/direct2d-portal) para la superposición. XAML tiene muchas ventajas con respecto a Direct2D para dibujar elementos de la interfaz de usuario. La ventaja más importante es que facilita la incorporación de la apariencia de Windows 10 en su juego de DirectX. Muchos de los elementos, estilos y comportamientos comunes que definen a una aplicación para UWP están estrechamente integrados en el modelo XAML, por lo que implementarlos supone mucho menos trabajo para el desarrollador del juego. Si tu diseño de juego tiene una interfaz de usuario complicada, piensa en la posibilidad de usar XAML en lugar de Direct2D.

Con XAML, podemos crear una interfaz de juego que tenga un aspecto similar al de Direct2D que se realizó anteriormente.

### <a name="xaml"></a>XAML
![Superposición XAML](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![Superposición D2D](./images/simple-dx-game-extend-d2d.PNG)

Aunque tienen resultados finales similares, hay una serie de diferencias entre la implementación de las interfaces de Direct2D y XAML.

Característica | XAML| Direct2D
:----------|:----------- | :-----------
Definir superposición | Definido en un archivo XAML, `\*.xaml` . Una vez que entienda el código XAML, la creación y configuración de superposiciones más complicadas se convierten en simpiler en comparación con Direct2D.| Se define como una colección de primitivos de Direct2D y cadenas de [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) colocadas y escritas manualmente en un búfer de destino de direct2d. 
Elementos de la interfaz de usuario | Los elementos de la interfaz de usuario XAML proceden de elementos estandarizados que forman parte de la Windows Runtime las API XAML, incluidas [**Windows:: UI:: XAML**](/uwp/api/Windows.UI.Xaml) y [**Windows:: UI:: XAML:: Controls**](/uwp/api/Windows.UI.Xaml.Controls). El código que controla el comportamiento de los elementos de interfaz de usuario XAML se define en un archivo de código subyacente: Main.xaml.cpp. | Las formas simples se pueden dibujar como rectángulos y elipses.
Cambio de tamaño de ventana | Controla naturalmente el cambio de tamaño y la visualización de los eventos de cambio de estado, transformando la superposición en consecuencia | Es necesario especificar manualmente cómo volver a dibujar los componentes de la superposición.

Otra gran diferencia implica la [cadena de intercambio](/windows/uwp/graphics-concepts/swap-chains). No tiene que adjuntar la cadena de intercambio a un objeto [**Windows:: UI:: Core:: CoreWindow**](/uwp/api/windows.ui.core.corewindow) . En su lugar, una aplicación de DirectX que incorpora XAML asocia una cadena de intercambio cuando se construye un nuevo objeto [**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel) . 

En el fragmento de código siguiente se muestra cómo declarar XAML para **SwapChainPanel** en el archivo [**DirectXPage. Xaml**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml) .
```xml
<Page
    x:Class="Simple3DGameXaml.DirectXPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:Simple3DGameXaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">
    <SwapChainPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainPanel>
</Page>
```

El objeto **SwapChainPanel** se establece como la propiedad [**Content**](/uwp/api/Windows.UI.Xaml.Window.Content) del objeto Window actual creado [en el inicio](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51) por el singleton de la aplicación.

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```

Para asociar la cadena de intercambio configurada a la instancia de [**SwapChainPanel**](/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) definida por el código XAML, debe obtener un puntero a la implementación de la interfaz [**ISwapChainPanelNative**](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nn-windows-ui-xaml-media-dxinterop-iswapchainpanelnative) nativa subyacente y llamar a [**ISwapChainPanelNative:: SetSwapChain**](/windows/desktop/api/windows.ui.xaml.media.dxinterop/nf-windows-ui-xaml-media-dxinterop-iswapchainpanelnative-setswapchain) en ella, pasándole la cadena de intercambio configurada. 

El siguiente fragmento de código de [**DX::D eviceresources:: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) detalla esto para la interoperabilidad de DirectX/XAML:

```cpp
        ComPtr<IDXGIDevice3> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        // When using XAML interop, the swap chain must be created for composition.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Associate swap chain with SwapChainPanel
        // UI changes will need to be dispatched back to the UI thread
        m_swapChainPanel->Dispatcher->RunAsync(CoreDispatcherPriority::High, ref new DispatchedHandler([=]()
        {
            // Get backing native interface for SwapChainPanel
            ComPtr<ISwapChainPanelNative> panelNative;
            DX::ThrowIfFailed(
                reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(IID_PPV_ARGS(&panelNative))
                );
            DX::ThrowIfFailed(
                panelNative->SetSwapChain(m_swapChain.Get())
                );
        }, CallbackContext::Any));

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }
```

Para obtener más información sobre este proceso, consulta el tema sobre [interoperación entre DirectX y XAML](directx-and-xaml-interop.md).

## <a name="sample"></a>Muestra

Para descargar la versión de este juego que usa XAML para la superposición, vaya al [juego de ejemplo de filmación de Direct3D (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml).

A diferencia de la versión del juego de ejemplo que se describe en el resto de estos temas, la versión de XAML define su marco en los archivos [app. Xaml. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) y [DirectXPage. Xaml. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp) , en lugar de [app. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) y [GameInfoOverlay. cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp), respectivamente.
