---
title: Acceso rápido a las propiedades de archivos de UWP
description: Recopila de forma eficaz una lista de archivos y sus propiedades de una biblioteca para usarlos en una aplicación para UWP.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, archivo, propiedades
ms.localizationpriority: medium
ms.openlocfilehash: 75d835a5f7de6fd26e6a05a80b5f47a8fe0c0e1b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173229"
---
# <a name="fast-access-to-file-properties-in-uwp"></a>Acceso rápido a las propiedades de archivos de UWP 

Aprenda rápidamente a recopilar una lista de archivos y sus propiedades de una biblioteca y use esas propiedades en una aplicación.  

Requisitos previos 
- **Programación asincrónica para aplicaciones para la Plataforma universal de Windows (UWP)**     Para aprender a escribir aplicaciones asincrónicas en C# o Visual Basic, consulta [Llamar a API asincrónicas en C# o Visual Basic](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md). 
- **Permisos de acceso a bibliotecas**   El código en estos ejemplos requiere la funcionalidad **picturesLibrary**, pero es posible que la ubicación de su archivo requiera otra funcionalidad o que no requiera ninguna. Para más información, consulta [Permisos de acceso de archivos](./file-access-permissions.md). 
- **Enumeración de archivo sencilla**    Este ejemplo usa [QueryOptions](/uwp/api/Windows.Storage.Search.QueryOptions) para establecer algunas propiedades avanzadas de enumeración. Para obtener más información sobre cómo obtener una simple lista de archivos para un directorio más pequeño, consulte [Enumerar y consultar archivos y carpetas](./quickstart-listing-files-and-folders.md). 

## <a name="usage"></a>Uso  
Muchas aplicaciones necesitan enumerar las propiedades de un grupo de archivos, pero no siempre es necesario interactuar directamente con los archivos. Por ejemplo, un aplicación de música reproduce (o abre) un archivo a la vez, pero necesita las propiedades de todos los archivos en una carpeta para que la aplicación pueda mostrar la cola de canciones o el usuario pueda elegir un archivo válido para reproducirlo. 

Los ejemplos de esta página no deben usarse en aplicaciones que modifican los metadatos de todos los archivos o aplicaciones que interactúan con todos los StorageFiles resultantes más allá de leer sus propiedades. Consulte [Enumerar y consultar archivos y carpetas](./quickstart-listing-files-and-folders.md) para obtener más información. 

## <a name="enumerate-all-the-pictures-in-a-location"></a>Enumerar todas las imágenes en una ubicación 
En este ejemplo, realizaremos lo siguiente:
-  Crearemos un objeto de [QueryOptions](/uwp/api/Windows.Storage.Search.QueryOptions) para especificar que la aplicación quiera enumerar los archivos tan rápido como sea posible.
-  Recuperaremos propiedades de archivo mediante la paginación de objetos de StorageFile en la aplicación. La paginación de archivos reduce la cantidad de memoria usada por la aplicación y mejora la capacidad de respuesta percibida.

### <a name="creating-the-query"></a>Creación de la consulta 
Para crear la consulta, usamos un objeto de QueryOptions para especificar que la aplicación está interesada en enumerar solo determinados tipos de archivos de imágenes y para filtrar los archivos protegidos con Windows Information Protection (System.Security.EncryptionOwners). 

Es importante establecer las propiedades a las que la aplicación tendrá acceso usando [QueryOptions.SetPropertyPrefetch](/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch). Si la aplicación accede a una propiedad no capturada previamente, esto provocará una importante penalización de rendimiento.

La configuración [IndexerOption.OnlyUseIndexerAndOptimzeForIndexedProperties](/uwp/api/Windows.Storage.Search.IndexerOption) indica al sistema que devuelva los resultados lo más rápido posible, pero solo para incluir las propiedades especificadas en [SetPropertyPrefetch](/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch). 

### <a name="paging-in-the-results"></a>Paginación en los resultados 
Los usuarios pueden tener miles o millones de archivos en su biblioteca de imágenes, por lo tanto, una llamada a [GetFilesAsync](/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) podría saturar su equipo porque se creará un StorageFile por cada imagen. Esto puede resolverse al crear un número fijo de StorageFiles a la vez, procesarlos en la interfaz de usuario y, a continuación, liberando la memoria. 

