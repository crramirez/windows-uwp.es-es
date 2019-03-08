---
description: En este tema le guiará a través de los pasos de creación de un control personalizado simple con C / c++ / WinRT. Puede crear en la información aquí para crear sus propios controles de interfaz de usuario completos y personalizables.
title: Controles (con plantilla) personalizados de XAML con C++ / WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, XAML, un control personalizado con plantilla,
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ce4f7eea074233c625a2cc92ef773f0b06c2be9f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635150"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>Controles (con plantilla) personalizados de XAML con C++ / WinRT

> [!IMPORTANT]
> Los conceptos esenciales como términos que admiten la comprensión de cómo consumir y crear clases en tiempo de ejecución con [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), consulte [consumir las API con C++ / c++ / WinRT](consume-apis.md) y [API autor con C++ / c++ / WinRT](author-apis.md).

Una de las características más eficaces de la plataforma Universal de Windows (UWP) es la flexibilidad que proporciona la pila de la interfaz de usuario (UI) para crear controles personalizados basados en el XAML [ **Control** ](/uwp/api/windows.ui.xaml.controls.control) tipo. El marco XAML UI proporciona características como [propiedades de dependencia personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties) y [propiedades adjuntas](/windows/uwp/xaml-platform/custom-attached-properties), y [plantillas de control](/windows/uwp/design/controls-and-patterns/control-templates), que facilitan la creación de controles de característica enriquecida y personalizables. En este tema le guiará a través de los pasos necesarios para crear un control personalizado (plantilla) con C++ / c++ / WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Creación de una aplicación en blanco (BgLabelControlApp)
Comienza creando un proyecto nuevo en Microsoft Visual Studio Crear un **Visual C++** > **Windows Universal** > **aplicación vacía (C++ / c++ / WinRT)** del proyecto y asígnele el nombre *BgLabelControlApp* . En una sección posterior de este tema, se le dirigirá al compilar el proyecto (no se compilan hasta ese momento).

Vamos a crear una nueva clase para representar un control personalizado (plantilla). Vamos a crear y consumir la clase dentro de la misma unidad de compilación. Pero queremos ser capaz de crear una instancia de esta clase desde el marcado XAML y para que se va a ser una clase en tiempo de ejecución del motivo. Y vamos a usar C++/WinRT para crearla y consumirla.

El primer paso para crear una nueva clase en tiempo de ejecución es agregar un nuevo elemento **Midl File (.idl)** al proyecto. Asígnale el nombre `BgLabelControl.idl`. Elimina el contenido predeterminado de `BgLabelControl.idl` y pégalo en esta declaración de clase en tiempo de ejecución.

```idl
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Windows.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

La lista anterior muestra el patrón que siguen al declarar una propiedad de dependencia (DP). Hay dos piezas para cada DP. En primer lugar, se declara una propiedad estática de solo lectura de tipo [ **DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty). Tiene el nombre de su DP más *propiedad*. Usará esta propiedad estática en la implementación. En segundo lugar, se declara una propiedad de instancia de lectura y escritura con el tipo y el nombre de su DP. Si desea crear un *propiedad adjunta* (en lugar de un DP), a continuación, vea los ejemplos de código en [propiedades adjuntas personalizadas](/windows/uwp/xaml-platform/custom-attached-properties).

> [!NOTE]
> Si desea un DP con un tipo de punto flotante, a continuación, hágalo `double` (`Double` en [MIDL 3.0](/uwp/midl-3/)). Declarar e implementar un DP de tipo `float` (`Single` en MIDL), y, a continuación, establecer un valor para ese DP en marcado XAML, se producirá el error *no se pudo crear un 'Windows.Foundation.Single' desde el texto '<NUMBER>'*.

Guarda el archivo y compila el proyecto. Durante el proceso de compilación, la herramienta `midl.exe` se ejecutará para crear un archivo de metadatos de Windows Runtime (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) que describe la clase en tiempo de ejecución. Después se ejecutará la herramienta `cppwinrt.exe` para generar archivos de código fuente y ayudarte a crear y consumir tu clase en tiempo de ejecución. Estos archivos incluyen códigos auxiliares para ayudarle a comenzar a implementar el **BgLabelControl** clase en tiempo de ejecución que declaró en el archivo IDL. Estos archivos de código auxiliar son `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` y `BgLabelControl.cpp`

Copia los archivos de código auxiliar`BgLabelControl.h``BgLabelControl.cpp` de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` a la carpeta del proyecto, que es `\BgLabelControlApp\BgLabelControlApp\`. En el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Haz clic con el botón derecho en los archivos de código auxiliar que has copiado y haz clic en **Incluir en el proyecto**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implemente el **BgLabelControl** clase del control personalizado
Ahora, vamos a abrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` y `BgLabelControl.cpp` e implementar nuestra clase en tiempo de ejecución. En `BgLabelControl.h`, cambie el constructor para establecer la implementación de claves, de estilo predeterminada **etiqueta** y **LabelProperty**, agregar un controlador de evento estático denominado **OnLabelChanged** a procesar los cambios en el valor de la propiedad de dependencia y agregar un miembro privado para almacenar el campo de respaldo para **LabelProperty**.

Después de agregarlos, su `BgLabelControl.h` tiene este aspecto.

