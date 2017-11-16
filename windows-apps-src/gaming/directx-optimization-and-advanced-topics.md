---
author: joannaleecy
title: "Optimización y temas avanzados para juegos DirectX"
description: "Optimización y temas avanzados para la programación de juegos DirectX."
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, Windows 10, uwp, UWP, game, juego, directx, DirectX, optimizar, muestreo múltiple, cadenas de intercambio"
ms.openlocfilehash: 9fb2c8c92cdf5a498f3e3db4d9250c9dcdfe204f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>Optimización y temas avanzados para juegos DirectX

En esta sección se proporciona información sobre cómo optimizar el rendimiento de juegos DirectX y otros temas avanzados.

En el tema sobre la programación asincrónica para juegos se describen los diversos puntos a tener en cuenta al usar la programación asincrónica para paralelizar parte de los componentes y al usar multithreading para maximizar el uso de una poderosa GPU.

En el tema sobre el control de escenarios cuando se quitan dispositivos en Direct3D 11 se usa un tutorial para explicar cómo los juegos desarrollados con Direct3D 11 pueden detectar y responder ante situaciones donde se restablece, quita o cambia el adaptador de gráficos.

En el tema sobre el muestreo múltiple en aplicaciones para UWP se proporciona información general de cómo usar el suavizado de contorno de muestras múltiples, una técnica de gráficos para reducir la apariencia de bordes afilados en los juegos para UWP compilados con Direct3D.

En el tema sobre la optimización de la entrada y el bucle de representación se proporciona una guía sobre cómo elegir la opción correcta de procesamiento de eventos de entrada para administrar la latencia de entrada y optimizar el bucle de representación.

En el tema sobre la reducción de la latencia con cadenas de intercambio DXGI 1.3 se explica cómo reducir la latencia efectiva de los fotogramas esperando a que la cadena de intercambio señale el momento adecuado para empezar a representar un nuevo fotograma.

En el tema sobre el escalado y superposiciones de cadenas de intercambio se explica cómo mejorar los tiempos de representación mediante el uso de cadenas de intercambio con escala para representar el contenido del juego en tiempo real en una resolución inferior a la capacidad nativa de la pantalla. También se explica cómo crear cadenas de intercambio de superposición para dispositivos con la funcionalidad de superposición de hardware; esta técnica puede usarse para solucionar el problema de una interfaz de usuario reducida verticalmente resultante del uso del escalado de cadenas de intercambio.

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
<td align="left"><p>[Programación asincrónica para juegos](asynchronous-programming-directx-and-cpp.md)</p></td>
<td align="left"><p>Comprende la programación asincrónica y los subprocesos con DirectX.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Controlar escenarios cuando se quitan dispositivos en Direct3D11](handling-device-lost-scenarios.md)</p></td>
<td align="left"><p>Recrea la cadena de la interfaz de dispositivo Direct3D y DXGI cuando se quita o reinicializa el adaptador de gráficos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Muestreo múltiple en aplicaciones para UWP](multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md)</p></td>
<td align="left"><p>Usa el muestreo múltiple en juegos para UWP compilados con Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Optimizar la entrada y el bucle de representación](optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md)</p></td>
<td align="left"><p>Reduce la latencia de entrada y optimiza el bucle de representación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Reducir la latencia con cadenas de intercambio DXGI1.3](reduce-latency-with-dxgi-1-3-swap-chains.md)</p></td>
<td align="left"><p>Usa DXGI 1.3 para reducir la latencia efectiva de los fotogramas.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Escalado y superposiciones de cadenas de intercambio](multisampling--scaling--and-overlay-swap-chains.md)</p></td>
<td align="left"><p>Crea cadenas de intercambio con escala para que las representaciones en los dispositivos móviles sean más rápidas y usa cadenas de intercambio de superposición para mejorar la calidad visual.</p></td>
</tr>
</tbody>
</table>