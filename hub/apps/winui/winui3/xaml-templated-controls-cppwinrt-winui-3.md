---
description: En este artículo se recorren los pasos para la creación de un control XAML con plantilla para WinUI 3 con C++/WinRT.
title: Controles XAML con plantilla para aplicaciones para WinUI 3 con C++/WinRT
ms.date: 07/09/2020
ms.topic: article
keywords: windows 10, uwp, custom control, templated control, winui, C++/WinRT
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 9790db23160538e90e0386c34003831e5efb20ac
ms.sourcegitcommit: aabd6f40df6cc82bb8ce3a43275e4abd568c236f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061713"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-cwinrt"></a>Controles XAML con plantilla para aplicaciones para WinUI 3 con C++/WinRT

En este artículo se recorren los pasos para la creación de un control XAML con plantilla para WinUI 3 con C++/WinRT. Los controles con plantilla se heredan de **Microsoft.UI.Xaml.Controls.Control** y tienen una estructura y comportamiento visual que se pueden personalizar mediante plantillas de control XAML. En este artículo se describe el mismo escenario que en el artículo [Controles personalizados (con plantilla) de XAML con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl), pero se ha adaptado para usar WinUI 3.

Antes de seguir los pasos de este artículo, debe asegurarse de que el entorno de desarrollo está configurado para crear aplicaciones de WinUI 3. Para información de configuración, consulte [Introducción a WinUI 3 para aplicaciones de escritorio](./get-started-winui3-for-desktop.md). También deberá descargar e instalar la versión más reciente de la [Extensión de Visual Studio (VSIX) de C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) desde [Visual Studio Marketplace](https://marketplace.visualstudio.com).

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Crear una aplicación en blanco (BgLabelControlApp)

Para empezar, crea un proyecto en Microsoft Visual Studio. En el cuadro de diálogo `Create a new project`, seleccione la plantilla de proyecto **Aplicación vacía (WinUI en UWP)** y asegúrese de seleccionar la versión de lenguaje C++. Establezca el nombre del proyecto en "BgLabelControlApp" para que los nombres de archivo se alineen con el código en los ejemplos siguientes. Establezca **Versión de destino** en Windows 10, versión 1903 (compilación 18362) y **Versión mínima** en Windows 10, versión 1803 (compilación 17134). Este tutorial también funcionará para las aplicaciones de escritorio creadas con la plantilla de proyecto **Blank App, Packaged (WinUI in Desktop)** [Aplicación vacía, empaquetada (WinUI en el escritorio)], solo tiene que asegurarse de realizar todos los pasos del proyecto **BgLabelControlApp (escritorio)** .

![Plantilla de proyecto de aplicación vacía](images/WinUI-cpp-newproject-UWP.png)

## <a name="add-a-templated-control-to-your-app"></a>Adición de un control con plantilla a la aplicación

Para agregar un control con plantilla, haga clic en el menú **Proyecto** de la barra de herramientas o haga clic con el botón derecho en el proyecto en **Explorador de soluciones** y seleccione **Agregar nuevo elemento** . En **Visual C++->WinUI**, seleccione la plantilla **Control personalizado (WinUI** ). Asigne al nuevo control el nombre "BgLabelControl" y haga clic en *Agregar*. Se agregarán tres nuevos archivos al proyecto. `BgLabelControl.h` es el encabezado que contiene las declaraciones de control y `BgLabelControl.cpp` contiene el elemento C++/WinRTmimplementation del control. `BgLabelControl.idl` es el archivo de definición de interfaz que permite que se creen instantáneas del control como una clase en tiempo de ejecución.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implementación de la clase de control personalizado BgLabelControl

En los pasos siguientes, actualizará el código de los archivos `BgLabelControl.idl`, `BgLabelControl.h` y `BgLabelControl.cpp` en el directorio del proyecto para implementar la clase en tiempo de ejecución. 


Puesto que se crearán instancias de la clase del control con plantilla a partir del marcado XAML, deberá ser una clase en tiempo de ejecución. Al compilar el proyecto finalizado, el compilador MIDL (midl.exe) usará el archivo `BgLabelControl.idl` a fin de generar el archivo de metadatos de Windows Runtime (.winmd) para el control, al que harán referencia los consumidores del componente. Para obtener más información sobre la creación de clases en tiempo de ejecución, consulte [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

El control con plantilla que se va a crear expondrá una sola propiedad, que es una cadena que se utilizará como etiqueta para el control. Reemplace el contenido de `BgLabelControl.idl` por el código siguiente.

```cppwinrt
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Microsoft.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Microsoft.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

La lista anterior muestra el patrón que se sigue al declarar una propiedad de dependencia (DP). Hay dos partes en cada DP. En primer lugar, se declara una propiedad estática de solo lectura de tipo DependencyProperty. Tiene el nombre de la DP más Property. Usarás esta propiedad estática en la implementación. En segundo lugar, se declara una propiedad de instancia de lectura y escritura con el tipo y el nombre del DP. Si quiere crear una propiedad adjunta (en lugar de una DP), consulte los ejemplos de código en [Propiedades adjuntas personalizadas](/windows/uwp/xaml-platform/custom-attached-properties).

Tenga en cuenta que las clases XAML a las que se hace referencia en el código anterior se encuentran en los espacios de nombres Microsoft.UI.Xaml. Esto es lo que los distingue como controles WinUI en lugar de controles XAML de UWP, que se definen en los espacios de nombres Windows.UI.XAML.

Reemplace el contenido de BgLabelControl.h por el código siguiente.

```cppwinrt
// BgLabelControl.h
#pragma once
#include "BgLabelControl.g.h"

namespace winrt::BgLabelControlApp::implementation
{
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

        static Microsoft::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

        static void OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const&, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const&);

    private:
        static Microsoft::UI::Xaml::DependencyProperty m_labelProperty;
    };
}
namespace winrt::BgLabelControlApp::factory_implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl, implementation::BgLabelControl>
    {
    };
}
```

El código mostrado anteriormente implementa las propiedades **Label** y **LabelProperty**, agrega un controlador de eventos estáticos denominado **OnLabelChanged** para procesar los cambios en el valor de la propiedad de dependencia y agrega un miembro privado para almacenar el campo de respaldo para **LabelProperty**. De nuevo, tenga en cuenta que las clases XAML a las que se hace referencia en el archivo de encabezado se encuentran en los espacios de nombres Microsoft.UI.Xaml que pertenecen al marco de WinUI 3 en lugar de los espacios de nombres Windows.UI.Xaml utilizados por el marco de trabajo de UWP.


A continuación, reemplace el contenido de BgLabelControl.cpp por el código siguiente.

```cppwinrt
// BgLabelControl.cpp
#include "pch.h"
#include "BgLabelControl.h"
#if __has_include("BgLabelControl.g.cpp")
#include "BgLabelControl.g.cpp"

