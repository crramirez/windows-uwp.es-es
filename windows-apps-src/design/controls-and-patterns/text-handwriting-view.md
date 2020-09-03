---
Description: Personaliza la vista integrada de escritura a mano para entrada de lápiz en texto que sea compatible con controles de texto de Windows tales como TextBox y RichEditBox (y controles, como AutoSuggestBox, que proporcionan una experiencia similar de entrada de texto).
title: Entrada de texto con la vista de escritura a mano
label: Text input with the handwriting view
template: detail.hbs
ms.date: 10/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e7c25a77b552ffc187a4e49a02b7facd771e8258
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175589"
---
# <a name="text-input-with-the-handwriting-view"></a>Entrada de texto con la vista de escritura a mano

![El cuadro de texto se amplía cuando se pulsa con el lápiz](images/handwritingview/handwritingview2.gif)

Personaliza la vista integrada de escritura a mano para entrada de lápiz en texto que sea compatible con controles de texto de Windows tales como [TextBox](/uwp/api/windows.ui.xaml.controls.textbox) y [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox) y controles derivados de estos tales como [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox).

## <a name="overview"></a>Introducción

Los cuadros de entrada de texto XAML cuentan con soporte incrustado de entrada manuscrita con [Windows Ink](../input/pen-and-stylus-interactions.md). Cuando un usuario pulsa en un cuadro de entrada de texto con un lápiz de Windows, el cuadro de texto se transforma en una superficie de escritura a mano, en lugar de abrir un panel de entrada independiente.

El texto se reconoce cuando el usuario escribe en cualquier parte del cuadro de texto y una ventana de candidatos muestra los resultados del reconocimiento. El usuario puede pulsar un resultado para elegirlo o seguir escribiendo para aceptar al candidato propuesto. Los resultados del reconocimiento literal (letra por letra) se incluyen en la ventana de candidatos, por lo que el reconocimiento no se restringe a las palabras de un diccionario. Conforme el usuario escribe, la entrada de texto aceptada se convierte en una fuente de script que se parece mucho a la escritura natural.

> [!NOTE]
> La vista de escritura a mano está habilitada de forma predeterminada, pero puede deshabilitarla por control y volver al panel de entrada de texto en su lugar.

![Cuadro de texto con entrada de lápiz y sugerencias](images/handwritingview/handwritingview-inksuggestion1.gif)

Un usuario puede editar el texto con acciones y gestos estándar, como:

- _strike through_ o _scratch out_: dibujar atravesando una palabra o parte de ella para eliminarla
- _join_: dibujar un arco entre las palabras para eliminar el espacio entre ellas
- _insert_: dibujar un símbolo de intercalación para insertar un espacio
- _overwrite_: escribir sobre texto existente para sustituirlo

