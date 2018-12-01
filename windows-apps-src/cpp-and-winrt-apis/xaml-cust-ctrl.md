---
description: En este tema te guiará por los pasos de creación de un control personalizado simple con C++ / WinRT. Se puede generar la información aquí para crear tus propios controles de interfaz de usuario enriquecida y personalizables.
title: Controles (con plantilla) personalizados de XAML con C++ / WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, XAML, un control personalizado con plantilla,
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e929f217c71a90540803b180e6e79b98802f9c7a
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8347877"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>Controles (con plantilla) personalizados de XAML con C++ / WinRT

> [!IMPORTANT]
> Para los conceptos esenciales y los términos que ayuden a entender cómo consumir y crear clases en tiempo de ejecución con [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), consulta [consumir API con C++ / WinRT](consume-apis.md) y [crear API con C++ / WinRT](author-apis.md).

Una de las características más eficaces de la plataforma Universal de Windows (UWP) es la flexibilidad que proporciona la pila de la interfaz de usuario (UI) para crear controles personalizados en función del tipo de [**Control**](/uwp/api/windows.ui.xaml.controls.control) XAML. El marco de la UI de XAML proporciona características como [Propiedades de dependencia personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties) y las propiedades adjuntas y [plantillas de control](/windows/uwp/design/controls-and-patterns/control-templates), lo que hacen más fácil crear controles enriquecida y personalizables. En este tema te guiará por el proceso de creación de un control personalizado (con plantilla) con C++ / WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Crear una aplicación vacía (BgLabelControlApp)
Comienza creando un proyecto nuevo en Microsoft Visual Studio Crear un **Visual C++** > **Windows Universal** > **aplicación vacía (C++ / WinRT)** del proyecto y asígnale el nombre *BgLabelControlApp*.

Vamos a crear una nueva clase para representar un control personalizado (con plantilla). Vamos a crear y consumir la clase dentro de la misma unidad de compilación. Pero queremos poder crear una instancia de esta clase desde el marcado XAML y para ello va a ser una clase en tiempo de ejecución. Y vamos a usar C++/WinRT para crearla y consumirla.

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

La descripción de la anterior muestra el patrón que sigue al declarar una propiedad de dependencia (DP). Hay dos piezas para cada DP. En primer lugar, declara una propiedad estática de solo lectura de tipo [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty). Tiene el nombre de la *propiedad*DP. Esta propiedad estática que usarás en tu implementación. En segundo lugar, se declara una propiedad de instancia de lectura y escritura con el tipo y el nombre de tu DP.

> [!NOTE]
> Si quieres un DP con un tipo de punto flotante, que, a continuación, convierte `double` (`Double` en [MIDL 3.0](/uwp/midl-3/)). Declarar e implementar un DP de tipo `float` (`Single` en MIDL), y, a continuación, establecer un valor para esa DP en el marcado XAML, da como resultado el error *no pudo crear un 'Windows.Foundation.Single' desde el texto '<NUMBER>'*.

Guarda el archivo y compila el proyecto. Durante el proceso de compilación, la herramienta `midl.exe` se ejecutará para crear un archivo de metadatos de Windows Runtime (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) que describe la clase en tiempo de ejecución. Después se ejecutará la herramienta `cppwinrt.exe` para generar archivos de código fuente y ayudarte a crear y consumir tu clase en tiempo de ejecución. Estos archivos incluyen códigos auxiliares para que puedas empezar a implementar la clase en tiempo de ejecución de **BgLabelControl** que declaraste en tu archivo IDL. Estos archivos de código auxiliar son `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` y `BgLabelControl.cpp`

Copia los archivos de código auxiliar`BgLabelControl.h``BgLabelControl.cpp` de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` a la carpeta del proyecto, que es `\BgLabelControlApp\BgLabelControlApp\`. En el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Haz clic con el botón derecho en los archivos de código auxiliar que has copiado y haz clic en **Incluir en el proyecto**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implementa la clase de control personalizado **BgLabelControl**
Ahora, vamos a abrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` y `BgLabelControl.cpp` e implementar nuestra clase en tiempo de ejecución. En `BgLabelControl.h`, cambie el constructor para establecer la clave de estilo predeterminado, implementar **etiqueta** y **LabelProperty**, agrega un controlador de eventos estáticos denominado **OnLabelChanged** para procesar los cambios en el valor de la propiedad de dependencia y agrega un miembro privado para almacenar el campo de respaldo para **LabelProperty**.

Una vez agregados, tu `BgLabelControl.h` este aspecto.

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

En `BgLabelControl.cpp`, definir los miembros estáticos como este.

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

