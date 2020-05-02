---
title: Radio de redondeo
description: Obtén información sobre los principios del radio de redondeo, los enfoques de diseño y las opciones de personalización.
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10;uwp;radio de redondeo;corner radius;redondeo;rounded
ms.openlocfilehash: 134a49ac57678eea0da718e93a14e3d0cf8896d5
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "81001482"
---
# <a name="corner-radius"></a>Radio de redondeo

A partir de la versión 2.2 de la [biblioteca de interfaz de usuario de Windows](/uwp/toolkits/winui/) (WinUI), el estilo predeterminado de muchos controles se actualizó para usar esquinas redondeadas. Estos nuevos estilos están diseñados para provocar calidez y confianza, así como facilitar a los usuarios el procesamiento visual de la interfaz de usuario.

A continuación se muestran dos controles de botón: el primero, sin esquinas redondeadas; y el segundo, con el nuevo estilo de esquinas redondeadas.

![Botones con y sin esquinas redondeadas](images/rounded-corner/my-button.png)

Al instalar el paquete NuGet para WinUI 2.2 o una versión posterior, se instalan nuevos estilos predeterminados para los controles WinUI y los controles de plataforma. Estos estilos se aplican automáticamente cuando se usa WinUI 2.2 en la aplicación; no es necesario realizar ninguna otra acción para usar los nuevos estilos. Sin embargo, más adelante en este artículo se muestra cómo personalizar las esquinas redondeadas en caso de que sea necesario hacerlo.

> [!IMPORTANT]
> Algunos controles están disponibles en la plataforma ([Windows.UI.Xaml.Controls](/uwp/api/windows.ui.xaml.controls)) y en WinUI ([Microsoft.UI.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls?view=winui-2.2)); por ejemplo, **TreeView** o **ColorPicker**. Al usar WinUI en la aplicación, se debe usar la versión WinUI del control. El redondeo de esquinas podría aplicarse de forma incoherente en la versión de la plataforma cuando se usa con WinUI.

> **API importantes**: [Propiedad Control.CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius)

## <a name="default-control-designs"></a>Diseños de controles predeterminados

Hay tres áreas de los controles en los que se utilizan los estilos de esquinas redondeadas: los elementos rectangulares, los elementos de control flotante y los elementos de barras.

### <a name="corners-of-rectangle-ui-elements"></a>Esquinas de los elementos rectangulares de la interfaz de usuario

- Estos elementos de la interfaz de usuario incluyen controles básicos (como los botones) que los usuarios ven en pantalla en todo momento.
- El valor de radio predeterminado que se usa para estos elementos de la interfaz de usuario es **2 px**.

![Botón con las esquinas redondeadas resaltadas](images/rounded-corner/button.png)

**Controles**

- AutoSuggestBox
- Botón
  - Botones de ContentDialog
- CalendarDatePicker
- CheckBox
  - Casillas de selección múltiple de TreeView
- ComboBox
- DatePicker
- DropDownButton
- FlipView
- PasswordBox
- RichEditBox
- SplitButton
- TextBox
- TimePicker
- ToggleButton
- ToggleSplitButton

### <a name="corners-of-flyout-and-overlay-ui-elements"></a>Esquinas de los elementos de control flotante y superposición de la interfaz de usuario

- Pueden ser elementos de interfaz de usuario transitorios que aparecen en la pantalla de forma temporal (como MenuFlyout), o elementos que se superponen a otras interfaces de usuario (como las pestañas TabView).
- El valor de radio predeterminado que se usa para estos elementos de la interfaz de usuario es **4 px**.

![Ejemplo de control flotante](images/rounded-corner/flyout.png)

**Controles**

- CommandBarFlyout
- ContentDialog
- Control flotante
- MenuFlyout
- Pestañas de TabView
- TeachingTip
- Información sobre herramientas
- Elemento de control flotante (cuando está abierto)
  - AutoSuggestBox
  - CalendarDatePicker
  - ComboBox
  - DatePicker
  - DropDownButton
  - MenuBar
  - SplitButton
  - TimePicker
  - ToggleSplitButton

### <a name="bar-elements"></a>Elementos de barras

- Estos elementos de la interfaz de usuario tienen la forma de barras o líneas; por ejemplo, ProgressBar.
- Los valores de radio predeterminados que se usan aquí son de **2 px**.

![Ejemplo de barra de progreso](images/rounded-corner/bars.png)

**Controles**

- Indicador de selección de NavigationView
- Indicador de selección dinámica
- ProgressBar
- ScrollBar (cuando `IndicatorMode=TouchIndicator`)
- Control deslizante
  - Control deslizante de color de ColorPicker
  - Control deslizante de la barra de búsqueda de MediaTransportControls

## <a name="customization-options"></a>Opciones de personalización

Los valores de radios de redondeo predeterminados que proporcionamos no son definitivos y hay varias formas de modificar fácilmente la cantidad de redondeo de las esquinas. Esto puede hacerse a través de dos recursos globales o a través de la propiedad [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) directamente en el control, en función del nivel de detalle que quieras en la personalización.

### <a name="when-not-to-round"></a>Cuándo no se debe redondear

Hay situaciones en las que no se debe redondear la esquina de un control y no se redondean de forma predeterminada.

- Cuando varios elementos de la interfaz de usuario contenidos en un contenedor se tocan entre sí, como las dos partes de un SplitButton. No debe haber ningún espacio cuando entran en contacto.

![SplitButton](images/rounded-corner/split-button-2.png)

