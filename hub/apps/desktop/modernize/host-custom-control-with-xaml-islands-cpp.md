---
description: En este artículo se muestra cómo hospedar un control personalizado de UWP en una aplicación Win32 de C++ mediante la API de hospedaje de XAML.
title: Hospedaje de un control de UWP personalizado en una aplicación Win32 de C++ mediante la API de hospedaje de XAML
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10;uwp;C++;Win32;xaml islands;custom controls;user controls;host controls;islas XAML;controles personalizados;controles de usuario;hospedar controles
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 93badc28c9c4fa1684836fc4a883e54661e8d4dc
ms.sourcegitcommit: 7112e4ec3f19d46a1fc4d81d1c29fd9c01522610
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2020
ms.locfileid: "80986968"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>Hospedaje de un control personalizado de UWP en una aplicación Win32 de C++

En este artículo se muestra cómo usar la [API de hospedaje de XAML de UWP](using-the-xaml-hosting-api.md) para hospedar un control XAML de UWP personalizado en una nueva aplicación Win32 de C++. Si tienes un proyecto de aplicación Win32 de C++ actual, puedes adaptar estos pasos y ejemplos de código para dicho proyecto.

Para hospedar un control XAML de UWP personalizado, crearás los siguientes proyectos y componentes como parte del tutorial:

* **Proyecto de aplicaciones de escritorio de Windows**. Este proyecto implementa una aplicación de escritorio Win32 de C++ nativa. Agregarás código a este proyecto que usa la API de hospedaje de XAML de UWP para hospedar un control XAML de UWP personalizado.

* **Proyecto de aplicación para UWP (C++/WinRT)** . En este proyecto se implementa un control XAML de UWP personalizado. También se implementa un proveedor de metadatos raíz para cargar los metadatos de los tipos XAML de UWP personalizados en el proyecto.

## <a name="requirements"></a>Requisitos

* Visual Studio 2019, versión 16.4.3 o posterior.
* Windows 10, versión 1903 con SDK (versión 10.0.18362) o posterior.
* [Extensión de Visual Studio de C++/WinRT (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) instala con Visual Studio. C++/WinRT es una moderna proyección de lenguaje C++17 totalmente estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivos de encabezado y diseñada para darte acceso de primera clase a la API moderna de Windows. Para obtener más información, consulta [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/).

## <a name="create-a-desktop-application-project"></a>Creación de un proyecto de aplicación de escritorio

1. En Visual Studio, crea un nuevo **proyecto de aplicación de escritorio de Windows** con el nombre **MyDesktopWin32App**. Esta plantilla de proyecto está disponible en los filtros de proyecto **C++** , **Windows** y **Escritorio**.

2. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de la solución, haz clic en **Redestinar solución**, selecciona la versión **10.0.18362.0** del SDK o una posterior y después haz clic en **Aceptar**.

3. Instala el paquete NuGet [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) para incluir compatibilidad con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) en el proyecto:

    1. Haz clic con el botón derecho en el proyecto **MyDesktopWin32App** en el **Explorador de soluciones** y elige **Administrar paquetes de NuGet**.
    2. Selecciona la pestaña **Examinar**, busca el paquete [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) e instala la última versión de dicho paquete.

