---
ms.assetid: F87DBE2F-77DB-4573-8172-29E11ABEFD34
title: Abrir archivos y carpetas con un selector
description: Para obtener acceso a archivos y carpetas, permite al usuario interactuar con un selector. Puedes usar las clases FileOpenPicker y FileSavePicker para obtener acceso a archivos, y la clase FolderPicker para obtener acceso a una carpeta.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b3143247eab0bd8ed4266b78e34ec54dafa4dd7e
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259568"
---
# <a name="open-files-and-folders-with-a-picker"></a>Abrir archivos y carpetas con un selector

**API importantes**

-   [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)
-   [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker)
-   [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)

Para obtener acceso a archivos y carpetas, permite al usuario interactuar con un selector. Puedes usar las clases [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) y [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) para acceder a archivos y la clase [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker) para acceder a una carpeta.

> [!NOTE]
> Para obtener una muestra completa, consulta [File picker sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker) (Muestra de selector de archivos).

## <a name="prerequisites"></a>Requisitos previos


-   **Comprender la programación asincrónica de las aplicaciones para Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Permisos de acceso a la ubicación**

    Consulta [Permisos de acceso de archivos](file-access-permissions.md).

## <a name="file-picker-ui"></a>Interfaz de usuario del selector de archivos


Un selector de archivos muestra información para guiar a los usuarios y proporcionar una experiencia coherente cuando estos abren o guardan archivos.

Esta información incluye:

-   La ubicación actual
-   El elemento o los elementos que el usuario ha seleccionado
-   Un árbol de ubicaciones que el usuario puede examinar. Estas ubicaciones son ubicaciones del sistema de archivos (como las carpetas Música o Descargas), así como aplicaciones que implementan el contrato del selector de archivos (como Cámara, Fotos y Microsoft OneDrive).

Es probable que una aplicación de correo muestre un selector de archivos para que el usuario pueda seleccionar archivos adjuntos.

![Un selector de archivos con dos archivos seleccionados para abrirse.](images/picker-multifile-600px.png)

## <a name="how-pickers-work"></a>Funcionamiento de los selectores


Con un selector, la aplicación puede acceder a archivos y carpetas, examinarlos y guardarlos en cualquier parte del sistema del usuario. La aplicación recibe estas selecciones como objetos [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) y [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder), sobre los que luego puedes actuar.

