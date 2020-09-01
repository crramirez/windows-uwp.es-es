---
Description: Agregue un control InkToolbar predeterminado a una aplicación de entrada manuscrita de la aplicación de Windows, agregue un botón de lápiz personalizado al control InkToolbar y enlace el botón del lápiz personalizado a una definición de lápiz personalizada.
title: Agregar un control InkToolbar a una aplicación de Windows
label: Add an InkToolbar to a Windows app
template: detail.hbs
keywords: Windows Ink, entrada manuscrita de Windows, DirectInk, InkPresenter, InkCanvas, InkToolbar, Plataforma universal de Windows, UWP, interacción del usuario, entrada
ms.date: 02/08/2017
ms.topic: article
ms.assetid: d888f75f-c2a0-4134-81db-907b5e24fcc5
ms.localizationpriority: medium
ms.openlocfilehash: 721e54fc7e0fc9d6e6dc18109ea39b1326534646
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156989"
---
# <a name="add-an-inktoolbar-to-a-windows-app"></a>Agregar un control InkToolbar a una aplicación de Windows



Hay dos controles diferentes que facilitan la entrada manuscrita en las aplicaciones de Windows: [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) e [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar).

El control [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) proporciona la funcionalidad básica de Windows Ink. Úsalo para representar la entrada manuscrita como un trazo de lápiz (con la configuración predeterminada de color y espesor) o como un trazo de borrado.

> Para obtener información detallada sobre la implementación de InkCanvas, consulte [interacciones de lápiz y lápiz en aplicaciones de Windows](pen-and-stylus-interactions.md).

Como una superposición completamente transparente, el control InkCanvas no proporciona ninguna interfaz de usuario integrada para establecer propiedades de trazo de lápiz. Si quieres cambiar la experiencia de entrada manuscrita predeterminada, permite que los usuarios establezcan las propiedades de trazo de lápiz y admite otras características de entrada manuscrita personalizadas. Para hacerlo, tienes dos opciones:

- En el código subyacente, usa el objeto subyacente [**InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter) enlazado al control InkCanvas.

  Las API de InkPresenter admiten una amplia personalización de la experiencia de entrada manuscrita. Para obtener más información, consulte [interacciones de lápiz y lápiz en aplicaciones de Windows](pen-and-stylus-interactions.md).

- Enlazar un control [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) al control InkCanvas. De forma predeterminada, el control InkToolbar proporciona una colección personalizable y extensible de botones para activar características relacionadas con la tinta, como el tamaño del trazo, el color de la tinta y la punta del lápiz.

  El control InkToolbar se describe en este tema.

> **API importantes**: [**clase InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas), clase [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar), [**clase InkPresenter**](/uwp/api/windows.ui.input.inking.inkpresenter), [**Windows. UI. Input. inking**](/uwp/api/Windows.UI.Input.Inking)

## <a name="default-inktoolbar"></a>Control InkToolbar predeterminado

De forma predeterminada, el control [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) incluye botones para dibujar, borrar, resaltar y mostrar una galería de símbolos (regla o Protractor). Dependiendo de la característica, en un control flotante se proporcionan otros comandos y opciones de configuración, como los destinados a definir el color y el grosor del trazo o a borrar todas las entradas de lápiz.

![InkToolbar](./images/ink/ink-tools-invoked-toolbar-small.png)  
*Barra de herramientas de Windows Ink predeterminada*

Para agregar un control [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar) predeterminado a una aplicación de entrada manuscrita, simplemente colóquelo en la misma página que el [**InkCanvas**](/uwp/api/windows.ui.xaml.controls.inkcanvas) y asocie los dos controles.

1. En MainPage.xaml, declara un objeto contenedor (para este ejemplo, usamos un control Grid) para la superficie de entrada manuscrita.
2. Declara un objeto InkCanvas como elemento secundario del contenedor. (El tamaño del control InkCanvas se hereda del contenedor).
3. Declara un control InkToolbar y usa el atributo TargetInkCanvas para enlazarlo al control InkCanvas.

> [!NOTE]
> Asegúrate de que el control InkToolbar se declare después de InkCanvas. De lo contrario, la superposición de InkCanvas hará que el control InkToolbar sea inaccesible.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
    </Grid>
</Grid>
```

## <a name="basic-customization"></a>Personalización básica

En esta sección, nos centraremos en algunos escenarios básicos de personalización de la barra de herramientas de Windows Ink.

### <a name="specify-location-and-orientation"></a>Especificar la ubicación y la orientación

Cuando agrega una barra de herramientas de entrada manuscrita a la aplicación, puede aceptar la ubicación predeterminada y orientaion de la barra de herramientas o establecerlas según lo requiera la aplicación o el usuario.

**XAML**

Especifique explícitamente la ubicación y la orientación de la barra de herramientas a través de las propiedades [VerticalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.VerticalAlignment), [HorizontalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment)y [Orientation](/uwp/api/windows.ui.xaml.controls.inktoolbar?branch=rs3.Orientation) .

| Valor predeterminado | Explícita |
| --- | --- |
| ![Ubicación y orientación de la barra de herramientas de entrada manuscrita predeterminada](./images/ink/location-default-small.png) | ![Ubicación y orientación de la barra de herramientas de tinta explícita](./images/ink/location-explicit-small.png) |
| *Ubicación y orientación predeterminadas de la barra de herramientas de Windows Ink* | *Ubicación y orientación explícita de la barra de herramientas de Windows Ink* |

Este es el código para establecer explícitamente la ubicación y la orientación de la barra de herramientas de entrada manuscrita en XAML.
```xaml
<InkToolbar x:Name="inkToolbar" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Right" 
    Orientation="Vertical" 
    TargetInkCanvas="{x:Bind inkCanvas}" />
