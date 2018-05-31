---
author: stevewhims
description: Un valor escalar debe estar encapsulado dentro de un objeto de clase de referencia antes de pasarlo a una función que espera **IInspectable**. Dicho proceso de encapsulación se conoce como la *conversión boxing* del valor.
title: Conversión boxing y unboxing de valores escalar a IInspectable con C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, XAML, control, conversión boxing, escalar, valor
ms.localizationpriority: medium
ms.openlocfilehash: 61d5c7a35fb7a6ff9952f3fe768f4faa3f6c6347
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832011"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Conversión boxing y unboxing de valores escalar a IInspectable con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 
La [**interfaz IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821) es la interfaz de raíz de cada clase en tiempo de ejecución de Windows Runtime (WinRT). Esto es una idea análoga de [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) que se encuentra en la raíz de cada interfaz y clase COM; y **System.Object** que se encuentra en la raíz de cada clase con [sistema de tipo común](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system).

En otras palabras, una función que espera **IInspectable** se puede pasar a una instancia de cualquier clase en tiempo de ejecución. Pero no puedes pasar directamente un valor escalar, como un valor numérico o de texto, a este tipo de función. En su lugar, un valor escalar debe estar encapsulado dentro de un objeto de clase de referencia. Dicho proceso de encapsulación se conoce como la *conversión boxing* del valor.

C++ / WinRT proporciona la función [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value), que toma un valor escalar y devuelve el valor de conversión boxing en una **IInspectable**. Para realizar la conversión unboxing de una **IInspectable** de vuelta a un valor escalar, están las funciones [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) y [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or).

## <a name="examples-of-boxing-a-value"></a>Ejemplos de conversiones boxing de un valor
La función de descriptor de acceso [**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) devuelve un [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), que es un valor escalar. Podemos hacer la conversión boxing de dicho valor **hstring** y pasarlo a una función que espera **IInspectable** de este modo.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

Para establecer la propiedad de contenido de un [**Button**](/uwp/api/windows.ui.xaml.controls.button) XAML, llama a la función de mutación [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?). Para establecer la propiedad de contenido a un valor de cadena, puedes usar este código.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

Primero, el constructor de cadenas **hstring** convierte la cadena literal en un **hstring**. A continuación, se invoca la sobrecarga de **winrt::box_value** que toma un **hstring**.

## <a name="examples-of-unboxing-an-iinspectable"></a>Ejemplos de conversiones unboxing de una IInspectable
En tus propias funciones que esperan **IInspectable**, puedes usar [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) para realizar una conversión unboxing, y puedes usar [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) para realizar una conversión unboxing con un valor predeterminado.

```cppwinrt
void Unbox(Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="important-apis"></a>API importantes
* [Interfaz IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821)
* [Plantilla de función winrt::box_value](/uwp/cpp-ref-for-winrt/box-value)
* [Plantilla de función winrt::unbox_value](/uwp/cpp-ref-for-winrt/unbox-value)
* [Plantilla de función winrt::unbox_value_or](/uwp/cpp-ref-for-winrt/unbox-value-or)
