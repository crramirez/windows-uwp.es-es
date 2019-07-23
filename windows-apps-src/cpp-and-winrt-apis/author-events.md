---
description: En este tema se muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que genera eventos. También se muestra una aplicación que consume el componente y controla los eventos.
title: Crear eventos en C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, author, event
ms.localizationpriority: medium
ms.openlocfilehash: fc3b07848215699afe971674acfa7606ffb21bce
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660172"
---
# <a name="author-events-in-cwinrt"></a>Crear eventos en C++/WinRT

Este tema muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que representa una cuenta bancaria, lo cual genera un evento cuando su saldo pasa a estar en débito. También muestra una aplicación principal que consume la clase en tiempo de ejecución de la cuenta bancaria, llama a una función para ajustar el saldo y controla cualquier evento que surja.

> [!NOTE]
> Para más información sobre cómo instalar y usar [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) Visual Studio Extension (VSIX) y el paquete de NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación), consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT ](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Crea un componente de Windows Runtime (BankAccountWRC)

Para empezar, crea un proyecto en Microsoft Visual Studio. Crea un proyecto **Componente de Windows Runtime (C++/WinRT)** y asígnale el nombre *BankAccountWRC* (de "componente de cuenta bancaria de Windows Runtime"). No compiles aún el proyecto.

El proyecto recién creado contiene un archivo llamado `Class.idl`. Cambia el nombre de ese archivo `BankAccount.idl` (al cambiar el nombre del archivo `.idl` se cambia automáticamente el nombre de los archivos `.h` y `.cpp` dependientes). Reemplaza el contenido de `BankAccount.idl` por la lista siguiente.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        event Windows.Foundation.EventHandler<Single> AccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

Guarda el archivo. El proyecto no se compilará totalmente en este momento, pero compilar ahora resulta útil porque genera los archivos de código fuente en el que implementarás la clase en tiempo de ejecución **BankAccount**. Así que puedes compilar ahora. En esta fase, verás errores de compilación porque no se han encontrado `Class.h` y `Class.g.h`.

Durante el proceso de compilación, la herramienta `midl.exe` se ejecuta para crear el archivo de metadatos de Windows Runtime de tu componente, que es `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`. Después se ejecutará la herramienta `cppwinrt.exe` (con la opción `-component`) para generar archivos de código fuente y ayudarte a crear tu componente. Estos archivos incluyen código auxiliar para que puedas empezar a implementar la clase en tiempo de ejecución **BankAccount** que declaraste en tu archivo IDL. Estos archivos de código auxiliar son `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` y `BankAccount.cpp`.

Haz clic con el botón derecho en el nodo del proyecto y haz clic en **Abrir carpeta en el Explorador de archivos**. Se abre la carpeta del proyecto en el Explorador de archivos. Allí, copia los archivos de código auxiliar `BankAccount.h` y `BankAccount.cpp` desde la carpeta `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` en la carpeta que contiene los archivos de proyecto, que es `\BankAccountWRC\BankAccountWRC\`, y reemplaza los archivos que hay en el destino. Ahora, vamos a abrir `BankAccount.h` y `BankAccount.cpp` e implementar nuestra clase en tiempo de ejecución. En `BankAccount.h`, agrega dos miembros privados a la implementación (*no* la implementación de fábrica) de BankAccount.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_accountIsInDebitEvent;
        float m_balance{ 0.f };
    };
}
...
```

Como puedes ver, el evento se implementa en términos de la plantilla de estructura [**winrt::event**](/uwp/cpp-ref-for-winrt/event), y se parametriza con un tipo de delegado determinado.

En `BankAccount.cpp`, implementa las funciones, como se muestra en el siguiente ejemplo de código. En C++ / WinRT, un evento declarado IDL se implementa como un conjunto de funciones sobrecargadas (similares a la forma en que se implementa una propiedad como un par de funciones get y set sobrecargadas). Una sobrecarga toma un delegado que se va a registrar y devuelve un token. La otra toma un token y revoca el registro del delegado asociado.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(winrt::event_token const& token) noexcept
    {
        m_accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f) m_accountIsInDebitEvent(*this, m_balance);
    }
}
```

> [!NOTE]
> Para conocer de qué se trata un revocador automático de eventos, consulta [Revocación de un delegado registrado](handle-events.md#revoke-a-registered-delegate). La implementación del revocador automático de eventos se incluye de forma gratuita para el evento. Es decir, no es necesario implementar la sobrecarga para el revocador de eventos &mdash;que la proyección de C++/WinRT proporciona automáticamente.

Las otras sobrecargas (las sobrecargas de revocación manual y registro) *no* se incluyen en la proyección. Esto te proporciona la flexibilidad para implementarlas de forma óptima para tu escenario. Una llamada a [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) y a [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) tal como se muestra en estas implementaciones es un valor predeterminado eficaz y seguro para simultaneidad o subprocesos. Pero si tienes un gran número de eventos, es posible que no desees un campo de evento para cada uno, sino optar en su lugar por algún tipo de implementación dispersa.

También puedes ver por encima la implementación de la función **AdjustBalance** genera el evento **AccountIsInDebit** si el saldo pasa a ser negativo.

Si las advertencias te impiden compilar, resuélvelas o establece la propiedad de proyecto **C/C++**  > **General** > **Tratar advertencias como errores** en **No (/WX-)** y vuelve a compilar el proyecto.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Crear una aplicación principal (BankAccountCoreApp) para probar el componente de Windows Runtime

Ahora, crea un nuevo proyecto (en tu solución `BankAccountWRC` o en una nueva). Crea un proyecto **Core App (C++/WinRT)** y asígnale el nombre *BankAccountCoreApp*.

Agrega una referencia y busca `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (o agrega una referencia entre los proyectos, si los dos están en la misma solución). Haz clic en **Agregar** y después en **Aceptar**. Ahora compila BankAccountCoreApp. En el improbable caso de que aparezca un error que indica que el archivo de carga `readme.txt` no existe, excluye este archivo del proyecto de componente de Windows Runtime, vuelve a compilarlo y luego compila de nuevo BankAccountCoreApp.

