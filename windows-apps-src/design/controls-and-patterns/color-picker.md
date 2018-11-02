---
author: Jwmsft
Description: A color picker lets a user browse through and select colors.
title: Selector de colores
label: Color Picker
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f2b6270fcd7bc3dcf45d6e80dd547ee783d8e9ae
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5941143"
---
# <a name="color-picker"></a>Selector de colores

Un selector de colores se usa para explorar y seleccionar colores. De manera predeterminada, permite a un usuario navegar por los colores en un espectro de colores o especificar un color en los cuadros de texto rojo, verde y azul (RGB), valor de matiz-saturación (HSV) o hexadecimal.

> **API importantes**: [Clase ColorPicker](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker), [Propiedad Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color), [Evento ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged)

![Selector de colores predeterminado](images/color-picker-default.png)


## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa el selector de colores para permitir que un usuario seleccione colores en tu aplicación. Por ejemplo, úsalo para cambiar la configuración de color, como los colores de fuente, el fondo o los colores de tema de la aplicación.

Si tu aplicación está dibujando o realizando tareas similares con el lápiz, piensa en la posibilidad de usar [Controles de entrada manuscrita](inking-controls.md) junto con el selector de colores.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/ColorPicker">abrir la aplicación y ver ColorPicker en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-color-picker"></a>Crear un selector de colores

En este ejemplo se muestra cómo crear un selector de colores predeterminado en XAML.

```xaml
<ColorPicker x:Name="myColorPicker"/>
```

De manera predeterminada, el selector de colores muestra una vista previa del color seleccionado en la barra rectangular que se encuentra junto al espectro de colores. Puedes usar el evento [ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged) o la propiedad [Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color) para obtener acceso al color seleccionado y usarlo en tu aplicación. Consulta los siguientes ejemplos de código detallado.

### <a name="bind-to-the-chosen-color"></a>Enlazar al color elegido

Cuando la selección de colores debe surtir efecto inmediatamente, puedes usar el enlace de datos para enlazar a la propiedad Color o bien, controlar el evento ColorChanged para obtener acceso al color seleccionado en el código.

En este ejemplo, enlazas la propiedad Color de un SolidColorBrush que se usa como el relleno de un rectángulo directamente al color seleccionado del selector de colores. Cualquier cambio en el selector de colores produce un cambio dinámico en la propiedad enlazada.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape=”Ring”
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>

<Rectangle Height="50" Width="50">
    <Rectangle.Fill>
        <SolidColorBrush Color="{x:Bind myColorPicker.Color, Mode=OneWay}"/>
    </Rectangle.Fill>
</Rectangle>
```

En este ejemplo se usa un selector de colores simplificado únicamente con el círculo y el control deslizante, lo cual es una experiencia de selección de color informal habitual. Cuando se puede ver el cambio del color y se produce en tiempo real en el objeto afectado, no necesitas mostrar la barra de vista previa de color. Consulta la sección *Personalizar el selector de colores* para obtener más información.

### <a name="save-the-chosen-color"></a>Guardar el color elegido

En algunos casos, no quieres aplicar el cambio de color inmediatamente. Por ejemplo, cuando hospedes un selector de colores en un control flotante, te recomendamos aplicar el color seleccionado únicamente después de que el usuario confirme la selección o cierre el control flotante. También puedes guardar el valor del color seleccionado para usarlo más adelante.

En este ejemplo, hospedas un selector de colores en un control flotante con botones Confirmar y Cancelar. Cuando el usuario confirma su elección del color, puedes guardar el color seleccionado para usarlo más adelante en la aplicación.

```xaml
<Page.Resources>
    <Flyout x:Key="myColorPickerFlyout">
        <RelativePanel>
            <ColorPicker x:Name="myColorPicker"
                         IsColorChannelTextInputVisible="False"
                         IsHexInputVisible="False"/>

            <Grid RelativePanel.Below="myColorPicker"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Content="OK" Click="confirmColor_Click"
                        Margin="0,12,2,0" HorizontalAlignment="Stretch"/>
                <Button Content="Cancel" Click="cancelColor_Click"
                        Margin="2,12,0,0" HorizontalAlignment="Stretch"
                        Grid.Column="1"/>
            </Grid>
        </RelativePanel>
    </Flyout>
</Page.Resources>

<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Button x:Name="colorPickerButton"
            Content="Pick a color"
            Flyout="{StaticResource myColorPickerFlyout}"/>
</Grid>
```

```csharp
private Color mycolor;

private void confirmColor_Click(object sender, RoutedEventArgs e)
{
    // Assign the selected color to a variable to use outside the popup.
    myColor = myColorPicker.Color;

    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}

private void cancelColor_Click(object sender, RoutedEventArgs e)
{
    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}
