---
author: stevewhims
description: Una introducción a C++/WinRT&mdash;una proyección de lenguaje C++ estándar para las API de Windows Runtime.
title: Introducción a C++/WinRT
ms.author: stwhi
ms.date: 05/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección
ms.localizationpriority: medium
ms.openlocfilehash: 968afd6fdad1e7bf6b3c38d929ab79eefa71819a
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832329"
---
# <a name="introduction-to-cwinrt"></a>Introducción a C++/WinRT
> [!NOTE]
> **Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.**

Presentado en la versión 10.0.17134.0 (Windows 10, versión 1803), el Windows SDK ahora incluye C++/WinRT.

> [!IMPORTANT]
> Las partes más importantes de C++/WinRT a tener en cuenta se describen en las secciones [Soporte SDK para C++/WinRT](#sdk-support-for-cwinrt) y [Soporte de Visual Studio para C++/WinRT y VSIX](#visual-studio-support-for-cwinrt-and-the-vsix).

C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime, implementada únicamente en los archivos de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows. Con C++/WinRT, puedes crear y consumir API de Windows Runtime usando cualquier compilador de C ++17 compatible con estándares.

## <a name="language-projections"></a>Proyecciones de lenguaje
Windows Runtime se basa en las API de modelos de objetos componentes (COM) y se ha diseñado para acceder a él a través de *proyecciones de lenguaje*. Una proyección oculta los detalles del COM y proporciona una experiencia de programación más natural para un determinado lenguaje.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Proyección de lenguaje C++/WinRT en el contenido de referencia de las API de Windows UWP
Cuando vayas a las [API de Windows UWP](https://docs.microsoft.com/uwp/api/), haz clic en el cuadro combinado **Language** en la esquina superior derecha y selecciona **C++/WinRT** para ver los bloques de sintaxis de API tal y como aparecen en la proyección de lenguaje C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Soporte de SDK para C++/WinRT
A partir de la versión 10.0.17134.0 (Windows 10, versión 1803), el Windows SDK contiene los encabezados y las herramientas del espacio de nombres de Windows de la proyección C++/WinRT. Una herramienta importante es `cppwinrt.exe` que, a partir de un `.winmd`, genera archivos de código fuente que proyectan los metadatos en C++/WinRT. Los archivos de metadatos de Windows Runtime (`.winmd`) proporcionan una forma canónica de describir una superficie de API de Windows Runtime. A partir de los metadatos de Windows Runtime, `cppwinrt.exe` genera una biblioteca de C++ estándar que describe completamente&mdash;o *proyecta*&mdash;la superficie de API; ya sean API de Windows o API de componentes de Windows Runtime de terceros. `Cppwinrt.exe` Desempeña un papel importante en el flujo de trabajo de desarrollo para consumir API originales y API de terceros, además de crear tu propia API en los componentes.

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>Soporte de Visual Studio para C++/WinRT y VSIX.
Para obtener las plantillas de proyecto C++/WinRT en Visual Studio, así como las propiedades y destinos de MSBuild de C++/WinRT, descarga e instala la [extensión de Visual Studio de C++/WinRT](https://aka.ms/cppwinrt/vsix) desde [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

Necesitarás la versión 15.6 de Visual Studio 2017 o posterior y la versión 10.0.17134.0 de Windows SDK (Windows 10, versión 1803). A continuación, puedes crear un nuevo proyecto en Visual Studio o puedes convertir un proyecto existente añadiendo la propiedad `<CppWinRTProject>true</CppWinRTProject>` a tu archivo `.vcxproj` dentro de Project > PropertyGroup. Una vez que hayas agregado dicha propiedad, obtendrás soporte de MSBuild C++/WinRT para el proyecto, incluido invocar la herramienta `cppwinrt.exe`.

Dado que C++/WinRT usa características del estándar C++17, necesita la propiedad del proyecto **C/C++** > **Language** > **ISO C++17 Standard (/std:c++17)**. También puedes establecer **Conformance mode: Yes (/permissive-)**, que limita todavía más tu código para que sea compatible con los estándares.

Otra propiedad de proyecto a tener en cuenta es **C/C++** > **General** > **Tratar advertencias como errores**. Establece esto a **Yes (/WX)** o **No (/WX-)** según te parezca. A veces, los archivos de origen generados por la herramienta `cppwinrt.exe` generan advertencias hasta que les agregas tu implementación.

Estas son las plantillas de proyecto de Visual Studio proporcionadas por VSIX.

### <a name="windows-console-application-cwinrt"></a>Aplicación de consola de Windows (C++/WinRT)
Una plantilla de proyecto para una aplicación cliente C+/WinRT para dispositivos de escritorio de Windows, con una interfaz de usuario de consola.

### <a name="blank-app-cwinrt"></a>Aplicación vacía (C++/WinRT)
Una plantilla de proyecto para una aplicación de la plataforma universal de Windows (UWP) que tiene una interfaz de usuario XAML.

Visual Studio proporciona soporte con el compilador XAML para generar implementaciones y códigos auxiliares de encabezados a partir del archivo basado en lenguaje de definición de interfaz (IDL) (`.idl`) que se encuentra detrás de cada archivo de marcado XAML. En un archivo IDL, define las clases en tiempo de ejecución locales a las que quieras hacer referencia en las páginas XAML de tu aplicación, y después compila el proyecto una sola vez para generar las plantillas de implementación en `Generated Files` y las definiciones del tipo de código auxiliar en `Generated Files\sources`. Luego usa estas definiciones de tipo de código auxiliar como referencia para implementar tus clases en tiempo de ejecución locales. Te recomendamos que declares cada clase en tiempo de ejecución en su propio archivo IDL.

### <a name="core-app-cwinrt"></a>Aplicación principal (C++/WinRT)
Una plantilla de proyecto para una aplicación de la plataforma universal de Windows (UWP) que no usa XAML.

En su lugar, usa el encabezado del espacio de nombres de Windows de la proyección C++/WinRT para el espacio de nombres Windows.ApplicationModel.Core. Después de compilar y ejecutar, haz clic en un espacio vacío para agregar un cuadrado de color; luego haz clic en un cuadrado de color para arrastrarlo.

### <a name="windows-runtime-component-cwinrt"></a>Componente de Windows Runtime (C++/WinRT)
Una plantilla de proyecto para un componente; normalmente para el consumo desde una plataforma universal de Windows (UWP).

Esta plantilla muestra la cadena de herramientas `midl.exe` > `cppwinrt.exe`, donde se generan los metadatos de Windows Runtime (`.winmd`) a partir de IDL, y después se generan los códigos auxiliares de las implementación y el encabezado a partir de los metadatos de Windows Runtime.

En un archivo IDL, define las clases en tiempo de ejecución en tu componente, su interfaz predeterminada y cualquier otra interfaz que lo implemente. Compila el proyecto una sola vez para generar `module.g.cpp`, `module.h.cpp`, plantillas de implementación en `Generated Files` y la definición del tipo de código auxiliar en `Generated Files\sources`. Luego usa estas definiciones de tipo de código auxiliar como referencia para implementar las clases en tiempo de ejecución en tu componente. Te recomendamos que declares cada clase en tiempo de ejecución en su propio archivo IDL.

Agrupa el binario compilado de componente de Windows Runtime y su `.winmd` con la aplicación para UWP consumiéndolos.

## <a name="a-cwinrt-quick-start"></a>Inicio rápido C++/WinRT
Crea un nuevo proyecto **Aplicación de consola de Windows (C++/WinRT)**. Edita `main.cpp` para que tenga este aspecto

```cppwinrt
// main.cpp

#include "pch.h"
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

int main()
{
    winrt::init_apartment();

    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    for (const SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

Los encabezados incluidos `winrt/Windows.Foundation.h` y `winrt/Windows.Web.Syndication.h` se encuentran en el SDK, dentro de la carpeta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt\`. Visual Studio incluye dicha ruta de acceso en su macro *IncludePath*. Los encabezados contienen las API de Windows proyectadas en C++/WinRT. Siempre que quieras usar tipos desde los espacios de nombres de Windows, incluye los correspondientes encabezados del espacio de nombres de Windows de la proyección C++/WinRT. Las directivas `using namespace` son opcionales, pero adecuadas.

Todos los tipos proyectados se encuentran en el espacio de nombres raíz **winrt** C++/WinRT. Tanto [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) como el Windows SDK declaran tipos en el espacio de nombres raíz **Windows**. Estos espacios de nombres diferentes te permiten migrar de C++/CX a C++/WinRT a tu propio ritmo.

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) es un ejemplo de una función asincrónica de Windows Runtime. El siguiente ejemplo de código recibe un objeto de operación asincrónica desde **RetrieveFeedAsync** y llama a **get** en dicho objeto para bloquear el subproceso de llamada y esperar los resultados. Para más información sobre la simultaneidad y técnicas de no bloqueo, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md).

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) es un intervalo definido por los iteradores devueltos a partir de las funciones **begin** y **end** (o sus variantes constantes, inversas y constante-inversas). Por este motivo, puedes enumerar **Items** con una declaración `for` basada en intervalo o con la función de plantilla **std::for_each**.

El código obtiene el texto del título del feed, como un objeto [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (consulta [Control de cadenas en C++/WinRT](strings.md)). Después se emite la **hstring** a través de **c_str**, que te resultará familiar si has usado cadenas desde la biblioteca estándar de C++.

Como puedes ver, C++/WinRT fomenta expresiones C++ modernas y de clase, como `syndicationItem.Title().Text()`. Es un estilo de programación diferente y más limpio que la tradicional programación COM. No tienes que inicializar COM explícitamente (**winrt::init_apartment** lo hace por ti), trabajar con punteros COM, ni controlar códigos de retorno HRESULT. C++/WinRT convierte los errores HRESULT en excepciones para lograr un estilo de programación natural y moderno.

## <a name="custom-types-in-the-cwinrt-projection"></a>Tipos personalizados en la proyección C++/WinRT
Puedes usar características del lenguaje C++ estándar y [tipos de datos C++ estándar y C++/WinRT](std-cpp-data-types.md)&mdash;incluidos algunos tipos de datos de la biblioteca estándar de C++&mdash;en tu programación C++/WinRT. Pero también serás consciente de algunos tipos de datos personalizados en la proyección, y podrás optar por usarlos. Por ejemplo, usamos [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) en el anterior inicio rápido.

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) es otro tipo que probablemente uses en algún momento. Pero es menos probable que uses directamente un tipo como [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view). O bien puedes optar por no usarlo para no tener ningún código que cambiar si aparece un tipo equivalente en la biblioteca estándar de C++.

También hay tipos que probablemente verás si estudias detenidamente encabezados del espacio de nombres de Windows de la proyección C++/WinRT. Un ejemplo es **winrt::param::hstring**. Estos existen solo por motivos de eficacia y no deberías usarlos en tu código.

## <a name="important-apis"></a>API importantes
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [Estructura winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Artículos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Control de cadenas en C++/WinRT](strings.md)
* [Visual Studio Marketplace](https://marketplace.visualstudio.com/)
* [API de Windows UWP](https://docs.microsoft.com/uwp/api/)
