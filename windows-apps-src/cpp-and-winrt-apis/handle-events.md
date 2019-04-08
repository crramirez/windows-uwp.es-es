---
description: Este tema muestra cómo registrar y revocar delegados de control de eventos con C++/WinRT.
title: Controlar eventos usando delegados en C++/WinRT
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, estándar c ++ cpp, winrt, proyectado, proyección, controlador, evento, delegado
ms.localizationpriority: medium
ms.openlocfilehash: 193d821b44722e150f38da7430504f5d528770a4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602430"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>Controlar eventos usando delegados en C++/WinRT

Este tema muestra cómo registrar y revocar los delegados de control de eventos mediante [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Puedes controlar un evento usando cualquier objeto tipo función de C++ estándar.

> [!NOTE]
> Para obtener información sobre cómo instalar y usar C++ / c++ / extensión de Visual Studio (VSIX) de WinRT (que proporciona compatibilidad con plantillas de proyecto, así como C++ / c++ / WinRT MSBuild propiedades y destinos) vea [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="register-a-delegate-to-handle-an-event"></a>Registrar un delegado para controlar un evento

Un ejemplo sencillo es controlar el evento clic de un botón. Es habitual usar el marcado XAML para registrar una función miembro para controlar el evento, como este.

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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

> [!IMPORTANT]
> Cuando se registra el delegado, el ejemplo de código anterior pasa sin formato *esto* puntero (que señala al objeto actual). Para obtener información sobre cómo establecer un fuerte o una referencia débil al objeto actual, vea el **si usa una función miembro como un delegado** subsección en la sección [con seguridad de acceso a la *esto* puntero con un delegado de control de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

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

    Button().Click([this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
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

    auto click_handler = [](IInspectable const& sender, RoutedEventArgs const& /* args */)
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
            // ...
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

En lugar de una referencia segura, como se muestra en el ejemplo anterior, puede almacenar una referencia débil al botón (consulte [referencias fuertes y débiles en C / c++ / WinRT](weak-references.md)).

Como alternativa, al registrar un delegado, puede especificar **winrt::auto_revoke** (que es un valor de tipo [ **winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)) para solicitar un revoker eventos (de tipo [ **winrt::event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker)). El revoker de evento contiene una referencia débil al origen del evento (el objeto que provoca el evento) por usted. Puedes revocar manualmente mediante una llamada a la función de miembro **event_revoker::revoke**, pero el revocador de eventos llama a la propia función automáticamente cuando sale del ámbito. La función **revoke** comprueba si el origen del evento aún existe y, si es así, revoca el delegado. En este ejemplo, no es necesario almacenar el origen del evento y no se necesita ningún destructor.

```cppwinrt
struct Example : ExampleT<Example>
{
    Example(winrt::Windows::UI::Xaml::Controls::Button button)
    {
        m_event_revoker = button.Click(winrt::auto_revoke, [this](IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
        {
            // ...
        });
    }

private:
    winrt::Windows::UI::Xaml::Controls::Button::Click_revoker m_event_revoker;
};
```

A continuación te mostramos el bloque de sintaxis extraído del tema de documentación relativo al evento [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Muestra las tres funciones diferentes de registro y revocación. Puedes ver exactamente qué tipo de revocador de eventos tienes que declarar desde la tercera sobrecarga.

```cppwinrt
// Register
winrt::event_token Click(winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;

// Revoke with event_token
void Click(winrt::event_token const& token) const;

// Revoke with event_revoker
Button::Click_revoker Click(winrt::auto_revoke_t,
    winrt::Windows::UI::Xaml::RoutedEventHandler const& handler) const;
```

> [!NOTE]
> En el ejemplo de código anterior, `Button::Click_revoker` es un alias de tipo para `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`. Un patrón similar se aplica a todos los eventos C++/WinRT. Cada evento de Windows en tiempo de ejecución tiene una sobrecarga de función revoke que devuelve a un revoker de evento y tipo de revoker es un miembro del origen del evento. Por lo tanto, realizar otro ejemplo, el [ **CoreWindow::SizeChanged** ](/uwp/api/windows.ui.core.corewindow.sizechanged) el evento tiene una sobrecarga de función de registro que devuelve un valor de tipo **CoreWindow::SizeChanged_revoker**.


Puede que tengas que considerar la posibilidad de revocar controladores en un escenario de navegación de páginas. Si vas a navegar varias veces por una página y luego vas a volver atrás, podrías revocar los controladores cuando salgas de la página. Como alternativa, si vuelves a usar la misma instancia de página, comprueba el valor de tu token y registra solo si no se ha establecido todavía (`if (!m_token){ ... }`). Una tercera opción es almacenar un revocador de eventos en la página como un miembro de datos. Y una cuarta opción, tal y como se describe más adelante en este tema, es capturar una referencia fuerte o débil al objeto *this* en tu función lambda.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>Tipos de delegados para acciones y operaciones asincrónicas

Los ejemplos anteriores usan el tipo de delegado **RoutedEventHandler** pero, evidentemente, hay muchos otros tipos de delegados. Por ejemplo, las acciones y operaciones asincrónicas (con y sin progreso) tienen eventos completados o en progreso que esperan delegados del correspondiente tipo. Por ejemplo, el evento en progreso de una operación asincrónica con progreso (que es cualquier cosa que implemente [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)) requiere un delegado de tipo [**AsyncOperationProgressHandler** ](/uwp/api/windows.foundation.asyncoperationprogresshandler). Este es un ejemplo de código para crear un delegado de este tipo con una función lambda. El ejemplo también muestra cómo crear un delegado [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler).

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
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& /* sender */, RetrievalProgress const& args)
    {
        uint32_t bytes_retrieved = args.BytesRetrieved;
        // use bytes_retrieved;
    });

    async_op_with_progress.Completed(
        [](IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> const& sender, AsyncStatus const /* asyncStatus */)
    {
        SyndicationFeed syndicationFeed = sender.GetResults();
        // use syndicationFeed;
    });
    
    // or (but this function must then be a coroutine, and return IAsyncAction)
    // SyndicationFeed syndicationFeed{ co_await async_op_with_progress };
}
```

Como sugiere el comentario de corrutina anterior, en lugar de usar un delegado con los eventos completados de acciones y operaciones asincrónicas, posiblemente te resulte más natural usar las corrutinas. Para obtener información detallada y ejemplos de código, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md).

> [!NOTE]
> No es correcto implementar más de un *controlador de finalización* para una operación o acción asincrónica. Puede tener un solo delegado para su evento completado, o bien puede `co_await` lo. Si tiene ambos, se producirá un error en la segunda.

Si te quedas con delegados en lugar de una corrutina, a continuación, puede optar por una sintaxis más sencilla.

```cppwinrt
async_op_with_progress.Completed(
    [](auto&& /*sender*/, AsyncStatus const /* args */)
{
    // ...
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

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Acceso de forma segura la *esto* puntero con un delegado de control de eventos

Si controla un evento con la función de miembro de un objeto, o desde dentro de una función lambda dentro de la función de miembro de un objeto, a continuación, debe considerar la duración relativa del destinatario de eventos (el objeto que controla el evento) y el origen del evento (el objeto Provoca el evento). Para obtener más información y ejemplos de código, vea [referencias fuertes y débiles en C / c++ / WinRT](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

## <a name="important-apis"></a>API importantes
* [winrt::auto_revoke_t marcador struct](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [función winrt::Implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsgetweak-function)
* [función winrt::Implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsgetstrong-function)

## <a name="related-topics"></a>Temas relacionados
* [Crear eventos en C / c++ / WinRT](author-events.md)
* [Simultaneidad y operaciones asincrónicas con C++ / c++ / WinRT](concurrency.md)
* [Referencias fuertes y débiles de C++/WinRT](weak-references.md)
