---
author: stevewhims
description: Para ponerte al día con el uso de C++/WinRT, este artículo te guía a través de un ejemplo de código sencillo.
title: Introducción a C++/WinRT
ms.author: stwhi
ms.date: 10/19/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, obtener, obteniendo, iniciado
ms.localizationpriority: medium
ms.openlocfilehash: 6cb8e18904f61976103689c8d83475ec248eb38b
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7570973"
---
# <a name="get-started-with-cwinrt"></a>Introducción a C++/WinRT

Para obtener acelerar con el uso de [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), en este tema te guiará a través de un ejemplo de código sencillo basado en un nuevo **aplicación de consola de Windows (C++ / WinRT)** proyecto. En este tema también se muestra cómo [Agregar C++ / WinRT soporte a un proyecto de aplicación de escritorio de Windows](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!IMPORTANT]
> Si estás usando Visual Studio 2017 (versión 15.8.0 o posterior) y el destino de Windows SDK versión 10.0.17134.0 (Windows 10, versión 1803), a continuación, recién creado C++ / WinRT proyecto puede producir un error al compilar con el error "error*C3861: 'from_abi': identificador no encuentra*"y con otros errores que se origine en *base.h*. La solución es cualquier destino una posterior (más compatible) versión del SDK de Windows, o la propiedad de proyecto de conjunto de **C/c ++** > **idioma** > **Conformance mode: No** (Además, si **/ permissive-** aparece en la propiedad de proyecto ** C/C++** > **idioma** > de**línea de comandos** en **Las opciones adicionales**, elimínela).

## <a name="a-cwinrt-quick-start"></a>Inicio rápido C++/WinRT

