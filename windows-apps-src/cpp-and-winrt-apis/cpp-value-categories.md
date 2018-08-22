---
author: stevewhims
description: En este tema se describe las distintas categorías de los valores que existen en C++. Sin duda habrá oído hablar de valores l y valores r, pero existen otros tipos.
title: Categorías de valor y las referencias a ellos
ms.author: stwhi
ms.date: 08/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, mover, transferencia, categorías de valor, la semántica de movimientos, reenvío perfecto, valor l, valor r, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.openlocfilehash: cbccaf78b45d85d93619977d149431c4eec9e10a
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2796952"
---
# <a name="value-categories-and-references-to-them"></a>Categorías de valor y las referencias a ellos
En este tema se describe las distintas categorías de valores (y las referencias a valores) que existen en C++. Sin duda habrá oído hablar de *valores l* y *valores r*, pero no puede pensar de ellos en los términos que presenta este tema. Y hay otros tipos de valores.

Todas las expresiones en C++ produzca como resultado un valor que pertenece a una de las categorías que se tratan en este tema. Hay aspectos del lenguaje C++, su facilies y reglas, que exigen una correcta comprensión de estas categorías de valor y las referencias a ellos. Por ejemplo, tomar la dirección de un valor, copiar un valor, mover un valor y un valor a otra función de transferencia. En este tema no entra en todos los aspectos en profundidad, pero proporciona información fundamental para una sólida comprensión de ellos.

