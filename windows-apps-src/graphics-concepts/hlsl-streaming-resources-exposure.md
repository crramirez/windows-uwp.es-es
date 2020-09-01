---
title: Exposición de recursos de streaming de HLSL
description: Se requiere una sintaxis específica de lenguaje de sombreado de alto nivel (HLSL) de Microsoft para admitir recursos de streaming en el modelo de sombreador 5.
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- Exposición de recursos de streaming de HLSL
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: decb0ff2d791417326a70b06c0fdd6a25ea2d119
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173089"
---
# <a name="hlsl-streaming-resources-exposure"></a>Exposición de recursos de streaming de HLSL


Se requiere una sintaxis específica de lenguaje de sombreado de alto nivel (HLSL) de Microsoft para admitir recursos de streaming en el [modelo de sombreador 5](/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5).

La sintaxis de HLSL para el modelo de sombreador 5 solo se permite en dispositivos con compatibilidad con recursos de streaming. Cada método HLSL relevante para los recursos de streaming en la tabla siguiente acepta uno (comentarios) o dos (abrazadera y comentarios en este orden) parámetros opcionales adicionales. Por ejemplo, un método de **ejemplo** es:

**Ejemplo (muestra, ubicación \[ , desplazamiento \[ , abrazadera \[ , comentarios \] \] \] )**

Un ejemplo de método de **ejemplo** es [**Texture2D. sample (S, Float, int, Float, uint)**](/windows/desktop/direct3dhlsl/t2darray-sample-s-float-int-float-uint-).

Los parámetros offset, Clamp y feedback son opcionales. Debe especificar todos los parámetros opcionales hasta el que necesite, lo que es coherente con las reglas de C++ de los argumentos de función predeterminados. Por ejemplo, si se necesita el estado de los comentarios, los parámetros offset y Clamp deben proporcionarse explícitamente al **ejemplo**, aunque es posible que no sean necesarios lógicamente.

El parámetro Clamp es un valor Float escalar. El valor literal de Clamp = 0.0 f indica que no se realiza la operación de abrazadera.

El parámetro feedback es una variable **uint** que se puede proporcionar a la función [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) intrínseca de consulta de acceso a la memoria. No debe modificar ni interpretar el valor del parámetro feedback. pero el compilador no proporciona ningún diagnóstico y análisis avanzados para detectar si modificó el valor.

Esta es la sintaxis de [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped):

**bool CheckAccessFullyMapped (en uint FeedbackVar);**

[**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) interpreta el valor de *FeedbackVar* y devuelve true si todos los datos a los que se tiene acceso se asignaron en el recurso; de lo contrario, **CheckAccessFullyMapped** devuelve false.

Si el parámetro clamp o feedback está presente, el compilador emite una variante de la instrucción básica. Por ejemplo, el ejemplo de un recurso de streaming genera la `sample_cl_s` instrucción.

Si no se especifican Clamp ni feedback, el compilador emite la instrucción básica, de modo que no hay ningún cambio en el comportamiento actual.

El valor de Clamp de 0,0 f indica que no se realiza ninguna abrazadera; por lo tanto, el compilador de controladores puede personalizar aún más la instrucción al hardware de destino. Si los comentarios son un registro nulo en una instrucción, los comentarios no se usan. por lo tanto, el compilador de controladores puede adaptar aún más la instrucción a la arquitectura de destino.

Si el compilador de HLSL infiere que la abrazadera es 0,0 f y los comentarios no se usan, el compilador emite la instrucción básica correspondiente (por ejemplo, `sample` en lugar de `sample_cl_s` ).

Si un acceso a recursos de streaming consta de varias instrucciones de código de bytes constituyentes, por ejemplo, para los recursos estructurados, el compilador agrega valores de comentarios individuales a través de la operación o para generar el valor final de los comentarios. Por lo tanto, verá un valor de comentarios único para este tipo de acceso complejo.

Esta es la tabla de Resumen de los métodos de HLSL que se cambian para admitir comentarios y/o abrazadera. Todas ellas funcionan en recursos en mosaico y sin streaming de todas las dimensiones. Los recursos que no son de streaming parecen estar totalmente asignados.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5-objects">Objetos HLSL</a> </th>
<th align="left">Métodos intrínsecos con la opción feedback (*)-también tiene la opción Clamp</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>RW Texture2D</p>
<p>RW Texture2DArray</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Recopilar</p>
<p>GatherRed</p>
<p>GatherGreen</p>
<p>GatherBlue</p>
<p>GatherAlpha</p>
<p>GatherCmp</p>
<p>GatherCmpRed</p>
<p>GatherCmpGreen</p>
<p>GatherCmpBlue</p>
<p>GatherCmpAlpha</p></td>
</tr>
<tr class="even">
<td align="left"><p>RW Texture1D</p>
<p>RW Texture1DArray</p>
<p>RW Texture2D</p>
<p>RW Texture2DArray</p>
<p>RW Texture3D</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>AdventureWorks</p>
<p>SampleBias*</p>
<p>SampleCmp*</p>
<p>SampleCmpLevelZero</p>
<p>SampleGrad*</p>
<p>SampleLevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>RW Texture1D</p>
<p>RW Texture1DArray</p>
<p>RW Texture2D</p>
<p>Texture2DMS</p>
<p>RW Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>RW Texture3D</p>
<p>RW Búfer</p>
<p>RW ByteAddressBuffer</p>
<p>RW StructuredBuffer</p></td>
<td align="left">Cargar</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Acceso de canalización a recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 