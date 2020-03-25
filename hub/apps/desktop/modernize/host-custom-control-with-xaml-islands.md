---
description: En este artículo se muestra cómo hospedar un control de UWP personalizado en una aplicación WPF mediante el uso de islas XAML.
title: Hospedar un control personalizado de UWP en una aplicación WPF con islas XAML
ms.date: 01/24/2020
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Islas XAML, controles personalizados, controles de usuario, controles host
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b1ac53e0a6b6e01cd2129e2b1893f91fae2ef0fe
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218605"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hospedar un control personalizado de UWP en una aplicación WPF con islas XAML

En este artículo se muestra cómo usar el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la comunidad de Windows para hospedar un control personalizado de UWP en una aplicación WPF que tiene como destino .net Core 3. El control personalizado contiene varios controles UWP de primera parte del Windows SDK y enlaza una propiedad de uno de los controles de UWP a una cadena en la aplicación WPF. En este artículo también se muestra cómo hospedar un control de UWP de la [biblioteca WinUI](https://docs.microsoft.com/uwp/toolkits/winui/).

Aunque en este artículo se muestra cómo hacerlo en una aplicación WPF, el proceso es similar para una aplicación Windows Forms. Para obtener información general sobre el hospedaje de controles de UWP en WPF y Windows Forms aplicaciones, consulte [este artículo](xaml-islands.md#wpf-and-windows-forms-applications).

## <a name="required-components"></a>Componentes necesarios

Para hospedar un control personalizado de UWP en una aplicación de WPF (o Windows Forms), necesitará los siguientes componentes en la solución. En este artículo se proporcionan instrucciones para crear cada uno de estos componentes.

* **El proyecto y el código fuente de la aplicación**. El uso del control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar controles UWP personalizados solo se admite en aplicaciones destinadas a .net Core 3. Este escenario no se admite en las aplicaciones que tienen como destino el .NET Framework.

* **Control UWP personalizado**. Necesitará el código fuente para el control personalizado de UWP que quiera hospedar para que pueda compilarlo con la aplicación. Normalmente, el control personalizado se define en un proyecto de biblioteca de clases de UWP al que se hace referencia en la misma solución que el proyecto de WPF o Windows Forms.

* **Un proyecto de aplicación para UWP que define una clase de aplicación raíz que se deriva de XamlApplication**. El proyecto de WPF o Windows Forms debe tener acceso a una instancia de la clase [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) proporcionada por el kit de herramientas de la comunidad de Windows. La manera recomendada de hacerlo es definir este objeto en un proyecto de aplicación para UWP independiente que forme parte de la solución para la aplicación WPF o Windows Forms. Este objeto actúa como proveedor de metadatos raíz para cargar los metadatos de los tipos XAML de UWP personalizados en los ensamblados en el directorio actual de la aplicación.

    > [!NOTE]
    > La solución solo puede contener un proyecto que define un objeto de `XamlApplication`. Todos los controles personalizados de UWP de la aplicación comparten el mismo objeto de `XamlApplication`. El proyecto que define el objeto de `XamlApplication` debe incluir referencias a todas las demás bibliotecas y proyectos de UWP que se usan en los controles del host para UWP en la isla XAML.

## <a name="create-a-wpf-project"></a>Crear un proyecto de WPF

Antes de comenzar, siga estas instrucciones para crear un proyecto de WPF y configurarlo para hospedar islas XAML. Si tiene un proyecto de WPF existente, puede adaptar estos pasos y ejemplos de código para el proyecto.

> [!NOTE]
> Si tiene un proyecto existente que tiene como destino el .NET Framework, deberá migrar el proyecto a .NET Core 3. Para obtener más información, vea [esta serie de blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

1. Si todavía no lo ha hecho, instale la versión más reciente del [SDK de .net Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. En Visual Studio 2019, cree un nuevo proyecto de **aplicación de WPF (.net Core)** .

3. Asegúrese de que [las referencias de paquete](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) están habilitadas:

    1. En Visual Studio, haga clic en **herramientas-> administrador de paquetes NuGet-> configuración del administrador de paquetes**.
    2. Asegúrese de que **PackageReference** está seleccionado para el **formato de administración de paquetes predeterminado**.

4. Haga clic con el botón derecho en el proyecto de WPF en **Explorador de soluciones** y elija **administrar paquetes NuGet**.

5. En la ventana **Administrador de paquetes NuGet** , asegúrese de que esté seleccionada la opción **incluir versión preliminar** .

6. Seleccione la pestaña **examinar** , busque el paquete [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (versión v 6.0.0 o posterior) e instale el paquete. Este paquete proporciona todo lo que necesita para usar el control **WindowsXamlHost** para hospedar un control de UWP, incluidos otros paquetes de NuGet relacionados.
    > [!NOTE]
    > Windows Forms aplicaciones deben usar el paquete [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (versión v 6.0.0 o posterior).

7. Configure la solución para que tenga como destino una plataforma específica, como x86 o x64. Los controles personalizados de UWP no se admiten en los proyectos que tienen como destino **cualquier CPU**.

    1. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución y seleccione **propiedades** -> **propiedades de configuración** -> **Configuration Manager**.
    2. En **plataforma de soluciones activas**, seleccione **nuevo**. 
    3. En el cuadro de diálogo **nueva plataforma de solución** , seleccione **x64** o **x86** y presione **Aceptar**. 
    4. Cierre los cuadros de diálogo abiertos.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Definición de una clase XamlApplication en un proyecto de aplicación para UWP

Después, agregue un proyecto de aplicación para UWP a la solución y revise la clase `App` predeterminada en este proyecto para derivar de la clase [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) proporcionada por el kit de herramientas de la comunidad de Windows. La aplicación usará esta clase como proveedor de metadatos raíz para cargar los metadatos para los tipos XAML de UWP personalizados en los ensamblados en el directorio actual de la aplicación.

1. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución y seleccione **Agregar** -> **nuevo proyecto**.
2. Agrega un proyecto **Aplicación vacía (Windows universal)** a la solución. Asegúrese de que la versión de destino y la versión mínima están establecidas en **Windows 10, versión 1903** o posterior.
3. En el proyecto de aplicación para UWP, instale el paquete NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (versión v 6.0.0 o posterior).
4. Abra el archivo **app. Xaml** y reemplace el contenido de este archivo por el código XAML siguiente. Reemplace `MyUWPApp` por el espacio de nombres del proyecto de aplicación para UWP.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Abra el archivo **app.Xaml.CS** y reemplace el contenido de este archivo por el código siguiente. Reemplace `MyUWPApp` por el espacio de nombres del proyecto de aplicación para UWP.

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

6. Elimine el archivo **mainpage. Xaml** del proyecto de aplicación para UWP.
7. Limpie el proyecto de aplicación de UWP y luego genérelo.
8. En el proyecto de WPF, haga clic con el botón derecho en el nodo **dependencias** y agregue una referencia al proyecto de aplicación de UWP.

## <a name="instantiate-the-xamlapplication-object-in-the-entry-point-of-your-wpf-app"></a>Crear una instancia del objeto XamlApplication en el punto de entrada de la aplicación WPF

A continuación, agregue código al punto de entrada de la aplicación de WPF para crear una instancia de la clase `App` que acaba de definir en el proyecto de UWP (esta es la clase que ahora deriva de `XamlApplication`). Este objeto actúa como proveedor de metadatos raíz para cargar los metadatos de los tipos XAML de UWP personalizados en los ensamblados en el directorio actual de la aplicación.

1. En el proyecto de WPF, haga clic con el botón secundario en el nodo del proyecto, seleccione **agregar** -> **nuevo elemento**y, a continuación, seleccione **clase**. Asigne un nombre al **programa** de la clase y haga clic en **Agregar**.

2. Reemplace la clase `Program` generada por el código siguiente y guarde el archivo. Reemplace `MyUWPApp` por el espacio de nombres del proyecto de aplicación de UWP y reemplace `MyWPFApp` por el espacio de nombres del proyecto de aplicación de WPF.

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

3. Haga clic con el botón secundario en el nodo del proyecto y elija **propiedades**.

4. En la pestaña **aplicación** de las propiedades, haga clic en la lista desplegable **objeto de inicio** y elija el nombre completo de la clase `Program` que ha agregado en el paso anterior. 
    > [!NOTE]
    > De forma predeterminada, los proyectos de WPF definen un `Main` función de punto de entrada en un archivo de código generado que no está diseñado para modificarse. Este paso cambia el punto de entrada del proyecto al método `Main` de la nueva clase `Program`, lo que le permite agregar código que se ejecuta lo antes posible en el proceso de inicio de la aplicación. 

5. Guarde los cambios en las propiedades del proyecto.

## <a name="create-a-custom-uwp-control"></a>Creación de un control personalizado de UWP

Para hospedar un control personalizado de UWP en la aplicación WPF, debe tener el código fuente del control para que pueda compilarlo con la aplicación. Normalmente, los controles personalizados se definen en un proyecto de biblioteca de clases de UWP para facilitar la portabilidad.

En esta sección, definirá un sencillo control de UWP personalizado en un nuevo proyecto de biblioteca de clases. También puede definir el control personalizado de UWP en el proyecto de aplicación para UWP que creó en la sección anterior. Sin embargo, estos pasos lo hacen en un proyecto de biblioteca de clases independiente con fines ilustrativos, ya que normalmente se implementan los controles personalizados para la portabilidad.

Si ya tiene un control personalizado, puede usarlo en lugar del control que se muestra aquí. Sin embargo, todavía necesitará configurar el proyecto que contiene el control, tal y como se muestra en estos pasos.

1. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución y seleccione **Agregar** -> **nuevo proyecto**.
2. Agregue un proyecto de **biblioteca de clases (Windows universal)** a la solución. Asegúrese de que la versión de destino y la versión mínima están establecidas en **Windows 10, versión 1903** o posterior.
3. Haga clic con el botón derecho en el archivo de proyecto y seleccione **Descargar proyecto**. Vuelva a hacer clic con el botón derecho en el archivo de proyecto y seleccione **Editar**.
4. Antes del elemento de `</Project>` de cierre, agregue el siguiente código XML para deshabilitar varias propiedades y, a continuación, guarde el archivo del proyecto. Estas propiedades deben estar habilitadas para hospedar el control de UWP personalizado en una aplicación WPF (o Windows Forms).

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. Haga clic con el botón derecho en el archivo de proyecto y seleccione **volver a cargar el proyecto**.
6. Elimine el archivo predeterminado **Class1.CS** y agregue un nuevo elemento de **control de usuario** al proyecto.
7. En el archivo XAML del control de usuario, agregue el siguiente `StackPanel` como un elemento secundario de la `Grid`predeterminada. En este ejemplo se agrega un control ``TextBlock`` y, a continuación, se enlaza el atributo ``Text`` de ese control al campo ``XamlIslandMessage``.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. En el archivo de código subyacente del control de usuario, agregue el campo `XamlIslandMessage` a la clase de control de usuario, como se muestra a continuación.

    ```csharp
    public sealed partial class MyUserControl : UserControl
    {
        public string XamlIslandMessage { get; set; }

        public MyUserControl()
        {
            this.InitializeComponent();
        }
    }
    ```

9. Compile el proyecto de biblioteca de clases de UWP.
10. En el proyecto de WPF, haga clic con el botón derecho en el nodo **dependencias** y agregue una referencia al proyecto de biblioteca de clases de UWP.
11. En el proyecto de aplicación de UWP que configuró anteriormente, haga clic con el botón secundario en el nodo **referencias** y agregue una referencia al proyecto de biblioteca de clases de UWP.
12. Vuelva a generar toda la solución y asegúrese de que todos los proyectos se compilan correctamente.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>Hospedar el control de UWP personalizado en la aplicación de WPF

1. En **Explorador de soluciones**, expanda el proyecto de WPF y abra el archivo MainWindow. XAML o cualquier otra ventana en la que desee hospedar el control personalizado.
2. En el archivo XAML, agregue la siguiente declaración de espacio de nombres al elemento `<Window>`.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. En el mismo archivo, agregue el siguiente control al elemento `<Grid>`. Cambie el atributo `InitialTypeName` por el nombre completo del control de usuario en el proyecto de biblioteca de clases de UWP.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. Abra el archivo de código subyacente y agregue el código siguiente a la clase `Window`. Este código define un controlador de eventos `ChildChanged` que asigna el valor del campo ``XamlIslandMessage`` del control personalizado de UWP al valor del campo `WPFMessage` en la aplicación WPF. Cambie `UWPClassLibrary.MyUserControl` por el nombre completo del control de usuario en el proyecto de biblioteca de clases de UWP.

    ```csharp
    private void WindowsXamlHost_ChildChanged(object sender, EventArgs e)
    {
        // Hook up x:Bind source.
        global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost windowsXamlHost =
            sender as global::Microsoft.Toolkit.Wpf.UI.XamlHost.WindowsXamlHost;
        global::UWPClassLibrary.MyUserControl userControl =
            windowsXamlHost.GetUwpInternalObject() as global::UWPClassLibrary.MyUserControl;

        if (userControl != null)
        {
            userControl.XamlIslandMessage = this.WPFMessage;
        }
    }

    public string WPFMessage
    {
        get
        {
            return "Binding from WPF to UWP XAML";
        }
    }
    ```

6. Compile y ejecute la aplicación y confirme que el control de usuario de UWP se muestra como se esperaba.

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>Agregar un control de la biblioteca WinUI al control personalizado

Tradicionalmente, los controles de UWP se han publicado como parte del sistema operativo Windows 10 y están disponibles para los desarrolladores a través de la Windows SDK. La [biblioteca WinUI](https://docs.microsoft.com/uwp/toolkits/winui/) es un enfoque alternativo, en el que las versiones actualizadas de los controles de UWP del Windows SDK se distribuyen en un paquete de NuGet que no está asociado a las versiones de Windows SDK. Esta biblioteca también incluye nuevos controles que no forman parte de la Windows SDK y la plataforma UWP predeterminada. Consulte nuestra [Guía básica de la biblioteca WinUI](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) para obtener más detalles.

En esta sección se muestra cómo agregar un control de UWP desde la biblioteca WinUI al control de usuario para que pueda hospedar este control en la aplicación WPF.

1. En el proyecto de aplicación para UWP, instale la versión más reciente del paquete de NuGet [Microsoft. UI. Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) .

2. En el archivo app. XAML de este proyecto, agregue el siguiente elemento secundario al elemento `<xaml:XamlApplication>`.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    Después de agregar este elemento, el contenido de este archivo debe tener ahora un aspecto similar al siguiente.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
        <Application.Resources>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </Application.Resources>
    </xaml:XamlApplication>
    ```

3. En el proyecto de biblioteca de clases de UWP, instale la versión más reciente del paquete de NuGet [Microsoft. UI. Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) (la misma versión que instaló en el proyecto de aplicación para UWP).

4. En el mismo proyecto, abra el archivo XAML para el control de usuario y agregue la siguiente declaración de espacio de nombres al elemento `<UserControl>`.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. En el mismo archivo, agregue un elemento `<winui:RatingControl />` como secundario del `<StackPanel>`. Este elemento agrega una instancia de la clase [RatingControl](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.ratingcontrol?view=winui-2.2) de la biblioteca WinUI. Después de agregar este elemento, el `<StackPanel>` debe tener ahora un aspecto similar a este.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. Compilar y ejecutar la aplicación y confirmar que el nuevo control de clasificación se muestra como se esperaba.

## <a name="package-the-app"></a>Empaquetar la aplicación

Opcionalmente, puede empaquetar la aplicación WPF en un [paquete de MSIX](https://docs.microsoft.com/windows/msix) para la implementación. MSIX es la tecnología moderna de empaquetado de aplicaciones para Windows y se basa en una combinación de tecnologías de instalación de MSI,. appx, App-V y ClickOnce.

Las instrucciones siguientes muestran cómo empaquetar todos los componentes de la solución en un paquete de MSIX mediante el proyecto de paquete de [aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) en Visual Studio 2019. Estos pasos solo son necesarios si desea empaquetar la aplicación WPF en un paquete MSIX. Tenga en cuenta que estos pasos incluyen actualmente algunas soluciones específicas para el escenario de hospedaje de controles personalizados de UWP.

> [!NOTE]
> Si decide no empaquetar la aplicación en un [paquete de MSIX](https://docs.microsoft.com/windows/msix) para la implementación, los equipos que ejecutan la aplicación deben tener instalado [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) .

1. Agregue un nuevo [proyecto de paquete de aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución. Cuando cree el proyecto, seleccione **Windows 10, versión 1903 (10,0; Compilación 18362)** para la **versión de destino** y la **versión mínima**.

2. En el proyecto de empaquetado, haga clic con el botón secundario en el nodo **aplicaciones** y elija **Agregar referencia**. En la lista de proyectos, seleccione el proyecto de WPF en la solución y haga clic en **Aceptar**.

3. Edite el archivo de proyecto de WPF. Estos cambios son necesarios actualmente para empaquetar aplicaciones de WPF que hospedan controles personalizados de UWP.

    1. En Explorador de soluciones, haga clic con el botón secundario en el nodo del proyecto WPF y seleccione **descargar el proyecto**.
    2. Haga clic con el botón secundario en el nodo del proyecto WPF y seleccione **Editar**.
    3. Busque la última etiqueta de cierre de `</PropertyGroup>` en el archivo y agregue el siguiente código XML inmediatamente después de esa etiqueta.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. Guarda el archivo del proyecto y ciérralo.
    5. Haga clic con el botón secundario en el nodo del proyecto WPF y elija **volver a cargar el proyecto**.

4. Compile y ejecute el proyecto de empaquetado. Confirme que se ejecuta WPF y que el control personalizado de UWP se muestra como se esperaba.

## <a name="related-topics"></a>Temas relacionados

* [Hospedar controles XAML de UWP en aplicaciones de escritorio (Islas XAML)](xaml-islands.md)
* [Ejemplos de código de islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
