---
author: stevewhims
description: Respuestas a preguntas que probablemente tengas acerca de la creación y consumo de las API de Windows Runtime con C++/WinRT.
title: Preguntas más frecuentes sobre C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, preguntas más frecuentes, P+F
ms.localizationpriority: medium
ms.openlocfilehash: 617f9ee49130a55cf0378f2a70b72296224dcefc
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2018
ms.locfileid: "1881026"
---
# <a name="frequently-asked-questions-about-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Preguntas más frecuentes sobre [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Respuestas a preguntas que probablemente tengas acerca de la creación y consumo de las API de Windows Runtime con C++/WinRT.

> [!NOTE]
> Si tu pregunta es sobre un mensaje de error que has visto, consulta también el tema [Solución de problemas de C++/WinRT](troubleshooting.md).

## <a name="what-are-the-requirements-for-the-cwinrt-visual-studio-extension-vsixhttpsakamscppwinrtvsix"></a>¿Cuáles son los requisitos para la [extensión de Visual Studio (VSIX) C++/WinRT](https://aka.ms/cppwinrt/vsix)?
La extensión [VSIX](https://aka.ms/cppwinrt/vsix) exige una versión de destino de Windows SDK mínima de 10.0.17134.0 (Windows 10, versión 1803). También necesitarás Visual Studio 2017 (al menos la versión 15.6; te recomendamos al menos la 15.7). Puedes identificar un proyecto que usa la extensión VSIX mediante la presencia de `<CppWinRTEnabled>true</CppWinRTEnabled>` en `<PropertyGroup Label="Globals">` en el archivo `.vcxproj`. Para obtener más información, consulta [Soporte de Visual Studio para C++/WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="whats-a-runtime-class"></a>¿Qué es una *clase en tiempo de ejecución*?
Una clase en tiempo de ejecución es un tipo que puede activarse y consumirse a través de interfaces de COM modernas, normalmente a través de límites de archivo ejecutables. Sin embargo, una clase en tiempo de ejecución también puede usarse dentro de la unidad de compilación que la implementa. Al declarar una clase en tiempo de ejecución en el lenguaje de definición de interfaz (IDL), puedes implementarla en C++ estándar con C++/WinRT.

## <a name="what-do-the-projected-type-and-the-implementation-type-mean"></a>¿Qué significa *el tipo proyectado* y *el tipo de implementación*?
Si solo vas a *consumir* una clase de Windows Runtime (clase en tiempo de ejecución), tratarás exclusivamente con *tipos proyectados*. C++/WinRT es una *proyección de lenguaje*, de modo que los tipos proyectados forman parte de la superficie de Windows Runtime que se *proyecta* en C++ con C++/WinRT. Para obtener más información, consulta [Consumir API con C++/WinRT](consume-apis.md).

El *tipo de implementación* contiene la implementación de una clase en tiempo de ejecución, por lo que solo está disponible en el proyecto que implementa la clase en tiempo de ejecución. Cuando estés trabajando en un proyecto que implemente clases en tiempo de ejecución (un proyecto de componente de Windows Runtime o un proyecto que use interfaz de usuario XAML), es importante que te sientas cómodo con la distinción entre tu tipo de implementación para una clase en tiempo de ejecución y el tipo proyectado que representa la clase en tiempo de ejecución proyectada en C++/WinRT. Para obtener más información, consulta [Crear API con C++/WinRT](author-apis.md).

## <a name="do-i-need-to-declare-a-constructor-in-my-runtime-classs-idl"></a>¿Tengo que declarar un constructor en el archivo IDL de mi clase en tiempo de ejecución?
Solo si la clase en tiempo de ejecución se ha diseñado para consumirse desde fuera de su unidad de compilación de implementación (es un componente de Windows Runtime destinado al consumo general por aplicaciones cliente de Windows Runtime). Para obtener información completa sobre el propósito y las consecuencias de declarar constructores en IDL, consulta [Constructores de clases en tiempo de ejecución](author-apis.md#runtime-class-constructors).

## <a name="why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error"></a>¿Por qué el enlazador que me ofrece el error "LNK2019: símbolo externo sin resolver?
Si el símbolo no resuelto es una API de los encabezados de espacio de nombres de Windows para la proyección C++/WinRT (en el espacio de nombres **winrt**), la API se declara más adelante en un encabezado que hayas incluido, pero su definición está en un encabezado que todavía no hayas incluido. Incluye el encabezado nombrado para el espacio de nombres de la API y vuelve a crearlo. Para obtener más información, consulta [Encabezados de proyección de C++/WinRT](consume-apis.md#cwinrt-projection-headers).

Si el símbolo no resuelto es una función libre de Windows Runtime, como [RoInitialize](https://msdn.microsoft.com/library/br224650), tendrás que incluir explícitamente la biblioteca [WindowsApp.lib](/uwp/win32-and-com/win32-apis) paraguas en el proyecto. La proyección de C++/WinRT depende de algunas de estas funciones (no miembro) y puntos de entrada libres . Si usas una de las plantillas de proyecto [Extensión de Visual Studio (VSIX) de C++ /WinRT](https://aka.ms/cppwinrt/vsix) para la aplicación, `WindowsApp.lib` se vincula por ti automáticamente. De lo contrario, puedes usar la configuración de enlace del proyecto para incluirla o hacerlo en el código fuente.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

## <a name="should-i-implement-windowsfoundationiclosableuwpapiwindowsfoundationiclosable-and-if-so-how"></a>¿Debo implementar [**Windows::Foundation::IClosable**](/uwp/api/windows.foundation.iclosable)? Y, si es así, ¿cómo?
Si tienes una clase en tiempo de ejecución que libera recursos en su destructor, y si dicha clase en tiempo de ejecución se ha diseñado para usarse desde fuera de su unidad de compilación de implementación (es un componente de Windows Runtime destinado al consumo general por aplicaciones cliente Windows Runtime), te recomendamos que también implementes **IClosable** para poder admitir el consumo de tu clase en tiempo de ejecución con lenguajes que carecen de finalización determinista. Asegúrate de que tus recursos se liberan si se llama al constructor [**IClosable::Close**](/uwp/api/windows.foundation.iclosable.Close) o a ambos. **IClosable::Close** puede llamarse un número arbitrario de veces.

## <a name="do-i-need-to-call-iclosablecloseuwpapiwindowsfoundationiclosablewindowsfoundationiclosableclose-on-runtime-classes-that-i-consume"></a>¿Tengo que llamar a [**IClosable::Close**](/uwp/api/windows.foundation.iclosable#Windows_Foundation_IClosable_Close_) en las clases en tiempo de ejecución que consumo?
**IClosable** existe para admitir los lenguajes que carecen de finalización determinista. Por lo tanto, no debes llamar a **IClosable::Close** desde C++/WinRT, excepto en casos muy raros que impliquen carreras de apagado o interbloqueos. Si vas a usar tipos **Windows.UI.Composition**, por ejemplo, puede que te encuentres con casos en los que quieras deshacerte de objetos en una secuencia definida, como una alternativa para permitir que la destrucción del contenedor C++/WinRT haga el trabajo por ti.

## <a name="can-i-use-llvmclang-to-compile-with-cwinrt"></a>¿Puedo usar LLVM/Clang para compilar con C++/WinRT?
No admitimos la cadena de herramientas de LLVM y Clang para C++/WinRT, pero hacemos uso de ella internamente para validar la conformidad con los estándares de C++/WinRT. Por ejemplo, si quisieras emular lo que hacemos internamente, podrías intentar un experimento, como el que se describe a continuación.

Ve a la [página de descarga de LLVM](https://releases.llvm.org/download.html), busca **Descargar LLVM 6.0.0** > **Archivos binarios integrados previamente**y descarga **Clang para Windows (64 bits)**. Durante la instalación, opta por agregar LLVM a la variable del sistema PATH para poder invocarlo desde un símbolo del sistema. Para los fines de este experimento, puede hacer caso omiso de los errores "No se pudo encontrar el directorio de conjuntos de herramientas de MSBuild" o "Error de instalación de la integración de MSVC", si aparecen. Hay una gran variedad de formas para invocar LLVM/Clang; el siguiente ejemplo muestra una de las formas.

```
C:\ExperimentWithLLVMClang>type main.cpp
// main.cpp
#pragma comment(lib, "windowsapp")
#pragma comment(lib, "ole32")

#include "winrt/Windows.Foundation.h"
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

Visual Studio es la herramienta de desarrollo que admite y se recomienda para C++/WinRT. Consulta [Soporte de Visual Studio para C++/WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

> [!NOTE]
> Si en este tema no responde a tu pregunta, puedes buscar ayuda mediante la [etiqueta `c++-winrt` en Stack Overflow](https://stackoverflow.com/questions/tagged/c%2b%2b-winrt).