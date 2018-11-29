---
title: Combinación de texturas multipase
description: Las aplicaciones de Direct3D pueden lograr numerosos efectos especiales al aplicar varias texturas a un primitivo a lo largo de varios pasos de representación.
ms.assetid: FB4D6E3F-4EF5-4D20-BF7E-1008E790E30C
keywords:
- Combinación de texturas multipase
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d6b1e8958874ede50a18f2d2446c8f156361210e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "7986715"
---
# <a name="multipass-texture-blending"></a>Combinación de texturas multipase


Las aplicaciones de Direct3D pueden lograr numerosos efectos especiales mediante la aplicación de varias texturas a un primitivo a lo largo de varios pases de representación. El término habitual para esto es *combinación de texturas multipase*. Un uso habitual de la combinación de texturas multipase es emular los efectos de modelos complejos de iluminación y sombreado con la aplicación de varios colores de distintas texturas diferentes. Este tipo de aplicaciones se conocen como *mapas de luz*. Consulta [Mapas de luz con texturas](light-mapping-with-textures.md).

**Nota**  algunos dispositivos son capaces de aplicar varias texturas a primitivos en un solo paso. Consulta [Combinación de texturas](texture-blending.md).

 

Si el hardware del usuario no admite la combinación de varias texturas, la aplicación puede usar la combinación de texturas multipase para lograr los mismos efectos visuales. Sin embargo, la aplicación no puede admitir la posible velocidad de fotogramas cuando usa la combinación de texturas multipase.

Para realizar una combinación de texturas multipase en una aplicación de C o C++:

1.  Establece una textura en la fase de textura 0.
2.  Selecciona los argumentos y las operaciones de combinación de color y alfa que quieras. La configuración predeterminada es adecuada para una combinación de texturas multipase.
3.  Representa los objetos apropiados de la escena.
4.  Establece la siguiente textura en la fase de textura 0.
5.  Establece los estados de representación para ajustar los factores de combinación de origen y destino según necesites. El sistema combina las nuevas texturas con los píxeles existentes en la superficie de destino de la representación de acuerdo con estos parámetros.
6.  Repite los pasos 3, 4 y 5 con el número de texturas que necesites.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Combinación de texturas](texture-blending.md)

 

 




