---
author: mijacobs
Description: A flyout is a lightweight popup that is used to temporarily show UI that is related to what the user is currently doing.
title: Menús y menús contextuales
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e38e9d61e8546d412cc30bad26680243f3a188e4
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2918986"
---
# <a name="menus-and-context-menus"></a>Menús y menús contextuales



Los menús y los menús contextuales muestran una lista de opciones o comandos cuando el usuario los solicita.

> **API importantes**: [Clase MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout), [Propiedad ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx), [Propiedad FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx)

![Ejemplo de un menú contextual típico](images/contextmenu_rs2_icons.png)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?
Los menús y los menús contextuales ahorran espacio al organizar los comandos y ocultarlos hasta que el usuario los necesite. Si un comando se va a usar con frecuencia y tienes el espacio disponible, considera la posibilidad de colocarlo directamente en su propio elemento, en lugar de en un menú, para que los usuarios no tengan que pasar por un menú para acceder a él.

Los menús y los menús contextuales sirven para organizar los comandos; para mostrar contenido arbitrario, como una notificación, o para solicitar confirmación, usa un [cuadro de diálogo o un control flotante](dialogs.md).  

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/MenuFlyout">abrir la aplicación y ver MenuFlyout en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>Menús frente a menús contextuales

Los menús y los menús contextuales son idénticos en su apariencia y en lo que pueden contener. De hecho, debes usar el mismo control, [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030), para crearlos. La única diferencia es cómo se permite al usuario acceder a ellos.

¿Cuándo debes usar un menú o un menú contextual?
* Si el elemento host es un botón o algún otro elemento de comando cuyo rol principal es presentar comandos adicionales, usa un menú.
* Si el elemento host es algún otro tipo de elemento con otra finalidad principal (por ejemplo, presentar texto o una imagen), usa un menú contextual.

Por ejemplo, usa un menú en un botón de un panel de navegación para proporcionar opciones de navegación adicionales. En este escenario, el propósito principal del control de botón es proporcionar acceso a un menú.

![Ejemplo de menú en Correo](images/Mail_Menu.png)

Si quieres agregar comandos (como cortar, copiar y pegar) a un elemento de texto, usa un menú contextual en lugar de un menú. En este escenario, el rol principal del elemento de texto es presentar y editar texto; los comandos adicionales (como cortar, copiar y pegar) son secundarios y se incluyen en un menú contextual.

![Ejemplo de menú contextual en galería de fotos](images/ContextMenu_example.png) 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Menús</b></p>
<ul>
<li>Tienen un solo punto de entrada (un menú Archivo en la parte superior de la pantalla, por ejemplo) que se muestra siempre.</li>
<li>Por lo general, se adjuntan a un botón o un elemento de menú principal.</li>
<li>Se invocan mediante un clic con el botón izquierdo (o una acción equivalente, como pulsar con el dedo).</li><li>Se asocian con un elemento a través de sus propiedades <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx">Flyout</a> o <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx">FlyoutBase.AttachedFlyout</a>.</li>
</ul>
</div>
  <div class="side-by-side-content-right">
   <p><b>Menús contextuales</b></p>

<ul>
<li>Se adjuntan a un solo elemento y muestran comandos secundarios.</li>
<li>Se invocan al hacer clic con el botón derecho (o una acción equivalente, como pulsar y sostener con el dedo).</li><li>Se asocian con un elemento a través de su propiedad <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx">ContextFlyout</a>.</li>
</ul>
  </div>
</div>
</div>

## <a name="icons"></a>Iconos

Considera la posibilidad de proporcionar iconos de elemento de menú para:

<ul>
<li> Los elementos que se usan con mayor frecuencia </li>
<li> Elementos de menú cuyo icono es estándar o bien conocido </li>
<li> Elementos de menú cuyo icono muestra bien lo que hace el comando </li>
</ul>

No te sientas obligado a proporcionar iconos para comandos que no tienen una visualización estándar. Los iconos crípticos no son útiles, provocan una aglutinación visual y evitan que los usuarios se centren en los elementos de menú importantes.

