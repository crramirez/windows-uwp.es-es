---
description: Respuestas a preguntas que probablemente tengas acerca de la creación y consumo de las API de Windows Runtime con C++/WinRT.
title: Preguntas más frecuentes sobre C++/WinRT
ms.date: 10/26/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, preguntas más frecuentes, P+F
ms.localizationpriority: medium
ms.openlocfilehash: 9dd051ffe3af9e18370666f5c6c772b7f188e54a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635580"
---
# <a name="frequently-asked-questions-about-cwinrt"></a>Preguntas más frecuentes sobre C++/WinRT
Respuestas a preguntas que le pueda tener sobre la creación y consumo de Windows en tiempo de ejecución APIs con [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

> [!NOTE]
> Si tu pregunta es sobre un mensaje de error que has visto, consulta también el tema [Solución de problemas de C++/WinRT](troubleshooting.md).

## <a name="how-do-i-retarget-my-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>¿Cómo redirigir mi C++ / c++ / WinRT proyecto a una versión posterior del SDK de Windows?
Consulte [cómo redestinar C++ / c++ / WinRT proyecto a una versión posterior del SDK de Windows](news.md#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk).

## <a name="why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134"></a>¿Por qué no se compilará mi nuevo proyecto? Estoy usando Visual Studio 2017 (versión 15.8.0 o superior) y el SDK versión 17134
Si usa Visual Studio 2017 (versión 15.8.0 o superior) y como destino el SDK de Windows versión 10.0.17134.0 (Windows 10, versión 1803), a continuación, un recién creado C++ / c++ / WinRT proyecto puede producir un error al compilar con el error "*error C3861: 'from_abi': no se encontró el identificador*"y con otros errores que se originan en *base.h*. La solución consiste en cualquier destino una posterior (mayor cumplimiento) versión del SDK de Windows, o la propiedad de proyecto conjunto **C o C++** > **lenguaje** > **modo de conformidad: No** (Además, si **/ permissive-** aparece en la propiedad del proyecto **C o C++** > **línea de comandos** en **opciones adicionales** , a continuación, eliminarlo).

## <a name="how-do-i-resolve-the-build-error-the-cwinrt-vsix-no-longer-provides-project-build-support--please-add-a-project-reference-to-the-microsoftwindowscppwinrt-nuget-package"></a>Cómo se puede resolver el error de compilación "C++ / c++ / WinRT VSIX ya no proporciona soporte técnico de compilación del proyecto.  ¿Agregue una referencia de proyecto al paquete Microsoft.Windows.CppWinRT Nuget"?
Instalar el **Microsoft.Windows.CppWinRT** paquete de NuGet en el proyecto. Para obtener más información, consulte [versiones anteriores de la extensión VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsix"></a>¿Cuáles son los requisitos para C++ / c++ / WinRT Visual Studio extensión (VSIX)?
Para la versión 1.0.190128.4 de la extensión VSIX y versiones posteriores, consulte [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package). Para otras versiones, consulte [versiones anteriores de la extensión VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="whats-a-runtime-class"></a>¿Qué es una *clase en tiempo de ejecución*?
Una clase en tiempo de ejecución es un tipo que puede activarse y consumirse a través de interfaces de COM modernas, normalmente a través de límites de archivo ejecutables. Sin embargo, una clase en tiempo de ejecución también puede usarse dentro de la unidad de compilación que la implementa. Al declarar una clase en tiempo de ejecución en el lenguaje de definición de interfaz (IDL), puedes implementarla en C++ estándar con C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>¿Qué significa *el tipo proyectado* y *el tipo de implementación*?
Si solo vas a *consumir* una clase de Windows Runtime (clase en tiempo de ejecución), tratarás exclusivamente con *tipos proyectados*. C++/WinRT es una *proyección de lenguaje*, de modo que los tipos proyectados forman parte de la superficie de Windows Runtime que se *proyecta* en C++ con C++/WinRT. Para obtener más información, consulte [consumir las API con C++ / c++ / WinRT](consume-apis.md).

El *tipo de implementación* contiene la implementación de una clase en tiempo de ejecución, por lo que solo está disponible en el proyecto que implementa la clase en tiempo de ejecución. Cuando estés trabajando en un proyecto que implemente clases en tiempo de ejecución (un proyecto de componente de Windows Runtime o un proyecto que use interfaz de usuario XAML), es importante que te sientas cómodo con la distinción entre tu tipo de implementación para una clase en tiempo de ejecución y el tipo proyectado que representa la clase en tiempo de ejecución proyectada en C++/WinRT. Para obtener más información, consulta [Crear API con C++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>¿Tengo que declarar un constructor en el archivo IDL de mi clase en tiempo de ejecución?
Solo si la clase en tiempo de ejecución se ha diseñado para consumirse desde fuera de su unidad de compilación de implementación (es un componente de Windows Runtime destinado al consumo general por aplicaciones cliente de Windows Runtime). Para obtener información completa sobre el propósito y las consecuencias de declarar constructores en IDL, consulta [Constructores de clases en tiempo de ejecución](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>¿Por qué el enlazador me va a dar un "error LNK2019: ¿Error de símbolo externo sin resolver"?
Si el símbolo no resuelto es una API de los encabezados de espacio de nombres de Windows para la proyección C++/WinRT (en el espacio de nombres **winrt**), la API se declara más adelante en un encabezado que hayas incluido, pero su definición está en un encabezado que todavía no hayas incluido. Incluye el encabezado nombrado para el espacio de nombres de la API y vuelve a crearlo. Para obtener más información, consulta [Encabezados de proyección de C++/WinRT](consume-apis.md#cwinrt-projection-headers).

Si el símbolo sin resolver es una función en tiempo de ejecución de Windows gratuita, como [RoInitialize](https://msdn.microsoft.com/library/br224650), a continuación, tendrá que vincule explícitamente el [WindowsApp.lib](/uwp/win32-and-com/win32-apis) biblioteca paraguas en el proyecto. La proyección de C++/WinRT depende de algunas de estas funciones (no miembro) y puntos de entrada libres . Si usas una de las plantillas de proyecto [Extensión de Visual Studio (VSIX) de C++ /WinRT](https://aka.ms/cppwinrt/vsix) para la aplicación, `WindowsApp.lib` se vincula por ti automáticamente. De lo contrario, puedes usar la configuración de enlace del proyecto para incluirla o hacerlo en el código fuente.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Es importante que resolver los errores del vinculador que puede efectuar mediante la vinculación **WindowsApp.lib** en lugar de una biblioteca de vínculos estáticos alternativa, en caso contrario, la aplicación no pasar la [Windows App Certification Kit](../debug-test-perf/windows-app-certification-kit.md) pruebas usadas por Visual Studio y la Microsoft Store para validar los envíos (es decir, que por lo tanto no será posible para la aplicación para que se introducen correctamente en la Microsoft Store).

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>¿Debo implementar [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)? Y, si es así, ¿cómo?
Si tienes una clase en tiempo de ejecución que libera recursos en su destructor, y si dicha clase en tiempo de ejecución se ha diseñado para usarse desde fuera de su unidad de compilación de implementación (es un componente de Windows Runtime destinado al consumo general por aplicaciones cliente Windows Runtime), te recomendamos que también implementes **IClosable** para poder admitir el consumo de tu clase en tiempo de ejecución con lenguajes que carecen de finalización determinista. Asegúrate de que tus recursos se liberan si se llama al constructor [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) o a ambos. **IClosable::Close** puede llamarse un número arbitrario de veces.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>¿Tengo que llamar a [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.close) en las clases en tiempo de ejecución que consumo?
**IClosable** existe para admitir los lenguajes que carecen de finalización determinista. Por lo tanto, no debes llamar a **IClosable::Close** desde C++/WinRT, excepto en casos muy raros que impliquen carreras de apagado o interbloqueos. Si vas a usar tipos **Windows.UI.Composition**, por ejemplo, puede que te encuentres con casos en los que quieras deshacerte de objetos en una secuencia definida, como una alternativa para permitir que la destrucción del contenedor C++/WinRT haga el trabajo por ti.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>¿Puedo usar LLVM/Clang para compilar con C++/WinRT?
No admitimos la cadena de herramientas de LLVM y Clang para C++/WinRT, pero hacemos uso de ella internamente para validar la conformidad con los estándares de C++/WinRT. Por ejemplo, si quisieras emular lo que hacemos internamente, podrías intentar un experimento, como el que se describe a continuación.

Ve a la [página de descarga de LLVM](https://releases.llvm.org/download.html), busca **Descargar LLVM 6.0.0** > **Archivos binarios integrados previamente**y descarga **Clang para Windows (64 bits)**. Durante la instalación, opta por agregar LLVM a la variable del sistema PATH para poder invocarlo desde un símbolo del sistema. Para los fines de este experimento, puede hacer caso omiso de los errores "No se pudo encontrar el directorio de conjuntos de herramientas de MSBuild" o "Error de instalación de la integración de MSVC", si aparecen. Hay una gran variedad de formas para invocar LLVM/Clang; el siguiente ejemplo muestra una de las formas.

```
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

Dado que C++/WinRT usa características del estándar 17 de C++, deberás usar los marcadores del compilador que sean necesarios para obtener dicho soporte; estos marcadores difieren entre compiladores.

Visual Studio es la herramienta de desarrollo que admite y se recomienda para C++/WinRT. Consulte [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="why-doesnt-the-generated-implementation-function-for-a-read-only-property-have-the-const-qualifier"></a>¿Por qué no tiene la función de implementación generado para una propiedad de solo lectura del `const` calificador?
Cuando se declara una propiedad de solo lectura en [MIDL 3.0](/uwp/midl-3/), cabría esperar el `cppwinrt.exe` herramienta para generar una función de implementación para usted que es `const`-completo (const función trata la *Este* puntero como const).

Sin duda, se recomienda usar const siempre que sea posible, pero la `cppwinrt.exe` propia herramienta no intenta razón sobre la implementación de las funciones posiblemente podrían ser constantes y que es posible que no. Puede realizar cualquiera de las funciones de implementación const, como en este ejemplo.

```cppwinrt
struct MyStringable : winrt::implements<MyStringable, winrt::Windows::Foundation::IStringable>
{
    winrt::hstring ToString() const
    {
        return L"MyStringable";
    }
};
```

Puede eliminar ese `const` calificador en **ToString** debe decide que necesita modificar algún estado de objeto en su implementación. Pero cada uno de los miembros que las funciones constantes o no es const, no ambos. En otras palabras, no sobrecargar una función de la implementación en `const`.

Aparte de las funciones de implementación, otro otros colocar donde const entra en la imagen se encuentra en las proyecciones de función en tiempo de ejecución de Windows. Considere este código.

```cppwinrt
int main()
{
    winrt::Windows::Foundation::IStringable s{ winrt::make<MyStringable>() };
    auto result{ s.ToString() };
}
```

Para la llamada a **ToString** anterior, el **ir a declaración** comando en Visual Studio que muestra la proyección de Windows Runtime **IStringable::ToString** en C++ / c++ / WinRT tiene este aspecto.

```
winrt::hstring ToString() const;
```

Funciones de la proyección son const independientemente de cómo decida calificar la implementación de ellas. En segundo plano, la proyección llama a la interfaz binaria de aplicación (ABI), que las cantidades a una llamada a través de un puntero de interfaz COM. El único estado que el previsto **ToString** interactúa con es ese puntero de interfaz COM; y ciertamente no tiene necesidad de modificar ese puntero, por lo que la función es const. Esto le ofrece la garantía de que no cambiará nada sobre la **IStringable** referencia que está llamando a través, y garantiza que puede llamar a **ToString** incluso con una referencia const a un  **IStringable**.

Comprender que estos ejemplos de `const` son detalles de implementación de C / c++ / WinRT proyecciones e implementaciones; que constituyen la higiene de código para ayudarle. No hay nada como `const` en COM ni las ABI de Windows en tiempo de ejecución (para funciones miembro).

## <a name="do-you-have-any-recommendations-for-decreasing-the-code-size-for-cwinrt-binaries"></a>¿Tiene las recomendaciones para reducir el tamaño del código de C++ / c++ / WinRT los archivos binarios?
Cuando se trabaja con objetos en tiempo de ejecución de Windows, debe evitar el patrón de codificación que se muestra a continuación, ya que puede tener un impacto negativo en la aplicación haciendo más código binario que es necesario que se genere.

```cppwinrt
anobject.b().c().d();
anobject.b().c().e();
anobject.b().c().f();
```

En el mundo de Windows en tiempo de ejecución, el compilador no puede almacenar en caché el valor de `c()` o las interfaces para cada método que se llama a través de un direccionamiento indirecto ('. '). A menos que intervenir, que da como resultado varias llamadas virtuales y sobrecarga de recuento de referencias. El patrón anterior podría generar fácilmente dos veces mucho código como sea estrictamente necesario. En su lugar, se prefiere el patrón que se muestra a continuación dondequiera que puede. Genera mucho menos código y puede mejorar drásticamente el rendimiento de tiempo de ejecución.

```cppwinrt
auto a{ anobject.b().c() };
a.d();
a.e();
a.f();
```

El patrón recomendado mostrado anteriormente se aplica no solo a C++ / c++ / WinRT, pero para todas las proyecciones de lenguaje de Windows en tiempo de ejecución.

## <a name="how-do-i-turn-a-string-into-a-typemdashfor-navigation-for-example"></a>¿Cómo convertir una cadena en un tipo&mdash;para la navegación, por ejemplo?
Al final de la [ejemplo de código de la vista de navegación](/windows/uwp/design/controls-and-patterns/navigationview#code-example) (lo que es principalmente en C#), hay un C++ / c++ / WinRT fragmento de código que muestra cómo hacerlo.

> [!NOTE]
> Si en este tema no hayamos respondido su pregunta, a continuación, podría encontrar ayuda, visite la [Comunidad de desarrolladores de Visual Studio C++](https://developercommunity.visualstudio.com/spaces/62/index.html), o mediante el [ `c++-winrt` etiquetar en Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).
