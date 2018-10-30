---
author: stevewhims
description: Una introducción a C++/WinRT&mdash;una proyección de lenguaje C++ estándar para las API de Windows Runtime.
title: Introducción a C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, introducción
ms.localizationpriority: medium
ms.openlocfilehash: 8b88eac972cd65b771827d7e3125476265cf671e
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5754731"
---
# <a name="introduction-to-cwinrt"></a>Introducción a C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivo de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows. Con C++/WinRT, puedes crear y consumir API de Windows Runtime usando cualquier compilador de C ++17 compatible con estándares. Windows SDK incluye C++/WinRT. Se introdujo en la versión 10.0.17134.0 (Windows 10, versión 1803).

C++ / WinRT es el reemplazo recomendado de Microsoft para la [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) proyección de lenguaje y la [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). La lista completa de [los temas sobre C++ / WinRT](index.md#topics-about-cwinrt) se incluye información sobre la interoperación con tanto a portar desde, C++ / CX y WRL.

> [!IMPORTANT]
> Dos de las partes más importantes de C++/WinRT a tener en cuenta se describen en las secciones [Soporte SDK para C++/WinRT](#sdk-support-for-cwinrt) y [Soporte de Visual Studio para C++/WinRT y VSIX](#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="language-projections"></a>Proyecciones de lenguaje
Windows Runtime se basa en las API de modelos de objetos componentes (COM) y se ha diseñado para acceder a él a través de *proyecciones de lenguaje*. Una proyección oculta los detalles del COM y proporciona una experiencia de programación más natural para un determinado lenguaje.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Proyección de lenguaje C++/WinRT en el contenido de referencia de las API de Windows UWP
Cuando vayas a las [API de Windows UWP](https://docs.microsoft.com/uwp/api/), haz clic en el cuadro combinado **Language** en la esquina superior derecha y selecciona **C++/WinRT** para ver los bloques de sintaxis de API tal y como aparecen en la proyección de lenguaje C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Soporte de SDK para C++/WinRT
A partir de la versión 10.0.17134.0 (Windows 10, versión 1803), el Windows SDK contiene una biblioteca de C++ estándar basada en archivos de encabezado para el uso de API de Windows propias (API de Windows Runtime en espacios de nombres de Windows). C++/WinRT también se distribuye con la herramienta `cppwinrt.exe`, que puedes apuntar a un archivo de metadatos de Windows Runtime (`.winmd`) para generar una biblioteca de C++ estándar basados en archivos de encabezado que *proyecta* las API descritas en los metadatos para el consumo de código C++/WinRT. Los archivos de metadatos de Windows Runtime (`.winmd`) proporcionan una forma canónica de describir una superficie de API de Windows Runtime. Al apuntar a `cppwinrt.exe` en los metadatos, puedes generar una biblioteca para usarla con cualquier clase en tiempo de ejecución implementada en un componente de Windows Runtime de terceros o segunda parte o implementado en tu propia aplicación. Para obtener más información, consulta [Consumir API con C++/WinRT](consume-apis.md).

Con C++/WinRT, también puedes implementar tus propias clases en tiempo de ejecución con C++ estándar, sin tener que recurrir a la programación de estilo COM. Para una clase en tiempo de ejecución, solo tienes que describir tus tipos en un archivo IDL, y `midl.exe` y `cppwinrt.exe` generarán para ti tus archivos de código fuente de implementación reutilizable. Como alternativa puedes simplemente implementar interfaces derivando de una clase base de C++/WinRT. Para obtener más información, consulta [Crear API con C++/WinRT](author-apis.md).

## <a name="visual-studio-support-for-cwinrt-and-the-vsix"></a>Soporte de Visual Studio para C++/WinRT y VSIX.
Para obtener las plantillas de proyecto C++/WinRT en Visual Studio, así como las propiedades y destinos de MSBuild de C++/WinRT, descarga e instala la [extensión de Visual Studio de C++/WinRT](https://aka.ms/cppwinrt/vsix) desde [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

> [!NOTE]
> Con la versión 1.0.181002.2 (o posterior) de la extensión VSIX instalada, crear un nuevo C++ / WinRT proyecto instala automáticamente el [paquete de Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) para ese proyecto. Proporciona el paquete de Microsoft.Windows.CppWinRT NuGet mejorada C++ / WinRT soporte para compilación de proyectos, hacer que el proyecto portable entre un equipo de desarrollo y un agente de compilación (en el que solo el paquete de NuGet y no VSIX, se instala).
>
> Para un proyecto existente&mdash;después de que has instalado versión 1.0.181002.2 (o posterior) de la extensión VSIX&mdash;te recomendamos que abre el proyecto en Visual Studio, haz clic en el **proyecto** \> **Administrar paquetes de NuGet …**  \>  **Examinar**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de búsqueda y, a continuación, haz clic en **instalar** para instalar el paquete de ese proyecto.

Tendrás que Visual Studio 2017 (tendrás que al menos la versión 15.6, pero te recomendamos al menos la 15.7) y Windows SDK versión 10.0.17134.0 (Windows 10, versión 1803). Si ya no has instalado, tendrás que instalar la opción de **Herramientas de la plataforma Universal de Windows de C++** desde el instalador de Visual Studio. Y, en la **configuración**de Windows > **Update \ & seguridad** > **para desarrolladores**, elige la opción de **modo de desarrollador** , en lugar de la opción de **instalar aplicaciones** .

A continuación, podrás crear y generar o abrir, C++ / WinRT de proyecto en Visual Studio e implementarla. Como alternativa, puedes convertir un proyecto existente mediante la adición de la `<CppWinRTEnabled>true</CppWinRTEnabled>` propiedad a su `.vcxproj` archivo.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Una vez que hayas agregado dicha propiedad, obtendrás soporte de MSBuild C++/WinRT para el proyecto, incluido invocar la herramienta `cppwinrt.exe`.

Dado que C++ / WinRT usa características del estándar C ++ 17, necesita la propiedad **C/c ++** project > **idioma** > **Lenguaje de C++ estándar** > **ISO C ++ 17 Standard (/ STD: c ++ 17)**. También puedes establecer **Conformance mode: Yes (/permissive-)**, que limita todavía más tu código para que sea compatible con los estándares.

Otra propiedad de proyecto a tener en cuenta es **C/C++** > **General** > **Tratar advertencias como errores**. Establece esto a **Yes (/WX)** o **No (/WX-)** según te parezca. A veces, los archivos de origen generados por la herramienta `cppwinrt.exe` generan advertencias hasta que les agregas tu implementación.

La extensión VSIX también te ofrece visualización de depuración nativa de Visual Studio (natvis) de tipos proyectados de C++/ WinRT, proporcionando una experiencia similar a la depuración de C#. Natvis es automático para compilaciones de depuración. Puedes participar en el lanzamiento de versiones definiendo el símbolo WINRT_NATVIS.

Estas son las plantillas de proyecto de Visual Studio proporcionadas por VSIX.

### <a name="windows-console-application-cwinrt"></a>Aplicación de consola de Windows (C++/WinRT)
Una plantilla de proyecto para una aplicación cliente C+/WinRT para dispositivos de escritorio de Windows, con una interfaz de usuario de consola.

### <a name="blank-app-cwinrt"></a>Aplicación vacía (C++/WinRT)
Una plantilla de proyecto para una aplicación de la plataforma universal de Windows (UWP) que tiene una interfaz de usuario XAML.

Visual Studio proporciona soporte con el compilador XAML para generar implementaciones y códigos auxiliares de encabezados a partir del archivo basado en lenguaje de definición de interfaz (IDL) (`.idl`) que se encuentra detrás de cada archivo de marcado XAML. En un archivo IDL, define las clases en tiempo de ejecución locales a las que quieras hacer referencia en las páginas XAML de tu aplicación, y después compila el proyecto una sola vez para generar las plantillas de implementación en `Generated Files` y las definiciones del tipo de código auxiliar en `Generated Files\sources`. Luego usa estas definiciones de tipo de código auxiliar como referencia para implementar tus clases en tiempo de ejecución locales. Te recomendamos que declares cada clase en tiempo de ejecución en su propio archivo IDL.

### <a name="core-app-cwinrt"></a>Aplicación principal (C++/WinRT)
Una plantilla de proyecto para una aplicación de la Plataforma universal de Windows (UWP) que no usa XAML.

En su lugar, usa el encabezado del espacio de nombres de Windows de C++/WinRT para el espacio de nombres Windows.ApplicationModel.Core. Después de compilar y ejecutar, haz clic en un espacio vacío para agregar un cuadrado de color; luego haz clic en un cuadrado de color para arrastrarlo.

### <a name="windows-runtime-component-cwinrt"></a>Componente de Windows Runtime (C++/WinRT)
Una plantilla de proyecto para un componente; normalmente para el consumo desde una plataforma universal de Windows (UWP).

Esta plantilla muestra la cadena de herramientas `midl.exe` > `cppwinrt.exe`, donde se generan los metadatos de Windows Runtime (`.winmd`) a partir de IDL, y después se generan los códigos auxiliares de las implementación y el encabezado a partir de los metadatos de Windows Runtime.

En un archivo IDL, define las clases en tiempo de ejecución en tu componente, su interfaz predeterminada y cualquier otra interfaz que lo implemente. Compila el proyecto una sola vez para generar `module.g.cpp`, `module.h.cpp`, plantillas de implementación en `Generated Files` y la definición del tipo de código auxiliar en `Generated Files\sources`. Luego usa estas definiciones de tipo de código auxiliar como referencia para implementar las clases en tiempo de ejecución en tu componente. Te recomendamos que declares cada clase en tiempo de ejecución en su propio archivo IDL.

Agrupa el binario compilado de componente de Windows Runtime y su `.winmd` con la aplicación para UWP consumiéndolos.

## <a name="custom-types-in-the-cwinrt-projection"></a>Tipos personalizados en la proyección C++/WinRT
En tu C++ / WinRT de programación, puedes usar características del lenguaje C++ estándar y [tipos de datos C++ estándar y C++ / WinRT](std-cpp-data-types.md)&mdash;incluidos algunos tipos de datos de la biblioteca estándar de C++. Pero también serás consciente de algunos tipos de datos personalizados en la proyección, y podrás optar por usarlos. Por ejemplo, usamos [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) en el ejemplo de código de inicio rápido de [Introducción a C++/WinRT](get-started.md).

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) es otro tipo que probablemente uses en algún momento. Pero es menos probable que uses directamente un tipo como [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view). O bien puedes optar por no usarlo para no tener ningún código que cambiar si aparece un tipo equivalente en la biblioteca estándar de C++.

> [!WARNING]
> También hay tipos que probablemente verás si estudias detenidamente encabezados del espacio de nombres de Windows de C++/WinRT. Un ejemplo es **winrt::param::hstring**, pero hay también ejemplos de colección. Estos existen solamente para optimizar el enlace de parámetros de entrada y producen grandes mejoras de rendimiento y hacen que la mayoría de los patrones de llamada "solo funcionen" para contenedores y tipos de C++ estándar relacionados. Estos tipos solo se usan por la proyección en casos en los que agregan el mayor valor. Están muy optimizados y no son para uso general; no intentes usarlos tú mismo. Tampoco debes utilizar nada del espacio de nombres `winrt::impl` puesto que esos son tipos de implementación y, por tanto, están sujetos a cambios. Debes seguir usando tipos estándar o tipos del [espacio de nombres winrt](/uwp/cpp-ref-for-winrt/winrt).

## <a name="important-apis"></a>API importantes
* [Estructura winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Artículos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Extensión de Visual Studio (VSIX) de C++/WinRT](https://aka.ms/cppwinrt/vsix)
* [Introducción a C++/WinRT](get-started.md)
* [Tipos de datos C++ estándar y C++/WinRT](std-cpp-data-types.md)
* [Control de cadenas en C++/WinRT](strings.md)
* [API de Windows UWP](https://docs.microsoft.com/uwp/api/)
