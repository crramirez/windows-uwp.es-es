---
author: TylerMSFT
title: Controlar el inicio previo de aplicaciones
description: "Aprende a controlar el inicio previo de aplicaciones al invalidar el método OnLaunched y llamar a CoreApplication.EnablePrelaunch(true)."
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
translationtype: Human Translation
ms.sourcegitcommit: ea9aa37e15dbb6c977b0c0be4f91f77f3879e622
ms.openlocfilehash: cf7cb9f81207f4f25eb8e78283079df27f83d7dc

---

# Administrar el inicio previo de aplicaciones

\[ Actualizado para aplicaciones para UWP en Windows10. Para leer más artículos sobre Windows8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Obtén información sobre cómo controlar el inicio previo de las aplicaciones mediante el reemplazo del método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

## Introducción

Cuando los recursos del sistema disponibles lo permiten, el rendimiento de inicio de las aplicaciones de la Tienda Windows en dispositivos de la familia de dispositivos de escritorio mejora al iniciar en segundo plano y de manera proactiva las aplicaciones que el usuario usa con más frecuencia. Una aplicación iniciada previamente se pone en estado suspendido poco después de iniciarse. Después, cuando el usuario invoca la aplicación, esta pasa del estado de suspensión al de ejecución para reanudarse, lo que resulta más rápido que iniciarla en frío. La experiencia del usuario es que la aplicación se inicia muy rápidamente.

Antes de Windows10, las aplicaciones no aprovechaban el inicio previo automáticamente. En Windows 10, versión 1511, todas las aplicaciones de la Plataforma universal de Windows (UWP) son candidatas para el inicio previo. En Windows 10, versión 1607, debes participar en el comportamiento de inicio previo llamando a [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx). Es un buen lugar para poner esta llamada dentro de `OnLaunched()` cerca de la ubicación en que se realiza la comprobación `if (e.PrelaunchActivated == false)`.

Si una aplicación se inicia previamente depende de los recursos del sistema. Si el sistema experimenta una presión del recurso, las aplicaciones no se inician previamente.

Algunos tipos de aplicaciones pueden necesitar un cambio en el comportamiento de inicio para funcionar con el inicio previo. Por ejemplo, una aplicación que reproduce música cuando se inicia, un juego que asume que el usuario está presente y muestra elaborados elementos visuales cuando se inicia la aplicación, una aplicación de mensajería que cambia la visibilidad del usuario en línea durante el inicio, puede identificar si la aplicación se inició previamente y cambiar su comportamiento de inicio como se describe en las secciones siguientes.

Las plantillas predeterminadas para proyectos XAML (C#, VB, C++) y WinJS permiten el inicio previo en Visual Studio 2015 Update 3.

## Inicio previo y ciclo de vida de la aplicación

Cuando se hace un inicio previo de una aplicación, esta enseguida entra en estado de suspensión. (Consulta [Controlar la suspensión de la aplicación](suspend-an-app.md)).

## Detectar y controlar el inicio previo

Las aplicaciones reciben la marca [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) durante la activación. Usa esta marca para ejecutar código que solo debe ejecutarse cuando el usuario inicia explícitamente la aplicación, como se muestra en el siguiente fragmento de [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

```cs
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content - rather just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        if (!e.PrelaunchActivated)
        {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation parameter
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Sugerencia** Si tu aplicación se dirige a una versión de Windows 10 anterior a la versión 1607 y prefieres no usar el inicio previo, activa [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740). Si se activa, vuelve de OnLaunched() antes de hacer cualquier trabajo para crear un marco o activar la ventana.

## Usar el evento VisibilityChanged

Las aplicaciones que se activan con el inicio previo no son visibles para el usuario. Son visibles cuando el usuario cambia a estas. Quizás quieras retrasar determinadas operaciones hasta que la ventana principal de la aplicación sea visible. Por ejemplo, si la aplicación muestra una lista de elementos de novedades de una fuente, podrías actualizar la lista durante el evento [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458), en lugar de usar la lista que se creó durante el inicio previo de la aplicación, ya que podría estar obsoleta en el momento en que el usuario active la aplicación. El siguiente código controla el evento **VisibilityChanged** de **MainPage**:

```cs
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than
        // when it is prelaunched, such as building a what's new feed
    }
}
```

## Guía de juegos de DirectX

Los juegos de DirectX, por lo general, no deben habilitar el inicio previo porque muchos juegos de DirectX hacen su inicialización antes de que el inicio previo pueda detectarse. A partir de Windows 1607, edición de aniversario, tu juego no se podrá iniciar previamente de manera predeterminada.  Si quieres que el juego aproveche el inicio previo, llama a [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx).

Si el juego está destinado a una versión anterior de Windows 10, puedes controlar la condición de inicio previo para salir de la aplicación:

```cs
void ViewProvider::OnActivated(CoreApplicationView^ appView,IActivatedEventArgs^ args)
{
    if (args->Kind == ActivationKind::Launch)
    {
        auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args);
        if (launchArgs->PrelaunchActivated)
        {
            // Opt-out of Prelaunch
            CoreApplication::Exit();
            return;
        }
    }
}
```

## Guía de aplicaciones de WinJS

Si la aplicación de WinJS está destinada a una versión anterior de Windows 10, puedes controlar la condición de inicio previo desde el controlador [onactivated](https://msdn.microsoft.com/en-us/library/windows/apps/br212679.aspx):

```js
    app.onactivated = function (args) {
        if (!args.detail.prelaunchActivated) {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }
    }
