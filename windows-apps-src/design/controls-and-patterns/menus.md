---
Description: Los menús y los menús contextuales muestran una lista de opciones o comandos cuando el usuario los solicita.
title: Menús y menús contextuales
label: Menus and context menus
template: detail.hbs
ms.date: 04/19/2019
ms.topic: article
ms.custom: RS5, 19H1
keywords: windows 10, uwp
ms.assetid: 0327d8c1-8329-4be2-84e3-66e1e9a0aa60
pm-contact: yulikl
design-contact: kimsea
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 31371c2b2a4826939de428fb6d7c082b78d05843
ms.sourcegitcommit: 6951827b7d0948618e1fbb082c28794c7f23f83c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2019
ms.locfileid: "70923153"
---
# <a name="menus-and-context-menus"></a>Menús y menús contextuales

Los menús y los menús contextuales muestran una lista de opciones o comandos cuando el usuario los solicita. Usa un control flotante de menú para mostrar un menú en línea único. Usa una barra de menús para mostrar un conjunto de menús en una fila horizontal, normalmente en la parte superior de una ventana de aplicación. Cada menú puede tener elementos de menú y submenús.

![Ejemplo de un menú contextual típico](images/contextmenu_rs2_icons.png)

| **Obtener la biblioteca de interfaz de usuario de Windows** |
| - |
| Este control se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones para UWP. Para obtener más información, incluidas instrucciones sobre la instalación, consulta la [introducción a la biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de plataforma** | **API de la biblioteca de interfaz de usuario de Windows** |
| - | - |
| [Clase MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout), [clase MenuBar](/uwp/api/windows.ui.xaml.controls.menubar), [propiedad ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout), [propiedad FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties) | [Clase MenuBar](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Los menús y los menús contextuales ahorran espacio al organizar los comandos y ocultarlos hasta que el usuario los necesite. Si un comando se va a usar con frecuencia y tienes el espacio disponible, considera la posibilidad de colocarlo directamente en su propio elemento, en lugar de en un menú, para que los usuarios no tengan que pasar por un menú para acceder a él.

Los menús y los menús contextuales sirven para organizar los comandos; para mostrar contenido arbitrario, como una notificación o una solicitud de confirmación, usa un [cuadro de diálogo o un control flotante](dialogs.md).

### <a name="menubar-vs-menuflyout"></a>MenuBar frente a MenuFlyout

Para mostrar un menú en un control flotante adjunto a un elemento de interfaz de usuario en el lienzo, usa el control MenuFlyout para hospedar los elementos de menú. Puedes invocar un control flotante de menú como menú regular o como menú contextual. Un control flotante de menú hospeda un único menú de nivel superior (y submenús opcionales).

Para mostrar un conjunto de varios menús de nivel superior en una fila horizontal, usa una barra de menús. Normalmente colocas la barra de menús en la parte superior de la ventana de la aplicación.

### <a name="menubar-vs-commandbar"></a>MenuBar frente a CommandBar

MenuBar y CommandBar representan las superficies que puedes usar para exponer comandos a los usuarios. MenuBar proporciona una manera rápida y sencilla de exponer un conjunto de comandos para las aplicaciones que podrían necesitar una mayor organización o agrupación que las que permite un elemento CommandBar.

También puedes usar un elemento MenuBar junto con un elemento CommandBar. Usa MenuBar para proporcionar la mayor parte de los comandos y CommandBar para resaltar los comandos más usados.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/MenuFlyout">abrir la aplicación y ver MenuFlyout en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="menus-vs-context-menus"></a>Menús frente a menús contextuales

Los menús y los menús contextuales son parecidos en su apariencia y en lo que pueden contener. De hecho, puedes usar el mismo control, [MenuFlyout](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MenuFlyout), para crearlos. La diferencia es cómo se permite al usuario acceder a ellos.

¿Cuándo debes usar un menú o un menú contextual?

- Si el elemento host es un botón o algún otro elemento de comando cuyo rol principal es presentar comandos adicionales, usa un menú.
- Si el elemento host es algún otro tipo de elemento con otra finalidad principal (por ejemplo, presentar texto o una imagen), usa un menú contextual.

Por ejemplo, usa un menú en un botón para proporcionar opciones de filtrado y ordenación para una lista. En este escenario, el propósito principal del control de botón es proporcionar acceso a un menú.

![Ejemplo de menú en Correo](images/Mail_Menu.png)

Si quieres agregar comandos (como cortar, copiar y pegar) a un elemento de texto, usa un menú contextual en lugar de un menú. En este escenario, el rol principal del elemento de texto es presentar y editar texto; los comandos adicionales (como cortar, copiar y pegar) son secundarios y se incluyen en un menú contextual.

![Ejemplo de menú contextual en Galería de fotos](images/ContextMenu_example.png)

### <a name="menus"></a>Menús

- Tienen un solo punto de entrada (un menú Archivo en la parte superior de la pantalla, por ejemplo) que se muestra siempre.
- Por lo general, se adjuntan a un botón o un elemento de menú principal.
- Se invocan mediante un clic con el botón izquierdo (o una acción equivalente, como pulsar con el dedo).
- Se asocian con un elemento a través de sus propiedades [Flyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button.flyout) o [FlyoutBase.AttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase#xaml-attached-properties) o se agrupan en una barra de menús en la parte superior de la ventana de aplicación.

### <a name="context-menus"></a>Menús contextuales

- Se adjuntan a un solo elemento y muestran comandos secundarios.
- Se invocan al hacer clic con el botón derecho (o una acción equivalente, como pulsar y sostener con el dedo).
- Se asocian con un elemento a través de su propiedad [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout).

## <a name="icons"></a>Iconos

Considera la posibilidad de proporcionar iconos de elemento de menú para:

- Los elementos que se usan con mayor frecuencia.
- Elementos de menú cuyo icono es estándar o bien conocido.
- Elementos de menú cuyo icono muestra bien lo que hace el comando.

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

> [!TIP]
> El tamaño del icono de un elemento MenuFlyoutItems es de 16 x 16 píxeles. Si usas SymbolIcon, FontIcon o PathIcon, el icono escalará automáticamente hasta el tamaño correcto sin pérdida de fidelidad. Si usas BitmapIcon, asegúrate de que el activo mide 16 x 16 píxeles.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>Creación de un control flotante de menú o un menú contextual

Para crear un control flotante de menú o un menú contextual, usa la [clase MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout). Debes definir el contenido del menú mediante la adición de los objetos [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem), [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem), [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem), [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem) y [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator) a la clase MenuFlyout.

Estos objetos permiten:

- [MenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutitem): realizar una acción inmediata.
- [MenuFlyoutSubItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutsubitem): contener una lista de elementos de menú en cascada.
- [ToggleMenuFlyoutItem](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.togglemenuflyoutitem): activar o desactivar una opción.
- [RadioMenuFlyoutItem](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem): cambiar entre los elementos de menú que se excluyen mutuamente.
- [MenuFlyoutSeparator](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutseparator): separar visualmente los elementos de menú.

En este ejemplo se crea un elemento [MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) y se usa la propiedad [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout), una propiedad disponible para la mayoría de los controles, para mostrar el elemento MenuFlyout como un menú contextual.

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

El siguiente ejemplo es casi idéntico, pero en lugar de usar la propiedad [ContextFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextflyout) para mostrar la [clase MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout) como un menú contextual, el ejemplo usa la propiedad [FlyoutBase.ShowAttachedFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) para que se muestre como un menú.

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

### <a name="light-dismiss"></a>Cierre del elemento por cambio de foco

Los controles de cierre del elemento por cambio de foco, tales como menús, menús contextuales y otros controles flotantes, capturan el foco del teclado y del controlador para juegos dentro de la interfaz de usuario transitoria hasta que se descartan. Para proporcionar una indicación visual para este comportamiento, los controles de cierre del elemento por cambio de foco de Xbox dibujarán una superposición que atenuará la visibilidad de la interfaz de usuario que está fuera del ámbito. Este comportamiento se puede modificar con la propiedad [LightDismissOverlayMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode). De manera predeterminada, las interfaces de usuario transitorias dibujarán la superposición del cierre del elemento por cambio de foco en Xbox (**Automático**), pero no en otras familias de dispositivos. Puedes elegir forzar la superposición para que siempre esté **Activado** o **Desactivado**.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>Creación de una barra de menús

> [!IMPORTANT]
> MenuBar requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior, o la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Usa los mismos elementos para crear menús en una barra de menús, tal como aparece en un control flotante de menú. Sin embargo, en lugar de agrupar objetos MenuFlyoutItem en un elemento MenuFlyout, agrúpalos en un elemento MenuBarItem. Cada MenuBarItem se agrega a MenuBar como un menú de nivel superior.

![Ejemplo de una barra de menús](images/menu-bar-submenu.png)

> [!NOTE]
> En este ejemplo solo se muestra cómo crear la estructura de la interfaz de usuario; no se muestra la implementación de ninguno de los comandos.

```xaml
<muxc:MenuBar>
    <muxc:MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save"/>
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="View">
        <MenuFlyoutItem Text="Output"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Landscape" GroupName="OrientationGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Portrait" GroupName="OrientationGroup" IsChecked="True"/>
        <MenuFlyoutSeparator/>
        <muxc:RadioMenuFlyoutItem Text="Small icons" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Medium icons" IsChecked="True" GroupName="SizeGroup"/>
        <muxc:RadioMenuFlyoutItem Text="Large icons" GroupName="SizeGroup"/>
    </muxc:MenuBarItem>

    <muxc:MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </muxc:MenuBarItem>
</muxc:MenuBar>
```

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplos de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): consulta todos los controles XAML en un formato interactivo.
- [Ejemplo del menú contextual XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Artículos relacionados

- [Clase MenuFlyout](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyout)
- [Clase MenuBar](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.menubar)