```

**Inicializar en función de las preferencias del usuario o el estado del dispositivo**

En algunos casos, es posible que desee establecer la ubicación y la orientación de la barra de herramientas de la tinta en función de la preferencia del usuario o el estado del dispositivo. En el ejemplo siguiente se muestra cómo establecer la ubicación y la orientación de la barra de herramientas de entrada manuscrita en función de las preferencias de escritura de izquierda o derecha especificadas a través de la **configuración > dispositivos > lápiz & lápiz de > de Windows ink > elegir la mano con la que escribe**.

![Configuración de mano dominante](./images/ink/location-handedness-setting.png)  
*Configuración de mano dominante*

Puede consultar este valor mediante la propiedad HandPreference de Windows. UI. ViewManagement y establecer el [HorizontalAlignment](/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment) según el valor devuelto. En este ejemplo, se busca la barra de herramientas en el lado izquierdo de la aplicación para una persona que se va a usar a la izquierda y en el lado derecho de una persona con la mano adecuada.

**Descargar este ejemplo desde la ubicación de la [barra de herramientas de entrada y el ejemplo de orientación (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)**

```csharp
public MainPage()
{
    this.InitializeComponent();

    Windows.UI.ViewManagement.UISettings settings = 
        new Windows.UI.ViewManagement.UISettings();
    HorizontalAlignment alignment = 
        (settings.HandPreference == 
            Windows.UI.ViewManagement.HandPreference.LeftHanded) ? 
            HorizontalAlignment.Left : HorizontalAlignment.Right;
    inkToolbar.HorizontalAlignment = alignment;
}
```

**Ajustar dinámicamente al estado de usuario o de dispositivo**

También puede usar el enlace para buscar las actualizaciones de la interfaz de usuario en función de los cambios en las preferencias del usuario, la configuración del dispositivo o los Estados de los dispositivos. En el ejemplo siguiente, se expande el ejemplo anterior y se muestra cómo colocar dinámicamente la barra de herramientas de la tinta en función de la orientación del dispositivo mediante el enlace, un objeto ViewMOdel y la interfaz [INotifyPropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged) . 

**Descargar este ejemplo desde la ubicación de la [barra de herramientas de entrada y el ejemplo de orientación (dinámico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)**

1. En primer lugar, vamos a agregar el ViewModel.
    1. Agregue una nueva carpeta al proyecto y llámela **ViewModels**.
    1. Agregue una nueva clase a la carpeta ViewModels (en este ejemplo, se le llamó **InkToolbarSnippetHostViewModel.CS**).
        > [!NOTE] 
        > Usamos el [patrón singleton](/previous-versions/msp-n-p/ff650849(v=pandp.10)) , ya que solo necesitamos un objeto de este tipo para la vida útil de la aplicación.

    1. Agregue `using System.ComponentModel` el espacio de nombres al archivo.
    1. Agregue una variable miembro estática denominada **instancia**y una propiedad estática de solo lectura denominada **instancia**. Haga que el constructor sea privado para asegurarse de que solo se puede tener acceso a esta clase a través de la propiedad de instancia.   
        > [!NOTE] 
        > Esta clase hereda de la interfaz [INotifyPropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged) , que se usa para notificar a los clientes, normalmente los clientes de enlace, que un valor de propiedad ha cambiado. Vamos a usar esto para controlar los cambios en la orientación del dispositivo (expandiremos este código y explicaremos más adelante en un paso posterior).  

        ```csharp
        using System.ComponentModel;

        namespace locationandorientation.ViewModels
        {
            public class InkToolbarSnippetHostViewModel : INotifyPropertyChanged
            {
                private static InkToolbarSnippetHostViewModel instance;
                
                public static InkToolbarSnippetHostViewModel Instance
                {
                    get
                    {
                        if (null == instance)
                        {
                            instance = new InkToolbarSnippetHostViewModel();
                        }
                        return instance;
                    }
                }
            }

            private InkToolbarSnippetHostViewModel() { }
        }
        ```

    1. Agregue dos propiedades bool a la clase InkToolbarSnippetHostViewModel: **LeftHandedLayout** (la misma funcionalidad que el ejemplo anterior solo XAML) y **PortraitLayout** (orientación del dispositivo).
        >[!NOTE] 
        > La propiedad PortraitLayout es configurable e incluye la definición para el evento [PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) .

        ```csharp
        public bool LeftHandedLayout
        {
            get
            {
                bool leftHandedLayout = false;
                Windows.UI.ViewManagement.UISettings settings =
                    new Windows.UI.ViewManagement.UISettings();
                leftHandedLayout = (settings.HandPreference ==
                    Windows.UI.ViewManagement.HandPreference.LeftHanded);
                return leftHandedLayout;
            }
        }

        public bool portraitLayout = false;
        public bool PortraitLayout
        {
            get
            {
                Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                    Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
                portraitLayout = 
                    (winOrientation == 
                        Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
                return portraitLayout;
            }
            set
            {
                if (value.Equals(portraitLayout)) return;
                portraitLayout = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
            }
        }
        ```

1. Ahora, vamos a agregar un par de clases de convertidor a nuestro proyecto. Cada clase contiene un objeto Convert que devuelve un valor de alineación ( [HorizontalAlignment](/uwp/api/windows.ui.xaml.horizontalalignment) o [VerticalAlignment](/uwp/api/windows.ui.xaml.verticalalignment)).
    1. Agregue una nueva carpeta al proyecto y llámela **convertidores**.
    1. Agregue dos clases nuevas a la carpeta Converters (en este ejemplo, se les llama **HorizontalAlignmentFromHandednessConverter.CS** y **VerticalAlignmentFromAppViewConverter.CS**).
    1. Agregue `using Windows.UI.Xaml` `using Windows.UI.Xaml.Data` espacios de nombres y a cada archivo.
    1. Cambie cada clase a `public` y especifique que implementa la interfaz [IValueConverter](/uwp/api/windows.ui.xaml.data.ivalueconverter) .
    1. Agregue los métodos [Convert](/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) y [ConvertBack](/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) a cada archivo, como se muestra aquí (se deja el método ConvertBack en modo no implementado).
        - HorizontalAlignmentFromHandednessConverter coloca la barra de herramientas de entrada manuscrita en el lado derecho de la aplicación para los usuarios que se entregan a la derecha y en el lado izquierdo de la aplicación para los usuarios que se entregan a la izquierda.
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class HorizontalAlignmentFromHandednessConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool leftHanded = (bool)value;
                    HorizontalAlignment alignment = HorizontalAlignment.Right;
                    if (leftHanded)
                    {
                        alignment = HorizontalAlignment.Left;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

        - VerticalAlignmentFromAppViewConverter coloca la barra de herramientas de entrada manuscrita en el centro de la aplicación para orientarla verticalmente y en la parte superior de la aplicación para la orientación horizontal (a fin de mejorar la facilidad de uso, es simplemente una opción arbitraria para fines de demostración).
        ```csharp
        using System;

        using Windows.UI.Xaml;
        using Windows.UI.Xaml.Data;

        namespace locationandorientation.Converters
        {
            public class VerticalAlignmentFromAppViewConverter : IValueConverter
            {
                public object Convert(object value, Type targetType,
                    object parameter, string language)
                {
                    bool portraitOrientation = (bool)value;
                    VerticalAlignment alignment = VerticalAlignment.Top;
                    if (portraitOrientation)
                    {
                        alignment = VerticalAlignment.Center;
                    }
                    return alignment;
                }

                public object ConvertBack(object value, Type targetType,
                    object parameter, string language)
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1. Ahora, abra el archivo MainPage.xaml.cs.
    1. Agregue `using using locationandorientation.ViewModels` a la lista de espacios de nombres para asociar el ViewModel.
    1. Agregue `using Windows.UI.ViewManagement` a la lista de espacios de nombres para habilitar la escucha de cambios en la orientación del dispositivo.
    1. Agregue el código [WindowSizeChangedEventHandler](/uwp/api/windows.ui.xaml.windowsizechangedeventhandler) .
    1. Establezca [DataContext](/uwp/api/Windows.UI.Xaml.FrameworkElement.DataContext) para la vista en la instancia singleton de la clase InkToolbarSnippetHostViewModel. 
    ```csharp
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Controls;

    using locationandorientation.ViewModels;
    using Windows.UI.ViewManagement;

    namespace locationandorientation
    {
        public sealed partial class MainPage : Page
        {
            public MainPage()
            {
                this.InitializeComponent();

                Window.Current.SizeChanged += (sender, args) =>
                {
                    ApplicationView currentView = ApplicationView.GetForCurrentView();

                    if (currentView.Orientation == ApplicationViewOrientation.Landscape)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = false;
                    }
                    else if (currentView.Orientation == ApplicationViewOrientation.Portrait)
                    {
                        InkToolbarSnippetHostViewModel.Instance.PortraitLayout = true;
                    }
                };

                DataContext = InkToolbarSnippetHostViewModel.Instance;
            }
        }
    }
    ```

1. A continuación, abra el archivo MainPage. Xaml.
    1. Agregue `xmlns:converters="using:locationandorientation.Converters"` al `Page` elemento para enlazar a nuestros convertidores.
        ```xaml
        <Page
        x:Class="locationandorientation.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:locationandorientation"
        xmlns:converters="using:locationandorientation.Converters"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        ```

    1. Agregue un `PageResources` elemento y especifique referencias a nuestros convertidores.
        ```xaml
        <Page.Resources>
            <converters:HorizontalAlignmentFromHandednessConverter x:Key="HorizontalAlignmentConverter"/>
            <converters:VerticalAlignmentFromAppViewConverter x:Key="VerticalAlignmentConverter"/>
        </Page.Resources>
        ```

    1. Agregue los elementos InkCanvas y InkToolbar y enlace las propiedades VerticalAlignment y HorizontalAlignment del control InkToolbar.
        ```xaml
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="{Binding PortraitLayout, Converter={StaticResource VerticalAlignmentConverter} }" 
                    HorizontalAlignment="{Binding LeftHandedLayout, Converter={StaticResource HorizontalAlignmentConverter} }" 
                    Orientation="Vertical" 
                    TargetInkCanvas="{x:Bind inkCanvas}" />
        ```

1. Vuelva al archivo InkToolbarSnippetHostViewModel.cs para agregar `PortraitLayout` `LeftHandedLayout` las propiedades y bool a la `InkToolbarSnippetHostViewModel` clase, junto con la compatibilidad para volver a enlazar `PortraitLayout` cuando cambia el valor de la propiedad. 
    ```csharp
    public bool LeftHandedLayout
    {
        get
        {
            bool leftHandedLayout = false;
            Windows.UI.ViewManagement.UISettings settings =
                new Windows.UI.ViewManagement.UISettings();
            leftHandedLayout = (settings.HandPreference ==
                Windows.UI.ViewManagement.HandPreference.LeftHanded);
            return leftHandedLayout;
        }
    }

    public bool portraitLayout = false;
    public bool PortraitLayout
    {
        get
        {
            Windows.UI.ViewManagement.ApplicationViewOrientation winOrientation = 
                Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            portraitLayout = 
                (winOrientation == 
                    Windows.UI.ViewManagement.ApplicationViewOrientation.Portrait);
            return portraitLayout;
        }
        set
        {
            if (value.Equals(portraitLayout)) return;
            portraitLayout = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("PortraitLayout"));
        }
    }

    #region INotifyPropertyChanged Members

    public event PropertyChangedEventHandler PropertyChanged;

    protected void OnPropertyChanged(string property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }

    #endregion
    ```

Ahora debería tener una aplicación de entrada manuscrita que se adapte a la preferencia de la mano dominante del usuario y responda dinámicamente a la orientación del dispositivo del usuario.

### <a name="specify-the-selected-button"></a>Especificar el botón seleccionado  
![Botón de lápiz seleccionado durante la inicialización](./images/ink/ink-tools-default-toolbar.png)  
*Barra de herramientas de Windows Ink con el botón de lápiz seleccionado durante la inicialización*

De manera predeterminada, se selecciona el primer botón (más a la izquierda) cuando se inicia la aplicación y se inicializa la barra de herramientas. En la barra de herramientas de Windows Ink predeterminada, este es el botón de bolígrafo.

Dado que el marco define el orden de los botones integrados, el primer botón podría no ser el lápiz o la herramienta que quieres activar de forma predeterminada.

Puedes invalidar este comportamiento predeterminado y especificar el botón seleccionado en la barra de herramientas.

Para este ejemplo, inicializamos la barra de herramientas predeterminada con el botón de lápiz seleccionado y el lápiz activado (en lugar del bolígrafo).

1. Usa la declaración de XAML para los controles InkCanvas e InkToolbar del ejemplo anterior.
2. En el código subyacente, configura un controlador para el evento [Loaded](/uwp/api/windows.ui.xaml.frameworkelement.loaded) del objeto [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar).

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loaded += inkToolbar_Loaded;
  }
  ```

3. En el controlador del evento [Loaded](/uwp/api/windows.ui.xaml.frameworkelement.loaded):
    1. Obtén una referencia al botón [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) integrado.

    Al pasar un objeto [InkToolbarTool.Pencil](/uwp/api/windows.ui.xaml.controls.inktoolbartool) en el método [GetToolButton](/uwp/api/windows.ui.xaml.controls.inktoolbar.gettoolbutton), se devuelve un objeto [InkToolbarToolButton](/uwp/api/windows.ui.xaml.controls.inktoolbartoolbutton) para el botón [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton).

    2. Establece [ActiveTool](/uwp/api/windows.ui.xaml.controls.inktoolbar.activetool) en el objeto devuelto en el paso anterior.

```CSharp
/// <summary>
/// Handle the Loaded event of the InkToolbar.
/// By default, the active tool is set to the first tool on the toolbar.
/// Here, we set the active tool to the pencil button.
/// </summary>
/// <param name="sender"></param>
/// <param name="e"></param>
private void inkToolbar_Loaded(object sender, RoutedEventArgs e)
{
    InkToolbarToolButton pencilButton = inkToolbar.GetToolButton(InkToolbarTool.Pencil);
    inkToolbar.ActiveTool = pencilButton;
}
```

### <a name="specify-the-built-in-buttons"></a>Especificar los botones integrados

![Botones específicos que se incluyen durante la inicialización](./images/ink/ink-tools-specific.png)  
*Botones específicos que se incluyen durante la inicialización*

Como hemos mencionado, la barra de herramientas de Windows Ink incluye una colección de botones integrados predeterminados. Estos botones aparecen en el siguiente orden (de izquierda a derecha):

- [InkToolbarBallpointPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton)
- [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton)
- [InkToolbarHighlighterButton](/uwp/api/windows.ui.xaml.controls.inktoolbarhighlighterbutton)
- [InkToolbarEraserButton](/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton)
- [InkToolbarRulerButton](/uwp/api/windows.ui.xaml.controls.inktoolbarrulerbutton)

Para este ejemplo, inicializamos la barra de herramientas solo con los botones de bolígrafo, lápiz y borrador integrados.

Puedes hacerlo mediante XAML o código subyacente.

**XAML**

Modifica la declaración de XAML para los controles InkCanvas e InkToolbar del primer ejemplo.
- Agrega un atributo [InitialControls](/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols) y establece su valor en "[None](/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols)". Esta acción borra la colección predeterminada de botones integrados.
- Agrega los botones InkToolbar específicos necesarios para la aplicación. Aquí, agregamos solo los botones [InkToolbarBallpointPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton), [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) e [InkToolbarEraserButton](/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton).
> [!NOTE]
> Los botones se agregan a la barra de herramientas en el orden definido por el marco, no en el orden especificado aquí.

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header"
                   Text="Basic ink sample"
                   Style="{ThemeResource HeaderTextBlockStyle}"
                   Margin="10,0,0,0" />
    </StackPanel>
    <Grid Grid.Row="1">
        <Image Source="Assets\StoreLogo.png" />
        <!-- Clear the default InkToolbar buttons by setting InitialControls to None. -->
        <!-- Set the active tool to the pencil button. -->
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar"
                    VerticalAlignment="Top"
                    TargetInkCanvas="{x:Bind inkCanvas}"
                    InitialControls="None">
            <!--
             Add only the ballpoint pen, pencil, and eraser.
             Note that the buttons are added to the toolbar in the order
             defined by the framework, not the order we specify here.
            -->
            <InkToolbarEraserButton />
            <InkToolbarBallpointPenButton />
            <InkToolbarPencilButton/>
        </InkToolbar>
    </Grid>
</Grid>
```

**Código subyacente**
1. Usa la declaración de XAML para los controles InkCanvas e InkToolbar del primer ejemplo.

  ```xaml
  <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
          <RowDefinition Height="Auto"/>
          <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
          <TextBlock x:Name="Header"
                     Text="Basic ink sample"
                     Style="{ThemeResource HeaderTextBlockStyle}"
                     Margin="10,0,0,0" />
      </StackPanel>
      <Grid Grid.Row="1">
          <Image Source="Assets\StoreLogo.png" />
          <InkCanvas x:Name="inkCanvas" />
          <InkToolbar x:Name="inkToolbar"
          VerticalAlignment="Top"
          TargetInkCanvas="{x:Bind inkCanvas}" />
      </Grid>
  </Grid>
  ```

2. En el código subyacente, configura un controlador para el evento [Loading](/uwp/api/windows.ui.xaml.frameworkelement.loading) del objeto [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar).

  ```csharp
  /// <summary>
  /// An empty page that can be used on its own or navigated to within a Frame.
  /// Here, we set up InkToolbar event listeners.
  /// </summary>
  public MainPage_CodeBehind()
  {
      this.InitializeComponent();
      // Add handlers for InkToolbar events.
      inkToolbar.Loading += inkToolbar_Loading;
  }
  ```

3. Establece [InitialControls](/uwp/api/windows.ui.xaml.controls.inktoolbar.initialcontrols) en "[None](/uwp/api/windows.ui.xaml.controls.inktoolbarinitialcontrols)".
4. Crea referencias de objetos para los botones que necesita tu aplicación. Aquí, agregamos solo los botones [InkToolbarBallpointPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarballpointpenbutton), [InkToolbarPencilButton](/uwp/api/windows.ui.xaml.controls.inktoolbarpencilbutton) e [InkToolbarEraserButton](/uwp/api/windows.ui.xaml.controls.inktoolbareraserbutton).
  > [!NOTE]
  > Los botones se agregan a la barra de herramientas en el orden definido por el marco, no en el orden especificado aquí.

5. Método [Add](/uwp/api/windows.ui.xaml.dependencyobjectcollection.add) para agregar los botones al control InkToolbar.

  ```csharp
  /// <summary>
  /// Handles the Loading event of the InkToolbar.
  /// Here, we identify the buttons to include on the InkToolbar.
  /// </summary>
  /// <param name="sender">The InkToolbar</param>
  /// <param name="args">The InkToolbar event data.
  /// If there is no event data, this parameter is null</param>
  private void inkToolbar_Loading(FrameworkElement sender, object args)
  {
      // Clear all built-in buttons from the InkToolbar.
      inkToolbar.InitialControls = InkToolbarInitialControls.None;

      // Add only the ballpoint pen, pencil, and eraser.
      // Note that the buttons are added to the toolbar in the order
      // defined by the framework, not the order we specify here.
      InkToolbarBallpointPenButton ballpoint = new InkToolbarBallpointPenButton();
      InkToolbarPencilButton pencil = new InkToolbarPencilButton();
      InkToolbarEraserButton eraser = new InkToolbarEraserButton();
      inkToolbar.Children.Add(eraser);
      inkToolbar.Children.Add(ballpoint);
      inkToolbar.Children.Add(pencil);
  }
  ```

<!--
### Support touch input
By default, the InkToolbar supports both pen and mouse input, you have to enable support for touch input.
-->

## <a name="custom-buttons-and-inking-features"></a>Botones personalizados y características de entrada manuscrita

Puedes personalizar y ampliar la colección de botones (y las características de entrada manuscrita asociadas) que se proporcionan a través del control InkToolbar.

El control InkToolbar consta de dos grupos de tipos de botones:

1. Un grupo de botones de "herramienta" que contiene los botones para dibujar, borrar y resaltar integrados. Las herramientas y los lápices personalizados se agregan aquí.
> **Nota:** &nbsp; &nbsp; La selección de características es mutuamente excluyente.

2. Un grupo de botones de "alternancia" que contiene el botón de regla integrado. Las alternancias personalizadas se agregan aquí.
> **Nota:** &nbsp; &nbsp; Las características no son mutuamente excluyentes y se pueden usar simultáneamente con otras herramientas activas.

Dependiendo de la aplicación y de la funcionalidad de entrada manuscrita necesaria, puedes agregar cualquiera de los siguientes botones (enlazados a sus características de entrada de lápiz personalizada) al control InkToolbar:

- Lápiz personalizado: lápiz para el cual la aplicación host define las propiedades de punta de lápiz y paleta de colores del trazo, como la forma, la rotación y el tamaño.
- Herramienta personalizada: herramienta que no es un lápiz definida por la aplicación host.
- Alternancia personalizada: establece el estado de una característica definida por la aplicación en activado o desactivado. Cuando se activa, la característica funciona junto con la herramienta activa.

> **Nota:** &nbsp; &nbsp; No se puede cambiar el orden de presentación de los botones integrados. El orden de presentación predeterminado es: bolígrafo, lápiz, marcador de resaltado, borrador y regla. Los lápices personalizados se agregan al último lápiz predeterminado, los botones de la herramienta personalizada se agregan entre el último botón del lápiz y el botón de borrador, y los botones de alternancia personalizada se agregan detrás del botón de regla. (Los botones personalizados se agregan en el orden en que se especifican).

### <a name="custom-pen"></a>Lápiz personalizado

Puedes crear un lápiz personalizado (que se activa mediante un botón de lápiz personalizado) con el que defines la paleta de colores de entrada de lápiz y las propiedades de punta de lápiz, como la forma, la rotación y el tamaño.

![Botón del lápiz caligráfico personalizado](./images/ink/ink-tools-custompen.png)  
*Botón del lápiz caligráfico personalizado*

En este ejemplo, se define un lápiz personalizado con una punta ancha que permite trazos de lápiz caligráficos básicos. También se personaliza la colección de pinceles de la paleta que se muestra en el control flotante de botones.

**Código subyacente**

En primer lugar, definimos nuestro lápiz personalizado y especificamos los atributos de dibujo en el código subyacente. Hacemos referencia este lápiz personalizado de XAML más adelante.

1. Haz clic con el botón secundario en el proyecto en el Explorador de soluciones y selecciona Agregar > Nuevo elemento.
2. En Visual C# -> Código, agrega un nuevo archivo de clase y llámalo CalligraphicPen.cs.
3. En Calligraphic.cs, reemplaza el valor predeterminado mediante un bloque con lo siguiente:
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. Especifica que la clase CalligraphicPen se deriva de [InkToolbarCustomPen](/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. Invalida [CreateInkDrawingAttributesCore](/uwp/api/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore) para especificar tu propio tamaño de trazo y pincel.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. Crea un objeto [InkDrawingAttributes](/uwp/api/windows.ui.input.inking.inkdrawingattributes) y establece las propiedades de [forma de la punta del lápiz](/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentip), [rotación de la punta](/uwp/api/windows.ui.input.inking.inkdrawingattributes.pentiptransform), [tamaño del trazo](/uwp/api/windows.ui.input.inking.inkdrawingattributes.size) y [color del trazo](/uwp/api/windows.ui.input.inking.inkdrawingattributes.color).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
        InkDrawingAttributes inkDrawingAttributes =
          new InkDrawingAttributes();
        inkDrawingAttributes.PenTip = PenTipShape.Circle;
        inkDrawingAttributes.Size =
          new Windows.Foundation.Size(strokeWidth, strokeWidth * 20);
        SolidColorBrush solidColorBrush = brush as SolidColorBrush;
        if (solidColorBrush != null)
        {
            inkDrawingAttributes.Color = solidColorBrush.Color;
        }
        else
        {
            inkDrawingAttributes.Color = Colors.Black;
        }

        Matrix3x2 matrix = Matrix3x2.CreateRotation(45);
        inkDrawingAttributes.PenTipTransform = matrix;

        return inkDrawingAttributes;
    }
}
```

**XAML**

A continuación, agregamos las referencias necesarias para el lápiz personalizado en MainPage.xaml.

1. Declaramos un diccionario de recursos de página local que cree una referencia al lápiz personalizado (`CalligraphicPen`) definido en CalligraphicPen.cs y una [colección de pinceles](/uwp/api/Windows.UI.Xaml.Media.BrushCollection) compatibles con el lápiz personalizado (`CalligraphicPenPalette`).
```xaml
<Page.Resources>
    <!-- Add the custom CalligraphicPen to the page resources. -->
    <local:CalligraphicPen x:Key="CalligraphicPen" />
    <!-- Specify the colors for the palette of the custom pen. -->
    <BrushCollection x:Key="CalligraphicPenPalette">
        <SolidColorBrush Color="Blue" />
        <SolidColorBrush Color="Red" />
    </BrushCollection>
</Page.Resources>
```

2. A continuación, agregamos un control InkToolbar con un elemento [InkToolbarCustomPenButton](/uwp/api/windows.ui.xaml.controls.inktoolbarcustompenbutton) secundario.

  El botón de lápiz personalizado incluye las dos referencias de recursos estáticos declaradas en los recursos de página: `CalligraphicPen` y `CalligraphicPenPalette`.

  También especificamos el alcance del control deslizante de tamaño de trazo ([MinStrokeWidth](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth), [MaxStrokeWidth](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth) y [SelectedStrokeWidth](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty)), el pincel seleccionado ([SelectedBrushIndex](/uwp/api/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex)) y el icono para el botón de lápiz personalizado ([SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon)).
```xaml
<Grid Grid.Row="1">
    <InkCanvas x:Name="inkCanvas" />
    <InkToolbar x:Name="inkToolbar"
                VerticalAlignment="Top"
                TargetInkCanvas="{x:Bind inkCanvas}">
        <InkToolbarCustomPenButton
            CustomPen="{StaticResource CalligraphicPen}"
            Palette="{StaticResource CalligraphicPenPalette}"
            MinStrokeWidth="1" MaxStrokeWidth="3" SelectedStrokeWidth="2"
            SelectedBrushIndex ="1">
            <SymbolIcon Symbol="Favorite" />
            <InkToolbarCustomPenButton.ConfigurationContent>
                <InkToolbarPenConfigurationControl />
            </InkToolbarCustomPenButton.ConfigurationContent>
        </InkToolbarCustomPenButton>
    </InkToolbar>
</Grid>
```

### <a name="custom-toggle"></a>Alternancia personalizada

Puedes crear una alternancia personalizada (que se activa a través de un botón de alternancia personalizada) para activar o desactivar el estado de una característica definida por la aplicación. Cuando se activa, la característica funciona junto con la herramienta activa.

En este ejemplo, definimos un botón de alternancia personalizada que permite la entrada manuscrita con entrada táctil (de manera predeterminada, la entrada manuscrita táctil no está habilitada).

> [!NOTE]  
> Si necesitas compatibilidad con la entrada manuscrita mediante función táctil, te recomendamos habilitar esta opción mediante un botón de alternancia personalizada, con el icono y la información sobre herramientas especificados en este ejemplo.

Por lo general, la entrada táctil se utiliza para la manipulación directa de un objeto o de la interfaz de usuario de la aplicación. Para demostrar las diferencias de comportamiento cuando la entrada manuscrita táctil está habilitada, colocamos InkCanvas en un contenedor ScrollViewer y establecemos que las dimensiones del elemento ScrollViewer sean menores que las de InkCanvas. 

Cuando se inicia la aplicación, solo se admite la entrada manuscrita con lápiz y la entrada táctil se usa para realizar movimientos panorámicos o de zoom en la superficie de entrada manuscrita. Si la entrada manuscrita táctil está habilitada, no se pueden realizar movimientos panorámicos ni de zoom en la superficie de entrada manuscrita mediante la entrada táctil.

> [!NOTE]
> Consulta [Controles de entrada manuscrita](../controls-and-patterns/inking-controls.md) para conocer las directrices sobre la experiencia del usuario de [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) y [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar). Las siguientes recomendaciones son pertinentes para este ejemplo:
> - El control [**InkToolbar**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbar)y la entrada manuscrita en general se experimentan mejor a través de un lápiz activo. Sin embargo, la entrada manuscrita con el mouse y táctil puede admitirse si tu aplicación lo requiere. 
> - Si admites la entrada manuscrita con la entrada táctil, se recomienda usar el icono de "ED5F" de la fuente "Segoe MLD2 Assets" para el botón de alternancia, con una información sobre herramientas "Escritura táctil". 

**XAML**

1. En primer lugar, declaramos un elemento [**InkToolbarCustomToggleButton**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton) (toggleButton) con un agente de escucha de eventos Click que especifica el controlador de eventos (Toggle_Custom).

```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>

    <StackPanel Grid.Row="0" 
                x:Name="HeaderPanel" 
                Orientation="Horizontal">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10" />
    </StackPanel>

    <ScrollViewer Grid.Row="1" 
                  HorizontalScrollBarVisibility="Auto" 
                  VerticalScrollBarVisibility="Auto">
        
        <Grid HorizontalAlignment="Left" VerticalAlignment="Top">
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
            
            <InkToolbar Grid.Row="0" 
                        Margin="10"
                        x:Name="inkToolbar" 
                        VerticalAlignment="Top"
                        TargetInkCanvas="{x:Bind inkCanvas}">
                <InkToolbarCustomToggleButton 
                x:Name="toggleButton" 
                Click="CustomToggle_Click" 
                ToolTipService.ToolTip="Touch Writing">
                    <SymbolIcon Symbol="{x:Bind TouchWritingIcon}"/>
                </InkToolbarCustomToggleButton>
            </InkToolbar>
            
            <ScrollViewer Grid.Row="1" 
                          Height="500"
                          Width="500"
                          x:Name="scrollViewer" 
                          ZoomMode="Enabled" 
                          MinZoomFactor=".1" 
                          VerticalScrollMode="Enabled" 
                          VerticalScrollBarVisibility="Auto" 
                          HorizontalScrollMode="Enabled" 
                          HorizontalScrollBarVisibility="Auto">
                
                <Grid x:Name="outputGrid" 
                      Height="1000"
                      Width="1000"
                      Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}">
                    <InkCanvas x:Name="inkCanvas"/>
                </Grid>
                
            </ScrollViewer>
        </Grid>
    </ScrollViewer>
