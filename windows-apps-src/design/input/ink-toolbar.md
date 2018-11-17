---
author: Karl-Bridge-Microsoft
Description: Add a default InkToolbar to a Universal Windows Platform (UWP) inking app, add a custom pen button to the InkToolbar, and bind the custom pen button to a custom pen definition.
title: Agregar un control InkToolbar a una aplicación para la Plataforma universal de Windows (UWP)
label: Add an InkToolbar to a Universal Windows Platform (UWP) app
template: detail.hbs
keywords: Windows Ink, entrada manuscrita de Windows, DirectInk, InkPresenter, InkCanvas, InkToolbar, Plataforma universal de Windows, UWP, interacción del usuario, entrada
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.assetid: d888f75f-c2a0-4134-81db-907b5e24fcc5
ms.localizationpriority: medium
ms.openlocfilehash: b6896a4c149084dd5609f2ac6737c803a18d14ac
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "7160863"
---
# <a name="add-an-inktoolbar-to-a-universal-windows-platform-uwp-app"></a>Agregar un control InkToolbar a una aplicación para la Plataforma universal de Windows (UWP)



Hay dos controles diferentes que facilitan la entrada manuscrita en aplicaciones para la Plataforma universal de Windows (UWP): [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) e [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx).

El control [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx) proporciona la funcionalidad básica de Windows Ink. Úsalo para representar la entrada manuscrita como un trazo de lápiz (con la configuración predeterminada de color y espesor) o como un trazo de borrado.

> Para obtener detalles sobre la implementación del control InkCanvas, consulta [Interacciones de pluma y lápiz en aplicaciones para UWP](pen-and-stylus-interactions.md).

Como una superposición completamente transparente, el control InkCanvas no proporciona ninguna interfaz de usuario integrada para establecer propiedades de trazo de lápiz. Si quieres cambiar la experiencia de entrada manuscrita predeterminada, permite que los usuarios establezcan las propiedades de trazo de lápiz y admite otras características de entrada manuscrita personalizadas. Para hacerlo, tienes dos opciones:

- En el código subyacente, usa el objeto subyacente [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx) enlazado al control InkCanvas.

  Las API de InkPresenter admiten una amplia personalización de la experiencia de entrada manuscrita. Para obtener más información, consulta [Interacciones de pluma y lápiz en aplicaciones para UWP](pen-and-stylus-interactions.md).

- Enlazar un control [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) al control InkCanvas. De manera predeterminada, el control InkToolbar incluye una colección de botones personalizable y ampliable para activar las funciones relacionadas con la entrada de lápiz, como el tamaño del trazo, el color de la entrada de lápiz y la forma de la punta del lápiz.

  El control InkToolbar se describe en este tema.

