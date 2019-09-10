---
description: Respuestas a preguntas que probablemente tengas acerca de la creación y el consumo de las API de Windows Runtime con C++/WinRT.
title: Preguntas más frecuentes sobre C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, frequently, asked, questions, faq
ms.localizationpriority: medium
ms.openlocfilehash: a8da69f0041c71ecfc7429cae2ed51eee0f87d5e
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393488"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>Preguntas más frecuentes sobre C++/WinRT
Respuestas a preguntas que probablemente tengas acerca de la creación y del consumo de las API de Windows Runtime con [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

> [!NOTE]
> Si tu pregunta es sobre un mensaje de error que has visto, consulta también el tema [Solución de problemas de C++/WinRT](troubleshooting.md).

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>¿Cómo redirijo el proyecto de C++/WinRT a una versión posterior del SDK de Windows?
Consulta [Procedimientos para redirigir el proyecto de C++/WinRT a una versión posterior del SDK de Windows](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk).

## <a name="why-wont-my-new-project-compile-now-that-ive-moved-to-cwinrt-20"></a>¿Por qué no se va a compilar mi nuevo proyecto, ahora que he cambiado a C++WinRT 2.0?
Para todo el conjunto de cambios (incluidos los cambios importantes), consulta [Noticias y cambios, en C++/WinRT 2.0](news.md#news-and-changes-in-cwinrt-20). Por ejemplo, si usas `for` basado en intervalo en una colección de Windows Runtime, deberás aplicar ahora `#include <winrt/Windows.Foundation.Collections.h>`.

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>¿Por qué no se va a compilar mi nuevo proyecto? Estoy usando Visual Studio 2017 (versión 15.8.0 o superior) y el SDK versión 17134
Si usas Visual Studio 2017 (versión 15.8.0 o superior) y te diriges al SDK de Windows versión 10.0.17134.0 (Windows 10, versión 1803), un proyecto recién creado de C++/WinRT puede generar un error al compilarse con el "*error C3861: 'from_abi': no se encontró el identificador*" y otros errores que se originan en *base.h*. La solución consiste en dirigirte a una versión posterior (más compatible) del SDK de Windows o establecer la propiedad del proyecto **C/C++**  > **Lenguaje** > **Modo de conformidad: No** (además, si **/permissive-** aparece en la propiedad del proyecto **C/C++**  > **Línea de comandos** en **Opciones adicionales**, elimínalo).

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>¿Cómo resuelvo el error de compilación "The C++/WinRT VSIX no longer provides project build support.  Please add a project reference to the Microsoft.Windows.CppWinRT Nuget package"? (VSIX de C++WinRT ya no proporciona compatibilidad con la compilación del proyecto. Agregue una referencia de proyecto al paquete NuGet Microsoft.Windows.CppWinRT).
Instala el paquete NuGet **Microsoft.Windows.CppWinRT** en el proyecto. Para más información, consulta [Versiones anteriores de la extensión VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>¿Cuáles son los requisitos para la Extensión de Visual Studio (VSIX) para C++/WinRT?
Para la versión 1.0.190128.4 de la extensión de VSIX y posteriores, consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package). Para otras versiones, consulta [Versiones anteriores de la extensión VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="whats-a-runtime-class"></a>¿Qué es una *clase en tiempo de ejecución*?
Una clase en tiempo de ejecución es un tipo que puede activarse y consumirse a través de interfaces COM modernas, normalmente a través de límites de archivos ejecutables. Sin embargo, una clase en tiempo de ejecución también puede usarse dentro de la unidad de compilación que la implementa. Al declarar una clase en tiempo de ejecución en el lenguaje de definición de interfaz (IDL), puedes implementarla en C++ estándar con C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>¿Qué significa *tipo proyectado* y *tipo de implementación*?
Si solo vas a *consumir* una clase de Windows Runtime (clase en tiempo de ejecución), tratarás exclusivamente con *tipos proyectados*. C++/WinRT es una *proyección de lenguaje*, de modo que los tipos proyectados forman parte de la superficie de Windows Runtime que se *proyecta* en C++ con C++/WinRT. Para más información, consulta [Consumir API con C++/WinRT](consume-apis.md).

El *tipo de implementación* contiene la implementación de una clase en tiempo de ejecución, por lo que solo está disponible en el proyecto que implementa la clase en tiempo de ejecución. Cuando estés trabajando en un proyecto que implemente clases en tiempo de ejecución (un proyecto de componente de Windows Runtime o un proyecto que use interfaz de usuario XAML), es importante que te sientas cómodo con la distinción entre tu tipo de implementación para una clase en tiempo de ejecución y el tipo proyectado que representa la clase en tiempo de ejecución proyectada en C++/WinRT. Para más información, consulta [Crear API con C++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>¿Tengo que declarar un constructor en el archivo IDL de mi clase en tiempo de ejecución?
Solo si la clase en tiempo de ejecución se ha diseñado para consumirse desde fuera de su unidad de compilación de implementación (es un componente de Windows Runtime destinado al consumo general por aplicaciones cliente de Windows Runtime). Para una información más completa sobre el propósito y las consecuencias de declarar constructores en IDL, consulta [Constructores de clases en tiempo de ejecución](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>¿Por qué el enlazador me da un "error LNK2019: símbolo externo sin resolver"?
Si el símbolo no resuelto es una API de los encabezados de espacio de nombres de Windows para la proyección de C++/WinRT (en el espacio de nombres **winrt**), la API se declara más adelante en un encabezado que hayas incluido, pero su definición está en un encabezado que todavía no has incluido. Incluye el encabezado nombrado para el espacio de nombres de la API y vuelve a compilar. Para más información, consulta [Encabezados de proyección de C++/WinRT](consume-apis.md#cwinrt-projection-headers).

Si el símbolo no resuelto es una función libre de Windows Runtime, como [RoInitialize](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roinitialize), tendrás que vincular explícitamente la biblioteca paraguas [WindowsApp.lib](/uwp/win32-and-com/win32-apis) en el proyecto. La proyección de C++/WinRT depende de algunas de estas funciones y puntos de entrada libres (no miembros). Si usas una de las plantillas de proyecto de la [Extensión de Visual Studio (VSIX) para C++/WinRT](https://aka.ms/cppwinrt/vsix) para la aplicación, `WindowsApp.lib` se vincula automáticamente. De lo contrario, puedes usar la configuración de vínculo del proyecto para incluirla o hacerlo en el código fuente.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Es importante que resuelvas los errores del enlazador que puedas mediante la vinculación de **WindowsApp.lib** en lugar de una biblioteca de vínculos estáticos alternativa, en caso contrario, la aplicación no pasará las pruebas del [Kit de certificación de aplicaciones de Windows](../debug-test-perf/windows-app-certification-kit.md) usadas por Visual Studio y Microsoft Store para validar los envíos (es decir que no será posible que tu aplicación se ingiera correctamente en Microsoft Store).

## <a name="why-am-i-getting-a-class-not-registered-exception"></a>¿Por qué se muestra la excepción "clase no registrada"?

En este caso, el síntoma es que &mdash;al construir una clase en tiempo de ejecución o acceder a un miembro estático&mdash;, aparece una excepción en tiempo de ejecución con un valor HRESULT de REGDB_E_CLASSNOTREGISTERED.

Una causa de este error puede ser que no se puede cargar el componente de Windows Runtime. Asegúrate de que el archivo de metadatos (`.winmd`) de Windows Runtime del componente tenga el mismo nombre que el binario del componente (el `.dll`), que también es el nombre del proyecto y el nombre del espacio de nombres raíz. Asegúrate también de que el proceso de compilación haya copiado correctamente los metadatos de Windows Runtime y el binario en la carpeta `Appx` de la aplicación de consumo. Y comprueba que el `AppxManifest.xml` de la aplicación de consumo (también en la carpeta `Appx`) contenga un elemento **&lt;InProcessServer&gt;** que declare correctamente la clase activable y el nombre del binario.

### <a name="uniform-construction"></a>Construcción uniforme

Este error también puede ocurrir si intentas crear una instancia de una clase en tiempo de ejecución implementada localmente a través de cualquiera de los constructores del tipo proyectado (distinto de su constructor **std::nullptr_t**). Para ello, necesitarás la característica C++/WinRT 2.0 que suele denominarse construcción uniforme. Si quieres participar en esa característica, consulta [Participación en la construcción uniforme y acceso de implementación directa](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access) para obtener más información y ejemplos de código.

Si buscas una forma de crear una instancia de tus clases en tiempo de ejecución implementadas localmente que *no* requiera la construcción uniforme, consulta [Controles de XAML; enlazar a una propiedad de C++/WinRT](binding-property.md).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>¿Debo implementar [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)? Y, si es así, ¿cómo?
Si tienes una clase en tiempo de ejecución que libera recursos en su destructor, y si dicha clase en tiempo de ejecución se ha diseñado para consumirse desde fuera de su unidad de compilación de implementación (es un componente de Windows Runtime destinado al consumo general por aplicaciones cliente de Windows Runtime), te recomendamos que también implementes **IClosable** para poder admitir el consumo de tu clase en tiempo de ejecución con lenguajes que carecen de finalización determinista. Asegúrate de que tus recursos se liberan si se llama al constructor, a [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) o a ambos. Se puede llamar a **IClosable::Close** un número arbitrario de veces.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>¿Tengo que llamar a [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) en las clases en tiempo de ejecución que consumo?
**IClosable** existe para admitir los lenguajes que carecen de finalización determinista. Por lo tanto, no debes llamar a **IClosable::Close** desde C++/WinRT, excepto en casos muy raros que impliquen carreras de apagado o interbloqueos. Si vas a usar tipos **Windows.UI.Composition**, por ejemplo, puede que te encuentres con casos en los que quieras deshacerte de objetos en una secuencia definida, como una alternativa para permitir que la destrucción del contenedor de C++/WinRT haga el trabajo por ti.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>¿Puedo usar LLVM/Clang para compilar con C++/WinRT?
No admitimos la cadena de herramientas de LLVM y Clang para C++/WinRT, pero hacemos uso de ella internamente para validar la conformidad con los estándares de C++/WinRT. Por ejemplo, si quisieras emular lo que hacemos internamente, podrías intentar un experimento, como el que se describe a continuación.

Ve a [LLVM Download Page](https://releases.llvm.org/download.html) (Página de descarga de LLVM), busca **Download LLVM 6.0.0** (Descargar LLVM 6.0.0) > **Pre-Built Binaries** (Archivos binarios integrados previamente) y descarga **Clang for Windows (64-bit)** [Clang para Windows (64 bits)]. Durante la instalación, opta por agregar LLVM a la variable del sistema PATH para que puedas invocarlo desde un símbolo del sistema. Para los fines de este experimento, puedes hacer caso omiso de los errores de tipo "No se pudo encontrar el directorio de conjuntos de herramientas de MSBuild" o "Error de instalación de la integración de MSVC", si aparecen. Hay una gran variedad de formas para invocar a LLVM/Clang; el siguiente ejemplo constituye solo una de ellas.

```cmd
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include <winrt/Windows.Foundation.h>
#include <stdio.h>
#include <iostream>

using namespace winrt;

int main()
{
    winrt::init_apartment();
    Windows::Foundation::Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    std::wcout << rssFeedUri.Domain().c_str() << std::endl;
}

C:\ExperimentWithLLVMClang>clang-cl main.cpp /EHsc /I ..\.. -Xclang -std=c++17 -Xclang -Wno-delete-non-virtual-dtor -o app.exe

C:\ExperimentWithLLVMClang>app
windows.com
```

Dado que C++/WinRT usa características del estándar de C++17, deberás utilizar los marcadores del compilador que sean necesarios para obtener dicha compatibilidad; estos marcadores difieren entre compiladores.

Visual Studio es la herramienta de desarrollo que admitimos y recomendamos para C++/WinRT. Consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>¿Por qué no tiene la función de implementación generada para una propiedad de solo lectura el calificador `const`?
Cuando se declara una propiedad de solo lectura en [MIDL 3.0](/uwp/midl-3/), puedes esperar que la herramienta `cppwinrt.exe` te genere una función de implementación que es `const`-qualified (una función const trata el puntero *this* como const).

Sin duda, recomendamos usar const siempre que sea posible, pero la propia herramienta `cppwinrt.exe` no intenta razonar sobre qué funciones de implementación podrían ser const y cuáles no. Puedes elegir que cualquiera de las funciones de implementación sea const, como en este ejemplo.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

Puedes eliminar ese calificador `const` en **ToString** si decides que necesitas modificar algún estado de objeto en su implementación. Pero haz que cada una de las funciones de miembros sean const o non-const, y no ambos. En otras palabras, no sobrecargues una función de implementación en `const`.

Aparte de las funciones de implementación, otro lugar donde const aparece en la imagen es en las proyecciones de función de Windows Runtime. Observa este código.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

Para la llamada a **ToString** anterior, el comando **Ir a declaración** en Visual Studio muestra que la proyección de Windows Runtime **IStringable::ToString** en C++/WinRT tiene este aspecto.

```cppwinrt
winrt::hstring ToString() const;
```

Las funciones de la proyección son const independientemente de cómo decidas calificar su implementación. En segundo plano, la proyección llama a la interfaz binaria de aplicación (ABI), lo que equivale a una llamada mediante un puntero de interfaz COM. El único estado con el que el objeto **ToString** previsto interactúa es ese puntero de interfaz COM y, ciertamente, no tiene necesidad de modificar ese puntero, por lo que la función es const. Esto te ofrece la garantía de que no cambiará nada sobre la referencia **IStringable** a la que estás llamando y garantiza que puedas llamar a **ToString**, incluso con una referencia const a **IStringable**.

Entiende que estos ejemplos de `const` son detalles de implementación de proyecciones e implementaciones de C++/WinRT; constituyen la higiene de código para ayudarte. No existe nada como `const` en COM ni en ABI de Windows Runtime (para funciones de miembros).

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>¿Tienes recomendaciones para reducir el tamaño del código para los archivos binarios de C++/WinRT?
Cuando se trabaja con objetos de Windows Runtime, debes evitar el patrón de codificación que se muestra a continuación, ya que puede tener un impacto negativo en la aplicación al generar más código binario de lo necesario.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

En el mundo de Windows Runtime, el compilador no puede almacenar en caché el valor de `c()` o las interfaces para cada método al que se llama mediante un direccionamiento indirecto ('.'). A menos de que intervengas, esto da como resultado varias llamadas virtuales y sobrecarga de recuento de referencias. El patrón anterior podría generar fácilmente el doble de código de lo estrictamente necesario. En su lugar, es preferible que utilices el patrón que se muestra a continuación siempre que puedas. Genera mucho menos código y puede mejorar drásticamente el rendimiento del tiempo de ejecución.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

El patrón recomendado mostrado anteriormente no solo se aplica a C++/WinRT, sino a todas las proyecciones de lenguaje de Windows Runtime.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>¿Cómo convertir una cadena en un tipo, para la navegación, por ejemplo?
Al final del [ejemplo de código en la vista de navegación](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (que está principalmente en C#), hay un fragmento de código de C++/WinRT que muestra cómo hacerlo.

## <a name="how-do-i-resolve-ambiguities-with-getcurrenttime-andor-try"></a>¿Cómo se pueden resolver las ambigüedades con GetCurrentTime o TRY?

El archivo de encabezado `winrt/Windows.UI.Xaml.Media.Animation.h` declara un método denominado **GetCurrentTime**, mientras que `windows.h` (mediante `winbase.h`) define una macro denominada **GetCurrentTime**. Cuando los dos entran en conflicto, el compilador de C++ genera el error "*error C4002: demasiados argumentos para la invocación de la macro como función GetCurrentTime*".

De forma similar, `winrt/Windows.Globalization.h` declara un método denominado **TRY**, mientras que `afx.h` define una macro denominada **GetCurrentTime**. Cuando estos entran en conflicto, el compilador de C++ genera el error "*error C2334: tokens inesperados antes de '{'; se omite el cuerpo de función aparente*".

Para solucionar uno o ambos problemas, puedes hacer lo siguiente.

```cppwinrt
#pragma push_macro("GetCurrentTime")
#pragma push_macro("TRY")
#undef GetCurrentTime
#undef TRY
#include <winrt/include_your_cppwinrt_headers_here.h>
#include <winrt/include_your_cppwinrt_headers_here.h>
#pragma pop_macro("TRY")
#pragma pop_macro("GetCurrentTime")
```

> [!NOTE]
> Si en este tema no hemos respondido a tu pregunta, podrás encontrar ayuda en la [comunidad de desarrolladores de C++ de Visual Studio](https://developercommunity.visualstudio.com/spaces/62/index.html) o mediante la [etiqueta `c++-winrt` en Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
