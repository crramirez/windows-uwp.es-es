---
ms.assetid: CC1BF51D-3DAC-4198-ADCB-1770B901C2FC
Description: The TextBox control lets a user enter text into an app.
title: Cuadro de texto
label: Text box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a40449b66d2187aacb60501240a0c38de2dc3abc
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8898610"
---
# <a name="text-box"></a>Cuadro de texto

 

El control TextBox permite a un usuario escribir texto en una aplicación. Se usa normalmente para capturar una sola línea de texto, pero se puede configurar para capturar varias líneas de texto. El texto se muestra en la pantalla en un formato simple, uniforme y no cifrado.

TextBox tiene varias características que pueden simplificar la entrada de texto. Incluye un menú contextual integrado y familiar con compatibilidad para copiar y pegar texto. El botón "Borrar todo" permite al usuario eliminar rápidamente todo el texto que se ha escrito. También cuenta con funcionalidades de revisión ortográfica integradas y habilitadas de manera predeterminada.

> **API importantes**: [Clase TextBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx), [Propiedad Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un control **TextBox** para permitir al usuario escribir y editar texto sin formato, como en un formulario. Puedes usar la propiedad [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) para obtener el texto y establecerlo en un TextBox.

Puedes hacer un TextBox de solo lectura, pero este debe ser un estado temporal y condicional. Si el texto nunca es editable, piensa en la posibilidad de usar un [TextBlock](text-block.md) en su lugar.

Usa un control [PasswordBox](password-box.md) para recopilar la contraseña u otros datos privados, como el número de la Seguridad social. Un cuadro de contraseña parece un cuadro de entrada de texto, salvo que muestra viñetas, en vez del texto que se ha escrito.

Usa un control [AutoSuggestBox](auto-suggest-box.md) para permitir al usuario escribir términos de búsqueda o para mostrar al usuario una lista de sugerencias entre las elegir a medida que escribe.

Usa un [RichEditBox](rich-edit-box.md) para mostrar y editar archivos de texto enriquecido.

Para obtener más información sobre cómo elegir el control de texto correcto, consulta el artículo [Controles de texto](text-controls.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/TextBox">abrir la aplicación y ver TextBox en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

![Cuadro de texto](images/text-box.png)

## <a name="create-a-text-box"></a>Crear un cuadro de texto

Este es el XAML para un cuadro de texto sencillo con un texto de encabezado y marcador de posición.

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

Este es el cuadro de texto que se obtiene de este XAML.

![Cuadro de texto sencillo](images/text-box-ex1.png)

### <a name="use-a-text-box-for-data-input-in-a-form"></a>Usa un cuadro de texto para la entrada de datos en un formulario

Es habitual usar un cuadro de texto para aceptar la entrada de datos en un formulario y usar la propiedad [Text](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.text.aspx) para obtener la cadena de texto completa del cuadro de texto. Normalmente usas un evento como un clic del botón Enviar para tener acceso a la propiedad Text, pero puedes controlar el evento [TextChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textchanged.aspx) o [TextChanging](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textchanging.aspx) eventos si necesitas hacer algo cuando cambia el texto.

Puedes agregar un [Header](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.header.aspx) (o etiqueta) y [PlaceholderText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.placeholdertext.aspx) (o marca de agua) al cuadro de texto para dar al usuario una indicación de para qué se usa el cuadro de texto. Para personalizar el aspecto del encabezado, puedes establecer la propiedad [HeaderTemplate](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.headertemplate.aspx) en lugar de Header. *Para obtener información acerca del diseño, consulta Directrices para etiquetas*.

Puedes restringir el número de caracteres que el usuario puede escribir estableciendo la propiedad [MaxLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.maxlength.aspx). Sin embargo, MaxLength no restringe la longitud del texto pegado. Usa el evento [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx) para modificar el texto pegado si esto es importante para la aplicación.

El cuadro de texto incluye un botón Borrar todo ("X") que aparece cuando se escribe texto en el cuadro. Cuando un usuario hace clic en la "X", se borra el texto del cuadro de texto. Tiene esta apariencia.

![Cuadro de texto con un botón Borrar todo](images/text-box-clear-all.png)

El botón Borrar todo solo se muestra para cuadros de texto editables, de una única línea que contienen texto y tienen el foco.

El botón Borrar todo no se muestra en ninguno de estos casos:
- **IsReadOnly** es **true**
- **AcceptsReturn** es **true**
- **TextWrap** tiene un valor distinto de **NoWrap**

### <a name="make-a-text-box-read-only"></a>Hacer un cuadro de texto de solo lectura

Puedes hacer que un cuadro de texto sea de solo lectura si estableces la propiedad [IsReadOnly](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.isreadonly.aspx) en **true**. Normalmente se cambia esta propiedad en el código de aplicación según las condiciones de la aplicación. Si necesitas texto que sea siempre de solo lectura, piensa en usar un control TextBlock en su lugar.

Puedes hacer que un TextBox sea de solo lectura si estableces la propiedad IsReadOnly en true. Por ejemplo, podrías tener un TextBox para que un usuario escribiera comentarios que se habilitarán únicamente en determinadas condiciones. Puedes hacer el TextBox de solo lectura hasta que se cumplan las condiciones. Si solo necesitas mostrar texto, piensa en la posibilidad de usar un TextBlock o RichTextBlock en su lugar.

Un cuadro de texto de solo lectura tiene el mismo aspecto que un cuadro de texto de lectura y escritura, por lo que podría resultar confuso para el usuario.
Un usuario puede seleccionar y copiar texto.
IsEnabled


### <a name="enable-multi-line-input"></a>Habilitar la entrada de varias líneas

Hay dos propiedades que puedes usar para controlar si el cuadro de texto muestra texto en más de una línea. Normalmente, se establecen ambas propiedades para crear un cuadro de texto multilínea.
- Para indicar que el cuadro de texto puede permitir y mostrar los caracteres de nueva línea o retorno, establece la propiedad [AcceptsReturn](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.acceptsreturn.aspx) en **true**.
- Para habilitar el ajuste de texto, establece la propiedad [TextWrapping](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.textwrapping.aspx) en **Wrap**. Esto hace que el texto se ajuste cuando llega al borde del cuadro de texto, independientemente de los caracteres del separador de línea.

> **Nota**&nbsp;&nbsp;TextBox y RichEditBox no admiten el valor **WrapWholeWords** de sus propiedades TextWrapping. Si intentas usar WrapWholeWords como valor para TextBox.TextWrapping o RichEditBox.TextWrapping, se genera una excepción de argumento no válida.

Un cuadro de texto multilínea continuará creciendo verticalmente a medida que se escriba texto. a menos que se restrinja por medio de su propiedad [Height](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) o [MaxHeight](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxheight.aspx) o mediante un contenedor primario. Debes probar que un cuadro de texto multilínea no crece más allá de su área visible y limitar su crecimiento si lo hace. Te recomendamos que especifiques siempre una altura adecuada para un cuadro de texto multilínea y que no dejes que crezca en altura a medida que el usuario escribe.

El desplazamiento con una rueda de desplazamiento o mediante entrada táctil se habilita automáticamente cuando sea necesario. Sin embargo, las barras de desplazamiento verticales no son visibles de manera predeterminada. Para mostrar las barras de desplazamiento vertical, establece la propiedad [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibility.aspx) en **Auto** en el ScrollViewer incrustado, como se muestra aquí.

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

Este es el aspecto del cuadro de texto después de agregarse texto.

![Cuadro de texto multilínea](images/text-box-multi-line.png)

### <a name="format-the-text-display"></a>Dar formato a la visualización del texto

Usa la propiedad [TextAlignment]() para alinear el texto dentro de un cuadro de texto. Para alinear el cuadro de texto en el diseño de la página, usa las propiedades [HorizontalAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) y [VerticalAlignment](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx).

Aunque el cuadro texto solo admite texto sin formato, puedes personalizar la manera en que se muestra el texto en el cuadro de texto para que así coincida con tu personalización de marca. Puedes establecer las propiedades [Control](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.aspx), como [FontFamily](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontfamily.aspx), [FontSize](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontsize.aspx), [FontStyle](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.fontstyle.aspx), [Background](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.background.aspx), [Foreground](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.foreground.aspx) y [CharacterSpacing](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.control.characterspacing.aspx) para cambiar el aspecto del texto. Estas propiedades solo afectan al modo en que el cuadro de texto muestra el texto de forma local, por lo que, por ejemplo, si copias y pegas el texto en un control de texto enriquecido no se aplica ningún formato.

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

### <a name="modify-the-context-menu"></a>Modificar el menú contextual

De manera predeterminada, los comandos que se muestran en el menú contextual del cuadro de texto dependen del estado del cuadro de texto. Por ejemplo, los siguientes comandos pueden mostrarse cuando el cuadro de texto es editable.

Comando | Se muestra cuando...
------- | -------------
Copiar | el texto está seleccionado.
Cortar | el texto está seleccionado.
Pegar | el Portapapeles contiene texto.
Seleccionar todos | el TextBox contiene texto.
Deshacer | se ha cambiado el texto.

Para modificar los comandos mostrados en el menú contextual, controle el evento [ContextMenuOpening](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.contextmenuopening.aspx). Para obtener un ejemplo de esto, consulta el escenario 2 de la [muestra de ContextMenu](http://go.microsoft.com/fwlink/p/?linkid=234891). Para obtener la información de diseño, consulta las directrices para menús contextuales.

### <a name="select-copy-and-paste"></a>Seleccionar, copiar y pegar

Puedes obtener o establecer el texto seleccionado en un cuadro de texto mediante la propiedad [SelectedText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectedtext.aspx). Usa las propiedades [SelectionStart](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionstart.aspx) y [SelectionLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionlength.aspx) y los métodos [Select](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.select.aspx) y [SelectAll](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectall.aspx) para manipular la selección de texto. Controla el evento [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionchanged.aspx) para hacer algo cuando el usuario seleccione el texto o anule la selección. Puedes cambiar el color usado para resaltar el texto seleccionado estableciendo la propiedad [SelectionHighlightColor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionhighlightcolor.aspx).

TextBox admite las operaciones de copiar y pegar de manera predeterminada. Puedes proporcionar administración personalizada del evento [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx) en controles de texto editables de tu aplicación. Por ejemplo, podrías quitar los saltos de línea de una dirección de varias línea al pegarla en un cuadro de búsqueda de una línea. O bien, puedes comprobar la longitud del texto pegado y advertir al usuario si supera la longitud máxima que se puede guardar en una base de datos. Para obtener más información y ejemplos, consulta el evento [Paste](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.paste.aspx).

Este es un ejemplo del uso de estas propiedades y métodos. Cuando selecciones texto en el primer cuadro de texto, el texto seleccionado se mostrará en el segundo cuadro de texto, que es de solo lectura. Los valores de las propiedades [SelectionLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionlength.aspx) y [SelectionStart](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionstart.aspx) se muestran en dos bloques de texto. Esto se consigue mediante el evento [SelectionChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.selectionchanged.aspx).

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

## <a name="choose-the-right-keyboard-for-your-text-control"></a>Elegir el teclado adecuado para el control de texto

Para ayudar a que los usuarios escriban datos con el teclado táctil o con el panel de entrada por software (SIP), puedes establecer el ámbito de entrada del control de texto para que coincida con el tipo de datos que se espera que escriba el usuario.

El teclado táctil se puede usar para escribir texto cuando la aplicación se ejecuta en un dispositivo con pantalla táctil. El teclado táctil se invoca cuando el usuario pulsa en un campo de entrada editable, como un TextBox o RichEditBox. Es posible conseguir que los usuarios escriban datos en la aplicación de forma mucho más rápida y sencilla, si estableces el ámbito de entrada del control de texto para que coincida con el tipo de datos que esperas que el usuario escriba. El ámbito de entrada proporciona una sugerencia al sistema sobre el tipo de entrada de texto que espera el control para que el sistema pueda proporcionar una distribución del teclado táctil especializada para el tipo de entrada.

Por ejemplo, si un cuadro de texto se usa únicamente para escribir un PIN de 4 dígitos, establece la propiedad [InputScope](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.inputscope.aspx) en **Number**. Esto indica al sistema que debe mostrar el diseño de teclado numérico, lo cual facilita al usuario la inserción del PIN.

> **Importante**&nbsp;&nbsp;El ámbito de entrada no implica que se realice ninguna validación de entrada y tampoco impide que el usuario proporcione cualquier entrada a través de un teclado de hardware u otro dispositivo de entrada. Sigues siendo responsable de la validación de la entrada en tu código, según sea necesario.

Otras propiedades que afectan el teclado táctil son [IsSpellCheckEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.isspellcheckenabled.aspx), [IsTextPredictionEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.istextpredictionenabled.aspx) y [PreventKeyboardDisplayOnProgrammaticFocus](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.preventkeyboarddisplayonprogrammaticfocus.aspx). (IsSpellCheckEnabled también afecta al TextBox cuando se usa un teclado de hardware.)

Para obtener más información y ejemplos, consulta [Usar el ámbito de entrada para cambiar el teclado táctil](https://msdn.microsoft.com/library/windows/apps/mt280229) y la documentación de la propiedad.

## <a name="recommendations"></a>Recomendaciones

-   Usa una etiqueta o un texto de marcador de posición si el propósito del cuadro de texto no está claro. Una etiqueta es visible, tenga o no un valor el cuadro de entrada de texto. El texto de marcador de posición se muestra dentro del cuadro de entrada y desaparece una vez que se ha escrito un valor.
-   Dota al cuadro de texto de un ancho apropiado para los valores que se pueden escribir. La longitud de las palabras varía según el lenguaje, así que tenlo en cuenta si quieres que tu aplicación sea internacional.
-   Un cuadro de entrada de texto suele ser de línea única (`TextWrap = "NoWrap"`). Si los usuarios tienen que escribir o editar una cadena larga, debes establecer un cuadro de entrada de texto de varias líneas (`TextWrap = "Wrap"`).
-   Por lo general, un cuadro de entrada de texto se usa para texto editable. Sin embargo, puedes hacer que sea de solo lectura, de modo que sea posible leer, seleccionar y copiar su contenido, pero no modificarlo.
-   Si quieres reducir el número de elementos de una vista que se encuentra atestada, podrías hacer que se muestre un conjunto de cuadros de entrada de texto solo cuando se marque una casilla de control. También puedes vincular el estado habilitado de un cuadro de entrada de texto a un control, por ejemplo, de tipo casilla.
-   Piensa en cómo quieres que se comporte un cuadro de entrada de texto cuando contenga un valor y el usuario lo pulse. El comportamiento predeterminado es adecuado para editar el valor en lugar de reemplazarlo; el punto de inserción se coloca entre las palabras y no se selecciona nada. Si reemplazar es el caso más usado para un cuadro de entrada de texto, puedes seleccionar todo el texto del campo siempre que el control reciba el foco; al escribir, se reemplazará el texto seleccionado.

**Cuadros de entrada de texto de una línea**

-   Usa varios cuadros de texto de una línea para capturar muchos fragmentos pequeños de información de texto. Si los cuadros de texto están relacionados, agrúpalos.

-   Haz tus cuadros de texto de una línea ligeramente más grandes que la entrada más larga prevista. Si al hacerlo el control queda demasiado ancho, sepáralo en dos controles. Por ejemplo, puedes dividir una única entrada de dirección en "Línea 1 de dirección" y "Línea 2 de dirección".
-   Establece la longitud máxima de caracteres que es posible escribir. Si el origen de datos de respaldo no permite una cadena de entrada larga, limita la entrada y usa una ventana emergente de validación para avisar al usuario cuando alcance el límite.
-   Usa controles de entrada de texto de una línea para recopilar pequeños fragmentos de texto de los usuarios.

    El siguiente ejemplo muestra un cuadro de texto de una línea para capturar una respuesta a una pregunta de seguridad. La respuesta que se espera es presumiblemente corta y por eso el cuadro de texto de una línea es el más adecuado en este caso.

    ![Entrada de texto básica](images/guidelines_and_checklist_for_singleline_text_input_type_text.png)

-   Usa un conjunto de controles cortos de entrada de texto en una línea de tamaño fijo para escribir datos con un formato específico.

    ![Entrada de texto con formato](images/textinput_example_productkey.png)

-   Usa un control de entrada de texto sin restricciones en una línea para escribir o editar cadenas, en combinación con un botón de comando que ayude a los usuarios a seleccionar valores válidos.

    ![Entrada de texto asistida](images/textinput_example_assisted.png)


**Controles de entrada de texto de varias líneas**

-   Cuando crees un cuadro de texto enriquecido, proporciona botones de estilo e implementa sus acciones.
-   Usa una fuente coherente con el estilo de la aplicación.
-   Ajusta la altura del control de texto para que permita acomodar las entradas típicas.
-   Si tienes que capturar textos largos con un número máximo de caracteres o palabras, usa un cuadro de texto sin formato e incluye un contador con ejecución dinámica que muestre al usuario cuántos caracteres o palabras le quedan antes de alcanzar el límite. Deberás crear el contador tú mismo; colócalo debajo del cuadro de texto y actualízalo dinámicamente a medida que el usuario escriba cada carácter o palabra.

    ![Texto largo](images/guidelines_and_checklist_for_multiline_text_input_text_limits.png)

-   No permitas que los controles de entrada de texto crezcan en altura a medida que los usuarios escriben.
-   No uses un cuadro de texto multilínea si los usuarios solo necesitan una única línea.
-   No uses un control de texto enriquecido si un control de texto sin formato es suficiente.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Controles de texto](text-controls.md)
- [Directrices sobre revisión ortográfica](text-controls.md)
- [Adición de búsqueda](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [Directrices para la entrada de texto](text-controls.md)
- [Clase TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Clase PasswordBox](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propiedad String.Length](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
