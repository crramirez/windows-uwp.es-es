---
description: C++/WinRT 2.0 le permite aplazar la destrucción de los tipos de implementación y realizar consultas de forma segura durante la destrucción. En este tema se describen esas características y se explica cuándo usarlas.
title: Detalles acerca de los destructores
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, destrucción diferida, consultas seguras
ms.localizationpriority: medium
ms.openlocfilehash: 9806ea54665b24c246f2023714a14d94ec3bcc8e
ms.sourcegitcommit: 02cc7aaa408efe280b089ff27484e8bc879adf23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68387802"
---
# <a name="details-about-destructors"></a>Detalles acerca de los destructores

C++/WinRT 2.0 le permite aplazar la destrucción de los tipos de implementación y realizar consultas de forma segura durante la destrucción. En este tema se describen esas características y se explica cuándo usarlas.

## <a name="deferred-destruction"></a>Destrucción diferida

En el tema [Diagnóstico de asignaciones directas](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc), hemos mencionado que el tipo de implementación no puede tener un destructor privado.

La ventaja de tener un destructor público es que permite la destrucción diferida, que es la capacidad de detectar la llamada final de [**IUnknown::Release**](/windows/win32/api/unknwn/nf-unknwn-iunknown-release) en el objeto y después tomar posesión de ese objeto para aplazar su destrucción indefinidamente.

Recuerda que a los objetos COM clásicos se les cuentan las referencias de forma intrínseca; el recuento de referencias se administra mediante las funciones [**IUnknown::AddRef**](/windows/win32/api/unknwn/nf-unknwn-iunknown-addref) e **IUnknown::Release**. En una implementación tradicional de **Release**, se invoca un destructor de C++ de un objeto COM clásico una vez que ese recuento de referencias llega a 0.

```cppwinrt
uint32_t WINRT_CALL Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        delete this;
    }
 
    return remaining;
}
```

`delete this;` llama al destructor del objeto antes de liberar la memoria que ocupa el objeto. Esto funciona lo suficientemente bien siempre que no tengas que hacer nada interesante en el destructor.

```cppwinrt
using namespace winrt::Windows::Foundation;
... 
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    ~Sample() noexcept
    {
        // Too late to do anything interesting.
    }
};
```

¿Qué queremos decir con *interesante*? Por una parte, un destructor es intrínsecamente sincrónico. No puedes cambiar de subprocesos, quizás para destruir algunos recursos específicos del subproceso en otro contexto. No puedes consultar de forma confiable el objeto de alguna otra interfaz que puedas necesitar para liberar determinados recursos. Y la lista continúa. En los casos en los que la destrucción no es trivial, necesitas una solución más flexible. Aquí es donde entra la función **final_release** de C++/WinRT.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // This is the first stop...
    }
 
    ~Sample() noexcept
    {
        // ...And this happens only when *unique_ptr* finally deletes the object.
    }
};
```

Hemos actualizado la implementación de C++/WinRT de **Release** para llamar a **final_release** justo cuando el recuento de referencias del objeto llegue a 0. En ese estado, el objeto puede estar seguro de que no hay más referencias pendientes y ahora tiene la propiedad exclusiva de sí mismo. Por ese motivo, puede transferir la propiedad de sí mismo a la función **final_release** estática.

En otras palabras, el objeto se ha transformado de uno que admite la propiedad compartida en uno que tiene una propiedad exclusiva. **std::unique_ptr** tiene la propiedad exclusiva del objeto, por lo que destruirá de forma innata el objeto como parte de su semántica (de ahí la necesidad de un destructor público) cuando **std::unique_ptr** salga del ámbito (siempre que no se mueva a otra parte antes de eso). Esa es la clave. Puedes usar el objeto de forma indefinida, siempre que **std::unique_ptr** mantenga el objeto activo. A continuación se muestra un ejemplo de cómo puedes trasladar el objeto a cualquier otra parte.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        gc.push_back(std::move(ptr));
    }
};
```

