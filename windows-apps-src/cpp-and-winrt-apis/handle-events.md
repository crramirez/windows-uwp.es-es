---
author: stevewhims
description: Este tema muestra cómo registrar y revocar delegados de control de eventos con C++/WinRT.
title: Controlar eventos usando delegados en C ++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar c ++ cpp, winrt, proyectado, proyección, controlador, evento, delegado
ms.localizationpriority: medium
ms.openlocfilehash: 1cf3c87411bb6d8eb5886e7205f96c466d707220
ms.sourcegitcommit: 633dd07c3a9a4d1c2421b43c612774c760b4ee58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "1976493"
---
# <a name="handle-events-by-using-delegates-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Controlar eventos usando delegados en [C ++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Este tema muestra cómo registrar y revocar delegados de control de eventos con C++/WinRT. Puedes controlar un evento usando cualquier objeto tipo función de C++ estándar.

> [!NOTE]
> Para obtener información sobre la instalación y uso de la extensión de Visual Studio (VSIX) de C++/WinRT (la cual ofrece soporte para plantillas de proyectos, así como propiedades y destinos de MSBBuild de C++/WinRT), consulta el [Soporte de Visual Studio para C++/WinRT, y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="register-a-delegate-to-handle-an-event"></a>Registrar un delegado para controlar un evento
Un ejemplo sencillo es controlar el evento clic de un botón. Es habitual usar el marcado XAML para registrar una función miembro para controlar el evento, como este.

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    Button().Content(box_value(L"Clicked"));
}
```

En lugar de hacerlo mediante declaración en el marcado, puedes registrar de manera imperativa una función miembro para controlar un evento. Es posible que no se muestre de forma evidente en el siguiente ejemplo de código, pero el argumento para la llamada [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) es una instancia del delegado [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler). En este caso, usaremos la sobrecarga de constructor **RoutedEventHandler** que toma un objeto y un puntero a una función miembro.

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

Hay otras formas de construir un **RoutedEventHandler**. A continuación te mostramos el bloque de sintaxis extraído del tema de documentación relativo al [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) (elige *C++/WinRT* en la lista desplegable **Language** de la página). Ten en cuenta los diversos constructores: uno toma un lambda, otro una función libre y otro (el que hemos usado anteriormente) toma un objeto y un puntero a una función miembro.

```cppwinrt
struct RoutedEventHandler : winrt::Windows::Foundation::IUnknown
{
    RoutedEventHandler(std::nullptr_t = nullptr) noexcept;
    template <typename L> RoutedEventHandler(L lambda);
    template <typename F> RoutedEventHandler(F* function);
    template <typename O, typename M> RoutedEventHandler(O* object, M method);
    void operator()(winrt::Windows::Foundation::IInspectable const& sender,
        winrt::Windows::UI::Xaml::RoutedEventArgs const& e) const;
};
```

También es útil ver la sintaxis del operador de llamada de la función. Te indica qué parámetros de tu delegado deben estar. Como puedes ver, en este caso la sintaxis del operador de la llamada de la función coincide con los parámetros de nuestro **MainPage::ClickHandler**.

Si no vas a hacer mucho en tu controlador de eventos, puedes usar una función lambda en lugar de una función miembro. Una vez más, puede que sea evidente con el siguiente ejemplo de código, pero un delegado **RoutedEventHandler** se está construyendo a partir de una función lambda que, de nuevo, debe coincidir con la sintaxis del operador de la llamada de la función.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click([this](IInspectable const&, RoutedEventArgs const&)
    {
        Button().Content(box_value(L"Clicked"));
    });
}
```

Puedes ser un poco más explícito cuando construyas tu delegado. Por ejemplo, si quieres pasarlo o usarlo más de una vez.

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const&)
    {
        sender.as<winrt::Windows::UI::Xaml::Controls::Button>().Content(box_value(L"Clicked"));
    };
    Button().Click(click_handler);
    AnotherButton().Click(click_handler);
}
```

## <a name="revoke-a-registered-delegate"></a>Revocar un delegado registrado
Cuando registras un delegado, normalmente recibes un token a cambio. Posteriormente podrás usar dicho token para revocar tu delegado; lo que significa que se elimina el registro del delegado desde el evento y no se llamará en caso de que el evento vuelva a generarse. Para mayor sencillez, ninguno de los ejemplos de código anteriores muestra cómo hacerlo. Pero el siguiente ejemplo de código almacena el token en el miembro de datos privados de la estructura y revoca su controlador en el destructor.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button const& button) : m_button(button)
    {
        m_token = m_button.Click([this](IInspectable const&, RoutedEventArgs const&)
        {
            ...
        });
    }
    ~Example()
    {
        m_button.Click(m_token);
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button m_button;
    winrt::event_token m_token;
};
```

