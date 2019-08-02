---
description: Este tema analiza en profundidad una característica de C++/WinRT 2.0 que le ayuda a diagnosticar el error que se produce al crear un objeto de tipo de implementación en la pila, en lugar de usar la familia [**winrt::make**](/uwp/cpp-ref-for-winrt/make) de aplicaciones auxiliares como correspondería.
title: Diagnóstico de asignaciones directas
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, directo, pila, asignaciones, proyectado, implementación
ms.localizationpriority: medium
ms.openlocfilehash: 7fe8ff6653b8655ee25cd9adc0c11acb22d42a11
ms.sourcegitcommit: 4e74c920f1fef507c5cdf874975003702d37bcbb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372796"
---
# <a name="diagnosing-direct-allocations"></a>Diagnóstico de asignaciones directas

Tal como se explica en [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis), cuando crees un objeto de tipo de implementación, debes usar la familia de aplicaciones auxiliares [**winrt::make**](/uwp/cpp-ref-for-winrt/make) para hacerlo. En este tema se analiza en profundidad una característica de C++/WinRT 2.0 que te ayuda a diagnosticar el error que se produce al asignar directamente un objeto de tipo de implementación en la pila.

Estos errores pueden convertirse en bloqueos o daños misteriosos que son difíciles de depurar y requieren mucho tiempo. Por lo tanto, se trata de una característica importante de la que merece la pena comprender su trasfondo.

## <a name="setting-the-scene-with-mystringable"></a>Establecimiento de la escena, con **MyStringable**

En primer lugar, veamos una implementación sencilla de [**IStringable**](/uwp/api/windows.foundation.istringable).

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const { return L"MyStringable"; }
};
```

Ahora imagina que necesitas llamar a una función (desde la implementación) que espera **IStringable** como argumento.

```cppwinrt
void Print(IStringable const& stringable)
{
    printf("%ls\n", stringable.ToString().c_str());
}
```

El problema es que nuestro tipo **MyStringable** *no* es **IStringable**.

- Nuestro tipo **MyStringable** es una implementación de la interfaz **IStringable**.
- El tipo **IStringable** es un tipo proyectado.

> [!IMPORTANT]
> Es importante comprender la diferencia entre un *tipo de implementación* y un *tipo proyectado*. Para conocer los conceptos y términos esenciales, asegúrate de leer [Consumo de API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT](author-apis.md).

El espacio que hay entre una implementación y la proyección puede ser difícil de comprender. Y, de hecho, para intentar que la implementación se parezca un poco más a la proyección, la implementación proporciona conversiones implícitas a cada uno de los tipos proyectados que implementa. Eso no significa que podamos hacerlo simplemente.

```cppwinrt
struct MyStringable : implements<MyStringable, IStringable>
{
    winrt::hstring ToString() const;
 