La información de este tema enmarcada en términos de análisis de Stroustrup de categorías de valor en las dos propiedades independientes de la identidad y movability [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Un valor l tiene identidad
¿Qué significa un valor para tienen *identidad*? Si tiene la dirección de memoria de un valor (o que se pueden realizar) y usarlo con la seguridad y, a continuación, el valor tiene identidad. De este modo, puede hacer más que compare el contenido de los valores: puede comparar o distinguirlos por identity.

Un *valor l* tiene identidad. Ahora es una cuestión de interés sólo histórica que "l" en el valor "l" es una abreviatura de "left" (como en la izquierda lado de una asignación). En C++, un valor l puede aparecer en la izquierda *o* a la derecha de una asignación. "L" en valores "l", a continuación, no realmente le ayuda a comprender ni definir qué son. Sólo necesita comprender que lo que llamamos un valor l es un valor que tiene la identidad.

Ejemplos de expresiones que son valores l: una variable con nombre o una constante; o bien, una función que devuelve una referencia. Algunos ejemplos de expresiones que son valores *no* l son: un archivo temporal; o bien, una función que devuelve por valor.

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

Ahora, si bien es una instrucción true que valores l tiene identidad, también lo hacen xvalues. Abordaremos más en qué un *xvalue* es más adelante en este tema. Por ahora, sólo tenga en cuenta que hay una categoría de valor denominada glvalue, para "generalizado valor l". El superconjunto de glvalues contiene valores l (también conocido como *valores l clásica*) y xvalues. Por lo tanto, mientras "un valor l tiene identidad" es true, el conjunto completo de las cosas que tienen identidad es el conjunto de glvalues, como se muestra en esta ilustración.

![Un valor l tiene identidad](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Un valor r es móvil; un valor l no es
Pero hay valores que no están glvalues. Por consiguiente, hay valores que *no se puede* obtener una dirección de memoria para (o no puede confiar en que sea válido). Se han visto algunas dichos valores en el ejemplo de código anterior. Esto le suena un inconveniente. Pero en realidad la ventaja de un valor que es decir que se puede *mover* de él (que es generalmente barato), en lugar de copia de él (que es resulta caro). Desplazamiento de un valor significa que ya no está en el lugar que solía ser. Por lo tanto, intenta obtener acceso a él en el lugar que solía ser es algo que deben evitarse. Una explicación de cuándo y *cómo* para mover que un valor está fuera del ámbito de este tema. Para este tema, sólo es necesario saber que un valor que es móvil se conoce como un *valor r* (o *valor r clásica*).

La "r" en el valor "r" es una abreviatura de "derecha" (como en el derecho lado de una asignación). Pero se pueden usar valores r y las referencias a valores r, fuera de las asignaciones. La "r" en valores "r", a continuación, no es lo que debe centrarse en. Sólo necesita comprender que lo que llamamos un valor r es un valor que es móvil.

Un valor l, por el contrario, no es móvil, como se muestra en esta ilustración. Un valor l que se haya movido sería Enfréntese a la definición de *valor l*y sería un problema inesperado para código que muy razonablemente que puedan seguir teniendo acceso al valor l.

![Un valor r es móvil; un valor l no es](images/is-movable.png)

No se puede mover un valor l. Sin embargo, existen *es* un tipo de glvalue (el conjunto de cosas con identidad) que se puede mover&mdash;si sabe lo que está haciendo (incluidos con cuidado de no tener acceso a él tras el traslado)&mdash;y que es la xvalue. Podrá revisar esta idea una vez más más adelante, cuando nos fijamos en la imagen completa de las categorías de valor.

## <a name="rvalue-references-and-reference-binding-rules"></a>Referencias de valores r y reglas de enlace de referencia
En esta sección se presenta la sintaxis de una referencia a un valor r. Tenemos espere a que otro tema ir a un tratamiento sustancial de mover y transferencia, pero esos son los problemas que se resuelven por referencias de valores r. Antes de estudiar las referencias de valores r, sin embargo, primero es necesario ser más clara acerca de `T&` &mdash;lo que nos hemos anteriormente se han llamar a sólo "una referencia". Realmente es "una valor l (que no sean const) referencia", que hace referencia a un valor en el que puede escribir el usuario de la referencia.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Una referencia de valor l se puede enlazar a un valor l, pero no a un valor r.

A continuación, hay un valor l constantes referencias (`T const&`), que hacen referencia a objetos en el que el usuario de la referencia *no se puede* escribir (por ejemplo, una constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Una referencia const valor l puede enlazar a un valor l o a un valor r.

La sintaxis de una referencia a un valor r de tipo `T` está escrito como `T&&`. Una referencia de valor r hace referencia a un valor móvil&mdash;un valor cuyo contenido no es necesario conservar una vez que hemos usado lo (por ejemplo, un archivo temporal). Desde el punto de todo para mover de (con lo que modificar) el valor depende de una referencia a valor r, `const` y `volatile` calificadores (también conocido como calificadores cv) no se aplican a referencias de valores r.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Una referencia de valor r se enlaza a un valor r. De hecho, en términos de resolución de sobrecarga, un valor r *prefiere* se enlaza a una referencia de valor r que para una referencia const de valor l. Pero una referencia de valor r no se puede enlazar a un valor l porque, tal y como hemos dicho, una referencia de valor r hace referencia a un valor cuyo contenido se supone que no es necesario conservar (por ejemplo, el parámetro de un constructor de movimiento).

También se puede pasar un valor r donde se espera un argumento por valor, a través de construcción de copia (o a través de construcción de movimiento, si el valor de r es un xvalue).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Un glvalue tiene identidad; no lo hace un prvalue
En esta etapa, sabemos lo que tiene la identidad. Y sabemos qué es móvil y qué no lo es. Pero no disponemos todavía el conjunto de valores con nombre que *no* tienen identidad. Ese conjunto se conoce como la *prvalue*o *valor r pura*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Un valor l tiene identidad; no lo hace un prvalue](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>La imagen completa de las categorías de valor
Sólo se guardan combinar la información e ilustraciones encima en una único de la imagen grande.

![La imagen completa de las categorías de valor](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Un glvalue (valor l generalizado) tiene la identidad.

### <a name="lvalue-im"></a>valor l (i\ & \!m)
Un valor l (un tipo de glvalue) tiene identidad, pero no es móvil. Estos son los valores normalmente de lectura y escritura que se pasen alrededor de referencia o referencia const o valor si la copia es barato. No se puede enlazar un valor l a una referencia de valor r.

### <a name="xvalue-im"></a>xValue (i\ & m)
Un xvalue (una especie de glvalue, pero también un tipo de valor r) tiene la identidad y también es móvil. Esto puede resultar un valor l erstwhile que se ha decidido a mover porque copiar es costosa y se cuidado de no tener acceso a él más adelante. Aquí es cómo puede convertir un valor l en un xvalue.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

En el ejemplo de código anterior, no hemos pasamos nada todavía. Simplemente hemos creado una xvalue convirtiendo un valor de l en una referencia a valor r sin nombre. Aún pueden identificarse por su nombre de valor l; pero, como un xvalue, ahora es *capaz* de va a mover. Las razones para hacerlo, y qué mover realmente el aspecto, tendrá que esperar para otro tema. Pero puede considerar la "x" en "xvalue" como significado "experto"sólo si sirve de ayuda. Convirtiendo un valor l a un xvalue (un tipo de valor r), el valor, a continuación, pasa a ser capaz de estar enlazado a una referencia de valor r.

Estos son dos otros ejemplos de xvalues&mdash;llama a una función que devuelve una referencia de valor r sin nombre y obtener acceso a un miembro de un xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
Un prvalue (valor r pura; un tipo de valor r) no tiene identidad, pero es móvil. Normalmente, estos son temporales, el resultado de una llamada a una función que devuelve por valor, o el resultado de la evaluación de cualquier otra expresión que no es un glvalue

### <a name="rvalue-m"></a>valor r (m)
Un valor r es móvil. Un valor r *referencia* siempre hace referencia a un valor r (un valor cuyo contenido se supone que no es necesario conservar).

¿Sin embargo, es una referencia de valor r: sí mismo un valor r? Una referencia a valor r *sin nombre* (como las que se muestra en los ejemplos de código de xvalue anteriores) es un xvalue por lo tanto, sí, es un valor r. Prefiere a estar enlazada a un parámetro de la función de referencia de valor r, como el de un constructor de movimiento. Por el contrario (y, posiblemente, counter-intuitively), si una referencia de valor r tiene un nombre, entonces la expresión formada por ese nombre es un valor l. Por lo tanto, *no se* puede enlazar a un parámetro de referencia de valor r. Pero es fácil que lleve a cabo lo&mdash;acaba convertirlo en una referencia a valor r sin nombre (un xvalue) nuevo.

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

### <a name="im"></a>\!i\ & \!m
El tipo de valor que no tiene la identidad y no es móvil es la combinación de uno que aún no hemos analizado. Pero nos podemos pasar por alto, debido a que esa categoría no es una idea útil en el lenguaje C++.

## <a name="reference-collapsing-rules"></a>Reglas de contracción de referencia
Varias referencias like en una expresión (una referencia de valor l a una referencia de valor l, o una referencia de valor r a una referencia de valor r) cancelación una espera otra.

- `A& &` contrae en `A&`.
- `A&& &&` contrae en `A&&`.

Contraer varios a diferencia de referencias en una expresión a una referencia de valor l.

- `A& &&` contrae en `A&`.
- `A&& &` contrae en `A&`.

## <a name="forwarding-references"></a>Referencias de desvío de llamadas
En esta última sección contrasta referencias de valores r, que ya hemos analizado, con el concepto de una *referencia de desvío*diferente.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` es una referencia a valor r, como hemos visto. Const y volatile no se aplican a referencias de valores r.
- `foo` acepta sólo valores r de tipo **A**.
- El valor de r motivo hace referencia (como `A&&`) existe es de modo que puede crear una sobrecarga que está optimizada para el caso de un archivo temporal (o en otros valores de r) que se transfieren.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` es una *referencia de reenvío*. Dependiendo de lo que se pasa a `bar`, tipo **_Ty** podría ser const/no const independientemente volátiles o no volátiles.
- `bar` acepta cualquier valor l o valor r de tipo **_Ty**.
- Pasar un valor l hace que la referencia de desvío de llamadas se convierta en `_Ty& &&`, que se contrae en la referencia de valor l `_Ty&`.
- Pasar un valor r hace que la referencia de desvío de llamadas se convierta en `_Ty&& &&`, que se contrae en la referencia de valor r `_Ty&&`.
- La razón por la transferencia referencias (tales como `_Ty&&`) existe es *no* para la optimización, pero para realizar lo que se pasa en los mismos y reenviarla en forma transparente y eficaz. Es probable que se encuentre una referencia de desvío sólo si escribir (o estudiar estrechamente) código de biblioteca de&mdash;por ejemplo, una función de fábrica que reenvía en argumentos de constructor.

## <a name="sources"></a>Orígenes
* \[Stroustrup, 2013\] B. Stroustrup: el lenguaje de programación C++, cuarta edición. Addison-Wesley. 2013.