```

## Instrucciones generales

-   Las aplicaciones no deben realizar operaciones de ejecución prolongada durante el inicio previo porque la aplicación finalizará si no se puede suspender rápidamente.
-   Las aplicaciones no deben iniciar la reproducción de audio desde [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) si la aplicación ya está iniciada. De lo contrario, la aplicación no será visible y no estará claro por qué se está reproduciendo audio.
-   Durante el inicio, las aplicaciones no deben realizar operaciones que supongan que la aplicación es visible para el usuario o que el usuario la inició explícitamente. Dado que ahora se inicia una aplicación en segundo plano sin ninguna acción explícita del usuario, los desarrolladores deberían tener en cuenta la privacidad, la experiencia del usuario y las implicaciones de rendimiento.
    -   Un ejemplo de consideración de privacidad es la necesidad de que una aplicación social cambie el estado del usuario a conectado. Debería esperar hasta que el usuario cambie a la aplicación en lugar de cambiar el estado en el inicio previo de la aplicación.
    -   Un ejemplo de consideración de la experiencia del usuario es que si tienes una aplicación, como un juego, que muestra una secuencia de introducción al iniciarse, podrías retrasar la secuencia de introducción hasta que el usuario cambie a la aplicación.
    -   Un ejemplo de implicación de rendimiento es que podrías esperar a que el usuario cambie a la aplicación para recuperar la información meteorológica actual, en lugar de cargarla con el inicio previo de la aplicación y tener que volver a cargarla cuando la aplicación sea visible para garantizar que la información esté actualizada.
-   Si la aplicación borra su icono dinámico al iniciarse, aplaza esto hasta el evento de cambio de visibilidad.
-   La telemetría de la aplicación debería distinguir entre las activaciones de icono normales y las de inicio previo para que puedas limitar el escenario en caso de problemas.
-   Si tienes Microsoft Visual Studio 2015 Update 1 y Windows10, versión 1511, puedes simular el inicio previo de la aplicación en Visual Studio2015. Para ello, elige **Depurar** &gt; **Otros destinos de depuración** &gt; **Depurar inicio previo de la aplicación universal de Windows**.

## Temas relacionados

* [Ciclo de vida de la aplicación](app-lifecycle.md)
* [CoreApplication.EnablePrelaunch](https://msdn.microsoft.com/en-us/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)



<!--HONumber=Aug16_HO4-->


