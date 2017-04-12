---
title: Texturas
description: "Las texturas son una herramienta muy eficaz para dotar de realismo a las imágenes en 3D generadas por PC. Direct3D admite un amplio conjunto de características de texturas, lo que ofrece a los desarrolladores un acceso fácil a técnicas avanzadas de creación de texturas."
ms.assetid: B9E85C9E-B779-4852-9166-6FA2240B7046
keywords: Texturas
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: ef5c72f3c667c63cb48c469349ae26c364050c19
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="textures"></a>Texturas


Las texturas son una herramienta muy eficaz para dotar de realismo a las imágenes en 3D generadas por PC. Direct3D admite un amplio conjunto de características de texturas, lo que ofrece a los desarrolladores un acceso fácil a técnicas avanzadas de creación de texturas.

Para mejorar el rendimiento, piensa en la posibilidad de usar texturas dinámicas. Una textura dinámica se puede bloquear, escribir y desbloquear en cada fotograma.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>En esta sección


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
<td align="left"><p>[Introducción a las texturas](introduction-to-textures.md)</p></td>
<td align="left"><p>Un recurso de textura es una estructura de datos para almacenar elementos de textura, que son las unidades más pequeñas de una textura que pueden leerse o en las que se puede escribir. Cuando un sombreador lee la textura, las muestras de texturas pueden filtrarla.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Conceptos básicos sobre la creación de texturas](basic-texturing-concepts.md)</p></td>
<td align="left"><p>Las primeras imágenes en 3D generadas por PC, aunque en general eran avanzadas para su época, solían tener una apariencia plástica y brillante. Carecían de los tipos de marcas (como rayas, fisuras, huellas digitales y manchas) que dan a los objetos 3D una complejidad visual realista. Las texturas han hecho populares por mejorar el realismo de las imágenes en 3D generadas por el equipo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Modos de direccionamiento de las texturas](texture-addressing-modes.md)</p></td>
<td align="left"><p>La aplicación de Direct3D puede asignar coordenadas de textura a todos los vértices de cualquier primitivo. Por lo general, las coordenadas de textura u y v que se asignan a un vértice se encuentran en el intervalo de 0.0 a 1.0, ambos incluidos. Sin embargo, si se asignan coordenadas de textura fuera de ese intervalo, puedes crear determinados efectos especiales de texturas.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Filtrado de texturas](texture-filtering.md)</p></td>
<td align="left"><p>El filtrado de texturas produce un color para cada píxel en la imagen representada en 2D del primitivo cuando un primitivo se representa mediante la asignación de un primitivo en 3D a una pantalla 2D.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Recursos de texturas](texture-resources.md)</p></td>
<td align="left"><p>Las texturas son un tipo de recurso usado para la representación.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Ajuste de texturas](texture-wrapping.md)</p></td>
<td align="left"><p>El ajuste de texturas cambia la forma básica en la que Direct3D rasteriza los polígonos texturizados mediante las coordenadas de textura especificadas para cada vértice. Durante la rasterización de un polígono, el sistema interpola entre las coordenadas de textura en cada uno de los vértices del polígono para determinar los elementos de textura que deberían usarse para cada píxel de dicho polígono.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Combinación de texturas](texture-blending.md)</p></td>
<td align="left"><p>Direct3D puede combinar hasta ocho texturas en primitivos en un solo pase. El uso de la combinación de texturas múltiples puede aumentar profundamente la velocidad de fotogramas de una aplicación de Direct3D. Una aplicación emplea la combinación de texturas múltiples para aplicar texturas, sombras, iluminación especular, iluminación difusa y otros efectos especiales en un solo pase.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Mapas de luz con texturas](light-mapping-with-textures.md)</p></td>
<td align="left"><p>Un mapa de luz es una textura o un grupo de texturas que contiene información sobre la iluminación de una escena en 3D. Los mapas de luz asignan áreas de luz y sombra a primitivos. La combinación de texturas múltiples y multipase permite a la aplicación representar las escenas con una apariencia más realista que técnicas de sombreado.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Recursos de texturas comprimidas](compressed-texture-resources.md)</p></td>
<td align="left"><p>Los mapas de texturas son imágenes digitalizadas que se dibujan sobre formas tridimensionales para agregar detalles visuales. Se dibujan en estas formas durante la rasterización y el proceso puede consumir grandes cantidades de bus y memoria del sistema. Para reducir la cantidad de memoria que consumen las texturas, Direct3D admite la compresión de superficies de texturas. Algunos dispositivos Direct3D admiten las superficies de texturas comprimidas de forma nativa.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Guía de aprendizaje de gráficos de Direct3D](index.md)

 

 




