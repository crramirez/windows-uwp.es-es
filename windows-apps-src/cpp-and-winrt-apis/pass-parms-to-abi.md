---
description: C++/WinRT simplifica el paso de parámetros a los límites de la ABI ya que proporciona conversiones automáticas para casos habituales.
title: Pasar parámetros a los límites de la ABI
ms.date: 07/10/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, pasar, parámetros, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 05a627349ad2c4fda890a4f5280f5d33454ea910
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154459"
---
# <a name="passing-parameters-into-the-abi-boundary"></a>Pasar parámetros a los límites de la ABI

Con los tipos en el espacio de nombres **winrt::param**, C++/WinRT simplifica el paso de parámetros a los límites de ABI ya que proporciona conversiones automáticas para casos habituales. Puedes ver más detalles y ejemplos de código en [Control de cadenas ](./strings.md) y [Tipos de datos de C++ estándar y C++/WinRT](./std-cpp-data-types.md).

> [!IMPORTANT]
> No debes usar los tipos en el espacio de nombres **winrt::param** tú mismo. Esto es porque se usarán en beneficio de la proyección.

Muchos tipos vienen en versiones sincrónicas y asincrónicas. C++/WinRT usa la versión sincrónica cuando está pasando un parámetro a un método asincrónico; para ello, usa la versión asincrónica cuando pasa un parámetro a un método asincrónico. La versión asincrónica realiza pasos adicionales para dificultar que la persona que llama mute la colección antes de que la operación haya finalizado. Sin embargo, ten en cuenta que ninguna de estas variantes te protegen contra la recopilación que se esté mutando desde otro subproceso. Prevenir eso es tu responsabilidad.

## <a name="string-parameters"></a>Parámetros de cadena

**winrt::param::hstring** simplifica la operación de pasar parámetros a las API que toman **HSTRING**.

|Tipos que puedes pasar|Notas|
|-|-|
|`{}`|Pasa una cadena vacía.|
|**winrt::hstring**||
|**std::wstring_view**|Para los valores literales, se puede escribir `L"Name"sv`. La vista debe tener un terminador nulo después del final.|
|**std::wstring**|-|
|**wchar_t const\***|Una cadena terminada en null.|

`nullptr` no está permitido. Usa `{}` en su lugar.

El compilador sabe cómo evaluar `wcslen` en valores literales de cadena durante el tiempo de compilación. Por tanto, `L"Name"sv` y `L"Name"` son equivalentes para los valores literales.

Ten en cuenta que los objetos **std::wstring_view** no terminan en Null, pero C++/WinRT requiere que el carácter del final de la cadena sea Null. Si pasas un objeto **std::wstring_view** que no termine en Null, el proceso terminará.

## <a name="iterable-parameters"></a>Parámetros iterables

**winrt::param::iterable\<T\>** y **winrt::param::async_iterable\<T\>** simplifican la transferencia de parámetros a las API que usan **IIterable\<T\>** .

Las colecciones de Windows Runtime ya son de tipo **Iterable**.

|Tipos que puedes pasar|Sincronización|Async|Notas|
|-|-|-|-|
| `nullptr` | Sí | Sí | Debes comprobar que el método subyacente sea compatible con `nullptr`.|
| **IIterable\<T\>** | Sí | Sí | O cualquier elemento que se pueda convertir a él.|
| **std::vector\<T\> const&** | Sí | No ||
| **std::vector\<T\>&&** | Sí | Sí | El contenido se mueve al iterador para evitar mutaciones.|
| **std::initializer_list\<T\>** | Sí | Sí | La versión asincrónica copia los elementos.|
| **std::initializer_list\<U\>** | Sí | No | **U** debe ser convertible a **T**.|
| `{ ForwardIt begin, ForwardIt end }` | Sí | No | `*begin` debe ser convertible a **T**.|

Tenga en cuenta que **IIterable\<U\>** y **std::vector\<U\>** no están permitidos, incluso si **U** es convertible a **T**. En cuanto a **std::vector\<U\>** , puede usar la versión de doble iterador (tiene más detalles a continuación).