namespace winrt::BgLabelControlApp::implementation
{
    Microsoft::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
        Microsoft::UI::Xaml::DependencyProperty::Register(
            L"Label",
            winrt::xaml_typename<winrt::hstring>(),
            winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
            Microsoft::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Microsoft::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
    );

    void BgLabelControl::OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const& d, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
    {
        if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
        {
            // Call members of the projected type via theControl.

            BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
            // Call members of the implementation type via ptr.
        }
    }
}
```
En este tutorial no se utilizará la devolución de llamada **OnLabelChanged**, pero se proporciona para que pueda ver cómo se registra una propiedad de dependencia con una devolución de llamada de propiedad modificada. La implementación de **OnLabelChanged** también muestra cómo obtener un tipo proyectado derivado a partir de un tipo proyectado base (en este caso, el tipo proyectado base es DependencyObject). Además, muestra cómo obtener un puntero al tipo que implementa el tipo proyectado. Esa segunda operación solo será de por sí posible en el proyecto que implemente el tipo proyectado (es decir, el proyecto que implemente la clase de tiempo de ejecución).

La función [xaml_typename](/uwp/cpp-ref-for-winrt/xaml-typename) la proporciona el espacio de nombres Windows.UI.Xaml.Interop que no se incluye de forma predeterminada en la plantilla de proyecto WinUI 3. Agregue una línea al archivo de encabezado precompilado para el proyecto `pch.h`, para incluir el archivo de encabezado asociado a este espacio de nombres.

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Xaml.Interop.h>
...
```



## <a name="define-the-default-style-for-bglabelcontrol"></a>Diseño del estilo predeterminado para BgLabelControl

En su constructor, **BgLabelControl** establece una clave de estilo predeterminado para sí mismo. Un control con plantilla ha de tener un estilo predeterminado (que contenga una plantilla de control predeterminado) que pueda usar para representarse a sí mismo en caso de que el usuario del control no establezca un estilo y/o una plantilla. En esta sección vamos a agregar un archivo de marcado al proyecto que contiene el estilo predeterminado.

Asegúrate de que **Mostrar todos los archivos** esté activado (en el **Explorador de soluciones**). En el nodo del proyecto, crea una carpeta (no un filtro, sino una carpeta) y asígnale el nombre "Themes". En `Themes`, agregue un nuevo elemento de tipo **Visual C++ > WinUI > Diccionario de recursos (WinUI)** y asígnele el nombre "Generic.xaml". Los nombres de carpeta y archivo deben ser similares a este para que el marco XAML encuentre el estilo predeterminado para un control con plantilla. Elimine el contenido predeterminado de Generic.xaml y péguelo en el siguiente marcado.

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

