---
pm-contact: kisai
design-contact: ksulliv
dev-contact: Shmazlou
doc-status: Published
description: Obtenga información sobre cómo usar los comandos de deslizar con el dedo como acelerador táctil para los menús contextuales, de modo que los usuarios puedan realizar la misma operación en varios elementos en una sucesión rápida.
title: Deslizar rápidamente
label: Swipe
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f81e440670ccee34269ddbe5d55d93637b8d89df
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043557"
---
# <a name="swipe"></a>Deslizar rápidamente

Los comandos de deslizar el dedo son aceleradores para menús contextuales que permiten a los usuarios acceder fácilmente a las acciones de menú comunes de forma táctil, sin necesidad de cambiar los estados de la aplicación.

> **API de la biblioteca de interfaz de usuario de Windows**: [SwipeControl](/uwp/api/microsoft.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/microsoft.ui.xaml.controls.swipeitem)
>
> **API de plataforma**: [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem), [clase ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)

![Ejecutar y mostrar el tema claro](images/LightThemeSwipe.png)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Los comandos de deslizar el dedo ahorran espacio. Resultan útiles en situaciones en las que el usuario puede realizar la misma operación en varios elementos rápidamente. Y proporcionan "acciones rápidas" en los elementos que no necesitan un cambio de estado ni que aparezca un cuadro de diálogo en la página.

Debes usar los comandos de deslizar el dedo cuando haya un grupo de elementos potencialmente grande y cada elemento tenga de 1 a 3 acciones que un usuario quizá quiera realizar habitualmente. Estas acciones pueden incluir, entre otras:

- Eliminando
- Marcar o archivar
- Guardar o descargar
- Responder

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/SwipeControl">abrir la aplicación y ver SwipeControl en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev015/player]

## <a name="how-does-swipe-work"></a>¿Cómo funciona Deslizar el dedo?

Los comandos de deslizar el dedo para UWP tienen dos modos: [Mostrar](/uwp/api/windows.ui.xaml.controls.swipemode) y [Ejecutar](/uwp/api/windows.ui.xaml.controls.swipemode). También admiten cuatro direcciones distintas para deslizar el dedo: arriba, abajo, izquierda y derecha.

### <a name="reveal-mode"></a>Modo Mostrar

En el modo Mostrar, el usuario desliza el dedo por un elemento para abrir un menú de uno o varios comandos y debe pulsar explícitamente un comando para ejecutarlo. Cuando el usuario desliza el dedo por un elemento y lo suelta, el menú permanece abierto hasta que se selecciona un comando o el menú se vuelve a cerrar deslizando el dedo al lado contrario, dejando de pulsar o desplazando el elemento abierto fuera de la pantalla.

![Deslizar el dedo para mostrar](images/SwipeCommand-Reveal_v2.gif)

El modo Mostrar es un modo más seguro y versátil de deslizar el dedo, y puede usarse para la mayoría de los tipos de acción de menú, incluso acciones potencialmente destructivas, como una eliminación.

Cuando el usuario selecciona una de las opciones de menú que se muestran en el estado de abierto y descansando de Mostrar, se invoca el comando de ese elemento y se cierra el control Deslizar el dedo.

### <a name="execute-mode"></a>Modo Ejecutar

En el modo Ejecutar, el usuario desliza el dedo por un elemento abierto para mostrar y ejecutar un único comando con ese único movimiento. Si el usuario suelta el elemento que está deslizando antes de pasar un umbral, el menú se cierra y el comando no se ejecuta. Si el usuario desliza el dedo más allá del umbral y luego lo suelta, el comando se ejecuta inmediatamente.

![Deslizar el dedo para ejecutar](images/SwipeCommand_Delete_v2.gif)

Si el usuario no suelta el dedo tras llegar el umbral y vuelve a cerrar el elemento, el comando no se ejecuta y no se realiza ninguna acción con el elemento.

El modo Ejecutar proporciona más comentarios visuales a través de la orientación de etiquetas y color mientras se desliza el dedo por un elemento.

Se recomienda usar el modo Ejecutar cuando la acción que el usuario realiza es la más común.

También puede usarse para acciones más destructivas, como eliminar un elemento. Sin embargo, ten en cuenta que para Ejecutar basta con un solo deslizamiento en una dirección, en lugar de Mostrar, que requiere que el usuario haga clic explícitamente en un botón.

