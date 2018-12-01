---
Description: The toggle switch represents a physical switch that allows users to turn things on or off.
title: Directrices para controles de modificadores para alternar
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Toggle switches
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d0436f360facceb4a7ee0d02defd5fac2e33eaae
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8344463"
---
# <a name="toggle-switches"></a>Modificadores para alternar

El modificador para alternar representa un conmutador físico que permite a los usuarios activar o desactivar opciones, como un interruptor de la luz. Usa los controles del modificador para alternar para presentar a los usuarios dos opciones que se excluyan mutuamente (como activar/desactivar). Cuando elijan una opción, se producirá un resultado inmediato.

Para crear un control de modificador para alternar, usa la [clase ToggleSwitch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch).

> **API importantes**: [Clase ToggleSwitch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch), [Propiedad IsOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison), [Evento Toggled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un modificador para alternar operaciones binarias que surten efecto inmediatamente después de que el usuario gira el modificador para alternar.

![Modificador para alternar Wi-Fi, activado y desactivado](images/toggleswitches01.png)

Piensa en el modificador para alternar como un interruptor de alimentación físico para un dispositivo: lo activas o desactivas cuando quieres habilitar o deshabilitar la acción que lleva a cabo el dispositivo.

Para que que el modificador para alternar sea sencillo de comprender, etiquétalo con una o dos palabras, preferiblemente nombres, que describan la funcionalidad que controla. Por ejemplo, "Wi-Fi" o "Luces de cocina". 

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles de XAML</strong>, haz clic aquí para abrir la aplicación y ver <a href="xamlcontrolsgallery:/item/ToggleSwitch">ToggleSwitch</a> o <a href="xamlcontrolsgallery:/item/ToggleButton">ToggleButton</a> en acción.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (MicrosoftStore)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="choosing-between-toggle-switch-and-check-box"></a>Elegir entre un modificador para alternar y una casilla

Para algunas acciones, tanto un modificador para alternar como una casilla podrían ser adecuados. Para decidir qué control funcionará mejor, sigue estas sugerencias:

- Usa un modificador para alternar con opciones binarias cuando los cambios surten efecto justo después de que el usuario las cambia.

    ![Modificador para alternar frente a casilla](images/toggleswitches02.png)

    En este ejemplo, queda claro en el caso del modificador para alternar que las luces de cocina están configuradas como "Activado". Pero en el caso de la casilla, el usuario tiene que pensar si las luces están activadas ahora o si es necesario marcar la casilla para activarlas.

- Usa casillas para los elementos opcionales (deseables).
- Usa una casilla cuando el usuario tiene que realizar algunos pasos más antes de que los cambios surtan efecto. Por ejemplo, si el usuario tiene que hacer clic en un botón "enviar" o "siguiente" para aplicar los cambios, usa una casilla.
- Usa casillas cuando el usuario pueda seleccionar varios elementos que están relacionados con un único valor o característica.

## <a name="toggle-switches-in-the-windows-ui"></a>Modificadores para alternar en la interfaz de usuario de Windows

Estas imágenes muestran cómo la interfaz de usuario de Windows usa modificadores para alternar. Así es como la pantalla de configuración de almacenamiento inteligente usa los modificadores para alternar:

![Modificadores para alternar en el almacenamiento inteligente](images/SmartStorageToggle.png)

Este ejemplo es de la página de configuración de luz nocturna:

![Modificadores para alternar en la configuración del menú Inicio de Windows](images/NightLightToggle.png)

## <a name="create-a-toggle-switch"></a>Crear un modificador para alternar

Aquí te mostramos cómo crear un modificador para alternar sencillo. Este código XAML crea el modificador para alternar que se mostró anteriormente.

```xaml
<ToggleSwitch x:Name="lightToggle" Header="Kitchen Lights"/>
```

Aquí te mostramos cómo crear el mismo modificador para alternar en el código.

```csharp
ToggleSwitch lightToggle = new ToggleSwitch();
lightToggle.Header = "Kitchen Lights";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(lightToggle);
```

### <a name="ison"></a>IsOn

El modificador puede estar activado o desactivado. Usa la propiedad [IsOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison) para determinar el estado del modificador. Cuando se usa el modificador para controlar el estado de otra propiedad binaria, puedes usar un enlace como se muestra aquí.

```xaml
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### <a name="toggled"></a>Toggled

En otros casos, puedes controlar el evento [Toggled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled) para responder a cambios en el estado.

En este ejemplo se muestra cómo agregar un controlador de eventos Toggled en XAML y en el código. El evento Toggled se controla para activar o desactivar un anillo de progreso y cambiar su visibilidad.

```xaml
<ToggleSwitch x:Name="toggleSwitch1" IsOn="True"
              Toggled="ToggleSwitch_Toggled"/>
```

Aquí te mostramos cómo crear el mismo modificador para alternar en el código.

```csharp
// Create a new toggle switch and add a Toggled event handler.
ToggleSwitch toggleSwitch1 = new ToggleSwitch();
toggleSwitch1.Toggled += ToggleSwitch_Toggled;

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(toggleSwitch1);
```

Este es el controlador para el evento Toggled.

```csharp
private void ToggleSwitch_Toggled(object sender, RoutedEventArgs e)
{
    ToggleSwitch toggleSwitch = sender as ToggleSwitch;
    if (toggleSwitch != null)
    {
        if (toggleSwitch.IsOn == true)
        {
            progress1.IsActive = true;
            progress1.Visibility = Visibility.Visible;
        }
        else
        {
            progress1.IsActive = false;
            progress1.Visibility = Visibility.Collapsed;
        }
    }
}
```

### <a name="onoff-labels"></a>Etiquetas On y Off

De forma predeterminada, el modificador para alternar incluye las etiquetas literales On y Off, que se localizan automáticamente. Puedes reemplazar estas etiquetas si configuras las propiedades [OnContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontent) y [OffContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontent).

En este ejemplo, se reemplazan las etiquetas On/Off con etiquetas Show/Hide.

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide"
              Toggled="ToggleSwitch_Toggled"/>
```

También puedes usar contenido más complejo si configuras las propiedades [OnContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontenttemplate) y [OffContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontenttemplate).

## <a name="recommendations"></a>Recomendaciones

- Usa las etiquetas predeterminadas On y Off cuando sea posible; reemplázalas solo cuando sea necesario para que el modificador para alternar tenga sentido. Si las reemplaza, usa una sola palabra que describa con mayor precisión la alternancia. En general, si las palabras "On" y "Off" no describen la acción vinculada a un modificador para alternar, probablemente necesites un control diferentes.
- Evita reemplazar las etiquetas On y Off a menos que debas hacerlo; usa las etiquetas predeterminadas a menos que la situación requiera que las personalices.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Clase ToggleSwitch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)
- [Botones de radio](radio-button.md)
- [Modificadores para alternar](toggles.md)
- [Casillas](checkbox.md)
