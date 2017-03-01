---
title: "Conversión de tipos de datos"
description: "En las secciones siguientes se describe cómo Direct3D controla las conversiones entre tipos de datos."
ms.assetid: B50AB8DE-CAED-465B-B18C-81F3A984B8AC
keywords:
- "Conversión de tipos de datos"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d6c9e684f4e555bc725077696973be2d3f15a2c5
ms.lasthandoff: 02/07/2017

---

# <a name="data-type-conversion"></a>Conversión de tipos de datos


En las secciones siguientes se describe cómo Direct3D controla las conversiones entre tipos de datos.

## <a name="span-iddatatypeterminologyspanspan-iddatatypeterminologyspanspan-iddatatypeterminologyspandata-type-terminology"></a><span id="Data_Type_Terminology"></span><span id="data_type_terminology"></span><span id="DATA_TYPE_TERMINOLOGY"></span>Terminología de tipos de datos


El conjunto de términos siguiente se usa posteriormente para caracterizar varias conversiones de formato.

| Término  | Definición                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SNORM | Entero con signo normalizado, lo que significa que, para un número complementario de 2 de n bits, el valor máximo significa 1.0f (por ejemplo, el valor de 5 bits 01111 se asigna a 1.0f) y el valor mínimo significa - 1.0f (por ejemplo, el valor de 5 bits 10000 se asigna a -1.0f). Además, el segundo número mínimo se asigna a -1.0f (por ejemplo, el valor de 5 bits 10001 se asigna a - 1.0f). Por lo tanto, existen dos representaciones de enteros para -1.0f. Hay una sola representación para 0.0f y una sola representación para 1.0f. Esto da como resultado un conjunto de representaciones de enteros para valores de punto flotante espaciados uniformemente en el intervalo (-1.0f...0.0f) y también un conjunto complementario de representaciones de números en el intervalo (0.0f...1.0f) |
| UNORM | Entero con signo normalizado, lo que significa que, para un número de n bits, todos los 0 significan 0.0f y todos los 1 significan 1.0f. Se representa una secuencia de valores de punto flotante espaciados uniformemente de 0, 0f a 1.0f. Por ejemplo, un UNORM de 2 bits representa 0.0f, 1/3, 2/3 y 1.0f.                                                                                                                                                                                                                                                                                                                                                                                                                                |
| SINT  | Entero con signo. Entero complementario de 2. Por ejemplo, un SINT de 3 bits representa los valores integrales -4, -3, -2, -1, 0, 1, 2, 3.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| UINT  | Entero sin signo. Por ejemplo, un UINT de 3 bits representa los valores integrales 0, 1, 2, 3, 4, 5, 6, 7.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| FLOAT | Un valor de punto flotante en cualquiera de las representaciones definidas por Direct3D.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| sRGB  | Es similar a UNORM, en que, para un número de n bits, todos los 0 significan 0.0f y todos los 1 significan 1.0f. Sin embargo, a diferencia de UNORM, con sRGB, la secuencia de codificaciones de enteros sin signo entre todos los 0 para todos los 1 representa una progresión no lineal en la interpretación de punto flotante de los números, entre 0.0f y 1.0f. A grandes rasgos, si esta progresión no lineal, sRGB, se muestra como una secuencia de colores, aparecería como una rampa lineal de niveles de luminosidad para un observador "promedio" en condiciones de vista "promedias" en una pantalla "promedia". Para obtener información detallada, consulta el estándar de colores sRGB, IEC 61996-2-1, en IEC (Comisión Electrotécnica Internacional).                |

 

Los términos anteriores se suelen usar como "Modificadores de nombres de formato" y describen cómo se disponen los datos en la memoria y qué conversión se debe realizar en la ruta de transporte (incluido el filtrado posiblemente) de la memoria a una unidad de canalización, como un sombreador, o desde ella.

## <a name="span-idfloatingpointconversionspanspan-idfloatingpointconversionspanspan-idfloatingpointconversionspanfloating-point-conversion"></a><span id="Floating_Point_Conversion"></span><span id="floating_point_conversion"></span><span id="FLOATING_POINT_CONVERSION"></span>Conversión de punto de flotante


Siempre que se produzca una conversión de punto flotante entre diferentes representaciones, incluida la conversión a o desde representaciones de punto no flotante, se aplican las reglas siguientes.

