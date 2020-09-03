---
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: Determinación de la disponibilidad de los archivos de Microsoft OneDrive
description: Determina si un archivo de Microsoft OneDrive está disponible mediante la propiedad StorageFile.IsAvailable.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8eda7226547706cb7a8a4ef69d04407d749a0c86
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159449"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>Determinación de la disponibilidad de los archivos de Microsoft OneDrive


**API importantes**

-   [**Clase FileIO**](/uwp/api/Windows.Storage.FileIO)
-   [**Clase StorageFile**](/uwp/api/Windows.Storage.StorageFile)
-   [**Propiedad StorageFile.IsAvailable**](/uwp/api/windows.storage.storagefile.isavailable)

Determina si un archivo de Microsoft OneDrive está disponible mediante la propiedad [**StorageFile.IsAvailable**](/uwp/api/windows.storage.storagefile.isavailable).

## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica de las aplicaciones para Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

-   **Declaraciones de funcionalidades de la aplicación**

    Consulta [Permisos de acceso de archivos](file-access-permissions.md).

## <a name="using-the-storagefileisavailable-property"></a>Usar la propiedad StorageFile.IsAvailable

Los usuarios pueden marcar los archivos de OneDrive como disponibles sin conexión (valor predeterminado) o disponibles solo en línea. Esta función les permite mover archivos grandes (como imágenes y vídeos) a OneDrive, marcarlos como disponibles solo en línea y ahorrar espacio en disco (lo único que se guarda de manera local es un archivo de metadatos).

[**StorageFile.IsAvailable**](/uwp/api/windows.storage.storagefile.isavailable) se usa para determinar si un archivo está disponible actualmente. En la siguiente tabla se muestra el valor de la propiedad **StorageFile.IsAvailable** en diversos escenarios.

| Tipo de archivo                              | En línea | Red de uso medido        | Sin conexión |
|-------------------------------------------|--------|------------------------|---------|
| Archivo local                                | True   | True                   | True    |
| Archivo de OneDrive marcado como disponible sin conexión | True   | True                   | True    |
| Archivo de OneDrive marcado como disponible solo en línea       | True   | Según la configuración del usuario | False   |
| Archivo de red                              | True   | Según la configuración del usuario | False   |

 

Los pasos siguientes ilustran cómo determinar si un archivo está disponible actualmente.

1.  Declara una funcionalidad adecuada para la biblioteca a la que deseas obtener acceso.
2.  Incluye el espacio de nombres [**Windows.Storage**](/uwp/api/Windows.Storage). Este espacio de nombres incluye los tipos para administrar archivos, carpetas y opciones de configuración de la aplicación. También incluye el tipo [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) necesario.
3.  Adquiere un objeto [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) para los archivos deseados. Si deseas enumerar una biblioteca, este paso suele completarse mediante una llamada al método [**StorageFolder.CreateFileQuery**](/uwp/api/windows.storage.storagefolder.createfilequery), seguida de una llamada al método [**GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync) del objeto [**StorageFileQueryResult**](/uwp/api/Windows.Storage.Search.StorageFileQueryResult) resultante. El método **GetFilesAsync** devuelve una colección [IReadOnlyList](/dotnet/api/system.collections.generic.ireadonlylist-1) de objetos **StorageFile**.
4.  Cuando ya tengas acceso a un objeto [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) que represente los archivos deseados, el valor de la propiedad [**StorageFile.IsAvailable**](/uwp/api/windows.storage.storagefile.isavailable) reflejará si el archivo está disponible o no.

El siguiente método genérico muestra cómo enumerar cualquier carpeta y devolver la colección de objetos [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) para dicha carpeta. El método que llama itera en la colección devuelta haciendo referencia a la propiedad [**StorageFile.IsAvailable**](/uwp/api/windows.storage.storagefile.isavailable) en cada archivo.

```cs
/// <summary>
/// Generic function that retrieves all files from the specified folder.
/// </summary>
/// <param name="folder">The folder to be searched.</param>
/// <returns>An IReadOnlyList collection containing the file objects.</returns>
async Task<System.Collections.Generic.IReadOnlyList<StorageFile>> GetLibraryFilesAsync(StorageFolder folder)
{
    var query = folder.CreateFileQuery();
    return await query.GetFilesAsync();
}

private async void CheckAvailabilityOfFilesInPicturesLibrary()
{
    // Determine availability of all files within Pictures library.
    var files = await GetLibraryFilesAsync(KnownFolders.PicturesLibrary);
    for (int i = 0; i < files.Count; i++)
    {
        StorageFile file = files[i];

        StringBuilder fileInfo = new StringBuilder();
        fileInfo.AppendFormat("{0} (on {1}) is {2}",
                    file.Name,
                    file.Provider.DisplayName,
                    file.IsAvailable ? "available" : "not available");
    }
}
```