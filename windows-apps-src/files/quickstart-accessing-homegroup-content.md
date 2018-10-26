---
author: laurenhughes
ms.assetid: 12ECEA89-59D2-4BCE-B24C-5A4DD525E0C7
title: Acceso a contenido de Grupo Hogar
description: Obtén acceso al contenido almacenado en la carpeta Grupo Hogar, que incluye imágenes, música y vídeos.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 398c44db6a391008605ed6fa4dad877bcead035d
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5562994"
---
# <a name="accessing-homegroup-content"></a>Acceso a contenido de Grupo Hogar



**API importantes**

-   [**Clase Windows.Storage.KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151)

Obtén acceso al contenido almacenado en la carpeta Grupo Hogar, que incluye imágenes, música y vídeos.

## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Declaraciones de funcionalidades de la aplicación**

    Para acceder al contenido del Grupo Hogar, el equipo del usuario debe tener un Grupo Hogar configurado y la aplicación debe contar con al menos una de las siguientes funcionalidades: **picturesLibrary**, **musicLibrary** o **videosLibrary**. Cuando la aplicación obtiene acceso a la carpeta Grupo Hogar, podrá ver solamente las bibliotecas que corresponden a las funcionalidades declaradas en el manifiesto de la aplicación. Para más información, consulta [Permisos de acceso de archivos](file-access-permissions.md).

    > [!NOTE]
    >  El contenido de la biblioteca de documentos de un Grupo Hogar no está visible para la aplicación independientemente de las funcionalidades declaradas en el manifiesto de la aplicación y de la configuración de uso compartido del usuario.     

-   **Comprender cómo usar los selectores de archivos**

    Generalmente, el selector de archivos se usa para tener acceso a los archivos y carpetas del Grupo Hogar. Para aprender a usar el selector de archivo, consulta [Apertura de archivos y carpetas con un selector](quickstart-using-file-and-folder-pickers.md).

-   **Descripción de las consultas de archivos y carpetas**

    Puedes usar las consultas para enumerar los archivos y carpetas del Grupo Hogar. Para más información sobre consultas de archivos y carpetas, consulta [Enumeración y consulta de archivos y carpetas](quickstart-listing-files-and-folders.md).

## <a name="open-the-file-picker-at-the-homegroup"></a>Abrir el selector de archivos en el Grupo Hogar

Sigue estos pasos para abrir una instancia del selector de archivos que permite al usuario seleccionar los archivos y carpetas del Grupo Hogar:

