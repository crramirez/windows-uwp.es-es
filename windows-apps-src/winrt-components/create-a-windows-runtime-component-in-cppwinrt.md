---
title: Componentes de Windows Runtime con C++/WinRT
description: En este tema se muestra cómo usar [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para crear y utilizar un componente de Windows Runtime &mdash; un componente al que se puede llamar desde una aplicación universal de Windows compilada con cualquier lenguaje de Windows Runtime.
ms.date: 07/06/2020
ms.topic: article
keywords: Windows 10, UWP, Windows, Runtime, componente, componentes, Windows Runtime componente, WRC, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: 25286260c4abd6686939393b3bf81df818879bf9
ms.sourcegitcommit: 21eb13a50402bf5442a5f0a4bf34800d1dc679c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2020
ms.locfileid: "90804755"
---
# <a name="windows-runtime-components-with-cwinrt"></a>Componentes de Windows Runtime con C++/WinRT

En este tema se muestra cómo usar [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para crear y utilizar un componente de Windows Runtime &mdash; un componente al que se puede llamar desde una aplicación universal de Windows compilada con cualquier lenguaje de Windows Runtime.

Hay varias razones para compilar un componente de Windows Runtime en C++/WinRT.
- Para disfrutar de las ventajas de rendimiento de C++ en operaciones complejas o de cálculo intensivo.
- Para reutilizar el código de C++ estándar que ya está escrito y probado.
- Para exponer la funcionalidad de Win32 en una aplicación Plataforma universal de Windows (UWP) escrita en, por ejemplo, C#.

En general, cuando se crea el componente/WinRT de C++, se pueden usar los tipos de la biblioteca estándar de C++ y los tipos integrados, excepto en el límite de la interfaz binaria de aplicación (ABI), donde se pasan los datos hacia y desde el código de otro `.winmd` paquete. En la ABI, use tipos de Windows Runtime. Además, en el código de C++/WinRT, use tipos como Delegate y Event para implementar eventos que se puedan generar desde su componente y que se controlen en otro lenguaje. Vea [c++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para obtener más información acerca de c++/WinRT.

En el resto de este tema se explica cómo crear un componente de Windows Runtime en C++/WinRT y, a continuación, cómo consumirlo desde una aplicación.

El Windows Runtime componente que se va a crear en este tema contiene una clase en tiempo de ejecución que representa una cuenta bancaria. En el tema también se muestra una aplicación principal que utiliza la clase de tiempo de ejecución de la cuenta bancaria y llama a una función para ajustar el saldo.

> [!NOTE]
> Para más información sobre cómo instalar y usar [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Visual Studio Extension (VSIX) y el paquete de NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación), consulta [Compatibilidad de Visual Studio para C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](../cpp-and-winrt-apis/consume-apis.md) y [Crear API con C++/WinRT ](../cpp-and-winrt-apis/author-apis.md).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Crear un componente de Windows Runtime (BankAccountWRC)

Para empezar, crea un proyecto en Microsoft Visual Studio. Crea un proyecto **Componente de Windows Runtime (C++/WinRT)** y asígnale el nombre *BankAccountWRC* (de "componente de cuenta bancaria de Windows Runtime"). Asegúrese de que la opción **Colocar la solución y el proyecto en el mismo directorio** esté desactivada. Elija como destino la versión más reciente disponible de manera general (es decir, no en versión preliminar) de Windows SDK. Asignar el nombre *BankAccountWRC* al proyecto simplificará la experiencia con el resto de los pasos de este tema. 

No compile aún el proyecto.

El proyecto recién creado contiene un archivo llamado `Class.idl`. En el Explorador de soluciones, cambie el nombre del archivo `BankAccount.idl` (al cambiar el nombre del archivo `.idl`, se cambia automáticamente el nombre de los archivos `.h` y `.cpp` dependientes). Reemplaza el contenido de `BankAccount.idl` por la lista siguiente.

> [!NOTE]
> No es necesario decir que no implemente software financiero de producción de esta manera. usamos `Single` en este ejemplo únicamente para mayor comodidad.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        void AdjustBalance(Single value);
    };
}
```

Guarde el archivo. El proyecto no se compilará totalmente en este momento, pero compilar ahora resulta útil porque genera los archivos de código fuente en el que implementarás la clase en tiempo de ejecución **BankAccount**. Así que puedes compilar ahora. En esta fase, verás errores de compilación porque no se han encontrado `Class.h` y `Class.g.h`.

Durante el proceso de compilación, la herramienta `midl.exe` se ejecuta para crear el archivo de metadatos de Windows Runtime de tu componente, que es `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`. Después se ejecutará la herramienta `cppwinrt.exe` (con la opción `-component`) para generar archivos de código fuente y ayudarte a crear tu componente. Estos archivos incluyen código auxiliar para que puedas empezar a implementar la clase en tiempo de ejecución **BankAccount** que declaraste en tu archivo IDL. Estos archivos de código auxiliar son `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` y `BankAccount.cpp`.

Haz clic con el botón derecho en el nodo del proyecto y haz clic en **Abrir carpeta en el Explorador de archivos**. Se abre la carpeta del proyecto en el Explorador de archivos. Allí, copia los archivos de código auxiliar `BankAccount.h` y `BankAccount.cpp` desde la carpeta `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` en la carpeta que contiene los archivos de proyecto, que es `\BankAccountWRC\BankAccountWRC\`, y reemplaza los archivos que hay en el destino. Ahora, vamos a abrir `BankAccount.h` y `BankAccount.cpp` e implementar nuestra clase en tiempo de ejecución. En `BankAccount.h` , agregue un nuevo miembro privado a la implementación (*no* a la implementación de fábrica) de **BankAccount**.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        float m_balance{ 0.f };
    };
}
...
```

En `BankAccount.cpp` , implemente el método **AdjustBalance** como se muestra en la lista siguiente.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
    }
}
```

También deberá eliminar el elemento `static_assert` de ambos archivos.

Si las advertencias te impiden compilar, resuélvelas o establece la propiedad de proyecto **C/C++**  > **General** > **Tratar advertencias como errores** en **No (/WX-)** y vuelve a compilar el proyecto.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Crear una aplicación principal (BankAccountCoreApp) para probar el componente de Windows Runtime

Ahora crea un proyecto nuevo (en la solución *BankAccountWRC* o en una nueva). Crea un proyecto **Core App (C++/WinRT)** y asígnale el nombre *BankAccountCoreApp*. Configure *BankAccountCoreApp* como proyecto de inicio si ambos proyectos se encuentran en la misma solución.

> [!NOTE]
> Como se mencionó anteriormente, el archivo de metadatos de Windows Runtime para el componente de Windows Runtime (a cuyo proyecto asignaste el nombre *BankAccountWRC*) se crea en la carpeta `\BankAccountWRC\Debug\BankAccountWRC\`. El primer segmento de esa ruta de acceso es el nombre de la carpeta que contiene el archivo de solución; el siguiente segmento es el subdirectorio de la que se denomina `Debug`; y el último segmento es el subdirectorio de la asignada para el componente de Windows Runtime. Si no asignaste el nombre *BankAccountWRC* al proyecto, el archivo de metadatos estará en la carpeta `\<YourProjectName>\Debug\<YourProjectName>\`.

Ahora, en el proyecto de la aplicación principal (*BankAccountCoreApp*), agrega una referencia y navega a `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (o agrega una referencia de proyecto a proyecto, si los dos proyectos están en la misma solución). Haz clic en **Agregar** y después en **Aceptar**. Ahora compila *BankAccountCoreApp*. En el improbable caso de que aparezca un error que indique que el archivo de carga útil `readme.txt` no existe, excluye este archivo del proyecto del componente de Windows Runtime, vuelve a compilarlo y, a continuación, compila de nuevo *BankAccountCoreApp*.

