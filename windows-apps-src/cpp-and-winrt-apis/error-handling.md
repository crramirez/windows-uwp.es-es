---
author: stevewhims
description: Este tema describe estrategias para controlar los errores de programación con C++/WinRT.
title: Gestión de errores con C++/WinRT
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, error, gestión, excepción
ms.localizationpriority: medium
ms.openlocfilehash: 36f6248452d97d10b6004067b6c0a973973443db
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5752648"
---
# <a name="error-handling-with-cwinrt"></a>Gestión de errores con C++/WinRT

En este tema describe estrategias para controlar los errores de programación con [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Para obtener información más general y antecedentes, consulta [Gestión de errores y excepciones (C++ moderno)](/cpp/cpp/errors-and-exception-handling-modern-cpp).

## <a name="avoid-catching-and-throwing-exceptions"></a>Evitar detección y lanzamiento de excepciones
Te recomendamos que sigas escribiendo [código seguro para excepciones](/cpp/cpp/how-to-design-for-exception-safety), pero puede que prefieras evitar la detección y el lanzamiento de excepciones en la medida de lo posible. Si no hay ningún controlador para una excepción, Windows genera automáticamente un informe de errores (e incluye un minivolcado del bloqueo), que te ayudará a hacer un seguimiento de dónde está el problema.

No lances una excepción que esperas detectar. Y no uses excepciones para errores esperados. Lanza una excepción *solo cuando se produzca un error inesperado en tiempo de ejecución* y controla todo directamente con códigos de error/resultado&mdash;directamente y cerca del origen del error. De este modo, cuando *se* lanza una excepción, sabes que la causa es un error en el código, o un estado de error excepcional en el sistema.

Ten en cuenta el escenario de acceso al registro de Windows. Si la aplicación no puede leer un valor del registro, eso es de esperar y debes controlarlo sin problemas. No lances una excepción; en su lugar devolver un valor `bool` o `enum` que indica que, y quizás por qué, no se lee el valor. Si no se *escribe* un valor en el registro, por otro lado, es probable que indique que hay un problema mayor que puede controlar de forma razonable en la aplicación. En un caso similar, no quieres que la aplicación continúe, por lo que una excepción que da como resultado un informe de errores es la forma más rápida para evitar que la aplicación cause daños.

Para otro ejemplo, considera la posibilidad de recuperar una imagen en miniatura de una llamada a [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) y después pasa dicha miniatura a [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_). Si dicha secuencia de llamadas hace que pases `nullptr` a **SetSourceAsync** (el archivo de imagen no se puede leer; tal vez su extensión de archivo hace parecer que contiene datos de imagen, pero en realidad no es así), podrás producir que se inicie una excepción de puntero no válida. Si descubres un caso como ese en el código, en lugar de detectar y controlar el caso como una excepción, en su lugar comprueba `nullptr` devuelto desde **GetThumbnailAsync **.

Iniciar excepciones tiende a ser más lento que usar códigos de error. Si solo se lanza una excepción cuando se produce un error irrecuperable, si todo va bien nunca pagarás el precio de rendimiento.

Pero una disminución del rendimiento más probable implica la sobrecarga del tiempo de ejecución de garantizar que se llaman a los destructores apropiados en el improbable caso de que se inicie una excepción. El coste de esta comprobación incluye si realmente se produce o no una excepción. Por tanto, debes asegurarte de que el compilador tenga una buena idea de qué funciones pueden potencialmente lanzar excepciones. Si el compilador puede demostrar que no habrá excepciones de determinadas funciones (la especificación `noexcept`), se puede optimizar el código que genera.

## <a name="catching-exceptions"></a>Excepciones de detección
Una condición de error que se produce en el nivel de [ABI de Windows Runtime](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) se devuelve en forma de un valor HRESULT. Pero no es necesario gestionar HRESULT en el código. El código de proyección de C++/WinRT que se genera para una API en el lado de consumo detecta un código error HRESULT en el nivel de la ABI y convierte el código en una excepción [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error), que puede detectar y gestionar.

Por ejemplo, si el usuario elimina una imagen de la biblioteca de imágenes mientras la aplicación está recorriendo dicha colección, la proyección lanza una excepción. Y este es un caso donde tendrás detectar y gestionar dicha excepción. Este es un ejemplo de código que muestra este caso.

```cppwinrt
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
            HRESULT hr = ex.to_abi(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

Usa este mismo patrón en una corrutina al llamar a una función en la que se ha aplicado `co_await`. Otro ejemplo de esta conversión HRESULT a excepción es que cuando una API de componente devuelve E_OUTOFMEMORY, que hace que se lance una **std::bad_alloc**.

## <a name="throwing-exceptions"></a>Iniciar excepciones
Habrá casos en los que decidas que, si falla la llamada a una función determinada, la aplicación no podrá recuperarse (ya no podrás confiar en que funcione de forma predecible). El ejemplo de código siguiente usa un valor [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) como un contenedor alrededor del MANIPULADOR devuelto desde [**CreateEvent**](https://msdn.microsoft.com/library/windows/desktop/ms682396). A continuación, pasa el manipulador (creando un valor `bool` a partir de él) a la plantilla de función [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool). **winrt::check_bool** funciona con un `bool`, o con cualquier valor que se pueda convertir en `false` (una condición de error), o `true` (una condición de éxito).

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

Si el valor que pasas a [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) es false, se realiza la siguiente secuencia de las acciones.

- **winrt::check_bool** llama a la función [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error).
- **winrt::throw_last_error** llama a [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360) para recuperar el valor del último código de error del subproceso de llamada y, a continuación, llama a la función [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult).
- **winrt::throw_hresult** lanza una excepción mediante un objeto [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) (o un objeto estándar) que representa dicho código de error.

Dado que las API de Windows notifican errores de tiempo de ejecución mediante diversos tipos de valor de devolución, además de **winrt::check_bool** hay unas cuantas funciones auxiliares útiles adicionales para comprobar los valores y lanzar excepciones.

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult). Comprueba si el código HRESULT representa un error y, si es así, llama a **winrt::throw_hresult**.
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt). Comprueba si un código representa un error y, si es así, llama a **winrt::throw_hresult**.
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer). Comprueba si un puntero es null y, si es así, llama a **winrt::throw_last_error**.
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32). Comprueba si un código representa un error y, si es así, llama a **winrt::throw_hresult**.

Puedes usar estas funciones auxiliares para tipos comunes de código de retorno, o puedes responder a cualquier condición de error y llamar a [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) o [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult). 

## <a name="throwing-exceptions-when-authoring-an-api"></a>Lanzar excepciones al crear una API
Dado que no es válido que una excepción cruzar los límites de la [ABI de Windows Runtime](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types), una condición de error surge en una implementación devuelta en el nivel de la ABI en forma de un código de error HRESULT. Cuando estás creando una API con C++/WinRT, se genera código para convertir cualquier excepción que *lanzas* en la implementación en un HRESULT. La función [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) se usa en el código generado en un patrón como este.

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

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) controla las excepciones que se derivan de **std::exception** y [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) y sus tipos derivados. En su implementación, debes preferir **winrt::hresult_error** o un tipo derivado, para que los usuarios de tu API reciban información de errores valiosa. **std::exception** (que se asigna a E_FAIL) se admite en caso de que surjan excepciones por el uso de la biblioteca de plantillas estándar.

## <a name="assertions"></a>Aserciones
Para suposiciones internas de la aplicación, existen las aserciones. Opta por **static_assert** para la validación de tiempo de compilación, siempre que sea posible. En condiciones de tiempo de ejecución, usa WINRT_ASSERT con una expresión booleana.

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

## <a name="related-topics"></a>Artículos relacionados
* [Gestión de errores y excepciones (C++ moderno)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [Cómo: Diseño de seguridad de excepciones](/cpp/cpp/how-to-design-for-exception-safety)