### <a name="span-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanspan-idconververtingfromahigherrangerepresentationtoalowerrangerepresentationspanconververting-from-a-higher-range-representation-to-a-lower-range-representation"></a><span id="Conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="conververting_from_a_higher_range_representation_to_a_lower_range_representation"></span><span id="CONVERVERTING_FROM_A_HIGHER_RANGE_REPRESENTATION_TO_A_LOWER_RANGE_REPRESENTATION"></span>Conversión de una representación de intervalo superior a una representación de intervalo inferior

-   El redondeo a cero se usa durante la conversión a otro formato float. Si el destino es un entero o un formato de punto fijo, se usa el redondeo a par más cercano, a menos que la conversión se documente explícitamente con otro comportamiento de redondeo, por ejemplo, redondear al valor más cercano de FLOAT a SNORM, de FLOAT a UNORM o de FLOAT a sRGB. Otras excepciones son las instrucciones de sombreador ftoi y ftou, que usan el redondeo a cero. Por último, las conversiones de valor float a fijo que usa el muestrario de textura y el rasterizador tienen una tolerancia especificada medida en ULP (unidades en último lugar) desde un ideal infinitamente preciso.
-   Para los valores de origen superiores al intervalo dinámico de un formato de destino de intervalo inferior (p. ej., un valor float grande de 32 bits se escribe en un elemento RenderTarget de tipo float de 16 bits), da como resultado el valor máximo que se puede representar (con un signo correcto), SIN incluir un valor infinito con signo (debido al redondeo a cero que se describe anteriormente).
-   NaN en un formato de intervalo superior se convertirá en la representación de NaN del formato de intervalo inferior, si la representación de NaN existe en el formato de intervalo inferior. Si el formato inferior no tiene ninguna representación de NaN, el resultado será 0.
-   INF en un formato de intervalo superior se convertirá en INF del formato de intervalo inferior si está disponible. Si el formato inferior no tiene ninguna representación de INF, se convertirá en el valor máximo que se pueda representar. El signo se conservará si está disponible en el formato de destino.
-   El número no normalizado en un formato de intervalo superior se convertirá en la representación del número no normalizado del formato de intervalo inferior, si está disponible en el formato de intervalo inferior y la conversión es posible; de lo contrario, el resultado será 0. El bit de signo se conservará si está disponible en el formato de destino.

### <a name="span-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanspan-idconvertingfromalowerrangerepresentationtoahigherrangerepresentationspanconverting-from-a-lower-range-representation-to-a-higher-range-representation"></a><span id="Converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="converting_from_a_lower_range_representation_to_a_higher_range_representation"></span><span id="CONVERTING_FROM_A_LOWER_RANGE_REPRESENTATION_TO_A_HIGHER_RANGE_REPRESENTATION"></span>Conversión de una representación de intervalo inferior a una representación de intervalo superior

-   NaN en un formato de intervalo inferior se convertirá en la representación de NaN del formato de intervalo superior, si está disponible en el formato de intervalo superior. Si el formato de intervalo superior no tiene ninguna representación de NaN, se convertirá en 0.
-   INF en un formato de intervalo inferior se convertirá en la representación de INF del formato de intervalo superior, si está disponible en el formato de intervalo superior. Si el formato superior no tiene ninguna representación de INF, se convertirá en el valor máximo que se pueda representar (MAX\_FLOAT en ese formato). El signo se conservará si está disponible en el formato de destino.
-   El número no normalizado en un formato de intervalo inferior se convertirá en una representación normalizada del formato de intervalo superior, si es posible, o bien en una representación del número no normalizado del formato de intervalo superior, si esta existe. En caso de que se produzca un error en estos, si el formato del intervalo superior no tiene ninguna representación de número no normalizado, este se convertirá en 0. El signo se conservará si está disponible en el formato de destino. Observa que los números de tipo float de 32 bits cuentan como formato sin una representación de número no normalizado (porque los números no normalizados en operaciones de valores float de 32 bits se vacían en 0 con signo).

## <a name="span-idintegerconversionspanspan-idintegerconversionspanspan-idintegerconversionspaninteger-conversion"></a><span id="Integer_Conversion"></span><span id="integer_conversion"></span><span id="INTEGER_CONVERSION"></span>Conversión de enteros


En la siguiente tabla se describen las conversiones de varias representaciones descritas anteriormente para otras representaciones. Solo se muestran las conversiones que realmente se producen en Direct3D.