### <a name="swipe-directions"></a>Direcciones de deslizar el dedo

Deslizar el dedo funciona en todas las direcciones cardinales: arriba, abajo, izquierda y derecha. Cada dirección para deslizar el dedo puede contener sus propios elementos o contenido, pero solo puede establecerse una dirección a la vez en un único elemento con deslizamiento.

Por ejemplo, no puedes tener dos definiciones [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems) en el mismo SwipeControl.

## <a name="how-to-create-a-swipe-command"></a>Cómo crear un comando de deslizar el dedo

Los comandos de deslizar el dedo tienen dos componentes que hay que definir:

- [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), que se ajusta alrededor del contenido. En una colección, como ListView, se encuentra dentro de DataTemplate.
- Los elementos de menú de deslizar con el dedo, es decir, uno o más objetos [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem) colocados en contenedores direccionales del control de deslizamiento con el dedo: [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems), [RightItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.RightItems), [TopItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.TopItems) o [BottomItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.BottomItems)

El contenido del deslizamiento con el dedo puede colocarse en línea o definirse en la sección de recursos de la página o aplicación.

Este es un ejemplo sencillo de un SwipeControl alrededor de un texto. Muestra la jerarquía de elementos XAML necesaria para crear un comando de deslizar el dedo.

```xaml
<SwipeControl HorizontalAlignment="Center" VerticalAlignment="Center">
    <SwipeControl.LeftItems>
        <SwipeItems>
            <SwipeItem Text="Pin">
                <SwipeItem.IconSource>
                    <SymbolIconSource Symbol="Pin"/>
                </SwipeItem.IconSource>
            </SwipeItem>
        </SwipeItems>
    </SwipeControl.LeftItems>

     <!-- Swipeable content -->
    <Border Width="180" Height="44" BorderBrush="Black" BorderThickness="2">
        <TextBlock Text="Swipe to Pin" Margin="4,8,0,0"/>
    </Border>
</SwipeControl>
```

Ahora echaremos un vistazo a un ejemplo más completo de cómo usar normalmente comandos para deslizar el dedo en una lista. En este ejemplo, configurarás un comando Eliminar que use el modo Ejecutar y un menú de otros comandos que usa el modo Mostrar. Ambos conjuntos de comandos se definen en la sección Recursos de la página. Aplicarás los comandos para deslizar el dedo a los elementos de una ListView.

Primero, crea los elementos de deslizamiento del dedo, que representan los comandos, como recursos de nivel de página. SwipeItem usa [IconSource](/uwp/api/windows.ui.xaml.controls.iconsource) como icono. Crea también iconos como recursos.

```xaml
<Page.Resources>
    <SymbolIconSource x:Key="ReplyIcon" Symbol="MailReply"/>
    <SymbolIconSource x:Key="DeleteIcon" Symbol="Delete"/>
    <SymbolIconSource x:Key="PinIcon" Symbol="Pin"/>

    <SwipeItems x:Key="RevealOptions" Mode="Reveal">
        <SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"/>
        <SwipeItem Text="Pin" IconSource="{StaticResource PinIcon}"/>
    </SwipeItems>

    <SwipeItems x:Key="ExecuteDelete" Mode="Execute">
        <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
                   Background="Red"/>
    </SwipeItems>
</Page.Resources>
```

Recuerda que tienes que mantener los elementos de menú de tu contenido de deslizamiento del dedo con etiquetas de texto breves y concisas. Estas acciones deben ser las principales que un usuario puede querer realizar varias veces durante un breve período.

Configurar un comando de deslizar el dedo para trabajar en una colección o ListView es exactamente lo mismo que definir un único comando de deslizar el dedo (ejemplo antes mencionado), excepto que se define SwipeControl en una DataTemplate para que se aplique a cada elemento de la colección.

Esta es una clase ListView con SwipeControl aplicado en la clase DataTemplate de su elemento. Las propiedades LeftItems y RightItems hacen referencia a los elementos para deslizar el dedo que creaste como recursos.

