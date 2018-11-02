---
author: laurenhughes
ms.assetid: BF929A68-9C82-4866-BC13-A32B3A550005
title: Seguimiento de los archivos y carpetas usados recientemente
description: Realiza un seguimiento de los archivos a los que el usuario accede con mayor frecuencia; para ello, agrégalos a la lista de elementos más usados recientemente (MRU) de la aplicación.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 12b8a6462f6cc39ba85cddfaa7a92212955a79f5
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5969350"
---
# <a name="track-recently-used-files-and-folders"></a>Seguimiento de los archivos y carpetas usados recientemente

**API importantes**

- [**MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458)
- [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369)

Realiza un seguimiento de los archivos a los que el usuario accede con mayor frecuencia; para ello, agrégalos a la lista de elementos usados recientemente (MRU) de la aplicación. La plataforma administra la lista MRU automáticamente ordenando los elementos según la hora del último acceso y eliminando los más antiguos cuando se alcanza el límite de 25 elementos en la lista. Todas las aplicaciones tienen sus propias listas de MRU.

La lista de MRU de una aplicación se representa mediante la clase [**StorageItemMostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207475), que se obtiene a partir de la propiedad estática [**StorageApplicationPermissions.MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458). Los elementos MRU se almacenan como objetos [**IStorageItem**](https://msdn.microsoft.com/library/windows/apps/br227129), lo que significa que los objetos [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) (que representan archivos) y los objetos [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) (que representan carpetas) pueden agregarse a la lista de MRU.

> [!NOTE]
> Consulta también la [Muestra de selector de archivos](http://go.microsoft.com/fwlink/p/?linkid=619994) y la [Muestra de acceso de archivos](http://go.microsoft.com/fwlink/p/?linkid=619995).

 

## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Permisos de acceso a la ubicación**

    Consulta [Permisos de acceso de archivos](file-access-permissions.md).

-   [Abrir archivos y carpetas con un selector](quickstart-using-file-and-folder-pickers.md)

    Los archivos seleccionados, por lo general, son los archivos que los usuarios usan una y otra vez.

 ## <a name="add-a-picked-file-to-the-mru"></a>Agregar archivos seleccionados a la lista de MRU

-   Los archivos que el usuario selecciona son a menudo archivos que se usan repetidas veces. Por lo tanto, considera la posibilidad de agregar los archivos seleccionados a la lista de MRU de la aplicación conforme se seleccionan. A continuación se muestran cómo hacerlo.

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    var mru = Windows.Storage.AccessCache.StorageApplicationPermissions.MostRecentlyUsedList;
    string mruToken = mru.Add(file, "profile pic");
    ```

    [**StorageItemMostRecentlyUsedList.Add**](https://msdn.microsoft.com/library/windows/apps/br207476) es un método sobrecargado. En el ejemplo, se usa [**Add(IStorageItem, String)**](https://msdn.microsoft.com/library/windows/apps/br207481) para poder asociar metadatos al archivo. La configuración de metadatos permite grabar el propósito del elemento como, por ejemplo, "imagen de perfil". También puedes agregar el archivo sin metadatos a la lista de MRU, llamando al método [**Add(IStorageItem)**](https://msdn.microsoft.com/library/windows/apps/br207480). Cuando se agrega un elemento a la lista de MRU, el método devuelve una cadena de identificación única, denominada token, que se usa para recuperar el elemento.

> [!TIP]
> Necesitarás el token para recuperar un elemento de la lista de MRU, así que insiste hasta encontrarlo. Para más información sobre los datos de aplicación, consulta [Administrar datos de la aplicación](https://msdn.microsoft.com/library/windows/apps/hh465109).

## <a name="use-a-token-to-retrieve-an-item-from-the-mru"></a>Usar un token para recuperar elementos de la lista de MRU

Usa el método de recuperación más apropiado para el elemento que quieres recuperar.

-   Puedes recuperar un archivo como un elemento [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171), mediante [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br207486).
-   Puedes recuperar una carpeta como un elemento [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230), mediante [**GetFolderAsync**](https://msdn.microsoft.com/library/windows/apps/br207489).
-   Puedes recuperar un elemento [**IStorageItem**](https://msdn.microsoft.com/library/windows/apps/br227129) genérico, que puede representar un archivo o carpeta, mediante [**GetItemAsync**](https://msdn.microsoft.com/library/windows/apps/br207492).

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

[**AccessListEntryView**](https://msdn.microsoft.com/library/windows/apps/br227349) permite iterar entradas de la lista de MRU. Estas entradas son estructuras [**AccessListEntry**](https://msdn.microsoft.com/library/windows/apps/br227348) que contienen el token y los metadatos de un elemento.

## <a name="removing-items-from-the-mru-when-its-full"></a>Quitar elementos de la lista de MRU cuando está llena

Cuando se alcanza el límite de 25 elementos en la lista de MRU y quieres agregar uno nuevo, el elemento más antiguo se eliminará automáticamente. Por lo tanto, nunca es necesario quitar un elemento antes de agregar uno nuevo.

## <a name="future-access-list"></a>Lista de acceso futuro

Además de una lista de MRU, tu aplicación también tiene una lista de acceso futuro. Mediante la selección de archivos y carpetas, el usuario concede permiso a la aplicación para acceder a elementos que es posible que no estén disponibles de otro modo. Si agregas estos elementos a la lista de acceso futuro, podrás conservar este permiso cuando la aplicación desee tener acceso a ellos más tarde. La lista de acceso futuro de la aplicación se representa mediante la clase [**StorageItemAccessList**](https://msdn.microsoft.com/library/windows/apps/br207459), que se obtiene a partir de la propiedad estática [**StorageApplicationPermissions.FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457).

Cuando un usuario selecciona un elemento, considera la posibilidad de agregarlo a la lista de acceso futuro, así como a la lista de MRU.

-   [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) puede conservar hasta 1000 elementos. Recuerda: puede contener carpetas y archivos, lo que implica una gran cantidad de carpetas.
-   La plataforma nunca quita los elementos de [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) de forma automática. Cuando se alcance el límite de 1000 elementos, no podrás agregar más elementos hasta que no liberes espacio con el método [**Remove**](https://msdn.microsoft.com/library/windows/apps/br207497).
