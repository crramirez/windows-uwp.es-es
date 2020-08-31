---
title: Reglas de rasterización
description: Las reglas de rasterización definen cómo se asignan los datos vectoriales a los datos de tramas.
ms.assetid: B604725F-96A5-4DB6-B597-9EC57FBBC024
keywords:
- Reglas de rasterización
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7cc139fe9b3ad5324ea6e325f2806d9cf9c5ee1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168049"
---
# <a name="rasterization-rules"></a>Reglas de rasterización


Las reglas de rasterización definen cómo se asignan los datos vectoriales a los datos de tramas. Los datos de trama se ajustan a las ubicaciones de enteros que se seleccionan y recortan (para dibujar el número mínimo de píxeles), y los atributos por píxel se interpolan (a partir de los atributos por vértice) antes de que se pasen a un sombreador de píxeles.

Hay varios tipos de reglas, que dependen del tipo de primitiva que se está asignando, y de si los datos utilizan el muestreo múltiple para reducir el alias. En las ilustraciones siguientes se muestra cómo se administran los casos de esquina.

## <a name="span-idtrianglespanspan-idtrianglespanspan-idtrianglespantriangle-rasterization-rules-without-multisampling"></a><span id="Triangle"></span><span id="triangle"></span><span id="TRIANGLE"></span>Reglas de rasterización de triángulos (sin muestreo múltiple)


Se dibuja cualquier centro de píxeles que se encuentre dentro de un triángulo. se supone que un píxel está dentro si pasa la regla superior izquierda. La regla de la parte superior izquierda es que se define un centro de píxeles para que se encuentre dentro de un triángulo si se encuentra en el borde superior o en el borde izquierdo de un triángulo.

donde:

-   Un borde superior es un borde que es exactamente horizontal y está encima de los demás bordes.
-   Un borde izquierdo, es un borde que no es exactamente horizontal y está en el lado izquierdo del triángulo, un triángulo puede tener uno o dos bordes izquierdos.

La regla de la parte superior izquierda garantiza que los triángulos adyacentes se dibujan una vez.

En esta ilustración se muestran ejemplos de píxeles que se dibujan porque se encuentran dentro de un triángulo o siguen la regla superior izquierda.

![ejemplos de rasterización del triángulo superior izquierdo](images/d3d10-rasterrulestriangle.png)

El color gris claro y oscuro que cubre los píxeles se muestran como grupos de píxeles para indicar el triángulo en el que se encuentran.

## <a name="span-idline_1spanspan-idline_1spanspan-idline_1spanline-rasterization-rules-aliased-without-multisampling"></a><span id="Line_1"></span><span id="line_1"></span><span id="LINE_1"></span>Reglas de rasterización de líneas (con alias, sin muestreo múltiple)


Las reglas de rasterización de línea usan un área de prueba de rombo para determinar si una línea cubre un píxel. En el caso de las líneas x principales (líneas con-1 &lt; = Slope &lt; = + 1), el área de prueba de rombo incluye (se muestra sólido) el borde inferior izquierdo, el borde inferior derecho y la esquina inferior; el rombo excluye (se muestra con puntos) el borde superior izquierdo, el borde superior derecho, el cable superior, la esquina izquierda y la esquina derecha. Una línea principal y es cualquier línea que no sea una línea x principal; el área de rombos de prueba es la misma que la descrita para la línea x principal, excepto en que también se incluye la esquina derecha.

Dado el área de rombo, una línea cubre un píxel si la línea sale del área de prueba de rombo del píxel cuando viaja a lo largo de la línea desde el principio hacia el final. Una franja de líneas se comporta igual, ya que se dibuja como una secuencia de líneas.

En la ilustración siguiente se muestran algunos ejemplos.

![ejemplos de rasterización de líneas con alias](images/d3d10-rasterrulesline.png)

## <a name="span-idline_2spanspan-idline_2spanspan-idline_2spanline-rasterization-rules-antialiased-without-multisampling"></a><span id="Line_2"></span><span id="line_2"></span><span id="LINE_2"></span>Reglas de rasterización de línea (antialiased, sin muestreo múltiple)


Una línea alisada se rasteriza como si fuera un rectángulo (con el ancho = 1). El rectángulo forma una intersección con un destino de representación que genera valores de cobertura por píxel, que se multiplican por componentes alfa de salida del sombreador de píxeles. No hay ningún suavizado de contorno cuando se dibujan líneas en un destino de representación multimuestreado.

Se considera que no hay una única manera "mejor" de realizar la representación de líneas alisadas. Direct3D adopta como directriz el método que se muestra en la siguiente ilustración. Este método se deriva empíricamente, lo que presenta un número de propiedades visuales que se consideran deseables.

