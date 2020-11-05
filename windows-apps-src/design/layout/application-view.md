---
description: Usa la clase ApplicationView para ver diferentes partes de la aplicación en ventanas independientes.
title: Uso de la clase ApplicationView para mostrar las ventanas secundarias de una aplicación
ms.date: 07/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2cdee3c0844fc5a01d0749e4e6219d92cd8178a0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034858"
---
# <a name="show-multiple-views-with-applicationview"></a>Mostrar varias vistas con ApplicationView

Ayuda a tus usuarios a ser más productivos al permitirles ver partes independientes de la aplicación en ventanas distintas. Si creas varias ventanas para una aplicación, cada una de ellas se comporta de manera independiente. La barra de tareas muestra cada ventana por separado. Los usuarios pueden mover, cambiar de tamaño, mostrar y ocultar ventanas de la aplicación de manera independiente, así como cambiar entre ventanas de la aplicación como si usaran aplicaciones distintas. Cada ventana funciona en su propio subproceso.

> **API importantes** : [**ApplicationViewSwitcher**](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher), [**CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)

## <a name="what-is-a-view"></a>¿Qué es una vista?

Una vista de la aplicación es el emparejamiento 1:1 de un subproceso y una ventana que la aplicación usa para mostrar contenido. Está representada por un objeto [**Windows.ApplicationModel.Core.CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView).

Las vistas las administra el objeto [**CoreApplication**](/uwp/api/Windows.ApplicationModel.Core.CoreApplication). Llama al método [**CoreApplication.CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) para crear un objeto [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView). LA **CoreApplicationView** reúne una [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) y un [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) (almacenado en la [**CoreWindow**](/uwp/api/windows.applicationmodel.core.coreapplicationview.corewindow) y las propiedades de [**Dispatcher**](/uwp/api/windows.applicationmodel.core.coreapplicationview.dispatcher)). Puedes considerar la **CoreApplicationView** como el objeto que Windows Runtime usa para interactuar con el sistema Windows principal.

Normalmente, no trabajas directamente con la [**CoreApplicationView**](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView). En su lugar, Windows Runtime proporciona la clase [**ApplicationView**](/uwp/api/Windows.UI.ViewManagement.ApplicationView) en el espacio de nombres [**Windows.UI.ViewManagement**](/uwp/api/Windows.UI.ViewManagement). Esta clase ofrece propiedades, métodos y eventos que usas cuando tu aplicación interactúa con el sistema de ventanas. Para trabajar con una **ApplicationView** , llama al método [**ApplicationView.GetForCurrentView**](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) estático, que obtiene una instancia **ApplicationView** vinculada a la conversación actual de la **CoreApplicationView**.

De igual modo, el marco XAML encapsula el objeto [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) en un objeto [**Windows.UI.XAML.Window**](/uwp/api/Windows.UI.Xaml.Window). En una aplicación XAML, sueles interactuar con el objeto **Ventana** en lugar de trabajar directamente con la **CoreWindow**.

## <a name="show-a-new-view"></a>Mostrar una vista nueva

Aunque cada diseño de aplicación es único, te recomendamos que incluyas un botón de "ventana nueva" en una ubicación predecible, como la esquina superior derecha del contenido que se puede abrir en una ventana nueva. También puedes incluir una opción de menú contextual "Abrir en una nueva ventana".

