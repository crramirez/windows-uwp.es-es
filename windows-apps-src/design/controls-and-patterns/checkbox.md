---
author: QuinnRadich
Description: Used to select or deselect action items. Can be used for a single list item or for multiple list items.
title: Casillas
ms.assetid: 6231A806-287D-43EE-BD8D-39D2FF761914
label: Check boxes
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a3e6180c23208f02c3f7fb2294eabe2ee080f504
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "3115739"
---
# <a name="check-boxes"></a>Casillas

 

Una casilla se usa para seleccionar o anular la selección de elementos de acción. Puede usarse para un solo elemento o para una lista de varios elementos que un usuario puede elegir. El control tiene tres estados de selección: no seleccionado, seleccionado e indeterminado. Usa el estado indeterminado cuando una colección de opciones secundarias tienen los estados seleccionado y no seleccionado.

> **API importantes**: [Clase CheckBox](https://msdn.microsoft.com/library/windows/apps/br209316), [Evento Checked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx), [Propiedad IsChecked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx)

![Ejemplo de los estados de una casilla](images/templates-checkbox-states-default.png)


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
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/CheckBox">abrir la aplicación y ver CheckBox en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-checkbox"></a>Crear una casilla

Para asignar una etiqueta a la casilla, configura la propiedad [Content](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentcontrol.content.aspx). La etiqueta se muestra junto a la casilla.

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

Usa la propiedad [IsChecked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.ischecked.aspx) para determinar si la casilla está activada o desactivada. Puedes enlazar el valor de la propiedad IsChecked en otro valor binario. Sin embargo, dado que IsChecked es un valor booleano [que acepta valores NULL](https://msdn.microsoft.com/library/windows/apps/b3h38hb0.aspx), debes usar un convertidor de valores para enlazarlo a un valor booleano.

En este ejemplo, la propiedad de la casilla **IsChecked** para aceptar los términos de servicio está enlazada a la propiedad [IsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled.aspx) de un botón Enviar. El botón Enviar se habilita únicamente si se han aceptado los términos del servicio.

> Nota&nbsp;&nbsp;Aquí solo mostramos el código relevante. Para obtener más información sobre los convertidores de valores y de enlaces de datos, consulta el tema [Introducción al enlace de datos](../../data-binding/data-binding-quickstart.md).

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

Para realizar una acción cuando cambia el estado de la casilla, puedes controlar el evento [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx), o bien los eventos [Checked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.checked.aspx) y [Unchecked](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.unchecked.aspx). 

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

El control CheckBox hereda del elemento [ToggleButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.aspx) y puede tener tres estados: 

Estado | Propiedad | Valor
------|----------|------
activado | IsChecked | **true** 
sin activar | IsChecked | **false** 
indeterminado | IsChecked | **nulo** 

Para que la casilla notifique el estado indeterminado, debes establecer la propiedad [IsThreeState](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.togglebutton.isthreestate.aspx) como **true**. 

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

## <a name="dos-and-donts"></a>Lo que se debe y no se debe hacer

-   Asegúrate de que esté claro el objetivo y el estado actual de la casilla.
-   Limita el contenido textual de la casilla a dos líneas como máximo.
-   Redacta la etiqueta de la casilla como una declaración que sea cierta si se marca la casilla y falsa si no se marca.
-   Usa la fuente predeterminada salvo que se indique lo contrario en las directrices de tu marca.
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

    ![Grupo de botones de radio con tres opciones: Sin picante, Picante y Muy picante.](images/spicyoptions.png)

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Clase CheckBox](https://msdn.microsoft.com/library/windows/apps/br209316) 
- [Botones de radio](radio-button.md)
- [Modificador para alternar](toggles.md)
