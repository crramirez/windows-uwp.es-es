---
Description: Aprende a habilitar la navegación de punto a punto entre dos páginas básicas en una aplicación de Windows.
title: Navegación de punto a punto entre dos páginas
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
ms.date: 07/13/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 50211600931fc67c43aa577fabe23f1277a0e897
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/12/2020
ms.locfileid: "83233823"
---
# <a name="implement-navigation-between-two-pages"></a>Implementación del desplazamiento entre dos páginas

Aprende a usar un marco y páginas para habilitar la navegación punto a punto básica en una aplicación. 

> **API importantes**: Clase [**Windows.UI.Xaml.Controls.Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame), Clase [**Windows.UI.Xaml.Controls.Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page), espacio de nombres [**Windows.UI.Xaml.Navigation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation)

![navegación punto a punto](images/peertopeer.png)

## <a name="1-create-a-blank-app"></a>1. Creación de una aplicación vacía

1.  En el menú de Microsoft Visual Studio, elige **Archivo** > **Nuevo proyecto**.
2.  En el panel izquierdo del cuadro de diálogo **Nuevo proyecto**, elige el nodo **Visual C#**  > **Windows** > **Universal** o **Visual C++**  > **Windows** > **Universal**.
3.  En el panel central, elige **Aplicación vacía**.
4.  En el cuadro **Nombre**, escribe **NavApp1** y, después, elige el botón **Aceptar**.
    La solución se crea y los archivos del proyecto aparecen en el **Explorador de soluciones**.
5.  Para ejecutar el programa, elige **Depurar** > **Iniciar depuración** en el menú o presiona F5.
    Aparecerá una página vacía.
6.  Para detener la depuración y volver a Visual Studio, sal de la aplicación o haz clic en la opción **Detener depuración** del menú.

## <a name="2-add-basic-pages"></a>2. Agregar páginas básicas

A continuación, agrega dos páginas al proyecto.

1.  En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de proyecto **BlankApp** para abrir el menú contextual.
2.  Elige **Agregar** > **Nuevo elemento** en el menú contextual.
3.  En el cuadro de diálogo **Agregar nuevo elemento**, elige **Página en blanco** en el panel central.
4.  En el cuadro **Nombre**, escribe **Page1** (o **Page2**) y presiona el botón **Agregar**.
5. Repite los pasos 1 a 4 para agregar la segunda página.

Ahora estos archivos deberían aparecer como parte del proyecto NavApp1.

<table>
<thead>
<tr class="header">
<th align="left">C#</th>
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cs</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cs</li>
</ul></td>
<td style="vertical-align:top;"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cpp</li>
<li>Page1.xaml.h</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cpp</li>
<li>Page2.xaml.h

</li>
</ul></td>
</tr>
</tbody>
</table>

En Page1.xaml, agrega el siguiente contenido:

-   Un elemento [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) llamado `pageTitle` como elemento secundario de la raíz [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid). Cambia la propiedad [**Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) a `Page 1`.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   Un elemento [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) como elemento secundario de la raíz [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) y después el elemento `pageTitle` [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

En el archivo de código subyacente Page1.xaml, agrega el siguiente código para controlar el evento `Click` del objeto [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) que has agregado para ir a Page2.xaml.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>());
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

En Page2.xaml, agrega el siguiente contenido:

-   Un elemento [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) llamado `pageTitle` como elemento secundario de la raíz [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid). Cambia el valor de la propiedad [**Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text) a `Page 2`.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   Un elemento [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) como elemento secundario de la raíz [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) y después el elemento `pageTitle` [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock).
```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

En el archivo de código subyacente Page2.xaml, agrega el siguiente código para controlar el evento `Click` del objeto [**HyperlinkButton**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) para ir a Page1.xaml.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```

```cppwinrt
void Page2::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page1>());
}
```

```cpp
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

> [!NOTE]
> En cuanto a los proyectos de C++, debes agregar una directiva `#include` en el archivo de encabezado de cada página que haga referencia a otra página. Para el ejemplo de navegación entre páginas que se presenta aquí, el archivo page1.xaml.h contiene `#include "Page2.xaml.h"` y, a su vez, el archivo page2.xaml.h contiene `#include "Page1.xaml.h"`.

