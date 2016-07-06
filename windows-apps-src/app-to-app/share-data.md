---
description: "Este artículo explica cómo admitir el contrato para contenido compartido en una aplicación para la Plataforma universal de Windows (UWP)."
title: Compartir datos
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
author: awkoren
ms.sourcegitcommit: 9a8fd6d34c4b89dae1ec4be2db69498b5d458b5a
ms.openlocfilehash: a91f0eb8b62a860809f8ffb63278be1eff31a2f3

---

# Compartir datos

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artículo explica cómo admitir el contrato para contenido compartido en una aplicación para la Plataforma universal de Windows (UWP). El contrato para contenido compartido es una manera sencilla de compartir rápidamente los datos, como texto, vínculos, fotos y vídeos, entre aplicaciones. Por ejemplo, es posible que un usuario quiera compartir una página web con sus amigos mediante una aplicación de red social o guardar un vínculo en una aplicación de bloc de notas para consultarlo más adelante.

## Configurar un controlador de eventos

Agrega un controlador de eventos [**DataRequested**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.DataRequested) para que se llame siempre que un usuario invoque el recurso compartido. Esto puede producirse si el usuario pulsa un control de la aplicación (por ejemplo, un botón o un comando de la barra de la aplicación) o automáticamente en un escenario específico (si el usuario finaliza un nivel y obtiene una puntuación alta, por ejemplo).

[!code-cs[Principal](./code/share_data/cs/MainPage.xaml.cs#SnippetPrepareToShare)]

Cuando se produce un evento [**DataRequested**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.DataRequested), tu aplicación recibe un objeto [**DataRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataRequest). Este objeto contiene un [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) que puedes usar para proporcionar el contenido que el usuario quiere compartir. Debes proporcionar un título y datos para compartir. La descripción es opcional, pero se recomienda.

[!code-cs[Principal](./code/share_data/cs/MainPage.xaml.cs#SnippetCreateRequest)]

## Elige los datos

Puedes compartir varios tipos de datos, incluidos:

-   Texto sin formato
-   Identificadores uniformes de recursos (URI)
-   HTML
-   Texto con formato
-   Bitmaps
-   Texto sin formato
-   Archivos
-   Personalizar datos definidos por el desarrollador

El objeto [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) puede contener uno o más de estos formatos, en cualquier combinación. En el ejemplo a continuación se muestra el uso compartido de un texto.

[!code-cs[Principal](./code/share_data/cs/MainPage.xaml.cs#SnippetSetContent)]

## Establecer las propiedades

Cuando empaquetas datos para compartirlos, puedes suministrar diversas propiedades que proporcionan información adicional acerca del contenido que se va a compartir. Estas propiedades ayudan a las aplicaciones de destino a mejorar la experiencia del usuario. Por ejemplo, una descripción sirve de ayuda cuando el usuario está compartiendo contenido con más de una aplicación. Agregar una miniatura al compartir una imagen o un vínculo a una página web proporciona al usuario una referencia visual. Para obtener más información, consulta [**DataPackagePropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet).

Todas las propiedades excepto el título son opcionales. La propiedad de título es obligatoria y debe establecerse.

[!code-cs[Principal](./code/share_data/cs/MainPage.xaml.cs#SnippetSetProperties)]

## Iniciar la interfaz de usuario de uso compartido

El sistema proporciona una interfaz de usuario para uso compartido. Para iniciarla, llama al método [**ShowShareUI**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataTransferManager.ShowShareUI).

[!code-cs[Principal](./code/share_data/cs/MainPage.xaml.cs#SnippetShowUI)]

## Controlar errores

En la mayoría de los casos, compartir contenido es un proceso sencillo. Sin embargo, siempre existe la posibilidad de que ocurra algo inesperado. Por ejemplo, es posible que la aplicación solicite al usuario que seleccione contenido para compartir, pero el usuario no seleccionó nada. Para controlar estas situaciones, usa el método [**FailWithDisplayText**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataRequest.FailWithDisplayText(System.String)), que muestra un mensaje al usuario si se produce un error.

## Retrasar el uso compartido con funciones delegadas

A veces, puede que no tenga sentido preparar de inmediato los datos que el usuario quiere compartir. Por ejemplo, si la aplicación admite el envío de un archivo de imagen grande en varios formatos posibles, no resulta eficaz crear esas todas esas imágenes antes de que el usuario haga su selección.

Para solucionar este problema, un [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) puede contener una función delegada, es decir, una función a la que se llama cuando la aplicación receptora solicita datos. Recomendamos el uso de un delegado cuando los datos que un usuario quiera compartir hagan un uso intensivo de los recursos.

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## Consulta también 

* [Recibir datos](receive-data.md)
* [DataPackage](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx)
* [DataPackagePropertySet](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx)
* [DataRequested](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx)
* [ShowShareUi](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx)
 




<!--HONumber=Jun16_HO4-->