4. En la ventana **Administrar paquetes NuGet**, instala los siguientes paquetes de NuGet adicionales:

    * [Microsoft.Toolkit.Win32.UI.SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (versión 6.0.0 o posterior). Este paquete incluye varios recursos de compilación y tiempo de ejecución que permiten que las islas XAML funcionen en la aplicación.
    * [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (versión 6.0.0 o posterior). Este paquete define la clase [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication), que usarás más adelante en este tutorial.
    * [Microsoft.VCRTForwarders.140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140).

5. Compila la solución y confirma que dicho proceso se realiza correctamente.

## <a name="create-a-uwp-app-project"></a>Creación de un proyecto de aplicación para UWP

Después, agrega un proyecto de aplicación para **UWP (C++/WinRT)** a la solución y realiza algunos cambios de configuración en este proyecto. Más adelante en este tutorial, agregarás código a este proyecto para implementar un control XAML de UWP personalizado y definir una instancia de la clase [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication). 

1. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de la solución y selecciona **Agregar** -> **Nuevo proyecto**.

2. Agrega un proyecto **Aplicación en blanco (C++/WinRT)** a la solución. Asigna al proyecto el nombre **MyUWPApp** y asegúrate de que la versión de destino y la versión mínima están configuradas en **Windows 10, versión 1903** o posterior.

3. Instala el paquete NuGet [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) en el proyecto **MyUWPApp**. Este paquete define la clase [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication), que usarás más adelante en este tutorial.

    1. Haz clic con el botón derecho en el proyecto **MyUWPApp** y selecciona **Administrar paquetes NuGet**.
    2. Selecciona la pestaña **Examinar**, busca el paquete [Microsoft.Toolkit.Win32.UI.XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) e instala la versión 6.0.0 o posterior de dicho paquete.

4. Haz clic con el botón derecho en el nodo **MyUWPApp** y selecciona **Propiedades**. En la página **Propiedades comunes** -> **C++/WinRT**, establece las siguientes propiedades y, a continuación, haz clic en **Aplicar**.

    * Establece **Nivel de detalle** en **normal**.
    * Establece **Optimizado** en **No**.

    Cuando hayas terminado, la página de propiedades debe tener el siguiente aspecto.

    ![Propiedades de proyecto de C++/WinRT](images/xaml-islands/xaml-island-cpp-1.png)

5. En la página **Propiedades de configuración** -> **General** de la ventana Propiedades, establece **Tipo de configuración** en **Biblioteca dinámica (.dll)** y, a continuación, haz clic en **Aceptar** para cerrar la ventana Propiedades.

    ![Propiedades generales del proyecto](images/xaml-islands/xaml-island-cpp-2.png)

6. Agrega un archivo ejecutable de marcador de posición al proyecto **MyUWPApp**. Este archivo ejecutable de marcador de posición es necesario para que Visual Studio genere los archivos de proyecto necesarios y compile el proyecto correctamente.

    1. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de proyecto **MyUWPApp** y selecciona **Agregar** -> **Nuevo elemento**.
    2. En el cuadro de diálogo **Agregar nuevo elemento**, selecciona **Utilidad** en la página izquierda y después selecciona **Archivo de texto (.txt)** . Escribe el nombre **placeholder.exe** y haz clic en **Agregar**.
      ![Agregar archivo de texto](images/xaml-islands/xaml-island-cpp-3.png)
    3. En el **Explorador de soluciones**, selecciona el archivo **placeholder.exe**. En la ventana **Propiedades**, asegúrate de que la propiedad **Contenido** esté establecida en **True**.
    4. En el **Explorador de soluciones**, haz clic con el botón derecho en el archivo **Package.appxmanifest** del proyecto **MyUWPApp**, selecciona **Abrir con**, luego **Editor XML (texto)** y haz clic en **Aceptar**.
    5. Busca el elemento **&lt;Application&gt;** y cambia el atributo **Executable** al valor `placeholder.exe`. Cuando hayas terminado, el elemento **&lt;Application&gt;** tendrá un aspecto similar al siguiente.

        ```xml
        <Application Id="App" Executable="placeholder.exe" EntryPoint="MyUWPApp.App">
          <uap:VisualElements DisplayName="MyUWPApp" Description="Project for a single page C++/WinRT Universal Windows Platform (UWP) app with no predefined layout"
            Square150x150Logo="Assets\Square150x150Logo.png" Square44x44Logo="Assets\Square44x44Logo.png" BackgroundColor="transparent">
            <uap:DefaultTile Wide310x150Logo="Assets\Wide310x150Logo.png">
            </uap:DefaultTile>
            <uap:SplashScreen Image="Assets\SplashScreen.png" />
          </uap:VisualElements>
        </Application>
        ```

    6. Guarda y cierra el archivo **Package.appxmanifest**.

7. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo **MyUWPApp** y selecciona **Descargar proyecto**.
8. Haz clic con el botón derecho en el nodo **MyUWPApp** y selecciona **Edit MyUWPApp.vcxproj**.
9. Busca el elemento `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />` y sustitúyelo por el siguiente código XML. Este XML agrega varias propiedades nuevas inmediatamente antes del elemento.

    ```xml
    <PropertyGroup Label="Globals">
        <WindowsAppContainer>true</WindowsAppContainer>
        <AppxGeneratePriEnabled>true</AppxGeneratePriEnabled>
        <ProjectPriIndexName>App</ProjectPriIndexName>
        <AppxPackage>true</AppxPackage>
    </PropertyGroup>
    <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
    ```

10. Guarda y cierra el archivo de proyecto.
11. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo **MyUWPApp** y selecciona **Volver a cargar el proyecto**.

## <a name="configure-the-solution"></a>Configuración de la solución

En esta sección, actualizarás la solución que contiene ambos proyectos para configurar las dependencias del proyecto y las propiedades de compilación necesarias para que los proyectos se compilen de forma correcta.

1. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de la solución y agrega un nuevo archivo XML denominado **Solution.props**.
2. Agrega el siguiente código XML al archivo **Solution.props**.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <PropertyGroup>
        <IntDir>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</IntDir>
        <OutDir>$(SolutionDir)\bin\$(Platform)\$(Configuration)\$(MSBuildProjectName)\</OutDir>
        <GeneratedFilesDir>$(IntDir)Generated Files\</GeneratedFilesDir>
      </PropertyGroup>
    </Project>
    ```

3. En el menú **Ver**, haz clic en **Administrador de propiedades** (según la configuración, es posible que esté en **Ver** -> **Otras ventanas**).
4. En la ventana del **Administrador de propiedades**, haz clic con el botón derecho en **MyDesktopWin32App** y selecciona **Agregar hoja de propiedades existente**. Ve al archivo **Solution.props** que acabas de agregar y haz clic en **Abrir**.
5. Repite el paso anterior para agregar el archivo **Solution.props** al proyecto **MyUWPApp** en la ventana del **Administrador de propiedades**.
6. Cierra la ventana del **Administrador de propiedades**.
7. Confirma si los cambios de la hoja de propiedades se guardaron correctamente. En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto **MyDesktopWin32App** y elige **Propiedades**. Haz clic en **Propiedades de configuración** -> **General** y confirme si las propiedades del **Directorio de salida** y el **Directorio intermedio** tienen los valores que agregaste al archivo **Solution.props**. También puedes confirmar lo mismo para el proyecto **MyUWPApp**.
    ![Propiedades del proyecto](images/xaml-islands/xaml-island-cpp-4.png)

8. En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de la solución y elige **Dependencias del proyecto**. En la lista desplegable **Proyectos**, asegúrate de que **MyDesktopWin32App** esté seleccionado y, a continuación, selecciona **MyUWPApp** en la lista **Depende de**.
    ![Dependencias del proyecto](images/xaml-islands/xaml-island-cpp-5.png)

9. Haga clic en **Aceptar**.

## <a name="add-code-to-the-uwp-app-project"></a>Adición de código al proyecto de aplicación para UWP

Ahora estás listo para agregar código al proyecto **MyUWPApp** para realizar estas tareas:

* Implementa un control XAML de UWP personalizado. Más adelante en este tutorial, agregarás código que hospeda este control en el proyecto **MyDesktopWin32App**.
* Define un tipo que se derive de la clase [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) en el kit de herramientas de la comunidad de Windows.

### <a name="define-a-custom-uwp-xaml-control"></a>Definición de un control XAML de UWP personalizado

1. En el **Explorador de soluciones**, haz clic con el botón derecho en **MyUWPApp** y selecciona **Agregar** -> **Nuevo elemento**. Selecciona el nodo **Visual C++** en el panel izquierdo, selecciona **Control de usuario en blanco (C++/WinRT)** , asígnale el nombre **MyUserControl** y haz clic en **Agregar**.
2. En el editor XAML, reemplaza el contenido del archivo **MyUserControl.xaml** por el siguiente código XAML y, a continuación, guarda el archivo.

    ```xml
    <UserControl
        x:Class="MyUWPApp.MyUserControl"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyUWPApp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <StackPanel HorizontalAlignment="Center" Spacing="10" 
                    Padding="20" VerticalAlignment="Center">
            <TextBlock HorizontalAlignment="Center" TextWrapping="Wrap" 
                           Text="Hello from XAML Islands" FontSize="30" />
            <TextBlock HorizontalAlignment="Center" Margin="15" TextWrapping="Wrap"
                           Text="😍❤💋🌹🎉😎�🐱‍👤" FontSize="16" />
            <Button HorizontalAlignment="Center" 
                    x:Name="Button" Click="ClickHandler">Click Me</Button>
        </StackPanel>
    </UserControl>
    ```

### <a name="define-a-xamlapplication-class"></a>Definición de una clase XamlApplication

A continuación, revisa la clase **App** predeterminada en el proyecto **MyUWPApp** para derivarla de la clase [Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) proporcionada por el kit de herramientas de la comunidad de Windows. Esta clase admite la interfaz [IXamlMetadaraProvider](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.IXamlMetadataProvider), que permite que la aplicación detecte y cargue metadatos para controles XAML de UWP personalizados en ensamblados del directorio actual de la aplicación en tiempo de ejecución. Esta clase también inicializa el marco XAML de UWP para el subproceso actual. Más adelante en este tutorial, actualizarás el proyecto de escritorio para crear una instancia de esta clase.

  > [!NOTE]
  > Cada solución que usa islas XAML puede contener solo un proyecto que define un objeto `XamlApplication`. Todos los controles XAML de UWP personalizados de la aplicación comparten el mismo objeto `XamlApplication`. 

1. En el **Explorador de soluciones**, haz clic con el botón derecho en el archivo **MainPage.xaml** del proyecto **MyUWPApp**. Haz clic en **Quitar** y, a continuación, en **Eliminar** para eliminar este archivo del proyecto de forma permanente.
2. En el proyecto **MyUWPApp**, expande el archivo **App.xaml**.
3. Reemplaza el contenido de los archivos **App.xaml**, **App.cpp**, **App.h** y **App.idl** por el siguiente código.

    * **App.xaml**:

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **App.idl**:

        ```IDL
        namespace MyUWPApp
        {
             [default_interface]
             runtimeclass App : Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication
             {
                App();
             }
        }
        ```

    * **App.h**:

        ```cpp
        #pragma once
        #include "App.g.h"
        #include "App.base.h"
        namespace winrt::MyUWPApp::implementation
        {
            class App : public AppT2<App>
            {
            public:
                App();
                ~App();
            };
        }
        namespace winrt::MyUWPApp::factory_implementation
        {
            class App : public AppT<App, implementation::App>
            {
            };
        }
        ```

    * **App.cpp**:

        ```cpp
        #include "pch.h"
        #include "App.h"
        using namespace winrt;
        using namespace Windows::UI::Xaml;
        namespace winrt::MyUWPApp::implementation
        {
            App::App()
            {
                Initialize();
                AddRef();
                m_inner.as<::IUnknown>()->Release();
            }
            App::~App()
            {
                Close();
            }
        }
        ```

4. Agrega un nuevo archivo de encabezado al proyecto **MyUWPApp** con el nombre **app.base.h**.
5. Agrega el código siguiente al archivo **app.base.h**, guarda el archivo y ciérralo.

    ```cpp
    #pragma once
    namespace winrt::MyUWPApp::implementation
    {
        template <typename D, typename... I>
        struct App_baseWithProvider : public App_base<D, ::winrt::Windows::UI::Xaml::Markup::IXamlMetadataProvider>
        {
            using IXamlType = ::winrt::Windows::UI::Xaml::Markup::IXamlType;
            IXamlType GetXamlType(::winrt::Windows::UI::Xaml::Interop::TypeName const& type)
            {
                return AppProvider()->GetXamlType(type);
            }
            IXamlType GetXamlType(::winrt::hstring const& fullName)
            {
                return AppProvider()->GetXamlType(fullName);
            }
            ::winrt::com_array<::winrt::Windows::UI::Xaml::Markup::XmlnsDefinition> GetXmlnsDefinitions()
            {
                return AppProvider()->GetXmlnsDefinitions();
            }
        private:
            bool _contentLoaded{ false };
            std::shared_ptr<XamlMetaDataProvider> _appProvider;
            std::shared_ptr<XamlMetaDataProvider> AppProvider()
            {
                if (!_appProvider)
                {
                    _appProvider = std::make_shared<XamlMetaDataProvider>();
                }
                return _appProvider;
            }
        };
        template <typename D, typename... I>
        using AppT2 = App_baseWithProvider<D, I...>;
    }
    ```

6. Compila la solución y confirma que dicho proceso se realiza correctamente.

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>Configuración del proyecto de escritorio para usar tipos de controles personalizados

Antes de que la aplicación **MyDesktopWin32App** pueda hospedar un control XAML de UWP personalizado en una isla XAML, debe configurarse para usar tipos de control personalizados del proyecto **MyUWPApp**. Hay dos maneras de realizar esta configuración y puedes elegir cualquiera de ellas a medida que completes este tutorial.

### <a name="option-1-package-the-app-using-msix"></a>Opción 1: empaquetar la aplicación mediante MSIX

Puedes empaquetar la aplicación en un [paquete MSIX](https://docs.microsoft.com/windows/msix) para implementarla. MSIX es una tecnología de empaquetado de aplicaciones moderna para Windows y está basada en una combinación de las tecnologías de instalación .msi, .appx, App-V y ClickOnce.

1. Agrega un nuevo [proyecto de paquete de aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución. Al crear el proyecto, asígnale el nombre **MyDesktopWin32Project** y selecciona **Windows 10, versión 1903 (10.0; compilación 18362)** para la **Versión de destino** y la **Versión mínima**.

2. En el proyecto de empaquetado, haz clic con el botón derecho en el nodo **Applications** y elige **Agregar referencia**. En la lista de proyectos, activa la casilla situada junto al proyecto **MyDesktopWin32App** y haz clic en **Aceptar**.
    ![Proyecto de referencia](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> Si decides no empaquetar la aplicación en un [paquete MSIX](https://docs.microsoft.com/windows/msix) para su implementación, los equipos que ejecuten dicha aplicación deberán tener instalado el [Tiempo de ejecución de Visual C++](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads).

### <a name="option-2-create-an-application-manifest"></a>Opción 2: seleccionar un manifiesto de aplicación

Puedes agregar un [manifiesto de aplicación](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) a la aplicación.

1. Haz clic con el botón derecho en el proyecto **MyDesktopWin32App** y selecciona **Agregar** -> **Nuevo elemento**. 
2. En el cuadro de diálogo **Agregar nuevo elemento**, haz clic en **Web** en el panel izquierdo y selecciona **Archivo XML (.xml)** . 
3. Asigna al nuevo archivo el nombre **app.manifest** y haz clic en **Agregar**.
4. Reemplaza el contenido del nuevo archivo por el siguiente XML. Este XML registra los tipos de control personalizados en el proyecto **MyUWPApp**.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly
     xmlns="urn:schemas-microsoft-com:asm.v1"
     xmlns:asmv3="urn:schemas-microsoft-com:asm.v3"
     manifestVersion="1.0">
      <asmv3:file name="MyUWPApp.dll">
        <activatableClass
            name="MyUWPApp.App"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.XamlMetadataProvider"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
        <activatableClass
            name="MyUWPApp.MyUserControl"
            threadingModel="both"
            xmlns="urn:schemas-microsoft-com:winrt.v1" />
      </asmv3:file>
    </assembly>
    ```

## <a name="configure-additional-desktop-project-properties"></a>Configuración de propiedades adicionales del proyecto de escritorio

A continuación, actualiza el proyecto **MyDesktopWin32App** para definir una macro para directorios de inclusión adicionales y configurar propiedades adicionales.

1. En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto **MyDesktopWin32App** y elige **Descargar el proyecto**.

2. Haz clic con el botón derecho en **MyDesktopWin32App (descargado)** y selecciona **Editar MyDesktopWin32App.vcxproj**.

3. Agrega el siguiente código XML justo delante de la etiqueta de cierre `</Project>` al final del archivo. Después, guarda y cierra el archivo.

    ```xml
      <!-- Configure these for your UWP project -->
      <PropertyGroup>
        <AppProjectName>MyUWPApp</AppProjectName>
      </PropertyGroup>
      <PropertyGroup>
        <AppIncludeDirectories>$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\;$(SolutionDir)\obj\$(Platform)\$(Configuration)\$(AppProjectName)\Generated Files\;</AppIncludeDirectories>
      </PropertyGroup>
      <ItemGroup>
        <ProjectReference Include="..\$(AppProjectName)\$(AppProjectName).vcxproj" />
      </ItemGroup>
      <!-- End Section-->
    ```

4. En el **Explorador de soluciones**, haz clic con el botón derecho en **MyDesktopWin32App (descargado)** y selecciona **Volver a cargar el proyecto**.

5. Haz clic con el botón derecho en **MyDesktopWin32App**, selecciona **Propiedades** y haz clic en el nodo **C/C++** en el panel izquierdo. Confirma que la macro **Directorios de inclusión adicionales** se definió a partir del cambio en el archivo del proyecto que realizaste en el paso anterior.

    ![Configuración del proyecto C/C++](images/xaml-islands/xaml-island-cpp-7.png)

6. En el cuadro de diálogo **Páginas de propiedades**, expande **Herramienta Manifiesto** -> **Entrada y salida**. Establece la propiedad **Reconocimiento de ppp** en **Reconocimiento de ppp elevado por monitor**. Si no estableces esta propiedad, es posible que se produzca un error de configuración del manifiesto en ciertos escenarios de PPP elevado.

    ![Configuración del proyecto C/C++](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>Hospedaje del control XAML de UWP personalizado en el proyecto de escritorio

Por último, estás listo para agregar código al proyecto **MyDesktopWin32App** para hospedar el control XAML de UWP personalizado que definiste anteriormente en el proyecto **MyUWPApp**.

1. En el proyecto **MyDesktopWin32App**, abre el archivo **framework.h** y convierte en comentario la siguiente línea de código. Cuando hayas terminado, guarda el archivo.

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. Abre el archivo **MyDesktopWin32App.h** y reemplaza su contenido por el código siguiente para hacer referencia a los archivos de encabezado de C++/WinRT necesarios. Cuando hayas terminado, guarda el archivo.

    ```cpp
    #pragma once

    #include "resource.h"
    #include <winrt/Windows.Foundation.Collections.h>
    #include <winrt/Windows.system.h>
    #include <winrt/windows.ui.xaml.hosting.h>
    #include <windows.ui.xaml.hosting.desktopwindowxamlsource.h>
    #include <winrt/windows.ui.xaml.controls.h>
    #include <winrt/Windows.ui.xaml.media.h>
    #include <winrt/Windows.UI.Core.h>
    #include <winrt/MyUWPApp.h>

    using namespace winrt;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Xaml::Hosting;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI::Xaml::Controls;
    ```

3. Abre el archivo **MyDesktopWin32App.cpp** y agrega el código siguiente a la sección `Global Variables:`.

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. En el mismo archivo, agrega el siguiente código a la sección `Forward declarations of functions included in this code module:`.

    ```cpp
    void AdjustLayout(HWND);
    ```

5. En el mismo archivo, agrega el siguiente código inmediatamente después del comentario `TODO: Place code here.` en la función `wWinMain`.

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. En el mismo archivo, reemplaza la función `InitInstance` predeterminada por el código siguiente.

    ```cpp
    BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
    {
        hInst = hInstance; // Store instance handle in our global variable

        HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
            CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

        if (!hWnd)
        {
            return FALSE;
        }

        // Begin XAML Islands walkthrough code.
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            check_hresult(interop->AttachToWindow(hWnd));
            HWND hWndXamlIsland = nullptr;
            interop->get_WindowHandle(&hWndXamlIsland);
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(hWndXamlIsland, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
            _myUserControl = winrt::MyUWPApp::MyUserControl();
            _desktopWindowXamlSource.Content(_myUserControl);
        }
        // End XAML Islands walkthrough code.

        ShowWindow(hWnd, nCmdShow);
        UpdateWindow(hWnd);

        return TRUE;
    }
    ```

7. En el mismo archivo, agrega la siguiente función nueva al final del archivo.

    ```cpp
    void AdjustLayout(HWND hWnd)
    {
        if (_desktopWindowXamlSource != nullptr)
        {
            auto interop = _desktopWindowXamlSource.as<IDesktopWindowXamlSourceNative>();
            HWND xamlHostHwnd = NULL;
            check_hresult(interop->get_WindowHandle(&xamlHostHwnd));
            RECT windowRect;
            ::GetWindowRect(hWnd, &windowRect);
            ::SetWindowPos(xamlHostHwnd, NULL, 0, 0, windowRect.right - windowRect.left, windowRect.bottom - windowRect.top, SWP_SHOWWINDOW);
        }
    }
    ```

8. En el mismo archivo, busca la función `WndProc`. Reemplaza el controlador de `WM_DESTROY` predeterminado en la instrucción switch por el código siguiente.

    ```cpp
    case WM_DESTROY:
        PostQuitMessage(0);
        if (_desktopWindowXamlSource != nullptr)
        {
            _desktopWindowXamlSource.Close();
            _desktopWindowXamlSource = nullptr;
        }
        break;
    case WM_SIZE:
        AdjustLayout(hWnd);
        break;
    ```

9. Guarde el archivo.
10. Compila la solución y confirma que dicho proceso se realiza correctamente.

## <a name="test-the-app"></a>Pruebas de la aplicación

Ejecuta la solución y confirma que **MyDesktopWin32App** se abre con la siguiente ventana.

![Aplicación MyDesktopWin32App](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>Pasos siguientes

Muchas aplicaciones de escritorio que hospedan islas XAML deberán controlar escenarios adicionales para ofrecer una experiencia de usuario fluida. Por ejemplo, es posible que las aplicaciones de escritorio deban controlar la entrada del teclado en las islas XAML y la navegación del foco entre islas XAML y otros elementos de la interfaz de usuario, así como los cambios de diseño.

Para obtener más información acerca de cómo controlar estos escenarios y obtener vínculos a ejemplos de código relacionados, consulta [Escenarios avanzados para islas XAML en aplicaciones Win32 de C++](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Temas relacionados

* [Cómo usar los controles XAML de UWP en aplicaciones de escritorio (islas XAML)](xaml-islands.md)
* [Uso de la API de hospedaje XAML de UWP en una aplicación Win32 de C++](using-the-xaml-hosting-api.md)
* [Hospedaje de un control estándar de UWP en una aplicación Win32 de C++](host-standard-control-with-xaml-islands-cpp.md)
* [Escenarios avanzados para islas XAML en aplicaciones Win32 en C++](advanced-scenarios-xaml-islands-cpp.md)
* [Ejemplos de código de las islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