> [!NOTE]
> Para obtener información sobre la instalación y uso de la extensión de Visual Studio (VSIX) de C++/WinRT (la cual ofrece soporte para plantillas de proyectos, así como propiedades y destinos de MSBBuild de C++/WinRT), consulta el [Soporte de Visual Studio para C++/WinRT, y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

Crea un nuevo proyecto **Aplicación de consola de Windows (C++/WinRT)**.

Edita `pch.h` y `main.cpp` para que tenga este aspecto.

```cppwinrt
// pch.h
...
#include <iostream>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
...
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
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
```

Los encabezados que incluimos forman parte del SDK, que se encuentra dentro de la carpeta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Visual Studio incluye dicha ruta de acceso en su macro *IncludePath*. Los encabezados contienen las API de Windows proyectadas en C++/WinRT. En otras palabras, para cada tipo de Windows, C++ / WinRT define un equivalente de C++ descriptivo (denominado el *tipo proyectado*). Un tipo proyectado tiene el mismo nombre totalmente cualificado que el tipo de Windows, pero se coloca en el espacio de nombres **winrt** de C++. La colocación de estos en tu encabezado precompilado reduce el tiempo de compilación incremental.

> [!IMPORTANT]
> Siempre que quieras usar un tipo desde un espacio de nombres de Windows, incluye el archivo de encabezado de espacio de nombres de Windows C++/WinRT correspondiente, como se muestra. El encabezado *correspondiente* es el que tiene el mismo nombre que el espacio de nombres del tipo. Por ejemplo, para usar la proyección C++ / WinRT para la clase en tiempo de ejecución [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset),`#include <winrt/Windows.Foundation.Collections.h>`.

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

Las directivas `using namespace` son opcionales, pero adecuadas. El patrón mostrado anteriormente para dichas directivas (que permite la búsqueda de nombre sin calificar en el espacio de nombres **winrt**) es adecuado para cuando comienzas un proyecto nuevo y C++/WinRT es la única proyección de lenguaje que usas dentro de ese proyecto. Si, por otro lado, mezclas código de C++/WinRT con [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) o código de interfaz binaria de aplicación SDK (ABI) (desde el que vas a migrar, o con el que vas a interoperar, uno o ambos de esos modelos), consulta los temas [Interoperabilidad entre C++/WinRT y C++/ CX](interop-winrt-cx.md), [Mover a C++/WinRT desde C++/ CX](move-to-winrt-from-cx.md) e [interoperabilidad entre C++/WinRT y la ABI](interop-winrt-abi.md).

```cppwinrt
winrt::init_apartment();
```

La llamada a **winrt::init_apartment** inicializa COM; de manera predeterminada, en un contenedor multiproceso.

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

Asigna a la pila dos objetos: representan el URI del blog de Windows y un cliente de sindicación. Construimos el URI con un literal de cada ancho sencillo (consulta [Control de cadenas en C++/ WinRT](strings.md) para ver más maneras con las que trabajar con cadenas).

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) es un ejemplo de una función asincrónica de Windows Runtime. El siguiente ejemplo de código recibe un objeto de operación asincrónica desde **RetrieveFeedAsync** y llama a **get** en dicho objeto para bloquear el subproceso de llamada y esperar el resultado (que es una fuente de distribución en este caso). Para más información sobre la simultaneidad y técnicas de no bloqueo, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) es un intervalo definido por los iteradores devueltos a partir de las funciones **begin** y **end** (o sus variantes constantes, inversas y constante-inversas). Por este motivo, puedes enumerar **Items** con una declaración `for` basada en intervalo o con la función de plantilla **std::for_each**.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Obtiene el texto del título del feed, como un objeto [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (consulta más detalles en [Control de cadenas en C++/WinRT](strings.md)). La **hstring** se genera entonces, mediante la función **c_str**, que refleja la trama usada con cadenas de la biblioteca estándar de C++.

Como puedes ver, C++/WinRT fomenta expresiones C++ modernas y de clase, como `syndicationItem.Title().Text()`. Es un estilo de programación diferente y más limpio que la tradicional programación COM. No tienes que inicializar COM directamente, trabaja con punteros COM.

Tampoco es necesario controlar códigos de retorno HRESULT. C++/WinRT convierte los errores HRESULT en excepciones como [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) para lograr un estilo de programación natural y moderno. Para obtener más información acerca del control de errores y los ejemplos de código, consulta [Gestión de errores con C++/WinRT](error-handling.md).

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Modificar un proyecto de aplicación de escritorio de Windows para agregar C++ / WinRT soporte

Esta sección muestra cómo se puede agregar C++ / WinRT soporte a un proyecto de aplicación de escritorio de Windows que es posible que tengas. Si no tienes un proyecto de aplicación de escritorio de Windows existente, puede seguir junto con estos pasos, uno crear primero. Por ejemplo, abre Visual Studio y crear un **Visual C++** \> **Escritorio de Windows** \> proyecto de**Aplicación de escritorio de Windows** .

### <a name="set-project-properties"></a>Establecer las propiedades de proyecto

Ve a la propiedad **General**del proyecto \> **Versión del SDK de Windows**y selecciona **Todas las configuraciones** y **Todas las plataformas**. Asegúrate de que la **Versión del SDK de Windows** se establece en 10.0.17134.0 (Windows 10, versión 1803) o superior.

Confirma que no se ven afectadas por [por qué no compila mi proyecto nuevo?](/windows/uwp/cpp-and-winrt-apis/faq).

Dado que C++ / WinRT usa características del estándar C ++ 17, Establece la propiedad de proyecto **C o C++** > **idioma** > **Lenguaje de C++ estándar** para *ISO C ++ 17 Standard (/ STD: c ++ 17)*.

### <a name="the-precompiled-header"></a>El encabezado precompilado

Cambiar el nombre de tu `stdafx.h` y `stdafx.cpp` a `pch.h` y `pch.cpp`, respectivamente. Establece la propiedad de proyecto **C o C++** > **Encabezados precompilados** > el**Archivo de encabezado precompilado** en *pch.h*.

Buscar y todas las reemplazar `#include "stdafx.h"` con `#include "pch.h"`.

En `pch.h`, incluyen `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>Vinculación

C++ / WinRT proyección de lenguaje depende de determinadas funciones de Windows Runtime libres (no miembro) y puntos de entrada, que requieren un vínculo a la biblioteca de paraguas [WindowsApp.lib](/uwp/win32-and-com/win32-apis) . Esta sección describen tres maneras de satisfacer al enlazador.

La primera opción es agregar a tu Visual Studio proyecto todos C++ / WinRT MSBuild propiedades y destinos. Editar la `.vcxproj` de archivos, busca `<PropertyGroup Label="Globals">` y, dentro de ese grupo de propiedades, Establece la propiedad `<CppWinRTEnabled>true</CppWinRTEnabled>`.

Como alternativa, puedes usar la configuración de vínculo de proyecto para vincular explícitamente `WindowsApp.lib`.

O bien, puedes hacerlo en el código fuente (en `pch.h`, por ejemplo) de la siguiente manera.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Ahora puede compilar y vincular y agregar C++ / WinRT código al proyecto (por ejemplo, el código se muestra en la [A c++ / WinRT inicio rápido](#a-cwinrt-quick-start) sección posterior)

## <a name="important-apis"></a>API importantes
* [Método syndicationclient:: Retrievefeedasync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Propiedad SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [Estructura winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [estructura de HRESULT-error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Gestión de errores con C++/WinRT](error-handling.md)
* [Interoperabilidad entre C++/WinRT y C++/CX](interop-winrt-cx.md)
* [Interoperabilidad entre C++/WinRT y la ABI](interop-winrt-abi.md)
* [Migrar a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md)
* [Control de cadenas en C++/WinRT](strings.md)
