---
author: laurenhughes
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: Obtener las propiedades de archivos
description: Obtén las propiedades (de nivel superior, básicas y extendidas) de un archivo representado mediante un objeto StorageFile.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 8fc44300376efb5b56f390457e516f35a3ec4202
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6662672"
---
# <a name="get-file-properties"></a>Obtener las propiedades de archivos



**API importantes**

-   [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737)
-   [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770652)

Obtiene las propiedades —de nivel superior, básicas y extendidas— de un archivo representado mediante un objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

> [!NOTE]
> Consulta también la [Muestra de acceso de archivos](http://go.microsoft.com/fwlink/p/?linkid=619995).

 


## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Permisos de acceso a la ubicación**

    Por ejemplo, el código de estos ejemplos requiere la funcionalidad **picturesLibrary**, pero es posible que la ubicación requiera una funcionalidad distinta o ninguna. Para más información, consulta [Permisos de acceso de archivos](file-access-permissions.md).

## <a name="getting-a-files-top-level-properties"></a>Obtener las propiedades de nivel superior de un archivo

Muchas de las propiedades de archivo de nivel superior son accesibles como miembros de la clase [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). Estas propiedades incluyen los atributos del archivo, el tipo de contenido, la fecha de creación, el nombre para mostrar, el tipo de archivo, etc.

**Nota**recuerda declarar la funcionalidad de **picturesLibrary** .

 

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

Muchas propiedades de archivo básicas se obtienen mediante una llamada al método [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737). Este método devuelve un objeto [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113), que define propiedades para el tamaño del elemento (archivo o carpeta) y la última vez que se modificó el elemento.

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

Además de las propiedades de archivo de nivel superior y básicas, hay muchas propiedades asociadas al contenido del archivo. Para acceder a estas propiedades extendidas hay que llamar al método [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124). (Un objeto [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113) se obtiene mediante una llamada a la propiedad [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)). Si bien se puede acceder a las propiedades de archivo de nivel superior y básico como propiedades de una clase, [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) y **BasicProperties**, respectivamente, las propiedades extendidas se obtienen al pasar una colección [IEnumerable](http://go.microsoft.com/fwlink/p/?LinkID=313091) de objetos [String](http://go.microsoft.com/fwlink/p/?LinkID=325032) que representan los nombres de las propiedades que deben recuperarse en el método **BasicProperties.RetrievePropertiesAsync**. A continuación, este método devuelve una colección [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238). Cada propiedad extendida se recupera entonces de la colección por el nombre o índice.

En este ejemplo se enumeran todos los archivos de la biblioteca de imágenes, se especifican los nombres de las propiedades deseadas (**DataAccessed** y **FileOwner**) en un objeto [List](http://go.microsoft.com/fwlink/p/?LinkID=325246), se pasa ese objeto [List](http://go.microsoft.com/fwlink/p/?LinkID=325246) a [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124) para recuperar esas propiedades y luego se recuperan por el nombre dichas propiedades del objeto [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238) devuelto.

Consulta las [Propiedades básicas de Windows](https://msdn.microsoft.com/library/windows/desktop/mt805470) para obtener una lista completa de las propiedades extendidas de un archivo.

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

 

 
