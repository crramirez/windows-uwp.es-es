---
description: En este artículo se muestra cómo hospedar un control de UWP C++ personalizado en una aplicación Win32 mediante la API de hospedaje de XAML.
title: Hospedar un control personalizado de UWP C++ en una aplicación Win32 mediante la API de hospedaje de XAML
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, C++, Win32, Islas XAML, controles personalizados, controles de usuario, controles host
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 2f34c9c56cf9db5dfcfd702b97f2d34273b86e6a
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226319"
---
# <a name="host-a-custom-uwp-control-in-a-c-win32-app"></a>Hospedar un control personalizado de UWP C++ en una aplicación Win32

En este artículo se muestra cómo usar la [API de hospedaje XAML de UWP](using-the-xaml-hosting-api.md) para hospedar un control XAML de C++ UWP personalizado en una nueva aplicación de Win32. Si tiene un proyecto de C++ aplicación de Win32 existente, puede adaptar estos pasos y ejemplos de código para el proyecto.

Para hospedar un control XAML de UWP personalizado, creará los siguientes proyectos y componentes como parte de este tutorial:

* **Proyecto de aplicación de escritorio de Windows**. Este proyecto implementa una aplicación de C++ escritorio de Win32 nativa. Agregará código a este proyecto que usa la API de hospedaje XAML de UWP para hospedar un control XAML de UWP personalizado.

* **Proyecto de aplicación paraC++UWP (/WinRT)** . Este proyecto implementa un control XAML de UWP personalizado. También implementa un proveedor de metadatos raíz para cargar los metadatos para los tipos XAML de UWP personalizados en el proyecto.

## <a name="requirements"></a>Requisitos

* Visual Studio 2019 versión 16.4.3 o posterior.
* Windows 10, versión 1903 SDK (versión 10.0.18362) o posterior.
* [Extensión de Visual Studio/WinRT (VSIX) instalada con Visual Studio. C++](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) C++/WinRT es una moderna proyección de lenguaje C++17 totalmente estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivos de encabezado y diseñada para darte acceso de primera clase a la API moderna de Windows. Para obtener más información, vea [ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/).

## <a name="create-a-desktop-application-project"></a>Crear un proyecto de aplicación de escritorio

1. En Visual Studio, cree un nuevo proyecto de **aplicación de escritorio de Windows** denominado **MyDesktopWin32App**. Esta plantilla de proyecto está disponible en **C++** los filtros de proyecto de, **Windows**y proyecto de **escritorio** .

2. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución, haga clic en **redestinar solución**, seleccione el **10.0.18362.0** o una versión posterior del SDK y, a continuación, haga clic en **Aceptar**.

3. Instale el paquete NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) para habilitar la compatibilidad con [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis) en el proyecto:

    1. Haga clic con el botón derecho en el proyecto **MyDesktopWin32App** en **Explorador de soluciones** y elija **administrar paquetes NuGet**.
    2. Seleccione la pestaña **examinar** , busque el paquete [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) e instale la versión más reciente de este paquete.

4. En la ventana **administrar paquetes Nuget** , instale los siguientes paquetes de Nuget adicionales:

    * [Microsoft. Toolkit. Win32. UI. SDK](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.SDK) (versión v 6.0.0 o posterior). Este paquete proporciona varios recursos de compilación y tiempo de ejecución que permiten a las islas XAML trabajar en la aplicación.
    * [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) (versión v 6.0.0 o posterior).
    * [Microsoft. VCRTForwarders. 140](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140).

5. Compile la solución y confirme que se compila correctamente.

## <a name="create-a-uwp-app-project"></a>Creación de un proyecto de aplicación para UWP

Después, agregue un proyecto de aplicación de **UWP (C++/WinRT)** a la solución y realice algunos cambios de configuración en este proyecto. Más adelante en este tutorial, agregará código a este proyecto para implementar un control XAML de UWP personalizado y definir una instancia de la clase [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) proporcionada por el kit de herramientas de la comunidad de Windows. La aplicación usará esta clase como un proveedor de metadatos raíz para cargar los metadatos para los tipos XAML personalizados de UWP.

1. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución y seleccione **Agregar** -> **nuevo proyecto**.