Echemos un vistazo a los pasos para crear una nueva vista. Aquí, la nueva vista se inicia en respuesta al clic de un botón.

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    CoreApplicationView newView = CoreApplication.CreateNewView();
    int newViewId = 0;
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
}
```

**Para mostrar una vista nueva**

1.  Llama a [**CoreApplication.CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) para crear una nueva ventana y una conversación para el contenido de la vista.

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  Realiza un seguimiento del [**Id**](/uwp/api/windows.ui.viewmanagement.applicationview.id) de la nueva vista. Úsalo para mostrar la vista más adelante.

    Es posible que desees considerar la posibilidad de crear infraestructura en tu aplicación para facilitar el seguimiento de las vistas que crees. Consulta la clase `ViewLifetimeControl` en la [muestra de varias vistas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MultipleViews) para ver un ejemplo.

    ```csharp
    int newViewId = 0;
    ```

3.  En el nuevo subproceso, rellena la ventana.

    Debes usar el método [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) para programar el trabajo de la conversación de la interfaz de usuario para la nueva vista. Debes usar una [expresión lambda](/dotnet/csharp/language-reference/operators/lambda-expressions) para pasar una función como un argumento para el método **RunAsync**. El trabajo que realices en la función lambda se realizará en el subproceso de la nueva vista.

    En XAML, lo normal es agregar un elemento [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) a la propiedad [**Content**](/uwp/api/windows.ui.xaml.window.content) del elemento [**Window**](/uwp/api/Windows.UI.Xaml.Window) y luego desplazar al elemento **Frame** a una [**página**](/uwp/api/Windows.UI.Xaml.Controls.Page) XAML donde has definido el contenido de la aplicación. Para obtener más información sobre marcos y páginas, consulta [Navegación de punto a punto entre dos páginas](../basics/navigate-between-two-pages.md).

    Después de rellenar el elemento nuevo [**Window**](/uwp/api/Windows.UI.Xaml.Window) debes llamar al método [**Activate**](/uwp/api/windows.ui.xaml.window.activate) del elemento **Window** para mostrar el elemento **Window** más adelante. Este trabajo se realiza en la conversación de la nueva vista, de modo que la nueva **Ventana** se activa.

    Por último, obtén el [**Id**](/uwp/api/windows.ui.viewmanagement.applicationview.id) de la nueva vista que vas a usar para mostrar la vista más adelante. De nuevo, este trabajo se realiza en la conversación de la nueva vista, por lo que [**ApplicationView.GetForCurrentView**](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) obtiene el **Id** de la nueva vista.

    ```csharp
    await newView.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        Frame frame = new Frame();
        frame.Navigate(typeof(SecondaryPage), null);   
        Window.Current.Content = frame;
        // You have to activate the window in order to show it later.
        Window.Current.Activate();

        newViewId = ApplicationView.GetForCurrentView().Id;
    });
    ```

4.  Para mostrar la nueva vista llama a [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync).

    Después de crear una nueva vista, puedes mostrarla en una ventana nueva mediante una llamada al método [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.tryshowasstandaloneasync). El parámetro *viewId* de este método es un entero que identifica de manera única cada una de las vistas de la aplicación. Para recuperar el [**Id**](/uwp/api/windows.ui.viewmanagement.applicationview.id) de la vista, usa la propiedad **ApplicationView.Id** o el método [**ApplicationView.GetApplicationViewIdForWindow**](/uwp/api/windows.ui.viewmanagement.applicationview.getapplicationviewidforwindow).

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>La vista principal


La primera vista que se crea al iniciar la aplicación se denomina *vista principal*. Esta vista se almacena en la propiedad [**CoreApplication.MainView**](/uwp/api/windows.applicationmodel.core.coreapplication.mainview) y su propiedad [**IsMain**](/uwp/api/windows.applicationmodel.core.coreapplicationview.ismain) es true. El usuario no crea esta vista, sino que la crea la aplicación. El subproceso de la vista principal sirve de administrador de la aplicación, y todos los eventos de activación de la aplicación se entregan en este subproceso.

Si se abren vistas secundarias, se puede ocultar la ventana de la vista principal (por ejemplo, con un clic en el botón de cierre (x) de la barra de título de la ventana), pero su subproceso permanece activo. Al llamar a [**Cerrar**](/uwp/api/windows.ui.xaml.window.close) en la [**Ventana**](/uwp/api/Windows.UI.Xaml.Window) de la vista principal se produce una **InvalidOperationException**. (Usa [**Aplication.Exit**](/uwp/api/windows.ui.xaml.application.exit) para cerrar la aplicación). Si finaliza el subproceso de la vista principal, la aplicación se cierra.

## <a name="secondary-views"></a>Vistas secundarias


Otras vistas, incluidas todas las vistas que se crean llamando a [**CreateNewView**](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview) en el código de la aplicación, son vistas secundarias. La vista principal y las vistas secundarias se almacenan en la colección [**CoreApplication.Views**](/uwp/api/windows.applicationmodel.core.coreapplication.views). Por lo general, las vistas secundarias se crean en respuesta a una acción del usuario. En algunos casos, el sistema crea vistas secundarias para tu aplicación.

> [!NOTE]
> Puedes usar la característica *acceso asignado* de Windows para ejecutar una aplicación en [modo de pantalla completa](/windows/manage/set-up-a-device-for-anyone-to-use). Al hacerlo, el sistema crea una vista secundaria para presentar la interfaz de usuario de la aplicación sobre la pantalla de bloqueo. No se permiten vistas secundarias creadas por la aplicación, por lo que si intentas mostrar tu propia vista secundaria en modo de pantalla completa, se iniciará una excepción.

## <a name="switch-from-one-view-to-another"></a>Cambiar de una vista a otra

Considera la posibilidad de proporcionar al usuario una manera de navegar desde una ventana secundaria a su ventana principal. Para ello, usa el método [**ApplicationViewSwitcher.SwitchAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync). Se llama a este método desde el subproceso de la ventana desde la que se cambia y se pasa el identificador de vista de la ventana a la que se cambia.

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

Cuando uses [**SwitchAsync**](/uwp/api/windows.ui.viewmanagement.applicationviewswitcher.switchasync), puedes elegir si quieres cerrar la ventana inicial y quitarla de la barra de tareas; para ello, especifica el valor de [**ApplicationViewSwitchingOptions**](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitchingOptions).

## <a name="related-topics"></a>Temas relacionados

- [Mostrar varias vistas](show-multiple-views.md)
- [Mostrar varias vistas con AppWindow](app-window.md)
- [ApplicationViewSwitcher](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