Con enteros, a menos que se especifique lo contrario, todas las conversiones a/de representaciones de enteros y a representaciones float que se describen a continuación se realizarán con exactitud.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tipo de datos de origen</th>
<th align="left">Tipo de datos de destino</th>
<th align="left">Regla de conversión</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">SNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>Dado un valor entero de n bits que representa el intervalo con signo [de -1.0f a 1.0f], la conversión a punto flotante es la siguiente.</p>
<ul>
<li>El valor más negativo se asigna a - 1.0f. p. ej., el valor de 5 bits 10000 se asigna a - 1.0f.</li>
<li>Todos los demás valores se convierten en un valor float (denominado c) y, a continuación, da como resultado = c * (1.0f / (2⁽ⁿ⁻¹⁾-1)). Por ejemplo, el valor de 5 bits 10001 se convierte en -15.0f y, a continuación, se divide por 15.0f y da -1.0f.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">SNORM</td>
<td align="left"><p>Dado un número de punto flotante, la conversión a un valor entero de n bits que representa el intervalo con signo [de -1.0f a 1.0f] es la siguiente.</p>
<ul>
<li>Permite que c represente el valor inicial.</li>
<li>Si c es NaN, el resultado es 0.</li>
<li>Si c es &gt; 1.0f, incluido INF, se restringe hasta 1.0f.</li>
<li>Si c es &lt; -1.0f, incluido -INF, se restringe hasta -1.0f.</li>
<li>Se realiza una conversión de escala de tipo float a escala de entero: c = c * (2ⁿ⁻¹-1).</li>
<li>Se realiza una conversión a un entero de la siguiente manera.
<ul>
<li>Si c es &gt;= 0, c = c + 0.5F; en caso contrario, c = c - 0.5F.</li>
<li>Coloca la fracción decimal y el punto flotante restante (integral) se convierte directamente en un entero.</li>
</ul></li>
</ul>
<p>A esta conversión se le permite una tolerancia de D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_Unit-Last-Place Unit-Last-Place (en el lado entero). Esto significa que, después de la conversión de la escala float a entera, cualquier valor dentro de D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place de un valor de formato de destino que se pueda representar puede asignarse a ese valor. El requisito adicional de invertibilidad de datos garantiza que la conversión no es descendente entre el intervalo y que pueden obtenerse todos los valores de salida. (En las constantes que se muestran aquí, <em>xx</em> debe reemplazarse por la versión de Direct3D, por ejemplo, 10, 11 o 12).</p></td>
</tr>
<tr class="odd">
<td align="left">UNORM</td>
<td align="left">FLOAT</td>
<td align="left"><p>El valor inicial de n bits se convierte en float (0.0f, 1.0f, 2.0f, etc.) y, a continuación, se divide por (2ⁿ-1).</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">UNORM</td>
<td align="left"><p>Permite que c represente el valor inicial.</p>
<ul>
<li>Si c es NaN, el resultado es 0.</li>
<li>Si c es &gt; 1.0f, incluido INF, se restringe hasta 1.0f.</li>
<li>Si c es &lt; 0.0f, incluido -INF, se restringe hasta 0.0f.</li>
<li>Se realiza una conversión de escala de tipo float a escala de entero: c = c * (2ⁿ-1).</li>
<li>Se realiza una conversión a un entero.
<ul>
<li>c = c + 0.5f.</li>
<li>Se coloca la fracción decimal y el punto flotante restante (integral) se convierte directamente en un entero.</li>
</ul></li>
</ul>
<p>A esta conversión se le permite una tolerancia de D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place (en el lado entero). Esto significa que, después de la conversión de la escala float a entera, cualquier valor dentro de D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place de un valor de formato de destino que se puede representar puede asignarse a ese valor. El requisito adicional de invertibilidad de datos garantiza que la conversión no es descendente entre el intervalo y que pueden obtenerse todos los valores de salida.</p></td>
</tr>
<tr class="odd">
<td align="left">sRGB</td>
<td align="left">FLOAT</td>
<td align="left"><p>A continuación, se muestra la conversión ideal de sRGB a FLOAT.</p>
<ul>
<li>Toma el valor de n bits inicial, conviértelo en un valor float (0.0f, 1.0f, 2.0f, etc.); llámalo c.</li>
<li>c = c * (1.0f / (2ⁿ-1))</li>
<li>Si (c &lt; = D3D<em>xx</em>_SRGB_TO_FLOAT_THRESHOLD), entonces el resultado es igual a c / D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_1, o bien es igual a ((c + D3D<em>xx</em>_SRGB_TO_FLOAT_OFFSET)/D3D<em>xx</em>_SRGB_TO_FLOAT_DENOMINATOR_2)D3D<em>xx</em>_SRGB_TO_FLOAT_EXPONENT</li>
</ul>
<p>A esta conversión se le permite una tolerancia de D3D<em>xx</em>_SRGB_TO_FLOAT_TOLERANCE_IN_ULP Unit-Last-Place (en el lado de sRGB).</p></td>
</tr>
<tr class="even">
<td align="left">FLOAT</td>
<td align="left">sRGB</td>
<td align="left"><p>A continuación, se muestra la conversión ideal de FLOAT -&gt; sRGB.</p>
<p>Suponiendo que el componente de color sRGB de destino tiene n bits:</p>
<ul>
<li>Se supone que el valor inicial es c.</li>
<li>Si c es NaN, el resultado es 0.</li>
<li>Si c es &gt; 1.0f, incluido INF, se restringe hasta 1.0f.</li>
<li>Si c es &lt; 0.0f, incluido -INF, se restringe hasta 0.0f.</li>
<li>Si (c &lt;= D3D<em>xx</em>_FLOAT_TO_SRGB_THRESHOLD), entonces c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_1 * c, o bien c = D3D<em>xx</em>_FLOAT_TO_SRGB_SCALE_2 * c(D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_NUMERATOR/D3D<em>xx</em>_FLOAT_TO_SRGB_EXPONENT_DENOMINATOR) - D3D<em>xx</em>_FLOAT_TO_SRGB_OFFSET</li>
<li>Se realiza una conversión de escala de tipo float a escala de entero: c = c * (2ⁿ-1).</li>
<li>Se realiza una conversión a un entero:
<ul>
<li>c = c + 0.5f.</li>
<li>Se coloca la fracción decimal y el punto flotante restante (integral) se convierte directamente en un entero.</li>
</ul></li>
</ul>
<p>A esta conversión se le permite una tolerancia de D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place (en el lado entero). Esto significa que, después de la conversión de la escala float a entera, cualquier valor dentro de D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP Unit-Last-Place de un valor de formato de destino que se puede representar puede asignarse a ese valor. El requisito adicional de invertibilidad de datos garantiza que la conversión no es descendente entre el intervalo y que pueden obtenerse todos los valores de salida.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">SINT con más bits</td>
<td align="left"><p>Para realizar una conversión de SINT a SINT con más bits, el bit más significativo (MSB) del número inicial produce una &quot;extensión de signo&quot; a los bits adicionales disponibles en el formato de destino.</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">SINT con más bits</td>
<td align="left"><p>Para realizar una conversión de UINT a SINT con más bits, el número se copia en los bits menos importantes (LSB) del formato de destino y los bits más importantes (MSB) adicionales se rellenan con 0.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT</td>
<td align="left">UINT con más bits</td>
<td align="left"><p>Para realizar una conversión de SINT a UINT con más bits: si el valor es negativo, se restringe hasta 0. De lo contrario, el número se copia en los bits menos importantes (LSB) del formato de destino y los bits más importantes (MSB) adicionales se rellenan con 0.</p></td>
</tr>
<tr class="even">
<td align="left">UINT</td>
<td align="left">UINT con más bits</td>
<td align="left"><p>Para realizar una conversión de UINT a UINT con más bits, el número se copia en los LSB del formato objetivo y los MSB adicionales se rellenan con 0.</p></td>
</tr>
<tr class="odd">
<td align="left">SINT o UINT</td>
<td align="left">SINT o UINT con más o los mismos bits</td>
<td align="left"><p>Para realizar una conversión de SINT o UINT a SINT o UINT con menos bits o los mismos (o cambiar de tipo signed/unsigned), el valor inicial se restringe simplemente en el intervalo del formato de destino.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanspan-idfixedpointintegerconversionspanfixed-point-integer-conversion"></a><span id="Fixed_Point_Integer_Conversion"></span><span id="fixed_point_integer_conversion"></span><span id="FIXED_POINT_INTEGER_CONVERSION"></span>Conversión de número entero de punto fijo


