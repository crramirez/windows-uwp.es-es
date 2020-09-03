---
title: Creación de una aplicación "Hola mundo" con C++/WinRT
description: En este tema se explica cómo crear una aplicación "Hola mundo" para UWP de Windows 10 con C++/WinRT. La interfaz de usuario de la aplicación se define en lenguaje XAML.
ms.date: 07/11/2020
ms.topic: article
keywords: windows 10, uwp, cppwinrt, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: bb6a76f2e8096d63907daf5ededdb6a22eb72a6c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175209"
---
# <a name="create-a-hello-world-app-using-cwinrt"></a>Creación de una aplicación "Hola mundo" con C++/WinRT

En este tema se explica cómo crear una aplicación "Hola mundo" para la Plataforma universal de Windows (UWP) de Windows 10 con C++/WinRT. La interfaz de usuario (UI) de la aplicación se define en lenguaje XAML.

C++/WinRT es una proyección del lenguaje C++17, moderna y totalmente estándar para las API de Windows Runtime (WinRT). Para más información, así como más tutoriales y ejemplos de código, consulte la documentación de [C++/WinRT](../cpp-and-winrt-apis/index.md). Un buen tema para comenzar es [Introducción a C++/WinRT](../cpp-and-winrt-apis/get-started.md).

## <a name="set-up-visual-studio-for-cwinrt"></a>Configuración de Visual Studio para C++/WinRT

