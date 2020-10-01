---
Description: Se usa para seleccionar o anular la selección de elementos de acción. Se puede usar para un solo elemento de lista o varios elementos de lista.
title: Casillas
ms.assetid: 6231A806-287D-43EE-BD8D-39D2FF761914
label: Check boxes
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 1a0d935ed43cda098a7e0c451956676227282fad
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217588"
---
# <a name="check-boxes"></a>Casillas

Una casilla se usa para seleccionar o anular la selección de elementos de acción. Puede usarse para un solo elemento o para una lista de varios elementos que un usuario puede elegir. El control tiene tres estados de selección: no seleccionado, seleccionado e indeterminado. Usa el estado indeterminado cuando una colección de opciones secundarias presente los estados seleccionado y no seleccionado.

![Ejemplo de los estados de una casilla](images/templates-checkbox-states-default.png)

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | La biblioteca de interfaz de usuario de Windows 2.2 o posterior incluye una nueva plantilla para este control que usa esquinas redondeadas. Para obtener más información, consulta [Radio de redondeo](../style/rounded-corner.md). WinUI es un paquete NuGet que contiene nuevas características de interfaz de usuario y controles para aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de plataforma:** [clase CheckBox](/uwp/api/Windows.UI.Xaml.Controls.CheckBox), [evento Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked), [propiedad IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa una **casilla** para elegir entre las opciones de tipo sí y no, como en el escenario de inicio de sesión "Recordar mi cuenta" o con los términos del acuerdo de servicio.

![Una casilla usada para una sola selección](images/checkbox1.png)

Para una selección de dos posibles opciones, la principal diferencia entre una **casilla** y un [modificador para alternar](toggles.md) es que la casilla se usa para un estado y el modificador para alternar se usa para una acción. Se puede retrasar la confirmación de una interacción de casilla (como parte del envío de un formulario, por ejemplo), pero se debe confirmar inmediatamente la interacción de un modificador para alternar. Además, las casillas permiten la selección múltiple.

Usa **varias casillas** para escenarios de múltiple elección en los que un usuario selecciona uno o más elementos de un grupo de opciones que no son mutuamente excluyentes.

Crea un grupo de casillas cuando los usuarios puedan seleccionar cualquier combinación de opciones.

![Seleccionar varias opciones con casillas](images/checkbox2.png)

Cuando se pueden agrupar las opciones, puedes usar una casilla indeterminada para representar todo el grupo. Usa el estado indeterminado de la casilla cuando un usuario selecciona algunos, pero no todos los elementos secundarios del grupo.

![Casillas usadas para mostrar una opción mixta](images/checkbox3.png)

Ambos controles **Casilla** y **Botón de radio** permiten que el usuario pueda seleccionar de una lista de opciones. Las casillas permiten que el usuario seleccione una combinación de opciones. En cambio, los botones de radio permiten al usuario seleccionar una única opción de varias opciones que son mutuamente excluyentes. Cuando haya más de una opción pero solo se pueda seleccionar una, usa un botón de radio en vez de una casilla.

## <a name="examples"></a>Ejemplos

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/CheckBox">abrir la aplicación y ver CheckBox en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-checkbox"></a>Crear una casilla

Para asignar una etiqueta a la casilla, establece la propiedad [Content](/uwp/api/windows.ui.xaml.controls.contentcontrol.content). La etiqueta se muestra junto a la casilla.

Este código XAML crea una casilla que se usa para aceptar los términos del servicio antes de poder enviar un formulario. 

```xaml
<CheckBox x:Name="termsOfServiceCheckBox" 
          Content="I agree to the terms of service."/>
```

Esta es la misma casilla creada con código.

```csharp
CheckBox checkBox1 = new CheckBox();
checkBox1.Content = "I agree to the terms of service.";
```

### <a name="bind-to-ischecked"></a>Enlazar a IsChecked

Usa la propiedad [IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked) para determinar si la casilla está activada o desactivada. Puedes enlazar el valor de la propiedad IsChecked en otro valor binario.
Sin embargo, dado que IsChecked es un valor booleano [que acepta valores NULL](/dotnet/api/system.nullable-1), debes usar una conversión o un convertidor de valores para enlazarlo a una propiedad booleana. Depende del tipo de enlace real que estés usando. Encontrarás ejemplos a continuación para cada tipo posible. 

En este ejemplo, la propiedad **IsChecked** de la casilla para aceptar los términos de servicio está enlazada a la propiedad [IsEnabled](/uwp/api/windows.ui.xaml.controls.control.isenabled) de un botón Enviar. El botón Enviar se habilita únicamente si se han aceptado los términos del servicio.

#### <a name="using-xbind"></a>Uso de x:Bind

> Nota&nbsp;&nbsp;Aquí solo mostramos el código pertinente. Para obtener más información sobre el enlace de datos, consulta [Introducción al enlace de datos](../../data-binding/data-binding-quickstart.md). La información específica de {x:Bind} (como la conversión) se detalla [aquí](../../xaml-platform/x-bind-markup-extension.md).

```xaml
<StackPanel Grid.Column="2" Margin="40">
    <CheckBox x:Name="termsOfServiceCheckBox" Content="I agree to the terms of service."/>
    <Button Content="Submit" 
            IsEnabled="{x:Bind (x:Boolean)termsOfServiceCheckBox.IsChecked, Mode=OneWay}"/>
</StackPanel>
```

Si la casilla también puede estar en el estado **indeterminado**, usamos la propiedad [FallbackValue](/uwp/api/windows.ui.xaml.data.binding.fallbackvalue) del enlace para especificar el valor booleano que representa este estado. En este caso, no queremos tener el botón Enviar también habilitado:

```xaml
<Button Content="Submit" 
        IsEnabled="{x:Bind (x:Boolean)termsOfServiceCheckBox.IsChecked, Mode=OneWay, FallbackValue=False}"/>
```

#### <a name="using-xbind-or-binding"></a>Uso de x:Bind o Binding

> Nota&nbsp;&nbsp;Aquí solo se muestra el código pertinente con {x:Bind}. En el ejemplo de {Binding}, se reemplazaría {x:Bind} por {Binding}. Para obtener más información sobre el enlace de datos, los convertidores de valores y las diferencias entre las extensiones de marcado {x:Bind} y {Binding}, consulta [Introducción al enlace de datos](../../data-binding/data-binding-quickstart.md).

```xaml
...
<Page.Resources>
    <local:NullableBooleanToBooleanConverter x:Key="NullableBooleanToBooleanConverter"/>
</Page.Resources>

...

<StackPanel Grid.Column="2" Margin="40">
    <CheckBox x:Name="termsOfServiceCheckBox" Content="I agree to the terms of service."/>
    <Button Content="Submit" 
            IsEnabled="{x:Bind termsOfServiceCheckBox.IsChecked, 
                        Converter={StaticResource NullableBooleanToBooleanConverter}, Mode=OneWay}"/>
</StackPanel>
```


```csharp
public class NullableBooleanToBooleanConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, string language)
    {
        if (value is bool?)
        {
            return (bool)value;
        }
        return false;
    }

    public object ConvertBack(object value, Type targetType, object parameter, string language)
    {
        if (value is bool)
            return (bool)value;
        return false;
    }
}
```

### <a name="handle-click-and-checked-events"></a>Controlar eventos Click y Checked

Para realizar una acción cuando cambia el estado de la casilla, puedes controlar el evento [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), o bien los eventos [Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked) y [Unchecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked). 

El evento **Click** se produce siempre que cambia el estado de activación. Si controlas el evento Click, usa la propiedad **IsChecked** para determinar el estado de la casilla.

Los eventos **Checked** y **Unchecked** se producen de forma independiente. Si controlas estos eventos, debes controlar ambos para que respondan a los cambios de estado de la casilla.

En los siguientes ejemplos, te mostramos cómo controlar el evento Click y los eventos Checked y Unchecked.

Varias casillas pueden compartir el mismo controlador de eventos. Este ejemplo crea cuatro casillas para seleccionar ingredientes de una pizza. Las cuatro casillas comparten el mismo controlador de eventos **Click** para actualizar la lista de ingredientes.

```XAML
<StackPanel Margin="40">
    <TextBlock Text="Pizza Toppings"/>
    <CheckBox Content="Pepperoni" x:Name="pepperoniCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Beef" x:Name="beefCheckbox" 
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Mushrooms" x:Name="mushroomsCheckbox"
              Click="toppingsCheckbox_Click"/>
    <CheckBox Content="Onions" x:Name="onionsCheckbox"
              Click="toppingsCheckbox_Click"/>

    <!-- Display the selected toppings. -->
    <TextBlock Text="Toppings selected:"/>
    <TextBlock x:Name="toppingsList"/>
</StackPanel>
```

Este es el controlador de eventos para el evento Click. Cada vez que se hace clic en una casilla, el controlador examina las casillas para ver las que están seleccionadas y actualizar la lista de los ingredientes seleccionados.

```csharp
private void toppingsCheckbox_Click(object sender, RoutedEventArgs e)
{
    string selectedToppingsText = string.Empty;
    CheckBox[] checkboxes = new CheckBox[] { pepperoniCheckbox, beefCheckbox,
                                             mushroomsCheckbox, onionsCheckbox };
    foreach (CheckBox c in checkboxes)
    {
        if (c.IsChecked == true)
        {
            if (selectedToppingsText.Length > 1)
            {
                selectedToppingsText += ", ";
            }
            selectedToppingsText += c.Content;
        }
    }
    toppingsList.Text = selectedToppingsText;
}
```

### <a name="use-the-indeterminate-state"></a>Usar el estado indeterminado

El control CheckBox hereda del elemento [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton) y puede tener tres estados: 

Estado | Propiedad | Value
------|----------|------
activado | IsChecked | **true** 
sin activar | IsChecked | **false** 
indeterminado | IsChecked | **null** 

Para que la casilla notifique el estado indeterminado, debes establecer la propiedad [IsThreeState](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.isthreestate) como **true**. 

Cuando se pueden agrupar las opciones, puedes usar una casilla indeterminada para representar todo el grupo. Usa el estado indeterminado de la casilla cuando un usuario selecciona algunos, pero no todos los elementos secundarios del grupo.

En el siguiente ejemplo, la casilla "Seleccionar todo" tiene la propiedad IsThreeState definida como **true**. La casilla "Seleccionar todos" está activada si los elementos secundarios están activados, desactivada si todos los elementos secundarios están desactivados e indeterminada en otros casos.

```xaml
<StackPanel>
    <CheckBox x:Name="OptionsAllCheckBox" Content="Select all" IsThreeState="True" 
              Checked="SelectAll_Checked" Unchecked="SelectAll_Unchecked" 
              Indeterminate="SelectAll_Indeterminate"/>
    <CheckBox x:Name="Option1CheckBox" Content="Option 1" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
    <CheckBox x:Name="Option2CheckBox" Content="Option 2" Margin="24,0,0,0" 
              Checked="Option_Checked" Unchecked="Option_Unchecked" IsChecked="True"/>
    <CheckBox x:Name="Option3CheckBox" Content="Option 3" Margin="24,0,0,0"
              Checked="Option_Checked" Unchecked="Option_Unchecked" />
</StackPanel>
```

```csharp
private void Option_Checked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void Option_Unchecked(object sender, RoutedEventArgs e)
{
    SetCheckedState();
}

private void SelectAll_Checked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = true;
}

private void SelectAll_Unchecked(object sender, RoutedEventArgs e)
{
    Option1CheckBox.IsChecked = Option2CheckBox.IsChecked = Option3CheckBox.IsChecked = false;
}

private void SelectAll_Indeterminate(object sender, RoutedEventArgs e)
{
    // If the SelectAll box is checked (all options are selected), 
    // clicking the box will change it to its indeterminate state.
    // Instead, we want to uncheck all the boxes,
    // so we do this programatically. The indeterminate state should
    // only be set programatically, not by the user.

    if (Option1CheckBox.IsChecked == true &&
        Option2CheckBox.IsChecked == true &&
        Option3CheckBox.IsChecked == true)
    {
        // This will cause SelectAll_Unchecked to be executed, so
        // we don't need to uncheck the other boxes here.
        OptionsAllCheckBox.IsChecked = false;
    }
}

private void SetCheckedState()
{
    // Controls are null the first time this is called, so we just 
    // need to perform a null check on any one of the controls.
    if (Option1CheckBox != null)
    {
        if (Option1CheckBox.IsChecked == true &&
            Option2CheckBox.IsChecked == true &&
            Option3CheckBox.IsChecked == true)
        {
            OptionsAllCheckBox.IsChecked = true;
        }
        else if (Option1CheckBox.IsChecked == false &&
            Option2CheckBox.IsChecked == false &&
            Option3CheckBox.IsChecked == false)
        {
            OptionsAllCheckBox.IsChecked = false;
        }
        else
        {
            // Set third state (indeterminate) by setting IsChecked to null.
            OptionsAllCheckBox.IsChecked = null;
        }
    }
}
```

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

-   Asegúrate de que esté claro el objetivo y el estado actual de la casilla.
-   Limita el contenido textual de la casilla a dos líneas como máximo.
-   Redacta la etiqueta de la casilla como una declaración que sea cierta si se marca la casilla y falsa si no se marca.
-   Utiliza la fuente predeterminada salvo que se indique lo contrario en las directrices de tu marca.
-   Si el contenido es dinámico, analiza cómo se reajustará el tamaño del control y qué sucederá con los elementos visuales que lo rodean.
-   Si hay dos o más opciones mutuamente exclusivas entre las que se debe elegir, considera la posibilidad de usar [botones de radio](radio-button.md).
-   No coloques dos grupos de casillas juntos. Usa etiquetas de grupo para separar los grupos.
-   No uses una casilla como control de activado/desactivado ni para ejecutar un comando; para eso es mejor usar un modificador para alternar.
-   No uses una casilla para mostrar otros controles, como un cuadro de diálogo.
-   Usa el estado indeterminado para indicar que una opción está definida para algunos de las opciones secundarias, pero no para todas.
-   Cuando uses el estado indeterminado, usa casillas subordinadas para mostrar las opciones que se han seleccionado y las que no. Diseña la interfaz de usuario de modo que el usuario pueda ver las opciones secundarias.
-   No uses el estado indeterminado para representar un tercer estado. El estado indeterminado se usa para indicar que una opción está definida para algunas de las opciones secundarias, pero no para todas. Por lo tanto, no dejes que los usuarios establezcan directamente un estado indeterminado. Para ver un ejemplo de lo que no debes hacer, esta casilla usa el estado indeterminado para indicar una cantidad media de picante:

    ![Casilla indeterminada](images/checkbox4_spicy.png)

    En su lugar, usa un grupo de botones de radio con tres opciones: Sin picante, Picante y Muy picante.

    ![Grupo de botones de radio con tres opciones: Sin picante, Picante y Muy picante](images/spicyoptions.png)

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Clase CheckBox](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 
- [Botones de radio](radio-button.md)
- [Modificador para alternar](toggles.md)
