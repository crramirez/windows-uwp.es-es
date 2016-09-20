---
author: mijacobs
Description: "Un control flotante es un elemento emergente ligero que se usa para mostrar temporalmente opciones de la interfaz de usuario relacionadas con lo que el usuario esté haciendo en ese momento."
title: "Menús y menús contextuales"
label: Menus and context menus
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: f6ce4bc08e3647cd26dc1537bba5499ddb646a49

---
# Menús y menús contextuales

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Los menús y los menús contextuales muestran una lista de opciones o comandos cuando el usuario los solicita.

![Ejemplo de un menú contextual típico](images/controls_contextmenu_singlepane.png)

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/dn299030">Clase MenuFlyout</a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx">Propiedad ContextFlyout</a></li>
<li><a href="https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx">Propiedad FlyoutBase.AttachedFlyout</a></li>
</ul>

</div>
</div>




## ¿Es este el control adecuado?
Los menús y los menús contextuales ahorran espacio al organizar los comandos y ocultarlos hasta que el usuario los necesite. Si un comando se va a usar con frecuencia y tienes el espacio disponible, considera la posibilidad de colocarlo directamente en su propio elemento, en lugar de en un menú, para que los usuarios no tengan que pasar por un menú para acceder a él. 

Los menús y los menús contextuales sirven para organizar los comandos; para mostrar contenido arbitrario, como una notificación, o para solicitar confirmación, usa un [cuadro de diálogo o un control flotante](dialogs.md).  


## Menús frente a menús contextuales

Los menús y los menús contextuales son idénticos en su apariencia y en lo que pueden contener. De hecho, debes usar el mismo control, [MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030), para crearlos. La única diferencia es cómo se permite al usuario acceder a ellos. 

¿Cuándo debes usar un menú o un menú contextual?
* Si el elemento host es un botón o algún otro elemento de comando cuyo rol principal es presentar comandos adicionales, usa un menú.
* Si el elemento host es algún otro tipo de elemento con otra finalidad principal (por ejemplo, presentar texto o una imagen), usa un menú contextual. 

Por ejemplo, usa un menú en un botón de un panel de navegación para proporcionar opciones de navegación adicionales. En este escenario, el propósito principal del control de botón es proporcionar acceso a un menú. 

Si quieres agregar comandos (como cortar, copiar y pegar) a un elemento de texto, usa un menú contextual en lugar de un menú. En este escenario, el rol principal del elemento de texto es presentar y editar texto; los comandos adicionales (como cortar, copiar y pegar) son secundarios y se incluyen en un menú contextual. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Menús</b></p>
<p>
<ul>
<li>Tienen un solo punto de entrada (un menú Archivo en la parte superior de la pantalla, por ejemplo) que se muestra siempre.</li>
<li>Por lo general, se adjuntan a un botón o un elemento de menú principal.</li>
<li>Se invocan mediante un clic con el botón izquierdo (o una acción equivalente, como pulsar con el dedo).</li>  
<li>Se asocian con un elemento a través de sus propiedades [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) o [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx).</li> 
</ul>
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>Menús contextuales</b></p>
   
<ul>
<li>Se adjuntan a un solo elemento, pero solo son accesibles cuando el contexto tiene sentido.</li>
<li>Se invocan al hacer clic con el botón derecho (o una acción equivalente, como pulsar y sostener con el dedo).</li>
<li>Se asocian con un elemento a través de su propiedad [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx).  
</ul><br/>

  </div>
</div>
</div>

## Crear un menú o un menú contextual

Para crear un menú o un menú contextual, usa la [clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030). Debes definir el contenido del menú mediante la adición de los objetos [MenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx), [ToggleMenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx) y [MenuFlyoutSeparator](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx) a la clase MenuFlyout. Estos objetos permiten:
* [MenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutitem.aspx): realizar una acción inmediata.
* [ToggleMenuFlyoutItem](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.togglemenuflyoutitem.aspx): activar o desactivar una opción.
* [MenuFlyoutSeparator](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.controls.menuflyoutseparator.aspx): separar visualmente los elementos de menú.


En este ejemplo se crea una [clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) y se usa la propiedad [ContextFlyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx), una propiedad disponible para la mayoría de los controles, para mostrar la [clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) como un menú contextual.

````xaml
<Rectangle 
  Height="100" Width="100" 
  Tapped="Rectangle_Tapped">
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

El siguiente ejemplo es casi idéntico, pero en lugar de usar la propiedad [ContextFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.uielement.contextflyout.aspx) para mostrar la [clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030) como un menú contextual, el ejemplo usa la propiedad [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) para que se muestre como un menú. 

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

> **Nota**&nbsp;&nbsp;Los controles de cierre del elemento por cambio de foco, tales como menús, menús contextuales y otros controles flotantes, capturan el foco del teclado y del controlador para juegos dentro de la interfaz de usuario transitoria hasta que se descartan. Para proporcionar una indicación visual para este comportamiento, los controles de cierre del elemento por cambio de foco de Xbox dibujarán una superposición que atenuará la visibilidad de la interfaz de usuario que está fuera del ámbito. Este comportamiento se puede modificar con la nueva propiedad [LightDismissOverlayMode](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx). De manera predeterminada, las interfaces de usuario transitorias dibujarán la superposición de cierre del elemento por cambio de foco en Xbox pero no de otras familias de dispositivos, aunque las aplicaciones pueden optar por forzar la superposición siempre en **Activado** o siempre en **Desactivado**.
> 
> ```xaml
> <MenuFlyout LightDismissOverlayMode="Off">
```

## Get the samples
*   [XAML UI basics](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    See all of the XAML controls in an interactive format.

## Related articles

- [**MenuFlyout class**](https://msdn.microsoft.com/library/windows/apps/dn299030)



<!--HONumber=Aug16_HO3-->


