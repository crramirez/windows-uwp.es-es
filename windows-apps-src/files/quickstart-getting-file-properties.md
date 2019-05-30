---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Obtener las propiedades de archivos
description: Obtener propiedades de &\#8212; de nivel superior, básico y extendido &\#8212; para un archivo representado por un StorageFile de objeto.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 01eda8eefea7e1b3b1102ef154a019e1630e80c2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369317"
---
# <a name="get-file-properties"></a>Obtener las propiedades de archivos

**API importantes**

-   [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)
-   [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.retrievepropertiesasync)

Obtiene las propiedades —de nivel superior, básicas y extendidas— de un archivo representado mediante un objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile).

> [!NOTE]
> Para obtener un ejemplo completo, vea el [ejemplo de acceso de archivo](https://go.microsoft.com/fwlink/p/?linkid=619995).

## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica para las aplicaciones de la plataforma Universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Permisos de acceso a la ubicación**

    Por ejemplo, el código de estos ejemplos requiere la funcionalidad **picturesLibrary**, pero es posible que la ubicación requiera una funcionalidad distinta o ninguna. Para más información, consulta [Permisos de acceso de archivos](file-access-permissions.md).

## <a name="getting-a-files-top-level-properties"></a>Obtener las propiedades de nivel superior de un archivo

Muchas de las propiedades de archivo de nivel superior son accesibles como miembros de la clase [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile). Estas propiedades incluyen los atributos del archivo, el tipo de contenido, la fecha de creación, el nombre para mostrar, el tipo de archivo, etc.

> [!NOTE]
> Recuerda declarar la funcionalidad **picturesLibrary**.

Este ejemplo enumera todos los archivos de la biblioteca Imágenes al tener acceso a unas cuantas propiedades de nivel superior de cada archivo.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get top-level file properties.
    fileProperties.AppendLine("File name: " + file.Name);
    fileProperties.AppendLine("File type: " + file.FileType);
}
```

## <a name="getting-a-files-basic-properties"></a>Obtener las propiedades básicas de un archivo

Muchas propiedades de archivo básicas se obtienen mediante una llamada al método [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync). Este método devuelve un objeto [**BasicProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties), que define propiedades para el tamaño del elemento (archivo o carpeta) y la última vez que se modificó el elemento.

Este ejemplo enumera todos los archivos de la biblioteca Imágenes y obtiene acceso a unas cuantas propiedades básicas de cada archivo.

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get file's basic properties.
    Windows.Storage.FileProperties.BasicProperties basicProperties =
        await file.GetBasicPropertiesAsync();
    string fileSize = string.Format("{0:n0}", basicProperties.Size);
    fileProperties.AppendLine("File size: " + fileSize + " bytes");
    fileProperties.AppendLine("Date modified: " + basicProperties.DateModified);
}
 ```

## <a name="getting-a-files-extended-properties"></a>Obtener las propiedades extendidas de un archivo

Además de las propiedades de archivo de nivel superior y básicas, hay muchas propiedades asociadas al contenido del archivo. Para acceder a estas propiedades extendidas hay que llamar al método [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync). (Un [ **BasicProperties** ](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties) objeto se obtiene mediante una llamada a la [ **StorageFile.Properties** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties) propiedad.) Mientras que las propiedades de archivo de nivel superior y básicos son accesibles como propiedades de una clase,[**StorageFile** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) y **BasicProperties**, respectivamente, son las propiedades extendidas Obtiene pasando una [IEnumerable](https://go.microsoft.com/fwlink/p/?LinkID=313091) colección de [cadena](https://go.microsoft.com/fwlink/p/?LinkID=325032) objetos que representan los nombres de las propiedades que se va a recuperar para el  **BasicProperties.RetrievePropertiesAsync** método. A continuación, este método devuelve una colección [IDictionary](https://go.microsoft.com/fwlink/p/?LinkId=325238). Cada propiedad extendida se recupera entonces de la colección por el nombre o índice.

En este ejemplo se enumeran todos los archivos de la biblioteca de imágenes, se especifican los nombres de las propiedades deseadas (**DataAccessed** y **FileOwner**) en un objeto [List](https://go.microsoft.com/fwlink/p/?LinkID=325246), se pasa ese objeto [List](https://go.microsoft.com/fwlink/p/?LinkID=325246) a [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) para recuperar esas propiedades y luego se recuperan por el nombre dichas propiedades del objeto [IDictionary](https://go.microsoft.com/fwlink/p/?LinkId=325238) devuelto.

Consulta las [Propiedades básicas de Windows](https://docs.microsoft.com/windows/desktop/properties/core-bumper) para obtener una lista completa de las propiedades extendidas de un archivo.

```csharp
const string dateAccessedProperty = "System.DateAccessed";
const string fileOwnerProperty = "System.FileOwner";

// Enumerate all files in the Pictures library.
var folder = KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Define property names to be retrieved.
    var propertyNames = new List<string>();
    propertyNames.Add(dateAccessedProperty);
    propertyNames.Add(fileOwnerProperty);

    // Get extended properties.
    IDictionary<string, object> extraProperties =
        await file.Properties.RetrievePropertiesAsync(propertyNames);

    // Get date-accessed property.
    var propValue = extraProperties[dateAccessedProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("Date accessed: " + propValue);
    }

    // Get file-owner property.
    propValue = extraProperties[fileOwnerProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("File owner: " + propValue);
    }
}
```

 

 