</Grid>
```

**Código subyacente**

2. En el fragmento anterior, declaramos un agente de escucha de eventos Click y un controlador (Toggle_Custom) en el botón de alternancia personalizada para la entrada manuscrita táctil (toggleButton). Este controlador activa o desactiva la compatibilidad con CoreInputDeviceTypes.Touch a través de la propiedad InputDeviceTypes del objeto InkPresenter.

   También hemos especificado un icono para el botón con el elemento SymbolIcon y la extensión de marcado {x: Bind} que lo enlaza a un campo definido en el archivo de código subyacente (TouchWritingIcon).

   El siguiente fragmento de código incluye el controlador de eventos Click y la definición de TouchWritingIcon.

```csharp 
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomToggle : Page
    {
        Symbol TouchWritingIcon = (Symbol)0xED5F;

        public MainPage_AddCustomToggle()
        {
            this.InitializeComponent();
        }

        // Handler for the custom toggle button that enables touch inking.
        private void CustomToggle_Click(object sender, RoutedEventArgs e)
        {
            if (toggleButton.IsChecked == true)
            {
                inkCanvas.InkPresenter.InputDeviceTypes |= CoreInputDeviceTypes.Touch;
            }
            else
            {
                inkCanvas.InkPresenter.InputDeviceTypes &= ~CoreInputDeviceTypes.Touch;
            }
        }
    }
}
```

### <a name="custom-tool"></a>Herramienta personalizada

Puedes crear un botón de herramienta personalizada para invocar una herramienta que no sea lápiz y que esté definida por la aplicación.

De manera predeterminada, [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) procesa todas las entradas como trazos de lápiz o trazos de borrado. Esto incluye la entrada que modificó una prestación de hardware secundaria, como el botón de menú contextual del lápiz, el botón secundario del mouse o similares. Sin embargo, [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) puede configurarse para dejar una entrada determinada sin procesar que, a continuación, se pasa a la aplicación para procesarla de forma personalizada.

En este ejemplo, definimos un botón de herramienta personalizada que, cuando se activa, hace que los trazos posteriores se procesen y se representen como un lazo de selección (línea discontinua) en lugar de como una entrada manuscrita. Todos los trazos de lápiz que se hagan dentro de los límites del área de selección se establecen en [**Selected**](/uwp/api/windows.ui.input.inking.inkstroke.selected).

> [!NOTE]
> Consulta Controles de entrada manuscrita para conocer las directrices sobre la experiencia del usuario de InkCanvas y InkToolbar. La siguiente recomendación es pertinente para este ejemplo:
> - Si se proporciona selección de trazo, se recomienda usar el icono de EF20 de la fuente Segoe MLD2 Assets para el botón de herramienta, con una información sobre herramientas "Herramienta de selección". 
 
**XAML**

1. En primer lugar, declaramos un elemento [**InkToolbarCustomToolButton**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) (customToolButton) con un agente de escucha de eventos Click que especifica el controlador de eventos (customToolButton_Click) donde se configura la selección de trazos. (También hemos agregado un conjunto de botones para copiar, cortar y pegar la selección de trazos).

2. También agregamos un elemento Canvas para dibujar el trazo de selección. Si usas una capa independiente para dibujar el trazo de selección, [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) y su contenido no se modifican. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" Orientation="Horizontal" Grid.Row="0">
        <TextBlock x:Name="Header" 
                   Text="Basic ink sample" 
                   Style="{ThemeResource HeaderTextBlockStyle}" 
                   Margin="10,0,0,0" />
    </StackPanel>
    <StackPanel x:Name="ToolPanel" Orientation="Horizontal" Grid.Row="1">
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="Top" 
                    TargetInkCanvas="{x:Bind inkCanvas}">
            <InkToolbarCustomToolButton 
                x:Name="customToolButton" 
                Click="customToolButton_Click" 
                ToolTipService.ToolTip="Selection tool">
                <SymbolIcon Symbol="{x:Bind SelectIcon}"/>
            </InkToolbarCustomToolButton>
        </InkToolbar>
        <Button x:Name="cutButton" 
                Content="Cut" 
                Click="cutButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="copyButton" 
                Content="Copy"  
                Click="copyButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
        <Button x:Name="pasteButton" 
                Content="Paste"  
                Click="pasteButton_Click"
                Width="100"
                Margin="5,0,0,0"/>
    </StackPanel>
    <Grid Grid.Row="2" x:Name="outputGrid" 
              Background="{ThemeResource SystemControlBackgroundChromeWhiteBrush}" 
              Height="Auto">
        <!-- Canvas for displaying selection UI. -->
        <Canvas x:Name="selectionCanvas"/>
        <!-- Canvas for displaying ink. -->
        <InkCanvas x:Name="inkCanvas" />
    </Grid>
</Grid>
```

