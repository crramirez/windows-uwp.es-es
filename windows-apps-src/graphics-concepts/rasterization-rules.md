---
title: Reglas de rasterización
description: Las reglas de rasterización definen cómo se asignan los datos de vector en datos de trama.
ms.assetid: B604725F-96A5-4DB6-B597-9EC57FBBC024
keywords:
- Reglas de rasterización
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 72be66c3bf7b0e58092ae3a7e1baf82c9e686f0c
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7150157"
---
# <a name="rasterization-rules"></a>Reglas de rasterización


las reglas de rasterización definen cómo se asignan los datos de vector en datos de trama. Los datos de trama se ajustan a las ubicaciones de enteros, que luego se seleccionan y recortan (para dibujar el número mínimo de píxeles), y los atributos por píxel se interpolan (a partir de los atributos por vértice) antes de que se pasen a un sombreador de píxeles.

Existen varios tipos de reglas, que dependen del tipo de primitivo que se está asignando y de si los datos utilizan el muestreo múltiple para reducir el suavizado de contorno. Las siguientes ilustraciones demuestran cómo se controlan los casos extremos.

## <a name="span-idtrianglespanspan-idtrianglespanspan-idtrianglespantriangle-rasterization-rules-without-multisampling"></a><span id="Triangle"></span><span id="triangle"></span><span id="TRIANGLE"></span>Reglas de rasterización de triángulo (sin muestreo múltiple)


Cualquier centro de píxel que se encuentre dentro de un triángulo se dibuja; se supone que un píxel está dentro si pasa la regla de la parte superior-izquierda. La regla de la parte superior-izquierda consiste en que un centro de píxel se define para estar dentro de un triángulo si se encuentra en el borde superior o en el borde izquierdo de un triángulo.

Donde:

-   Un borde superior es un borde exactamente horizontal que está por encima de los demás bordes.
-   Un borde izquierdo no es exactamente horizontal y está en el lado izquierdo del triángulo. Un triángulo puede tener uno o dos de bordes izquierdos.

La regla de la parte superior-izquierda garantiza que los triángulos adyacentes se dibujan una vez.

Esta ilustración muestra ejemplos de píxeles que se dibujan porque se encuentran dentro de un triángulo o siguen la regla de la parte superior-izquierda.

![Ejemplos de rasterización de triángulo de la parte superior-izquierda](images/d3d10-rasterrulestriangle.png)

La cobertura gris clara y oscura de los píxeles los muestra como grupos de los píxeles para indicar en qué triángulo se encuentran.

## <a name="span-idline1spanspan-idline1spanspan-idline1spanline-rasterization-rules-aliased-without-multisampling"></a><span id="Line_1"></span><span id="line_1"></span><span id="LINE_1"></span>Reglas de rasterización de línea (con alias y sin muestreo múltiple)


Las reglas de rasterización de línea usan un área de prueba en forma de rombo para determinar si una línea cubre un píxel. Para las líneas X principales (líneas con -1 &lt;= pendiente &lt;= + 1), el área de prueba en forma de rombo incluye (línea continua) el borde inferior izquierdo, el borde inferior derecho y la esquina inferior; el rombo excluye (línea discontinua) el borde superior izquierdo, el borde superior derecho, la esquina superior, la esquina izquierda y la esquina derecha. Una línea Y principal es cualquier línea que no es una línea X principal; el área en forma de rombo de la prueba es la misma que hemos descrito para la línea X principal, excepto en que la esquina derecha también se incluye.

Dada el área en forma de rombo, una línea incluye un píxel si la línea sale del área de prueba en forma de rombo al desplazarse por la línea de principio a fin. Una serie de líneas se comporta igual que si se hubiese dibujado como una secuencia de líneas.

La ilustración siguiente muestra algunos ejemplos.

![Ejemplos de rasterización de línea con alias](images/d3d10-rasterrulesline.png)

## <a name="span-idline2spanspan-idline2spanspan-idline2spanline-rasterization-rules-antialiased-without-multisampling"></a><span id="Line_2"></span><span id="line_2"></span><span id="LINE_2"></span>Reglas de rasterización de línea (con suavizado de contorno y sin muestreo múltiple)


Una línea con suavizado de contorno se rasteriza como si fuera un rectángulo (con ancho = 1). El rectángulo se interseca con un destino de representación y produce valores de cobertura por píxel, que se multiplican en los componentes alfa de salida del sombreador de píxeles. No se realiza ningún suavizado de contorno al dibujar líneas en un destino de representación multimuestra.

Se considera que no existe un único método "ideal" para realizar la representación de líneas con suavizado de contorno. Direct3D adopta como una directriz el método que se muestra en la siguiente ilustración. Este método se ha derivado empíricamente y presenta varias propiedades visuales que se consideran deseables.

