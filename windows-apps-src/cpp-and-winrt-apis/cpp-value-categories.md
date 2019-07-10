---
description: En este tema se describen las distintas categorías de valores que existen en C++. Sin duda habrás oído hablar de lvalues y rvalues, pero existen también otros tipos.
title: Categorías de valor y referencias a estas
ms.date: 08/11/2018
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, movimiento, reenvío, categorías de valor, semántica de movimiento, reenvío directo, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "63797106"
---
# <a name="value-categories-and-references-to-them"></a>Categorías de valor y referencias a estas
En este tema se describen las distintas categorías de valores (y referencias a los valores) que existen en C++. Seguro que has oído hablar de *lvalues* y *rvalues*, pero puede que no pienses en ellos de la forma que presenta este tema. Y también hay otros tipos de valores.

Todas las expresiones de C++ dan como resultado un valor que pertenece a una de las categorías que se tratan en este tema. Hay aspectos del lenguaje C++, sus instalaciones y reglas, que exigen una comprensión apropiada de estas categorías de valor y las referencias a estas. Por ejemplo, tomar la dirección de un valor, copiarlo, moverlo y reenviarlo a otra función. En este tema no se tratan todos esos aspectos en profundidad, pero se proporciona información básica para una comprensión sólida.

La información de este tema se encuadra en términos de análisis de Stroustrup de categorías de valor mediante las dos propiedades independientes de identidad y movilidad [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Un valor lvalue tiene identidad
¿Qué significa que un valor tiene *identidad*? Si tienes la dirección de memoria de un valor (o la puedes tomar) y la usas con seguridad, el valor tiene identidad. De este modo, puedes hacer algo más que comparar el contenido de los valores: puedes compararlos o distinguirlos por su identidad.

Un valor *lvalue* tiene identidad. Como apunte de interés únicamente histórico, la "l" en "lvalue" es una abreviación de "left" (izquierda), como en el lado izquierdo de una asignación. En C++, un valor lvalue puede aparecer a la izquierda *o* a la derecha de una asignación. La "l" de "lvalues" realmente no ayuda a comprender ni definir lo que son. Solo tienes que entender que lo que llamamos lvalue es un valor que tiene identidad.

Entre los ejemplos de expresiones que son lvalues se incluyen: una variable o una constante con nombre, o una función que devuelve una referencia. Entre los ejemplos de expresiones que *no* son lvalues se incluyen: un archivo temporal o una función que devuelve por valor.

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

Ahora, si bien la afirmación de que lvalues tiene identidad es una instrucción verdadera, lo es también para xvalues. Veremos más en detalle lo que es un valor *xvalue* más adelante en este tema. Por ahora, simplemente ten en cuenta que hay una categoría de valor llamada glvalue, para "lvalue generalizado". El supraconjunto de glvalues contiene ambos valores lvalues (también conocidos como *lvalues clásicos*) y xvalues. De este modo, mientras "un valor lvalue tiene identidad" es verdadero, el conjunto completo de cosas que tienen identidades es el conjunto de glvalues, tal como se muestra en esta ilustración.

![Un valor lvalue tiene identidad](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Un valor rvalue es móvil; un elemento lvalue no lo es
Sin embargo, hay valores que no son glvalues. Por lo tanto, hay valores para los que *no* se puede obtener una dirección de memoria (o no puedes basarte en ella para que sean válidos). Hemos visto algunos de estos valores en el ejemplo de código anterior. Esto parece una desventaja. Pero, en realidad, la ventaja de un valor como este es que te puedes *mover* desde él (lo que es normalmente barato), en lugar de copiar desde él (que normalmente es costoso). Moverse desde un valor significa que ya no está en el lugar en el que solía estar. Por lo tanto, intentar acceder a él en el lugar en el que solía estar es algo que se debe evitar hacer. En el ámbito de este tema no se explica cuándo y *cómo* mover un valor. Para ello, nos basta con saber que un valor que se puede mover es conocido como un valor *rvalue* (o *rvalue clásico*).

La "r" de "rvalue" es una abreviatura de "right" (derecha), como en el lado derecho de una asignación. Pero puedes usar rvalues y referencias a rvalues fuera de las asignaciones. Por tanto, la "r" de "rvalues" no es el elemento en el que centrarse. Solo tienes que entender que lo que llamamos rvalue es un valor que es móvil.

Un valor lvalue, por el contrario, no es móvil, tal como se muestra en esta ilustración. Un valor lvalue que se ha movido desafiaría a la propia definición de *lvalue*, y sería un problema inesperado para el código, que de forma muy razonable esperaría poder seguir teniendo acceso a lvalue.

![Un valor rvalue es móvil; un elemento lvalue no lo es](images/is-movable.png)

No se puede mover un valor lvalue. Sin embargo, *existe* un tipo de valores glvalue (el conjunto de cosas con identidad) que se puede mover, si sabes lo que estás haciendo (incluido tener cuidado de no acceder a él después de moverlo), y se llama xvalue. Más adelante retomaremos esta idea cuando nos centremos en la imagen completa de las categorías de valor.

## <a name="rvalue-references-and-reference-binding-rules"></a>Referencias rvalue y reglas de enlace de referencias
En esta sección se presenta la sintaxis para una referencia a un valor rvalue. Tendremos que esperar a que otro tema reciba un tratamiento detallado del movimiento y reenvío, pero esos son problemas que las referencias rvalue resuelven. Aunque, antes de adentrarnos en las referencias rvalue, primero es necesario ser más claros sobre `T&`, lo que anteriormente hemos llamado simplemente "una referencia". Realmente es "una referencia lvalue (distinta de const)", que se refiere a un valor en el que el usuario de la referencia puede escribir.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Una referencia lvalue puede enlazar a un valor lvalue, pero no a uno rvalue.

Luego, hay referencias const de lvalue (`T const&`), que hacen referencia a objetos en los que el usuario de la referencia *no* puede escribir (por ejemplo, una constante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Una referencia const de lvalue puede enlazar a un valor lvalue o a uno rvalue.

La sintaxis para una referencia a un valor rvalue de tipo `T` se escribe como `T&&`. Una referencia rvalue hace referencia a un valor móvil (un valor cuyo contenido no es necesario conservar una vez que lo hemos usado; por ejemplo, un archivo temporal). Puesto que el objetivo es moverlo desde el valor enlazado a una referencia rvalue (y de esta forma, modificarlo), los calificadores `const` y `volatile` (también conocidos como calificadores cv) no se aplican a las referencias rvalue.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Una referencia rvalue se enlaza a un valor rvalue. De hecho, en términos de resolución de sobrecarga, un valor rvalue *prefiere* enlazarse a una referencia rvalue más que a una referencia const de lvalue. Pero una referencia rvalue no se puede enlazar a un valor lvalue porque, como ya hemos dicho, una referencia rvalue hace referencia a un valor cuyo contenido se supone que no es necesario conservar (por ejemplo, el parámetro para un constructor de movimiento).

También puedes pasar un valor rvalue donde se espera un argumento por valor, con la construcción de copia (o con la construcción de movimiento, si el valor rvalue es un valor xvalue).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Un valor glvalue tiene identidad; un valor prvalue no
En esta etapa ya sabemos lo que tiene identidad. Y sabemos lo que es móvil y lo que no. Pero aún no hemos denominado el conjunto de valores que *no* tiene identidad. Este conjunto se conoce como el valor *prvalue* o *rvalue puro*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Un valor lvalue tiene identidad; un valor prvalue no](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>La imagen completa de las categorías de valor
Solo queda combinar la información y las ilustraciones anteriores en una sola imagen general.

![La imagen completa de las categorías de valor](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Un valor glvalue (lvalue generalizado) tiene identidad.

### <a name="lvalue-im"></a>lvalue (i\&\!m)
Un valor lvalue (un tipo de glvalue) tiene identidad, pero no es móvil. Normalmente estos son los valores de lectura y escritura que distribuyes por referencia o referencia de tipo const, o por valor si copiar es barato. No se puede enlazar un valor lvalue a una referencia rvalue.

### <a name="xvalue-im"></a>xvalue (i\&m)
Un valor xvalue (un tipo de glvalue, pero también un tipo de rvalue) tiene identidad y también es móvil. Esto podría ser un valor antiguo lvalue que has decidido mover porque la copia es costosa. En este caso, tendrás que tener cuidado de no acceder a él posteriormente. Aquí se muestra cómo puedes convertir un valor lvalue en uno xvalue.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

En el ejemplo de código anterior, todavía no hemos movido nada. Simplemente hemos creado un valor xvalue al convertir un valor lvalue en una referencia rvalue sin nombre. Todavía puede identificarse por su nombre de lvalue; pero, como un valor xvalue, ahora se *puede* mover. Los motivos para hacerlo y el aspecto que tiene realmente el movimiento se tratarán en otro tema. Pero se puede pensar que el significado de la "x" en "xvalue" es "solo experto", si esto ayuda. Al convertir un valor lvalue en uno xvalue (un tipo de valor rvalue), el valor pasa a poder enlazarse a una referencia rvalue.

Estos son dos ejemplos más de xvalues, que llaman a una función que devuelve una referencia rvalue sin nombre y acceden a un miembro de un valor xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\&m)
Un valor prvalue (rvalue puro; un tipo de valor rvalue) no tiene identidad, pero es móvil. Normalmente son temporales, el resultado de llamar a una función que devuelve por valor o el resultado de evaluar cualquier otra expresión que no es un valor glvalue.

### <a name="rvalue-m"></a>rvalue (m)
Un valor rvalue es móvil. Una *referencia* rvalue siempre hace referencia a un valor rvalue (un valor cuyo contenido se supone que no es necesario conservar).

Sin embargo, ¿una referencia rvalue no es en sí misma un valor rvalue? Una referencia rvalue *sin nombre* (como las que se muestran en los ejemplos de código xvalue anteriores) es un valor xvalue, por lo tanto, es un valor rvalue. Prefiere enlazarse a un parámetro de función de referencia rvalue, como el de un constructor de movimiento. En cambio, y quizás contra toda lógica, si una referencia rvalue tiene un nombre, la expresión que forma ese nombre es un valor lvalue. Por tanto, *no se puede* enlazar a un parámetro de referencia rvalue. Es sencillo: simplemente conviértelo otra vez en una referencia de rvalue sin nombre (un valor xvalue).

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
El tipo de valor que no tiene identidad y no es móvil es la combinación que todavía no hemos analizado. Pero podemos pasarlo por alto, porque esa categoría no es una idea útil en el lenguaje C++.

## <a name="reference-collapsing-rules"></a>Reglas de contracción de referencia
Varias referencias de tipo like en una expresión (una referencia lvalue a una referencia lvalue o una referencia rvalue en una referencia rvalue) se anulan mutuamente.

- `A& &` se contrae en `A&`.
- `A&& &&` se contrae en `A&&`.

Varias referencias de tipo unlike en una expresión se contraen a una referencia lvalue.

- `A& &&` se contrae en `A&`.
- `A&& &` se contrae en `A&`.

## <a name="forwarding-references"></a>Referencias de reenvío
En esta última sección se contrastan las referencias rvalue, que ya se han analizado, con el concepto distinto de una *referencia de reenvío*.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` es una referencia rvalue, tal como hemos visto. Los tipos const y volatile no se aplican a las referencias rvalue.
- `foo` solo acepta valores rvalue de tipo **A**.
- Las referencias rvalue (como `A&&`) existen para que puedas crear una sobrecarga que esté optimizada en el caso de que se pase un archivo temporal (u otro valor rvalue).

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` es una *referencia de reenvío*. En función de lo que pases a `bar`, el tipo **_Ty** puede ser de tipo const o no const independientemente de si es de tipo volatile o no volatile.
- `bar` acepta cualquier valor lvalue o rvalue de tipo **_Ty**.
- Pasar un valor lvalue provoca que la referencia de reenvío se convierta en `_Ty& &&`, que se contrae a la referencia lvalue `_Ty&`.
- Pasar un valor rvalue provoca que la referencia de reenvío se convierta en `_Ty&& &&`, que se contrae a la referencia rvalue `_Ty&&`.
- Las referencias de reenvío (como `_Ty&&`) *no* existen para la optimización, sino para que puedas tomar lo que les pasas y reenviarlo de forma transparente y eficiente. Es probable que encuentres una referencia de reenvío solo si escribes (o estudias de cerca) código de biblioteca; por ejemplo, una función de generador que reenvía argumentos de constructor.

## <a name="sources"></a>Orígenes
* \[Stroustrup, 2013\] B. Stroustrup: The C++ Programming Language, Fourth Edition. Addison-Wesley. 2013.
