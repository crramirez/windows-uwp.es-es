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
ms.openlocfilehash: 7496725d84339de5e318ee6c00aebefb204af751
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "4467398"
---
# <a name="boxing-and-unboxing-scalar-values-to-iinspectable-with-cwinrt"></a>Conversión boxing y unboxing de valores escalar a IInspectable con C++/WinRT
 
La [**interfaz IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) es la interfaz de raíz de cada clase en tiempo de ejecución de Windows Runtime (WinRT). Esto es una idea análoga de [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) que se encuentra en la raíz de cada interfaz y clase COM; y **System.Object** que se encuentra en la raíz de cada clase con [sistema de tipo común](https://docs.microsoft.com/dotnet/standard/base-types/common-type-system).

En otras palabras, una función que espera **IInspectable** se puede pasar a una instancia de cualquier clase en tiempo de ejecución. Pero no puedes pasar directamente un valor escalar, como un valor numérico o de texto, a este tipo de función. En su lugar, un valor escalar debe estar encapsulado dentro de un objeto de clase de referencia. Dicho proceso de encapsulación se conoce como la *conversión boxing* del valor.

[C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) proporciona la función [**winrt:: box_value**](/uwp/cpp-ref-for-winrt/box-value) , que toma un valor escalar y devuelve el valor de conversión boxing en una **IInspectable**. Para realizar la conversión unboxing de una **IInspectable** de vuelta a un valor escalar, están las funciones [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) y [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or).

## <a name="examples-of-boxing-a-value"></a>Ejemplos de conversiones boxing de un valor
La función de descriptor de acceso [**LaunchActivatedEventArgs::Arguments**](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.Arguments) devuelve un [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), que es un valor escalar. Podemos hacer la conversión boxing de dicho valor **hstring** y pasarlo a una función que espera **IInspectable** de este modo.

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    ...
    rootFrame.Navigate(winrt::xaml_typename<BlankApp1::MainPage>(), winrt::box_value(e.Arguments()));
    ...
}
```

Para establecer la propiedad de contenido de un [**Button**](/uwp/api/windows.ui.xaml.controls.button) XAML, llama a la función de mutación [**Button::Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content?). Para establecer la propiedad de contenido a un valor de cadena, puedes usar este código.

```cppwinrt
Button().Content(winrt::box_value(L"Clicked"));
```

Primero, el constructor de cadenas [**hstring**](/uwp/cpp-ref-for-winrt/hstring) convierte la cadena literal en un **hstring**. A continuación, se invoca la sobrecarga de **winrt::box_value** que toma un **hstring**.

## <a name="examples-of-unboxing-an-iinspectable"></a>Ejemplos de conversiones unboxing de una IInspectable
En tus propias funciones que esperan **IInspectable**, puedes usar [**winrt::unbox_value**](/uwp/cpp-ref-for-winrt/unbox-value) para realizar una conversión unboxing, y puedes usar [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) para realizar una conversión unboxing con un valor predeterminado.

```cppwinrt
void Unbox(winrt::Windows::Foundation::IInspectable const& object)
{
    hstring hstringValue = unbox_value<hstring>(object); // Throws if object is not a boxed string.
    hstringValue = unbox_value_or<hstring>(object, L"Default"); // Returns L"Default" if object is not a boxed string.
    float floatValue = unbox_value_or<float>(object, 0.f); // Returns 0.0 if object is not a boxed float.
}
```

## <a name="determine-the-type-of-a-boxed-value"></a>Determinar el tipo de un valor empaquetado
Si recibes un valor empaquetado y no estás seguro de qué tipo contiene (es necesario conocer su tipo para aplicar la conversión unboxing), a continuación, puedes consultar el valor empaquetado para su interfaz [**IPropertyValue**](/uwp/api/windows.foundation.ipropertyvalue) interfaz y llamar a **Type** en él. Aquí tienes un ejemplo de código.

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
