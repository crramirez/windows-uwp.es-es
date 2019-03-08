---
title: Reglas de punto flotante
description: Direct3D admite varias representaciones de punto flotante. Todos los cálculos de punto flotante funcionan en un subconjunto de las reglas de punto flotante IEEE 754 de precisión simple de 32 bits.
ms.assetid: 3B0C95E2-1025-4F70-BF14-EBFF2BB53AFF
keywords:
- Reglas de punto flotante
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4de5ba146c8241598527dd268d604fcc9bb97d6d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662360"
---
# <a name="span-iddirect3dconceptsfloating-pointrulesspanfloating-point-rules"></a><span id="direct3dconcepts.floating-point_rules"></span>Reglas de punto flotante


Direct3D admite varias representaciones de punto flotante. Todos los cálculos de punto flotante funcionan en un subconjunto de las reglas de punto flotante IEEE 754 de precisión simple de 32 bits.

## <a name="span-idalpha32bitspanspan-idalpha32bitspan32-bit-floating-point-rules"></a><span id="alpha_32_bit"></span><span id="ALPHA_32_BIT"></span>reglas de punto flotante de 32 bits


Existen dos conjuntos de reglas: aquellos que se ajustan a IEEE-754 y los que difieren del estándar.

### <a name="span-idalpha754rulesspanspan-idalpha754rulesspanspan-idalpha754rulesspanhonored-ieee-754-rules"></a><span id="alpha_754_Rules"></span><span id="alpha_754_rules"></span><span id="ALPHA_754_RULES"></span>Respetar las reglas de IEEE-754

Algunas de estas reglas son una opción única en que IEEE-754 ofrece posibilidades.

-   La división entre 0 produce +/- INF, excepto 0/0, que da como resultado NaN.
-   El registro de (+/-) 0 genera -INF.  

    El registro de un valor negativo (que no sea -0) da como resultado NaN.
-   La raíz cuadrada recíproca (rsq) o la raíz cuadrada (sqrt) de un número negativo da como resultado NaN.  

    La excepción es -0; sqrt(-0) da como resultado -0, y rsq(-0) da como resultado -INF.
-   INF - INF = NaN
-   (+/-)INF / (+/-)INF = NaN
-   (+/-) INF \* 0 = NaN
-   Cualquier valor NaN (cualquier OP) = NaN
-   Las comparaciones EQ, GT, GE, LT y LE, cuando uno o ambos operandos es NaN, devuelven **FALSE**.
-   Las comparaciones omiten el signo de 0 (por lo que +0 es igual a -0).
-   La comparación NE, cuando uno o ambos operandos es NaN, devuelve **TRUE**.
-   Las comparaciones de cualquier valor que no sea NaN con +/-INF devuelven el resultado correcto.

### <a name="span-idalpha754deviationsspanspan-idalpha754deviationsspanspan-idalpha754deviationsspandeviations-or-additional-requirements-from-ieee-754-rules"></a><span id="alpha_754_Deviations"></span><span id="alpha_754_deviations"></span><span id="ALPHA_754_DEVIATIONS"></span>Desviaciones o requisitos adicionales de IEEE-754 reglas

-   IEEE-754 requiere operaciones de punto flotante para dar un resultado que sea el valor más cercano representable en un resultado infinitamente preciso, conocido como redondeo a par más cercano.

    Direct3D 11 y definir hasta el mismo requisito como IEEE-754: operaciones de punto flotante de 32 bits producen un resultado que está dentro de 0,5 unidad last-contexto (ULP) del resultado infinitamente precisa. Esto significa que, por ejemplo, el hardware puede truncar resultados en 32 bits en lugar de realizar un redondeo a par más cercano, lo que generaría un error máximo de 0.5 ULP Esta regla se aplica solo a la suma, la resta y la multiplicación.

    Las versiones anteriores de Direct3D definen un requisito de IEEE-754 más flexible: operaciones de punto flotante de 32 bits producen un resultado que está dentro de una unidad última-contexto (1 ULP) del resultado infinitamente precisa. Esto significa que, por ejemplo, el hardware puede truncar resultados en 32 bits en lugar de realizar un redondeo a par más cercano, lo que generaría un error de como máximo un ULP