En lugar de una referencia fuerte, como se muestra en el ejemplo anterior, puedes almacenar una referencia débil al botón (consulta [Referencias débiles en C++/WinRT](weak-references.md)).

Como alternativa, cuando registras un delegado, puedes especificar **winrt::auto_revoke** (que es un valor de tipo [**winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)) para solicitar un revocador de eventos (de tipo **winrt::event_revoker**). El revocador de eventos mantiene una referencia débil al origen del evento (el objeto que genera el evento) para ti. Puedes revocar manualmente mediante una llamada a la función de miembro **event_revoker::revoke**, pero el revocador de eventos llama a la propia función automáticamente cuando sale del ámbito. La función **revoke** comprueba si el origen del evento aún existe y, si es así, revoca el delegado. En este ejemplo, no es necesario almacenar el origen del evento y no se necesita ningún destructor.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const&, RoutedEventArgs const&)
        {
            ...
        });
    }

private:
    winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase> m_event_revoker;
};
```

A continuación te mostramos el bloque de sintaxis extraído del tema de documentación relativo al evento [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Muestra las tres funciones diferentes de registro y revocación. Puedes ver exactamente qué tipo de revocador de eventos tienes que declarar desde la tercera sobrecarga.

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase> Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

Un patrón similar se aplica a todos los eventos C++/WinRT.

Puede que tengas que considerar la posibilidad de revocar controladores en un escenario de navegación de páginas. Si vas a navegar varias veces por una página y luego vas a volver atrás, podrías revocar los controladores cuando salgas de la página. Como alternativa, si vuelves a usar la misma instancia de página, comprueba el valor de tu token y registra solo si no se ha establecido todavía (`if (!m_token){ ... }`). Una tercera opción es almacenar un revocador de eventos en la página como un miembro de datos. Y una cuarta opción, tal y como se describe más adelante en este tema, es capturar una referencia fuerte o débil al objeto *this* en tu función lambda.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>Tipos de delegados para acciones y operaciones asincrónicas
Los ejemplos anteriores usan el tipo de delegado **RoutedEventHandler** pero, evidentemente, hay muchos otros tipos de delegados. Por ejemplo, las acciones y operaciones asincrónicas (con y sin progreso) tienen eventos completados o en progreso que esperan delegados del correspondiente tipo. Por ejemplo, el evento en progreso de una operación asincrónica con progreso (que es cualquier cosa que implemente [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)) requiere un delegado de tipo [**AsyncOperationProgressHandler **](/uwp/api/windows.foundation.asyncoperationprogresshandler). Este es un ejemplo de código para crear un delegado de este tipo con una función lambda. El ejemplo también muestra cómo crear un delegado [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler).

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    auto async_op_with_progress = syndicationClient.RetrieveFeedAsync(rssFeedUri);

    async_op_with_progress.Progress(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const&, RetrievalProgress const& args)
    {
        uint32_t bytes_retrieved = args.BytesRetrieved;
        // use bytes_retrieved;
    });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const)
    {
        SyndicationFeed syndicationFeed = sender.GetResults();
        // use syndicationFeed;
    });
    
    // or (but this function must then be a coroutine and return IAsyncAction)
    // SyndicationFeed syndicationFeed = co_await async_op_with_progress;
}
```

Como sugiere el comentario de corrutina anterior, en lugar de usar un delegado con los eventos completados de acciones y operaciones asincrónicas, posiblemente te resulte más natural usar las corrutinas. Para obtener información detallada y ejemplos de código, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md).

Pero si te quedas con delegados, puedes optar por una sintaxis más sencilla.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const)
{
    ....
});
```

## <a name="delegate-types-that-return-a-value"></a>Tipos de delegado que devuelven un valor
Algunos tipos de delegado deben devolver un valor. Un ejemplo es [**ListViewItemToKeyHandler**](/uwp/api/windows.ui.xaml.controls.listviewitemtokeyhandler), que devuelve una cadena. Aquí tienes un ejemplo de creación de un delegado de este tipo (ten en cuenta que la función lambda devuelve un valor).

```cppwinrt
using namespace winrt::Windows::UI::Xaml::Controls;

