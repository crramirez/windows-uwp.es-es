---
Description: Los botones de radio permiten a los usuarios seleccionar una opción entre dos o más opciones
title: Directrices para botones de radio
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 06/10/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: dc6f5eb32cdedf442b6866e1e53be85edfb98dcb
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493440"
---
# <a name="radio-buttons"></a>Botones de radio

Los botones de radio, también llamados botones de opción, permiten que los usuarios seleccionen una opción entre una colección de dos o más opciones mutuamente excluyentes, aunque relacionadas. Cada botón de radio representa una opción.

De manera predeterminada, no está seleccionado ningún botón de radio en un grupo RadioButtons. Es decir, todos los botones de radio están desactivados. Sin embargo, cuando se selecciona un botón de radio, no se puede restaurar el estado desactivado del grupo.

El comportamiento único de un grupo RadioButtons lo distingue de las [casillas](checkbox.md), que admiten la selección múltiple y la anulación de la selección, o la desactivación.

![Ejemplo de un grupo RadioButtons con un botón de radio seleccionado](images/controls/radio-button.png)

## <a name="get-the-windows-ui-library"></a>Obtener la biblioteca de interfaz de usuario de Windows

| &nbsp; | &nbsp; |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | El control RadioButtons se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y las características de interfaz de usuario para las aplicaciones de Windows. Para más información e instrucciones sobre la instalación, consulte [Biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

**API de la biblioteca de interfaz de usuario de Windows**: 
* [Clase RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
* [Evento SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
* [Propiedad SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)
* [Propiedad SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)

**API de plataforma**: 
* [Clase RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)
* [Evento Checked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)
* [Propiedad IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Use botones de radio para permitir que los usuarios seleccionen entre dos o más opciones mutuamente excluyentes.

![Grupo RadioButtons con un botón de radio seleccionado](images/radiobutton_basic.png)

Use botones de radio cuando los usuarios necesiten ver todas las opciones antes de realizar una selección. Los botones de radio enfatizan todas las opciones por igual, lo que significa que algunas opciones pueden atraer más atención de la necesaria o deseada. 

Salvo que todas las opciones se merezcan la misma atención, debería plantearse usar otros controles. Por ejemplo, para recomendar una única mejor opción para la mayoría de los usuarios y en la mayoría de las situaciones, use un [cuadro combinado](combo-box.md) para mostrar la mejor opción como opción predeterminada.

![Un cuadro combinado, que muestra una opción predeterminada](images/combo_box_collapsed.png)

Si solo hay dos opciones, que se excluyen mutuamente, combínelas en un solo control de [casilla](checkbox.md) o de [conmutador de alternancia](toggles.md). Por ejemplo, use una única casilla para "Acepto", en lugar de dos botones de radio para "Acepto" y "No acepto".

![Una casilla es una buena alternativa para presentar una elección entre dos opciones](images/radiobutton_vs_checkbox.png)

Use [casillas](checkbox.md) cuando el usuario pueda seleccionar varias opciones.

![Casillas que admiten la selección múltiple](images/checkbox2.png)

Cuando las opciones de los usuarios se encuentran dentro de un intervalo de valores (por ejemplo, *10, 20, 30... 100*), use un [control deslizante](slider.md).

![Control deslizante que muestra un valor en un intervalo de valores](images/controls/slider.png)

Si hay ocho opciones o más, utilice un [cuadro combinado](combo-box.md).

![Cuadro de lista que muestra varias opciones](images/combo_box_scroll.png)

> [!NOTE]
> Si las opciones disponibles se basan en el contexto actual de la aplicación o pueden variar dinámicamente, use un control de lista.

## <a name="radiobuttons-behavior"></a>Comportamiento del elemento RadioButtons

El acceso de teclado y el comportamiento de la navegación se han optimizado en la [clase RadioButton](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041). Estas mejoras ayudan a los usuarios avanzados de accesibilidad y de teclado a desplazarse por la lista de opciones de manera más rápida y sencilla.

Además de estas mejoras, el diseño visual predeterminado de los botones de radio individuales de un grupo RadioButtons se ha optimizado a través de la configuración automatizada de la orientación, el espaciado y los márgenes. Esta optimización elimina la necesidad de especificar estas propiedades, como quizá deba hacer cuando use un control de agrupación más primitivo, como [StackPanel](../layout/layout-panels.md#stackpanel) o [Grid](../layout/layout-panels.md#grid).

### <a name="navigating-a-radiobuttons-group"></a>Navegación por un grupo RadioButtons

El control RadioButtons admite dos estados:

- No se ha seleccionado ningún botón de radio.
- Se ha seleccionado un botón de radio.

Las dos secciones siguientes cubren ambos comportamientos de foco de los botones de radio.

#### <a name="no-radio-button-is-selected"></a>No se ha seleccionado ningún botón de radio.

Cuando no se selecciona ningún botón de radio, el primero de la lista obtiene el foco.

> [!NOTE]
> El elemento que recibe el foco de tabulación en la navegación por tabulación inicial no está seleccionado.

|Lista sin foco de tabulación | Lista con el foco de tabulación inicial|
|:--:|:--:|
| ![Lista sin foco de tabulación](images/radiobutton-no-selected-item-no-tab-focus.png) | ![Lista con el foco de tabulación inicial](images/radiobutton-no-selected-item-tab-focus.png)|

#### <a name="one-radio-button-is-selected"></a>Se ha seleccionado un botón de radio.

Cuando se selecciona un botón de radio y el usuario usa la tabulación en la lista, el botón de radio seleccionado obtiene el foco.

|Lista sin foco de tabulación | Lista con el foco de tabulación inicial |
|:--:|:--:|
| ![Lista sin foco de tabulación](images/radiobutton-selected-item-no-tab-focus.png) | ![Lista con el foco de tabulación inicial](images/radiobutton-selected-item-tab-focus.png)|


### <a name="keyboard-navigation"></a>Navegación con el teclado

Cuando los usuarios tienen una sola fila o columna de opciones de botón de radio y un elemento ya ha recibido el foco de tabulación, pueden usar las teclas de dirección para una "navegación interna" entre los elementos que contiene el control RadioButtons. Para más información sobre los comportamientos de la navegación con el teclado, consulte [Interacciones de teclado: navegación](../input/keyboard-interactions.md#navigation).

En el caso de un control RadioButtons, cuando la lista de opciones está organizada solo de forma vertical, las teclas de dirección arriba y abajo mueven entre los elementos, y las teclas de dirección izquierda y derecha no hacen nada. No obstante, en una lista que esté organizada solo de forma horizontal, las teclas de dirección izquierda, derecha, arriba y abajo mueven entre los elementos de la misma manera.

![Ejemplo de navegación con el teclado en un grupo RadioButtons de una sola columna o fila](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
*Ejemplo de navegación con el teclado en un grupo RadioButtons de una sola columna o fila*

#### <a name="navigating-within-multi-column-or-multi-row-layouts"></a>Navegación por diseños de varias columnas o filas

En el orden de columna principal, el foco se mueve de arriba a abajo y de izquierda a derecha. Cuando el foco se encuentra en el último elemento de una columna y se presiona la tecla de dirección abajo, el foco se mueve al primer elemento de la columna siguiente. Este mismo comportamiento se produce a la inversa: cuando el foco se encuentra en el primer elemento de una columna y se presiona la tecla de dirección arriba, el foco se mueve al último elemento de la columna anterior.

![Ejemplo de navegación con el teclado de un grupo RadioButtons de varias columnas o filas](images/radiobutton-keyboard-navigation-multi-column-row.png)

En el orden de fila mayor (en el que los elementos se rellenan de izquierda a derecha y de arriba abajo), cuando el foco está en el último elemento de una fila y se presiona la tecla de flecha derecha, este se mueve al primer elemento de la fila siguiente. Este mismo comportamiento se produce a la inversa: cuando el foco se encuentra en el primer elemento en una fila y se presiona la tecla de dirección izquierda, el foco se mueve al último elemento de la fila anterior.

Para más información, consulte [Interacciones de teclado](https://docs.microsoft.com/windows/uwp/design/input/keyboard-interactions#wrapping-homogeneous-list-and-grid-view-items).

##### <a name="wrapping"></a>Ajuste

El grupo RadioButtons no se ajusta. Esto se debe a que, cuando los usuarios usan un lector de pantalla, se pierde la sensación de que haya límites y la indicación clara de dónde está el principio y el fin, lo que dificulta que los usuarios con discapacidad visual naveguen por la lista. El control RadioButtons tampoco es compatible con la enumeración, porque el control está diseñado para contener un número razonable de elementos (consulte [¿Es este el control adecuado?](#is-this-the-right-control)).

## <a name="selection-follows-focus"></a>La selección sigue el foco

Cuando los usuarios usan el teclado para navegar por los elementos de una lista RadioButtons en la que ya hay un elemento seleccionado, cuando el foco se mueve de un elemento al siguiente, el elemento recién enfocado se selecciona y el elemento que anteriormente tenía el foco se desactiva.

|Antes de la navegación con el teclado | Después de la navegación con el teclado|
|:--|:--|
| ![Ejemplo de foco y selección antes de la navegación con el teclado](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*Ejemplo de foco y selección antes de la navegación con el teclado* | ![Ejemplo de foco y selección después de la navegación con el teclado](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*Ejemplo de foco y selección después de la navegación con el teclado, en el que la tecla de dirección abajo o derecha mueve el foco al botón de radio 3, lo selecciona y desactiva el botón de radio 2* |

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Navegación con el controlador para juegos y el control remoto de Xbox

Si un usuario usa un controlador para juegos o un control remoto de Xbox para moverse entre botones de radio, el comportamiento "la selección sigue el foco" estará deshabilitado y el usuario debe presionar el botón "A" para seleccionar el botón de radio que tiene el foco.

## <a name="accessibility-behavior"></a>Comportamiento de accesibilidad

En la tabla siguiente se describe cómo el Narrador controla un grupo RadioButtons y lo que se anuncia. Este comportamiento depende de la forma en que un usuario ha establecido las preferencias de detalle del Narrador.

| Foco inicial | El foco se mueve a un elemento seleccionado |
|:--|:--|
| La colección RadioButton "nombre de grupo" tiene el foco y el elemento x de N elementos está seleccionado. | Si se selecciona el RadioButton "nombre", el elemento x tiene el foco. |
| La colección RadioButton "nombre de grupo" tiene el foco y no se ha seleccionado ningún elemento.| Si no se selecciona el RadioButton "nombre", el elemento x tiene el foco. <br> Si el usuario usa las teclas de dirección de desplazamiento, ninguna selección sigue el foco. |

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="The XAML Controls Gallery app icon"></img></td>
<td>
    <p>Si tiene instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, <a href="xamlcontrolsgallery:/item/RadioButton">ábrala para ver el control RadioButtons en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="using-the-winui-radiobuttons-control"></a>Uso del control RadioButtons de WinUI

Si usa [WinUI](https://github.com/microsoft/microsoft-ui-xaml), se recomienda que use el control [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons).

El control RadioButtons es fácil de configurar y usar y, además, garantiza un comportamiento adecuado y esperado del teclado y el Narrador.

En el código siguiente, se declara un control RadioButtons básico con tres opciones:

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```
El resultado se muestra en la siguiente imagen:

![Botones de radio en dos grupos](images/default-radiobutton-group.png)

### <a name="defining-multiple-columns"></a>Definición de varias columnas

Puede declarar un control RadioButtons de varias columnas mediante la especificación de la propiedad [MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns).

```xaml
<muxc:RadioButtons Header="App Mode" MaxColumns="3">
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
    <x:String>Column 1</x:String>
    <x:String>Column 2</x:String>
    <x:String>Column 3</x:String>
</muxc:RadioButtons>
```

![Botones de radio en grupos de dos o tres columnas](images/radiobutton-multi-columns.png)

### <a name="data-binding"></a>Enlace de datos

El control RadioButtons admite el enlace de datos mediante la propiedad [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource), tal y como se muestra en el siguiente fragmento de código.

```xaml
<RadioButtons Header="App Mode" ItemsSource="{x:Bind radioButtonItems}" />
```

```c#
public sealed partial class MainPage : Page
{
    public class OptionDataModel
    {
        public string Label;
        public override string ToString()
        {
            return Label;
        }
    }

    List<OptionDataModel> radioButtonItems;

    public MainPage()
    {
        this.InitializeComponent();

        radioButtonItems = new List<OptionDataModel>();
        radioButtonItems.Add(new OptionDataModel() { label = "Item 1" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 2" });
        radioButtonItems.Add(new OptionDataModel() { label = "Item 3" });
    }
}
```

## <a name="create-your-own-radiobuttons-group"></a>Creación de su propio grupo RadioButtons

> [!Important]
> A menos que use una versión anterior de WinUI, se recomienda usar el control RadioButtons de WinUI para agrupar los elementos RadioButton.

Los botones de radio funcionan en grupos. Puede agrupar los botones de radio de una de estas dos maneras:

- Colocarlos dentro del mismo contenedor principal.
- Establece la propiedad [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) de cada botón de radio en el mismo valor.

En este ejemplo, el primer grupo de botones de radio se agrupará implícitamente al estar en el mismo panel de pila. El segundo grupo se divide entre dos paneles de pila, por lo que se agrupan explícitamente por GroupName.

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

En la siguiente imagen se muestra cómo se representa este grupo RadioButtons:

![Botones de radio en dos grupos](images/radio-button-groups.png)

## <a name="radio-button-states"></a>Estados de los botones de radio

Un botón de radio tiene dos estados: seleccionado o desactivado. Cuando se selecciona un botón de radio, su propiedad [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) es `true`. Cuando se desactiva un botón de radio, su propiedad IsChecked es `false`. Un botón de radio se puede desactivar si un usuario selecciona otro botón de radio del mismo grupo, pero no se puede desactivar si el usuario lo vuelve a seleccionar. Sin embargo, puede desactivar un botón de radio mediante programación estableciendo su propiedad IsChecked en `false`.

## <a name="recommendations"></a>Recomendaciones

- Asegúrese de que tanto el objetivo como el estado actual de un conjunto de botones de radio sean explícitos.
- Limite la etiqueta de texto del botón de radio a una única línea.
- Si la etiqueta de texto es dinámica, piense en cómo cambiará el tamaño del botón automáticamente y qué sucederá con los elementos visuales que la rodean.
- Use la fuente predeterminada, salvo que se indique lo contrario en las directrices de su marca.
- No coloque dos grupos RadioButtons de lado. Cuando se colocan dos grupos RadioButtons uno al lado del otro, resulta difícil para los usuarios determinar qué botones pertenecen a uno u otro grupo.

### <a name="visuals-to-consider"></a>Elementos visuales que se deben considerar

En las siguientes imágenes se muestra la mejor manera de organizar los botones de radio en un grupo RadioButtons.

![Imagen que muestra un conjunto de botones de radio, organizados verticalmente](images/radiobutton-layout.png)

![Imagen que muestra directrices de espaciado para botones de radio](images/radiobutton-redline.png)

> [!NOTE]
> Si usa un control RadioButtons de WinUI, tanto el espaciado como los márgenes y la orientación ya están optimizados.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- Para obtener todos los controles XAML en un formato interactivo, consulte [Ejemplo de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery). 

## <a name="related-topics"></a>Temas relacionados

### <a name="for-designers"></a>Para diseñadores

- [Botones](buttons.md)
- [Modificadores para alternar](toggles.md)
- [Casillas](checkbox.md)
- [Listas y cuadros combinados](lists.md)
- [Controles deslizantes](slider.md)

### <a name="for-developers-xaml"></a>Para desarrolladores (XAML)

- [Clase RadioButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
