---
description: Una introducción a C++/WinRT&mdash;una proyección de lenguaje C++ estándar para las API de Windows Runtime.
title: Introducción a C++/WinRT
ms.date: 01/31/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, introducción
ms.localizationpriority: medium
ms.openlocfilehash: 883463f291864016ebc32f2d510936452c931366
ms.sourcegitcommit: fde2d41ef4b5658785723359a8c4b856beae8f95
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "9079223"
---
# <a name="introduction-to-cwinrt"></a>Introducción a C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivo de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows. Con C++/WinRT, puedes crear y consumir API de Windows Runtime usando cualquier compilador de C ++17 compatible con estándares. Windows SDK incluye C++/WinRT. Se introdujo en la versión 10.0.17134.0 (Windows 10, versión 1803).

C++ / WinRT es el reemplazo recomendado de Microsoft para la [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) proyección de lenguaje y la [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). La lista completa de [temas sobre C++ / WinRT](index.md#topics-about-cwinrt) se incluye información sobre la interoperación con tanto migrar de C++ / CX y WRL.

> [!IMPORTANT]
> Dos de las partes más importantes de C++ / WinRT a tener en cuenta se describen en las secciones [soporte SDK para C++ / WinRT](#sdk-support-for-cwinrt) y [soporte de Visual Studio para C++ / WinRT, XAML, la extensión VSIX y el paquete de NuGet](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="language-projections"></a>Proyecciones de lenguaje
Windows Runtime se basa en las API de modelos de objetos componentes (COM) y se ha diseñado para acceder a él a través de *proyecciones de lenguaje*. Una proyección oculta los detalles del COM y proporciona una experiencia de programación más natural para un determinado lenguaje.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Proyección de lenguaje C++/WinRT en el contenido de referencia de las API de Windows UWP
Cuando vayas a las [API de Windows UWP](https://docs.microsoft.com/uwp/api/), haz clic en el cuadro combinado **Language** en la esquina superior derecha y selecciona **C++/WinRT** para ver los bloques de sintaxis de API tal y como aparecen en la proyección de lenguaje C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Soporte de SDK para C++/WinRT
A partir de la versión 10.0.17134.0 (Windows 10, versión 1803), el Windows SDK contiene una biblioteca de C++ estándar basada en archivos de encabezado para el uso de API de Windows propias (API de Windows Runtime en espacios de nombres de Windows). C++/WinRT también se distribuye con la herramienta `cppwinrt.exe`, que puedes apuntar a un archivo de metadatos de Windows Runtime (`.winmd`) para generar una biblioteca de C++ estándar basados en archivos de encabezado que *proyecta* las API descritas en los metadatos para el consumo de código C++/WinRT. Los archivos de metadatos de Windows Runtime (`.winmd`) proporcionan una forma canónica de describir una superficie de API de Windows Runtime. Al apuntar a `cppwinrt.exe` en los metadatos, puedes generar una biblioteca para usarla con cualquier clase en tiempo de ejecución implementada en un componente de Windows Runtime de terceros o segunda parte o implementado en tu propia aplicación. Para obtener más información, consulta [Consumir API con C++/WinRT](consume-apis.md).

Con C++/WinRT, también puedes implementar tus propias clases en tiempo de ejecución con C++ estándar, sin tener que recurrir a la programación de estilo COM. Para una clase en tiempo de ejecución, solo tienes que describir tus tipos en un archivo IDL, y `midl.exe` y `cppwinrt.exe` generarán para ti tus archivos de código fuente de implementación reutilizable. Como alternativa puedes simplemente implementar interfaces derivando de una clase base de C++/WinRT. Para obtener más información, consulta [Crear API con C++/WinRT](author-apis.md).

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Soporte de Visual Studio para C++ / WinRT, XAML, la extensión VSIX y el paquete de NuGet
Para la compatibilidad de Visual Studio, además de una versión de destino de Windows SDK mínima de 10.0.17134.0 (Windows 10, versión 1803), tendrás que Visual Studio 2017 (al menos la versión 15.6; te recomendamos al menos la 15.7), o 2019 de Visual Studio. Si ya no has instalado, tendrás que instalar la opción de **Herramientas de la plataforma Universal de Windows de C++** desde el instalador de Visual Studio. Y, en la **configuración**de Windows > **Actualizar \& seguridad** > **para desarrolladores**, elige la opción de **modo de desarrollador** en lugar de la opción de **instalar aplicaciones** .

Tendrás que descargar e instalar la versión más reciente de la [C++ / extensión de Visual Studio (VSIX) de WinRT](https://aka.ms/cppwinrt/vsix) de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

- Te ofrece la extensión VSIX C++ / WinRT plantillas de proyecto y de elementos en Visual Studio, para que pueda empezar a trabajar con C++ / WinRT desarrollo.
- Además, te ofrece visualización de depuración nativa de Visual Studio (natvis) de C++ / WinRT proyectados tipos; proporcionando una experiencia similar a la depuración de C#. Natvis es automático para compilaciones de depuración. Puedes participar en el lanzamiento de versiones definiendo el símbolo WINRT_NATVIS.

Las plantillas de proyecto de Visual Studio para C++ / WinRT se describen a continuación. Al crear un nuevo C++ / WinRT proyecto con la versión más reciente de la extensión VSIX instalada, el nuevo C++ / WinRT proyecto instala automáticamente el [paquete de Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/). Proporciona el paquete de NuGet **Microsoft.Windows.CppWinRT** C++ / WinRT de compilación admiten (MSBuild propiedades y destinos), realizar portátiles entre un equipo de desarrollo y un agente de compilación del proyecto (en el que solo el paquete de NuGet y no la extensión VSIX, se instala).

> [!IMPORTANT]
> Si tienes proyectos que se crearon con (o actualizados para funcionar con) una versión de la extensión VSIX anteriormente que 1.0.190128.4, a continuación, consulta [las versiones anteriores de la extensión VSIX](#earlier-versions-of-the-vsix-extension). Esa sección contiene información importante acerca de la configuración de los proyectos, lo que necesitas saber para realizar la actualización para usar la versión más reciente de la extensión VSIX.

Dado que C++ / WinRT usa características del estándar C ++ 17, necesita la propiedad **C/c ++** project > **idioma** > **Lenguaje de C++ estándar** > **ISO C ++ 17 Standard (/ STD: c ++ 17)**. También puedes establecer **Conformance mode: Yes (/permissive-)**, que limita todavía más tu código para que sea compatible con los estándares.

Otra propiedad de proyecto a tener en cuenta es **C/C++** > **General** > **Tratar advertencias como errores**. Establece esto a **Yes (/WX)** o **No (/WX-)** según te parezca. A veces, los archivos de origen generados por la herramienta `cppwinrt.exe` generan advertencias hasta que les agregas tu implementación.

Con el sistema establece hasta como se describió anteriormente, podrás crear y generar o abrir, C++ / WinRT del proyecto en Visual Studio e implementarla.

Como alternativa, puedes convertir un proyecto existente al instalar manualmente el paquete de NuGet **Microsoft.Windows.CppWinRT** . Después de instalar (o actualizar a) la versión más reciente de la extensión VSIX, abre el proyecto existente en Visual Studio, haz clic en el **proyecto** \> **Administrar paquetes de NuGet …**  \>  **Examinar**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de búsqueda y, a continuación, haz clic en **instalar** para instalar el paquete para el proyecto. Una vez que hayas agregado el paquete, obtendrás C++ / WinRT MSBuild soporte para el proyecto, incluido invocar la `cppwinrt.exe` herramienta.

Puedes identificar un proyecto que usa C++ / WinRT MSBuild soporte por la presencia del paquete NuGet **Microsoft.Windows.CppWinRT** instalado en el proyecto.

Estas son las plantillas de proyecto de Visual Studio proporcionadas por la extensión VSIX.

### <a name="windows-console-application-cwinrt"></a>Aplicación de consola de Windows (C++/WinRT)
Una plantilla de proyecto para una aplicación cliente C+/WinRT para dispositivos de escritorio de Windows, con una interfaz de usuario de consola.

### <a name="blank-app-cwinrt"></a>Aplicación vacía (C++/WinRT)
Una plantilla de proyecto para una aplicación de la plataforma universal de Windows (UWP) que tiene una interfaz de usuario XAML.

Visual Studio proporciona soporte con el compilador XAML para generar implementaciones y códigos auxiliares de encabezados a partir del archivo basado en lenguaje de definición de interfaz (IDL) (`.idl`) que se encuentra detrás de cada archivo de marcado XAML. En un archivo IDL, define las clases en tiempo de ejecución locales a las que quieras hacer referencia en las páginas XAML de tu aplicación, y después compila el proyecto una sola vez para generar las plantillas de implementación en `Generated Files` y las definiciones del tipo de código auxiliar en `Generated Files\sources`. Luego usa estas definiciones de tipo de código auxiliar como referencia para implementar tus clases en tiempo de ejecución locales. Te recomendamos que declares cada clase en tiempo de ejecución en su propio archivo IDL.

Soporte de superficie de diseño XAML y de Visual Studio para C++ / WinRT está cerca de la paridad con C#. Una excepción es la pestaña de **eventos** de la ventana **Propiedades** . Con un proyecto de C#, puedes usar esa ficha para agregar controladores de eventos; con C++ / WinRT proyecto, dicha instalación no está presente. Pero ver [controlar eventos usando delegados en C++ / WinRT](handle-events.md) para obtener información sobre cómo agregar controladores de eventos en el código.

### <a name="core-app-cwinrt"></a>Aplicación principal (C++/WinRT)
Una plantilla de proyecto para una aplicación de la Plataforma universal de Windows (UWP) que no usa XAML.

En su lugar, usa el encabezado del espacio de nombres de Windows de C++/WinRT para el espacio de nombres Windows.ApplicationModel.Core. Después de compilar y ejecutar, haz clic en un espacio vacío para agregar un cuadrado de color; luego haz clic en un cuadrado de color para arrastrarlo.

### <a name="windows-runtime-component-cwinrt"></a>Componente de Windows Runtime (C++/WinRT)
Una plantilla de proyecto para un componente; normalmente para el consumo desde una plataforma universal de Windows (UWP).

Esta plantilla muestra la cadena de herramientas `midl.exe` > `cppwinrt.exe`, donde se generan los metadatos de Windows Runtime (`.winmd`) a partir de IDL, y después se generan los códigos auxiliares de las implementación y el encabezado a partir de los metadatos de Windows Runtime.

En un archivo IDL, define las clases en tiempo de ejecución en tu componente, su interfaz predeterminada y cualquier otra interfaz que lo implemente. Compila el proyecto una sola vez para generar `module.g.cpp`, `module.h.cpp`, plantillas de implementación en `Generated Files` y la definición del tipo de código auxiliar en `Generated Files\sources`. Luego usa estas definiciones de tipo de código auxiliar como referencia para implementar las clases en tiempo de ejecución en tu componente. Te recomendamos que declares cada clase en tiempo de ejecución en su propio archivo IDL.

Agrupa el binario compilado de componente de Windows Runtime y su `.winmd` con la aplicación para UWP consumiéndolos.

## <a name="earlier-versions-of-the-vsix-extension"></a>Versiones anteriores de la extensión VSIX
Te recomendamos que instales (o actualizar a) la última versión de la [extensión VSIX](https://aka.ms/cppwinrt/vsix). Se configura para actualizarse de manera predeterminada. Si haces y tienes proyectos que se crearon con una versión de la extensión VSIX anteriores a 1.0.190128.4 y, a continuación, en esta sección contiene información importante sobre cómo actualizar los proyectos para que funcione con la nueva versión. Si no actualizas, a continuación, sigue encontrarás la información de esta sección útil.

En términos de compatibles con el SDK de Windows y las versiones de Visual Studio y configuración de Visual Studio, la información de la [soporte de Visual Studio para C++ / WinRT, XAML, la extensión VSIX y el paquete de NuGet](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) sección anterior se aplica a las versiones anteriores de la extensión VSIX extensión. La información siguiente describe las diferencias importantes en relación con el comportamiento y configuración de proyectos creadas con (o actualizado para que funcione con) anteriores versiones.

### <a name="created-earlier-than-101810022"></a>Crea anteriores a 1.0.181002.2
Si el proyecto se creó con una versión de la extensión VSIX anteriores a 1.0.181002.2, a continuación, C++ / WinRT soporte de compilación se creó en esa versión de la extensión VSIX. El proyecto tiene la `<CppWinRTEnabled>true</CppWinRTEnabled>` propiedad establecida el `.vcxproj` archivo.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Puedes actualizar el proyecto al instalar manualmente el paquete de NuGet **Microsoft.Windows.CppWinRT** . Después de instalar (o actualizar a) la versión más reciente de la extensión VSIX, abre el proyecto en Visual Studio, haz clic en el **proyecto** \> **Administrar paquetes de NuGet …**  \>  **Examinar**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de búsqueda y, a continuación, haz clic en **instalar** para instalar el paquete para el proyecto.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>Creado con (o actualizar a) entre 1.0.181002.2 y 1.0.190128.3
Si el proyecto se creó con una versión de la extensión VSIX entre 1.0.181002.2 y 1.0.190128.3, ambos inclusive, a continuación, el paquete de NuGet **Microsoft.Windows.CppWinRT** se instaló en el proyecto automáticamente por la plantilla de proyecto. También puede actualizar un proyecto anterior para utilizar una versión de la extensión VSIX de este intervalo. Si lo hizo, a continuación,&mdash;debido a que soporte de compilación también está todavía presente en las versiones de la extensión VSIX en este intervalo&mdash;el proyecto actualizado puede o no tener instalado el paquete de NuGet **Microsoft.Windows.CppWinRT** .

Para actualizar el proyecto, sigue las instrucciones en la sección anterior y asegúrate de que el proyecto tiene instalado el paquete de NuGet **Microsoft.Windows.CppWinRT** .

### <a name="invalid-upgrade-configurations"></a>Configuraciones de actualización no válidas
Con la versión más reciente de la extensión VSIX, no es válido para un proyecto de tener la `<CppWinRTEnabled>true</CppWinRTEnabled>` propiedad si no tiene instalado el paquete de NuGet **Microsoft.Windows.CppWinRT** también. Un proyecto con esta configuración produce el mensaje de error de compilación, "C++ / WinRT VSIX ya no proporciona soporte técnico de compilación del proyecto.  Por favor, agregar una referencia de proyecto para el paquete de Microsoft.Windows.CppWinRT Nuget".

Como se mencionó anteriormente, C++ / WinRT proyecto ahora debe tener instalado el paquete de NuGet.

Dado que la `<CppWinRTEnabled>` elemento ahora está obsoleto, opcionalmente, puedes editar tu `.vcxproj`y eliminar el elemento. No es estrictamente necesario, pero es una opción.

Además, si tu `.vcxproj` contiene `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`, a continuación, puede quitarla para que se pueden generar sin necesidad de C++ / extensión VSIX WinRT para instalarse.

## <a name="custom-types-in-the-cwinrt-projection"></a>Tipos personalizados en la proyección C++/WinRT
En su C++ / WinRT de programación, puedes usar características del lenguaje C++ estándar y [tipos de datos C++ estándar y C++ / WinRT](std-cpp-data-types.md)&mdash;incluidos algunos tipos de datos de la biblioteca estándar de C++. Pero también serás consciente de algunos tipos de datos personalizados en la proyección, y podrás optar por usarlos. Por ejemplo, usamos [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) en el ejemplo de código de inicio rápido de [Introducción a C++/WinRT](get-started.md).

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