El hardware no tiene que coincidir exactamente con este algoritmo; las pruebas en esta referencia tendrán tolerancias "razonables", que se regirán por algunos de los principios que se describen más adelante, lo que permite varias implementaciones de hardware y varios tamaños de kernel de filtro. Sin embargo, esta flexibilidad permitida en la implementación de hardware no se pueden comunicar a través de Direct3D a las aplicaciones, aparte de dibujar líneas y observar o medir su apariencia.

![Ejemplos de rasterización de línea con suavizado de contorno](images/d3d10-rasterruleslineaa.png)

Este algoritmo genera líneas relativamente suaves, con intensidad uniforme y con galones o bordes con las mínimas irregularidades. La creación de patrones de muaré para líneas cerradas se ha minimizado. Existe una buena cobertura para las uniones entre segmentos de línea que se colocan de extremo a extremo.

El kernel del filtro es una compensación razonable entre la cantidad de desenfoque del borde y los cambios en la intensidad que causan las correcciones gamma. El valor de la cobertura se multiplica en el sombreador de píxeles o0.a (srcAlpha) según la fórmula siguiente mediante la [fase de fusión de salida (OM)](output-merger-stage--om-.md):

srcColor \* srcAlpha + destColor \* (1-srcAlpha)

## <a name="span-idpointspanspan-idpointspanspan-idpointspanpoint-rasterization-rules-without-multisampling"></a><span id="Point"></span><span id="point"></span><span id="POINT"></span>Reglas de rasterización de punto (sin muestreo múltiple)


Un punto se interpreta como si estuviese formado por dos triángulos de un patrón Z que usan reglas de rasterización de triángulo. La coordenada identifica el centro de un cuadrado de ancho de un píxel. No hay ninguna selección de puntos.

La ilustración siguiente muestra algunos ejemplos.

![Ejemplos de rasterización de punto](images/d3d10-rasterrulespoint.png)

## <a name="span-idmultisamplespanspan-idmultisamplespanspan-idmultisamplespanmultisample-anti-aliasing-rasterization-rules"></a><span id="Multisample"></span><span id="multisample"></span><span id="MULTISAMPLE"></span>Reglas de rasterización de suavizado de contorno multimuestra


El suavizado de contorno multimuestra (MSAA) reduce los alias de geometría con pruebas de cobertura de píxeles y de galería de símbolos de profundidad en varias ubicaciones de submuestras. Para mejorar el rendimiento, los cálculos por píxel se realizan una vez por cada píxel incluido, para lo cual se comparten las salidas del sombreador en los subpíxeles incluidos. El suavizado de contorno multimuestra no reduce los alias de superficie. Las ubicaciones de muestra y las funciones de reconstrucción dependen de la implementación del hardware.

La ilustración siguiente muestra algunos ejemplos.

![Ejemplos de rasterización de suavizado de contorno multimuestra](images/d3d10-rasterrulesmsaa.png)

El número de ubicaciones de muestra depende del modo de muestra múltiple. Los atributos de vértice se interpolan en los centros de píxel, ya que es donde se invoca el sombreador de píxeles (este proceso se convierte en extrapolación si no se incluye el centro). Los atributos pueden marcarse en el sombreador de píxeles para el muestreo centroide, que hace que los píxeles no incluidos interpolen el atributo en la intersección del área del píxel y el primitivo.

Un sombreador de píxeles se ejecuta para cada área de 2 x 2 píxeles para admitir cálculos derivados (que utilizan deltas x e y). Esto significa que las invocaciones del sombreador se producen más de lo que se muestra para rellenar la cantidad mínima de 2 x 2 (que es independiente del muestreo múltiple). El resultado del sombreador se escribe para cada muestra incluida que pasa la prueba de la galería de símbolos de profundidad por muestra.

Por lo general, las reglas de rasterización de primitivos no cambian con el suavizado de contorno multimuestra, excepto en estos casos:

-   Para un triángulo se realiza una prueba de cobertura para cada ubicación de muestra (no de un centro de píxel). Si se incluye más de una ubicación de muestra, un sombreador de píxeles se ejecuta una vez con atributos interpolados en el centro de píxel. El resultado se almacena (replica) para cada ubicación de muestra incluida en el píxel que pasa la prueba de profundidad o galería de símbolos.

    Una línea se trata como un rectángulo formado por dos triángulos, con un ancho de línea de 1,4.

-   Para un punto se realiza una prueba de cobertura para cada ubicación de muestra (no de un centro de píxel).

