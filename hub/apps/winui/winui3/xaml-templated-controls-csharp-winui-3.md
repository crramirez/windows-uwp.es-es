---
description: En este artículo se recorren los pasos para la creación de un control XAML con plantilla para WinUI 3 con C#.
title: Controles XAML con plantilla para aplicaciones para WinUI 3 con C#
ms.date: 09/11/2020
ms.topic: article
keywords: windows 10, uwp, control personalizado, control con plantilla, winui
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 618bfc5a9937d29c546dc9420a1cba1c25fcc0ea
ms.sourcegitcommit: aabd6f40df6cc82bb8ce3a43275e4abd568c236f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061703"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-c"></a>Controles XAML con plantilla para aplicaciones para WinUI 3 con C#

En este artículo se recorren los pasos para la creación de un control XAML con plantilla para WinUI 3 con C#. Los controles con plantilla se heredan de **Microsoft.UI.Xaml.Controls.Control** y tienen una estructura y comportamiento visual que se pueden personalizar mediante plantillas de control XAML.

Antes de seguir los pasos de este artículo, debe asegurarse de que el entorno de desarrollo está configurado para crear aplicaciones de WinUI 3. Para información de configuración, consulte [Introducción a WinUI 3 para aplicaciones de escritorio](./get-started-winui3-for-desktop.md).

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Crear una aplicación en blanco (BgLabelControlApp)

Para empezar, crea un proyecto en Microsoft Visual Studio. En el cuadro de diálogo **Crear un proyecto nuevo**, seleccione la plantilla de proyecto **Aplicación vacía (WinUI en UWP)** y asegúrese de seleccionar la versión de lenguaje C#. Establezca el nombre del proyecto en "BgLabelControlApp" para que los nombres de archivo se alineen con el código en los ejemplos siguientes. Establezca **Versión de destino** en Windows 10, versión 1903 (compilación 18362) y **Versión mínima** en Windows 10, versión 1803 (compilación 17134). Este tutorial también funcionará para las aplicaciones de escritorio creadas con la plantilla de proyecto **Blank App, Packaged (WinUI in Desktop)** [Aplicación vacía, empaquetada (WinUI en el escritorio)], solo tiene que asegurarse de realizar todos los pasos del proyecto **BgLabelControlApp (escritorio)** .

![Plantilla de proyecto de aplicación vacía](images/winui-csharp-new-project-uwp.png)

## <a name="add-a-templated-control-to-your-app"></a>Adición de un control con plantilla a la aplicación

Para agregar un control con plantilla, haga clic en el menú **Proyecto** de la barra de herramientas o haga clic con el botón derecho en el proyecto en **Explorador de soluciones** y seleccione **Agregar nuevo elemento** . En **Visual C#->WinUI**, seleccione la plantilla **Control personalizado (WinUI**). Asigne al nuevo control el nombre "BgLabelControl" y haga clic en *Agregar*. Se agregarán dos nuevos archivos al proyecto. `BgLabelControl.cs` contiene el código subyacente para el control. 

## <a name="update-the-code-behind-file"></a>Actualización del archivo de código subyacente

En el archivo de código subyacente, BgLabelControl.xaml.cs, tenga en cuenta que el constructor define la propiedad **DefaultStyleKey** para el control. Esta clave identifica la plantilla predeterminada que se usará si el consumidor del control no especifica explícitamente ninguna plantilla. El valor de clave es el *tipo* de nuestro control. Veremos esta clave en uso más adelante cuando implementemos el archivo de plantilla genérico.

```csharp
public BgLabelControl()
{
    this.DefaultStyleKey = typeof(BgLabelControl);
}
```

Nuestro control con plantilla tendrá una etiqueta de texto que se puede establecer mediante programación en el código, en XAML o mediante el enlace de datos. Para que el sistema mantenga actualizado el texto de la etiqueta del control, es necesario implementarlo como [DependencyPropety](/uwp/api/Windows.UI.Xaml.DependencyProperty). Para ello, primero declaramos una propiedad de cadena y la denominamos **Label**. En lugar de usar una variable alternativa, establecemos y obtenemos el valor de nuestra propiedad de dependencia mediante una llamada a [GetValue](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) y [SetValue](/uwp/api/windows.ui.xaml.dependencyobject.setvalue). Estos métodos los proporciona [DependencyObject](/uwp/api/windows.ui.xaml.dependencyobject), que hereda el elemento **Microsoft.UI.Xaml.Controls.Control**.

