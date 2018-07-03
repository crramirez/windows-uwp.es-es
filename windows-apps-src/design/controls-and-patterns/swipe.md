---
author: jwmsft
pm-contact: kisai
design-contact: ksulliv
dev-contact: Shmazlou
doc-status: Published
Description: Swipe commanding is a touch accelerator for context menus.
title: Deslizar el dedo
label: Swipe
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 47a2e48be14df6a59aec4404d760f416804bad6b
ms.sourcegitcommit: 3522d888781ff6f063b129b54760a5cbefd38139
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "1937091"
---
# <a name="swipe"></a>Deslizar el dedo

El comando de deslizar el dedo es un acelerador para menús contextuales que permite a los usuarios acceder fácilmente a las acciones de menú comunes de forma táctil, sin necesidad de cambiar los estados de la aplicación.

> **API importantes**: [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem), [clase ListView](/uwp/api/Windows.UI.Xaml.Controls.ListView)

![Ejecutar y mostrar el tema claro](images/LightThemeSwipe.png)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Los comandos de deslizar el dedo ahorran espacio. Resulta útil en situaciones en las que el usuario puede realizar la misma operación en varios elementos rápidamente. Y proporciona "acciones rápidas" en los elementos que no necesitan un cambio de estado ni que aparezca un cuadro de diálogo en la página.

Deben usarse los comandos de deslizar el dedo cuando haya un grupo de elementos potencialmente grande y cada elemento tenga de 1a 3 acciones que un usuario quizá quiera realizar con regularidad. Estas acciones pueden incluir, entre otros aspectos:

- Eliminar
- Marcar o archivar
- Guardar o descargar
- Responder

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/SwipeControl">abrir la aplicación y ver SwipeControl en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="video-summary"></a>Resumen en vídeo

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev015/player]

## <a name="how-does-swipe-work"></a>¿Cómo funciona Deslizar el dedo?

Los comandos de deslizar el dedo de UWP tienen dos modos: [Mostrar](/uwp/api/windows.ui.xaml.controls.swipemode) y [Ejecutar](/uwp/api/windows.ui.xaml.controls.swipemode). También admite cuatro direcciones distintas para deslizar el dedo: arriba, abajo, izquierda y derecha.

### <a name="reveal-mode"></a>Modo Mostrar

En el modo Mostrar, el usuario desliza el dedo por un elemento para abrir un menú de uno o varios comandos y debe pulsar explícitamente un comando para ejecutarlo. Cuando el usuario desliza el dedo por un elemento y lo suelta, el menú permanece abierto hasta que se selecciona un comando o el menú se vuelve a cerrar deslizando el dedo al lado contrario, dejando de pulsar o desplazando el elemento abierto fuera de la pantalla.

![Deslizar el dedo para mostrar](images/SwipeCommand-Reveal_v2.gif)

El modo Mostrar es un modo de deslizar el dedo más seguro y versátil y puede usarse para la mayoría de tipos de acciones de menú, incluso acciones potencialmente destructivas, como una eliminación.

Cuando el usuario selecciona una de las opciones que se muestran en el estado de abierto y descansando de Mostrar, se invoca el comando de ese elemento y se cierra el control Deslizar el dedo.

### <a name="execute-mode"></a>Modo Ejecutar

En el modo Ejecutar, el usuario desliza el dedo por un elemento abierto para mostrar y ejecutar un único comando con ese único movimiento. Si el usuario suelta el elemento que se está deslizando antes de pasar un umbral, el menú se cierra y el comando no se ejecuta. Si el usuario desliza el dedo más allá del umbral y luego lo suelta, el comando se ejecuta inmediatamente.

![Deslizar el dedo para ejecutar](images/SwipeCommand_Delete_v2.gif)

Si el usuario no suelta el dedo tras llegar el umbral y vuelve a cerrar el elemento, el comando no se ejecuta y no se realiza ninguna acción con el elemento.

El modo Ejecutar proporciona más comentarios visuales a través de la orientación de etiquetas y color mientras se desliza el dedo por un elemento.

Se recomienda usar el modo Ejecutar cuando la acción que el usuario realiza es la más común.

También puede usarse para acciones más destructivas, como eliminar un elemento. Sin embargo, ten en cuenta que para Ejecutar basta con un solo deslizamiento en una dirección, en lugar de Mostrar, que requiere que el usuario haga clic explícitamente en un botón.

### <a name="swipe-directions"></a>Direcciones de deslizar el dedo

Deslizar el dedo funciona en todas las direcciones cardinales: arriba, abajo, izquierda y derecha. Cada dirección para deslizar el dedo puede contener sus propios elementos o contenido, pero solo puede establecerse una dirección a la vez en un elemento con deslizamiento de dedo único.

Por ejemplo, no puedes tener dos definiciones [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems) en el mismo SwipeControl.

