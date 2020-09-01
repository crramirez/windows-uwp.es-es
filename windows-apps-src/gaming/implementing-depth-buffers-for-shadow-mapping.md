---
title: 'Tutorial: búferes de profundidad de volumen sombreado de Direct3D 11'
description: En este tutorial se muestra cómo representar volúmenes de sombra mediante mapas de profundidad en Direct3D 11 en dispositivos de todos los niveles de característica de Direct3D.
ms.assetid: d15e6501-1a1d-d99c-d1d8-ad79b849db90
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX, volúmenes de instantáneas, búferes de profundidad, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: e2b54f179d61a9479bdb921ef0a68b4c1772c20c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159389"
---
# <a name="walkthrough-implement-shadow-volumes-using-depth-buffers-in-direct3d-11"></a>Tutorial - implementar volúmenes de sombra con búferes de profundidad en Direct3D 11



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
<td align="left"><p><a href="create-depth-buffer-resource--view--and-sampler-state.md">Crear recursos de dispositivo para búferes de profundidad</a></p></td>
<td align="left"><p>Aprende cómo crear los recursos de dispositivo Direct3D necesarios para admitir la realización de pruebas de profundidad para volúmenes de sombra.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="render-the-shadow-map-to-the-depth-buffer.md">Representar el mapa de sombras en el búfer de profundidad</a></p></td>
<td align="left"><p>Representa desde el punto de vista de la luz para crear un mapa de profundidad de dos dimensiones representando el volumen de sombra.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="render-the-scene-with-depth-testing.md">Representar la escena con prueba de profundidad</a></p></td>
<td align="left"><p>Crea un efecto de sombra agregando pruebas de profundidad al sombreador de vértices (o geometría) y al sombreador de píxeles.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="target-a-range-of-hardware.md">Compatibilidad con mapas de sombras en una variedad de hardware</a></p></td>
<td align="left"><p>Representa sombras de alta fidelidad en dispositivos más rápidos, y sombras más rápidas en dispositivos menos eficaces.</p></td>
</tr>
</tbody>
</table>

 

## <a name="shadow-mapping-application-to-direct3d-9-desktop-porting"></a>Migración de la aplicación de asignación de sombras al escritorio de Direct3D 9


Funcionalidad de comparación de profundidad de agrega d de Windows 8 para el nivel de características 9 \_ 1 y 9 \_ 3. Ahora puedes migrar el código de representación con volúmenes de sombra a DirectX 11 y el representador de Direct3D 11 ofrecerá compatibilidad con nivel inferior para dispositivos que tengan el nivel de característica 9. En este tutorial se muestra de qué manera cualquier aplicación o juego de Direct3D 11 puede implementar volúmenes de sombra mediante la prueba de profundidad. El código abarca los siguientes procesos:

1.  Crear recursos de dispositivo Direct3D para asignación de sombras.
2.  Agregar un pase de representación para crear el mapa de profundidad.
3.  Agregar la prueba de profundidad al pase de representación principal.
4.  Implementar el código de sombreador necesario.
5.  Opciones para una representación rápida en hardware de nivel inferior.

Después de completar este tutorial, debe estar familiarizado con cómo implementar una técnica básica de volumen de sombra compatible en Direct3D 11 que sea compatible con el nivel de características 9 \_ 1 y versiones posteriores.

## <a name="prerequisites"></a>Requisitos previos


Debes [preparar el entorno de desarrollo para el desarrollo de juegos de DirectX para la Plataforma universal de Windows (UWP)](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Todavía no necesitas una plantilla, pero sí necesitarás Microsoft Visual Studio 2015 para compilar el ejemplo de código de este tutorial.

## <a name="related-topics"></a>Temas relacionados


**Direct3D**

* [Escribir sombreadores HLSL en Direct3D 9](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-writing-shaders-9)
* [Crear un nuevo proyecto de DirectX 11 para UWP](user-interface.md)

**Artículos técnicos sobre la asignación de sombras**

* [Técnicas habituales para mejorar los mapas de profundidad de sombras](/windows/desktop/DxTechArts/common-techniques-to-improve-shadow-depth-maps)
* [Mapas de instantáneas en cascada](/windows/desktop/DxTechArts/cascaded-shadow-maps)

 

 