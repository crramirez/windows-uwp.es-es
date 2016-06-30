---
author: Jwmsft
Description: "Ayuda a tus usuarios a ser más productivos al permitirles ver varias partes independientes de la aplicación en ventanas distintas."
title: "Mostrar varias vistas de una aplicación"
ms.assetid: BAF9956F-FAAF-47FB-A7DB-8557D2548D88
label: Show multiple views for an app
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: 23e999f86fb0552b96cddbd3b9d11803106bf6c2

---

# Mostrar varias vistas de una aplicación

Puedes ayudar a tus usuarios a ser más productivos si les permites ver varias partes independientes de la aplicación en ventanas distintas. Un ejemplo típico es una aplicación de correo electrónico donde la interfaz de usuario principal muestra la lista de mensajes de correo electrónico y una vista previa del correo electrónico seleccionado. No obstante, los usuarios también pueden abrir mensajes en distintas ventanas y verlas en paralelo.

**API importantes**

-   [**ApplicationViewSwitcher**](https://msdn.microsoft.com/library/windows/apps/dn281094)
-   [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278)

Si creas varias ventanas para una aplicación, cada una de ellas se comporta de manera independiente. La barra de tareas muestra cada ventana por separado. Los usuarios pueden mover, cambiar de tamaño, mostrar y ocultar ventanas de la aplicación de manera independiente, así como cambiar entre ventanas de la aplicación como si usaran aplicaciones distintas. Cada ventana funciona en su propio subproceso.

## <span id="What_is_a_view_"></span><span id="what_is_a_view_"></span><span id="WHAT_IS_A_VIEW_"></span>¿Qué es una vista?


Una vista de la aplicación es el emparejamiento 1:1 de un subproceso y una ventana que la aplicación usa para mostrar contenido. Está representada por un objeto [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017).

Las vistas las administra el objeto [**CoreApplication**](https://msdn.microsoft.com/library/windows/apps/br225016). Debes llamar a [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) para crear un objeto [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017). LA **CoreApplicationView** reúne una [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) y un [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) (almacenado en la [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) y las propiedades de [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/dn433264)). Puedes considerar la **CoreApplicationView** como el objeto que Windows Runtime usa para interactuar con el sistema Windows principal.

Normalmente, no trabajas directamente con la [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017). En su lugar, Windows Runtime proporciona la clase [**ApplicationView**](https://msdn.microsoft.com/library/windows/apps/hh701658) en el espacio de nombres [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/br242295). Esta clase ofrece propiedades, métodos y eventos que usas cuando tu aplicación interactúa con el sistema de ventanas. Para trabajar con una **ApplicationView**, llama al método [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) estático, que obtiene una instancia **ApplicationView** vinculada a la conversación actual de la **CoreApplicationView**.

De igual modo, el marco XAML encapsula el objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) en un objeto [**Windows.UI.XAML.Window**](https://msdn.microsoft.com/library/windows/apps/br209041). En una aplicación XAML, sueles interactuar con el objeto **Ventana** en lugar de trabajar directamente con la **CoreWindow**.

## <span id="Show_a_new_view"></span><span id="show_a_new_view"></span><span id="SHOW_A_NEW_VIEW"></span>Mostrar una vista nueva


Antes de avanzar, veamos los pasos para crear una nueva vista. Aquí, la nueva vista se inicia en respuesta al clic de un botón.

```CSharp
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

```    CSharp
CoreApplicationView newView = CoreApplication.CreateNewView();</code></pre></td>
    </tr>
    </tbody>
    </table>
```

2.  Realiza un seguimiento del [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) de la nueva vista. Úsalo para mostrar la vista más adelante.

    Es posible que desees considerar la posibilidad de crear infraestructura en tu aplicación para facilitar el seguimiento de las vistas que crees. Consulta la clase `ViewLifetimeControl` en la [muestra de varias vistas](http://go.microsoft.com/fwlink/p/?LinkId=620574) para ver un ejemplo.

    <span codelanguage="CSharp"></span>
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
int newViewId = 0;</code></pre></td>
    </tr>
    </tbody>
    </table>
```

3.  En el nuevo subproceso, rellena la ventana.

    Debes usar el método [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para programar el trabajo de la conversación de la interfaz de usuario para la nueva vista. Debes usar una [expresión lambda](http://go.microsoft.com/fwlink/p/?LinkId=389615) para pasar una función como un argumento para el método **RunAsync**. El trabajo que realices en la función lambda se realizará en el subproceso de la nueva vista.

    En XAML, lo normal es agregar un [**Marco**](https://msdn.microsoft.com/library/windows/apps/br242682) a la propiedad [**Contenido**](https://msdn.microsoft.com/library/windows/apps/br209051) de la [**Ventana**](https://msdn.microsoft.com/library/windows/apps/br209041) y luego desplazar al **Marco** a una [**Página**](https://msdn.microsoft.com/library/windows/apps/br227503) XAML donde has definido el contenido de la aplicación. Para obtener más información, consulta [Navegación de punto a punto entre dos páginas](peer-to-peer-navigation-between-two-pages.md).

    Después de rellenar la nueva [**Ventana**](https://msdn.microsoft.com/library/windows/apps/br209041) debes llamar al método [**Activar**](https://msdn.microsoft.com/library/windows/apps/br209046) de la **Ventana** para mostrar la **Ventana** más adelante. Este trabajo se realiza en la conversación de la nueva vista, de modo que la nueva **Ventana** se activa.

    Por último, obtén el [**Id**](https://msdn.microsoft.com/library/windows/apps/dn281120) de la nueva vista que vas a usar para mostrar la vista más adelante. De nuevo, este trabajo se realiza en la conversación de la nueva vista, por lo que [**ApplicationView.GetForCurrentView**](https://msdn.microsoft.com/library/windows/apps/hh701672) obtiene el **Id** de la nueva vista.

    <span codelanguage="CSharp"></span>
```    CSharp
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">C#</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
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

```    CSharp
bool viewShown = await ApplicationViewSwitcher.TryShowAsStandaloneAsync(newViewId);</code></pre></td>
    </tr>
    </tbody>
    </table>
```

## <span id="The_main_view"></span><span id="the_main_view"></span><span id="THE_MAIN_VIEW"></span>La vista principal


La primera vista que se crea al iniciar la aplicación se denomina *vista principal*. Esta vista se almacena en la propiedad [**CoreApplication.MainView**](https://msdn.microsoft.com/library/windows/apps/hh700465) y su propiedad [**IsMain**](https://msdn.microsoft.com/library/windows/apps/hh700452) es true. El usuario no crea esta vista, sino que la crea la aplicación. El subproceso de la vista principal sirve de administrador de la aplicación, y todos los eventos de activación de la aplicación se entregan en este subproceso.

Si se abren vistas secundarias, se puede ocultar la ventana de la vista principal (por ejemplo, con un clic en el botón de cierre (x) de la barra de título de la ventana), pero su subproceso permanece activo. Al llamar a [**Cerrar**](https://msdn.microsoft.com/library/windows/apps/br209049) en la [**Ventana**](https://msdn.microsoft.com/library/windows/apps/br209041) de la vista principal se produce una **InvalidOperationException**. (Usa [**Application.Exit**](https://msdn.microsoft.com/library/windows/apps/br242327) para cerrar la aplicación). Si finaliza la conversación principal de la vista, se cierra la aplicación.

## <span id="Secondary_views"></span><span id="secondary_views"></span><span id="SECONDARY_VIEWS"></span>Vistas secundarias


Otras vistas, incluidas todas las vistas que se crean llamando a [**CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278) en el código de la aplicación, son vistas secundarias. La vista principal y las vistas secundarias se almacenan en la colección [**CoreApplication.Views**](https://msdn.microsoft.com/library/windows/apps/br205861). Por lo general, las vistas secundarias se crean en respuesta a una acción del usuario. En algunos casos, el sistema crea vistas secundarias para tu aplicación.

**Nota** Puedes usar la característica *acceso asignado* de Windows para ejecutar una aplicación en [modo de pantalla completa](https://technet.microsoft.com/library/mt219050.aspx). Al hacerlo, el sistema crea una vista secundaria para presentar la interfaz de usuario de la aplicación sobre la pantalla de bloqueo. No se permiten vistas secundarias creadas por la aplicación, por lo que si intentas mostrar tu propia vista secundaria en modo de pantalla completa, se iniciará una excepción.

 

## <span id="Switch_from_one_view_to_another"></span><span id="switch_from_one_view_to_another"></span><span id="SWITCH_FROM_ONE_VIEW_TO_ANOTHER"></span>Cambiar de una vista a otra


Debes ofrecer al usuario una manera de volver de una ventana secundaria a la ventana principal. Para ello, usa el método [**ApplicationViewSwitcher.SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097). Se llama a este método desde el subproceso de la ventana desde la que se cambia y se pasa el identificador de vista de la ventana a la que se cambia.

<span codelanguage="CSharp"></span>
```CSharp
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C#</th>
</tr>
</thead>
<tbody>
<tr class="odd">
await ApplicationViewSwitcher.SwitchAsync(viewIdToShow);</code></pre></td>
</tr>
</tbody>
</table>
```

Cuando uses [**SwitchAsync**](https://msdn.microsoft.com/library/windows/apps/dn281097), puedes elegir si quieres cerrar la ventana inicial y quitarla de la barra de tareas; para ello, especifica el valor de [**ApplicationViewSwitchingOptions**](https://msdn.microsoft.com/library/windows/apps/dn281105).

 

 







<!--HONumber=Jun16_HO4-->


