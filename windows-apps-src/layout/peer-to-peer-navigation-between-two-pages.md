---
author: Jwmsft
Description: "Obtén información sobre cómo navegar en una aplicación básica para la Plataforma universal de Windows (UWP) de punto a punto entre dos páginas."
title: "Navegación de punto a punto entre dos páginas"
ms.assetid: 0A364C8B-715F-4407-9426-92267E8FB525
label: Peer-to-peer navigation between two pages
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a4e9a90edd2aae9d2fd5d7bead948422d43dad59
ms.openlocfilehash: ec1c0339017fb60ed37f45dfa6f809a5eba6fbb1

---

# <span id="dev_navigation.peer-to-peer_navigation_between_two_pages"></span>Navegación de punto a punto entre dos páginas

Obtén información sobre cómo navegar en una aplicación básica para la Plataforma universal de Windows (UWP) de punto a punto entre dos páginas.

![ejemplo de navegación de punto a punto entre dos páginas](images/nav-peertopeer-2page.png)


**API importantes**

-   [**Windows.UI.Xaml.Controls.Frame**](https://msdn.microsoft.com/library/windows/apps/br242682)
-   [**Windows.UI.Xaml.Controls.Page**](https://msdn.microsoft.com/library/windows/apps/br227503)
-   [**Windows.UI.Xaml.Navigation**](https://msdn.microsoft.com/library/windows/apps/br243300)


## <span id="Create_the_blank_app"></span><span id="create_the_blank_app"></span><span id="CREATE_THE_BLANK_APP"></span>Crear una aplicación vacía


1.  En el menú de Microsoft Visual Studio, elige **Archivo &gt; Nuevo proyecto**.
2.  En el panel izquierdo del cuadro de diálogo **Nuevo proyecto**, elige el nodo **Visual C# -&gt; Windows -&gt; Universal** o **Visual C++ -&gt; Windows -&gt; Universal**.
3.  En el panel central, elige **Aplicación vacía**.
4.  En el cuadro **Nombre**, escribe **NavApp1** y, después, elige el botón **Aceptar**.

    La solución se crea y los archivos del proyecto aparecen en el **Explorador de soluciones**.

    **Importante** Cuando ejecutes Visual Studio por primera vez, te pedirá que obtengas una licencia de desarrollador. Para obtener más información, consulta [Habilitar el dispositivo para el desarrollo](https://msdn.microsoft.com/library/windows/apps/dn706236).

     

5.  Para ejecutar el programa, elige **Depurar**&gt;**Iniciar depuración** en el menú o presiona F5.

    Aparecerá una página vacía.

6.  Presiona Mayus + F5 para detener la depuración y volver a Visual Studio.

## <span id="Add_basic_pages"></span><span id="add_basic_pages"></span><span id="ADD_BASIC_PAGES"></span>Agregar páginas básicas


A continuación, agrega dos páginas de contenido al proyecto.

Realiza los siguientes pasos dos veces para agregar dos páginas y navegar entre ellas.

1.  En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo de proyecto **BlankApp** para abrir el menú contextual.
2.  Elige **Agregar**&gt;**Nuevo elemento** en el menú contextual.
3.  En el cuadro de diálogo **Agregar nuevo elemento**, elige **Página en blanco** en el panel central.
4.  En el cuadro **Nombre**, escribe **Page1** (o **Page2**) y presiona el botón **Agregar**.

Estos archivos deben aparecer ahora como parte del proyecto NavApp1.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">CS</th>
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cs</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cs</li>
</ul></td>
<td align="left"><ul>
<li>Page1.xaml</li>
<li>Page1.xaml.cpp</li>
<li>Page1.xaml.h</li>
<li>Page2.xaml</li>
<li>Page2.xaml.cpp</li>
<li>Page2.xaml.h
<div class="alert">
<strong>Nota</strong>  
<p>Las funciones se declaran en el archivo de encabezado (.h) y se implementan en el archivo de código subyacente (.cpp).</p>
</div>
<div>
 
</div></li>
</ul></td>
</tr>
</tbody>
</table>

 

Agrega el siguiente contenido a la interfaz de usuario de Page1.xaml.

-   Agrega un elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) denominado `pageTitle` como elemento secundario de la raíz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Cambia la propiedad [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) a `Page 1`.

```    XAML
<TextBlock x:Name="pageTitle" Text="Page 1" /></code></pre></td>
    </tr>
    </tbody>
    </table>
```

-   Agrega el siguiente elemento [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) como elemento secundario de la raíz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) y después el elemento `pageTitle`[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652).

    <span codelanguage="XAML"></span>
```    XAML
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">XAML</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
<HyperlinkButton HorizontalAlignment="Center" Content="Click to go to page 2" Click="HyperlinkButton_Click"/></code></pre></td>
    </tr>
    </tbody>
    </table>
```

Agrega el siguiente código a la clase `Page1` del archivo de código subyacente Page1.xaml para controlar el evento `Click` del objeto [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que agregaste anteriormente. Desde aquí, navegaremos a Page2.xaml.

<span codelanguage="ManagedCPlusPlus"></span>
```ManagedCPlusPlus
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
void Page1::HyperlinkButton_Click_nodata(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid));
}
```

```CSharp
private void HyperlinkButton_Click_nodata(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2));
}
```

Realiza los siguientes cambios a la interfaz de usuario de Page2.xaml.

-   Agrega un elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) denominado `pageTitle` como elemento secundario de la raíz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704). Cambia el valor de la propiedad [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) a `Page 2`.

```    XAML
<TextBlock x:Name="pageTitle" Text="Page 2" /></code></pre></td>
    </tr>
    </tbody>
    </table>
```

-   Agrega el siguiente elemento [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) como elemento secundario de la raíz [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) y después el elemento `pageTitle`[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652).

    <span codelanguage="XAML"></span>
```    XAML
    <colgroup>
    <col width="100%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">XAML</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
<HyperlinkButton HorizontalAlignment="Center" Content="Click to go to page 1" Click="HyperlinkButton_Click"/></code></pre></td>
    </tr>
    </tbody>
    </table>
```

Agrega el siguiente código a la clase `Page2` del archivo de código subyacente Page2.xaml para controlar el evento `Click` del objeto [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que agregaste anteriormente. Desde aquí, navegaremos a Page1.xaml.

**Nota**  
En cuanto a los proyectos de C++, debes agregar una directiva `#include` en el archivo de encabezado de cada página que haga referencia a otra página. Para el ejemplo de navegación entre páginas que se presenta aquí, el archivo page1.xaml.h contiene `#include "Page2.xaml.h"` y, a su vez, el archivo page2.xaml.h contiene `#include "Page1.xaml.h"`.

 

<span codelanguage="ManagedCPlusPlus"></span>
```ManagedCPlusPlus
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">C++</th>
</tr>
</thead>
<tbody>
<tr class="odd">
void Page2::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid));
}
```

```CSharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page1));
}
```

Ahora que hemos preparado las páginas de contenido, necesitamos que Page1.xaml muestre el momento en que se inicia la aplicación.

Abre el archivo de código subyacente app.xaml y cambia el controlador `OnLaunched`.

En este apartado, especificaremos `Page1` en la llamada a [**Frame.Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) en lugar de `MainPage`.

```ManagedCPlusPlus
/// <summary>
/// Invoked when the application is launched normally by the end user. 
/// Other entry points will be used in specific cases, such as when the 
/// application is launched to open a specific file.
/// </summary>
/// <param name="e">Details about the launch request and process.</param>
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
                this, &amp;App::OnNavigationFailed);

        if (e->PreviousExecutionState == ApplicationExecutionState::Terminated)
        {
            // TODO: Load state from previously suspended application
        }

        
        // Place the frame in the current Window
        Window::Current->Content = rootFrame;

    }

    if (rootFrame->Content == nullptr)
    {
        // When the navigation stack isn&#39;t restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page1::typeid), e->Arguments);
    }

    // Ensure the current window is active
    Window::Current->Activate();
}
```

```CSharp
/// <summary>
/// Invoked when the application is launched normally by the end user. 
/// Other entry points will be used in specific cases, such as when the 
/// application is launched to open a specific file.
/// </summary>
/// <param name="e">Details about the launch request and process.</param>
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
        // When the navigation stack isn&#39;t restored navigate to the first page,
        // configuring the new page by passing required information as a navigation
        // parameter
        rootFrame.Navigate(typeof(Page1), e.Arguments);
    }
    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Nota** Este código usa el valor devuelto del método [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) para generar una excepción de aplicación si la navegación al marco de la ventana inicial de la aplicación es errónea. Cuando **Navigate** devuelve **true**, se produce la navegación.

 

Ahora, compila y ejecuta la aplicación. Haz clic en el vínculo que dice "Haz clic para ir a la página 2". La segunda página que muestra "Página 2" en la parte superior, se debería cargar y mostrar en el marco.

## <span id="Frame_and_Page_classes"></span><span id="frame_and_page_classes"></span><span id="FRAME_AND_PAGE_CLASSES"></span>Clases Frame y Page


Antes de agregar más funciones a la aplicación, veamos de qué manera las páginas que incluimos ofrecen compatibilidad de navegación para la aplicación.

Primero, se crea una clase [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) (`rootFrame`) para la aplicación en el método `App.OnLaunched` del archivo de código subyacente App.xaml. El método [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) se usa para mostrar contenido en la clase **Frame**.

**Nota**  
La clase [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) admite diversos métodos de navegación, tales como [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694), [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) y [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693), y propiedades como, por ejemplo, [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543), [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) y [**BackStackDepth**](https://msdn.microsoft.com/library/windows/apps/hh967995).

 

En nuestro ejemplo, `Page1` se pasa al método [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694). Este método establece el contenido de la ventana actual de la aplicación en la clase [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) y carga el contenido de la página especificada en **Frame** (que es Page1.xaml en nuestro ejemplo o MainPage.xaml de forma predeterminada).

`Page1` es una subclase de la clase [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503). La clase **Page** tiene la propiedad de solo lectura [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504) que a su vez obtiene la clase [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) que contiene **Page**. Cuando el controlador de eventos [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) de la clase [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) llama a` Frame.Navigate(typeof(Page2))`, la clase **Frame** de la ventana de la aplicación muestra el contenido de Page2.xaml.

Siempre que se cargue una página en el marco, esa página se agregará como una clase [**PageStackEntry**](https://msdn.microsoft.com/library/windows/apps/dn298572) a la propiedad [**BackStack**](https://msdn.microsoft.com/library/windows/apps/dn279543) o [**ForwardStack**](https://msdn.microsoft.com/library/windows/apps/dn279547) de [**Frame**](https://msdn.microsoft.com/library/windows/apps/br227504).

## <span id="Pass_information_between_pages"></span><span id="pass_information_between_pages"></span><span id="PASS_INFORMATION_BETWEEN_PAGES"></span>Pasar información entre páginas


Nuestra aplicación ya navega entre dos páginas, pero aún no hace nada interesante. A menudo, cuando una aplicación tiene varias páginas, las páginas necesitan compartir información. Pasemos parte de la información de la primera página a la segunda.

En Page1.xaml, reemplaza la clase [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739) que agregaste antes por [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635).

Una vez hecho esto, agregamos una etiqueta [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) y una clase [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) (`name`) para escribir una cadena de texto.

```XAML
<StackPanel>
    <TextBlock HorizontalAlignment="Center" Text="Enter your name"/>
    <TextBox HorizontalAlignment="Center" Width="200" Name="name"/>
    <HyperlinkButton HorizontalAlignment="Center" Content="Click to go to page 2" Click="HyperlinkButton_Click"/>
</StackPanel>
```

En el controlador de eventos `HyperlinkButton_Click` del archivo de código subyacente Page1.xaml, agrega al método `Navigate` un parámetro que haga referencia a la propiedad `Text` del elemento `name`[**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683).

```ManagedCPlusPlus
void Page1::HyperlinkButton_Click(Platform::Object^ sender, RoutedEventArgs^ e)
{
    this->Frame->Navigate(Windows::UI::Xaml::Interop::TypeName(Page2::typeid), name->Text);
}
```

```CSharp
private void HyperlinkButton_Click(object sender, RoutedEventArgs e)
{
    this.Frame.Navigate(typeof(Page2), name.Text);
}
```

En el archivo de código subyacente Page2.xaml invalida el método `OnNavigatedTo` mediante las siguientes acciones:

```ManagedCPlusPlus
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

```CSharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    if (e.Parameter is string)
    {
        greeting.Text = "Hi, " + e.Parameter.ToString();
    }
    else
    {
        greeting.Text = "Hi!";
    }
    base.OnNavigatedTo(e);
}
```

Ejecuta la aplicación, escribe tu nombre en el cuadro de texto y haz clic en el vínculo que dice **Haz clic para ir a la página 2**. Cuando llamaste a `this.Frame.Navigate(typeof(Page2), tb1.Text)` en el evento [**Click**](https://msdn.microsoft.com/library/windows/apps/br227737) del elemento [**HyperlinkButton**](https://msdn.microsoft.com/library/windows/apps/br242739), la propiedad `name.Text` se pasó a `Page2` y se usa el valor de los datos del evento para el mensaje que se muestra en la página.

## <span id="Cache_a__page"></span><span id="cache_a__page"></span><span id="CACHE_A__PAGE"></span>Almacenar una página en caché


El contenido y el estado de la página no se almacena en caché de forma predeterminada, debes habilitar esta opción en cada página de la aplicación.

En nuestro ejemplo básico de punto a punto, no hay ningún botón Atrás (demostraremos la navegación hacia atrás en [Navegación con el botón Atrás](navigation-history-and-backwards-navigation.md)), pero si hiciste clic en un botón Atrás en `Page2`, el elemento [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) (y cualquier otro campo) en `Page1` se establecerá en su estado predeterminado. Una manera de solucionar este problema es usar la propiedad [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) para especificar que una página se agregue a la memoria caché de la página del marco.

En el constructor de `Page1`, establece [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) en [**Enabled**](https://msdn.microsoft.com/library/windows/apps/br243284). Esta acción conservará todos los valores de estado y de contenido de la página hasta que se supere la memoria caché de la página del marco.

Establece [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506) en [**Required**](https://msdn.microsoft.com/library/windows/apps/br243284) si quieres hacer caso omiso de los límites de tamaño de la memoria caché del marco. Sin embargo, ten en cuenta que los límites de tamaño de caché pueden ser esenciales según los límites de memoria de un dispositivo.

**Nota** La propiedad [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) especifica el número de páginas del historial de navegación que se pueden almacenar en caché para el marco.

 

```ManagedCPlusPlus
Page1::Page1()
{
    this->InitializeComponent();
    this->NavigationCacheMode = Windows::UI::Xaml::Navigation::NavigationCacheMode::Enabled;
}
```

```CSharp
public Page1()
{
    this.InitializeComponent();
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

## <span id="related_topics"></span>Artículos relacionados

* [Conceptos básicos del diseño de navegación para las aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/dn958438)
* [Directrices para pestañas y tablas dinámicas](https://msdn.microsoft.com/library/windows/apps/dn997788)
* [Directrices sobre paneles de navegación](https://msdn.microsoft.com/library/windows/apps/dn997766)
 

 







<!--HONumber=Jun16_HO4-->


