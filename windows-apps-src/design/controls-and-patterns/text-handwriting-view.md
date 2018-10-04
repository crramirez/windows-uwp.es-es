---
author: Karl-Bridge-Microsoft
Description: Customize the built-in handwriting view for ink to text input that is supported by UWP text controls such as the TextBox, RichEditBox (and controls like the AutoSuggestBox that provide a similar text input experience).
title: Entrada de texto con la vista de escritura a mano
label: Text input with the handwriting view
template: detail.hbs
ms.author: kbridge
ms.date: 09/10/18
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 3aeb400da4b3abe61e086732eaceb0e53fd1b005
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "4352977"
---
# <a name="text-input-with-the-handwriting-view"></a>Entrada de texto con la vista de escritura a mano

![El cuadro de texto se amplía cuando se pulsa con el lápiz](images/pen-input-expand-cropped.gif)

Personalizar la vista integrada de escritura a mano para entrada de lápiz a la entrada de texto que sea compatible con los controles de texto UWP como [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox), [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)y otros controles que proporcionan una experiencia de entrada de texto similares (como [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)).

## <a name="overview"></a>Introducción

Los cuadros de entrada de texto XAML cuentan con soporte incrustado para usar [Windows Ink](../input/pen-and-stylus-interactions.md)una entrada manuscrita. Cuando un usuario pulsa en un cuadro de entrada de texto con un lápiz de Windows, el cuadro de texto se transforma en una superficie de escritura a mano, en lugar de abrir un panel de entrada independiente.

El texto se reconoce cuando el usuario las escrituras en cualquier lugar en el cuadro de texto y un candidato ventana muestra los resultados del reconocimiento. El usuario puede pulsar un resultado para elegirlo, o seguir escribiendo para aceptar al candidato propuesto. Los resultados del reconocimiento literal (letra por letra) se incluyen en la ventana candidata, por lo que el reconocimiento no se restringe a las palabras de un diccionario. Conforme el usuario escribe, la entrada de texto aceptada se convierte en una fuente de script que se parece mucho a la escritura natural.

> [!NOTE]
> La vista de escritura a mano está habilitada de manera predeterminada, pero puedes deshabilitar por el control y revertir al panel de entrada de texto en su lugar.


![Cuadro de texto con entrada manuscrita](images/pen-input-1.png)

Un usuario puede editar el texto con acciones y gestos estándar, como:

- _strike through_ o _scratch out_: dibujar atravesando una palabra o parte de ella para eliminarla
- _join_: dibujar un arco entre las palabras para eliminar el espacio entre ellas
- _insert_: dibujar un símbolo de intercalación para insertar un espacio
- _overwrite_: escribir sobre texto existente para sustituirlo

![Sobrescribir una entrada manuscrita](images/pen-input-2.png)

## <a name="disable-the-handwriting-view"></a>Deshabilitar la vista de escritura a mano

La vista integrada de escritura a mano está habilitada de manera predeterminada.

Es posible que quieras deshabilitar la vista de escritura a mano si ya se ha proporcionado una funcionalidad de entrada de lápiz en texto equivalente en la aplicación, o tu experiencia de entrada de texto se basa en algún tipo de formato ni un carácter especial (como una pestaña) no están disponible a través de escritura a mano.

En este ejemplo, se deshabilita la vista de escritura a mano estableciendo la propiedad [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) del control [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) en false. Todos los controles de texto que admiten la vista de escritura a mano admiten una propiedad similar.

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>Especifica la alineación de la vista de escritura a mano

La vista de escritura a mano se encuentra encima del control de texto subyacente y un tamaño para dar cabida a las preferencias del usuario de escritura a mano (consulta **Configuración -> dispositivos -> lápiz y entrada de lápiz de Windows -> escritura a mano -> tamaño de fuente al escribir directamente en el campo de texto **). La vista también automáticamente se alinea en relación con el control de texto y su ubicación dentro de la aplicación.

La interfaz de usuario de la aplicación no se redistribuye para acomodar el control más grande, por lo que el sistema podría hacer que la vista tapar la interfaz de usuario importante.

