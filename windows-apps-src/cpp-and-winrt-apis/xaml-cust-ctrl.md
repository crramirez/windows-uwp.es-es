---
description: Este tema te guía por los pasos de creación de un control personalizado sencillo con C++/WinRT. Puedes usar esta información para crear tus propios controles de interfaz de usuario repletos de características y personalizables.
title: Controles (basados en modelo) personalizados de XAML con C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, proyección, XAML, personalizado, basado en modelo, control
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: c0b2d8fb17b90bc55834f6bf2200b22af9352ef6
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270084"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>Controles (basados en modelo) personalizados de XAML con C++/WinRT

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), consulta [Consumir API con C++/WinRT](author-apis.md) y [Crear API con C++/WinRT ](consume-apis.md).

Una de las características más eficaces de la plataforma Universal de Windows (UWP) es la flexibilidad que ofrece la pila de la interfaz de usuario (UI) para crear controles personalizados basados en el tipo [**Control**](/uwp/api/windows.ui.xaml.controls.control) de XAML. El marco de interfaz de usuario de XAML ofrece características tales como [propiedades de dependencia personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties) y [propiedades adjuntas](/windows/uwp/xaml-platform/custom-attached-properties), así como [plantillas de control](/windows/uwp/design/controls-and-patterns/control-templates), que facilitan la creación de controles repletos de características y personalizables. Este tema te guía por los pasos de creación de un control (basado en modelo) personalizado con C++/WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Crear una aplicación en blanco (BgLabelControlApp)
Comienza creando un proyecto en Microsoft Visual Studio Crea un proyecto de **Aplicación vacía (C++/WinRT)** y asígnale el nombre *BgLabelControlApp*. En una sección posterior de este tema, se te indicará que compiles el proyecto (no lo compiles hasta entonces).

Vamos a crear una nueva clase que represente un control (basado en modelo) personalizado. Vamos a crear y consumir la clase dentro de la misma unidad de compilación. Pero queremos poder crear instancias de esta clase desde XAML y por ello tiene que ser una clase en tiempo de ejecución. Y vamos a usar C++/WinRT tanto para crearla como para consumirla.

El primer paso para crear una clase en tiempo de ejecución es agregar un nuevo elemento **Archivo MIDL (.idl)** al proyecto. Asígnale el nombre `BgLabelControl.idl`. Elimina el contenido predeterminado de `BgLabelControl.idl` y pégalo en esta declaración de clase en tiempo de ejecución.

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

La lista anterior muestra el patrón que se sigue al declarar una propiedad de dependencia (DP). Hay dos partes en cada DP. En primer lugar, se declara una propiedad estática de solo lectura de tipo [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty). Tiene el nombre de la DP más *Property*. Usarás esta propiedad estática en la implementación. En segundo lugar, se declara una propiedad de instancia de lectura y escritura con el tipo y el nombre del DP. Si quieres crear una *propiedad adjunta* (en lugar de una DP), consulta los ejemplos de código en [Propiedades adjuntas personalizadas](/windows/uwp/xaml-platform/custom-attached-properties).

> [!NOTE]
> Si quieres una DP con un tipo de punto flotante, conviértela en `double` (`Double` en [MIDL 3.0](/uwp/midl-3/)). Si se declara e implementa una DP de tipo `float` (`Single` en MIDL) y luego se establece un valor para esa DP en marcado XAML, se producirá el error *Failed to create a 'Windows.Foundation.Single' from the text '<NUMBER>'* (No se pudo crear un "Windows.Foundation.Single" a partir del texto).

Guarda el archivo y compila el proyecto. Durante el proceso de compilación, la herramienta `midl.exe` se ejecuta para crear un archivo de metadatos de Windows Runtime (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) que describe la clase en tiempo de ejecución. Después se ejecutará la herramienta `cppwinrt.exe` para generar archivos de código fuente y ayudarte a crear y consumir tu clase en tiempo de ejecución. Estos archivos incluyen códigos auxiliares para que puedas empezar a implementar la clase en tiempo de ejecución **BgLabelControl** que declaraste en el archivo IDL. Estos archivos de código auxiliar son `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` y `BgLabelControl.cpp`

Copia los archivos de código auxiliar `BgLabelControl.h` y `BgLabelControl.cpp` de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` a la carpeta del proyecto, que es `\BgLabelControlApp\BgLabelControlApp\`. En el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Haz clic con el botón derecho en los archivos de código auxiliar que has copiado y luego haz clic en **Incluir en el proyecto**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implementar la clase de control personalizado **BgLabelControl**
Ahora, vamos a abrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` y `BgLabelControl.cpp` e implementar nuestra clase en tiempo de ejecución. En `BgLabelControl.h`, cambia el constructor para establecer la clave de estilo predeterminado, implementa **Label** y **LabelProperty**, agrega un controlador de eventos estáticos de nombre **OnLabelChanged** que procese los cambios de valor de la propiedad de dependencia y agrega un miembro privado que almacene el campo de respaldo para **LabelProperty**.

