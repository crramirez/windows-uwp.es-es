---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Obtener las propiedades de archivos
description: Obtén las propiedades&\#8212;de nivel superior, básicas y extendidas&\#8212; de un archivo representado mediante un objeto StorageFile.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 81fcb4b62f9a10e8ff1fcb233c95317746cdb3b0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259583"
---
# <a name="get-file-properties"></a>Obtener las propiedades de archivos

**API importantes**

-   [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)
-   [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.retrievepropertiesasync)

Obtiene las propiedades —de nivel superior, básicas y extendidas— de un archivo representado mediante un objeto [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile).

> [!NOTE]
> Para obtener una muestra completa, consulta [File access sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) (Muestra de acceso a archivos).

## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica de las aplicaciones para Plataforma universal de Windows (UWP)**

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

Además de las propiedades de archivo de nivel superior y básicas, hay muchas propiedades asociadas al contenido del archivo. Para acceder a estas propiedades extendidas hay que llamar al método [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync). (Un objeto [**BasicProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties) se obtiene mediante una llamada a la propiedad [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties)). Si bien se puede acceder a las propiedades de archivo de nivel superior y básico como propiedades de una clase, [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) y **BasicProperties**, respectivamente, las propiedades extendidas se obtienen al pasar una colección [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) de objetos [String](https://msdn.microsoft.com/library/system.string.aspx) que representan los nombres de las propiedades que deben recuperarse en el método **BasicProperties.RetrievePropertiesAsync**. A continuación, este método devuelve una colección [IDictionary](https://msdn.microsoft.com/library/system.collections.idictionary.aspx). Cada propiedad extendida se recupera entonces de la colección por el nombre o índice.

En este ejemplo se enumeran todos los archivos de la biblioteca de imágenes, se especifican los nombres de las propiedades deseadas (**DataAccessed** y **FileOwner**) en un objeto [List](https://msdn.microsoft.com/library/6sh2ey19.aspx), se pasa ese objeto [List](https://msdn.microsoft.com/library/6sh2ey19.aspx) a [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) para recuperar esas propiedades y luego se recuperan por el nombre dichas propiedades del objeto [IDictionary](https://msdn.microsoft.com/library/system.collections.idictionary.aspx) devuelto.

Consulta [Windows Core Properties](https://docs.microsoft.com/windows/desktop/properties/core-bumper) (Propiedades básicas de Windows) para obtener una lista completa de las propiedades extendidas de un archivo.

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

 

 
