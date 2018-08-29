---
author: stevewhims
description: Este tema le guía por los pasos para crear un control personalizado simple utilizando C + + / WinRT. Puede basarse en la información aquí para crear sus propios controles de interfaz de usuario personalizables y variado.
title: Controles personalizados de (plantillas) XAML con C + + / WinRT
ms.author: stwhi
ms.date: 08/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10 uwp, estándar, c ++, cpp, winrt, proyección, XAML, control con plantilla, personalizado
ms.localizationpriority: medium
ms.openlocfilehash: c108175c66d27b2cdbd910a0f7653ca1befb68e9
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2917314"
---
# <a name="xaml-custom-templated-controls-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Controles personalizados de (plantillas) XAML con [C + + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

Una de las características más eficaces de la plataforma de Windows Universal (UWP) es la flexibilidad que proporciona la pila de la interfaz de usuario (IU) para crear controles personalizados basados en el tipo de [Control](/uwp/api/windows.ui.xaml.controls.control) XAML. El marco de la UI XAML proporciona características como [Propiedades de dependencia personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties) y propiedades adjuntas y [plantillas de control](/windows/uwp/design/controls-and-patterns/control-templates), lo que facilita crear controles variado y personalizables. Este tema le guiará por los pasos para crear un control personalizado (plantilla) con C + + / WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Crear una aplicación en blanco (BgLabelControlApp)
Comienza creando un proyecto nuevo en Microsoft Visual Studio Crear un **App en blanco de Visual C++ (C + + / WinRT)** del proyecto y asígnele el nombre *BgLabelControlApp*.

Vamos a crear una nueva clase para representar un control personalizado (plantilla). Vamos a crear y consumir la clase dentro de la misma unidad de compilación. Pero queremos ser capaces de crear una instancia de esta clase del marcado XAML y para que la razón de que va a ser una clase en tiempo de ejecución. Y vamos a usar C++/WinRT para crearla y consumirla.

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

El ejemplo anterior muestra el patrón que siguen al declarar una propiedad de dependencia (DP). Hay dos piezas en cada DP. En primer lugar, declare una propiedad estática de sólo lectura de tipo [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty). Tiene el nombre de la *propiedad*además de DP. Utilizará esta propiedad estática en la implementación. En segundo lugar, se declara una propiedad de instancia de lectura y escritura con el tipo y el nombre de su DP.

> [!NOTE]
> Si desea un DP con un tipo de punto flotante, asegúrese de `double` (`Double` en [MIDL 3.0](/uwp/midl-3/)). Declarar e implementar un DP de tipo `float` (`Single` en MIDL), y, a continuación, establecer un valor para ese DP en el marcado XAML, se produce el error *Error al crear un 'Windows.Foundation.Single' desde el texto '<NUMBER>'*.

Guarda el archivo y compila el proyecto. Durante el proceso de compilación, la herramienta `midl.exe` se ejecutará para crear un archivo de metadatos de Windows Runtime (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) que describe la clase en tiempo de ejecución. Después se ejecutará la herramienta `cppwinrt.exe` para generar archivos de código fuente y ayudarte a crear y consumir tu clase en tiempo de ejecución. Estos archivos incluyen códigos auxiliares para comenzar la implementación de la clase en tiempo de ejecución de **BgLabelControl** que declara en el archivo IDL. Estos archivos de código auxiliar son `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` y `BgLabelControl.cpp`

Copia los archivos de código auxiliar`BgLabelControl.h``BgLabelControl.cpp` de `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` a la carpeta del proyecto, que es `\BgLabelControlApp\BgLabelControlApp\`. En el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Haz clic con el botón derecho en los archivos de código auxiliar que has copiado y haz clic en **Incluir en el proyecto**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implementar la clase del control personalizado **BgLabelControl**
Ahora, vamos a abrir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` y `BgLabelControl.cpp` e implementar nuestra clase en tiempo de ejecución. En `BgLabelControl.h`, cambie el constructor para establecer la clave de estilo predeterminada, implementar la **etiqueta** y **LabelProperty**, agregar un controlador de evento estático denominado **OnLabelChanged** para cambiar el valor de la propiedad de dependencia, y agregar un miembro privado para almacenar el campo de respaldo para **LabelProperty**.

En este tutorial, no se utilizarán **OnLabelChanged**. Pero está allí para que pueda ver cómo registrar una propiedad de dependencia con una devolución de llamada de cambio de propiedad.

Después de agregarlos, la `BgLabelControl.h` tiene este aspecto.

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

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

En `BgLabelControl.cpp`, defina los miembros estáticos como este.

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

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e) {}
...
```

## <a name="design-the-default-style-for-bglabelcontrol"></a>El estilo predeterminado de diseño de **BgLabelControl**

En su constructor, **BgLabelControl** establece una clave de estilo predeterminada para sí mismo. Pero ¿qué *es* un estilo predeterminado? Debe tener un estilo predeterminado de un control personalizado (plantilla)&mdash;que contiene una plantilla de control predeterminada&mdash;que puede utilizar para representar a sí mismo con en caso de que el consumidor del control no establece un estilo o plantilla. En esta sección, agregaremos un archivo de marcado para el proyecto que contiene nuestro estilo predeterminado.

Bajo el nodo del proyecto, cree una carpeta nueva y asígnele el nombre "Themes". En `Themes`, agregar un nuevo elemento de tipo **Visual C++** > **XAML** > la**Vista XAML**y asígnele el nombre "Generic.xaml". Los nombres de carpeta y el archivo tienen que ser así para el marco XAML buscar el estilo predeterminado de un control personalizado. Elimine el contenido predeterminado de `Generic.xaml`y pegar en el marcado siguiente.

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

En este caso, la única propiedad que se establece el estilo predeterminado es la plantilla de control. La plantilla está formada por un cuadrado (cuyo fondo se enlaza a la propiedad **Background** que tienen todas las instancias del tipo de [Control](/uwp/api/windows.ui.xaml.controls.control) XAML) y un elemento de texto (cuyo texto está enlazado a la propiedad de dependencia de **BgLabelControl::Label** ).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Agregue una instancia de **BgLabelControl** a la página principal de la interfaz de usuario

Abre `MainPage.xaml`, que contiene la marcación XAML de nuestra página principal de la interfaz de usuario. Inmediatamente después del elemento de **botón** (dentro de **StackPanel**), agregue el marcado siguiente.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Asimismo, agregue la siguiente directiva #include `MainPage.h` para que el tipo de **MainPage** (una combinación de compilación de marcado XAML y código imperativo) es conocer el tipo de control personalizado de **BgLabelControl** .

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Ahora compila y ejecuta el proyecto. Verá que la plantilla de control predeterminada es vinculante para el pincel del fondo y la etiqueta de la instancia de **BgLabelControl** en el marcado.

Este tutorial ha mostrado un ejemplo sencillo de un control personalizado (plantilla) en C + + / WinRT. Puede hacer sus propios controles personalizados arbitrariamente enriquecido y completo. Por ejemplo, un control personalizado puede adoptar la forma de algo tan complicado como una cuadrícula de datos modificable, un Reproductor de vídeo o un visualizador de geometría 3D.

## <a name="important-apis"></a>API importantes
* [Control](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty](/uwp/api/windows.ui.xaml.dependencyproperty)

## <a name="related-topics"></a>Temas relacionados
* [Plantillas de control](/windows/uwp/design/controls-and-patterns/control-templates)
* [Propiedades de dependencia personalizadas](/windows/uwp/xaml-platform/custom-dependency-properties)
