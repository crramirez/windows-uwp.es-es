---
author: TylerMSFT
title: "Controlar la activación de aplicaciones"
description: "Obtén información sobre cómo controlar la activación de aplicaciones mediante la invalidación del método OnLaunched."
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
ms.sourcegitcommit: fb83213a4ce58285dae94da97fa20d397468bdc9
ms.openlocfilehash: f47a3b7fcb4bec4138e11a079c3d10e918c1eb95

---

# Controlar la activación de aplicaciones


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

Obtén información sobre cómo controlar la activación de aplicaciones al invalidar el método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

## Invalidar el controlador de inicio

Cuando se activa una aplicación, por cualquier motivo, el sistema envía el evento [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018). Para ver una lista de los tipos de activación, consulta la enumeración [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

La clase [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) define los métodos que puedes invalidar para controlar los distintos tipos de activación. Varios de los tipos de activación tienen un método específico que puedes invalidar. Para los demás tipos de activación, invalida el método [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330).

Define la clase de la aplicación.

```xml
<Application xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AppName.App" >
```

Invalida el método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335). Este método se llama cada vez que el usuario inicia la aplicación. El parámetro [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) contiene el estado anterior de la aplicación y los argumentos de activación.


            **Nota**: En las aplicaciones de la Tienda de Windows Phone, se invoca este método cada vez que el usuario inicia la aplicación desde el icono Inicio o la lista de aplicaciones, incluso si la aplicación está suspendida en la memoria en ese momento. En Windows, al iniciar una aplicación suspendida desde el icono Inicio o la lista de aplicaciones, no se llama a este método.

> [!div class="tabbedCodeSnippets"]
> ```cs
> using System;
> using Windows.ApplicationModel.Activation;
> using Windows.UI.Xaml;
>
> namespace AppName
> {
>    public partial class App
>    {
>       async protected override void OnLaunched(LaunchActivatedEventArgs args)
>       {
>          EnsurePageCreatedAndActivate();
>       }
>
>       // Creates the MainPage if it isn't already created.  Also activates
>       // the window so it takes foreground and input focus.
>       private MainPage EnsurePageCreatedAndActivate()
>       {
>          if (Window.Current.Content == null)
>          {
>              Window.Current.Content = new MainPage();
>          }
>
>          Window.Current.Activate();
>          return Window.Current.Content as MainPage;
>       }
>    }
> }
> ```
> ```vb
> Class App
>    Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
>       Window.Current.Content = New MainPage()
>       Window.Current.Activate()
>    End Sub
> End Class
> ```
> ```cpp
> using namespace Windows::ApplicationModel::Activation;
> using namespace Windows::Foundation;
> using namespace Windows::UI::Xaml;
> using namespace AppName;
> void App::OnLaunched(LaunchActivatedEventArgs^ args)
> {
>    EnsurePageCreatedAndActivate();
> }
>
> // Creates the MainPage if it isn't already created.  Also activates
> // the window so it takes foreground and input focus.
> void App::EnsurePageCreatedAndActivate()
> {
>     if (_mainPage == nullptr)
>     {
>         // Save the MainPage for use if we get activated later
>         _mainPage = ref new MainPage();
>     }
>     Window::Current->Content = _mainPage;
>     Window::Current->Activate();
> }
> ```

## [!div class="tabbedCodeSnippets"]


Restaurar los datos de la aplicación si se suspendió y después finalizó Cuando el usuario cambia a la aplicación finalizada, el sistema envía el evento [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018), con el objeto [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) establecido en **Launch** y el objeto [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) establecido en **Terminated** o **ClosedByUser**.

> [!div class="tabbedCodeSnippets"]
> ```cs
> async protected override void OnLaunched(LaunchActivatedEventArgs args)
> {
>    if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
>        args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
>    {
>       // TODO: Populate the UI with the previously saved application data
>    }
>    else
>    {
>       // TODO: Populate the UI with defaults
>    }
>
>    EnsurePageCreatedAndActivate();
> }
> ```
> ```vb
> Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
>    Dim restoreState As Boolean = False
>
>    Select Case args.PreviousExecutionState
>       Case ApplicationExecutionState.Terminated
>          ' TODO: Populate the UI with the previously saved application data
>          restoreState = True
>       Case ApplicationExecutionState.ClosedByUser
>          ' TODO: Populate the UI with the previously saved application data
>          restoreState = True
>       Case Else
>          ' TODO: Populate the UI with defaults
>    End Select
>
>    Window.Current.Content = New MainPage(restoreState)
>    Window.Current.Activate()
> End Sub
> ```
> ```cpp
> void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
> {
>    if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
>        args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
>    {
>       // TODO: Populate the UI with the previously saved application data
>    }
>    else
>    {
>       // TODO: Populate the UI with defaults
>    }
>
>    EnsurePageCreatedAndActivate();
> }
> ```

La aplicación debe cargar sus datos de aplicación guardados y actualizar el contenido que muestra.

## [!div class="tabbedCodeSnippets"]

> Si el valor de [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) es **NotRunning**, la aplicación no pudo guardar sus datos de aplicación correctamente y debe iniciarse desde cero, como si se estuviera iniciando por primera vez. Comentarios 
            **Nota**  En las aplicaciones de la Tienda de WindowsPhone, el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) siempre va seguido del método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335), aunque la aplicación esté suspendida en ese momento y el usuario la reinicie desde un icono principal o una lista de aplicaciones.

## Las aplicaciones pueden omitir la inicialización si ya hay contenido establecido en la ventana actual.

* [Puedes comprobar la propiedad [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) para determinar si la aplicación se ha iniciado desde un icono principal o secundario y, según esta información, decidir si quieres presentar una experiencia de aplicación nueva o reanudar la existente.](suspend-an-app.md)
* [Temas relacionados](resume-an-app.md)
* [Controlar la suspensión de la aplicación](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Controlar la reanudación de la aplicación](app-lifecycle.md)

**Directrices para suspender y reanudar una aplicación**

* [**Ciclo de vida de la aplicación**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Referencia**](https://msdn.microsoft.com/library/windows/apps/br242324)

 

 



<!--HONumber=Jun16_HO5-->