**Código subyacente**

2. A continuación, controlamos el evento Click para [**InkToolbarCustomToolButton**](/uwp/api/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) en el archivo de código subyacente MainPage.xaml.cs.

   Este controlador configura [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) para pasar la entrada sin procesar a la aplicación. 

   Para obtener un paso más detallado de este código: Consulte la sección entrada de paso a través para el procesamiento avanzado de [interacciones de lápiz y Windows Ink en aplicaciones Windows](pen-and-stylus-interactions.md).

   También hemos especificado un icono para el botón con el elemento SymbolIcon y la extensión de marcado {x: Bind} que lo enlaza a un campo definido en el archivo de código subyacente (SelectIcon).

   El siguiente fragmento de código incluye el controlador de eventos Click y la definición de SelectIcon.

```csharp
namespace Ink_Basic_InkToolbar
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage_AddCustomTool : Page
    {
        // Icon for custom selection tool button.
        Symbol SelectIcon = (Symbol)0xEF20;

        // Stroke selection tool.
        private Polyline lasso;
        // Stroke selection area.
        private Rect boundingRect;

        public MainPage_AddCustomTool()
        {
            this.InitializeComponent();

            // Listen for new ink or erase strokes to clean up selection UI.
            inkCanvas.InkPresenter.StrokeInput.StrokeStarted +=
                StrokeInput_StrokeStarted;
            inkCanvas.InkPresenter.StrokesErased +=
                InkPresenter_StrokesErased;
        }

        private void customToolButton_Click(object sender, RoutedEventArgs e)
        {
            // By default, the InkPresenter processes input modified by 
            // a secondary affordance (pen barrel button, right mouse 
            // button, or similar) as ink.
            // To pass through modified input to the app for custom processing 
            // on the app UI thread instead of the background ink thread, set 
            // InputProcessingConfiguration.RightDragAction to LeaveUnprocessed.
            inkCanvas.InkPresenter.InputProcessingConfiguration.RightDragAction =
                InkInputRightDragAction.LeaveUnprocessed;

            // Listen for unprocessed pointer events from modified input.
            // The input is used to provide selection functionality.
            inkCanvas.InkPresenter.UnprocessedInput.PointerPressed +=
                UnprocessedInput_PointerPressed;
            inkCanvas.InkPresenter.UnprocessedInput.PointerMoved +=
                UnprocessedInput_PointerMoved;
            inkCanvas.InkPresenter.UnprocessedInput.PointerReleased +=
                UnprocessedInput_PointerReleased;
        }

        // Handle new ink or erase strokes to clean up selection UI.
        private void StrokeInput_StrokeStarted(
            InkStrokeInput sender, Windows.UI.Core.PointerEventArgs args)
        {
            ClearSelection();
        }

        private void InkPresenter_StrokesErased(
            InkPresenter sender, InkStrokesErasedEventArgs args)
        {
            ClearSelection();
        }

        private void cutButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
            inkCanvas.InkPresenter.StrokeContainer.DeleteSelected();
            ClearSelection();
        }

        private void copyButton_Click(object sender, RoutedEventArgs e)
        {
            inkCanvas.InkPresenter.StrokeContainer.CopySelectedToClipboard();
        }

        private void pasteButton_Click(object sender, RoutedEventArgs e)
        {
            if (inkCanvas.InkPresenter.StrokeContainer.CanPasteFromClipboard())
            {
                inkCanvas.InkPresenter.StrokeContainer.PasteFromClipboard(
                    new Point(0, 0));
            }
            else
            {
                // Cannot paste from clipboard.
            }
        }

        // Clean up selection UI.
        private void ClearSelection()
        {
            var strokes = inkCanvas.InkPresenter.StrokeContainer.GetStrokes();
            foreach (var stroke in strokes)
            {
                stroke.Selected = false;
            }
            ClearBoundingRect();
        }

        private void ClearBoundingRect()
        {
            if (selectionCanvas.Children.Any())
            {
                selectionCanvas.Children.Clear();
                boundingRect = Rect.Empty;
            }
        }

        // Handle unprocessed pointer events from modifed input.
        // The input is used to provide selection functionality.
        // Selection UI is drawn on a canvas under the InkCanvas.
        private void UnprocessedInput_PointerPressed(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Initialize a selection lasso.
            lasso = new Polyline()
            {
                Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                StrokeThickness = 1,
                StrokeDashArray = new DoubleCollection() { 5, 2 },
            };

            lasso.Points.Add(args.CurrentPoint.RawPosition);

            selectionCanvas.Children.Add(lasso);
        }

        private void UnprocessedInput_PointerMoved(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add a point to the lasso Polyline object.
            lasso.Points.Add(args.CurrentPoint.RawPosition);
        }

        private void UnprocessedInput_PointerReleased(
            InkUnprocessedInput sender, PointerEventArgs args)
        {
            // Add the final point to the Polyline object and 
            // select strokes within the lasso area.
            // Draw a bounding box on the selection canvas 
            // around the selected ink strokes.
            lasso.Points.Add(args.CurrentPoint.RawPosition);

            boundingRect =
                inkCanvas.InkPresenter.StrokeContainer.SelectWithPolyLine(
                    lasso.Points);

            DrawBoundingRect();
        }

        // Draw a bounding rectangle, on the selection canvas, encompassing 
        // all ink strokes within the lasso area.
        private void DrawBoundingRect()
        {
            // Clear all existing content from the selection canvas.
            selectionCanvas.Children.Clear();

            // Draw a bounding rectangle only if there are ink strokes 
            // within the lasso area.
            if (!((boundingRect.Width == 0) ||
                (boundingRect.Height == 0) ||
                boundingRect.IsEmpty))
            {
                var rectangle = new Rectangle()
                {
                    Stroke = new SolidColorBrush(Windows.UI.Colors.Blue),
                    StrokeThickness = 1,
                    StrokeDashArray = new DoubleCollection() { 5, 2 },
                    Width = boundingRect.Width,
                    Height = boundingRect.Height
                };

                Canvas.SetLeft(rectangle, boundingRect.X);
                Canvas.SetTop(rectangle, boundingRect.Y);

                selectionCanvas.Children.Add(rectangle);
            }
        }
    }
}
```