```

### <a name="configure-the-color-picker"></a>Configurar el selector de colores

No todos los campos son necesarios para permitir a un usuario seleccionar un color, por lo que el selector de colores es flexible. Proporciona una variedad de opciones que te permiten configurar el control según tus necesidades.

Por ejemplo, cuando el usuario no necesita un control preciso, como seleccionar un color de marcador de resaltado en una aplicación de toma de notas, puedes usar una interfaz de usuario simplificada. Puedes ocultar los campos de entrada de texto y cambiar el espectro de colores a un círculo.

Cuando el usuario necesita un control preciso, como en una aplicación de diseño gráfico, puedes mostrar controles deslizantes y campos de entrada de texto para todos los aspectos del color.

#### <a name="show-the-circle-spectrum"></a>Mostrar el espectro de círculo

En este ejemplo se muestra cómo usar la propiedad [ColorSpectrumShape](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorSpectrumShape) para configurar el selector de colores con el fin de usar un espectro circular en lugar del cuadrado predeterminado.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"/>
```

![Un selector de colores con un espectro de círculo](images/color-picker-ring.png)

Cuando se deba elegir entre el espectro de colores de cuadrado y círculo, la precisión será una cuestión principal que se deberá tener en cuenta. Un usuario tiene un mayor control cuando selecciona un color específico con un cuadrado porque se muestra más de la gama de colores. Debes pensar en el espectro de círculo como más de la experiencia de elección de color informal.

#### <a name="show-the-alpha-channel"></a>Mostrar el canal alfa

En este ejemplo, habilitas un control deslizante de opacidad y cuadro de texto en el selector de colores.

```xaml
<ColorPicker x:Name="myColorPicker"
             IsAlphaEnabled="True"/>
```

![Selector de colores con IsAlphaEnabled establecido en true](images/color-picker-alpha.png)

#### <a name="show-a-simple-picker"></a>Mostrar un selector sencillo

En este ejemplo se muestra cómo configurar el selector de colores con una interfaz de usuario sencilla para su uso informal. Muestras el espectro circular y ocultas los cuadros de entrada de texto predeterminados. Cuando se puede ver el cambio del color y se produce en tiempo real en el objeto afectado, no necesitas mostrar la barra de vista previa de color. De lo contrario, debes dejar visible la vista previa del color.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>
```

![Selector de colores sencillo](images/color-picker-casual.png)

#### <a name="show-or-hide-additional-features"></a>Mostrar u ocultar características adicionales

En esta tabla se muestran todas las opciones que puedes usar para configurar el control ColorPicker.

Función | Propiedades
--------|-----------
Espectro de colores | IsColorSpectrumVisible, ColorSpectrumShape, ColorSpectrumComponents
Vista previa del color | IsColorPreviewVisible
Valores de color| IsColorSliderVisible, IsColorChannelTextInputVisible
Valores de opacidad | IsAlphaEnabled, IsAlphaSliderVisible, IsAlphaTextInputVisible
Valores hexadecimales | IsHexInputVisible

> [!NOTE]
> IsAlphaEnabled debe ser **true** para mostrar el control deslizante y el cuadro de texto de opacidad. La visibilidad de los controles de entrada se puede modificar a continuación con las propiedades IsAlphaTextInputVisible y IsAlphaSliderVisible. Consulta la documentación sobre API para obtener más información.

## <a name="dos-and-donts"></a>Qué hacer y qué no hacer

- Piensa acerca de qué tipo de experiencia de selección de colores es adecuada para tu aplicación. Es posible que algunos escenarios no requieran la selección de colores detallada y que se beneficien de un selector simplificado.
- Para lograr la experiencia de selección de colores más precisa, usa el espectro de cuadrado y asegúrate de que el tamaño es de al menos 256 x 256 píxeles, o incluye los campos de entrada de texto para permitir a los usuarios que ajusten su color seleccionado.
- Cuando se usa en un control flotante, pulsar en el espectro o ajustar solo el control deslizante no debe confirmar la selección de colores. Para confirmar la selección:
  - Proporciona botones de confirmar y cancelar para aplicar o cancelar la selección. Se descartará al presionar el botón Atrás o pulsar fuera del control flotante y no se guardará la selección del usuario.
  - O bien, confirma la selección tras descartar el control flotante pulsando fuera del control flotante o presionando el botón Atrás.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Interacciones de lápices en aplicaciones para UWP](../input/pen-and-stylus-interactions.md)
- [Entrada manuscrita](inking-controls.md)

<!--
<div class=”microsoft-internal-note”>
<p>
<p>
Note: For more info, see the [color picker redlines](http://uni/DesignDepot.FrontEnd/#/ProductNav/3666/15/dv/?t=Windows%7CControls&f=RS2) on UNI.
</div>
-->