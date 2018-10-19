---
author: mijacobs
Description: Menus and context menus display a list of commands or options when the user requests them.
title: Menús y menús contextuales
label: Menus and context menus
template: detail.hbs
ms.author: mijacobs
ms.date: 10/02/2018
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
ms.openlocfilehash: 20b6f54f39be116ad77cb5a179ff8c3d188eb8c4
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "5158059"
---
# <a name="menus-and-context-menus"></a>Menús y menús contextuales

Los menús y los menús contextuales muestran una lista de opciones o comandos cuando el usuario los solicita. Usa un control flotante de menú para mostrar un único, menú en línea. Usar una barra de menús para mostrar un conjunto de menús en una fila horizontal, por lo general, en la parte superior de una ventana de aplicación. Cada menú puede tener submenús y elementos de menú.

![Ejemplo de un menú contextual típico](images/contextmenu_rs2_icons.png)

| **Obtén la biblioteca de la interfaz de usuario de Windows** |
| - |
| Este control se incluye como parte de la biblioteca de la interfaz de usuario de Windows, un paquete de NuGet que contiene los nuevos controles y funciones de la interfaz de usuario para aplicaciones para UWP. Para obtener más información, incluidas las instrucciones de instalación, vea la [información general de la biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **API de la plataforma** | **API de la biblioteca de la interfaz de usuario de Windows** |
| - | - |
| [Clase MenuFlyout](/uwp/api/windows.ui.xaml.controls.menuflyout), la [clase de la barra de menús](/uwp/api/windows.ui.xaml.controls.menubar), [propiedad ContextFlyout](/uwp/api/windows.ui.xaml.uielement.contextflyout), [propiedad FlyoutBase.AttachedFlyout](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout) | [Clase de la barra de menús](/uwp/api/microsoft.ui.xaml.controls.menubar) |

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Los menús y los menús contextuales ahorran espacio al organizar los comandos y ocultarlos hasta que el usuario los necesite. Si un comando se va a usar con frecuencia y tienes el espacio disponible, considera la posibilidad de colocarlo directamente en su propio elemento, en lugar de en un menú, para que los usuarios no tengan que pasar por un menú para acceder a él.

Menús y menús contextuales sirven para organizar los comandos; para mostrar contenido arbitrario, como una solicitud de confirmación o notificación, usa un [cuadro de diálogo o un control flotante](dialogs.md).

### <a name="menubar-vs-menuflyout"></a>Barra de menús frente a MenuFlyout

Para mostrar un menú en un control flotante adjunto a un elemento de interfaz de usuario en el lienzo, usa el control de MenuFlyout para hospedar los elementos de menú. Se puede invocar un menú flotante como un menú normal o como un menú contextual. Un control flotante de menú hospeda un único menú de nivel superior (y submenús opcionales).

Para mostrar un conjunto de varios menús de nivel superior en una fila horizontal, usa una barra de menús. Por lo general, posición de la barra de menús en la parte superior de la ventana de la aplicación.

### <a name="menubar-vs-commandbar"></a>Barra de menús frente a CommandBar

Barra de menús y CommandBar ambos representan las superficies que puedes usar para exponer comandos a los usuarios. La barra de menús proporciona una forma rápida y sencilla para exponer un conjunto de comandos para las aplicaciones que puede que tengas más de organización o agrupamiento que permite un control CommandBar.

También puedes usar una barra de menús junto con un control CommandBar. Usar la barra de menús para proporcionar la mayor parte de los comandos y el control CommandBar para resaltar los comandos usados con más.

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

Menús y menús contextuales son similares en su apariencia y lo que pueden contener. De hecho, puedes usar el mismo control, [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030), para crearlos. La diferencia es cómo se permite al usuario acceder a ella.

¿Cuándo debes usar un menú o un menú contextual?

- Si el elemento host es un botón o algún otro elemento de comando cuyo rol principal es presentar comandos adicionales, usa un menú.
- Si el elemento host es algún otro tipo de elemento con otra finalidad principal (por ejemplo, presentar texto o una imagen), usa un menú contextual.

Por ejemplo, usa un menú en un botón para proporcionar el filtrado y las opciones para obtener una lista de ordenación. En este escenario, el propósito principal del control de botón es proporcionar acceso a un menú.

![Ejemplo de menú en Correo](images/Mail_Menu.png)

Si quieres agregar comandos (como cortar, copiar y pegar) a un elemento de texto, usa un menú contextual en lugar de un menú. En este escenario, el rol principal del elemento de texto es presentar y editar texto; los comandos adicionales (como cortar, copiar y pegar) son secundarios y se incluyen en un menú contextual.

![Ejemplo de menú contextual en galería de fotos](images/ContextMenu_example.png)

### <a name="menus"></a>Menús

- Tienen un solo punto de entrada (un menú Archivo en la parte superior de la pantalla, por ejemplo) que se muestra siempre.
- Por lo general, se adjuntan a un botón o un elemento de menú principal.
- Se invocan mediante un clic con el botón izquierdo (o una acción equivalente, como pulsar con el dedo).
- Están asociados con un elemento a través de sus propiedades de [control flotante](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) o [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx) o agrupados en una barra de menús de la parte superior de la ventana de la aplicación.

### <a name="context-menus"></a>Menús contextuales

- Se adjuntan a un solo elemento y muestran comandos secundarios.
- Se invocan al hacer clic con el botón derecho (o una acción equivalente, como pulsar y sostener con el dedo).
- Se asocian con un elemento a través de su propiedad [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx).

## <a name="icons"></a>Iconos

Considera la posibilidad de proporcionar iconos de elemento de menú para:

- Los más usados elementos.
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
> El tamaño del icono en un MenuFlyoutItem es 16 x 16 píxeles. Si usas SymbolIcon, FontIcon o PathIcon, el icono se escala automáticamente al tamaño correcto sin pérdida de fidelidad. Si usas BitmapIcon, asegúrate de que el activo mide 16 x 16 píxeles.  

## <a name="create-a-menu-flyout-or-a-context-menu"></a>Crear un control flotante de menú o un menú contextual

Para crear un control flotante de menú o un menú contextual, puedes usar la [clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030). Debes definir el contenido del menú mediante la adición de los objetos [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) y [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) a la clase MenuFlyout.

Estos objetos permiten:

- [MenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx): realizar una acción inmediata.
- [ToggleMenuFlyoutItem](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx): activar o desactivar una opción.
- [MenuFlyoutSeparator](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx): separar visualmente los elementos de menú.

En este ejemplo se crea un [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) y se usa la propiedad [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) , una propiedad disponible para la mayoría de los controles, para mostrar el MenuFlyout como un menú contextual.

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

### <a name="light-dismiss"></a>Cierre del elemento

Los controles de cierre del elemento por cambio de foco, tales como menús, menús contextuales y otros controles flotantes, capturan el foco del teclado y del controlador para juegos dentro de la interfaz de usuario transitoria hasta que se descartan. Para proporcionar una indicación visual para este comportamiento, los controles de cierre del elemento por cambio de foco de Xbox dibujarán una superposición que atenuará la visibilidad de la interfaz de usuario que está fuera del ámbito. Este comportamiento se puede modificar con la propiedad [LightDismissOverlayMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx). De manera predeterminada, las interfaces de usuario transitorias dibujarán la superposición de cierre del elemento por cambio de foco en Xbox (**Auto**) pero no de otras familias de dispositivos, aunque las aplicaciones pueden optar por forzar la superposición siempre en **Activado** o siempre en **Desactivado**.

```xaml
<MenuFlyout LightDismissOverlayMode="Off" />
```

## <a name="create-a-menu-bar"></a>Crear una barra de menús

> **Vista previa**: barra de menús requiere la [última compilación de Windows 10 Insider Preview y SDK](https://insider.windows.com/for-developers/) o la [Biblioteca de la interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Usa los mismos elementos para crear menús en una barra de menús, como se muestra en un control flotante de menú. Sin embargo, en lugar de agrupar los objetos de MenuFlyoutItem en un MenuFlyout, agrupan en un elemento MenuBarItem. Cada MenuBarItem se agrega a la barra de menús como un menú de nivel superior.

![Ejemplo de una barra de menús](images/menu-bar-submenu.png)

> [!NOTE]
> En este ejemplo se muestra solo cómo crear la estructura de la interfaz de usuario, pero no muestra la implementación de cualquiera de los comandos.

```xaml
<MenuBar>
    <MenuBarItem Title="File">
        <MenuFlyoutSubItem Text="New">
            <MenuFlyoutItem Text="Plain Text Document"/>
            <MenuFlyoutItem Text="Rich Text Document"/>
            <MenuFlyoutItem Text="Other Formats..."/>
        </MenuFlyoutSubItem>
        <MenuFlyoutItem Text="Open..."/>
        <MenuFlyoutItem Text="Save">
        <MenuFlyoutSeparator />
        <MenuFlyoutItem Text="Exit"/>
    </MenuBarItem>

    <MenuBarItem Title="Edit">
        <MenuFlyoutItem Text="Undo"/>
        <MenuFlyoutItem Text="Cut"/>
        <MenuFlyoutItem Text="Copy"/>
        <MenuFlyoutItem Text="Paste"/>
    </MenuBarItem>

    <MenuBarItem Title="Help">
        <MenuFlyoutItem Text="About"/>
    </MenuBarItem>
</MenuBar>
```

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.
- [Muestra del menú contextual XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlContextMenu)

## <a name="related-articles"></a>Artículos relacionados

- [Clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [Clase de la barra de menús](/uwp/api/Windows.UI.Xaml.Controls.MenuBar)
