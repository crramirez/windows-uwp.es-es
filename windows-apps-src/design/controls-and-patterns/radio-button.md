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
ms.openlocfilehash: 6705c314d9a70f8b6282841a7f8b1df76c6ef880
ms.sourcegitcommit: 6dd6d61c912daab2cc4defe5ba0cf717339f7765
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2020
ms.locfileid: "84978407"
---
# <a name="radio-buttons"></a>Botones de radio

Los botones de radio permiten que los usuarios seleccionen una opción de una colección de dos o más opciones mutuamente excluyentes, aunque relacionadas. Cada botón de radio representa una opción.

De manera predeterminada, no se selecciona ninguno de los botones de radio de un grupo. Sin embargo, cuando un usuario selecciona una opción de botón de radio, no puede restaurar el estado no seleccionado del grupo.

El comportamiento único de un grupo de botones de radio lo distingue de las [casillas](checkbox.md), que admiten la selección múltiple y la anulación de la selección.

![Botones de radio](images/controls/radio-button.png)

**Obtención de la biblioteca de la interfaz de usuario de Windows**

|  |  |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | El control **RadioButtons** se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y las características de interfaz de usuario para las aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de la biblioteca de interfaz de usuario de Windows:** [Clase RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons), [evento SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged), [propiedad SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) y [propiedad SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)
>
> **API de plataforma:** [Clase RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [Evento Checked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked) y [Propiedad IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Use botones de radio para ofrecer a los usuarios la posibilidad de seleccionar entre dos o más opciones mutuamente exclusivas.

![Un grupo de botones de radio](images/radiobutton_basic.png)

Usa botones de radio cuando los usuarios necesiten ver todas las opciones para realizar una selección. Puesto que los botones de radio enfatizan todas las opciones por igual, atraen más atención de la necesaria o deseada sobre las opciones. Salvo que todas las opciones se merezcan la misma atención por parte del usuario, debería plantearse usar otros controles. Por ejemplo, la opción predeterminada si se recomienda a la mayoría de los usuarios en la mayor parte de las situaciones, use un [cuadro combinado](combo-box.md) en su lugar.

![Lista desplegable que se usa para resaltar una opción predeterminada](images/combo_box_collapsed.png)

Si solo hay dos opciones que se excluyen mutuamente, combínelas en una sola [casilla](checkbox.md) o [conmutador de alternancia](toggles.md). Por ejemplo, usa una casilla para "Acepto", en lugar de dos botones de radio para "Acepto" y "No acepto".

![Una casilla es una buena alternativa para presentar una elección entre dos opciones](images/radiobutton_vs_checkbox.png)

Utiliza una [casilla](checkbox.md) cuando el usuario pueda seleccionar varias opciones.

![Las casillas admiten la selección múltiple](images/checkbox2.png)

Utiliza un [control deslizante](slider.md) cuando las opciones sean números separados por intervalos (10, 20, 30).

![Control deslizante que se usa para seleccionar valores escalonados](images/controls/slider.png)

Si hay más de ocho opciones, use un [cuadro combinado o de lista](combo-box.md).

![Cuadro de lista que se usa para presentar varias opciones](images/combo_box_scroll.png)

> [!NOTE]
> Si las opciones disponibles se basan en el contexto actual de la aplicación o pueden variar dinámicamente, usa un [cuadro de lista](combo-box.md#list-boxes) de selección única.

## <a name="radiobuttons-behavior"></a>Comportamiento del elemento RadioButtons

Se optimizó el comportamiento de navegación y acceso con el teclado en los grupos [RadioButton](/uwp/api/windows.ui.xaml.controls.radiobutton?view=winrt-19041) para ayudar tanto a los usuarios avanzados del teclado como a los de accesibilidad a navegar por la lista de opciones de forma más rápida y sencilla.

Además de las mejoras de accesibilidad y los métodos abreviados de teclado, el diseño visual predeterminado de los botones de radio individuales de un grupo RadioButton también se optimizó a través de la configuración automatizada de la orientación, el espaciado y los márgenes. De este modo, se elimina la necesidad de especificar estas propiedades, como quizá debe hacer cuando usa un control de agrupación más primitivo, como [StackPanel](../layout/layout-panels.md#stackpanel) o [Grid](../layout/layout-panels.md#grid).

### <a name="navigating-a-radiobuttons-group"></a>Navegación por un grupo RadioButtons

El control RadioButtons admite dos estados:

- Una lista de controles RadioButton en la que ninguno está marcado ni seleccionado
- Una lista de controles RadioButton en la que uno de ellos está marcado o seleccionado

Las dos secciones siguientes cubren ambos comportamientos de foco de los botones de radio.

#### <a name="item-already-selected"></a>Elemento ya seleccionado

Cuando se selecciona un botón de radio y el usuario usa la tecla TAB para ir a la lista, el botón de radio seleccionado obtiene el foco.

|Lista sin foco de tabulación | Lista con el foco de tabulación inicial |
|:--:|:--:|
| ![Lista sin foco de tabulación](images/radiobutton-selected-item-no-tab-focus.png) | ![Lista con el foco de tabulación inicial](images/radiobutton-selected-item-tab-focus.png)|

#### <a name="no-item-selected"></a>Ningún elemento seleccionado

Cuando no se selecciona ningún botón de radio, el primero de la lista obtiene el foco.

> [!NOTE]
> El elemento que recibe el foco de tabulación al iniciarse la navegación por tabulación no se seleccionará ni se marcará.

|Lista sin foco de tabulación | Lista con el foco de tabulación inicial|
|:--:|:--:|
| ![Lista sin foco de tabulación](images/radiobutton-no-selected-item-no-tab-focus.png) | ![Lista con el foco de tabulación inicial](images/radiobutton-no-selected-item-tab-focus.png)|

### <a name="keyboard-navigation"></a>Navegación con el teclado

Si tiene una sola fila o columna de opciones de botón de radio y un elemento ya recibió el foco de tabulación, las teclas de dirección proporcionan una "navegación interna" entre los elementos que contiene el control RadioButtons. Para obtener más información sobre los comportamientos de la navegación con el teclado, consulte la sección [Navegación del artículo Interacciones de teclado](../input/keyboard-interactions.md#navigation).

En el caso de un control RadioButtons, cuando la lista de opciones está organizada de forma vertical (únicamente), las teclas de flecha arriba y abajo se desplazan entre los elementos y las teclas de flecha izquierda y derecha no hacen nada. No obstante, en una lista que esté organizada de forma horizontal (únicamente), las teclas de flecha izquierda, derecha, arriba y abajo se desplazan entre los elementos de la misma manera.

![Ejemplo de navegación con el teclado en un grupo de controles RadioButton de una sola columna o fila](images/radiobutton-keyboard-navigation-single-column-row.png)<br/>
*Ejemplo de navegación con el teclado en un grupo de controles RadioButton de una sola columna o fila*

#### <a name="navigating-within-multi-columnrow-layouts"></a>Navegación por los diseños de varias columnas o filas

En el orden de columna mayor (en el que los elementos se rellenan de arriba abajo y de izquierda a derecha), cuando el foco está en el último elemento de una columna y se presiona la tecla de flecha abajo, este se mueve al primer elemento de la columna siguiente. Este mismo comportamiento se presenta a la inversa: cuando el foco se encuentra en el primer elemento de una columna y se presiona la tecla de flecha arriba, este se mueve al último elemento de la columna anterior.

![Ejemplo de navegación con el teclado de un grupo de controles RadioButton de varias columnas o filas](images/radiobutton-keyboard-navigation-multi-column-row.png)

En el orden de fila mayor (en el que los elementos se rellenan de izquierda a derecha y de arriba abajo), cuando el foco está en el último elemento de una fila y se presiona la tecla de flecha derecha, este se mueve al primer elemento de la fila siguiente. Este mismo comportamiento se presenta a la inversa: cuando el foco se encuentra en el primer elemento de una fila y se presiona la tecla de flecha izquierda, este se mueve al último elemento de la fila anterior.

##### <a name="wrapping"></a>Ajuste

El grupo RadioButtons no se ajusta. Esto se debe a que, al usar un lector de pantalla, se pierde la sensación de que haya límites y la indicación clara de dónde está el principio y el fin, lo que dificulta que los usuarios con discapacidades visuales naveguen por la lista. El control RadioButtons tampoco es compatible con la enumeración, ya que está diseñado para contener un número razonable de elementos (consulte [¿Es este el control adecuado?](#is-this-the-right-control)).

## <a name="selection-follows-focus"></a>La selección sigue el foco

Al usar el teclado para desplazarse por los elementos de una lista RadioButtons (en la que ya hay un elemento seleccionado), cuando el foco se mueve de un elemento al siguiente, el elemento recién enfocado se selecciona o marca y el elemento que anteriormente tenía el foco se anula o desmarca.

|Antes de la navegación con el teclado | Después de la navegación con el teclado|
|:--|:--|
| ![Ejemplo de foco y selección antes de la navegación con el teclado](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>*Ejemplo de foco y selección antes de la navegación con el teclado* | ![Ejemplo de foco y selección después de la navegación con el teclado](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>*Ejemplo de foco y selección después de la navegación con el teclado, en el que la tecla de flecha abajo o derecha mueve el foco al elemento RadioButton "3", selecciona el "3" y anula la selección del "2".*

### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Navegación con el controlador para juegos y el control remoto de Xbox

Si usa un controlador para juegos o un control remoto de Xbox para navegar dentro de un control RadioButtons, el comportamiento "la selección sigue el foco" estará deshabilitado y deberá presionar el botón "A" para seleccionar el botón de radio que tiene el foco.

## <a name="accessibility-behavior"></a>Comportamiento de accesibilidad

En la tabla siguiente se detalla cómo el Narrador administra un grupo de botones de radio y lo que se anuncia (depende de las preferencias de detalle del Narrador que el usuario haya configurado).

| Foco inicial | El foco se mueve a un elemento seleccionado |
|:--|:--|
| Colección RadioButton "Nombre de grupo"; se seleccionó la opción x de N | Se seleccionó el control RadioButton "nombre"; x de N |
|Colección RadioButton "nombre de grupo"; ninguna opción seleccionada| Control RadioButton "nombre" sin seleccionar; x de N <br> *(Si navega con las teclas Mayús y de dirección, lo que indica que no está disponible la selección que sigue al foco)* |

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/RadioButton">abrir la aplicación y ver RadioButton en acción</a>.</p>
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

A continuación, se declara un control RadioButtons básico con tres opciones.

```xaml
<RadioButtons Header="App Mode" SelectedIndex="2">
    <RadioButton>Item 1</RadioButton>
    <RadioButton>Item 2</RadioButton>
    <RadioButton>Item 3</RadioButton>
</RadioButtons>
```

![Botones de radio en dos grupos](images/default-radiobutton-group.png)

### <a name="defining-multiple-columns"></a>Definición de varias columnas

Puede declarar un control RadioButtons de varias columnas al especificar la propiedad [MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns).

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

![Botones de radio en grupos de dos columnas](images/radiobutton-multi-columns.png)

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

## <a name="create-your-own-radio-button-group"></a>Creación de su propio grupo de botones de radio

> [!Important]
> Se recomienda usar el control RadioButtons de WinUI para agrupar los elementos RadioButton (a menos que use una versión anterior de WinUI).

Los botones de radio funcionan en grupos. Hay dos formas de agrupar los controles de botón de radio:

- Colocarlos dentro del mismo contenedor principal.
- Establecer la propiedad [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) de cada botón de radio en el mismo valor.

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

En la siguiente imagen se muestra cómo se representa este grupo de botones de radio:

![Botones de radio en dos grupos](images/radio-button-groups.png)

## <a name="radio-button-states"></a>Estados de los botones de radio

Un botón de radio tiene dos estados: seleccionado o sin seleccionar. Cuando se selecciona un botón de radio, su propiedad [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) es **true**. Cuando un botón de radio está sin seleccionar, su propiedad **IsChecked** es **false**. Un botón de radio se puede desmarcar al hacer clic en otro botón de radio del mismo grupo, pero no al hacer clic de nuevo en él. No obstante, puede desmarcar un botón de radio mediante programación si establece su propiedad IsChecked en **false**.

## <a name="recommendations"></a>Recomendaciones

- Asegúrate de que esté claro tanto el objetivo como el estado actual de un conjunto de botones de radio.
- Limita el contenido textual del botón de radio a una única línea.
- Si el contenido es dinámico, piensa en cómo se reajustará el botón y qué sucederá con los elementos visuales que lo rodean.
- Use la fuente predeterminada, salvo que se indique lo contrario en las directrices de su marca.
- No coloques dos grupos de botones de radio en paralelo. Cuando se colocan dos grupos de botones de radio juntos, es difícil determinar qué botones pertenecen a uno u otro grupo.

### <a name="visuals-to-consider"></a>Elementos visuales que se deben considerar

En las siguientes imágenes se muestra la mejor manera de organizar los botones de radio en un grupo RadioButton.

![Un grupo de botones de radio.](images/radiobutton-layout.png)

![Directrices de espaciado para botones de radio](images/radiobutton-redline.png)

> [!NOTE]
> Si se usa un control RadioButtons de WinUI, tanto el espaciado como los márgenes y la orientación ya están optimizados.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-topics"></a>Temas relacionados

### <a name="for-designers"></a>Para diseñadores

- [Botones](buttons.md)
- [Modificadores para alternar](toggles.md)
- [Casillas](checkbox.md)
- [Listas y cuadros combinados](lists.md)
- [Controles deslizantes](slider.md)

### <a name="for-developers-xaml"></a>Para desarrolladores (XAML)

- [Clase RadioButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