> **API importantes**: [**InkCanvas class**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inkcanvas.aspx), [**InkToolbar class**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx), [**InkPresenter class**](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkpresenter.aspx), [**Windows.UI.Input.Inking**](https://msdn.microsoft.com/library/windows/apps/br208524)

## <a name="default-inktoolbar"></a>Control InkToolbar predeterminado

De manera predeterminada, el control [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) incluye botones para dibujar, borrar, resaltar y mostrar una plantilla (regla o transportador). Dependiendo de la característica, en un control flotante se proporcionan otros comandos y opciones de configuración, como los destinados a definir el color y el grosor del trazo o a borrar todas las entradas de lápiz.

![InkToolbar](./images/ink/ink-tools-invoked-toolbar-small.png)  
*Barra de herramientas de Windows Ink predeterminada*

Para agregar un control [**InkToolbar**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) predeterminado a una aplicación de entrada de lápiz, solo tienes que colocarlo en la misma página que el control [**InkCanvas**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) y asociar los dos controles.

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

Al agregar una barra de herramientas de Ink a tu aplicación, puedes aceptar la ubicación y orientación predeterminadas de la barra de herramientas o establecerlas según requiera tu aplicación o usuario.

**XAML**

Especifica de forma explícita la ubicación y orientación de la barra de herramientas a través de sus propiedades [VerticalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.VerticalAlignment), [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment) y [Orientation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar?branch=rs3.Orientation).

| Predeterminadas | Explícitas |
| --- | --- |
| ![Ubicación y orientación de la barra de herramientas de Ink predeterminadas](./images/ink/location-default-small.png) | ![Ubicación y orientación de la barra de herramientas de Ink explícitas](./images/ink/location-explicit-small.png) |
| *Ubicación y orientación predeterminadas de la barra de herramientas de Windows Ink* | *Ubicación y orientación explícitas de la barra de herramientas de Windows Ink* |

Este es el código para establecer de forma explícita la ubicación y orientación de la barra de herramientas de Ink en XAML.
```xaml
<InkToolbar x:Name="inkToolbar" 
    VerticalAlignment="Center" 
    HorizontalAlignment="Right" 
    Orientation="Vertical" 
    TargetInkCanvas="{x:Bind inkCanvas}" />
```

**Inicializar en función de las preferencias del usuario o el estado del dispositivo**

En algunos casos, es posible que quieras establecer la ubicación y orientación de la barra de herramientas de Ink en función de la preferencia del usuario o el estado del dispositivo. En el siguiente ejemplo se muestra cómo establecer la ubicación y orientación de la barra de herramientas de Ink en función de las preferencias de escritura con la mano izquierda o derecha especificadas a través de **Configuración > Dispositivos > Lápiz y Windows Ink > Lápiz > Elige la mano con la que escribes**.

![Ajuste de mano dominante](./images/ink/location-handedness-setting.png)  
*Ajuste de mano dominante*

Puedes consultar este ajuste a través de la propiedad HandPreference de Windows.UI.ViewManagement y establecer la propiedad [HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.HorizontalAlignment) en función del valor devuelto. En este ejemplo, encontramos la barra de herramientas en el lado izquierdo de la aplicación para una persona zurda y en el lado derecho para una persona diestra.

**Descargar este ejemplo de [Ejemplo de ubicación y orientación (básicas) de la barra de herramientas de Ink](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)**

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

**Ajustar de forma dinámica al estado del dispositivo o usuario**

También puedes usar el enlace para encargarte de las actualizaciones de la interfaz de usuario en función de los cambios en las preferencias del usuario, la configuración o los estados del dispositivo. En el siguiente ejemplo, expandimos el ejemplo anterior y mostramos cómo posicionar de forma dinámica la barra de herramientas de Ink en función de la orientación del dispositivo con enlaces, un objeto ViewMOdel y la interfaz [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged). 

**Descargar este ejemplo de [Ejemplo de ubicación y orientación (dinámicas) de la barra de herramientas de Ink](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)**

1. En primer lugar, vamos a agregar nuestro ViewModel.
    1. Agrega una nueva carpeta a tu proyecto y llámala **ViewModels**.
    1. Agrega una nueva clase a la carpeta ViewModels (en este ejemplo, la llamamos **InkToolbarSnippetHostViewModel.cs**).
        > [!NOTE] 
        > Usamos el [patrón de singleton](https://msdn.microsoft.com/library/ff650849.aspx) porque solo necesitamos un objeto de este tipo durante la vigencia de la aplicación.

    1. Agrega el espacio de nombres `using System.ComponentModel` al archivo.
    1. Agrega una variable de miembro estática llamada **instance** y una propiedad de solo lectura estática llamada **Instance**. Convierte el constructor en privado para garantizar que solo se pueda tener acceso a esta clase a través de la propiedad Instance.   
        > [!NOTE] 
        > Esta clase hereda de la interfaz [INotifyPropertyChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.inotifypropertychanged), que se usa para notificar a los clientes, normalmente de enlace, que un valor de propiedad ha cambiado. Usaremos esta para controlar los cambios en la orientación del dispositivo (expandiremos este código y facilitaremos más detalles en un paso posterior).  

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

    1. Agrega dos propiedades bool a la clase InkToolbarSnippetHostViewModel: **LeftHandedLayout** (misma funcionalidad que el ejemplo de solo XAML anterior) y **PortraitLayout** (orientación del dispositivo).
        >[!NOTE] 
        > La propiedad PortraitLayout se puede establecer e incluye la definición para el evento [PropertyChanged](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged).

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

1. Ahora, vamos a agregar un par de clases del convertidor a nuestro proyecto. Cada clase contiene un objeto Convert que devuelve un valor de alineación ([HorizontalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.horizontalalignment) o [VerticalAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.verticalalignment)).
    1. Agrega una nueva carpeta a tu proyecto y llámala **Converters**.
    1. Agrega dos nuevas clases a la carpeta Converters (en este ejemplo, las llamamos **HorizontalAlignmentFromHandednessConverter.cs** y **VerticalAlignmentFromAppViewConverter.cs**).
    1. Agrega los espacios de nombres `using Windows.UI.Xaml` y `using Windows.UI.Xaml.Data` a cada archivo.
    1. Cambia cada clase por `public` y especifica que implementa la interfaz [IValueConverter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.ivalueconverter).
    1. Agrega los métodos [Convert](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) y [ConvertBack](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) a cada archivo, como se muestra aquí (dejamos el método ConvertBack sin implementar).
        - HorizontalAlignmentFromHandednessConverter coloca la barra de herramientas de Ink en el lado derecho de la aplicación para usuarios diestros y en el lado izquierdo de la aplicación para usuarios zurdos.
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

        - VerticalAlignmentFromAppViewConverter coloca la barra de herramientas de Ink en el centro de la aplicación para la orientación vertical y en la parte superior de la aplicación para la orientación horizontal (aunque está pensada para mejorar la facilidad de uso, se trata solo de una opción arbitraria con fines de demostración).
        ````csharp
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

1. Now, open the MainPage.xaml.cs file.
    1. Add `using using locationandorientation.ViewModels` to the list of namespaces to associate our ViewModel.
    1. Add `using Windows.UI.ViewManagement` to the list of namespaces to enable listening for changes to the device orientation.
    1. Add the [WindowSizeChangedEventHandler](https://docs.microsoft.com/uwp/api/windows.ui.xaml.windowsizechangedeventhandler) code.
    1. Set the [DataContext](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement.DataContext) for the view to the singleton instance of the InkToolbarSnippetHostViewModel class. 
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

1. Next, open the MainPage.xaml file.
    1. Add `xmlns:converters="using:locationandorientation.Converters"` to the `Page` element for binding to our converters.
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

    1. Add a `PageResources` element and specify references to our converters.
        ```xaml
        <Page.Resources>
            <converters:HorizontalAlignmentFromHandednessConverter x:Key="HorizontalAlignmentConverter"/>
            <converters:VerticalAlignmentFromAppViewConverter x:Key="VerticalAlignmentConverter"/>
        </Page.Resources>
        ```

    1. Add the InkCanvas and InkToolbar elements and bind the VerticalAlignment and HorizontalAlignment properties of the InkToolbar.
        ```xaml
        <InkCanvas x:Name="inkCanvas" />
        <InkToolbar x:Name="inkToolbar" 
                    VerticalAlignment="{Binding PortraitLayout, Converter={StaticResource VerticalAlignmentConverter} }" 
                    HorizontalAlignment="{Binding LeftHandedLayout, Converter={StaticResource HorizontalAlignmentConverter} }" 
                    Orientation="Vertical" 
                    TargetInkCanvas="{x:Bind inkCanvas}" />
        ```

1. Return to the InkToolbarSnippetHostViewModel.cs file to add our `PortraitLayout` and `LeftHandedLayout` bool properties to the `InkToolbarSnippetHostViewModel` class, along with support for rebinding `PortraitLayout` when that property value changes. 
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

You should now have an inking app that adapts to both the dominant hand preference of the user and dynamically responds to the orientation of the user's device.

### Specify the selected button  
![Pencil button selected at initialization](./images/ink/ink-tools-default-toolbar.png)  
*Windows Ink toolbar with pencil button selected at initialization*

By default, the first (or leftmost) button is selected when your app is launched and the toolbar is initialized. In the default Windows Ink toolbar, this is the ballpoint pen button.

Because the framework defines the order of the built-in buttons, the first button might not be the pen or tool you want to activate by default.

You can override this default behavior and specify the selected button on the toolbar.

For this example, we initialize the default toolbar with the pencil button selected and the pencil activated (instead of the ballpoint pen).

1. Use the XAML declaration for the InkCanvas and InkToolbar from the previous example.
2. In code-behind, set up a handler for the [Loaded](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) event of the [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) object.

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

3. In the handler for the [Loaded](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loaded.aspx) event:
    1. Get a reference to the built-in [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx).

    Passing an [InkToolbarTool.Pencil](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbartool.aspx) object in the [GetToolButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.gettoolbutton.aspx) method returns an [InkToolbarToolButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbartoolbutton.aspx) object for the [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx).

    2. Set [ActiveTool](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.activetool.aspx) to the object returned in the previous step.

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

### Specify the built-in buttons

![Specific buttons included at initialization](./images/ink/ink-tools-specific.png)  
*Specific buttons included at initialization*

As mentioned, the Windows Ink toolbar includes a collection of default, built-in buttons. These buttons are displayed in the following order (from left to right):

- [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx)
- [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx)
- [InkToolbarHighlighterButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarhighlighterbutton.aspx)
- [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx)
- [InkToolbarRulerButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarrulerbutton.aspx)

For this example, we initialize the toolbar with only the built-in ballpoint pen, pencil, and eraser buttons.

You can do this using either XAML or code-behind.

**XAML**

Modify the XAML declaration for the InkCanvas and InkToolbar from the first example.
- Add an [InitialControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) attribute and set its value to "[None](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)". This clears the default collection of built-in buttons.
- Add the specific InkToolbar buttons required by your app. Here, we add [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx), and [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx) only.
> [!NOTE]
> Buttons are added to the toolbar in the order defined by the framework, not the order specified here.

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

**Code-behind**
1. Use the XAML declaration for the InkCanvas and InkToolbar from the first example.

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

2. In code-behind, set up a handler for the [Loading](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.loading.aspx) event of the [InkToolbar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx) object.

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

3. Set [InitialControls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.initialcontrols.aspx) to "[None](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarinitialcontrols.aspx)".
4. Create object references for the buttons required by your app. Here, we add [InkToolbarBallpointPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarballpointpenbutton.aspx), [InkToolbarPencilButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpencilbutton.aspx), and [InkToolbarEraserButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbareraserbutton.aspx) only.
  > [!NOTE]
  > Buttons are added to the toolbar in the order defined by the framework, not the order specified here.

5. [Add](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.dependencyobjectcollection.add.aspx) the buttons to the InkToolbar.

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

## Custom buttons and inking features

You can customize and extend the collection of buttons (and associated inking features) that are provided through the InkToolbar.

The InkToolbar consists of two distinct groups of button types:

1. A group of "tool" buttons containing the built-in drawing, erasing, and highlighting buttons. Custom pens and tools are added here.
> **Note**&nbsp;&nbsp;Feature selection is mutually exclusive.

2. A group of "toggle" buttons containing the built-in ruler button. Custom toggles are added here.
> **Note**&nbsp;&nbsp;Features are not mutually exclusive and can be used concurrently with other active tools.

Depending on your application and the inking functionality required, you can add any of the following buttons (bound to your custom ink features) to the InkToolbar:

- Custom pen – a pen for which the ink color palette and pen tip properties, such as shape, rotation, and size, are defined by the host app.
- Custom tool – a non-pen tool, defined by the host app.
- Custom toggle – Sets the state of an app-defined feature to on or off. When turned on, the feature works in conjunction with the active tool.

> **Note**&nbsp;&nbsp;You cannot change the display order of the built-in buttons. The default display order is: Ballpoint pen, pencil, highlighter, eraser, and ruler. Custom pens are appended to the last default pen, custom tool buttons are added between the last pen button and the eraser button and custom toggle buttons are added after the ruler button. (Custom buttons are added in the order they are specified.)

### Custom pen

You can create a custom pen (activated through a custom pen button) where you define the ink color palette and pen tip properties, such as shape, rotation, and size.

![Custom calligraphic pen button](./images/ink/ink-tools-custompen.png)  
*Custom calligraphic pen button*

For this example, we define a custom pen with a broad tip that enables basic calligraphic ink strokes. We also customize the collection of brushes in the palette displayed on the button flyout.

**Code-behind**

First, we define our custom pen and specify the drawing attributes in code-behind. We reference this custom pen from XAML later.

1. Right click the project in Solution Explorer and select Add -> New item.
2. Under Visual C# -> Code, add a new Class file and call it CalligraphicPen.cs.
3. In Calligraphic.cs, replace the default using block with the following:
```csharp
using System.Numerics;
using Windows.UI;
using Windows.UI.Input.Inking;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
```

4. Specify that the CalligraphicPen class is derived from [InkToolbarCustomPen](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.aspx).
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
}
```

5. Override  [CreateInkDrawingAttributesCore](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompen.createinkdrawingattributescore.aspx)  to specify your own brush and stroke size.
```csharp
class CalligraphicPen : InkToolbarCustomPen
{
    protected override InkDrawingAttributes
      CreateInkDrawingAttributesCore(Brush brush, double strokeWidth)
    {
    }
}
```

6. Create an [InkDrawingAttributes](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.aspx) object and set the [pen tip shape](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentip.aspx), [tip rotation](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.pentiptransform.aspx), [stroke size](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.size.aspx), and [ink color](https://msdn.microsoft.com/library/windows/apps/windows.ui.input.inking.inkdrawingattributes.color.aspx).
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

Next, we add the necessary references to the custom pen in MainPage.xaml.

1. We declare a local page resource dictionary that creates a reference to the custom pen (`CalligraphicPen`) defined in CalligraphicPen.cs, and a [brush collection](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.BrushCollection.aspx) supported by the custom pen (`CalligraphicPenPalette`).
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

2. We then add an InkToolbar with a child [InkToolbarCustomPenButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarcustompenbutton.aspx) element.

  The custom pen button includes the two static resource references declared in the page resources: `CalligraphicPen` and `CalligraphicPenPalette`.

  We also specify the range for the stroke size slider ([MinStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.minstrokewidth.aspx), [MaxStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.maxstrokewidth.aspx), and [SelectedStrokeWidth](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedstrokewidthproperty.aspx)), the selected brush ([SelectedBrushIndex](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbarpenbutton.selectedbrushindex.aspx)), and the icon for the custom pen button ([SymbolIcon](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.symbolicon.aspx)).
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

### Custom toggle

You can create a custom toggle (activated through a custom toggle button) to set the state of an app-defined feature to on or off. When turned on, the feature works in conjunction with the active tool.

In this example, we define a custom toggle button that enables inking with touch input (by default, touch inking is not enabled).

> [!NOTE]  
> If you need to support inking with touch, we recommended that you enable it using a CustomToggleButton, with the icon and tooltip specified in this example.

Typically, touch input is used for direct manipulation of an object or the app UI. To demonstrate the differences in behavior when touch inking is enabled, we place the InkCanvas within a ScrollViewer container and set the dimensions of the ScrollViewer to be smaller than the InkCanvas. 

When the app starts, only pen inking is supported and touch is used to pan or zoom the inking surface. When touch inking is enabled, the inking surface cannot be panned or zoomed through touch input.

> [!NOTE]
> See [Inking controls](../controls-and-patterns/inking-controls.md) for both [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) and [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar) UX guidelines. The following recommendations are relevant to this example:
> - The [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar), and inking in general, is best experienced through an active pen. However, inking with mouse and touch can be supported if required by your app. 
> - If supporting inking with touch input, we recommend using the "ED5F" icon from the "Segoe MLD2 Assets" font for the toggle button, with a "Touch writing" tooltip. 

**XAML**

1. First, we declare an [**InkToolbarCustomToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToggleButton) element (toggleButton) with a Click event listener that specifies the event handler (Toggle_Custom).

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

**Code-behind**

2. In the previous snippet, we declared a Click event listener and handler (Toggle_Custom) on the custom toggle button for touch inking (toggleButton). This handler simply toggles support for CoreInputDeviceTypes.Touch through the InputDeviceTypes property of the InkPresenter.

   We also specified an icon for the button using the SymbolIcon element and the {x:Bind} markup extension that binds it to a field defined in the code-behind file (TouchWritingIcon).

   The following snippet includes both the Click event handler and the definition of TouchWritingIcon.

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

### Custom tool

You can create a custom tool button to invoke a non-pen tool that is defined by your app.

By default, an [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) processes all input as either an ink stroke or an erase stroke. This includes input modified by a secondary hardware affordance such as a pen barrel button, a right mouse button, or similar. However, [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) can be configured to leave specific input unprocessed, which can then be passed through to your app for custom processing.

In this example, we define a custom tool button that, when selected, causes subsequent strokes to be processed and rendered as a selection lasso (dashed line) instead of ink. All ink strokes within the bounds of the selection area are set to [**Selected**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkStroke.Selected).

> [!NOTE]
> See Inking controls for both InkCanvas and InkToolbar UX guidelines. The following recommendation is relevant to this example:
> - If providing stroke selection, we recommend using the "EF20" icon from the "Segoe MLD2 Assets" font for the tool button, with a "Selection tool" tooltip. 
 
**XAML**

1. First, we declare an [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) element (customToolButton) with a Click event listener that specifies the event handler (customToolButton_Click) where stroke selection is configured. (We've also added a set of buttons for copying, cutting, and pasting the stroke selection.)

2. We also add a Canvas element for drawing our selection stroke. Using a separate layer to draw the selection stroke ensures the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) and its content remain untouched. 

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

**Code-behind**

2. We then handle the Click event for the [**InkToolbarCustomToolButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbarCustomToolButton) in the MainPage.xaml.cs code-behind file.

   This handler configures the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.Inking.InkPresenter) to pass unprocessed input through to the app. 

   For a more detailed step through of this code:  See the Pass-through input for advanced processing section of [Pen interactions and Windows Ink in UWP apps](pen-and-stylus-interactions.md).

   We also specified an icon for the button using the SymbolIcon element and the {x:Bind} markup extension that binds it to a field defined in the code-behind file (SelectIcon).

   The following snippet includes both the Click event handler and the definition of SelectIcon.

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



### Custom ink rendering

By default, ink input is processed on a low-latency background thread and rendered "wet" as it is drawn. When the stroke is completed (pen or finger lifted, or mouse button released), the stroke is processed on the UI thread and rendered "dry" to the [**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/dn858535) layer (above the application content and replacing the wet ink).

The ink platform enables you to override this behavior and completely customize the inking experience by custom drying the ink input.

For more info on custom drying, see [Pen interactions and Windows Ink in UWP apps](https://msdn.microsoft.com/windows/uwp/input/pen-and-stylus-interactions#custom-ink-rendering).

> [!NOTE]
> Custom drying and the [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.inktoolbar.aspx)  
> If your app overrides the default ink rendering behavior of the [**InkPresenter**](https://msdn.microsoft.com/library/windows/apps/dn922011) with a custom drying implementation, the rendered ink strokes are no longer available to the InkToolbar and the built-in erase commands of the InkToolbar do not work as expected. To provide erase functionality, you must handle all pointer events, perform hit-testing on each stroke, and override the built-in "Erase all ink" command.

## Related articles

* [Pen and stylus interactions](pen-and-stylus-interactions.md)

**Topic samples**
* [Ink toolbar location and orientation sample (basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness.zip)
* [Ink toolbar location and orientation sample (dynamic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-ink-toolbar-handedness-dynamic.zip)

**Other samples**
* [Simple ink sample (C#/C++)](http://go.microsoft.com/fwlink/p/?LinkID=620312)
* [Complex ink sample (C++)](http://go.microsoft.com/fwlink/p/?LinkID=620314)
* [Ink sample (JavaScript)](http://go.microsoft.com/fwlink/p/?LinkID=620308)
* [Get Started Tutorial: Support ink in your UWP app](https://aka.ms/appsample-ink)
* [Coloring book sample](https://aka.ms/cpubsample-coloringbook)
* [Family notes sample](https://aka.ms/cpubsample-familynotessample)

