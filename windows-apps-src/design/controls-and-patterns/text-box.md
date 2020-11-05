---
ms.assetid: CC1BF51D-3DAC-4198-ADCB-1770B901C2FC
description: El control TextBox permite a un usuario escribir texto en una aplicación.
title: Cuadro de texto
label: Text box
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 542822b27f356c9471ec8a6c6f5bec0aac2144ce
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034758"
---
# <a name="text-box"></a>Cuadro de texto

El control TextBox permite a un usuario escribir texto en una aplicación. Se usa normalmente para capturar una sola línea de texto, pero se puede configurar para capturar varias. El texto se muestra en la pantalla en un formato simple, uniforme y no cifrado.

El control TextBox tiene varias características que pueden simplificar la entrada de texto. Incluye un menú contextual integrado y familiar que permite copiar y pegar texto. El botón "Borrar todo" permite al usuario eliminar rápidamente todo el texto que se ha escrito. También cuenta con funcionalidades de revisión ortográfica integradas y habilitadas de manera predeterminada.

**Obtención de la biblioteca de la interfaz de usuario de Windows**

:::row:::
   :::column:::
      ![Logotipo de WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      La biblioteca de interfaz de usuario de Windows 2.2 o posterior incluye una nueva plantilla para este control que usa esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](../style/rounded-corner.md). WinUI es un paquete NuGet que contiene nuevas características de interfaz de usuario y controles para aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de plataforma** : [Clase de cuadro de texto](/uwp/api/Windows.UI.Xaml.Controls.TextBox), [Propiedad Text](/uwp/api/windows.ui.xaml.controls.textbox.text)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un control **TextBox** para permitir al usuario escribir y editar texto sin formato, como en un formulario. Puedes usar la propiedad [Text](/uwp/api/windows.ui.xaml.controls.textbox.text) para obtener el texto y establecerlo en un control TextBox.

Puedes hacer que un control TextBox sea de solo lectura, pero este debe ser un estado temporal y condicional. Si el texto nunca se puede editar, piensa en la posibilidad de usar un [TextBlock](text-block.md) en su lugar.

Usa un control [PasswordBox](password-box.md) para recopilar la contraseña u otros datos privados, como el número de la Seguridad Social. Un cuadro de contraseña parece un cuadro de entrada de texto, salvo que muestra viñetas en vez del texto que se ha escrito.

Usa un control [AutoSuggestBox](auto-suggest-box.md) para permitir al usuario escribir términos de búsqueda o para mostrar al usuario una lista de sugerencias entre las elegir a medida que escribe.

Usa [RichEditBox](rich-edit-box.md) para mostrar y editar archivos de texto enriquecido.

