---
author: stevewhims
description: El soporte de referencia débil C++/WinRT se basa en el sistema de pago pay-to-play, esto es, no te cuesta nada a menos que se consulte tu objeto para IWeakReferenceSource.
title: Referencias débiles en C++/WinRT
ms.author: stwhi
ms.date: 04/19/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, débil, referencia
ms.localizationpriority: medium
ms.openlocfilehash: 63ffad19c0ae8a52737ae13a54e5657df875d0b5
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832609"
---
# <a name="weak-references-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Referencias débiles en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
En la mayoría de los casos deberías poder diseñar tu propia API C++/WinRT de modo que se evite la necesidad de referencias cíclicas y referencias débiles. Sin embargo, cuando se trata de la implementación nativa de frameworkL de la interfaz de usuario basada en XAML&mdash;debido al diseño histórico del marco&mdash;el mecanismo de referencia débil en C++/WinRT es necesario para controlar referencias cíclicas. Fuera de XAML, es poco probable que necesites usar referencias débiles (aunque en teoría, no haya nada específico de XAML en lo que a ellas respecta).

Para cualquier tipo en particular que declares, a C ++/WinRT no le resulta inmediatamente evidente saber si se necesitan referencias débiles o cuándo se necesitan. De este modo, C++ / WinRT proporciona soporte técnico de referencia débil automáticamente en la plantilla de estructura [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements), desde la que tus propios tipos C++/WinRT derivan directa o indirectamente. Es un sistema de pago pay-to-play, esto es, no te cuesta nada a menos que en realidad se consulte tu objeto para [**IWeakReferenceSource**](https://msdn.microsoft.com/library/br224609). Y puedes [rechazar recibir tal soporte](#opting-out-of-weak-reference-support) explícitamente.

## <a name="code-examples"></a>Ejemplos de código
La plantilla de estructura [**winrt::weak_ref**](/uwp/cpp-ref-for-winrt/weak-ref) es una opción para obtener una referencia débil a una instancia de clase.

```cppwinrt
Class c;
winrt::weak_ref<Class> weak{ c };
```
O bien, puedes usar la función auxiliar [**winrt::make_weak**](/uwp/cpp-ref-for-winrt/make-weak).

```cppwinrt
Class c;
auto weak = winrt::make_weak(c);
```

Crear una referencia débil no afecta el recuento de referencias en el propio objeto; simplemente hace que se asigne un bloque de control. El bloque de control se encarga de implementar la semántica de la referencia débil. Puedes intentar promover la referencia débil a una referencia fuerte y, si se realiza correctamente, utilizarla.

```cppwinrt
if (Class strong = weak.get())
{
    // use strong, for example strong.DoWork();
}
```

Siempre que exista alguna otra referencia fuerte, la llamada [**weak_ref::get**](/uwp/cpp-ref-for-winrt/weak-ref#weakrefget-function) incrementa el recuento de referencias y devuelve la referencia fuerte al autor de la llamada.

## <a name="a-weak-reference-to-the-this-pointer"></a>Una referencia débil al puntero *this*
Un objeto C++/WinRT se deriva directa o indirectamente desde la plantilla de estructura [**winrt::implements**](/uwp/cpp-ref-for-winrt/implements). La función miembro protegida [**implements::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function) devuelve una referencia débil a un puntero *this* del objeto C++/WinRT. [**implements.get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) obtiene una referencia fuerte.

## <a name="opting-out-of-weak-reference-support"></a>Rechazar el soporte de referencia débil
El soporte de referencia débil es automático. Pero puedes optar por rechazar explícitamente tal soporte pasando la estructura del marcador [**winrt::no_weak_ref**](/uwp/cpp-ref-for-winrt/no-weak-ref) como un argumento plantilla a tu clase base.

Si derivas directamente desde **winrt::implements**.

```cppwinrt
struct MyImplementation: implements<MyImplementation, IStringable, no_weak_ref>
{
    ...
}
```

Si vas a crear una clase en tiempo de ejecución.

```cppwinrt
struct MyRuntimeClass: MyRuntimeClassT<MyRuntimeClass, no_weak_ref>
{
    ...
}
```

No importa dónde aparezca la estructura del marcador dentro del paquete de parámetro variádicas. Si solicitas una referencia débil para un tipo rechazado, el compilador te ayudará con "*Esto es solo para soporte de referencia débil*".

## <a name="important-apis"></a>API importantes
* [Función winrt::implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [Plantilla de función winrt::make_weak](/uwp/cpp-ref-for-winrt/make-weak)
* [Estructura de marcador winrt::no_weak_ref](/uwp/cpp-ref-for-winrt/no-weak-ref)
* [Plantilla de estructura winrt::weak_ref](/uwp/cpp-ref-for-winrt/weak-ref)