Aquí te mostramos cómo usar la propiedad [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) de un [cuadro de texto](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para especificar qué anclaje en el control de texto subyacente se utiliza para alinear la vista de escritura a mano.

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

## <a name="disable-auto-completion-candidates"></a>Deshabilitar a los candidatos de finalización automática

El control emergente sugerencias de texto está habilitado de manera predeterminada para proporcionar una lista de entrada de lápiz superior candidatos de reconocimiento desde el que el usuario puede seleccionar en caso de que el candidato superior es incorrecto.

Si la aplicación ya proporciona sólida, funcionalidad de reconocimiento personalizadas, puedes usar la propiedad [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) para deshabilitar las sugerencias integradas, como se muestra en el siguiente ejemplo.

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

Un usuario puede elegir entre una colección predefinida de fuentes basadas en escritura a mano para usar cuando texto de representación en función de reconocimiento de entrada de lápiz (consulta **Configuración -> dispositivos -> lápiz y entrada de lápiz de Windows -> escritura a mano -> fuente al usar la escritura a mano**).

> [!NOTE]
> Los usuarios incluso pueden crear una fuente en función de su propia escritura a mano.
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

La aplicación puede acceder a esta configuración y usar la fuente seleccionada para el texto reconocido en el control de texto.

En este ejemplo, hemos escuchar el evento [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) de un [cuadro de texto](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) y Aplicar fuente seleccionada del usuario si el cambio de texto se originó desde el HandwritingView (o una fuente predeterminada, si no es así).

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>Obtener acceso a la HandwritingView en controles compuestos

Los controles compuestos que usan los controles [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) o [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) , como [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) también admiten una [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview).

Para acceder a la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) en un control compuesto, usa el [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API.

El siguiente fragmento XAML muestra un control [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) .

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

En el correspondiente código subyacente, te mostramos cómo deshabilitar el [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) en [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox).

1. En primer lugar, controlamos evento Loaded de la aplicación donde llamamos a una función FindInnerTextBox para iniciar el cruce seguro de árbol visual.

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. A continuación, comenzamos recorrer el árbol visual (a partir de un AutoSuggestBox) en la función FindInnerTextBox con una llamada a FindVisualChildByName.

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

3. Por último, esta función recorre el árbol visual hasta que se recupera el cuadro de texto.

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

En algunos casos, es posible que debes asegurarte de que el [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) cubre los elementos de la interfaz de usuario que lo contrario, podría no ser.

A continuación, se crea un cuadro de texto que admita dictado (que se implementa mediante la colocación de un cuadro de texto y un botón de dictado en un elemento StackPanel).

![Cuadro de texto con el dictado](images/handwritingview/textbox-with-dictation.png)

Como el elemento StackPanel ahora es mayor que el cuadro de texto, el [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) no podría tapar todos lo OLE compuesto.

![Cuadro de texto con el dictado](images/handwritingview/textbox-with-dictation-handwritingview.png)

Para solucionar esto, Establece la propiedad PlacementTarget de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) al elemento de interfaz de usuario a la que debe estar alineada.

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

También puedes establecer el tamaño de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview), lo que puede ser útil cuando, debes asegurarte de que la vista no tapar la interfaz de usuario importante.

Al igual que el ejemplo anterior, creamos un cuadro de texto que admita dictado (que se implementa mediante la colocación de un cuadro de texto y un botón de dictado en un elemento StackPanel).

![Cuadro de texto con el dictado](images/handwritingview/textbox-with-dictation.png)

En este caso, queremos garantizar que el botón de dictado siempre está visible.

![Cuadro de texto con el dictado](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

Para ello, enlazamos la propiedad MaxWidth de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) con el ancho del elemento de interfaz de usuario que debe tapar.

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

Si tienes una interfaz de usuario personalizada que aparece en respuesta a la entrada de texto, por ejemplo, un elemento emergente informativo, debes cambiar la posición de esa interfaz de usuario para que no tapar la vista de escritura a mano.

![Cuadro de texto con la interfaz de usuario personalizada](images/handwritingview/textbox-with-customui.png)

En el ejemplo siguiente se muestra cómo escuchar los eventos [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened)y [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
)con [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) de la [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para establecer la posición de un [elemento emergente](https://docs.microsoft.com/uwp/api/windows.ui.popups).

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

## <a name="retemplate-the-handwritingview-control"></a>Volver el control de HandwritingView

Al igual que con todos los controles de marco XAML, puedes personalizar la estructura visual y el comportamiento visual de un [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) para tus requisitos específicos.

Para ver un ejemplo completo de creación de una plantilla personalizada desproteger los procedimientos de [crear controles de transporte personalizados](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls) o la [muestra de Control de edición personalizado](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).