En este caso, la única propiedad que el estilo predeterminado establece es la plantilla de control. La plantilla consta de un cuadrado (cuyo fondo está enlazado a la propiedad **Background** que todas las instancias del tipo **Control** de XAML tienen) y un elemento de texto (cuyo texto está enlazado a la propiedad de dependencia **BgLabelControl::Label**).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Adición de una instancia de BgLabelControl a la página principal de la interfaz de usuario

Abre `MainPage.xaml`, que contiene el marcado XAML de nuestra página principal de la interfaz de usuario. Inmediatamente después del elemento **Button** (dentro de la clase **StackPanel**), agrega el marcado siguiente.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Además, agregue la siguiente directiva de inclusión a `MainPage.h` para que el tipo **MainPage** (una combinación de compilación de marcado XAML y código imperativo) tenga constancia del tipo de control con plantilla **BgLabelControl**. Si quieres usar **BgLabelControl** desde otra página XAML, agrega también esta misma directiva Include al archivo de encabezado de esa página. O bien, puedes poner una sola directiva Include en el archivo de encabezado precompilado.

```cppwinrt
//MainPage.h
...
#include "BgLabelControl.h"
...
```

Ahora compila y ejecuta el proyecto. Verás que la plantilla de control predeterminado está enlazada al pincel de fondo y a la etiqueta de la instancia de **BgLabelControl** en el marcado.

![Resultado del control con plantilla](images/winui-templated-control-result.png)

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implementación de funciones reemplazables, como **MeasureOverride** y **OnApplyTemplate**

Un control con plantilla se deriva de la clase de tiempo de ejecución **Control**, que a su vez se deriva de las clases base de tiempo de ejecución. Además, hay métodos reemplazables de **Control**, **FrameworkElement** y **UIElement** que puede reemplazar en la clase derivada. Este es un ejemplo de código que muestra cómo hacerlo.

```cppwinrt
// Control overrides.
void OnPointerPressed(Microsoft::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

// FrameworkElement overrides.
Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
void OnApplyTemplate() const { ... };

// UIElement overrides.
Microsoft::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
```

Las funciones *reemplazables* se presentan de forma diferente en las distintas proyecciones de lenguaje. En C#, por ejemplo, las funciones reemplazables suelen aparecen como funciones virtuales protegidas. En C++/WinRT, no son ni virtuales ni protegidas, pero aun así puedes reemplazarlas y proporcionar tu propia implementación, tal y como se indicó anteriormente.



## <a name="generating-the-control-source-files-without-using-a-template"></a>Generación de los archivos de origen del control sin usar ninguna plantilla

En esta sección se muestra cómo puede generar los archivos de origen necesarios para crear el control personalizado sin usar la plantilla de elemento de **control personalizado**. 

En primer lugar, agregue un nuevo elemento de archivo Midl (.idl) al proyecto. En el menú **Proyecto**, seleccione **Agregar un nuevo elemento...** y escriba "MIDL" en el cuadro de diálogo para buscar el elemento archivo .idl. Asigne al nuevo archivo el nombre `BgLabelControl.idl` para que el nombre sea coherente con los pasos de este artículo. Elimine el contenido predeterminado de `BgLabelControl.idl` y péguelo en la declaración de clase en tiempo de ejecución que se muestra en los pasos anteriores.


Después de guardar el nuevo archivo .idl, el siguiente paso es generar el archivo de metadatos de Windows Runtime (.winmd) y los códigos auxiliares para los archivos de implementación .cpp y .h que se usarán para implementar el control con plantilla. Genere estos archivos compilando la solución, lo que hará que el compilador MIDL (midl.exe) compile el archivo .idl que creó. Tenga en cuenta que, la solución no se compilará correctamente y Visual Studio mostrará errores de compilación en la ventana de salida, pero se generarán los archivos necesarios.

Copie los archivos de código auxiliar BgLabelControl.h y BgLabelControl.cpp de \BgLabelControlApp\BgLabelControlApp\Generated Files\sources\ en la carpeta del proyecto. En el **Explorador de soluciones**, asegúrese de que Mostrar todos los archivos esté activado. Haz clic con el botón derecho en los archivos de código auxiliar que has copiado y luego haz clic en **Incluir en el proyecto**.

El compilador coloca una línea static_assert en la parte superior de BgLabelControl.h y BgLabelControl.cpp para evitar que los archivos generados se compilen. Al implementar el control, debe quitar estas líneas de los archivos que ha colocado en el directorio del proyecto. En este tutorial, simplemente puede sobrescribir todo el contenido de los archivos con el código que se proporciona anteriormente.