winrt::hstring f(ListView listview)
{
    return ListViewPersistenceHelper::GetRelativeScrollPosition(listview, [](IInspectable const& item)
    {
        return L"key for item goes here";
    });
}
```

## <a name="using-the-this-object-in-an-event-handler"></a>Con el objeto *this* en controlador de eventos
Si controlas un evento desde dentro de una función lambda dentro de la función miembro de un objeto, debes tener en cuenta las duraciones relativas del destinatario del evento (el objeto que controla el evento) y el origen del evento (el objeto que genera el evento).

En muchos casos, un destinatario sobrevive a todas las dependencias en su puntero *this* desde dentro de una función lambda determinada. Algunos de estos casos son evidentes, por ejemplo, cuando una página de interfaz de usuario controla un evento generado por un control que se encuentra en la página. El botón no sobrevive a la página, por lo tanto, tampoco lo hace controlador. Esto es válido siempre que el destinatario posea el origen (como un miembro de datos, por ejemplo), o cada vez que el destinatario y el origen estén relacionados o pertenezcan directamente a otro objeto. Si estás seguro de que tienes un caso en el que el controlador no sobrevivirá al objeto *this* del que depende, puedes capturar *this* de forma normal, sin tener en cuenta una duración fuerte o débil.

Pero todavía hay casos donde *this* no sobrevive a su uso en un controlador (incluidos los controladores para eventos de finalización y progreso generados por acciones y operaciones asincrónicas).

- Si vas a crear una corrutina para implementar un método asincrónico, entonces es posible.
- En raras ocasiones con ciertos objetos del marco de la interfaz de usuario XAML ([**SwapChainPanel**](/uwp/api/windows.ui.xaml.controls.swapchainpanel), por ejemplo) es posible, siempre que se haya finalizado el destinatario sin anular el registro del origen del evento.

En estos casos, se produce una infracción de acceso desde el código en un controlador o en la continuación de una corrutina que intenta utilizar el objeto *this* no válido.

> [!IMPORTANT]
> Si encuentras alguna de estas situaciones, tendrás que pensar en la duración del *this*; y si el objeto capturado *this* sobrevive o no a la captura. Si no es así, captúralo con una referencia fuerte o débil, según corresponda. Consulta [**implements::get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function) y [**implements::get_weak **](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function).
> O bien&mdash;si tiene sentido para tu escenario y si las consideraciones de subprocesos lo hacen posible&mdash;, otra opción es revocar el controlador una vez hecho el destinatario con el evento o en el destructor del destinatario.

Este ejemplo de código usa el evento [**SwapChainPanel.CompositionScaleChanged**](/uwp/api/windows.ui.xaml.controls.swapchainpanel.compositionscalechanged) como ejemplo. Se registra un controlador de eventos usando un lambda que captura una referencia débil al destinatario. Para obtener más información acerca de las referencias débiles, consulta [Referencias débiles en C++/WinRT ](weak-references.md). 

```cppwinrt
winrt::Windows::UI::Xaml::Controls::SwapChainPanel m_swapChainPanel;
winrt::event_token m_compositionScaleChangedEventToken;

void RegisterEventHandler()
{
    m_compositionScaleChangedEventToken = m_swapChainPanel.CompositionScaleChanged([weakReferenceToThis{ get_weak() }]
        (Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
        Windows::Foundation::IInspectable const& object)
    {
        if (auto strongReferenceToThis = weakReferenceToThis.get())
        {
            strongReferenceToThis->OnCompositionScaleChanged(sender, object);
        }
    });
}

void OnCompositionScaleChanged(Windows::UI::Xaml::Controls::SwapChainPanel const& sender,
    Windows::Foundation::IInspectable const& object)
{
    // Here, we know that the "this" object is valid.
}
```

En la cláusula de captura lamba, se crea una variable temporal que representa una referencia débil a *this*. En el cuerpo del lambda, si puede obtenerse una referencia fuerte a *this*, se llama a la función **OnCompositionScaleChanged**. De esta forma, *this* puede usarse con seguridad dentro de ** OnCompositionScaleChanged**.

## <a name="important-apis"></a>API importantes
* [winrt::auto_revoke_t](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [winrt::implements::get_weak function](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [winrt::implements::get_strong function](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)

## <a name="related-topics"></a>Artículos relacionados
* [Crear eventos en C++/WinRT](author-events.md)
* [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md)
* [Referencias débiles en C++/WinRT](weak-references.md)