2. Agregue un proyecto de **aplicaciónC++en blanco (/WinRT)** a la solución. Asigne al proyecto el nombre **MyUWPApp** y asegúrese de que la versión de destino y la versión mínima están establecidas en **Windows 10, versión 1903** o posterior.

3. Instale el paquete NuGet [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) en el proyecto **MyUWPApp** :

    1. Haga clic con el botón derecho en el proyecto **MyUWPApp** y elija **administrar paquetes NuGet**.
    2. Seleccione la pestaña **examinar** , busque el paquete [Microsoft. Toolkit. Win32. UI. XamlApplication](https://www.nuget.org/packages/Microsoft.Toolkit.Win32.UI.XamlApplication) e instale v 6.0.0 o una versión posterior de este paquete.

4. Haga clic con el botón secundario en el nodo **MyUWPApp** y seleccione **propiedades**. En la página **propiedades comunes** ->  **C++/WinRT** , establezca las siguientes propiedades y, a continuación, haga clic en **aplicar**.

    * Establezca el nivel de **detalle** en **normal**.
    * Establezca **optimizado** en **no**.

    Cuando haya terminado, la página de propiedades debe tener el siguiente aspecto.

    ![C++Propiedades del proyecto/WinRT](images/xaml-islands/xaml-island-cpp-1.png)

5. En la **página Propiedades de configuración** -> **General** de la ventana Propiedades, establezca **tipo de configuración** en **biblioteca dinámica (. dll)** y, a continuación, haga clic en **Aceptar** para cerrar la ventana Propiedades.

    ![Propiedades generales del proyecto](images/xaml-islands/xaml-island-cpp-2.png)

6. Agregue un archivo ejecutable de marcador de posición al proyecto **MyUWPApp** . Este archivo ejecutable de marcador de posición es necesario para que Visual Studio genere los archivos de proyecto necesarios y compile correctamente el proyecto.

    1. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo del proyecto **MyUWPApp** y seleccione **Agregar** -> **nuevo elemento**.
    2. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **utilidad** en la página izquierda y, a continuación, seleccione **archivo de texto (. txt)** . Escriba el nombre **placeholder. exe** y haga clic en **Agregar**.
      ![Agregar archivo de texto](images/xaml-islands/xaml-island-cpp-3.png)
    3. En **Explorador de soluciones**, seleccione el archivo **placeholder. exe** . En la ventana **propiedades** , asegúrese de que la propiedad **contenido** esté establecida en **true**.
    4. En **Explorador de soluciones**, haga clic con el botón derecho en el archivo **Package. appxmanifest** en el proyecto **MyUWPApp** , seleccione **abrir con**, seleccione **Editor XML (texto)** y haga clic en **Aceptar**.
    5. Busque el elemento **&lt;&gt;** de la aplicación y cambie el atributo **ejecutable** al valor `placeholder.exe`. Cuando haya terminado, el elemento **&lt;aplicación&gt;** debe ser similar a este.

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

    6. Guarde y cierre el archivo **Package. appxmanifest** .

7. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo **MyUWPApp** y seleccione **descargar el proyecto**.
8. Haga clic con el botón secundario en el nodo **MyUWPApp** y seleccione **Editar MyUWPApp. vcxproj**.
9. Busque el elemento `<Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />` y reemplácelo por el siguiente código XML. Este XML agrega varias propiedades nuevas inmediatamente antes del elemento.

    ```xml
    <PropertyGroup Label="Globals">
        <WindowsAppContainer>true</WindowsAppContainer>
        <AppxGeneratePriEnabled>true</AppxGeneratePriEnabled>
        <ProjectPriIndexName>App</ProjectPriIndexName>
        <AppxPackage>true</AppxPackage>
    </PropertyGroup>
    <Import Project="$(VCTargetsPath)\Microsoft.Cpp.Default.props" />
    ```

10. Guarde y cierre el archivo de proyecto.
11. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo **MyUWPApp** y seleccione **volver a cargar el proyecto**.

## <a name="configure-the-solution"></a>Configurar la solución

En esta sección, actualizará la solución que contiene ambos proyectos para configurar las dependencias del proyecto y las propiedades de compilación necesarias para que los proyectos se compilen correctamente.

1. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución y agregue un nuevo archivo XML denominado **Solution. props**.
2. Agregue el siguiente código XML al archivo **Solution. props** .

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

3. En el menú **Ver** , haga clic en **Administrador de propiedades** (dependiendo de la configuración, es posible que esté en **vista** -> **otras ventanas**).
4. En la ventana de **Administrador de propiedades** , haga clic con el botón secundario en **MyDesktopWin32App** y seleccione **Agregar hoja de propiedades existente**. Navegue hasta el archivo **Solution. props** que acaba de agregar y haga clic en **abrir**.
5. Repita el paso anterior para agregar el archivo **Solution. props** al proyecto **MyUWPApp** en la ventana **Administrador de propiedades** .
6. Cierre la ventana **Administrador de propiedades** .
7. Confirme que los cambios de la hoja de propiedades se guardaron correctamente. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **MyDesktopWin32App** y elija **propiedades**. Haga clic en **propiedades de configuración** -> **genneral**y confirme que el **Directorio de salida** y las propiedades del **directorio intermedio** tienen los valores agregados al archivo **. props** de la solución. También puede confirmar lo mismo para el proyecto **MyUWPApp** .
    ![propiedades del proyecto](images/xaml-islands/xaml-island-cpp-4.png)

8. En **Explorador de soluciones**, haga clic con el botón secundario en el nodo de la solución y elija **dependencias del proyecto**. En el menú desplegable **proyectos** , asegúrese de que **MyDesktopWin32App** está seleccionado y seleccione **MyUWPApp** en la lista **depende de** .
    ![las dependencias del proyecto](images/xaml-islands/xaml-island-cpp-5.png)

9. Haga clic en **Aceptar**.

## <a name="add-code-to-the-uwp-app-project"></a>Agregar código al proyecto de aplicación para UWP

Ahora está listo para agregar código al proyecto **MyUWPApp** para realizar estas tareas:

* Implemente un control XAML de UWP personalizado. Más adelante en este tutorial, agregará código que hospeda este control en el proyecto **MyDesktopWin32App** .
* Defina un tipo que se derive de la clase [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) en el kit de herramientas de la comunidad de Windows. Esta clase actúa como proveedor de metadatos raíz para cargar los metadatos para los tipos XAML personalizados de UWP.

### <a name="define-a-custom-uwp-xaml-control"></a>Definir un control XAML de UWP personalizado

1. En **Explorador de soluciones**, haga clic con el botón secundario en **MyUWPApp** y seleccione **Agregar** -> **nuevo elemento**. Seleccione el **nodo C++ visual** en el panel izquierdo, seleccione **control de usuario enC++blanco (/WinRT)** , asígnele el nombre **MyUserControl**y haga clic en **Agregar**.
2. En el editor XAML, reemplace el contenido del archivo **MyUserControl. Xaml** por el código XAML siguiente y, después, guarde el archivo.

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

### <a name="define-a-xamlapplication-class"></a>Definir una clase XamlApplication

A continuación, revise la clase de **aplicación** predeterminada en el proyecto **MyUWPApp** para derivar de la clase [Microsoft. Toolkit. Win32. UI. XamlHost. XamlApplication](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/tree/master/Microsoft.Toolkit.Win32.UI.XamlApplication) proporcionada por el kit de herramientas de la comunidad de Windows. Más adelante en este tutorial, actualizará el proyecto de escritorio para crear una instancia de esta clase como proveedor de metadatos raíz para cargar los metadatos para los tipos XAML de UWP personalizados.

  > [!NOTE]
  > Cada solución que usa islas XAML solo puede contener un proyecto que define un objeto `XamlApplication`. Todos los controles XAML de UWP personalizados de la aplicación comparten el mismo objeto de `XamlApplication`. 

1. En **Explorador de soluciones**, haga clic con el botón secundario en el archivo **mainpage. Xaml** en el proyecto **MyUWPApp** . Haga clic en **quitar** y luego en **eliminar** para eliminar este archivo permamently del proyecto.
2. En el proyecto **MyUWPApp** , expanda archivo **app. Xaml** .
3. Reemplace el contenido de los **archivos app. Xaml**, **app. cpp**, **app. h**y **app. idl** por el código siguiente.

    * **App. Xaml**:

        ```xml
        <Toolkit:XamlApplication
            x:Class="MyUWPApp.App"
            xmlns:Toolkit="using:Microsoft.Toolkit.Win32.UI.XamlHost"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:local="using:MyUWPApp">
        </Toolkit:XamlApplication>
        ```

    * **App. idl**:

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

    * **App. h**:

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

    * **App. cpp**:

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

4. Agregue un nuevo archivo de encabezado al proyecto **MyUWPApp** denominado **app. base. h**.
5. Agregue el código siguiente al archivo **app. base. h** , guarde el archivo y ciérrelo.

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

6. Compile la solución y confirme que se compila correctamente.

## <a name="configure-the-desktop-project-to-consume-custom-control-types"></a>Configurar el proyecto de escritorio para usar tipos de controles personalizados

Antes de que la aplicación **MyDesktopWin32App** pueda hospedar un control XAML de UWP personalizado en una isla XAML, debe configurarse para usar tipos de control personalizados del proyecto **MyUWPApp** . Hay dos maneras de hacerlo y puede elegir cualquiera de las opciones a medida que complete este tutorial.

### <a name="option-1-package-the-app-using-msix"></a>Opción 1: empaquetar la aplicación con MSIX

Puede empaquetar la aplicación en un [paquete de MSIX](https://docs.microsoft.com/windows/msix) para la implementación. MSIX es la tecnología moderna de empaquetado de aplicaciones para Windows y se basa en una combinación de tecnologías de instalación de MSI,. appx, App-V y ClickOnce.

1. Agregue un nuevo [proyecto de paquete de aplicación de Windows](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) a la solución. Cuando cree el proyecto, asígnele el nombre **MyDesktopWin32Project** y seleccione **Windows 10, versión 1903 (10,0; Compilación 18362)** para la **versión de destino** y la **versión mínima**.

2. En el proyecto de empaquetado, haga clic con el botón secundario en el nodo **aplicaciones** y elija **Agregar referencia**. En la lista de proyectos, active la casilla situada junto al proyecto **MyDesktopWin32App** y haga clic en **Aceptar**.
    proyecto de referencia de ![](images/xaml-islands/xaml-island-cpp-6.png)

> [!NOTE]
> Si decide no empaquetar la aplicación en un [paquete de MSIX](https://docs.microsoft.com/windows/msix) para la implementación, los equipos que ejecutan la aplicación deben tener instalado [Visual C++ Runtime](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads) .

### <a name="option-2-create-an-application-manifest"></a>Opción 2: crear un manifiesto de aplicación

Puede Agregar un [manifiesto de aplicación](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) a la aplicación.

1. Haga clic con el botón derecho en el proyecto **MyDesktopWin32App** y seleccione **Agregar** -> **nuevo elemento**. 
2. En el cuadro de diálogo **Agregar nuevo elemento** , haga clic en **Web** en el panel izquierdo y seleccione **archivo XML (. xml)** . 
3. Asigne al nuevo archivo el nombre **app. manifest** y haga clic en **Agregar**.
4. Reemplace el contenido del nuevo archivo por el siguiente código XML. Este XML registra los tipos de control personalizados en el proyecto **MyUWPApp** .

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

## <a name="configure-additional-desktop-project-properties"></a>Configurar propiedades adicionales del proyecto de escritorio

A continuación, actualice el proyecto **MyDesktopWin32App** para definir una macro para directorios de inclusión adicionales y configurar propiedades adicionales.

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **MyDesktopWin32App** y seleccione **descargar el proyecto**.

2. Haga clic con el botón derecho en **MyDesktopWin32App (descargado)** y seleccione **Editar MyDesktopWin32App. vcxproj**.

3. Agregue el siguiente código XML justo delante de la etiqueta de cierre `</Project>` al final del archivo. A continuación, guarde y cierre el archivo.

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

4. En **Explorador de soluciones**, haga clic con el botón secundario en **MyDesktopWin32App (descargado)** y seleccione **volver a cargar el proyecto**.

5. Haga clic con el botón secundario en **MyDesktopWin32App**, seleccione **propiedades**y haga clic en el nodo **C/C++**  del panel izquierdo. Confirme que la macro **directorios de inclusión adicionales** se ha definido a partir del cambio de archivo del proyecto realizado en el paso anterior.
    ![configuración deC++ C/Project](images/xaml-islands/xaml-island-cpp-7.png)

6. En el cuadro de diálogo **páginas de propiedades** , expanda **herramienta manifiesto** -> **entrada y salida**. Establezca la propiedad **reconocimiento de PPP** en compatible con **altas ppp por monitor**. Si no establece esta propiedad, es posible que se produzca un error de configuración del manifiesto en ciertos escenarios altos de PPP.
    ![configuración deC++ C/Project](images/xaml-islands/xaml-island-cpp-8.png)

## <a name="host-the-custom-uwp-xaml-control-in-the-desktop-project"></a>Hospedar el control XAML de UWP personalizado en el proyecto de escritorio

Por último, está listo para agregar código al proyecto **MyDesktopWin32App** para hospedar el control XAML personalizado de UWP que definió anteriormente en el proyecto **MyUWPApp** .

1. En el proyecto **MyDesktopWin32App** , abra el archivo **Framework. h** y comente la siguiente línea de código. Cuando haya terminado, guarde el archivo.

    ```cpp
    #define WIN32_LEAN_AND_MEAN
    ```

2. Abra el archivo **MyDesktopWin32App. h** y reemplace el contenido de este archivo por el código siguiente para hacer referencia C++a los archivos de encabezado/WinRT necesarios. Cuando haya terminado, guarde el archivo.

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

3. Abra el archivo **MyDesktopWin32App. cpp** y agregue el código siguiente a la sección `Global Variables:`.

    ```cpp
    winrt::MyUWPApp::App hostApp{ nullptr };
    winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource _desktopWindowXamlSource{ nullptr };
    winrt::MyUWPApp::MyUserControl _myUserControl{ nullptr };
    ```

4. En el mismo archivo, agregue el siguiente código a la sección `Forward declarations of functions included in this code module:`.

    ```cpp
    void AdjustLayout(HWND);
    ```

5. En el mismo archivo, agregue el siguiente código inmediatamente después del comentario `TODO: Place code here.` en la función `wWinMain`.

    ```cpp
    // TODO: Place code here.
    winrt::init_apartment(winrt::apartment_type::single_threaded);
    hostApp = winrt::MyUWPApp::App{};
    _desktopWindowXamlSource = winrt::Windows::UI::Xaml::Hosting::DesktopWindowXamlSource{};
    ```

6. En el mismo archivo, reemplace la función `InitInstance` predeterminada por el código siguiente.

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

7. En el mismo archivo, agregue la siguiente función nueva al final del archivo.

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

8. En el mismo archivo, busque la función `WndProc`. Reemplace el controlador de `WM_DESTROY` predeterminado en la instrucción switch por el código siguiente.

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
10. Compile la solución y confirme que se compila correctamente.

## <a name="test-the-app"></a>Prueba de la aplicación

Ejecute la solución y confirme que **MyDesktopWin32App** se abre con la siguiente ventana.

![Aplicación MyDesktopWin32App](images/xaml-islands/xaml-island-cpp-9.png)

## <a name="next-steps"></a>Pasos siguientes

Muchas aplicaciones de escritorio que hospedan islas XAML deberán controlar escenarios adicionales para proporcionar una experiencia de usuario fluida. Por ejemplo, las aplicaciones de escritorio pueden necesitar controlar la entrada del teclado en las islas XAML, la navegación centrada entre las islas XAML y otros elementos de la interfaz de usuario y los cambios de diseño.

Para obtener más información sobre cómo controlar estos escenarios y punteros a ejemplos de código relacionados, vea [escenarios avanzados para C++ islas XAML en aplicaciones Win32](advanced-scenarios-xaml-islands-cpp.md).

## <a name="related-topics"></a>Temas relacionados

* [Hospedar controles XAML de UWP en aplicaciones de escritorio (Islas XAML)](xaml-islands.md)
* [Uso de la API de hospedaje XAML de C++ UWP en una aplicación Win32](using-the-xaml-hosting-api.md)
* [Hospedar un control estándar de UWP C++ en una aplicación Win32](host-standard-control-with-xaml-islands-cpp.md)
* [Escenarios avanzados para Islas XAML en C++ aplicaciones Win32](advanced-scenarios-xaml-islands-cpp.md)
* [Ejemplos de código de islas XAML](https://github.com/microsoft/Xaml-Islands-Samples)