![Menú contextual de ejemplo con iconos](images/contextmenu_rs2_icons.png)

````xaml
<MenuFlyout>
  <MenuFlyoutItem Text="Share" >
    <MenuFlyoutItem.Icon>
      <FontIcon Glyph="&#xE72D;" />
    </MenuFlyoutItem.Icon>
  </MenuFlyoutItem>
  <MenuFlyoutItem Text="Copy" Icon="Copy" />
  <MenuFlyoutItem Text="Delete" Icon="Delete" />
  <MenuFlyoutSeparator />
  <MenuFlyoutItem Text="Rename" />
  <MenuFlyoutItem Text="Select" />
</MenuFlyout>
````
> El tamaño de los iconos de MenuFlyoutItems es de 16 x 16 píxeles. Si usas SymbolIcon, FontIcon o PathIcon, el icono escalará automáticamente hasta el tamaño correcto sin pérdida de fidelidad. Si usas BitmapIcon, asegúrate de que el activo mide 16 x 16 píxeles.  

## <a name="create-a-menu-or-a-context-menu"></a>Crear un menú o un menú contextual

Para crear un menú o un menú contextual, usa la [clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030). Debes definir el contenido del menú mediante la adición de los objetos [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) y [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) a la clase MenuFlyout. Estos objetos permiten:
* [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx): realizar una acción inmediata.
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx): activar o desactivar una opción.
* [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx): separar visualmente los elementos de menú.


En este ejemplo se crea una [clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) y se usa la propiedad [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx), una propiedad disponible para la mayoría de los controles, para mostrar la [clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) como un menú contextual.

````xaml
<Rectangle
  Height="100" Width="100">
  <Rectangle.ContextFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </Rectangle.ContextFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````

El siguiente ejemplo es casi idéntico, pero en lugar de usar la propiedad [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) para mostrar la [clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) como un menú contextual, el ejemplo usa la propiedad [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) para que se muestre como un menú.

````xaml
<Rectangle
  Height="100" Width="100"
  Tapped="Rectangle_Tapped">
  <FlyoutBase.AttachedFlyout>
    <MenuFlyout>
      <MenuFlyoutItem Text="Change color" Click="ChangeColorItem_Click" />
    </MenuFlyout>
  </FlyoutBase.AttachedFlyout>
  <Rectangle.Fill>
    <SolidColorBrush x:Name="rectangleFill" Color="Red" />
  </Rectangle.Fill>
</Rectangle>
````

````csharp
private void Rectangle_Tapped(object sender, TappedRoutedEventArgs e)
{
    FlyoutBase.ShowAttachedFlyout((FrameworkElement)sender);
}

private void ChangeColorItem_Click(object sender, RoutedEventArgs e)
{
    // Change the color from red to blue or blue to red.
    if (rectangleFill.Color == Windows.UI.Colors.Red)
    {
        rectangleFill.Color = Windows.UI.Colors.Blue;
    }
    else
    {
        rectangleFill.Color = Windows.UI.Colors.Red;
    }
}
````


> Los controles de cierre del elemento por cambio de foco, tales como menús, menús contextuales y otros controles flotantes, capturan el foco del teclado y del controlador para juegos dentro de la interfaz de usuario transitoria hasta que se descartan. Para proporcionar una indicación visual para este comportamiento, los controles de cierre del elemento por cambio de foco de Xbox dibujarán una superposición que atenuará la visibilidad de la interfaz de usuario que está fuera del ámbito. Este comportamiento se puede modificar con la propiedad [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx). De manera predeterminada, las interfaces de usuario transitorias dibujarán la superposición de cierre del elemento por cambio de foco en Xbox (**Auto**) pero no de otras familias de dispositivos, aunque las aplicaciones pueden optar por forzar la superposición siempre en **Activado** o siempre en **Desactivado**.

> ```xaml
> <MenuFlyout LightDismissOverlayMode="Off" />
> ```

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.
- [Muestra del menú contextual XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Artículos relacionados

- [Clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)
