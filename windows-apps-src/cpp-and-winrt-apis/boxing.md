---
description: Un valor escalar debe estar encapsulado dentro de un objeto de clase de referencia antes de pasarlo a una función que espera **IInspectable**. Dicho proceso de encapsulación se conoce como la *conversión boxing* del valor.
title: Conversión boxing y unboxing de valores escalares a IInspectable con C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, XAML, control, boxing, scalar, value
ms.localizationpriority: medium
ms.openlocfilehash: 29263260217de154f1a942d37d1e18fece15e3d0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "79038098"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>Conversión boxing y unboxing de valores escalares a IInspectable con C++/WinRT
 
La [**interfaz IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) es la interfaz de raíz de todas las clases en tiempo de ejecución de Windows Runtime (WinRT). Esto es una idea análoga a que [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) se encuentra en la raíz de todas las clases e interfaces COM y a que **System.Object** se encuentra en la raíz de todas las clases [Common Type System ](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system).

En otras palabras, una función que espera **IInspectable** se puede pasar a una instancia de cualquier clase en tiempo de ejecución. Sin embargo, no se puede pasar directamente un valor escalar, como un valor numérico o de texto, a dicha función. En su lugar, los valores escalares deben encapsularse dentro de un objeto de clase de referencia. Dicho proceso de encapsulación se conoce como la *conversión boxing* del valor.

> [!IMPORTANT]
> Puedes aplicar la conversión boxing y unboxing a cualquier tipo que puedas pasar a una API de Windows Runtime. En otras palabras, un tipo de Windows Runtime. Los valores numéricos y de texto (cadenas) son los ejemplos indicados anteriormente. Otro ejemplo es un `struct` que definas en IDL. Si intentas aplicar una conversión boxing a un `struct` de C++ normal (uno que no se defina en IDL), el compilador te recordará que solo puedes aplicar la conversión boxing a un tipo de Windows Runtime. Una clase en tiempo de ejecución es un tipo de Windows Runtime, pero claro que puedes pasar clases en tiempo de ejecución a las API de Windows Runtime sin una conversión boxing.

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) proporciona la función [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value), que toma un valor escalar y devuelve el valor de conversión boxing a **IInspectable**. Para realizar la conversión unboxing de **IInspectable** en un valor escalar se usan las funciones [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) y [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or).

## <a name="examples-of-boxing-a-value"></a>Ejemplos de conversión boxing de un valor
La función del descriptor de acceso [**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) devuelve [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), que es un valor escalar. Podemos hacer la conversión boxing del valor **hstring** y pasarlo a una función que espera **IInspectable** de este modo.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

Para establecer la propiedad de contenido de una clase [**Button**](/uwp/api/windows.ui.xaml.controls.button) de XAML, llama a la función de mutación [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?). Para establecer la propiedad de contenido a un valor de cadena, puedes usar este código.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

En primer lugar, el constructor de conversión [**hstring**](/uwp/cpp-ref-for-winrt/hstring) convierte el literal de la cadena en un **hstring**. Luego, se invoca la sobrecarga de **winrt::box_value** que toma un **hstring**.

## <a name="examples-of-unboxing-an-iinspectable"></a>Ejemplos de conversiones unboxing de IInspectable
En tus propias funciones que esperan **IInspectable**, puedes usar [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) para realizar una conversión unboxing y [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) para realizar una conversión unboxing con un valor predeterminado.

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>Determinación del tipo de un valor de conversión boxing
Si recibes un valor de conversión boxing y no estás seguro de qué tipo contiene (necesitas conocer su tipo para aplicar la conversión unboxing), puedes consultar el valor de conversión boxing en su interfaz de [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue) y, después, llamar a **Type** en él. Aquí tienes un ejemplo de código.

`WINRT_ASSERT` es una definición de macro y se expande a [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
float pi = 3.14f;
auto piInspectable = winrt::box_value(pi);
auto piPropertyValue = piInspectable.as<winrt::Windows::Foundation::IPropertyValue>();
WINRT_ASSERT(piPropertyValue.Type() == winrt::Windows::Foundation::PropertyType::Single);
```

## <a name="important-apis"></a>API importantes
* [Interfaz IInspectable](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)
* [Plantilla de función winrt::box_value](/uwp/cpp-ref-for-winrt/box-value)
* [Estructura winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Plantilla de función winrt::unbox_value](/uwp/cpp-ref-for-winrt/unbox-value)
* [Plantilla de función winrt::unbox_value_or](/uwp/cpp-ref-for-winrt/unbox-value-or)