```csharp
public string Label
{
    get => (string)GetValue(LabelProperty);
    set => SetValue(LabelProperty, value);
}
```
A continuación, declare la propiedad de dependencia y regístrela en el sistema mediante una llamada a [DependencyProperty.Register](/uwp/api/windows.ui.xaml.dependencyproperty.register). Este método especifica el nombre y el tipo de la propiedad **Label**, el tipo de propietario de la propiedad, nuestra clase **BgLabelControl** y el valor predeterminado de la propiedad.

```csharp
DependencyProperty LabelProperty = DependencyProperty.Register(
    nameof(Label), 
    typeof(string),
    typeof(BgLabelControl), 
    new PropertyMetadata(default(string)));
```

Estos dos pasos son todos necesarios para implementar una propiedad de dependencia, pero, en este ejemplo, vamos a agregar un controlador opcional para el evento **OnLabelChanged**. El sistema genera este evento cada vez que se actualiza el valor de la propiedad. En este caso, comprobamos si el nuevo texto de etiqueta es una cadena vacía o no y actualizamos una variable de clase según corresponda.

```csharp
public bool HasLabelValue { get; set; }

private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    {
        BgLabelControl labelControl = d as BgLabelControl; //null checks omitted
        String s = e.NewValue as String; //null checks omitted
        if (s == String.Empty)
        {
            labelControl.HasLabelValue = false;
        }
        else
        {
            labelControl.HasLabelValue = true;
        }
    }
}
```
Si quiere obtener más información acerca de cómo funcionan las propiedades de dependencia, consulte [Introducción a las propiedades de dependencia](/windows/uwp/xaml-platform/dependency-properties-overview).

## <a name="define-the-default-style-for-bglabelcontrol"></a>Diseño del estilo predeterminado para BgLabelControl
Un control con plantilla debe proporcionar una plantilla de estilo predeterminada que se utilice si el usuario del control no establece explícitamente un estilo. En este paso, crearemos un archivo de plantilla genérico para el control.

Asegúrate de que **Mostrar todos los archivos** esté activado (en el **Explorador de soluciones**). En el nodo del proyecto, crea una carpeta y asígnale el nombre "Themes". En `Themes`, agregue un nuevo elemento de tipo **Visual C# > WinUI > Diccionario de recursos (WinUI)** y asígnele el nombre "Generic.xaml". Los nombres de carpeta y archivo deben ser similares a este para que el marco XAML encuentre el estilo predeterminado para un control con plantilla. Elimine el contenido predeterminado de Generic.xaml y péguelo en el siguiente marcado.



```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

En este ejemplo, puede ver que el atributo **TargetType** del elemento **Style** está establecido en el tipo **BgLabelControl** del espacio de nombres **BgLabelControlApp**. Este tipo es el mismo valor que especificamos anteriormente para la propiedad **DefaultStyleKey** en el constructor del control, que lo identifica como el estilo predeterminado del control.

La propiedad **Text** de **TextBlock** de la plantilla de control está enlazada a la propiedad de dependencia **Label** del control. La propiedad se enlaza mediante la extensión de marcado [TemplateBinding](/windows/uwp/xaml-platform/templatebinding-markup-extension). En este ejemplo también se enlaza el fondo **Grid** a la propiedad de dependencia **Background** que se hereda de la clase **Control**.

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Adición de una instancia de BgLabelControl a la página principal de la interfaz de usuario

Abre `MainPage.xaml`, que contiene el marcado XAML de nuestra página principal de la interfaz de usuario. Inmediatamente después del elemento **Button** (dentro de la clase **StackPanel**), agrega el marcado siguiente.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Compile y ejecute la aplicación y verá el control con plantilla, con el color de fondo y la etiqueta especificados.

![Resultado del control con plantilla](images/winui-templated-control-result.png)


