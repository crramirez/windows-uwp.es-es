---
author: mtoepke
title: Representar la escena con prueba de profundidad
description: Crea un efecto de sombra agregando pruebas de profundidad al sombreador de vértices (o geometría) y al sombreador de píxeles.
ms.assetid: bf496dfb-d7f5-af6b-d588-501164608560
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, games, juegos, rendering, representación, scene, escena, depth testing, pruebas de profundidad, direct3d, shadows, sombras
ms.localizationpriority: medium
ms.openlocfilehash: dc776a60e771cc8d5961e8c7b9c67eb99fabea3a
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5933797"
---
# <a name="render-the-scene-with-depth-testing"></a>Representar la escena con prueba de profundidad




Crea un efecto de sombra agregando pruebas de profundidad al sombreador de vértices (o geometría) y al sombreador de píxeles. Parte 3 de [Tutorial: implementar volúmenes de sombra con búferes de profundidad en Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="include-transformation-for-light-frustum"></a>Incluir la transformación para el frustum de la luz


Tu sombreador de vértices necesita calcular la posición transformada del espacio de luz para cada vértice. Proporciona el modelo de espacio de luz y las matrices de proyección con un búfer de constantes. También puedes usar este búfer de constantes para proporcionar la posición de la luz y la normal para los cálculos de iluminación. La posición transformada en el espacio de luz se usará durante la prueba de profundidad.

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    float4 modelPos = mul(pos, model);
    pos = mul(modelPos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    // Transform the vertex position into projected space from the POV of the light.
    float4 lightSpacePos = mul(modelPos, lView);
    lightSpacePos = mul(lightSpacePos, lProjection);
    output.lightSpacePos = lightSpacePos;

    // Light ray
    float3 lRay = lPos.xyz - modelPos.xyz;
    output.lRay = lRay;
    
    // Camera ray
    output.view = eyePos.xyz - modelPos.xyz;

    // Transform the vertex normal into world space.
    float4 norm = float4(input.norm, 1.0f);
    norm = mul(norm, model);
    output.norm = norm.xyz;
    
    // Pass through the color and texture coordinates without modification.
    output.color = input.color;
    output.tex = input.tex;

    return output;
}
```

Luego el sombreador de píxeles usará la posición del espacio de luz interpolado que proporcionó el sombreador de vértices para probar si el píxel está en la sombra.

## <a name="test-whether-the-position-is-in-the-light-frustum"></a>Probar si la posición está en el frustum de la luz


Primero comprueba que el píxel esté en el frustum de vista de la luz normalizando las coordenadas X e Y. Si ambas están en el rango \[0, 1\], es posible que el píxel esté en la sombra. De lo contrario, puedes omitir la prueba de profundidad. Un sombreador puede probar esto rápidamente llamando a [Saturar](https://msdn.microsoft.com/library/windows/desktop/hh447231) y comparando el resultado con el valor original.

```cpp
// Compute texture coordinates for the current point's location on the shadow map.
float2 shadowTexCoords;
shadowTexCoords.x = 0.5f + (input.lightSpacePos.x / input.lightSpacePos.w * 0.5f);
shadowTexCoords.y = 0.5f - (input.lightSpacePos.y / input.lightSpacePos.w * 0.5f);
float pixelDepth = input.lightSpacePos.z / input.lightSpacePos.w;

float lighting = 1;

// Check if the pixel texture coordinate is in the view frustum of the 
// light before doing any shadow work.
if ((saturate(shadowTexCoords.x) == shadowTexCoords.x) &&
    (saturate(shadowTexCoords.y) == shadowTexCoords.y) &&
    (pixelDepth > 0))
{
```

## <a name="depth-test-against-the-shadow-map"></a>Prueba de profundidad con el mapa de sombras


Usa una función de comparación de muestras ([SampleCmp](https://msdn.microsoft.com/library/windows/desktop/bb509696) o [SampleCmpLevelZero](https://msdn.microsoft.com/library/windows/desktop/bb509697)) para probar la profundidad del píxel en espacio de luz con respecto al mapa de profundidad. Calcula el valor de profundidad de espacio de luz normalizada, que es `z / w`, y pasa el valor a la función de comparación. Dado que usamos una prueba de comparación LessOrEqual para el muestrario, la función intrínseca devuelve cero cuando se aprueba la prueba de comparación. Esto indica que el píxel está en sombra.

```cpp
// Use an offset value to mitigate shadow artifacts due to imprecise 
// floating-point values (shadow acne).
//
// This is an approximation of epsilon * tan(acos(saturate(NdotL))):
float margin = acos(saturate(NdotL));
#ifdef LINEAR
// The offset can be slightly smaller with smoother shadow edges.
float epsilon = 0.0005 / margin;
#else
float epsilon = 0.001 / margin;
#endif
// Clamp epsilon to a fixed range so it doesn't go overboard.
epsilon = clamp(epsilon, 0, 0.1);

// Use the SampleCmpLevelZero Texture2D method (or SampleCmp) to sample from 
// the shadow map, just as you would with Direct3D feature level 10_0 and
// higher.  Feature level 9_1 only supports LessOrEqual, which returns 0 if
// the pixel is in the shadow.
lighting = float(shadowMap.SampleCmpLevelZero(
    shadowSampler,
    shadowTexCoords,
    pixelDepth + epsilon
    )
    );
```

## <a name="compute-lighting-in-or-out-of-shadow"></a>Calcular la iluminación dentro o fuera de la sombra


Si el píxel no está en sombra, el sombreador de píxeles debe calcular la iluminación directa y agregarla al valor del píxel.

```cpp
return float4(input.color * (ambient + DplusS(N, L, NdotL, input.view)), 1.f);
```

```cpp
float3 DplusS(float3 N, float3 L, float NdotL, float3 view)
{
    const float3 Kdiffuse = float3(.5f, .5f, .4f);
    const float3 Kspecular = float3(.2f, .2f, .3f);
    const float exponent = 3.f;

    // Compute the diffuse coefficient.
    float diffuseConst = saturate(NdotL);

    // Compute the diffuse lighting value.
    float3 diffuse = Kdiffuse * diffuseConst;

    // Compute the specular highlight.
    float3 R = reflect(-L, N);
    float3 V = normalize(view);
    float3 RdotV = dot(R, V);
    float3 specular = Kspecular * pow(saturate(RdotV), exponent);

    return (diffuse + specular);
}
```

De lo contrario, el sombreador de píxeles debe calcular el valor del píxel con iluminación ambiente.

```cpp
return float4(input.color * ambient, 1.f);
```

En la siguiente parte de este tutorial, aprenderás sobre la [compatibilidad con mapas de sombras en una variedad de hardware](target-a-range-of-hardware.md).

 

 




