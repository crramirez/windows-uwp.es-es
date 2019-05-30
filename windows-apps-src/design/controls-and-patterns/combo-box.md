---
Description: Un cuadro de entrada de texto que proporciona sugerencias como tipos de usuarios.
title: Cuadro combinado (lista de desplegable)
label: Combo box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 297b907191dfa07084e5e4ada0e3468733e47090
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363128"
---
# <a name="combo-box"></a>Cuadro combinado

Utilice un cuadro combinado (también conocido como una lista desplegable) para presentar una lista de elementos que puede seleccionar un usuario. Un cuadro combinado se inicia en un estado compact y se expande para mostrar una lista de elementos seleccionables.

Cuando se cierra el cuadro combinado, se muestra la selección actual o está vacío si no hay ningún elemento seleccionado. Cuando el usuario expande el cuadro combinado, muestra la lista de elementos seleccionables.

> **API importantes**: [Clase de cuadro combinado](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [propiedad IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [propiedad Text](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [TextSubmitted eventos](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

Un cuadro combinado en su estado compact con un encabezado.

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
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para <a href="xamlcontrolsgallery:/item/ComboBox">abra la aplicación y ver el cuadro combinado en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
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

Rellenar el cuadro combinado agregando objetos directamente a la [elementos](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) colección o enlazando el [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) propiedad a un origen de datos. Los elementos agregados al ComboBox se encapsulan en [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem) contenedores.

Este es un cuadro combinado simple con los elementos agregados en XAML.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

El ejemplo siguiente se muestra cómo enlazar un cuadro combinado a una colección de objetos FontFamily.

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

Como ListView y GridView, se deriva de ComboBox [Selector](/uwp/api/windows.ui.xaml.controls.primitives.selector), por lo que puede obtener la selección del usuario de la misma manera estándar.

Puede obtener o establecer el elemento seleccionado del cuadro combinado mediante el [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) propiedad y get o set el índice del elemento seleccionado mediante el uso de la [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex) propiedad.

Para obtener el valor de una propiedad determinada en el elemento de datos seleccionado, puede usar el [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue) propiedad. En este caso, establezca el [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) para especificar qué propiedad del elemento seleccionado para obtener el valor de.

> [!TIP]
> Si establece SelectedItem o SelectedIndex para indicar la selección predeterminada, se produce una excepción si la propiedad se establece antes de que se rellena la colección de elementos de cuadro combinado. A menos que defina los elementos en XAML, es mejor controlar el evento Loaded de cuadro combinado y establecer SelectedItem o SelectedIndex en el controlador de eventos de carga.

Puede enlazar a estas propiedades en XAML o controlar la [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) eventos para responder a cambios de selección.

En el evento código del controlador, puede obtener el elemento seleccionado de la [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) propiedad. Puede obtener el elemento seleccionado anteriormente (si existe) de la [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems) propiedad. Las colecciones AddedItems y RemovedItems contienen solo 1 elemento porque el cuadro combinado no es compatible con la selección múltiple.

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

De forma predeterminada, el evento SelectionChanged se produce cuando un usuario hace clic en, pulsa o presiona ENTRAR en un elemento en la lista para confirmar su selección y cierra el cuadro combinado. Selección no cambia cuando el usuario navega a la lista del cuadro combinado abierto con las teclas de flecha del teclado.

Para hacer un combinado cuadro ese "live updates" mientras el usuario desplaza a la apertura de la lista con las teclas de dirección (por ejemplo, una fuente selección desplegable), establezca [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) a [siempre](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger). Esto hace que el evento SelectionChanged se producen cuando cambia el foco a otro elemento de la apertura de la lista.

#### <a name="selected-item-behavior-change"></a>Cambio de comportamiento del elemento seleccionado

En Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o versiones posteriores, el comportamiento de los elementos seleccionados se actualiza para admitir los cuadros combinados editable.

Antes de SDK 17763, el valor de la propiedad SelectedItem (y por lo tanto, SelectedValue y SelectedIndex) debía ser de colección de elementos del cuadro combinado. El ejemplo anterior, establecer `colorComboBox.SelectedItem = "Pink"` da como resultado:

- SelectedItem = null
- SelectedValue = null
- SelectedIndex = -1

En el SDK 17763 y versiones posteriores, el valor de la propiedad SelectedItem (y por lo tanto, SelectedValue y SelectedIndex) no tiene que estar en la colección de elementos del cuadro combinado. El ejemplo anterior, establecer `colorComboBox.SelectedItem = "Pink"` da como resultado:

- SelectedItem = rosa
- SelectedValue = rosa
- SelectedIndex = -1

### <a name="text-search"></a>Búsqueda de texto

Los cuadros combinados admiten automáticamente la búsqueda dentro de sus colecciones. A medida que los usuarios escriben caracteres en un teclado físico mientras se centran en un cuadro combinado abierto o cerrado, los candidatos que coincidan con la cadena del usuario se incluyen en la vista. Esta funcionalidad es especialmente útil cuando se navega en una lista larga. Por ejemplo, al interactuar con una lista desplegable que contiene una lista de Estados, los usuarios la tecla "w" para poner a "Washington" en la vista de selección rápida. La búsqueda de texto no distingue mayúsculas de minúsculas.

Puede establecer el [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) propiedad **false** para deshabilitar esta funcionalidad.

## <a name="make-a-combo-box-editable"></a>Convertir en un cuadro combinado editable

> [!IMPORTANT]
> Esta característica requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o una versión posterior.

De forma predeterminada, un cuadro combinado permite al usuario seleccionar de una lista predefinida de opciones. Sin embargo, hay casos donde la lista contiene solo un subconjunto de los valores válidos y el usuario debería poder escribir otros valores que no aparezcan. Para ello, puede hacer modificable del cuadro combinado.

Para que un cuadro combinado editable, establezca el [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) propiedad **true**. A continuación, controlar el [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) evento para que funcione con el valor especificado por el usuario.

De forma predeterminada, el valor de SelectedItem se actualiza cuando el usuario confirma texto personalizado. Puede invalidar este comportamiento estableciendo **Handled** a **true** en los argumentos del evento TextSubmitted. Cuando el evento está marcado como controlado, el cuadro combinado realizará ninguna acción adicional después del evento y permanecerá en el estado de edición. No se actualizará SelectedItem.

En este ejemplo se muestra un cuadro combinado editable simple. La lista contiene cadenas simples, y cualquier valor escrito por el usuario se usa como se haya escrito.

Un selector de "los nombres usados recientemente" permite al usuario escribir cadenas personalizadas. La lista 'RecentlyUsedNames' contiene algunos valores que el usuario puede elegir, pero el usuario también puede agregar un nuevo valor personalizado. La propiedad 'CurrentName' representa el nombre especificado actualmente.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>Texto enviado

Puede controlar la [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) evento para que funcione con el valor especificado por el usuario. En caso de que el controlador, normalmente se validará que el valor especificado por el usuario es válido, a continuación, utilice el valor de la aplicación. Según la situación, puede agregar también el valor a la lista del cuadro combinado de opciones para un uso futuro.

El evento TextSubmitted se produce cuando se cumplen estas condiciones:

- La propiedad IsEditable es **true**
- El usuario escribe el texto que no coincide con una entrada existente en la lista del cuadro combinado
- El usuario presiona ENTRAR o mueve el foco en el cuadro combinado.

No se produce el evento TextSubmitted si el usuario escribe texto y, a continuación, se desplaza hacia arriba o hacia abajo por la lista.

### <a name="sample---validate-input-and-use-locally"></a>Ejemplo: validar la entrada y usar de forma local

En este ejemplo, un selector de tamaño de fuente contiene un conjunto de valores correspondiente a la vía de tamaño de fuente, pero el usuario puede escribir los tamaños de fuente que no están en la lista.

Cuando el usuario agrega un valor que no está en la lista, las actualizaciones de tamaño de fuente, pero el valor no se agrega a la lista de tamaños de fuente.

Si el valor recién introducido no es válido, que use el elemento SelectedValue para volver a la última de la propiedad Text conoce el valor correcto.

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

### <a name="sample---validate-input-and-add-to-list"></a>Ejemplo: validar la entrada y agregar a lista

En este caso, un "selector de color favorito" contiene los colores favoritos más comunes (rojo, azul, verde, naranja), pero el usuario puede especificar un color favorito que no está en la lista. Cuando el usuario agrega un color válido (por ejemplo, rosa), el color recién introducido se agrega a la lista y establecer como activo "color favorito".

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

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- El texto de los elementos del cuadro combinado no debe ocupar más de una línea.
- Ordena los elementos de un cuadro combinado en el orden más lógico. Agrupa opciones relacionadas y coloca las opciones más comunes en la parte superior. Ubica los nombres en orden alfabético, los números en orden numérico y las fechas en orden cronológico.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery): ve todos los controles XAML en un formato interactivo.
- [Ejemplo de AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Artículos relacionados

- [Controles de texto](text-controls.md)
- [Corrector ortográfico](text-controls.md)
- [Buscar](search.md)
- [Clase de cuadro de texto](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Clase Windows.UI.Xaml.Controls PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [Propiedad String.Length](https://docs.microsoft.com/dotnet/api/system.string.length?redirectedfrom=MSDN#System_String_Length)