En este tutorial, no se puede usar **OnLabelChanged**. Pero está ahí para que puedas ver cómo registrar una propiedad de dependencia con una devolución de llamada modificado por la propiedad. La implementación de **OnLabelChanged** también muestra cómo obtener un tipo proyectado derivado de un tipo proyectado base (el tipo proyectado base es **DependencyObject**, en este caso). Y se muestra cómo obtener un puntero para el tipo que implementa el tipo proyectado. Esa segunda operación naturalmente solo serán posible en el proyecto que implementa el tipo proyectado (es decir, el proyecto que implementa la clase en tiempo de ejecución).

> [!NOTE]
> Si no has instalado Windows SDK versión 10.0.17763.0 (Windows 10, versión 1809) o una versión posterior, a continuación, debes llamar a [**winrt:: from_abi**](/uwp/cpp-ref-for-winrt/from-abi) en el controlador de eventos cambiados de propiedad de dependencia anteriormente, en lugar de [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

## <a name="design-the-default-style-for-bglabelcontrol"></a>Diseñar el estilo predeterminado para **BgLabelControl**

En su constructor, **BgLabelControl** establece una clave de estilo predeterminado por sí mismo. Pero ¿qué *es* un estilo predeterminado? Un control personalizado (con plantilla) debe tener un estilo predeterminado&mdash;que contiene una plantilla de control predeterminada&mdash;que puede usar para su representación con en caso de que el consumidor del control no establece un estilo o una plantilla. En esta sección, agregaremos un archivo de marcado al proyecto que contiene nuestra estilo predeterminado.

En el nodo del proyecto, crea una nueva carpeta y asígnale el nombre "Themes". Bajo `Themes`, agrega un nuevo elemento de tipo de **Visual C++** > **XAML** > **Vista XAML**y el nombre "Generic.xaml". Los nombres de archivos y carpetas tienen que ser así en orden para el marco XAML encontrar el estilo predeterminado de un control personalizado. Elimina el contenido predeterminado de `Generic.xaml`y pégalo en el marcado siguiente.

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

En este caso, la única propiedad que establece el estilo predeterminado es la plantilla de control. La plantilla consta de un cuadrado (cuya en segundo plano se enlaza a la propiedad **en segundo plano** que todas las instancias del tipo de [**Control**](/uwp/api/windows.ui.xaml.controls.control) XAML tienen) y un elemento de texto (cuyo texto está enlazado a la propiedad de dependencia **BgLabelControl::Label** ).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Agregar una instancia de **BgLabelControl** a la página principal de la interfaz de usuario

Abre `MainPage.xaml`, que contiene la marcación XAML de nuestra página principal de la interfaz de usuario. Inmediatamente después del elemento de **botón** (dentro de **StackPanel**), agrega el siguiente marcado.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Además, agrega el siguiente directiva #include `MainPage.h` para que el tipo de **MainPage** (una combinación de compilación de marcado XAML y código imperativo) es conocer el tipo de control personalizado **BgLabelControl** .

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Ahora compila y ejecuta el proyecto. Podrás ver que se enlace la plantilla de control predeterminada para el pincel de fondo y a la etiqueta de la instancia de **BgLabelControl** en el marcado.

En este tutorial se ha mostrado un ejemplo sencillo de un control personalizado (con plantilla) en C++ / WinRT. Puedes realizar tus propios controles personalizados arbitrariamente enriquecidos y completas. Por ejemplo, un control personalizado puede adoptar la forma de algo tan complicado como una cuadrícula de datos editable, un Reproductor de vídeo o un visualizador de geometría 3D.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implementar *reemplazables* funciones, como **MeasureOverride** y **OnApplyTemplate**

Derivar un control personalizado de la clase en tiempo de ejecución de [**Control**](/uwp/api/windows.ui.xaml.controls.control) , que aún más deriva de clases base en tiempo de ejecución. Y hay métodos reemplazables de **Control**, [**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement)y [**UIElement**](/uwp/api/windows.ui.xaml.uielement) que se puede reemplazar en la clase derivada. Este es un ejemplo de código que muestra cómo hacerlo.

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

Funciones de *Overridable* se presenten distinto en las proyecciones de lenguaje diferente. En C#, por ejemplo, funciones reemplazables normalmente aparecen como funciones virtuales protegidas. En C++ / WinRT, que estén ni virtual ni protegidos, pero podrás seguir anularlos y proporcionar tu propia implementación, como se mostró anteriormente.

## <a name="important-apis"></a>API importantes
* [Clase de control](/uwp/api/windows.ui.xaml.controls.control)
* [Clase DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)
* [Clase de FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement)
* [Clase UIElement](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Temas relacionados
* [Plantillas de control](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propiedades de dependencia personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties)