Durante el proceso de compilación, la herramienta `cppwinrt.exe` se ejecutará para procesar el archivo `.winmd` al que se hace referencia en los archivos de código fuente que contienen tipos proyectados para ayudarte a consumir tu componente. El encabezado de los tipos proyectados para las clases en tiempo de ejecución de tu componente&mdash;denominado `BankAccountWRC.h`&mdash;se genera en la carpeta `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\`.

Incluye dicho encabezado en `App.cpp`.

```cppwinrt
// App.cpp
...
#include <winrt/BankAccountWRC.h>
...
```

También en `App.cpp` , agregue el código siguiente para crear una instancia de un objeto **BankAccount** (mediante el constructor predeterminado del tipo previsto) y llamar a un método en el objeto de cuenta bancaria.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(1.f);
        ...
    }
    ...
};
```

Cada vez que haga clic en la ventana, incrementará el saldo del objeto de la cuenta bancaria. Puede establecer puntos de interrupción Si desea recorrer el código para confirmar que la aplicación realmente está llamando al componente de Windows Runtime.

## <a name="next-steps"></a>Pasos siguientes

Para agregar más funcionalidad, o nuevos tipos de Windows Runtime, al componente Windows Runtime de C++/WinRT, puede seguir los mismos patrones mostrados anteriormente. En primer lugar, use IDL para definir la funcionalidad que desea exponer. A continuación, compile el proyecto en Visual Studio para generar una implementación de código auxiliar. Y, a continuación, complete la implementación según corresponda. Los métodos, las propiedades y los eventos que se definen en IDL son visibles para la aplicación que utiliza el componente de Windows Runtime. Para obtener más información acerca de IDL, vea [Introducción a Lenguaje de definición de interfaz de Microsoft 3,0](/uwp/midl-3/intro).

Para obtener un ejemplo de cómo agregar un evento a su componente de Windows Runtime, vea [crear eventos en C++/WinRT](../cpp-and-winrt-apis/author-events.md).

## <a name="troubleshooting"></a>Solución de problemas

| Síntoma | Solución |
|---------|--------|
|En una aplicación/WinRT de C++, al consumir un [componente de Windows Runtime de C#](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic) que usa XAML, el compilador genera un error con el formato ""*MyNamespace_XamlTypeInfo ': no es un miembro de "WinRT:: myNameSpace*" ", &mdash; donde *myNameSpace* es el nombre del espacio de nombres del componente Windows Runtime. | En `pch.h` , en la aplicación de C++/WinRT de consumo, agregue `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>` &mdash; *myNameSpace* , según corresponda. |
