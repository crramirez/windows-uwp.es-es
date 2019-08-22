---
description: En este artículo se muestra cómo hospedar un control estándar de UWP en una aplicación WPF mediante el uso de islas XAML.
title: Hospedar un control estándar de UWP en una aplicación WPF con islas XAML
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Islas XAML, controles ajustados, controles estándar, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: fc1b20561f67907fa973a0e30d0dc3b804c2472c
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643759"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hospedar un control estándar de UWP en una aplicación WPF con islas XAML

En este artículo se muestran dos maneras de hospedar un control estándar de UWP (es decir, un control UWP de primera parte proporcionado por la biblioteca Windows SDK o WinUI) en una aplicación WPF con [islas XAML](xaml-islands.md):

* Muestra cómo hospedar un control [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) y [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar) de UWP mediante [controles ajustados](xaml-islands.md#wrapped-controls) en el kit de herramientas de la comunidad de Windows. Estos controles encapsulan la interfaz y la funcionalidad de un pequeño conjunto de controles de UWP útiles. Puede agregarlos directamente a la superficie de diseño de su proyecto de WPF o Windows Forms y, a continuación, usarlos como cualquier otro control de WPF o Windows Forms en el diseñador.
* También se muestra cómo hospedar un control [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) de UWP mediante el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la comunidad de Windows. Dado que solo hay un pequeño conjunto de controles de UWP disponibles como controles encapsulados, puede usar [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar cualquier otro control estándar de UWP.

Aunque en este artículo se muestra cómo hospedar estos controles en una aplicación WPF, el proceso es similar para una aplicación Windows Forms.

## <a name="create-a-wpf-project"></a>Crear un proyecto de WPF

Antes de comenzar, siga estas instrucciones para crear un proyecto de WPF y configurarlo para hospedar islas XAML. Si tiene un proyecto de WPF existente, puede adaptar estos pasos y ejemplos de código para el proyecto.

1. En Visual Studio 2019, cree un nuevo proyecto de **aplicación de WPF (.NET Framework)** o de **aplicación WPF (.net Core)** .

2. Asegúrese de que [las referencias de paquete](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) están habilitadas:

    1. En Visual Studio, haga clic en **herramientas-> administrador de paquetes NuGet-> configuración del administrador de paquetes**.
    2. Asegúrese de que **PackageReference** está seleccionado para el **formato de administración de paquetes predeterminado**.

3. Haga clic con el botón derecho en el proyecto de WPF en **Explorador de soluciones** y elija **administrar paquetes NuGet**.

4. En la ventana **Administrador de paquetes NuGet** , asegúrese de que esté seleccionada la opción **incluir versión preliminar** .

5. Seleccione la pestaña **examinar** , busque el paquete [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) (versión v 6.0.0-preview7 o posterior) e instale el paquete. Este paquete proporciona todo lo necesario para usar los controles de UWP ajustados para WPF (incluidos [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) y el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) .
    > [!NOTE]
    > Windows Forms aplicaciones deben usar el paquete [Microsoft. Toolkit. Forms. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) (versión v 6.0.0-preview7 o posterior).

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>Hospedar un control InkCanvas e InkToolbar mediante controles ajustados

Ahora que ha configurado el proyecto para que use las islas XAML de UWP, ahora está listo para agregar los controles [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) ajustados a la aplicación.

1. En **Explorador de soluciones**, abra el archivo **MainWindow. Xaml** .

2. En el elemento **Window** situado cerca de la parte superior del archivo XAML, agregue el siguiente atributo. Esto hace referencia al espacio de nombres XAML para el control de UWP encapsulado para [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) y [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) .

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Después de agregar este atributo, el elemento de **ventana** debe tener ahora un aspecto similar a este.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

3. En el archivo **MainWindow. Xaml** , reemplace el elemento `<Grid>` existente por el código XAML siguiente. Este código XAML agrega un control [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) y [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) (precedido por la palabra clave **Controls** que definió anteriormente como `<Grid>`un espacio de nombres) a.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400"
            Margin="10,10,10,10" VerticalAlignment="Top" />
    </Grid>
    ```

    > [!NOTE]
    > También puede agregar estos y otros controles ajustados a la ventana arrastrándolos desde la sección del **Kit** de herramientas de la comunidad de Windows del **cuadro de herramientas** al diseñador.

4. Guarde el archivo **MainWindow. Xaml** .

    Si tiene un dispositivo que admite un lápiz digital, como una superficie, y está ejecutando este laboratorio en una máquina física, ahora puede compilar y ejecutar la aplicación y dibujar la tinta digital en la pantalla con el lápiz. Sin embargo, si no tiene un dispositivo compatible con el lápiz y intenta iniciar sesión con el mouse, no ocurrirá nada. Esto sucede porque el control **InkCanvas** solo está habilitado para los lápices digitales de forma predeterminada. Sin embargo, puede cambiar este comportamiento.

5. Abra el archivo **MainWindow.Xaml.CS** .

6. Agregue la siguiente declaración de espacio de nombres en la parte superior del archivo:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. Busque el `MainWindow()` constructor. Agregue la siguiente línea de código después del `InitializeComponent()` método y guarde el archivo de código.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Puede usar el objeto **InkPresenter** para personalizar la experiencia de entrada manuscrita predeterminada. Este código usa la propiedad **InputDeviceTypes** para habilitar el mouse, así como la entrada manuscrita.

8. Vuelva a presionar F5 para volver a compilar y ejecutar la aplicación en el depurador. Si utiliza un equipo con un mouse, confirme que puede dibujar algo en el espacio del lienzo de la tinta con el mouse.

## <a name="host-a-calendarview-by-using-the-host-control"></a>Hospedar un CalendarView mediante el control host

Ahora que ha agregado los controles [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) y [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) ajustado a la aplicación, ahora está listo para usar el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para agregar un [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) a la aplicación.

> [!NOTE]
> El control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) lo proporciona el paquete [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) . Este paquete se incluye con el paquete [Microsoft. Toolkit. WPF. UI. Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) que instaló anteriormente.

1. En **Explorador de soluciones**, abra el archivo **MainWindow. Xaml** .

2. En el elemento **Window** situado cerca de la parte superior del archivo XAML, agregue el siguiente atributo. Esto hace referencia al espacio de nombres XAML para el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) .

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Después de agregar este atributo, el elemento de **ventana** debe tener ahora un aspecto similar a este.

    ```xml
    <Window xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:WpfApp"
            xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
            xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
            x:Class="WpfApp.MainWindow"
            mc:Ignorable="d"
            Title="MainWindow" Height="800" Width="800">
    ```

4. En el archivo **MainWindow. Xaml** , reemplace el elemento `<Grid>` existente por el código XAML siguiente. Este código XAML agrega una fila a la cuadrícula y agrega un objeto [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) a la última fila. Para hospedar un control [CalendarView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CalendarView) de UWP, este XAML `InitialTypeName` establece la propiedad en el nombre completo del control. Este código XAML también define un controlador de eventos `ChildChanged` para el evento, que se genera cuando se ha representado el control hospedado.

    ```xml
    <Grid Margin="10,50,10,10">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <Controls:InkToolbar x:Name="myInkToolbar" TargetInkCanvas="{x:Reference myInkCanvas}" Grid.Row="0" Width="300"
            Height="50" Margin="10,10,10,10" HorizontalAlignment="Left" VerticalAlignment="Top" />
        <Controls:InkCanvas x:Name="myInkCanvas" Grid.Row="1" HorizontalAlignment="Left" Width="600" Height="400" 
            Margin="10,10,10,10" VerticalAlignment="Top" />
        <xamlhost:WindowsXamlHost x:Name="myCalendar" InitialTypeName="Windows.UI.Xaml.Controls.CalendarView" Grid.Row="2" 
              Margin="10,10,10,10" Width="600" Height="300" ChildChanged="MyCalendar_ChildChanged"  />
    </Grid>
    ```

5. Guarde el archivo **MainWindow. Xaml** y abra el archivo **MainWindow.Xaml.CS** .

7. Agregue la siguiente declaración de espacio de nombres en la parte superior del archivo:

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. Agregue el siguiente `ChildChanged` método de controlador de eventos `MainWindow` a la clase y guarde el archivo de código. Cuando se ha representado el control host, este controlador de eventos se ejecuta y crea un controlador de eventos simple `SelectedDatesChanged` para el evento del control de calendario.

    ```csharp
    private void MyCalendar_ChildChanged(object sender, EventArgs e)
    {
        WindowsXamlHost windowsXamlHost = (WindowsXamlHost)sender;

        Windows.UI.Xaml.Controls.CalendarView calendarView =
            (Windows.UI.Xaml.Controls.CalendarView)windowsXamlHost.Child;

        if (calendarView != null)
        {
            calendarView.SelectedDatesChanged += (obj, args) =>
            {
                if (args.AddedDates.Count > 0)
                {
                    MessageBox.Show("The user selected a new date: " + 
                        args.AddedDates[0].DateTime.ToString());
                }
            };
        }
    }
    ```

11. Vuelva a presionar F5 para volver a compilar y ejecutar la aplicación en el depurador. Confirme que el control de calendario se muestra ahora en la parte inferior de la ventana.

## <a name="package-the-app"></a>Empaquetar la aplicación

Opcionalmente, puede empaquetar la aplicación WPF en un [paquete de MSIX](https://docs.microsoft.com/windows/msix) para la implementación. MSIX es la tecnología moderna de empaquetado de aplicaciones para Windows y se basa en una combinación de tecnologías de instalación de MSI, APPX, App-V y ClickOnce.

Las instrucciones siguientes muestran cómo empaquetar todos los componentes de la solución en un paquete de MSIX mediante el proyecto de paquete de [aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) en Visual Studio 2019. Estos pasos solo son necesarios si desea empaquetar la aplicación WPF en un paquete MSIX.

1. Agregue un nuevo [proyecto de paquete de aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución. Cuando cree el proyecto, seleccione **Windows 10, versión 1903 (10,0; Compilación 18362)** para la **versión de destino** y la **versión mínima**.

2. En el proyecto de empaquetado, haga clic con el botón secundario en el nodo **aplicaciones** y elija **Agregar referencia**. En la lista de proyectos, seleccione el proyecto de WPF en la solución y haga clic en **Aceptar**.

3. Si el proyecto de WPF tiene como destino .NET Core 3, debe editar el archivo de proyecto de empaquetado. Estos cambios son necesarios actualmente para empaquetar aplicaciones WPF destinadas a .NET Core 3 y que hospedan controles de UWP.

    1. En Explorador de soluciones, haga clic con el botón secundario en el nodo del proyecto de empaquetado y seleccione **Editar archivo de proyecto**.
    2. Busca el elemento `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` en el archivo. Reemplace este elemento por el siguiente código XML.

        ``` xml
        <ItemGroup>
            <SDKReference Include="Microsoft.VCLibs,Version=14.0">
            <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
            <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
            <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
            <Implicit>true</Implicit>
            </SDKReference>
        </ItemGroup>
        <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
        <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
            <ItemGroup>
            <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
            <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
            <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
                <SourceProject>
                </SourceProject>
            </_FilteredNonWapProjProjectOutput>
            </ItemGroup>
        </Target>
        ```

    3. Guarde el archivo de proyecto y ciérrelo.

4. Configure la solución para que tenga como destino una plataforma específica, como x86 o x64. Esto es necesario para compilar la aplicación de WPF en un paquete de MSIX mediante el proyecto de paquete de aplicación de Windows.

    1. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución y seleccione **propiedades** -> **configuración propiedades** -> **Configuration Manager**.
    2. En **plataforma de soluciones activas**, seleccione **x64** o **x86**.
    3. En la fila del proyecto de WPF, en la columna **plataforma** , seleccione **nuevo**.
    4. En el cuadro de diálogo **nueva plataforma de solución** , seleccione **x64** o **x86** (la misma plataforma que seleccionó para **plataforma de soluciones activa**) y haga clic en **Aceptar**.
    5. Cierre los cuadros de diálogo abiertos.

5. Compile y ejecute el proyecto de empaquetado. Confirme que se ejecuta WPF y que el control personalizado de UWP se muestra como se esperaba.

## <a name="related-topics"></a>Temas relacionados

* [Controles de UWP en aplicaciones de escritorio](xaml-islands.md)
* [InkCanvas](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)