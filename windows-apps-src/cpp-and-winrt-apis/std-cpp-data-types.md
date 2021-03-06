---
description: Con C++/WinRT, puedes llamar a las API de Windows Runtime mediante tipos de datos de C++ estándar.
title: Tipos de datos de C++ estándar y C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, data, types
ms.localizationpriority: medium
ms.openlocfilehash: d61de7acdfa2fc3b563aa77630a9eb8043bce3d7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154339"
---
# <a name="standard-c-data-types-and-cwinrt"></a>Tipos de datos de C++ estándar y C++/WinRT

Con [C++/WinRT](./intro-to-using-cpp-with-winrt.md), puedes llamar a las API de Windows Runtime con tipos de datos C++ estándar, incluidos algunos tipos de datos de la biblioteca estándar de C++. Puedes pasar cadenas estándar a las API (consulta [Control de cadenas en C++/WinRT](strings.md)) y puedes pasar contenedores estándar y listas de inicializadores a las API que esperan una colección semánticamente equivalente.

Consulta también [Pasar parámetros a los límites de la ABI](./pass-parms-to-abi.md).

## <a name="standard-initializer-lists"></a>Listas de inicializadores estándares
Una lista de inicializadores (**std::initializer_list**) es una construcción de la biblioteca estándar de C++. Puedes usar las listas de inicializadores cuando llames a algunos constructores y métodos de Windows Runtime. Por ejemplo, puedes llamar a [**DataWriter::WriteBytes**](/uwp/api/windows.storage.streams.datawriter.writebytes) con una lista.

```cppwinrt
#include <winrt/Windows.Storage.Streams.h>

using namespace winrt::Windows::Storage::Streams;

int main()
{
    winrt::init_apartment();

    InMemoryRandomAccessStream stream;
    DataWriter dataWriter{stream};
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to a winrt::array_view before being passed to WriteBytes.
}
```

Hay dos piezas implicadas en este trabajo. En primer lugar, el método **DataWriter::WriteBytes** toma un parámetro de tipo [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

```cppwinrt
void WriteBytes(winrt::array_view<uint8_t const> value) const
```

**winrt::array_view** es un tipo C++/WinRT personalizado que representa de forma segura una serie contigua de valores (definida en la biblioteca base de C++/WinRT, que es `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`).

En segundo lugar, **winrt::array_view** tiene un constructor de listas de inicializadores.

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

En muchos casos, puedes elegir si quieres tener en cuenta **winrt::array_view** o no en tu programación. Si eliges *no* tenerla en cuenta, no tendrás ningún código que cambiar si aparece un tipo equivalente en la biblioteca estándar de C++.

Puedes pasar una lista de inicializadores a una API de Windows Runtime que espera un parámetro de colección. Toma **StorageItemContentProperties::RetrievePropertiesAsync** como ejemplo.

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

Puedes llamar a dicha API con una lista de inicializadores como esta.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" }) };
}
```

Aquí funcionan dos factores. En primer lugar, el destinatario construye un **std:: vector** desde la lista de inicializadores (este destinatario es asincrónico, lo que quiere decir que puede y debe poseer dicho objeto). En segundo lugar, C++/WinRT enlaza **std:: vector** de forma transparente (y sin introducir copias) como un parámetro de la colección de Windows Runtime.

## <a name="standard-arrays-and-vectors"></a>Matrices y vectores estándares
[**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) también tiene los constructores de conversión de **std::vector** y **std::array**.

```cppwinrt
template <typename C, size_type N> winrt::array_view(std::array<C, N>& value) noexcept
template <typename C> winrt::array_view(std::vector<C>& vectorValue) noexcept
```

Por lo tanto, en lugar de ello podrías llamar a **DataWriter::WriteBytes** con un **std:: vector**.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to a winrt::array_view before being passed to WriteBytes.
```

O con un **std::array**.

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to a winrt::array_view before being passed to WriteBytes.
```

C++/WinRT enlaza **std:: vector** como un parámetro de la colección de Windows Runtime. Por lo tanto, puedes pasar un **std:: vector&lt;winrt::hstring&gt;** , y se convertirá a la colección adecuada de Windows Runtime de **winrt::hstring**. Hay un detalle adicional que hay que tener en cuenta si el destinatario es asincrónico. Debido a los detalles de implementación de ese caso, tendrás que proporcionar un rvalue, por lo que debes proporcionar una copia o un movimiento del vector. En el ejemplo de código siguiente, movemos la propiedad del vector al objeto de tipo de parámetro aceptado por el destinatario asincrónico. Después, tenemos cuidado de no acceder a `vecH` después de moverla. Si quieres saber más sobre rvalues, consulta [Categorías de valor y referencias a ellas](cpp-value-categories.md).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

Pero no puedes pasar un **std:: vector&lt;std:: wstring&gt;** donde se espera una colección de Windows Runtime. Esto se debe a que, al haber convertido a la colección adecuada de Windows Runtime de **std:: wstring**, el lenguaje de C++ no forzará el o los parámetro/s de tipo de dicha colección. Por lo tanto, no se compilará el siguiente ejemplo de código (y la solución es pasar un **std::vector&lt;winrt::hstring&gt;** en su lugar, como se muestra arriba).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Matrices sin procesar e intervalos de puntero
Teniendo en cuenta que podría existir un tipo equivalente en el futuro en la biblioteca estándar de C++, también puedes trabajar directamente con **winrt::array_view** si así lo decides o lo necesitas.

**winrt::array_view** tiene los constructores de conversión de una matriz sin procesar y de un intervalo de **T&ast;** (punteros para el tipo de elemento).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the winrt::array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the winrt::array_view is passed to WriteBytes.
```

## <a name="winrtarray_view-functions-and-operators"></a>Funciones y operadores de winrt::array_view
Se ha implementado una gran cantidad de constructores, operadores, funciones e iteradores para **winrt::array_view**. **winrt::array_view** es un intervalo, así que puedes usarlo con `for` basado en intervalos o con **std::for_each**.

Para obtener más ejemplos e información, consulta el tema de referencia de API [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;** y construcciones de iteración estándar
[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) es un ejemplo de una API de Windows Runtime que devuelve una colección del tipo [**IVector&lt;T&gt;** ](/uwp/api/windows.foundation.collections.ivector_t_) (proyectada en C++/WinRT como **winrt::Windows::Foundation::Collections::IVector&lt;T&gt;** ). Puedes usar este tipo con construcciones de iteración estándar, como `for` basado en intervalos.

```cppwinrt
// main.cpp
#include "pch.h"
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}
```

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>Corrutinas de C++ con las API asincrónicas de Windows Runtime
Puedes seguir usando la [biblioteca de patrones paralelos (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) al llamar a las API asincrónicas de Windows Runtime. Sin embargo, en muchos casos, las corrutinas de C++ son un modo eficiente y fácil de interactuar con objetos asincrónicos. Para obtener más información y ejemplos de código, consulta [Operaciones simultáneas y asincrónicas con C++/WinRT](concurrency.md).

## <a name="important-apis"></a>API importantes
* [Interfaz IVector&lt;T&gt;](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::array_view struct template](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>Temas relacionados
* [Control de cadenas en C++/WinRT](strings.md)