- Cuando un control contenido dentro de otro contenedor; por ejemplo, la barra y los botones de una ScrollBar que forman parte del contenedor del control ScrollBar, que a su vez forma parte de un ScrollViewer.

![ScrollBar](images/rounded-corner/scrollbar.png)

- Cuando un elemento de control flotante de la interfaz de usuario está conectado con una interfaz de usuario que invoca el control flotante en uno de sus lados.

![AutoSuggest](images/rounded-corner/autosuggest.png)

### <a name="keyboard-focus-rectangle-and-shadow"></a>Rectángulo y sombra del foco de teclado

Nuestro diseño predeterminado no se encarga en particular de redondear las esquinas del rectángulo de foco del teclado ni de la sombra del control. El uso de un valor de radio de redondeo no los averiará en términos de funcionalidad; sin embargo, es conveniente tenerlo en cuenta para evitar los problemas visuales no deseados que podrían surgir con un valor mayor.

Este es un ejemplo de cómo un radio de redondeo mayor puede hacer que la sombra no parezca agradable:

![ContentDialogShadow](images/rounded-corner/larger-corner-radius.png)

### <a name="rounded-corners-and-performance"></a>Esquinas redondeadas y rendimiento

La representación de esquinas redondeadas utiliza de forma natural más potencia que la representación de las esquinas cuadradas. Al seleccionar los valores de radio de redondeo predeterminados, no solo se toman en cuenta los principios de diseño, sino que también se garantiza que los controles predeterminados funcionan bien al usarlos en las aplicaciones.

Cuando se medita sobre el rendimiento de la aplicación en este contexto, principalmente se debe tomar en cuenta el tiempo de carga de la página y el tiempo de inicio de la aplicación. Ten en cuenta que las esquinas redondeadas en una superficie de interfaz de usuario más grande tienen un mayor impacto en el rendimiento. Evita dibujar esquinas redondeadas en una interfaz de usuario de aplicación a pantalla completa. Esto apenas resulta un problema si la interfaz de usuario se muestra brevemente y después de que se carga la página, como un elemento ContentDialog.

### <a name="page-or-app-wide-cornerradius-changes"></a>Cambios de CornerRadius en la página o aplicación

Hay dos recursos de aplicación que controlan los radios de redondeo de todos los controles:

- `ControlCornerRadius`: el valor predeterminado es 2 px.
- `OverlayCornerRadius`: el valor predeterminado es 4 px.

Si invalidas el valor de estos recursos en cualquier ámbito, todos los controles de ese ámbito se verán afectados en consecuencia.

Esto significa que si quieres cambiar la cantidad redondeo de todos los controles donde se puede aplicar, puedes definir ambos recursos en el nivel de la aplicación con nuevos valores de CornerRadius de la siguiente manera:

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
            <ResourceDictionary>
                <CornerRadius x:Key="OverlayCornerRadius">0</CornerRadius>
                <CornerRadius x:Key="ControlCornerRadius">0</CornerRadius>
            </ResourceDictionary>
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Si quieres cambiar la cantidad de redondeo de todos los controles dentro de un ámbito determinado (como en el nivel de página o contenedor), también puedes seguir un patrón similar:

```xaml
<Grid>
    <Grid.Resources>
        <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
    </Grid.Resources>
    <Button Content="Button"/>
</Grid>
```

> [!NOTE]
> El recurso `OverlayCornerRadius` debe definirse en el nivel de la aplicación para que surta efecto.
>
>Esto se debe a que los elementos emergentes y los controles flotantes son dinámicos y se crean en el elemento raíz del árbol visual, por lo que todos los recursos que utilizan también se deben definir allí. De lo contrario, están fuera del ámbito.

### <a name="per-control-cornerradius-changes"></a>Cambios de CornerRadius por control

Puedes modificar la propiedad [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) en los controles directamente si quieres cambiar la cantidad de redondeo de solo un número reducido de controles.

|Valor predeterminado | Propiedad modificada |
|:-- |:-- |
|![DefaultCheckBox](images/rounded-corner/default-checkbox.png)| ![CustomCheckBox](images/rounded-corner/custom-checkbox.png)|
|`<CheckBox Content="Checkbox"/>` | `<CheckBox Content="Checkbox" CornerRadius="5"/> ` |

No todas las esquinas de los controles responderán a la modificación de la propiedad `CornerRadius`. Para asegurarte de que el control cuyas esquinas quieres redondear responderá realmente a la propiedad `CornerRadius` de la manera esperada, comprueba primero que los recursos globales `ControlCornerRadius` o `OverlayCornerRadius` afecten al control en cuestión. Si no es así, comprueba que el control que quieres redondear tiene esquinas. Muchos de nuestros controles no representan visualmente bordes reales y, por lo tanto, no pueden hacer un uso correcto de la propiedad `CornerRadius`.

### <a name="basing-custom-styles-on-winui"></a>Estilos personalizados basados en WinUI

Puedes basar tus estilos personalizados en los estilos de esquinas redondeadas de WinUI si especificas el atributo `BasedOn` correcto en el estilo. Por ejemplo, para crear un estilo de botón personalizado basado en el estilo de botón de WinUI, haz lo siguiente:

```xaml
<Style x:Key="MyCustomButtonStyle" BasedOn="{StaticResource DefaultButtonStyle}">
   ...
</Style>
```

En general, los estilos de control de WinUI siguen una convención de nomenclatura coherente: "DefaultXYZStyle" donde "XYZ" es el nombre del control. Para obtener una referencia completa, puedes examinar los archivos XAML en el repositorio de WinUI.