Para más información sobre cómo instalar Visual Studio para la implementación de C++/WinRT &mdash;incluida la instalación y el uso de la Extensión de Visual Studio (VSIX) para C++/WinRT y el paquete NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación)&mdash;, consulta [Compatibilidad de Visual Studio para C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Para descargar Visual Studio, consulte [Descargas](https://visualstudio.microsoft.com/downloads/).

Para una introducción a XAML, vea [Información general sobre XAML](../xaml-platform/xaml-overview.md)

## <a name="create-a-blank-app-helloworldcppwinrt"></a>Creación de una aplicación vacía (HelloWorldCppWinRT)

Nuestra primera aplicación es una aplicación "Hola mundo" que muestra algunas características básicas de interactividad, diseño y estilos.

Para empezar, crea un proyecto en Microsoft Visual Studio. Cree un proyecto **Blank App (C++/WinRT)** [Aplicación vacía (C++/WinRT)] y asígnele el nombre *HelloWorldCppWinRT*. Asegúrese de que la opción **Colocar la solución y el proyecto en el mismo directorio** esté desactivada. Elija como destino la versión más reciente disponible de manera general (es decir, no en versión preliminar) de Windows SDK.

En una sección posterior de este tema, se te indicará que compiles el proyecto (pero no lo compiles hasta entonces).

### <a name="about-the-project-files"></a>Sobre los archivos de proyecto

Normalmente, en la carpeta del proyecto, cada archivo `.xaml` (marcado XAML) tiene unos archivos `.idl`, `.h` y `.cpp` correspondientes. Juntos, estos archivos se compilan en un tipo de página XAML.

Puede modificar un archivo de marcado XAML para crear elementos de interfaz de usuario, y puede enlazar esos elementos a orígenes de datos (una tarea conocida como [enlace de datos](../data-binding/index.md)). Modifique los archivos `.h` y `.cpp` (y, en ocasiones, el archivo `.idl`) para agregar lógica personalizada a la página XAML como, por ejemplo, controladores de eventos.

Veamos algunos archivos del proyecto.

- `App.idl`, `App.xaml`, `App.h` y `App.cpp`. Estos archivos representan la especialización de la aplicación de la clase [**Windows::UI::Xaml::Application**](/uwp/api/windows.ui.xaml.application), que incluye el punto de entrada de la aplicación. `App.xaml` no contiene ningún marcado específico de página, pero puede agregar estilos de elementos de interfaz de usuario, así como cualquier otro elemento que quiera que esté accesible desde todas las páginas. Los archivos `.h` y `.cpp` contienen controladores para distintos eventos del ciclo de vida de la aplicación. Por lo general, se agrega código personalizado allí para inicializar la aplicación cuando se inicia y para realizar la limpieza cuando se suspende o finaliza.
- `MainPage.idl`, `MainPage.xaml`, `MainPage.h` y `MainPage.cpp`. Contenga el marcado XAML, y la implementación, para el tipo predeterminado de página principal (inicio) en una aplicación, que es la clase de tiempo de ejecución **MainPage**. **MainPage** no admite la navegación, pero proporciona una interfaz de usuario predeterminada y un controlador de eventos para ayudarle a comenzar.
- `pch.h` y `pch.cpp`. Estos archivos representan el archivo de encabezado precompilado del proyecto. En `pch.h`, incluya los archivos de encabezado que no cambian con frecuencia y, a continuación, incluya `pch.h` en otros archivos del proyecto.

## <a name="a-first-look-at-the-code"></a>Un primer vistazo al código

### <a name="runtime-classes"></a>Clases en tiempo de ejecución

Como sabe, todas las clases de una aplicación para Plataforma universal de Windows (UWP) escrita en C# son de tipos de Windows Runtime. Sin embargo, cuando creas un tipo en una aplicación de C++/WinRT, puedes elegir si ese tipo es un tipo de Windows Runtime o una clase, estructura o enumeración de C++ normal.

Cualquier tipo de página XAML del proyecto debe ser un tipo de Windows Runtime. Por lo tanto, **MainPage** es un tipo de Windows Runtime. En concreto, se trata de una *clase en tiempo de ejecución*. Cualquier tipo que una página XAML use también debe ser un tipo de Windows Runtime. Al escribir un [componente de Windows Runtime](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md), si desea crear un tipo que se pueda usar desde otra aplicación, debe crear un tipo de Windows Runtime. En otros casos, el tipo puede ser un tipo C++ normal. En general, un tipo de Windows Runtime se puede usar con cualquier lenguaje de Windows Runtime.

Una buena indicación de que un tipo es un tipo de Windows Runtime es que se define en [Lenguaje de definición de interfaz de Microsoft (MIDL)](/uwp/midl-3/) dentro de un archivo de lenguaje de definición de interfaz (`.idl`). Tomemos **MainPage** como ejemplo.

```idl
// MainPage.idl
namespace HelloWorldCppWinRT
{
    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        Int32 MyProperty;
    }
}
```

Esta es la estructura básica de la implementación de la clase de tiempo de ejecución **MainPage** y su generador de activación, tal como se muestra en `MainPage.h`.

```cppwinrt
// MainPage.h
...
namespace winrt::HelloWorldCppWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        int32_t MyProperty();
        void MyProperty(int32_t value);
        ...
    };
}

namespace winrt::HelloWorldCppWinRT::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```    

Para obtener más información sobre si debes crear una clase en tiempo de ejecución para un tipo determinado, consulta el tema [Crear API con C++/WinRT](../cpp-and-winrt-apis/author-apis.md). Para más información sobre la conexión entre clases de tiempo de ejecución e IDL (archivos `.idl`), lea y siga con el tema [Controles de XAML; enlazar a una propiedad de C++/WinRT](../cpp-and-winrt-apis/binding-property.md). En ese tema encontrarás una guía por el proceso de creación de una clase en tiempo de ejecución, cuyo primer paso es agregar un nuevo elemento **Archivo Midl (.idl)** al proyecto.

Ahora vamos a agregar alguna funcionalidad al proyecto **HelloWorldCppWinRT**.

## <a name="step-1-modify-your-startup-page"></a>Paso 1. Modificación de la página de inicio

En el **Explorador de soluciones**, abra `MainPage.xaml` para que pueda crear los controles que forman la interfaz de usuario (UI).

Elimine el elemento **StackPanel** que ya está allí, así como su contenido. En su lugar, pegue el siguiente código XAML.

```xaml
<StackPanel x:Name="contentPanel" Margin="120,30,0,0">
    <TextBlock HorizontalAlignment="Left" Text="Hello, World!" FontSize="36"/>
    <TextBlock Text="What's your name?"/>
    <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
        <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
        <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
    </StackPanel>
    <TextBlock x:Name="greetingOutput"/>
</StackPanel>
```

Este nuevo [**StackPanel**](/uwp/api/Windows.UI.Xaml.Controls.StackPanel) tiene un elemento [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) que solicita el nombre del usuario, un elemento [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) que acepta el nombre del usuario, un elemento [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) y otro elemento **TextBlock**.

Dado que se ha eliminado el elemento **Button** denominado *myButton*, tendrá que quitar del código la referencia que se le haga. Por lo tanto, en `MainPage.cpp`, elimine la línea de código dentro de la función **MainPage::ClickHandler**.

En este punto, ha creado una aplicación universal de Windows muy básica. Para ver el aspecto de la aplicación para UWP, compile y ejecute la aplicación.

![Pantalla de la aplicación para UWP, con controles](images/xaml-hw-app2.png)

En la aplicación, puede escribir en el cuadro de texto. Pero al hacer clic en el botón todavía no hace nada.

## <a name="step-2-add-an-event-handler"></a>Paso 2. Adición de un controlador de eventos

En `MainPage.xaml`, busque el elemento **Button** denominado *inputButton* y declare un controlador de eventos para su evento [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). El marcado de **Button** debe parecerse al siguiente.

```xaml
<Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="inputButton_Click"/>
```

Implemente el controlador de eventos de esta manera.

```cppwinrt
// MainPage.h
struct MainPage : MainPageT<MainPage>
{
    ...
    void inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e);
};

// MainPage.cpp
namespace winrt::HelloWorldCppWinRT::implementation
{
    ...
    void MainPage::inputButton_Click(
        winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e)
    {
        greetingOutput().Text(L"Hello, " + nameInput().Text() + L"!");
    }
}
```

Para más información, consulte [Control de eventos mediante delegados](../cpp-and-winrt-apis/handle-events.md).

La implementación recupera el nombre del usuario del cuadro de texto, lo usa para crear un saludo y lo muestra en el bloque de texto *greetingOutput*.

Compila y ejecuta la aplicación. Escriba su nombre en el cuadro de texto y haga clic en el botón. La aplicación muestra un saludo personalizado.

![Pantalla de la aplicación mostrando un mensaje](images/xaml-hw-app3.png)

## <a name="step-3-style-the-startup-page"></a>Paso 3. Aplicación de estilo a la página de inicio

### <a name="choose-a-theme"></a>Elección de un tema

Es fácil personalizar la apariencia de tu aplicación. De manera predeterminada, la aplicación usa recursos con un estilo de colores claros. Los recursos del sistema también incluyen un tema oscuro.

Para probar el tema oscuro, edite `App.xaml` y agregue un valor para [**Application::RequestedTheme**](/uwp/api/windows.ui.xaml.application.requestedtheme).

```xaml
<Application
    ...
    RequestedTheme="Dark">

</Application>
```

Recomendamos usar el tema oscuro para las aplicaciones que muestren principalmente imágenes o vídeo y el tema claro para las aplicaciones que contengan mucho texto. Si va a usar un esquema de colores personalizado, use el tema que mejor se ajuste a la apariencia de la aplicación.

> [!NOTE]
> El tema se aplica al iniciar la aplicación. No se puede cambiar mientras la aplicación está en ejecución.

### <a name="use-system-styles"></a>Uso de estilos del sistema

En esta sección, cambiaremos el aspecto del texto (por ejemplo, aumentaremos el tamaño de la fuente).

En `MainPage.xaml`, busque "What's your name?" en **TextBlock**. Establezca su propiedad [**Style**](/uwp/api/windows.ui.xaml.style) en una referencia a la clave del recurso del sistema *BaseTextBlockStyle*.

```xaml
<TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
```

*BaseTextBlockStyle* es la clave de un recurso definido en [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) en `\Program Files (x86)\Windows Kits\10\DesignTime\CommonConfiguration\Neutral\UAP\<version>\Generic\generic.xaml`. Estos son los valores de propiedad establecidos por ese estilo.

```xaml
<Style x:Key="BaseTextBlockStyle" TargetType="TextBlock">
    <Setter Property="FontFamily" Value="XamlAutoFontFamily" />
    <Setter Property="FontWeight" Value="SemiBold" />
    <Setter Property="FontSize" Value="14" />
    <Setter Property="TextTrimming" Value="None" />
    <Setter Property="TextWrapping" Value="Wrap" />
    <Setter Property="LineStackingStrategy" Value="MaxHeight" />
    <Setter Property="TextLineBounds" Value="Full" />
</Style>
```

También en `MainPage.xaml`, busque el elemento **TextBlock** denominado `greetingOutput`. Establezca su **Style** en *BaseTextBlockStyle* también. Si compila y ejecuta la aplicación ahora, verá que la apariencia de ambos bloques de texto ha cambiado (por ejemplo, el tamaño de fuente es ahora mayor).

## <a name="step-4-have-the-ui-adapt-to-different-window-sizes"></a>Paso 4. Adaptación de la interfaz de usuario a diferentes tamaños de ventana

Ahora haremos que la interfaz de usuario se adapte dinámicamente a un tamaño de ventana cambiante y que tenga un aspecto correcto en dispositivos con pantallas pequeñas. Para ello, agregará una sección [**VisualStateManager**](/uwp/api/Windows.UI.Xaml.VisualStateManager) a `MainPage.xaml`. Definirá distintos estados visuales para distintos tamaños de ventana y, a continuación, establecerá las propiedades que se aplicarán para cada uno de esos estados visuales.

### <a name="adjust-the-ui-layout"></a>Ajuste del diseño de interfaz de usuario

Agregue este bloque de XAML como primer elemento secundario del elemento raíz **StackPanel**.

```xaml
<StackPanel ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="wideState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="641" />
                </VisualState.StateTriggers>
            </VisualState>
            <VisualState x:Name="narrowState">
                <VisualState.StateTriggers>
                    <AdaptiveTrigger MinWindowWidth="0" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                    <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                    <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    ...
</StackPanel>
```

Compila y ejecuta la aplicación. Verá que la interfaz de usuario es igual que antes, hasta que se reduce el tamaño de la ventana a un ancho inferior a 641 píxeles independientes del dispositivo (DIP). En ese momento, se aplica el estado visual *narrowState* y, junto con él, todos los establecedores de propiedad definidos para ese estado.

El [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState) llamado `wideState` tiene un [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) con su propiedad [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) establecida en 641. Esto significa que el estado se aplicará únicamente cuando el ancho de la ventana no sea inferior al mínimo de 641 DIP. Para este estado no defina ningún objeto [**Setter**](/uwp/api/Windows.UI.Xaml.Setter), de este modo se usan las propiedades de diseño definidas en el XAML para el contenido de la página.

El segundo [**VisualState**](/uwp/api/Windows.UI.Xaml.VisualState), `narrowState`, tiene un [**AdaptiveTrigger**](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) con su propiedad [**MinWindowWidth**](/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth) establecida en 0. Este estado se aplica cuando el ancho de la ventana es mayor que 0, pero inferior a 641 DIP. En exactamente 641 DIP, `wideState` está en vigor. En `narrowState`, defina algunos objetos [**Setter**](/uwp/api/Windows.UI.Xaml.Setter) para cambiar las propiedades de diseño de los controles en la interfaz de usuario.

- Reduzca el margen izquierdo del elemento *contentPanel* de 120 a 20.
- Cambie [**Orientation**](/uwp/api/windows.ui.xaml.controls.stackpanel.orientation) del elemento  *inputPanel* de **Horizontal** a **Vertical**.
- Agregue un margen superior de 4 DIP al elemento *inputButton*.

## <a name="summary"></a>Resumen

En este tutorial se ha mostrado cómo se agrega contenido a una aplicación universal de Windows, cómo se le agrega interactividad y cómo se cambia su apariencia.