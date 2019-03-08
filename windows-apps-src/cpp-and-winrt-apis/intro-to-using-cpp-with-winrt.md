---
description: Una introducción a C++/WinRT&mdash;una proyección de lenguaje C++ estándar para las API de Windows Runtime.
title: Introducción a C++/WinRT
ms.date: 01/31/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, introducción
ms.localizationpriority: medium
ms.openlocfilehash: 883463f291864016ebc32f2d510936452c931366
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649700"
---
# <a name="introduction-to-cwinrt"></a>Introducción a C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT es una completa proyección de lenguaje C++17 estándar para las API de Windows Runtime (WinRT), implementada como una biblioteca basada en archivo de encabezado y diseñada para darte acceso de primera clase a la moderna API de Windows. Con C++/WinRT, puedes crear y consumir API de Windows Runtime usando cualquier compilador de C ++17 compatible con estándares. Windows SDK incluye C++/WinRT. Se introdujo en la versión 10.0.17134.0 (Windows 10, versión 1803).

C++ / c++ / WinRT está recomendada para sustituirla de Microsoft para la [C++ / c++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) proyección del lenguaje y el [biblioteca de plantillas de C++ (WRL) de Windows en tiempo de ejecución](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). La lista completa de [temas sobre C++ / c++ / WinRT](index.md#topics-about-cwinrt) incluye información sobre los interoperar con y portabilidad desde, C++ / c++ / CX y WRL.

> [!IMPORTANT]
> Dos de las partes más importantes de C++ / c++ / WinRT a tener en cuenta que se describen en las secciones [compatibilidad con SDK de C++ / c++ / WinRT](#sdk-support-for-cwinrt) y [compatibilidad con Visual Studio C++ / c++ / WinRT, XAML, la extensión VSIX y el paquete NuGet del paquete](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="language-projections"></a>Proyecciones de lenguaje
Windows Runtime se basa en las API de modelos de objetos componentes (COM) y se ha diseñado para acceder a él a través de *proyecciones de lenguaje*. Una proyección oculta los detalles del COM y proporciona una experiencia de programación más natural para un determinado lenguaje.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>Proyección de lenguaje C++/WinRT en el contenido de referencia de las API de Windows UWP
Cuando vayas a las [API de Windows UWP](https://docs.microsoft.com/uwp/api/), haz clic en el cuadro combinado **Language** en la esquina superior derecha y selecciona **C++/WinRT** para ver los bloques de sintaxis de API tal y como aparecen en la proyección de lenguaje C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Soporte de SDK para C++/WinRT
A partir de la versión 10.0.17134.0 (Windows 10, versión 1803), el Windows SDK contiene una biblioteca de C++ estándar basada en archivos de encabezado para el uso de API de Windows propias (API de Windows Runtime en espacios de nombres de Windows). C++/WinRT también se distribuye con la herramienta `cppwinrt.exe`, que puedes apuntar a un archivo de metadatos de Windows Runtime (`.winmd`) para generar una biblioteca de C++ estándar basados en archivos de encabezado que *proyecta* las API descritas en los metadatos para el consumo de código C++/WinRT. Los archivos de metadatos de Windows Runtime (`.winmd`) proporcionan una forma canónica de describir una superficie de API de Windows Runtime. Al apuntar a `cppwinrt.exe` en los metadatos, puedes generar una biblioteca para usarla con cualquier clase en tiempo de ejecución implementada en un componente de Windows Runtime de terceros o segunda parte o implementado en tu propia aplicación. Para obtener más información, consulta [Consumir API con C++/WinRT](consume-apis.md).

Con C++/WinRT, también puedes implementar tus propias clases en tiempo de ejecución con C++ estándar, sin tener que recurrir a la programación de estilo COM. Para una clase en tiempo de ejecución, solo tienes que describir tus tipos en un archivo IDL, y `midl.exe` y `cppwinrt.exe` generarán para ti tus archivos de código fuente de implementación reutilizable. Como alternativa puedes simplemente implementar interfaces derivando de una clase base de C++/WinRT. Para obtener más información, consulta [Crear API con C++/WinRT](author-apis.md).

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Compatibilidad de Visual Studio para C / c++ / WinRT, XAML, la extensión VSIX y el paquete de NuGet
Para obtener soporte técnico de Visual Studio, además de una versión de destino de Windows SDK mínima de 10.0.17134.0 (Windows 10, versión 1803), necesitará Visual Studio 2017 (al menos versión 15.6; se recomienda al menos 15.7), o Visual Studio de 2019. Si ya no ha instalado, deberá instalar el **herramientas de C++ Universal Windows Platform** opción desde dentro el instalador de Visual Studio. Y, en Windows **configuración** > **actualización \& seguridad** > **para desarrolladores**, elija el **para desarrolladores modo** opción en lugar de **agregar o quitar aplicaciones** opción.

Deberá descargar e instalar la versión más reciente de la [C++ / c++ / extensión de Visual Studio (VSIX) de WinRT](https://aka.ms/cppwinrt/vsix) desde el [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

- La extensión VSIX ofrece C++ / c++ / WinRT plantillas de proyecto y elemento en Visual Studio, por lo que puede empezar a trabajar con C++ / c++ / WinRT desarrollo.
- Además, le proporciona visualización de depuración nativo de Visual Studio (natvis) de C++ / c++ / WinRT proyecta tipos; proporcionar una experiencia similar a C# depuración. Natvis es automático para compilaciones de depuración. Puedes participar en el lanzamiento de versiones definiendo el símbolo WINRT_NATVIS.

Las plantillas de proyecto de Visual Studio para C / c++ / WinRT se describen a continuación. Cuando se crea un nuevo C++ / c++ / WinRT proyecto con la versión más reciente de la extensión VSIX instalada, el nuevo C++ / c++ / WinRT proyecto instala automáticamente el [paquete Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/). El **Microsoft.Windows.CppWinRT** paquete NuGet ofrece C++ / c++ / WinRT crear compatibilidad (propiedades de MSBuild y destinos), hacer que el proyecto que sea portátil entre una máquina de desarrollo y un agente de compilación (en el que el paquete de NuGet, y no la extensión de VSIX está instalado).

> [!IMPORTANT]
> Si tiene proyectos creados con (o actualizados para trabajar con) una versión de la extensión VSIX anteriormente que 1.0.190128.4, a continuación, consulte [versiones anteriores de la extensión VSIX](#earlier-versions-of-the-vsix-extension). Esa sección contiene información importante sobre la configuración de los proyectos, lo que necesita saber para actualizarlos para usar la versión más reciente de la extensión VSIX.

Dado que C++ / c++ / WinRT usa las características del estándar C ++ 17, es necesario que la propiedad de proyecto **C o C++** > **lenguaje** > **estándar de lenguaje C++**  >  **Estándar ISO C ++ 17 (/ STD: c ++ 17)**. También puede establecer **modo de conformidad: Sí (/ permissive-)**, que restringe aún más el código sea compatible con los estándares.

Otra propiedad de proyecto a tener en cuenta es **C/C++** > **General** > **Tratar advertencias como errores**. Establece esto a **Yes (/WX)** o **No (/WX-)** según te parezca. A veces, los archivos de origen generados por la herramienta `cppwinrt.exe` generan advertencias hasta que les agregas tu implementación.

Con el sistema establecido como se describió anteriormente, podrá crear y compilar o abrir a + C, c++ / WinRT de proyecto en Visual Studio e implementarlo.

Como alternativa, puede convertir un proyecto existente al instalar manualmente el **Microsoft.Windows.CppWinRT** paquete NuGet. Después de instalar (o actualizar a) la versión más reciente de la extensión VSIX, abra el proyecto existente en Visual Studio, haga clic en **proyecto** \> **administrar paquetes NuGet...** \> **Examinar**, escriba o pegue **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, seleccione el elemento en los resultados de búsqueda y, a continuación, haga clic en **instalar** para instalar el paquete para ese proyecto. Una vez haya agregado el paquete, obtendrá C++ / c++ / WinRT MSBuild soporte para el proyecto, incluidas invocando el `cppwinrt.exe` herramienta.

Puede identificar un proyecto que usa C++ / c++ / WinRT MSBuild soporte por la presencia de la **Microsoft.Windows.CppWinRT** paquete de NuGet instalado dentro del proyecto.

Estas son las plantillas de proyecto de Visual Studio proporcionadas por la extensión VSIX.

### <a name="windows-console-application-cwinrt"></a>Aplicación de consola de Windows (C++/WinRT)
Una plantilla de proyecto para una aplicación cliente C+/WinRT para dispositivos de escritorio de Windows, con una interfaz de usuario de consola.

### <a name="blank-app-cwinrt"></a>Aplicación vacía (C++/WinRT)
Una plantilla de proyecto para una aplicación de la plataforma universal de Windows (UWP) que tiene una interfaz de usuario XAML.

Visual Studio proporciona soporte con el compilador XAML para generar implementaciones y códigos auxiliares de encabezados a partir del archivo basado en lenguaje de definición de interfaz (IDL) (`.idl`) que se encuentra detrás de cada archivo de marcado XAML. En un archivo IDL, define las clases en tiempo de ejecución locales a las que quieras hacer referencia en las páginas XAML de tu aplicación, y después compila el proyecto una sola vez para generar las plantillas de implementación en `Generated Files` y las definiciones del tipo de código auxiliar en `Generated Files\sources`. Luego usa estas definiciones de tipo de código auxiliar como referencia para implementar tus clases en tiempo de ejecución locales. Te recomendamos que declares cada clase en tiempo de ejecución en su propio archivo IDL.

Compatibilidad de superficie de diseño XAML y de Visual Studio para C++ / c++ / WinRT está cerca de la paridad con C#. Una excepción es el **eventos** pestaña de la **propiedades** ventana. Con un C# proyecto, puede utilizar esta pestaña para agregar controladores de eventos; con C / c++ / WinRT proyecto, ese recurso no está presente. Pero ver [controlar eventos mediante el uso de delegados en C / c++ / WinRT](handle-events.md) para obtener información sobre cómo agregar controladores de eventos en el código.

### <a name="core-app-cwinrt"></a>Aplicación principal (C++/WinRT)
Una plantilla de proyecto para una aplicación de la Plataforma universal de Windows (UWP) que no usa XAML.

En su lugar, usa el encabezado del espacio de nombres de Windows de C++/WinRT para el espacio de nombres Windows.ApplicationModel.Core. Después de compilar y ejecutar, haz clic en un espacio vacío para agregar un cuadrado de color; luego haz clic en un cuadrado de color para arrastrarlo.

### <a name="windows-runtime-component-cwinrt"></a>Componente de Windows Runtime (C++/WinRT)
Una plantilla de proyecto para un componente; normalmente para el consumo desde una plataforma universal de Windows (UWP).

Esta plantilla muestra la cadena de herramientas `midl.exe` > `cppwinrt.exe`, donde se generan los metadatos de Windows Runtime (`.winmd`) a partir de IDL, y después se generan los códigos auxiliares de las implementación y el encabezado a partir de los metadatos de Windows Runtime.

En un archivo IDL, define las clases en tiempo de ejecución en tu componente, su interfaz predeterminada y cualquier otra interfaz que lo implemente. Compila el proyecto una sola vez para generar `module.g.cpp`, `module.h.cpp`, plantillas de implementación en `Generated Files` y la definición del tipo de código auxiliar en `Generated Files\sources`. Luego usa estas definiciones de tipo de código auxiliar como referencia para implementar las clases en tiempo de ejecución en tu componente. Te recomendamos que declares cada clase en tiempo de ejecución en su propio archivo IDL.

Agrupa el binario compilado de componente de Windows Runtime y su `.winmd` con la aplicación para UWP consumiéndolos.

## <a name="earlier-versions-of-the-vsix-extension"></a>Versiones anteriores de la extensión VSIX
Le recomendamos que instale (o actualizar a) la versión más reciente de la [extensión VSIX](https://aka.ms/cppwinrt/vsix). Se configura para actualizarse automáticamente de forma predeterminada. Si lo hace, y tiene proyectos creados con una versión de la extensión VSIX anteriores a 1.0.190128.4 y, a continuación, en esta sección contiene información importante acerca de cómo actualizar los proyectos para que funcione con la nueva versión. Si no se actualiza, a continuación, todavía encontrará la información de esta sección útil.

En términos de admiten Windows SDK y versiones de Visual Studio y configuración de Visual Studio, la información en el [compatibilidad con Visual Studio C++ / c++ / WinRT, XAML, la extensión VSIX y el paquete NuGet](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) sección anterior se aplica a versiones anteriores versiones de la extensión VSIX. La información siguiente describe las diferencias importantes en relación con el comportamiento y configuración de proyectos creados con (o actualizado para trabajar con) anteriores versiones.

### <a name="created-earlier-than-101810022"></a>Anteriores a 1.0.181002.2 creado
Si el proyecto se creó con una versión de la extensión VSIX anteriores a 1.0.181002.2 y, después, C++ / c++ / WinRT compilación soporte se integró en esa versión de la extensión VSIX. El proyecto tiene el `<CppWinRTEnabled>true</CppWinRTEnabled>` propiedad establecida el `.vcxproj` archivo.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Puede actualizar el proyecto instalando manualmente el **Microsoft.Windows.CppWinRT** paquete NuGet. Después de instalar (o actualizar a) la versión más reciente de la extensión VSIX, abra el proyecto en Visual Studio, haga clic en **proyecto** \> **administrar paquetes NuGet...** \> **Examinar**, escriba o pegue **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, seleccione el elemento en los resultados de búsqueda y, a continuación, haga clic en **instalar** para instalar el paquete para el proyecto.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>Creado con (o actualizar a) entre 1.0.181002.2 y 1.0.190128.3
Si el proyecto se creó con una versión de la extensión VSIX entre 1.0.181002.2 y 1.0.190128.3, ambos inclusive, la **Microsoft.Windows.CppWinRT** se instaló el paquete NuGet en el proyecto automáticamente por el proyecto plantilla. Es posible que también ha actualizado un proyecto anterior para usar una versión de la extensión VSIX en este intervalo. Si lo hizo, a continuación,&mdash;desde soporte técnico de compilación también se siguen presente en las versiones de la extensión VSIX en este intervalo&mdash;el proyecto actualizado puede o no tener el **Microsoft.Windows.CppWinRT** paquete NuGet instalado.

Para actualizar el proyecto, siga las instrucciones en la sección anterior y asegúrese de que el proyecto tiene el **Microsoft.Windows.CppWinRT** instalado el paquete de NuGet.

### <a name="invalid-upgrade-configurations"></a>Configuraciones de actualización no válidas
Con la versión más reciente de la extensión VSIX, no es válido para un proyecto tenga el `<CppWinRTEnabled>true</CppWinRTEnabled>` propiedad si no tiene también la **Microsoft.Windows.CppWinRT** instalado el paquete de NuGet. Un proyecto con esta configuración genera el mensaje de error de compilación, "C++ / c++ / WinRT VSIX ya no proporciona soporte técnico de compilación del proyecto.  Agregue una referencia de proyecto al paquete Microsoft.Windows.CppWinRT Nuget."

Como se mencionó anteriormente, C / c++ / WinRT proyecto ahora debe tener instalado el paquete de NuGet.

Puesto que la `<CppWinRTEnabled>` elemento ahora está obsoleto, opcionalmente, puede editar su `.vcxproj`y elimine el elemento. No es estrictamente necesario, pero es una opción.

Además, si su `.vcxproj` contiene `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`, a continuación, puede quitarlo para que pueda crear sin necesidad de C++ / c++ / extensión WinRT VSIX esté instalada.

## <a name="custom-types-in-the-cwinrt-projection"></a>Tipos personalizados en la proyección C++/WinRT
En C / c++ / WinRT de programación, puede usar características del lenguaje C++ estándares y [los tipos de datos estándar de C++ y C++ / c++ / WinRT](std-cpp-data-types.md)&mdash;incluidos algunos tipos de datos de la biblioteca estándar de C++. Pero también serás consciente de algunos tipos de datos personalizados en la proyección, y podrás optar por usarlos. Por ejemplo, usamos [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) en el ejemplo de código de inicio rápido de [Introducción a C++/WinRT](get-started.md).

[**winrt::com_array** ](/uwp/cpp-ref-for-winrt/com-array) es otro tipo que es probable que se usa en algún momento. Pero es menos probable que uses directamente un tipo como [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view). O bien puedes optar por no usarlo para no tener ningún código que cambiar si aparece un tipo equivalente en la biblioteca estándar de C++.

> [!WARNING]
> También hay tipos que probablemente verás si estudias detenidamente encabezados del espacio de nombres de Windows de C++/WinRT. Un ejemplo es **winrt::param::hstring**, pero hay también ejemplos de colección. Estos existen solamente para optimizar el enlace de parámetros de entrada y producen grandes mejoras de rendimiento y hacen que la mayoría de los patrones de llamada "solo funcionen" para contenedores y tipos de C++ estándar relacionados. Estos tipos solo se usan por la proyección en casos en los que agregan el mayor valor. Están muy optimizados y no son para uso general; no intentes usarlos tú mismo. Tampoco debes utilizar nada del espacio de nombres `winrt::impl` puesto que esos son tipos de implementación y, por tanto, están sujetos a cambios. Debes seguir usando tipos estándar o tipos del [espacio de nombres winrt](/uwp/cpp-ref-for-winrt/winrt).

## <a name="important-apis"></a>API importantes
* [struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++ / c++ / WinRT Visual Studio extensión (VSIX)](https://aka.ms/cppwinrt/vsix)
* [Introducción a C++ / c++ / WinRT](get-started.md)
* [Tipos de datos estándar de C++ y C / c++ / WinRT](std-cpp-data-types.md)
* [Cadena de control en C++ / c++ / WinRT](strings.md)
* [API de UWP de Windows](https://docs.microsoft.com/uwp/api/)