En algunos casos, el objeto que tienes puede implementar el valor **Iterable** que quieras. Por ejemplo, el elemento **IVectorView\<StorageFile\>** producido por [**FileOpenPicker.PickMultipleFilesAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.pickmultiplefilesasync) implementa **IIterable\<StorageFile\>** . Pero también implementa  **IIterable \<IStorageItem\>** ; solo tienes que pedirlo explícitamente.

```cppwinrt
IVectorView<StorageFile> pickedFiles{ co_await filePicker.PickMultipleFilesAsync() };
requestData.SetStorageItems(storageItems.as<IIterable<IStorageItem>>());
```

En otros casos, puedes usar la versión del iterador doble.

```cppwinrt
std::vector<StorageFile> storageFiles;
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() });
```

El iterador doble funciona de manera más general si resulta que tienes una colección que no se ajusta a ninguno de los escenarios anteriores; recuerda que debes poder iterarla y producir elementos que puedan convertirse a **T**. Lo usamos anteriormente para iterar un vector de tipos derivados. Aquí lo usaremos para iterar un elemento que no es un vector de tipos derivados.

```cppwinrt
std::array<StorageFile, 3> storageFiles;
requestData.SetStorageItems(storageFiles); // This doesn't work.
requestData.SetStorageItems({ storageFiles.begin(), storageFiles.end() }); // But this works.
```

La implementación de [**IIterator\<T\>.GetMany(T\[\])** ](/uwp/api/windows.foundation.collections.iiterator-1.getmany) es más eficiente si el iterador es `RandomAcessIt`. De lo contrario, realizará varias transferencias en el rango.

|Tipos que puedes pasar|Sincronización|Async|Notas|
|-|-|-|-|
| `nullptr` | Sí | Sí | Debes comprobar que el método subyacente sea compatible con `nullptr`.|
| **IIterable\<IKeyValuePair\<K, V\>\>** | Sí | Sí | O cualquier elemento que se pueda convertir a él.|
| **std::map\<K, V\> const&** | Sí | No ||
| **std::map\<K, V\>&&** | Sí | Sí | El contenido se mueve al iterador para evitar mutaciones.|
| **std::unordered_map\<K, V\> const&** | Sí | No ||
| **std::unordered_map\<K, V\>&&** | Sí | Sí | El contenido se mueve al iterador para evitar mutaciones.|
| **std::initializer_list\<std::pair\<K, V\>\>** | Sí | Sí | Los tipos **K** y **V** deben coincidir exactamente. No se pueden duplicar las claves. La versión asincrónica copia los elementos.|
| `{ ForwardIt begin, ForwardIt end }` | Sí | No | `begin->first` y `begin->second` deben ser convertibles a **K** y **V**, respectivamente.|

## <a name="vector-view-parameters"></a>Parámetros de la vista de vector

**winrt::param::vector_view\<T\>** y **winrt::param::async_vector_view\<T\>** simplifican la transferencia de parámetros a las API que usan **IVectorView\<T\>** .

Puede usar [**IVector\<T\>.GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview) para obtener un elemento **IVectorView** de **IVector**.

|Tipos que puedes pasar|Sincronización|Async|Notas|
|-|-|-|-|
| `nullptr` | Sí | Sí | Debes comprobar que el método subyacente sea compatible con `nullptr`.|
| **IVectorView\<T\>** | Sí | Sí | O cualquier elemento que se pueda convertir a él.|
| **std::vector\<T\>const&** | Sí | No ||
| **std::vector\<T\>&&** | Sí | Sí | El contenido se mueve a la vista para evitar mutaciones.|
| **std::initializer_list\<T\>** | Sí | Sí | El tipo debe coincidir exactamente. La versión asincrónica copia los elementos.|
| `{ ForwardIt begin, ForwardIt end }` | Sí | No | `*begin` debe ser convertible a **T**.|

La versión de iterador doble se puede usar para crear vistas vectoriales a partir de cosas que no se ajustan a los requisitos necesarios para pasarlas directamente. Ten en cuenta que, como los vectores admiten el acceso aleatorio, es recomendable que pases un `RandomAcessIter`.

## <a name="map-view-parameters"></a>Asignar parámetros de vista

