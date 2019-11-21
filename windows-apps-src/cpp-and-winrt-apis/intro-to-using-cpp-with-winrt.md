---
description: Una introducción a C++/WinRT, una proyección de lenguaje C++ estándar para las API de Windows Runtime.
title: Introducción a C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c ++, cpp, winrt, proyección, introducción
ms.localizationpriority: medium
ms.openlocfilehash: fd267f96ca6931252ab3130d363447ae79820108
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255100"
---
# <a name="introduction-to-cwinrt"></a>Introducción a C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/X41j_gzSwOY]

&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT es una moderna proyección de lenguaje C++17 totalmente estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivos de encabezado y diseñada para darte acceso de primera clase a la API moderna de Windows. Con C++/WinRT, puedes crear y consumir API de Windows Runtime mediante cualquier compilador de C++17 compatible con estándares. Windows SDK incluye C++/WinRT; se introdujo en la versión 10.0.17134.0 (Windows 10, versión 1803).

C++/WinRT es la sustitución recomendada de Microsoft para la proyección de lenguaje [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) y la [Biblioteca de plantillas de Windows Runtime C++ (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). En la lista completa de [temas sobre C++/WinRT](index.md#topics-about-cwinrt) se incluye información sobre la interoperabilidad y la portabilidad de C++/CX y WRL.

> [!IMPORTANT]
> Algunas de las partes más importantes de C++/WinRT que se deben tener en cuenta se describen en las secciones [Compatibilidad del SDK con C++/WinRT](#sdk-support-for-cwinrt) y [Compatibilidad de Visual Studio con C++/WinRT, XAML, la extensión VSIX y el paquete NuGet](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="language-projections"></a>Proyecciones de lenguaje
Windows Runtime se basa en las API del Modelo de objetos componentes (COM) y se ha diseñado para acceder a él a través de *proyecciones de lenguaje*. Una proyección oculta los detalles del COM y proporciona una experiencia de programación más natural para un lenguaje determinado.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Proyección de lenguaje C++/WinRT en el contenido de referencia de las API de Windows UWP
Cuando vayas a las [API de Windows UWP](https://docs.microsoft.com/uwp/api/), haz clic en el cuadro combinado **Lenguaje** en la esquina superior derecha y selecciona **C++/WinRT** para ver los bloques de sintaxis de API tal y como aparecen en la proyección de lenguaje C++/WinRT.

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Compatibilidad de Visual Studio con C++/WinRT, XAML, la extensión VSIX y el paquete NuGet
Para obtener compatibilidad con Visual Studio, necesitarás Visual Studio 2019 o Visual Studio 2017 (como mínimo la versión 15.6, pero se recomienda la 15.7). Desde el instalador de Visual Studio, instala la carga de trabajo **Desarrollo con la Plataforma universal de Windows**. En **Detalles de la instalación** > **Desarrollo de la Plataforma universal de Windows**, marca las opciones de las **herramientas de la Plataforma universal de Windows de C++ (v14x)** , si aún no lo has hecho. Además, en Windows, en **Configuración** > **Actualización y seguridad** > **Para desarrolladores**, selecciona la opción **Modo de programador** en lugar de **Efectuar instalación de prueba de aplicaciones**.

Aunque se recomienda que realices el desarrollo con las versiones más recientes de Visual Studio y Windows SDK, si usas una versión de C++/WinRT incluida con Windows SDK anterior a 10.0.17763.0 (Windows 10, versión 1809), para usar los encabezados de los espacios de nombres de Windows mencionados anteriormente, necesitarás en el proyecto una versión de destino de Windows SDK como mínimo 10.0.17134.0 (Windows 10, versión 1803).

Te conviene descargar e instalar la versión más reciente de la [Extensión de Visual Studio (VSIX) de C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) desde [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

- La extensión VSIX incluye plantillas de proyecto y elemento de C++/WinRT en Visual Studio, por lo que puedes empezar a trabajar en el desarrollo de C++/WinRT.
- Además, ofrece visualización de depuración nativa de Visual Studio (natvis) de tipos proyectados de C++/ WinRT, con lo que proporciona una experiencia similar a la depuración de C#. Natvis es automático para compilaciones de depuración. Puedes participar en el lanzamiento de versiones si defines el símbolo WINRT_NATVIS.

Las plantillas de proyecto de Visual Studio para C++/WinRT se describen en las secciones siguientes. Cuando se crea un proyecto de C++/WinRT con la versión más reciente de la extensión VSIX instalada, el nuevo proyecto de C++/WinRT instala automáticamente el [paquete NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/). El paquete NuGet **Microsoft.Windows.CppWinRT** ofrece compatibilidad con la compilación de C++/WinRT (propiedades y destinos de MSBuild), lo que permite que el proyecto sea portátil entre una máquina de desarrollo y un agente de compilación (en el que solo está instalado el paquete NuGet, y no la extensión de VSIX).

También puedes convertir un proyecto existente si instalas manualmente el paquete NuGet **Microsoft.Windows.CppWinRT**. Después de instalar la versión más reciente de la extensión VSIX (o de actualizar a esta), abre el proyecto existente en Visual Studio, haz clic en **Proyecto** \> **Administrar paquetes NuGet…** \> **Examinar**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de la búsqueda y haz clic en **Instalar** para instalar el paquete de ese proyecto. Una vez que hayas agregado el paquete, obtendrás compatibilidad de MSBuild con C++/WinRT para el proyecto, incluida la invocación de la herramienta `cppwinrt.exe`.

> [!IMPORTANT]
> Si tienes proyectos creados con una versión de la extensión VSIX anterior a 1.0.190128.4 (o actualizados para funcionar con esta), consulta [Versiones anteriores de la extensión VSIX](#earlier-versions-of-the-vsix-extension). En esta sección encontrarás información importante sobre la configuración de los proyectos que necesitarás conocer a fin de actualizarlos para usar la versión más reciente de la extensión VSIX.

- Dado que C++/WinRT usa características del estándar C++17, el paquete NuGet establece la propiedad del proyecto **C/C++**  > **Language** > **C++ Language Standard** > **ISO C++17 Standard (/std:c++17)** en Visual Studio.
- También agrega la opción del compilador [/bigobj](/cpp/build/reference/bigobj-increase-number-of-sections-in-dot-obj-file).
- Agrega la opción de compilador [/await](/cpp/build/reference/await-enable-coroutine-support) para habilitar `co_await`.
- Le indica al compilador XAML que emita código de C++/WinRT.
- También puedes establecer **Modo de conformidad: Yes (/permissive-)** , que limita todavía más tu código para que sea compatible con los estándares.
- Otra propiedad de proyecto que se debe tener en cuenta es **C/C++**  > **General** > **Tratar advertencias como errores**. Establece esto en **Sí (/WX)** o **No (/WX-)** según te parezca. A veces, los archivos de origen generados por la herramienta `cppwinrt.exe` generan advertencias hasta que les agregas tu implementación.

Con el sistema establecido tal como se ha descrito anteriormente, podrás crear y compilar (o abrir) un proyecto de C++/WinRT en Visual Studio e implementarlo.

A partir de la versión 2.0, el paquete NuGet **Microsoft.Windows.CppWinRT** incluye la herramienta `cppwinrt.exe`. Puedes apuntar la herramienta `cppwinrt.exe` a un archivo de metadatos de Windows Runtime (`.winmd`) para generar una biblioteca de C++ estándar basada en archivos de encabezado que *proyecta* las API descritas en los metadatos para el consumo desde código de C++/WinRT. Los archivos de metadatos de Windows Runtime (`.winmd`) proporcionan una forma canónica de describir una superficie de API de Windows Runtime. Al apuntar `cppwinrt.exe` a los metadatos, puedes generar una biblioteca para usarla con cualquier clase en tiempo de ejecución implementada en un componente de Windows Runtime de terceros o segunda parte o implementado en tu propia aplicación. Para obtener más información, consulta [Consumir API con C++/WinRT](consume-apis.md).

Con C++/WinRT, también puedes implementar tus propias clases en tiempo de ejecución con C++ estándar, sin tener que recurrir a la programación de estilo COM. Para una clase en tiempo de ejecución, solo tienes que describir tus tipos en un archivo IDL, y `midl.exe` y `cppwinrt.exe` generarán para ti tus archivos de código fuente de implementación reutilizable. Como alternativa, puedes simplemente implementar interfaces si derivas de una clase base de C++/WinRT. Para obtener más información, consulta [Crear API con C++/WinRT](author-apis.md).

Para conocer una lista de opciones de personalización para la herramienta `cppwinrt.exe`, a través de las propiedades del proyecto, consulta [readme](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing) del paquete NuGet Microsoft.Windows.CppWinRT.

Puedes identificar que un proyecto usa compatibilidad de MSBuild con C++WinRT porque el paquete NuGet **Microsoft.Windows.CppWinRT** está instalado en el proyecto.

Estas son las plantillas de proyecto de Visual Studio proporcionadas por la extensión VSIX.

### <a name="blank-app-cwinrt"></a>Aplicación vacía (C++/WinRT)
Plantilla de proyecto para una aplicación de la Plataforma universal de Windows (UWP) que tiene una interfaz de usuario XAML.

Visual Studio proporciona compatibilidad con el compilador XAML para generar los códigos auxiliares de los encabezados y las implementaciones a partir del archivo basado en lenguaje de definición de interfaz (IDL) (`.idl`) que se encuentra detrás de cada archivo de marcado XAML. En un archivo IDL, define las clases en tiempo de ejecución locales a las que quieras hacer referencia en las páginas XAML de tu aplicación y, después, compila el proyecto una sola vez para generar las plantillas de implementación en `Generated Files` y las definiciones del tipo de código auxiliar en `Generated Files\sources`. Luego, usa estas definiciones de subtipo de código auxiliar como referencia para implementar tus clases en tiempo de ejecución locales. Consulta [Factorizar clases en tiempo de ejecución en archivos Midl (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl).

La compatibilidad de la superficie de diseño XAML en Visual Studio 2019 con C++/WinRT es casi igual a la de C#. En Visual Studio 2019, puedes usar la pestaña **Eventos** de la ventana **Propiedades** para agregar controladores de eventos en un proyecto de C++/WinRT. También puedes agregar controladores de eventos en el código manualmente. Consulta [Controlar eventos usando delegados en C++/WinRT](handle-events.md) para obtener más información.

### <a name="core-app-cwinrt"></a>Aplicación principal (C++/WinRT)
Plantilla de proyecto para una aplicación de la Plataforma universal de Windows (UWP) que no usa XAML.

En su lugar, usa el encabezado del espacio de nombres de Windows de C++/WinRT para el espacio de nombres Windows.ApplicationModel.Core. Después de compilar y ejecutar, haz clic en un espacio vacío para agregar un cuadrado de color; luego, haz clic en un cuadrado de color para arrastrarlo.

### <a name="windows-console-application-cwinrt"></a>Aplicación de consola de Windows (C++/WinRT)
Plantilla de proyecto para una aplicación cliente C+/WinRT para escritorio de Windows, con una interfaz de usuario de consola.

### <a name="windows-desktop-application-cwinrt"></a>Aplicación de escritorio de Windows (C++/WinRT)
Plantilla de proyecto para una aplicación cliente C++/WinRT para escritorio de Windows, que muestra una clase [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri) de Windows Runtime en un **MessageBox** de Win32.

### <a name="windows-runtime-component-cwinrt"></a>Componente de Windows Runtime (C++/WinRT)
Plantilla de proyecto para un componente, normalmente para el consumo desde una Plataforma universal de Windows (UWP).

Esta plantilla muestra la cadena de herramientas `midl.exe` > `cppwinrt.exe`, donde se generan los metadatos de Windows Runtime (`.winmd`) a partir de IDL y, después, se generan los códigos auxiliares de los encabezados y las implementaciones a partir de los metadatos de Windows Runtime.

En un archivo IDL, define las clases en tiempo de ejecución de tu componente, su interfaz predeterminada y cualquier otra interfaz que implementen. Compila el proyecto una sola vez para generar `module.g.cpp`, `module.h.cpp`, plantillas de implementación en `Generated Files` y la definición del tipo de código auxiliar en `Generated Files\sources`. Luego, usa estas definiciones del tipo de código auxiliar como referencia para implementar las clases en tiempo de ejecución en tu componente. Consulta [Factorizar clases en tiempo de ejecución en archivos Midl (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl).

Agrupa el binario compilado de componente de Windows Runtime y su `.winmd` con la aplicación para UWP que los consume.

## <a name="earlier-versions-of-the-vsix-extension"></a>Versiones anteriores de la extensión VSIX
Te recomendamos que instales la versión más reciente de la [extensión VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) (o que actualices a esta). Está configurada de forma predeterminada para actualizarse automáticamente. Si lo haces y tienes proyectos creados con una versión de la extensión VSIX anterior a 1.0.190128.4, encontrarás en esta sección información importante sobre cómo actualizar esos proyectos para que funcionen con la nueva versión. Aunque no lleves a cabo la actualización, la información de esta sección te resultará útil.

En lo que respecta a las versiones admitidas de Windows SDK y Visual Studio, así como a la configuración de Visual Studio, la información contenida en la sección anterior ([Compatibilidad de Visual Studio con C++/WinRT, XAML, la extensión VSIX y el paquete NuGet](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)) se aplica a versiones anteriores de la extensión VSIX. La información siguiente describe diferencias importantes relacionadas con el comportamiento y la configuración de proyectos creados con anteriores versiones (o actualizados para funcionar con estas).

### <a name="created-earlier-than-101810022"></a>Proyectos creados con una versión anterior a 1.0.181002.2
Si el proyecto se creó con una versión de la extensión VSIX anterior a 1.0.181002.2, la compatibilidad con la compilación de C++/WinRT está integrada en esa versión de la extensión VSIX. El proyecto tiene la propiedad `<CppWinRTEnabled>true</CppWinRTEnabled>` establecida en el archivo `.vcxproj`.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Puedes actualizar el proyecto si instalas manualmente el paquete NuGet **Microsoft.Windows.CppWinRT**. Después de instalar la versión más reciente de la extensión VSIX (o de actualizar a esta), abre el proyecto en Visual Studio, haz clic en **Proyecto** \> **Administrar paquetes NuGet…** \> **Examinar**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de la búsqueda y haz clic en **Instalar** para instalar el paquete del proyecto.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>Proyectos creados con una versión entre 1.0.181002.2 y 1.0.190128.3 (o actualizados a estas)
Si el proyecto se creó con una versión de la extensión VSIX entre 1.0.181002.2 y 1.0.190128.3 (ambas incluidas), la plantilla de proyecto instaló automáticamente el paquete NuGet **Microsoft.Windows.CppWinRT** en el proyecto. También es posible que hayas actualizado un proyecto anterior para usar una versión de la extensión VSIX de este intervalo. Si lo hiciste, dado que la compatibilidad con la compilación también estaba presente en las versiones de la extensión VSIX de este intervalo, el proyecto actualizado podría tener instalado el paquete NuGet **Microsoft.Windows.CppWinRT**.

Para actualizar el proyecto, sigue las instrucciones de la sección anterior y asegúrate de que el proyecto tenga instalado el paquete NuGet **Microsoft.Windows.CppWinRT**.

### <a name="invalid-upgrade-configurations"></a>Configuraciones de actualización no válidas
Con la versión más reciente de la extensión VSIX, no es válido que un proyecto tenga la propiedad `<CppWinRTEnabled>true</CppWinRTEnabled>` si no tiene instalado también el paquete NuGet **Microsoft.Windows.CppWinRT**. Un proyecto con esta configuración genera el siguiente mensaje de error de compilación: "The C++/WinRT VSIX no longer provides project build support.  Please add a project reference to the Microsoft.Windows.CppWinRT Nuget package" (VSIX de C++WinRT ya no proporciona compatibilidad con la compilación del proyecto. Agregue una referencia de proyecto al paquete Microsoft.Windows.CppWinRT Nuget).

Como se mencionó anteriormente, ahora los proyectos C++/WinRT deben tener instalado el paquete NuGet.

Puesto que el elemento `<CppWinRTEnabled>` ahora está obsoleto, puedes editar `.vcxproj` y eliminar el elemento. No es estrictamente necesario, pero es una opción.

Además, si `.vcxproj` contiene `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`, puedes quitarlo para compilar sin que esté instalada la extensión VSIX de C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Compatibilidad del SDK con C++/WinRT
Aunque solo está presente por motivos de compatibilidad, a partir de la versión 10.0.17134.0 (Windows 10, versión 1803), Windows SDK contiene una biblioteca de C++ estándar basada en archivos de encabezado para el uso de API de Windows propias (API de Windows Runtime en espacios de nombres de Windows). Estos encabezados se encuentran en la carpeta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. A partir de Windows SDK versión 10.0.17763.0 (Windows 10, versión 1809), estos encabezados se generan automáticamente en la carpeta *$(GeneratedFilesDir)* del proyecto.

También por motivos de compatibilidad, Windows SDK incluye la herramienta `cppwinrt.exe`. Aun así, recomendamos que en su lugar instales y uses la versión más reciente de `cppwinrt.exe`, que se incluye con el paquete NuGet **Microsoft.Windows.CppWinRT**. Ese paquete y `cppwinrt.exe` se describen en las secciones anteriores.

## <a name="custom-types-in-the-cwinrt-projection"></a>Tipos personalizados en la proyección C++/WinRT
En tu programación de C++/WinRT, puedes usar características del lenguaje C++ estándar y [tipos de datos C++ estándar y C++/WinRT](std-cpp-data-types.md), incluidos algunos tipos de datos de la biblioteca estándar de C++. Pero también descubrirás algunos tipos de datos personalizados en la proyección y podrás optar por usarlos. Por ejemplo, usamos [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) en el ejemplo de código de inicio rápido de [Introducción a C++/WinRT](get-started.md).

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) es otro tipo que probablemente uses en algún momento, pero es menos probable que uses directamente un tipo como [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view). También puedes optar por no usarlo para no tener ningún código que cambiar si aparece un tipo equivalente en la biblioteca estándar de C++.

> [!WARNING]
> Además, hay tipos que probablemente verás si estudias detenidamente los encabezados del espacio de nombres de Windows de C++/WinRT. Un ejemplo es **winrt::param::hstring**, pero hay también ejemplos de colección. Estos existen solamente para optimizar el enlace de parámetros de entrada. Además, producen grandes mejoras de rendimiento y hacen que la mayoría de los patrones de llamada "solo funcionen" con contenedores y tipos de C++ estándar relacionados. La proyección solo usa estos tipos en los casos en los que agregan un mayor valor. Están muy optimizados y no están destinados a un uso general, por lo que no debes intentar usarlos. Tampoco debes usar nada del espacio de nombres `winrt::impl`, puesto que son tipos de implementación y, por tanto, están sujetos a cambios. Debes seguir usando los tipos estándar o los tipos del [espacio de nombres winrt](/uwp/cpp-ref-for-winrt/winrt).
>
> Consulta también [Pasar parámetros a los límites de la ABI](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi).

## <a name="important-apis"></a>API importantes
* [Estructura winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Extensión de Visual Studio (VSIX) de C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)
* [Introducción a C++/WinRT](get-started.md)
* [Tipos de datos de C++ estándar y C++/WinRT](std-cpp-data-types.md)
* [Control de cadenas en C++/WinRT](strings.md)
* [API de Windows UWP](https://docs.microsoft.com/uwp/api/)
