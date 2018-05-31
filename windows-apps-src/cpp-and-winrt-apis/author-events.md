---
author: stevewhims
description: Este tema muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que genera eventos. También muestra una aplicación que consume el componente y controla los eventos.
title: Crear eventos en C++/WinRT
ms.author: stwhi
ms.date: 04/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, autor, evento
ms.localizationpriority: medium
ms.openlocfilehash: b7574f1a3406dae665ced80294f7bc1cf91aeb8c
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832029"
---
# <a name="author-events-in-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Crear eventos en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.**

Este tema muestra cómo crear un componente de Windows Runtime con una clase en tiempo de ejecución que representa una cuenta bancaria, lo cual genera un evento cuando su saldo pasa a estar en débito. También muestra una aplicación principal que consume la clase en tiempo de ejecución de la cuenta bancaria, llama a una función para ajustar el saldo y controla cualquier evento que surja.

> [!NOTE]
> Para obtener información sobre la actual disponibilidad de la extensión de Visual Studio (VSIX) de C++/WinRT (la cual ofrece soporte para plantillas de proyectos, así como propiedades y destinos de MSBBuild C++/WinRT), consulta el [Soporte de Visual Studio para C++/WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT ](author-apis.md).

## <a name="windowsfoundationeventhandlerlttgt-and-typedeventhandlerlttgt"></a>Windows::Foundation::EventHandler&lt;T&gt; y TypedEventHandler&lt;T&gt;
Si quieres generar un evento desde una clase en tiempo de ejecución implementado en un componente de Windows Runtime, debes usar [**Windows::Foundation::EventHandler**](/uwp/api/windows.foundation.eventhandler) o [**TypedEventHandler**](/uwp/api/windows.foundation.eventhandler) para el tipo de delegado de tu evento. Los parámetros de tipo deben ser tipos de Windows Runtime, de este modo se permiten los tipos primitivos y las clases en tiempo de ejecución de terceros.

El compilador te ayudará con el error"*debe ser de tipo WinRT*" si olvida esta restricción.

## <a name="winrtdelegatelttgt"></a>winrt::delegate&lt;...T&gt;
Si quieres generar un evento desde un tipo C++ (creado y consumido en el mismo proyecto), puedes usar el **winrt::delegate&lt;... T&gt;** de C++/WinRT para el tipo de delegado de tu evento. En tal caso, el parámetro de tipo no necesita ser un tipo de Windows Runtime.

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Crea un componente de Windows Runtime (BankAccountWRC)
Comienza creando un proyecto nuevo en Microsoft Visual Studio Crea un proyecto **componente Visual C++ de Windows Runtime (C++/WinRT)** y asígnale el nombre *BankAccountWRC* (para "componente cuenta bancaria de Windows Runtime").

El proyecto recién creado contiene un archivo denominado `Class.idl`. Cambia el nombre de este archivo `BankAccountWRC.idl` de modo que cuando compiles, el archivo de metadatos de Windows Runtime de tu componente sea denominado para el propio componente. En `BankAccountWRC.idl`, define tu interfaz al igual que en la siguiente lista.

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

Guarda el archivo y compila el proyecto. La compilación no se realizará correctamente todavía. Pero durante el proceso de compilación, la herramienta `midl.exe` se ejecutará para crear el archivo de metadatos de Windows Runtime de tu componente, el cual es `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`. Después se ejecutará la herramienta `cppwinrt.exe` (con la opción `-component`) para generar archivos de código fuente y ayudarte a crear tu componente. Estos archivos incluyen códigos auxiliares para que puedas empezar a implementar la clase en tiempo de ejecución `BankAccount` que declaraste en tu archivo IDL. Estos archivos de código auxiliar son `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` y `BankAccount.cpp`

Copia los archivos de código auxiliar`BankAccount.h``BankAccount.cpp` de `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` a la carpeta del proyecto, que es `\BankAccountWRC\BankAccountWRC\`. En el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Haz clic con el botón derecho en los archivos de código auxiliar que has copiado y haz clic en **Incluir en el proyecto**. Además, haz clic con el botón derecho en `Class.h`y `Class.cpp`, y luego en **Excluir del proyecto **.

Ahora, vamos a abrir `BankAccount.h` y `BankAccount.cpp` e implementar nuestra clase en tiempo de ejecución. En `BankAccount.h`, agrega dos miembros privados a la implementación (*no* la implementación de fábrica) de BankAccount.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        event<Windows::Foundation::EventHandler<float>> accountIsInDebitEvent;
        float balance{ 0.f };
    };
}
...
```

Implementa las funciones en `BankAccount.cpp` de este modo.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    event_token BankAccount::AccountIsInDebit(Windows::Foundation::EventHandler<float> const& handler)
    {
        return accountIsInDebitEvent.add(handler);
    }

    void BankAccount::AccountIsInDebit(event_token const& token)
    {
        accountIsInDebitEvent.remove(token);
    }

    void BankAccount::AdjustBalance(float value)
    {
        balance += value;
        if (balance < 0.f) accountIsInDebitEvent(*this, balance);
    }
}
```

La implementación de la función **AdjustBalance** genera el evento **AccountIsInDebit** si el saldo pasa a ser negativo.

Si las advertencias te impiden compilar, establece la propiedad de proyecto **C/C++** > **General** > **Tratar advertencias como errores** a **No (/WX-)** y vuelve a compilar el proyecto.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Crea una aplicación principal (BankAccountCoreApp) para probar el componente de Windows Runtime.
Ahora crea un nuevo proyecto (en tu solución `BankAccountWRC` o en una nueva). Crea un proyecto **Visual C++ Core App (C++/WinRT)** y asígnale el nombre *BankAccountCoreApp *.

Agrega una referencia y busca `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`. Haz clic en **Agregar** y después en **Aceptar**. Ahora compila BankAccountCoreApp. Si aparece un error indicando que el archivo de carga `readme.txt` no existe, excluya este archivo del proyecto componente de Windows Runtime, vuelve a compilarlo y luego compila de nuevo BankAccountCoreApp.

Durante el proceso de compilación, la herramienta `cppwinrt.exe` se ejecutará para procesar el archivo `.winmd` al que se hace referencia en los archivos de código fuente que contienen tipos proyectados para ayudarte a consumir tu componente. El encabezado de los tipos proyectados para las clases en tiempo de ejecución de tu componente&mdash;denominado `BankAccountWRC.h`&mdash;se genera en la carpeta `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Incluye dicho encabezado en `App.cpp`.

```cppwinrt
#include <winrt/BankAccountWRC.h>
```

También en `App.cpp`, agrega el siguiente código para crear una instancia de un BankAccount (usando el constructor predeterminado del tipo proyectado), registra un controlador de eventos y, a continuación, haz que la cuenta pase a estar en débito.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount bankAccount;
    event_token eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        eventToken = bankAccount.AccountIsInDebit([](const auto &, float balance)
        {
            assert(balance < 0.f);
        });
    }
    ...

    void Uninitialize()
    {
        bankAccount.AccountIsInDebit(eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        bankAccount.AdjustBalance(-1.f);
        ...
    }
    ...
};
```

Cada vez que hagas clic en la ventana, restarás 1 del saldo de la cuenta bancaria. Para demostrar que se genera el evento según lo esperado, coloca un punto de interrupción en la expresión lambda, ejecuta la aplicación y haz clic dentro de la ventana.

## <a name="related-topics"></a>Artículos relacionados
* [Crear API con C++/WinRT](author-apis.md)
* [Consumir API con C++/WinRT](consume-apis.md)
* [Controlar eventos usando delegados en C ++/WinRT](handle-events.md)