No es necesario que el hardware coincida exactamente con este algoritmo; las pruebas realizadas en esta referencia deben tener tolerancias "razonables", guiadas por algunos de los principios que se enumeran más adelante, con lo que se permiten varias implementaciones de hardware y filtran los tamaños de kernel. Sin embargo, no se permite ninguna de estas flexibilidad en la implementación de hardware, sino que se puede comunicar a través de Direct3D a aplicaciones, además de dibujar líneas y observar y medir su aspecto.

![ejemplos de rasterización de líneas suavizadas](images/d3d10-rasterruleslineaa.png)

Este algoritmo genera líneas relativamente suaves, con intensidad uniforme, con bordes dentados mínimos o trenzado. Se minimiza el patrón Moire para las líneas de cierre. Existe una buena cobertura para las uniones entre segmentos de línea colocados de un extremo a otro.

El kernel de filtro es un equilibrio razonable entre la cantidad de desenfoque de borde y los cambios de intensidad causados por las correcciones gamma. El valor de cobertura se multiplica por el sombreador de píxeles O0. a (srcAlpha) por la siguiente fórmula en la [fase de fusión de salida (OM)](output-merger-stage--om-.md):

srcColor \* srcAlpha + destColor \* (1-srcAlpha)

## <a name="span-idpointspanspan-idpointspanspan-idpointspanpoint-rasterization-rules-without-multisampling"></a><span id="Point"></span><span id="point"></span><span id="POINT"></span>Reglas de rasterización de punto (sin muestreo múltiple)


Un punto se interpreta como si estuviese compuesto de dos triángulos en un patrón Z, que usan reglas de rasterización de triángulo. La coordenada identifica el centro de un cuadrado ancho de un píxel. No hay selección de puntos.

En la ilustración siguiente se muestran algunos ejemplos.

![ejemplos de rasterización de puntos](images/d3d10-rasterrulespoint.png)

## <a name="span-idmultisamplespanspan-idmultisamplespanspan-idmultisamplespanmultisample-anti-aliasing-rasterization-rules"></a><span id="Multisample"></span><span id="multisample"></span><span id="MULTISAMPLE"></span>Reglas de rasterización del suavizado de contorno de muestreo múltiple


El suavizado de contorno de muestreo múltiple (MSAA) reduce el alias de geometría mediante pruebas de cobertura de píxeles y de estarcido de profundidad en varias ubicaciones de submuestra. Para mejorar el rendimiento, los cálculos por píxel se realizan una vez para cada píxel incluido, compartiendo las salidas del sombreador entre los subpíxeles incluidos. El suavizado de contorno de muestreo Multimuestra no reduce el alias de la superficie. Las ubicaciones de ejemplo y las funciones de reconstrucción dependen de la implementación de hardware.

En la ilustración siguiente se muestran algunos ejemplos.

![ejemplos de rasterización del suavizado de contorno Multimuestra](images/d3d10-rasterrulesmsaa.png)

El número de ubicaciones de ejemplo depende del modo de muestreo Multimuestra. Los atributos de vértice se interpolan en centros de píxeles, ya que es donde se invoca el sombreador de píxeles (esto se convierte en una extrapolación si no se trata el centro). Los atributos se pueden marcar en el sombreador de píxeles para que se muestren en centroide Sample, lo que hace que los píxeles no descritos interpolen el atributo en la intersección del área del píxel y el primitivo.

Un sombreador de píxeles se ejecuta para cada área de píxeles de 2x2 para admitir cálculos derivados (que usan diferencias x e y). Esto significa que las invocaciones del sombreador se producen más de lo que se muestra para rellenar el mínimo de 2x2 cuantos (que es independiente del multimuestreo). El resultado del sombreador se escribe para cada ejemplo descrito que pasa la prueba por muestra de la galería de símbolos de profundidad.

Las reglas de rasterización para primitivas son, en general, sin cambios mediante el suavizado de contorno de muestreo Multimuestra, excepto:

-   En el caso de un triángulo, se realiza una prueba de cobertura para cada ubicación de ejemplo (no para un centro de píxeles). Si se trata de más de una ubicación de ejemplo, un sombreador de píxeles se ejecuta una vez con atributos interpolados en el centro de píxeles. El resultado se almacena (replica) para cada ubicación de ejemplo tratada en el píxel que pasa la prueba de profundidad/estarcido.

    Una línea se trata como un rectángulo compuesto por dos triángulos, con un ancho de línea de 1,4.

-   En un momento dado, se realiza una prueba de cobertura para cada ubicación de ejemplo (no para un centro de píxeles).

Los formatos de muestreo múltiple se pueden usar en destinos de representación que se pueden volver a leer en sombreadores mediante la [carga](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load), ya que no se requiere ninguna resolución para los ejemplos individuales a los que tiene acceso el sombreador. Los formatos de profundidad no se admiten para recursos de ejemplo Multimuestra; por lo tanto, los formatos de profundidad están restringidos exclusivamente a los destinos de representación.

