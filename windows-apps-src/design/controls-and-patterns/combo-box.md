---
Description: Un cuadro de entrada de texto que proporciona sugerencias como tipos de usuarios.
title: Cuadro combinado y cuadro de lista
label: Combo box and list box
template: detail.hbs
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: ''
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: f484df97c6d29281941c8eed7b91fd0b156fff60
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82968790"
---
# <a name="combo-box-and-list-box"></a>Cuadro combinado y cuadro de lista

Usa un cuadro combinado (también conocido como lista desplegable) para presentar una lista de elementos entre los que un usuario puede seleccionar. Un cuadro combinado se inicia en un estado compacto y se amplía para mostrar una lista de elementos seleccionables. Un cuadro de lista es similar a un cuadro combinado, pero no es contraíble y no tiene un estado compacto. Puedes obtener más información sobre los cuadros de lista al final de este artículo.

Cuando se cierra el cuadro combinado, o bien se muestra la selección actual o bien está vacío si no hay ningún elemento seleccionado. Cuando el usuario amplía el cuadro combinado, este muestra la lista de elementos seleccionables.

![Ejemplo de una lista desplegable en su estado compacto](images/combo_box_collapsed.png)

> _Un cuadro combinado en su estado compacto con un encabezado._

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | La biblioteca de interfaz de usuario de Windows 2.2 o posterior incluye una nueva plantilla para este control que usa esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](/windows/uwp/design/style/rounded-corner). WinUI es un paquete NuGet que contiene nuevas características de interfaz de usuario y controles para aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de plataforma:** [clase de cuadro combinado](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [propiedad IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable), [propiedad Text](/uwp/api/Windows.UI.Xaml.Controls.ComboBox), [evento TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

- Usa una lista desplegable para que los usuarios puedan seleccionar un único valor en un conjunto de elementos que pueden representarse correctamente con una línea de texto.
- Usa una vista de lista o cuadrícula en lugar de un cuadro combinado para mostrar elementos que contengan varias líneas de texto o imágenes.
- Cuando haya menos de cinco elementos, considera el uso de [botones de radio](radio-button.md) (si solo se puede seleccionar un elemento) o [casillas](checkbox.md) (si se pueden seleccionar varios elementos).
- Usa un cuadro combinado cuando los elementos de selección sean de importancia secundaria en el flujo de tu aplicación. Si la opción predeterminada es la recomendada para la mayoría de los usuarios en la mayoría de situaciones, mostrar todos los elementos usando una vista de lista podría atraer más atención de la necesaria sobre las opciones. Usar un cuadro combinado te permite ahorrar espacio y minimizar la distracción.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la app <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/ComboBox">abrir la app y ver ComboBox en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
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

## <a name="create-a-combo-box"></a>Creación de un cuadro combinado

Puedes rellenar el cuadro combinado mediante la adición de objetos directamente a la colección [Items](/uwp/api/windows.ui.xaml.controls.itemscontrol.items) o el enlace de la propiedad [ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) a un origen de datos. Los elementos agregados al elemento ComboBox se ajustan en contenedores [ComboBoxItem](/uwp/api/windows.ui.xaml.controls.comboboxitem).

Este es un cuadro combinado simple con los elementos agregados en XAML.

```xaml
<ComboBox Header="Colors" PlaceholderText="Pick a color" Width="200">
    <x:String>Blue</x:String>
    <x:String>Green</x:String>
    <x:String>Red</x:String>
    <x:String>Yellow</x:String>
</ComboBox>
```

En el ejemplo siguiente se muestra cómo enlazar un cuadro combinado a una colección de objetos FontFamily.

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

Igual que ListView y GridView, ComboBox deriva de [Selector](/uwp/api/windows.ui.xaml.controls.primitives.selector), por lo que puedes obtener la selección del usuario de la misma manera estándar.

Puedes obtener o establecer el elemento seleccionado del cuadro combinado mediante la propiedad [SelectedItem](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) y obtener o establecer el índice del elemento seleccionado mediante el uso de la propiedad [SelectedIndex](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex).

Para obtener el valor de una propiedad determinada en el elemento de datos seleccionado, puedes usar la propiedad [SelectedValue](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvalue). En este caso, establece [SelectedValuePath](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedvaluepath) para especificar la propiedad del elemento seleccionado de la que se obtiene el valor.

> [!TIP]
> Si estableces SelectedItem o SelectedIndex para indicar la selección predeterminada, se produce una excepción si la propiedad se establece antes de que se rellene la colección Items del cuadro combinado. A menos que definas los elementos en XAML, es mejor controlar el evento Loaded del cuadro combinado y establecer SelectedItem o SelectedIndex en el controlador de eventos Loaded.

Puedes enlazar a estas propiedades en XAML o controlar el evento [SelectionChanged](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) para responder a los cambios de selección.

En el código del controlador de eventos, puedes obtener el elemento seleccionado a partir de la propiedad [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems). Puedes obtener el elemento seleccionado anteriormente, si lo hay, a partir de la propiedad [SelectionChangedEventArgs.RemovedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.removeditems). Las colecciones AddedItems y RemovedItems contienen solo 1 elemento porque el cuadro combinado no es compatible con la selección múltiple.

En este ejemplo se muestra cómo controlar el evento SelectionChanged y cómo enlazar al elemento seleccionado.

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

#### <a name="selectionchanged-and-keyboard-navigation"></a>SelectionChanged y navegación de teclado

De manera predeterminada, el evento SelectionChanged se produce cuando un usuario hace clic en Entrar, o lo pulsa o presiona, en un elemento de la lista para confirmar su selección, y se cierra el cuadro combinado. La selección no cambia cuando el usuario navega por la lista abierta del cuadro combinado con las teclas de dirección del teclado.

Para crear un cuadro combinado que se "actualice en directo" mientras el usuario se desplaza por la lista abierta con las teclas de dirección (como un menú desplegable de fuente), establece [SelectionChangedTrigger](/uwp/api/windows.ui.xaml.controls.combobox.selectionchangedtrigger) en [Siempre](/uwp/api/windows.ui.xaml.controls.comboboxselectionchangedtrigger). Esto provoca que el evento SelectionChanged se produzca cuando cambia el foco a otro elemento de la lista abierta.

#### <a name="selected-item-behavior-change"></a>Cambio de comportamiento del elemento seleccionado

En Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o versiones posteriores, el comportamiento de los elementos seleccionados se actualiza para admitir los cuadros combinados editables.

Antes de SDK 17763, el valor de la propiedad SelectedItem (y, por lo tanto, SelectedValue y SelectedIndex) debía estar en la colección Items del cuadro combinado. Con el ejemplo anterior, establece los resultados de `colorComboBox.SelectedItem = "Pink"` en:

- SelectedItem = null
- SelectedItem = null
- SelectedIndex = -1

En SDK 17763 y versiones posteriores, el valor de la propiedad SelectedItem (y, por lo tanto, SelectedValue y SelectedIndex) no hace falta que esté en la colección Items del cuadro combinado. Con el ejemplo anterior, establece los resultados de `colorComboBox.SelectedItem = "Pink"` en:

- SelectedItem = Pink
- SelectedValue = Pink
- SelectedIndex = -1

### <a name="text-search"></a>Búsqueda de texto

Los cuadros combinados admiten automáticamente la búsqueda dentro de sus colecciones. A medida que los usuarios escriben caracteres en un teclado físico mientras se centran en un cuadro combinado abierto o cerrado, los candidatos que coincidan con la cadena del usuario se incluyen en la vista. Esta funcionalidad es especialmente útil cuando se navega en una lista larga. Por ejemplo, cuando se interactúa con un menú desplegable que contiene una lista de estados, los usuarios pueden presionar la tecla "w" para mostrar "Washington" para la selección rápida. En el texto de búsqueda no se distinguen mayúsculas de minúsculas.

Puedes establecer la propiedad [IsTextSearchEnabled](/uwp/api/windows.ui.xaml.controls.combobox.istextsearchenabled) en **false** para deshabilitar esta función.

## <a name="make-a-combo-box-editable"></a>Realización de un cuadro combinado editable

> [!IMPORTANT]
> Esta característica requiere Windows 10, versión 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) o una versión posterior.

De manera predeterminada, un cuadro combinado permite al usuario seleccionar de una lista predefinida de opciones. Sin embargo, hay casos en los que la lista solo contiene un subconjunto de valores válidos y el usuario debe ser capaz de escribir otros valores que no aparezcan. Para ello, puedes hacer que el cuadro combinado sea editable.

Para hacer que un cuadro combinado sea editable, establece la propiedad [IsEditable](/uwp/api/windows.ui.xaml.controls.combobox.iseditable) en **true**. Luego, controla el evento [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) para que funcione con el valor que ha introducido el usuario.

De manera predeterminada, el valor de SelectedItem se actualiza cuando el usuario confirma texto personalizado. Puedes invalidar este comportamiento estableciendo **Handled** en **true** en los argumentos del evento TextSubmitted. Cuando el evento está marcado como controlado, el cuadro combinado no realizará ninguna acción adicional después del evento y permanecerá en el estado de edición. SelectedItem no se actualizará.

En este ejemplo se muestra un cuadro combinado editable simple. La lista contiene cadenas sencillas, y cualquier valor que haya introducido el usuario se usa tal como se ha introducido.

Un selector de "nombres usados recientemente" permite al usuario introducir cadenas personalizadas. La lista "RecentlyUsedNames" contiene algunos valores entre los que el usuario puede elegir, pero también puede agregar un nuevo valor personalizado. La propiedad "CurrentName" representa el nombre introducido actualmente.

```xaml
<ComboBox IsEditable="true"
          ItemsSource="{x:Bind RecentlyUsedNames}"
          SelectedItem="{x:Bind CurrentName, Mode=TwoWay}"/>
```

### <a name="text-submitted"></a>Texto enviado

Puedes controlar el evento [TextSubmitted](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) para que funcione con el valor que ha introducido el usuario. En el controlador del evento, se suele validar que el valor que el usuario introduce es válido. Después, usa el valor en la aplicación. En función de la situación, también podrías agregar el valor a la lista de opciones del cuadro combinado para un uso futuro.

El evento TextSubmitted se produce cuando se cumplen estas condiciones:

- La propiedad IsEditable es **true**.
- El usuario escribe el texto que no coincide con una entrada existente en la lista del cuadro combinado.
- El usuario presiona Entrar o mueve el foco en el cuadro combinado.

No se produce el evento TextSubmitted si el usuario escribe texto y luego navega hacia arriba o hacia abajo por la lista.

### <a name="sample---validate-input-and-use-locally"></a>Ejemplo: validación de la entrada y uso de forma local

En este ejemplo, un selector de tamaño de fuente contiene un conjunto de valores correspondiente a la rampa de tamaño de fuente, pero el usuario puede escribir tamaños de fuente que no estén en la lista.

Cuando el usuario agrega un valor que no está en la lista, el tamaño de fuente se actualiza, pero el valor no se agrega a la lista de tamaños de fuente.

Si el valor recién escrito no es válido, usa el elemento SelectedValue para revertir la propiedad Text al último valor correcto conocido.

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
        // Update the app's font size.
        _fontSize = newValue;
    }
    else
    {
        // If the item is invalid, reject it and revert the text.
        // Mark the event as handled so the framework doesn't update the selected item.
        sender.Text = sender.SelectedValue.ToString();
        e.Handled = true;
    }
}
```

### <a name="sample---validate-input-and-add-to-list"></a>Ejemplo: validación de la entrada y adición a una lista

En este caso, un "selector de color favorito" contiene los colores favoritos más comunes (rojo, azul, verde, naranja), pero el usuario puede escribir un color favorito que no esté en la lista. Cuando el usuario agrega un color válido (por ejemplo, rosa), el color recién introducido se agrega a la lista y se establece como el "color favorito" activo.

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
        // Mark the event as handled so the framework doesn't update the selected item.
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

## <a name="list-boxes"></a>Cuadros de lista

Un cuadro de lista permite al usuario seleccionar uno o varios elementos de una colección. Son similares a las listas desplegables, salvo por que los cuadros de lista estén siempre abiertos y no dispongan de estado compacto (no expandido). Si no hay espacio para mostrar todos los elementos de un cuadro de lista, es posible desplazarse por ellos.

### <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

- Un cuadro de lista puede ser útil cuando los elementos de la lista son lo suficientemente importantes como para mostrarse en un lugar destacado y cuando hay suficiente espacio en la pantalla para mostrar la lista completa.
- Un cuadro de lista debe atraer la atención del usuario hacia el conjunto de alternativas de una elección importante. Por el contrario, una lista desplegable inicialmente capta la atención del usuario hacia el elemento seleccionado.
- Evita usar un cuadro de lista si:
    - Existe un número muy pequeño de elementos de la lista. Si el cuadro de lista siempre tiene las mismas 2 opciones y solo se puede elegir una, es mejor usar [botones de radio](radio-button.md). Considera también la posibilidad de usar los botones de radio cuando hay 3 o 4 elementos estáticos en la lista.
    - El cuadro de lista siempre tiene las mismas 2 opciones de las que solo se puede seleccionar una y, además, una supone la imposibilidad de la otra (por ejemplo, “on” y “off”). Usa una única casilla o un conmutador de alternancia.
    - El número de elementos es muy elevado. Para listas largas, es mejor usar vistas de cuadrícula y de lista. Para listas muy largas de datos agrupados, se recomienda usar zoom semántico.
    - Los elementos son valores numéricos contiguos. Si ese es el caso, considera la posibilidad de usar un [control deslizante](slider.md).
    - Los elementos de la selección tienen una importancia secundaria en el flujo de la aplicación o la opción predeterminada es la recomendada para la mayoría de usuarios y situaciones. Usa una lista desplegable.

### <a name="recommendations"></a>Recomendaciones

- El intervalo ideal de elementos en un cuadro de lista es de 3 a 9.
- El cuadro de lista funciona mejor si sus elementos pueden variar dinámicamente.
- Si puedes, establece el tamaño del cuadro de lista de modo que no haya que desplazarse para ver todos los elementos.
- Comprueba que la finalidad del cuadro de lista sea evidente y que los elementos seleccionados se distingan con claridad.
- Reserva los efectos visuales y las animaciones para la información táctil y para los elementos con estado “seleccionado”.
- El texto de los elementos del cuadro de lista no debe ocupar más de una línea. Si los elementos son visuales, puedes personalizar el tamaño. Si un elemento contiene varias líneas de texto o imágenes, es preferible usar una vista de cuadrícula o de lista.
- Utiliza la fuente predeterminada, salvo que se indique lo contrario en las directrices de tu marca.
- No uses un cuadro de lista para los comandos ni para mostrar u ocultar dinámicamente otros controles.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.
- [Ejemplo de AutoSuggestBox](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlAutoSuggestBox)

## <a name="related-articles"></a>Artículos relacionados

- [Controles de texto](text-controls.md)
- [Revisión ortográfica](text-controls.md)
- [Búsqueda](search.md)
- [Clase TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)
- [Clase Windows.UI.Xaml.Controls PasswordBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PasswordBox)
- [Propiedad String.Length](https://docs.microsoft.com/dotnet/api/system.string.length)
