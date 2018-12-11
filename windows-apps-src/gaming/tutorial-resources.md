---
title: Extender la muestra de juego
description: Aprende a implementar una superposición de XAML para un juego DirectX UWP.
keywords: DirectX, XAML
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7cb1c9f9cf6cbc6cce0c5d4547ed503bb9a06e56
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8891000"
---
# <a name="extend-the-game-sample"></a>Amplía el juego de ejemplo

Llegados a este punto, ya hemos hablado de los componentes clave de un juego 3D DirectX para la Plataforma universal de Windows (UWP). Puedes establecer el marco para un juego, incluida la canalización de representación y el proveedor de vista, e implementar un bucle de juego básico. También puedes crear una superposición de interfaz de usuario básica, incorporar sonidos e implementar controles. Estás a punto de crear un juego propio, pero si necesitas más ayuda e información, échale un vistazo a estos recursos.

-   [Gráficos y juegos DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)
-   [Introducción a Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476345)
-   [Referencia de Direct3D11](https://msdn.microsoft.com/library/windows/desktop/ff476147)

## <a name="using-xaml-for-the-overlay"></a>Uso de XAML para la superposición


Una opción que no hemos tratado en profundidad es el uso de XAML en lugar de [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) para la superposición. XAML presenta muchas ventajas con respecto a Direct2D para dibujar elementos de interfaz de usuario. La ventaja más importante es que facilita mucho incorporar la apariencia de Windows 10 en tu juego DirectX. Muchos de los elementos, estilos y comportamientos comunes que definen a una aplicación para UWP están estrechamente integrados en el modelo XAML, por lo que implementarlos supone mucho menos trabajo para el desarrollador del juego. Si tu diseño de juego tiene una interfaz de usuario complicada, piensa en la posibilidad de usar XAML en lugar de Direct2D.

Con XAML, podemos hacer una interfaz de juego similar a la de Direct2D hecha anteriormente.

### <a name="xaml"></a>XAML
![Superposición XAML](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![Superposición D2D](./images/simple-dx-game-extend-d2d.PNG)

Aunque los resultados finales son similares, hay una serie de diferencias entre la implementación de interfaces Direct2D y XAML.

Función | XAML| Direct2D
:----------|:----------- | :-----------
Superposición de definición | Definido en un archivo XAML, `\*.xaml`. Al entender XAML, crear y configurar superposiciones más complicadas es más sencillo que con Direct2D.| Definida como una colección de primitivos Direct2D y cadenas [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) colocadas y escritas manualmente en un búfer de destino Direct2D. 
Elemento de la interfaz de usuario | Los elementos de la interfaz de usuario XAML proceden de elementos estandarizados que forman parte de las API XAML de Windows Runtime, incluidos [**Windows::UI::Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) y [**Windows::UI::Xaml::Controls**](https://msdn.microsoft.com/library/windows/apps/br227716). El código que controla el comportamiento de los elementos de interfaz de usuario XAML se define en un archivo de código subyacente: Main.xaml.cpp. | Es posible dibujar formas simples como rectángulos y puntos suspensivos.
Cambio de tamaño de ventana | Controla de manera natural los cambios de tamaño y los eventos de cambio de estado de vista, y transforma la superposición en correspondencia. | Es necesario especificar manualmente cómo deben redibujarse los componentes de la superposición.


Otra gran diferencia tiene que ver con la [cadena de intercambio](https://docs.microsoft.com/windows/uwp/graphics-concepts/swap-chains). No es necesario adjuntar la cadena de intercambio a un objeto [**Windows::UI::Core::CoreWindow**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow). En su lugar, una aplicación DirectX que incorpora XAML asocia una cadena de intercambio cuando se genera un nuevo objeto [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel). 

El siguiente fragmento de código muestra cómo declarar XAML para **SwapChainPanel** en el archivo [**DirectXPage.xaml**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml).
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

El objeto **SwapChainPanel** se establece como la propiedad [**Contenido**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.Content) del objeto de ventana actual creado [al ejecutar](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51) el singleton de la aplicación.

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```


Para adjuntar la cadena de intercambio configurada a la instancia [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) definida en tu XAML, debes obtener un puntero a la implementación de la interfaz [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/dn302143) nativa subyacente y llamar a [**ISwapChainPanelNative::SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144) en ella, pasándole tu cadena de intercambio configurada. 

El siguiente fragmento de [**DX::DeviceResources::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) detalla esto para la interoperabilidad DirectX/XAML:

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

Para obtener más información sobre este proceso, consulta el tema sobre [Interoperabilidad de DirectX y XAML](directx-and-xaml-interop.md).

## <a name="sample"></a>Muestra

Para descargar una versión de este juego que usa XAML para la superposición, ve al [Ejemplo de juego de disparos Direct3D (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml).


A diferencia de la versión de la muestra de juego que se explica en el resto de estos temas, la versión XAML define su marco en los archivos [App.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) y [DirectXPage.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp), en lugar de hacerlo en [App.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) y [GameInfoOverlay.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp) respectivamente.