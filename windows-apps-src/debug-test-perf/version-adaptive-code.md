---
title: Código adaptativo para versiones
description: Usa la clase ApiInformation para aprovechar las nuevas API mientras mantienes la compatibilidad con versiones anteriores.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3293e91e-6888-4cc3-bad3-61e5a7a7ab4e
ms.localizationpriority: medium
ms.openlocfilehash: d62ce9abd84a0769a2393db169b8198d3d9f6cec
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8194318"
---
# <a name="version-adaptive-code"></a>Código adaptativo para versiones

Puedes considerar la escritura de código adaptativo algo similar a [crear una interfaz de usuario adaptativa](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml). Podrías diseñar la interfaz de usuario base para que se ejecute en la pantalla más pequeña y luego mover o agregar elementos cuando detectes que la aplicación se ejecuta en una pantalla más grande. Con el código adaptativo, se escribe el código base para que se ejecute en la versión más baja del sistema operativo y se pueden agregar características seleccionadas especialmente cuando se detecte que la aplicación se ejecuta en una versión superior en la que haya una nueva característica disponible.

Para obtener más información general acerca de ApiInformation, contratos de API y la configuración de Visual Studio, consulta [Aplicaciones adaptables para versiones](version-adaptive-apps.md).

### <a name="runtime-api-checks"></a>Comprobaciones de API en tiempo de ejecución

Se usa la clase [Windows.Foundation.Metadata.ApiInformation](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx) en una condición del código para probar la presencia de la API que se desee llamar. Esta condición se evaluará donde se ejecute la aplicación, pero solo se evaluará como **true** en los dispositivos en los que la API esté presente y en los que, por lo tanto, se encuentre disponible para llamarla. Esto permite escribir código adaptativo para versiones con el fin de crear aplicaciones que usen las API que solo estén disponibles en algunas versiones del sistema operativo.

