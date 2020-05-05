---
description: En este tema se muestra cómo registrar y revocar delegados de control de eventos con C++/WinRT.
title: Control de eventos mediante delegados en C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, estándar c ++ cpp, winrt, proyectado, proyección, controlador, evento, delegado
ms.localizationpriority: medium
ms.openlocfilehash: eae966c130c52305b53cc4122844aeae49ecab92
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82267493"
---
# <a name="handle-events-by-using-delegates-in-cwinrt"></a>Control de eventos mediante delegados en C++/WinRT

En este tema se muestra cómo registrar y revocar delegados de control de eventos con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Puedes controlar un evento mediante cualquier objeto de tipo función de C++ estándar.

> [!NOTE]
> Para más información sobre cómo instalar y usar C++/WinRT Visual Studio Extension (VSIX) y el paquete de NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación), consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="using-visual-studio-2019-to-add-an-event-handler"></a>Usar Visual Studio 2019 para agregar un controlador de eventos

Una manera cómoda de agregar un controlador de eventos al proyecto es mediante la interfaz de usuario (IU) del diseñador XAML en Visual Studio 2019. Con la página XAML abierta en el diseñador XAML, selecciona el control cuyo evento quieres controlar. En la página de propiedades de ese control, haz clic en el icono con forma de rayo para enumerar todos los eventos que provienen de ese control. A continuación, haz doble clic en el evento que quieres administrar; por ejemplo, *OnClicked*.

El diseñador XAML agrega el prototipo apropiado de la función del controlador de eventos (además de una implementación de código auxiliar) a tus archivos de origen, que ya estarán listos para que los reemplaces con tu propia implementación.

> [!NOTE]
> Normalmente, no es necesario describir los controladores de eventos en el archivo Midl (`.idl`). Por lo tanto, el diseñador XAML no agrega prototipos de función del controlador de eventos a tu archivo Midl. Solo les agrega los archivos `.h` y `.cpp`.

## <a name="register-a-delegate-to-handle-an-event"></a>Registro de un delegado para controlar un evento

Un ejemplo sencillo es controlar el evento clic de un botón. Es habitual usar el marcado XAML para registrar una función miembro con el fin de controlar el evento, como este.

```xaml
// MainPage.xaml
<Button x:Name="Button" Click="ClickHandler">Click Me</Button>
```

```cppwinrt
// MainPage.h
void ClickHandler(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::RoutedEventArgs const& args);

// MainPage.cpp
void MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
{
    Button().Content(box_value(L"Clicked"));
}
```

En lugar de hacerlo mediante declaración en el marcado, puedes registrar de manera imperativa una función miembro para controlar un evento. Es posible que no se muestre de forma evidente en el siguiente ejemplo de código, pero el argumento para la llamada [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) es una instancia del delegado [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler). En este caso, usaremos la sobrecarga de constructor **RoutedEventHandler** que toma un objeto y un puntero a función miembro.

```cppwinrt
// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click({ this, &MainPage::ClickHandler });
}
```

