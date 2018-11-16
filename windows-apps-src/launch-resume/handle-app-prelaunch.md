---
author: TylerMSFT
title: Controlar el inicio previo de aplicaciones
description: Aprende a controlar el inicio previo de aplicaciones al invalidar el método OnLaunched y llamar a CoreApplication.EnablePrelaunch(true).
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a13ec942080d7fe517a10b837bea9ae8fae27750
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7128745"
---
# <a name="handle-app-prelaunch"></a>Administrar el inicio previo de aplicaciones

Obtén información sobre cómo controlar el inicio previo de las aplicaciones mediante el reemplazo del método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

## <a name="introduction"></a>Introducción

Cuando los recursos del sistema disponibles lo permiten, se mejora el rendimiento de inicio de aplicaciones para UWP en dispositivos de familia de dispositivos de escritorio iniciando de forma proactiva aplicaciones usadas con más frecuencia del usuario en segundo plano. Una aplicación iniciada previamente se pone en estado suspendido poco después de iniciarse. Después, cuando el usuario invoca la aplicación, esta pasa del estado de suspensión al de ejecución para reanudarse, lo que resulta más rápido que iniciarla en frío. La experiencia del usuario es que la aplicación se inicia muy rápidamente.

Antes de Windows10, las aplicaciones no aprovechaban el inicio previo automáticamente. En Windows 10, versión 1511, todas las aplicaciones de la plataforma Universal de Windows (UWP) son candidatas para el inicio previo. En Windows 10, versión 1607, debes participar en el comportamiento de inicio previo llamando a [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx). Es un buen lugar para poner esta llamada dentro de `OnLaunched()` cerca de la ubicación en que se realiza la comprobación `if (e.PrelaunchActivated == false)`.

Si una aplicación se inicia previamente depende de los recursos del sistema. Si el sistema experimenta una presión del recurso, las aplicaciones no se inician previamente.

Algunos tipos de aplicaciones pueden necesitar un cambio en el comportamiento de inicio para funcionar con el inicio previo. Por ejemplo, una aplicación que reproduce música cuando se inicia, un juego que asume que el usuario está presente y muestra elaborados elementos visuales cuando se inicia la aplicación, una aplicación de mensajería que cambia la visibilidad del usuario en línea durante el inicio, puede identificar si la aplicación se inició previamente y cambiar su comportamiento de inicio como se describe en las secciones siguientes.

Las plantillas predeterminadas para proyectos XAML (C#, VB, C++) y WinJS permiten el inicio previo en Visual Studio 2015 Update 3.

## <a name="prelaunch-and-the-app-lifecycle"></a>Inicio previo y ciclo de vida de la aplicación

Cuando se hace un inicio previo de una aplicación, esta enseguida entra en estado de suspensión. (Consulta [Controlar la suspensión de la aplicación](suspend-an-app.md)).

## <a name="detect-and-handle-prelaunch"></a>Detectar y controlar el inicio previo

Las aplicaciones reciben la marca [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) durante la activación. Usa esta marca para ejecutar código que solo debe ejecutarse cuando el usuario inicia explícitamente la aplicación, como se muestra en la siguiente modificación [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // CoreApplication.EnablePrelaunch was introduced in Windows 10 version 1607
    bool canEnablePrelaunch = Windows.Foundation.Metadata.ApiInformation.IsMethodPresent("Windows.ApplicationModel.Core.CoreApplication", "EnablePrelaunch");

    // NOTE: Only enable this code if you are targeting a version of Windows 10 prior to version 1607
    // and you want to opt-out of prelaunch.
    // In Windows 10 version 1511, all UWP apps were candidates for prelaunch.
    // Starting in Windows 10 version 1607, the app must opt-in to be prelaunched.
    //if ( !canEnablePrelaunch && e.PrelaunchActivated == true)
    //{
    //    return;
    //}

    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        // On Windows 10 version 1607 or later, this code signals that this app wants to participate in prelaunch
        if (canEnablePrelaunch)
        {
            TryEnablePrelaunch();
        }

        // TODO: This is not a prelaunch activation. Perform operations which
        // assume that the user explicitly launched the app such as updating
        // the online presence of the user on a social network, updating a
        // what's new feed, etc.

        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();
    }
}

