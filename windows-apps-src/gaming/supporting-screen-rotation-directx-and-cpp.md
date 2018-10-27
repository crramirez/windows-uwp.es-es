---
author: mtoepke
title: Compatibilidad con la orientación de pantalla (DirectX y C++)
description: Aquí se comentan los procedimientos recomendados para controlar la rotación de pantalla en la aplicación DirectX para UWP, para que el hardware de gráficos del dispositivo Windows 10 se usan de forma eficaz y eficiente.
ms.assetid: f23818a6-e372-735d-912b-89cabeddb6d4
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, orientación de pantalla, directx
ms.localizationpriority: medium
ms.openlocfilehash: 4ed8739f8ba7b2049af154d458ccaa831b8526a5
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5706262"
---
# <a name="supporting-screen-orientation-directx-and-c"></a>Compatibilidad con la orientación de pantalla (DirectX y C++)



Su aplicación de la Plataforma universal de Windows (UWP) puede admitir varias orientaciones de pantalla cuando controles el evento [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268). Aquí se comentan los procedimientos recomendados para controlar la rotación de pantalla en la aplicación DirectX para UWP, para que el hardware de gráficos del dispositivo Windows 10 se usan de forma eficaz y eficiente.

Antes de comenzar, recuerda que el hardware gráfico siempre reproduce los datos en píxeles del mismo modo, independientemente de la orientación del dispositivo. Los dispositivos de Windows 10 pueden determinar su orientación de pantalla actual (mediante algún tipo de sensor o con un botón de alternancia de software) y permitir que los usuarios cambiar la configuración de pantalla. Debido a esto, Windows 10 propio controla la rotación de las imágenes para asegurarse de que son "verticales" en función de la orientación del dispositivo. De manera predeterminada, tu aplicación recibe la notificación de que algo cambió de orientación, como por ejemplo, el tamaño de una ventana. Cuando esto ocurre, Windows 10 gira inmediatamente la imagen para la presentación final. Para tres de las cuatro orientaciones de pantalla específicas (se explica más adelante), Windows 10 usa otros recursos gráficos y cálculos para mostrar la imagen final.

Para aplicaciones DirectX de UWP, el objeto [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258) proporciona datos de orientación de pantalla básicos que tu aplicación puede consultar. La orientación predeterminada es *horizontal*, donde el ancho de píxeles de la pantalla es mayor que el alto; la orientación alternativa es *vertical*, donde la pantalla se gira 90 grados en cualquier dirección y el ancho se vuelve menor que el alto.

Windows 10 define cuatro modos de orientación de pantalla específicos:

-   Horizontal: el valor predeterminado orientación para Windows 10 de la pantalla y se considera el ángulo base o identidad para la rotación (0 grados).
-   Vertical: la pantalla se giró 90 grados en el sentido de las agujas del reloj (o 270 grados en el sentido contrario de las agujas del reloj).
-   Horizontal, volteada: la pantalla se giró 180 grados (se dio la vuelta).
-   Vertical, volteada: la pantalla se giró 270 grados en el sentido de las agujas del reloj (o 90 grados en el sentido contrario de las agujas del reloj).

Cuando la pantalla se gira de una orientación a otra, Windows 10 realiza internamente una operación de rotación para alinear la imagen con la nueva orientación y el usuario ve una imagen derecha en la pantalla.

Además, Windows 10 muestra automáticamente animaciones de transición para crear una experiencia del usuario al cambiar de una orientación a otra. Cuando cambia la orientación de pantalla, el usuario ve estos cambios como una animación de rotación y zoom fijo de la imagen de pantalla mostrada. Windows 10 asigna tiempo a la aplicación para el diseño en la nueva orientación.

Este es el proceso general para administrar los cambios en la orientación de pantalla:

1.  Usa una combinación de valores de límites de la ventana y los datos de la orientación de pantalla para mantener la cadena de intercambio alineada con la orientación de pantalla nativa del dispositivo.
2.  Notificar a Windows 10 la orientación de la cadena de intercambio con [**idxgiswapchain1:: SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801).
3.  Cambia el código de representación para generar imágenes alineadas con la orientación del usuario del dispositivo.

