---
description: En este tema se describe las distintas categorías de valores que existen en C++. Sin duda habrá oído hablar de valores l y valores r, pero hay otros tipos.
title: Categorías de valor y referencias a ellos
ms.date: 08/11/2018
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, movimiento, reenvío, categorías de valor, semántica de movimiento, reenvío perfecto, valor l, valor r, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8794588"
---
# <a name="value-categories-and-references-to-them"></a>Categorías de valor y referencias a ellos
En este tema se describe las distintas categorías de valores (y las referencias a valores) que existen en C++. Sin duda habrá oído hablar de *valores l* y *valores r*, pero no puede pensar de ellos en términos que presenta de este tema. Y hay otros tipos de valores.

Cada expresión en C++ produce un valor que pertenece a una de las categorías que se tratan en este tema. Hay aspectos del lenguaje C++, su facilies y reglas, que exigen un conocimiento correcta de estas categorías de valor y referencias a ellos. Por ejemplo, dando la dirección de un valor de un valor de copiar, mover un valor y reenvío de un valor en función de otra. En este tema no entra en todos los aspectos en profundidad, pero proporciona información básica sobre información para obtener una descripción sólida de ellos.

La información de este tema es de trama en términos de análisis de Stroustrup de categorías de valor de las dos propiedades independientes de la identidad y movability [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Un valor l tiene identidad
¿Qué significa para que un valor tener *identidad*? Si tienes (o emprender) la dirección de memoria de un valor y usar de forma segura, a continuación, el valor tiene identidad. De este modo, puede hacer algo más que comparar el contenido de valores: puedes comparar o distinguirlas por su identidad.

Un *valor l* tiene identidad. Ahora es una cuestión de interés solo histórica que el "l" en "valor"l es una abreviatura de "izquierda" (como en la izquierda lado de una asignación). En C++, un valor l puede aparecer en la izquierda *o* la derecha de una asignación. El "l" en "valores l", a continuación, no realmente te ayuda a comprender ni definir qué son. Solo necesitas comprender que lo llamamos a un valor l es un valor que tiene la identidad.

Ejemplos de expresiones que son valores l: una variable con nombre o constante; o bien, una función que devuelve una referencia. Ejemplos de expresiones que son valores *no* l: un temporal; o una función que devuelva por valor.

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

Ahora, mientras que es una instrucción true que valores l tiene identidades, por lo tanto, hacer xvalues. Vaya más en qué una *xvalue* es más adelante en este tema. Por ahora, simplemente Ten en cuenta que hay una categoría de valor que glvalue, se denomina "generalizadas valor l". El superconjunto de glvalues contiene valores l (también conocida como *valores l clásicos*) y xvalues. Por lo tanto, mientras que "un valor l tiene identidad" es true, el conjunto completo de las cosas que tienen identidad es el conjunto de glvalues, como se muestra en la siguiente ilustración.

![Un valor l tiene identidad](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Un valor r es móvil; un valor l no es
Sin embargo, hay valores que no son glvalues. Por lo tanto, hay valores que *no se puede* obtener una dirección de memoria para (o no puedes depender para que sea válido). Observamos que algunos de estos valores en el ejemplo de código anterior. Esto suena como un inconveniente. Pero en realidad la ventaja de un valor que es decir que se puede *mover* de ella (que es barata generalmente), en lugar de copia de él (que es resulta caro). Pasar de un valor significa que ya no está en el lugar eran. Por lo tanto, al intentar acceder a ella en el lugar eran es algo que deben evitarse. Una explicación de cuándo y *cómo* mover que un valor está fuera del ámbito de este tema. Este tema, al igual que necesitamos saber que un valor que es móvil se conoce como un *valor r* (o *valores de r clásicos*).

La "r" en "valor"r es una abreviatura de "derecha" (como en el derecho lado de una asignación). Pero puedes usar valores r y referencias a valores de r, fuera de las asignaciones. La "r" de valores "r", a continuación, no es lo que se centran en. Solo necesitas comprender que lo llamamos a un valor r es un valor que es móvil.

Un valor de l, por el contrario, no es móvil, como se muestra en la siguiente ilustración. Un valor de l que podría Enfréntese a la definición de *valor l*y sería un problema inesperado código que muy razonablemente para que puedan continuar teniendo acceso al valor de l.

![Un valor r es móvil; un valor l no es](images/is-movable.png)

No puedes mover un valor l. Sin embargo, existen *es* un tipo de glvalue (el conjunto de cosas con identidad) que se puede mover&mdash;Si sabes que realizas (incluidos con cuidado de no tener acceso a ella después del movimiento)&mdash;y que es la xvalue. Tendrás revisar esta idea una vez más a continuación, cuando echaremos un vistazo a la imagen completa de las categorías de valor.

## <a name="rvalue-references-and-reference-binding-rules"></a>Referencias de valores r y las reglas de enlace de referencia
Esta sección presenta la sintaxis de una referencia a un valor r. Tendremos que espera a que otro tema ir a un tratamiento considerable de mover y reenviar, pero estos son los problemas que se resuelven las referencias de valor r. Antes de echaremos un vistazo a las referencias de valor r, sin embargo, primero tenemos para que sea más claro sobre `T&` &mdash;lo que nos hemos anteriormente se han una llamada a simplemente "una referencia". Realmente es "una valor l (no const) referencia", que hace referencia a un valor en el que puede escribir el usuario de la referencia.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Una referencia de valor l puede enlazar a un valor l, pero no en un valor r.

A continuación, hay referencias const de valor l (`T const&`), lo que hacen referencia a objetos en el que el usuario de la referencia *no* se escriben (por ejemplo, una constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Una referencia de valor l const puede enlazar a un valor l o a un valor r.

La sintaxis de una referencia a un valor r de tipo `T` se escribe como `T&&`. Una referencia de valor r hace referencia a un valor móvil&mdash;un valor cuyo contenido no necesitamos conservar una vez que hemos usado lo (por ejemplo, un temporal). Desde el punto de todo es pasar de (lo que modificar) el valor enlazado a una referencia de valor r, `const` y `volatile` calificadores (también conocida como VC-calificadores) no se aplican a las referencias de valor r.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Una referencia de valor r se enlaza a un valor r. De hecho, en términos de resolución de sobrecarga, un valor r *prefiere* se puede enlazar a una referencia de valor r que con un valor l de referencia const. Pero una referencia de valor r no se puede enlazar a un valor l porque, como hemos dicho, una referencia de valor r hace referencia a un valor cuyo contenido se supone que no necesitamos conservar (por ejemplo, el parámetro de un constructor de movimiento).

También puedes pasar un valor r donde se espera un argumento por valor, a través de construcción de copia (o a través de construcción de movimiento, si el valor de r es un xvalue).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Un glvalue tiene identidad; un prvalue no
En esta etapa, sabemos lo que tiene la identidad. Y sabemos qué es móvil y qué no es. Pero aún te no, denominado el conjunto de valores que *no* tienen identidad. Ese conjunto se conoce como el *prvalue*o *valores de r pura*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Un valor l tiene identidad; un prvalue no](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>La imagen completa de las categorías de valor
Solo permanece combinar la información e ilustraciones anteriormente en una única imagen grande.

![La imagen completa de las categorías de valor](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Un glvalue (valor generalizado l) tiene identidad.

### <a name="lvalue-im"></a>valor l (i\ & \!m)
Un valor de l (un tipo de glvalue) tiene identidad, pero no móvil. Estos son los valores de lectura-escritura normalmente que pasas alrededor de referencia o referencia const o valor si copiar es barata. Un valor l no se puede enlazar a una referencia de valor r.

### <a name="xvalue-im"></a>xValue (i\ & m)
Un xvalue (un tipo de glvalue, pero también un tipo de valor r) tiene identidad y también es móvil. Esto puede ser un valor de l erstwhile que hayas decidido mover porque copiar es costoso y se cuidado de no tener acceso a él más adelante. Aquí te mostramos cómo puede convertir un valor l en un xvalue.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

En el ejemplo de código anterior, no hemos pasamos nada aún. Solo hemos creado un xvalue convirtiendo un valor de l en una referencia a valores de r sin nombre. Aún pueda identificarse por su nombre de valor l; pero, como un xvalue, ahora es *capaz* de se mueve. Las razones para hacerlo, y qué mover realmente tiene el siguiente aspecto, tendrá que espera a que otro tema. Pero puedes considerar la "x" en "xvalue" como significado "experto solo" si ayuda a. Convirtiendo un valor l en un xvalue (un tipo de valor r), el valor se convierte entonces en capaz de enlazado a una referencia de valor r.

Estos son dos otros ejemplos de xvalues&mdash;llamar a una función que devuelve una referencia de valores de r sin nombre y el acceso a un miembro de un xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\ & m)
Un prvalue (valores de r pura; un tipo de valor r) no tiene identidad, pero es móvil. Por lo general, estos son temporales, el resultado de llamar a una función que se devuelve por valor, o el resultado de la evaluación de cualquier otra expresión que no es un glvalue

### <a name="rvalue-m"></a>valores de r (m)
Un valor r es móvil. Un valor r *referencia* siempre hace referencia a un valor de r (un valor, cuyo contenido se supone que no necesitamos conservar).

¿Sin embargo, es una referencia de valor r propio un valor r? Una referencia a valores r *sin nombre* (como los que se muestra en los ejemplos de código de xvalue anteriores) es un xvalue por lo tanto, sí, es un valor r. Prefiere se puede enlazar a un parámetro de la función de referencia de valor r, como los de un constructor de movimiento. Por el contrario (y quizás counter-intuitively), si una referencia de valor r tiene un nombre, a continuación, la expresión que consta de ese nombre es un valor l. Por lo tanto, *no* se puede enlazar a un parámetro de referencia de valor r. Pero es fácil para que sea hacerlo&mdash;solo convertirlo en una referencia a valores de r sin nombre (un xvalue) nuevo.

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
El tipo de valor que no tiene la identidad y no móviles es la combinación de uno que aún no hemos mencionado. Pero te podemos pasar por alto, porque esa categoría no es una idea útil en el lenguaje de C++.

## <a name="reference-collapsing-rules"></a>Reglas de contracción de referencia
Varias referencias like en una expresión (una referencia de valor l a una referencia de valor l o una referencia de valor r a una referencia de valor r) cancelar una fuera de otro.

- `A& &` contrae en `A&`.
- `A&& &&` contrae en `A&&`.

Varios a diferencia de las referencias en una expresión contraer a una referencia de valor l.

- `A& &&` contrae en `A&`.
- `A&& &` contrae en `A&`.

## <a name="forwarding-references"></a>Reenvío de referencias
En esta sección final contrasta referencias de valor r, que ya hemos mencionado, con el concepto de una *referencia de reenvío*diferentes.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` es una referencia de valor r, como ya hemos visto. Const y volátil no se aplican a las referencias de valor r.
- `foo` acepta solo los valores de r de tipo **A**.
- Hace referencia los valores de r motivo (como `A&&`) existe es para que puedes crear una sobrecarga que está optimizada para el caso de un temporal (u otros valores de r) que se pasan.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` es una *referencia de reenvío*. Dependiendo de lo que se pasa a `bar`, tipo **_Ty** podría ser const/no const independientemente volátil o no volátil.
- `bar` acepta cualquier valor l o los valores de r de tipo **_Ty**.
- Pasa un valor l hace que la referencia de reenvío para convertirse en `_Ty& &&`, que se contrae en la referencia de valor l `_Ty&`.
- Pasa un valor r hace que la referencia de reenvío para convertirse en `_Ty&& &&`, que se contrae en la referencia de valor r `_Ty&&`.
- El motivo por el reenvío de referencias (como `_Ty&&`) existe es *no* para la optimización, pero para realizar lo que se pasa en los mismos y reenviarlos en forma transparente y eficaz. Es probable que producen una referencia de reenvío solo si escribir (o estudias) código de la biblioteca&mdash;por ejemplo, una función de fábrica que reenvía los argumentos de constructor.

## <a name="sources"></a>Orígenes
* \[Stroustrup, 2013\] B. Stroustrup: el lenguaje de programación de C++, la cuarta edición. Addison-Wesley. 2013.