Una vez agregados, tu `BgLabelControl.h` tendrá este aspecto.

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

En `BgLabelControl.cpp`, define los miembros estáticos de la manera siguiente.

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

En este tutorial, no se usará **OnLabelChanged**. Pero está ahí para que puedas ver cómo se registra una propiedad de dependencia con una devolución de llamada modificada por propiedades. La implementación de **OnLabelChanged** también muestra cómo obtener un tipo proyectado derivado a partir de un tipo proyectado base (en este caso, el tipo proyectado base es **DependencyObject**). Además, muestra cómo obtener un puntero al tipo que implementa el tipo proyectado. Esa segunda operación solo será de por sí posible en el proyecto que implemente el tipo proyectado (es decir, el proyecto que implemente la clase de tiempo de ejecución).

> [!NOTE]
> Si no has instalado Windows SDK versión 10.0.17763.0 (Windows 10, versión 1809) o posterior, tendrás que llamar a [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) en el controlador de eventos de cambio de la propiedad de la dependencia anterior, en lugar de a [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

## <a name="design-the-default-style-for-bglabelcontrol"></a>Diseñar el estilo predeterminado para **BgLabelControl**

En su constructor, **BgLabelControl** establece una clave de estilo predeterminado para sí mismo. Pero, ¿qué *es* un estilo predeterminado? Un control (basado en modelo) personalizado ha de tener un estilo predeterminado (que contiene una plantilla de control predeterminado) que pueda usar para representarse a sí mismo en caso de que el usuario del control no establezca un estilo y/o una plantilla. En esta sección vamos a agregar un archivo de marcado al proyecto que contiene el estilo predeterminado.

En el nodo del proyecto, crea una carpeta y asígnale el nombre "Themes". En `Themes`, agrega un nuevo elemento de tipo **Visual C++**  > **XAML** > **Vista XAML** y asígnale el nombre "Generic.xaml". Los nombres de carpeta y archivo deben ser similares a este para que el marco XAML encuentre el estilo predeterminado para un control personalizado. Elimina el contenido predeterminado de `Generic.xaml` y pégalo en el marcado siguiente.

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

En este caso, la única propiedad que el estilo predeterminado establece es la plantilla de control. La plantilla consta de un cuadrado (cuyo fondo está enlazado a la propiedad **Background** que todas las instancias del tipo [**Control**](/uwp/api/windows.ui.xaml.controls.control) de XAML tienen) y un elemento de texto (cuyo texto está enlazado a la propiedad de dependencia **BgLabelControl::Label**).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Agregar una instancia de **BgLabelControl** a la página principal de la interfaz de usuario

Abre `MainPage.xaml`, que contiene el marcado XAML de nuestra página principal de la interfaz de usuario. Inmediatamente después del elemento **Button** (dentro de la clase **StackPanel**), agrega el marcado siguiente.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Además, agrega la siguiente directiva Include a `MainPage.h` para que el tipo **MainPage** (una combinación de compilación de marcado XAML y código imperativo) tenga constancia del tipo de control personalizado **BgLabelControl**. Si quieres usar **BgLabelControl** desde otra página XAML, agrega también esta misma directiva Include al archivo de encabezado de esa página. O bien, puedes poner una sola directiva Include en el archivo de encabezado precompilado.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Ahora compila y ejecuta el proyecto. Verás que la plantilla de control predeterminado está enlazada al pincel de fondo y a la etiqueta de la instancia de **BgLabelControl** en el marcado.

Este tutorial ha mostrado un ejemplo sencillo de un control (basado en modelo) personalizado en C++/WinRT. Puedes crear tus propios controles personalizados enriquecidos y con todas las funciones de forma arbitraria. Por ejemplo, un control personalizado puede adoptar la forma de algo tan complicado como una cuadrícula de datos editable, un reproductor de vídeo o un visualizador de geometría 3D.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implementar funciones *overridable*, tales como **MeasureOverride** y **OnApplyTemplate**

Un control personalizado se deriva de la clase de tiempo de ejecución [ **{Control**](/uwp/api/windows.ui.xaml.controls.control), que a su vez se deriva de las clases base de tiempo de ejecución. Además, hay métodos reemplazables **Control**, [**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement) y [**UIElement**](/uwp/api/windows.ui.xaml.uielement) que puedes reemplazar en la clase derivada. Este es un ejemplo de código que muestra cómo hacerlo.

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

Las funciones *reemplazables* se presentan de forma diferente en las distintas proyecciones de lenguaje. En C#, por ejemplo, las funciones reemplazables suelen aparecen como funciones virtuales protegidas. En C++/WinRT, no son ni virtuales ni protegidas, pero aun así puedes reemplazarlas y proporcionar tu propia implementación, tal y como se indicó anteriormente.

## <a name="important-apis"></a>API importantes
* [Clase Control](/uwp/api/windows.ui.xaml.controls.control)
* [Clase DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)
* [Clase FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)
* [Clase UIElement](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Temas relacionados
* [Plantillas de control](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propiedades de dependencia personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties)
