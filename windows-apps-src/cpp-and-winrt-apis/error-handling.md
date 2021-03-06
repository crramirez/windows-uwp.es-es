---
description: En este tema se describen estrategias para controlar los errores de programación con C++/WinRT.
title: Control de errores con C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, error, handling, exception
ms.localizationpriority: medium
ms.openlocfilehash: c721c70f19533053821139429b9bbd8bcd17f68a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170229"
---
# <a name="error-handling-with-cwinrt"></a>Control de errores con C++/WinRT

En este tema se describen estrategias para controlar los errores de programación con [C++/WinRT](./intro-to-using-cpp-with-winrt.md). Para más información general y antecedentes, consulta [Control de errores y excepciones (C++ moderno)](/cpp/cpp/errors-and-exception-handling-modern-cpp).

## <a name="avoid-catching-and-throwing-exceptions"></a>Evitar la detección y el inicio de excepciones
Te recomendamos que sigas escribiendo [código seguro para excepciones](/cpp/cpp/how-to-design-for-exception-safety), pero puede que prefieras evitar la detección y el inicio de excepciones en la medida de lo posible. Si no hay ningún controlador para una excepción, Windows genera automáticamente un informe de errores (e incluye un minivolcado del bloqueo), que te ayudará a hacer un seguimiento de dónde está el problema.

No lances una excepción que esperas detectar. Y no uses excepciones para errores esperados. Inicia una excepción *solo cuando se produzca un error inesperado en tiempo de ejecución* y controla todo directamente con códigos de error o resultado&mdash;directamente y cerca del origen del error. De este modo, cuando *se* inicia una excepción, sabes que la causa es un error en el código, o un estado de error excepcional en el sistema.

Ten en cuenta el escenario de acceso al registro de Windows. Si la aplicación no puede leer un valor del Registro, eso es de esperar y debes controlarlo sin problemas. No lances una excepción; en su lugar devuelve un valor `bool` o `enum` que indica que, y quizás por qué, no se lee el valor. Por otro lado, si no se *escribe* un valor en el Registro, es probable que indique que hay un problema mayor que puede controlar de forma razonable en la aplicación. En un caso similar, no quieres que la aplicación continúe, por lo que una excepción que da como resultado un informe de errores es la forma más rápida para evitar que la aplicación cause daños.

Para ver otro ejemplo, considera la posibilidad de recuperar una imagen en miniatura de una llamada a [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) y después pasa dicha miniatura a [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_). Si dicha secuencia de llamadas hace que pases `nullptr` a **SetSourceAsync** (el archivo de imagen no se puede leer; tal vez su extensión de archivo hace parecer que contiene datos de imagen, pero en realidad no es así), podrás producir que se inicie una excepción de puntero no válido. Si descubres un caso como ese en el código, en lugar de detectar y controlar el caso como una excepción, comprueba `nullptr` devuelto desde **GetThumbnailAsync** .

El inicio de excepciones tiende a ser más lento que usar códigos de error. Si solo se inicia una excepción cuando se produce un error irrecuperable, si todo va bien nunca pagarás el precio de rendimiento.

Pero una disminución del rendimiento más probable implica la sobrecarga del tiempo de ejecución de garantizar que se llaman a los destructores apropiados en el improbable caso de que se inicie una excepción. El costo de esta garantía incluye si realmente se inicia o no una excepción. Por tanto, debes asegurarte de que el compilador tenga una buena idea de qué funciones pueden potencialmente iniciar excepciones. Si el compilador puede demostrar que no habrá excepciones de determinadas funciones (la especificación `noexcept`), se puede optimizar el código que genera.

## <a name="catching-exceptions"></a>Detección de excepciones
Una condición de error que se produce en el nivel de [ABI de Windows Runtime](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) se devuelve en forma de un valor HRESULT. Pero no es necesario tratar los valores HRESULT en el código. El código de proyección de C++/WinRT que se genera para una API en el lado de consumo detecta un código error HRESULT en el nivel de la ABI y convierte el código en una excepción [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error), que puede detectar y tratar. Si *quieres* controlar los valores HRESULT, utiliza el tipo **winrt::hresult**.

Por ejemplo, si el usuario elimina una imagen de la biblioteca de imágenes mientras la aplicación está recorriendo dicha colección, la proyección inicia una excepción. Y este es un caso donde tendrás detectar y tratar dicha excepción. Este es un ejemplo de código que muestra este caso.

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.UI.Xaml.Media.Imaging.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage;
using namespace Windows::UI::Xaml::Media::Imaging;

