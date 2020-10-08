---
description: Obtenga información sobre cómo usar un selector de colores para permitir que los usuarios examinen y seleccionen colores, o especifiquen colores en formato RGB, HSV o hexadecimal.
title: Selector de colores
label: Color Picker
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 886236000c31dbeda12ae95e4c920be0a2cb03f3
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750561"
---
# <a name="color-picker"></a>Selector de colores

Un selector de colores se usa para explorar y seleccionar colores. De manera predeterminada, permite a un usuario navegar por los colores en un espectro de colores o especificar un color en los cuadros de texto rojo, verde y azul (RGB), valor de matiz-saturación (HSV) o hexadecimal.

![Selector de colores predeterminado](images/color-picker-default.png)

**Obtención de la biblioteca de la interfaz de usuario de Windows**

:::row:::
   :::column:::
      ![Logotipo de WinUI](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      El control **ColorPicker** se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **API de la biblioteca de interfaz de usuario de Windows:** [Clase ColorPicker](/uwp/api/microsoft.ui.xaml.controls.colorpicker), [propiedad Color](/uwp/api/microsoft.ui.xaml.controls.colorpicker.Color), [evento ColorChanged](/uwp/api/microsoft.ui.xaml.controls.colorpicker.ColorChanged)
>
> **API de plataforma:** [Clase ColorPicker](/uwp/api/windows.ui.xaml.controls.colorpicker), [propiedad Color](/uwp/api/windows.ui.xaml.controls.colorpicker.Color), [evento ColorChanged](/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa el selector de colores para permitir que un usuario seleccione colores en tu aplicación. Por ejemplo, úsalo para cambiar la configuración de color, como los colores de fuente, el fondo o los colores del tema de la aplicación.

Si tu aplicación está pensada para dibujar o realizar tareas similares con el lápiz, considera la posibilidad de usar [controles de entrada manuscrita](inking-controls.md) junto con el selector de colores.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/ColorPicker">abrir la aplicación y ver ColorPicker en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-color-picker"></a>Crear un selector de colores

En este ejemplo se muestra cómo crear un selector de colores predeterminado en XAML.

```xaml
<ColorPicker x:Name="myColorPicker"/>
```

De manera predeterminada, el selector de colores muestra una vista previa del color seleccionado en la barra rectangular que se encuentra junto al espectro de colores. Puedes usar el evento [ColorChanged](/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged) o la propiedad [Color](/uwp/api/windows.ui.xaml.controls.colorpicker.Color) para acceder al color seleccionado y usarlo en tu aplicación. Consulta los siguientes ejemplos de código detallado.

### <a name="bind-to-the-chosen-color"></a>Enlazar al color elegido

Cuando la selección de colores debe surtir efecto inmediatamente, puedes usar el enlace de datos para enlazar a la propiedad Color o controlar el evento ColorChanged para acceder al color seleccionado en el código.

En este ejemplo, enlazas la propiedad Color de SolidColorBrush que se usa como el relleno de un rectángulo directamente al color seleccionado en el selector de colores. Cualquier cambio en el selector de colores produce un cambio dinámico en la propiedad enlazada.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
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

En algunos casos, no te interesa aplicar el cambio de color inmediatamente. Por ejemplo, cuando hospedes un selector de colores en un control flotante, te recomendamos que apliques el color seleccionado únicamente después de que el usuario confirme la selección o cierre el control flotante. También puedes guardar el valor del color seleccionado para usarlo más adelante.

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

En este ejemplo se muestra cómo usar la propiedad [ColorSpectrumShape](/uwp/api/windows.ui.xaml.controls.colorpicker.ColorSpectrumShape) para configurar el selector de colores con el fin de usar un espectro circular en lugar del cuadrado predeterminado.

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"/>
```

![Selector de colores con un espectro de círculo](images/color-picker-ring.png)

Cuando se deba elegir entre el espectro de colores de cuadrado y círculo, la precisión será una cuestión principal que se deberá tener en cuenta. Un usuario tiene un mayor control cuando selecciona un color específico con un cuadrado, ya que se muestra más de la gama de colores. Debes considerar el espectro de círculo una parte más de la experiencia de elección de color informal.

#### <a name="show-the-alpha-channel"></a>Mostrar el canal alfa

En este ejemplo, habilitas un control deslizante de opacidad y un cuadro de texto en el selector de colores.

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

Característica | Propiedades
--------|-----------
Espectro de colores | IsColorSpectrumVisible, ColorSpectrumShape, ColorSpectrumComponents
Vista previa del color | IsColorPreviewVisible
Valores de color| IsColorSliderVisible, IsColorChannelTextInputVisible
Valores de opacidad | IsAlphaEnabled, IsAlphaSliderVisible, IsAlphaTextInputVisible
Valores hexadecimales | IsHexInputVisible

> [!NOTE]
> IsAlphaEnabled debe ser **true** para mostrar el control deslizante y el cuadro de texto de opacidad. La visibilidad de los controles de entrada se puede modificar después con las propiedades IsAlphaTextInputVisible e IsAlphaSliderVisible. Consulta la documentación sobre API para obtener más información.

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- Piensa en qué tipo de experiencia de selección de colores es adecuada para tu aplicación. Es posible que algunos escenarios no requieran la selección de colores detallada y que se beneficien de un selector simplificado.
- Para lograr la experiencia de selección de colores más precisa, usa el espectro de cuadrado y asegúrate de que el tamaño es de al menos 256 x 256 píxeles, o incluye los campos de entrada de texto para permitir que los usuarios ajusten el color seleccionado.
- Cuando se usa en un control flotante, la acción de pulsar en el espectro o ajustar solo el control deslizante no debe confirmar la selección de colores. Para confirmar la selección:
  - Proporciona botones de confirmar y cancelar para aplicar o cancelar la selección. Se descartará al presionar el botón Atrás o pulsar fuera del control flotante, y no se guardará la selección del usuario.
  - También se puede confirmar la selección tras descartar el control flotante al pulsar fuera del control flotante o al presionar el botón Atrás.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Interacciones de lápiz en aplicaciones de Windows](../input/pen-and-stylus-interactions.md)
- [Entrada manuscrita](inking-controls.md)

<!--
<div class="microsoft-internal-note">
<p>
<p>
Note: For more info, see the [color picker redlines](https://uni/DesignDepot.FrontEnd/#/ProductNav/3666/15/dv/?t=Windows%7CControls&f=RS2) on UNI.
</div>
-->
