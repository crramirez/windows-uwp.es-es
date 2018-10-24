---
author: stevewhims
description: Un objeto ágil es aquel al que se puede acceder desde cualquier subproceso. Tus tipos C++/WinRT son ágiles de manera predeterminada, pero puedes optar por rechazarlos.
title: Objetos ágiles con C++/WinRT
ms.author: stwhi
ms.date: 10/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: awndows 10, uwp, estándar, c++, cpp, winrt, proyección, ágil, objeto, agilidad, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 6cc8ebb24eb051cd8e9b141f361f47041b122d5c
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "5436650"
---
# <a name="agile-objects-in-cwinrt"></a>Objetos ágiles en C++/WinRT

En la mayoría de los casos, una instancia de una clase en tiempo de ejecución de Windows se puede acceder desde cualquier subproceso (al igual que pueden en la mayoría de los objetos de C++). Dicha clase en tiempo de ejecución de Windows es *ágil*. Solo un número reducido de clases de Windows Runtime que se incluyen con Windows no es ágil, pero cuando las consumas debes tener en cuenta su modelo de subprocesos y el comportamiento de serialización (la serialización transmite datos a través de un límite de contenedor). Es una buena opción predeterminada para cada objeto de Windows Runtime sean ágiles, por lo que tu propio [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) tipos son ágiles de manera predeterminada.

Sin embargo, puedes optar por rechazarlos. Es posible que tengas una buena razón para requerir un objeto de tu tipo para residir, por ejemplo, en un determinado contenedor uniproceso. Por lo general, esto está relacionado con los requisitos de reentrada. Pero cada vez más, incluso las API de interfaces de usuarios ofrecen objetos ágiles. En general, la agilidad es la opción más sencilla y aporta el mayor rendimiento. Además, cuando se implementa una fábrica de activaciones, debe ser ágil aunque tu correspondiente clase en tiempo de ejecución no lo sea.

> [!NOTE]
> Windows Runtime se basa en COM. En términos COM, se registra una clase ágil con `ThreadingModel` = *both*. Para obtener más información acerca de modelos de subprocesos y apartamentos de COM, vea la [Descripción y uso de los modelos de subprocesos COM](https://msdn.microsoft.com/library/ms809971).

## <a name="code-examples"></a>Ejemplos de código

Vamos a usar un ejemplo de implementación de una clase en tiempo de ejecución para ilustrar cómo C++ / WinRT admite agilidad.

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

Puedes optar por rechazar explícitamente el soporte para objetos ágiles pasando la estructura del marcador [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non_agile) como un argumento plantilla a tu clase base.

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

Incluso optar por no agilidad, puedes implementar **IMarshal** tú mismo. Por ejemplo, puedes usar el marcador de **winrt:: non_agile** para evitar la implementación de agilidad predeterminada e implementar **IMarshal** tú mismo&mdash;quizás para admitir la semántica de cálculo de referencias por valor.

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
* [Interfaz IMarshal](https://docs.microsoft.com/previous-versions/windows/embedded/ms887993)
* [Plantilla de estructura winrt::agile_ref](/uwp/cpp-ref-for-winrt/agile-ref)
* [Plantilla de estructura winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Plantilla de función winrt::make_agile](/uwp/cpp-ref-for-winrt/make-agile)
* [Estructura del marcador de winrt::non_agile](/uwp/cpp-ref-for-winrt/non_agile)
* [Función winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Función winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)

## <a name="related-topics"></a>Artículos relacionados

* [Descripción y uso de modelos de subprocesos COM](https://msdn.microsoft.com/library/ms809971)
