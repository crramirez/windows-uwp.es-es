---
title: Componentes de Windows Runtime con C++/WinRT
description: En este tema se muestra cómo usar [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para crear y utilizar un componente de Windows Runtime &mdash; un componente al que se puede llamar desde una aplicación universal de Windows compilada con cualquier lenguaje de Windows Runtime.
ms.date: 07/06/2020
ms.topic: article
keywords: Windows 10, UWP, Windows, Runtime, componente, componentes, Windows Runtime componente, WRC, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: 12fa7c499dd0203a489b5657f1e3e2634bdad8b1
ms.sourcegitcommit: a93a309a11cdc0931e2f3bf155c5fa54c23db7c3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2020
ms.locfileid: "91646298"
---
# <a name="windows-runtime-components-with-cwinrt"></a>Componentes de Windows Runtime con C++/WinRT

En este tema se muestra cómo usar [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para crear y utilizar un componente de Windows Runtime &mdash; un componente al que se puede llamar desde una aplicación universal de Windows compilada con cualquier lenguaje de Windows Runtime.

Hay varias razones para compilar un componente de Windows Runtime en C++/WinRT.
- Para disfrutar de las ventajas de rendimiento de C++ en operaciones complejas o de cálculo intensivo.
- Para reutilizar el código de C++ estándar que ya está escrito y probado.
- Para exponer la funcionalidad de Win32 en una aplicación Plataforma universal de Windows (UWP) escrita en, por ejemplo, C#.

En general, cuando se crea el componente/WinRT de C++, se pueden usar los tipos de la biblioteca estándar de C++ y los tipos integrados, excepto en el límite de la interfaz binaria de aplicación (ABI), donde se pasan los datos hacia y desde el código de otro `.winmd` paquete. En la ABI, use tipos de Windows Runtime. Además, en el código de C++/WinRT, use tipos como Delegate y Event para implementar eventos que se puedan generar desde su componente y que se controlen en otro lenguaje. Vea [c++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) para obtener más información acerca de c++/WinRT.

En el resto de este tema se explica cómo crear un componente de Windows Runtime en C++/WinRT y, a continuación, cómo consumirlo desde una aplicación.

El Windows Runtime componente que se va a crear en este tema contiene una clase en tiempo de ejecución que representa un termómetro. En el tema también se muestra una aplicación principal que utiliza la clase en tiempo de ejecución del termómetro y llama a una función para ajustar la temperatura.

> [!NOTE]
> Para más información sobre cómo instalar y usar [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Visual Studio Extension (VSIX) y el paquete de NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación), consulta [Compatibilidad de Visual Studio para C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](../cpp-and-winrt-apis/consume-apis.md) y [Crear API con C++/WinRT ](../cpp-and-winrt-apis/author-apis.md).

## <a name="create-a-windows-runtime-component-thermometerwrc"></a>Crear un componente de Windows Runtime (ThermometerWRC)

Para empezar, crea un proyecto en Microsoft Visual Studio. Cree un proyecto de **componente de Windows Runtime (C++/WinRT)** y asígnele el nombre *ThermometerWRC* (para "componente Windows Runtime termómetro"). Asegúrese de que la opción **Colocar la solución y el proyecto en el mismo directorio** esté desactivada. Elija como destino la versión más reciente disponible de manera general (es decir, no en versión preliminar) de Windows SDK. El nombre del proyecto *ThermometerWRC* le proporcionará la experiencia más sencilla con el resto de los pasos de este tema. 

No compile aún el proyecto.

El proyecto recién creado contiene un archivo llamado `Class.idl`. En el Explorador de soluciones, cambie el nombre del archivo `Thermometer.idl` (al cambiar el nombre del archivo `.idl`, se cambia automáticamente el nombre de los archivos `.h` y `.cpp` dependientes). Reemplaza el contenido de `Thermometer.idl` por la lista siguiente.

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
    };
}
```

Guarde el archivo. El proyecto no se compilará hasta su finalización en ese momento, pero crear ahora es algo útil porque genera los archivos de código fuente en los que se implementará la clase en tiempo de ejecución de **termómetro** . Así que puedes compilar ahora. En esta fase, verás errores de compilación porque no se han encontrado `Class.h` y `Class.g.h`.

Durante el proceso de compilación, la herramienta `midl.exe` se ejecuta para crear el archivo de metadatos de Windows Runtime de tu componente, que es `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd`. Después se ejecutará la herramienta `cppwinrt.exe` (con la opción `-component`) para generar archivos de código fuente y ayudarte a crear tu componente. Estos archivos incluyen códigos auxiliares para empezar a implementar la clase de tiempo de ejecución de **termómetro** que declaró en el IDL. Estos archivos de código auxiliar son `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` y `Thermometer.cpp`.

Haz clic con el botón derecho en el nodo del proyecto y haz clic en **Abrir carpeta en el Explorador de archivos**. Se abre la carpeta del proyecto en el Explorador de archivos. Allí, copia los archivos de código auxiliar `Thermometer.h` y `Thermometer.cpp` desde la carpeta `\ThermometerWRC\ThermometerWRC\Generated Files\sources\` en la carpeta que contiene los archivos de proyecto, que es `\ThermometerWRC\ThermometerWRC\`, y reemplaza los archivos que hay en el destino. Ahora, vamos a abrir `Thermometer.h` y `Thermometer.cpp` e implementar nuestra clase en tiempo de ejecución. En `Thermometer.h` , agregue un nuevo miembro privado a la implementación (*no* a la implementación de fábrica) del **termómetro**.

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

    private:
        float m_temperatureFahrenheit { 0.f };
    };
}
...
```