Los formatos sin tipo admiten el muestreo múltiple para permitir que una vista de recursos interprete los datos de maneras diferentes. Por ejemplo, puede crear un recurso de ejemplo múltiple mediante R8G8B8A8 \_ sin tipo, representarlo con un recurso Render-Target-View con un \_ formato R8G8B8A8 uint y, a continuación, resolver el contenido en otro recurso con un \_ formato de datos R8G8B8A8 UNORM.

### <a name="span-idhardware_supportspanspan-idhardware_supportspanspan-idhardware_supportspanhardware-support"></a><span id="Hardware_Support"></span><span id="hardware_support"></span><span id="HARDWARE_SUPPORT"></span>Compatibilidad de hardware

La API informa de la compatibilidad de hardware para el muestreo múltiple a través del número de niveles de calidad. Por ejemplo, un nivel de calidad 0 significa que el hardware no admite el muestreo múltiple (con un formato y un nivel de calidad determinados). Un 3 para los niveles de calidad significa que el hardware admite tres diseños de ejemplo diferentes y/o los algoritmos de resolución. También puede suponer lo siguiente:

-   Cualquier formato que admita el muestreo múltiple admite el mismo número de niveles de calidad para cada formato de esa familia.
-   Cada formato que admite el muestreo múltiple y tiene los \_ formatos UNORM, \_ sRGB, \_ SNORM o \_ float, también admite la resolución.

### <a name="span-idcentroid_samplingspanspan-idcentroid_samplingspanspan-idcentroid_samplingspancentroid-sampling-of-attributes-when-multisample-antialiasing"></a><span id="Centroid_Sampling"></span><span id="centroid_sampling"></span><span id="CENTROID_SAMPLING"></span>Centroide muestreo de atributos cuando el suavizado de contorno de muestreo múltiple

De forma predeterminada, los atributos de vértice se interpolan en un centro de píxeles durante el suavizado de contorno de muestreo. Si no se trata el centro de píxeles, los atributos se extrapolan a un centro de píxeles. Si una entrada de sombreador de píxeles que contiene la semántica de centroide (suponiendo que el píxel no está totalmente incluido) se muestreará en alguna parte dentro del área tratada del píxel, posiblemente en una de las ubicaciones de ejemplo tratadas. Una máscara de ejemplo (especificada por el estado del rasterizador) se aplica antes del cálculo de centroide. Por lo tanto, un ejemplo que se enmascara no se usará como ubicación centroide.

El rasterizador de referencia elige una ubicación de ejemplo para el muestreo de centroide similar a la siguiente:

-   La máscara de ejemplo permite todos los ejemplos. Use un centro de píxeles si el píxel está incluido o si no se trata ninguno de los ejemplos. De lo contrario, se elige el primer ejemplo descrito, empezando por el centro de píxeles y desplazándose hacia fuera.
-   La máscara de ejemplo desactiva todos los ejemplos, pero uno (un escenario común). Una aplicación puede implementar el supermuestreo de Multipass mediante el ciclo de valores de máscara de ejemplo de un solo bit y volver a representar la escena para cada ejemplo mediante el muestreo de centroide. Esto requeriría que una aplicación ajustara los derivados para seleccionar el MIPS de texturas adecuado de forma adecuada para la densidad de muestreo de textura superior.

### <a name="span-idderivative_calculationsspanspan-idderivative_calculationsspanspan-idderivative_calculationsspanderivative-calculations-when-multisampling"></a><span id="Derivative_Calculations"></span><span id="derivative_calculations"></span><span id="DERIVATIVE_CALCULATIONS"></span>Cálculos derivados en el muestreo múltiple

Los sombreadores de píxeles siempre se ejecutan con un área de píxeles 2x2 mínima para admitir cálculos derivados, que se calculan tomando diferencias entre los datos de los píxeles adyacentes (suponiendo que los datos de cada píxel se muestren con un espaciado horizontal o vertical). Esto no se ve afectado por el muestreo múltiple.

Si se solicitan derivados en un atributo que se ha centroide muestreado, el cálculo de hardware no se ajusta, lo que puede provocar derivados inexactos. Un sombreador esperará un vector de unidad en el espacio de destino de representación, pero puede obtener un vector no de unidad con respecto a otro espacio vectorial. Por lo tanto, es responsabilidad de una aplicación tener cuidado al solicitar derivados de atributos que se centroide muestrear.

De hecho, se recomienda no combinar el muestreo de derivados y centroide. El muestreo de centroide puede ser útil en situaciones en las que es fundamental que no se extrapolan los atributos interpolados de una primitiva, pero esto incluye inconvenientes, como los atributos que parecen saltar cuando un borde primitivo cruza un píxel (en lugar de cambiarse continuamente) o derivados que no se pueden usar en las operaciones de muestreo de textura que derivan LOD.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Apéndices](appendix.md)

[Fase del rasterizador (RS)](rasterizer-stage--rs-.md)

 

 