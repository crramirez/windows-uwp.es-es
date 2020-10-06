---
title: Canalización del proceso
description: La canalización del proceso de Direct3D está diseñada para controlar los cálculos que se pueden llevar a cabo principalmente en paralelo con la canalización de gráficos.
ms.assetid: 355B66C6-C0DF-47BA-A9C9-7AFA50B5B614
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36d1bd524d6e71b0a1aa9477d7a2b7a5f27544aa
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750051"
---
# <a name="compute-pipeline"></a>Canalización del proceso


\[Algunos datos se relacionan con productos de versiones preliminares que pueden modificarse sustancialmente antes de su lanzamiento comercial. Microsoft no proporciona ninguna garantía, expresa o implícita, con respecto a la información proporcionada aquí.\]


La canalización del proceso de Direct3D está diseñada para controlar los cálculos que se pueden llevar a cabo principalmente en paralelo con la canalización de gráficos. Hay solo unos pocos pasos en la canalización de proceso, con datos que fluyen de la entrada a la salida a través de la fase de sombreador de cálculo programable.

## <a name="purpose"></a>Propósito

Al igual que otros sombreadores programables, la [fase del sombreador de cálculo (CS)](compute-shader-stage--cs-.md) se diseña e implementa con HLSL. Un sombreador de cálculo proporciona una informática de uso general de alta velocidad y aprovecha el gran número de procesadores en paralelo de la unidad de procesamiento de gráficos (GPU). El sombreador de cálculo proporciona características de uso compartido de memoria y sincronización de subprocesos para permitir métodos de programación paralelas más eficaces.

## <a name="input"></a>Entrada

A diferencia de otros sombreadores programables, la definición de entrada es abstracta. La entrada puede tener una, dos o tres dimensiones por naturaleza, determinando el número de invocaciones del sombreador de cálculo que se va a ejecutar. Es posible definir los datos compartidos para un conjunto de invocaciones que se van a leer. |

## <a name="output"></a>Output

Los datos de salida del sombreador de cálculo, que pueden ser muy variados, se pueden sincronizar con la canalización de representación de gráficos cuando se requieren los datos calculados.

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

 

 
