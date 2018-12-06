---
title: Canalización del proceso
description: La canalización del proceso de Direct3D está diseñada para controlar los cálculos que se pueden llevar a cabo principalmente en paralelo con la canalización de gráficos.
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 911546f1c2973a79aea4b597a47352149a4e4210
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8753373"
---
# <a name="compute-pipeline"></a>Canalización del proceso


\[Parte de la información hace referencia a la versión preliminar del producto, que puede sufrir modificaciones importantes antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, respecto a la información que se ofrece aquí.\]


La canalización del proceso de Direct3D está diseñada para controlar los cálculos que se pueden llevar a cabo principalmente en paralelo con la canalización de gráficos. La canalización del proceso solo incluye unos pocos pasos, donde los datos fluyen de la entrada a la salida a través de la fase del sombreador de cálculo programable.

| | |
|-|-|
|Propósito|Al igual que otros sombreadores programables, la [fase del sombreador de cálculo (CS)](compute-shader-stage--cs-.md) está diseñada e implementada con HLSL. Un sombreador de cálculo proporciona cálculos de uso general de alta velocidad y aprovecha las ventajas de la gran cantidad de procesadores en paralelo de la unidad de procesamiento gráfico (GPU). El sombreador de cálculo ofrece características de uso compartido de memoria y de sincronización de subprocesos para permitir métodos de programación en paralelos más eficaces.|
|Entrada|A diferencia de otros sombreadores programables, la definición de entrada es abstracta. Por naturaleza, la entrada puede ser uni, bi o tridimensional, lo que determina el número de invocaciones que debe ejecutar el sombreador de cálculo. Es posible definir los datos compartidos que leerá un conjunto de invocaciones.|
|Salida|Los datos de salida del sombreador de cálculo, que pueden variar ampliamente, se pueden sincronizar con la canalización de representación de gráficos cuando se necesitan los datos calculados.|
| | |




<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">Like other programmable shaders, <a href="#compute-shader-stage--cs-.md">Compute Shader (CS) stage</a> is designed and implemented with HLSL. A compute shader provides high-speed general purpose computing and takes advantage of the large numbers of parallel processors on the graphics processing unit (GPU). The compute shader provides memory sharing and thread synchronization features to allow more effective parallel programming methods.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike other programmable shaders, the definition of input is abstract. The input can be one, two or three-dimensional in nature, determining the number of invocations of the compute shader to execute. It is possible to define shared data for one set of invocations to read.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Output data from the compute shader, which can be highly varied, can be synchronized with the graphics rendering pipeline when the computed data is required.</td>
</tr>
</tbody>
</table>
-->

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Guía de aprendizaje de gráficos de Direct3D](index.md)

 

 