Para obtener más información sobre cómo elegir el control de texto correcto, consulta el artículo [Controles de texto](text-controls.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/TextBox">abrirla y ver TextBox en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

![Cuadro de texto](images/text-box.png)

## <a name="create-a-text-box"></a>Creación de un cuadro de texto

Este es el código XAML para un cuadro de texto sencillo con un encabezado y texto de marcador de posición.

```xaml
<TextBox Width="500" Header="Notes" PlaceholderText="Type your notes here"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Width = 500;
textBox.Header = "Notes";
textBox.PlaceholderText = "Type your notes here";
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

Este es el cuadro de texto que se obtiene de este código XAML.

![Cuadro de texto simple](images/text-box-ex1.png)

### <a name="use-a-text-box-for-data-input-in-a-form"></a>Uso de un cuadro de texto para la entrada de datos en un formulario

Es habitual usar un cuadro de texto para aceptar la entrada de datos en un formulario y utilizar la propiedad [Text](/uwp/api/windows.ui.xaml.controls.textbox.text) para obtener la cadena de texto completa del cuadro de texto. Normalmente usas un evento como un clic del botón Enviar para acceder a la propiedad Text, pero puedes controlar los eventos [TextChanged](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) o [TextChanging](/uwp/api/windows.ui.xaml.controls.textbox.textchanging) si necesitas hacer algo cuando cambia el texto.

En este ejemplo se muestra cómo obtener y establecer el contenido actual de un cuadro de texto.

```xaml
<TextBox name="SampleTextBox" Text="Sample Text"/>
```

```csharp
string sampleText = SampleTextBox.Text;
...
SampleTextBox.Text = "Sample text retrieved";
```

Puedes agregar un objeto [Header](/uwp/api/windows.ui.xaml.controls.textbox.header) (o etiqueta) y [PlaceholderText](/uwp/api/windows.ui.xaml.controls.textbox.placeholdertext) (o marca de agua) al cuadro de texto para proporcionar al usuario una indicación de la finalidad del cuadro de texto. Para personalizar el aspecto del encabezado, puedes establecer la propiedad [HeaderTemplate](/uwp/api/windows.ui.xaml.controls.textbox.headertemplate) en lugar de Header. *Para obtener información de diseño, consulta Directrices para etiquetas*.

Para restringir el número de caracteres que el usuario puede escribir, puedes establecer la propiedad [MaxLength](/uwp/api/windows.ui.xaml.controls.textbox.maxlength). Sin embargo, MaxLength no restringe la longitud del texto pegado. Usa el evento [Paste](/uwp/api/windows.ui.xaml.controls.textbox.paste) para modificar el texto pegado si es importante para la aplicación.

El cuadro de texto incluye un botón Borrar todo ("X") que aparece cuando se escribe texto en el cuadro. Cuando un usuario hace clic en la "X", se borra el texto del cuadro de texto. Tiene esta apariencia.

![Cuadro de texto con un botón Borrar todo](images/text-box-clear-all.png)

El botón Borrar todo solo se muestra para cuadros de texto editables, de una única línea que contienen texto y tienen el foco.

El botón Borrar todo no se muestra si se da alguno de estos casos:

- **IsReadOnly** es **true**.
- **AcceptsReturn** es **true**.
- **TextWrap** tiene un valor distinto de **NoWrap**.

En este ejemplo se muestra cómo obtener y establecer el contenido actual de un cuadro de texto.

```xaml
<TextBox name="SampleTextBox" Text="Sample Text"/>
```

```csharp
string sampleText = SampleTextBox.Text;
...
SampleTextBox.Text = "Sample text retrieved";
```


### <a name="make-a-text-box-read-only"></a>Conversión de un cuadro de texto en de solo lectura

Puedes hacer que un cuadro de texto sea de solo lectura si estableces la propiedad [IsReadOnly](/uwp/api/windows.ui.xaml.controls.textbox.isreadonly) en **true**. Normalmente se cambia esta propiedad en el código de la aplicación según las condiciones de la aplicación. Si necesitas texto que sea siempre de solo lectura, piensa en usar un control TextBlock en su lugar.

Puedes hacer que un control TextBox sea de solo lectura si estableces la propiedad IsReadOnly en true. Por ejemplo, puedes tener un TextBox para que un usuario escriba comentarios que se habilitarán únicamente en determinadas condiciones. Puedes convertir el control TextBox en de solo lectura hasta que se cumplan las condiciones. Si solo necesitas mostrar texto, piensa en la posibilidad de usar un control TextBlock o RichTextBlock en su lugar.

Un cuadro de texto de solo lectura tiene el mismo aspecto que un cuadro de texto de lectura y escritura, por lo que podría resultar confuso para el usuario.
Un usuario puede seleccionar y copiar texto.
IsEnabled

### <a name="enable-multi-line-input"></a>Habilitación de la entrada multilínea

Hay dos propiedades que puedes usar para controlar si el cuadro de texto muestra texto en más de una línea. Normalmente, se establecen ambas propiedades para crear un cuadro de texto multilínea.

- Para indicar que el cuadro de texto puede mostrar los caracteres de nueva línea o retorno, establece la propiedad [AcceptsReturn](/uwp/api/windows.ui.xaml.controls.textbox.acceptsreturn) en **true**.
- Para habilitar el ajuste de texto, establece la propiedad [TextWrapping](/uwp/api/windows.ui.xaml.controls.textbox.textwrapping) en **Wrap**. Esto hace que el texto se ajuste cuando llega al borde del cuadro de texto, independientemente de los caracteres del separador de línea.

> **Nota**&nbsp;&nbsp;TextBox y RichEditBox no admiten el valor **WrapWholeWords** para sus propiedades TextWrapping. Si intentas usar WrapWholeWords como valor para TextBox.TextWrapping o RichEditBox.TextWrapping, se genera una excepción de argumento no válida.

Un cuadro de texto multilínea continuará creciendo verticalmente a medida que se escriba texto, a menos que se restrinja por medio de su propiedad [Height](/uwp/api/windows.ui.xaml.frameworkelement.height) o [MaxHeight](/uwp/api/windows.ui.xaml.frameworkelement.maxheight) o con un contenedor principal. Debes probar que un cuadro de texto multilínea no crece más allá de su área visible y limitar su crecimiento si lo hace. Te recomendamos que siempre especifiques una altura adecuada para un cuadro de texto multilínea y que no dejes que crezca en altura a medida que el usuario escribe.

El desplazamiento con una rueda de desplazamiento o mediante la entrada táctil se habilita automáticamente cuando es necesario. Sin embargo, las barras de desplazamiento verticales no son visibles de manera predeterminada. Para mostrar las barras de desplazamiento vertical, establece la propiedad [ScrollViewer.VerticalScrollBarVisibility](/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility) en **Auto** en el objeto ScrollViewer insertado, como se muestra aquí.

```xaml
<TextBox AcceptsReturn="True" TextWrapping="Wrap"
         MaxHeight="172" Width="300" Header="Description"
         ScrollViewer.VerticalScrollBarVisibility="Auto"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.AcceptsReturn = true;
textBox.TextWrapping = TextWrapping.Wrap;
textBox.MaxHeight = 172;
textBox.Width = 300;
textBox.Header = "Description";
ScrollViewer.SetVerticalScrollBarVisibility(textBox, ScrollBarVisibility.Auto);
```

Este es el aspecto del cuadro de texto después de agregar texto.

![Cuadro de texto multilínea](images/text-box-multi-line.png)

### <a name="format-the-text-display"></a>Formato de la visualización del texto

Usa la propiedad [TextAlignment](/uwp/api/windows.ui.xaml.controls.textbox.textalignment) para alinear el texto dentro de un cuadro de texto. Para alinear el cuadro de texto en el diseño de la página, usa las propiedades [HorizontalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) y [VerticalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment).

Aunque el cuadro texto solo admite texto sin formato, puedes personalizar la manera en que se muestra el texto en el cuadro de texto para que coincida con tu personalización de marca. Puedes establecer las propiedades de [Control](/uwp/api/Windows.UI.Xaml.Controls.Control), como [FontFamily](/uwp/api/windows.ui.xaml.controls.control.fontfamily), [FontSize](/uwp/api/windows.ui.xaml.controls.control.fontsize), [FontStyle](/uwp/api/windows.ui.xaml.controls.control.fontstyle), [Background](/uwp/api/windows.ui.xaml.controls.control.background), [Foreground](/uwp/api/windows.ui.xaml.controls.control.foreground) y [CharacterSpacing](/uwp/api/windows.ui.xaml.controls.control.characterspacing) para cambiar el aspecto del texto. Estas propiedades solo afectan al modo en que el cuadro de texto muestra el texto de forma local, de modo que no se aplica ningún formato si, por ejemplo, copias y pegas el texto en un control de texto enriquecido.

Este ejemplo muestra un cuadro de texto de solo lectura con varias propiedades establecidas para personalizar el aspecto del texto.

```xaml
<TextBox Text="Sample Text" IsReadOnly="True"
         FontFamily="Verdana" FontSize="24"
         FontWeight="Bold" FontStyle="Italic"
         CharacterSpacing="200" Width="300"
         Foreground="Blue" Background="Beige"/>
```

```csharp
TextBox textBox = new TextBox();
textBox.Text = "Sample Text";
textBox.IsReadOnly = true;
textBox.FontFamily = new FontFamily("Verdana");
textBox.FontSize = 24;
textBox.FontWeight = Windows.UI.Text.FontWeights.Bold;
textBox.FontStyle = Windows.UI.Text.FontStyle.Italic;
textBox.CharacterSpacing = 200;
textBox.Width = 300;
textBox.Background = new SolidColorBrush(Windows.UI.Colors.Beige);
textBox.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
// Add the TextBox to the visual tree.
rootGrid.Children.Add(textBox);
```

El cuadro de texto resultante tiene este aspecto.

![Cuadro de texto con formato](images/text-box-formatted.png)

### <a name="modify-the-context-menu"></a>Modificación del menú contextual

De manera predeterminada, los comandos que se muestran en el menú contextual del cuadro de texto dependen del estado del cuadro de texto. Por ejemplo, los siguientes comandos pueden mostrarse cuando el cuadro de texto es editable.

Comando | Se muestra cuando...
------- | -------------
Copiar | el texto está seleccionado.
Cortar | el texto está seleccionado.
Pegar | el Portapapeles contiene texto.
Seleccionar todo | el objeto TextBox contiene texto.
Deshacer | se ha cambiado el texto.

Para modificar los comandos que se muestran en el menú contextual, controla el evento [ContextMenuOpening](/uwp/api/windows.ui.xaml.controls.textbox.contextmenuopening). Para ver un ejemplo de esto, consulta **Customizing RichEditBox's CommandBarFlyout - adding 'Share'** (Personalización de la clase CommandBarFlyout de RichEditBox: adición de "Share") en la <a href="xamlcontrolsgallery:/item/RichEditBox">Galería de controles XAML</a>. Para obtener la información de diseño, consulta las directrices para [menús contextuales](menus.md).

### <a name="select-copy-and-paste"></a>Seleccionar, copiar y pegar

Puedes obtener o establecer el texto seleccionado en un cuadro de texto mediante la propiedad [SelectedText](/uwp/api/windows.ui.xaml.controls.textbox.selectedtext). Usa las propiedades [SelectionStart](/uwp/api/windows.ui.xaml.controls.textbox.selectionstart) y [SelectionLength](/uwp/api/windows.ui.xaml.controls.textbox.selectionlength) y los métodos [Select](/uwp/api/windows.ui.xaml.controls.textbox.select) y [SelectAll](/uwp/api/windows.ui.xaml.controls.textbox.selectall) para manipular la selección de texto. Controla el evento [SelectionChanged](/uwp/api/windows.ui.xaml.controls.textbox.selectionchanged) para hacer algo cuando el usuario seleccione el texto o anule la selección. Para cambiar el color usado para resaltar el texto seleccionado, puedes establecer la propiedad [SelectionHighlightColor](/uwp/api/windows.ui.xaml.controls.textbox.selectionhighlightcolor).

El control TextBox admite las operaciones de copiar y pegar de manera predeterminada. Puedes proporcionar un control personalizado del evento [Paste](/uwp/api/windows.ui.xaml.controls.textbox.paste) en los controles de texto editables de tu aplicación. Por ejemplo, podrías quitar los saltos de línea de una dirección multilínea al pegarla en un cuadro de búsqueda de una línea. O bien, puedes comprobar la longitud del texto pegado y advertir al usuario si supera la longitud máxima que se puede guardar en una base de datos. Para obtener más información y ejemplos, consulta el evento [Paste](/uwp/api/windows.ui.xaml.controls.textbox.paste).

Este es un ejemplo del uso de estas propiedades y métodos. Cuando selecciones texto en el primer cuadro de texto, el texto seleccionado se mostrará en el segundo cuadro de texto, que es de solo lectura. Los valores de las propiedades [SelectionLength](/uwp/api/windows.ui.xaml.controls.textbox.selectionlength) y [SelectionStart](/uwp/api/windows.ui.xaml.controls.textbox.selectionstart) se muestran en dos bloques de texto. Esto se consigue mediante el evento [SelectionChanged](/uwp/api/windows.ui.xaml.controls.textbox.selectionchanged).

```xaml
<StackPanel>
   <TextBox x:Name="textBox1" Height="75" Width="300" Margin="10"
         Text="The text that is selected in this TextBox will show up in the read only TextBox below."
         TextWrapping="Wrap" AcceptsReturn="True"
         SelectionChanged="TextBox1_SelectionChanged" />
   <TextBox x:Name="textBox2" Height="75" Width="300" Margin="5"
         TextWrapping="Wrap" AcceptsReturn="True" IsReadOnly="True"/>
   <TextBlock x:Name="label1" HorizontalAlignment="Center"/>
   <TextBlock x:Name="label2" HorizontalAlignment="Center"/>
</StackPanel>
```

```csharp
private void TextBox1_SelectionChanged(object sender, RoutedEventArgs e)
{
    textBox2.Text = textBox1.SelectedText;
    label1.Text = "Selection length is " + textBox1.SelectionLength.ToString();
    label2.Text = "Selection starts at " + textBox1.SelectionStart.ToString();
}
```

Este es el resultado del código.

![Texto seleccionado en un cuadro de texto](images/text-box-selection.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>Elección del teclado adecuado para el control de texto

Para ayudar a los usuarios a escribir datos con el teclado táctil o con el panel de entrada de software (SIP), puedes establecer el ámbito de entrada del control de texto para que coincida con el tipo de datos que se espera que escriba el usuario.

El teclado táctil se puede usar para escribir texto cuando la aplicación se ejecuta en un dispositivo con pantalla táctil. El teclado táctil se invoca cuando el usuario pulsa un campo de entrada editable, como TextBox o RichEditBox. Para que los usuarios puedan escribir datos en la aplicación de forma mucho más rápida y sencilla, establece el ámbito de entrada del control de texto para que coincida con el tipo de datos que esperas que escriba el usuario. El ámbito de entrada proporciona una sugerencia al sistema sobre el tipo de entrada de texto que espera el control para que el sistema pueda proporcionar un diseño de táctil especializado para el tipo de entrada.

Por ejemplo, si un cuadro de texto se usa únicamente para escribir un PIN de 4 dígitos, establece la propiedad [InputScope](/uwp/api/windows.ui.xaml.controls.textbox.inputscope) en **Number**. Esto indica al sistema que debe mostrar el diseño de teclado numérico, lo cual facilita al usuario la entrada del PIN.

> **Importante**&nbsp;&nbsp;El ámbito de entrada no implica que se realice ninguna validación de entrada ni impide que el usuario realice cualquier entrada a través de un teclado de hardware u otro dispositivo de entrada. Sigues siendo responsable de la validación de la entrada en el código, según sea necesario.

Otras propiedades que afectan el teclado táctil son [IsSpellCheckEnabled](/uwp/api/windows.ui.xaml.controls.textbox.isspellcheckenabled), [IsTextPredictionEnabled](/uwp/api/windows.ui.xaml.controls.textbox.istextpredictionenabled) y [PreventKeyboardDisplayOnProgrammaticFocus](/uwp/api/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus). (IsSpellCheckEnabled también afecta al control TextBox cuando se usa un teclado de hardware).

Para obtener más información y ejemplos, consulta [Usar el ámbito de entrada para cambiar el teclado táctil](../input/use-input-scope-to-change-the-touch-keyboard.md) y la documentación de la propiedad.

## <a name="recommendations"></a>Recomendaciones

- Usa una etiqueta o un texto de marcador de posición si el propósito del cuadro de texto no está claro. Una etiqueta es visible, tenga o no un valor el cuadro de entrada de texto. El texto de marcador de posición se muestra dentro del cuadro de entrada y desaparece una vez que se ha escrito un valor.
- Dota al cuadro de texto de un ancho apropiado para el intervalo de valores que se pueden escribir. La longitud de las palabras varía según el lenguaje, así que tenlo en cuenta si quieres que tu aplicación sea internacional.
- Un cuadro de entrada de texto suele ser de línea única (`TextWrap = "NoWrap"`). Si los usuarios tienen que escribir o editar una cadena larga, debes establecer un cuadro de entrada de texto multilínea (`TextWrap = "Wrap"`).
- Por lo general, un cuadro de entrada de texto se usa para texto editable. Sin embargo, puedes hacer que sea de solo lectura, de modo que sea posible leer, seleccionar y copiar su contenido, pero no modificarlo.
- Si quieres reducir el desorden de una vista, considera la posibilidad de que se muestre un conjunto de cuadros de entrada de texto solo si se marca una casilla de control. También puedes enlazar el estado habilitado de un cuadro de entrada de texto a un control, por ejemplo, de tipo casilla.
- Piensa cómo quieres que se comporte un cuadro de entrada de texto cuando contenga un valor y el usuario lo pulse. El comportamiento predeterminado es adecuado para editar el valor en lugar de reemplazarlo; el punto de inserción se coloca entre las palabras y no se selecciona nada. Si el reemplazo es el caso de uso más común para un cuadro de entrada de texto determinado, puedes seleccionar todo el texto del campo siempre que el control reciba el foco; al escribir, se reemplaza el texto seleccionado.

### <a name="single-line-input-boxes"></a>Cuadros de entrada de texto de una línea

- Usa varios cuadros de texto de una línea para capturar muchos fragmentos pequeños de información de texto. Si los cuadros de texto están relacionados, agrúpalos.

- Crea cuadros de texto de una línea ligeramente más anchos que la entrada más larga prevista. Si al hacerlo el control queda demasiado ancho, sepáralo en dos controles. Por ejemplo, puedes dividir una única entrada de dirección en "Línea de dirección 1" y "Línea de dirección 2".
- Establece la longitud máxima de caracteres que se pueden escribir. Si el origen de datos de respaldo no permite una cadena de entrada larga, limita la entrada y usa una ventana emergente de validación para avisar a los usuarios cuando alcancen el límite.
- Usa controles de entrada de texto de una línea para recopilar pequeños fragmentos de texto de los usuarios.

    El siguiente ejemplo muestra un cuadro de texto de una línea para capturar una respuesta a una pregunta de seguridad. Se prevé una respuesta corta, por lo que un cuadro de texto de una línea es adecuado en este caso.

    ![Entrada de datos básicos](images/guidelines_and_checklist_for_singleline_text_input_type_text.png)

- Usa un conjunto de controles de entrada de texto cortos, de tamaño fijo y una sola línea para escribir datos con un formato específico.

    ![Entrada de datos con formato](images/textinput-example-productkey.png)

- Usa un control de entrada de texto de una línea sin restricciones para escribir o editar cadenas, en combinación con un botón de comando que ayude a los usuarios a seleccionar valores válidos.

    ![Entrada de datos asistida](images/textinput-example-assisted.png)

### <a name="multi-line-text-input-controls"></a>Controles de entrada de texto multilínea

- Cuando crees un cuadro de texto enriquecido, proporciona botones de aplicación de estilos e implementa sus acciones.
- Usa una fuente coherente con el estilo de la aplicación.
- Ajusta la altura del control de texto para que admita entradas típicas.
- Si tienes que capturar textos largos con un número máximo de caracteres o palabras, usa un cuadro de texto sin formato e incluye un contador con ejecución dinámica que muestre al usuario cuántos caracteres o palabras le quedan antes de alcanzar el límite. Deberás crear el contador tú mismo; colócalo debajo del cuadro de texto y actualízalo de forma dinámica a medida que el usuario escriba cada carácter o palabra.

    ![Texto largo](images/guidelines_and_checklist_for_multiline_text_input_text_limits.png)

- No permitas que los controles de entrada de texto crezcan en altura a medida que los usuarios escriben.
- No uses un cuadro de texto multilínea si los usuarios solo necesitan una única línea.
- No uses un control de texto enriquecido si un control de texto sin formato es suficiente.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Controles de texto](text-controls.md)
- [Directrices sobre revisión ortográfica](text-controls.md)
- [Adición de búsqueda](/previous-versions/windows/apps/hh465231(v=win.10))
- [Directrices sobre la entrada de texto](text-controls.md)
- [Clase TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Clase PasswordBox](/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [Propiedad String.Length](/dotnet/api/system.string.length)