El selector usa una interfaz única y unificada para que el usuario pueda seleccionar archivos y carpetas del sistema de archivos o de otras aplicaciones. Los archivos seleccionados desde otras aplicaciones son como archivos del sistema de archivos: se devuelven como objetos [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile). En general, la aplicación puede actuar sobre ellos de la misma manera que otros objetos similares. Otras aplicaciones hacen que los archivos estén disponibles mediante su participación en los contratos del selector de archivos. Si quieres que tu aplicación proporcione archivos, una ubicación para guardar o actualizaciones de archivos a otras aplicaciones, consulta [Integrar con contratos del selector de archivos](https://docs.microsoft.com/previous-versions/windows/apps/hh465192(v=win.10)).

Por ejemplo, podrías llamar al selector de archivos en la aplicación para que el usuario pueda abrir un archivo. Esto hace que la aplicación llame a la aplicación. El selector de archivos interactúa con el sistema o con otras aplicaciones para permitir que el usuario navegue y seleccione el archivo. Cuando el usuario elige un archivo, el selector de archivos devuelve ese archivo a la aplicación. A continuación se muestra este proceso para el caso en el que el usuario elige un archivo de una aplicación proveedora, como OneDrive.

![Diagrama que muestra el proceso de una aplicación que obtiene un archivo para abrirlo desde otra aplicación con el selector de archivos como interfaz entre ambas aplicaciones.](images/app-to-app-diagram-600px.png)

## <a name="pick-a-single-file-complete-code-listing"></a>Seleccionar un solo archivo: lista completa de códigos


```cs
var picker = new Windows.Storage.Pickers.FileOpenPicker();
picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
picker.FileTypeFilter.Add(".jpg");
picker.FileTypeFilter.Add(".jpeg");
picker.FileTypeFilter.Add(".png");

Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();
if (file != null)
{
    // Application now has read/write access to the picked file
    this.textBlock.Text = "Picked photo: " + file.Name;
}
else
{
    this.textBlock.Text = "Operation cancelled.";
}
```

## <a name="pick-a-single-file-step-by-step"></a>Seleccionar un solo archivo: paso a paso


El uso del selector de archivos requiere la creación y la personalización de un objeto de selector de archivos y, luego, la visualización del selector de archivos para que el usuario pueda seleccionar uno o varios elementos.

1.  **Crear y personalizar un FileOpenPicker**

    ```cs
    var picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
    picker.FileTypeFilter.Add(".jpg");
    picker.FileTypeFilter.Add(".jpeg");
    picker.FileTypeFilter.Add(".png");
    ```
    Establece las propiedades en el objeto de selector de archivos que sean relevantes para los usuarios y la aplicación.

    En este ejemplo, se crea en una práctica ubicación una representación visual enriquecida con imágenes, que el usuario puede seleccionar estableciendo tres propiedades: [**ViewMode**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.viewmode), [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) y [**FileTypeFilter**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter).

    -   Al establecer la propiedad [**ViewMode**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.viewmode) en el valor de enumeración **Thumbnail** de [**PickerViewMode**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerViewMode), se crea una representación visual enriquecida con miniaturas de imágenes para representar archivos en el selector de archivos. Haz esto para seleccionar archivos visuales, como imágenes o vídeos. De lo contrario, usa [**PickerViewMode.List**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerViewMode). Una aplicación hipotética de correo electrónico con las características **Adjuntar imagen o vídeo** y **Adjuntar documento** establecería el **ViewMode** adecuado para la característica antes de mostrar el selector de archivos.

    -   Establecer [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) en Imágenes mediante [**PickerLocationId.PicturesLibrary**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerLocationId) permite al usuario iniciar en una ubicación donde probablemente pueda encontrar imágenes. Establece **SuggestedStartLocation** en una ubicación adecuada para el tipo de archivo que se está seleccionando, por ejemplo, música, imágenes, vídeos o documentos. Desde la ubicación de inicio, el usuario puede dirigirse a otras ubicaciones.

    -   El uso de [**FileTypeFilter**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) para especificar los tipos de archivo mantiene al usuario concentrado en la selección de los archivos relevantes. Para reemplazar los tipos de archivo anteriores de la propiedad **FileTypeFilter** con entradas nuevas, usa [**ReplaceAll**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileextensionvector.replaceall) en lugar de [**Add**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileextensionvector.append).

2.  **Mostrar FileOpenPicker**

    - **Para seleccionar un único archivo**

        ```cs
        Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();
        if (file != null)
        {
            // Application now has read/write access to the picked file
            this.textBlock.Text = "Picked photo: " + file.Name;
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
        ```

    - **Para seleccionar varios archivos**  

        ```cs
        var files = await picker.PickMultipleFilesAsync();
        if (files.Count > 0)
        {
            StringBuilder output = new StringBuilder("Picked files:\n");
    
            // Application now has read/write access to the picked file(s)
            foreach (Windows.Storage.StorageFile file in files)
            {
                output.Append(file.Name + "\n");
            }
            this.textBlock.Text = output.ToString();
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
        ```

## <a name="pick-a-folder-complete-code-listing"></a>Seleccionar una carpeta: lista completa de códigos


```cs
var folderPicker = new Windows.Storage.Pickers.FolderPicker();
folderPicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.Desktop;
folderPicker.FileTypeFilter.Add("*");

Windows.Storage.StorageFolder folder = await folderPicker.PickSingleFolderAsync();
if (folder != null)
{
    // Application now has read/write access to all contents in the picked folder
    // (including other sub-folder contents)
    Windows.Storage.AccessCache.StorageApplicationPermissions.
    FutureAccessList.AddOrReplace("PickedFolderToken", folder);
    this.textBlock.Text = "Picked folder: " + folder.Name;
}
else
{
    this.textBlock.Text = "Operation cancelled.";
}
```

> [!TIP]
> Cuando la aplicación obtenga acceso a un archivo o a una carpeta a través de un selector, agrégalo a la propiedad [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) o a la propiedad [**MostRecentlyUsedList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist) de la aplicación para tenerlo controlado en el futuro. Obtendrás más información sobre cómo usar estas listas en el tema [Cómo hacer un seguimiento de los archivos y carpetas usados recientemente](how-to-track-recently-used-files-and-folders.md).