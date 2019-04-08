---
description: Para ponerte al día con el uso de C++/WinRT, este artículo te guía a través de un ejemplo de código sencillo.
title: Introducción a C++/WinRT
ms.date: 10/19/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, obtener, obteniendo, iniciado
ms.localizationpriority: medium
ms.openlocfilehash: c0d11a8718f61666d6285d8a1c91b48992044b22
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602240"
---
# <a name="get-started-with-cwinrt"></a>Introducción a C++/WinRT

Para ayudarle a ponerse al día con el uso de [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), en este tema le guía a través de un ejemplo de código simple basado en un nuevo **aplicación de consola de Windows (C++ / c++ / WinRT)** proyecto. Este tema también se muestra cómo [agregar C++ / c++ / WinRT soporte a un proyecto de aplicación de escritorio de Windows](#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

> [!IMPORTANT]
> Si usa Visual Studio 2017 (versión 15.8.0 o superior) y como destino el SDK de Windows versión 10.0.17134.0 (Windows 10, versión 1803), a continuación, un recién creado C++ / c++ / WinRT proyecto puede producir un error al compilar con el error "*error C3861: 'from_abi': no se encontró el identificador*"y con otros errores que se originan en *base.h*. La solución consiste en cualquier destino una posterior (mayor cumplimiento) versión del SDK de Windows, o la propiedad de proyecto conjunto **C o C++** > **lenguaje** > **modo de conformidad: No** (Además, si **/ permissive-** aparece en la propiedad del proyecto **C o C++** > **lenguaje** > **línea de comandos**  en **opciones adicionales**, a continuación, eliminarlo).

## <a name="a-cwinrt-quick-start"></a>Inicio rápido C++/WinRT

> [!NOTE]
> Para obtener información sobre cómo instalar y usar C++ / c++ / WinRT extensión de Visual Studio (VSIX) (que proporciona compatibilidad con plantillas de proyecto) vea [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

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

[**SyndicationClient::RetrieveFeedAsync** ](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) es un ejemplo de una función de Windows en tiempo de ejecución asincrónica. El siguiente ejemplo de código recibe un objeto de operación asincrónica desde **RetrieveFeedAsync** y llama a **get** en dicho objeto para bloquear el subproceso de llamada y esperar el resultado (que es una fuente de distribución en este caso). Para más información sobre la simultaneidad y técnicas de no bloqueo, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md).

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items) es un intervalo, definido por los iteradores devueltos por **comenzar** y **final** funciones (o sus variantes constantes inversas y constante inverso). Por este motivo, puedes enumerar **Items** con una declaración `for` basada en intervalo o con la función de plantilla **std::for_each**.

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

Obtiene el texto del título del feed, como un objeto [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (consulta más detalles en [Control de cadenas en C++/WinRT](strings.md)). La **hstring** se genera entonces, mediante la función **c_str**, que refleja la trama usada con cadenas de la biblioteca estándar de C++.

Como puedes ver, C++/WinRT fomenta expresiones C++ modernas y de clase, como `syndicationItem.Title().Text()`. Es un estilo de programación diferente y más limpio que la tradicional programación COM. No tienes que inicializar COM directamente, trabaja con punteros COM.

Tampoco es necesario controlar códigos de retorno HRESULT. C++/WinRT convierte los errores HRESULT en excepciones como [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) para lograr un estilo de programación natural y moderno. Para obtener más información acerca del control de errores y los ejemplos de código, consulta [Gestión de errores con C++/WinRT](error-handling.md).

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>Modificar un proyecto de aplicación de escritorio de Windows para agregar C++ / c++ / WinRT soporte

Esta sección muestra cómo se puede agregar C++ / c++ / WinRT soporte a un proyecto de aplicación de escritorio de Windows que pueda haber. Si no tiene un proyecto de aplicación de escritorio de Windows existente, puede seguir estos pasos por la primera creación. Por ejemplo, abra Visual Studio y cree un **Visual C++** \> **Windows Desktop** \> **aplicación de escritorio de Windows** proyecto.

También puede instalar opcionalmente el [C++ / c++ / extensión de Visual Studio (VSIX) de WinRT](https://aka.ms/cppwinrt/vsix). Para obtener más información, consulte [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="set-project-properties"></a>Conjunto de propiedades del proyecto

Vaya a la propiedad del proyecto **General** \> **Windows SDK versión**y seleccione **todas las configuraciones de** y **todas las plataformas**. Asegúrese de que **Windows SDK versión** está establecido en 10.0.17134.0 (Windows 10, versión 1803) o superior.

Confirme que no esté afectada por [¿por qué no mi nuevo proyecto se compilará?](/windows/uwp/cpp-and-winrt-apis/faq).

Dado que C++ / c++ / WinRT usa características de la propiedad C ++ 17 proyecto estándar, establezca **C o C++** > **lenguaje** > **estándar de lenguaje C++** a *Estándar ISO C ++ 17 (/ STD: c ++ 17)*.

### <a name="the-precompiled-header"></a>El encabezado precompilado

Cambiar el nombre de su `stdafx.h` y `stdafx.cpp` a `pch.h` y `pch.cpp`, respectivamente. Establezca la propiedad del proyecto **C o C++** > **encabezados precompilados** > **el archivo de encabezado precompilado** a *pch.h*.

Buscar y reemplazar todo `#include "stdafx.h"` con `#include "pch.h"`.

En `pch.h`, incluir `winrt/base.h`.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>La vinculación

C++ / c++ / proyección de lenguaje de WinRT depende de ciertas funciones de Windows en tiempo de ejecución libres (no miembro) y puntos de entrada, que requieren la vinculación a la [WindowsApp.lib](/uwp/win32-and-com/win32-apis) biblioteca paraguas. En esta sección se describe tres maneras de satisfacer al vinculador.

La primera opción consiste en Agregar para Visual Studio proyecto todo de C++ / c++ / WinRT MSBuild propiedades y destinos. Para ello, instale el [paquete Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) en el proyecto. Haga clic en el proyecto en Visual Studio, abra **proyecto** \> **administrar paquetes NuGet...** \> **Examinar**, escriba o pegue **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, seleccione el elemento en los resultados de búsqueda y, a continuación, haga clic en **instalar** para instalar el paquete para ese proyecto.

También puede usar la configuración del proyecto de vínculo para vincularse explícitamente `WindowsApp.lib`. O bien, puede hacerlo en el código fuente (en `pch.h`, por ejemplo) como en este ejemplo.

```cppwinrt
#pragma comment(lib, "windowsapp")
```

Ahora puede compilar y vincular y agregar C++ / c++ / WinRT código al proyecto (por ejemplo, el código se muestra en el [c++ A c++ / WinRT de inicio rápido](#a-cwinrt-quick-start) sección posterior)

## <a name="important-apis"></a>API importantes
* [Método SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [Propiedad SyndicationFeed.Items](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [struct winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [error de winrt::HRESULT struct](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Control de errores con C++/WinRT](error-handling.md)
* [Interoperabilidad entre C++/WinRT y C++/CX](interop-winrt-cx.md)
* [Interoperabilidad entre C++/WinRT y la ABI](interop-winrt-abi.md)
* [Migrar a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md)
* [Cadena de control en C++ / c++ / WinRT](strings.md)