## <a name="resizing-the-swap-chain-and-pre-rotating-its-contents"></a>Cambiar el tamaño de la cadena de intercambio y girar previamente su contenido


Para realizar un cambio de tamaño básico de pantalla y girar previamente su contenido en tu aplicación DirectX de UWP, sigue estos pasos:

1.  Controla el evento [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268).
2.  Cambia el tamaño de la cadena de intercambio a las nuevas dimensiones de la ventana.
3.  Llama a [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) para establecer la orientación de la cadena de intercambio.
4.  Vuelve a crear cualquier recurso que dependa del tamaño de ventana, como los destinos de representación y otros búferes de datos de píxeles.

Ahora, echemos un vistazo más detallado a estos pasos.

El primer paso consiste en registrar un controlador para el evento [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268). Este evento se desencadena en tu aplicación cada vez que cambia la orientación de la ventana, como cuando se gira la pantalla.

Para administrar el evento [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268), conecta el controlador para **DisplayInformation::OrientationChanged** en el método [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) necesario, que es uno de los métodos de la interfaz [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) que el proveedor de vistas debe implementar.

En este ejemplo de código, el controlador de eventos [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) es un método llamado **OnOrientationChanged**. Cuando se genera **DisplayInformation::OrientationChanged**, a su vez, llama a un método denominado **SetCurrentOrientation** que, a continuación, llama a **CreateWindowSizeDependentResources**.

```cpp
void App::SetWindow(CoreWindow^ window)
{
  // ... Other UI event handlers assigned here ...
  
    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);

  // ...
}
}
```

```cpp
void App::OnOrientationChanged(DisplayInformation^ sender, Object^ args)
{
    m_deviceResources->SetCurrentOrientation(sender->CurrentOrientation);
    m_main->CreateWindowSizeDependentResources();
}

// This method is called in the event handler for the OrientationChanged event.
void DX::DeviceResources::SetCurrentOrientation(DisplayOrientations currentOrientation)
{
    if (m_currentOrientation != currentOrientation)
    {
        m_currentOrientation = currentOrientation;
        CreateWindowSizeDependentResources();
    }
}
```

A continuación, cambias el tamaño de la cadena de intercambio para la nueva orientación de pantalla y la preparas para girar el contenido de la canalización de elementos gráficos cuando se realiza la representación. En este ejemplo, **DirectXBase::CreateWindowSizeDependentResources** es un método que controla la llamada a IDXGISwapChain::ResizeBuffers, el establecimiento de una matriz de rotación 2D y 3D, la llamada a SetRotation y la nueva creación de recursos.

