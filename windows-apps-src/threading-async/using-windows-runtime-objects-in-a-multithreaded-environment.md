---
title: Usar objetos de Windows Runtime en un entorno multiproceso | Microsoft Docs
description: En este artículo se describe la forma en que el .NET Framework controla las llamadas desde C# y el código de Visual Basic a los objetos proporcionados por el Windows Runtime o por los componentes de Windows Runtime.
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
keywords: Windows 10, UWP, temporizador y subprocesos
ms.localizationpriority: medium
ms.openlocfilehash: 4287ed5fd5659b7d39d52ada9e3958c592771d59
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164139"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>Usar objetos de Windows Runtime en un entorno multiproceso
En este artículo se describe la forma en que el .NET Framework controla las llamadas desde C# y el código de Visual Basic a los objetos proporcionados por el Windows Runtime o por los componentes de Windows Runtime.

En .NET Framework, puede acceder a cualquier objeto desde varios subprocesos de forma predeterminada, sin ningún control especial. Todo lo necesario es una referencia al objeto. En el Windows Runtime, estos objetos se denominan *Agile*. La mayoría de las clases de Windows Runtime son ágiles, pero algunas clases no, e incluso las clases ágiles pueden requerir un tratamiento especial.

Siempre que sea posible, el Common Language Runtime (CLR) trata los objetos de otros orígenes, como el Windows Runtime, como si fueran objetos .NET Framework:

- Si el objeto implementa la interfaz [IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject) o tiene el atributo [MarshalingBehaviorAttribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) con [MarshalingType. Agile](/uwp/api/Windows.Foundation.Metadata.MarshalingType), CLR lo trata como Agile.

- Si CLR puede calcular las referencias de una llamada desde el subproceso donde se creó en el contexto del subproceso del objeto de destino, lo hace de manera transparente.

- Si el objeto tiene el atributo [MarshalingBehaviorAttribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) con [MarshalingType. None](/uwp/api/Windows.Foundation.Metadata.MarshalingType), la clase no proporciona información de serialización. CLR no puede calcular las referencias de la llamada, por lo que produce una excepción [InvalidCastException](/dotnet/api/system.invalidcastexception) con un mensaje que indica que el objeto solo se puede usar en el contexto del subproceso en el que se creó.

En las secciones siguientes se describen los efectos de este comportamiento en objetos de distintos orígenes.

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>Objetos de un componente de Windows Runtime escrito en C# o Visual Basic
Todos los tipos del componente que se pueden activar son ágiles de forma predeterminada.

> [!NOTE]
>  La agilidad no implica la seguridad de los subprocesos. Tanto en el Windows Runtime como en el .NET Framework, la mayoría de las clases no son seguras para subprocesos porque la seguridad para subprocesos tiene un costo de rendimiento y varios subprocesos nunca tienen acceso a la mayoría de los objetos. Resulta más eficaz para sincronizar el acceso a objetos individuales (o utilizar clases seguras para subprocesos) según sea necesario.

Al crear un componente de Windows Runtime, puede invalidar el valor predeterminado. Vea la interfaz [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) y la interfaz [IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject) .

## <a name="objects-from-the-windows-runtime"></a>Objetos de la Windows Runtime
La mayoría de las clases del Windows Runtime son ágiles y CLR las trata como Agile. La documentación de estas clases enumera "MarshalingBehaviorAttribute(Agile)" entre los atributos de clase. Sin embargo, los miembros de algunas de estas clases ágiles, como los controles XAML, lanzan excepciones si no se llaman en el subproceso de interfaz de usuario. Por ejemplo, el código siguiente intenta usar un subproceso en segundo plano para establecer una propiedad del botón en el que se hizo clic. La propiedad de [contenido](/uwp/api/Windows.UI.Xaml.Controls.ContentControl) del botón produce una excepción.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await Task.Run(() => {
        b.Content += ".";
    });
}
```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await Task.Run(Sub()
                       b.Content &= "."
                   End Sub)
End Sub
```

Puede tener acceso al botón de forma segura mediante su propiedad [Dispatcher](/uwp/api/Windows.UI.Xaml.DependencyObject) o la `Dispatcher` propiedad de cualquier objeto que exista en el contexto del subproceso de la interfaz de usuario (como la página en la que se encuentra el botón). En el código siguiente se usa el método [RunAsync](/uwp/api/Windows.UI.Core.CoreDispatcher) del objeto [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) para enviar la llamada en el subproceso de la interfaz de usuario.

```csharp
private async void Button_Click_2(object sender, RoutedEventArgs e)
{
    Button b = (Button) sender;
    await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            b.Content += ".";
    });
}

```