Los enteros de punto fijo son simplemente enteros con algunos bits de tamaño que tienen un punto decimal implícito en una ubicación fija.

El tipo de datos ubicuo "entero" es un caso especial de un entero de punto fijo con el decimal al final del número.

Las representaciones de números de punto fijo se caracterizan como: i.f, donde i es el número de bits del entero y f es el número de bits fraccionario. p. ej., 16.8 significa entero de 16 bits seguido de 8 bits de fracción. La parte del entero se almacena en el complementario de 2, al menos como se define aquí (aunque también se puede definir igualmente para enteros sin signo). La parte fraccionaria se almacena en un formulario sin signo. La parte fraccionaria siempre representa la fracción positiva entre los dos valores integrales más cercanos, empezando por el más negativo.

Las operaciones de suma y resta en números de punto fijo se realizan simplemente con aritmética de enteros estándar, sin tener en cuenta la ubicación en la que se encuentra el decimal implícito. Sumar 1 al número de punto fijo 16.8 simplemente significa sumar 256, dado que decimal se encuentra a 8 lugares del final del número menos significativo. También pueden realizarse otras operaciones, como la multiplicación. Para ello, simplemente debe usarse la aritmética de enteros, siempre que se tenga en cuenta el efecto en el decimal fijo. Por ejemplo, el resultado de multiplicar dos enteros 16.8 mediante un múltiplo entero es 32.16.