Ahora que hemos preparado las páginas, necesitamos que Page1.xaml muestre el momento en el que se inicia la aplicación.

Abre el archivo de código subyacente App.xaml y cambia el controlador `OnLaunched`.

En este apartado, especificaremos `Page1` en la llamada a [**Frame.Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) en lugar de `MainPage`.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
 
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();
        rootFrame.NavigationFailed += OnNavigationFailed;
 
        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }
 
        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }
 
    if (rootFrame.Content == null)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame.Navigate(typeof(Page1), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

```cppwinrt
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = Frame();

        rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
        {
            // Restore the saved session state only when appropriate, scheduling the
            // final launch steps after the restore is complete
        }

        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Place the frame in the current Window
            Window::Current().Content(rootFrame);
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
    else
    {
        if (e.PrelaunchActivated() == false)
        {
            if (rootFrame.Content() == nullptr)
            {
                // When the navigation stack isn't restored navigate to the first page,
                // configuring the new page by passing required information as a navigation
                // parameter
                rootFrame.Navigate(xaml_typename<NavApp1::Page1>(), box_value(e.Arguments()));
            }
            // Ensure the current window is active
            Window::Current().Activate();
        }
    }
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ e)
{
    auto rootFrame = dynamic_cast<Frame^>(Window::Current->Content);

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // Create a Frame to act as the navigation context and associate it with
        // a SuspensionManager key
        rootFrame = ref new Frame();

        rootFrame->NavigationFailed += 
            ref new Windows::UI::Xaml::Navigation::NavigationFailedEventHandler(
                this, &App::OnNavigationFailed);

        if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
        {
            // TODO: Load state from previously suspended application
        }
        
        // Place the frame in the current Window
        Window::Current->Content = rootFrame;
    }

    if (rootFrame->Content == nullptr)
    {
        // When the navigation stack isn't restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
    }

    // Ensure the current window is active
    Window::Current->Activate();
}
```

> [!NOTE]
> Este código usa el valor devuelto del método [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) para generar una excepción de aplicación si se produce un error en la navegación al marco de la ventana inicial de la aplicación. Cuando **Navigate** devuelve **true**, se produce la navegación.

Ahora, compila y ejecuta la aplicación. Haz clic en el vínculo que dice "Haz clic para ir a la página 2". La segunda página que muestra "Página 2" en la parte superior, se debería cargar y mostrar en el marco.

### <a name="about-the-frame-and-page-classes"></a>Acerca de las clases Frame y Page

Antes de agregar más funciones a la aplicación, vamos a ver de qué manera las páginas que hemos agregado permiten desplazarse por la aplicación.

En primer lugar, se crea una clase [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) llamado `rootFrame` para la aplicación en el método `App.OnLaunched` del archivo de código subyacente App.xaml. La clase **Frame** admite diversos métodos de desplazamiento, como [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate), [**GoBack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback) y [**GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward), y propiedades como [**BackStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstack), [**ForwardStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.forwardstack) y [**BackStackDepth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstackdepth).
 
El método [**Navigate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) se usa para mostrar contenido en la clase **Frame**. De manera predeterminada, este método carga MainPage.xaml. En nuestro ejemplo, `Page1` se pasa al método **Navigate**, por lo que dicho método carga `Page1` en **Frame**. 

El elemento `Page1` es una subclase de la clase [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page). La clase **Page** tiene la propiedad de solo lectura **Frame**, que a su vez obtiene la clase **Frame**que contiene **Page**. Cuando el controlador de eventos **Click** de **HyperlinkButton** en `Page1` llama a `this.Frame.Navigate(typeof(Page2))`, la clase **Frame** muestra el contenido de Page2.xaml.

Por último, cada vez que se carga una página en el marco, se agrega como una clase [**PageStackEntry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation.PageStackEntry) a la propiedad [**BackStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.backstack) o [**ForwardStack**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.forwardstack) de [**Frame**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.frame), lo que permite [el historial y la navegación hacia atrás](navigation-history-and-backwards-navigation.md).

## <a name="3-pass-information-between-pages"></a>3. Pasar información entre páginas

Nuestra aplicación ya navega entre dos páginas, pero aún no hace nada interesante. A menudo, cuando una aplicación tiene varias páginas, las páginas necesitan compartir información. Pasemos parte de la información de la primera página a la segunda.

En Page1.xaml, reemplaza la clase **HyperlinkButton** que agregaste antes por el siguiente [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel).

Una vez hecho esto, agregamos una etiqueta [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) y un valor de `name` [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) para escribir una cadena de texto.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

En el controlador de eventos `HyperlinkButton_Click` del archivo de código subyacente Page1.xaml, agrega un parámetro que haga referencia a la `Text` propiedad de `name` **TextBox**al método `Navigate`.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```

```cppwinrt
void Page1::HyperlinkButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args)
{
    Frame().Navigate(winrt::xaml_typename<NavApp1::Page2>(), winrt::box_value(name().Text()));
}
```

```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

