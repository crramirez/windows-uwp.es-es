---
description: Este tema muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que genera eventos. También muestra una aplicación que consume el componente y controla los eventos.
title: Crear eventos en C++/WinRT
ms.date: 07/18/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, autor, evento
ms.localizationpriority: medium
ms.openlocfilehash: ace1c276b878d07f5750483740dfe90ed8cb6211
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644490"
---
# <a name="author-events-in-cwinrt"></a>Crear eventos en C++/WinRT

Este tema muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que representa una cuenta bancaria, lo cual genera un evento cuando su saldo pasa a estar en débito. También muestra una aplicación principal que consume la clase en tiempo de ejecución de la cuenta bancaria, llama a una función para ajustar el saldo y controla cualquier evento que surja.

> [!NOTE]
> Para obtener información sobre cómo instalar y usar el [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) extensión de Visual Studio (VSIX) (que proporciona compatibilidad con plantillas de proyecto) vea [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT ](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Crea un componente de Windows Runtime (BankAccountWRC)

Comienza creando un proyecto nuevo en Microsoft Visual Studio Crear un **Visual C++** > **Windows Universal** > **componente en tiempo de ejecución de Windows (C++ / c++ / WinRT)** del proyecto y asígnele el nombre  *BankAccountWRC* (para "cuenta bancaria componente en tiempo de ejecución de Windows").

El proyecto recién creado contiene un archivo denominado `Class.idl`. Cambiar el nombre de ese archivo `BankAccount.idl` (cambiar el nombre de la `.idl` archivo cambia automáticamente el nombre dependiente `.h` y `.cpp` archivos, demasiado). Reemplace el contenido de `BankAccount.idl` con la lista siguiente.

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

Guarda el archivo. No compilará el proyecto hasta su finalización en este momento, pero crear ahora es algo útil hacer porque genera los archivos de código fuente en el que implementará el **BankAccount** clase en tiempo de ejecución. Así que ahora la compilación (los errores de compilación puede esperar ver en esta fase tienen que ver con `Class.h` y `Class.g.h` no haberse encontrado). Durante el proceso de compilación, el `midl.exe` se ejecuta la herramienta para crear el archivo de metadatos del componente en tiempo de ejecución de Windows (que es `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Después se ejecutará la herramienta `cppwinrt.exe` (con la opción `-component`) para generar archivos de código fuente y ayudarte a crear tu componente. Estos archivos incluyen códigos auxiliares para ayudarle a comenzar a implementar el **BankAccount** clase en tiempo de ejecución que declaró en el archivo IDL. Estos archivos de código auxiliar son `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` y `BankAccount.cpp`

Haga clic en el nodo del proyecto y haga clic en **Abrir carpeta en el Explorador de archivos**. Se abre la carpeta del proyecto en el Explorador de archivos. Allí, copie los archivos de código auxiliar `BankAccount.h` y `BankAccount.cpp` desde la carpeta `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` y en la carpeta que contiene los archivos de proyecto, que es `\BankAccountWRC\BankAccountWRC\`y reemplazar los archivos en el destino. Ahora, vamos a abrir `BankAccount.h` y `BankAccount.cpp` e implementar nuestra clase en tiempo de ejecución. En `BankAccount.h`, agrega dos miembros privados a la implementación (*no* la implementación de fábrica) de BankAccount.

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

Como puede ver anteriormente, el evento se implementa en términos de la [ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) plantilla struct, parametrizado por un tipo determinado de delegado.

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

    void BankAccount::AccountIsInDebit(winrt::event_token const& token)
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

No tienes que implementar la sobrecarga del revocador del evento (para obtener más información, consulta [Revocar un delegado registrado](handle-events.md#revoke-a-registered-delegate))&mdash;de las que se ocupa la proyección de C++/WinRT por ti. Las demás sobrecargas no se incorporan en la proyección, con el fin de ofrecerte la flexibilidad para implementarlas de forma óptima para tu escenario. Una llamada a [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) y [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) como esta es un configuración predeterminada eficaz y segura para subprocesos/concurrencia. Pero si tienes un gran número de eventos, es posible que no desees un campo de evento para cada uno, sino optar en su lugar por algún tipo de implementación dispersa.

También puedes ver por encima la implementación de la función **AdjustBalance** genera el evento **AccountIsInDebit** si el saldo pasa a ser negativo.

Si las advertencias le impiden edificio, después, resolverlos o establezca la propiedad de proyecto **C o C++** > **General** > **tratar advertencias como errores** a **No (/ WX-)** y compile el proyecto nuevo.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Crea una aplicación principal (BankAccountCoreApp) para probar el componente de Windows Runtime.

Ahora crea un nuevo proyecto (en tu solución `BankAccountWRC` o en una nueva). Crear un **Visual C++** > **Windows Universal** > **Core App (C++ / c++ / WinRT)** del proyecto y asígnele el nombre *BankAccountCoreApp* .

Agregue una referencia y vaya a `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (o agregue una referencia de proyecto a proyecto, si los dos proyectos están en la misma solución). Haz clic en **Agregar** y después en **Aceptar**. Ahora compila BankAccountCoreApp. En el caso poco probable que vea un error que el archivo de carga `readme.txt` no existe, excluya el proyecto de componente de Windows en tiempo de ejecución en ese archivo, volver a generarlo y luego volver a generar BankAccountCoreApp.

Durante el proceso de compilación, la herramienta `cppwinrt.exe` se ejecutará para procesar el archivo `.winmd` al que se hace referencia en los archivos de código fuente que contienen tipos proyectados para ayudarte a consumir tu componente. El encabezado de los tipos proyectados para las clases en tiempo de ejecución de tu componente&mdash;denominado `BankAccountWRC.h`&mdash;se genera en la carpeta `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Incluye dicho encabezado en `App.cpp`.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

También en `App.cpp`, agrega el siguiente código para crear una instancia de un BankAccount (usando el constructor predeterminado del tipo proyectado), registra un controlador de eventos y, a continuación, haz que la cuenta pase a estar en débito.

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
            WINRT_ASSERT(balance < 0.f);
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

Cada vez que hagas clic en la ventana, restarás 1 del saldo de la cuenta bancaria. Para demostrar que se provoca el evento según lo previsto, coloque un punto de interrupción dentro de la expresión lambda que lleva a cabo la **AccountIsInDebit** eventos, ejecute la aplicación y haga clic dentro de la ventana.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Delegados con parámetros y las señales de simple a través de una ABI

Si el evento debe ser accesible a través de una interfaz binaria de aplicación (ABI)&mdash;por ejemplo, entre un componente y su aplicación de consumo&mdash;, a continuación, el evento debe usar un tipo de delegado en tiempo de ejecución de Windows. El ejemplo anterior se usa el [ **Windows::Foundation::EventHandler\<T\>**  ](/uwp/api/windows.foundation.eventhandler) tipo de delegado en tiempo de ejecución de Windows. [**TypedEventHandler\<TSender, TResult\>**  ](/uwp/api/windows.foundation.eventhandler) es otro ejemplo de un tipo de delegado en tiempo de ejecución de Windows.

Los parámetros de tipo para esos tipos de dos delegado deben cruzar la ABI, por lo que los parámetros de tipo deben ser tipos en tiempo de ejecución de Windows, demasiado. Incluye las clases en tiempo de ejecución de primera y de otros fabricantes, así como los tipos primitivos como cadenas y números. El compilador le ayuda con un "*debe ser de tipo de WinRT*" error si se olvida de esa restricción.

Si no tiene que pasar los parámetros o los argumentos con el evento, a continuación, puede definir su propio tipo de delegado en tiempo de ejecución de Windows simple. El ejemplo siguiente muestra una versión más sencilla de la **BankAccount** clase en tiempo de ejecución. Declara un tipo de delegado denominado **SignalDelegate** y, a continuación, usa para generar un evento de tipo de señal en lugar de un evento con un parámetro.

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Delegados con parámetros, señales simple y las devoluciones de llamada dentro de un proyecto

Si el evento se usa internamente solo en C++ / c++ / WinRT de proyecto (no a través de los archivos binarios), a continuación, seguir usando el [ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) struct plantilla, pero puede parametrizar con C++ / c++ / de WinRT -Windows-Runtime [ **winrt::delegate&lt;... T&gt;**  ](/uwp/cpp-ref-for-winrt/delegate) plantilla de struct, que es un delegado de recuento de referencias y eficaz. Admite cualquier número de parámetros y no se limitan a los tipos en tiempo de ejecución de Windows.

El ejemplo siguiente muestra primero un delegado de firma que no toma ningún parámetro (esencialmente una señal simple) y, a continuación, una que toma una cadena.

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

Tenga en cuenta cómo puede agregar al evento delegados suscripción tantas como desee. Sin embargo, hay alguna sobrecarga asociada a un evento. Si todo lo que necesita es una devolución de llamada simple con un solo delegado suscripción, puede usar [ **winrt::delegate&lt;... T&gt;**  ](/uwp/cpp-ref-for-winrt/delegate) por sí mismo.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Si está migrando desde C++ / c++ / CX codebase donde los eventos y delegados se usan internamente dentro de un proyecto, a continuación, **winrt::delegate** le ayudará a replicar ese patrón en C++ / c++ / WinRT.

## <a name="design-guidelines"></a>Directrices de diseño

Se recomienda que los eventos y los delegados, pasar como parámetros de función. El **agregar** función de [ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) es la única excepción, ya que debe pasar un delegado en ese caso. La razón para esta directriz es que los delegados pueden adoptar diferentes formas en los diferentes idiomas de Windows en tiempo de ejecución (en términos de si son compatibles con el registro de un cliente o varios). Eventos con su modelo de suscriptor múltiple, constituyen una opción mucho más coherente y predecible.

La firma de un delegado de controlador de eventos debe constar de dos parámetros: *remitente* (**IInspectable**), y *args* (algún tipo de argumento de evento, por ejemplo [ **RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Tenga en cuenta que estas instrucciones no se aplican necesariamente si va a diseñar una API interna. Aunque las API internas suelen ser públicas con el tiempo.

## <a name="related-topics"></a>Temas relacionados
* [Crear las API con C++ / c++ / WinRT](author-apis.md)
* [Consumo de API con C / c++ / WinRT](consume-apis.md)
* [Controlar eventos mediante el uso de delegados en C / c++ / WinRT](handle-events.md)
