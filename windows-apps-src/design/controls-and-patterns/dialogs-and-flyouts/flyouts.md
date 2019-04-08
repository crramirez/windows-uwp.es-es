---
Description: Los cuadros de diálogo y los controles flotantes muestran elementos transitorios de la interfaz de usuario que aparecen cuando el usuario los solicita o cuando sucede algo que requiere notificación o aprobación.
title: Controles de ventana flotante
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 52de0933bf51adaae6b0923868e12eb92ced4a1a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625020"
---
# <a name="flyouts"></a>Controles flotantes

Un control flotante es un control de cierre del elemento por cambio de foco que puede mostrar una interfaz de usuario arbitraria como su contenido. Los controles flotantes pueden contener otros controles flotantes o menús contextuales para crear una experiencia anidada.

![Menú contextual anidado dentro de un control flotante](../images/flyout-nested.png)

> **API importantes**: [Clase de ventana flotante](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

* No uses un control flotante en lugar de una [información sobre herramientas](../tooltips.md) o un [menú contextual](../menus.md). Usa una información sobre herramientas para mostrar una descripción breve que se oculta tras un tiempo determinado. Usa un menú contextual para acciones contextuales relacionadas con un elemento de la interfaz de usuario, como copiar y pegar.

Para obtener recomendaciones sobre cuándo usar un control flotante frente a cuándo se debe usar un cuadro de diálogo (un control similar), vea [menús emergentes y cuadros de diálogo](index.md). 

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para abrirla y ver <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> o <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> en acción.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

##  <a name="how-to-create-a-flyout"></a>Creación de un control flotante


Los controles flotantes se asocian a controles específicos. Puede usar el [colocación](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.Placement) propiedad para especificar dónde aparece una ventana flotante: Parte superior, izquierda, abajo, derecha o completo. Si seleccionas el [modo de colocación completa](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutPlacementMode), la aplicación amplía el control flotante y lo centra dentro de la ventana de la aplicación. Algunos controles, como [Button](/uwp/api/Windows.UI.Xaml.Controls.Button), proporcionan una propiedad [Flyout](/uwp/api/Windows.UI.Xaml.Controls.Button.Flyout) que puedes usar para asociar un control flotante o [menú contextual](../menus.md).

En este ejemplo se crea un control flotante simple que muestra parte del texto cuando se presiona el botón.
````xaml
<Button Content="Click me">
  <Button.Flyout>
     <Flyout>
        <TextBlock Text="This is a flyout!"/>
     </Flyout>
  </Button.Flyout>
</Button>
````

Si el control no tiene una propiedad Flyout, puedes usar la propiedad asociada [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.AttachedFlyoutProperty). Si haces esto, también tienes que llamar al método [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase#Windows_UI_Xaml_Controls_Primitives_FlyoutBase_ShowAttachedFlyout_Windows_UI_Xaml_FrameworkElement_) para mostrar el control flotante.

En este ejemplo se agrega un control flotante simple a una imagen. Cuando el usuario pulsa la imagen, la aplicación muestra el control flotante.

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50"
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock Text="This is some text in a flyout."  />
    </Flyout>        
  </FlyoutBase.AttachedFlyout>
</Image>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}
````

En los ejemplos anteriores, los controles flotantes se definen alineados. También puedes definir un control flotante como un recurso estático y después usarlo con varios elementos. En este ejemplo se crea un control flotante más complicado que muestra una versión más grande de una imagen cuando se pulsa la miniatura.

````xaml
<!-- Declare the shared flyout as a resource. -->
<Page.Resources>
    <Flyout x:Key="ImagePreviewFlyout" Placement="Right">
        <!-- The flyout's DataContext must be the Image Source
             of the image the flyout is attached to. -->
        <Image Source="{Binding Path=Source}"
            MaxHeight="400" MaxWidth="400" Stretch="Uniform"/>
    </Flyout>
</Page.Resources>
````

````xaml
<!-- Assign the flyout to each element that shares it. -->
<StackPanel>
    <Image Source="Assets/cliff.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/grapes.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
    <Image Source="Assets/rainier.jpg" Width="50" Height="50"
           Margin="10" Tapped="Image_Tapped"
           FlyoutBase.AttachedFlyout="{StaticResource ImagePreviewFlyout}"
           DataContext="{Binding RelativeSource={RelativeSource Mode=Self}}"/>
</StackPanel>
````

````csharp
private void Image_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);  
}
````

## <a name="style-a-flyout"></a>Diseñar un control flotante
Para diseñar un control flotante, modifica su propiedad [FlyoutPresenterStyle](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Flyout.FlyoutPresenterStyle). En el siguiente ejemplo se muestra un párrafo de texto ajustado y se permite que el bloque de texto sea accesible para un lector de pantalla.

![Control flotante accesible texto ajustado](../images/flyout-wrapping-text.png)