Durante el proceso de compilación, la herramienta `cppwinrt.exe` se ejecutará para procesar el archivo `.winmd` al que se hace referencia en los archivos de código fuente que contienen tipos proyectados para ayudarte a consumir tu componente. El encabezado de los tipos proyectados para las clases en tiempo de ejecución de tu componente&mdash;denominado `BankAccountWRC.h`&mdash;se genera en la carpeta `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Incluye dicho encabezado en `App.cpp`.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

También en `App.cpp`, agrega el siguiente código para crear una instancia de un BankAccount (usando el constructor predeterminado del tipo proyectado), registra un controlador de eventos y, a continuación, haz que la cuenta pase a estar en débito.

`WINRT_ASSERT` es una definición de macro y se expande a [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            WINRT_ASSERT(balance < 0.f); // Put a breakpoint here.
        });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.AccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

Cada vez que hagas clic en la ventana, restarás 1 del saldo de la cuenta bancaria. Para demostrar que se genera el evento según lo esperado, coloca un punto de interrupción en la expresión lambda que controla el evento **AccountIsInDebit**, ejecuta la aplicación y haz clic dentro de la ventana.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Delegados con parámetros y señales simples a través de una ABI

Si el evento debe ser accesible mediante una interfaz binaria de aplicación (ABI), &mdash;por ejemplo, entre un componente y su aplicación consumidora&mdash;, el evento debe usar un tipo delegado de Windows Runtime. En el ejemplo anterior se usa el tipo delegado [**Windows::Foundation::EventHandler\<T\>** ](/uwp/api/windows.foundation.eventhandler) de Windows Runtime. [**TypedEventHandler\<TSender, TResult\>** ](/uwp/api/windows.foundation.eventhandler) es otro ejemplo de un tipo delegado de Windows Runtime.

Los parámetros de tipo de esos dos tipos delegados deben cruzar la ABI, por lo que los parámetros deben ser también tipos de Windows Runtime. Eso incluye clases en tiempo de ejecución propias y de terceros, así como tipos primitivos como cadenas y números. El compilador te ayuda con el error"*debe ser de tipo WinRT*" si olvidas esta restricción.

Si no tienes que pasar los parámetros o los argumentos con el evento, puedes definir tu propio tipo delegado simple de Windows Runtime. En el ejemplo siguiente se muestra una versión más sencilla de la clase en tiempo de ejecución **BankAccount**. Declara un tipo delegado llamado **SignalDelegate** y lo usa para generar un evento de tipo de señal en lugar de un evento con un parámetro.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    delegate void SignalDelegate();

    runtimeclass BankAccount
    {
        BankAccount();
        event BankAccountWRC.SignalDelegate SignalAccountIsInDebit;
        void AdjustBalance(Single value);
    };
}
```

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

        winrt::event_token SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler);
        void SignalAccountIsInDebit(winrt::event_token const& token);
        void AdjustBalance(float value);

    private:
        winrt::event<BankAccountWRC::SignalDelegate> m_signal;
        float m_balance{ 0.f };
    };
}
```

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    winrt::event_token BankAccount::SignalAccountIsInDebit(BankAccountWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void BankAccount::SignalAccountIsInDebit(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
        if (m_balance < 0.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_bankAccount.SignalAccountIsInDebit([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_bankAccount.SignalAccountIsInDebit(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Delegados con parámetros, señales simples y devoluciones de llamadas dentro de un proyecto

Si el evento solo se utiliza internamente dentro del proyecto de C++/WinRT (no en archivos binarios), se sigue usando la plantilla de estructura [**winrt::event**](/uwp/cpp-ref-for-winrt/event), pero se parametriza con el parámetro [**winrt::delegate&lt; de C++/WinRT que no es de Windows Runtime. Plantilla de estructura T&gt;** ](/uwp/cpp-ref-for-winrt/delegate), que es un delegado eficaz que se tiene en cuenta en las referencias. Admite cualquier número de parámetros y no se limita a los tipos de Windows Runtime.

El ejemplo siguiente muestra primero una firma delegada que no toma ningún parámetro (básicamente una señal simple) y, a continuación, una que toma una cadena.

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

Observa que puedes agregar al evento tantos delegados de suscripción como quieras. Sin embargo, los eventos conllevan una cierta sobrecarga. Si todo lo que necesitas es una devolución de llamada simple con un solo delegado de suscripción, puedes usar [**winrt::delegate&lt;. T&gt;**  ](/uwp/cpp-ref-for-winrt/delegate) por sí misma.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Si vas a migrar desde una base de código de C++/CX en la que los eventos y delegados se usan internamente (no en los archivos binarios), **winrt::delegate** te ayudará a replicar dicho patrón en C++/WinRT.

## <a name="design-guidelines"></a>Directrices de diseño

Te recomendamos que pases eventos, no delegados, como parámetros de función. La función **add** de [**winrt::event**](/uwp/cpp-ref-for-winrt/event) es la única excepción, porque tienes que pasar un delegado en ese caso. La razón de esta directriz es que los delegados pueden adoptar diferentes formas en los diferentes lenguajes de Windows Runtime (en términos de si son compatibles con el registro de uno o varios clientes). Los eventos, con su modelo de suscriptor múltiple, constituyen una opción mucho más coherente y predecible.

La firma de un delegado de controlador de eventos debe constar de dos parámetros: *sender* (**IInspectable**) y *args* (algún tipo de argumento de evento, por ejemplo [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Observa que estas instrucciones no se aplican necesariamente si vas a diseñar una API interna. Sin embargo, las API internas terminan por convertirse en públicas con el tiempo.

## <a name="related-topics"></a>Temas relacionados
* [Crear API con C++/WinRT](author-apis.md)
* [Consumir API con C++/WinRT](consume-apis.md)
* [Control de eventos mediante delegados en C++/WinRT](handle-events.md)