```cppwinrt
// BgLabelControl.h
...
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
    BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

    winrt::hstring Label()
    {
        return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
    }

    void Label(winrt::hstring const& value)
    {
        SetValue(m_labelProperty, winrt::box_value(value));
    }

    static Windows::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const&, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const&);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

En `BgLabelControl.cpp`, defina los miembros estáticos similar al siguiente.

```cppwinrt
// BgLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
);

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // Call members of the projected type via theControl.

        BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
        // Call members of the implementation type via ptr.
    }
}
...
```

En este tutorial, no se usará con **OnLabelChanged**. Pero está ahí para que puedan ver cómo registrar una propiedad de dependencia con una devolución de llamada de cambio de propiedad. La implementación de **OnLabelChanged** también muestra cómo obtener un tipo proyectado derivado de un tipo proyectado base (es el tipo base proyectado **DependencyObject**, en este caso). Y se muestra cómo obtener un puntero al tipo que implementa el tipo proyectado. Esa segunda operación naturalmente sólo será posible en el proyecto que implementa el tipo proyectado (es decir, el proyecto que implementa la clase en tiempo de ejecución).

> [!NOTE]
> Si no ha instalado el SDK de Windows versión 10.0.17763.0 (Windows 10, versión 1809) o posterior, a continuación, necesita llamar a [ **winrt::from_abi** ](/uwp/cpp-ref-for-winrt/from-abi) en el controlador de evento de cambio de propiedad de dependencia anterior, en lugar de [ **winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

## <a name="design-the-default-style-for-bglabelcontrol"></a>El estilo predeterminado de diseño **BgLabelControl**

En su constructor, **BgLabelControl** establece una clave de estilo predeterminado para sí mismo. Pero, ¿qué *es* un estilo predeterminado? Un control personalizado (plantilla) debe tener un estilo predeterminado&mdash;que contiene una plantilla de control predeterminada&mdash;que se puede utilizar para representarse a sí misma con en caso de que el consumidor del control no establece un estilo o plantilla. En esta sección vamos a agregar un archivo de marcado para el proyecto que contiene el estilo predeterminado.

En el nodo del proyecto, cree una nueva carpeta y asígnele el nombre "Temas". En `Themes`, agregue un nuevo elemento de tipo **Visual C++** > **XAML** > **vista XAML**y asígnele el nombre "Generic.xaml". Los nombres de carpeta y archivo deben ser similar al siguiente para el marco de trabajo XAML buscar el estilo predeterminado para un control personalizado. Elimine el contenido predeterminado de `Generic.xaml`y pegue en el marcado siguiente.

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

En este caso, la única propiedad que establece el estilo predeterminado es la plantilla de control. La plantilla consta de un cuadrado (cuyo fondo está enlazado a la **en segundo plano** propiedad que todas las instancias de la XAML [ **Control** ](/uwp/api/windows.ui.xaml.controls.control) tener tipo) y un elemento de texto (cuya texto que se enlaza a la **BgLabelControl::Label** propiedad de dependencia).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Agregar una instancia de **BgLabelControl** a la página principal de la interfaz de usuario

Abre `MainPage.xaml`, que contiene la marcación XAML de nuestra página principal de la interfaz de usuario. Inmediatamente después de la **botón** elemento (dentro de la **StackPanel**), agregue el marcado siguiente.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Además, agregue la siguiente directiva #include `MainPage.h` para que el **MainPage** (una combinación de compilación de marcado XAML y código imperativo) de tipo es consciente de la **BgLabelControl** tipo de control personalizado. Si desea usar **BgLabelControl** desde otra página XAML, a continuación, agregue esta directiva al archivo de encabezado para esa página, mismo include demasiado. O bien, o bien, simplemente poner una sola directiva #include en el archivo de encabezado precompilado.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Ahora compila y ejecuta el proyecto. Verá que la plantilla predeterminada del control se enlaza el pincel del fondo y la etiqueta, de la **BgLabelControl** instancia en el marcado.

Este tutorial ha mostrado un ejemplo sencillo de un control personalizado (plantilla) en C / c++ / WinRT. Puede realizar sus propios controles personalizados arbitrariamente enriquecido y completa. Por ejemplo, un control personalizado puede adoptar la forma de algo tan complicado como una cuadrícula de datos editable, un Reproductor de vídeo o un visualizador de geometría 3D.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implementar *reemplazable* funciones, como **MeasureOverride** y **OnApplyTemplate**

Derivar un control personalizado de la [ **Control** ](/uwp/api/windows.ui.xaml.controls.control) clase en tiempo de ejecución, que a su vez más se deriva de las clases base en tiempo de ejecución. Y hay métodos reemplazables de **Control**, [ **FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement), y [ **UIElement** ](/uwp/api/windows.ui.xaml.uielement) que se puede reemplazar en la clase derivada. Este es un ejemplo de código que muestra cómo hacerlo.

```cppwinrt
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
...
    // Control overrides.
    void OnPointerPressed(Windows::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

    // FrameworkElement overrides.
    Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
    void OnApplyTemplate() const { ... };

    // UIElement overrides.
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
...
};
```

*Reemplazable* funciones presentan forma diferente en las proyecciones de lenguaje diferente. En C#, por ejemplo, las funciones reemplazables suelen aparecen como funciones virtuales protegidas. En C / c++ / WinRT, son no virtuales ni protegidos, pero todavía puede invalidarlos y proporcionar su propia implementación, como se indicó anteriormente.

## <a name="important-apis"></a>API importantes
* [Clase de control](/uwp/api/windows.ui.xaml.controls.control)
* [Clase DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement (clase)](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement (clase)](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Temas relacionados
* [Plantillas de control](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propiedades de dependencia personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties)
