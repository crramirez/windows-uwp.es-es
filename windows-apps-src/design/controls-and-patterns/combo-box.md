---
author: Jwmsft
Description: A text entry box that provides suggestions as the user types.
title: Cuadro combinado (lista de desplegable)
label: Combo box
template: detail.hbs
ms.author: jimwalk
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 0c34dda3039a9b6a66428266e37f81b41695fbc0
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7422891"
---
# <a name="combo-box"></a>Cuadro combinado

Usa un cuadro combinado (también conocido como una lista desplegable) para presentar una lista de elementos que puede seleccionar un usuario. Un cuadro combinado se inicia en un estado compacto y se expande para mostrar una lista de elementos seleccionables.

Cuando se cierra el cuadro combinado, se muestra la selección actual o está vacío si no hay ningún elemento seleccionado. Cuando el usuario expande el cuadro combinado, muestra la lista de elementos seleccionables.

> **API importantes**: [clase ComboBox](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [IsEditable propiedad](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [propiedad Text](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [evento TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

Un cuadro combinado en su estado compacto con un encabezado.

![Ejemplo de una lista desplegable en su estado compacto](images/combo_box_collapsed.png)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

- Usa una lista desplegable para que los usuarios puedan seleccionar un único valor en un conjunto de elementos que pueden representarse correctamente con una línea de texto.
- Usa una vista de lista o cuadrícula en lugar de un cuadro combinado para mostrar elementos que contengan varias líneas de texto o imágenes.
- Cuando haya menos de cinco elementos, considera el uso de [botones de radio](radio-button.md) (si solo se puede seleccionar un elemento) o [casillas](checkbox.md) (si se pueden seleccionar varios elementos).
- Usa un cuadro combinado cuando los elementos de selección sean de importancia secundaria en el flujo de tu aplicación. Si la opción predeterminada es la recomendada para la mayoría de los usuarios en la mayoría de situaciones, mostrar todos los elementos usando una vista de lista podría atraer más atención de la necesaria sobre las opciones. Usar un cuadro combinado te permite ahorrar espacio y minimizar la distracción.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación de la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> , haz clic aquí para <a href="xamlcontrolsgallery:/item/ComboBox">Abrir la aplicación y ver el cuadro combinado en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Un cuadro combinado en su estado compacto puede mostrar un encabezado.

![Ejemplo de una lista desplegable en su estado compacto](images/combo_box_collapsed.png)

Aunque los cuadros combinados se expanden para admitir mayores longitudes de cadena, evita cadenas que sean demasiado largas y difíciles de leer.

![Ejemplo de una lista desplegable con cadena de texto larga](images/combo_box_listitemstate.png)

Si la colección en un cuadro combinado es lo suficientemente larga, aparecerá una barra de desplazamiento para albergarla. Agrupa los elementos de la lista de forma lógica.

![Ejemplo de una barra de desplazamiento en una lista desplegable](images/combo_box_scroll.png)

## <a name="create-a-combo-box"></a>Crear un cuadro combinado

Puedes rellenar el cuadro combinado agregando objetos directamente a la colección [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) o al enlazar la propiedad [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) a un origen de datos. Los elementos agregados en el cuadro combinado se agrupan en contenedores [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) .

Este es un cuadro combinado simple con elementos que se agregan en XAML.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue<x:String>
    <x:String>Green<x:String>
    <x:String>Red<x:String>
    <x:String>Yellow<x:String>
</ComboBox>
```

El siguiente ejemplo muestra un cuadro combinado de enlace a una colección de objetos FontFamily.

```xaml
<ComboBox x:Name="FontsCombo" Header="Fonts" Height="44" Width="296"
          ItemsSource="{x:Bind fonts}" DisplayMemberPath="Source"/>
```

```csharp
ObservableCollection<FontFamily> fonts = new ObservableCollection<FontFamily>();

public MainPage()
{
    this.InitializeComponent();
    fonts.Add(new FontFamily("Arial"));
    fonts.Add(new FontFamily("Courier New"));
    fonts.Add(new FontFamily("Times New Roman"));
}
```

### <a name="item-selection"></a>Selección de elementos

Como ListView y GridView, ComboBox se deriva de [Selector](/uwp/api/windows.ui.xaml.controls.primitives.selector), por lo que puedes obtener la selección del usuario de la misma manera estándar.

Puedes obtener o establecer el cuadro combinado seleccionado el elemento mediante la propiedad [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) y obtener o establecer el índice del elemento seleccionado mediante la propiedad [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) .

Para obtener el valor de una propiedad determinada del elemento de datos seleccionado, puedes usar la propiedad [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) . En este caso, Establece el [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) para especificar qué propiedad del elemento seleccionado para obtener el valor de.

> [!TIP]
> Si estableces SelectedItem o SelectedIndex para indicar la selección predeterminada, se produce una excepción si se establece la propiedad antes de que se rellena la colección de elementos del cuadro combinado. A menos que defina los elementos en XAML, es mejor controlar el evento Loaded del cuadro combinado y establecer SelectedItem o SelectedIndex en el controlador de eventos cargados.

Puedes enlazar a estas propiedades en XAML o controlar el evento [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) para responder a cambios de selección.

En caso de código del controlador, puedes obtener el elemento seleccionado desde la propiedad [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) . Puedes obtener el elemento seleccionado previamente (si existe) de la propiedad [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) . Las colecciones AddedItems y RemovedItems contienen solo 1 artículo porque cuadro combinado no admite la selección múltiple.

En este ejemplo se muestra cómo controlar el evento SelectionChanged y también cómo enlazar al elemento seleccionado.

```xaml
<StackPanel>
    <ComboBox x:Name="colorComboBox" Width="200"
              Header="Colors" PlaceholderText="Pick a color"
              SelectionChanged="ColorComboBox_SelectionChanged">
        <x:String>Blue</x:String>
        <x:String>Green</x:String>
        <x:String>Red</x:String>
        <x:String>Yellow</x:String>
    </ComboBox>

    <Rectangle x:Name="colorRectangle" Height="30" Width="100"
               Margin="0,8,0,0" HorizontalAlignment="Left"/>

    <TextBlock Text="{x:Bind colorComboBox.SelectedIndex, Mode=OneWay}"/>
    <TextBlock Text="{x:Bind colorComboBox.SelectedItem, Mode=OneWay}"/>
</StackPanel>
```

```csharp
private void ColorComboBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    // Add "using Windows.UI;" for Color and Colors.
    string colorName = e.AddedItems[0].ToString();
    Color color;
    switch (colorName)
    {
        case "Yellow":
            color = Colors.Yellow;
            break;
        case "Green":
            color = Colors.Green;
            break;
        case "Blue":
            color = Colors.Blue;
            break;
        case "Red":
            color = Colors.Red;
            break;
    }
    colorRectangle.Fill = new SolidColorBrush(color);
}
```

#### <a name="selectionchanged-and-keyboard-navigation"></a>Navegación SelectionChanged y teclado

De manera predeterminada, el evento SelectionChanged se produce cuando un usuario hace clic en, pulsa o presiona la tecla ENTRAR en un elemento en la lista para confirmar la selección y se cierra el cuadro combinado. Selección no cambia cuando el usuario navega a la lista del cuadro combinado abierto con las teclas de dirección del teclado.

Para crear un cuadro combinado que "se actualice" mientras el usuario está navegando a la lista Abrir con las teclas de dirección (por ejemplo, una fuente selección desplegable), establece [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) en [siempre](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger). Esto hace que el evento SelectionChanged cuando cambia el foco a otro elemento de la lista abierta.

#### <a name="selected-item-behavior-change"></a>Cambio de comportamiento del elemento seleccionado

En Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o una versión posterior, se actualiza el comportamiento de los elementos seleccionados para admitir los cuadros combinados editable.

Antes de SDK 17763, el valor de la propiedad SelectedItem (y por lo tanto, SelectedValue y SelectedIndex) se necesita para estar en la colección de elementos del cuadro combinado. Con el ejemplo anterior, establecer `colorComboBox.SelectedItem = "Pink"` da como resultado:

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

En el SDK 17763 y versiones posterior, el valor de la propiedad SelectedItem (y por lo tanto, SelectedValue y SelectedIndex) no es necesario para estar en la colección de elementos del cuadro combinado. Con el ejemplo anterior, establecer `colorComboBox.SelectedItem = "Pink"` da como resultado:

- SelectedItem = rosa
- SelectedValue = rosa
- SelectedIndex = -1

### <a name="text-search"></a>Búsqueda de texto

Los cuadros combinados admiten automáticamente la búsqueda dentro de sus colecciones. A medida que los usuarios escriben caracteres en un teclado físico mientras se centran en un cuadro combinado abierto o cerrado, los candidatos que coincidan con la cadena del usuario se incluyen en la vista. Esta funcionalidad es especialmente útil cuando se navega en una lista larga. Por ejemplo, cuando se interactúa con una lista desplegable que contiene una lista de Estados, los usuarios pueden presionar la tecla "w" para mostrar a "Washington" en la vista para la selección rápida. La búsqueda de texto no distingue entre mayúsculas y minúsculas.

Puedes establecer la propiedad [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) en **false** para deshabilitar esta funcionalidad.

## <a name="make-a-combo-box-editable"></a>Hacer que un cuadro combinado editable

> [!IMPORTANT]
> Esta característica requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o posterior.

De manera predeterminada, un cuadro combinado permite al usuario seleccionar de una lista predefinida de opciones. Sin embargo, hay casos donde la lista contiene solo un subconjunto de los valores válidos y el usuario debe poder especificar otros valores que no se mencionan. Para admitir esto, puedes hacer que el cuadro combinado editable.

Para hacer que un cuadro combinado editable, Establece la propiedad [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) en **true**. A continuación, controla el evento de [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) para trabajar con el valor especificado por el usuario.

De manera predeterminada, el valor de SelectedItem se actualiza cuando el usuario confirma texto personalizado. Puedes invalidar este comportamiento estableciendo **Handled** en **true** en los argumentos del evento de TextSubmitted. Cuando el evento se marca como controlados, el cuadro combinado no te llevará realizar ninguna acción adicional después del evento y permanecerá en el estado de la edición. No se actualizarán SelectedItem.

En este ejemplo se muestra un cuadro combinado editable simple. La lista contenga cadenas sencillas y se usa cualquier valor introducido por el usuario que escriba.

Un selector "usados recientemente nombres" permite al usuario escribir cadenas personalizadas. La lista de 'RecentlyUsedNames' contiene algunos valores de que el usuario puede elegir, pero el usuario también puede agregar un nuevo valor personalizado. La propiedad 'CurrentName' representa el nombre introducido actualmente.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>Texto presentado

Puedes controlar el evento [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) para trabajar con el valor especificado por el usuario. En caso de que el controlador, por lo general, se validará que el valor especificado por el usuario es válido, a continuación, usa el valor de la aplicación. Según la situación, también puede agregar el valor a la lista del cuadro combinado de opciones para uso futuro.

El evento TextSubmitted se produce cuando se cumplen estas condiciones:

- La propiedad IsEditable es **true**
- El usuario escribe texto que no coincide con una entrada existente de la lista del cuadro combinado
- El usuario presiona la tecla ENTRAR o mueve el foco en el cuadro combinado.

El evento TextSubmitted no ocurre si el usuario escribe texto y, a continuación, navega hacia arriba o hacia abajo por la lista.

### <a name="sample---validate-input-and-use-locally"></a>Ejemplo: validar la entrada y usar localmente

En este ejemplo, un selector de tamaño de fuente contiene un conjunto de valores correspondientes a la rampa de tamaño de fuente, pero el usuario puede escribir los tamaños de fuentes que no están en la lista.

Cuando el usuario agrega un valor que no está en la lista, las actualizaciones de tamaño de fuente, pero el valor no se agrega a la lista de tamaños de fuente.

Si el valor introducido recientemente no es válido, que puedes usar el valor SelectedValue para volver a la última de la propiedad Text conoce buen valor.

```xaml
<ComboBox x:Name="fontSizeComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfFontSizes}"
          TextSubmitted="FontSizeComboBox_TextSubmitted"/>
```

```csharp
private void FontSizeComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (byte.TryParse(e.Text, out double newValue))
    {
        // Update the app’s font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>Ejemplo: validar la entrada y agregar a la lista

Aquí, un selector de color favorito"" contiene los colores favoritos más comunes (rojo, azul, verde, naranja), pero el usuario puede escribir un color favorito que no está en la lista. Cuando el usuario agrega un color válido (como rosa), el color recién escrito es agregado a la lista y se establece como el activo "color favorito".

```xaml
<ComboBox x:Name="favoriteColorComboBox"
          IsEditable="true"
          ItemsSource="{x:Bind ListOfColors}"
          TextSubmitted="FavoriteColorComboBox_TextSubmitted"/>
```

```csharp
private void FavoriteColorComboBox_TextSubmitted(ComboBox sender, ComboBoxTextSubmittedEventArgs e)
{
    if (IsValid(e.Text))
    {
        FavoriteColor newColor = new FavoriteColor()
        {
            ColorName = e.Text,
            Color = ColorFromStringConverter(e.Text)
        }
        ListOfColors.Add(newColor);
    }
    else
    {
        // If the item is invalid, reject it but do not revert the text.
        // Mark the event as handled so the framework doesn’t update the selected item.
        e.Handled = true;
    }
}

bool IsValid(string Text)
{
    // Validate that the string is: not empty; a color.
}
```

## <a name="dos-and-donts"></a>Qué hacer y qué no hacer

- El texto de los elementos del cuadro combinado no debe ocupar más de una línea.
- Ordena los elementos de un cuadro combinado en el orden más lógico. Agrupa opciones relacionadas y coloca las opciones más comunes en la parte superior. Ubica los nombres en orden alfabético, los números en orden numérico y las fechas en orden cronológico.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.
- [Muestra de AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Artículos relacionados

- [Controles de texto](text-controls.md)
- [Revisión ortográfica](text-controls.md)
- [Buscar](search.md)
- [Clase TextBox](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Clase Windows.UI.Xaml.Controls PasswordBox](https://msdn.microsoft.com/library/windows/apps/br227519)
- [Propiedad String.Length](https://msdn.microsoft.com/library/system.string.length.aspx)