````xaml
<Flyout>
  <Flyout.FlyoutPresenterStyle>
    <Style TargetType="FlyoutPresenter">
      <Setter Property="ScrollViewer.HorizontalScrollMode"
          Value="Disabled"/>
      <Setter Property="ScrollViewer.HorizontalScrollBarVisibility" Value="Disabled"/>
      <Setter Property="IsTabStop" Value="True"/>
      <Setter Property="TabNavigation" Value="Cycle"/>
    </Style>
  </Flyout.FlyoutPresenterStyle>
  <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat."/>
</Flyout>
````

## <a name="styling-flyouts-for-10-foot-experiences"></a>Aplicar estilos a controles flotantes para experiencias de 10 pies

Los controles de cierre del elemento por cambio de foco como los controles flotantes capturan el foco del teclado y del controlador para juegos dentro de su interfaz de usuario transitoria hasta que se cierra. Para proporcionar una indicación visual para este comportamiento, los controles de cierre del elemento por cambio de foco de Xbox dibujan una superposición que atenúa el contraste y la visibilidad de la interfaz de usuario que está fuera del ámbito. Este comportamiento se puede modificar con la propiedad [`LightDismissOverlayMode`](/uwp/api/Windows.UI.Xaml.Controls.Primitives.FlyoutBase.LightDismissOverlayMode). De manera predeterminada, los controles flotantes dibujarán la superposición de cierre del elemento por cambio de foco en Xbox pero no de otras familias de dispositivos, aunque las aplicaciones pueden optar por forzar la superposición siempre en **Activado** o siempre en **Desactivado**.

![Control flotante con atenuación de superposición](../images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

## <a name="light-dismiss-behavior"></a>Comportamiento de control de cierre del elemento por cambio de foco
Los controles flotantes se pueden cerrar con una rápida acción de cierre del elemento por cambio de foco
-   Pulsar fuera del control flotante
-   Presionar la tecla de teclado Escape
-   Presionar el botón Atrás del sistema de hardware o software
-   Presionar el botón B del controlador para juegos

Al descartar con un gesto de pulsación, este gesto se absorbe normalmente y no pasa a la interfaz de usuario que se encuentra debajo. Por ejemplo, si hay un botón visible detrás detrás de un control flotante abierto, la primera pulsación del usuario descarta el control flotante, pero no activa este botón. Al presionar el botón se requiere una segunda pulsación.

Puedes cambiar este comportamiento designando el botón como elemento de paso a través de entrada para el control flotante. El control flotante se cierra como resultado de las acciones de cierre del elemento por cambio de foco descritas anteriormente y también pasará el evento de pulsación con su `OverlayInputPassThroughElement` designado. Piense en adoptar este comportamiento para acelerar las interacciones del usuario en elementos funcionalmente similares. Si la aplicación tiene una colección de favoritos y cada elemento de la colección incluye un control flotante adjunto, resulta razonable esperar que los usuarios puedan querer interactuar con varios controles flotantes en una sucesión rápida.

[!NOTE] Ten cuidado de no designar un elemento de paso a través de entrada de superposición que produzca una acción destructiva. Los usuarios se han habituado a acciones de cierre del elemento por cambio de foco discretas que no activan la interfaz de usuario principal. Los botones Cerrar, Eliminar o destructivos de forma similar no deberían activarse en el cierre del elemento por cambio de foco para evitar un comportamiento inesperado y que provoque interrupciones.

En el ejemplo siguiente, se activarán los tres botones dentro de FavoritesBar en la primera pulsación.

````xaml
<Page>
    <Page.Resources>
        <Flyout x:Name="TravelFlyout" x:Key="TravelFlyout"
                OverlayInputPassThroughElement="{x:Bind FavoritesBar}">
            <StackPanel>
                <HyperlinkButton Content="Washington Trails Association"/>
                <HyperlinkButton Content="Washington Cascades - Go Northwest! A Travel Guide"/>  
            </StackPanel>
        </Flyout>
    </Page.Resources>

    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <StackPanel x:Name="FavoritesBar" Orientation="Horizontal">
            <HyperlinkButton x:Name="PageLinkBtn">Bing</HyperlinkButton>  
            <Button x:Name="Folder1" Content="Travel" Flyout="{StaticResource TravelFlyout}"/>
            <Button x:Name="Folder2" Content="Entertainment" Click="Folder2_Click"/>
        </StackPanel>
        <ScrollViewer Grid.Row="1">
            <WebView x:Name="WebContent"/>
        </ScrollViewer>
    </Grid>
</Page>
````
````csharp
private void Folder2_Click(object sender, RoutedEventArgs e)
{
     Flyout flyout = new Flyout();
     flyout.OverlayInputPassThroughElement = FavoritesBar;
     ...
     flyout.ShowAt(sender as FrameworkElement);
{
````

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados
- [Información sobre herramientas](../tooltips.md)
- [Los menús y el menú contextual](../menus.md)
- [Clase de ventana flotante](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [Clase ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)