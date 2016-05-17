---
author: mtoepke
title: Tutorial - implementar volúmenes de sombra con búferes de profundidad en Direct3D 11
description: En este tutorial se muestra cómo representar volúmenes de sombra mediante mapas de profundidad en Direct3D 11 en dispositivos de todos los niveles de característica de Direct3D.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
---

# Tutorial - implementar volúmenes de sombra con búferes de profundidad en Direct3D 11


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este tutorial se muestra cómo representar volúmenes de sombra mediante mapas de profundidad en Direct3D 11 en dispositivos de todos los niveles de característica de Direct3D.
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
<td align="left"><p>[Crear recursos de dispositivo para búferes de profundidad](create-depth-buffer-resource--view--and-sampler-state.md)</p></td>
<td align="left"><p>Aprende cómo crear los recursos de dispositivo Direct3D necesarios para admitir la realización de pruebas de profundidad para volúmenes de sombra.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Representar el mapa de sombras en el búfer de profundidad](render-the-shadow-map-to-the-depth-buffer.md)</p></td>
<td align="left"><p>Representa desde el punto de vista de la luz para crear un mapa de profundidad de dos dimensiones representando el volumen de sombra.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Representar la escena con prueba de profundidad](render-the-scene-with-depth-testing.md)</p></td>
<td align="left"><p>Crea un efecto de sombra agregando pruebas de profundidad al sombreador de vértices (o geometría) y al sombreador de píxeles.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Compatibilidad con mapas de sombras en una variedad de hardware](target-a-range-of-hardware.md)</p></td>
<td align="left"><p>Representa sombras de alta fidelidad en dispositivos más rápidos, y sombras más rápidas en dispositivos menos eficaces.</p></td>
</tr>
</tbody>
</table>

 

## Migración de la aplicación de asignación de sombras al escritorio de Direct3D 9


Windows 8 agregó funciones de comparación de profundidad a los niveles de característica 9\_1 y 9\_3. Ahora puedes migrar el código de representación con volúmenes de sombra a DirectX 11 y el representador de Direct3D 11 ofrecerá compatibilidad con nivel inferior para dispositivos que tengan el nivel de característica 9. En este tutorial se muestra de qué manera cualquier aplicación o juego de Direct3D 11 puede implementar volúmenes de sombra mediante la prueba de profundidad. El código abarca los siguientes procesos:

1.  Crear recursos de dispositivo Direct3D para asignación de sombras.
2.  Agregar un pase de representación para crear el mapa de profundidad.
3.  Agregar la prueba de profundidad al pase de representación principal.
4.  Implementar el código de sombreador necesario.
5.  Opciones para una representación rápida en hardware de nivel inferior.

Cuando termines de leer este tutorial, sabrás cómo implementar una técnica básica de volúmenes de sombra en Direct3D 11 que sea compatible con el nivel de característica 9\_1 y los superiores.

## Requisitos previos


Debes [preparar el entorno de desarrollo para el desarrollo de juegos de DirectX para la Plataforma universal de Windows (UWP)](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Todavía no necesitas una plantilla, pero sí necesitarás Microsoft Visual Studio 2015 para compilar el ejemplo de código de este tutorial.

## Temas relacionados


**Direct3D**

* [Escribir sombreadores HLSL en Direct3D 9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [Crear un nuevo proyecto de DirectX 11 para UWP](user-interface.md)

**Artículos técnicos sobre la asignación de sombras**

* [Técnicas habituales para mejorar los mapas de profundidad de sombras](https://msdn.microsoft.com/library/windows/desktop/ee416324)
* [Mapas de instantáneas en cascada](https://msdn.microsoft.com/library/windows/desktop/ee416307)

 

 






<!--HONumber=May16_HO2-->