En nuestro ejemplo, hacemos esto usando [torageFileQueryResult.GetFilesAsync(UInt32 StartIndex, UInt32 maxNumberOfItems)](/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) para capturar solamente 100 archivos a la vez. La aplicación procesará los archivos y permitirá que el sistema operativo libere, a continuación, esa memoria. Esta técnica limita la máxima memoria de la aplicación y garantiza que el sistema siga respondiendo. Por supuesto, tendrá que ajustar el número de archivos devueltos para su escenario, pero para asegurarse de obtener una experiencia con capacidad de respuesta para todos los usuarios, se recomienda no capturar más de 500 archivos a la vez.


**Ejemplo**  
```csharp
StorageFolder folderToEnumerate = KnownFolders.PicturesLibrary; 
// Check if the folder is indexed before doing anything. 
IndexedState folderIndexedState = await folderToEnumerate.GetIndexedStateAsync(); 
if (folderIndexedState == IndexedState.NotIndexed || folderIndexedState == IndexedState.Unknown) 
{ 
    // Only possible in indexed directories.  
    return; 
} 
 
QueryOptions picturesQuery = new QueryOptions() 
{ 
    FolderDepth = FolderDepth.Deep, 
    // Filter out all files that have WIP enabled
    ApplicationSearchFilter = "System.Security.EncryptionOwners:[]", 
    IndexerOption = IndexerOption.OnlyUseIndexerAndOptimizeForIndexedProperties 
}; 

picturesQuery.FileTypeFilter.Add(".jpg"); 
string[] otherProperties = new string[] 
{ 
    SystemProperties.GPS.LatitudeDecimal, 
    SystemProperties.GPS.LongitudeDecimal 
}; 
 
picturesQuery.SetPropertyPrefetch(PropertyPrefetchOptions.BasicProperties | PropertyPrefetchOptions.ImageProperties, 
                                    otherProperties); 
SortEntry sortOrder = new SortEntry() 
{ 
    AscendingOrder = true, 
    PropertyName = "System.FileName" // FileName property is used as an example. Any property can be used here.  
}; 
picturesQuery.SortOrder.Add(sortOrder); 
 
// Create the query and get the results 
uint index = 0; 
const uint stepSize = 100; 
if (!folderToEnumerate.AreQueryOptionsSupported(picturesQuery)) 
{ 
    log("Querying for a sort order is not supported in this location"); 
    picturesQuery.SortOrder.Clear(); 
} 
StorageFileQueryResult queryResult = folderToEnumerate.CreateFileQueryWithOptions(picturesQuery); 
IReadOnlyList<StorageFile> images = await queryResult.GetFilesAsync(index, stepSize); 
while (images.Count != 0 || index < 10000) 
{ 
    foreach (StorageFile file in images) 
    { 
        // With the OnlyUseIndexerAndOptimizeForIndexedProperties set, this won't  
        // be async. It will run synchronously. 
        var imageProps = await file.Properties.GetImagePropertiesAsync(); 
 
        // Build the UI 
        log(String.Format("{0} at {1}, {2}", 
                    file.Path, 
                    imageProps.Latitude, 
                    imageProps.Longitude)); 
    } 
    index += stepSize; 
    images = await queryResult.GetFilesAsync(index, stepSize); 
} 
```

### <a name="results"></a>Results 
Los archivos resultantes de StorageFile solo contienen las propiedades solicitadas, pero se devuelven 10 veces más rápido en comparación con el resto de IndexerOptions. La aplicación puede solicitar acceso a las propiedades que no están aún incluidas en la consulta, pero hay una penalización de rendimiento para abrir el archivo y recuperar esas propiedades.  

## <a name="adding-folders-to-libraries"></a>Agregar carpetas a Bibliotecas 
Las aplicaciones pueden solicitar al usuario agregar la ubicación al índice mediante [StorageLibrary.RequestAddFolderAsync](/uwp/api/Windows.Storage.StorageLibrary.RequestAddFolderAsync). Una vez que se incluye la ubicación, se podrá indizar automáticamente y las aplicaciones pueden usar esta técnica para enumerar los archivos.
 
## <a name="see-also"></a>Consulta también
[Referencia de API de QueryOptions](/uwp/api/windows.storage.search.queryoptions)  
[Enumerar y consultar archivos y carpetas](./quickstart-listing-files-and-folders.md)  
[Permisos de acceso a archivos](./file-access-permissions.md)  
 
 