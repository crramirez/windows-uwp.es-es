---
description: Noticias y cambios en C++/WinRT.
title: Novedades de C++/WinRT
ms.date: 03/16/2020
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, news, what's, new
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3057a3d13ba1e7d368dd6bf8820710030687a04d
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80662409"
---
# <a name="whats-new-in-cwinrt"></a>Novedades de C++/WinRT

A medida que se publiquen las versiones posteriores de C++/WinRT, se describirán las novedades y los cambios en este tema.

## <a name="rollup-of-recent-improvementsadditions-as-of-march-2020"></a>Paquete acumulativo de mejoras o adiciones recientes a partir de marzo de 2020

### <a name="up-to-23-shorter-build-times"></a>Reducción de los tiempos de compilación de hasta un 23 %

Los equipos de compiladores de C++/WinRT y C++ han colaborado en todo lo posible para reducir los tiempos de compilación. Hemos leído atentamente los análisis del compilador para averiguar cómo se pueden reestructurar los elementos internos de C++/WinRT para ayudar al compilador de C++ a eliminar la sobrecarga de tiempo de compilación, y también cómo mejorar el compilador de C++ para controlar la biblioteca de C++/WinRT. C++/WinRT se ha optimizado para el compilador; y el compilador se ha optimizado para C++/WinRT.

Tomemos como ejemplo el peor escenario de creación de un encabezado precompilado (PCH) que contenga todos los encabezados del espacio de nombres de proyección de C++/WinRT.

| Version | Tamaño del encabezado PCH (bytes) | Tiempo (s) |
| - | - | - |
| C++/WinRT a partir de julio, con Visual C++ 16.3 | 3 004 104 632 | 31 |
| versión 2.0.200316.3 de C++/WinRT, con Visual C++ 16.5 | 2 393 515 336 | 24 |

Una reducción del 20 % del tamaño y del 23 % del tiempo de compilación.

### <a name="improved-msbuild-support"></a>Compatibilidad con MSBuild mejorada

Hemos invertido muchos esfuerzos en mejorar la compatibilidad con [MSBuild](/visualstudio/msbuild/msbuild?view=vs-2019) para una gran selección de escenarios diferentes.

### <a name="even-faster-factory-caching"></a>Almacenamiento en caché de fábrica aún más rápido

Hemos mejorado la inclusión de la memoria caché de fábrica para mejorar las rutas de acceso activas en línea, lo que conduce a una ejecución más rápida.

