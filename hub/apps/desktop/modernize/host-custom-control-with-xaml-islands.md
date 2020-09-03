---
description: En este artículo se muestra cómo hospedar un control personalizado de UWP en una aplicación de WPF mediante XAML Islands.
title: Hospedaje de un control personalizado de UWP en una aplicación de WPF mediante XAML Islands
ms.date: 01/24/2020
ms.topic: article
keywords: windows 10, uwp, windows forms, wpf, xaml islands, custom controls, user controls, host controls
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b7f46679e03f367f8521630365362a4eb110332d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170569"
---
# <a name="host-a-custom-uwp-control-in-a-wpf-app-using-xaml-islands"></a>Hospedaje de un control personalizado de UWP en una aplicación de WPF mediante XAML Islands

En este artículo se muestra cómo usar el control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) en el kit de herramientas de la comunidad Windows para hospedar un control de UWP personalizado en una aplicación de WPF orientada a .NET Core 3. El control personalizado contiene varios controles de UWP propios de Windows SDK y enlaza una propiedad de uno de los controles de UWP a una cadena de la aplicación WPF. En este artículo también se muestra cómo hospedar un control de UWP de la [biblioteca WinUI](/uwp/toolkits/winui/).

Aunque en este artículo se muestra cómo hacerlo en una aplicación de WPF, el proceso es similar para una aplicación de Windows Forms. Para obtener información general sobre el hospedaje de controles de UWP en WPF y aplicaciones de Windows Forms aplicaciones, consulta [este artículo](xaml-islands.md#wpf-and-windows-forms-applications).

## <a name="required-components"></a>Componentes necesarios

Para hospedar un control de UWP personalizado en una aplicación de WPF (o Windows Forms), necesitarás los siguientes componentes en la solución. En este artículo se proporcionan instrucciones para crear cada uno de estos componentes.

* **El proyecto y el código fuente de la aplicación**. El uso del control [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost) para hospedar controles de UWP personalizados solo se admite en las aplicaciones orientadas a .NET Core 3. Este escenario no se admite en aplicaciones destinadas a .NET Framework.

* **El control estándar de UWP**. Necesitarás el código fuente para el control de UWP que quieres hospedar a fin de poder compilarlo con la aplicación. Normalmente, el control personalizado se define en un proyecto de biblioteca de clases de UWP al que se hace referencia en la misma solución que el proyecto de WPF o Windows Forms.

* **Un proyecto de aplicación para UWP que define una clase de aplicación raíz que se deriva de XamlApplication**. El proyecto de WPF o Windows Forms debe tener acceso a una instancia de la clase [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) proporcionada por el kit de herramientas de la comunidad de Windows, a fin de que pueda detectar y cargar los controles XAML de UWP personalizados. La manera recomendada de hacerlo es definir este objeto en un proyecto de aplicación para UWP independiente que forme parte de la solución para la aplicación de WPF o Windows Forms. 

    > [!NOTE]
    > La solución puede contener solo un proyecto que define un objeto `XamlApplication`. Todos los controles de UWP personalizados de la aplicación comparten el mismo objeto `XamlApplication`. El proyecto que define el objeto `XamlApplication` debe incluir referencias a todas las demás bibliotecas y proyectos de UWP que se usan para hospedar controles de UWP en XAML Islands.

## <a name="create-a-wpf-project"></a>Creación de un proyecto de WPF

Antes de comenzar, sigue estas instrucciones para crear un proyecto de WPF y configurarlo para hospedar XAML Islands. Si tienes un proyecto de WPF existente, puedes adaptar estos pasos y ejemplos de código para el proyecto.

> [!NOTE]
> Si tienes un proyecto existente orientado a .NET Framework, tendrás que migrarlo a .NET Core 3. Para obtener más información, consulta [esta serie de blog](https://devblogs.microsoft.com/dotnet/migrating-a-sample-wpf-app-to-net-core-3-part-1/).

1. Si todavía no lo has hecho, instala la versión más reciente del [SDK de .NET Core 3](https://dotnet.microsoft.com/download/dotnet-core/3.0).

2. En Visual Studio 2019, crea un proyecto de **aplicación de WPF (.NET Core)** .

3. Asegúrate de que las [referencias de paquete](/nuget/consume-packages/package-references-in-project-files) están habilitadas:

    1. En Visual Studio, haz clic en **Herramientas-> Administrador de paquetes NuGet-> Configuración del Administrador de paquetes**.
    2. Asegúrate de que **PackageReference** está seleccionado para **Formato predeterminado de administración de paquetes**.

4. Haz clic con el botón derecho en el proyecto de WPF en el **Explorador de soluciones** y elige **Administrar paquetes NuGet**.

5. En la ventana **Administrador de paquetes NuGet**, asegúrate de seleccionar la opción **Incluir versión preliminar**.

6. Selecciona la pestaña **Examinar**, busca el paquete [Microsoft.Toolkit.Wpf.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Wpf.UI.XamlHost) (versión 6.0.0 o posterior) e instálalo. Este paquete proporciona todo lo que necesitas para usar el control **WindowsXamlHost** para hospedar un control de UWP, incluidos otros paquetes de NuGet relacionados.
    > [!NOTE]
    > Las aplicaciones de Windows Forms deben usar el paquete [Microsoft.Toolkit.Forms.UI.XamlHost](https://www.nuget.org/packages/Microsoft.Toolkit.Forms.UI.XamlHost) (versión 6.0.0 o posterior).

7. Configura la solución para que tenga como destino una plataforma específica, como x86 o x64. Los controles de UWP personalizados no se admiten en los proyectos que tienen como destino **cualquier CPU**.

    1. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de la solución y selecciona **Propiedades** -> **Propiedades de configuración** -> **Administrador de configuración**.
    2. En **Plataforma de soluciones activas**, selecciona **Nueva**. 
    3. En el cuadro de diálogo **Nueva plataforma de solución**, selecciona **x64** o **x86** y, después, presiona **Aceptar**. 
    4. Cierra los cuadros de diálogo abiertos.

## <a name="define-a-xamlapplication-class-in-a-uwp-app-project"></a>Definición de una clase XamlApplication en un proyecto de aplicación para UWP

A continuación, agrega un proyecto de aplicación para UWP a la solución y revisa la clase `App` predeterminada en este proyecto para derivarla de la clase [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) proporcionada por el kit de herramientas de la comunidad de Windows. Esta clase admite la interfaz [IXamlMetadaraProvider](/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider), que permite que la aplicación detecte y cargue metadatos para controles XAML de UWP personalizados en ensamblados del directorio actual de la aplicación en tiempo de ejecución. Esta clase también inicializa el marco XAML de UWP para el subproceso actual. 

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
7. Limpia el proyecto de aplicación para UWP y, a continuación, compílalo.
8. En el proyecto de WPF, haz clic con el botón derecho en el nodo **Dependencias** y agrega una referencia al proyecto de aplicación para UWP.

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

## <a name="create-a-custom-uwp-control"></a>Creación de un control estándar de UWP

Para crear un control de UWP personalizado en la aplicación de WPF, debes tener el código fuente del control para poder compilarlo con la aplicación. Normalmente, los controles personalizados se definen en un proyecto de biblioteca de clases de UWP para facilitar la portabilidad.

En esta sección, definirás un sencillo control de UWP personalizado en un nuevo proyecto de biblioteca de clases. También puedes definir el control personalizado de UWP en el proyecto de aplicación para UWP que creaste en la sección anterior. Sin embargo, estos pasos lo hacen en un proyecto de biblioteca de clases independiente con fines ilustrativos, ya que, normalmente, los controles personalizados para la portabilidad se implementan así.

Si ya tienes un control personalizado, puedes usarlo en lugar del control que se muestra aquí. Sin embargo, necesitarás, de todos modos, configurar el proyecto que contiene el control, tal y como se muestra en estos pasos.

1. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de la solución y selecciona **Agregar** -> **Nuevo proyecto**.
2. Agrega un proyecto de **Biblioteca de clases (Windows universal)** a la solución. Asegúrate de que la versión de destino y la versión mínima están configuradas en **Windows 10, versión 1903** o posterior.
3. Haz clic con el botón derecho en el archivo de proyecto y selecciona **Descargar el proyecto**. Vuelve a hacer clic con el botón derecho en el archivo del proyecto y selecciona **Editar**.
4. Antes del elemento `</Project>` de cierre, agrega el siguiente código XML para deshabilitar varias propiedades y, a continuación, guarda el archivo del proyecto. Estas propiedades deben estar habilitadas para hospedar el control de UWP personalizado en una aplicación de WPF (o Windows Forms).

    ```xml
    <PropertyGroup>
      <EnableTypeInfoReflection>false</EnableTypeInfoReflection>
      <EnableXBindDiagnostics>false</EnableXBindDiagnostics>
    </PropertyGroup>
    ```

5. Haz clic con el botón derecho en el archivo de proyecto y selecciona **Volver a cargar el proyecto**.
6. Elimina el archivo **Class1.cs** predeterminado y agrega un nuevo elemento de **Control de usuario**al proyecto.
7. En el archivo XAML del control de usuario, agrega el elemento `StackPanel` como elemento secundario del objeto `Grid` predeterminado. En este ejemplo, se agrega un control ``TextBlock`` y, a continuación, se enlaza el atributo ``Text`` de ese control al campo ``XamlIslandMessage``.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
    </StackPanel>
    ```

8. En el archivo de código subyacente del control de usuario, agrega el campo `XamlIslandMessage` a la clase de control de usuario, como se muestra a continuación.

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

9. Compila el proyecto de biblioteca de clases de UWP.
10. En el proyecto de WPF, haz clic con el botón derecho en el nodo **Dependencias** y agrega una referencia al proyecto de la biblioteca de clases de UWP.
11. En el proyecto de la aplicación para WPF que has configurado antes, haz clic con el botón derecho en el nodo **Referencias** y agrega una referencia al proyecto de la biblioteca de clases de UWP.
12. Vuelve a compilar toda la solución y asegúrate de que los proyectos se compilen correctamente.

## <a name="host-the-custom-uwp-control-in-your-wpf-app"></a>Hospedaje del control de UWP personalizado en la aplicación de WPF

1. En el **Explorador de soluciones**, expande el proyecto de WPF y abre el archivoMainWindow.xaml o cualquier otra ventana en la que quieras hospedar el control personalizado.
2. En el archivo XAML, agrega la siguiente declaración de espacio de nombres al elemento `<Window>`.

    ```xml
    xmlns:xaml="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost"
    ```

3. En el mismo archivo, agrega el siguiente control al elemento `<Grid>`. Cambia el atributo `InitialTypeName` por el nombre completo del control de usuario en el proyecto de la biblioteca de clases de UWP.

    ```xml
    <xaml:WindowsXamlHost InitialTypeName="UWPClassLibrary.MyUserControl" ChildChanged="WindowsXamlHost_ChildChanged" />
    ```

4. Abre el archivo de código subyacente y agrega el siguiente código a la clase `Window`. Este código define un controlador de eventos `ChildChanged` que asigna el valor del campo ``XamlIslandMessage`` del control personalizado de UWP al valor del campo `WPFMessage` en la aplicación de WPF. Cambia `UWPClassLibrary.MyUserControl` por el nombre completo del control de usuario en el proyecto de la biblioteca de clases de UWP.

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

6. Compila y ejecuta la aplicación y confirma que el control de usuario de UWP se muestra como se esperaba.

## <a name="add-a-control-from-the-winui-library-to-the-custom-control"></a>Adición de un control de la biblioteca WinUI al control personalizado

Tradicionalmente, los controles de UWP se han publicado como parte del sistema operativo Windows 10 y están disponibles para los desarrolladores desde Windows SDK. La [biblioteca WinUI](/uwp/toolkits/winui/) es un enfoque alternativo en el que las versiones actualizadas de los controles de UWP de Windows SDK se distribuyen en un paquete de NuGet que no está asociado con las versiones de Windows SDK. Esta biblioteca también incluye nuevos controles que no forman parte de Windows SDK y la plataforma UWP predeterminada. Consulta nuestra [hoja de ruta de la biblioteca WinUI](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) para obtener más detalles.

En esta sección se muestra cómo agregar un control de UWP desde la biblioteca WinUI al control de usuario para poder hospedar este control en la aplicación de WPF.

1. En el proyecto de la aplicación para UWP, instala la versión preliminar o la versión de lanzamiento más reciente del paquete NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml).

    > [!NOTE]
    > Si la aplicación de escritorio está empaquetada en un [paquete MSIX](/windows/msix), puedes usar una versión preliminar o de lanzamiento del paquete NugGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml). Si la aplicación de escritorio no está empaquetada con MSIX, debes instalar una versión preliminar del paquete NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml).

2. En el archivo App.xaml de este proyecto, agrega el siguiente elemento secundario al elemento `<xaml:XamlApplication>`.

    ```xml
    <Application.Resources>
        <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
    </Application.Resources>
    ```

    Después de agregar este elemento, el contenido de este archivo debe tener un aspecto similar al siguiente.

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

3. En el proyecto de biblioteca de clases de UWP, instala la versión más reciente del paquete de NuGet [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml) (la misma versión que instalaste en el proyecto de la aplicación para UWP).

4. En el mismo proyecto, abre el archivo XAML para el control de usuario y agrega la siguiente declaración de espacio de nombres al elemento `<UserControl>`.

    ```xml
    xmlns:winui="using:Microsoft.UI.Xaml.Controls"
    ```

5. En el mismo archivo, agrega un elemento `<winui:RatingControl />` como secundario de `<StackPanel>`. Este elemento agrega una instancia de la clase [RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol) de la biblioteca WinUI. Después de agregar este elemento, `<StackPanel>` debería tener un aspecto similar al siguiente.

    ```xml
    <StackPanel Background="LightCoral">
        <TextBlock>This is a simple custom UWP control</TextBlock>
        <Rectangle Fill="Blue" Height="100" Width="100"/>
        <TextBlock Text="{x:Bind XamlIslandMessage}" FontSize="50"></TextBlock>
        <winui:RatingControl />
    </StackPanel>
    ```

6. Compila y ejecuta la aplicación y confirma que el nuevo control de clasificación se muestra como se esperaba.

## <a name="package-the-app"></a>Empaquetado de la aplicación

También puedes empaquetar la aplicación de WPF en un [paquete MSIX](/windows/msix) para implementarla. MSIX es una tecnología de empaquetado de aplicaciones moderna para Windows y está basada en una combinación de las tecnologías de instalación MSI, .appx, App-V y ClickOnce.

Las instrucciones siguientes muestran cómo empaquetar todos los componentes de la solución en un paquete MSIX mediante el [Proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) en Visual Studio 2019. Estos pasos solo son necesarios si quieres empaquetar la aplicación de WPF en un paquete MSIX. Ten en cuenta que, actualmente, estos pasos incluyen algunas soluciones específicas para el escenario de hospedaje de controles personalizados de UWP.

> [!NOTE]
> Si decides no empaquetar la aplicación en un [paquete MSIX](/windows/msix) para su implementación, los equipos que ejecuten dicha aplicación deberán tener instalado el [Entorno en tiempo de ejecución de Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads).

1. Agrega un nuevo [Proyecto de paquete de aplicación de Windows](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución. Al crear el proyecto, selecciona **Windows 10, versión 1903 (10.0; compilación 18362)** para la **versión de destino** y la **versión mínima**.

2. En el proyecto de empaquetado, haz clic con el botón derecho en el nodo **Aplicaciones** y elige **Agregar referencia**. En la lista de proyectos, selecciona el proyecto de WPF en la solución y haz clic en **Aceptar**.

3. Edita el archivo de proyecto de WPF. Actualmente, estos cambios son necesarios para empaquetar aplicaciones de WPF que hospedan controles personalizados de UWP.

    1. En el Explorador de soluciones, haz clic con el botón derecho en el nodo de proyecto de WPF y selecciona **Descargar el proyecto**.
    2. Haz clic con el botón derecho en el nodo del proyecto de WPF y selecciona **Editar**.
    3. Busca la última etiqueta de cierre `</PropertyGroup>` en el archivo y agrega el siguiente código XML inmediatamente después de la etiqueta.

        ``` xml
        <PropertyGroup>
          <AssetTargetFallback>uap10.0.18362</AssetTargetFallback>
        </PropertyGroup>
        ```

    4. Guarda el archivo del proyecto y ciérralo.
    5. Haz clic con el botón derecho en el nodo de proyecto de WPF y elige **Volver a cargar el proyecto**.

4. Compila y ejecuta el proyecto de empaquetado. Confirma que se ejecuta WPF y que el control personalizado de UWP se muestra como se esperaba.

## <a name="related-topics"></a>Temas relacionados

* [Cómo usar los controles XAML de UWP en aplicaciones de escritorio (islas XAML)](xaml-islands.md)
* [Ejemplos de código de las islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
* [WindowsXamlHost](/windows/communitytoolkit/controls/wpf-winforms/windowsxamlhost)