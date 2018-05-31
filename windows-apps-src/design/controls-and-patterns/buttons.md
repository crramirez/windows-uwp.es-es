---
author: serenaz
Description: A button gives the user a way to trigger an immediate action.
title: Botones
label: Buttons
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f663ce9da6453922e35f850a8cd039f33770f434
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2018
ms.locfileid: "1494172"
---
# <a name="buttons"></a>Botones


Un botón ofrece al usuario una forma de desencadenar una acción inmediata.

> **API importantes**: [Clase Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx), [Clase RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx), [Evento Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)

![Ejemplo de botones](images/controls/button.png)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Un botón permite al usuario iniciar una acción inmediata, como enviar un formulario.

No uses un botón cuando la acción sea navegar a otra página. Para eso, es mejor usar un vínculo. Para obtener más información, consulta [Hipervínculos](hyperlinks.md).
> Excepción: para la navegación por asistentes, usa los botones llamados "Atrás" y "Siguiente". Para otros tipos de exploración hacia atrás o a un nivel superior, usa un botón Atrás.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Button">abrir la aplicación y ver el botón en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Este ejemplo usa dos botones, Permitir y Bloquear, en un cuadro de diálogo en el que se solicita acceso a la ubicación.

![Ejemplo de botones que se usan en un cuadro de diálogo](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>Crear un botón

Este ejemplo muestra un botón que responde a un clic.

Crear el botón en XAML.

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

O crear el botón en el código.

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

Controlar el evento Click.

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
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

### <a name="button-interaction"></a>Interacción del botón

Cuando se pulsa un botón con un dedo o lápiz o se presiona el botón izquierdo del mouse mientras el puntero está sobre él, botón genera el evento [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Si un botón tiene el foco del teclado, al presionar la tecla ENTRAR o la barra espaciadora también se genera el evento Click.

Por lo general, no se pueden manipular eventos [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) de bajo nivel en un Button porque tiene el comportamiento Click en su lugar. Para obtener más información, consulta el tema de [introducción a los eventos y eventos enrutados](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx).

Puedes cambiar la forma en que un botón genera el evento Click cambiando la propiedad [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode). El valor predeterminado de ClickMode es **Release**, pero también puedes configurar el ClickMode de un botón como **Hover** o **Press**. Si ClickMode es **Hover**, no se genera el evento Click con el teclado o de forma táctil.


### <a name="button-content"></a>Contenido de Button

Button es una clase [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx). Su propiedad de contenido XAML es [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx), que habilita una sintaxis así para XAML: `<Button>A button's content</Button>`. Puedes establecer cualquier objeto como contenido del botón. Si el contenido es una clase [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), se representa en el botón. Si el contenido es otro tipo de objeto, su representación de cadena se muestra en el botón.

El contenido de un botón suele ser texto. Estas son las recomendaciones de diseño para los botones con contenido de texto:
-   Usa un texto conciso, específico y descriptivo que explique claramente la acción que realiza el botón. Por lo general, el contenido del texto del botón es una única palabra, un verbo.
-   Use la fuente predeterminada, a menos que las directrices de marca le indiquen lo contrario.
-   Para un texto más corto, evita los botones de comando estrechos usando un ancho de botón mínimo de 120 píxeles.
- Para un texto más largo, evita los botones de comando anchos limitando el texto a una longitud máxima de 26 caracteres.
-   Si el contenido del texto del botón es dinámico, (por ejemplo, [localizado](../globalizing/globalizing-portal.md)), decide cómo cambiará de tamaño el botón y qué ocurrirá con los controles que lo rodean.

<table>
<tr>
<td> <b>Tienes que arreglar:</b><br> Botones con texto que se desborda. </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>Opción 1:</b><br> Aumenta el ancho de botón, los botones de la pila y el ajuste si la longitud del texto es mayor de 26 caracteres. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>Opción 2:</b><br> Aumenta la altura del botón y ajusta el texto. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

También puedes personalizar los elementos visuales que conforman la apariencia del botón. Por ejemplo, podrías reemplazar el texto con un icono o usar un icono con texto.

Aquí se ha establecido como contenido de un botón un elemento **StackPanel** que contiene una imagen y texto.

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

El botón tendrá el siguiente aspecto:

![Botón con contenido de texto e imagen](images/button-orange.png)

## <a name="create-a-repeat-button"></a>Crear un botón Repetir

Un [RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) es un botón que genera eventos [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) repetidamente desde que se presiona hasta que se suelta. Establece la propiedad [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx) para especificar el tiempo que RepeatButton espera después de presionarse antes de que empiece a repetir la acción de clic. Establece la propiedad [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx) para especificar el tiempo entre las repeticiones de la acción de clic. Los tiempos de ambas propiedades se especifican en milisegundos.

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

## <a name="recommendations"></a>Recomendaciones
-   Asegúrate de que el objetivo y el estado del botón sean claros para el usuario.
-   Cuando hay varios botones para la misma decisión (como en un cuadro de diálogo de confirmación), presenta los botones de confirmación en este orden, donde [Hacerlo] y [No hacerlo] son respuestas específicas a la instrucción principal:
    -   Aceptar/[Hacerlo]/Sí
    -   [No hacerlo]/No
    -   Cancelar
-   Expón solo uno de los dos botones al usuario a la vez. Por ejemplo, Aceptar y Cancelar. Si necesitas exponer más acciones al usuario, considera la posibilidad de usar [casillas](checkbox.md) o [botones de radio](radio-button.md) con los que el usuario pueda seleccionar acciones, con un único botón de comando para desencadenar estas acciones.
-   En el caso de las acciones que deban estar disponibles en varias páginas de la aplicación, en vez de duplicar el mismo botón en varias páginas, piensa en utilizar una [barra de la aplicación inferior](app-bars.md).

### <a name="recommended-single-button-layout"></a>Diseño de botón único recomendado

Si el diseño requiere un único botón, debe estar alineado a la izquierda o a la derecha en función de su contexto de contenedor.

-   Los cuadros de diálogo con un único botón deben **alinear a la derecha** el botón. Si el cuadro de diálogo contiene solo un botón, asegúrate de que el botón realizar la acción segura y no destructiva. Si usas [ContentDialog](dialogs.md) y especificas un único botón, se alineará automáticamente a la derecha.

![Botón dentro de un cuadro de diálogo](images/pushbutton_doc_dialog.png)

-   Si el botón aparece dentro de una interfaz de usuario de contenedor (por ejemplo, dentro de una notificación del sistema, un control flotante o un elemento de vista de lista), debes **alinear a la derecha** el botón dentro del contenedor.

![Botón dentro de un contenedor](images/pushbutton_doc_container.png)

-   En las páginas que contengan un solo botón (por ejemplo, un botón "Aplicar" en la parte inferior de una página de configuración), debes **alinear a la izquierda** el botón. Así se garantiza que el botón se alinea con el resto del contenido de la página.

![Botón de una página](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>Botones Atrás

El botón Atrás es un elemento de la interfaz de usuario proporcionado por el sistema que permite la navegación hacia atrás a través de la pila de retroceso o el historial de navegación del usuario. No es necesario que crees tu propio botón Atrás, pero es posible que debas realizar algunas acciones para permitir que la experiencia de navegación hacia atrás resulte adecuada. Para obtener más información, consulta [Historial y navegación hacia atrás](../basics/navigation-history-and-backwards-navigation.md)

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.


## <a name="related-articles"></a>Artículos relacionados
- [Clase Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [Botones de radio](radio-button.md)
- [Casillas](checkbox.md)
- [Modificadores para alternar](toggles.md)
