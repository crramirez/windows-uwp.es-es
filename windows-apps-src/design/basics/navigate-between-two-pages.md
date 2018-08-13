---
author: serenaz
Description: Learn how to enable peer-to-peer navigation between two basic pages in an Universal Windows Platform (UWP) app.
title: Navegación de punto a punto entre dos páginas
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a4081535cf8b9889a1f6864637c7fdefc982e9a5
ms.sourcegitcommit: b8c77ac8e40a27cf762328d730c121c28de5fbc4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2018
ms.locfileid: "1672742"
---
# <a name="implement-navigation-between-two-pages"></a>Implementar la navegación entre dos páginas

Obtén información sobre cómo usar un marco y páginas para habilitar la navegación punto a punto básica en tu aplicación. 

> **API importantes**: Clase [**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682), clase [**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503), espacio de nombres [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300).

![navegación punto a punto](images/peertopeer.png)

## <a name="1-create-a-blank-app"></a>1. Crear una aplicación vacía

1.  En el menú de Microsoft Visual Studio, elige **Archivo** > **Nuevo proyecto**.
2.  En el panel izquierdo del cuadro de diálogo **Nuevo proyecto**, elige el nodo **Visual C#** > **Windows** > **Universal** o **Visual C++** > **Windows** > **Universal**.
3.  En el panel central, elige **Aplicación vacía**.
4.  En el cuadro **Nombre**, escribe **NavApp1** y después elige el botón **Aceptar**.
    La solución se crea y los archivos del proyecto aparecen en el **Explorador de soluciones**.
5.  Para ejecutar el programa, elige **Depurar** > **Iniciar depuración** en el menú o presiona F5.
    Aparecerá una página en blanco.
6.  Para detener la depuración y volver a Visual Studio, sal de la aplicación o haz clic en **Detener depuración** desde el menú.

## <a name="2-add-basic-pages"></a>2. Agregar páginas básicas

A continuación, agrega dos páginas al proyecto.

1.  En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo del proyecto **BlankApp** para abrir el menú contextual.
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

-   Un elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) llamado `pageTitle` como elemento secundario de la raíz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Cambia la propiedad [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) a `Page 1`.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 1" />
```

-   Un elemento [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) como elemento secundario de la raíz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) y después el elemento `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652).
```xaml
<HyperlinkButton Content="Click to go to page 2"
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

En el archivo de código subyacente Page1.xaml, agrega el siguiente código para controlar el evento `Click` del objeto [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que has agregado para navegar a Page2.xaml.

```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

En Page2.xaml, agrega el siguiente contenido:

-   Un elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) llamado `pageTitle` como elemento secundario de la raíz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Cambia el valor de la propiedad [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) a `Page 2`.
```xaml
<TextBlock x:Name="pageTitle" Text="Page 2" />
```

-   Un elemento [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) como elemento secundario de la raíz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) y después el elemento `pageTitle` [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652).
```xaml
<HyperlinkButton Content="Click to go to page 1" 
                 Click="HyperlinkButton_Click"
                 HorizontalAlignment="Center"/>
```

En el archivo de código subyacente Page2.xaml, agrega el siguiente código para controlar el evento `Click` del objeto [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) para navegar a Page1.xaml.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
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

En este apartado, especificaremos `Page1` en la llamada a [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) en lugar de `MainPage`.


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

**Nota**: Este código usa el valor devuelto del método [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) para generar una excepción de la aplicación si se produce un error en la navegación al marco de ventana inicial de la aplicación. Cuando **Navigate** devuelve **true**, se produce la navegación.

Ahora, compila y ejecuta la aplicación. Haz clic en el vínculo que dice "Haz clic para ir a la página 2". La segunda página que muestra "Página 2" en la parte superior, se debería cargar y mostrar en el marco.

### <a name="about-the-frame-and-page-classes"></a>Acerca de las clases Frame y Page

Antes de agregar más funcionalidades a la aplicación, veamos de qué manera las páginas que hemos añadido ya ofrecen de navegación en la aplicación.

Primero se crea un [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) llamado `rootFrame` para la aplicación en el método `App.OnLaunched` del archivo de código subyacente App.xaml. La clase **Frame** admite diversos métodos de navegación, tales como [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694), [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) y [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693), y propiedades como [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543), [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) y [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995).

 
El método [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) se usa para mostrar contenido en la clase **Frame**. De manera predeterminada, este método carga MainPage.xaml. En nuestro ejemplo, `Page1` se pasa al método **Navigate**, por lo que dicho método carga `Page1` en **Frame**. 

