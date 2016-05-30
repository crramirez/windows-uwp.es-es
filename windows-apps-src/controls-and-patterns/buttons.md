---
author: Jwmsft
label: Buttons
template: detail.hbs
---
# Botones
Un botón ofrece al usuario una forma de desencadenar una acción inmediata.

![Ejemplo de botones](images/controls/button.png)


<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [**Clase Button**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
-   [**Clase RepeatButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx)
-   [**Evento Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

## ¿Es este el control adecuado?

Un botón permite al usuario iniciar una acción inmediata, como enviar un formulario.

No uses un botón cuando la acción sea navegar a otra página. Para eso, es mejor usar un vínculo. Para obtener más información, consulta [Hipervínculos](hyperlinks.md).
    
> Excepción: para la navegación por asistentes, usa los botones llamados "Atrás" y "Siguiente". Para otros tipos de exploración hacia atrás o a un nivel superior, usa un botón Atrás.

## Ejemplo

Este ejemplo usa dos botones, Cerrar todo y Cancelar, en un cuadro de diálogo en el navegador Microsoft Edge. 

![Ejemplo de botones que se usan en un cuadro de diálogo](images/control-examples/buttons-edge.png)

## Crear un botón

Este ejemplo muestra un botón que responde a un clic. 

Crear el botón en XAML.

```xaml
<Button Content="Submit" Click="SubmitButton_Click"/>
```

O crear el botón en el código.

```csharp
Button submitButton = new Button();
submitButton.Content = "Submit";
submitButton.Click += SubmitButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(submitButton);
```

Controlar el evento Click.

```csharp
private async void SubmitButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to submit form. For example:
    // form.Submit();
    Windows.UI.Popups.MessageDialog messageDialog = 
        new Windows.UI.Popups.MessageDialog("Thank you for your submission.");
    await messageDialog.ShowAsync();
}
```

### Interacción del botón

Cuando se pulsa un botón con un dedo o lápiz o se presiona el botón izquierdo del mouse mientras el puntero está sobre él, botón genera el evento [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Si un botón tiene el foco del teclado, al presionar la tecla ENTRAR o la barra espaciadora también se genera el evento Click.

Por lo general, no se pueden manipular eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) de bajo nivel en un Button porque tiene el comportamiento Click en su lugar. Para obtener más información, consulta el tema de [introducción a los eventos y eventos enrutados](https://msdn.microsoft.com/en-us/library/windows/apps/mt185584.aspx).

Puedes cambiar la forma en que un botón genera el evento Click cambiando la propiedad [**ClickMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.clickmode.aspx). El valor de ClickMode predeterminado es **Release**. Si ClickMode es **Hover**, no se genera el evento Click con el teclado o de forma táctil. 


### Contenido de Button

Button es una clase [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx). Su propiedad de contenido XAML es [**Content**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx), que habilita una sintaxis así para XAML: `<Button>A button's content</Button>`. Puedes establecer cualquier objeto como contenido del botón. Si el contenido es una clase [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), se representa en el botón. Si el contenido es otro tipo de objeto, su representación de cadena se muestra en el botón.

Aquí, un elemento **StackPanel** que contiene texto y una imagen de una banana se establece como el contenido de un botón.

```xaml
<Button Click="Button_Click" 
        Background="#FF0D6AA3" 
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Slices.png" Height="62"/>
        <TextBlock Text="Orange"  Foreground="White"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

El botón tendrá el siguiente aspecto:

![Botón con contenido de texto e imagen](images/button-orange.png)

## Crear un botón Repetir

Un [**RepeatButton**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) es un botón que genera eventos [**Click**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) repetidamente desde que se presiona hasta que se suelta. Establece la propiedad [**Delay**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) para especificar el tiempo que RepeatButton espera después de presionarse antes de que empiece a repetir la acción de clic. Establece la propiedad [**Interval**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) para especificar el tiempo entre las repeticiones de la acción de clic. Los tiempos de ambas propiedades se especifican en milisegundos.

El siguiente ejemplo muestra dos controles de RepeatButton cuyos respectivos eventos Click se usan para aumentar o disminuir el valor mostrado en un bloque de texto.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## Recomendaciones

-   Asegúrate de que el objetivo y el estado del botón sean claros para el usuario.
-   Usa un texto conciso, específico y descriptivo que explique claramente la acción que realiza el botón. Por lo general, el contenido del texto del botón es una única palabra, un verbo.
-   Si el contenido del texto del botón es dinámico, por ejemplo, está localizado, decide cómo cambiará de tamaño el botón y qué ocurrirá con los controles que lo rodean.
-   En el caso de los botones de comando con contenido de texto, usa un ancho de botón mínimo.
-   Evita los botones de comando con contenido de texto estrechos, cortos o altos.
-   Use la fuente predeterminada, a menos que las directrices de marca le indiquen lo contrario.
-   En el caso de las acciones que deban estar disponibles en varias páginas de la aplicación, en vez de duplicar el mismo botón en varias páginas, piensa en utilizar una [barra de la aplicación inferior](app-bars.md).
-   Expón solo uno de los dos botones al usuario a la vez. Por ejemplo, Aceptar y Cancelar. Si necesitas exponer más acciones al usuario, considera la posibilidad de usar [casillas](checkbox.md) o [botones de radio](radio-button.md) con los que el usuario pueda seleccionar acciones, con un único botón de comando para desencadenar estas acciones.
-   Usa el botón de comando predeterminado para indicar la acción más habitual o recomendada.
-   Sopesa la posibilidad de personalizar los botones. La forma de los botones es siempre rectangular, aunque puedes personalizar los elementos visuales que forman la apariencia del botón. Por lo general, el contenido de un botón es texto como, por ejemplo, Aceptar o Cancelar, pero puedes reemplazar el texto por un icono o usar un icono y texto.
-   Comprueba que, cuando el usuario interactúe con el botón, este cambie de estado y de apariencia para proporcionar información al usuario. Ejemplos de estados de un botón: normal, presionado o deshabilitado.
-   Desencadena la acción del botón cuando el usuario pulse o presione el botón. Normalmente, la acción se desencadena cuando el usuario libera el botón, aunque también puedes establecer que la acción del botón se desencadene cuando un dedo lo presione.
-   No use un botón de comando para establecer el estado.
-   No cambies el texto del botón mientras se ejecuta la aplicación. Por ejemplo, no cambies el texto de un botón que dice "Siguiente" a "Continuar".
-   No intercambies los estilos predeterminados de enviar, restablecer y botón.
-   No pongas demasiado contenido en un botón. Haz que el contenido sea escueto y fácil de entender (apenas una imagen y algo de texto).

## Botones Atrás
El botón Atrás es una prestación de interfaz de usuario proporcionada por el sistema que permite la navegación hacia atrás a través de la pila de retroceso o el historial de navegación del usuario.

El ámbito del historial de navegación (en la aplicación o global) depende del dispositivo y el modo de dispositivo.

## <span id="examples"></span><span id="EXAMPLES"></span>Ejemplo


La interfaz de usuario del botón Atrás del sistema está optimizada para cada dispositivo y tipo de entrada, pero la experiencia de navegación es global y coherente en los diferentes dispositivos y aplicaciones para la Plataforma universal de Windows (UWP). Estas experiencias diferentes incluyen lo siguiente:

Sistema de teléfono de dispositivo ![en un teléfono](images/nav-back-phone.png)
-   Siempre está presente.
-   Un botón de hardware o de software situado en la parte inferior del dispositivo.
-   Navegación hacia atrás global dentro de la aplicación y entre aplicaciones.

<span id="Tablet"></span><span id="tablet"></span><span id="TABLET"></span>Sistema de tableta ![en una tableta (en modo tableta)](images/nav-back-tablet.png)
-   Siempre está presente en el modo tableta.

    No está disponible en modo de escritorio. En su lugar, se puede habilitar botón Atrás de la barra de título. Consulta [PC, Portátil, Tableta](#PC).

    Los usuarios pueden cambiar entre ejecutar en modo tableta y modo de escritorio desde **Configuración &gt; Sistema &gt; Modo tableta** con la opción **Hacer que Windows se adapte mejor a los gestos táctiles al usar el dispositivo como tableta**.

-   Un botón de software de la barra de navegación situado en la parte inferior del dispositivo.
-   Navegación hacia atrás global dentro de la aplicación y entre aplicaciones.

<span id="PC"></span><span id="pc"></span>Sistema de PC, equipo portátil y tableta ![en un equipo de escritorio o portátil](images/nav-back-pc.png)
-   Opcional en el modo de escritorio.

    No está disponible en modo tableta. Consulta [Tableta](#Tablet).

    Deshabilitado de manera predeterminada. Deben participar para habilitarlo.

    Los usuarios pueden cambiar entre ejecutar en modo tableta y modo de escritorio desde **Configuración &gt; Sistema &gt; Modo tableta** con la opción **Hacer que Windows se adapte mejor a los gestos táctiles al usar el dispositivo como tableta**.

-   Un botón de software situado en la barra de título de la aplicación.
-   Navegación hacia atrás solo dentro de la aplicación. No es compatible con la navegación entre aplicaciones.

Sistema de Surface Hub ![en Surface Hub](images/nav-back-surfacehub.png)
-   Siempre está presente.
-   Un botón de software situado en la parte inferior del dispositivo.
-   La navegación hacia atrás dentro de la aplicación y entre aplicaciones.

 

## Lo que se debe y no se debe hacer


-   Habilitar la navegación hacia atrás.

    Si no se habilita la navegación hacia atrás, la aplicación se incluye en la pila de retroceso global pero no se mantiene el historial de navegación de página en la aplicación.

-   Habilitar el botón Atrás de la barra de título en modo de escritorio.

    Se mantiene el historial de navegación de página en la aplicación; no se admite la navegación hacia atrás entre aplicaciones.

    **Nota**  En el modo tableta, la barra de título se muestra cuando un usuario se desliza rápidamente hacia abajo desde la parte superior del dispositivo o mueve el puntero del mouse cerca de la parte superior del dispositivo. Para evitar confusiones y duplicaciones, el botón Atrás de la barra de título no se muestra en modo tableta.

     

-   Ocultar o deshabilitar el botón Atrás de la barra de título en modo de escritorio cuando el historial de navegación en la aplicación está agotado o no disponible.

    Proporciona una indicación clara al usuario de que ha navegado lo más atrás posible.

-   Cada comando atrás debe retroceder una página en la pila de retroceso o, si no está en modo de escritorio, a la aplicación inmediatamente anterior.

    Los usuarios podrían confundirse si la navegación hacia atrás no es intuitiva, coherente y predecible.

## Artículos relacionados

- [Botones de radio](radio-button.md)
- [Modificadores para alternar](toggles.md)
- [Casillas](checkbox.md)

**Para desarrolladores (XAML)**
- [**Clase Button**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)




<!--HONumber=May16_HO2-->


