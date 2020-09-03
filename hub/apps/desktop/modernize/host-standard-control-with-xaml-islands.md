---
description: En este artículo se muestra cómo hospedar un control estándar de UWP en una aplicación de WPF mediante las islas XAML.
title: Hospedaje de un control estándar de UWP en una aplicación de WPF mediante las islas XAML
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, wrapped controls, standard controls, InkCanvas, InkToolbar
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 0e8972a71feacd593edf98853ae1dcc0f88002fd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168899"
---
# <a name="host-a-standard-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hospedaje de un control estándar de UWP en una aplicación de WPF mediante las islas XAML

En este artículo se muestran dos maneras de hospedar un control estándar de UWP (es decir, un control de UWP de origen proporcionado por Windows SDK) en una aplicación de WPF mediante las [Islas XAML](xaml-islands.md):

* Muestra cómo hospedar los controles [InkCanvas](/uwp/api/Windows.UI.Xaml.Controls.InkCanvas) y [InkToolbar](/uwp/api/windows.ui.xaml.controls.inktoolbar) de UWP con [controles encapsulados](xaml-islands.md#wrapped-controls) en el kit de herramientas de la comunidad de Windows. Estos controles encapsulan la interfaz y la funcionalidad de un pequeño conjunto de controles de UWP útiles. Puedes agregarlos directamente en la superficie de diseño de tu proyecto de WPF o Windows Forms y, luego, usarlos como cualquier otro control de WPF o Windows Forms en el diseñador.

* También se muestra cómo hospedar un control [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) de UWP mediante el control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la comunidad de Windows. Dado que solo hay un pequeño conjunto de controles de UWP disponibles como controles encapsulados, puedes usar [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar cualquier otro control estándar de UWP.

Aunque en este artículo se muestra cómo hospedar controles de UWP en una aplicación de WPF, el proceso es similar para una aplicación de Windows Forms.

## <a name="required-components"></a>Componentes necesarios

Para hospedar un control de UWP en una aplicación de WPF o de Windows Forms, necesitarás los siguientes componentes en la solución. En este artículo se proporcionan instrucciones para crear cada uno de estos componentes.

* **El proyecto y el código fuente de la aplicación**. El uso del control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar controles estándar de UWP de origen se admite en las aplicaciones que tienen como destino .NET Framework o .NET Core 3.

* **Un proyecto de aplicación para UWP que define una clase de aplicación raíz que se deriva de XamlApplication**. El proyecto de WPF o Windows Forms debe tener acceso a una instancia de la clase [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) proporcionada por el kit de herramientas de la comunidad de Windows, a fin de que pueda detectar y cargar los controles XAML de UWP personalizados. La manera recomendada de hacerlo es definir este objeto en un proyecto de aplicación para UWP independiente que forme parte de la solución para la aplicación de WPF o Windows Forms. 

    > [!NOTE]
    > Aunque el objeto `XamlApplication` no es necesario para hospedar un control de UWP de origen, la aplicación necesita este objeto para admitir la gama completa de escenarios de la isla XAML, incluido el hospedaje de controles de UWP personalizados. Por lo tanto, se recomienda definir siempre un objeto `XamlApplication` en cualquier solución en la que se utilicen las islas XAML.

    > [!NOTE]
    > La solución puede contener solo un proyecto que define un objeto `XamlApplication`. Todos los controles de UWP personalizados de la aplicación comparten el mismo objeto `XamlApplication`. El proyecto que define el objeto `XamlApplication` debe incluir referencias a todas las demás bibliotecas y proyectos de UWP que se usan para hospedar controles de UWP en la isla XAML.

## <a name="create-a-wpf-project"></a>Creación de un proyecto de WPF

Antes de comenzar, sigue estas instrucciones para crear un proyecto de WPF y configurarlo para hospedar XAML Islands. Si tienes un proyecto de WPF existente, puedes adaptar estos pasos y ejemplos de código para el proyecto.

1. En Visual Studio 2019, crea un proyecto de **aplicación de WPF (.NET Framework)** o de **aplicación de WPF (.NET Core)** . Si deseas crear un proyecto de **aplicación de WPF (.NET Core)** , primero debes instalar la última versión del [SDK de .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. Asegúrate de que las [referencias de paquete](/nuget/consume-packages/package-references-in-project-files) están habilitadas:

    1. En Visual Studio, haz clic en **Herramientas-> Administrador de paquetes NuGet-> Configuración del Administrador de paquetes**.
    2. Asegúrate de que **PackageReference** está seleccionado para **Formato predeterminado de administración de paquetes**.

3. Haz clic con el botón derecho en el proyecto de WPF en el **Explorador de soluciones** y elige **Administrar paquetes NuGet**.

4. En la ventana **Administrador de paquetes NuGet**, asegúrate de seleccionar la opción **Incluir versión preliminar**.

5. Selecciona la pestaña **Examinar**, busca el paquete [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) (versión 6.0.0 o posterior) e instálalo. Este paquete proporciona todo lo necesario para usar los controles de UWP encapsulados para WPF (incluidos [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)) y el control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost).
    > [!NOTE]
    > Las aplicaciones de Windows Forms deben usar el paquete [Microsoft.Toolkit.Forms.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.Controls) (versión 6.0.0 o posterior).

6. Configura la solución para que tenga como destino una plataforma específica, como x86 o x64. La mayoría de los escenarios de islas XAML no se admiten en los proyectos que tienen como destino **cualquier CPU**.

    1. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de la solución y selecciona **Propiedades** -> **Propiedades de configuración** -> **Administrador de configuración**. 
    2. En **Plataforma de soluciones activas**, selecciona **Nueva**. 
    3. En el cuadro de diálogo **Nueva plataforma de solución**, selecciona **x64** o **x86** y, después, presiona **Aceptar**. 
    4. Cierra los cuadros de diálogo abiertos.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Definición de una clase XamlApplication en un proyecto de aplicación para UWP

A continuación, agrega un proyecto de aplicación para UWP a la solución y revisa la clase `App` predeterminada en este proyecto para derivarla de la clase [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) proporcionada por el kit de herramientas de la comunidad de Windows. Esta clase admite la interfaz [IXamlMetadaraProvider](/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider), que permite que la aplicación detecte y cargue metadatos para controles XAML de UWP personalizados en ensamblados del directorio actual de la aplicación en tiempo de ejecución. Esta clase también inicializa el marco XAML de UWP para el subproceso actual.

> [!NOTE]
> Aunque este paso no es necesario para hospedar un control de UWP de origen, la aplicación necesita el objeto `XamlApplication` para admitir la gama completa de escenarios de una isla XAML, incluido el hospedaje de controles de UWP personalizados. Por lo tanto, se recomienda definir siempre un objeto `XamlApplication` en cualquier solución en la que se utilicen islas XAML.

1. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de la solución y selecciona **Agregar** -> **Nuevo proyecto**.
2. Agrega un proyecto **Aplicación vacía (Windows universal)** a la solución. Asegúrate de que la versión de destino y la versión mínima están configuradas en **Windows 10, versión 1903** o posterior.
3. En el proyecto de aplicación para UWP, instala el paquete NuGet [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (versión 6.0.0 o posterior).
4. Abre el archivo **App.xaml** y reemplaza su contenido por el código XAML siguiente. Reemplaza `MyUWPApp` por el espacio de nombres del proyecto de aplicación para UWP.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Abre el archivo **App.xaml.cs** y reemplaza el contenido de este archivo por el código siguiente. Reemplaza `MyUWPApp` por el espacio de nombres del proyecto de aplicación para UWP.

    ```csharp
    namespace MyUWPApp
    {
        public sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Elimina el archivo **MainPage.xaml** del proyecto de aplicación para UWP.
7. Crea el proyecto de aplicación para UWP.
8. En el proyecto de WPF, agregue una referencia a su proyecto de aplicación para UWP. 

    * Si el proyecto de WDF está diseñado para .NET Core, haga clic con el botón derecho en el nodo **Dependencias** y agregue una referencia al proyecto de aplicación para UWP. 
    * Si el proyecto de WPF está diseñado para .NET Framework, haga clic con el botón derecho en el nodo del proyecto y seleccione **Dependencias de compilación** -> **Dependencias del proyecto** y el proyecto de aplicación para UWP.

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>Creación de una instancia del objeto XamlApplication en el punto de entrada de la aplicación de WPF

A continuación, agrega código al punto de entrada de la aplicación de WPF para crear una instancia de la clase `App` que acabas de definir en el proyecto de UWP (esta es la clase que ahora deriva de `XamlApplication`).

1. En el proyecto de WPF, haz clic con el botón derecho en el nodo del proyecto, selecciona **Agregar** -> **Nuevo elemento** y, después, selecciona **Clase**. Asigna un nombre a la clase **Programa** y haz clic en **Agregar**.

2. Reemplaza la clase `Program` generada por el código siguiente y guarda el archivo. Reemplaza `MyUWPApp` por el espacio de nombres del proyecto de aplicación para UWP y reemplaza `MyWPFApp` por el espacio de nombres del proyecto de aplicación de WPF.

    ```csharp
    public class Program
    {
        [System.STAThreadAttribute()]
        public static void Main()
        {
            using (new MyUWPApp.App())
            {
                MyWPFApp.App app = new MyWPFApp.App();
                app.InitializeComponent();
                app.Run();
            }
        }
    }
    ```

3. Haz clic con el botón derecho en el nodo del proyecto y selecciona **Propiedades**.

4. En la pestaña **Aplicación** de las propiedades, haz clic en la lista desplegable **Objeto de inicio** y elige el nombre completo de la clase `Program` que agregaste en el paso anterior. 
    > [!NOTE]
    > De forma predeterminada, los proyectos de WPF definen una función de punto de entrada `Main` en un archivo de código generado que no está diseñado para modificarse. Este paso cambia el punto de entrada del proyecto al método `Main` de la nueva clase `Program`, lo que permite agregar código que se ejecuta lo antes posible en el proceso de inicio de la aplicación. 

5. Guarda los cambios en las propiedades del proyecto.

## <a name="host-an-inkcanvas-and-inktoolbar-by-using-wrapped-controls"></a>Hospedaje de un control InkCanvas e InkToolbar mediante controles encapsulados

Ahora que has configurado el proyecto para que use islas XAML de UWP, ya puedes agregar los controles encapsulados de UWP [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) a la aplicación.

1. En el **Explorador de soluciones**, abre el archivo **MainWindow.xaml**.

2. En el elemento **Ventana** situado cerca de la parte superior del archivo XAML, agrega el atributo siguiente. Esto hace referencia al espacio de nombres de XAML para los controles encapsulados de UWP [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar).

    ```xml
    xmlns:Controls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls"
    ```

    Después de agregar este atributo, el elemento **Ventana** debe tener ahora un aspecto similar a este.

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

3. En el archivo **MainWindow.xaml**, reemplaza el elemento `<Grid>` existente por el código XAML siguiente. Este código XAML agrega un control [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) (precedido por la palabra clave **Controles** definida anteriormente como un espacio de nombres) a `<Grid>`.

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
    > También puedes agregar estos y otros controles encapsulados a la ventana arrastrándolos desde la sección **Kit de herramientas de la comunidad de Windows** del **Cuadro de herramientas** hasta el diseñador.

4. Guarda el archivo **MainWindow.xaml**.

    Si tienes un dispositivo que admite un lápiz digital, como Surface, y estás ejecutando este laboratorio en una máquina física, ahora puedes compilar y ejecutar la aplicación y dibujar la entrada de lápiz digital en la pantalla con el lápiz. Sin embargo, si no tienes un dispositivo compatible con el lápiz e intentas iniciar sesión con el mouse, no ocurrirá nada. Esto sucede porque el control **InkCanvas** está habilitado solo para los lápices digitales de forma predeterminada. Sin embargo, puedes cambiar este comportamiento.

5. Abre el archivo **MainWindow.xaml.cs**.

6. Agrega la siguiente declaración de espacio de nombres en la parte superior del archivo:

    ```csharp
    using Microsoft.Toolkit.Win32.UI.Controls.Interop.WinRT;
    ```

7. Busca el constructor `MainWindow()`. Agrega la siguiente línea de código después del método `InitializeComponent()` y guarda el archivo de código.

    ```csharp
    this.myInkCanvas.InkPresenter.InputDeviceTypes = CoreInputDeviceTypes.Mouse | CoreInputDeviceTypes.Pen;
    ```

    Puedes usar el objeto **InkPresenter** para personalizar la experiencia de entrada manuscrita predeterminada. Este código usa la propiedad **InputDeviceTypes** para habilitar el mouse, así como la entrada manuscrita.

8. Vuelve a presionar F5 para volver a compilar y ejecutar la aplicación en el depurador. Si utilizas un equipo con un mouse, confirma que puedes dibujar algo en el espacio del lienzo de entrada de lápiz con el mouse.

## <a name="host-a-calendarview-by-using-the-host-control"></a>Hospedaje de un control CalendarView mediante el control host

Ahora que has agregado los controles encapsulados de UWP [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas) e [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar) a la aplicación, ya puedes usar el control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para agregar un control [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) a la aplicación.

> [!NOTE]
> El control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) lo proporciona el paquete [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost). Este paquete se incluye en el paquete [Microsoft.Toolkit.Wpf.UI.Controls](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.Controls) que instalaste anteriormente.

1. En el **Explorador de soluciones**, abre el archivo **MainWindow.xaml**.

2. En el elemento **Ventana** situado cerca de la parte superior del archivo XAML, agrega el atributo siguiente. Esto hace referencia al espacio de nombres de XAML para el control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost).

    ```xml
    xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

    Después de agregar este atributo, el elemento **Ventana** debe tener ahora un aspecto similar a este.

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

4. En el archivo **MainWindow.xaml**, reemplaza el elemento `<Grid>` existente por el código XAML siguiente. Este código XAML agrega una fila a la cuadrícula y agrega un objeto [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) a la última fila. Para hospedar un control [CalendarView](/uwp/api/Windows.UI.Xaml.Controls.CalendarView) de UWP, este código XAML establece la propiedad `InitialTypeName` como el nombre completo del control. Este código XAML también define un controlador de eventos para el evento `ChildChanged`, que se genera cuando se ha representado el control hospedado.

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

5. Guarda el archivo **MainWindow.xaml** y abre el archivo **MainWindow.xaml.cs**.

7. Agrega la siguiente declaración de espacio de nombres en la parte superior del archivo:

    ```csharp
    using Microsoft.Toolkit.Wpf.UI.XamlHost;
    ```

10. Agrega el método del controlador de eventos `ChildChanged` siguiente a la clase `MainWindow` y guarda el archivo de código. Cuando se ha representado el control host, este controlador de eventos se ejecuta y crea un controlador de eventos simple para el evento `SelectedDatesChanged` del control del calendario.

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

11. Vuelve a presionar F5 para volver a compilar y ejecutar la aplicación en el depurador. Confirma que el control del calendario se muestra ahora en la parte inferior de la ventana.

## <a name="package-the-app"></a>Empaquetado de la aplicación

También puedes empaquetar la aplicación de WPF en un [paquete MSIX](/windows/msix) para implementarla. MSIX es una tecnología de empaquetado de aplicaciones moderna para Windows y está basada en una combinación de las tecnologías de instalación MSI, .appx, App-V y ClickOnce.

Las instrucciones siguientes muestran cómo empaquetar todos los componentes de la solución en un paquete MSIX mediante el [Proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) en Visual Studio 2019. Estos pasos solo son necesarios si quieres empaquetar la aplicación de WPF en un paquete MSIX.

> [!NOTE]
> Si decides no empaquetar la aplicación en un [paquete MSIX](/windows/msix) para su implementación, los equipos que ejecuten dicha aplicación deberán tener instalado el [Tiempo de ejecución de Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads).

1. Agrega un nuevo [Proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución. Al crear el proyecto, selecciona **Windows 10, versión 1903 (10.0; compilación 18362)** para la **versión de destino** y la **versión mínima**.

2. En el proyecto de empaquetado, haz clic con el botón derecho en el nodo **Aplicaciones** y elige **Agregar referencia**. En la lista de proyectos, selecciona el proyecto de WPF en la solución y haz clic en **Aceptar**.

3. Configura la solución para que tenga como destino una plataforma específica, como x86 o x64. Esto es necesario para compilar la aplicación de WPF en un paquete MSIX mediante el proyecto de paquete de aplicación de Windows.

    1. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de la solución y selecciona **Propiedades** -> **Propiedades de configuración** -> **Administrador de configuración**.
    2. En **Plataforma de soluciones activas**, selecciona **x64** o **x86**.
    3. En la fila del proyecto de WPF, en la columna **Plataforma**, selecciona **Nueva**.
    4. En el cuadro de diálogo **Nueva plataforma de solución**, selecciona **x64** o **x86** (la misma plataforma seleccionada para **Plataforma de soluciones activas**) y haz clic en **Aceptar**.
    5. Cierra los cuadros de diálogo abiertos.

5. Compila y ejecuta el proyecto de empaquetado. Confirma que se ejecuta WPF y que el control personalizado de UWP se muestra como se esperaba.

## <a name="related-topics"></a>Temas relacionados

* [Cómo usar los controles XAML de UWP en aplicaciones de escritorio (islas XAML)](xaml-islands.md)
* [Ejemplos de código de las islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [InkCanvas](/windows/communitytoolkit/controls/wpf-winforms/inkcanvas)
* [InkToolbar](/windows/communitytoolkit/controls/wpf-winforms/inktoolbar)
* [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)