IAsyncAction MakeThumbnailsAsync()
{
    auto imageFiles{ co_await KnownFolders::PicturesLibrary().GetFilesAsync() };

    for (StorageFile const& imageFile : imageFiles)
    {
        BitmapImage bitmapImage;
        try
        {
            auto thumbnail{ co_await imageFile.GetThumbnailAsync(FileProperties::ThumbnailMode::PicturesView) };
            if (thumbnail) bitmapImage.SetSource(thumbnail);
        }
        catch (winrt::hresult_error const& ex)
        {
            winrt::hresult hr = ex.code(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

Usa este mismo patrón en una corrutina al llamar a una función en la que se ha aplicado `co_await`. Otro ejemplo de esta conversión HRESULT a excepción es que cuando una API de componente devuelve E_OUTOFMEMORY, que hace que se inicie una excepción **std::bad_alloc**.

Opte por [**winrt::hresult_error::code**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorcode-function) cuando simplemente esté inspeccionando un código HRESULT. Por otro lado, la función [**winrt::hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) se convierte en un objeto de error COM e inserta el estado en el almacenamiento local de subprocesos COM.

## <a name="throwing-exceptions"></a>Iniciar excepciones
Habrá casos en los que decidas que, si se produce un error en la llamada a una función determinada, la aplicación no podrá recuperarse (ya no podrás confiar en que funcione de forma predecible). El ejemplo de código siguiente usa un valor [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) como un contenedor alrededor del controlador devuelto desde [**CreateEvent**](/windows/desktop/api/synchapi/nf-synchapi-createeventa). A continuación, pasa el controlador (mediante la creación de un valor `bool` a partir de él) a la plantilla de función [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool). **winrt::check_bool** funciona con un `bool`, o con cualquier valor que se pueda convertir en `false` (una condición de error), o `true` (una condición de éxito).

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

Si el valor que pasas a [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) es false, se realiza la siguiente secuencia de las acciones.

- **winrt::check_bool** llama a la función [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error).
- **winrt::throw_last_error** llama a [**GetLastError**](/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror) para recuperar el valor del último código de error del subproceso de llamada y, a continuación, llama a la función [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult).
- **winrt::throw_hresult** inicia una excepción mediante un objeto [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) (o un objeto estándar) que representa dicho código de error.

Dado que las API de Windows notifican errores de tiempo de ejecución mediante diversos tipos de valor de devolución, además de **winrt::check_bool**, hay unas cuantas funciones auxiliares adicionales útiles para comprobar los valores e iniciar excepciones.

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult). Comprueba si el código HRESULT representa un error y, si es así, llama a **winrt::throw_hresult**.
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt). Comprueba si un código representa un error y, si es así, llama a **winrt::throw_hresult**.
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer). Comprueba si un puntero es null y, si es así, llama a **winrt::throw_last_error**.
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32). Comprueba si un código representa un error y, si es así, llama a **winrt::throw_hresult**.

Puedes usar estas funciones auxiliares para tipos comunes de código de retorno, o puedes responder a cualquier condición de error y llamar a [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) o [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult). 

## <a name="throwing-exceptions-when-authoring-an-api"></a>Iniciar excepciones al crear una API
Todos los límites de la [interfaz binaria de aplicaciones de Windows Runtime](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) (o límites de ABI) deben ser *noexcept*&mdash;en el sentido de que las excepciones nunca deben escapar allí—. Al crear una API, siempre debes marcar el límite de ABI con la palabra clave `noexcept` de C++. `noexcept` tiene un comportamiento específico en C++. Si una excepción de C++ alcanza un límite `noexcept`, el proceso fracasará rápido con **std::terminate**. Este comportamiento suele ser conveniente, ya que una excepción no controlada casi siempre implica un estado desconocido del proceso.

Dado que las excepciones no deben cruzar el límite de ABI, una condición de error que surja en una implementación se devuelve en el nivel de la ABI en forma de un código de error HRESULT. Cuando estás creando una API con C++/WinRT, se genera código para convertir cualquier excepción que *inicias* en la implementación en un HRESULT. La función [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) se usa en el código generado en un patrón como este.

```cppwinrt
HRESULT DoWork() noexcept
{
    try
    {
        // Shim through to your C++/WinRT implementation.
        return S_OK;
    }
    catch (...)
    {
        return winrt::to_hresult(); // Convert any exception to an HRESULT.
    }
}
```

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) controla las excepciones que se derivan de **std::exception** y [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) y sus tipos derivados. En su implementación, debes preferir **winrt::hresult_error** o un tipo derivado, para que los usuarios de tu API reciban una valiosa información de errores. **std::exception** (que se asigna a E_FAIL) se admite en caso de que surjan excepciones por el uso de la biblioteca de plantillas estándar.

