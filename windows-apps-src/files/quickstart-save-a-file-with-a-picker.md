---
ms.assetid: 8BDDE64A-77D2-4F9D-A1A0-E4C634BCD890
title: Guardar un archivo con un selector
description: Usa FileSavePicker para permitir a los usuarios especificar el nombre y la ubicación donde desean que tu aplicación guarde un archivo.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a7e278df29a531e5bf1d0d92946cd0199f85515d
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8919201"
---
# <a name="save-a-file-with-a-picker"></a>Guardar un archivo con un selector

**API importantes**

-   [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)
-   [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)

Usa [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir a los usuarios especificar el nombre y la ubicación donde desean que tu aplicación guarde un archivo.

> [!NOTE]
> Consulta también la [muestra del selector de archivos](http://go.microsoft.com/fwlink/p/?linkid=619994).

 

## <a name="prerequisites"></a>Requisitos previos


-   **Comprender la programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Permisos de acceso a la ubicación**

    Consulta [Permisos de acceso de archivos](file-access-permissions.md).

## <a name="filesavepicker-step-by-step"></a>FileSavePicker: paso a paso

Usa [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para que los usuarios puedan especificar el nombre, el tipo de archivo y la ubicación de un archivo para guardarlo. Crea, personaliza y muestra un objeto del selector de archivos y luego guarda los datos mediante el objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) devuelto, que representa el archivo seleccionado.

1.  **Crear y personalizar FileSavePicker**

```cs
var savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.SuggestedStartLocation =
    Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
// Dropdown of file types the user can save the file as
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
// Default file name if the user does not type one in or select a file to replace
savePicker.SuggestedFileName = "New Document";
```

Establece las propiedades en el objeto del selector de archivos que sean relevantes para los usuarios y la aplicación.

En este ejemplo se establecen tres propiedades: [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207880), [**FileTypeChoices**](https://msdn.microsoft.com/library/windows/apps/br207875) y [**SuggestedFileName**](https://msdn.microsoft.com/library/windows/apps/br207878).

> [!NOTE]
>Los objetos de la clase [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) muestran el selector de archivos mediante la enumeración [**PickerViewMode.List**](https://msdn.microsoft.com/library/windows/apps/br207891).
     
- Dado que nuestro usuario está guardando un documento o archivo de texto, la muestra establece [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207880) en la carpeta local de la aplicación mediante [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621). Establece [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) en una ubicación adecuada para el tipo de archivo que se está guardando, por ejemplo, música, imágenes, vídeos o documentos. Desde la ubicación de inicio, el usuario puede dirigirse a otras ubicaciones.

- Como queremos asegurarnos de que nuestra aplicación puede abrir el archivo después de que este se haya guardado, usaremos [**FileTypeChoices**](https://msdn.microsoft.com/library/windows/apps/br207875) para especificar los tipos de archivo que admite la muestra (archivos de texto y documentos de MicrosoftWord). Procura que todos los tipos de archivo que indiques sean compatibles con tu aplicación. Los usuarios podrán guardar sus archivos como cualquier otro tipo de archivo que especifiques. También pueden cambiar el tipo de archivo seleccionando otro de los tipos de archivo especificados. La primera opción de tipo de archivo de la lista se seleccionará de manera predeterminada: para controlar esto, establece la propiedad [**DefaultFileExtension**](https://msdn.microsoft.com/library/windows/apps/br207873).

> [!NOTE]
> El selector de archivos usa también el tipo de archivo seleccionado actualmente para filtrar los archivos que se van a mostrar, de modo que el usuario solo podrá ver los tipos de archivo que coincidan con los tipos de archivo seleccionados.

- Para ahorrar trabajo al usuario, el ejemplo establece un [**SuggestedFileName**](https://msdn.microsoft.com/library/windows/apps/br207878). Haz que el nombre de archivo sugerido sea relevante para el archivo que se va a guardar. Por ejemplo, como en Word, puedes sugerir el nombre de archivo existente si solo hay uno, o la primera línea de un documento, si se va a guardar un archivo que aún no tiene nombre.

2.  **Mostrar FileSavePicker y guardar en el archivo seleccionado**

    Muestra el selector de archivos llamando a [**PickSaveFileAsync**](https://msdn.microsoft.com/library/windows/apps/br207876). Cuando el usuario especifique el nombre, el tipo de archivo, la ubicación y confirme que desea guardar el archivo, **PickSaveFileAsync** devuelve un objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa el archivo guardado. Puedes capturar y procesar este archivo ahora que has leído y escrito el acceso a dicho archivo.

    ```cs
    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
        if (file != null)
        {
            // Prevent updates to the remote version of the file until
            // we finish making changes and call CompleteUpdatesAsync.
            Windows.Storage.CachedFileManager.DeferUpdates(file);
            // write to file
            await Windows.Storage.FileIO.WriteTextAsync(file, file.Name);
            // Let Windows know that we're finished changing the file so
            // the other app can update the remote version of the file.
            // Completing updates may require Windows to ask for user input.
            Windows.Storage.Provider.FileUpdateStatus status =
                await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
            if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
            {
                this.textBlock.Text = "File " + file.Name + " was saved.";
            }
            else
            {
                this.textBlock.Text = "File " + file.Name + " couldn't be saved.";
            }
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
    ```

El ejemplo comprueba que el archivo es válido y escribe en él su propio nombre. Consulta también [Creación, escritura y lectura de archivos](quickstart-reading-and-writing-files.md).

**Sugerencia**siempre debe comprobar el archivo guardado para asegurarte de que sea válido antes de realizar otros procesos. Luego, puedes guardar contenido en el archivo según corresponda para la aplicación y proporcionar el comportamiento adecuado si el archivo no es válido.