    void Call()
    {
        Print(this);
    }
};
```

En su lugar, necesitamos obtener una referencia para que los operadores de conversión se puedan usar como candidatos para resolver la llamada.

```cppwinrt
void Call()
{
    Print(*this);
}
```

Eso funciona. Una conversión implícita proporciona una conversión (muy eficaz) del tipo de implementación al tipo proyectado, y es muy conveniente para muchos escenarios. Sin esa capacidad, muchos tipos de implementación serían muy complicados de crear. Siempre que solo uses la plantilla de función [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (o [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self)) para asignar la implementación, no habrá ningún problema.

```cppwinrt
IStringable stringable{ winrt::make<MyStringable>() };
```

## <a name="potential-pitfalls-with-cwinrt-10"></a>Posibles problemas con C++/WinRT 1.0

Aun así, las conversiones implícitas pueden ocasionarte problemas. Presta atención a esta función de aplicación auxiliar poco útil.

```cppwinrt
IStringable MakeStringable()
{
    return MyStringable(); // Incorrect.
}
```

O incluso simplemente a esta instrucción aparentemente inofensiva.

```cppwinrt
IStringable stringable{ MyStringable() }; // Also incorrect.
```

Lamentablemente, el código como ese *sí* que se ha compilado con C++/WinRT 1.0 debido a esa conversión implícita. El problema (muy grave) es que es posible que devolvamos un tipo proyectado que apunta a un objeto de recuento de referencias cuya memoria de respaldo está en la pila efímera.

Este es otro elemento que se ha compilado con C++/WinRT 1.0.

```cppwinrt
MyStringable* stringable{ new MyStringable() }; // Very inadvisable.
```

Los punteros básicos son una fuente de errores peligrosa y laboriosa. No los uses si no es necesario. C++/WinRT se las ha arreglado para que todo funcione de forma eficaz sin obligarte a usar punteros básicos. Este es otro elemento que se ha compilado con C++/WinRT 1.0.

```cppwinrt
auto stringable{ std::make_shared<MyStringable>(); } // Also very inadvisable.
```

Se trata de un error en varios niveles. Tenemos dos recuentos de referencias diferentes para el mismo objeto. Windows Runtime (y el COM clásico anterior a él) se basa en un recuento de referencias intrínseco que no es compatible con **std::shared_ptr**. **std::shared_ptr** tiene, por supuesto, muchas aplicaciones válidas, pero es totalmente innecesario al compartir objetos de Windows Runtime (y del COM clásico). Por último, esto también se ha compilado con C++/WinRT 1.0.

```cppwinrt
auto stringable{ std::make_unique<MyStringable>() }; // Highly dubious.
```

Esto vuelve a ser bastante cuestionable. La propiedad única está en oposición a la duración compartida del recuento de referencias intrínseco de **MyStringable**.

## <a name="the-solution-with-cwinrt-20"></a>Solución con C++/WinRT 2.0

Con C++/WinRT 2.0, todos estos intentos de asignar directamente los tipos de implementación producen un error del compilador. Este es el mejor tipo de error y es infinitamente mejor que un misterioso error en tiempo de ejecución.

Siempre que tengas que crear una implementación, puedes usar simplemente [**winrt::make**](/uwp/cpp-ref-for-winrt/make) o [**winrt::make_self**](/uwp/cpp-ref-for-winrt/make-self), como se ha mostrado anteriormente. Y ahora, si te has olvidado de hacerlo, recibirás un error del compilador que se referirá a esto con una referencia a una función abstracta denominada **use_make_function_to_create_this_object**. No es exactamente `static_assert`, pero se parece. Aun así, esta es la forma más confiable de detectar todos los errores descritos.

Esto implica que tenemos que colocar algunas restricciones secundarias en la implementación. Dado que nos basamos en la ausencia de una invalidación para detectar la asignación directa, la plantilla de función **winrt::make** debe cumplir de algún modo la función virtual abstracta con una invalidación. Para hacerlo, se deriva de la implementación con una clase `final` que proporciona la invalidación. Debes tener en cuenta algunos aspectos sobre este proceso.

En primer lugar, la función virtual solo está presente en las compilaciones de depuración. Esto significa que la detección no va a afectar al tamaño de vtable en las compilaciones optimizadas.

En segundo lugar, puesto que la clase derivada que usa **winrt::make** es `final`, cualquier desvirtualización que el optimizador pueda deducir se producirá incluso si anteriormente has elegido no marcar la clase de implementación como `final`. Se trata de una mejora. Lo contrario es que la implementación *no* puede ser `final`. De nuevo, no es ninguna consecuencia porque el tipo con instancias siempre será `final`.

En tercer lugar, nada te impide marcar las funciones virtuales en tu implementación como `final`. Por supuesto, C++/WinRT es muy diferente del COM clásico y de implementaciones como WRL, donde todo lo relacionado con la implementación tiende a ser virtual. En C++/WinRT, el envío virtual se limita a la interfaz binaria de aplicaciones (ABI) (que siempre es `final`) y los métodos de implementación se basan en el polimorfismo estático o en tiempo de compilación. Esto evita que haya un polimorfismo en tiempo de ejecución innecesario y también significa que hay pocas razones para tener funciones virtuales en la implementación de C++/WinRT. Esto es muy bueno y produce una inserción mucho más predecible.

En cuarto lugar, como **winrt::make** inserta una clase derivada, la implementación no puede tener un destructor privado. Los destructores privados eran populares con las implementaciones clásicas de COM porque, de nuevo, todo era virtual, era habitual tratar directamente con punteros básicos y, por tanto, era fácil llamar accidentalmente a `delete` en lugar de a [**Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release). C++/WinRT se las ha arreglado para que te sea difícil tratar directamente con los punteros básicos. Y tendrías que esforzarte *mucho* para conseguir un puntero básico en C++/WinRT en el que pudieras llamar a `delete`. La semántica de valores implica que estás tratando con valores y referencias, y raramente con punteros.

Por lo tanto, C++/WinRT desafía nuestras nociones preconcebidas de lo que significa escribir código COM clásico. Y eso es perfectamente razonable porque WinRT no es un COM clásico. El COM clásico es el lenguaje de ensamblado de Windows Runtime. No debería ser el código que escribas cada día. En su lugar, C++/WinRT te ayuda a escribir código que es más similar al C++ moderno y se parece mucho menos al COM clásico.

## <a name="important-apis"></a>API importantes
* [Plantilla de función winrt::make](/uwp/cpp-ref-for-winrt/make)
* [Plantilla de función winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>Temas relacionados
* [Consumir API con C++/WinRT](consume-apis.md)
* [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)