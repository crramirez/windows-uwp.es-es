---
Description: View multiple parts of your app in separate windows.
title: Mostrar varias vistas de una aplicación
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 107c904dc4b89941c0f453efd830504d2d032534
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8481582"
---
# <a name="show-multiple-views-for-an-app"></a>Mostrar varias vistas de una aplicación

![Trama reticular que muestra una aplicación con varias ventanas](images/multi-view.gif)

Ayuda a tus usuarios a ser más productivos al permitirles ver partes independientes de la aplicación en ventanas distintas. Si creas varias ventanas para una aplicación, cada una de ellas se comporta de manera independiente. La barra de tareas muestra cada ventana por separado. Los usuarios pueden mover, cambiar de tamaño, mostrar y ocultar ventanas de la aplicación de manera independiente, así como cambiar entre ventanas de la aplicación como si usaran aplicaciones distintas. Cada ventana funciona en su propio subproceso.

> **API importantes**: [**ApplicationViewSwitcher**](https://msdn.microsoft.com/library/windows/apps/dn281094), [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)

## <a name="when-should-an-app-use-multiple-views"></a>¿Cuándo debe una aplicación usar varias vistas?
Hay una variedad de escenarios que pueden beneficiarse de varias vistas. Estos son algunos ejemplos:
 - Una aplicación de correo electrónico que permite a los usuarios ver una lista de mensajes recibidos al redactar un correo electrónico nuevo
 - Una aplicación de la libreta de direcciones que permite a los usuarios comparar la información de contacto de varias personas en paralelo
 - Una aplicación de reproducción de música que permite a los usuarios ver lo que se está reproduciendo mientras exploras una lista de otra música disponible
 - Una aplicación de toma de notas que permite a los usuarios copiar información de una página de notas en otra
 - Una aplicación de lectura que permite a los usuarios abrir varios artículos para leerlos más tarde, después de la oportunidad de examinar todos los titulares de nivel superior

Para crear instancias independientes de tu aplicación, consulta [Crear una aplicación para UWP de varias instancias](../../launch-resume/multi-instance-uwp.md).

## <a name="what-is-a-view"></a>¿Qué es una vista?

Una vista de la aplicación es el emparejamiento 1:1 de un subproceso y una ventana que la aplicación usa para mostrar contenido. Está representada por un objeto [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017).

Las vistas las administra el objeto [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016). Llama al método [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) para crear un objeto [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017). LA **CoreApplicationView** reúne una [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) y un [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) (almacenado en la [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) y las propiedades de [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/dn433264)). Puedes considerar la **CoreApplicationView** como el objeto que Windows Runtime usa para interactuar con el sistema Windows principal.

Normalmente, no trabajas directamente con la [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017). En su lugar, Windows Runtime proporciona la clase [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/hh701658) en el espacio de nombres [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/br242295). Esta clase ofrece propiedades, métodos y eventos que usas cuando tu aplicación interactúa con el sistema de ventanas. Para trabajar con una **ApplicationView**, llama al método [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) estático, que obtiene una instancia **ApplicationView** vinculada a la conversación actual de la **CoreApplicationView**.

De igual modo, el marco XAML encapsula el objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) en un objeto [**Windows.UI.XAML.Window**](https://msdn.microsoft.com/library/windows/apps/br209041). En una aplicación XAML, sueles interactuar con el objeto **Ventana** en lugar de trabajar directamente con la **CoreWindow**.

## <a name="show-a-new-view"></a>Mostrar una vista nueva

Aunque cada diseño de aplicación es único, te recomendamos que incluyas un botón de "ventana nueva" en una ubicación predecible, como la esquina superior derecha del contenido que se puede abrir en una ventana nueva. También puedes incluir una opción de menú contextual para "Abrir en una nueva ventana".

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

1.  Llama a [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297291) para crear una nueva ventana y una conversación para el contenido de la vista.

    ```csharp
    CoreApplicationView newView = CoreApplication.CreateNewView();
    ```

2.  Realiza un seguimiento del [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) de la nueva vista. Úsalo para mostrar la vista más adelante.

    Es posible que desees considerar la posibilidad de crear infraestructura en tu aplicación para facilitar el seguimiento de las vistas que crees. Consulta la clase `ViewLifetimeControl` en la [muestra de varias vistas](http://go.microsoft.com/fwlink/p/?LinkId=620574) para ver un ejemplo.

    ```csharp
    int newViewId = 0;
    ```

3.  En el nuevo subproceso, rellena la ventana.

    Debes usar el método [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para programar el trabajo de la conversación de la interfaz de usuario para la nueva vista. Debes usar una [expresión lambda](http://go.microsoft.com/fwlink/p/?LinkId=389615) para pasar una función como un argumento para el método **RunAsync**. El trabajo que realices en la función lambda se realizará en el subproceso de la nueva vista.

    En XAML, lo normal es agregar un [**Marco**](https://msdn.microsoft.com/library/windows/apps/br242682) a la propiedad [**Contenido**](https://msdn.microsoft.com/library/windows/apps/br209051) de la [**Ventana**](https://msdn.microsoft.com/library/windows/apps/br209041) y luego desplazar al **Marco** a una [**Página**](https://msdn.microsoft.com/library/windows/apps/br227503) XAML donde has definido el contenido de la aplicación. Para obtener más información, consulta [Navegación de punto a punto entre dos páginas](../basics/navigate-between-two-pages.md).

    Después de rellenar la nueva [**Ventana**](https://msdn.microsoft.com/library/windows/apps/br209041) debes llamar al método [**Activar**](https://msdn.microsoft.com/library/windows/apps/br209046) de la **Ventana** para mostrar la **Ventana** más adelante. Este trabajo se realiza en la conversación de la nueva vista, de modo que la nueva **Ventana** se activa.

    Por último, obtén el [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) de la nueva vista que vas a usar para mostrar la vista más adelante. De nuevo, este trabajo se realiza en la conversación de la nueva vista, por lo que [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) obtiene el **Id** de la nueva vista.

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

4.  Para mostrar la nueva vista llama a [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101).

    Después de crear una nueva vista, puedes mostrarla en una ventana nueva mediante una llamada al método [**ApplicationViewSwitcher.TryShowAsStandaloneAsync**](https://msdn.microsoft.com/library/windows/apps/dn281101). El parámetro *viewId* de este método es un entero que identifica de manera única cada una de las vistas de la aplicación. Para recuperar el [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) de la vista, usa la propiedad **ApplicationView.Id** o el método [**ApplicationView.GetApplicationViewIdForWindow**](https://msdn.microsoft.com/library/windows/apps/dn281109).

    ```csharp
    bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);
    ```

## <a name="the-main-view"></a>La vista principal


La primera vista que se crea al iniciar la aplicación se denomina *vista principal*. Esta vista se almacena en la propiedad [**CoreApplication.MainView**](https://msdn.microsoft.com/library/windows/apps/hh700465) y su propiedad [**IsMain**](https://msdn.microsoft.com/library/windows/apps/hh700452) es true. El usuario no crea esta vista, sino que la crea la aplicación. El subproceso de la vista principal sirve de administrador de la aplicación, y todos los eventos de activación de la aplicación se entregan en este subproceso.

Si se abren vistas secundarias, se puede ocultar la ventana de la vista principal (por ejemplo, con un clic en el botón de cierre (x) de la barra de título de la ventana), pero su subproceso permanece activo. Al llamar a [**Cerrar**](https://msdn.microsoft.com/library/windows/apps/br209049) en la [**Ventana**](https://msdn.microsoft.com/library/windows/apps/br209041) de la vista principal se produce una **InvalidOperationException**. (Usa [**Application.Exit**](https://msdn.microsoft.com/library/windows/apps/br242327) para cerrar la aplicación). Si finaliza la conversación principal de la vista, se cierra la aplicación.

## <a name="secondary-views"></a>Vistas secundarias


Otras vistas, incluidas todas las vistas que se crean llamando a [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) en el código de la aplicación, son vistas secundarias. La vista principal y las vistas secundarias se almacenan en la colección [**CoreApplication.Views**](https://msdn.microsoft.com/library/windows/apps/br205861). Por lo general, las vistas secundarias se crean en respuesta a una acción del usuario. En algunos casos, el sistema crea vistas secundarias para tu aplicación.

> [!NOTE]
> Puedes usar la característica *acceso asignado* de Windows para ejecutar una aplicación en [modo de pantalla completa](https://technet.microsoft.com/library/mt219050.aspx). Al hacerlo, el sistema crea una vista secundaria para presentar la interfaz de usuario de la aplicación sobre la pantalla de bloqueo. No se permiten vistas secundarias creadas por la aplicación, por lo que si intentas mostrar tu propia vista secundaria en modo de pantalla completa, se iniciará una excepción.

## <a name="switch-from-one-view-to-another"></a>Cambiar de una vista a otra

Toma en consideración proporcionar al usuario una manera de navegar desde una ventana secundaria a su ventana principal. Para ello, usa el método [**ApplicationViewSwitcher.SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097). Se llama a este método desde el subproceso de la ventana desde la que se cambia y se pasa el identificador de vista de la ventana a la que se cambia.

```csharp
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);
```

Cuando uses [**SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097), puedes elegir si quieres cerrar la ventana inicial y quitarla de la barra de tareas; para ello, especifica el valor de [**ApplicationViewSwitchingOptions**](https://msdn.microsoft.com/library/windows/apps/dn281105).

## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer

* No proporciones un punto de entrada clara a la vista secundaria usando el glifo de "abrir ventana nueva".
* No comuniques el objetivo de la vista secundaria a los usuarios.
* Asegúrate de que tu aplicación es totalmente funcional en una sola vista y que los usuarios solo abrirán una vista secundaria para mayor comodidad.
* No confíes en la vista secundaria para proporcionar notificaciones u otros tipos de visualizaciones transitorias.

## <a name="related-topics"></a>Temas relacionados

* [ApplicationViewSwitcher](https://msdn.microsoft.com/library/windows/apps/dn281094)
* [CreateNewView](https://msdn.microsoft.com/library/windows/apps/dn297278)
 
