---
author: mijacobs
Description: "Los cuadros de diálogo y los controles flotantes muestran elementos transitorios de la interfaz de usuario que aparecen cuando el usuario los solicita o cuando sucede algo que requiere notificación o aprobación."
title: "Cuadros de diálogo y controles flotantes"
label: Dialogs
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: a1e7ecd60c960459d8d146bbda0fa6fba4bfc7f6
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2017
---
# <a name="dialogs-and-flyouts"></a>Cuadros de diálogo y controles flotantes

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Los cuadros de diálogo y los controles flotantes son elementos transitorios de la interfaz de usuario que aparecen cuando sucede algo que requiere notificación, aprobación o información adicional por parte del usuario.

> **API importantes**: [Clase ContentDialog](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx), [Clase Flyout](https://msdn.microsoft.com/library/windows/apps/dn279496)

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>Cuadros de diálogo</b> <br/><br/>
    ![Ejemplo de un cuadro de diálogo](images/dialogs/dialog_RS2_delete_file.png)</p>
<p>Los cuadros de diálogo son superposiciones modales en la interfaz de usuario que proporcionan información contextual sobre la aplicación. Los cuadros de diálogo bloquean las interacciones con la ventana de la aplicación hasta que se descarten de forma explícita. A menudo solicitan algún tipo de acción por parte del usuario.   
</p><br/>

  </div>
  <div class="side-by-side-content-right">
   <p><b>Controles flotantes</b> <br/><br/>
   ![Ejemplo de un control flotante](images/flyout-example2.png)</p>
<p>Un control flotante es un elemento emergente contextual ligero que muestra la interfaz de usuario relacionada con lo que está haciendo el usuario. Incluye lógica de colocación y tamaño, y se puede usar para mostrar un control secundario o más detalles acerca de un elemento.
</p><p>A diferencia de un cuadro de diálogo, un control flotante se puede descartar rápidamente al pulsar o hacer clic en algún lugar fuera de dicho control flotante, presionar la tecla Escape (Esc) o el botón Atrás, cambiar el tamaño de la ventana de la aplicación o cambiar la orientación del dispositivo.
</p><br/>

  </div>
</div>
</div>

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

* Usa los cuadros de diálogo para notificar a los usuarios información importante o para solicitar información adicional o confirmación para completar una acción.
* No uses un control flotante en lugar de una [información sobre herramientas](tooltips.md) o un [menú contextual](menus.md). Usa una información sobre herramientas para mostrar una descripción breve que se oculta tras un tiempo determinado. Usa un menú contextual para acciones contextuales relacionadas con un elemento de la interfaz de usuario, como copiar y pegar.  


Los cuadros de diálogo y los controles flotantes garantizan que los usuarios estén al tanto de información importante, pero también interrumpen la experiencia del usuario. Dado que los cuadros de diálogo son modales (bloqueo), interrumpen a los usuarios impidiéndoles realizar cualquier otra acción hasta que interactúan con ellos. Los controles flotantes ofrecen una experiencia menos molesta, pero si se muestran en exceso, pueden resultar perturbadores.

Ten en cuenta la importancia de la información que quieres compartir: ¿es lo suficientemente importante como para que interrumpa al usuario? Considera también la frecuencia con la que debe mostrarse la información. Si vas a mostrar un cuadro de diálogo o una notificación cada pocos minutos, tal vez quieras asignar espacio para dicha información en la interfaz de usuario principal. Por ejemplo, en un cliente de chat, en lugar de mostrar un control flotante cada vez que un amigo inicie sesión, podrías mostrar una lista de amigos que estén en línea en ese momento y resaltar los amigos a medida que inicien sesión.

Los cuadros de diálogo se usan con frecuencia para confirmar una acción (por ejemplo, eliminar un archivo) antes de ejecutarla. Si esperas que el usuario realice una acción concreta con frecuencia, considera la posibilidad de ofrecerle una forma de deshacer la acción si fuese un error, en lugar de obligarlo a confirmar la acción cada vez.

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
<li>Mostrar más información, como detalles o descripciones más largas de un elemento de la página.</li>
</ul></p>
  </div>
</div>
</div>

## <a name="dialogs"></a>Cuadros de diálogo
### <a name="general-guidelines"></a>Directrices generales

-   Identifica claramente el problema o el objetivo del usuario en la primera línea del texto del cuadro de diálogo.
-   El título del cuadro de diálogo es la instrucción principal y es opcional.
    -   Usa un título corto para explicar lo que se necesita hacer con el cuadro de diálogo.
    -   Puedes omitir el título si vas a usar el cuadro de diálogo para transmitir un mensaje sencillo, un error o una pregunta. Deja que el texto del contenido transmita esta información esencial.
    -   Asegúrate de que el título esté relacionado directamente con las opciones de botón.
-   El contenido del cuadro de diálogo contiene el texto descriptivo y es obligatorio.
    -   Presenta el mensaje, el error o la pregunta de bloqueo de la manera más sencilla posible.
    -   Si se usa un cuadro de diálogo, usa el área de contenido para proporcionar más detalles o definir terminología. No repitas el título con otras palabras ligeramente distintas.
-   Debe aparecer al menos un botón de cuadro de diálogo.
    -   Asegúrate de que el cuadro de diálogo tiene al menos un botón correspondiente a una acción segura y no destructiva como "Entendido", "Cerrar" o "Cancelar". Usa la API de CloseButton para agregar este botón.
    -   Usa respuestas específicas al contenido o a la instrucción principal como texto del botón. Un ejemplo sería: "¿Quieres permitir que nombreDeAplicación acceda a tu ubicación?", seguido de los botones "Permitir" y "Bloquear". Cuando las respuestas son específicas, se comprenden con mayor rapidez y la toma de decisiones resulta eficaz.
    - Asegúrate de que el texto de los botones de acción sea conciso. Las cadenas cortas permiten al usuario realizar una selección de manera rápida y segura.
    - Además de la acción segura y no destructiva, opcionalmente podrá presentar al usuario uno o dos botones de acción relacionados con la instrucción principal. Estos botones de acción "hacerlo" confirman el principal punto del cuadro de diálogo. Usa las API de PrimaryButton y SecondaryButton APIs para agregar estas acciones "hacerlo".
    - Los botones de acción "hacerlo" deberían aparecer como los botones situados más a la izquierda. La acción segura y no destructiva debe aparecer como el botón situado más a la derecha.
    - Opcionalmente, puedes elegir diferenciar uno de los tres botones como el botón predeterminado del cuadro de diálogo. Usa la API de DefaultButton para diferenciar uno de los botones.  
-   No uses cuadros de diálogo en el caso de los errores que son contextuales para un lugar específico de la página, como los errores de validación (en los campos de contraseña, por ejemplo); usa el lienzo de la aplicación para mostrar errores en línea.
- Usa la [clase ContentDialog](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx) para crear tu experiencia de cuadro de diálogo. No uses la API de MessageDialog en desuso.

### <a name="dialog-scenarios"></a>Escenarios de cuadros de diálogo
Dado que los cuadros de diálogo bloquean la interacción del usuario, y que los botones son el mecanismo principal para que los usuarios descarten el cuadro de diálogo, asegúrate de que el diálogo contiene al menos un botón "seguro" y un botón no destructivo como "Cerrar" o "Entendido". **Todos los cuadros de diálogo deben contener al menos un botón de acción segura para cerrar el cuadro de diálogo.** Esto garantiza que el usuario puede cerrar con confianza el cuadro de diálogo sin realizar una acción.
![Un cuadro de diálogo con un botón](images/dialogs/dialog_RS2_one_button.png)

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Cuando los cuadros de diálogo se usan para mostrar una pregunta de bloqueo, su cuadro de diálogo debe presentar al usuario los botones de acción relacionados con la pregunta. El botón "seguro" y no destructivo puede ir acompañado de uno o dos botones de acción "hacerlo". Cuando se le presenten al usuario varias opciones, asegúrate de que los botones explican con claridad las acciones "hacerlo" y segura o "no hacerlo" relacionadas con la pregunta propuesta.

![Un cuadro de diálogo con dos botones](images/dialogs/dialog_RS2_two_button.png)

```csharp
private async void DisplayLocationPromptDialog()
{
    ContentDialog locationPromptDialog = new ContentDialog
    {
        Title = "Allow AppName to access your location?",
        Content = "AppName uses this information to help you find places, connect with friends, and more.",
        CloseButtonText = "Block",
        PrimaryButtonText = "Allow"
    };

    ContentDialogResult result = await locationPromptDialog.ShowAsync();
}
```

Los cuadros de diálogo de tres botones se usan cuando le presentas al usuario dos acciones "hacerlo" y una acción "no hacerlo". Se deben usar cuadros de diálogo de tres botones con moderación con distinciones claras entre la acción secundaria y la acción segura/cerrar.

![Cuadro de diálogo de tres botones](images/dialogs/dialog_RS2_three_button.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="the-three-dialog-buttons"></a>Los tres botones de cuadro de diálogo
ContentDialog cuenta con tres tipos diferentes de botones que puedes usar para crear una experiencia de cuadro de diálogo.

- **CloseButton** (obligatorio): representa la acción no destructiva y segura que permite al usuario salir del cuadro de diálogo. Aparece como el botón que se encuentra situado más a la derecha.
- **PrimaryButton** (opcional): representa la primera acción "hacerlo". Aparece como el botón que se encuentra situado más a la izquierda.
- **SecondaryButton** (opcional): representa la segunda acción "hacerlo". Aparece como el botón central.

Con los botones integrados, los botones se colocarán correctamente; te asegurarás de que responden correctamente a eventos de teclado, de que el área de comandos permanece visible incluso cuando el teclado en pantalla está activado y de que el aspecto del cuadro de diálogo es coherente con otros cuadros de diálogo.

#### <a name="closebutton"></a>CloseButton
Cada cuadro de diálogo debe contener un botón de acción segura y no destructiva que permita al usuario salir del cuadro de diálogo con confianza.

Usa la API de ContentDialog.CloseButton para crear este botón. Esto te permite crear la experiencia de usuario adecuada para todas las entradas como el mouse, el teclado, táctiles y controlador para juegos. Esta experiencia tendrá lugar cuando:
<ol>
    <li>El usuario hace clic o pulsa en el CloseButton. </li>
    <li>El usuario presiona el botón Atrás del sistema. </li>
    <li>El usuario presiona la tecla ESC del teclado. </li>
    <li>El usuario presiona el botón B del controlador para juegos. </li>
</ol>

Cuando el usuario hace clic en un botón de cuadro de diálogo, el método [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) devuelve una enumeración [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) para que sepas en qué botón hace clic el usuario. Si se presiona en CloseButton se devuelve ContentDialogResult.None.

#### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton y SecondaryButton
Además del CloseButton, opcionalmente puede presentar al usuario uno o dos botones de acción relacionados con la instrucción principal.
Aprovecha el PrimaryButton para la primera acción "hacerlo" y el SecondaryButton para la segunda acción "hacerlo". En los cuadros de diálogo de tres botones, el PrimaryButton suele representar la acción afirmativa "hacerlo", mientras que el SecondaryButton suele representar una acción "hacerlo" neutra o secundaria.
Por ejemplo, una aplicación puede pedir al usuario que se suscriba a un servicio. El PrimaryButton como la acción "hacerlo" afirmativa hospedaría el texto Suscribirse mientras que el SecondaryButton como la acción "hacerlo" hospedaría el texto Pruébalo. El CloseButton permitiría al usuario cancelar sin llevar a cabo cualquiera de las acciones.

Cuando el usuario hace clic en el PrimaryButton, el método [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) devuelve ContentDialogResult.Primary.
Cuando el usuario hace clic en el SecondaryButton, el método [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) devuelve ContentDialogResult.Secondary.

![Cuadro de diálogo de tres botones](images/dialogs/dialog_RS2_three_button.png)

#### <a name="defaultbutton"></a>DefaultButton
Opcionalmente, puede elegir diferenciar uno de los tres botones como el botón predeterminado. Especificar el botón predeterminado hace que se produzca lo siguiente:
- El botón recibe el tratamiento visual del botón de énfasis.
- El botón responderá a la tecla ENTRAR automáticamente.
    - Cuando el usuario presiona la tecla ENTRAR del teclado, se activará el controlador de clic asociado con el botón predeterminado y el ContentDialogResult devolverá el valor asociado con el botón predeterminado.
    - Si el usuario ha colocado el foco del teclado en un control que controla ENTRAR, el botón predeterminado no responderá al presionar ENTRAR.
- El botón recibirá el foco automáticamente cuando se abra el cuadro de diálogo, a menos que el contenido del cuadro de diálogo contenga interfaz de usuario activable.

Usa la propiedad ContentDialog.DefaultButton para indicar el botón predeterminado. De manera predeterminada, no se establece ningún botón predeterminado.

![Un cuadro de diálogo de tres botones con un botón predeterminado](images/dialogs/dialog_RS2_three_button_default.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it",
        DefaultButton = ContentDialogButton.Primary
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="confirmation-dialogs-okcancel"></a>Cuadros de diálogo de confirmación (Aceptar/Cancelar)
Un cuadro de diálogo de confirmación ofrece a los usuarios la posibilidad de confirmar que desean realizar una acción. Pueden confirman la acción o cancelarla.  
Un cuadro de diálogo de confirmación típico tiene dos botones: un botón de afirmación ("Aceptar") y un botón de cancelación.  

<ul>
    <li>
        <p>En general, el botón afirmación debe estar a la izquierda (el botón primario) y el botón de cancelación (el botón secundario) debe estar en la derecha.</p>
         ![Un cuadro de diálogo de Aceptar/Cancelar](images/dialogs/dialog_RS2_delete_file.png)

    </li>
    <li>Como se explicó en la sección de recomendaciones generales, usa botones con texto que identifique respuestas específicas a la instrucción principal o al contenido.
    </li>
</ul>

> Algunas plataformas colocan el botón de afirmación a la derecha en lugar de a la izquierda. Entonces, ¿por qué recomendamos colocarlo a la izquierda?  Si supones que la mayoría de los usuarios son diestros y sujetan su teléfono con esa mano, realmente resulta más cómodo presionar el botón de afirmación cuando está a la izquierda, porque es más probable que el botón esté dentro del arco del pulgar del usuario. Los botones situados en el lado derecho de la pantalla requerirán que el usuario cambie la posición de su pulgar hacia dentro a una posición menos cómoda.

### <a name="create-a-dialog"></a>Crear un cuadro de diálogo
Para crear un cuadro de diálogo, usa la [clase ContentDialog](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx). Puedes crear un cuadro de diálogo en el código o el marcado. Aunque suele ser más fácil definir los elementos de la interfaz de usuario en XAML, en el caso de un cuadro de diálogo simple, es más sencillo usar código solamente. En este ejemplo se crea un cuadro de diálogo para notificar al usuario que no hay conexión Wi-Fi y luego se usa el método [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) para mostrarlo.

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Cuando el usuario hace clic en un botón de cuadro de diálogo, el método [ShowAsync](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialog.showasync.aspx) devuelve una enumeración [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) para que sepas en qué botón hace clic el usuario.

El cuadro de diálogo de este ejemplo formula una pregunta y usa el valor devuelto [ContentDialogResult](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.contentdialogresult.aspx) para determinar la respuesta del usuario.

```csharp
private async void DisplayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        CloseButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();

    // Delete the file if the user clicked the primary button.
    /// Otherwise, do nothing.
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file.
    }
    else
    {
        // The user clicked the CLoseButton, pressed ESC, Gamepad B, or the system back button.
        // Do nothing.
    }
}
```

## <a name="flyouts"></a>Controles flotantes
###  <a name="create-a-flyout"></a>Crear un control flotante

Un control flotante es un control de cierre del elemento por cambio de foco que puede mostrar una interfaz de usuario arbitraria como su contenido. Los controles flotantes pueden contener otros controles flotantes o menús contextuales para crear una experiencia anidada.

![Menú contextual anidado dentro de un control flotante](images/flyout-nested.png)

Los controles flotantes se asocian a controles específicos. Puedes usar la propiedad [Placement](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.placement.aspx) para especificar dónde aparece un control flotante: superior, inferior, izquierda, derecha o completo. Si seleccionas el [modo de colocación completa](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.primitives.flyoutplacementmode.aspx), la aplicación amplía el control flotante y lo centra dentro de la ventana de la aplicación. Algunos controles, como [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx), proporcionan una propiedad [Flyout](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.flyout.aspx) que puedes usar para asociar un control flotante o [menú contextual](menus.md).

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

### <a name="style-a-flyout"></a>Diseñar un control flotante
Para diseñar un control flotante, modifica su propiedad [FlyoutPresenterStyle](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.flyout.flyoutpresenterstyle.aspx). En el siguiente ejemplo se muestra un párrafo de texto ajustado y se permite que el bloque de texto sea accesible para un lector de pantalla.

![Control flotante accesible texto ajustado](images/flyout-wrapping-text.png)

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

#### <a name="styling-flyouts-for-10-foot-experience"></a>Dar estilo a controles flotantes para la experiencia de 10 pies

Los controles de cierre del elemento por cambio de foco como los controles flotantes capturan el foco del teclado y del controlador para juegos dentro de su interfaz de usuario transitoria hasta que se cierra. Para proporcionar una indicación visual para este comportamiento, los controles de cierre del elemento por cambio de foco de Xbox dibujan una superposición que atenúa el contraste y la visibilidad de la interfaz de usuario que está fuera del ámbito. Este comportamiento se puede modificar con la propiedad [`LightDismissOverlayMode`](https://msdn.microsoft.com/ library/windows/apps/windows.ui.xaml.controls.primitives.flyoutbase.lightdismissoverlaymode.aspx). De manera predeterminada, los controles flotantes dibujarán la superposición de cierre del elemento por cambio de foco en Xbox pero no de otras familias de dispositivos, aunque las aplicaciones pueden optar por forzar la superposición siempre en **Activado** o siempre en **Desactivado**.

![Control flotante con atenuación de superposición](images/flyout-smoke.png)

```xaml
<MenuFlyout LightDismissOverlayMode="On">
```

### <a name="light-dismiss-behavior"></a>Comportamiento de control de cierre del elemento por cambio de foco
Los controles flotantes se pueden cerrar con una rápida acción de cierre del elemento por cambio de foco
-    Pulsar fuera del control flotante
-    Presionar la tecla de teclado Escape
-    Presionar el botón Atrás del sistema de hardware o software
-    Presionar el botón B del controlador para juegos

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

## <a name="get-the-samples"></a>Obtener las muestras
*   [Conceptos básicos de la interfaz de usuario de XAML](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/XamlUIBasics)<br/>
    Consulta todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados
- [Información sobre herramientas](tooltips.md)
- [Menús y menús contextuales](menus.md)
- [Clase Flyout](https://msdn.microsoft.com/library/windows/apps/dn279496)
- [Clase ContentDialog](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentdialog.aspx)