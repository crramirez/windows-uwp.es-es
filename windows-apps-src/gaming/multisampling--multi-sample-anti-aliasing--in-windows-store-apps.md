---
title: Muestreo múltiple en aplicaciones para la Plataforma universal de Windows (UWP)
description: Aprende a usar el muestreo múltiple en aplicaciones para la Plataforma universal de Windows (UWP) compiladas con Direct3D.
ms.assetid: 1cd482b8-32ff-1eb0-4c91-83eb52f08484
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, muestreo múltiple, direct3d, games, multisampling
ms.localizationpriority: medium
ms.openlocfilehash: b547e47b7d896ab818349dcc70ee9dc3c7078847
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368385"
---
# <a name="span-iddevgamingmultisamplingmulti-sampleantialiasinginwindowsstoreappsspan-multisampling-in-universal-windows-platform-uwp-apps"></a><span id="dev_gaming.multisampling__multi-sample_anti_aliasing__in_windows_store_apps"></span> El muestreo múltiple en Universal Windows Platform (UWP) aplicaciones



Aprende a usar el muestreo múltiple en aplicaciones para la Plataforma universal de Windows (UWP) compiladas con Direct3D. El muestreo múltiple, también conocido como suavizado de contorno de muestras múltiples, es una técnica gráfica que se usa para disminuir la apariencia de bordes afilados. Se aplica dibujando más píxeles de los que existen en el destino de representación y después promediando valores para mantener la apariencia de un borde "parcial" en ciertos píxeles. Si quieres ver una descripción más minuciosa de cómo funciona el muestreo múltiple en Direct3D, consulta [Multisample Anti-Aliasing Rasterization Rules (Reglas de rasterización para el suavizado de contorno de muestras múltiples)](https://docs.microsoft.com/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage-rules).

## <a name="multisampling-and-the-flip-model-swap-chain"></a>Muestreo múltiple y la cadena de intercambio en el modelo flip


Las aplicaciones para UWP con DirectX deben usar cadenas de intercambio en el modelo flip. Las cadenas de intercambio en el modelo flip no admiten el muestreo múltiple directamente, pero este se puede aplicar de una manera diferente representando la escena en una vista de destino de representación de muestras múltiples y luego resolviendo ese destino en el búfer de reserva antes de la presentación. En este artículo se describen los pasos necesarios para agregar el muestreo múltiple a una aplicación para UWP.

### <a name="how-to-use-multisampling"></a>Procedimiento para usar el muestreo múltiple

Los niveles de característica de Direct3D garantizan la compatibilidad con funciones mínimas y específicas para el recuento de muestras y garantizan la disponibilidad de ciertos formatos de búfer que admitirán el muestreo múltiple. Los dispositivos de gráficos, por lo general, admiten una mayor variedad de formatos y recuentos de muestras que la cantidad mínima requerida. La compatibilidad con el muestreo múltiple se puede determinar en tiempo de ejecución. Para ello, debes comprobar la compatibilidad de las características con el muestreo múltiple en formatos DXGI específicos y, a continuación, debes comprobar los recuentos de muestras que se pueden usar con cada formato admitido.

1.  Llama a [**ID3D11Device::CheckFeatureSupport**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) para averiguar qué formatos DXGI se pueden usar con el muestreo múltiple. Suministra los formatos de destino de representación que tu juego puede usar. El destino de representación y el destino de la resolución debe usar el mismo formato, así que compruebe para ambos [ **D3D11\_formato\_soporte\_MULTIMUESTRA\_RENDERTARGET** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_format_support) y **D3D11\_formato\_soporte\_MULTIMUESTRA\_resolver**.

    **La característica de nivel de 9:  ** Aunque la característica de nivel 9 dispositivos [garantizar la compatibilidad con formatos de destino de representación de muestreo múltiple](https://docs.microsoft.com/previous-versions//ff471324(v=vs.85)), no se garantiza la compatibilidad para destinos de resolución de muestreo múltiple. Por lo tanto, debes realizar esta comprobación antes de usar la técnica del muestreo múltiple que se describe en este tema.

    El código siguiente comprueba el muestreo múltiple de soporte técnico para todos lo DXGI\_valores de formato:

    ```cpp
    // Determine the format support for multisampling.
    for (UINT i = 1; i < DXGI_FORMAT_MAX; i++)
    {
        DXGI_FORMAT inFormat = safe_cast<DXGI_FORMAT>(i);
        UINT formatSupport = 0;
        HRESULT hr = m_d3dDevice->CheckFormatSupport(inFormat, &formatSupport);

        if ((formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RESOLVE) &&
            (formatSupport & D3D11_FORMAT_SUPPORT_MULTISAMPLE_RENDERTARGET)
            )
        {
            m_supportInfo->SetFormatSupport(i, true);
        }
        else
        {
            m_supportInfo->SetFormatSupport(i, false);
        }
    }
    ```

2.  Por cada formato admitido, consulta la compatibilidad del recuento de muestras llamando a [**ID3D11Device::CheckMultisampleQualityLevels**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkmultisamplequalitylevels).

    El siguiente código comprueba la compatibilidad del tamaño de muestras en formatos DXGI admitidos:

    ```cpp
    // Find available sample sizes for each supported format.
    for (unsigned int i = 0; i < DXGI_FORMAT_MAX; i++)
    {
        for (unsigned int j = 1; j < MAX_SAMPLES_CHECK; j++)
        {
            UINT numQualityFlags;

            HRESULT test = m_d3dDevice->CheckMultisampleQualityLevels(
                (DXGI_FORMAT) i,
                j,
                &numQualityFlags
                );

            if (SUCCEEDED(test) && (numQualityFlags > 0))
            {
                m_supportInfo->SetSampleSize(i, j, 1);
                m_supportInfo->SetQualityFlagsAt(i, j, numQualityFlags);
            }
        }
    }
    ```

    > **Tenga en cuenta**    Use [ **ID3D11Device2::CheckMultisampleQualityLevels1** ](https://docs.microsoft.com/windows/desktop/api/d3d11_2/nf-d3d11_2-id3d11device2-checkmultisamplequalitylevels1) en su lugar si necesita comprobar compatibilidad de muestreo múltiple para los búferes de recursos en mosaico.

     

3.  Crea un búfer y una vista de destino de representación con el recuento de muestras deseado. Usar el mismo DXGI\_formato, ancho y alto que la cadena de intercambio, pero especifique un número de muestras mayor que 1 y usar una dimensión de textura de muestreo múltiple (**D3D11\_RTV\_dimensión\_TEXTURE2DMS** por ejemplo). Si es necesario, puedes volver a crear la cadena de intercambio mediante una configuración nueva que sea adecuada para el muestreo múltiple.

    El siguiente código crea un destino de representación de muestras múltiples:

    ```cpp
    float widthMulti = m_d3dRenderTargetSize.Width;
    float heightMulti = m_d3dRenderTargetSize.Height;

    D3D11_TEXTURE2D_DESC offScreenSurfaceDesc;
    ZeroMemory(&offScreenSurfaceDesc, sizeof(D3D11_TEXTURE2D_DESC));

    offScreenSurfaceDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
    offScreenSurfaceDesc.Width = static_cast<UINT>(widthMulti);
    offScreenSurfaceDesc.Height = static_cast<UINT>(heightMulti);
    offScreenSurfaceDesc.BindFlags = D3D11_BIND_RENDER_TARGET;
    offScreenSurfaceDesc.MipLevels = 1;
    offScreenSurfaceDesc.ArraySize = 1;
    offScreenSurfaceDesc.SampleDesc.Count = m_sampleSize;
    offScreenSurfaceDesc.SampleDesc.Quality = m_qualityFlags;

    // Create a surface that's multisampled.
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &offScreenSurfaceDesc,
        nullptr,
        &m_offScreenSurface)
        );

    // Create a render target view. 
    CD3D11_RENDER_TARGET_VIEW_DESC renderTargetViewDesc(D3D11_RTV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
        m_offScreenSurface.Get(),
        &renderTargetViewDesc,
        &m_d3dRenderTargetView
        )
        );
    ```

4.  El búfer de profundidad debe tener el mismo ancho, alto, recuento de muestras y dimensión de textura para que coincida con el destino de representación de muestras múltiples.

    El siguiente código crea un búfer de profundidad de varias muestras:

    ```cpp
    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT,
        static_cast<UINT>(widthMulti),
        static_cast<UINT>(heightMulti),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL,
        D3D11_USAGE_DEFAULT,
        0,
        m_sampleSize,
        m_qualityFlags
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
        &depthStencilDesc,
        nullptr,
        &depthStencil
        )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2DMS);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
        depthStencil.Get(),
        &depthStencilViewDesc,
        &m_d3dDepthStencilView
        )
        );
    ```

5.  Ahora es un buen momento para crear la ventanilla, porque esta también debe coincidir con el destino de representación en ancho y alto.

    El siguiente código crea una ventanilla:

    ```cpp
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        widthMulti / m_scalingFactor,
        heightMulti / m_scalingFactor
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

6.  Representa cada fotograma en el destino de representación de muestras múltiples. Cuando la representación esté completa, llama a [**ID3D11DeviceContext::ResolveSubresource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-resolvesubresource) antes de presentar el fotograma. Esta acción le indica a Direct3D que debe llevar a cabo la operación de muestreo múltiple, calculando el valor de cada píxel para mostrar y colocando el resultado en el búfer de reserva. El búfer de reserva incluye la imagen final con suavizado de contorno y puede presentarse.

    El siguiente código resuelve el subrecurso antes de presentar el fotograma:

    ```cpp
    if (m_sampleSize > 1)
    {
        unsigned int sub = D3D11CalcSubresource(0, 0, 1);

        m_d3dContext->ResolveSubresource(
            m_backBuffer.Get(),
            sub,
            m_offScreenSurface.Get(),
            sub,
            DXGI_FORMAT_B8G8R8A8_UNORM
            );
    }

    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    hr = m_swapChain->Present(1, 0);
    ```

 

 




