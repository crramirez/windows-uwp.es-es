---
description: Un objeto ágil es aquel al que se puede acceder desde cualquier subproceso. Tus tipos C++/WinRT son ágiles de manera predeterminada, pero puedes optar por rechazarlos.
title: Objetos ágiles con C++/WinRT
ms.date: 10/20/2018
ms.topic: article
keywords: awndows 10, uwp, estándar, c++, cpp, winrt, proyección, ágil, objeto, agilidad, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 2481396d9348250e14ebfc2d1f940b663b405f77
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639670"
---
# <a name="agile-objects-in-cwinrt"></a>Los objetos ágiles en C++ / c++ / WinRT

En la mayoría de los casos, una instancia de una clase en tiempo de ejecución de Windows se puede acceder desde cualquier subproceso (al igual que pueden la mayoría de los objetos de C++). Esta clase en tiempo de ejecución de Windows es *agile*. Solo un pequeño número de clases en tiempo de ejecución de Windows que se incluyen con Windows es no ágiles, pero cuando consume debe tener en cuenta su modelo de subprocesos y el comportamiento de serialización (serialización transmite datos a través de un límite del apartamento). Es una buena opción predeterminada para todos los objetos en tiempo de ejecución de Windows ser ágil, por lo que su propio [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) tipos son ágiles de forma predeterminada.

Pero puede dejar de participar. Es posible que tenga un motivo convincente para requerir un objeto del tipo residir, por ejemplo, en un determinado contenedor uniproceso. Por lo general, esto está relacionado con los requisitos de reentrada. Pero cada vez más, incluso las API de interfaces de usuarios ofrecen objetos ágiles. En general, la agilidad es la opción más sencilla y aporta el mayor rendimiento. Además, cuando se implementa una fábrica de activaciones, debe ser ágil aunque tu correspondiente clase en tiempo de ejecución no lo sea.

> [!NOTE]
> Windows Runtime se basa en COM. En términos COM, se registra una clase ágil con `ThreadingModel` = *both*. Para obtener más información sobre subprocesos modelos y apartamentos COM, vea [comprensión y Using COM Threading Models](https://msdn.microsoft.com/library/ms809971).

## <a name="code-examples"></a>Ejemplos de código

Vamos a usar un ejemplo de implementación de una clase en tiempo de ejecución para ilustrar cómo C++ / c++ / WinRT es compatible con agilidad.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Dado que no lo hemos rechazado todavía, esta implementación es ágil. La estructura base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) implementa [**IAgileObject**](https://msdn.microsoft.com/library/windows/desktop/hh802476) y [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal). La implementación **IMarshal** usa **CoCreateFreeThreadedMarshaler** para hacer lo correcto para el código heredado que no conoce **IAgileObject**.

Este código comprueba la agilidad de un objeto. La llamada a [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) inicia una excepción si `myimpl` no es ágil.

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

En lugar de manipular una excepción, puedes llamar a [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function) en su lugar.

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** no tiene ningún método propio, de modo que no podrás hacer mucho con él. Por lo tanto, esta siguiente variante es más habitual.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** es una *interfaz de marcador*. El mero éxito o error de consultar **IAgileObject** es la extensión de la información y la utilidad que obtienes a partir de él.

## <a name="opting-out-of-agile-object-support"></a>Rechazar el soporte para objetos ágiles

Puedes optar por rechazar explícitamente el soporte para objetos ágiles pasando la estructura del marcador [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) como un argumento plantilla a tu clase base.

Si derivas directamente desde **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, winrt::non_agile>
{
    ...
}
```

Si vas a crear una clase en tiempo de ejecución.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, winrt::non_agile>
{
    ...
}
```

No importa dónde aparezca la estructura del marcador dentro del paquete de parámetro variádicas.

Rechazar la agilidad, como si no se puede implementar **IMarshal** usted mismo. Por ejemplo, puede usar el **winrt::non_agile** marcador para evitar la implementación de la agilidad de forma predeterminada e implementar **IMarshal** usted mismo&mdash;quizás para admitir la semántica de cálculo de referencias por valor.

## <a name="agile-references-winrtagileref"></a>Referencias ágiles (winrt::agile_ref)

Si vas a consumir un objeto que no es ágil, pero tienes que pasarlo a algún contexto potencialmente ágil, una opción consiste en usar la plantilla de estructura [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) para obtener una referencia ágil a una instancia de un tipo que no sea ágil, o a una interfaz de un objeto que no sea ágil.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

O bien, puedes usar la función auxiliar [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile).

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

En cualquier caso, `agile` ya puede pasarse libremente a un subproceso en un contenedor diferente y usarse allí.

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

La llamada [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agilerefget-function) devuelve un proxy que puede usarse con seguridad dentro del contexto de subproceso donde se llame a **get**.

## <a name="important-apis"></a>API importantes

* [Interfaz IAgileObject](https://msdn.microsoft.com/library/windows/desktop/hh802476)
* [IMarshal (interfaz)](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993)
* [winrt::agile_ref struct template](/uwp/cpp-ref-for-winrt/agile-ref)
* [winrt::implements struct template](/uwp/cpp-ref-for-winrt/implements)
* [plantilla de función winrt::make_agile](/uwp/cpp-ref-for-winrt/make-agile)
* [winrt::non_agile marcador struct](/uwp/cpp-ref-for-winrt/non-agile)
* [winrt::Windows::Foundation::IUnknown::as function](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [winrt::Windows::Foundation::IUnknown::try_as function](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)

## <a name="related-topics"></a>Temas relacionados

* [Entender y usar modelos de subprocesamiento de com.](https://msdn.microsoft.com/library/ms809971)
