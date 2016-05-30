---
author: mcleblanc
title: Controlar el inicio previo de aplicaciones
description: Obtén información sobre cómo controlar el inicio previo de las aplicaciones mediante el reemplazo del método OnLaunched.
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
---

# Controlar el inicio previo de aplicaciones


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)

Obtén información sobre cómo controlar el inicio previo de las aplicaciones mediante el reemplazo del método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

## Introducción


Cuando los recursos del sistema disponibles lo permiten, el rendimiento de inicio de las aplicaciones de la Tienda Windows mejora al iniciar en segundo plano y de manera proactiva las aplicaciones que el usuario usa con más frecuencia. Una aplicación iniciada previamente se coloca en el estado suspendido poco después de iniciarse. Cuando el usuario invoca la aplicación, esta pasa del estado de suspensión al de ejecución para reanudarse, lo que resulta más rápido que iniciarla en frío.

Antes de Windows 10, las aplicaciones no aprovechaban el inicio previo automáticamente. A partir de Windows 10, todas las aplicaciones para la Plataforma universal de Windows (UWP) lo usan automáticamente.

La mayoría de las aplicaciones funcionan con el inicio previo sin realizar cambios. Sin embargo, algunos tipos de aplicaciones pueden necesitar un cambio en el comportamiento de inicio para funcionar con el inicio previo. Por ejemplo, una aplicación de mensajería que cambia la visibilidad en línea de los usuarios durante el inicio o un juego que supone que el usuario está presente y muestra los elementos visuales elaborados cuando se inicia la aplicación.

## Inicio previo y ciclo de vida de la aplicación


Cuando se hace un inicio previo de una aplicación, esta enseguida entra en estado de suspensión. (Consulta [Controlar la suspensión de la aplicación](suspend-an-app.md)).

## Detectar y controlar el inicio previo


Las aplicaciones reciben la marca [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) durante la activación. Usa esta marca para determinar si se realizarán operaciones que solo deberían realizarse cuando el usuario inicia explícitamente la aplicación, como se muestra en el siguiente fragmento de [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335).

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

**Sugerencia**  Si prefieres no usar el inicio previo, activa la marca [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740). Si se activa, vuelve de OnLaunched() antes de hacer cualquier trabajo para crear un marco o activar la ventana.

 

## Usar el evento VisibilityChanged


Las aplicaciones que se activan con el inicio previo no son visibles para el usuario. Son visibles cuando el usuario cambia a estas. Quizás quieras retrasar determinadas operaciones hasta que la ventana principal de la aplicación sea visible. Por ejemplo, si la aplicación muestra una lista de elementos de novedades de una fuente, podrías actualizar la lista durante el evento [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458), en lugar de basarte en una lista que se creó durante el inicio previo de la aplicación y que podría estar obsoleta en el momento en que el usuario active la aplicación. El siguiente código controla el evento **VisibilityChanged** de **MainPage**:

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

## Guía


-   Las aplicaciones no deben realizar operaciones de ejecución prolongada durante el inicio previo porque la aplicación finalizará si no se puede suspender rápidamente.
-   Las aplicaciones no deben iniciar la reproducción de audio desde [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) si la aplicación ya está iniciada. De lo contrario, la aplicación no será visible y no estará claro por qué se está reproduciendo audio.
-   Durante el inicio, las aplicaciones no deben realizar operaciones que supongan que la aplicación es visible para el usuario o que el usuario la inició explícitamente. Dado que ahora se inicia una aplicación en segundo plano sin ninguna acción explícita del usuario, los desarrolladores deberían tener en cuenta la privacidad, la experiencia del usuario y las implicaciones de rendimiento.
    -   Un ejemplo de consideración de privacidad es la necesidad de que una aplicación social cambie el estado del usuario a conectado. Debería esperar hasta que el usuario cambie a la aplicación en lugar de cambiar el estado en el inicio previo de la aplicación.
    -   Un ejemplo de consideración de la experiencia del usuario es que si tienes una aplicación, como un juego, que muestra una secuencia de introducción al iniciarse, podrías retrasar la secuencia de introducción hasta que el usuario cambie a la aplicación.
    -   Un ejemplo de implicación de rendimiento es que podrías esperar a que el usuario cambie a la aplicación para recuperar la información meteorológica actual, en lugar de cargarla con el inicio previo de la aplicación y tener que volver a cargarla cuando la aplicación sea visible para garantizar que la información esté actualizada.
-   Si la aplicación borra su icono dinámico al iniciarse, aplázala hasta el evento de cambio de visibilidad.
-   La telemetría de la aplicación deberá distinguir entre las activaciones de icono normales y las de inicio previo para que puedas identificar el escenario donde ocurren los problemas.
-   Si tienes Microsoft Visual Studio 2015 Update 1 y Windows 10, versión 1511, puedes simular el inicio previo de tu aplicación en Visual Studio 2015. Para ello, elige **Depurar**&gt;**Otros destinos de depuración**&gt;**Depurar inicio previo de la aplicación universal de Windows**.

## Temas relacionados

* [Ciclo de vida de la aplicación](app-lifecycle.md)

 

 





<!--HONumber=May16_HO2-->