```xaml
<ListView x:Name="sampleList" Width="300">
    <ListView.ItemContainerStyle>
        <Style TargetType="ListViewItem">
            <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
            <Setter Property="VerticalContentAlignment" Value="Stretch"/>
        </Style>
    </ListView.ItemContainerStyle>
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="x:String">
            <SwipeControl x:Name="ListViewSwipeContainer"
                          LeftItems="{StaticResource RevealOptions}"
                          RightItems="{StaticResource ExecuteDelete}"
                          Height="60">
                <StackPanel Orientation="Vertical">
                    <TextBlock Text="{x:Bind}" FontSize="18"/>
                    <StackPanel Orientation="Horizontal">
                        <TextBlock Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit..." FontSize="12"/>
                    </StackPanel>
                </StackPanel>
            </SwipeControl>
        </DataTemplate>
    </ListView.ItemTemplate>
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
</ListView>
```

## <a name="handle-an-invoked-swipe-command"></a>Manipular un comando invocado de deslizar el dedo

Para actuar en un comando de deslizar el ledo, manipula su evento [Invoked](/uwp/api/windows.ui.xaml.controls.swipeitem.Invoked). (Para más información sobre cómo un usuario puede invocar un comando, revisa la sección _¿Cómo funciona Deslizar el dedo?_ anteriormente en este artículo). Normalmente, un comando de deslizar el dedo se encuentra en un escenario de ListView o similar a una lista. En ese caso, cuando se invoca un comando, querrás realizar una acción en el elemento en el que has deslizado el dedo.

Aquí te mostramos cómo manipular el evento Invoked en el elemento de deslizar con el dedo _Eliminar_ que creaste anteriormente.

```xaml
<SwipeItems x:Key="ExecuteDelete" Mode="Execute">
    <SwipeItem Text="Delete" IconSource="{StaticResource DeleteIcon}"
               Background="Red" Invoked="Delete_Invoked"/>
</SwipeItems>
```

El elemento de datos es el DataContext del SwipeControl. En el código, puedes acceder al elemento que se deslizó obteniendo la propiedad SwipeControl.DataContext de los argumentos del evento, como se muestra aquí.

```csharp
 private void Delete_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
 {
     sampleList.Items.Remove(args.SwipeControl.DataContext);
 }
```

> [!NOTE]
> Aquí, los elementos se han agregado directamente a la colección ListView.Items por cuestiones de simplicidad, por lo que se elimina el elemento del mismo modo. Si en su lugar estableces ListView.ItemsSource en una colección, lo que es más habitual, tendrás que eliminar el elemento de la colección de origen.

En este caso concreto, se quita el elemento de la lista, por lo que no es importante el estado visual final del elemento deslizado. Sin embargo, en situaciones donde simplemente quieras realizar una acción y, luego, tengas que volver a contraer Deslizar el dedo, puedes configurar la propiedad [BehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipeitem.BehaviorOnInvoked) con uno de los valores de enumeración [SwipeBehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipebehavioroninvoked).

- **Automático**
  - En el modo Ejecutar, el elemento abierto de deslizar el dedo permanecerá abierto cuando se invoque.
  - En el modo Mostrar, el elemento abierto de deslizar el dedo se contraerá cuando se invoque.
- **Cerrar**
  - Cuando se invoque el elemento, el control de deslizar el dedo siempre se contraerá y volverá al estado normal, independientemente del modo.
- **RemainOpen**
  - Cuando se invoque el elemento, el control de deslizar el dedo siempre seguirá abierto, independientemente del modo.

Aquí, se establece un elemento de deslizar el dedo _reply_ para que se cierre después de invocarlo.

```xaml
<SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"
           Invoked="Reply_Invoked"
           BehaviorOnInvoked = "Close"/>
```

## <a name="dos-and-donts"></a>Consejos

- No uses deslizar el dedo en FlipViews, hubs ni tablas dinámicas. La combinación puede resultar confusa para el usuario debido a los conflictos en las direcciones para deslizar el dedo.
- No combines el deslizamiento horizontal del dedo con la navegación horizontal, ni el deslizamiento vertical con la navegación vertical.
- Asegúrate de que lo que el usuario deslice con el dedo sea la misma acción y que sea coherente en todos los elementos relacionados que se pueden deslizar.
- Usa deslizar el dedo para las acciones principales que un usuario quiera realizar.
- Usa deslizar el dedo con los elementos en los que se repite la misma acción muchas veces.
- Usa el deslizamiento horizontal del dedo en elementos más anchos, y el vertical en elementos más altos.
- Usa etiquetas de texto cortas y concisas.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Vista de lista y vista de cuadrícula](listview-and-gridview.md)
- [Plantillas y contenedores de elementos](item-containers-templates.md)
- [Extraer para actualizar](pull-to-refresh.md)
