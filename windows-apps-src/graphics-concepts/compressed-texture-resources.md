---
title: Recursos de texturas comprimidas
description: Los mapas de texturas son imágenes digitalizadas que se dibujan en formas tridimensionales para agregar detalles visuales.
ms.assetid: 2DD5FF94-A029-4694-B103-26946C8DFBC1
keywords:
- Recursos de texturas comprimidas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9318479fffc415e94407166bd1be20a93691a179
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646220"
---
# <a name="compressed-texture-resources"></a>Recursos de texturas comprimidas


Los mapas de texturas son imágenes digitalizadas que se dibujan en formas tridimensionales para agregar detalles visuales. Se dibujan en estas formas durante la rasterización y el proceso puede consumir grandes cantidades de bus y memoria del sistema. Para reducir la cantidad de memoria que consumen las texturas, Direct3D admite la compresión de superficies de texturas. Algunos dispositivos Direct3D admiten las superficies de texturas comprimidas de forma nativa. En estos dispositivos, cuando se ha creado una superficie comprimida y se le han cargado los datos, la superficie puede usarse en Direct3D como cualquier otra superficie de texturas. Direct3D controla la descompresión cuando la textura está asignada a un objeto 3D.

## <a name="span-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanspan-idstorage-efficiency-and-texture-compressionspanstorage-efficiency-and-texture-compression"></a><span id="Storage-Efficiency-and-Texture-Compression"></span><span id="storage-efficiency-and-texture-compression"></span><span id="STORAGE-EFFICIENCY-AND-TEXTURE-COMPRESSION"></span>Compresión de la eficacia y la textura de almacenamiento


Todos los formatos de compresión de texturas son potencias de dos. Si bien esto no significa que una textura sea necesariamente cuadrada, sí significa que tanto x como y son potencias de dos. Por ejemplo, si una textura originalmente mide 512 por 128 bytes, el siguiente mipmapping sería de 256 por 64 y así sucesivamente, donde cada nivel se reduce en una potencia de dos. En los niveles inferiores, donde la textura se filtra hasta 16 por 2 y 8 por 1, habrá bits desperdiciados porque el bloque de compresión es siempre un bloque de elementos de textura de 4 por 4. Se rellenan las partes sin usar del bloque.

Si bien hay bits desperdiciados en los niveles inferiores, la ganancia general sigue siendo significativa. En teoría, el peor de los casos es una textura de 2K por 1 (potencia de 2⁰). Aquí, solo se codifica una única fila de píxeles por bloque, mientras que el resto del bloque se deja sin usar.

## <a name="span-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanspan-idmixing-formats-within-a-single-texturespanmixing-formats-within-a-single-texture"></a><span id="Mixing-Formats-Within-a-Single-Texture"></span><span id="mixing-formats-within-a-single-texture"></span><span id="MIXING-FORMATS-WITHIN-A-SINGLE-TEXTURE"></span>Mezclar formatos dentro de una textura única


Es importante tener en cuenta que cualquier textura única debe especificar que sus datos se almacenen como 64 o 128 bits por grupo de 16 elementos de textura. Si para la textura se usan bloques de 64 bits —es decir, el formato de compresión de bloques BC1—, es posible combinar los formatos opaco y de alfa de 1 bit por cada bloque dentro de la misma textura. En otras palabras, la comparación de la magnitud de entero sin signo del color\_0 y el color\_1 se realiza de forma única para cada bloque de 16 elementos de textura.

Una vez que se usan los bloques de 128 bits, el canal alfa debe especificarse de modo explícito (formato BC2) o interpolado (formato BC3) para la textura entera. Al igual que con el color, cuando se selecciona el modo interpolado (formato BC3), pueden usarse ocho alfas interpolados o seis alfas interpolados en cada bloque. Vuelva a la comparación de la magnitud de alfa\_0 y alfa\_1 se realiza de forma exclusiva en forma de bloque a bloque.

Direct3D proporciona servicios para comprimir superficies que se usan para modelos de texturas 3D. En esta sección se proporciona información sobre cómo crear y manipular los datos en una superficie de texturas comprimidas.

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
<td align="left"><p><a href="opaque-and-1-bit-alpha-textures.md">Texturas de alfa opacas y de 1 bit</a></p></td>
<td align="left"><p>El formato de textura BC1 es para texturas opacas o que tienen un único color transparente.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures-with-alpha-channels.md">Texturas con canales alfa</a></p></td>
<td align="left"><p>Hay dos formas de codificar mapas de texturas que exhiben una transparencia más compleja. En cada caso, un bloque que describe la transparencia precede el bloque de 64 bits ya descrito. La transparencia se representa como un mapa de bits de 4×4 con 4 bits por píxel (codificación explícita), o con menos bits y una interpolación lineal que es similar a la que se usa para la codificación del color.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="block-compression.md">Compresión de bloques</a></p></td>
<td align="left"><p>La compresión de bloques es una técnica de compresión de texturas con pérdida de información para reducir la superficie de memoria y el tamaño de la textura, lo que da un aumento del rendimiento. Una textura comprimida en bloques puede ser menor que una textura con 32 bits por color.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="compressed-texture-formats.md">Formatos de texturas comprimidas</a></p></td>
<td align="left"><p>Esta sección contiene información sobre la organización interna de formatos de texturas comprimidos. No necesitas estos detalles para usar texturas comprimidas, porque puedes usar las funciones de Direct3D para la conversión desde y hacia los formatos comprimidos. Sin embargo, esta información resulta útil si quieres trabajar directamente sobre los datos de superficies comprimidos.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)

 

 