```vb
Private Async Sub Button_Click_2(sender As Object, e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    Await b.Dispatcher.RunAsync(
        Windows.UI.Core.CoreDispatcherPriority.Normal,
        Sub()
            b.Content &= "."
        End Sub)
End Sub
```

> [!NOTE]
>  La `Dispatcher` propiedad no inicia una excepción cuando se llama desde otro subproceso.

La duración de un objeto Windows Runtime que se crea en el subproceso de la interfaz de usuario está limitada por la duración del subproceso. No intente acceder a objetos en un subproceso de interfaz de usuario después de cerrar la ventana.

Si crea su propio control mediante la herencia de un control XAML o la composición de un conjunto de controles XAML, el control es ágil porque es un objeto de .NET Framework. Sin embargo, si llama a los miembros de su clase base o sus clases constituyentes, o si se llama a los miembros heredados, esos miembros lanzarán excepciones cuando se les llame desde cualquier subproceso, salvo el subproceso de interfaz de usuario.

### <a name="classes-that-cant-be-marshaled"></a>Clases de las que no se pueden calcular las referencias
Windows Runtime clases que no proporcionan información de serialización tienen el atributo [MarshalingBehaviorAttribute](/uwp/api/Windows.Foundation.Metadata.MarshalingBehaviorAttribute) con [MarshalingType. None](/uwp/api/Windows.Foundation.Metadata.MarshalingType). La documentación de este tipo de clase enumera "MarshalingBehaviorAttribute(Agile)" entre sus atributos.

En el código siguiente se crea un objeto [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI) en el subproceso de la interfaz de usuario y, a continuación, se intenta establecer una propiedad del objeto a partir de un subproceso del grupo de subprocesos. CLR no puede calcular las referencias de la llamada y produce una excepción [System. InvalidCastException](/dotnet/api/system.invalidcastexception) con un mensaje que indica que el objeto solo se puede usar en el contexto del subproceso en el que se creó.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_1(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Task.Run(() => {
        ccui.PhotoSettings.AllowCropping = true;
    });
}

```

```vb
Private ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_1(sender As Object, e As RoutedEventArgs)
    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Task.Run(Sub()
                       ccui.PhotoSettings.AllowCropping = True
                   End Sub)
End Sub
```

La documentación de [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI) también muestra "THREADINGATTRIBUTE (STA)" entre los atributos de la clase, ya que debe crearse en un contexto de un solo subproceso, como el subproceso de la interfaz de usuario.

Si desea tener acceso al objeto [CameraCaptureUI](/uwp/api/Windows.Media.Capture.CameraCaptureUI) desde otro subproceso, puede almacenar en caché el objeto [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) para el subproceso de la interfaz de usuario y usarlo más adelante para enviar la llamada en ese subproceso. O bien, puede obtener el distribuidor de un objeto XAML, como la página, como se muestra en el código siguiente.

```csharp
Windows.Media.Capture.CameraCaptureUI ccui;

private async void Button_Click_3(object sender, RoutedEventArgs e)
{
    ccui = new Windows.Media.Capture.CameraCaptureUI();

    await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () => {
            ccui.PhotoSettings.AllowCropping = true;
        });
}

```

```vb
Dim ccui As Windows.Media.Capture.CameraCaptureUI

Private Async Sub Button_Click_3(sender As Object, e As RoutedEventArgs)

    ccui = New Windows.Media.Capture.CameraCaptureUI()

    Await Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                                Sub()
                                    ccui.PhotoSettings.AllowCropping = True
                                End Sub)
End Sub
```

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c"></a>Objetos de un componente de Windows Runtime que está escrito en C++
De forma predeterminada, las clases del componente que se pueden activar son ágiles. Sin embargo, C++ permite una cantidad significativa de control sobre el comportamiento del suprocesamiento de modelos y el cálculo de referencias. Como se describió anteriormente en este artículo, CLR reconoce las clases ágiles, intenta serializar las llamadas cuando las clases no son ágiles y produce una excepción [System. InvalidCastException](/dotnet/api/system.invalidcastexception) cuando una clase no tiene información de cálculo de referencias.

En el caso de los objetos que se ejecutan en el subproceso de la interfaz de usuario y producen excepciones cuando se llaman desde un subproceso distinto del subproceso de interfaz de usuario, puede usar el objeto [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) del subproceso de interfaz de usuario para enviar la llamada.

## <a name="see-also"></a>Vea también
[Guía de C#](/dotnet/csharp/)

[Guía de Visual Basic](/dotnet/visual-basic/)