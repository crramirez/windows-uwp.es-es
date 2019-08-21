---
description: En este artículo se muestra cómo hospedar un control de UWP personalizado en una aplicación WPF mediante el uso de islas XAML.
title: Hospedar un control personalizado de UWP en una aplicación WPF con islas XAML
ms.date: 08/20/2019
ms.topic: article
keywords: Windows 10, UWP, Windows Forms, WPF, Islas XAML, controles personalizados, controles de usuario, controles host
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 9f8d9d56d8a05be7d22ea52c9b45908c246fc250
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643769"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hospedar un control personalizado de UWP en una aplicación WPF con islas XAML

En este artículo se muestra cómo usar el control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la comunidad de Windows para hospedar un control personalizado de UWP en una aplicación WPF que tiene como destino .net Core 3. El control personalizado contiene varios controles UWP de primera parte y enlaza una propiedad de uno de los controles de UWP a una cadena en la aplicación WPF.

Aunque en este artículo se muestra cómo hacerlo en una aplicación WPF, el proceso es similar para una aplicación Windows Forms. Para obtener información general sobre el hospedaje de controles de UWP en WPF y Windows Forms aplicaciones, consulte [este artículo](xaml-islands.md#wpf-and-windows-forms-applications).

## <a name="overview"></a>Información general

Para hospedar un control personalizado de UWP en una aplicación WPF, necesitará los siguientes componentes. En este artículo se proporcionan instrucciones para crear cada uno de estos componentes.

* **El proyecto y el código fuente de la aplicación WPF**. El uso del control [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar controles UWP personalizados solo se admite en las aplicaciones de WPF y Windows Forms destinadas a .net Core 3. Este escenario no se admite en las aplicaciones que tienen como destino el .NET Framework.

* **Control UWP personalizado**. Necesitará el código fuente para el control personalizado de UWP que quiera hospedar para que pueda compilarlo con la aplicación. Normalmente, el control personalizado se define en un proyecto de biblioteca de clases de UWP al que se hace referencia en la misma solución que el proyecto de WPF (o Windows Forms).

* **Un proyecto de aplicación para UWP que define un objeto XamlApplication**. El proyecto de WPF (o Windows Forms) debe tener acceso a una instancia de `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` la clase proporcionada por el kit de herramientas de la comunidad de Windows. Este objeto actúa como proveedor de metadatos raíz para cargar los metadatos de los tipos XAML de UWP personalizados en los ensamblados en el directorio actual de la aplicación. La manera recomendada de hacerlo es agregar un proyecto de **aplicación vacía (Windows universal)** a la misma solución que el proyecto de WPF (o Windows Forms) y revisar la clase predeterminada `App` en este proyecto.
  > [!NOTE]
  > La solución solo puede contener un proyecto que define un `XamlApplication` objeto. Todos los controles personalizados de UWP de la aplicación comparten `XamlApplication` el mismo objeto. El proyecto que define el `XamlApplication` objeto debe incluir referencias a todas las demás bibliotecas y proyectos de UWP que se usan para hospedar controles de UWP en la isla XAML.

## <a name="create-a-wpf-project"></a>Crear un proyecto de WPF

Antes de comenzar, siga estas instrucciones para crear un proyecto de WPF y configurarlo para hospedar islas XAML. Si tiene un proyecto de WPF existente, puede adaptar estos pasos y ejemplos de código para el proyecto.

> [!NOTE]
> Si tiene un proyecto existente que tiene como destino el .NET Framework, deberá migrar el proyecto a .NET Core 3. Para obtener más información, vea [esta serie de blogs](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

1. Si todavía no lo ha hecho, instale la versión preliminar más reciente disponible del [SDK de .net Core 3 Preview](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. En Visual Studio 2019, cree un nuevo proyecto de **aplicación de WPF (.net Core)** .

3. Asegúrese de que [las referencias de paquete](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) están habilitadas:

    1. En Visual Studio, haga clic en **herramientas-> administrador de paquetes NuGet-> configuración del administrador de paquetes**.
    2. Asegúrese de que **PackageReference** está seleccionado para el **formato de administración de paquetes predeterminado**.

4. Haga clic con el botón derecho en el proyecto de WPF en **Explorador de soluciones** y elija **administrar paquetes NuGet**.

5. En la ventana **Administrador de paquetes NuGet** , asegúrese de que esté seleccionada la opción **incluir versión preliminar** .

6. Seleccione la pestaña **examinar** , busque el paquete [Microsoft. Toolkit. WPF. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (versión v 6.0.0-preview7 o posterior) e instale el paquete. Este paquete proporciona todo lo que necesita para usar el control **WindowsXamlHost** para hospedar un control de UWP, incluidos otros paquetes de NuGet relacionados.
    > [!NOTE]
    > Windows Forms aplicaciones deben usar el paquete [Microsoft. Toolkit. Forms. UI. XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (versión v 6.0.0-preview7 o posterior).

7. Configure la solución para que tenga como destino una plataforma específica, como x86 o x64. Los controles personalizados de UWP no se admiten en los proyectos que tienen como destino **cualquier CPU**.

    1. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución y seleccione **propiedades** -> **configuración propiedades** -> **Configuration Manager**. 
    2. En **plataforma de soluciones activas**, seleccione **nuevo**. 
    3. En el cuadro de diálogo **nueva plataforma de solución** , seleccione **x64** o **x86** y presione **Aceptar**. 
    4. Cierre los cuadros de diálogo abiertos.

## <a name="create-a-xamlapplication-object-in-a-uwp-app-project"></a>Creación de un objeto XamlApplication en un proyecto de aplicación para UWP

Después, agregue un proyecto de aplicación para UWP a la misma solución que el proyecto de WPF. Modificará la clase predeterminada `App` en este proyecto para derivar de la `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` clase proporcionada por el kit de herramientas de la comunidad de Windows. El objeto **WindowsXamlHost** de la aplicación de WPF necesita `XamlApplication` este objeto para hospedar los controles personalizados de UWP.

1. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución y seleccione **Agregar** -> **nuevo proyecto**.
2. Agrega un proyecto **Aplicación vacía (Windows universal)** a la solución. Asegúrese de que la versión de destino y la versión mínima están establecidas en **Windows 10, versión 1903** o posterior.
3. En el proyecto de aplicación para UWP, instale el paquete NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (versión v 6.0.0-preview7 o posterior).
4. Abra el archivo **app. Xaml** y reemplace el contenido de este archivo por el código XAML siguiente. Reemplace `MyUWPApp` por el espacio de nombres de su proyecto de aplicación para UWP.

    ```xml
    <xaml:XamlApplication
        x:Class="MyUWPApp.App"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:xaml="using:Microsoft.Toolkit.Win32.UI.XamlHost"
        xmlns:local="using:MyUWPApp">
    </xaml:XamlApplication>
    ```

5. Abra el archivo **app.Xaml.CS** y reemplace el contenido de este archivo por el código siguiente. Reemplace `MyUWPApp` por el espacio de nombres de su proyecto de aplicación para UWP.

    ```csharp
    namespace MyUWPApp
    {
        sealed partial class App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
        {
            public App()
            {
                this.Initialize();
            }
        }
    }
    ```

6. Elimine el archivo **mainpage. Xaml** del proyecto de aplicación para UWP.
7. Compile el proyecto de aplicación para UWP.
8. En el proyecto de WPF, haga clic con el botón derecho en el nodo dependencias y agregue una referencia al proyecto de aplicación de UWP.

## <a name="create-a-custom-uwp-control"></a>Creación de un control personalizado de UWP

Para hospedar un control personalizado de UWP en la aplicación WPF, debe tener el código fuente del control para que pueda compilarlo con la aplicación. Normalmente, los controles personalizados se definen en un proyecto de biblioteca de clases de UWP para facilitar la portabilidad.

En esta sección, definirá un sencillo control de UWP personalizado en un nuevo proyecto de biblioteca de clases. También puede definir el control personalizado de UWP en el proyecto de aplicación para UWP que creó en la sección anterior. Sin embargo, estos pasos lo hacen en un proyecto de biblioteca de clases independiente con fines ilustrativos, ya que normalmente se implementan los controles personalizados para la portabilidad. 

Si ya tiene un control personalizado, puede usarlo en lugar del control que se muestra aquí. Sin embargo, todavía necesitará configurar el proyecto que contiene el control, tal y como se muestra en estos pasos.

1. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución y seleccione **Agregar** -> **nuevo proyecto**.
2. Agregue un proyecto de **biblioteca de clases (Windows universal)** a la solución. Asegúrese de que la versión de destino y la versión mínima están establecidas en **Windows 10, versión 1903** o posterior.
3. Haga clic con el botón derecho en el archivo de proyecto y seleccione **Descargar proyecto**. Vuelva a hacer clic con el botón derecho en el archivo de proyecto y seleccione **Editar**.
4. Antes del elemento `</Project>` de cierre, agregue el siguiente código XML para deshabilitar varias propiedades y, a continuación, guarde el archivo del proyecto. Estas propiedades deben estar habilitadas para hospedar el control de UWP personalizado en una aplicación WPF (o Windows Forms).

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. Haga clic con el botón derecho en el archivo de proyecto y seleccione **volver a cargar el proyecto**.
6. Elimine el archivo predeterminado **Class1.CS** y agregue un nuevo elemento de **control de usuario** al proyecto.
7. En el archivo XAML del control de usuario, agregue lo siguiente `StackPanel` como un elemento secundario del valor `Grid`predeterminado. En este ejemplo se ``TextBlock`` agrega un control y, a ``Text`` continuación, se enlaza el atributo ``XamlIslandMessage`` de ese control al campo.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. En el archivo de código subyacente del control de usuario, agregue el `XamlIslandMessage` campo a la clase de control de usuario, como se muestra a continuación.

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
10. En el proyecto de WPF, haga clic con el botón derecho en el nodo dependencias y agregue una referencia al proyecto de biblioteca de clases de UWP.
11. En el proyecto de aplicación de UWP que configuró anteriormente, haga clic con el botón secundario en el nodo **referencias** y agregue una referencia al proyecto de biblioteca de clases de UWP.
12. Vuelva a generar toda la solución y asegúrese de que todos los proyectos se compilan correctamente.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>Hospedar el control de UWP personalizado en la aplicación de WPF

1. En **Explorador de soluciones**, expanda el proyecto de WPF y abra el archivo MainWindow. XAML o cualquier otra ventana en la que desee hospedar el control personalizado.
2. En el archivo XAML, agregue la siguiente declaración de espacio de `<Window>` nombres al elemento.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. En el mismo archivo, agregue el siguiente control al `<Grid>` elemento. Cambie el `InitialTypeName` atributo por el nombre completo del control de usuario en el proyecto de biblioteca de clases de UWP.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. Abra el archivo de código subyacente y agregue el código siguiente a la `Window` clase. Este código define un `ChildChanged` controlador de eventos que asigna el valor ``XamlIslandMessage`` del campo del control personalizado de UWP `WPFMessage` al valor del campo en la aplicación WPF. Cambie `UWPClassLibrary.MyUserControl` al nombre completo del control de usuario en el proyecto de biblioteca de clases de UWP.

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

## <a name="package-the-app"></a>Empaquetar la aplicación

Opcionalmente, puede empaquetar la aplicación WPF en un [paquete de MSIX](https://docs.microsoft.com/windows/msix) para la implementación. MSIX es la tecnología moderna de empaquetado de aplicaciones para Windows y se basa en una combinación de tecnologías de instalación de MSI, APPX, App-V y ClickOnce.

Las instrucciones siguientes muestran cómo empaquetar todos los componentes de la solución en un paquete de MSIX mediante el proyecto de paquete de [aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) en Visual Studio 2019. Estos pasos solo son necesarios si desea empaquetar la aplicación WPF en un paquete MSIX. Tenga en cuenta que estos pasos incluyen actualmente algunas soluciones específicas para el escenario de hospedaje de controles personalizados de UWP.

1. Agregue un nuevo [proyecto de paquete de aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución. Cuando cree el proyecto, seleccione **Windows 10, versión 1903 (10,0; Compilación 18362)** para la **versión de destino** y la **versión mínima**.

2. En el proyecto de empaquetado, haga clic con el botón secundario en el nodo **aplicaciones** y elija **Agregar referencia**. En la lista de proyectos, seleccione el proyecto de WPF en la solución y haga clic en **Aceptar**.

3. Edite el archivo de proyecto de empaquetado. Estos cambios son necesarios actualmente para empaquetar aplicaciones WPF destinadas a .NET Core 3 y que hospedan islas XAML.

    1. En Explorador de soluciones, haga clic con el botón secundario en el nodo del proyecto de empaquetado y seleccione **Editar archivo de proyecto**.
    2. Busca el elemento `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` en el archivo. Reemplace este elemento por el siguiente código XML. Estos cambios son necesarios actualmente para empaquetar aplicaciones WPF destinadas a .NET Core 3 y que hospedan controles de UWP.

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

4. Edite el manifiesto del paquete para hacer referencia a la imagen de la pantalla de presentación predeterminada correcta. Esta solución es necesaria actualmente para empaquetar aplicaciones de WPF que hospedan controles personalizados de UWP.

    1. En el proyecto de empaquetado, haga clic con el botón derecho en el archivo **Package. appxmanifest** y haga clic en **Ver código**.
    2. Busque el siguiente elemento en el archivo.

        ```<uap:SplashScreen Image="Images\SplashScreen.png" />```

    3. Cambie este elemento a:

        ```<uap:SplashScreen Image="Images\SplashScreen.scale-200.png" />```

    4. Guarde el archivo **Package. appxmanifest** y ciérrelo.

5. Edite el archivo de proyecto de WPF. Estos cambios son necesarios actualmente para empaquetar aplicaciones de WPF que hospedan controles personalizados de UWP.

    1. En Explorador de soluciones, haga clic con el botón secundario en el nodo del proyecto WPF y seleccione **descargar el proyecto**.
    2. Haga clic con el botón secundario en el nodo del proyecto WPF y seleccione **Editar**.
    3. Busque la última `</PropertyGroup>` etiqueta de cierre en el archivo y agregue el siguiente código XML inmediatamente después de esa etiqueta.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. Guarde el archivo de proyecto y ciérrelo.
    5. Haga clic con el botón secundario en el nodo del proyecto WPF y elija **volver a cargar el proyecto**.

6. Compile y ejecute el proyecto de empaquetado. Confirme que se ejecuta WPF y que el control personalizado de UWP se muestra como se esperaba.

## <a name="related-topics"></a>Temas relacionados

* [Controles de UWP en aplicaciones de escritorio](xaml-islands.md)
* [WindowsXamlHost](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)