1.  **Crear y personalizar el selector de archivos**

    Usa [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) para crear el selector de archivos y después establece la propiedad [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) del selector en [**PickerLocationId.HomeGroup**](https://msdn.microsoft.com/library/windows/apps/br207890). O bien, establece otras propiedades que sean relevantes para los usuarios y la aplicación. Para obtener directrices que te ayuden a decidir cómo personalizar el selector de archivos, consulta [Directrices y lista de comprobación para selectores de archivos](https://msdn.microsoft.com/library/windows/apps/hh465182)

    En este ejemplo se crea un selector de archivos que se abre en el Grupo Hogar, incluye archivos de cualquier tipo y muestra los archivos como imágenes en miniatura:
    ```cs
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add("*");
    ```

2.  **Mostrar el selector de archivos y procesar el archivo seleccionado.**

    Después de crear y personalizar el selector de archivos, permite que el usuario seleccione un archivo llamando a [**FileOpenPicker.PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275), o varios archivos llamando a [**FileOpenPicker.PickMultipleFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br207851).

    En este ejemplo se muestra el selector de archivos que permite al usuario seleccionar un archivo:
    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    if (file != null)
    {
        // Do something with the file.
    }
    else
    {
        // No file returned. Handle the error.
    }   
    ```

## <a name="search-the-homegroup-for-files"></a>Buscar archivos en el Grupo Hogar

En esta sección se muestra cómo buscar elementos del Grupo Hogar que coinciden con un término de consulta proporcionado por el usuario.

1.  **Obtener el término de consulta del usuario.**

    Aquí obtenemos un término de consulta que el usuario especificó en un control [**TextBox**](https://msdn.microsoft.com/library/windows/apps/br209683) llamado `searchQueryTextBox`:
    ```cs
    string queryTerm = this.searchQueryTextBox.Text;    
    ```

2.  **Establece las opciones de consulta y el filtro de búsqueda.**

    Las opciones de consulta determinan de qué manera se ordenan los resultados de la búsqueda, mientras que el filtro de búsqueda determina qué elementos se incluyen en los resultados de la búsqueda.

    En este ejemplo se establecen las opciones de consulta que ordenan los resultados de la búsqueda por relevancia y luego por la fecha de modificación. El filtro de búsqueda es el término de la consulta que el usuario especificó en el paso anterior:
    ```cs
    Windows.Storage.Search.QueryOptions queryOptions =
            new Windows.Storage.Search.QueryOptions
                (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
    queryOptions.UserSearchFilter = queryTerm.Text;
    Windows.Storage.Search.StorageFileQueryResult queryResults =
            Windows.Storage.KnownFolders.HomeGroup.CreateFileQueryWithOptions(queryOptions);    
    ```

3.  **Ejecutar la consulta y procesar los resultados.**

    En el siguiente ejemplo se ejecuta la consulta de búsqueda en el Grupo Hogar y se guardan los nombres de cualquier archivo coincidente como una lista de cadenas.
    ```cs
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
        await queryResults.GetFilesAsync();

    if (files.Count > 0)
    {
        outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
        foreach (Windows.Storage.StorageFile file in files)
        {
            outputString += file.Name + "\n";
        }
    }    
    ```


## <a name="search-the-homegroup-for-a-particular-users-shared-files"></a>Buscar archivos compartidos de un usuario en particular en el Grupo Hogar

En esta sección se muestra cómo buscar archivos del Grupo Hogar que comparte un usuario en particular.

1.  **Obtén una colección de usuarios del Grupo Hogar.**

    Cada una de las carpetas de primer nivel del Grupo Hogar representa un usuario de Grupo Hogar individual. Por lo tanto, para obtener una colección de usuarios de Grupo Hogar, llama a [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227279) para recuperar las carpetas de Grupo Hogar de nivel superior.
    ```cs
    System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFolder> hgFolders =
        await Windows.Storage.KnownFolders.HomeGroup.GetFoldersAsync();    
    ```

2.  **Busca la carpeta del usuario de destino y crea una consulta de archivo específica para la carpeta del usuario en particular.**

    El ejemplo siguiente itera a través de las carpetas recuperadas para buscar la carpeta del usuario de destino. Después, establece las opciones de consulta para buscar todos los archivos de la carpeta, ordenados primero por importancia y posteriormente por la fecha de modificación. El ejemplo crea una cadena que informa acerca del número de archivos encontrados, junto con los nombres de los archivos.
    ```cs
    bool userFound = false;
    foreach (Windows.Storage.StorageFolder folder in hgFolders)
    {
        if (folder.DisplayName == targetUserName)
        {
            // Found the target user's folder, now find all files in the folder.
            userFound = true;
            Windows.Storage.Search.QueryOptions queryOptions =
                new Windows.Storage.Search.QueryOptions
                    (Windows.Storage.Search.CommonFileQuery.OrderBySearchRank, null);
            queryOptions.UserSearchFilter = "*";
            Windows.Storage.Search.StorageFileQueryResult queryResults =
                folder.CreateFileQueryWithOptions(queryOptions);
            System.Collections.Generic.IReadOnlyList<Windows.Storage.StorageFile> files =
                await queryResults.GetFilesAsync();

            if (files.Count > 0)
            {
                string outputString = "Searched for files belonging to " + targetUserName + "'\n";
                outputString += (files.Count == 1) ? "One file found\n" : files.Count.ToString() + " files found\n";
                foreach (Windows.Storage.StorageFile file in files)
                {
                    outputString += file.Name + "\n";
                }
            }
        }
    }    
    ```

## <a name="stream-video-from-the-homegroup"></a>Transmitir vídeo del Grupo Hogar

Sigue estos pasos para transmitir contenido de vídeo del Grupo Hogar:

1.  **Incluye un elemento MediaElement en la aplicación.**

    [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) te permite reproducir contenido de audio y vídeo en tu aplicación. Para obtener información sobre la reproducción de audio y vídeo, consulta [Crear controles de transporte personalizados](https://msdn.microsoft.com/library/windows/apps/mt187271) y [Audio, vídeo y cámara](https://msdn.microsoft.com/library/windows/apps/mt203788).
    ```HTML
    <Grid x:Name="Output" HorizontalAlignment="Left" VerticalAlignment="Top" Grid.Row="1">
        <MediaElement x:Name="VideoBox" HorizontalAlignment="Left" VerticalAlignment="Top" Margin="0" Width="400" Height="300"/>
    </Grid>    
    ```

2.  **Abre un selector de archivos en el Grupo Hogar y aplica un filtro que incluya archivos de vídeo con los formatos que admite tu aplicación.**

    Este ejemplo incluye archivos .mp4 y .wmv en el selector para abrir archivos.
    ```cs
    Windows.Storage.Pickers.FileOpenPicker picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.HomeGroup;
    picker.FileTypeFilter.Clear();
    picker.FileTypeFilter.Add(".mp4");
    picker.FileTypeFilter.Add(".wmv");
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();   
    ```

3.  **Abre la selección del usuario archivo para acceso de lectura y establece la secuencia de archivos como el origen de la** [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)y, a continuación, reproducir el archivo.
    ```cs
    if (file != null)
    {
        var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.Read);
        VideoBox.SetSource(stream, file.ContentType);
        VideoBox.Stop();
        VideoBox.Play();
    }
    else
    {
        // No file selected. Handle the error here.
    }    
    ```