En el archivo Page2.xaml, reemplaza el elemento **HyperlinkButton**que agregaste antes por  **StackPanel**.

Aquí agregaremos un elemento [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) para mostrar la cadena de texto que se pasa desde Page1.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Name="greeting"/>
    <HyperlinkButton Content="Click to go to page 1" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

En el archivo de código subyacente Page2.xaml, agrega lo siguiente para reemplazar el método `OnNavigatedTo`:

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string && !string.IsNullOrWhiteSpace((string)e.Parameter))
    {
        greeting.Text = $"Hi, {e.Parameter.ToString()}";
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```

```cppwinrt
void Page2::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto propertyValue{ e.Parameter().as<Windows::Foundation::IPropertyValue>() };
    if (propertyValue.Type() == Windows::Foundation::PropertyType::String)
    {
        greeting().Text(L"Hi, " + winrt::unbox_value<winrt::hstring>(e.Parameter()));
    }
    else
    {
        greeting().Text(L"Hi!");
    }
    __super::OnNavigatedTo(e);
}
```

```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi, " + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

Ejecuta la aplicación, escribe tu nombre en el cuadro de texto y haz clic en el vínculo que dice **Haz clic para ir a la página 2**. 

Cuando el evento **Click** de **HyperlinkButton** de `Page1` llama a `this.Frame.Navigate(typeof(Page2), name.Text)`, la propiedad `name.Text` se pasa a `Page2` y se usa el valor de los datos del evento para el mensaje que se muestra en la página.

## <a name="4-cache-a-page"></a>4. Almacenar una página en caché

El contenido y el estado de la página no se almacenan en caché de manera predeterminada, por lo que si quieres almacenar información en la caché, debes habilitar esta opción en todas las páginas de la aplicación.

En nuestro ejemplo de punto a punto básico, no hay ningún botón Atrás (demostramos el desplazamiento hacia atrás en [Navegación hacia atrás](navigation-history-and-backwards-navigation.md)), pero si hiciste clic en un botón Atrás en `Page2`, el elemento **TextBox** (y cualquier otro campo) de `Page1` se establecerán en su estado predeterminado. Una manera de solucionar este problema es usar la propiedad [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) para especificar que una página se agregue a la memoria caché de la página del marco. 

En el constructor de `Page1`, puedes establecer **NavigationCacheMode** en **Enabled** para conservar todos los valores de contenido y estado de la página hasta que se supere la memoria caché de la página correspondiente al marco. Establece [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode) en [**Required**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Navigation.NavigationCacheMode) si quieres ignorar los límites de [**CacheSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.cachesize), que especifican el número de páginas en el historial de navegación que pueden almacenarse en la caché para el marco. Sin embargo, no olvides que los límites de tamaño de la caché pueden ser esenciales en función de los límites de memoria de un dispositivo.

```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

```cppwinrt
Page1::Page1()
{
    InitializeComponent();
    NavigationCacheMode(Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled);
}
```

```cpp
Page1::Page1()
{
    this->InitializeComponent();
    this->NavigationCacheMode = Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled;
}
```

## <a name="related-articles"></a>Artículos relacionados
* [Conceptos básicos de diseño de navegación para aplicaciones de Windows](https://docs.microsoft.com/windows/uwp/layout/navigation-basics)
* [Pivot](../controls-and-patterns/pivot.md)
* [Vista de navegación](../controls-and-patterns/navigationview.md)
