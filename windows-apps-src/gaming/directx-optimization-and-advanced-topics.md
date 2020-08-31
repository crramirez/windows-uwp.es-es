---
title: Optimización y temas avanzados para juegos de DirectX
description: Consulte los artículos que proporcionan información acerca de cómo optimizar el rendimiento de juegos de DirectX y otros temas avanzados.
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juego, DirectX, optimizar, muestreo múltiple, cadenas de intercambio
ms.localizationpriority: medium
ms.openlocfilehash: f19081a2618ea683c7790db3cd7517078f19269c
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89053655"
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>Optimización y temas avanzados para juegos de DirectX

En esta sección se proporciona información sobre cómo optimizar el rendimiento de juegos de DirectX y otros temas avanzados.

En el tema programación asincrónica de juegos se describen los distintos puntos a tener en cuenta cuando se desea usar la programación asincrónica para paralelizar algunos de los componentes y usar multithreading para maximizar el uso de una GPU eficaz.

Controlar escenarios quitados de dispositivos en Direct3D 11: usa un tutorial para explicar cómo los juegos desarrollados con Direct3D 11 pueden detectar y responder a situaciones en las que el adaptador de gráficos se restablece, se quita o se cambia.

En el tema de muestreo múltiple en aplicaciones para UWP se proporciona información general sobre cómo usar el suavizado de contorno de muestras múltiples, una técnica de gráficos para reducir la apariencia de los bordes con alias en juegos UWP creados con Direct3D.

Optimizar el tema de entrada y de bucle de representación proporciona instrucciones sobre cómo elegir la opción de procesamiento de eventos de entrada correctos para administrar la latencia de entrada y optimizar el bucle de representación.

En el tema reducir la latencia con las cadenas de intercambio de DXGI 1,3 se explica cómo reducir la latencia de fotogramas efectiva esperando a que la cadena de intercambio señale el tiempo adecuado para empezar a representar un nuevo marco.

En el tema sobre el escalado y las superposiciones de cadenas de intercambio se explica cómo mejorar los tiempos de representación mediante el uso de cadenas de intercambio escalado para representar contenido de juegos en tiempo real en una resolución inferior a la que la visualización es compatible de forma nativa con. También se explica cómo crear cadenas de intercambio de superposición para dispositivos con la funcionalidad de superposición de hardware; Esta técnica se puede usar para solucionar el problema de una interfaz de usuario de reducción horizontal resultante del uso del escalado de la cadena de intercambio.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="asynchronous-programming-directx-and-cpp.md">Programación asincrónica para juegos</a></p></td>
<td align="left"><p>Comprenda la programación asincrónica y el subprocesamiento con DirectX.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="handling-device-lost-scenarios.md">Controlar escenarios quitados de dispositivos en Direct3D 11</a></p></td>
<td align="left"><p>Vuelva a crear la cadena de interfaz de dispositivo Direct3D y DXGI cuando se quite o reinicialice el adaptador de gráficos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md">Muestreo múltiple en aplicaciones para UWP</a></p></td>
<td align="left"><p>Use el muestreo múltiple en juegos de UWP creados con Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md">Optimizar el bucle de representación y de entrada</a></p></td>
<td align="left"><p>Reduzca la latencia de entrada y optimice el bucle de representación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="reduce-latency-with-dxgi-1-3-swap-chains.md">Reducir la latencia con cadenas de intercambio de DXGI 1.3</a></p></td>
<td align="left"><p>Use DXGI 1,3 para reducir la latencia de fotogramas efectiva.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multisampling--scaling--and-overlay-swap-chains.md">Intercambiar escalado y superposiciones de cadenas</a></p></td>
<td align="left"><p>Cree cadenas de intercambio escalado para una representación más rápida en dispositivos móviles y use cadenas de intercambio de superposición para aumentar la calidad visual.</p></td>
</tr>
</tbody>
</table>