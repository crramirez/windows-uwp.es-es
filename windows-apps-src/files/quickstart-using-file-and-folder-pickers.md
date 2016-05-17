---
author: TylerMSFT
ms.assetid: F87DBE2F-77DB-4573-8172-29E11ABEFD34
title: Abrir archivos y carpetas con un selector
description: Para obtener acceso a archivos y carpetas, permite al usuario interactuar con un selector. Puedes usar las clases FileOpenPicker y FileSavePicker para obtener acceso a archivos, y la clase FolderPicker para obtener acceso a una carpeta.
---

# Abrir archivos y carpetas con un selector


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)
-   [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)
-   [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)

Para obtener acceso a archivos y carpetas, permite al usuario interactuar con un selector. Puedes usar las clases [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) y [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) para obtener acceso a archivos, y la clase [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881) para obtener acceso a una carpeta.

**Nota**  Consulta también [File picker sample (Muestra del selector de archivos)](http://go.microsoft.com/fwlink/p/?linkid=619994)

 

## Requisitos previos


-   **Comprender la programación asincrónica de las aplicaciones para la Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Permisos de acceso a la ubicación**

    Consulta [Permisos de acceso de archivos](file-access-permissions.md).

## Interfaz de usuario del selector de archivos


Un selector de archivos muestra información para guiar al usuario y proporcionar una experiencia coherente cuando los usuarios abran o guarden archivos.

Esta información incluye:

-   La ubicación actual
-   El elemento o los elementos que el usuario ha seleccionado
-   Un árbol de ubicaciones que el usuario puede examinar. Estas ubicaciones son ubicaciones del sistema de archivos (como las carpetas Música o Descargas), así como aplicaciones que implementan el contrato del selector de archivos (como Cámara, Fotos y Microsoft OneDrive).

Puede que una aplicación de correo electrónico muestre un selector de archivos para que el usuario pueda seleccionar los datos adjuntos.

![Un selector de archivos con dos archivos seleccionados para abrirse.](images/picker-multifile-600px.png)

## Funcionamiento de los selectores


A través de un selector, la aplicación puede obtener acceso a archivos y carpetas en cualquier parte del sistema del usuario. Mediante el selector, el usuario examina su sistema para seleccionar los archivos (o carpetas) que desee abrir o usar para guardar otros archivos. La aplicación recibe estas selecciones como objetos [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) y [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230), sobre los que luego puede operar.

El selector usa una interfaz única y unificada para que el usuario pueda seleccionar archivos y carpetas del sistema de archivos o de otras aplicaciones. Los archivos seleccionados desde otras aplicaciones son como archivos del sistema de archivos: se devuelven como objetos [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). En general, la aplicación puede actuar sobre ellos de la misma manera que otros objetos similares. Otras aplicaciones hacen que los archivos estén disponibles mediante su participación en los contratos del selector de archivos. Si quieres que tu aplicación proporcione archivos, una ubicación para guardarlos o actualizaciones de archivos para otras aplicaciones, consulta [Integrar con contratos del selector de archivos](https://msdn.microsoft.com/library/windows/apps/hh465192).

Por ejemplo, podrías llamar al selector de archivos en la aplicación para que el usuario pueda abrir un archivo. Esto hace que la aplicación llame a la aplicación. El selector de archivos interactúa con el sistema o con otras aplicaciones para permitir que el usuario navegue y seleccione el archivo. Cuando el usuario elige un archivo, el selector de archivos devuelve ese archivo a la aplicación. A continuación se muestra este proceso para el caso en el que el usuario selecciona un archivo de otra aplicación, como OneDrive. En este caso, OneDrive es la aplicación proveedora.

![Diagrama que muestra el proceso de una aplicación que obtiene un archivo para abrirlo desde otra aplicación con el selector de archivos como interfaz entre las dos aplicaciones.](images/app-to-app-diagram-600px.png)

## Seleccionar un solo archivo para abrirlo: lista completa de códigos


```CSharp
var picker = new Windows.Storage.Pickers.FileOpenPicker();
picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
picker.SuggestedStartLocation = 
    Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
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

Para obtener el código para seleccionar varios archivos, consulta [File picker sample (Muestra de selector de archivos)](http://go.microsoft.com/fwlink/p/?linkid=619994).

## Seleccionar un solo archivo para abrirlo: paso a paso


Para usar el selector de archivos, debes crear y personalizar un objeto de selector de archivos y, a continuación, visualizar el selector de archivos para que el usuario pueda seleccionar uno o varios elementos.

1.  **Crear y personalizar un FileOpenPicker**

```CSharp
var picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = 
        Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
    picker.FileTypeFilter.Add(".jpg");
    picker.FileTypeFilter.Add(".jpeg");
    picker.FileTypeFilter.Add(".png");
```

Establece las propiedades en el objeto del selector de archivos que sean relevantes para los usuarios y la aplicación. Para obtener directrices que te ayuden a decidir cómo personalizar el selector de archivos, consulta [Directrices y lista de comprobación para selectores de archivos](https://msdn.microsoft.com/library/windows/apps/hh465182)

En este ejemplo se crea en una práctica ubicación una representación visual enriquecida con imágenes, que el usuario puede seleccionar estableciendo tres propiedades: [**ViewMode**](https://msdn.microsoft.com/library/windows/apps/br207855), [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) y [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850).

-   Al establecer [**ViewMode**](https://msdn.microsoft.com/library/windows/apps/br207855) en el valor de enumeración **Thumbnail**
          [**PickerViewMode**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.storage.pickers.pickerviewmode.aspx#thumbnail), se crea una representación visual enriquecida con miniaturas de imágenes para representar archivos en el selector de archivos. Haz esto para seleccionar archivos visuales, como imágenes o vídeos. De lo contrario, usa [**PickerViewMode.List**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.storage.pickers.pickerviewmode.aspx#list). Una aplicación hipotética de correo electrónico con las características **Adjuntar imagen o vídeo** y **Adjuntar documento** establecería el **ViewMode** adecuado para la característica antes de mostrar el selector de archivos.

-   Establecer [**SuggestedStartLocation**](https://msdn.microsoft.com/library/windows/apps/br207854) en Imágenes mediante [**PickerLocationId.PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br207890) permite al usuario iniciar en una ubicación donde probablemente pueda encontrar imágenes. Establece **SuggestedStartLocation** en una ubicación adecuada para el tipo de archivo que se está seleccionando, por ejemplo, música, imágenes, vídeos o documentos. Desde la ubicación de inicio, el usuario puede dirigirse a otras ubicaciones.

-   El uso de [**FileTypeFilter**](https://msdn.microsoft.com/library/windows/apps/br207850) para especificar los tipos de archivo mantiene al usuario concentrado en la selección de los archivos relevantes. Para reemplazar los tipos de archivo anteriores en la propiedad **FileTypeFilter** con entradas nuevas, usa el método [**ReplaceAll**](https://msdn.microsoft.com/library/windows/apps/br207844) en lugar de [**Add**](https://msdn.microsoft.com/library/windows/apps/br207834).

2.  **Mostrar FileOpenPicker**

    -   **Para seleccionar un único archivo**

```CSharp
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

    -   **To pick multiple files**

```CSharp
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

## Seleccionar una carpeta: lista completa de códigos


```CSharp
var folderPicker = new Windows.Storage.Pickers.FolderPicker();
folderPicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.Desktop;

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

**Sugerencia** Cuando la aplicación obtiene acceso a un archivo o a una carpeta a través del selector de archivos, lo agrega a la propiedad [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) o a la propiedad [**MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458) para realizar el seguimiento de ese elemento. Obtendrás más información sobre cómo usar estas listas en el tema [Cómo hacer un seguimiento de los archivos y carpetas usados recientemente](how-to-track-recently-used-files-and-folders.md).

 

 

 






<!--HONumber=May16_HO2-->