-   No se admiten las excepciones de punto flotante, los bits de estado ni las capturas.
-   Los números no normalizados se vacían en cero con signo en la entrada y la salida de cualquier operación matemática de punto flotante. Las excepciones están pensadas para cualquier operación de movimiento de datos o de E/S que no manipule los datos.
-   Los estados que contienen los valores de punto flotante, como los valores MinDepth/MaxDepth o BorderColor de ventanilla, pueden proporcionarse como valores no normalizados y pueden vaciarse o no antes de que el hardware los use.
-   Las operaciones Min o Max vacían los números no normalizados para hacer una comparación, pero el resultado puede estar vacío de números no normalizados o no.
-   La entrada de NaN en una operación siempre da como resultado NaN en la salida. Sin embargo, no se necesita el mismo patrón de bits de NaN para que siga dando lo mismo (a menos que la operación sea una instrucción de movimiento sin procesar, la cual no modifica los datos).
-   Las operaciones Min o Max para las que solo un operando es NaN devuelven el otro operando como resultado (al contrario que las reglas de comparación que hemos visto anteriormente). Se trata de una regla de IEEE 754R.

    La especificación IEEE 754R de las operaciones Min y Max de punto flotante indican que si una de las entradas a Min o Max es un valor QNaN silencioso, el resultado de la operación es el otro parámetro. Por ejemplo:

    ```ManagedCPlusPlus
    min(x,QNaN) == min(QNaN,x) == x (same for max)
    ```

    Una revisión de la especificación IEEE-754R adopta un comportamiento diferente para Min y Max cuando una entrada es un valor de SNaN "señalizado" frente a un valor QNaN:

    ```ManagedCPlusPlus
    min(x,SNaN) == min(SNaN,x) == QNaN (same for max)
     
    ```

    Por lo general, Direct3D siguen los estándares para la aritmética: IEEE-754 y 754R de IEEE. Pero en este caso, tenemos una desviación.

    Las reglas de aritmética de Direct3D 10 y versiones posteriores no hacen distinciones entre los valores NaN silenciosos y señalizados (QNaN frente a SNaN). Todos los valores de NaN se controlan del mismo modo. En el caso de Min y Max, el comportamiento de Direct3D de cualquier valor NaN equivale a cómo se controla QNaN en la definición de IEEE-754R. (Para ser más exactos, si ambas entradas son NaN, se devuelve cualquier valor NaN).

