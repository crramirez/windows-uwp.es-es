---
author: stevewhims
description: Con C++/WinRT, puedes llamar a las API de Windows Runtime con tipos de datos C++ estándar.
title: Tipos de datos C++ estándar y C++/WinRT
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, datos, tipos
ms.localizationpriority: medium
ms.openlocfilehash: ccf79b1ec21688d9573e62777def8f15295c3fca
ms.sourcegitcommit: ab92c3e0dd294a36e7f65cf82522ec621699db87
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "1832079"
---
# <a name="standard-c-data-types-and-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Tipos de datos C++ estándar y [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Con C++/WinRT, puedes llamar a las API de Windows Runtime con tipos de datos C++ estándar, incluidos algunos tipos de datos de la biblioteca estándar de C++.

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
    dataWriter.WriteBytes({ 99, 98, 97 }); // the initializer list is converted to an array_view before being passed to WriteBytes.
}
```

Hay dos piezas implicadas en este trabajo. En primer lugar, el método **DataWriter::WriteBytes** toma un parámetro de tipo [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

```cppwinrt
void WriteBytes(array_view<uint8_t const> value) const
```

 **array_view** es un tipo C++/WinRT personalizado que representa de forma segura una serie contigua de valores (definida en la biblioteca base de C++/WinRT, que es `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`).

En segundo lugar, **array_view** tiene un constructor de listas de inicializadores.

```cppwinrt
template <typename T> array_view(std::initializer_list<T> value) noexcept
```

En muchos casos, puedes elegir si quieres tener en cuenta **array_view** o no en tu programación. Si eliges *no* tenerla en cuenta, no tendrás ningún código que cambiar si aparece un tipo equivalente en la biblioteca estándar de C++.

Puedes pasar una lista de inicializadores a una API de Windows Runtime que espera un parámetro de colección. Toma **StorageItemContentProperties::RetrievePropertiesAsync** como ejemplo.

```cppwinrt
IAsyncOperation<IMap<winrt::hstring, IInspectable>> StorageItemContentProperties::RetrievePropertiesAsync(IIterable<winrt::hstring> propertiesToRetrieve) const;
```

Puedes llamar a dicha API con una lista de inicializadores como esta.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync({ L"System.ItemUrl" });
}
```

Aquí funcionan dos factores. En primer lugar, el destinatario construye un **std:: vector** desde la lista de inicializadores (este destinatario es asincrónico, de modo que puede poseer dicho objeto, lo cual debe hacerse). En segundo lugar, C++/WinRT enlaza **std:: vector** de forma transparente (y sin introducir copias) como un parámetro de la colección de Windows Runtime.

## <a name="standard-arrays-and-vectors"></a>Matrices y vectores estándares
**array_view** también tiene los constructores de conversión desde **std:: vector** y **std::array**.

```cppwinrt
template <typename C, size_type N> array_view(std::array<C, N>& value) noexcept
template <typename C> array_view(std::vector<C>& vectorValue) noexcept
```

Por lo tanto, en lugar de ello podrías llamar a **DataWriter::WriteBytes** con un **std:: vector**.

```cppwinrt
std::vector<byte> theVector{ 99, 98, 97 };
dataWriter.WriteBytes(theVector); // theVector is converted to an array_view before being passed to WriteBytes.
```

O con un **std::array**.

```cppwinrt
std::array<byte, 3> theArray{ 99, 98, 97 };
dataWriter.WriteBytes(theArray); // theArray is converted to an array_view before being passed to WriteBytes.
```

C++/WinRT enlaza **std:: vector** como un parámetro de la colección de Windows Runtime. Por lo tanto, puedes pasar un **std:: vector&lt;winrt::hstring&gt;**, y se convertirá a la colección adecuada de Windows Runtime de **winrt::hstring**. Si el destinatario es asincrónico, debes copiar o mover el vector. En el siguiente ejemplo de código, movemos la propiedad del vector al destinatario asincrónico.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<winrt::hstring> const& vecH)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH));
}
```

Pero no puedes pasar un **std:: vector&lt;std:: wstring&gt;** donde se espera una colección de Windows Runtime. Esto se debe a que, al haber convertido a la colección adecuada de Windows Runtime de **std:: wstring**, el lenguaje de C++ no forzará el o los parámetro/s de tipo de dicha colección. Por lo tanto, no se compilará el siguiente ejemplo de código.

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties = co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)); // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Matrices sin procesar e intervalos de puntero
Teniendo en cuenta que puede existir un tipo equivalente en el futuro en la biblioteca estándar de C++, también puedes trabajar directamente con **array_view** si así lo decides o lo necesitas.

**array_view** tiene los constructores de conversión desde una matriz sin procesar y desde un intervalo de **T*** (punteros para el tipo de elemento).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>Funciones y operadores de winrt::array_view
Se ha implementado una gran cantidad de constructores, operadores, funciones e iteradores para **array_view**. Un **array_view** es un intervalo, así que puedes usarlo con `for` basado en intervalo o con **std::for_each**.

Para obtener más ejemplos e información, consulta el tema de referencia de API [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="important-apis"></a>API importantes
* [Plantilla de estructura winrt::array_view](/uwp/cpp-ref-for-winrt/array-view)
