---
description: Obtenga información sobre cómo usar los botones de radio para permitir que los usuarios seleccionen una opción de una colección de dos o más opciones mutuamente excluyentes, aunque relacionadas.
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
ms.openlocfilehash: ee933bd28594263e61e654b14b0541c6fa9ed41b
ms.sourcegitcommit: 875bd348608547e7a66fa4b460efe64b3246807e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2020
ms.locfileid: "90080847"
---
# <a name="radio-buttons"></a>Botones de radio

Los botones de radio, también llamados botones de opción, permiten que los usuarios seleccionen una opción entre una colección de dos o más opciones mutuamente excluyentes, aunque relacionadas. Los botones de radio siempre se usan en grupos y cada opción se representa mediante un botón de radio del grupo.

De manera predeterminada, no está seleccionado ningún botón de radio en un grupo RadioButtons. Es decir, todos los botones de radio están desactivados. Sin embargo, una vez que un usuario ha seleccionado un botón de radio, el usuario no puede anular su selección para restaurar el grupo a su estado original desactivado.

El comportamiento único de un grupo RadioButtons lo distingue de las [casillas](checkbox.md), que admiten la selección múltiple y la anulación de la selección, o la desactivación.

:::image type="content" source="images/controls/radio-button.png" alt-text="Ejemplo de un grupo RadioButtons con un botón de radio seleccionado":::

**Obtención de la biblioteca de la interfaz de usuario de Windows**

| &nbsp; | &nbsp; |
| - | - |
| ![Logotipo de WinUI](images/winui-logo-64x64.png) | El control RadioButtons se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y las características de interfaz de usuario para las aplicaciones de Windows. Para obtener más información e instrucciones sobre la instalación, consulta el artículo [Windows UI Library](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows). |

