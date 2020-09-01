---
title: Introducción a las reglas de rasterización
description: A menudo, los puntos especificados para los vértices no coinciden exactamente con los píxeles de la pantalla. Cuando esto sucede, Direct3D aplica reglas de rasterización de triángulo para decidir qué píxeles se aplican a un triángulo determinado.
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- Introducción a las reglas de rasterización
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 38522be28280c0a08f6cb065e5dfb5c2f26642a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162799"
---
# <a name="introduction-to-rasterization-rules"></a>Introducción a las reglas de rasterización


A menudo, los puntos especificados para los vértices no coinciden exactamente con los píxeles de la pantalla. Cuando esto sucede, Direct3D aplica reglas de rasterización de triángulo para decidir qué píxeles se aplican a un triángulo determinado.

Esta es una introducción simplificada a las reglas de rasterización. Para obtener más información, consulte [reglas de rasterización](rasterization-rules.md). Consulte también [fase de rasterizador (RS)](rasterizer-stage--rs-.md).

## <a name="span-idtriangle_rasterization_rulesspanspan-idtriangle_rasterization_rulesspanspan-idtriangle_rasterization_rulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>Reglas de rasterización de triángulos


Direct3D usa una Convención de llenado superior izquierda para rellenar la geometría. Se trata de la misma Convención que se usa para los rectángulos en GDI y OpenGL. En Direct3D, el centro del píxel es el punto decisivo. Si el centro está dentro de un triángulo, el píxel es parte del triángulo. Los centros de píxeles están en coordenadas de enteros.

Esta descripción de las reglas de rasterización triangular que usa Direct3D no se aplica necesariamente a todo el hardware disponible. Las pruebas pueden revelar pequeñas variaciones en la implementación de estas reglas.

En la ilustración siguiente se muestra un rectángulo cuya esquina superior izquierda está en (0,0) y cuya esquina inferior derecha está en (5, 5). Este rectángulo rellena 25 píxeles, tal y como cabría esperar. El ancho del rectángulo se define como Right menos a la izquierda. El alto se define como inferior menos superior.

![cuadrado numerado dividido en seis filas y columnas](images/pixmap.png)

En la Convención de llenado superior izquierda, la *parte superior* se refiere a la ubicación vertical de los intervalos horizontales y a la *izquierda* hace referencia a la ubicación horizontal de los píxeles dentro de un intervalo. Un borde no puede ser un borde superior a menos que sea horizontal. En general, la mayoría de los triángulos solo tienen bordes izquierdo y derecho. En la ilustración siguiente se muestra un borde superior y un borde derecho.

![cuadrado numerado que contiene dos triángulos](images/triedge.png)

La Convención de llenado superior izquierda determina la acción realizada por Direct3D cuando un triángulo pasa a través del centro de un píxel. En la ilustración siguiente se muestran dos triángulos, uno en (0,0), (5, 0) y (5, 5), y el otro en (0, 5), (0, 0) y (5, 5). En este caso, el primer triángulo obtiene 15 píxeles (se muestra en negro), mientras que el segundo obtiene solo 10 píxeles (mostrados en gris) porque el borde compartido es el borde izquierdo del primer triángulo.

![cuadrado numerado que muestra dos triángulos](images/twotris.png)

Si define un rectángulo con la esquina superior izquierda en (0,5, 0,5) y la esquina inferior derecha en (2,5, 4,5), el punto central de este rectángulo está en (1,5, 2,5). Cuando el rasterizador de Direct3D tessellates este rectángulo, el centro de cada píxel es inequívoca dentro de cada uno de los cuatro triángulos, y no es necesaria la Convención de llenado superior izquierda. En la ilustración siguiente se muestra esto. Los píxeles del rectángulo se etiquetan según el triángulo en el que Direct3D los incluye.

![cuadrado numerado que contiene un rectángulo que se divide en cuatro triángulos.](images/noambig.png)

Si mueve el rectángulo de la ilustración anterior para que la esquina superior izquierda esté en (1,0, 1,0), la esquina inferior derecha en (3,0, 5,0) y su punto central en (2,0, 3,0), Direct3D aplica la Convención de llenado superior izquierda. La mayoría de los píxeles de este rectángulo ocupan el borde entre dos o más triángulos, como se muestra en la ilustración siguiente.

![cuadrado numerado que contiene un rectángulo que se divide en cuatro triángulos.](images/fillrule.png)

En ambos rectángulos, los mismos píxeles se ven afectados, tal como se muestra en la siguiente ilustración.

![píxeles afectados por los dos cuadrados numerados anteriores](images/samepix.png)

## <a name="span-idpoint_and_line_rulesspanspan-idpoint_and_line_rulesspanspan-idpoint_and_line_rulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>Reglas de punto y línea


Los puntos se representan igual que los sprites de punto, que se representan como quadrilaterals alineados por pantalla y, por tanto, se ajustan a las mismas reglas que la representación de polígonos.

Las reglas de representación de líneas sin alias son exactamente iguales que las de [las líneas de GDI](/windows/desktop/gdi/lines).

## <a name="span-idpoint_sprite_rulesspanspan-idpoint_sprite_rulesspanspan-idpoint_sprite_rulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>Reglas de Sprite de punto


Los sprites de punto y los primitivos de revisión se rasterizan como si los primitivos estuviesen en primer lugar en triángulos y los triángulos resultantes se rasterizan.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Dispositivos](devices.md)

[Fase del rasterizador (RS)](rasterizer-stage--rs-.md)

[Reglas de rasterización](rasterization-rules.md)

 

 