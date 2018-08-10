---
author: stevewhims
description: En este tema se describe las distintas categorías de los valores que existen en C++. Sin duda habrá oído hablar de valores l y valores r, pero existen otros tipos.
title: Categorías de valor
ms.author: stwhi
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, mover, transferencia, categorías de valor, la semántica de movimientos, reenvío perfecto, valor l, valor r, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.openlocfilehash: 75176334909e0e6bcf81763b543a19b70a4cba73
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2018
ms.locfileid: "2739116"
---
# <a name="value-categories"></a>Categorías de valor
En este tema se describe las distintas categorías de los valores que existen en C++. Sin duda habrá oído hablar de *valores l* y *valores r*, pero existen otros tipos. Todas las expresiones en C++ produzca como resultado un valor que pertenece a una de las categorías que se tratan en este tema. Hay aspectos del lenguaje C++, su facilies y reglas, que exigen una correcta comprensión de las categorías de valor.

## <a name="an-lvalue-has-identity"></a>Un valor l tiene identidad
¿Qué significa un valor para tienen *identidad*? Si tiene la dirección de memoria de un valor (o que se pueden realizar), el valor tiene identidad. De este modo, puede hacer más que compare el contenido de los valores: puede comparar o distinguirlos por identity.

Un *valor l* tiene identidad. Ahora es una cuestión de interés sólo histórica que "l" en el valor "l" es una abreviatura de "left" (como en la izquierda lado de una asignación). En C++, un valor l puede aparecer en la izquierda *o* a la derecha de una asignación. "L" en valores "l", a continuación, no realmente le ayuda a comprender ni definir qué son. Sólo necesita comprender que lo que llamamos un valor l es un valor que tiene la identidad.

Ahora, si bien es una instrucción true que valores l tiene identidad, también lo hacen xvalues. Abordaremos un poco más en qué un *xvalue* es más adelante en este tema (aunque, un tratamiento completo de ellos no está en el ámbito aquí). Por ahora, sólo tenga en cuenta que hay una categoría de valor denominada glvalue, para "generalizado valor l". El superconjunto de glvalues contiene valores l (también conocido como "clásico valores l") y xvalues. Por lo tanto, mientras "un valor l tiene identidad" es true, el conjunto completo de las cosas que tienen identidad es el conjunto de glvalues, como se muestra en esta ilustración.

![Un valor l tiene identidad](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Un valor r es móvil; un valor l no es
Pero hay valores que no están glvalues. Por consiguiente, hay valores que *no se puede* obtener una dirección de memoria para (o no puede confiar en que sea válido). Qué suena como un inconveniente. Pero en realidad la ventaja de un valor que es decir que se puede *mover* (lo que es generalmente barato), en lugar de copia TI (que es resulta caro). Mover un valor significa que ya no está en el lugar que solía ser. Por lo tanto, intenta obtener acceso a él en el lugar que solía ser es algo que deben evitarse. Una explicación de cuándo y *cómo* para mover que un valor está fuera del ámbito de este tema. Para este tema, sólo es necesario saber que un valor que es móvil se conoce como un *valor r*.

La "r" en el valor "r" es una abreviatura de "derecha" (como en el derecho lado de una asignación). Pero se pueden usar valores r y las referencias a valores r, fuera de las asignaciones. La "r" en valores "r", a continuación, no es lo que debe centrarse en. Sólo necesita comprender que lo que llamamos un valor r es un valor que es móvil.

Un valor l, por el contrario, no es móvil, como se muestra en esta ilustración. No se puede mover un valor l debido a que, si es posible, a continuación, sería no seguras (o incluso desastroso) para continuar obtener acceso a él más adelante. Recuerde que tienen su identidad.

![Un valor r es móvil; un valor l no es](images/is-movable.png)

No se puede mover un valor l. Sin embargo, existen *es* un tipo de glvalue (el conjunto de cosas con identidad) que se puede mover&mdash;si sabe lo que está haciendo&mdash;y que es la xvalue. Podrá revisar esta idea una vez más más adelante, cuando nos fijamos en la imagen completa de las categorías de valor.

## <a name="an-lvalue-has-identity-a-prvalue-does-not"></a>Un valor l tiene identidad; no lo hace un prvalue
En esta etapa, sabemos lo que tiene la identidad. Y sabemos qué es móvil y qué no lo es. Pero no disponemos todavía el conjunto de valores con nombre que *no* tienen identidad. Ese conjunto se conoce como la *prvalue*o "valor r puro".

![Un valor l tiene identidad; no lo hace un prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>La imagen completa de las categorías de valor
Sólo se guardan combinar la información e ilustraciones encima en una único de la imagen grande.

![La imagen completa de las categorías de valor](images/value-categories.png)

- Un glvalue (valor l generalizado) tiene la identidad.
- Un valor l (un tipo de glvalue) tiene identidad, pero no es móvil. Estos son los valores normalmente de lectura y escritura que se pasen alrededor de referencia o referencia const o valor si la copia es barato.
- Un xvalue (una especie de glvalue, pero también un tipo de valor r) tiene la identidad y también es móvil. Esto puede resultar un valor l erstwhile que se ha decidido a mover porque copiar es costosa y se cuidado de no tener acceso a él más adelante. La forma en que un valor l para convertir en un xvalue y las razones para mover uno, tendrá que esperar para otro tema. Pero se puede considerar de la "x" en "xvalue" significado "experto" si sirve de ayuda.
- Un prvalue (valor r pura; un tipo de valor r) no tiene identidad, pero es móvil. Normalmente, estos son los literales, temporales, devolver valores&mdash;todo lo que es el resultado de la evaluación de una expresión (es decir, una expresión que no es un glvalue), o cualquier elemento devuelto por el valor de una función.
- Un valor r es móvil.
