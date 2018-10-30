---
title: Exposición de recursos de streaming de HLSL
description: Se necesita una sintaxis específica del lenguaje High Level Shader Language (HLSL) de Microsoft para admitir los recursos de streaming en Shader Model 5.
ms.assetid: 00A40D82-0565-43DC-82AB-0675B7E772E3
keywords:
- Exposición de recursos de streaming de HLSL
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a8523f4895c541ffb3b92ee00d5b62c57343ae00
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5759872"
---
# <a name="hlsl-streaming-resources-exposure"></a>Exposición de recursos de streaming de HLSL


Se necesita una sintaxis específica del lenguaje High Level Shader Language (HLSL) de Microsoft para admitir los recursos de streaming en [Shader Model 5](https://msdn.microsoft.com/library/windows/desktop/ff471356).

La sintaxis HLSL para Shader Model 5 solo se admite en dispositivos con compatibilidad con recursos de streaming. Cada método HLSL pertinente para los recursos de streaming de la siguiente tabla acepta un parámetro opcional adicional (comentarios) o dos (restricción y comentarios en este orden). Por ejemplo, un método **Sample** es:

**Sample(sampler, location \[, offset \[, clamp \[, feedback\] \] \])**

Un ejemplo de método **Sample** es [**Texture2D.Sample(S,float,int,float,uint)**](https://msdn.microsoft.com/library/windows/desktop/dn393787).

Los parámetros de desplazamiento, restricción y comentarios son opcionales. Debes especificar todos los parámetros opcionales hasta el que necesites, que sea coherente con las reglas de C++ de los argumentos de la función predeterminada. Por ejemplo, si es necesario el estado de comentarios, los parámetros de desplazamiento y restricción deben proporcionarse explícitamente en **Sample**, aunque no sean necesarios lógicamente.

El parámetro de restricción es un valor de tipo float escalar. El valor literal de restricción =0.0f indica que no se realiza la operación de restricción.

El parámetro de comentarios es una variable **uint** que puedes proporcionar a la función intrínseca de consulta de acceso a la memoria [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083). No debes modificar ni interpretar el valor del parámetro; sin embargo, el compilador no proporciona ningún análisis avanzado ni diagnóstico para detectar si se ha modificado el valor.

Esta es la sintaxis de [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083):

**bool CheckAccessFullyMapped(in uint FeedbackVar);**

[**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) interpreta el valor de *FeedbackVar* y devuelve true si se asignaron todos los datos accedidos en el recurso; de lo contrario, **CheckAccessFullyMapped** devuelve false.

Si hay el parámetro de restricción o comentarios, el compilador emite una variante de la instrucción básica. Por ejemplo, la muestra de un recurso de streaming genera la instrucción `sample_cl_s`.

Si no se especifica ni la restricción ni los comentarios, el compilador emite la instrucción básica, de tal forma que no hay ningún cambio en el comportamiento actual.

El valor de restricción de 0.0f indica que no se realiza ninguna restricción; por lo tanto, el compilador del controlador puede personalizar aún más la instrucción en el hardware de destino. Si los comentarios son un registro NULL en una instrucción, no se usan; por lo tanto, el compilador del controlador puede personalizar aún más la instrucción en la arquitectura de destino.

Si el compilador HLSL deduce que la restricción es 0.0f y los comentarios no se usan, el compilador emite la instrucción básica correspondiente (por ejemplo, `sample` en lugar de `sample_cl_s`).

Si un acceso a los recursos de streaming consta de varias instrucciones de código de bytes constituyentes, por ejemplo, para recursos estructurados, el compilador agrega valores de comentarios individuales a través de la operación OR para generar el valor de comentarios final. Por lo tanto, verás un valor de comentarios solo para dicho acceso complejo.

En esta tabla se resumen los métodos HLSL que se modifican para admitir los comentarios o la restricción. Todos funcionan en recursos que no son de streaming y en iconos de todas las dimensiones. Los recursos que no son de streaming siempre se muestran como asignados por completo.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left"><a href="https://msdn.microsoft.com/library/windows/desktop/ff471359">Objetos HLSL</a> </th>
<th align="left">Métodos intrínsecos con la opción de comentarios (*); también hay la opción de restricción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Gather</p>
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
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>[RW]Texture2DArray</p>
<p>[RW]Texture3D</p>
<p>TextureCUBE</p>
<p>TextureCUBEArray</p></td>
<td align="left"><p>Sample*</p>
<p>SampleBias*</p>
<p>SampleCmp*</p>
<p>SampleCmpLevelZero</p>
<p>SampleGrad*</p>
<p>SampleLevel</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[RW]Texture1D</p>
<p>[RW]Texture1DArray</p>
<p>[RW]Texture2D</p>
<p>Texture2DMS</p>
<p>[RW]Texture2DArray</p>
<p>Texture2DArrayMS</p>
<p>[RW]Texture3D</p>
<p>[RW]Buffer</p>
<p>[RW]ByteAddressBuffer</p>
<p>[RW]StructuredBuffer</p></td>
<td align="left">Load</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Acceso de canalización a recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 




