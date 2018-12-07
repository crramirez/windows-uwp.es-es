---
title: Asignar OpenGL ES2.0 a Direct3D11
description: Cuando comiences el proceso de migrar tu arquitectura de gráficos desde OpenGL ES 2.0 a Direct3D por primera vez, familiarízate con las diferencias clave entre las API.
ms.assetid: 7f9b136c-aa22-04b3-d385-6e9e1f38b948
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, OpenGL, Direct3D, portar, games, porting
ms.localizationpriority: medium
ms.openlocfilehash: e09dcb1830e62d17983f564771b4808d132179a0
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8790301"
---
# <a name="map-opengl-es-20-to-direct3d-11"></a>Asignar OpenGL ES2.0 a Direct3D11



Cuando comiences el proceso de migrar tu arquitectura de gráficos desde OpenGL ES 2.0 a Direct3D por primera vez, familiarízate con las diferencias clave entre las API. Los temas de esta sección te ayudarán a planear tu estrategia de migración y los cambios de API que debes realizar cuando traslades el procesamiento de gráficos a Direct3D.
## 
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
<td align="left"><p><a href="compare-opengl-es-2-0-api-design-to-directx.md">Planear la migración de OpenGL ES 2.0 a Direct3D</a></p></td>
<td align="left"><p>Si vas a migrar un juego de plataformas iOs o Android, probablemente has hecho una gran inversión en OpenGL ES 2.0. Cuando te preparas para trasladar el código base de la canalización de gráficos a Direct3D 11 y Windows Runtime, debes considerar algunas cosas antes de empezar.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="moving-from-egl-to-dxgi.md">Comparar código EGL con DXGI y Direct3D</a></p></td>
<td align="left"><p>DirectX Graphics Interface (DXGI) y varias API de Direct3D cumplen el mismo rol que EGL. Este tema ayuda a comprender DXGI y Direct3D 11 desde la perspectiva de EGL.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="porting-uniforms-and-attributes.md">Comparar búferes, uniformes y atributos de vértice de OpenGL ES 2.0 con Direct3D</a></p></td>
<td align="left"><p>Durante el proceso de migración de OpenGL ES 2.0 a Direct3D 11, debes cambiar la sintaxis y el comportamiento de API para pasar datos entre la aplicación y los programas sombreadores.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="change-your-shader-loading-code.md">Comparar la canalización de sombreador de OpenGL ES 2.0 con Direct3D</a></p></td>
<td align="left"><p>En términos conceptuales, la canalización de sombreador de Direct3D 11 es muy similar a la de OpenGL ES 2.0. En términos de diseño de API, sin embargo, los principales componentes para crear y administrar las fases de sombreador forman parte de dos interfaces importantes: <a href="https://msdn.microsoft.com/library/windows/desktop/hh404575"><strong>ID3D11Device1</strong></a> y <a href="https://msdn.microsoft.com/library/windows/desktop/hh404598"><strong>ID3D11DeviceContext1</strong></a>. En este tema intentamos asignar patrones comunes de API para la canalización de sombreador de OpenGL ES 2.0 a sus equivalentes en Direct3D 11 en estas interfaces.</p></td>
</tr>
</tbody>
</table>

 

## <a name="notes-on-specific-opengl-es-20-providers"></a>Notas sobre proveedores específicos de OpenGL ES 2.0


Estos temas usan la especificación Khronos de OpenGL ES 2.0 con C válido para la plataforma. Tanto iOS como Android utilizan la misma especificación, y el código OpenGL ES 2.0 desarrollado para estas plataformas es muy similar a los fragmentos de código que analizaremos, aunque generalmente se las conoce como API orientadas a objetos. Además, debido a la dificultad y las diferencias de lenguaje de cada plataforma, puede haber unas pequeñas diferencias, especialmente en los tipos de parámetros de método o en la sintaxis de lenguaje general. iOS, por ejemplo, usa Objective-C. Android tiene la capacidad de usar C++. No obstante, algunos desarrolladores probablemente se hayan basado en una implementación de Java pura. A pesar de esto, estos temas todavía deben seguir siendo útiles dado que los conceptos, la estructura y el uso generales de las API de OpenGL ES no difieren.

 

 




