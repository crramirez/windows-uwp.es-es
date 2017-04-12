---
author: mijacobs
Description: "Los cuadros de diálogo y los controles flotantes muestran elementos transitorios de la interfaz de usuario que aparecen cuando el usuario los solicita o cuando sucede algo que requiere notificación o aprobación."
title: "Cuadros de diálogo y controles flotantes"
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
ms.openlocfilehash: ba29bd309b3fdaeeee5bfa143a0f74a58b8bd1c5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="dialogs-and-flyouts"></a>Cuadros de diálogo y controles flotantes

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Los cuadros de diálogo y los controles flotantes son elementos transitorios de la interfaz de usuario que aparecen cuando sucede algo que requiere notificación, aprobación o información adicional por parte del usuario.

<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li>[Clase ContentDialog](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)</li>
<li>[Clase Flyout](https://msdn.microsoft.com/library/windows/apps/dn279496)</li>
</ul>
</div>


<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Cuadros de diálogo</b> <br/><br/>
    ![Ejemplo de un cuadro de diálogo](images/dialogs/dialog-delete-file-example.png)</p>
<p>Los cuadros de diálogo son superposiciones modales en la interfaz de usuario que proporcionan información contextual sobre la aplicación. Los cuadros de diálogo bloquean las interacciones con la ventana de la aplicación hasta que se descarten de forma explícita. A menudo solicitan algún tipo de acción por parte del usuario.   
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>Controles flotantes</b> <br/><br/>
   ![Ejemplo de un control flotante](images/flyout-example.png)</p>
<p>Un control flotante es un elemento emergente contextual ligero que muestra la interfaz de usuario relacionada con lo que está haciendo el usuario. Incluye lógica de colocación y tamaño, y se puede usar para revelar un control oculto, mostrar más detalles sobre un elemento o pedirle al usuario que confirme una acción. 
</p><p>A diferencia de un cuadro de diálogo, un control flotante se puede descartar rápidamente al pulsar o hacer clic en algún lugar fuera de dicho control flotante, presionar la tecla Escape (Esc) o el botón Atrás, cambiar el tamaño de la ventana de la aplicación o cambiar la orientación del dispositivo.
</p><br/>

  </div>
</div>
</div>

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

* Usa los cuadros de diálogo y los controles flotantes para notificar a los usuarios información importante o para solicitar información adicional o confirmación para completar una acción. 
* No uses un control flotante en lugar de una [información sobre herramientas](tooltips.md) o un [menú contextual](menus.md). Usa una información sobre herramientas para mostrar una descripción breve que se oculta tras un tiempo determinado. Usa un menú contextual para acciones contextuales relacionadas con un elemento de la interfaz de usuario, como copiar y pegar.  


Los cuadros de diálogo y los controles flotantes garantizan que los usuarios estén al tanto de información importante, pero también interrumpen la experiencia del usuario. Dado que los cuadros de diálogo son modales (bloqueo), interrumpen a los usuarios impidiéndoles realizar cualquier otra acción hasta que interactúan con ellos. Los controles flotantes ofrecen una experiencia menos molesta, pero si se muestran en exceso, pueden resultar perturbadores. 

Ten en cuenta la importancia de la información que quieres compartir: ¿es lo suficientemente importante como para que interrumpa al usuario? Considera también la frecuencia con la que debe mostrarse la información. Si vas a mostrar un cuadro de diálogo o una notificación cada pocos minutos, tal vez quieras asignar espacio para dicha información en la interfaz de usuario principal. Por ejemplo, en un cliente de chat, en lugar de mostrar un control flotante cada vez que un amigo inicie sesión, podrías mostrar una lista de amigos que estén en línea en ese momento y resaltar los amigos a medida que inicien sesión. 

Los controles flotantes y los cuadros de diálogo se usan con frecuencia para confirmar una acción (por ejemplo, eliminar un archivo) antes de ejecutarla. Si esperas que el usuario realice una acción concreta con frecuencia, considera la posibilidad de ofrecerle una forma de deshacer la acción si fuese un error, en lugar de obligarlo a confirmar la acción cada vez. 



## <a name="dialogs-vs-flyouts"></a>Cuadros de diálogo frente a controles flotantes

Una vez que hayas determinado que quieres usar un cuadro de diálogo o un control flotante, debes elegir cuál quieres usar. 

Dado que los cuadros de diálogo bloquean las interacciones y los controles flotantes no, los cuadros de diálogo deben reservarse para situaciones en las que quieres que el usuario centre toda su atención en un dato específico o responda a una pregunta. Los controles flotantes, por el contrario, se pueden usar si quieres llamar la atención sobre algo, pero no hay problemas si el usuario quiere pasarlo por alto. 

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Usa un cuadro de diálogo para...</b> <br/>
<ul>
<li>Expresar información importante que el usuario **debe** leer y confirmar antes de continuar. Algunos ejemplos son los siguientes:
<ul>
  <li>Los casos en que se puede ver comprometida la seguridad del usuario.</li>
  <li>Los casos en que el usuario está a punto de modificar un activo valioso de manera permanente</li>
  <li>Los casos en que el usuario está a punto de eliminar un activo valioso</li>
  <li>Para confirmar una compra desde la aplicación</li>
</ul>

</li>
<li>Mensajes de error que se aplican al contexto general de la aplicación, como un error de conectividad.</li>
<li>Preguntas cuando la aplicación deba hacer al usuario una pregunta de bloqueo, por ejemplo, cuando la aplicación no puede elegir en nombre del usuario. Una pregunta de bloqueo no se puede ignorar ni posponer, y debe ofrecer al usuario opciones bien definidas.</li>
</ul> 
</p>
  </div>
  <div class="side-by-side-content-right">
   <p><b>Usa un control flotante para...</b> <br/>
<ul>
<li>Recopilar información adicional necesaria para poder completar una acción.</li>
<li>Mostrar información que solo sea importante algunas de las veces. Por ejemplo, en una aplicación de galería fotográfica, cuando el usuario hace clic en una imagen en miniatura, podrías usar un control flotante para mostrar una versión grande de la imagen.</li>
<li>Advertencias y confirmaciones, incluidas las que están relacionadas con acciones potencialmente destructivas.</li>
<li>Mostrar más información, como detalles o descripciones más largas de un elemento de la página.</li>
</ul></p>
  </div>
</div>
</div>

<div class="microsoft-internal-note">
Los controles de cierre del elemento por cambio de foco capturan el foco del teclado y del controlador para juegos dentro de la interfaz de usuario transitoria hasta que se cierra. Para proporcionar una indicación visual para este comportamiento, los controles de cierre del elemento por cambio de foco de Xbox dibujarán una superposición que atenuará la visibilidad de la interfaz de usuario que está fuera del ámbito. Este comportamiento se puede modificar con la nueva propiedad `LightDismissOverlayMode`. De manera predeterminada, las interfaces de usuario transitorias dibujarán la superposición de cierre del elemento por cambio de foco en Xbox pero no de otras familias de dispositivos, aunque las aplicaciones pueden optar por forzar la superposición siempre en **Activado** o siempre en **Desactivado**.

```xaml
<MenuFlyout LightDismissOverlayMode=\"Off\">
```
</div>

## <a name="dialogs"></a>Cuadros de diálogo
### <a name="general-guidelines"></a>Directrices generales

-   Identifica claramente el problema o el objetivo del usuario en la primera línea del texto del cuadro de diálogo.
-   El título del cuadro de diálogo es la instrucción principal y es opcional.
    -   Usa un título corto para explicar lo que se necesita hacer con el cuadro de diálogo. Los títulos largos no se ajustan y se truncan.
    -   Puedes omitir el título si vas a usar el cuadro de diálogo para transmitir un mensaje sencillo, un error o una pregunta. Deja que el texto del contenido transmita esta información esencial.
    -   Asegúrate de que el título esté relacionado directamente con las opciones de botón.
-   El contenido del cuadro de diálogo contiene el texto descriptivo y es obligatorio.
    -   Presenta el mensaje, el error o la pregunta de bloqueo de la manera más sencilla posible.
    -   Si se usa un cuadro de diálogo, usa el área de contenido para proporcionar más detalles o definir terminología. No repitas el título con otras palabras ligeramente distintas.
-   Al menos debe aparecer un botón de cuadro de diálogo.
    -   Los botones son el único mecanismo para que los usuarios descarten el cuadro de diálogo.
    -   Usa botones con texto que identifique respuestas específicas a la instrucción principal o el contenido. Un ejemplo sería: "¿Quieres permitir que nombreDeAplicación acceda a tu ubicación?", seguido de los botones "Permitir" y "Bloquear". Cuando las respuestas son específicas, se comprenden con mayor rapidez y la toma de decisiones resulta eficaz.
    - Presenta los botones de confirmación en el siguiente orden: 
        -   Aceptar/[Hacerlo]/Sí
        -   [No hacerlo]/No
        -   Cancelar
        
        (donde [Hacerlo] y [No hacerlo] son respuestas específicas a la instrucción principal).
   
-   Los cuadros de diálogo de error muestran el mensaje de error en el cuadro de diálogo, junto con la información pertinente. El único botón que se usa en un cuadro de diálogo de error debe ser "Cerrar" o una acción similar.
-   No uses cuadros de diálogo en el caso de los errores que son contextuales para un lugar específico de la página, como los errores de validación (en los campos de contraseña, por ejemplo); usa el lienzo de la aplicación para mostrar errores en línea.

### <a name="confirmation-dialogs-okcancel"></a>Cuadros de diálogo de confirmación (Aceptar/Cancelar)
Un cuadro de diálogo de confirmación ofrece a los usuarios la posibilidad de confirmar que desean realizar una acción. Pueden confirman la acción o cancelarla.  
Un cuadro de diálogo de confirmación típico tiene dos botones: un botón de afirmación ("Aceptar") y un botón de cancelación.  

<ul>
    <li>
        <p>En general, el botón afirmación debe estar a la izquierda (el botón primario) y el botón de cancelación (el botón secundario) debe estar en la derecha.</p>
         ![Un cuadro de diálogo de Aceptar/Cancelar](images/dialogs/dialog-delete-file-example.png)
        
    </li>
    <li>Como se explicó en la sección de recomendaciones generales, usa botones con texto que identifique respuestas específicas a la instrucción principal o al contenido.
    </li>
</ul>

> Algunas plataformas colocan el botón de afirmación a la derecha en lugar de a la izquierda. Entonces, ¿por qué recomendamos colocarlo a la izquierda?  Si supones que la mayoría de los usuarios son diestros y sujetan su teléfono con esa mano, realmente resulta más cómodo presionar el botón de afirmación cuando está a la izquierda, porque es más probable que el botón esté dentro del arco del pulgar del usuario. Los botones situados en el lado derecho de la pantalla requerirán que el usuario cambie la posición de su pulgar hacia dentro a una posición menos cómoda.

### <a name="create-a-dialog"></a>Crear un cuadro de diálogo
Para crear un cuadro de diálogo, usa la [clase ContentDialog](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx). Puedes crear un cuadro de diálogo en el código o el marcado. Aunque suele ser más fácil definir los elementos de la interfaz de usuario en XAML, en el caso de un cuadro de diálogo simple, es más sencillo usar código solamente. En este ejemplo se crea un cuadro de diálogo para notificar al usuario que no hay conexión Wi-Fi y luego se usa el método [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) para mostrarlo.

```csharp
private async void displayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog()
    {
        Title = "No wifi connection",
        Content = "Check connection and try again",
        PrimaryButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Cuando el usuario hace clic en un botón de cuadro de diálogo, el método [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) devuelve una enumeración [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) para que sepas en qué botón hace clic el usuario. 

El cuadro de diálogo de este ejemplo formula una pregunta y usa el valor devuelto [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) para determinar la respuesta del usuario. 

```csharp
private async void displayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog()
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        SecondaryButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();
    
    // Delete the file if the user clicked the primary button. 
    /// Otherwise, do nothing. 
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file. 
    }
}
```

## <a name="flyouts"></a>Controles flotantes
###  <a name="create-a-flyout"></a>Crear un control flotante

Un control flotante es un contenedor abierto que puede mostrar una interfaz de usuario arbitraria como su contenido. 

<div class="microsoft-internal-note">
Esto incluye controles flotantes y menús contextuales, que se pueden anidar dentro de otros controles flotantes.
</div>

Los controles flotantes se asocian a controles específicos. Puedes usar la propiedad [Placement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.placement.aspx) para especificar dónde aparece el control flotante: superior, inferior, izquierda, derecha o completo. Si seleccionas el [modo de colocación completa](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutplacementmode.aspx), la aplicación amplía el control flotante y lo centra dentro de la ventana de la aplicación. Cuando son visibles, deben anclarse al objeto de invocación y se debe especificar su posición relativa preferida con relación al objeto: arriba, izquierda, abajo o derecha. El control flotante también tiene un modo de colocación completa que intenta ampliar el control flotante y centrarlo dentro de la ventana de la aplicación. Algunos controles, como [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx), proporcionan una propiedad [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) que puedes usar para asociar un control flotante. 

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

Si el control no tiene una propiedad Flyout, puedes usar la propiedad asociada [FlyoutBase.AttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.attachedflyout.aspx). Si haces esto, también tienes que llamar al método [FlyoutBase.ShowAttachedFlyout](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.showattachedflyout) para mostrar el control flotante. 

En este ejemplo se agrega un control flotante simple a una imagen. Cuando el usuario pulsa la imagen, la aplicación muestra el control flotante. 

````xaml
<Image Source="Assets/cliff.jpg" Width="50" Height="50" 
  Margin="10" Tapped="Image_Tapped">
  <FlyoutBase.AttachedFlyout>
    <Flyout>
      <TextBlock TextWrapping="Wrap" Text="This is some text in a flyout."  />
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
        <Flyout.FlyoutPresenterStyle>
            <Style TargetType="FlyoutPresenter">
                <Setter Property="ScrollViewer.ZoomMode" Value="Enabled"/>
                <Setter Property="Background" Value="Black"/>
                <Setter Property="BorderBrush" Value="Gray"/>
                <Setter Property="BorderThickness" Value="5"/>
                <Setter Property="MinHeight" Value="300"/>
                <Setter Property="MinWidth" Value="300"/>
            </Style>
        </Flyout.FlyoutPresenterStyle>
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

### <a name="style-a-flyout"></a>Diseñar un control flotante
Para diseñar un control flotante, modifica su propiedad [FlyoutPresenterStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.flyoutpresenterstyle.aspx). En el siguiente ejemplo se muestra un párrafo de texto ajustado y se permite que el bloque de texto sea accesible para un lector de pantalla.

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

## <a name="get-the-samples"></a>Obtener las muestras
*   [Conceptos básicos de la interfaz de usuario de XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    Consulta todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados
- [Información sobre herramientas](tooltips.md)
- [Menús y menús contextuales](menus.md)
- [**Clase Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [**Clase ContentDialog**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)
