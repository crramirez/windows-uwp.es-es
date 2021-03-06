---
title: Migrar de OpenGL ES 2.0 a Direct3D 11
description: Incluye artículos, información general y tutoriales para portar una canalización de elementos gráficos de OpenGL ES 2.0 a Direct3D 11 y Windows Runtime.
ms.assetid: 1e1cf668-a15f-0c7b-8daf-3260d27c6d9c
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, games, juegos, opengl, direct3d 11, port, portar, graphics, gráficos
ms.localizationpriority: medium
ms.openlocfilehash: 9215c06c97fb8e45a445ccd3691f484a6b2ad513
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258454"
---
# <a name="port-from-opengl-es-20-to-direct3d-11"></a>Migrar de OpenGL ES 2.0 a Direct3D 11



Incluye artículos, información general y tutoriales para portar una canalización de elementos gráficos de OpenGL ES 2.0 a Direct3D 11 y Windows Runtime.

> **Tenga en cuenta**   un paso intermedio para migrar el proyecto de opengl es 2,0 es usar el ángulo para Microsoft Store. ANGLE te permite ejecutar contenido de OpenGL ES en Windows mediante la conversión de llamadas a la API de OpenGL ES en llamadas a la API de DirectX 11. Para obtener más información acerca de ANGLE, ve a [ANGLE para Wiki de Microsoft Store](https://github.com/microsoft/angle/wiki).

 

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
<td align="left"><p><a href="map-concepts-and-infrastructure.md">Asignar OpenGL ES 2,0 a Direct3D 11,1</a></p></td>
<td align="left"><p>Cuando comiences el proceso para portar la arquitectura de gráficos de OpenGL ES 2.0 a Direct3D por primera vez, familiarízate con las principales diferencias entre las API. Los temas de esta sección te ayudarán a planear tu estrategia de migración y los cambios de API que debes realizar cuando traslades el procesamiento de gráficos a Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md">Cómo: trasladar un representador simple de OpenGL ES 2,0 a Direct3D 11,1</a></p></td>
<td align="left"><p>Para este ejercicio de migración, empezaremos con algo básico: llevar un representador simple de un cubo giratorio con vértices sombreados de OpenGL ES 2.0 a Direct3D, de modo que coincida con la plantilla Aplicación DirectX 11 (Windows universal) de Visual Studio 2015.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="opengl-es-2-0-to-directx-11-1-reference.md">Referencia de OpenGL ES 2,0 a Direct3D 11,1</a></p></td>
<td align="left"><p>Usa estos temas de referencia para buscar muestras de código corto y asignación de API al realizar la migración de OpenGL ES 2.0 a Direct3D 11.</p></td>
</tr>
</tbody>
</table>

 

 

 




