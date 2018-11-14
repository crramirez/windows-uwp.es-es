---
author: QuinnRadich
Description: Radio buttons let users select one option from two or more choices.
title: Directrices para botones de radio
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 577e4ca0716427298344ac2eec5155c786d5530c
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6646270"
---
# <a name="radio-buttons"></a>Botones de radio

> **API importantes**: [Clase RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [Evento Checked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked), [Propiedad IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

Los botones de radio permiten a los usuarios seleccionar una opción entre un conjunto. Cada opción aparece representada por un botón de radio y los usuarios solo pueden seleccionar un único botón de radio en un grupo de botones de radio.

(Si tienes curiosidad acerca del origen del nombre, los botones de radio se llaman así por los botones de canales preestablecidos de una radio).

![Botones de radio](images/controls/radio-button.png)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa botones de radio para presentar a los usuarios dos o más opciones mutuamente exclusivas.

![Un grupo de botones de radio](images/radiobutton_basic.png)

Usa botones de radio cuando los usuarios necesiten ver todas las opciones para realizar una selección. Puesto que los botones de radio enfatizan todas las opciones por igual, atraen más atención de la necesaria sobre las opciones. Salvo que las opciones se merezcan una atención extra por parte del usuario, deberías plantearte utilizar otros controles. Por ejemplo, si la opción predeterminada se recomienda a la mayoría de los usuarios en la mayoría de las situaciones, usa en su lugar una [lista desplegable](lists.md).

![lista desplegable](images/combo_box_collapsed.png)

Si únicamente hay dos opciones que se excluyen mutuamente, combínalas en una [casilla](checkbox.md) o [modificador para alternar](toggles.md). Por ejemplo, usa una casilla para "Acepto", en lugar de dos botones de radio para "Acepto" y "No acepto".

![Dos formas de presentar una selección binaria](images/radiobutton_vs_checkbox.png)

Cuando el usuario puede seleccionar varias opciones, usa una [casilla](checkbox.md).

![Seleccionar varias opciones con casillas](images/checkbox2.png)

Cuando las opciones sean números separados por intervalos (10, 20, 30), usa un [control deslizante](slider.md).

![control deslizante](images/controls/slider.png)

Si hay más de ocho opciones, usa una [lista desplegable](lists.md) o [cuadro de lista](lists.md).

![cuadro combinado](images/combo_box_scroll.png)

Si las opciones disponibles se basan en el contexto actual de la aplicación o pueden variar de forma dinámica, usa un [cuadro de lista](lists.md) de selección única.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/RadioButton">abrir la aplicación y ver RadioButton en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Botones de radio en la configuración del navegador Microsoft Edge.

![Botones de radio en la configuración del navegador Microsoft Edge](images/control-examples/radio-buttons-edge.png)

## <a name="create-a-radio-button"></a>Crear un botón de radio

Los botones de radio funcionan en grupos. Hay dos formas de agrupar los controles de botón de radio:
- Colocarlos dentro del mismo contenedor principal.
- Establece la propiedad [GroupName](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) en cada botón de radio en el mismo valor.

En este ejemplo, el primer grupo de botones de radio se agrupará implícitamente al estar en el mismo panel de pila. El segundo grupo se divide entre dos paneles de pila, por lo que explícitamente están agrupados por GroupName.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

Los grupos de botones de radio tienen este aspecto.

![Botones de radio en dos grupos](images/radio-button-groups.png)

Un botón de radio tiene dos estados: *seleccionado* o *borrado*. Cuando se selecciona un botón de radio, su propiedad [IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) es **true**. Cuando un botón de radio está desactivado, su propiedad **IsChecked** es **false**. Un botón de radio se puede desactivar haciendo clic en otro botón de radio del mismo grupo, pero no se puede borrar haciendo clic de nuevo en él. Sin embargo, puedes borrar un botón de radio mediante programación estableciendo su propiedad IsChecked en **false**. Se puede comparar la propiedad **IsChecked** con un valor booleano obteniendo el **Valor** de la propiedad **IsChecked**

## <a name="recommendations"></a>Recomendaciones

-   Asegúrate de que esté claro tanto el objetivo como el estado actual de un conjunto de botones de radio.
-   Limita el contenido textual del botón de radio a una única línea.
-   Si el contenido es dinámico, piensa en cómo se reajustará el botón y qué sucederá con los elementos visuales que lo rodean.
-   Usa la fuente predeterminada salvo que se indique lo contrario en las directrices de tu marca.
-   No coloques dos grupos de botones de radio en paralelo. Cuando se colocan dos grupos de botones de radio juntos, es difícil determinar qué botones pertenecen a uno u otro grupo.

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales

En esta ilustración, se muestra el modo correcto de colocar y espaciar botones de radio.

![Un grupo de botones de radio.](images/radiobutton-layout.png)

![directrices de espaciado para botones de radio](images/radiobutton-redlines.png)

## <a name="related-topics"></a>Artículos relacionados

**Para diseñadores**
- [Botones](buttons.md)
- [Modificadores para alternar](toggles.md)
- [Casillas](checkbox.md)
- [Listas y cuadros combinados](lists.md)
- [Controles deslizantes](slider.md)

**Para desarrolladores (XAML)**
- [Clase RadioButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