> [!IMPORTANT]
> Al registrar el delegado, el ejemplo de código anterior pasa sin formato el puntero *this* (que apunta al objeto actual). Para saber cómo establecer una referencia segura o poco segura al objeto actual, consulta [If you use a member function as a delegate](weak-references.md#if-you-use-a-member-function-as-a-delegate) (Si usas una función miembro como delegado).

Este es un ejemplo que usa una función miembro estático; ten en cuenta la sintaxis más sencilla.

```cppwinrt
// MainPage.h
static void ClickHandler(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::RoutedEventArgs const& args);

// MainPage.cpp
MainPage::MainPage()
{
    InitializeComponent();

    Button().Click( MainPage::ClickHandler );
}
void MainPage::ClickHandler(IInspectable const& /* sender */, RoutedEventArgs const& /* args */) { ... }
```

Hay otras formas de construir un **RoutedEventHandler**. Debajo te mostramos el bloque de sintaxis extraído del tema de documentación relativo al [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler) (elige *C++/WinRT* en la lista desplegable **Lenguaje** situada en la esquina superior izquierda de la página web). Ten en cuenta los diversos constructores: uno toma una expresión lambda, otro una función libre y otro (el que hemos usado anteriormente) toma un objeto y un puntero a función miembro.

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

También es útil ver la sintaxis del operador de la llamada de la función. Te indica qué parámetros de tu delegado deben estar. Como puedes ver, en este caso la sintaxis del operador de la llamada de la función coincide con los parámetros de nuestro **MainPage::ClickHandler**.

> [!NOTE]
> Para un evento determinado, con el fin de averiguar los detalles de su delegado y los parámetros de ese delegado, ve primero al tema de documentación para el propio evento. Veamos el [evento UIElement.KeyDown](/uwp/api/windows.ui.xaml.uielement.keydown) como ejemplo. Visita ese tema y elige  *C++/WinRT* en la lista desplegable **Lenguaje**. En el bloque de sintaxis al principio del tema, verás esto.
> 
> ```cppwinrt
> // Register
> event_token KeyDown(KeyEventHandler const& handler) const;
> ```
>
> Esa información nos indica que el evento **UIElement.KeyDown** (el tema en el que estamos) tiene un tipo de delegado de **KeyEventHandler**, ya que es el tipo que se pasa al registrar un delegado con este tipo de evento. Por lo tanto, sigue ahora el vínculo en el tema a ese tipo de [delegado KeyEventHandler](/uwp/api/windows.ui.xaml.input.keyeventhandler). En este caso, el bloque de sintaxis contiene un operador de la llamada de la función. Y, tal como se ha mencionado anteriormente, eso indica los parámetros de tu delegado que deben estar.
> 
> ```cppwinrt
> void operator()(winrt::Windows::Foundation::IInspectable const& sender, winrt::Windows::UI::Xaml::Input::KeyRoutedEventArgs const& e) const;
> ```
>
>  Como puedes ver, el delegado necesita que se declare para que tome un **IInspectable** como remitente y una instancia de la [clase KeyRoutedEventArgs](/uwp/api/windows.ui.xaml.input.keyroutedeventargs) como argumentos.
>
> Para mostrar otro ejemplo, echemos un vistazo al [evento Popup.Closed](/uwp/api/windows.ui.xaml.controls.primitives.popup.closed). Su tipo de delegado es [EventHandler\<IInspectable\>](/uwp/api/windows.foundation.eventhandler). Por lo tanto, el delegado tomará un **IInspectable** como remitente y otro **IInspectable** (porque es el tipo de parámetro de **EventHandler**) como argumentos.

Si no vas a hacer mucho en tu controlador de eventos, puedes usar una función lambda en lugar de una función miembro. Una vez más, puede que sea evidente con el siguiente ejemplo de código, pero un delegado **RoutedEventHandler** se está construyendo a partir de una función lambda que, de nuevo, debe coincidir con la sintaxis del operador de la llamada de la función que se ha tratado anteriormente.

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

## <a name="revoke-a-registered-delegate"></a>Revocación de un delegado registrado

Cuando registras un delegado, normalmente recibes un token a cambio. Posteriormente podrás usar dicho token para revocar tu delegado; lo que significa que se elimina el registro del delegado desde el evento y no se llamará en caso de que el evento vuelva a generarse.

Para mayor sencillez, ninguno de los ejemplos de código anteriores muestra cómo hacerlo. Pero el siguiente ejemplo de código almacena el token en el miembro de datos privados de la estructura y revoca su controlador en el destructor.

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

En lugar de una referencia fuerte, como se muestra en el ejemplo anterior, puedes almacenar una referencia débil al botón (consulta [Referencias fuertes y débiles en C++/WinRT](weak-references.md)).

> [!NOTE]
> Cuando un origen de eventos genera sus eventos sincrónicamente, puedes revocar el controlador y estar seguro de que no recibirás más eventos. Pero para los eventos asincrónicos, incluso después de la revocación (y especialmente al revocar dentro del destructor), un evento en curso podría alcanzar el objeto después de que se haya iniciado la destrucción. Buscar un lugar para cancelar la suscripción antes de la destrucción puede mitigar el problema o, para una solución más estable, consulta [Acceso de forma segura al puntero *this* con un delegado de control de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

Como alternativa, cuando registras un delegado, puedes especificar **winrt::auto_revoke** (que es un valor de tipo [**winrt::auto_revoke_t**](/uwp/cpp-ref-for-winrt/auto-revoke-t)) para solicitar un revocador de eventos (de tipo [**winrt::event_revoker**](/uwp/cpp-ref-for-winrt/event-revoker)). El revocador de eventos mantiene una referencia débil al origen del evento (el objeto que genera el evento) para ti. Puedes revocar manualmente mediante una llamada a la función miembro **event_revoker::revoke**, pero el revocador de eventos llama a la propia función automáticamente cuando sale del ámbito. La función **revoke** comprueba si el origen del evento aún existe y, si es así, revoca el delegado. En este ejemplo, no es necesario almacenar el origen del evento y no se necesita ningún destructor.

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

Debajo te mostramos el bloque de sintaxis extraído del tema de documentación relativo al evento [**ButtonBase::Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click). Muestra las tres funciones diferentes de registro y revocación. Puedes ver exactamente qué tipo de revocador de eventos tienes que declarar desde la tercera sobrecarga.

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
> En el ejemplo de código anterior, `Button::Click_revoker` es un alias de tipo para `winrt::event_revoker<winrt::Windows::UI::Xaml::Controls::Primitives::IButtonBase>`. Un patrón similar se aplica a todos los eventos C++/WinRT. Cada evento de Windows Runtime tiene una sobrecarga de función revoke que devuelve un revocador de eventos y ese tipo de revocador es un miembro del origen del evento. Por lo tanto, para mostrar otro ejemplo, el evento [**CoreWindow::SizeChanged**](/uwp/api/windows.ui.core.corewindow.sizechanged) tiene una sobrecarga de función de registro que devuelve un valor de tipo **CoreWindow::SizeChanged_revoker**.

Puede que tengas que considerar la posibilidad de revocar controladores en un escenario de navegación de páginas. Si vas a navegar varias veces por una página y luego vas a volver atrás, podrías revocar los controladores cuando salgas de la página. Como alternativa, si vuelves a usar la misma instancia de página, comprueba el valor de tu token y regístralo solo si no se ha establecido todavía (`if (!m_token){ ... }`). Una tercera opción es almacenar un revocador de eventos en la página como un miembro de datos. Y una cuarta opción, tal y como se describe más adelante en este tema, es capturar una referencia fuerte o débil al objeto *this* en tu función lambda.

### <a name="if-your-auto-revoke-delegate-fails-to-register"></a>Si el delegado de revocación automática no se registra

Si intentas especificar [**winrt::auto_revoke**](/uwp/cpp-ref-for-winrt/auto-revoke-t) al registrar un delegado y el resultado es una excepción [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface), eso generalmente significa que el origen del evento no admite referencias poco seguras. Esta situación es común en el espacio de nombres [**Windows.UI.Composition**](/uwp/api/windows.ui.composition), por ejemplo. En esta situación, no puedes usar la función de revocación automática. Tendrás que recurrir a la revocación manual de tus controladores de eventos.

## <a name="delegate-types-for-asynchronous-actions-and-operations"></a>Tipos de delegados para acciones y operaciones asincrónicas

Los ejemplos anteriores usan el tipo de delegado **RoutedEventHandler** pero, evidentemente, hay muchos otros tipos de delegados. Por ejemplo, las acciones y operaciones asincrónicas (con y sin progreso) tienen eventos completados y en progreso que esperan delegados del correspondiente tipo. Por ejemplo, el evento en progreso de una operación asincrónica con progreso (que es cualquier cosa que implemente [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)) requiere un delegado de tipo [**AsyncOperationProgressHandler** ](/uwp/api/windows.foundation.asyncoperationprogresshandler-2). Este es un ejemplo de código para crear un delegado de este tipo con una función lambda. El ejemplo también muestra cómo crear un delegado [**AsyncOperationWithProgressCompletedHandler**](/uwp/api/windows.foundation.asyncoperationwithprogresscompletedhandler-2).

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

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
> No es correcto implementar más de un *controlador de finalización* para una operación o acción asincrónica. Puedes tener un solo delegado para su evento completado o bien puedes aplicar `co_await`. Si tienes ambos, se producirá un error en el segundo.

Si continúas con delegados en lugar de con una corrutina, podrás optar por una sintaxis más sencilla.

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

## <a name="safely-accessing-the-this-pointer-with-an-event-handling-delegate"></a>Acceso de forma segura al puntero *this* con un delegado de control de eventos

Si controlas un evento con la función miembro de un objeto o desde dentro de una función lambda dentro de la función miembro de un objeto, debes tener en cuenta las duraciones relativas del destinatario del evento (el objeto que controla el evento) y el origen del evento (el objeto que genera el evento). Para obtener más información y ejemplos de código, consulta [Referencias fuertes y débiles en C++/WinRT](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate).

## <a name="important-apis"></a>API importantes
* [estructura de marcador winrt::auto_revoke_t](/uwp/cpp-ref-for-winrt/auto-revoke-t)
* [función winrt::implements::get_weak](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)
* [función winrt::implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)

## <a name="related-topics"></a>Temas relacionados
* [Crear eventos en C++/WinRT](author-events.md)
* [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md)
* [Referencias fuertes y débiles de C++/WinRT](weak-references.md)
