---
title: Usar objetos de Windows Runtime en un entorno multiproceso | Microsoft Docs
description: En este artículo se describe la manera en que .NET Framework controla las llamadas de código C# y Visual Basic a los objetos que proporcionan Windows Runtime o los componentes de Windows Runtime.
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 43ffd28c-c4df-405c-bf5c-29c94e0d142b
author: normesta
ms.author: normesta
keywords: windows 10, uwp, temporizador, subprocesos
ms.localizationpriority: medium
ms.openlocfilehash: 9f4b8249a81cb7d71ba1f4775fd858ae87779c85
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "6149339"
---
# <a name="using-windows-runtime-objects-in-a-multithreaded-environment"></a>Usar objetos de Windows Runtime en un entorno multiproceso
En este artículo se describe la manera en que .NET Framework controla las llamadas de código C# y Visual Basic a los objetos que proporcionan Windows Runtime o los componentes de Windows Runtime.

En .NET Framework, puedes acceder a cualquier objeto desde varios subprocesos de manera predeterminada, sin un control especial. Lo único que necesitas es una referencia al objeto. En Windows Runtime, estos objetos se denominan *ágiles*. La mayoría de las clases de Windows Runtime son ágiles, pero algunas clases no lo son, e incluso las clases ágiles pueden exigir un control especial.

Siempre que sea posible, Common Language Runtime (CLR) trata los objetos de otros orígenes, como Windows Runtime, como si fueran objetos de .NET Framework:

- Si el objeto implementa la interfaz [IAgileObject](http://msdn.microsoft.com/library/Hh802476.aspx) o tiene el atributo [MarshalingBehaviorAttribute](http://go.microsoft.com/fwlink/p/?LinkId=256022) con [MarshalingType.Agile](http://go.microsoft.com/fwlink/p/?LinkId=256023), CLR lo trata como ágil.

- Si CLR puede serializar una llamada desde el subproceso de donde se hizo hasta el contexto de subproceso del objeto de destino, lo hace de manera transparente.

- Si el objeto tiene el atributo [MarshalingBehaviorAttribute](http://go.microsoft.com/fwlink/p/?LinkId=256022) con [MarshalingType.None](http://go.microsoft.com/fwlink/p/?LinkId=256023), la clase no proporciona información de serialización. CLR no puede serializar la llamada, por lo que se inicia una excepción [InvalidCastException](/dotnet/api/system.invalidcastexception) con un mensaje que indica que el objeto puede usarse únicamente en el contexto del subproceso donde se creó.

En las siguientes secciones se describen los efectos de este comportamiento en objetos de distintos orígenes.

## <a name="objects-from-a-windows-runtime-component-that-is-written-in-c-or-visual-basic"></a>Objetos de un componente de Windows Runtime que está escrito en C# o Visual Basic
Todos los tipos en el componente que pueden activarse son ágiles de manera predeterminada.

> [!NOTE]
>  La agilidad no implica la seguridad de los subprocesos. Tanto en Windows Runtime como en .NET Framework, la mayoría de clases no son seguras para subprocesos, ya que la seguridad de los subprocesos tiene un costo de rendimiento, y a la mayoría de los objetos nunca se accede desde varios subprocesos. Es más eficiente sincronizar el acceso a objetos individuales (o usar clases seguras para subprocesos) según sea necesario únicamente.

Cuando creas un componente de Windows Runtime, puedes invalidar el valor predeterminado. Consulta la interfaz [ICustomQueryInterface](/dotnet/api/system.runtime.interopservices.icustomqueryinterface) y la interfaz [IAgileObject](http://msdn.microsoft.com/library/Hh802476.aspx).

## <a name="objects-from-the-windows-runtime"></a>Objetos de Windows Runtime
La mayoría de las clases en Windows Runtime son ágiles, y CLR las trata como ágiles. En la documentación para estas clases aparece "MarshalingBehaviorAttribute(Agile)" entre los atributos de clase. Sin embargo, los miembros de algunas de estas clases ágiles, como los controles XAML, inician excepciones si no se los llama en el subproceso de la interfaz de usuario. Por ejemplo, el siguiente código intenta usar un subproceso en segundo plano para establecer una propiedad del botón donde se hizo clic. La propiedad [Content](http://go.microsoft.com/fwlink/p/?LinkId=256025) del botón inicia una excepción.

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

Puedes acceder al botón de forma segura mediante su propiedad [Dispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256026) o la propiedad `Dispatcher` de cualquier objeto que existe en el contexto del subproceso de la interfaz de usuario (como la página en la que está el botón). El siguiente código usa el método [RunAsync](http://go.microsoft.com/fwlink/p/?LinkId=256030) del objeto [CoreDispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256029) para enviar la llamada en el subproceso de la interfaz de usuario.

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
>  La propiedad `Dispatcher` no inicia una excepción cuando se la llama desde otro subproceso.

La duración de un objeto de Windows Runtime que se crea en el subproceso de la interfaz de usuario está limitada por la duración del subproceso. No intentes acceder a objetos en un subproceso de la interfaz de usuario después de que se cierre la ventana.

Si creas tu propio control al heredar un control XAML o al crear un conjunto de controles XAML, el control es ágil porque es un objeto de .NET Framework. Sin embargo, si llama a miembros de su clase base o de clases constituyentes, o bien, si llamas a miembros heredados, estos miembros iniciarán excepciones cuando se los llame desde cualquier subproceso, excepto el subproceso de la interfaz de usuario.

### <a name="classes-that-cant-be-marshaled"></a>Clases que no pueden serializarse
Las clases de Windows Runtime que no proporcionan información de serialización tienen el atributo [MarshalingBehaviorAttribute](http://go.microsoft.com/fwlink/p/?LinkId=256022) con [MarshalingType.None](http://go.microsoft.com/fwlink/p/?LinkId=256023). La documentación para una clase como esta muestra "MarshalingBehaviorAttribute(None)" entre sus atributos.

El siguiente código crea un objeto [CameraCaptureUI](http://go.microsoft.com/fwlink/p/?LinkId=256027) en el subproceso de la interfaz de usuario y luego intenta establecer una propiedad del objeto de un subproceso del grupo de subprocesos. CLR no puede serializar la llamada e inicia una excepción [System.InvalidCastException](/dotnet/api/system.invalidcastexception) con un mensaje que indica que el objeto puede usarse únicamente en el contexto del subproceso donde se creó.

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

La documentación de [CameraCaptureUI](http://go.microsoft.com/fwlink/p/?LinkId=256027) también enumera "ThreadingAttribute(STA)" entre los atributos de la clase, porque debe crearse en un contexto de subproceso único, como el subproceso de la interfaz de usuario.

Si quieres acceder al objeto [CameraCaptureUI](http://go.microsoft.com/fwlink/p/?LinkId=256027) desde otro subproceso, puedes almacenar en caché el objeto [CoreDispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256029) para el subproceso de la interfaz de usuario y usarlo más adelante para enviar la llamada en ese subproceso. O bien, puedes obtener el distribuidor de un objeto XAML, como la página, como se muestra en el siguiente código.

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
De manera predeterminada, las clases en el componente que pueden activarse son ágiles. Sin embargo, C++ permite un control significativo de los modelos de subprocesos y del comportamiento de serialización. Como se describió anteriormente en este artículo, CLR reconoce clases ágiles, intenta serializar las llamadas cuando las clases no son ágiles e inicia una excepción [System.InvalidCastException](/dotnet/api/system.invalidcastexception) cuando una clase no tiene ninguna información de serialización.

Para los objetos que se ejecutan en el subproceso de la interfaz de usuario e inician excepciones cuando se las llama desde un subproceso distinto del subproceso de la interfaz de usuario, puedes usar el objeto [CoreDispatcher](http://go.microsoft.com/fwlink/p/?LinkId=256029) del subproceso de la interfaz de usuario para enviar la llamada.

## <a name="see-also"></a>Consulta también
[Guía de C#](/dotnet/articles/csharp/)

[Guía de Visual Basic](/dotnet/articles/visual-basic/)