```cpp
void DX::DeviceResources::CreateWindowSizeDependentResources() 
{
    // Clear the previous window size specific context.
    ID3D11RenderTargetView* nullViews[] = {nullptr};
    m_d3dContext->OMSetRenderTargets(ARRAYSIZE(nullViews), nullViews, nullptr);
    m_d3dRenderTargetView = nullptr;
    m_d2dContext->SetTarget(nullptr);
    m_d2dTargetBitmap = nullptr;
    m_d3dDepthStencilView = nullptr;
    m_d3dContext->Flush();

    // Calculate the necessary render target size in pixels.
    m_outputSize.Width = DX::ConvertDipsToPixels(m_logicalSize.Width, m_dpi);
    m_outputSize.Height = DX::ConvertDipsToPixels(m_logicalSize.Height, m_dpi);
    
    // Prevent zero size DirectX content from being created.
    m_outputSize.Width = max(m_outputSize.Width, 1);
    m_outputSize.Height = max(m_outputSize.Height, 1);

    // The width and height of the swap chain must be based on the window's
    // natively-oriented width and height. If the window is not in the native
    // orientation, the dimensions must be reversed.
    DXGI_MODE_ROTATION displayRotation = ComputeDisplayRotation();

    bool swapDimensions = displayRotation == DXGI_MODE_ROTATION_ROTATE90 || displayRotation == DXGI_MODE_ROTATION_ROTATE270;
    m_d3dRenderTargetSize.Width = swapDimensions ? m_outputSize.Height : m_outputSize.Width;
    m_d3dRenderTargetSize.Height = swapDimensions ? m_outputSize.Width : m_outputSize.Height;

    if (m_swapChain != nullptr)
    {
        // If the swap chain already exists, resize it.
        HRESULT hr = m_swapChain->ResizeBuffers(
            2, // Double-buffered swap chain.
            lround(m_d3dRenderTargetSize.Width),
            lround(m_d3dRenderTargetSize.Height),
            DXGI_FORMAT_B8G8R8A8_UNORM,
            0
            );

        if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
        {
            // If the device was removed for any reason, a new device and swap chain will need to be created.
            HandleDeviceLost();

            // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
            // and correctly set up the new device.
            return;
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    else
    {
        // Otherwise, create a new one using the same adapter as the existing Direct3D device.
        DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

        swapChainDesc.Width = lround(m_d3dRenderTargetSize.Width); // Match the size of the window.
        swapChainDesc.Height = lround(m_d3dRenderTargetSize.Height);
        swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
        swapChainDesc.Stereo = false;
        swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
        swapChainDesc.SampleDesc.Quality = 0;
        swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
        swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
        swapChainDesc.Flags = 0;    
        swapChainDesc.Scaling = DXGI_SCALING_NONE;
        swapChainDesc.AlphaMode = DXGI_ALPHA_MODE_IGNORE;

        // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
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

        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForCoreWindow(
                m_d3dDevice.Get(),
                reinterpret_cast<IUnknown*>(m_window.Get()),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }

    // Set the proper orientation for the swap chain, and generate 2D and
    // 3D matrix transformations for rendering to the rotated swap chain.
    // Note the rotation angle for the 2D and 3D transforms are different.
    // This is due to the difference in coordinate spaces.  Additionally,
    // the 3D matrix is specified explicitly to avoid rounding errors.

    switch (displayRotation)
    {
    case DXGI_MODE_ROTATION_IDENTITY:
        m_orientationTransform2D = Matrix3x2F::Identity();
        m_orientationTransform3D = ScreenRotation::Rotation0;
        break;

    case DXGI_MODE_ROTATION_ROTATE90:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(90.0f) *
            Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
        m_orientationTransform3D = ScreenRotation::Rotation270;
        break;

    case DXGI_MODE_ROTATION_ROTATE180:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(180.0f) *
            Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
        m_orientationTransform3D = ScreenRotation::Rotation180;
        break;

    case DXGI_MODE_ROTATION_ROTATE270:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(270.0f) *
            Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
        m_orientationTransform3D = ScreenRotation::Rotation90;
        break;

    default:
        throw ref new FailureException();
    }


    //SDM: only instance of SetRotation
    DX::ThrowIfFailed(
        m_swapChain->SetRotation(displayRotation)
        );

    // Create a render target view of the swap chain back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );

    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT, 
        lround(m_d3dRenderTargetSize.Width),
        lround(m_d3dRenderTargetSize.Height),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
            &depthStencilDesc,
            nullptr,
            &depthStencil
            )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2D);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
            depthStencil.Get(),
            &depthStencilViewDesc,
            &m_d3dDepthStencilView
            )
        );
    
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);

    // Create a Direct2D target bitmap associated with the
    // swap chain back buffer and set it as the current target.
    D2D1_BITMAP_PROPERTIES1 bitmapProperties = 
        D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
            m_dpi,
            m_dpi
            );

    ComPtr<IDXGISurface2> dxgiBackBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiBackBuffer))
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiBackBuffer.Get(),
            &bitmapProperties,
            &m_d2dTargetBitmap
            )
        );

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());

    // Grayscale text anti-aliasing is recommended for all UWP apps.
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

}

```

Después de guardar los valores actuales de alto y ancho de la ventana para la próxima vez que se llame a este método, convierte los valores de píxel independiente de dispositivo (DIP) en píxeles para los límites de pantalla. En la muestra, llama a **ConvertDipsToPixels**, que es una función simple que devuelve este código:

