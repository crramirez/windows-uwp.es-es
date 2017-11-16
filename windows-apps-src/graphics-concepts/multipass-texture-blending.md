---
title: "Combinación de texturas multipase"
description: "Las aplicaciones de Direct3D pueden lograr numerosos efectos especiales al aplicar varias texturas a un primitivo a lo largo de varios pasos de representación."
ms.assetid: FB4D6E3F-4EF5-4D20-BF7E-1008E790E30C
keywords: "Combinación de texturas multipase"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 1ea0f10e4cec774a0b7d85bd813b8c4f720d0048
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="multipass-texture-blending"></a>Combinación de texturas multipase


Las aplicaciones de Direct3D pueden lograr numerosos efectos especiales al aplicar varias texturas a un primitivo a lo largo de varios pasos de representación. El término habitual para esto es *combinación de texturas multipase*. Un uso típico de una combinación de texturas multipase es emular los efectos de los modelos complejos de iluminación y sombreado aplicando varios colores desde varias texturas distintas. Este tipo de aplicaciones se conocen como *mapas de luz*. Consulta [Mapas de luz con texturas](light-mapping-with-textures.md).

**Nota** algunos dispositivos pueden aplicar varias texturas a primitivos en un solo paso. Consulta [Combinación de texturas](texture-blending.md).

 

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

 

 



