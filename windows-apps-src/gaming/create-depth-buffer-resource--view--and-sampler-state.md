---
title: Crear recursos de dispositivo para búferes de profundidad
description: Aprende cómo crear los recursos de dispositivo Direct3D necesarios para admitir la realización de pruebas de profundidad para volúmenes de sombra.
ms.assetid: 86d5791b-1faa-17e4-44a8-bbba07062756
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, Direct3D, búfer de profundidad
ms.localizationpriority: medium
ms.openlocfilehash: dfd45f620addcf7a3f6292ed2257bdfccc862cd3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368892"
---
# <a name="create-depth-buffer-device-resources"></a>Crear recursos de dispositivo para búferes de profundidad




Aprende cómo crear los recursos de dispositivo Direct3D necesarios para admitir la realización de pruebas de profundidad para volúmenes de sombra. Parte 1 de [Tutorial: Implementar los volúmenes de instantáneas con búferes de profundidad en Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="resources-youll-need"></a>Recursos que necesitarás


Representar una asignación de profundidad para instantáneas de volumen requiere los siguientes recursos dependientes del dispositivo de Direct3D:

-   Un recurso (búfer) para la asignación de profundidad
-   Una vista de galería de símbolos de profundidad y una vista de recursos del sombreador para el recurso
-   Un objeto de estado de muestra de comparación
-   Búferes de constantes para matrices de POV livianas
-   Una ventanilla para representar la asignación de instantáneas (generalmente una ventanilla cuadrada)
-   Un objeto de estado de representación que permite la selección de la cara anterior
-   También necesitarás un objeto de estado de representación para volver a la selección de la cara anterior si no usas una ya.

Ten en cuenta que la creación de estos recursos debe incluirse en una rutina de creación de recursos dependientes de dispositivos. De esa manera tu procesador puede recrearlos si, por ejemplo, se instala un controlador de dispositivo nuevo o si el usuario mueve tu aplicación a un monitor conectado a una tarjeta gráfica diferente.

## <a name="check-feature-support"></a>Comprobar la compatibilidad de la característica


Antes de crear el mapa de profundidad, llame a la [ **CheckFeatureSupport** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) método en el dispositivo Direct3D, solicitar **D3D11\_característica\_D3D9\_ SOMBRA\_soporte**y proporcione un [ **D3D11\_característica\_datos\_D3D9\_SOMBRA\_soporte** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_feature_data_d3d9_shadow_support) estructura.

```cpp
D3D11_FEATURE_DATA_D3D9_SHADOW_SUPPORT isD3D9ShadowSupported;
ZeroMemory(&isD3D9ShadowSupported, sizeof(isD3D9ShadowSupported));
pD3DDevice->CheckFeatureSupport(
    D3D11_FEATURE_D3D9_SHADOW_SUPPORT,
    &isD3D9ShadowSupported,
    sizeof(D3D11_FEATURE_D3D9_SHADOW_SUPPORT)
    );

if (isD3D9ShadowSupported.SupportsDepthAsTextureWithLessEqualComparisonFilter)
{
    // Init shadow map resources

```

Si no se admite esta característica, no intente cargar los sombreadores compilados para nivel 4 del modelo de sombreador 9\_x que llaman a funciones de comparación de ejemplo. En muchos casos, la falta de compatibilidad para esta característica significa que la GPU es un dispositivo antiguo con un controlador que no está actualizado para admitir al menos WDDM 1.2. Si el dispositivo admite la característica de al menos nivel 10\_0, a continuación, se puede cargar un sombreador de comparación de ejemplo compilado para el modelo de sombreador 4\_0 en su lugar.

## <a name="create-depth-buffer"></a>Crear un búfer de profundidad


Primero, intenta crear la asignación de profundidad con un formato de profundidad de más alta precisión. Debes configurar primero las propiedades de vista de recurso de sombreador coincidentes. Si la creación de recursos es errónea, por ejemplo debido a poca memoria en el dispositivo o a un formato que el hardware no admite, prueba un formato de menor precisión y cambiar las propiedades para que coincidan.

Este paso es opcional si solo necesita un formato de profundidad de precisión baja, por ejemplo, al representar en el nivel de características de Direct3D 9 de resolución Media\_1 dispositivos.

```cpp
D3D11_TEXTURE2D_DESC shadowMapDesc;
ZeroMemory(&shadowMapDesc, sizeof(D3D11_TEXTURE2D_DESC));
shadowMapDesc.Format = DXGI_FORMAT_R24G8_TYPELESS;
shadowMapDesc.MipLevels = 1;
shadowMapDesc.ArraySize = 1;
shadowMapDesc.SampleDesc.Count = 1;
shadowMapDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE | D3D11_BIND_DEPTH_STENCIL;
shadowMapDesc.Height = static_cast<UINT>(m_shadowMapDimension);
shadowMapDesc.Width = static_cast<UINT>(m_shadowMapDimension);

HRESULT hr = pD3DDevice->CreateTexture2D(
    &shadowMapDesc,
    nullptr,
    &m_shadowMap
    );
```

Después crea las vistas de recurso. Establece el segmento de MIP en cero en la vista de galería de símbolos de profundidad y establece los niveles de MIP en 1 en la vista de recursos del sombreador. Ambos tienen una dimensión de textura de TEXTURE2D, y ambos deben usar una coincidencia [ **DXGI\_formato**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format).

```cpp
D3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc;
ZeroMemory(&depthStencilViewDesc, sizeof(D3D11_DEPTH_STENCIL_VIEW_DESC));
depthStencilViewDesc.Format = DXGI_FORMAT_D24_UNORM_S8_UINT;
depthStencilViewDesc.ViewDimension = D3D11_DSV_DIMENSION_TEXTURE2D;
depthStencilViewDesc.Texture2D.MipSlice = 0;

D3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc;
ZeroMemory(&shaderResourceViewDesc, sizeof(D3D11_SHADER_RESOURCE_VIEW_DESC));
shaderResourceViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
shaderResourceViewDesc.Format = DXGI_FORMAT_R24_UNORM_X8_TYPELESS;
shaderResourceViewDesc.Texture2D.MipLevels = 1;

hr = pD3DDevice->CreateDepthStencilView(
    m_shadowMap.Get(),
    &depthStencilViewDesc,
    &m_shadowDepthView
    );

hr = pD3DDevice->CreateShaderResourceView(
    m_shadowMap.Get(),
    &shaderResourceViewDesc,
    &m_shadowResourceView
    );
```

## <a name="create-comparison-state"></a>Crear estado de comparación


Ahora crea el objeto de estado de muestra de comparación. 9 de nivel de características\_1 solo es compatible con D3D11\_comparación\_menos\_igual. Las opciones de filtrado se explican con más profundidad en [Compatibilidad con mapas de sombras en una variedad de hardware](target-a-range-of-hardware.md) o simplemente puedes elegir el filtrado de puntos para asignaciones de instantáneas más rápidas.

Tenga en cuenta que puede especificar el D3D11\_textura\_dirección\_modo de direccionamiento de borde y trabajará en el nivel de característica 9\_1 dispositivos. Esto se aplica a los sombreadores de píxeles que no prueban si el píxel está en el frustum de visualización de la luz antes de hacer la prueba de profundidad. Especificando 0 o 1 para cada borde, puedes controlar si los píxeles que se encuentran fuera del frustum de visualización de la luz aprueban o no la prueba de profundidad y, por lo tanto, si están iluminados o en sombra.

En función de nivel 9\_1, los siguientes necesarios se deben establecer los valores: **MinLOD** se establece en cero, **MaxLOD** está establecido en **D3D11\_FLOAT32\_MAX**, y **MaxAnisotropy** se establece en cero.

```cpp
D3D11_SAMPLER_DESC comparisonSamplerDesc;
ZeroMemory(&comparisonSamplerDesc, sizeof(D3D11_SAMPLER_DESC));
comparisonSamplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_BORDER;
comparisonSamplerDesc.BorderColor[0] = 1.0f;
comparisonSamplerDesc.BorderColor[1] = 1.0f;
comparisonSamplerDesc.BorderColor[2] = 1.0f;
comparisonSamplerDesc.BorderColor[3] = 1.0f;
comparisonSamplerDesc.MinLOD = 0.f;
comparisonSamplerDesc.MaxLOD = D3D11_FLOAT32_MAX;
comparisonSamplerDesc.MipLODBias = 0.f;
comparisonSamplerDesc.MaxAnisotropy = 0;
comparisonSamplerDesc.ComparisonFunc = D3D11_COMPARISON_LESS_EQUAL;
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_POINT;

// Point filtered shadows can be faster, and may be a good choice when
// rendering on hardware with lower feature levels. This sample has a
// UI option to enable/disable filtering so you can see the difference
// in quality and speed.

DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_point
        )
    );
```

## <a name="create-render-states"></a>Crear estados de representación


Ahora crea un estado de representación que puedas usar para habilitar la selección de la cara anterior. Tenga en cuenta ese nivel de característica 9\_1 dispositivos requieren **DepthClipEnable** establecido en **true**.

```cpp
D3D11_RASTERIZER_DESC drawingRenderStateDesc;
ZeroMemory(&drawingRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
drawingRenderStateDesc.CullMode = D3D11_CULL_BACK;
drawingRenderStateDesc.FillMode = D3D11_FILL_SOLID;
drawingRenderStateDesc.DepthClipEnable = true; // Feature level 9_1 requires DepthClipEnable == true
DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &drawingRenderStateDesc,
        &m_drawingRenderState
        )
    );
```

Crea un estado de representación que puedas usar para habilitar la selección de la cara posterior. Si tu código de representación ya habilita la selección de la cara posterior, puedes omitir este paso.

```cpp
D3D11_RASTERIZER_DESC shadowRenderStateDesc;
ZeroMemory(&shadowRenderStateDesc, sizeof(D3D11_RASTERIZER_DESC));
shadowRenderStateDesc.CullMode = D3D11_CULL_FRONT;
shadowRenderStateDesc.FillMode = D3D11_FILL_SOLID;
shadowRenderStateDesc.DepthClipEnable = true;

DX::ThrowIfFailed(
    pD3DDevice->CreateRasterizerState(
        &shadowRenderStateDesc,
        &m_shadowRenderState
        )
    );
```

## <a name="create-constant-buffers"></a>Crear búferes de constantes


No te olvides de crear un búfer de constantes para representar desde el punto de visualización de la luz. También puedes usar este búfer de constantes para especificar la posición de la luz respecto del sombreador. Usa una matriz de perspectiva para las luces puntuales y usa una matriz ortogonal para las luces direccionales (como la luz del sol).

```cpp
DX::ThrowIfFailed(
    m_deviceResources->GetD3DDevice()->CreateBuffer(
        &viewProjectionConstantBufferDesc,
        nullptr,
        &m_lightViewProjectionBuffer
        )
    );
```

Rellena los datos de búfer de constantes. Actualiza los búferes de constantes una vez durante la inicialización y de nuevo si los valores de luz han cambiado desde el marco anterior.

```cpp
{
    XMMATRIX lightPerspectiveMatrix = XMMatrixPerspectiveFovRH(
        XM_PIDIV2,
        1.0f,
        12.f,
        50.f
        );

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.projection,
        XMMatrixTranspose(lightPerspectiveMatrix)
        );

    // Point light at (20, 15, 20), pointed at the origin. POV up-vector is along the y-axis.
    static const XMVECTORF32 eye = { 20.0f, 15.0f, 20.0f, 0.0f };
    static const XMVECTORF32 at = { 0.0f, 0.0f, 0.0f, 0.0f };
    static const XMVECTORF32 up = { 0.0f, 1.0f, 0.0f, 0.0f };

    XMStoreFloat4x4(
        &m_lightViewProjectionBufferData.view,
        XMMatrixTranspose(XMMatrixLookAtRH(eye, at, up))
        );

    // Store the light position to help calculate the shadow offset.
    XMStoreFloat4(&m_lightViewProjectionBufferData.pos, eye);
}
```

```cpp
context->UpdateSubresource(
    m_lightViewProjectionBuffer.Get(),
    0,
    NULL,
    &m_lightViewProjectionBufferData,
    0,
    0
    );
```

## <a name="create-a-viewport"></a>Crear una ventanilla


Necesitas una ventanilla separada para representar en la asignación de instantáneas. La ventanilla no es un recurso basado en dispositivos. Eres libre de crearla en cualquier otro lugar de tu código. Crear una ventanilla junto con la asignación de instantáneas puede ayudarte a que sea más conveniente mantener la dimensión de la ventanilla congruente con la dimensión de la asignación de instantáneas.

```cpp
// Init viewport for shadow rendering
ZeroMemory(&m_shadowViewport, sizeof(D3D11_VIEWPORT));
m_shadowViewport.Height = m_shadowMapDimension;
m_shadowViewport.Width = m_shadowMapDimension;
m_shadowViewport.MinDepth = 0.f;
m_shadowViewport.MaxDepth = 1.f;
```

En la parte siguiente de este tutorial, aprenderás a crear la asignación de instantáneas [representando en el búfer de profundidad](render-the-shadow-map-to-the-depth-buffer.md).

 

 