/// <summary>
/// Encapsulates the call to CoreApplication.EnablePrelaunch() so that the JIT
/// won't encounter that call (and prevent the app from running when it doesn't
/// find it), unless this method gets called. This method should only
/// be called when the caller determines that we are running on a system that
/// supports CoreApplication.EnablePrelaunch().
/// </summary>
private void TryEnablePrelaunch()
{
    Windows.ApplicationModel.Core.CoreApplication.EnablePrelaunch(true);
}
```

Ten en cuenta la `TryEnablePrelaunch()` funciona, anteriormente. La razón por la llamada a `CoreApplication.EnablePrelaunch()` se incluyen en esta función es porque cuando se llama a un método, intentará JIT (solo en la compilación) compilar todo el método. Si la aplicación se ejecuta en una versión de Windows 10 que no es compatible con `CoreApplication.EnablePrelaunch()`, a continuación, se producirá un error JIT. Mediante la factorización de la llamada a un método que solo se llama cuando la aplicación determina que es compatible con la plataforma `CoreApplication.EnablePrelaunch()`, evitar este problema.

Hay también código en el ejemplo anterior que puede quite si la aplicación debe rechazar el inicio previo cuando se ejecuta en Windows 10, versión 1511. En la versión 1511, todas las aplicaciones para UWP se han optado por automáticamente por en el inicio previo, que puede no ser adecuado para tu aplicación.

## <a name="use-the-visibilitychanged-event"></a>Usar el evento VisibilityChanged

Las aplicaciones que se activan con el inicio previo no son visibles para el usuario. Son visibles cuando el usuario cambia a estas. Quizás quieras retrasar determinadas operaciones hasta que la ventana principal de la aplicación sea visible. Por ejemplo, si la aplicación muestra una lista de elementos de novedades de una fuente, podrías actualizar la lista durante el evento [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458), en lugar de usar la lista que se creó durante el inicio previo de la aplicación, ya que podría estar obsoleta en el momento en que el usuario active la aplicación. El siguiente código controla el evento **VisibilityChanged** de **MainPage**:

```csharp
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

## <a name="directx-games-guidance"></a>Guía de juegos de DirectX

Los juegos de DirectX, por lo general, no deben habilitar el inicio previo porque muchos juegos de DirectX hacen su inicialización antes de que el inicio previo pueda detectarse. A partir de Windows 1607, edición de aniversario, tu juego no se podrá iniciar previamente de manera predeterminada.  Si quieres que el juego aproveche el inicio previo, llama a [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx).

Si el juego está destinado a una versión anterior de Windows 10, puedes controlar la condición de inicio previo para salir de la aplicación:

```cppwinrt
void ViewProvider::OnActivated(CoreApplicationView const& /* appView */, Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Launch)
    {
        auto launchArgs{ args.as<Windows::ApplicationModel::Activation::LaunchActivatedEventArgs>()};
        if (launchArgs.PrelaunchActivated())
        {
            // Opt-out of Prelaunch.
            CoreApplication::Exit();
        }
    }
}

void ViewProvider::Initialize(CoreApplicationView const & appView)
{
    appView.Activated({ this, &App::OnActivated });
}
```

```cpp
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

## <a name="winjs-app-guidance"></a>Guía de aplicaciones de WinJS

Si la aplicación de WinJS está destinada a una versión anterior de Windows 10, puedes controlar la condición de inicio previo desde el controlador [onactivated](https://msdn.microsoft.com/library/windows/apps/br212679.aspx):

```javascript
    app.onactivated = function (args) {
        if (!args.detail.prelaunchActivated) {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }
    }
```

## <a name="general-guidance"></a>Instrucciones generales

-   Las aplicaciones no deben realizar operaciones de ejecución prolongada durante el inicio previo porque la aplicación finalizará si no se puede suspender rápidamente.
-   Las aplicaciones no deben iniciar la reproducción de audio desde [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) si la aplicación ya está iniciada. De lo contrario, la aplicación no será visible y no estará claro por qué se está reproduciendo audio.
-   Durante el inicio, las aplicaciones no deben realizar operaciones que supongan que la aplicación es visible para el usuario o que el usuario la inició explícitamente. Dado que ahora se inicia una aplicación en segundo plano sin ninguna acción explícita del usuario, los desarrolladores deberían tener en cuenta la privacidad, la experiencia del usuario y las implicaciones de rendimiento.
    -   Un ejemplo de consideración de privacidad es la necesidad de que una aplicación social cambie el estado del usuario a conectado. Debería esperar hasta que el usuario cambie a la aplicación en lugar de cambiar el estado en el inicio previo de la aplicación.
    -   Un ejemplo de consideración de la experiencia del usuario es que si tienes una aplicación, como un juego, que muestra una secuencia de introducción al iniciarse, podrías retrasar la secuencia de introducción hasta que el usuario cambie a la aplicación.
    -   Un ejemplo de implicación de rendimiento es que podrías esperar a que el usuario cambie a la aplicación para recuperar la información meteorológica actual, en lugar de cargarla con el inicio previo de la aplicación y tener que volver a cargarla cuando la aplicación sea visible para garantizar que la información esté actualizada.
-   Si la aplicación borra su icono dinámico al iniciarse, aplaza esto hasta el evento de cambio de visibilidad.
-   La telemetría de la aplicación debería distinguir entre las activaciones de icono normales y las de inicio previo para que puedas limitar el escenario en caso de problemas.
-   Si tienes Microsoft Visual Studio2015 Update 1 y Windows 10, versión 1511, puedes simular el inicio previo de la aplicación en Visual Studio2015 seleccionando **Depurar** &gt; **Otros destinos de depuración** &gt; **Depurar la aplicación Universal de Windows El inicio previo**.

## <a name="related-topics"></a>Temas relacionados

* [Ciclo de vida de la aplicación](app-lifecycle.md)
* [CoreApplication.EnablePrelaunch](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)