En `Thermometer.cpp` , implemente el método **AdjustTemperature** como se muestra en la lista siguiente.

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
    }
}
```

También deberá eliminar el elemento `static_assert` de ambos archivos.

Si las advertencias te impiden compilar, resuélvelas o establece la propiedad de proyecto **C/C++**  > **General** > **Tratar advertencias como errores** en **No (/WX-)** y vuelve a compilar el proyecto.

## <a name="create-a-core-app-thermometercoreapp-to-test-the-windows-runtime-component"></a>Creación de una aplicación principal (ThermometerCoreApp) para probar el componente Windows Runtime

Ahora cree un nuevo proyecto (ya sea en la solución *ThermometerWRC* o en uno nuevo). Cree un proyecto de **aplicación principal (C++/WinRT)** y asígnele el nombre *ThermometerCoreApp*. Establezca *ThermometerCoreApp* como el proyecto de inicio si los dos proyectos están en la misma solución.

> [!NOTE]
> Como se mencionó anteriormente, el archivo de metadatos de Windows Runtime para el componente Windows Runtime (cuyo proyecto llamado *ThermometerWRC*) se crea en la carpeta `\ThermometerWRC\Debug\ThermometerWRC\` . El primer segmento de esa ruta de acceso es el nombre de la carpeta que contiene el archivo de solución; el siguiente segmento es el subdirectorio de la que se denomina `Debug`; y el último segmento es el subdirectorio de la asignada para el componente de Windows Runtime. Si no ha indicado el nombre del proyecto *ThermometerWRC*, el archivo de metadatos estará en la carpeta `\<YourProjectName>\Debug\<YourProjectName>\` .

Ahora, en el proyecto de aplicación principal (*ThermometerCoreApp*), agregue una referencia y busque `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd` (o agregue una referencia de proyecto a proyecto, si los dos proyectos están en la misma solución). Haz clic en **Agregar** y después en **Aceptar**. Ahora compile *ThermometerCoreApp*. En el caso improbable de que vea un error que no existe el archivo de carga `readme.txt` , excluya el archivo del proyecto de componente de Windows Runtime, vuelva a compilarlo y, a continuación, vuelva a generar *ThermometerCoreApp*.

Durante el proceso de compilación, la herramienta `cppwinrt.exe` se ejecutará para procesar el archivo `.winmd` al que se hace referencia en los archivos de código fuente que contienen tipos proyectados para ayudarte a consumir tu componente. El encabezado de los tipos proyectados para las clases en tiempo de ejecución de tu componente&mdash;denominado `ThermometerWRC.h`&mdash;se genera en la carpeta `\ThermometerCoreApp\ThermometerCoreApp\Generated Files\winrt\`.

Incluye dicho encabezado en `App.cpp`.

```cppwinrt
// App.cpp
...
#include <winrt/ThermometerWRC.h>
...
```

También en `App.cpp` , agregue el código siguiente para crear una instancia de un objeto de **termómetro** (mediante el constructor predeterminado del tipo proyectado) y llamar a un método en el objeto de termómetro.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(1.f);
        ...
    }
    ...
};
```

Cada vez que se hace clic en la ventana, se incrementa la temperatura del objeto de termómetro. Puede establecer puntos de interrupción Si desea recorrer el código para confirmar que la aplicación realmente está llamando al componente de Windows Runtime.

## <a name="next-steps"></a>Pasos siguientes

Para agregar más funcionalidad, o nuevos tipos de Windows Runtime, al componente Windows Runtime de C++/WinRT, puede seguir los mismos patrones mostrados anteriormente. En primer lugar, use IDL para definir la funcionalidad que desea exponer. A continuación, compile el proyecto en Visual Studio para generar una implementación de código auxiliar. Y, a continuación, complete la implementación según corresponda. Los métodos, las propiedades y los eventos que se definen en IDL son visibles para la aplicación que utiliza el componente de Windows Runtime. Para obtener más información acerca de IDL, vea [Introducción a Lenguaje de definición de interfaz de Microsoft 3,0](/uwp/midl-3/intro).

Para obtener un ejemplo de cómo agregar un evento a su componente de Windows Runtime, vea [crear eventos en C++/WinRT](../cpp-and-winrt-apis/author-events.md).

## <a name="troubleshooting"></a>Solución de problemas

| Síntoma | Solución |
|---------|--------|
|En una aplicación de C++ o WinRT, al consumir un [componente de Windows Runtime para C#](./creating-windows-runtime-components-in-csharp-and-visual-basic.md) que usa XAML, el compilador genera un error con el formato " *'MyNamespace_XamlTypeInfo': no es miembro de 'winrt::MyNamespace'* "&mdash; donde *MyNamespace* es el nombre del espacio de nombres del componente de Windows Runtime. | En el archivo `pch.h` de la aplicación que consume C++ o WinRT, agregue `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>`&mdash; reemplace *MyNamespace* según corresponda. |
