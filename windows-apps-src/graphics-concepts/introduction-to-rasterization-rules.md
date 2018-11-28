---
title: Introducción a las reglas de rasterización
description: A menudo, los puntos especificados para los vértices no coinciden con precisión con los píxeles en la pantalla. Cuando esto ocurre, Direct3D aplica reglas de rasterización de triángulo para decidir qué píxeles se aplican a un triángulo determinado.
ms.assetid: 4232CDBA-F669-4417-9378-F9013E83462C
keywords:
- Introducción a las reglas de rasterización
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d1907be029254d99be9e6158c93c179baea1fb0
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7853440"
---
# <a name="introduction-to-rasterization-rules"></a>Introducción a las reglas de rasterización


A menudo, los puntos especificados para los vértices no coinciden con precisión con los píxeles en la pantalla. Cuando esto ocurre, Direct3D aplica reglas de rasterización de triángulo para decidir qué píxeles se aplican a un triángulo determinado.

Esto es una introducción simplificada a las reglas de rasterización. Para obtener más información, consulta [Reglas de rasterización](rasterization-rules.md). Consulta también [Fase del rasterizador (RS)](rasterizer-stage--rs-.md).

## <a name="span-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspanspan-idtrianglerasterizationrulesspantriangle-rasterization-rules"></a><span id="Triangle_Rasterization_Rules"></span><span id="triangle_rasterization_rules"></span><span id="TRIANGLE_RASTERIZATION_RULES"></span>Reglas de rasterización de triángulo


Direct3D usa una convención de relleno de la parte superior izquierda para rellenar la geometría. Es la misma convención que se usa para los rectángulos en GDI y OpenGL. En Direct3D, el centro del píxel es el punto decisivo. Si el centro está dentro de un triángulo, el píxel es parte del triángulo. Los centros de píxeles están en coordenadas de enteros.

Esta descripción de las reglas de rasterización de triángulo que usa Direct3D no se aplica necesariamente a todo el hardware disponible. Es posible que en las pruebas que hagas descubras pequeñas variaciones en la implementación de estas reglas.

En la siguiente ilustración se muestra un rectángulo cuya esquina superior izquierda está en (0, 0) y cuya esquina inferior derecha está en (5, 5). Este rectángulo ocupa 25 píxeles, lo esperado. El ancho del rectángulo se define como derecha menos izquierda. El alto se define como abajo menos arriba.

![un cuadrado numerado dividido en seis filas y columnas](images/pixmap.png)

En la convención de relleno de la parte superior izquierda, *top* hace referencia a la ubicación vertical de intervalos horizontales y *left* hace referencia a la ubicación horizontal de píxeles en un intervalo. Un borde no puede ser un borde superior a menos que sea horizontal. En general, la mayoría de triángulos tienen solo los bordes izquierdo y derecho. En la siguiente ilustración se muestra un borde superior y un borde derecho.

![un cuadrado numerado que contiene dos triángulos](images/triedge.png)

La convención de relleno de la parte superior izquierda determina la acción que realiza Direct3D cuando un triángulo pasa a través del centro de un píxel. En la siguiente ilustración se muestran dos triángulos, uno en (0, 0), (5, 0) y (5, 5) y el otro en (0, 5), (0, 0) y (5, 5). En este caso, el primer triángulo obtiene 15 píxeles (en negro), mientras que el segundo obtiene solo 10 píxeles (en gris) porque el borde compartido es el borde izquierdo del primer triángulo.

![un cuadrado numerado en el que se muestran dos triángulos](images/twotris.png)

Si defines un rectángulo con la esquina superior izquierda en (0,5, 0,5) y la esquina inferior derecha en (2,5, 4,5), el punto central de este rectángulo está en (1,5, 2,5). Cuando el rasterizador Direct3D realiza este rectángulo con elementos de textura, el centro de cada píxel está de forma inequívoca dentro de cada uno de los cuatro triángulos y no es necesaria la convención de relleno de la parte superior izquierda. En la siguiente ilustración se muestra este caso. Los píxeles del rectángulo están etiquetados según el triángulo en el que Direct3D los incluye.

![un cuadrado numerado que contiene un rectángulo que se divide en cuatro triángulos](images/noambig.png)

Si se mueve el rectángulo de la ilustración anterior para que la esquina superior izquierda esté en (1,0, 1,0), la esquina inferior derecha en (3,0, 5,0) y el punto central en (2,0, 3,0), Direct3D aplica la convención de relleno de la parte superior izquierda. La mayoría de píxeles del rectángulo cubren el límite entre dos o más triángulos, como se muestra en la siguiente ilustración.

![un cuadrado numerado que contiene un rectángulo que se divide en cuatro triángulos](images/fillrule.png)

Para ambos rectángulos, los mismos píxeles se ven afectados, como se muestra en la siguiente ilustración.

![píxeles afectados por los dos cuadrados numerados anteriores](images/samepix.png)

## <a name="span-idpointandlinerulesspanspan-idpointandlinerulesspanspan-idpointandlinerulesspanpoint-and-line-rules"></a><span id="Point_and_Line_Rules"></span><span id="point_and_line_rules"></span><span id="POINT_AND_LINE_RULES"></span>Reglas de punto y línea


Los puntos se representan igual que los sprites de punto, ambos se representan como cuadriláteros alineados a la pantalla y, por consiguiente, cumplen las mismas reglas que la representación de polígonos.

Las reglas de representación de líneas sin suavizado de contorno son exactamente las mismas que para las [líneas GDI](https://msdn.microsoft.com/library/windows/desktop/dd145027).

## <a name="span-idpointspriterulesspanspan-idpointspriterulesspanspan-idpointspriterulesspanpoint-sprite-rules"></a><span id="Point_Sprite_Rules"></span><span id="point_sprite_rules"></span><span id="POINT_SPRITE_RULES"></span>Reglas de sprites de punto


Los sprites de punto y los primitivos de revisión se rasterizan como si los primitivos encajaran primero en triángulos y se rasterizaran los triángulos resultantes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Dispositivos](devices.md)

[Fase del rasterizador (RS)](rasterizer-stage--rs-.md)

[Reglas de rasterización](rasterization-rules.md)

 

 