Las representaciones de enteros de punto fijo se usan de dos formas en Direct3D.

-   Las posiciones de vértice recortadas posteriormente en el rasterizador se ajustan al punto fijo, a fin de distribuir uniformemente la precisión en el área de RenderTarget. Muchas operaciones del rasterizador, incluida la selección de la cara por ejemplo, se producen en posiciones acopladas de punto fijo, mientras que otras operaciones, como la configuración del interpolador del atributo, usan posiciones que han vuelto a convertirse en punto flotante desde las posiciones acopladas de punto fijo.
-   Las coordenadas de textura para las operaciones de ejemplo se ajustan a un punto fijo (después de que los escale el tamaño de la textura), para distribuir uniformemente la precisión a través del espacio de la textura a la hora de elegir el grosor y las ubicaciones de pulsación de filtro. Los valores de peso se vuelven a convertir en un punto flotante antes de que se realice la aritmética real de filtrado.

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tipo de datos de origen</th>
<th align="left">Tipo de datos de destino</th>
<th align="left">Regla de conversión</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">FLOAT</td>
<td align="left">Entero de punto fijo</td>
<td align="left"><p>A continuación, se muestra el procedimiento general para convertir un número de punto flotante n a un i.f entero de punto fijo, donde i es el número de bits del entero (con signo) y f es el número de bits fraccionario.</p>
<ul>
<li>Calcular FixedMin = -2⁽ⁱ⁻¹⁾</li>
<li>Calcular FixedMax = 2⁽ⁱ⁻¹⁾ - 2<sup>(-f)</sup></li>
<li>Si n es NaN, el resultado es = 0; si n es +Inf, el resultado es = FixedMax*2<sup>f</sup>; si n es -Inf, el resultado es = FixedMin*2<sup>f</sup></li>
<li>Si n es &gt;= FixedMax, el resultado es = Fixedmax*2<sup>f</sup>; si n es &lt;= FixedMin, el resultado es = FixedMin*2<sup>f</sup></li>
<li>De lo contrario, debe calcularse n*2<sup>f</sup> y convertirlo a entero.</li>
</ul>
<p>A las implementaciones se les permite la tolerancia D3D<em>xx</em>_FLOAT32_TO_INTEGER_TOLERANCE_IN_ULP del resultado del entero, en lugar del valor infinitamente preciso n*2<sup>f</sup> después del último paso anterior.</p></td>
</tr>
<tr class="even">
<td align="left">Entero de punto fijo</td>
<td align="left">FLOAT</td>
<td align="left"><p>Supone que la representación de punto fijo específica que se convierte en tipo float no contiene más de un total de 24 bits de información, de los cuales, menos de 23 bits está en el componente fraccionario. Supone que un número de punto fijo dado, fxp, se encuentra en la forma i.f. (entero de bits i, fracción de bits f). La conversión al valor float es como el siguiente seudocódigo.</p>
<p>resultado de float = (float)(fxp &gt;&gt; f) + // entero de extracción</p>
((float)(fxp &amp; (2<sup>f</sup> - 1)) / (2<sup>f</sup>)); // fracción de extracción</td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Anexos](appendix.md)

 

 





