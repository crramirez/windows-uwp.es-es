---
description: Noticias y cambios en C++/WinRT.
title: Novedades de C / c++ / WinRT
ms.date: 04/02/2019
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, noticias, lo que de, new
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 8ee10450a7a346c1ae032240aaecc65e7f87822d
ms.sourcegitcommit: 940645c705865ba9635ccae2da9d917420faf608
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58812614"
---
# <a name="whats-new-in-cwinrt"></a>Novedades de C / c++ / WinRT

## <a name="news-and-changes-in-cwinrt-20"></a>Noticias y los cambios, en C++WinRT 2.0

Para obtener más información acerca de la [ C++extensión Visual Studio (VSIX) de WinRT](https://aka.ms/cppwinrt/vsix), el [paquete Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)y el `cppwinrt.exe` herramienta&mdash;incluido cómo adquirir e instalarlas&mdash;vea [compatibilidad con Visual Studio C++/WinRT, XAML, la extensión VSIX y el paquete NuGet](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

### <a name="changes-to-the-cwinrt-visual-studio-extension-vsix-for-version-20"></a>Cambia a la C++WinRT extensión Visual Studio (VSIX) para la versión 2.0

- El visualizador de depuración ahora es compatible con Visual Studio de 2019; Además de continuar la compatibilidad con Visual Studio 2017.
- Se han realizado numerosas correcciones de errores.

### <a name="changes-to-the-microsoftwindowscppwinrt-nuget-package-for-version-20"></a>Cambios realizados en el paquete Microsoft.Windows.CppWinRT NuGet para la versión 2.0

- El `cppwinrt.exe` herramienta ahora se incluye en el paquete Microsoft.Windows.CppWinRT NuGet y la herramienta genera los encabezados de la proyección de Frameworks para cada proyecto bajo demanda. Por lo tanto, el `cppwinrt.exe` herramienta ya no depende del SDK de Windows (aunque todavía, la herramienta se distribuye con el SDK por motivos de compatibilidad).
- `cppwinrt.exe` Ahora genera los encabezados de proyección debajo de cada carpeta intermedio específicas de la plataforma y configuración ($IntDir) para habilitar las compilaciones en paralelo.
- El C++/WinRT de soporte técnico de compilación (archivos props/targets) está ahora totalmente documentada, en caso de que desea personalizar manualmente los archivos de proyecto. Consulte [paquete de NuGet Microsoft.Windows.CppWinRT](https://github.com/Microsoft/xlang/tree/user/sjones/cppwinrt_nuget/src/package/nuget).
- Se han realizado numerosas correcciones de errores.

### <a name="changes-to-cwinrt-for-version-20"></a>Cambia a C++/WinRT para la versión 2.0

#### <a name="open-source"></a>Código abierto

El `cppwinrt.exe` herramienta toma los metadatos de un Runtime de Windows (`.winmd`) de archivo y genera a partir de él un estándar basado en archivos de encabezado C++ biblioteca que *proyectos* las API descritas en los metadatos. De este modo, puede consumir estas API desde su C++/WinRT código.

Esta herramienta es ahora un proyecto de código completamente abierto, disponible en GitHub. Visite [Microsoft\/xlang](https://github.com/Microsoft/xlang)y, a continuación, haga clic en a **src** > **herramienta** > **cppwinrt**.

#### <a name="xlang-libraries"></a>bibliotecas de XLANG

Una biblioteca solo de encabezados completamente portátil (para analizar el formato de metadatos ECMA-335 utilizado por el tiempo de ejecución de Windows) constituye la base de todas las herramientas en el futuro de xlang y en tiempo de ejecución de Windows. En concreto, también se reescribe el `cppwinrt.exe` herramienta a desde el principio mediante las bibliotecas de xlang. Esto ofrece mucho más precisas consultas de metadatos, solucionar algunos viejos problemas con la C++/WinRT proyección del lenguaje.

#### <a name="fewer-dependencies"></a>Menos dependencias

Debido al lector de metadatos xlang, el `cppwinrt.exe` propia herramienta tiene menos dependencias. Esto facilita mucho más flexible, además de ser utilizable en escenarios más&mdash;especialmente en restringida a crear entornos. En concreto, que ya no se basa en `RoMetadata.dll`.
 
Estas son las dependencias para `cppwinrt.exe` 2.0.
 
- api-ms-win-core-processenvironment-l1-1-0.dll
- api-ms-win-core-libraryloader-l1-2-0.dll
- XmlLite.dll
- api-ms-win-core-memory-l1-1-0.dll
- api-ms-win-core-handle-l1-1-0.dll
- api-ms-win-core-file-l1-1-0.dll
- SHLWAPI.dll
- ADVAPI32.dll
- KERNEL32.dll
- api-ms-win-core-rtlsupport-l1-1-0.dll
- api-ms-win-core-processthreads-l1-1-0.dll
- api-ms-win-core-heap-l1-1-0.dll
- api-ms-win-core-console-l1-1-0.dll
- api-ms-win-core-localization-l1-2-0.dll

Contraste con estas dependencias, que `cppwinrt.exe` tiene 1.0.

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

#### <a name="the-windows-runtime-noexcept-attribute"></a>El tiempo de ejecución de Windows `noexcept` atributo

El tiempo de ejecución de Windows tiene un nuevo `[noexcept]` atributo, que puede utilizar para decorar sus métodos y propiedades de [MIDL 3.0](/uwp/midl-3/predefined-attributes). Indica la presencia del atributo para que su implementación no genere una excepción de herramientas de compatibilidad (ni devuelva un error HRESULT). Esto permite que las proyecciones de lenguaje optimizar la generación de código, ya que evita la sobrecarga de control de excepciones que se necesita para admitir llamadas (ABI) de interfaz binaria de aplicación que podrían producir errores.

C++/ WinRT aprovecha las ventajas de esto mediante la generación de C++ `noexcept` implementaciones de consumo y creación de código. Si dispone de métodos de API o las propiedades que son libres de errores y usted le preocupa sobre el tamaño del código, a continuación, puede investigar este atributo.

#### <a name="optimized-code-generation"></a>Generación de código optimizada

C++/ WinRT genera ahora aún más eficaz C++ origen código (en segundo plano) hasta que el C++ compilador puede generar más pequeño y más eficaz código binario posibles. Muchas de las mejoras están orientados a reducir el costo de control de excepciones, ya que evita innecesaria de información de desenredo. Los archivos binarios que usan grandes cantidades de C++/WinRT código verá aproximadamente una reducción 4% del tamaño del código. El código también es más eficaz (se ejecuta más rápido) porque el recuento de instrucciones reducido.

Estas mejoras se basan en una nueva característica de interoperabilidad que también está disponible para usted. Todos los C++/tipos de WinRT que son propietarios de recursos ahora incluyen un constructor para tomar posesión directamente, evitar el enfoque de dos pasos anteriores.

```cppwinrt
ABI::Windows::Foundation::IStringable* raw = ...

IStringable projected(raw, take_ownership_from_abi);

printf("%ls\n", projected.ToString().c_str());
```

#### <a name="optimized-exception-handling-eh-code-generation"></a>Control de excepciones (EH) código-generación optimizada

Este cambio complementa el trabajo que se ha realizado por Microsoft C++ equipo optimizador para reducir el costo del control de excepciones. Si usa mucho interfaces binarias de aplicación (ABI) (por ejemplo, COM) en el código, observará una gran cantidad de código que se sigue este patrón.

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

C++/ WinRT propio genera este patrón para todas las API que se implementa. Con miles de funciones de la API, aquí cualquier optimización puede ser considerable. En el pasado, el optimizador no detecta que los bloques catch todos sean idénticos, por lo que era duplicar una gran cantidad de código alrededor de cada ABI (que a su vez han contribuido a la creencia de que el uso de excepciones en código del sistema genera archivos binarios grandes). Sin embargo, desde Visual Studio de 2019, el C++ compilador contrae todas esas catch funclets y almacena solo aquellos que son únicos. El resultado es una reducción de 18% adicional y el tamaño del código para archivos binarios que dependen en gran medida de este patrón. No solo código EH ahora es más eficaz que usar códigos de retorno, sino también la preocupación acerca de los archivos binarios de mayor tamaño es ahora una cosa del pasado.

#### <a name="incremental-build-improvements"></a>Mejoras de generación incremental

El `cppwinrt.exe` herramienta ahora compara la salida de un archivo de encabezado/código fuente generado con el contenido de los archivos existentes en el disco, y solo escribe el archivo si el archivo ha cambiado en realidad. Esto ahorra mucho tiempo con E/S de disco y se garantiza que los archivos no se consideran "sucios" por el C++ compilador. El resultado es que la recompilación es evitar, o se reduce, en muchos casos.

#### <a name="generic-interfaces-are-now-all-generated"></a>Ahora, las interfaces genéricas son todos generado

Debido al lector de metadatos xlang, C++/WinRT genera ahora todas las interfaces con parámetros o genéricas, de los metadatos. Las interfaces como [Windows::Foundation::Collections::IVector\<T\> ](/uwp/api/windows.foundation.collections.ivector_t_) están ahora generado a partir de los metadatos en lugar de escrito a mano `winrt/base.h`. El resultado es que el tamaño de `winrt/base.h` se ha eliminado en la mitad, y que se generan las optimizaciones del botón secundario en el código (que era difícil de hacer con el enfoque escribimos).

> [!IMPORTANT]
> Interfaces, como el ejemplo presentado aparecen ahora en los encabezados del espacio de nombres correspondientes, en lugar de en `winrt/base.h`. Por lo tanto, si aún no lo ha hecho, tendrá que incluir el encabezado del espacio de nombres adecuado para usar la interfaz.

#### <a name="component-optimizations"></a>Optimizaciones de componente

Esta actualización agrega compatibilidad para varias participar en las optimizaciones adicionales para C++/WinRT, se describe en las secciones siguientes. Dado que estas optimizaciones son los últimos cambios (que es posible que deba realizar cambios menores para admitir), deberá activar de forma explícita mediante la `cppwinrt.exe` la herramienta `-opt` marca.

Usará un nuevo proyecto (desde una plantilla de proyecto) `-opt` de forma predeterminada.

##### <a name="uniform-construction-and-direct-implementation-access"></a>Construcción uniforme y el acceso de implementación directa

Estos dos optimizaciones permiten el acceso directo a sus propios tipos de implementación, incluso cuando se usa solo los tipos proyectados. No hay ninguna necesidad de usar [ **realizar**](/uwp/cpp-ref-for-winrt/make), [ **make_self**](/uwp/cpp-ref-for-winrt/make-self), ni [ **get_self** ](/uwp/cpp-ref-for-winrt/get-self) si desea usar la superficie de API pública. Las llamadas se compilarán como llamadas directas a la implementación y, incluso los que podrían ser completamente insertados.

##### <a name="type-erased-factories"></a>Generadores de tipo borrado

Esta optimización se evita la #include dependencias en `module.g.cpp` para que no necesitan compilarse cada vez que se produce cualquier clase de implementación solo cambiar. El resultado es el rendimiento de la compilación mejorada.

#### <a name="smarter-and-more-efficient-modulegcpp-for-large-projects-with-multiple-libs"></a>Más inteligentes y más eficaz `module.g.cpp` para proyectos grandes con varias bibliotecas

El `module.g.cpp` archivo también contiene ahora dos asistentes que se pueden componer adicionales, denominados **winrt_can_unload_now**, y **winrt_get_activation_factory**. Estos se han diseñado para proyectos grandes, donde un archivo DLL se compone de un número de bibliotecas, cada uno con sus propias clases de tiempo de ejecución. En esa situación, debe unir manualmente la DLL **DllGetActivationFactory** y **DllCanUnloadNow**. Estos elementos auxiliares facilitan mucho más fácil de hacer eso, al evitar errores falsos de origen. El `cppwinrt.exe` la herramienta `-lib` marca también puede utilizarse para proporcionar su propio preámbulo a cada lib individual (en lugar de `winrt_xxx`) para que las funciones de cada lib tenga nombre individualmente y, por tanto, se combinan de forma inequívoca.

#### <a name="new-winrtcoroutineh-header"></a>Nuevo `winrt/coroutine.h` encabezado

El `winrt/coroutine.h` encabezado es el nuevo lugar para todos C++la compatibilidad con corrutinas de /WinRT. Anteriormente, esta compatibilidad residía en distintos lugares, lo que pensamos fue muy restrictivo. Puesto que ahora se generan las interfaces asincrónicas de Windows en tiempo de ejecución, en lugar de escritos a mano, que ahora residen en `winrt/Windows.Foundation.h`. Aparte de ser más fácil de mantener y dar soporte, significa que esa corrutina aplicaciones auxiliares como [ **resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground) ya no se ha agregado al final de un encabezado de espacio de nombres específico. En su lugar, pueden incluir más de forma natural sus dependencias. Esto permite que más **resume_foreground** para admitir no solo reanudar en un determinado [ **Windows::UI::Core::CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher), pero puede ahora también admite reanudar en un determinado [ **Windows::System::DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue). Anteriormente, solo uno podría tener soporte técnico; pero no ambos, ya que la definición solo puede residir en un espacio de nombres.

Este es un ejemplo de la **DispatcherQueue** admite.

```cppwinrt
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

Las aplicaciones auxiliares de corrutina ahora se representan también con `[[nodiscard]]`, lo que mejora su facilidad de uso. Si olvida (o no se dan cuenta deberá) `co_await` ellos para que funcione, después, debido a `[[nodiscard]]`, tales errores de regla ahora producen una advertencia del compilador.

#### <a name="help-with-diagnosing-stack-allocations"></a>Ayudar a diagnosticar las asignaciones de la pila

Dado que los nombres de clase proyectados y la implementación son (de forma predeterminada), los mismos y solo difieren en el espacio de nombres, es posible confundir el uno del otro y crear accidentalmente una implementación en la pila, en lugar de mediante el [ **realizar** ](/uwp/cpp-ref-for-winrt/make) familia de aplicaciones auxiliares. Esto puede ser difícil de diagnosticar en algunos casos, porque es posible que se destruya el objeto mientras haya referencias pendientes de vuelo. Una aserción ahora toma esto, para las compilaciones de depuración. Mientras que la aserción no detecta la asignación de pila dentro de una corrutina, sin embargo, es útil para detectar la mayoría de estos errores.

#### <a name="improved-capture-helpers-and-variadic-delegates"></a>Las aplicaciones auxiliares de captura mejorada y delegados variádicas

Esta actualización corrige la limitación con las aplicaciones auxiliares de captura, permitiendo así tipos proyectados. Esto aparece ahora y, a continuación, con la API de interoperabilidad de Windows en tiempo de ejecución, cuando se devuelven un tipo proyectado.

Esta actualización también agrega compatibilidad para [ **get_strong** ](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function) y [ **get_weak** ](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function) al crear un delegado variádicas (no Windows en tiempo de ejecución).

#### <a name="support-for-deferred-destruction-and-safe-qi-during-destruction"></a>Compatibilidad con QI seguro durante la destrucción y la destrucción aplazada

Una aplicación XAML se puede obtener en dificultades debido a su necesidad de realizar una [ **QueryInterface** ](/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) (QI) en un destructor, para poder llamar a alguna implementación limpieza hacia arriba o hacia abajo en la jerarquía. Sin embargo, que llamada implica un QI después de recuento de referencias del objeto ya ha había llegado a cero. Esta actualización agrega compatibilidad para contra rebotes el recuento de referencias, lo que garantiza una vez que llega a cero que nunca se pueden recuperar; mientras sigue permitiendo el QI para temporales que se necesita durante la destrucción. Este procedimiento es inevitable en ciertas aplicaciones/controles de XAML, y C++/WinRT ahora es resistente a él.

Se puede aplazar la destrucción proporcionando estático **final_release** de función y pasar la propiedad de la **unique_ptr** a algún otro contexto.

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

    static void final_release(std::unique_ptr<Sample> ptr) noexcept
    {
        // Move 'ptr' as needed to delay destruction.
    }
};
```

En el ejemplo siguiente, una vez el **MainPage** se libera (por última vez), **final_release** se llama. Que invierte en función de cinco segundos de espera (en el grupo de subprocesos), y, a continuación, se reanuda mediante la página **distribuidor** (que requiere el QI, AddRef/Release funcione). A continuación, borra el **unique_ptr**, lo que hace que el **MainPage** destructor llama realmente a obtener. Incluso aquí, **DataContext** se llama, lo que requiere un QI para **IFrameworkElement**. Obviamente, no tiene que implementar su **final_release** como una corrutina. Pero eso funciona y resulta muy sencillo mover la destrucción a un subproceso diferente.

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

    static IAsyncAction final_release(std::unique_ptr<MainPage> ptr)
    {
        co_await 5s;

        co_await resume_foreground(ptr->Dispatcher());

        ptr = nullptr;
    }
};
```

#### <a name="improved-support-for-com-style-single-interface-inheritance"></a>Compatibilidad mejorada para la herencia única interfaz de estilo com.

Así como para la programación de Windows en tiempo de ejecución, C++/WinRT también se utiliza para crear y consumir API COM solo. Esta actualización hace posible implementar un servidor COM donde existe una jerarquía de la interfaz. Esto no es necesario para el tiempo de ejecución de Windows; pero es necesario para algunas implementaciones de COM.

#### <a name="correct-handling-of-out-params"></a>Correcto control de `out` params

Puede ser complicado trabajar con `out` params; especialmente las matrices de Windows en tiempo de ejecución. Con esta actualización, C++/WinRT es considerablemente más sólida y resistente a errores en cuanto a `out` params y matrices; si esos parámetros llegan a través de una proyección de lenguaje, o de un desarrollador de COM quién está usando la ABI sin procesar y quién está realizando Error de no inicializar las variables de forma coherente. En cualquier caso, C++/WinRT ahora hace lo correcto en cuanto a entregar tipos proyectados a la ABI (recordando liberar cualquier recurso) y en cuanto a pone a cero o borrarlos de los parámetros que llegan a través de ABI.

#### <a name="events-now-handle-invalid-tokens-reliably"></a>Eventos ahora controlan forma confiable los tokens no válidos

El [ **winrt::event** ](/uwp/cpp-ref-for-winrt/event) implementación ahora controla correctamente el caso donde su **quitar** se llama al método con un valor de token no válido (un valor que no está presente en el matriz).

#### <a name="coroutine-locals-are-now-destroyed-before-the-coroutine-returns"></a>Variables locales de corrutina ahora se destruyen antes de que la corrutina

La forma tradicional de la implementación de un tipo de corrutina puede permitir que las variables locales dentro de la corrutina se destruye *después* la corrutina Devuelve o se completa (en lugar de antes de la suspensión final). La reanudación de cualquier objeto waiter ahora se aplaza hasta que la suspensión final, con el fin de evitar este problema y se acumulan otras ventajas.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Noticias y los cambios, en la versión 10.0.17763.0 del SDK de Windows (Windows 10, versión 1809)

En la tabla siguiente contiene noticias y cambia a [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) en la última versión disponible con carácter general del SDK de Windows, que es 10.0.17763.0 (Windows 10, versión 1809). Estos cambios también pueden estar presentes en las versiones posteriores de SDK Insider Preview.

| Características nuevas o modificadas | Más información |
| - | - |
| **Cambio importante**. Para que se compile, C++ / c++ / WinRT no depende de los encabezados del SDK de Windows. | Consulte [aislada con respecto a los archivos de encabezado de Windows SDK](#isolation-from-windows-sdk-header-files), a continuación. |
| Ha cambiado el formato de sistema de proyecto de Visual Studio. | Consulte [cómo redestinar C++ / c++ / WinRT proyecto a una versión posterior del SDK de Windows](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk), a continuación. |
| Existen nuevas funciones y clases base para ayudarle a pasar un objeto de colección a una función en tiempo de ejecución de Windows, o para implementar sus propias propiedades de la colección y los tipos de colección. | Consulte [colecciones con C++ / c++ / WinRT](collections.md). |
| Puede usar el [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) extensión de marcado con C++ / c++ / WinRT clases de tiempo de ejecución. | Para obtener más información y ejemplos de código, vea [información general sobre el enlace de datos](/windows/uwp/data-binding/data-binding-quickstart). |
| Soporte técnico para la cancelación de una corrutina permite registrar una devolución de llamada de cancelación. | Para obtener más información y ejemplos de código, vea [cancelar una operación asincrónica y las devoluciones de llamada de cancelación](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Al crear un delegado que apunta a una función miembro, puede establecer un fuerte o una referencia débil al objeto actual (en lugar de sin formato *esto* puntero) en el punto donde se registra el controlador. | Para obtener más información y ejemplos de código, vea el **si usa una función miembro como un delegado** subsección en la sección [con seguridad de acceso a la *esto* puntero con un delegado de control de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Se corrigieron errores que estaban descubiertos por conformidad mejorada de Visual Studio con el estándar de C++. La cadena de herramientas LLVM y Clang se aprovecha mejor también para validar C++conformidad con los estándares de /WinRT. | Ya no nos encontramos el problema descrito en [¿por qué no mi nuevo proyecto se compilará? Estoy usando Visual Studio 2017 (versión 15.8.0 o superior) y el SDK versión 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Otros cambios.

- **Cambio importante**. [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) ahora devuelve `void*` en lugar de `HSTRING`. Puede usar `static_cast<HSTRING>(get_abi(my_hstring));` para obtener una cadena HSTRING.
- **Cambio importante**. [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) ahora devuelve `void**` en lugar de `HSTRING*`. Puede usar `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` para obtener una cadena HSTRING *.
- **Cambio importante**. HRESULT ahora se proyecta como **winrt::hresult**. Si necesita un valor HRESULT (para la comprobación de tipos, o para admitir rasgos de tipo), puede `static_cast` un **winrt::hresult**. En caso contrario, **winrt::hresult** convierte a HRESULT, siempre y cuando incluya `unknwn.h` antes de incluir cualquier C++ / c++ / WinRT encabezados.
- **Cambio importante**. GUID ahora se proyecta como **winrt::guid**. Para las API que implementan, debe usar **winrt::guid** para los parámetros GUID. En caso contrario, **winrt::guid** convierte en GUID, siempre y cuando incluya `unknwn.h` antes de incluir cualquier C++/WinRT encabezados.
- **Cambio importante**. El [ **winrt::handle_type constructor** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typehandle_type-constructor) se ha protegido mediante la realización de explícita (es ahora más difícil para escribir código incorrecto con él). Si tiene que asignar un valor de identificador sin formato, llame a la [ **handle_type::attach función** ](/uwp/cpp-ref-for-winrt/handle-type#handle_typeattach-function) en su lugar.
- **Cambio importante**. Las firmas de **WINRT_CanUnloadNow** y **WINRT_GetActivationFactory** han cambiado. No debe declarar estas funciones en absoluto. En su lugar, incluya `winrt/base.h` (que se incluye automáticamente si se incluye C++ / c++ / archivos de encabezado del espacio de nombres de WinRT Windows) para incluir las declaraciones de estas funciones.
- Para el [ **winrt::clock struct**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** están en desuso en favor de **from_file_time/to_file_time**.
- API que esperan **IBuffer** se simplifican los parámetros. Aunque la mayoría de las API prefiere colecciones o matrices, suficiente API se basan en **IBuffer** que debían ser más fácil usar estas API desde C++. Esta actualización proporciona acceso directo a los datos detrás de un **IBuffer** implementación, con la misma convención de nomenclatura de datos utilizada por los contenedores de la biblioteca estándar de C++. Esto evita también chocar con nombres de los metadatos que convencionalmente comienzan con una letra mayúscula.
- Generación de código mejorada: varias mejoras para reducir el tamaño del código, mejorar la inserción y optimizar la caché del generador.
- Quita la recursividad innecesaria. Cuando la hace la línea de comandos de referencia a una carpeta, en lugar de a un determinado `.winmd`, `cppwinrt.exe` herramienta ya no busca de forma recursiva `.winmd` archivos. El `cppwinrt.exe` herramienta también ahora controla los duplicados de manera más inteligente, lo que sea más resistente a errores del usuario y mal formado a `.winmd` archivos.
- Punteros inteligentes reforzados. Anteriormente, el revokers de evento no se pudo revocar cuando move-asignan un nuevo valor. Esto ayudó a descubrir un problema donde las clases de puntero inteligente no estaban Gestión fiable de asignación automática; ha modificado en el [ **winrt::com_ptr struct plantilla**](/uwp/cpp-ref-for-winrt/com-ptr). **winrt::com_ptr** se ha corregido, y el revokers de evento se ha corregido para controlar la semántica de transferencia correctamente para que estos revocarán tras la asignación.

> [!IMPORTANT]
> Se realizaron cambios importantes en el [C++ / c++ / extensión de Visual Studio (VSIX) de WinRT](https://aka.ms/cppwinrt/vsix), tanto en la versión 1.0.181002.2 y, a continuación, más adelante en la versión 1.0.190128.4. Para obtener información detallada de estos cambios y cómo afectan a los proyectos existentes, [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) y [versiones anteriores de la extensión VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

### <a name="isolation-from-windows-sdk-header-files"></a>Aislamiento de los archivos de encabezado de Windows SDK

Potencialmente, esto es un cambio importante para el código.

Para que se compile, C++ / c++ / WinRT ya no depende de los archivos de encabezado desde el SDK de Windows. Archivos de encabezado de la biblioteca de tiempo de ejecución de C (CRT) y la biblioteca de plantillas estándar (STL) de C++ también no incluyen los encabezados del SDK de Windows. Y que mejora el cumplimiento de estándares, evita la dependencia involuntario y reduce en gran medida el número de macros que se deben protegerse.

Esta independencia significa que C++ / c++ / WinRT es ahora más portátil y con las normas y contribuye a mejorar la posibilidad de que se convierta en una biblioteca multiplataforma y compilador cruzado. También significa que C++ / c++ / WinRT encabezados no están afectadas negativamente macros.

Si previamente ha dejado a C++ / c++ / WinRT para incluir los encabezados de Windows en el proyecto, ahora deberá incluir usted mismo. Es, en cualquier caso, siempre recomendado para incluir explícitamente los encabezados que depende y no lo deja a otra biblioteca para incluirlas en el.

Actualmente, las únicas excepciones a aislamiento de archivo de encabezado de Windows SDK son intrínsecos y valores numéricos. No hay ningún problema conocido con estas últimas dependencias restantes.

En el proyecto, puede habilitar volver a la interoperabilidad con los encabezados del SDK de Windows si necesita. Por ejemplo, podría implementar una interfaz COM (arraigada en [ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Por ejemplo, incluye `unknwn.h` antes de incluir cualquier C++ / c++ / WinRT encabezados. Si lo hace C / c++ / WinRT biblioteca base para habilitar varios enlaces admitir las interfaces COM clásicas. Para obtener un ejemplo de código, vea [componentes COM de autor con C++ / c++ / WinRT](author-coclasses.md). De forma similar, incluir explícitamente los encabezados de Windows SDK que declaran tipos o funciones que desea llamar.

### <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Cómo redirigir C++ / c++ / WinRT proyecto a una versión posterior del SDK de Windows

El método para redestinar el proyecto que es probable que produzca el problema del compilador y vinculador menor también es más trabajosa. Ese método implica crear un nuevo proyecto (como destino la versión del SDK de Windows de su elección) y, a continuación, copiar archivos a través el nuevo proyecto desde el antiguo. Habrá secciones de la antigua `.vcxproj` y `.vcxproj.filters` archivos que basta con que copie en Guardar, agregar archivos en Visual Studio.

Sin embargo, hay otras dos maneras de redirigir el proyecto en Visual Studio.

- Vaya a la propiedad del proyecto **General** \> **Windows SDK versión**y seleccione **todas las configuraciones de** y **todas las plataformas**. Establecer **Windows SDK versión** a la versión que desea tener como destino.
- En **el Explorador de soluciones**, haga clic en el nodo del proyecto, haga clic en **redestinar proyectos**, elija las versiones que desea tener como destino y, a continuación, haga clic en **Aceptar**.

Si aparecen errores del vinculador o compilador después de usar cualquiera de estos dos métodos, puede probar la solución de limpieza (**compilar** > **Limpiar solución** o elimine manualmente todos archivos y carpetas temporales) antes de intentar volver a crearla.

Si genera el compilador de C++ "*error C2039: 'IUnknown': no es un miembro de '\`espacio de nombres global''*", a continuación, agregue `#include <unknwn.h>` a la parte superior de su `pch.h` archivo (antes de incluir cualquier C++ / c++ / WinRT encabezados).

También es posible que deba agregar `#include <hstring.h>` después de eso.

Si genera el vinculador de C++ "*error LNK2019: símbolo externo sin resolver _WINRT_CanUnloadNow@0 hace referencia en la función _VSDesignerCanUnloadNow@0* ", a continuación, se puede resolver mediante la adición de `#define _VSDESIGNER_DONT_LOAD_AS_DLL` a su `pch.h` archivo.