Esa mejora no afecta al tamaño del código &mdash;tal y como se describe a continuación en [Generación de código optimizada para el control de excepciones](#optimized-exception-handling-eh-code-generation). Si la aplicación usa de forma intensiva el control de excepciones de C++, puede reducir el binario mediante la opción `/d2FH4`, que está activada de forma predeterminada en los proyectos nuevos creados con Visual Studio 2019 16.3 y versiones posteriores.

### <a name="more-efficient-boxing"></a>Conversión boxing más eficiente

Si se usa en una aplicación XAML, [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) ahora es más eficaz (consulta [Conversión boxing y unboxing](/windows/uwp/cpp-and-winrt-apis/boxing)). Las aplicaciones que realizan muchas conversiones boxing también observarán una reducción del tamaño del código.

### <a name="support-for-implementing-com-interfaces-that-implement-iinspectable"></a>Compatibilidad para implementar interfaces COM que implementan IInspectable

Si necesitas implementar una interfaz COM (que no sea de Windows Runtime) que solo implemente [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable), ahora puedes hacerlo con C++/WinRT. Consulta [Interfaces COM que implementan IInspectable](https://github.com/microsoft/xlang/pull/603).

### <a name="module-locking-improvements"></a>Mejoras en el bloqueo de módulos

El control sobre el bloqueo de módulos ahora permite tanto los escenarios de hospedaje personalizados como la eliminación total de bloqueos en el nivel de módulos. Consulta [Mejoras en el bloqueo de módulos](https://github.com/microsoft/xlang/pull/583).

### <a name="support-for-non-windows-runtime-error-information"></a>Soporte para información de errores que no son de Windows Runtime

Algunas API (incluidas algunas de Windows Runtime) notifican errores sin utilizar las API que han originado los errores de Windows Runtime. En estos casos, C++/WinRT ahora vuelve a usar la información de error de COM. Consulta [Soporte de C++/WinRT para información de errores que no son de WinRT](https://github.com/microsoft/xlang/pull/582).

### <a name="enable-c-module-support"></a>Habilitar la compatibilidad con módulos de C++ 

La compatibilidad con módulos de C++ vuelve a estar disponible, pero solo de forma experimental. La característica no está completa en el compilador de C++ de momento.

### <a name="more-efficient-coroutine-resumption"></a>Reanudación más eficaz de la corrutina

Las corrutinas de C++/WinRT ya funcionan bien, pero seguimos buscando formas de mejorarlas. Consulta [Mejorar la escalabilidad de la reanudación de corrutinas](https://github.com/microsoft/xlang/pull/546).

### <a name="new-when_all-and-when_any-async-helpers"></a>Nuevas aplicaciones auxiliares asincrónicas **when_all** y **when_any**

La función auxiliar **when_all** crea un objeto [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) que se completa una vez completados todos los objetos que admiten await proporcionados. La aplicación auxiliar **when_any** crea un objeto **IAsyncAction** que se completa una vez completados todos los objetos que admiten await proporcionados. 

Consulta [Agregar la aplicación auxiliar asincrónica when_any](https://github.com/microsoft/xlang/pull/520) y [Agregar la aplicación auxiliar asincrónica when_all](https://github.com/microsoft/xlang/pull/516).

### <a name="other-optimizations-and-additions"></a>Otras optimizaciones y adiciones

Además, se han introducido numerosas correcciones de errores y optimizaciones y adiciones menores, incluidas varias mejoras para simplificar la depuración y optimizar las implementaciones internas y predeterminadas. Sigue este vínculo para obtener una lista exhaustiva: [https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed](https://github.com/microsoft/xlang/pulls?q=is%3Apr+is%3Aclosed).

## <a name="news-and-changes-in-cwinrt-20"></a>Noticias y cambios en C++/WinRT 2.0

Para más información sobre la [Extensión de Visual Studio (VSIX) de C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264), el [paquete NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) y la herramienta `cppwinrt.exe`, incluido cómo adquirirlos e instalarlos, consulta [Compatibilidad de Visual Studio con C++/WinRT, XAML, la extensión VSIX y el paquete NuGet](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>Cambios de la Extensión de Visual Studio (VSIX) de C++/WinRT para la versión 2.0

- El visualizador de depuración ahora es compatible con Visual Studio 2019, así como con Visual Studio 2017.
- Se han realizado numerosas correcciones de errores.

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Cambios realizados en el paquete NuGet Microsoft.Windows.CppWinRT para la versión 2.0

- La herramienta `cppwinrt.exe` ahora se incluye en el paquete NuGet Microsoft.Windows.CppWinRT y la herramienta genera a petición encabezados de proyección de la plataforma para cada proyecto. Por lo tanto, la herramienta `cppwinrt.exe` ya no depende de Windows SDK (aunque la herramienta todavía se distribuye con el SDK por motivos de compatibilidad).
- `cppwinrt.exe` ahora genera los encabezados de proyección debajo de cada carpeta intermedia específica de la plataforma y de la configuración ($IntDir) para permitir las compilaciones en paralelo.
- La compatibilidad con la compilación de C++/WinRT (propiedades y destinos) está ahora completamente documentada, en caso de que quieras personalizar manualmente tus archivos de proyecto. Consulta [readme](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing) del paquete NuGet Microsoft.Windows.CppWinRT.
- Se han realizado numerosas correcciones de errores.

### <a name="changes-to-cwinrt-for-version-20"></a>Cambios en C++/WinRT para la versión 2.0

#### <a name="open-source"></a>Código abierto

La herramienta `cppwinrt.exe` toma un archivo de metadatos de Windows Runtime (`.winmd`) y genera desde él una biblioteca de C++ estándar basada en archivos de encabezado que *proyecta* las API descritas en los metadatos. De este modo, puedes consumir estas API desde tu código de C++/WinRT.

Esta herramienta es ahora un proyecto de código completamente abierto, disponible en GitHub. Visita [Microsoft\/cppwinrt](https://github.com/microsoft/cppwinrt).

#### <a name="xlang-libraries"></a>Bibliotecas de xlang

Una biblioteca completamente portátil de solo encabezados (para analizar el formato de metadatos ECMA-335 utilizado por Windows Runtime) constituye la base de todas las herramientas de Windows Runtime y xlang en el futuro. En particular, también reescribimos la herramienta `cppwinrt.exe` desde cero mediante las bibliotecas de xlang. Esto proporciona consultas de metadatos mucho más precisas, que resuelven algunos problemas antiguos con la proyección del lenguaje C++/WinRT.

#### <a name="fewer-dependencies"></a>Menos dependencias

Debido al lector de metadatos de xlang, la herramienta `cppwinrt.exe` tiene menos dependencias. Esto la hace mucho más flexible, además de poderse utilizar en más escenarios, especialmente en entornos de compilación restringidos. En concreto, ya no depende de `RoMetadata.dll`.
 
Estos servicios son las dependencias para `cppwinrt.exe` 2.0.
 
- ADVAPI32.dll
- KERNEL32.dll
- SHLWAPI.dll
- XmlLite.dll

Todos los DLL están disponibles no solo en Windows 10, sino también en Windows 7 y versiones posteriores e incluso en Windows Vista. Ahora, tu antiguo servidor de compilación con Windows 7 puede ejecutar `cppwinrt.exe`, si quieres, para generar encabezados de C++ para tu proyecto. Con un poco de trabajo, puedes incluso [ejecutar C++/WinRT en Windows 7](https://github.com/kennykerr/win7) si te interesa.

Contrasta la lista anterior con estas dependencias, que tiene `cppwinrt.exe` 1.0.

- ADVAPI32.dll
- SHELL32.dll
- api-ms-win-core-file-l1-1-0.dll
- XmlLite.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- api-ms-win-core-processenvironment-l1-1-0.dll
- RoMetadata.dll
- SHLWAPI.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-timezone-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll
- OLEAUT32.dll
- api-ms-win-core-winrt-error-l1-1-0.dll
- api-ms-win-core-winrt-error-l1-1-1.dll
- api-ms-win-core-winrt-l1-1-0.dll
- api-ms-win-core-winrt-string-l1-1-0.dll
- api-ms-win-core-synch-l1-1-0.dll
- api-ms-win-core-threadpool-l1-2-0.dll
- api-ms-win-core-com-l1-1-0.dll
- api-ms-win-core-com-l1-1-1.dll
- api-ms-win-core-synch-l1-2-0.dll 

#### <a name="the-windows-runtime-noexcept-attribute"></a>El atributo `noexcept` de Windows Runtime

Windows Runtime tiene un nuevo atributo `[noexcept]`, que puedes utilizar para decorar tus métodos y propiedades en [MIDL 3.0](/uwp/midl-3/predefined-attributes). La presencia del atributo indica a las herramientas auxiliares que su implementación no inicia ninguna excepción (ni devuelve un HRESULT con error). Esto permite que las proyecciones de lenguaje optimicen la generación de código al evitar la sobrecarga del control de excepciones que se requiere para admitir las llamadas de interfaz binaria de aplicaciones (ABI) que podrían producir errores.

C+++/WinRT se aprovecha de esto al producir implementaciones `noexcept` de C++ tanto del código de consumo como de creación. Si tienes métodos o propiedades de API sin errores y te preocupa el tamaño del código, puedes investigar este atributo.

#### <a name="optimized-code-generation"></a>Generación de código optimizada

C++/WinRT ahora genera un código fuente C++ aún más eficaz (en segundo plano) para que el compilador de C++ pueda producir el código binario más pequeño y eficiente posible. Muchas de las mejoras están orientadas a reducir el costo del control de excepciones al evitar la información innecesaria. Los archivos binarios que utilizan grandes cantidades de código C++/WinRT verán una reducción de aproximadamente un 4 % en el tamaño del código. El código también es más eficaz (se ejecuta más rápido) debido al reducido número de instrucciones.

Estas mejoras dependen de una nueva característica de interoperabilidad que también tienes disponible. Todos los tipos de C++/WinRT que son propietarios de recursos ahora incluyen un constructor para asumir la propiedad directamente, con lo que se evita el enfoque anterior de dos pasos.

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>Generación de código optimizada para el control de excepciones

Este cambio complementa el trabajo realizado por el equipo de optimizadores de Microsoft C++ para reducir el costo del control de excepciones. Si usas mucho las interfaces binarias de aplicaciones (ABI) (como COM) en tu código, entonces observarás mucho código que sigue este patrón.

```cpp
int32_t Function() noexcept
{
    try
    {
        // code here constitutes unique value.
    }
    catch (...)
    {
        // code here is always duplicated.
    }
}
```

El propio C++/WinRT genera este patrón para cada API que se implementa. Con miles de funciones de API, cualquier optimización aquí puede ser significativa. En el pasado, el optimizador no detectaría que esos bloques de captura son todos idénticos, así que estaba duplicando una gran cantidad de código alrededor de cada ABI (lo que a su vez contribuía a la creencia de que usar excepciones en el código del sistema producía archivos binarios grandes). Sin embargo, a partir de Visual Studio 2019, el compilador C++ pliega todos los funclets de captura y solo almacena aquellos que son únicos. El resultado es una reducción del 18 % en el tamaño del código para los archivos binarios que dependen en gran medida de este patrón. No solo el código de control de excepciones es ahora más eficaz que el uso de códigos de retorno, sino que también la preocupación por los binarios más grandes es cosa del pasado.

#### <a name="incremental-build-improvements"></a>Mejoras en la compilación incremental

La herramienta `cppwinrt.exe` ahora compara la salida de un archivo de encabezado o código fuente generado con el contenido de cualquier archivo existente en el disco, y solo escribe en el archivo si este ha cambiado. Esto ahorra mucho tiempo con la E/S de disco y asegura que los archivos no se consideren "sucios" por el compilador de C++. El resultado es que se evita la recompilación, o se reduce, en muchos casos.

#### <a name="generic-interfaces-are-now-all-generated"></a>Ahora se generan todas las interfaces genéricas

Gracias al lector de metadatos de xlang, C++/WinRT genera ahora todas las interfaces parametrizadas o genéricas a partir de los metadatos. Las interfaces como [Windows::Foundation::Collections::IVector\<T\>](/uwp/api/windows.foundation.collections.ivector_t_) ahora se generan a partir de los metadatos en lugar de escribirse a mano en `winrt/base.h`. El resultado es que el tamaño de `winrt/base.h` se ha reducido a la mitad y que las optimizaciones se generan directamente en el código (lo que era difícil de hacer con el enfoque de escritura a mano).

> [!IMPORTANT]
> Las interfaces, como el ejemplo presentado, ahora aparecen en sus respectivos encabezados de espacio de nombres, en lugar de en `winrt/base.h`. Por lo tanto, si aún no lo has hecho, tendrás que incluir el encabezado del espacio de nombres apropiado para poder usar la interfaz.

#### <a name="component-optimizations"></a>Optimizaciones de componentes

Esta actualización agrega soporte para varias optimizaciones adicionales de participación para C++/WinRT, descritas en las secciones siguientes. Debido a que estas optimizaciones son cambios importantes (puede que tengas que hacer cambios menores en el soporte), tendrás que activarlas explícitamente. En Visual Studio, debes establecer la propiedad de proyecto **Propiedades comunes** > **C++/WinRT** > **Optimizado** en *Sí*. Esto agregará `<CppWinRTOptimized>true</CppWinRTOptimized>` al archivo de proyecto. Y tiene el mismo efecto que agregar el modificador `-opt[imize]` al invocar `cppwinrt.exe` desde la línea de comandos.

Un nuevo proyecto (de una plantilla de proyecto) utilizará `-opt` de manera predeterminada.

##### <a name="uniform-construction-and-direct-implementation-access"></a>Construcción uniforme y acceso de implementación directa

Estas dos optimizaciones permiten a tu componente acceder directamente a sus propios tipos de implementación, incluso cuando solo utiliza los tipos proyectados. No hay necesidad de usar [**make**](/uwp/cpp-ref-for-winrt/make), [**make_self**](/uwp/cpp-ref-for-winrt/make-self) ni [**get_self**](/uwp/cpp-ref-for-winrt/get-self) si simplemente quieres utilizar la superficie de la API pública. Tus llamadas se compilarán para dirigir las llamadas a la implementación e incluso podrían estar completamente insertadas.

Para obtener más información y ejemplos de código, consulta [Participación en la construcción uniforme y acceso de implementación directa](/windows/uwp/cpp-and-winrt-apis/author-apis#opt-in-to-uniform-construction-and-direct-implementation-access).

##### <a name="type-erased-factories"></a>Generadores de borrado de tipos

Esta optimización evita las dependencias #include en `module.g.cpp` de modo que no es necesario volver a compilarlas cada vez que se produce un cambio en una clase de implementación. El resultado es un mejor rendimiento de la compilación.

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>`module.g.cpp` más inteligente y eficaz para grandes proyectos con varias bibliotecas

El archivo `module.g.cpp` también contiene ahora dos asistentes adicionales que admiten composición, denominados **winrt_can_unload_now** y **winrt_get_activation_factory**. Se han diseñado para proyectos más grandes donde un archivo DLL está compuesto de un número de bibliotecas, cada una con sus propias clases de entorno de ejecución. En esa situación, debes unir manualmente **DllGetActivationFactory** y **DllCanUnloadNow** de la DLL. Estos asistentes hacen que sea mucho más fácil para ti hacer esto, al evitar errores de origen falsos. La marca `-lib` de la herramienta `cppwinrt.exe` también se puede utilizar para dar a cada biblioteca individual su propio preámbulo (en lugar de `winrt_xxx`), de modo que las funciones de cada biblioteca se puedan nombrar individualmente y, por lo tanto, combinarse de forma inequívoca.

#### <a name="coroutine-support"></a>Compatibilidad con corrutinas

La compatibilidad con corrutinas se incluye automáticamente. Anteriormente, la compatibilidad residía en varios lugares, lo que nos parecía demasiado restrictivo. Y luego, temporalmente para la versión 2.0, era necesario un archivo de encabezado `winrt/coroutine.h`, pero ya no lo es. Dado que ahora se generan las interfaces asincrónicas de Windows Runtime, en lugar de escribirse a mano, ahora residen en `winrt/Windows.Foundation.h`. Aparte de ser más fácil de mantener y de dar soporte, significa que los asistentes de corrutinas como [**resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) ya no tienen que agregarse al final de un encabezado de espacio de nombres específico. En cambio, pueden incluir de forma más natural sus dependencias. Esto permite además a **resume_foreground** admitir no solo la reanudación en una función [**Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher) determinada, sino que ahora también puede admitir la reanudación en una función [**Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue) determinada. Anteriormente, solo se podía admitir una, pero no ambas, ya que la definición solo podía residir en un espacio de nombres.

Este es un ejemplo de la compatibilidad de **DispatcherQueue**.

```cppwinrt
...
#include <winrt/Windows.System.h>
using namespace Windows::System;
...
fire_and_forget Async(DispatcherQueueController controller)
{
    bool queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(queued);

    // This is just to simulate queue failure...
    co_await controller.ShutdownQueueAsync();

    queued = co_await resume_foreground(controller.DispatcherQueue());
    assert(!queued);
}
```

Los asistentes de corrutina ahora se representan también con `[[nodiscard]]`, lo que mejora su facilidad de uso. Si te olvidas de aplicar `co_await` (o no te das cuenta de hacerlo) para que funcionen, debido a `[[nodiscard]]`, esos errores ahora producen una advertencia del compilador.

#### <a name="help-with-diagnosing-direct-stack-allocations"></a>Ayuda con el diagnóstico de las asignaciones directas (de pilas)

Dado que los nombres de las clases de proyección y de implementación son (de manera predeterminada) los mismos y solo difieren por espacio de nombres, es posible confundir el uno con el otro y crear accidentalmente una implementación en la pila, en lugar de utilizar la familia de aplicaciones auxiliares [**make**](/uwp/cpp-ref-for-winrt/make). Esto puede ser difícil de diagnosticar en algunos casos, porque el objeto puede destruirse mientras las referencias pendientes aún están presentes. Una aserción ahora recoge esto para las compilaciones de depuración. Aunque la aserción no detecta la asignación de pilas dentro de una corrutina, es útil para detectar la mayoría de estos errores.

Para obtener más información, consulta [Diagnóstico de asignaciones directas](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc).

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>Aplicaciones auxiliares de captura mejoradas y delegados variádicos

Esta actualización corrige la limitación con los asistentes de captura al admitir también los tipos proyectados. Esto aparece de vez en cuando con las API de interoperabilidad de Windows Runtime, cuando devuelven un tipo proyectado.

Esta actualización también agrega compatibilidad para [**get_strong**](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) y [**get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) al crear un delegado variádico (que no sea de Windows Runtime).

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>Compatibilidad con la destrucción diferida y con QueryInterface seguro durante la destrucción

No es raro que el destructor de un objeto de clase de tiempo de ejecución llame a un método que supera temporalmente el recuento de referencias. Cuando el recuento de referencias vuelve a cero, el objeto se destruye una segunda vez. En una aplicación XAML, es posible que necesites aplicar un elemento [**QueryInterface**](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI) en un destructor, para llamar a alguna implementación de limpieza que suba o baje por la jerarquía. Aún así, el recuento de referencias del objeto ya habrá llegado a cero, por lo que QI también devolverá el recuento de referencias.

Esta actualización agrega compatibilidad para eliminar el rebote del recuento de referencias, lo que garantiza que una vez que llegue a cero nunca se pueda recuperar; al mismo tiempo permite llamar a QueryInterface para cualquier archivo temporal que se requiera durante la destrucción. Este procedimiento no se puede evitar en ciertas aplicaciones o controles XAML, y C++/WinRT es ahora resistente a él.

Puedes aplazar la destrucción al proporcionar una función estática **final_release** en el tipo de implementación. El último puntero al objeto, que tiene un formato de **std::unique_ptr**, se pasa a **final_release**. A continuación, puedes optar por mover la propiedad de ese puntero a otro contexto. Es seguro que QI use el puntero sin desencadenar una doble destrucción. Pero el cambio neto en el recuento de referencias debe ser cero en el punto en que se destruye el objeto.

El valor devuelto de **final_release** puede ser `void`, un objeto de operación asincrónica como [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction), o **winrt::fire_and_forget**.

```cppwinrt
struct Sample : implements<Sample, IStringable>
{
    hstring ToString()
    {
        return L"Sample";
    }

    ~Sample()
    {
        // Called when the unique_ptr below is reset.
    }

    static void final_release(std::unique_ptr<Sample> self) noexcept
    {
        // Move 'self' as needed to delay destruction.
    }
};
```

En el ejemplo siguiente, cuando **MainPage** se libera (por última vez), se llama a **final_release**. Esa función pasa cinco segundos esperando (en el grupo de subprocesos) y después se reanuda mediante el **distribuidor** de la página (que requiere QueryInterface, AddRef o Release para funcionar). A continuación, se limpia un recurso en ese subproceso de interfaz de usuario. Por último, se borra **unique_ptr**, lo que hace que se llame al destructor **MainPage**. Incluso en ese destructor, se llama a **DataContext**, que requiere un elemento QI para **IFrameworkElement**.

Obviamente, no tienes que implementar **final_release** como una corrutina. Pero eso funciona y hace que sea muy simple mover la destrucción a un subproceso diferente, que es lo que está sucediendo en este ejemplo.

```cppwinrt
struct MainPage : PageT<MainPage>
{
    MainPage()
    {
    }

    ~MainPage()
    {
        DataContext(nullptr);
    }

    static IAsyncAction final_release(std::unique_ptr<MainPage> self)
    {
        co_await 5s;

        co_await resume_foreground(self->Dispatcher());
        co_await self->resource.CloseAsync();

        // The object is destructed normally at the end of final_release,
        // when the std::unique_ptr<MyClass> destructs. If you want to destruct
        // the object earlier than that, then you can set *self* to `nullptr`.
        self = nullptr;
    }
};
```

Para obtener más información, consulta [Destrucción diferida](/windows/uwp/cpp-and-winrt-apis/details-about-destructors#deferred-destruction).

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>Compatibilidad mejorada para la herencia de interfaz única de estilo COM

Al igual que para la programación de Windows Runtime, C++/WinRT también se utiliza para crear y consumir API solo COM. Esta actualización permite implementar un servidor COM donde existe una jerarquía de interfaces. Esto no es necesario para Windows Runtime, pero sí para algunas implementaciones COM.

#### <a name="correct-handling-of-out-params"></a>Control correcto de parámetros `out`

Puede ser difícil trabajar con parámetros `out`, en particular con matrices de Windows Runtime. Con esta actualización, C++/WinRT es considerablemente más sólido y resistente a errores cuando se trata de parámetros y matrices `out`; ya sea que esos parámetros lleguen a través de una proyección de lenguaje, o de un desarrollador COM que esté usando ABI básico, y que esté cometiendo el error de no inicializar las variables de forma coherente. En cualquier caso, C++/WinRT ahora hace lo correcto cuando se trata de entregar los tipos proyectados a ABI (recordando liberar cualquier recurso) y cuando se trata de poner a cero o borrar los parámetros que llegan a través de ABI.

#### <a name="events-now-handle-invalid-tokens-reliably"></a>Los eventos ahora controlan de forma confiable los tokens no válidos

La implementación de [**winrt::event**](/uwp/cpp-ref-for-winrt/event) ahora controla correctamente el caso en el que se llama al método **remove** con un valor de token no válido (un valor que no está presente en la matriz).

#### <a name="coroutine-local-variables-are-now-destroyed-before-the-coroutine-returns"></a>Las variables locales de la corrutina ahora se destruyen antes de que regrese la corrutina.

La forma tradicional de implementar un tipo de corrutina puede permitir que se destruyan las variables locales dentro de la corrutina *después* de que la corrutina regrese o se complete (en lugar de antes de la suspensión final). La reanudación de cualquier objeto waiter se aplaza hasta la suspensión final, con el fin de evitar este problema y acumular otras ventajas.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Noticias y cambios de la versión 10.0.17763.0 de Windows SDK (Windows 10, versión 1809)

En la tabla siguiente se incluyen las noticias y los cambios de C++/WinRT en la versión 10.0.17763.0 de Windows SDK (Windows 10, versión 1809).

| Característica nueva o modificada | Más información |
| - | - |
| **Cambio importante**. Para que se compile, C++/WinRT no depende de los encabezados de Windows SDK. | Consulta [Aislamiento de los archivos de encabezado de Windows SDK](#isolation-from-windows-sdk-header-files), más abajo. |
| Ha cambiado el formato del sistema de proyecto de Visual Studio. | Consulta [Procedimientos para redestinar el proyecto de C++/WinRT a una versión posterior de Windows SDK](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk). |
| Hay nuevas funciones y clases base para ayudar a pasar un objeto de colección a una función de Windows Runtime o implementar tus propias propiedades de la colección y los tipos de colección. | Consulta [Colecciones con C++/WinRT](collections.md). |
| Puedes usar la extensión de marcado [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) con tus clases del entorno de ejecución de C++/WinRT. | Para obtener más información y ejemplos de código, consulta [Introducción al enlace de datos](/windows/uwp/data-binding/data-binding-quickstart). |
| La compatibilidad con la cancelación de una corrutina te permite registrar una devolución de llamada de cancelación. | Para obtener más información y ejemplos de código, consulta [Cancelación de una operación asincrónica y devoluciones de llamadas de cancelación](concurrency-2.md#canceling-an-asynchronous-operation-and-cancellation-callbacks). |
| Al crear un delegado que apunta a una función de miembro, puedes establecer una referencia fuerte o débil al objeto actual (en lugar de un puntero *this* básico) en el punto en el que se registra el controlador. | Para obtener información y ejemplos de código, consulta la subsección **Si usas una función miembro como delegado** en la sección [Acceso seguro al puntero *this* con un delegado de control de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Se han corregido los errores que se detectaron por la mejor conformidad de Visual Studio con el estándar C++. La cadena de herramientas LLVM y Clang también se aprovechan mejor para validar la conformidad con los estándares de C++/WinRT. | Ya no te encontrarás con el problema descrito en [¿Por qué no se va a compilar mi nuevo proyecto? Estoy usando Visual Studio 2017 (versión 15.8.0 o superior) y el SDK versión 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Otros cambios.

- **Cambio importante**. [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) ahora devuelve `void*` en lugar de `HSTRING`. Puedes usar `static_cast<HSTRING>(get_abi(my_hstring));` para obtener una cadena HSTRING. Consulta [Interoperaciones con el valor HSTRING de ABI](interop-winrt-abi.md#interoperating-with-the-abis-hstring).
- **Cambio importante**. [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) ahora devuelve `void**` en lugar de `HSTRING*`. Puede usar `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` para obtener una cadena HSTRING*. Consulta [Interoperaciones con el valor HSTRING de ABI](interop-winrt-abi.md#interoperating-with-the-abis-hstring).
- **Cambio importante**. HRESULT ahora se proyecta como **winrt::hresult**. Si necesitas un valor HRESULT (para la comprobación de tipos o para admitir rasgos de tipo), puedes `static_cast` un **winrt::hresult**. De lo contrario, **winrt::hresult** se convierte en HRESULT, siempre y cuando incluyas `unknwn.h` antes de incluir cualquier encabezado de C++/WinRT.
- **Cambio importante**. La GUID ahora se proyecta como **winrt::guid**. Para las API que implementes, debes usar **winrt::guid** para los parámetros GUID. De lo contrario, **winrt::guid** se convierte en GUID, siempre y cuando incluyas `unknwn.h` antes de incluir cualquier encabezado de C++/WinRT. Consulta [Interoperaciones con la estructura GUID de ABI](interop-winrt-abi.md#interoperating-with-the-abis-guid-struct).
- **Cambio importante**. El [**constructor winrt::handle_type**](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor) se ha protegido haciéndose explícito (ahora es más difícil escribir código incorrecto con él). Si tienes que asignar un valor básico de controlador, llama a la [**función handle_type::attach** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function) en su lugar.
- **Cambio importante**. Las firmas de **WINRT_CanUnloadNow** y **WINRT_GetActivationFactory** han cambiado. No debes declarar estas funciones. En su lugar, contiene `winrt/base.h` (que se incluye automáticamente si contiene cualquier archivo de encabezado del espacio de nombres de Windows de C++/WinRT) para incluir las declaraciones de estas funciones.
- Para la [**estructura winrt::clock**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** están en desuso a favor de **from_file_time/to_file_time**.
- Las API simplificadas que esperan los parámetros **IBuffer**. La mayoría de las API prefiere colecciones o matrices. Pero pensamos que debíamos facilitar las llamadas a las API que se basan en **IBuffer**. Esta actualización proporciona acceso directo a los datos que hay detrás de una implementación de **IBuffer**. Usa la misma convención de nomenclatura de datos que la usada por los contenedores de la biblioteca estándar de C++. Esta convención también evita chocar con nombres de metadatos que normalmente comienzan con una letra mayúscula.
- Generación del código mejorada: varias mejoras para reducir el tamaño del código, mejorar la inserción y optimizar el almacenamiento en caché del generador.
- Recursividad innecesaria retirada. Cuando la línea de comandos se refiere a una carpeta, en lugar de a un `.winmd` específico, la herramienta `cppwinrt.exe` ya no busca recursivamente archivos `.winmd`. La herramienta `cppwinrt.exe` también controla ahora los duplicados de forma más inteligente, que la hace más resistente a los errores del usuario y a los archivos `.winmd` mal formados.
- Punteros inteligentes protegidos. Anteriormente, los revocadores de eventos no se pudieron revocar cuando se movía o asignaba un nuevo valor. Esto ayudó a descubrir un problema en el que las clases de punteros inteligentes no estaban tratando de forma confiable la autoasignación, con raíz en la [**plantilla de estructura winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr). **winrt::com_ptr** se ha corregido y los revocadores de eventos resueltos para controlar la semántica de movimiento correctamente para que se revoquen en el momento de la asignación.

> [!IMPORTANT]
> Se realizaron cambios importantes en la [Extensión de Visual Studio (VSIX) de C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264), tanto en la versión 1.0.181002.2 como después en la versión 1.0.190128.4. Para obtener más información sobre estos cambios y cómo afectan a los proyectos existentes, consulta [Compatibilidad de Visual Studio con C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) y [Versiones anteriores de la extensión VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

### <a name="isolation-from-windows-sdk-header-files"></a>Aislamiento de los archivos de encabezado de Windows SDK

Esto es posiblemente un cambio importante para el código.

Para su compilación, C++/WinRT ya no depende de los archivos de encabezado de Windows SDK. Los archivos de encabezado de la biblioteca de tiempo de ejecución de C (CRT) y de la biblioteca de plantillas estándar de C++ (STL) tampoco incluyen ningún encabezado de Windows SDK. Y eso mejora el cumplimiento de los estándares, evita dependencias inadvertidas y reduce en gran medida el número de macros contra las que debes protegerte.

Esta independencia significa que C++/WinRT es ahora más portátil y cumple con los estándares, y aumenta la posibilidad de que se convierta en una biblioteca multiplataforma y multicompiladores. También significa que los encabezados de C++/WinRT no se ven afectados negativamente por las macros.

Si anteriormente has dejado que C++/WinRT incluyera cualquier encabezado de Windows en tu proyecto, ahora tendrás que incluirlo tú mismo. En cualquier caso, siempre se recomienda incluir explícitamente los encabezados de los que dependes y no dejar que otra biblioteca los incluya por ti.

Actualmente, las únicas excepciones al aislamiento de los archivos de encabezado de Windows SDK son intrínsecas y numéricas. No hay problemas conocidos con estas últimas dependencias.

En el proyecto, puedes volver a habilitar la interoperabilidad con los encabezados de Windows SDK si lo necesitas. Por ejemplo, podrías implementar una interfaz COM (con raíz en [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)). Por ejemplo, incluye `unknwn.h` antes de incluir cualquier encabezado de C++/WinRT. Esto hace que la biblioteca base de C++/WinRT permita que varios enlaces admitan las interfaces COM clásicas. Para ver un ejemplo de código, consulta [Crear componentes COM con C++/WinRT](author-coclasses.md). Del mismo modo, incluye explícitamente cualquier otro encabezado de Windows SDK que declare los tipos o funciones que deseas llamar.

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Procedimientos para redestinar el proyecto de C++/WinRT a una versión posterior de Windows SDK

El método para redestinar tu proyecto que probablemente tenga como resultado un menor número de problemas con el compilador y el enlazador es también el que requiere más trabajo. Este método implica la creación de un nuevo proyecto (dirigido a la versión de Windows SDK que prefieras) y, después, la copia de archivos de tu antiguo proyecto al nuevo. Habrá secciones de tus antiguos archivos `.vcxproj` y `.vcxproj.filters` que puedes copiar para guardar y agregar archivos en Visual Studio.

Sin embargo, hay otras dos maneras de redestinar el proyecto en Visual Studio.

- En la propiedad del proyecto **General** \> **Versión del SDK de Windows**, selecciona **Todas las configuraciones** y **Todas las plataformas**. Establece la **versión de Windows SDK** en la que quieres tener como destino.
- En el **Explorador de soluciones**, haz clic con el botón derecho en el nodo del proyecto, haz clic en **Redestinar proyectos**, selecciona la versión o versiones que quieras utilizar como destino y, a continuación, haz clic en **Aceptar**.

Si encuentras algún error del compilador o del enlazador después de usar cualquiera de estos dos métodos, puedes intentar limpiar la solución (**Compilar** > **Limpiar solución** o eliminar manualmente todas las carpetas y archivos temporales) antes de intentar volver a realizar la compilación.

Si el compilador de C++ genera el "*error C2039: IUnknown': no es miembro de "\`espacio de nombres global''* ", agregue después `#include <unknwn.h>` a la parte superior del archivo `pch.h` (antes de incluir cualquier encabezado de C++/WinRT).

También es posible que debas agregar `#include <hstring.h>` después de eso.

Si el enlazador de C++ genera un "*error LNK2019: símbolo externo sin resolver_WINRT_CanUnloadNow@0 al que se hace referencia en la función _VSDesignerCanUnloadNow@0* ", entonces podrás resolverlo mediante la adición de `#define _VSDESIGNER_DONT_LOAD_AS_DLL` al archivo `pch.h`.