### <a name="debuggability-with-noexcept"></a>Capacidad de depuración con noexcept
Como se mencionó anteriormente, una excepción de C++ que alcance un límite `noexcept` fracasará rápido con **std::terminate**. Esto no es lo ideal para depuración, ya que **std::terminate** suele perder gran parte o la totalidad del error o el contexto de la excepción que se produce, especialmente cuando hay involucradas corrutinas.

Por lo tanto, en esta sección se trata el caso en el que el método de ABI (que has anotado correctamente con `noexcept`) usa `co_await` para llamar al código de proyección asincrónico de C++/WinRT. Se recomienda encapsular las llamadas al código de proyección de C++/WinRT en **winrt::fire_and_forget**. Al hacerlo, obtienes un lugar adecuado para que una excepción no controlada se registre correctamente como Stowed Exception, lo que aumenta en gran medida la capacidad de depuración.

```cppwinrt
HRESULT MyWinRTObject::MyABI_Method() noexcept
{
    winrt::com_ptr<Foo> foo{ get_a_foo() };

    [/*no captures*/](winrt::com_ptr<Foo> foo) -> winrt::fire_and_forget
    {
        co_await winrt::resume_background();

        foo->ABICall();

        AnotherMethodWithLotsOfProjectionCalls();
    }(foo);

    return S_OK;
}
```

**winrt::fire_and_forget** tiene un método auxiliar `unhandled_exception` integrado, que llama a **winrt::terminate**, que, a su vez, llama a **RoFailFastWithErrorContext**. Esto garantiza que todo contexto (Stowed Exception, código de error, mensaje de error, seguimiento regresivo de pila, etc.) se conserve para la depuración en directo o para un volcado final. Para mayor comodidad, puedes factorizar la parte de fire_and_forget en una función independiente que devuelva **winrt::fire_and_forget** y, luego, llamar allí.

### <a name="synchronous-code"></a>Código sincrónico
En algunos casos, el método de ABI (que, de nuevo, has anotado correctamente con `noexcept`) solo llama a código sincrónico. En otras palabras, nunca usa `co_await`, ya sea para llamar a un método de Windows Runtime asincrónico ni para cambiar entre subprocesos de primer y segundo plano. En ese caso, la técnica de fire_and_forget seguirá funcionando, pero no es eficiente. En su lugar, puedes hacer algo parecido a esto.

```cppwinrt
HRESULT abi() noexcept try
{
    // ABI code goes here.
} catch (...) { winrt::terminate(); }
```

### <a name="fail-fast"></a>Fracaso rápido
En el código de la sección anterior aún se produce un fracaso rápido. Tal y como está escrito, ese código no controla ninguna excepción. Toda excepción no controlada provocará la finalización del programa.

Pero esa forma es superior, ya que garantiza la capacidad de depuración. En contadas ocasiones, es posible que quieras `try/catch` y controlar ciertas excepciones. Pero esto debería ser poco frecuente, ya que, como se explica en este tema, se desaconseja el uso de excepciones como mecanismo de control del flujo para las condiciones que se esperan.

Recuerda que es mala idea dejar que una excepción no controlada escape a un contexto `noexcept` desnudo. En ese estado, el tiempo de ejecución de C++ aplicará **std::terminate** al proceso, con lo que se pierde la información de Stowed Exceptions que C++/WinRT ha registrado cuidadosamente.

## <a name="assertions"></a>Aserciones
Para suposiciones internas de la aplicación, existen las aserciones. Opta por **static_assert** para la validación de tiempo de compilación, siempre que sea posible. En condiciones de tiempo de ejecución, usa `WINRT_ASSERT` con una expresión booleana. `WINRT_ASSERT` es una definición de macro y se expande a [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT se compila inmediatamente en versiones de lanzamiento; en una compilación de depuración, detiene la aplicación en el depurador en la línea de código donde se encuentra la aserción.

No debes usar excepciones en los destructores. Por lo tanto, al menos en las compilaciones de depuración, puedes distinguir el resultado de llamar a una función desde un destructor con WINRT_VERIFY (con una expresión booleana) y WINRT_VERIFY_ (con un resultado esperado y una expresión booleana).

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>API importantes
* [Plantilla de función winrt::check_bool](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [Función winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [Plantilla de función winrt::check_nt](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [Plantilla de función winrt::check_pointer](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [Plantilla de función winrt::check_win32](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [Estructura winrt::handle](/uwp/cpp-ref-for-winrt/handle)
* [Estructura winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [Función winrt::throw_hresult](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [Función winrt::throw_last_error](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [Función winrt::to_hresult](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>Temas relacionados
* [Control de errores y excepciones (C++ moderno)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [Procedimientos: Diseño de seguridad de excepciones](/cpp/cpp/how-to-design-for-exception-safety)