` floor((dips * dpi / 96.0f) + 0.5f);`

Agrega 0,5f para redondear al valor entero más próximo.

Además, las coordenadas de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) siempre se definen en DIP. Para Windows 10 y versiones anteriores de Windows, un DIP se define como 1/96 de una pulgada y está alineado a la definición del sistema operativo de *arriba*. Cuando la orientación de la pantalla gira al modo vertical, la aplicación voltea el ancho y el alto de **CoreWindow** y el tamaño de destino de representación (límites) debe cambiar según corresponda. Dado que las coordenadas de Direct3D siempre se dan en píxeles físicos, debes realizar la conversión de valores DIP de **CoreWindow** a valores de píxel enteros antes de pasar estos valores a Direct3D para configurar la cadena de intercambio.

En lo que respecta al proceso, estás realizando un poco más de trabajo que si simplemente cambiases el tamaño de la cadena de cambio: en realidad estás girando los componentes de Direct2D y Direct3D de la imagen antes de componerlos para su presentación, y estás indicando a la cadena de cambio que has representado los resultados en una nueva orientación. Aquí mostramos más detalles sobre este proceso, como se muestra en el ejemplo de código para **DX::DeviceResources::CreateWindowSizeDependentResources**:

-   Determina la nueva orientación de la pantalla. Si la pantalla ha cambiado del modo horizontal al vertical, o viceversa, cambia los valores de alto y ancho por supuesto, cambia también de valores DIP a píxeles para los límites de pantalla.

-   A continuación, comprueba si la cadena de intercambio se ha creado. Si no se ha creado, créala llamando a [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559). De lo contrario, cambia el tamaño de los búferes de la cadena de intercambio actual con las nuevas dimensiones de pantalla, llamando a [**IDXGISwapchain:ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577). Aunque no es necesario cambiar el tamaño de la cadena de intercambio para el evento de rotación después de todo, estás representando el contenido que ya está girado por la canalización de representación hay otros eventos de cambio de tamaño, como los eventos de ajuste y relleno, para los que es necesario cambiar el tamaño.

-   Después de hacerlo, establece la transformación de matriz adecuada en 2D o 3D para aplicar a los píxeles o los vértices (respectivamente) en la canalización de elementos gráficos cuando se representen en la cadena de intercambio. Tenemos cuatro posibles matrices de rotación:

    -   horizontal (DXGI\_MODE\_ROTATION\_IDENTITY)
    -   vertical (DXGI\_MODE\_ROTATION\_ROTATE270)
    -   horizontal, volteada (DXGI\_MODE\_ROTATION\_ROTATE180)
    -   vertical, volteada (DXGI\_MODE\_ROTATION\_ROTATE90)

    La matriz correcta se selecciona en función de los datos proporcionados por Windows 10 (por ejemplo, los resultados de [**displayinformation:: OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268)) para determinar la orientación de la pantalla y se multiplicarán por las coordenadas de cada píxel (Direct2D) o vértice (Direct3D) en la escena, girándolos para alinearlo con la orientación de la pantalla. (Ten en cuenta que en Direct2D, el origen de la pantalla se define como la esquina superior izquierda, mientras que Direct3D el origen se define como el centro lógico de la ventana).

