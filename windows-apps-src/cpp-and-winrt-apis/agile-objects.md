---
description: Un objeto ágil es aquel al que se puede acceder desde cualquier subproceso. Los tipos de C++/WinRT son ágiles de manera predeterminada, pero puedes excluirlos.
title: Objetos ágiles con C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, estándar, c++, cpp, winrt, projection, proyección, agile, ágil, object, objeto, agility, agilidad, IAgileObject
ms.localizationpriority: medium
ms.openlocfilehash: 71800de1d209a0164ab5a7e90bbc191c0f9bebe9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154639"
---
# <a name="agile-objects-in-cwinrt"></a>Objetos ágiles en C++/WinRT

En la mayoría de los casos, se puede acceder a una instancia de una clase en tiempo de ejecución de Windows desde cualquier subproceso (al igual que la mayoría de los objetos C++ estándar). Esta clase en tiempo de ejecución de Windows es *ágil*. Solo un número reducido de clases de Windows Runtime que se incluyen con Windows no son ágiles, pero cuando las consumas deberás tener en cuenta su modelo de subprocesos y el comportamiento de serialización (la serialización transmite datos a través de un límite de apartamento). Es una buena opción predeterminada de que todos los objetos Windows Runtime sean ágiles, por lo que tus propios tipos [C++/WinRT](./intro-to-using-cpp-with-winrt.md) son ágiles de manera predeterminada.

Sin embargo, puedes optar por rechazarlos. Es posible que tengas una buena razón para requerir un objeto de tu tipo para residir, por ejemplo, en un determinado apartamento uniproceso. Por lo general, esto está relacionado con los requisitos de reentrada. Pero cada vez más, incluso las API de interfaces de usuarios, ofrecen objetos ágiles. En general, la agilidad es la opción más sencilla y aporta el mayor rendimiento. Además, cuando se implementa una fábrica de activaciones, debe ser ágil aunque tu correspondiente clase en tiempo de ejecución no lo sea.

> [!NOTE]
> Windows Runtime se basa en COM. En términos COM, una clase ágil se registra con `ThreadingModel` = *both*. Para más información sobre los modelos de subprocesos COM y los apartamentos, consulta [Descripción y uso de los modelos de subprocesos COM](/previous-versions/ms809971(v=msdn.10)).

## <a name="code-examples"></a>Ejemplos de código

Usemos un ejemplo de implementación de una clase en tiempo de ejecución para mostrar cómo C++/WinRT admite la agilidad.

```cppwinrt
#include <winrt/Windows.Foundation.h>

using namespace winrt;
using namespace Windows::Foundation;

struct MyType : winrt::implements<MyType, IStringable>
{
    winrt::hstring ToString(){ ... }
};
```

Dado que no lo hemos rechazado todavía, esta implementación es ágil. La estructura base [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements) implementa [**IAgileObject**](/windows/desktop/api/objidl/nn-objidl-iagileobject) e [**IMarshal**](/windows/desktop/api/objidl/nn-objidl-imarshal). La implementación **IMarshal** usa **CoCreateFreeThreadedMarshaler** para hacer lo correcto para el código heredado que no conoce **IAgileObject**.

Este código comprueba la agilidad de un objeto. La llamada a [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) lanza una excepción si `myimpl` no es ágil.

```cppwinrt
winrt::com_ptr<MyType> myimpl{ winrt::make_self<MyType>() };
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.as<IAgileObject>() };
```

En lugar de controlar una excepción, puedes llamar a [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function) en su lugar.

```cppwinrt
winrt::com_ptr<IAgileObject> iagileobject{ myimpl.try_as<IAgileObject>() };
if (iagileobject) { /* myimpl is agile. */ }
```

**IAgileObject** no tiene ningún método propio, por lo que no podrás hacer mucho con él. Por lo tanto, esta siguiente variante es más habitual.

```cppwinrt
if (myimpl.try_as<IAgileObject>()) { /* myimpl is agile. */ }
```

**IAgileObject** es una *interfaz de marcador*. El mero éxito o error de consultar **IAgileObject** es la extensión de la información y la utilidad que obtienes a partir de él.

## <a name="opting-out-of-agile-object-support"></a>Rechazar la compatibilidad con objetos ágiles

Puedes optar por rechazar explícitamente la compatibilidad con objetos ágiles al pasar la estructura de marcador [**winrt::non_agile**](/uwp/cpp-ref-for-winrt/non-agile) como un argumento de plantilla a tu clase base.

Si derivas directamente de **winrt::implements**.

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

Ya optes o no por la agilidad, puedes implementar **IMarshal** tú mismo. Por ejemplo, puedes usar el marcador **winrt::non_agile** para evitar la implementación de agilidad predeterminada e implementar **IMarshal** tú mismo&mdash;quizás para admitir la semántica de cálculo de referencias por valor.

## <a name="agile-references-winrtagile_ref"></a>Referencias ágiles (winrt::agile_ref)

Si vas a consumir un objeto que no es ágil, pero necesitas pasarlo a algún contexto potencialmente ágil, una opción consiste en usar la plantilla de estructura [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) para obtener una referencia ágil a una instancia de un tipo que no sea ágil, o a una interfaz de un objeto que no sea ágil.

```cppwinrt
NonAgileType nonagile_obj;
winrt::agile_ref<NonAgileType> agile{ nonagile_obj };
```

O bien, puedes usar la función auxiliar [**winrt::make_agile**](/uwp/cpp-ref-for-winrt/make-agile).

```cppwinrt
NonAgileType nonagile_obj;
auto agile{ winrt::make_agile(nonagile_obj) };
```

En cualquier caso, `agile` ya puede pasarse libremente a un subproceso en un apartamento diferente y usarse allí.

```cppwinrt
co_await resume_background();
NonAgileType nonagile_obj_again{ agile.get() };
winrt::hstring message{ nonagile_obj_again.Message() };
```

La llamada a [**agile_ref::get**](/uwp/cpp-ref-for-winrt/agile-ref#agile_refget-function) devuelve un proxy que puede usarse con seguridad dentro del contexto de subproceso donde se llame a **get**.

## <a name="important-apis"></a>API importantes

* [Interfaz IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject)
* [Interfaz IMarshal](/windows/desktop/api/objidl/nn-objidl-imarshal)
* [Plantilla de estructura winrt::agile_ref](/uwp/cpp-ref-for-winrt/agile-ref)
* [Plantilla de estructura winrt::implements](/uwp/cpp-ref-for-winrt/implements)
* [Plantilla de función winrt::make_agile](/uwp/cpp-ref-for-winrt/make-agile)
* [Estructura del marcador de winrt::non_agile](/uwp/cpp-ref-for-winrt/non-agile)
* [Función winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Función winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)

## <a name="related-topics"></a>Temas relacionados

* [Descripción y uso de modelos de subprocesos COM](/previous-versions/ms809971(v=msdn.10))