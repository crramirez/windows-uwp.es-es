---
author: mtoepke
title: Compatibilidad con mapas de sombras en una variedad de hardware
description: "Representa sombras de alta fidelidad en dispositivos más rápidos, y sombras más rápidas en dispositivos menos eficaces."
ms.assetid: d97c0544-44f2-4e29-5e02-54c45e0dff4e
translationtype: Human Translation
ms.sourcegitcommit: d403e78b775af0f842ba2172295a09e35015dcc8
ms.openlocfilehash: 0cdc31f07560e7f1747806d1436bccbc1e50f8b9

---

# Compatibilidad con mapas de sombras en una variedad de hardware


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Representa sombras de alta fidelidad en dispositivos más rápidos, y sombras más rápidas en dispositivos menos eficaces. Parte 4 de [Tutorial: implementar volúmenes de sombra con búferes de profundidad en Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## Comparación de tipos de filtros


Usa solamente el filtrado lineal si el dispositivo puede tolerar una disminución de rendimiento. Por lo general, los dispositivos con nivel de característica 9\_1 de Direct3D no tienen suficiente energía para usar en sombras con filtrado lineal. En estos dispositivos, usa en cambio el filtrado de punto. Cuando usas el filtrado lineal, ajusta el sombreador de píxeles para que combine los contornos de sombra.

Crea el muestrario de comparación para el filtrado de punto:

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

Luego crea un muestrario para el filtrado lineal:

```cpp
comparisonSamplerDesc.Filter = D3D11_FILTER_COMPARISON_MIN_MAG_MIP_LINEAR;
DX::ThrowIfFailed(
    pD3DDevice->CreateSamplerState(
        &comparisonSamplerDesc,
        &m_comparisonSampler_linear
        )
    );
```

Elige un muestrario:

```cpp
ID3D11PixelShader* pixelShader;
ID3D11SamplerState** comparisonSampler;
if (m_useLinear)
{
    pixelShader = m_shadowPixelShader_linear.Get();
    comparisonSampler = m_comparisonSampler_linear.GetAddressOf();
}
else
{
    pixelShader = m_shadowPixelShader_point.Get();
    comparisonSampler = m_comparisonSampler_point.GetAddressOf();
}

// Attach our pixel shader.
context->PSSetShader(
    pixelShader,
    nullptr,
    0
    );

context->PSSetSamplers(0, 1, comparisonSampler);
context->PSSetShaderResources(0, 1, m_shadowResourceView.GetAddressOf());
```

Combina contornos de sombra con filtrado lineal:

```cpp
// Blends the shadow area into the lit area.
float3 light = lighting * (ambient + DplusS(N, L, NdotL, input.view));
float3 shadow = (1.0f - lighting) * ambient;
return float4(input.color * (light + shadow), 1.f);
```

## Tamaño del búfer de sombras


Si bien los mapas de sombras más grandes no tienen tanto efecto pixelado, requieren más espacio en la memoria de gráficos. Experimenta con distintos tamaños de mapas de sombras en tu juego y observa los resultados en distintos tipos de dispositivos y distintos tipos de pantalla. Considera una optimización, como mapas de sombras en cascada, para obtener mejores resultados con menos memoria de gráficos. Consulta [Técnicas habituales para mejorar los mapas de profundidad de sombras](https://msdn.microsoft.com/library/windows/desktop/ee416324).

## Profundidad del búfer de sombras


Una mayor precisión en el búfer de sombras dará resultados de prueba de profundidad más precisos, lo que ayuda a evitar problemas de rivalidad en el búfer z (z-buffer fighting). Pero al igual que los mapas de sombras más grandes, una mayor precisión requiere más memoria. Prueba distintos tipos de precisión de profundidad en tu juego, DXGI\_FORMAT\_R24G8\_TYPELESS contra DXGI\_FORMAT\_R16\_TYPELESS, y observa la velocidad y calidad en los distintos niveles de característica.

## Optimizar sombreadores precompilados


Las aplicaciones para la Plataforma universal de Windows (UWP) pueden usar compilación dinámica de sombreador, pero resulta más rápido usar vínculos dinámicos de sombreador. También puedes usar directivas de compilador y bloques `#ifdef` para crear versiones distintas de sombreadores. Esto se realiza al abrir el archivo del proyecto de Visual Studio en un editor de texto y agregar varias entradas `<FxcCompiler>` para el HLSL (cada una con las definiciones de preprocesador apropiadas). Ten en cuenta que para esto se necesitan nombres de archivo diferentes. En este caso, Visual Studio anexa \_point y \_linear a las distintas versiones del sombreador.

La entrada del archivo del proyecto para la versión de filtro lineal del sombreador se define LINEAR:

```xml
<FxCompile Include="Content\ShadowPixelShader.hlsl">
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|x64'">Pixel</ShaderType>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">false</DisableOptimizations>
  <EnableDebuggingInformation Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">true</EnableDebuggingInformation>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">false</DisableOptimizations>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">false</DisableOptimizations>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|x64'">$(OutDir)%(Filename)_linear.cso</ObjectFileOutput>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">LINEAR</PreprocessorDefinitions>
  <PreprocessorDefinitions Condition="'$(Configuration)|$(Platform)'=='Release|x64'">LINEAR</PreprocessorDefinitions>
</FxCompile>
```

La entrada del archivo del proyecto para la versión de filtro lineal del sombreador no incluye definiciones de preprocesador:

```xml
<FxCompile Include="Content\ShadowPixelShader.hlsl">
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">Pixel</ShaderType>
  <ShaderType Condition="'$(Configuration)|$(Platform)'=='Release|x64'">Pixel</ShaderType>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">false</DisableOptimizations>
  <EnableDebuggingInformation Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">true</EnableDebuggingInformation>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">false</DisableOptimizations>
  <DisableOptimizations Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">false</DisableOptimizations>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|ARM'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|ARM'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|Win32'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|Win32'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Debug|x64'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
  <ObjectFileOutput Condition="'$(Configuration)|$(Platform)'=='Release|x64'">$(OutDir)%(Filename)_point.cso</ObjectFileOutput>
</FxCompile>
```

 

 







<!--HONumber=Jun16_HO4-->


