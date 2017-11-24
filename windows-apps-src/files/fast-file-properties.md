---
author: laurenhughes
title: "Acceso rápido a las propiedades de archivo de UWP"
description: "Recopila de forma eficaz una lista de archivos y sus propiedades de una biblioteca para usarlos en una aplicación para UWP."
ms.author: lahugh
ms.date: 10/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, archivo, propiedades
localizationpriority: medium
ms.openlocfilehash: 1376774b619a94940d12b0c33439b9ebfe2e4a61
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2017
---
# <a name="fast-access-to-file-properties-in-uwp"></a>Acceso rápido a las propiedades de archivo de UWP 

Aprende rápidamente a recopilar una lista de archivos y sus propiedades de una biblioteca y usa esas propiedades en una aplicación.  

Prerrequisitos 
- **Programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**     
Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps). 
- **Permisos de acceso a Bibliotecas**  
El código de estos ejemplos requiere la funcionalidad **picturesLibrary**, pero es posible que la ubicación de tu archivo requiera una funcionalidad distinta o ninguna. Para más información, consulta [Permisos de acceso de archivos](https://docs.microsoft.com/windows/uwp/files/file-access-permissions). 
- **Enumeración sencilla de archivos**   
Este ejemplo usa [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) establecer unas cuantas propiedades avanzadas de enumeración. Para obtener más información sobre cómo obtener solo una simple lista de archivos para un directorio más pequeño, consulta [Enumerar y consultar archivos y carpetas](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders). 

## <a name="usage"></a>Uso  
Muchas aplicaciones necesitan enumerar las propiedades de un grupo de archivos, pero no siempre es necesario interactuar directamente con los archivos. Por ejemplo, un aplicación de música reproduce (o abre) un archivo a la vez, pero necesita las propiedades de todos los archivos en una carpeta para que la aplicación pueda mostrar la cola de canciones o el usuario pueda seleccionar un archivo válido para reproducirlo. 

Los ejemplos de esta página no deben usarse en aplicaciones que modifican los metadatos de todos los archivos o aplicaciones que interactúan con todos los StorageFiles resultantes más allá de leer sus propiedades. Consulta [Enumerar y consultar archivos y carpetas](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders) para obtener más información. 

## <a name="enumerate-all-the-pictures-in-a-location"></a>Enumerar todas las imágenes en una ubicación 
En este ejemplo, realizaremos lo siguiente:
-  Crear un objeto de [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) para especificar que la aplicación quiera enumerar los archivos tan rápido como sea posible.
-  Recuperar propiedades de archivo mediante la paginación de objetos de StorageFile en la aplicación. La paginación de archivos reduce la cantidad de memoria usada por la aplicación y mejora la capacidad de respuesta percibida.

### <a name="creating-the-query"></a>Creación de la consulta 
Para crear la consulta, usamos un objeto de QueryOptions para especificar que la aplicación está interesada en enumerar solo determinados tipos de archivos de imágenes y para filtrar los archivos protegidos con Windows Information Protection (System.Security.EncryptionOwners). 

Es importante establecer las propiedades a las que la aplicación tendrá acceso usando [QueryOptions.SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions#Windows_Storage_Search_QueryOptions_SetPropertyPrefetch_Windows_Storage_FileProperties_PropertyPrefetchOptions_Windows_Foundation_Collections_IIterable_System_String__). Si la aplicación accede a una propiedad no capturada previamente, esto provocará una importante penalización de rendimiento.

La configuración [IndexerOption.OnlyUseIndexerAndOptimzeForIndexedProperties](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.IndexerOption) indica al sistema que devuelva los resultados lo más rápido posible, pero solo para incluir las propiedades especificadas en [SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions#Windows_Storage_Search_QueryOptions_SetPropertyPrefetch_Windows_Storage_FileProperties_PropertyPrefetchOptions_Windows_Foundation_Collections_IIterable_System_String__). 

### <a name="paging-in-the-results"></a>Paginación en los resultados 
Los usuarios pueden tener miles o millones de archivos en su biblioteca de imágenes, por lo tanto, una llamada a [GetFilesAsync](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult#Windows_Storage_Search_StorageFileQueryResult_GetFilesAsync_System_UInt32_System_UInt32_) podría saturar su equipo porque se creará un StorageFile por cada imagen. Esto puede resolverse al crear un número fijo de StorageFiles a la vez, procesarlos en la interfaz de usuario y, a continuación, liberar la memoria. 

En nuestro ejemplo, hacemos esto usando [StorageFileQueryResult.GetFilesAsync (StartIndex UInt32, UInt32 maxNumberOfItems)](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult#Windows_Storage_Search_StorageFileQueryResult_GetFilesAsync_System_UInt32_System_UInt32_) para capturar 100 archivos a la vez. La aplicación procesará los archivos y permitirá que el sistema operativo libere, a continuación, esa memoria. Esta técnica captura la máxima memoria de la aplicación y garantiza que el sistema siga respondiendo. Por supuesto, tendrás que ajustar el número de archivos devueltos para tu escenario, pero para asegurarte de obtener una experiencia con capacidad de respuesta para todos los usuarios, se recomienda no capturar más de 500 archivos a la vez.


Muestra de C#  
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

### <a name="results"></a>Resultados 
Los archivos resultantes de StorageFile solo contienen las propiedades solicitadas, pero se devuelven 10 veces más rápido en comparación con el resto de IndexerOptions. La aplicación puede solicitar acceso a las propiedades que no están aún incluidas en la consulta, pero hay una penalización de rendimiento para abrir el archivo y recuperar esas propiedades.  

## <a name="adding-folders-to-libraries"></a>Agregar carpetas a Bibliotecas 
Las aplicaciones pueden solicitar al usuario agregar la ubicación al índice mediante [StorageLibrary.RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibrary#Windows_Storage_StorageLibrary_RequestAddFolderAsync). Una vez que se incluye la ubicación, se podrá indizar automáticamente y las aplicaciones pueden usar esta técnica para enumerar los archivos.
 
## <a name="see-also"></a>Consulta también
[Referencia de API de QueryOptions](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions)  
[Enumerar y consultar archivos y carpetas](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)  
[Permisos de acceso de archivos](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)   
 
 