**winrt::param::map_view\<T\>** y **winrt::param::async_map_view\<T\>** simplifican la transferencia de parámetros a las API que usan **IMapView\<T\>** .

Puedes usar **IMap::GetView** para obtener un elemento **IMapView** de un **IMap**.

|Tipos que puedes pasar|Sincronización|Async|Notas|
|-|-|-|-|
| `nullptr` | Sí | Sí | Debes comprobar que el método subyacente sea compatible con `nullptr`.|
| **IMapView\<K, V\>** | Sí | Sí | O cualquier elemento que se pueda convertir a él.|
| **std::map\<K, V\> const&** | Sí | No ||
| **std::map\<K, V\>&&** | Sí | Sí | El contenido se mueve a la vista para evitar mutaciones.|
| **std::unordered_map\<K, V\> const&**  | Sí | No ||
| **std::unordered_map\<K, V\>&&** | Sí | Sí | El contenido se mueve a la vista para evitar mutaciones.|
| **std::initializer_list\<std::pair\<K, V\>\>** | Sí | Sí | Las versiones sincrónicas y asincrónicas copian los elementos. No se pueden duplicar las claves.|

## <a name="vector-parameters"></a>Parámetros de vector

**winrt::param::vector\<T\>** simplifica la transferencia de parámetros a las API que usan **IVector\<T\>** .

|Tipos que puedes pasar|Notas|
|-|-|
| `nullptr` | Debes comprobar que el método subyacente sea compatible con `nullptr`.|
| **IVector\<T\>** | O cualquier elemento que se pueda convertir a él.|
| **std::vector\<T\>&&** | El contenido se mueve al parámetro para evitar mutaciones. Recuerda que no se devuelven los resultados.|
| **std::initializer_list\<T\>** | El contenido se copia al parámetro para evitar mutaciones.|

Si el método muta el vector, entonces la única forma de observar la mutación es pasar un **IVector** directamente. Si pasas un **std::vector**, el método mutará la copia y no el original.

## <a name="map-parameters"></a>Asignar parámetros

**winrt::param::map\<T\>** simplifica la transferencia de parámetros a las API que usan **IMap\<T\>** .

|Tipos que puedes pasar|Notas|
|-|-|
| `nullptr` | Debes comprobar que el método subyacente sea compatible con `nullptr`.|
| **IMap\<T\>** | O cualquier elemento que se pueda convertir a él.|
| **std::map\<K, V\>&&** | El contenido se mueve al parámetro para evitar mutaciones. Recuerda que no se devuelven los resultados.|
| **std::unordered_map\<K, V\>&&** | El contenido se mueve al parámetro para evitar mutaciones. Recuerda que no se devuelven los resultados.|
| **std::initializer_list\<std::pair\<K, V\>\>** | El contenido se copia al parámetro para evitar mutaciones.|

Si el método muta la asignación, entonces la única forma de observar la mutación es pasar un **IMap** directamente. Si pasas un **std::map** o **std::unordered_map**, el método mutará la copia y no el original.

## <a name="array-parameters"></a>Parámetros de matriz

El elemento **winrt::array_view\<T\>** no está en el espacio de nombres **winrt::param**, pero se usa para parámetros que son matrices de estilo C&mdash;también conocidas como *matrices conformes*.

|Tipos que puedes pasar|Notas|
|-|-|
| `{}` | Una matriz vacía.|
| **array** | Una matriz conforme de C (es decir, `C array[N];`), donde **C** se puede convertir a **T**, y `sizeof(C) == sizeof(T)`. |
| **std::array<C, N>** | Un C++ **std::array** de **C**, donde **C** se puede convertir a **T** y `sizeof(C) == sizeof(T)`. |
| **std::vector<C>** | Un C++ **std::vector** de **C**, donde **C** se puede convertir a **T** y `sizeof(C) == sizeof(T)`. |
| `{ T*, T* }` | Un par de punteros que representan el intervalo (comienzo, final).|
| **std::initializer_list\<T\>** ||

Consulta también la entrada de blog [Distintos patrones para pasar matrices de estilo C en el límite de ABI Windows Runtime](https://devblogs.microsoft.com/oldnewthing/20200205-00/?p=103398).