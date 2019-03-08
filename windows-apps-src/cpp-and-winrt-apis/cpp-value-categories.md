---
description: En este tema se describe las distintas categorías de valores que existen en C++. Sin duda habrá oído hablar de lvalues y rvalues, pero hay otros tipos.
title: Categorías de valor y las referencias a ellos
ms.date: 08/11/2018
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, mover, reenvío, categorías de valor, semántica de movimiento, reenvío directo, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593020"
---
# <a name="value-categories-and-references-to-them"></a>Categorías de valor y las referencias a ellos
En este tema se describe las distintas categorías de valores (y las referencias a valores) que existen en C++. Sin duda habrá oído hablar de *lvalues* y *rvalues*, pero es posible que no piensa en ellos en los términos de este tema se presenta. Y hay otros tipos de valores.

Todas las expresiones de C++, produce un valor que pertenece a una de las categorías que se tratan en este tema. Hay aspectos del lenguaje C++, su facilies y reglas, que exigen una comprensión apropiada de estas categorías de valor y las referencias a ellos. Por ejemplo, tomar la dirección de un valor, copiar un valor, pasar un valor y un valor a otra función de reenvío. Este tema no entra en todos esos aspectos en profundidad, pero proporciona información básica para una comprensión sólida de ellos.

La información de este tema se encuadra en términos de análisis de Stroustrup de categorías de valor en las dos propiedades independientes de la identidad y movability [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Un valor l tiene identidad
¿Qué significa tener un valor *identidad*? Si tiene la dirección de memoria de un valor (o puede tomar) y su uso con la seguridad y, a continuación, el valor tiene identidad. De este modo, se puede hacer más que compare el contenido de los valores: puede comparar o distinguirlas por su identidad.

Un *lvalue* tiene identidad. Ahora es una cuestión de interés histórico solo que la "l" de "valor l" es una abreviatura de "left" (como en el izquierdo lado de una asignación). En C++, puede aparecer un valor l a la izquierda *o* a la derecha de una asignación. La "l" de "valores l", a continuación, no realmente le ayuda a comprender ni definir qué son. Solo tendrá que entender que lo que llamamos un valor l es un valor que tiene identidad.

Ejemplos de expresiones que son valores l: una variable con nombre o una constante; o una función que devuelve una referencia. Ejemplos de expresiones que son *no* incluye valores l: un archivo temporal; o una función que devuelve por valor.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    std::vector<byte> vec{ 99, 98, 97 };
    std::vector<byte>* addr1{ &vec }; // ok: vec is an lvalue.
    int* addr2{ &get_by_ref() }; // ok: get_by_ref() is an lvalue.

    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is not an lvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is not an lvalue.
}
```

Ahora, si bien es una instrucción verdadera que valores l tiene identidad, también lo hacen xvalues. Vamos a más en lo que un *xvalue* es más adelante en este tema. Por ahora, simplemente tenga en cuenta que hay una categoría de valor denominada glvalue, para "generalizado lvalue". El supraconjunto de glvalues contiene ambos valores l (también conocido como *lvalues clásicas*) y xvalues. Por lo tanto, mientras que "identidad de tiene un valor l" es true, el conjunto completo de las cosas que tienen identidad es el conjunto de glvalues, tal como se muestra en esta ilustración.

![Un valor l tiene identidad](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Es móvil; un valor r un valor l no es
Sin embargo, hay valores que no son glvalues. Por lo tanto, hay valores que usted *no* obtener una dirección de memoria para (o no puede depender de que sea válido). Vimos que algunos de estos valores en el ejemplo de código anterior. Esto parece una desventaja. Pero en realidad la ventaja de un valor como este es el que se puede *mover* (lo que es normalmente barato), en lugar de copia de él (que es costoso por lo general). Pasar de un valor significa que ya no está en el lugar en que solía ser. Por lo tanto, intenta obtener acceso a él en el lugar en que solía ser es algo que deben evitarse. Una explicación de cuándo y *cómo* para mover un valor está fuera del ámbito de este tema. Este tema, nos basta con saber que se denomina un valor que se puede mover un *rvalue* (o *rvalue clásica*).

La "r" de "rvalue" es una abreviatura de "right" (como en el derecho lado de una asignación). Pero puede usar valores r y las referencias a valores r, fuera de las asignaciones. La "r" de "valores r", a continuación, no es lo centrarse. Solo tendrá que entender que lo que llamamos un valor r es un valor que se puede mover.

Un valor l, por el contrario, no puede mover, tal como se muestra en esta ilustración. Un valor l que movió sería Enfréntese a la definición de *lvalue*, y sería un problema inesperado para el código que espera muy razonable para poder seguir teniendo acceso a lvalue.

![Es móvil; un valor r un valor l no es](images/is-movable.png)

No se puede mover un valor l. Sin embargo, existen *es* una especie de glvalue (el conjunto de las cosas con identidad) que se puede mover&mdash;si sabe lo que está haciendo (incluido con cuidado de no tener acceso a él después del desplazamiento)&mdash;y que es el xvalue. Volveremos a esta idea una vez más a continuación, cuando nos centramos en la imagen completa de las categorías de valor.

## <a name="rvalue-references-and-reference-binding-rules"></a>Las referencias rvalue y reglas de enlace de referencia
Esta sección presenta la sintaxis para una referencia a un valor r. Tendremos que esperar otro tema a entrar en un tratamiento sustancial de mover y reenvío, pero son problemas que resolver por las referencias rvalue. Antes de adentrarnos en las referencias rvalue, sin embargo, primero es necesario ser más claro sobre `T&` &mdash;lo que nos hemos anteriormente ha estado llamando solo "una referencia". Es realmente "una (distinta de const) referencia lvalue", que hace referencia a un valor al que se puede escribir el usuario de la referencia.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Puede enlazar una referencia lvalue a un valor l, pero no a un valor r.

A continuación, hay referencias constantes lvalue (`T const&`), que hacen referencia a objetos a los que el usuario de la referencia *no* escritura (por ejemplo, una constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Una referencia const lvalue puede enlazar a un valor l o un valor r.

La sintaxis para una referencia a un valor r del tipo `T` se escribe como `T&&`. Una referencia rvalue hace referencia a un valor de bienes muebles&mdash;un valor cuyo contenido no es necesario conservar una vez que hemos usado lo (por ejemplo, un archivo temporal). Desde el objetivo es mover de (con lo que modificar) el valor enlazado a una referencia rvalue, `const` y `volatile` calificadores (también conocido como calificadores cv) no se aplican a las referencias rvalue.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Una referencia rvalue se enlaza a un valor r. De hecho, en términos de la resolución de sobrecarga, un valor r *prefiere* enlazar una referencia rvalue que a una referencia const lvalue. Pero una referencia rvalue no se puede enlazar a un valor l porque, como ya habíamos dicho, una referencia rvalue hace referencia a un valor cuyo contenido se supone que no es necesario conservar (por ejemplo, el parámetro para un constructor de movimiento).

También puede pasar un valor r donde se esperaba un argumento por valor, a través de la construcción de copia (o a través de la construcción de movimiento, si el valor de r es un xvalue).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Un glvalue tiene identidad; no es así un prvalue
En esta fase, sabemos lo que tiene identidad. Y sabemos qué es móvil y qué no. Pero no hemos aún denominamos el conjunto de valores que *no* tienen identidad. Este conjunto se conoce como el *prvalue*, o *rvalue puro*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Un valor l tiene identidad; no es así un prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>La imagen completa de las categorías de valor
Sólo se guardan combinar la información y las ilustraciones anteriormente en una sola imagen ampliada.

![La imagen completa de las categorías de valor](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Un glvalue (generalizado lvalue) tiene identidad.

### <a name="lvalue-im"></a>valor l (\&\!m)
Un valor l (un tipo de glvalue) tiene identidad, pero no puede mover. Estos son los valores normalmente de lectura y escritura que se pasan en torno a por referencia o referencia constante o valor si es barato copiar. No se puede enlazar un valor l en una referencia rvalue.

### <a name="xvalue-im"></a>xValue (\&m)
Un xvalue (una especie de glvalue, pero también un tipo de valor r) tiene identidad y también puede mover. Esto podría ser un lvalue erstwhile que ha decidido mover porque la copia es costosa y podrá cuidado de no tener acceso a él posteriormente. Aquí es cómo puede convertir un valor l en una xvalue.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

En el ejemplo de código anterior, no hemos pasamos nada todavía. Simplemente creamos una xvalue al convertir un valor de l en una referencia rvalue sin nombre. Todavía puede identificarse por su nombre de valor l; pero, como un xvalue, ahora es *capaz* de que se va a mover. Los motivos para hacerlo, y qué mover aspecto realmente, tendrá que esperar otro tema. Pero se puede considerar la "x" en "xvalue" significado "experto sólo" si ayuda a. Al convertir un valor l en una xvalue (un tipo de valor r), el valor, a continuación, pasa a ser capaz de que se enlaza a una referencia rvalue.

Estos son dos otros ejemplos de xvalues&mdash;llamar a una función que devuelve una referencia rvalue sin nombre y el acceso a un miembro de un xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!\&m)
Un prvalue (rvalue pura; un tipo de valor r) no tiene identidad, pero es móvil. Estos suelen ser objetos temporales, el resultado de llamar a una función que devuelve por valor o el resultado de evaluar cualquier otra expresión que no sea un glvalue,

### <a name="rvalue-m"></a>valor de r (m)
Un valor r es móvil. Un valor r *referencia* siempre hace referencia a un valor r (un valor cuyo contenido se supone que no es necesario conservar).

¿Sin embargo, es una referencia rvalue propio un valor r? Un *sin nombre* referencia rvalue (como las que se muestra en los ejemplos de código xvalue anteriores) es un xvalue por lo tanto, sí, es un valor r. Prefiere a enlazarse a un parámetro de función de referencia rvalue, como los de un constructor de movimiento. A la inversa (y quizás counter-intuitively), si una referencia rvalue tiene un nombre, la expresión formada por ese nombre es un valor l. Por lo que lo *no* enlazarse a un parámetro de referencia rvalue. Pero es fácil hacerlo hacerlo&mdash;simplemente convertirlo en una referencia de valor r sin nombre (un xvalue) nuevo.

```cppwinrt
void foo(A&) { ... }
void foo(A&&) { ... }
void bar(A&& a) // a is a named rvalue reference; it's an lvalue.
{
    foo(a); // Calls foo(A&).
    foo(static_cast<A&&>(a)); // Calls foo(A&&).
}
A&& get_by_rvalue_ref() { ... } // This unnamed rvalue reference is an xvalue.
```

### <a name="im"></a>\!i\&\!m
El tipo de valor que no tiene la identidad y no puede mover es la combinación de uno que no se han analizado. Pero nos podemos pasarlo por alto, porque esa categoría no es una idea útil del lenguaje C++.

## <a name="reference-collapsing-rules"></a>Reglas de contracción de referencia
Varias referencias like en una expresión (una referencia lvalue a una referencia lvalue o una referencia rvalue en una referencia rvalue) cancelación una salida de otro.

- `A& &` contrae en `A&`.
- `A&& &&` contrae en `A&&`.

Contraer varias a diferencia de las referencias en una expresión a una referencia lvalue.

- `A& &&` contrae en `A&`.
- `A&& &` contrae en `A&`.

## <a name="forwarding-references"></a>Referencias de reenvío
Esta última sección contrasta las referencias rvalue, que ya se ha explicado, con el concepto diferentes de un *reenvío referencia*.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` es una referencia rvalue, como hemos visto. Const y volatile no son aplicables a las referencias rvalue.
- `foo` acepta solo los valores de tipo **A**.
- Referencias rvalue razón (como `A&&`) existe es por lo que puede crear una sobrecarga que está optimizada para el caso de un archivo temporal (o en otro rvalue) que se pasa.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` es un *reenvío referencia*. Dependiendo de lo pasará a `bar`, tipo **_Ty** podría ser const/no constante independientemente volátil, no-volátil.
- `bar` acepta cualquier lvalue o rvalue de tipo **_Ty**.
- Si se pasa un valor l, la referencia de reenvío para convertirse en `_Ty& &&`, que se contrae a la referencia lvalue `_Ty&`.
- Si se pasa un valor r, la referencia de reenvío para convertirse en `_Ty&& &&`, que se contrae en la referencia rvalue `_Ty&&`.
- El motivo por el reenvío de las referencias (como `_Ty&&`) existe es *no* para la optimización, pero para aprovechar lo pasará a ellas y reenviarlo en forma transparente y eficaz. Es probable que encuentre una referencia de reenvío sólo si escribir (o estrechamente estudiar) código de biblioteca&mdash;por ejemplo, una función de generador que reenvía los argumentos de constructor.

## <a name="sources"></a>Orígenes
* \[Stroustrup, 2013\] Stroustrup B.: El lenguaje de programación de C++, cuarta edición. Addison-Wesley. 2013.
