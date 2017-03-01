---
title: Mapas de luz con texturas
description: "Un mapa de luz es una textura o un grupo de texturas que contiene información sobre la iluminación de una escena 3D."
ms.assetid: 5C7518D2-AC92-4A97-B7AF-4469D213D7BD
keywords:
- Mapas de luz con texturas
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 1154be19719cc3101c114c0bd700fb735824bcda
ms.lasthandoff: 02/07/2017

---

# <a name="light-mapping-with-textures"></a>Mapas de luz con texturas


Un mapa de luz es una textura o un grupo de texturas que contiene información sobre la iluminación de una escena 3D. Los mapas de luz asignan áreas de luz y sombra a primitivos. La combinación multipase y de varias texturas permite que la aplicación represente las escenas con una apariencia más realista que con las técnicas de sombreado.

Para que una aplicación represente una escena 3D de forma realista, debe tener en cuenta el efecto que tienen las fuentes de luz en la apariencia de la escena. Aunque las técnicas como el sombreado plano y el sombreado Gouraud son valiosas en este sentido, pueden ser insuficientes para cubrir tus necesidades. Direct3D admite la combinación multipase y de varias texturas. Estas funcionalidades permiten que la aplicación represente las escenas con una apariencia más realista que las escenas representadas solo con técnicas de sombreado. Al aplicar uno o varios mapas de luz, tu aplicación puede asignar áreas de luz y sombra a sus primitivos.

Un mapa de luz es una textura o un grupo de texturas que contiene información sobre la iluminación de una escena 3D. Puedes almacenar la información de iluminación en los valores alfa del mapa de luz, en los valores de colores o en ambos.

Si implementas los mapas de luz con la combinación de texturas multipase, la aplicación debería representar el mapa de luz en sus primitivos en el primer paso. Debería usar un segundo paso para representar la textura base. Los mapas de luz especular son la excepción. En ese caso, representa primero la textura base y, a continuación, agrega el mapa de luz.

La combinación de varias texturas permite que la aplicación represente el mapa de luz y la textura base en un solo paso. Si el hardware del usuario proporciona la combinación de varias texturas, la aplicación debería aprovecharse de ello al realizar los mapas de luz. Esto mejora significativamente el rendimiento de la aplicación.

Con los mapas de luz, una aplicación de Direct3D puede lograr una variedad de efectos de iluminación al representar primitivos. Puede asignar no solo luces de colores y monocromáticas en una escena, sino que también puede agregar detalles como iluminación especular y difusa.

La información sobre cómo usar la combinación de texturas de Direct3D para realizar mapas de luz se presenta en los siguientes temas.

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
<td align="left"><p>[Mapas de luz monocromática](monochrome-light-maps.md)</p></td>
<td align="left"><p>Los mapas de luz monocromática permiten que los adaptadores antiguos realicen la combinación de texturas multipase, cuando una placa aceleradora 3D anterior no admite la combinación de texturas con el valor alfa del píxel de destino.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Mapas de luz de colores](color-light-maps.md)</p></td>
<td align="left"><p>Un mapa de luz de color usa los datos RGB del mapa de luz para su información de iluminación. Una aplicación normalmente representa escenas 3D de forma más realista si usa mapas de luces de colores.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Mapas de luces especulares](specular-light-maps.md)</p></td>
<td align="left"><p>Cuando una fuente de luz los ilumina, los objetos brillantes que usan materiales muy reflectantes reciben resaltados especulares. A veces, puedes obtener resaltados más precisos si aplicas mapas de luces especulares a primitivos, en lugar de usar los resaltados especulares que produce el módulo de iluminación.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Mapas de luces difusas](diffuse-light-maps.md)</p></td>
<td align="left"><p>Las superficies mates tienen reflexión de la luz difusa. El brillo de la luz difusa depende de la distancia desde la fuente de luz y el ángulo entre la superficie normal y el vector de dirección de la fuente de luz. Los mapas de luces de textura pueden simular una iluminación difusa compleja.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)

 

 