-   Otra regla de IEEE 754R es min(-0,+0) == min(+0,-0) == -0 y max(-0,+0) == max(+0,-0) == + 0, que respeta el signo, en contraposición a las reglas de comparación para el cero con signo (tal y como hemos visto anteriormente). Direct3D recomienda el comportamiento de IEEE 754R en este caso, pero no lo aplica; es aceptable para el resultado de la comparación de ceros para que sea dependiente del orden de los parámetros, al usar una comparación que no distingue los signos.
-   x\*1.0f siempre da como resultado x (excepto denorm vaciado).
-   x/1.0f siempre da como resultado x (excepto el número no normalizado vaciado).
-   x +/- 0.0f siempre da como resultado x (excepto el número no normalizado vaciado). Pero -0 + 0 = +0.
-   Las operaciones fusionadas (por ejemplo, mad, dp3) producen resultados que son más precisos que la peor ordenación de serie posible de evaluación de la expansión sin fusionar de la operación. La definición de la posible peor ordenación, por motivos de tolerancia, no es una definición fija para una operación determinada combinada; depende de los valores concretos de las entradas. A los pasos individuales de la expansión sin fusionar se permite una tolerancia de 1 ULP (o bien, para las instrucciones en que Direct3D llama con una tolerancia un poco más laxa que 1 ULP, se permite la tolerancia más laxa).
-   Las operaciones fusionadas cumplen las mismas reglas NaN que las operaciones no fusionadas.
-   sqrt y rcp tienen una tolerancia de 1 ULP. Las instrucciones de raíz cuadrada recíprocas y recíprocas del sombreador, [**rcp**](https://msdn.microsoft.com/library/windows/desktop/hh447205) y [**rsq**](https://msdn.microsoft.com/library/windows/desktop/hh447221), tienen sus propios requisitos de precisión estrictos independientes.
-   La multiplicación y la división operan en el nivel de precisión de punto flotante de 32 bits (precisión de 0.5 ULP en la multiplicación y de 1.0 ULP para el valor recíproco). Si x/y se implementa directamente, los resultados deben tener la misma precisión o superior que un método de dos pasos.

## <a name="span-iddoubleprec64bitspanspan-iddoubleprec64bitspan64-bit-double-precision-floating-point-rules"></a><span id="double_prec_64_bit"></span><span id="DOUBLE_PREC_64_BIT"></span>64 bits (precisión doble) reglas de punto flotantes


Los controladores de hardware y de pantalla admiten opcionalmente el punto flotante de precisión doble. Para indicar la compatibilidad, cuando se llama a [ **ID3D11Device::CheckFeatureSupport** ](https://msdn.microsoft.com/library/windows/desktop/ff476497) con [ **D3D11\_característica\_dobles** ](https://msdn.microsoft.com/library/windows/desktop/ff476124#d3d11-feature-doubles), los conjuntos de controladores **DoublePrecisionFloatShaderOps** de [ **D3D11\_característica\_datos\_dobles** ](https://msdn.microsoft.com/library/windows/desktop/ff476127) en TRUE. A continuación, el controlador y el hardware deben admitir todas las instrucciones de punto flotante de precisión doble.

Las instrucciones de precisión doble siguen requisitos de comportamiento de IEEE 754R.

Se requiere compatibilidad con la generación de valores no normalizados para los datos de precisión doble (ningún comportamiento de vaciado a cero). De igual modo, la instrucciones no leen datos desnormalizados como un cero con signo, sino que respetan el valor de numero no normalizado.

## <a name="span-idalpha16bitspanspan-idalpha16bitspan16-bit-floating-point-rules"></a><span id="alpha_16_bit"></span><span id="ALPHA_16_BIT"></span>reglas de punto flotante de 16 bits


Direct3D también admite representaciones de 16 bits de números de punto flotante.

Formato:

-   1 bit de signo (s) en la posición de bit MSB
-   5 bits del exponente sesgado (e)
-   10 bits de fracción (f), con un bit oculto adicional

Un valor de tipo float16 (v) sigue estas reglas:

-   Si e == 31 y f != 0, v es NaN independientemente de s
-   Si e == 31 y f == 0, a continuación, v = (-1) s\*infinity (infinito con signo)
-   Si e es entre 0 y 31, v = (-1) s\*2(e-15)\*(1.f)
-   Si e == 0 y f! = 0, v = (-1) s\*2(e-14)\*(0.f) (números desnormalizados)
-   Si e == 0 y f == 0, a continuación, v = (-1) s\*0 (cero con signo)

Las reglas de punto flotante de 32 bits también incluyen números de punto flotante de 16 bits, ajustados para el diseño de bits que se ha descrito anteriormente. Las excepciones son:

-   Precisión: Operaciones unfused en números de punto flotante de 16 bits producen un resultado que es el más próximo al valor que se puede representar a un resultado preciso infinitamente (ida a incluso más cercano, por IEEE-754, que se aplica a los valores de 16 bits). Las reglas de punto flotante de 32 bits son conformes a la tolerancia de 1 ULP, y las reglas de punto flotante de 16 bits son conformes a 0.5 ULP para las operaciones no fusionadas y a 0.6 ULP para las operaciones fusionadas.
-   Los números de punto flotante de 16 bits conservan números no normalizados.

## <a name="span-idalpha11bitspanspan-idalpha11bitspan11-bit-and-10-bit-floating-point-rules"></a><span id="alpha_11_bit"></span><span id="ALPHA_11_BIT"></span>reglas de punto flotante de 11 y 10 bits


Direct3D también admite formatos de punto flotante de 11 y 10 bits.

Formato:

-   Bit sin signo
-   5 bits del exponente sesgado (e)
-   6 de bits de fracción (f) para un formato de 11 bits, 5 bits de fracción (f) para un formato de 10 bits, con un bit oculto adicional en ambos casos.

Un valor de float11/float10 (v) sigue las siguientes reglas:

-   Si e == 31 y f != 0, v es NaN
-   Si e == 31 y f == 0, v = +infinito
-   Si e es entre 0 y 31, v, a continuación, = 2(e-15)\*(1.f)
-   Si e == 0 y f! = 0, v = \*2(e-14)\*(0.f) (números desnormalizados)
-   Si e == 0 y f == 0, v = 0 (cero)

Las reglas de punto flotante de 32 bits también incluyen números de punto flotante de 11 y 10 bits, ajustados para el diseño de bits que se ha descrito anteriormente. Algunas excepciones son:

-   Precisión: 0,5 ULP cumplen las reglas de punto flotante de 32 bits.
-   Los números de punto flotante de 11 y 10 bits conservan números no normalizados.
-   Cualquier operación que podría dar lugar a un número menor que cero se restringe en cero.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Apéndices](appendix.md)

[Recursos](https://msdn.microsoft.com/library/windows/desktop/ff476894)

[Textures](https://msdn.microsoft.com/library/windows/desktop/ff476902)

 

 