![Cuadro de texto con corrección de entrada de lápiz](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>Deshabilitar la vista de escritura a mano

La vista integrada de escritura a mano está habilitada de forma predeterminada.

Es posible que quieras deshabilitar la vista de escritura a mano si ya ofreces una funcionalidad equivalente de entrada de lápiz a texto en la aplicación o tu experiencia de entrada de texto se basa en algún tipo de formato o carácter especial (por ejemplo, un tabulador) no disponible a través de la escritura a mano.

En este ejemplo, se deshabilita la vista de escritura a mano estableciendo la propiedad [IsHandwritingViewEnabled](/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) del control [TextBox](/uwp/api/windows.ui.xaml.controls.textbox) en false. Todos los controles de texto que admiten la vista de escritura a mano admiten una propiedad similar.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Especificar la alineación de la vista de escritura a mano

La vista de escritura a mano se encuentra encima del control de texto subyacente y tiene un tamaño que se adapta a las preferencias de escritura a mano del usuario (consulte **Configuración -> Dispositivos -> Pen & Windows Ink [Lápiz y Windows Ink] -> Escritura a mano -> Size of font when writing directly into text field [Tamaño de la fuente al escribir directamente en campo de texto]** ). La vista también se alinea automáticamente en relación con el control de texto y su ubicación dentro de la aplicación.

La interfaz de usuario de la aplicación no se redistribuye para incluir el control más grande, por lo que el sistema puede causar que la vista obstruya una interfaz de usuario importante.

Aquí se muestra cómo usar la propiedad [PlacementAlignment](/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) del elemento [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview) de un objeto [TextBox](/uwp/api/windows.ui.xaml.controls.textbox) para especificar qué delimitador del control de texto subyacente se usa para alinear la vista de escritura a mano.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView PlacementAlignment="TopLeft"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="disable-auto-completion-candidates"></a>Deshabilitar los candidatos de finalización automática

El elemento emergente de sugerencia de texto está habilitado de forma predeterminada para ofrecer una lista de los mejores candidatos de reconocimiento de entrada de lápiz entre los que el usuario puede seleccionar en caso de que el mejor candidato no sea correcto.

Si tu aplicación ya ofrece una funcionalidad de reconocimiento sólida y personalizada, puedes usar la propiedad [AreCandidatesEnabled](/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) para deshabilitar las sugerencias integradas, tal y como se muestra en el ejemplo siguiente.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView AreCandidatesEnabled="False"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="use-handwriting-font-preferences"></a>Usar las preferencias de fuente de escritura a mano

Un usuario puede elegir entre una colección predefinida de fuentes de escritura a mano que se utiliza al representar texto basado en el reconocimiento de entrada a lápiz (consulta **Configuración -> Dispositivos -> Pen & Windows Ink [Lápiz y Windows Ink] -> Escritura a mano -> Font when using handwriting [Fuente cuando se usa escritura a mano]** ).

> [!NOTE]
> Los usuarios pueden incluso crear una fuente según su propia escritura a mano.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

La aplicación puede acceder a esta opción y usar la fuente seleccionada para el texto reconocido en el control de texto.

En este ejemplo, se escucha el evento [TextChanged](/uwp/api/windows.ui.xaml.controls.textbox.textchanged) de un [TextBox](/uwp/api/windows.ui.xaml.controls.textbox) y se aplica la fuente seleccionada por el usuario si el cambio de texto se originó en el HandwritingView; si no, se aplica una fuente predeterminada.

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>Acceder al control HandwritingView en controles compuestos

Los controles compuestos que usan los controles [TextBox](/uwp/api/windows.ui.xaml.controls.textbox) o [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox), como [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox), también admiten un [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview).

Para acceder al control [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview) de un control compuesto, usa la [VisualTreeHelper](/uwp/api/windows.ui.xaml.media.visualtreehelper) API.

El fragmento de código XAML siguiente muestra un control [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox).

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

En el correspondiente código subyacente, se muestra cómo deshabilitar [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview) en el control compuesto [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox).

1. Primero, controlamos el evento Loaded de la aplicación donde llamamos a una función FindInnerTextBox para iniciar el recorrido del árbol visual.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. Luego se comienza a recorrer en iteración el árbol visual (comenzando en un AutoSuggestBox) de la función FindInnerTextBox con una llamada a FindVisualChildByName.

    ```csharp
    private bool FindInnerTextBox(AutoSuggestBox autoSuggestBox)
    {
        if (autoSuggestInnerTextBox == null)
        {
            // Cache textbox to avoid multiple tree traversals. 
            autoSuggestInnerTextBox = 
                (TextBox)FindVisualChildByName<TextBox>(autoSuggestBox);
        }
        return (autoSuggestInnerTextBox != null);
    }
    ```

3. Por último, esta función recorre en iteración el árbol visual hasta que se recupere el TextBox.

    ```csharp
    private FrameworkElement FindVisualChildByName<T>(DependencyObject obj)
    {
        FrameworkElement element = null;
        int childrenCount = 
            VisualTreeHelper.GetChildrenCount(obj);
        for (int i = 0; (i < childrenCount) && (element == null); i++)
        {
            FrameworkElement child = 
                (FrameworkElement)VisualTreeHelper.GetChild(obj, i);
            if ((child.GetType()).Equals(typeof(T)) || (child.GetType().GetTypeInfo().IsSubclassOf(typeof(T))))
            {
                element = child;
            }
            else
            {
                element = FindVisualChildByName<T>(child);
            }
        }
        return (element);
    }
    ```

## <a name="reposition-the-handwritingview"></a>Cambiar la posición del control HandwritingView

En algunos casos, puede que debas asegurarte de que el [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview) abarque elementos de la interfaz de usuario que, de lo contrario, tal vez no incluiría.

En este caso, se crea un TextBox que admite el dictado (que se implementa colocando un TextBox y un botón de dictado en un StackPanel).

![TextBox con dictado](images/handwritingview/textbox-with-dictation.png)

Como el StackPanel es ahora más grande que el TextBox, es posible que el [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview) no obstruya todo el control compuesto.

![TextBox con dictado](images/handwritingview/textbox-with-dictation-handwritingview.png)

Para solucionar este problema, establece la propiedad PlacementTarget del [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview) en el elemento de la interfaz de usuario con el que se debe alinear.

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" BorderBrush="DarkGray" 
    Height="55" Width="500" Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" BorderThickness="0" 
        FontSize="24" VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView PlacementTarget="{Binding ElementName=DictationBox}"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray"     Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="resize-the-handwritingview"></a>Cambiar el tamaño del control HandwritingView

También puedes establecer el tamaño del [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview), lo que puede resultar útil cuando tengas que asegurarte de que la vista no obstruye una interfaz de usuario importante.

En este caso, se crea un TextBox que admite el dictado (que se implementa colocando un TextBox y un botón de dictado en un StackPanel).

![TextBox con dictado](images/handwritingview/textbox-with-dictation.png)

En este caso, queremos asegurarnos de que el botón de dictado siempre está visible.

![TextBox con dictado](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Para ello, enlazamos la propiedad MaxWidth del [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview) al ancho del elemento de interfaz de usuario que debe obstruir.

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" 
    BorderBrush="DarkGray" 
    Height="55" Width="500" 
    Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" 
        BorderThickness="0" 
        FontSize="24" 
        VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView 
                PlacementTarget="{Binding ElementName=DictationBox}"
                MaxWidth="{Binding ElementName=DictationTextBox, Path=Width"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray" 
        Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="reposition-custom-ui"></a>Cambiar la posición de la interfaz de usuario personalizada

Si tienes una interfaz de usuario personalizada que aparece en respuesta a la entrada de texto, como un elemento emergente informativo, has de cambiar la posición de esa interfaz de usuario para que no obstruya la vista de escritura a mano.

![TextBox con interfaz de usuario personalizada](images/handwritingview/textbox-with-customui.png)

El siguiente ejemplo muestra cómo escuchar los eventos [Opened](/uwp/api/windows.ui.xaml.controls.handwritingview.opened), [Closed](/uwp/api/windows.ui.xaml.controls.handwritingview.closed) y [SizeChanged](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) del [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview) para establecer la posición de un [Popup](/uwp/api/windows.ui.popups).

```csharp
private void Search_HandwritingViewOpened(
    HandwritingView sender, HandwritingPanelOpenedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewClosed(
    HandwritingView sender, HandwritingPanelClosedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewSizeChanged(
    object sender, SizeChangedEventArgs e)
{
    UpdatePopupPositionForHandwritingView();
}

private void UpdatePopupPositionForHandwritingView()
{
if (CustomSuggestionUI.IsOpen)
    CustomSuggestionUI.VerticalOffset = GetPopupVerticalOffset();
}

private double GetPopupVerticalOffset()
{
    if (SearchTextBox.HandwritingView.IsOpen)
        return (SearchTextBox.Margin.Top + SearchTextBox.HandwritingView.ActualHeight);
    else
        return (SearchTextBox.Margin.Top + SearchTextBox.ActualHeight);    
}
```

## <a name="retemplate-the-handwritingview-control"></a>Volver a crear la plantilla del control HandwritingView

Como con todos los controles del marco de trabajo XAML, puedes personalizar la estructura visual y el comportamiento visual de un [HandwritingView](/uwp/api/windows.ui.xaml.controls.handwritingview) para tus requisitos específicos.

Para ver un ejemplo completo de creación de una plantilla personalizada, consulta los procedimientos de [Crear controles de transporte personalizados](./custom-transport-controls.md) o el [ejemplo de control de edición personalizado](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).







