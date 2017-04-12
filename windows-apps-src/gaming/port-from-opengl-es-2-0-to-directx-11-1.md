---
author: mtoepke
title: Migrar de OpenGL ES2.0 a Direct3D11
description: "Incluye artículos, información general y tutoriales para portar una canalización de gráficos de OpenGL ES 2.0 a Direct3D 11 y a Windows Runtime."
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, games, juegos, opengl, direct3d 11, port, portar, graphics, gráficos"
ms.openlocfilehash: 14ed2be84f295570dc95b3f1d28dfdd3720bada4
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="port-from-opengl-es-20-to-direct3d-11"></a>Portar de OpenGL ES2.0 a Direct3D11


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Incluye artículos, información general y tutoriales para portar una canalización de gráficos de OpenGL ES 2.0 a Direct3D 11 y Windows Runtime.

> **Nota**  Un paso intermedio para portar el proyecto de OpenGL ES 2.0 es usar ANGLE de la Tienda Windows. ANGLE te permite ejecutar contenido de OpenGL ES en Windows mediante la conversión de llamadas a la API de OpenGL ES a llamadas a la API de DirectX 11. Para obtener más información acerca de ANGLE, ve a la [wiki de ANGLE de la Tienda Windows](http://go.microsoft.com/fwlink/p/?linkid=618387).

 

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
<td align="left"><p>[Asignar de OpenGL ES2.0 a Direct3D11.1](map-concepts-and-infrastructure.md)</p></td>
<td align="left"><p>Cuando comiences el proceso para portar la arquitectura de gráficos de OpenGL ES 2.0 a Direct3D por primera vez, familiarízate con las principales diferencias entre las API. Los temas de esta sección te ayudarán a planear tu estrategia de migración y los cambios de API que debes realizar cuando traslades el procesamiento de gráficos a Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Portar un representador simple de OpenGL ES 2.0 a Direct3D 11.1](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)</p></td>
<td align="left"><p>Para este ejercicio de migración, empezaremos con algo básico: llevar un representador simple de un cubo giratorio con vértices sombreados de OpenGL ES 2.0 a Direct3D, de modo que coincida con la plantilla Aplicación DirectX 11 (Windows universal) de Visual Studio 2015.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Referencia de OpenGL ES 2.0 a Direct3D11.1](opengl-es-2-0-to-directx-11-1-reference.md)</p></td>
<td align="left"><p>Usa estos temas de referencia para buscar muestras de código corto y asignación de API al realizar la migración de OpenGL ES 2.0 a Direct3D 11.</p></td>
</tr>
</tbody>
</table>

 

> **Nota**  
Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

 

 