Piensa en esto como un recolector de elementos no utilizados más determinista. Quizás sea más práctico y eficaz que conviertas la función **final_release** en una corrutina y controles su posible destrucción en un lugar, al tiempo que puedes suspender y cambiar los subprocesos según sea necesario.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    winrt::hstring ToString() const;
 
    static winrt::fire_and_forget final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        co_await winrt::resume_background(); // Unwind the calling thread.
 
        // Safely perform complex teardown here.
    }
};
```

Un punto de suspensión hace que vuelva el subproceso de llamada, que ha iniciado originalmente la llamada a la función **IUnknown::Release** y, por lo tanto, que indique al autor de la llamada que el objeto que tuvo ya no está disponible mediante ese puntero de interfaz. Los marcos de trabajo de la interfaz de usuario suelen tener que asegurarse de que los objetos se destruyen en el subproceso de interfaz de usuario específico que creó originalmente el objeto. Esta característica hace que el cumplimiento de este requisito sea trivial, ya que la destrucción se separa de la liberación del objeto.

## <a name="safe-queries-during-destruction"></a>Consultas seguras durante la destrucción

Basarse en la noción de la destrucción diferida es la capacidad de consultar de forma segura las interfaces durante la destrucción.

El COM clásico se basa en dos conceptos principales. El primero es el recuento de referencias y el segundo es la consulta de interfaces. Además de **AddRef** y **Release**, la interfaz **IUnknown** proporciona [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(refiid_void). Ese método se usa mucho en determinados marcos de trabajo de la interfaz de usuario, como XAML, para recorrer la jerarquía XAML mientras simula su sistema de tipos que admite composición. Fíjate en este ejemplo sencillo.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }
};
```

Puede *parecer* inofensivo. Esta página XAML quiere borrar su contexto de datos en su destructor. Pero [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) es una propiedad de la clase base **FrameworkElement** y reside en la interfaz diferente **IFrameworkElement**. Como consecuencia, C++/WinRT debe insertar una llamada a **QueryInterface** para buscar la vtable correcta antes de poder llamar a la propiedad **DataContext**. Pero el motivo por el que estamos en el destructor es que el recuento de referencias ha llegado a 0. Al llamar a **QueryInterface** aquí se supera temporalmente ese recuento de referencias y, cuando vuelve a 0, el objeto se destruye de nuevo.

C++/WinRT 2.0 se ha mejorado para admitir esto. Esta es la implementación de C++/WinRT 2.0 de Release, en una forma simplificada.

```cppwinrt
uint32_t Release() noexcept
{
    uint32_t const remaining{ subtract_reference() };
 
    if (remaining == 0)
    {
        m_references = 1; // Debouncing!
        T::final_release(...);
    }
 
    return remaining;
}
```

Como puedes haber predicho, primero disminuye el recuento de referencias y, después, actúa únicamente si no hay referencias pendientes. En cambio, antes de llamar a la función **final_release** estática que hemos descrito anteriormente en este tema, establece el recuento de referencias en 1 para estabilizarlo. Nos referimos a esto como *eliminación del rebote* (tomando prestado un término de ingeniería eléctrica). Esto es fundamental para evitar que la referencia final se libere. Una vez que esto suceda, el recuento de referencias es inestable y no puede admitir de forma confiable una llamada a **QueryInterface**.

Llamar a **QueryInterface** es peligroso después de que se haya liberado la referencia final, ya que el recuento de referencias puede crecer indefinidamente. Es tu responsabilidad llamar solo a rutas de acceso de código conocidas que no prolongarán la vida del objeto. C++/WinRT se encuentra en el punto intermedio al asegurarse de que esas llamadas a **QueryInterface** se puedan realizar de forma confiable.

Para ello, estabiliza el recuento de referencias. Cuando se ha liberado la referencia final, el recuento de referencias real es 0 o algún valor extremadamente impredecible. Este último caso se puede producir si hay referencias débiles. En cualquier caso, esto no es sostenible si se produce una llamada posterior a **QueryInterface**, ya que esto hará que el recuento de referencias se incremente temporalmente (de ahí la referencia al rebote del recuento). Al establecerlo en 1, te aseguras de que nunca se producirá una llamada final a **Release** en este objeto. Eso es precisamente lo que queremos, ya que **std::unique_ptr** ahora posee el objeto, pero las llamadas enlazadas a los pares **QueryInterface**/**Release** serán seguras.

Observa un ejemplo más interesante.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    ~MainPage()
    {
        DataContext(nullptr);
    }

    static winrt::fire_and_forget final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;
        co_await winrt::resume_foreground(ptr->Dispatcher());
        ptr = nullptr;
    }
};
```

En primer lugar, se llama a la función **final_release** y se informa a la implementación de que es el momento de la limpieza. En este caso, **final_release** es una corrutina. Para simular un primer punto de suspensión, comienza por esperar en el grupo de subprocesos durante unos segundos. Después, se reanuda en el subproceso de distribuidor de la página. Ese último paso implica una consulta, ya que [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) es una propiedad de la clase base **DependencyObject**. Por último, la página se elimina realmente como consecuencia de asignar `nullptr` a **std::unique_ptr**. Esto, a su vez, llama al destructor de la página.

Dentro del destructor, borramos el contexto de datos que, como sabemos, requiere una consulta para la clase base **FrameworkElement**.

Todo esto es posible gracias a la eliminación del rebote del recuento de referencias (o a la estabilización del recuento de referencias) que proporciona C++/WinRT 2.0.