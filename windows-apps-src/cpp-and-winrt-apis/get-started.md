---
description: Para ponerte al día con el uso de C++/WinRT, este tema te guía a través de un ejemplo de código sencillo.
title: Introducción a C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, get, getting, started
ms.localizationpriority: medium
ms.openlocfilehash: f38269acd9f1d6e2e830b51b3fcfa3a9014f2d7e
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219908"
---
# <a name="get-started-with-cwinrt"></a>Introducción a C++/WinRT

Para ponerte al día con el uso de [C++/WinRT](./intro-to-using-cpp-with-winrt.md), este tema te guía mediante un ejemplo de código sencillo basado en un nuevo proyecto de **aplicación de consola Windows (C++/WinRT)** . En este tema también se muestra cómo [agregar compatibilidad de C++/WinRT a un proyecto de aplicación de escritorio de Windows](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!NOTE]
> Aunque recomendamos que desarrolles con las versiones más recientes de Visual Studio y del SDK de Windows, si usas Visual Studio 2017 (versión 15.8.0 o superior) y te diriges al SDK de Windows versión 10.0.17134.0 (Windows 10, versión 1803), un proyecto recién creado de C++/WinRT puede generar un error al compilarse con el "*error C3861: 'from_abi': no se encontró el identificador*" y otros errores que se originan en *base.h*. La solución consiste en dirigirte a una versión posterior (más compatible) del SDK de Windows o establecer la propiedad del proyecto **C/C++**  > **Lenguaje** > **Modo de conformidad: No** (además, si **/permissive-** aparece en la propiedad del proyecto **C/C++**  > **Lenguaje** > **Línea de comandos** en **Opciones adicionales**, elimínalo).

## <a name="a-cwinrt-quick-start"></a>Inicio rápido de C++/WinRT

