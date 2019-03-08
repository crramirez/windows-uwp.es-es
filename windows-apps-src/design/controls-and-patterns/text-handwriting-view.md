---
Description: Personalizar la vista integrada de escritura a mano para tinta de entrada de texto que sea compatible con los controles de texto UWP, como el cuadro de texto, RichEditBox (y controles, como el AutoSuggestBox que proporcionan una experiencia similar de entrada de texto).
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
ms.openlocfilehash: f7b31898e6a90410e4edc73ee36f71a7e4d94155
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634920"
---
# <a name="text-input-with-the-handwriting-view"></a>Entrada de texto con la vista de escritura a mano

![El cuadro de texto se amplía cuando se pulsa con el lápiz](images/handwritingview/handwritingview2.gif)

Personalizar la vista integrada de escritura a mano para tinta de entrada de texto admitidos por los controles de texto UWP, como el [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox), y los controles derivan de estas, como el [ AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

## <a name="overview"></a>Introducción

Soporte integrado para la entrada con el lápiz de características de cuadros de entrada de texto XAML [Windows Ink](../input/pen-and-stylus-interactions.md). Cuando un usuario pulsa en un cuadro de entrada de texto con un lápiz de Windows, el cuadro de texto se transforma en una superficie de escritura a mano, en lugar de abrir un panel de entrada independiente.

El texto se reconoce como el usuario en cualquier lugar a escribe en el cuadro de texto y un candidato ventana muestra los resultados del reconocimiento. El usuario puede pulsar un resultado para elegirlo, o seguir escribiendo para aceptar al candidato propuesto. Los resultados del reconocimiento literal (letra por letra) se incluyen en la ventana candidata, por lo que el reconocimiento no se restringe a las palabras de un diccionario. Conforme el usuario escribe, la entrada de texto aceptada se convierte en una fuente de script que se parece mucho a la escritura natural.

> [!NOTE]
> La vista de escritura a mano está habilitada de forma predeterminada, pero puede deshabilitarlo en una base por el control y volver al panel de entrada de texto en su lugar.

![Cuadro de texto con la escritura con lápiz y sugerencias](images/handwritingview/handwritingview-inksuggestion1.gif)

Un usuario puede editar el texto con acciones y gestos estándar, como:

- _strike through_ o _scratch out_: dibujar atravesando una palabra o parte de ella para eliminarla
- _join_: dibujar un arco entre las palabras para eliminar el espacio entre ellas
- _insert_: dibujar un símbolo de intercalación para insertar un espacio
- _overwrite_: escribir sobre texto existente para sustituirlo

![Cuadro de texto con corrección de tinta](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>Deshabilitar la vista de escritura a mano

La vista integrada de escritura a mano está habilitada de forma predeterminada.

Es posible que desee deshabilitar la vista de escritura a mano si ya proporcionan una funcionalidad equivalente tinta en texto en la aplicación o su experiencia de entrada de texto se basa en algún tipo de formato ni un carácter especial (por ejemplo, una pestaña) no está disponible a través de la escritura a mano.

En este ejemplo, se deshabilita la vista de escritura a mano estableciendo el [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) propiedad de la [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) control en false. Todos los controles de texto que admiten la vista de escritura a mano admiten una propiedad similar.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Especificar la alineación de la vista de escritura a mano

La vista de escritura a mano está situada sobre el control de texto subyacente y tamaño para dar cabida a la escritura a mano las preferencias de usuario (consulte **configuración -> dispositivos -> lápiz & Windows Ink -> escritura a mano -> tamaño de fuente al escribir directamente en campo de texto**). La vista se alinea automáticamente también en relación con el control de texto y su ubicación dentro de la aplicación.

La interfaz de usuario de la aplicación no reflujo para dar cabida a la mayor control, por lo que el sistema puede provocar que la vista de occlude la interfaz de usuario importante.

Aquí, se muestra cómo usar el [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) propiedad de un [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para especificar qué delimitador en el control de texto subyacente que se usa para alinear el vista de escritura a mano.

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

## <a name="disable-auto-completion-candidates"></a>Deshabilitar a los candidatos de Autocompletar

El elemento emergente sugerencias de texto está habilitado de forma predeterminada para proporcionar una lista de entradas manuscritas superior candidatos reconocimiento desde el que el usuario puede seleccionar en caso de los candidatos principales es incorrecto.

Si la aplicación ya proporciona la funcionalidad de reconocimiento sólidas y personalizadas, puede usar el [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) propiedad para deshabilitar las sugerencias integradas, como se muestra en el ejemplo siguiente.

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

## <a name="use-handwriting-font-preferences"></a>Utilice las preferencias de fuente de escritura a mano

Un usuario puede elegir entre una colección predefinida de fuentes de escritura a mano que se utiliza al representar texto en función de reconocimiento de tinta (consulte **configuración -> dispositivos -> lápiz & Windows Ink -> escritura a mano -> fuente cuando se usa la escritura a mano**).

> [!NOTE]
> Los usuarios incluso pueden crear una fuente según su propia escritura a mano.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

La aplicación puede tener acceso a esta configuración y usar la fuente seleccionada para el texto reconocido en el control de texto.

En este ejemplo, hemos escuchar el [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) eventos de un [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) y aplicar la fuente seleccionada del usuario si se originó el cambio de texto de la HandwritingView (o una fuente predeterminada, si no).

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>Obtener acceso a la HandwritingView en los controles compuestos

Controles compuestos que usan el [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) o [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) controles, como [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) también admiten un [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview).

Para tener acceso a la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) en un control compuesto, utilice el [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API.

El fragmento de código XAML siguiente muestra un [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) control.

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

En el código correspondiente, se muestra cómo deshabilitar la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) en el [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

1. En primer lugar, hemos controlar evento cargado de la aplicación donde se llame a una función FindInnerTextBox para iniciar el recorrido del árbol visual.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. Comenzamos a continuación, recorrer el árbol visual (comenzando por un AutoSuggestBox) en la función FindInnerTextBox con una llamada a FindVisualChildByName.

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

3. Por último, esta función recorre el árbol visual hasta que se recupere el cuadro de texto.

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

## <a name="reposition-the-handwritingview"></a>Cambiar la posición de la HandwritingView

En algunos casos, es posible que deba asegurarse de que el [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) cubre los elementos de interfaz de usuario que en caso contrario, es posible que no.

En este caso, creamos un cuadro de texto que admite el dictado (implementado por colocar un cuadro de texto y un botón de dictado en un elemento StackPanel).

![Cuadro de texto con el dictado](images/handwritingview/textbox-with-dictation.png)

Como StackPanel ahora es mayor que el cuadro de texto, el [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) podría no occlude todos lo compuesto OLE.

![Cuadro de texto con el dictado](images/handwritingview/textbox-with-dictation-handwritingview.png)

Para solucionar este problema, establezca la propiedad PlacementTarget del [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) al elemento de interfaz de usuario a la que se debe alinear.

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

## <a name="resize-the-handwritingview"></a>Cambiar el tamaño de la HandwritingView

También puede establecer el tamaño de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), que puede ser útil cuando deba asegurarse de la vista no occlude la interfaz de usuario importante.

Al igual que el ejemplo anterior, creamos un cuadro de texto que admite el dictado (implementado por colocar un cuadro de texto y un botón de dictado en un elemento StackPanel).

![Cuadro de texto con el dictado](images/handwritingview/textbox-with-dictation.png)

En este caso, queremos asegurarnos de que el botón de dictado siempre está visible.

![Cuadro de texto con el dictado](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Para ello, enlazamos la propiedad MaxWidth de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) el ancho del elemento de interfaz de usuario que debe occlude.

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
                PlacementTarget="{Binding ElementName=DictationBox}“
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

## <a name="reposition-custom-ui"></a>Cambiar la posición de interfaz de usuario personalizada

Si tiene una interfaz de usuario personalizada que aparece en la respuesta a la entrada de texto, como un elemento emergente informativo, necesita cambiar la posición de esa interfaz de usuario para que no occlude la vista de escritura a mano.

![Cuadro de texto con la interfaz de usuario personalizada](images/handwritingview/textbox-with-customui.png)

El ejemplo siguiente muestra cómo realizar escuchas para el [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened), [cerrado](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
), y [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) eventos de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para establecer el posición de un [emergente](https://docs.microsoft.com/uwp/api/windows.ui.popups).

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

## <a name="retemplate-the-handwritingview-control"></a>El control HandwritingView Retemplate

Como con todos los controles de marco de trabajo XAML, puede personalizar la estructura visual y el comportamiento visual de un [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para sus requisitos específicos.

Para ver un ejemplo completo de creación de una plantilla personalizada desprotección la [crear controles de transporte personalizado](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) procedimientos o [ejemplo de Control de edición personalizada](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).








