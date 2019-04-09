---
description: Con C++/WinRT, puedes llamar a las API de Windows Runtime con tipos de datos C++ estándar.
title: Tipos de datos C++ estándar y C++/WinRT
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, datos, tipos
ms.localizationpriority: medium
ms.openlocfilehash: 44de7b61264f8e0e04d1de6d2b1101844656f28b
ms.sourcegitcommit: 99271798fe53d9768fc52b21366de05268cadcb0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58221461"
---
# <a name="standard-c-data-types-and-cwinrt"></a>Tipos de datos C++ estándar y C++/WinRT

Con [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), puede llamar a Windows en tiempo de ejecución APIs utilizando tipos de datos estándar de C++, incluidos algunos tipos de datos de la biblioteca estándar de C++. Puede pasar cadenas estándar a las API (consulte [cadena de control en C++ / c++ / WinRT](strings.md)), y puede pasar contenedores estándares y las listas de inicializadores a las API que esperan un conjunto semánticamente equivalente.

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

**winrt::array_view** es personalizada C++/WinRT tipo que representa una serie de valores contigua de forma segura (se define en el C++/WinRT biblioteca base, que es `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`).

Segundo, **winrt::array_view** tiene un constructor de la lista de inicializadores.

```cppwinrt
template <typename T> winrt::array_view(std::initializer_list<T> value) noexcept
```

En muchos casos, puede elegir si se deben tener en cuenta **winrt::array_view** en su programación. Si eliges *no* tenerla en cuenta, no tendrás ningún código que cambiar si aparece un tipo equivalente en la biblioteca estándar de C++.

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

Aquí funcionan dos factores. En primer lugar, el destinatario crea un **std:: vector** desde la lista de inicializadores (este destinatario es asincrónica, por lo que es capaz de ese objeto, que debe ser el propietario). En segundo lugar, C++/WinRT enlaza **std:: vector** de forma transparente (y sin introducir copias) como un parámetro de la colección de Windows Runtime.

## <a name="standard-arrays-and-vectors"></a>Matrices y vectores estándares
[**winrt::array_view** ](/uwp/cpp-ref-for-winrt/array-view) también tiene los constructores de conversión de **std:: vector** y **std:: Array**.

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

C++/WinRT enlaza **std:: vector** como un parámetro de la colección de Windows Runtime. Por lo tanto, puedes pasar un **std:: vector&lt;winrt::hstring&gt;**, y se convertirá a la colección adecuada de Windows Runtime de **winrt::hstring**. Hay un detalle adicional a tener en cuenta si el destinatario es asincrónico. Debido a los detalles de implementación de ese caso, deberá proporcionar un valor r, por lo que debe proporcionar una copia o el movimiento del vector. En el ejemplo de código siguiente, se mueve la propiedad del vector en el objeto del tipo de parámetro aceptado por el destinatario de async (y, a continuación, estamos cuidados de no tener acceso a `vecH` nuevamente después de moverla). Si desea obtener más información sobre rvalues, vea [categorías de valor y las referencias a ellos](cpp-value-categories.md).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const storageFile, std::vector<winrt::hstring> vecH)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecH)) };
}
```

Pero no puedes pasar un **std:: vector&lt;std:: wstring&gt;** donde se espera una colección de Windows Runtime. Esto se debe a que, al haber convertido a la colección adecuada de Windows Runtime de **std:: wstring**, el lenguaje de C++ no forzará el o los parámetro/s de tipo de dicha colección. Por lo tanto, no se compilará el siguiente ejemplo de código (y la solución consiste en pasar un **std:: vector&lt;winrt::hstring&gt;**  en su lugar, como se muestra arriba).

```cppwinrt
IAsyncAction retrieve_properties_async(StorageFile const& storageFile, std::vector<std::wstring> const& vecW)
{
    auto properties{ co_await storageFile.Properties().RetrievePropertiesAsync(std::move(vecW)) }; // error! Can't convert from vector of wstring to async_iterable of hstring.
}
```

## <a name="raw-arrays-and-pointer-ranges"></a>Matrices sin procesar e intervalos de puntero
Teniendo en cuenta la salvedad de que exista un tipo equivalente en el futuro en el C++ biblioteca estándar, también puede trabajar directamente con **winrt::array_view** si elige, o que necesite.

**winrt::array_view** tiene constructores de conversión de una matriz sin formato y de un intervalo de **T&ast;**  (punteros al tipo de elemento).

```cppwinrt
using namespace winrt;
...
byte theRawArray[]{ 99, 98, 97 };
array_view<byte const> fromRawArray{ theRawArray };
dataWriter.WriteBytes(fromRawArray); // the winrt::array_view is passed to WriteBytes.

array_view<byte const> fromRange{ theArray.data(), theArray.data() + 2 }; // just the first two elements.
dataWriter.WriteBytes(fromRange); // the winrt::array_view is passed to WriteBytes.
```

## <a name="winrtarrayview-functions-and-operators"></a>Funciones y operadores de winrt::array_view
Un host de constructores, operadores, funciones y los iteradores se implementan para **winrt::array_view**. Un **winrt::array_view** es un intervalo, por lo que puede usar con basado en rango `for`, o con **std:: for_each**.

Para obtener más ejemplos e información, consulta el tema de referencia de API [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view).

## <a name="ivectorlttgt-and-standard-iteration-constructs"></a>**IVector&lt;T&gt;**  y construcciones de iteración estándar
[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items) es un ejemplo de una API de Windows en tiempo de ejecución que devuelve una colección de tipo [ **IVector&lt;T&gt;**  ](/uwp/api/windows.foundation.collections.ivector_t_) (proyectan en C++ / C++ / WinRT como **winrt::Windows::Foundation::Collections::IVector&lt;T&gt;**). Puede usar este tipo, como con construcciones de iteración estándar, basados en intervalos `for`.

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

## <a name="c-coroutines-with-asynchronous-windows-runtime-apis"></a>Corrutinas de C++ con las APIs asincrónicas de Runtime de Windows
Aún puede usar el [Parallel Patterns Library (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) cuando una llamada asincrónica Windows Runtime APIs. Sin embargo, en muchos casos, las corrutinas de C++ proporcionan un modismo eficiente y más fácilmente de forma rígida para interactuar con objetos asincrónicos. Para obtener más información y ejemplos de código, vea [simultaneidad y operaciones asincrónicas con C++ / c++ / WinRT](concurrency.md).

## <a name="important-apis"></a>API importantes
* [IVector&lt;T&gt; interfaz](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::array_view struct template](/uwp/cpp-ref-for-winrt/array-view)

## <a name="related-topics"></a>Temas relacionados
* [Cadena de control en C++ / c++ / WinRT](strings.md)