Los formatos de muestreo múltiple se pueden utilizar en destinos de representación que se pueden leer en sombreadores mediante [load](https://msdn.microsoft.com/library/windows/desktop/bb509694), dado que no es necesario resolver muestras individuales a las que accede el sombreador. Los formatos de profundidad no se admiten para el recurso multimuestra, por lo que están restringidos a los destinos de representación solamente.

Los formatos sin tipos admiten el muestreo múltiple para permitir que una vista de recursos interprete datos de diferentes maneras. Por ejemplo, podría crear un recurso multimuestra con R8G8B8A8\_TYPELESS, representarlo a través de un recurso render-target-view con un formato R8G8B8A8\_UINT y resolver el contenido en otro recurso con un formato de datos R8G8B8A8\_UNORM.

### <a name="span-idhardwaresupportspanspan-idhardwaresupportspanspan-idhardwaresupportspanhardware-support"></a><span id="Hardware_Support"></span><span id="hardware_support"></span><span id="HARDWARE_SUPPORT"></span>Compatibilidad de hardware

La API genera informes de compatibilidad de hardware para el muestreo múltiple en los distintos niveles de calidad. Por ejemplo, un nivel de calidad 0 indica que el hardware no admite el muestreo múltiple (en un formato y un nivel de calidad determinados). Un nivel de calidad 3 indica que el hardware admite tres diseños de muestras y/o algoritmos de resolución diferentes. También puedes suponer lo siguiente:

-   Cualquier formato que admita el muestreo múltiple es compatible con el mismo número de niveles de calidad para todos los formatos de esa familia.
-   Todos los formatos que admiten el muestreo múltiple y que tienen los formatos \_UNORM, \_SRGB, \_SNORM o \_FLOAT también admiten la resolución.

### <a name="span-idcentroidsamplingspanspan-idcentroidsamplingspanspan-idcentroidsamplingspancentroid-sampling-of-attributes-when-multisample-antialiasing"></a><span id="Centroid_Sampling"></span><span id="centroid_sampling"></span><span id="CENTROID_SAMPLING"></span>Muestreo centroide de atributos durante el suavizado de contorno multimuestra

De manera predeterminada, los atributos de vértice se interpolan en un centro de píxel durante el suavizado de contorno multimuestra; si el centro de píxel no se incluye, los atributos se extrapolan a un centro de píxel. Si la entrada de un sombreador de píxeles que contiene la semántica centroide (suponiendo que el píxel no está completamente incluido), se muestreará en algún lugar dentro del área incluida del píxel, posiblemente en una de las ubicaciones de la muestra incluidas. Se aplica una máscara de muestra (especificada por el estado del rasterizador) antes de cálculo centroide. Por lo tanto, una muestra enmascarada no se utilizará como una ubicación centroide.

El rasterizador de referencia elige una ubicación de muestra para el muestreo centroide similar a la siguiente:

-   La máscara de muestra permite todas las muestras. Utiliza un centro de píxel si el píxel está incluido o si no se ha incluido ninguna de las muestras. De lo contrario, se elige la primera muestra incluida, empezando por el centro de píxel y siguiendo hacia afuera.
-   La máscara de muestra desactiva todas las muestras, excepto una (un escenario común). Para implementar el supermuestreo de varias pasadas, una aplicación puede recorrer los valores de máscara de muestra de un bit y volver a representar la escena para cada muestra mediante el muestreo centroide. Esto requerirá que una aplicación ajuste los derivados para seleccionar correctamente mapas MIP de texturas más detallados para la densidad de muestreo de texturas más alta.

### <a name="span-idderivativecalculationsspanspan-idderivativecalculationsspanspan-idderivativecalculationsspanderivative-calculations-when-multisampling"></a><span id="Derivative_Calculations"></span><span id="derivative_calculations"></span><span id="DERIVATIVE_CALCULATIONS"></span>Cálculos derivados durante el muestreo múltiple

Los sombreadores de píxeles siempre se ejecutan con un área mínima de 2 x 2 píxeles para admitir cálculos derivados, que se calculan tomando las deltas entre los datos de píxeles adyacentes (suponiendo que se han muestreado los datos de cada píxel con un espaciado de unidad horizontal o vertical). Esto no se ve afectado por el muestreo múltiple.

Si se solicitan derivados en un atributo del que se ha realizado el muestreo centroide, el cálculo de hardware no se ajusta, lo que puede dar lugar a derivados inexactos. Un sombreador esperará un vector de unidad en el espacio de destino de representación, pero podría obtener un vector no unitario con respecto a otro espacio de vector. Por lo tanto, es responsabilidad de la aplicación mostrar precaución al solicitar derivados de atributos de los que se ha realizado el muestreo centroide.

De hecho, se recomienda no combinar derivados con el muestreo centroide. El muestreo centroide puede ser útil en situaciones donde es fundamental que los atributos interpolados de un primitivo no se extrapolen, pero implica compensaciones, como atributos que parecen saltar cuando un borde primitivo cruza un píxel (en lugar de cambiar de forma continua) o derivados que no pueden usarse en operaciones de muestreo de texturas que derivan el LOD.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Anexos](appendix.md)

[Fase del rasterizador (RS)](rasterizer-stage--rs-.md)

 

 




