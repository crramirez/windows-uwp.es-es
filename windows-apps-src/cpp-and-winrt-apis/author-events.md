---
author: stevewhims
description: Este tema muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que genera eventos. También muestra una aplicación que consume el componente y controla los eventos.
title: Crear eventos en C++/WinRT
ms.author: stwhi
ms.date: 07/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, autor, evento
ms.localizationpriority: medium
ms.openlocfilehash: 82239436acfe82bf99cd1e665cca14592bbcef74
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "5130423"
---
# <a name="author-events-in-cwinrt"></a>Crear eventos en C++/WinRT

Este tema muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que representa una cuenta bancaria, lo cual genera un evento cuando su saldo pasa a estar en débito. También muestra una aplicación principal que consume la clase en tiempo de ejecución de la cuenta bancaria, llama a una función para ajustar el saldo y controla cualquier evento que surja.

> [!NOTE]
> Para obtener información sobre cómo instalar y usar el [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) extensión de Visual Studio (VSIX) (que proporciona soporte para plantillas de proyecto, así como C++ / WinRT MSBuild propiedades y destinos) consulta [soporte de Visual Studio para C++ / WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT ](author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Crea un componente de Windows Runtime (BankAccountWRC)

Comienza creando un proyecto nuevo en Microsoft Visual Studio Crear un **Visual C++** > **Windows Universal** > **componente de Windows Runtime (C++ / WinRT)** del proyecto y asígnale *BankAccountWRC* (para "componente cuenta bancaria Windows en tiempo de ejecución").

El proyecto recién creado contiene un archivo denominado `Class.idl`. Cambiar el nombre de ese archivo `BankAccount.idl` (cambiar el nombre de la `.idl` archivo cambia automáticamente el nombre dependiente `.h` y `.cpp` archivos, demasiado). Reemplaza el contenido del `BankAccount.idl` con la siguiente lista.

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

Guarda el archivo. No se compilará el proyecto para finalización en ese momento, pero compilar ahora es algo útil ya que genera los archivos de código de origen en el que se implementa la clase en tiempo de ejecución de **BankAccount** . Por lo tanto, seguir adelante y generar ahora (los errores de compilación que puede esperar ver en esta etapa tienen que ver con `Class.h` y `Class.g.h` no se encuentra). Durante el proceso de compilación, la `midl.exe` herramienta se ejecutará para crear el archivo de metadatos de tu componente en tiempo de ejecución de Windows (que es `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`). Después se ejecutará la herramienta `cppwinrt.exe` (con la opción `-component`) para generar archivos de código fuente y ayudarte a crear tu componente. Estos archivos incluyen códigos auxiliares para que puedas empezar a implementar la clase en tiempo de ejecución de **BankAccount** que se declara en el archivo IDL. Estos archivos de código auxiliar son `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` y `BankAccount.cpp`

En el Explorador de archivos, copia los archivos de código auxiliar `BankAccount.h` y `BankAccount.cpp` desde la carpeta `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` a la carpeta que contiene los archivos de proyecto, que es `\BankAccountWRC\BankAccountWRC\`y reemplaza los archivos en el destino. Ahora, vamos a abrir `BankAccount.h` y `BankAccount.cpp` e implementar nuestra clase en tiempo de ejecución. En `BankAccount.h`, agrega dos miembros privados a la implementación (*no* la implementación de fábrica) de BankAccount.

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

Como puedes ver anteriormente, el evento se implementa en términos de la plantilla de estructura [**winrt::event**](/uwp/cpp-ref-for-winrt/event) , parametrizada por un tipo de delegado particular.

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

Si las advertencias te impiden compilar, a continuación, ya sea resolverlos o establecer la propiedad de proyecto **C o C++** > **General** > **Tratar advertencias como errores** a **No (/ WX-)** y vuelve a compilar el proyecto.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Crea una aplicación principal (BankAccountCoreApp) para probar el componente de Windows Runtime.

Ahora crea un nuevo proyecto (en tu solución `BankAccountWRC` o en una nueva). Crear un **Visual C++** > **Windows Universal** > **Core App (C++ / WinRT)** del proyecto y el nombre *BankAccountCoreApp*.

Agrega una referencia y busca `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (o agrega una referencia de proyecto a proyecto, si los dos proyectos están en la misma solución). Haz clic en **Agregar** y después en **Aceptar**. Ahora compila BankAccountCoreApp. En el improbable caso de que aparece un error que el archivo de carga `readme.txt` no existe, excluya este archivo desde el proyecto de componente de Windows Runtime, generarlo y después volver a compilar BankAccountCoreApp.

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

Cada vez que hagas clic en la ventana, restarás 1 del saldo de la cuenta bancaria. Para demostrar que se genera el evento según lo esperado, coloca un punto de interrupción dentro de la expresión lambda que se controla el evento de **AccountIsInDebit** , ejecuta la aplicación y haga clic dentro de la ventana.

## <a name="parameterized-delegates-and-simple-signals-across-an-abi"></a>Delegados con parámetros y señales simples, a través de una ABI

Si el evento debe ser accesible a través de una interfaz binaria de aplicaciones (ABI)&mdash;como entre un componente y su aplicación de consumo&mdash;, a continuación, el evento debe usar un tipo de delegado en tiempo de ejecución de Windows. El ejemplo anterior, usa el tipo de delegado [**Windows::Foundation::EventHandler\ < T\ >**](/uwp/api/windows.foundation.eventhandler) Windows Runtime. [**TypedEventHandler\ < TSender, TResult\ >**](/uwp/api/windows.foundation.eventhandler) es otro ejemplo de un tipo de delegado en tiempo de ejecución de Windows.

Los parámetros de tipo para esos tipos de dos delegado tienen cruzar la ABI, por lo que los parámetros de tipo deben ser tipos de Windows Runtime, demasiado. Que incluye las clases en tiempo de ejecución de primer y terceros, así como los tipos primitivos como cadenas y números. El compilador te ayuda a con un error de "*debe ser de tipo WinRT*" Si olvida esa restricción.

Si no tienes que pasar los parámetros o los argumentos con el evento, a continuación, puedes definir su propio tipo de delegado simple de Windows Runtime. El siguiente ejemplo muestra una versión más sencilla de la clase en tiempo de ejecución de **BankAccount** . Declara un tipo de delegado denominado **SignalDelegate** y, a continuación, utiliza para generar un evento de tipo de señal en lugar de un evento con un parámetro.

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

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>Delegados con parámetros, señales simples y las devoluciones de llamada dentro de un proyecto

Si el evento se usa solo internamente dentro de su C++ / WinRT del proyecto (no a través de archivos binarios), a continuación, seguir usan la plantilla de estructura [**winrt::event**](/uwp/cpp-ref-for-winrt/event) , pero se parametrizar con C++ / que no sea de Windows Runtime [**winrt:: Delegate WinRT&lt;… T&gt; **](/uwp/cpp-ref-for-winrt/delegate) plantilla de estructura, que es un delegado eficaz, recuento de referencias. Admite cualquier número de parámetros y que no están limitados a los tipos de Windows Runtime.

En primer lugar, el siguiente ejemplo muestra a un delegado firma que no toma los parámetros (esencialmente una señal simple) y, a continuación, que toma una cadena.

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

Ten en cuenta cómo puede agregar al evento tantas delegados de suscripción que deseas. Sin embargo, existe cierta sobrecarga asociada a un evento. Si todo lo que necesitas es una devolución de llamada simple con un solo delegado suscripción, puedes usar [**winrt:: Delegate&lt;… T&gt; **](/uwp/cpp-ref-for-winrt/delegate) en su propio.

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

Si vas a portar desde C++ / CX codebase donde se usan internamente eventos y delegados dentro de un proyecto y luego **winrt:: Delegate** te ayudará a replicar dicho patrón en C++ / WinRT.

## <a name="design-guidelines"></a>Directrices de diseño

Te recomendamos que pasas eventos y no delegados, como parámetros de función. La función **Agregue** de [**winrt::event**](/uwp/cpp-ref-for-winrt/event) es la única excepción, porque debe pasar a un delegado en ese caso. El motivo para esta directriz es porque los delegados pueden tener diferentes formas en diferentes idiomas de Windows Runtime (en términos de si admiten el registro de un cliente o varias). Eventos, con su modelo de suscriptor múltiple, constituyen una opción mucho más predecible y coherente.

La firma de un delegado de controlador de eventos debería constan de dos parámetros: el *remitente* (**IInspectable**) y *args* (algunos eventos tipo de argumento, por ejemplo [**RoutedEventArgs**](/uwp/api/windows.ui.xaml.routedeventargs)).

Ten en cuenta que estas instrucciones no se aplican necesariamente si estás diseñando una API interna. Aunque API internas suelen ser públicas con el tiempo.

## <a name="related-topics"></a>Artículos relacionados
* [Crear API con C++/WinRT](author-apis.md)
* [Consumir API con C++/WinRT](consume-apis.md)
* [Controlar eventos usando delegados en C ++/WinRT](handle-events.md)