> [!NOTE]
> Para más información sobre cómo instalar Visual Studio para la implementación de C++/WinRT &mdash;incluida la instalación y el uso de la Extensión de Visual Studio (VSIX) para C++/WinRT y el paquete NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación)&mdash;, consulta [Compatibilidad de Visual Studio para C++/WinRT](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Crea un nuevo proyecto de la **aplicación de consola Windows (C++/WinRT)** .

Edita `pch.h` y `main.cpp` para que tengan este aspecto.

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
```

```cppwinrt
// main.cpp
#include "pch.h"

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
        winrt::hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

Tomemos el ejemplo de código corto anterior parte por parte; vamos a explicar qué pasa en cada parte.

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
```

Con la configuración del proyecto predeterminada, los encabezados incluidos proceden del SDK de Windows, dentro de la carpeta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Visual Studio incluye dicha ruta de acceso en su macro *IncludePath*. Pero no hay ninguna dependencia estricta en el SDK de Windows porque el proyecto (a través de la herramienta `cppwinrt.exe`) genera los mismos encabezados en la carpeta *$(GeneratedFilesDir)* del proyecto. Se cargarán desde esa carpeta si no se encuentran en otra parte o si cambias la configuración del proyecto.

Los encabezados contienen las API de Windows proyectadas en C++/WinRT. En otras palabras, para cada tipo de Windows, C++/WinRT define un equivalente de C++ descriptivo (denominado el *tipo proyectado*). Un tipo proyectado tiene el mismo nombre completo que el tipo de Windows, pero se coloca en el espacio de nombres **winrt** de C++. La colocación de estas inclusiones en tu encabezado precompilado reduce el tiempo de compilación incremental.

> [!IMPORTANT]
> Siempre que quiera usar un tipo de un espacio de nombres de Windows, debe usar `#include` para el archivo de encabezado de espacio de nombres de Windows C++/WinRT correspondiente, tal como se ha mostrado anteriormente. El encabezado *correspondiente* es el que tiene el mismo nombre que el espacio de nombres del tipo. Por ejemplo, para usar la proyección de C++ o WinRT para la clase en tiempo de ejecución [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset), incluya el encabezado `winrt/Windows.Foundation.Collections.h`.
> 
> Es habitual que un encabezado de proyección de C++/WinRT incluya automáticamente su archivo de encabezado de espacio de nombres primario. Por lo tanto, por ejemplo, `winrt/Windows.Foundation.Collections.h` incluye `winrt/Windows.Foundation.h`. Pero no debe confiar en este comportamiento, ya que es un detalle de implementación que cambia con el tiempo. Debe incluir explícitamente los encabezados que necesite.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

Las directivas `using namespace` son opcionales, pero recomendables. El patrón mostrado anteriormente para dichas directivas (que permite la búsqueda de nombres no completos en el espacio de nombres **winrt**) es adecuado cuando comienzas un proyecto nuevo y C++/WinRT es la única proyección de lenguaje que usas dentro de ese proyecto. Si, por otro lado, combinas código de C++/WinRT con [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) o código de interfaz binaria de aplicación (ABI) del SDK (desde el que vas a migrar, o con el que vas a interoperar, uno o ambos de esos modelos), consulta los temas [Interoperabilidad entre C++/WinRT y C++/CX](interop-winrt-cx.md), [Mover a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md) e [Interoperabilidad entre C++/WinRT y la ABI](interop-winrt-abi.md).

```cppwinrt
winrt::init_apartment();
```

La llamada a **winrt::init_apartment** inicializa el subproceso en Windows Runtime; de manera predeterminada, en un contenedor multiproceso. La llamada también inicializa COM.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

Asigna a la pila dos objetos: representan el URI del blog de Windows y un cliente de distribución. Construimos el URI con un literal de cadena ancho sencillo (consulta [Control de cadenas en C++/WinRT](strings.md) para ver más maneras con las que puedes trabajar con cadenas).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) es un ejemplo de una función asincrónica de Windows Runtime. El siguiente ejemplo de código recibe un objeto de operación asincrónica desde **RetrieveFeedAsync** y llama a **get** en dicho objeto para bloquear el subproceso de llamada y esperar el resultado (que es una fuente de distribución en este caso). Para más información sobre la simultaneidad y las técnicas de no bloqueo, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) es un intervalo definido por los iteradores devueltos a partir de las funciones **begin** y **end** (o sus variantes constantes, inversas y constante-inversas). Por este motivo, puedes enumerar **Items** con una instrucción `for` basada en intervalo o con la función de plantilla **std::for_each**. Cada vez que recorras en iteración una colección de Windows Runtime similar a la siguiente, tendrás que aplicar `#include <winrt/Windows.Foundation.Collections.h>`.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Obtiene el texto del título de la fuente, como objeto [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (consulta más detalles en [Control de cadenas en C++/WinRT](strings.md)). Entonces se genera **hstring**, mediante la función **c_str**, que refleja el patrón utilizado con cadenas de la biblioteca estándar de C++.

Como puedes ver, C++/WinRT fomenta expresiones de C++ modernas y de tipo clase, como `syndicationItem.Title().Text()`. Es un estilo de programación diferente y más limpio que la tradicional programación COM. No tienes que inicializar COM directamente, ni trabajar con punteros COM.

Tampoco necesitas controlar códigos de devolución HRESULT. C++/WinRT convierte los errores HRESULT en excepciones como [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) para lograr un estilo de programación natural y moderno. Para más información acerca del control de errores y ejemplos de código, consulta [Gestión de errores con C++/WinRT](error-handling.md).

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Modificación de un proyecto de aplicación de escritorio de Windows para agregar compatibilidad de C++/WinRT

En esta sección se muestra cómo agregar compatibilidad de C++/WinRT a un proyecto de aplicación de escritorio de Windows que puedas tener. Si no tienes ningún proyecto de aplicación de escritorio de Windows, puedes seguir estos pasos para crear uno. Por ejemplo, abre Visual Studio y crea un proyecto de **Visual C++**  \> **Escritorio de Windows** \> **Aplicación de escritorio de Windows**.

También puedes instalar opcionalmente la [Extensión de Visual Studio (VSIX) para C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) y el paquete NuGet. Para más información, consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="set-project-properties"></a>Configuración de propiedades del proyecto

En la propiedad del proyecto **General** \> **Versión del SDK de Windows**, selecciona **Todas las configuraciones** y **Todas las plataformas**. Asegúrate de que **Versión del SDK de Windows** esté establecido en 10.0.17134.0 (Windows 10, versión 1803) o superior.

Confirma que no te afecte [¿Por qué no se compilará mi nuevo proyecto?](./faq.md).

Dado que C++/WinRT usa características del estándar C++17, establece la propiedad del proyecto **C/C++**  > **Lenguaje** > **Estándar de lenguaje C++** en *Estándar ISO C++17 (/std:c++17)* en Visual Studio.

### <a name="the-precompiled-header"></a>Encabezado precompilado

La plantilla de proyecto predeterminada crea automáticamente un encabezado precompilado, denominado `framework.h` o `stdafx.h`. Cámbiale el nombre a `pch.h`. Si tienes un archivo `stdafx.cpp`, cámbiale el nombre a `pch.cpp`. Establece la propiedad del proyecto **C/C++**  > **Encabezados precompilados** > **Encabezado precompilado** en *Crear (/Yc)* y **Archivo de encabezado precompilado** en *pch.h*.

Busca y reemplaza todos los `#include "framework.h"` (o `#include "stdafx.h"`) por `#include "pch.h"`.

En `pch.h`, incluye `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>Vinculación

La proyección de lenguaje de C++/WinRT depende de algunas funciones de Windows Runtime y puntos de entrada libres (no miembros), que requieren la vinculación a la biblioteca paraguas [WindowsApp.lib](/uwp/win32-and-com/win32-apis). En esta sección se describen tres maneras de satisfacer al enlazador.

La primera opción consiste en agregar al proyecto de Visual Studio todas las propiedades y los destinos de MSBuild de C++WinRT. Para ello, instala el [paquete NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) en el proyecto. Abre el proyecto en Visual Studio, haz clic en **Proyecto** \> **Administrar paquetes NuGet...** \> **Busca**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de la búsqueda y haz clic en **Instalar** para instalar el paquete de ese proyecto.

También puedes usar la configuración de vínculo del proyecto para vincular explícitamente `WindowsApp.lib`. O bien, puedes hacerlo en código fuente (en `pch.h`, por ejemplo), como en este ejemplo.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Ya puedes compilar y vincular, y agregar código de C++/WinRT al proyecto (por ejemplo, código similar al que se muestra en la sección [Inicio rápido de C++/WinRT](#a-cwinrt-quick-start) anterior).

## <a name="the-three-main-scenarios-for-cwinrt"></a>Los tres escenarios principales de C++/WinRT

A medida que uses C++/WinRT y te familiarices con él, y consultes el resto de la documentación que se incluye aquí, es probable que observes que hay tres escenarios principales, como se describe en las secciones siguientes.

### <a name="consuming-windows-runtime-apis-and-types"></a>Consumo de tipos y API de Windows Runtime

En otras palabras, *usar* o *llamar* a las API. Por ejemplo, realizar llamadas API para comunicarse mediante Bluetooth, para transmitir y presentar vídeo, para integrar con el shell de Windows, etc. C++/WinRT es totalmente compatible con esta categoría de escenario. Para más información, consulta [Consumir API con C++/WinRT](./consume-apis.md).

### <a name="authoring-windows-runtime-apis-and-types"></a>Creación de tipos y API de Windows Runtime

En otras palabras, *producir* tipos y API. Por ejemplo, producir los tipos de API que se describen en la sección anterior, o bien las API de gráficos, las API de almacenamiento y del sistema de archivos, las API de redes, etc. Para obtener más información, consulta [Crear API con C++/WinRT](./author-apis.md).

Crear API con C++/WinRT es un poco más complicado que consumirlas, porque debes usar IDL para definir la forma de la API antes de poder implementarla. Hay un tutorial para hacerlo en [Controles de XAML; enlazar a una propiedad de C++/WinRT](./binding-property.md).

### <a name="xaml-applications"></a>Aplicaciones XAML

En este escenario se trata la compilación de aplicaciones y controles en el marco de trabajo de la interfaz de usuario de XAML. Trabajar en una aplicación XAML equivale a una combinación de consumo y creación. Pero, dado que XAML es el marco de trabajo de la interfaz de usuario dominante en Windows hoy en día y su influencia en Windows Runtime es proporcional a eso, merece su propia categoría de escenario.

Ten en cuenta que XAML funciona mejor con los lenguajes de programación que ofrecen reflexión. En C++/WinRT, a veces tienes que esforzarte un poco más para interoperar con el marco XAML. Todos estos casos se describen en la documentación. Los mejores artículos para empezar son [Controles de XAML; enlazar a una propiedad de C++/WinRT](./binding-property.md) y [Controles (basados en modelo) personalizados de XAML con C++/WinRT](./xaml-cust-ctrl.md).

## <a name="sample-apps-written-in-cwinrt"></a>Aplicaciones de ejemplo escritas en C++/WinRT

Consulte [¿Dónde puedo encontrar aplicaciones de ejemplo de C++/WinRT?](./faq.md#where-can-i-find-cwinrt-sample-apps)

## <a name="important-apis"></a>API importantes
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Propiedad SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [Estructura winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Estructura winrt::hresult-error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Control de errores con C++/WinRT](error-handling.md)
* [Interoperabilidad entre C++/WinRT y C++/CX](interop-winrt-cx.md)
* [Interoperabilidad entre C++/WinRT y la ABI](interop-winrt-abi.md)
* [Migrar a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md)
* [Control de cadenas en C++/WinRT](strings.md)