`Page1` es una subclase de la clase [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503). La clase **Page** tiene la propiedad de solo lectura **Frame** que a su vez obtiene la clase **Frame** que contiene **Page**. Cuando el controlador de eventos **Click** de **HyperlinkButton** en `Page1` llama a `this.Frame.Navigate(typeof(Page2))`, la clase **Frame** de muestra el contenido de Page2.xaml.

Por último, siempre que se cargue una página en el marco, dicha página se agregará como una clase [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572) a la propiedad [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543) o [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) de [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504), lo que permite [el historial y la navegación hacia atrás](navigation-history-and-backwards-navigation.md).

## <a name="3-pass-information-between-pages"></a>3. Pasar información entre páginas

Nuestra aplicación ya navega entre dos páginas, pero aún no hace nada interesante. A menudo, cuando una aplicación tiene varias páginas, las páginas necesitan compartir información. Pasemos parte de la información de la primera página a la segunda.

En Page1.xaml, reemplaza el elemento **HyperlinkButton** que agregaste antes por el siguiente [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635).

Una vez hecho esto, agregamos una etiqueta [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) y una clase [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) `name` para escribir una cadena de texto.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton Content="Click to go to page 2" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

En el controlador de eventos `HyperlinkButton_Click` del archivo de código subyacente Page1.xaml, agrega un parámetro que haga referencia a la propiedad `Text` del elemento `name` **TextBox** para el método `Navigate`.

> [!div class="tabbedCodeSnippets"]
```csharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```
```cpp
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

En Page2.xaml, reemplaza la clase **HyperlinkButton** que agregaste antes por **StackPanel**.

Aquí agregaremos un elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) para mostrar la cadena de texto que se pasa desde Page1.

```xaml
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Name="greeting"/>
    <HyperlinkButton Content="Click to go to page 1" 
                     Click="HyperlinkButton_Click"
                     HorizontalAlignment="Center"/>
</StackPanel>
```

En el archivo de código subyacente Page2.xaml, añade lo siguiente para reemplazar el método `OnNavigatedTo`:

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
```cpp
void Page2::OnNavigatedTo(NavigationEventArgs^ e)
{
    if (dynamic_cast<Platform::String^>(e->Parameter) != nullptr)
    {
        greeting->Text = "Hi," + e->Parameter->ToString();
    }
    else
    {
        greeting->Text = "Hi!";
    }
    ::Windows::UI::Xaml::Controls::Page::OnNavigatedTo(e);
}
```

Ejecuta la aplicación, escribe tu nombre en el cuadro de texto y haz clic en el vínculo que dice **Haz clic para ir a la página 2**. 

Cuando el evento **Click** del elemento **HyperlinkButton** de `Page1` llama a `this.Frame.Navigate(typeof(Page2), name.Text)`, la propiedad `name.Text` se pasa a `Page2` y se usa el valor de los datos del evento para el mensaje que se muestra en la página.

## <a name="4-cache-a-page"></a>4. Almacenar una página en caché

El contenido y el estado de la página no se almacenan en caché de forma predeterminada, por lo que si quieres almacenar información en caché, debes habilitar esta opción en cada página de la aplicación.

En nuestro ejemplo básico de punto a punto, no hay ningún botón Atrás (demostraremos la navegación hacia atrás en [Navegación hacia atrás](navigation-history-and-backwards-navigation.md)), pero si hiciste clic en un botón Atrás en `Page2`, el elemento **TextBox** (y cualquier otro campo) de `Page1` se establecerá en su estado predeterminado. Una manera de solucionar este problema es usar la propiedad [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) para especificar que una página se agregue a la memoria caché de la página del marco. 

En el constructor de `Page1`, puedes establecer **NavigationCacheMode** en **Enabled** para conservar todos los valores de contenido y estado de la página hasta que se supere la memoria caché de la página correspondiente al marco. Establece [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) a [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284) si quieres ignorar los límites de [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683), que especifican el número de páginas en el historial de navegación que pueden almacenarse en caché para el marco. Sin embargo, no olvides que los límites de tamaño de caché pueden ser esenciales en función de los límites de memoria de un dispositivo.

```csharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
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
* [Conceptos básicos del diseño de navegación para las aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/dn958438)
* [Directrices para pestañas y tablas dinámicas](https://msdn.microsoft.com/library/windows/apps/dn997788)
* [Directrices sobre paneles de navegación](https://msdn.microsoft.com/library/windows/apps/dn997766)
 

 



