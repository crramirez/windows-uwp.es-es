---
author: laurenhughes
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: Determinar la disponibilidad de los archivos de Microsoft OneDrive
description: Determina si un archivo de Microsoft OneDrive está disponible mediante la propiedad StorageFile.IsAvailable.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 87eb93fbc100d143ab9fe75d34bb9c4d2caaf01d
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5864614"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>Determinar la disponibilidad de los archivos de Microsoft OneDrive


**API importantes**

-   [**Clase FileIO**](https://msdn.microsoft.com/library/windows/apps/Hh701440)
-   [**Clase StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171)
-   [**Propiedad StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx)

Determina si un archivo de Microsoft OneDrive está disponible mediante la propiedad [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx).

## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/Mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://msdn.microsoft.com/library/windows/apps/Mt187334).

-   **Declaraciones de funcionalidades de la aplicación**

    Consulta [Permisos de acceso de archivos](file-access-permissions.md).

## <a name="using-the-storagefileisavailable-property"></a>Usar la propiedad StorageFile.IsAvailable

Los usuarios pueden marcar los archivos de OneDrive como disponibles sin conexión (valor predeterminado) o disponibles solo en línea. Esta función les permite mover archivos grandes (como imágenes y vídeos) a OneDrive, marcarlos como disponibles solo en línea y ahorrar espacio en disco (lo único que se guarda de manera local es un archivo de metadatos).

[**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) se usa para determinar si un archivo está disponible actualmente. En la siguiente tabla se muestra el valor de la propiedad **StorageFile.IsAvailable** en diversos escenarios.

| Tipo de archivo                              | Con conexión | Red de uso medido        | Sin conexión |
|-------------------------------------------|--------|------------------------|---------|
| Archivo local                                | True   | True                   | True    |
| Archivo de OneDrive marcado como disponible sin conexión | True   | True                   | True    |
| Archivo de OneDrive marcado como disponible solo en línea       | True   | Según la configuración del usuario | False   |
| Archivo de red                              | True   | Según la configuración del usuario | False   |

 

Los pasos siguientes ilustran cómo determinar si un archivo está disponible actualmente.

1.  Declara una funcionalidad adecuada para la biblioteca a la que deseas obtener acceso.
2.  Incluye el espacio de nombres [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346). Este espacio de nombres incluye los tipos para administrar archivos, carpetas y opciones de configuración de la aplicación. También incluye el tipo [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) necesario.
3.  Adquiere un objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) para los archivos deseados. Si deseas enumerar una biblioteca, este paso suele completarse mediante una llamada al método [**StorageFolder.CreateFileQuery**](https://msdn.microsoft.com/library/windows/apps/BR227252), seguida de una llamada al método [**GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276.aspx) del objeto [**StorageFileQueryResult**](https://msdn.microsoft.com/library/windows/apps/BR208046) resultante. El método **GetFilesAsync** devuelve una colección [IReadOnlyList](http://go.microsoft.com/fwlink/p/?LinkId=324970) de objetos **StorageFile**.
4.  Cuando ya tengas acceso a un objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) que represente los archivos deseados, el valor de la propiedad [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) reflejará si el archivo está disponible o no.

El siguiente método genérico muestra cómo enumerar cualquier carpeta y devolver la colección de objetos [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/BR227171) para dicha carpeta. El método que llama itera en la colección devuelta haciendo referencia a la propiedad [**StorageFile.IsAvailable**](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx) en cada archivo.

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