> **Nota**  para obtener más información sobre las transformaciones 2D usadas para que la rotación y cómo definirlas, consulta [matrices de definición para la rotación de pantalla (2D)](#appendix-a-applying-matrices-for-screen-rotation-2-d). Para obtener más información sobre las transformaciones 3D usadas para la rotación, consulta el tema sobre [definición de matrices para la rotación de pantalla (3D)](#appendix-b-applying-matrices-for-screen-rotation-3-d).

 

Ahora viene la parte importante: llama a [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801) e indica la matriz de rotación actualizada, como esta:

`m_swapChain->SetRotation(rotation);`

Almacena la matriz de rotación seleccionada donde pueda obtenerla el método de representación cuando calcule la nueva proyección. Usarás esta matriz cuando representes la proyección 3D final o compongas el diseño 2D final. (No se aplica automáticamente).

Después, crea un nuevo destino de representación para la vista 3D girada y un búfer de galería de símbolos de profundidad para la vista. Configura la ventanilla de representación 3D para la escena girada. Para ello, llama a [**ID3D11DeviceContext:RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480).

Por último, si tienes que girar o diseñar imágenes 2D, crea un destino de representación 2D como un mapa de bits de escritura para la cadena de intercambio con el tamaño cambiado mediante [**ID2D1DeviceContext::CreateBitmapFromDxgiSurface**](https://msdn.microsoft.com/library/windows/desktop/hh404482) y conforma el nuevo diseño para la nueva orientación actualizada. Establece cualquier propiedad que necesites en el destino de representación, como el modo de suavizado de contorno (como se muestra en el ejemplo de código).

A continuación, presenta la cadena de cambio.

## <a name="reduce-the-rotation-delay-by-using-corewindowresizemanager"></a>Reduce el retraso de rotación mediante CoreWindowResizeManager.


De manera predeterminada, Windows 10 proporciona una ventana corta pero apreciable de tiempo para cualquier aplicación, independientemente del modelo de aplicación o el idioma, para completar la rotación de la imagen. Sin embargo, lo más probable es que cuando la aplicación realice el cálculo de rotación mediante una de las técnicas que hemos descrito, lo realice mucho antes de que finalice este período de tiempo. Te gustaría aprovechar ese tiempo y completar la animación de la rotación, ¿verdad? Aquí es donde entra en juego [**CoreWindowResizeManager**](https://msdn.microsoft.com/library/windows/apps/jj215603).

Así es como se usa [**CoreWindowResizeManager**](https://msdn.microsoft.com/library/windows/apps/jj215603): cuando se desencadene un evento [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268), llama a [**CoreWindowResizeManager::GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh404170) dentro del controlador para que el evento obtenga una instancia de **CoreWindowResizeManager** y, cuando se complete y presente el diseño de la nueva orientación, llama a [**NotifyLayoutCompleted**](https://msdn.microsoft.com/library/windows/apps/jj215605) para que Windows sepa que puede completar la animación de rotación y mostrar la pantalla de la aplicación.

Este es el aspecto que tendrá el código en el controlador de eventos para [**DisplayInformation::OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268):

```cpp
CoreWindowResizeManager^ resizeManager = Windows::UI::Core::CoreWindowResizeManager::GetForCurrentView();

// ... build the layout for the new display orientation ...

resizeManager->NotifyLayoutCompleted();
```

Cuando un usuario gira la orientación de la pantalla, Windows 10 muestra una animación independiente de la aplicación como respuesta al usuario. Hay tres partes en esta animación que se producen en el siguiente orden:

-   Windows 10 reduce la imagen original.
-   Windows 10 conserva la imagen durante el tiempo que se tarda en reconstruir el nuevo diseño. Este es el período de tiempo que te interesa reducir, porque es probable que la aplicación no necesite todo este tiempo.
-   Cuando finaliza el período de tiempo o cuando se recibe una notificación de finalización del diseño, Windows gira la imagen y encadena zooms a la nueva orientación.

Como sugerimos en el tercer punto, cuando una aplicación llama a [**NotifyLayoutCompleted**](https://msdn.microsoft.com/library/windows/apps/jj215605), Windows 10 detiene el período de tiempo de espera, completa la animación de rotación y devuelve el control a la aplicación, que ahora se en la nueva orientación de pantalla. El efecto general es que la aplicación se muestra algo más fluida, responde más fácilmente y funciona de manera más eficaz.

## <a name="appendix-a-applying-matrices-for-screen-rotation-2-d"></a>Apéndice A: Aplicar matrices para la rotación de pantalla (2D)


En el código de ejemplo del tema [Cambiar el tamaño de la cadena de intercambio y girar previamente su contenido](#resizing-the-swap-chain-and-pre-rotating-its-contents) (y en la [muestra de rotación de la cadena de intercambio DXGI](http://go.microsoft.com/fwlink/p/?linkid=257600)), puede que hayas notado que teníamos matrices de rotación independientes para el resultado de Direct2D y de Direct3D. Primero echemos un vistazo a las matrices 2D.

No podemos aplicar las mismas matrices de rotación al contenido de Direct2D y Direct3D por dos motivos:

-   El primero es que usan distintos modelos de coordenadas cartesianas. Direct2D usa la regla derecha, donde la coordenada y aumenta en un valor positivo al subir desde el origen. Sin embargo, Direct3D usa la regla izquierda, donde la coordenada y aumenta en un valor positivo al avanzar a la derecha desde el origen. El resultado es que el origen de las coordenadas de pantalla para Direct2D se encuentra en la parte superior izquierda, mientras que el origen de la pantalla (el plano de proyección) para Direct3D se encuentra en la parte inferior izquierda. (Consulta el tema sobre [sistemas de coordenadas 3D](https://msdn.microsoft.com/library/windows/apps/bb324490.aspx) para obtener más información).

    ![sistema de coordenadas direct3d.](images/direct3d-origin.png)![sistema de coordenadas direct2d.](images/direct2d-origin.png)

-   El segundo motivo es que las matrices de rotación 3D deben especificarse explícitamente para evitar errores de redondeo.

La cadena de cambio asume que el origen se encuentra en la parte inferior izquierda, por lo que debes realizar una rotación para alinear el sistema de coordenadas de Direct2D situado a la derecha con el sistema situado a la izquierda que usa la cadena de cambio. Concretamente, debes cambiar la posición de la imagen en la nueva orientación situada a la izquierda. Para ello, multiplica la matriz de rotación por una matriz de traslación para el origen del sistema de coordenadas girado y transforma la imagen del espacio de coordenadas de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) al espacio de coordenadas de la cadena de intercambio. Tu aplicación también debe aplicar de manera coherente esta transformación cuando el destino de representación de Direct2D está conectado a la cadena de intercambio. Sin embargo, si la aplicación se representa en superficies intermedias que no están asociadas directamente a la cadena de intercambio, no apliques esta transformación del espacio de coordenadas.

Es posible que el código para seleccionar la matriz de corrección de las cuatro rotaciones posibles tenga este aspecto (ten en cuenta la traslación al nuevo origen del sistema de coordenadas):

```cpp
   
// Set the proper orientation for the swap chain, and generate 2D and
// 3D matrix transformations for rendering to the rotated swap chain.
// Note the rotation angle for the 2D and 3D transforms are different.
// This is due to the difference in coordinate spaces.  Additionally,
// the 3D matrix is specified explicitly to avoid rounding errors.

switch (displayRotation)
{
case DXGI_MODE_ROTATION_IDENTITY:
    m_orientationTransform2D = Matrix3x2F::Identity();
    m_orientationTransform3D = ScreenRotation::Rotation0;
    break;

case DXGI_MODE_ROTATION_ROTATE90:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(90.0f) *
        Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
    m_orientationTransform3D = ScreenRotation::Rotation270;
    break;

case DXGI_MODE_ROTATION_ROTATE180:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(180.0f) *
        Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
    m_orientationTransform3D = ScreenRotation::Rotation180;
    break;

case DXGI_MODE_ROTATION_ROTATE270:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(270.0f) *
        Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
    m_orientationTransform3D = ScreenRotation::Rotation90;
    break;

default:
    throw ref new FailureException();
}
    
```

Una vez que tengas la matriz de rotación y el origen correctos para la imagen 2D, establécelo con una llamada a [**ID2D1DeviceContext::SetTransform**](https://msdn.microsoft.com/library/windows/desktop/dd742857) entre las llamadas a [**ID2D1DeviceContext::BeginDraw**](https://msdn.microsoft.com/library/windows/desktop/dd371768) y [**ID2D1DeviceContext::EndDraw**](https://msdn.microsoft.com/library/windows/desktop/dd371924).

**Advertencia**  Direct2D no tiene ninguna pila de transformación. Si la aplicación también está usando [**ID2D1DeviceContext::SetTransform**](https://msdn.microsoft.com/library/windows/desktop/dd742857) como parte de su código de representación, esta matriz necesita multiplicarse posteriormente para cualquier otra transformación que hayas aplicado.

 

```cpp
    ID2D1DeviceContext* context = m_deviceResources->GetD2DDeviceContext();
    Windows::Foundation::Size logicalSize = m_deviceResources->GetLogicalSize();

    context->SaveDrawingState(m_stateBlock.Get());
    context->BeginDraw();

    // Position on the bottom right corner.
    D2D1::Matrix3x2F screenTranslation = D2D1::Matrix3x2F::Translation(
        logicalSize.Width - m_textMetrics.layoutWidth,
        logicalSize.Height - m_textMetrics.height
        );

    context->SetTransform(screenTranslation * m_deviceResources->GetOrientationTransform2D());

    DX::ThrowIfFailed(
        m_textFormat->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_TRAILING)
        );

    context->DrawTextLayout(
        D2D1::Point2F(0.f, 0.f),
        m_textLayout.Get(),
        m_whiteBrush.Get()
        );

    // Ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = context->EndDraw();
```

La próxima vez que presentes la cadena de intercambio, la imagen 2D se girará para coincidir con la nueva orientación de pantalla.

## <a name="appendix-b-applying-matrices-for-screen-rotation-3-d"></a>Apéndice B: Aplicar matrices para la rotación de pantalla (3D)


En el código de ejemplo del tema [Cambiar el tamaño de la cadena de intercambio y girar previamente su contenido](#resizing-the-swap-chain-and-pre-rotating-its-contents) (y en la [muestra de rotación de la cadena de intercambio DXGI](http://go.microsoft.com/fwlink/p/?linkid=257600)), hemos definido una matriz de transformación específica para cada posible orientación de pantalla. Ahora veamos las matrices para rotar escenas 3D. Al igual que antes, tienes que crear un conjunto de matrices para cada una de las cuatro orientaciones posibles. Para evitar errores de redondeo y, por tanto, pequeñas interferencias visuales, declara las matrices explícitamente en el código.

Establece estas matrices de rotación 3D del siguiente modo. Las matrices que se muestran en el siguiente ejemplo de código son matrices de rotación estándar para rotaciones de 0, 90, 180 y 270 grados de los vértices que definen puntos en el espacio de la escena 3D de la cámara. Cada valor de coordenada de vértice \[x, y, z\] de la escena se multiplica por esta matriz de rotación cuando se calcula la proyección 2D de la escena.

```cpp
   
// 0-degree Z-rotation
static const XMFLOAT4X4 Rotation0( 
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 90-degree Z-rotation
static const XMFLOAT4X4 Rotation90(
    0.0f, 1.0f, 0.0f, 0.0f,
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 180-degree Z-rotation
static const XMFLOAT4X4 Rotation180(
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, -1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 270-degree Z-rotation
static const XMFLOAT4X4 Rotation270( 
    0.0f, -1.0f, 0.0f, 0.0f,
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );            
    }
```

Para establecer el tipo de rotación en la cadena de intercambio, llama a [**IDXGISwapChain1::SetRotation**](https://msdn.microsoft.com/library/windows/desktop/hh446801), del siguiente modo:

`   m_swapChain->SetRotation(rotation);`

Después, en el método de representación, implementa un código similar a este:

``` syntax
struct ConstantBuffer // This struct is provided for illustration.
{
    // Other constant buffer matrices and data are defined here.

    float4x4 projection; // Current matrix for projection
} ;
ConstantBuffer  m_constantBufferData;          // Constant buffer resource data

// ...

// Rotate the projection matrix as it will be used to render to the rotated swap chain.
m_constantBufferData.projection = mul(m_constantBufferData.projection, m_rotationTransform3D);
```

Después, cuando llames al método de representación, la matriz de rotación actual (especificada por variable de clase **m\_orientationTransform3D**) se multiplica por la matriz de proyección actual y asigna los resultados de esta operación como la nueva matriz de proyección para el representador. Presenta la cadena de intercambio para ver la escena en la orientación de pantalla actualizada.

 

 