Aquí analizamos ejemplos específicos para orientar a nuevas características en Windows Insider Preview. Para obtener una descripción general sobre el uso de **ApiInformation**, consulta [Device families overview](https://docs.microsoft.com/en-us/uwp/extension-sdks/device-families-overview#writing-code) (Información general sobre las familias de dispositivos) y la entrada de blog [Dynamically detecting features with API contracts](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/) (Detección dinámica de funciones con contratos de API).

> [!TIP]
> Si se realizan numerosas comprobaciones de API en tiempo de ejecución, el rendimiento de la aplicación puede verse afectado. En estos ejemplos te mostramos las comprobaciones incorporadas. En el código de producción, debes realizar la comprobación una vez, almacenar el resultado en caché y, a continuación, utilizar el resultado en caché en toda la aplicación. 

### <a name="unsupported-scenarios"></a>Escenarios no admitidos

En la mayoría de los casos, puedes mantener la versión mínima de la aplicación establecida en la versión del SDK 10240 y usar comprobaciones en tiempo de ejecución para habilitar las nuevas API cuando la aplicación se ejecute en una versión posterior. Sin embargo, hay algunos casos donde es necesario aumentar la versión mínima de la aplicación para poder usar nuevas características.

Deberás aumentar la versión mínima de la aplicación si usas:
- Una nueva API que requiera una funcionalidad que no esté disponible en una versión anterior. Deberás aumentar la versión mínima admitida a una que incluya dicha funcionalidad. Para obtener más información, consulta [Declaraciones de funcionalidades de las aplicaciones](../packaging/app-capability-declarations.md).
- Se agregue cualquier nueva clave de recurso a generic.xaml y esta no se encuentre disponible en una versión anterior. La versión de generic.xaml utilizada en tiempo de ejecución la determina la versión del sistema operativo en la que se ejecuta el dispositivo. No pueden usarse comprobaciones de API en tiempo de ejecución para determinar la presencia de recursos XAML. Por lo tanto, solo se deben usar claves de recursos que estén disponibles en la versión mínima que admita la aplicación o una [XAMLParseException](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.markup.xamlparseexception.aspx) hará que la aplicación se bloquee en tiempo de ejecución.

### <a name="adaptive-code-options"></a>Opciones de código adaptativo

Existen dos formas de crear código adaptativo. En la mayoría de los casos, el marcado de la aplicación se escribe para que se ejecute en la versión mínima y, a continuación, se usa el código de la aplicación para aprovechar características del sistema operativo más recientes cuando estén presentes. Sin embargo, si es necesario actualizar una propiedad en un estado visual y solo hay un cambio del valor de una propiedad o una enumeración entre las versiones del sistema operativo, es posible crear un desencadenador de estado extensible que se active en función de la presencia de una API.

Aquí comparamos estas opciones.

**Código de la aplicación**

Cuándo usarlo:
- Se recomienda para todos los escenarios de código adaptativo, excepto en casos específicos definidos más adelante para desencadenadores extensibles.

Ventajas:
- Evita al desarrollador la sobrecarga y la complejidad de vincular las diferencias de API en el marcado.

Inconvenientes:
- No existe compatibilidad con el diseñador.

**Desencadenadores de estado**

Cuándo usarlo:
- Se usa cuando solo hay un cambio de propiedad o enumeración entre las versiones del sistema operativo que no requiere cambios de la lógica y está conectado a un estado visual.

Ventajas:
- Permite crear estados visuales específicos que se desencadenan en función de la presencia de una API.
- Existe cierta compatibilidad con el diseñador.

Inconvenientes:
- El uso de desencadenadores personalizados está restringido a los estados visuales, lo que no se presta a diseños adaptativos complicados.
- Deben usarse establecedores para especificar los cambios de valor, por lo que solo son posibles cambios sencillos.
- Los desencadenadores de estado personalizados requieren bastante código para configurarlos y usarlos.

## <a name="adaptive-code-examples"></a>Ejemplos de código adaptativo

En esta sección se muestran varios ejemplos de código adaptativo que usan las API que son nuevas en Windows10, versión 1607 (Windows Insider Preview).

### <a name="example-1-new-enum-value"></a>Ejemplo 1: Nuevo valor de enumeración

Windows10, versión 1607, agrega un nuevo valor a la enumeración [InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.inputscopenamevalue.aspx): **ChatWithoutEmoji**. Este nuevo ámbito de entrada tiene el mismo comportamiento de entrada que el ámbito de entrada **Chat** (revisión ortográfica, autocompletar, uso de mayúsculas automático), pero se asigna a un teclado táctil sin botón de emoji. Esto resulta útil si creas tu propio selector de emoji y deseas deshabilitar el botón de emoji integrado en el teclado táctil. 

Este ejemplo muestra cómo comprobar si el valor de enumeración **ChatWithoutEmoji** está presente y establece la propiedad [InputScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.inputscope.aspx) de un **TextBox** si lo está. Si no se encuentra presente en el sistema en el que se ejecuta la aplicación, en su lugar la propiedad **InputScope** se establece en **Chat**. El código que se muestra podría colocarse en un controlador de eventos Page.Page constructor o Page.Loaded.

> [!TIP]
> Cuando compruebes una API, usa cadenas estáticas en lugar de basarte en las características del lenguaje. NET; de lo contrario, la aplicación podría intentar obtener acceso a un tipo que no esté definido y bloquearse en tiempo de ejecución.

**C#**
```csharp
// Create a TextBox control for sending messages 
// and initialize an InputScope object.
TextBox messageBox = new TextBox();
messageBox.AcceptsReturn = true;
messageBox.TextWrapping = TextWrapping.Wrap;
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();

// Check that the ChatWithEmoji value is present.
// (It's present starting with Windows 10, version 1607,
//  the Target version for the app. This check returns false on earlier versions.)         
if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
{
    // Set new ChatWithoutEmoji InputScope if present.
    scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
}
else
{
    // Fall back to Chat InputScope.
    scopeName.NameValue = InputScopeNameValue.Chat;
}

// Set InputScope on messaging TextBox.
scope.Names.Add(scopeName);
messageBox.InputScope = scope;

// For this example, set the TextBox text to show the selected InputScope.
messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();

// Add the TextBox to the XAML visual tree (rootGrid is defined in XAML).
rootGrid.Children.Add(messageBox);
```

En el ejemplo anterior, se crea el TextBox y todas las propiedades se establecen en el código. Sin embargo, si se dispone de un código XAML existente y solo se necesita cambiar la propiedad InputScope en sistemas que admitan el nuevo valor, puede hacerse sin cambiar el XAML, como se muestra aquí. En XAML, el valor predeterminado se establece en **Chat**, pero se reemplaza en el código si está presente el valor **ChatWithoutEmoji**.

**XAML**
```xaml
<TextBox x:Name="messageBox"
         AcceptsReturn="True" TextWrapping="Wrap"
         InputScope="Chat"
         Loaded="messageBox_Loaded"/>
```

**C#**
```csharp
private void messageBox_Loaded(object sender, RoutedEventArgs e)
{
    if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
    {
        // Check that the ChatWithEmoji value is present.
        // (It's present starting with Windows 10, version 1607,
        //  the Target version for the app. This code is skipped on earlier versions.)
        InputScope scope = new InputScope();
        InputScopeName scopeName = new InputScopeName();
        scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
        // Set InputScope on messaging TextBox.
        scope.Names.Add(scopeName);
        messageBox.InputScope = scope;
    }

    // For this example, set the TextBox text to show the selected InputScope.
    // This is outside of the API check, so it will happen on all OS versions.
    messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();
}
```

Ahora que tenemos un ejemplo concreto, veamos cómo se aplica a él la configuración de la versión de destino y mínima.

En estos ejemplos, se puede usar el valor de enumeración Chat en XAML o en el código sin comprobación, ya que se encuentra presente en la versión del sistema operativo mínima admitida. 

Si se usa el valor ChatWithoutEmoji en XAML o en el código sin comprobación, se compilará sin errores porque se encuentra presente en la versión del sistema operativo de destino. También se ejecutará sin errores en un sistema que posea la versión del sistema operativo de destino. Sin embargo, si la aplicación se ejecuta en un sistema con un sistema operativo que tenga la versión mínima, se bloqueará en tiempo de ejecución porque el valor de enumeración ChatWithoutEmoji no está presente. Por lo tanto, este valor solo debe usarse en el código y encapsularlo en una comprobación de API en tiempo de ejecución, de forma que solo se llame si está admitido en el sistema actual.

### <a name="example-2-new-control"></a>Ejemplo 2: Nuevo control

Una nueva versión de Windows normalmente aporta nuevos controles a la superficie de las API para UWP que aportan nuevas funciones a la plataforma. Para aprovechar la presencia de un nuevo control, usa el método [ApiInformation.IsTypePresent](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx).

Windows10, versión 1607, presenta un nuevo control multimedia llamado [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx). Este control se basa en la clase [MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx), por lo que aporta características como la capacidad para enlazar fácilmente el audio en segundo plano y aprovechar las mejoras de la arquitectura en la pila de multimedia.

Sin embargo, si la aplicación se ejecuta en un dispositivo que ejecute una versión de Windows10 anterior a la versión 1607, debes usar el control [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx) en lugar del nuevo control **MediaPlayerElement**. Puedes usar el método [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx) para comprobar la presencia del control MediaPlayerElement en tiempo de ejecución y cargar cualquier control adecuado para el sistema en el que se ejecute la aplicación.

Este ejemplo muestra cómo crear una aplicación que use el nuevo control MediaPlayerElement o el control MediaElement antiguo en función de si el tipo MediaPlayerElement está presente. En este código, la clase [UserControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol.aspx) se usa para separar por componentes tanto los controles como su interfaz de usuario y su código relacionados, por lo que se pueden alternar en función de la versión del sistema operativo. Como alternativa, es posible usar un control personalizado, lo que proporciona más funcionalidad y un comportamiento más personalizado de lo necesario para este sencillo ejemplo.
 
**MediaPlayerUserControl** 

`MediaPlayerUserControl` encapsula un **MediaPlayerElement** y varios botones que se usan para pasar por el contenido multimedia fotograma a fotograma. UserControl permite tratar estos controles como una entidad única y facilita alternar con un control MediaElement en sistemas más antiguos. Este control de usuario debe usarse solo en sistemas donde MediaPlayerElement esté presente, así que no uses comprobaciones ApiInformation en el código dentro de este control de usuario.

> [!NOTE]
> Para mantener este ejemplo sencillo y centrado, los botones de pasos de fotograma se colocan fuera el reproductor multimedia. Para ofrecer una mejor experiencia de usuario, deberías personalizar los MediaTransportControls para que incluyan los botones personalizados. Consulta [Crear controles de transporte personalizados](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/custom-transport-controls) para obtener más información. 

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaPlayerUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid x:Name="MPE_grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <StackPanel Orientation="Horizontal" 
                    HorizontalAlignment="Center" Grid.Row="1">
            <RepeatButton Click="StepBack_Click" Content="Step Back"/>
            <RepeatButton Click="StepForward_Click" Content="Step Forward"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**C#**
```csharp
using System;
using Windows.Media.Core;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace MediaApp
{
    public sealed partial class MediaPlayerUserControl : UserControl
    {
        public MediaPlayerUserControl()
        {
            this.InitializeComponent();
            
            // The markup code compiler runs against the Minimum OS version so MediaPlayerElement must be created in app code
            MPE = new MediaPlayerElement();
            Uri videoSource = new Uri("ms-appx:///Assets/UWPDesign.mp4");
            MPE.Source = MediaSource.CreateFromUri(videoSource);
            MPE.AreTransportControlsEnabled = true;
            MPE.MediaPlayer.AutoPlay = true;

            // Add MediaPlayerElement to the Grid
            MPE_grid.Children.Add(MPE);

        }

        private void StepForward_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }

        private void StepBack_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }
    }
}
```

**MediaElementUserControl**
 
`MediaElementUserControl` encapsula un control **MediaElement**.

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaElementUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid>
        <MediaElement AreTransportControlsEnabled="True" 
                      Source="Assets/UWPDesign.mp4"/>
    </Grid>
</UserControl>
```

> [!NOTE]
> La página de códigos de `MediaElementUserControl` contiene solo código generado, de modo que no se muestra.

**Inicializar un control basado en IsTypePresent**

En tiempo de ejecución, llama a **ApiInformation.IsTypePresent** para comprobar MediaPlayerElement. Si está presente, carga `MediaPlayerUserControl`, si no es así, carga `MediaElementUserControl`.

**C#**
```csharp
public MainPage()
{
    this.InitializeComponent();

    UserControl mediaControl;

    // Check for presence of type MediaPlayerElement.
    if (ApiInformation.IsTypePresent("Windows.UI.Xaml.Controls.MediaPlayerElement"))
    {
        mediaControl = new MediaPlayerUserControl();
    }
    else
    {
        mediaControl = new MediaElementUserControl();
    }

    // Add mediaControl to XAML visual tree (rootGrid is defined in XAML).
    rootGrid.Children.Add(mediaControl);
}
```

> [!IMPORTANT]
> Recuerda que esta comprobación solo establece el objeto `mediaControl` en `MediaPlayerUserControl` o en `MediaElementUserControl`. Es necesario realizar estas comprobaciones condicionales en cualquier otro lugar del código en el que haga falta para determinar si se debe usar la API MediaPlayerElement o la API MediaElement. La comprobación se debe realizar una vez, almacenar el resultado en caché y, a continuación, utilizar el resultado en caché en toda la aplicación.

## <a name="state-trigger-examples"></a>Ejemplos de desencadenador de estado

Los desencadenadores de estado extensible permiten usar juntos el marcado y el código para desencadenar cambios de estado visual en función de una condición que realice una comprobación en el código; en este caso, la presencia de una API específica. No recomendamos los desencadenadores de estado para los escenarios comunes de código adaptativo debido a la sobrecarga que implica, y recomendamos que se restrinja únicamente a los estados visuales. 

Debes usar los desencadenadores de estado para código adaptativo solo cuando haya pequeños cambios de la interfaz de usuario entre diferentes versiones del sistema operativo que no afecten a la interfaz de usuario restante, como un cambio de valor de una propiedad o una enumeración en un control.

### <a name="example-1-new-property"></a>Ejemplo 1: Nueva propiedad

El primer paso para configurar un desencadenador de estado extensible es crear una subclase de la clase [StateTriggerBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.statetriggerbase.aspx) para crear un desencadenador personalizado que se activará en función de la presencia de una API. Este ejemplo muestra un desencadenador que se activa si la presencia de la propiedad coincide con la variable `_isPresent` establecida en XAML.

**C#**
```csharp
class IsPropertyPresentTrigger : StateTriggerBase
{
    public string TypeName { get; set; }
    public string PropertyName { get; set; }

    private Boolean _isPresent;
    private bool? _isPropertyPresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;
            if (_isPropertyPresent == null)
            {
                // Call into ApiInformation method to determine if property is present.
                _isPropertyPresent =
                ApiInformation.IsPropertyPresent(TypeName, PropertyName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isPropertyPresent);
        }
    }
}
```

El siguiente paso es configurar el desencadenador de estado visual en XAML, para que se produzcan dos estados visuales diferentes en función de la presencia de la API. 

Windows10, versión 1607. introduce una nueva propiedad en la clase [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) llamada [AllowFocusOnInteraction](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.allowfocusoninteraction.aspx) que determina si un control toma el foco cuando un usuario interactúa con él. Esto resulta útil si se desea mantener el foco en un cuadro de texto para la entrada de datos (y mantener el teclado táctil visible) mientras el usuario hace clic en un botón.

El desencadenador de este ejemplo comprueba si la propiedad está presente. Si la propiedad está presente, establece la propiedad **AllowFocusOnInteraction** de un botón en **false**; si la propiedad no está presente, el botón conserva su estado original. TextBox se incluye para que sea más fácil ver el efecto de esta propiedad cuando se ejecuta el código.

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Width="300" Height="36"/>
        <!-- Button to set the new property on. -->
        <Button x:Name="testButton" Content="Test" Margin="12"/>
    </StackPanel>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="propertyPresentStateGroup">
            <VisualState>
                <VisualState.StateTriggers>
                    <!--Trigger will activate if the AllowFocusOnInteraction property is present-->
                    <local:IsPropertyPresentTrigger 
                        TypeName="Windows.UI.Xaml.FrameworkElement" 
                        PropertyName="AllowFocusOnInteraction" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="testButton.AllowFocusOnInteraction" 
                            Value="False"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="example-2-new-enum-value"></a>Ejemplo 2: Nuevo valor de enumeración

Este ejemplo muestra cómo establecer diferentes valores de enumeración en función de si un valor se encuentra presente. Usa un desencadenador de estado personalizado para lograr el mismo resultado que el ejemplo anterior de chat. En este ejemplo, se usa el nuevo ámbito de entrada ChatWithoutEmoji si el dispositivo ejecuta Windows10, versión 1607; de lo contrario, se usa el ámbito de entrada **Chat**. Los estados visuales que usan este desencadenador se configuran con un estilo *if-else*, en el que el ámbito de entrada se elige en función en la presencia del nuevo valor de enumeración.

**C#**
```csharp
class IsEnumPresentTrigger : StateTriggerBase
{
    public string EnumTypeName { get; set; }
    public string EnumValueName { get; set; }

    private Boolean _isPresent;
    private bool? _isEnumValuePresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;

            if (_isEnumValuePresent == null)
            {
                // Call into ApiInformation method to determine if value is present.
                _isEnumValuePresent =
                ApiInformation.IsEnumNamedValuePresent(EnumTypeName, EnumValueName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isEnumValuePresent);
        }
    }
}
```

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <TextBox x:Name="messageBox"
     AcceptsReturn="True" TextWrapping="Wrap"/>


    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="EnumPresentStates">
            <!--if-->
            <VisualState x:Name="isPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="ChatWithoutEmoji"/>
                </VisualState.Setters>
            </VisualState>
            <!--else-->
            <VisualState x:Name="isNotPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="False"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="Chat"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

## <a name="related-articles"></a>Artículos relacionados

- [Información general sobre las familias de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)
- [Detección dinámica de funciones con contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