### <a name="custom-ink-rendering"></a>Representación de entrada de lápiz personalizada

De manera predeterminada, la entrada de lápiz se procesa en un subproceso en segundo plano de baja latencia y se representa como "húmeda" mientras se dibuja. Cuando se completa el trazo (se levanta el lápiz o el dedo o se libera el botón del mouse), el trazo se procesa en el subproceso de la interfaz de usuario y se representa como "seco" en la capa de [**InkCanvas**](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) (lo verás sobre el contenido de la aplicación y reemplazando la entrada de lápiz húmeda).

La plataforma de entrada de lápiz te permite invalidar este comportamiento y personalizar totalmente la experiencia de entrada de lápiz mediante el secado personalizado de la entrada de lápiz.

Para obtener más información sobre el secado personalizado, consulte [interacciones de lápiz y Windows Ink en aplicaciones de Windows](./pen-and-stylus-interactions.md#custom-ink-rendering).

> [!NOTE]
> El secado personalizado y la clase [**InkToolbar**](/uwp/api/windows.ui.xaml.controls.inktoolbar)  
> Si tu aplicación invalida el comportamiento predeterminado de representación de entrada de lápiz de la clase [**InkPresenter**](/uwp/api/Windows.UI.Input.Inking.InkPresenter) con una implementación de secado personalizado, los trazos de lápiz representados ya no están disponibles para InkToolbar y los comandos de borrado integrados de InkToolbar no funcionan según lo previsto. Para proporcionar la funcionalidad de borrado, debes controlar todos los eventos de puntero, realizar la prueba de posicionamiento en cada trazo e invalidar el comando "Borrar todas las entradas de lápiz" integrado.

## <a name="related-articles"></a>Artículos relacionados

- [Interacciones de pluma y lápiz](pen-and-stylus-interactions.md)

### <a name="topic-samples"></a>Ejemplos del tema

- [Ejemplo de posición y orientación de la barra de herramientas de lápiz (básico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
- [Ejemplo de posición y orientación de la barra de herramientas de lápiz (dinámico)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)

### <a name="other-samples"></a>Otras muestras

- [Ejemplo de entrada de lápiz simple (C#/C + +)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk)
- [Ejemplo de tinta compleja (C++)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk)
- [Ejemplo de entrada de lápiz (JavaScript)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BJavaScript%5D-Windows%208%20app%20samples/JavaScript/Windows%208%20app%20samples/Input%20Ink%20sample%20(Windows%208))
- [Tutorial de introducción: compatibilidad con la entrada de lápiz en la aplicación de Windows](https://github.com/Microsoft/Windows-tutorials-inputs-and-devices/tree/master/GettingStarted-Ink)
- [Muestra de libro para colorear](https://github.com/Microsoft/Windows-appsample-coloringbook)
- [Muestra de notas familiares](https://github.com/Microsoft/Windows-appsample-familynotes)