> **API de la biblioteca de interfaz de usuario de Windows**: [Clase RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons), [propiedad SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [propiedad SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) y [evento SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
>
> **API de plataforma**: [Clase RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [Evento IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) y [evento Checked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)

> [!IMPORTANT]
> **RadioButtons frente a RadioButton**
>
>Existen dos maneras de crear grupos de botones de radio.
>
>- A partir de la versión WinUI 2.3, se recomienda el control **[RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)** . Este control simplifica el diseño, controla la navegación y la accesibilidad del teclado, y admite el enlace a un origen de datos.
>- Puede usar grupos de controles **[RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)** individuales. Si la aplicación no usa WinUI 2.3 o una versión posterior, esta es la única opción.

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Use botones de radio para permitir que los usuarios seleccionen entre dos o más opciones mutuamente excluyentes.

:::image type="content" source="images/radiobutton_basic.png" alt-text="Grupo RadioButtons con un botón de radio seleccionado":::

Use botones de radio cuando los usuarios necesiten ver todas las opciones antes de realizar una selección. Los botones de radio enfatizan todas las opciones por igual, lo que significa que algunas opciones pueden atraer más atención de la necesaria o deseada.

Salvo que todas las opciones se merezcan la misma atención, debería plantearse usar otros controles. Por ejemplo, para recomendar una única mejor opción para la mayoría de los usuarios y en la mayoría de las situaciones, use un [cuadro combinado](combo-box.md) para mostrar la mejor opción como opción predeterminada.

:::image type="content" source="images/combo_box_collapsed.png" alt-text="Un cuadro combinado, que muestra una opción predeterminada":::

Si solo hay dos opciones posibles que se pueden expresar claramente como una sola opción binaria, como on/off o yes/no, combínelas en una sola [casilla](checkbox.md) o un control [conmutador de alternancia](toggles.md). Por ejemplo, use una única casilla para "Acepto", en lugar de dos botones de radio para "Acepto" y "No acepto".

No use dos botones de opción para una única opción binaria:

:::image type="content" source="images/radiobutton-vs-checkbox-rb.png" alt-text="Dos botones de radio que presentan una selección binaria":::

En su lugar, utilice una casilla:

:::image type="content" source="images/radiobutton-vs-checkbox-cb.png" alt-text="Una casilla es una buena alternativa para presentar una elección entre dos opciones":::

Use [casillas](checkbox.md) cuando el usuario pueda seleccionar varias opciones.

:::image type="content" source="images/checkbox2.png" alt-text="Casillas que admiten la selección múltiple":::

Cuando las opciones de los usuarios se encuentran dentro de un intervalo de valores (por ejemplo, *10, 20, 30... 100*), use un [control deslizante](slider.md).

:::image type="content" source="images/controls/slider.png" alt-text="Control deslizante que muestra un valor en un intervalo de valores":::

Si hay más de ocho opciones, utilice un [cuadro combinado](combo-box.md).

:::image type="content" source="images/combo_box_scroll.png" alt-text="Cuadro de lista que muestra varias opciones":::

Si las opciones disponibles se basan en el contexto actual de la aplicación o pueden variar dinámicamente, use un control de lista.

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

## <a name="winui-radiobuttons-overview"></a>Información general sobre el control RadioButtons de WinUI

El acceso de teclado y el comportamiento de la navegación se han optimizado en el control [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons). Estas mejoras ayudan a los usuarios avanzados de accesibilidad y de teclado a desplazarse por la lista de opciones de manera más rápida y sencilla.

Además de estas mejoras, el diseño visual predeterminado de los botones de radio individuales de un grupo RadioButtons se ha optimizado a través de la configuración automatizada de la orientación, el espaciado y los márgenes. Esta optimización elimina la necesidad de especificar estas propiedades, como quizá deba hacer cuando use un control de agrupación más primitivo, como [StackPanel](../layout/layout-panels.md#stackpanel) o [Grid](../layout/layout-panels.md#grid).

### <a name="navigating-a-radiobuttons-group"></a>Navegación por un grupo RadioButtons

El control `RadioButtons` tiene un comportamiento de navegación especial que ayuda a los usuarios de teclado a navegar por la lista de manera más rápida y sencilla.

#### <a name="keyboard-focus"></a>Foco de teclado

El control `RadioButtons` admite dos estados:

- No se ha seleccionado ningún botón de radio.
- Se ha seleccionado un botón de radio.

En las secciones siguientes se describe el comportamiento del foco en cada estado.

##### <a name="no-radio-button-is-selected"></a>No se ha seleccionado ningún botón de radio.

Cuando no se selecciona ningún botón de radio, el primero de la lista obtiene el foco.

> [!NOTE]
> El elemento que recibe el foco de tabulación en la navegación por tabulación inicial no está seleccionado.

:::row:::
   :::column span="":::
     **_Lista sin el foco de tabulación, sin ninguna selección_**

     ![Lista sin el foco de tabulación y sin ningún elemento seleccionado](images/radiobutton-no-selected-item-no-tab-focus.png)
   :::column-end:::
   :::column span="":::
      **_Lista con el foco de tabulación inicial, sin ninguna selección_**

      ![Lista con el foco de tabulación inicial y sin ningún elemento seleccionado](images/radiobutton-no-selected-item-tab-focus.png)
   :::column-end:::
:::row-end:::

##### <a name="one-radio-button-is-selected"></a>Se ha seleccionado un botón de radio.

Cuando un usuario usa la tabulación en la lista y está seleccionado un botón de radio, dicho botón obtiene el foco.

:::row:::
   :::column span="":::
     **_Lista sin foco de tabulación_**

     ![Lista sin el foco de tabulación y un elemento seleccionado](images/radiobutton-selected-item-no-tab-focus.png)
   :::column-end:::
   :::column span="":::
      **_Lista con el foco de tabulación inicial_**

      ![Lista con el foco de tabulación inicial y un elemento seleccionado](images/radiobutton-selected-item-tab-focus.png)
   :::column-end:::
:::row-end:::

#### <a name="keyboard-navigation"></a>Navegación mediante teclado

Para más información sobre los comportamientos generales de la navegación con el teclado, consulte [Interacciones de teclado: navegación](../input/keyboard-interactions.md#navigation).

Cuando un elemento de un grupo `RadioButtons` ya tiene el foco, el usuario puede utilizar las teclas de dirección para la "navegación interna" entre los elementos del grupo. Las teclas de flecha arriba y abajo se mueven al elemento lógico "siguiente" o "anterior", tal y como se define en el marcado XAML. Las teclas de flecha izquierda y derecha se mueven espacialmente.

##### <a name="navigation-within-single-column-or-single-row-layouts"></a>Disposiciones de navegación de una sola columna o fila

En una disposición de navegación de una sola columna o fila, la navegación con el teclado tiene el siguiente comportamiento:

:::row:::
   :::column span="":::
     **_Columna única_**

     ![Ejemplo de navegación con el teclado en un grupo RadioButtons de una sola columna](images/radiobutton-keyboard-navigation-single-column.png)

     Las teclas de flecha arriba y flecha abajo se mueven entre los elementos.</br>Las teclas de flecha izquierda y flecha derecha no hacen nada.
   :::column-end:::
   :::column span="":::
      **_Fila única_**

      ![Ejemplo de navegación con el teclado en un grupo RadioButtons de una sola fila](images/radiobutton-keyboard-navigation-single-row.png)

      Las teclas de flecha izquierda y arriba se mueven al elemento anterior y las teclas de flecha derecha y abajo se mueven al elemento siguiente.
   :::column-end:::
:::row-end:::

##### <a name="navigation-within-multi-column-multi-row-layouts"></a>Disposiciones de navegación en varias columnas o filas

En una disposición de cuadrícula de varias columnas y varias filas, la navegación con el teclado produce este comportamiento:

**_Teclas de flecha izquierda y flecha derecha_**

:::row:::
   :::column span="":::
      ![Ejemplo de navegación horizontal con el teclado en un grupo RadioButtons de varias columnas o filas](images/radiobutton-keyboard-navigation-multi-column-row-1.png)

      

      Las teclas de flecha izquierda y flecha derecha mueven el foco horizontalmente entre los elementos de una fila.
   :::column-end:::
   :::column span="":::
     ![Ejemplo de navegación horizontal mediante el teclado con el foco en el último elemento de una columna](images/radiobutton-keyboard-navigation-multi-column-row-3.png)

      Cuando el foco se encuentra en el último elemento de una columna y se presiona la tecla de flecha derecha o izquierda, el foco se mueve al último elemento de la columna siguiente o anterior (si existe).
   :::column-end:::
:::row-end:::

**_Teclas de flecha arriba y flecha abajo_**

:::row:::
   :::column span="":::
      ![Ejemplo de navegación vertical con el teclado en un grupo RadioButtons de varias columnas o filas](images/radiobutton-keyboard-navigation-multi-column-row-2.png)

      Las teclas de flecha arriba y flecha abajo mueven el foco verticalmente entre los elementos de una columna.
   :::column-end:::
   :::column span="":::
     ![Ejemplo de navegación vertical mediante el teclado con el foco en el último elemento de una columna](images/radiobutton-keyboard-navigation-multi-column-row-4.png)

      Cuando el foco se encuentra en el último elemento de una columna y se presiona la tecla de flecha abajo, el foco se mueve al primer elemento de la columna siguiente (si existe). Cuando el foco se encuentra en el primer elemento de una columna y se presiona la tecla de flecha arriba, el foco se mueve al último elemento de la columna anterior (si existe).
   :::column-end:::
:::row-end:::

Para más información, consulte [Interacciones de teclado](../input/keyboard-interactions.md#wrapping-homogeneous-list-and-grid-view-items).

###### <a name="wrapping"></a>Ajuste

El grupo RadioButtons no ajusta el foco de la primera fila o columna a la última, ni de la última fila o columna a la primera. Esto se debe a que, cuando los usuarios usan un lector de pantalla, se pierde la sensación de que haya límites y la indicación clara de dónde está el principio y el fin, lo que dificulta que los usuarios con discapacidad visual naveguen por la lista.

El control `RadioButtons` tampoco es compatible con la enumeración, porque está diseñado para contener un número razonable de elementos (consulte [¿Es este el control adecuado?](#is-this-the-right-control)).

### <a name="selection-follows-focus"></a>La selección sigue el foco

Cuando los usuarios usan el teclado para navegar entre los elementos de una lista `RadioButtons`, cuando el foco se mueve de un elemento al siguiente, el elemento recién enfocado se selecciona y el elemento que anteriormente tenía el foco se desactiva.

:::row:::
   :::column span="":::
      **_Antes de la navegación con el teclado_**

      ![Ejemplo de foco y selección antes de la navegación con el teclado](images/radiobutton-two-selected-before-keyboard-navigation.png)

      Foco y selección antes de la navegación con el teclado.
   :::column-end:::
   :::column span="":::
     **_Después de la navegación con el teclado_**

      ![Ejemplo de foco y selección después de la navegación con el teclado](images/radiobutton-three-selected-after-keyboard-navigation.png)

      Foco y selección después de la navegación con el teclado, en el que la tecla de flecha abajo mueve el foco al botón de radio 3, lo selecciona y desactiva el botón de radio 2.
   :::column-end:::
:::row-end:::

Puede desplazar el foco sin cambiar la selección mediante CTRL + teclas de dirección para navegar. Después de mover el foco, puede usar la barra espaciadora para seleccionar el elemento que tiene el foco actualmente.

#### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Navegación con el controlador para juegos y el control remoto de Xbox

Si usa un controlador para juegos o un control remoto de Xbox para moverse entre los botones de radio, el comportamiento "la selección sigue el foco" está deshabilitado y el usuario debe presionar el botón "A" para seleccionar el botón de radio que tiene el foco actualmente.

## <a name="accessibility-behavior"></a>Comportamiento de accesibilidad

En la tabla siguiente se describe cómo el Narrador controla un grupo `RadioButtons` y lo que se anuncia. Este comportamiento depende de la forma en que un usuario ha establecido las preferencias de detalle del Narrador.

| Acción | Anuncio del Narrador |
|:--|:--|
| El foco se mueve a un elemento seleccionado | "Control RadioButton _name_ seleccionado; _x_ de _N_" |
|El foco se mueve a un elemento sin seleccionar<br> *(Si navega con CTRL y las teclas de dirección o con el controlador para juegos de Xbox,<br>lo que indica que la selección no siguen el foco).* | "Control RadioButton _name_ sin seleccionar; _x_ de _N_"  |

> [!NOTE]
> El _**nombre**_  que el Narrador anuncia para cada elemento es el valor de la propiedad adjunta [AutomationProperties.Name](/uwp/api/windows.ui.xaml.automation.automationproperties.nameproperty) si está disponible para el elemento; de lo contrario, es el valor devuelto por el método [ToString](/dotnet/api/system.object.tostring?view=dotnet-uwp-10.0) del elemento.
>
> _**x**_ es el número del elemento actual. _**N**_ es la cantidad total de elementos del grupo.

## <a name="create-a-winui-radiobuttons-group"></a>Creación de un grupo RadioButtons de WinUI

El control `RadioButtons` usa un modelo de contenido similar a un control [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol). Esto significa que puede hacer lo siguiente:

- Para rellenarlo, agregue elementos directamente a la colección [Items](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.items) o enlace datos a su propiedad [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource).
- Use las propiedades [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) o [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) para obtener y establecer la opción seleccionada.
- Controle el evento [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) para que se realice una acción cuando se elija una opción.

> [!TIP]
> En este documento, se usa el alias **muxc** en XAML para representar las API de la biblioteca de interfaz de usuario de Windows que hemos incluido en nuestro proyecto. Hemos agregado lo siguiente a nuestro elemento [Page](/uwp/api/windows.ui.xaml.controls.page): `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>En el código subyacente, se usa el alias **muxc** en C# para representar las API de la biblioteca de interfaz de usuario de Windows que hemos incluido en nuestro proyecto. Hemos agregado esta instrucción **using** en la parte superior del archivo: `using muxc = Microsoft.UI.Xaml.Controls;`

Aquí se declara un control `RadioButtons` simple con tres opciones. La propiedad [Header](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.header) está establecida para asignar al grupo una etiqueta y la propiedad `SelectedIndex` está establecida para proporcionar una opción predeterminada.

```xaml
<muxc:RadioButtons Header="Background color"
                   SelectedIndex="0"
                   SelectionChanged="BackgroundColor_SelectionChanged">
    <x:String>Red</x:String>
    <x:String>Green</x:String>
    <x:String>Blue</x:String>
</muxc:RadioButtons>
```

El resultado tiene el aspecto siguiente:

:::image type="content" source="images/radiobuttons-default-group.png" alt-text="Un grupo de tres botones de radio":::

Para realizar una acción cuando el usuario selecciona una opción, controle el evento [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged). Aquí, cambia el color de fondo de un elemento [Border](/uwp/api/windows.ui.xaml.controls.border) denominado "ExampleBorder" (`<Border x:Name="ExampleBorder" Width="100" Height="100"/>`).

```csharp
private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ExampleBorder != null && sender is muxc.RadioButtons rb)
    {
        string colorName = rb.SelectedItem as string;
        switch (colorName)
        {
            case "Red":
                ExampleBorder.Background = new SolidColorBrush(Colors.Red);
                break;
            case "Green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
        }
    }
}
```

> [!TIP]
> También puede obtener el elemento seleccionado de la propiedad [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems). Solo habrá un elemento seleccionado, en el índice 0, por lo que podría obtener el elemento seleccionado de la siguiente forma: `string colorName = e.AddedItems[0] as string;`.

### <a name="selection-states"></a>Estados de selección

Un botón de radio tiene dos estados: seleccionado o desactivado. Si se selecciona una opción en un grupo `RadioButtons`, puede obtener su valor de la propiedad [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) y su ubicación en la colección de la propiedad [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex). Un botón de radio se puede desactivar si un usuario selecciona otro botón de radio del mismo grupo, pero no se puede desactivar si el usuario lo vuelve a seleccionar. Sin embargo, para desactivar un grupo de botones de radio mediante programación, puede establecer en `SelectedItem = null` o `SelectedIndex = -1`. (Si se establece `SelectedIndex` en cualquier valor fuera del intervalo de la colección `Items`, no se produce ninguna selección).

### <a name="radiobuttons-content"></a>Contenido de RadioButtons

En el ejemplo anterior, rellenó el control `RadioButtons` con cadenas simples. El control proporcionó los botones de radio y usó las cadenas como etiqueta para cada uno de estos.

Sin embargo, puede rellenar el control `RadioButtons` con cualquier objeto. Normalmente, el objetivo es que el objeto proporcione una representación de cadena que se pueda usar como etiqueta de texto. En algunos casos, puede resultar adecuada una imagen en lugar de texto.

Aquí, se utilizan elementos [SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon) para rellenar el control.

```xaml
<muxc:RadioButtons Header="Choose the icon without an arrow">
    <SymbolIcon Symbol="Back"/>
    <SymbolIcon Symbol="Attach"/>
    <SymbolIcon Symbol="HangUp"/>
    <SymbolIcon Symbol="FullScreen"/>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-symbolicon.png" alt-text="Botones de radio de un grupo con iconos de símbolos":::

También puede usar controles [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) individuales para rellenar los elementos `RadioButtons`. Este es un caso especial que se tratará más adelante. Consulte [Controles RadioButton de un grupo RadioButtons]().

Una ventaja de poder usar cualquier objeto es que puede enlazar el control `RadioButtons` a un tipo personalizado del modelo de datos. Esto se demuestra en la siguiente sección.

### <a name="data-binding"></a>Enlace de datos

El control `RadioButtons` admite el enlace de datos a su propiedad [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource). En este ejemplo se muestra cómo puede enlazar el control a un origen de datos personalizado. La apariencia y la funcionalidad de este ejemplo son las mismas que en el ejemplo de color de fondo anterior, pero en este caso los pinceles de color se almacenan en el modelo de datos en lugar de crearse en el controlador de eventos `SelectionChanged`.

```xaml
 <muxc:RadioButtons Header="Background color"
                    SelectedIndex="0"
                    SelectionChanged="BackgroundColor_SelectionChanged"
                    ItemsSource="{x:Bind colorOptionItems}"/>
```

```c#
public sealed partial class MainPage : Page
{
    // Custom data item.
    public class ColorOptionDataModel
    {
        public string Label { get; set; }
        public SolidColorBrush ColorBrush { get; set; }

        public override string ToString()
        {
            return Label;
        }
    }

    List<ColorOptionDataModel> colorOptionItems;

    public MainPage1()
    {
        this.InitializeComponent();

        colorOptionItems = new List<ColorOptionDataModel>();
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Red", ColorBrush = new SolidColorBrush(Colors.Red) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Green", ColorBrush = new SolidColorBrush(Colors.Green) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Blue", ColorBrush = new SolidColorBrush(Colors.Blue) });
    }

    private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        var option = e.AddedItems[0] as ColorOptionDataModel;
        ExampleBorder.Background = option?.ColorBrush;
    }
}
```

### <a name="radiobutton-controls-in-a-radiobuttons-group"></a>Controles RadioButton de un grupo RadioButtons

Puede usar controles [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) individuales para rellenar los elementos `RadioButtons`. Puede hacerlo para obtener acceso a determinadas propiedades, como `AutomationProperties.Name`; o bien, puede que ya tenga código `RadioButton`, pero quiera utilizar el diseño y la navegación de `RadioButtons`.

```xaml
<muxc:RadioButtons Header="Background color">
    <RadioButton Content="Red" Tag="red" AutomationProperties.Name="red"/>
    <RadioButton Content="Green" Tag="green" AutomationProperties.Name="green"/>
    <RadioButton Content="Blue" Tag="blue" AutomationProperties.Name="blue"/>
</muxc:RadioButtons>
```

Cuando usa los controles `RadioButton` de un grupo `RadioButtons`, el control `RadioButtons` sabe cómo presentar el elemento `RadioButton`, para no terminar con dos círculos de selección.

Sin embargo, existen algunos comportamientos que debe tener en cuenta. Se recomienda controlar el estado y los eventos en los controles individuales o en `RadioButtons`, pero no en ambos, para evitar conflictos.

En esta tabla se muestran los eventos y las propiedades relacionados en ambos controles.

|RadioButton  |RadioButtons  |
|---------|---------|
|[Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked), [Unchecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked), [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) |    [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) |
|[IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)  | [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) |

Si controla eventos en un `RadioButton`individual, como `Checked` o `Unchecked`, y también controla el evento `RadioButtons.SelectionChanged`, se activarán ambos eventos. El evento `RadioButton` se produce primero y, a continuación, se produce el evento `RadioButtons.SelectionChanged`, lo que podría dar lugar a conflictos.

Las propiedades `IsChecked`, `SelectedItem` y `SelectedIndex` permanecen sincronizadas. Un cambio en una propiedad actualiza las otras dos.

La propiedad [RadioButton.GroupName](/uwp/api/windows.ui.xaml.controls.radiobutton.groupname) se ignora. El control `RadioButtons` crea el grupo.

### <a name="defining-multiple-columns"></a>Definición de varias columnas

De forma predeterminada, el control `RadioButtons` organiza verticalmente sus botones de radio en una sola columna. Puede establecer la propiedad [MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns) para que el control organice los botones de radio en varias columnas. (Al hacer esto, se colocan en orden de columna principal, donde los elementos se rellenan de arriba a abajo y de izquierda a derecha).

```xaml
<muxc:RadioButtons Header="Options" MaxColumns="3">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
    <x:String>Item 6</x:String>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-multi-column.png" alt-text="Botones de radio en grupos de dos o tres columnas":::

> [!TIP]
> Para que los elementos se organicen en una sola fila horizontal, establezca un valor de `MaxColumns` igual al número de elementos del grupo.

## <a name="create-your-own-radiobutton-group"></a>Creación de su propio grupo RadioButton

> [!Important]
> A menos que use una versión anterior de WinUI, se recomienda usar el control `RadioButtons` de WinUI para agrupar los elementos `RadioButton`.

Los botones de radio funcionan en grupos. Puede agrupar controles [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) individuales de una de estas dos maneras:

- Colocarlos dentro del mismo contenedor principal.
- Establece la propiedad [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) de cada botón de radio en el mismo valor.

En este ejemplo, el primer grupo de botones de radio se agrupará implícitamente al estar en el mismo panel de pila. El segundo grupo se divide entre dos paneles de pila, por lo que `GroupName` se usa para agruparlos explícitamente en un solo grupo.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 1 - implicit grouping -->
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="white" Checked="BGRadioButton_Checked"
                         IsChecked="True"/>
        </StackPanel>
    </StackPanel>

    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 2 - grouped by GroupName -->
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" Tag="green" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" Tag="yellow" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" Tag="blue" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" Tag="white"  GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="ExampleBorder"
            BorderBrush="#FFFFD700" Background="#FFFFFFFF"
            BorderThickness="10" Height="50" Margin="0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "white":
                ExampleBorder.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "green":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "blue":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "white":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

Estos dos grupos de controles `RadioButton` tienen el siguiente aspecto:

:::image type="content" source="images/radio-button-groups.png" alt-text="Botones de radio en dos grupos":::

### <a name="radio-button-states"></a>Estados de los botones de radio

Un botón de radio tiene dos estados: seleccionado o desactivado. Cuando se selecciona un botón de radio, su propiedad [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) es `true`. Cuando se desactiva un botón de radio, su propiedad `IsChecked` es `false`. Un botón de radio se puede desactivar si un usuario selecciona otro botón de radio del mismo grupo, pero no se puede desactivar si el usuario lo vuelve a seleccionar. Sin embargo, para desactivar un botón de radio mediante programación puede establecer su propiedad `IsChecked` en `false`.

### <a name="visuals-to-consider"></a>Elementos visuales que se deben considerar

El espaciado predeterminado de los controles `RadioButton` individuales es diferente del espaciado proporcionado por un grupo `RadioButtons`. Para aplicar el espaciado `RadioButtons` a controles `RadioButton` individuales, use un valor de `Margin` de `0,0,7,3`, como se muestra aquí.

```xaml
<StackPanel>
    <StackPanel.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="Margin" Value="0,0,7,3"/>
        </Style>
    </StackPanel.Resources>
    <TextBlock Text="Background"/>
    <RadioButton Content="Item 1"/>
    <RadioButton Content="Item 2"/>
    <RadioButton Content="Item 3"/>
</StackPanel>
```

Las siguientes imágenes muestran el espaciado preferido de los botones de radio de un grupo.

:::image type="content" source="images/radiobutton-layout.png" alt-text="Imagen que muestra un conjunto de botones de radio, organizados verticalmente":::

:::image type="content" source="images/radiobutton-redline.png" alt-text="Imagen que muestra directrices de espaciado para botones de radio":::

> [!NOTE]
> Si usa un control RadioButtons de WinUI, tanto el espaciado como los márgenes y la orientación ya están optimizados.

## <a name="recommendations"></a>Recomendaciones

- Asegúrese de que tanto el objetivo como el estado actual de un conjunto de botones de radio sean explícitos.
- Limite la etiqueta de texto del botón de radio a una única línea.
- Si la etiqueta de texto es dinámica, piense en cómo cambiará el tamaño del botón automáticamente y qué sucederá con los elementos visuales que la rodean.
- Use la fuente predeterminada, salvo que se indique lo contrario en las directrices de su marca.
- No coloque dos grupos RadioButtons de lado. Cuando se colocan dos grupos RadioButtons uno al lado del otro, resulta difícil para los usuarios determinar qué botones pertenecen a uno u otro grupo.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- Para obtener todos los controles XAML en un formato interactivo, consulte [Ejemplo de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="related-topics"></a>Temas relacionados

- [Botones](buttons.md)
- [Modificadores para alternar](toggles.md)
- [Casillas](checkbox.md)
- [Listas y cuadros combinados](lists.md)
- [Controles deslizantes](slider.md)
- [Clase RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
- [Clase RadioButton](/uwp/api/windows.ui.xaml.controls.radiobutton)