## <a name="how-to-create-a-swipe-command"></a>Cómo crear un comando de deslizar el dedo

Los comandos de deslizar el dedo tienen dos componentes que hay que definir:

- [SwipeControl](/uwp/api/windows.ui.xaml.controls.swipecontrol), que se ajusta alrededor del contenido. En una colección, como ListView, se encuentra dentro de la plantilla de datos.
- Los elementos de menú de deslizar con el dedo, es decir, uno o más objetos [SwipeItem](/uwp/api/windows.ui.xaml.controls.swipeitem) colocados en contenedores direccionales del control de deslizamiento con el dedo: [LeftItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.LeftItems), [RightItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.RightItems), [TopItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.TopItems) o [BottomItems](/uwp/api/windows.ui.xaml.controls.swipecontrol.BottomItems)

El contenido del deslizamiento con el dedo puede colocarse en línea o definirse en la sección de recursos de la página o aplicación.

Este es un ejemplo sencillo de un SwipeControl alrededor de un texto. Muestra la jerarquía de elementos XAML que se necesita para crear un comando de deslizar el dedo.

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

Primero, crea los elementos de deslizamiento del dedo, que representan los comandos, como recursos de nivel de página. SwipeItem usa una [IconSource](/uwp/api/windows.ui.xaml.controls.iconsource) como su icono. Crea también iconos como recursos.

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

Configurar un comando de Deslizar el dedo para trabajar en una colección o ListView es exactamente lo mismo que definir un solo comando de deslizar el dedo (ejemplo antes mencionado), excepto que se define SwipeControl en una DataTemplate para que se aplique a cada elemento de la colección.

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

Para actuar en un comando de deslizar el ledo, manipula su evento [Invoked](/uwp/api/windows.ui.xaml.controls.swipeitem.Invoked). (Para obtener más información acerca de cómo un usuario puede invocar un comando, revisa la anterior sección _¿Cómo se desliza con el dedo?_ de este artículo.) Por lo general, un comando de deslizar con el dedo se produce en un escenario de tipo lista o ListView. En ese caso, cuando se invoca un comando, tienes que realizar una acción en el elemento en el que has deslizado el dedo.

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
> Aquí, los elementos se han agregado directamente a la colección ListView.Items por cuestiones de simplicidad, por lo que se elimina el elemento del mismo modo. Si en su lugar estableces ListView.ItemsSource en una colección, que es más habitual, deberás eliminar el elemento de la colección de origen.

En este caso concreto, se quita el elemento de la lista, por lo que no es importante el estado visual final del elemento deslizado. Sin embargo, en situaciones donde simplemente quieras realizar una acción y, a continuación, tengas que volver a contraer Deslizar con el dedo, puedes configurar la propiedad [BehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipeitem.BehaviorOnInvoked) con uno de los valores de enumeración [SwipeBehaviorOnInvoked](/uwp/api/windows.ui.xaml.controls.swipebehavioroninvoked).

- **Automático**
  - En el modo Ejecutar, el elemento de deslizar con el dedo abierto permanecerá abierto cuando se invoca.
  - En el modo Mostrar, el elemento de deslizar con el dedo abierto se contraerá cuando se invoca.
- **Cerrar**
  - Cuando se invoca el elemento, el control de deslizar con el dedo siempre se contraerá y volverá al estado normal, sin importar el modo.
- **RemainOpen**
  - Cuando se invoca el elemento, el control de deslizar con el dedo siempre seguirá abierto, sin importar el modo.

Aquí, se configura un elemento de deslizar el dedo _responder_ como Cerrar después de invocarlo.

```xaml
<SwipeItem Text="Reply" IconSource="{StaticResource ReplyIcon}"
           Invoked="Reply_Invoked"
           BehaviorOnInvoked = "Close"/>
```

## <a name="dos-and-donts"></a>Qué hacer y qué no hacer

- No uses el gesto de deslizar el dedo en FlipViews, centros o controles dinámicos. La combinación puede resultar confusa para el usuario debido a las direcciones para deslizar el dedo en conflicto.
- No combines el deslizamiento horizontal del dedo con la navegación horizontal, ni el deslizamiento vertical con la navegación vertical.
- Asegúrate de que lo que el usuario desliza con el dedo es la misma acción, y que es coherente en todos los elementos relacionados que se pueden deslizar.
- Usa el deslizamiento del dedo para las acciones principales que un usuario quiera realizar.
- Usa el deslizamiento del dedo con los elementos en los que se repite la misma acción muchas veces.
- Usa el deslizamiento horizontal del dedo en elementos más anchos y uno vertical en elementos más altos.
- Usa etiquetas de texto cortas y concisas.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Vista de lista y vista de cuadrícula](listview-and-gridview.md)
- [Plantillas y contenedores de elementos](item-containers-templates.md)
- [Extraer para actualizar](pull-to-refresh.md)
