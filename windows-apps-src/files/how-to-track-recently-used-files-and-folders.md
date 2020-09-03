---
ms.assetid: BF929A68-9C82-4866-BC13-A32B3A550005
title: Seguimiento de los archivos y carpetas usados recientemente
description: Realiza un seguimiento de los archivos a los que el usuario accede con mayor frecuencia; para ello, agrégalos a la lista de elementos usados recientemente (MRU) de la aplicación.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 29d140672e80304bb0adb7081ba616e75d8d8ecd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173209"
---
# <a name="track-recently-used-files-and-folders"></a>Seguimiento de los archivos y carpetas usados recientemente

**API importantes**

- [**MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist)
- [**FileOpenPicker**](/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker)

Realiza un seguimiento de los archivos a los que el usuario accede con mayor frecuencia; para ello, agrégalos a la lista de elementos usados recientemente (MRU) de la aplicación. La plataforma administra la lista MRU automáticamente ordenando los elementos según la hora del último acceso y eliminando los más antiguos cuando se alcanza el límite de 25 elementos en la lista. Todas las aplicaciones tienen sus propias listas de MRU.

La lista de MRU de una aplicación se representa mediante la clase [**StorageItemMostRecentlyUsedList**](/uwp/api/Windows.Storage.AccessCache.StorageItemMostRecentlyUsedList), que se obtiene a partir de la propiedad estática [**StorageApplicationPermissions.MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist). Los elementos MRU se almacenan como objetos [**IStorageItem**](/uwp/api/Windows.Storage.IStorageItem), lo que significa que los objetos [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) (que representan archivos) y los objetos [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) (que representan carpetas) pueden agregarse a la lista de MRU.

> [!NOTE]
> Para ver muestras completas, consulte la [ muestra de selector de archivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker) y la [muestra de acceso de archivos ](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess).

## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica de las aplicaciones para Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

-   **Permisos de acceso a la ubicación**

    Consulta [Permisos de acceso de archivos](file-access-permissions.md).

-   [Abrir archivos y carpetas con un selector](quickstart-using-file-and-folder-pickers.md)

    Los archivos seleccionados, por lo general, son los archivos que los usuarios usan una y otra vez.

 ## <a name="add-a-picked-file-to-the-mru"></a>Agregar archivos seleccionados a la lista de MRU

-   Los archivos que el usuario selecciona son a menudo archivos que se usan repetidas veces. Por lo tanto, considera la posibilidad de agregar los archivos seleccionados a la lista de MRU de la aplicación conforme se seleccionan. A continuación se muestra cómo hacerlo.

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    var mru = Windows.Storage.AccessCache.StorageApplicationPermissions.MostRecentlyUsedList;
    string mruToken = mru.Add(file, "profile pic");
    ```

    [**StorageItemMostRecentlyUsedList.Add**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add) está sobrecargado. En el ejemplo, se usa [**Add(IStorageItem, String)** ](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add) para poder asociar metadatos al archivo. La configuración de metadatos permite grabar el propósito del elemento como, por ejemplo, "imagen de perfil". También puedes agregar el archivo sin metadatos a la lista de MRU, llamando al método [**Add(IStorageItem)** ](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add). Cuando se agrega un elemento a la lista de MRU, el método devuelve una cadena de identificación única, denominada token, que se usa para recuperar el elemento.

> [!TIP]
> Necesitará el token para recuperar un elemento de la lista de utilizado recientemente, así que consérvelo en algún lugar. Para más información sobre los datos de aplicación, consulta [Administrar datos de la aplicación](/previous-versions/windows/apps/hh465109(v=win.10)).

## <a name="use-a-token-to-retrieve-an-item-from-the-mru"></a>Usar un token para recuperar elementos de la lista de MRU

Usa el método de recuperación más apropiado para el elemento que quieres recuperar.

-   Puedes recuperar un archivo como un elemento [**StorageFile**](/uwp/api/Windows.Storage.StorageFile), mediante [**GetFileAsync**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getfileasync).
-   Puedes recuperar una carpeta como un elemento [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder), mediante [**GetFolderAsync**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getfolderasync).
-   Puedes recuperar un elemento [**IStorageItem**](/uwp/api/Windows.Storage.IStorageItem) genérico, que puede representar un archivo o carpeta, mediante [**GetItemAsync**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getitemasync).

A continuación se muestra cómo recuperar el archivo que acabamos de agregar.

```cs
StorageFile retrievedFile = await mru.GetFileAsync(mruToken);
```

A continuación se muestra cómo iterar todas las entradas para obtener tokens y después elementos.

```cs
foreach (Windows.Storage.AccessCache.AccessListEntry entry in mru.Entries)
{
    string mruToken = entry.Token;
    string mruMetadata = entry.Metadata;
    Windows.Storage.IStorageItem item = await mru.GetItemAsync(mruToken);
    // The type of item will tell you whether it's a file or a folder.
}
```

[  **AccessListEntryView**](/uwp/api/Windows.Storage.AccessCache.AccessListEntryView) permite iterar entradas de la lista de MRU. Estas entradas son estructuras [**AccessListEntry**](/uwp/api/Windows.Storage.AccessCache.AccessListEntry) que contienen el token y los metadatos de un elemento.

## <a name="removing-items-from-the-mru-when-its-full"></a>Quitar elementos de la lista de MRU cuando está llena

Cuando se alcanza el límite de 25 elementos en la lista de MRU y quieres agregar uno nuevo, el elemento más antiguo se eliminará automáticamente. Por lo tanto, nunca es necesario quitar un elemento antes de agregar uno nuevo.

## <a name="future-access-list"></a>Lista de acceso futuro

Además de una lista de MRU, tu aplicación también tiene una lista de acceso futuro. Mediante la selección de archivos y carpetas, el usuario concede permiso a la aplicación para acceder a elementos que es posible que no estén disponibles de otro modo. Si agregas estos elementos a la lista de acceso futuro, podrás conservar este permiso cuando la aplicación desee tener acceso a ellos más tarde. La lista de acceso futuro de la aplicación se representa mediante la clase [**StorageItemAccessList**](/uwp/api/Windows.Storage.AccessCache.StorageItemAccessList), que se obtiene a partir de la propiedad estática [**StorageApplicationPermissions.FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist).

Cuando un usuario selecciona un elemento, considera la posibilidad de agregarlo a la lista de acceso futuro, así como a la lista de MRU.

-   [  **FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) puede conservar hasta 1000 elementos. Recuerda: puede contener carpetas y archivos, lo que implica una gran cantidad de carpetas.
-   La plataforma nunca quita los elementos de [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) de forma automática. Cuando se alcance el límite de 1000 elementos, no podrás agregar más elementos hasta que no liberes espacio con el método [**Remove**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.remove).