---
description: Este artículo explica cómo admitir el contrato para contenido compartido en una aplicación para la Plataforma universal de Windows (UWP).
title: Compartir datos
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4ed74149552e6582bf133550d4db1a45625e8c39
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364048"
---
# <a name="share-data"></a>Compartir datos


Este artículo explica cómo admitir el contrato para contenido compartido en una aplicación para la Plataforma universal de Windows (UWP). El contrato para contenido compartido es una manera sencilla de compartir rápidamente los datos, como texto, vínculos, fotos y vídeos, entre aplicaciones. Por ejemplo, es posible que un usuario quiera compartir una página web con sus amigos mediante una aplicación de red social, o guardar un vínculo en una aplicación de notas para consultarlo más adelante.

> [!NOTE]
> Los ejemplos de código de este artículo están escritos para aplicaciones UWP. Las aplicaciones de escritorio de WPF, Windows Forms y C++/Win32 deben usar la interfaz [IDataTransferManagerInterop](/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) para obtener el objeto [DataTransferManager](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager) para una ventana específica. Para obtener más información, vea el ejemplo [ShareSource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource) .

## <a name="set-up-an-event-handler"></a>Configuración de un controlador de eventos

Agrega un controlador de eventos [**DataRequested**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) para que se llame siempre que un usuario invoque el recurso compartido. Esto puede producirse si el usuario pulsa un control de la aplicación (por ejemplo, un botón o un comando de la barra de la aplicación) o automáticamente en un escenario específico (si el usuario finaliza un nivel y obtiene una puntuación alta, por ejemplo).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetPrepareToShare":::

Cuando se produce un evento [**DataRequested**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested), tu aplicación recibe un objeto [**DataRequest**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest). Este objeto contiene un [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) que puedes usar para proporcionar el contenido que el usuario quiere compartir. Debes proporcionar un título y datos para compartir. La descripción es opcional, pero se recomienda.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetCreateRequest":::

## <a name="choose-data"></a>Elige los datos

Puedes compartir varios tipos de datos, incluidos:

-   Texto sin formato
-   Identificadores uniformes de recursos (URI)
-   HTML
-   Texto con formato
-   Mapas de bits
-   Archivos
-   Personalizar datos definidos por el desarrollador

El objeto [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) puede contener uno o más de estos formatos, en cualquier combinación. En el ejemplo a continuación se muestra el uso compartido de un texto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetSetContent":::

## <a name="set-properties"></a>Establecimiento de las propiedades

Cuando empaquetas datos para compartirlos, puedes suministrar diversas propiedades que proporcionan información adicional acerca del contenido que se va a compartir. Estas propiedades ayudan a las aplicaciones de destino a mejorar la experiencia del usuario. Por ejemplo, una descripción sirve de ayuda cuando el usuario está compartiendo contenido con más de una aplicación. Agregar una miniatura al compartir una imagen o un vínculo a una página web proporciona al usuario una referencia visual. Para obtener más información, consulta [**DataPackagePropertySet**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet).

Todas las propiedades excepto el título son opcionales. La propiedad de título es obligatoria y debe establecerse.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetSetProperties":::

## <a name="launch-the-share-ui"></a>Iniciar la interfaz de usuario de uso compartido

El sistema proporciona una interfaz de usuario para uso compartido. Para iniciarla, llama al método [**ShowShareUI**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetShowUI":::

## <a name="handle-errors"></a>Control de errores

En la mayoría de los casos, compartir contenido es un proceso sencillo. Sin embargo, siempre existe la posibilidad de que ocurra algo inesperado. Por ejemplo, es posible que la aplicación haya solicitado al usuario que seleccione contenido para compartir, pero el usuario no ha seleccionado nada. Para controlar estas situaciones, usa el método [**FailWithDisplayText**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest#Windows_ApplicationModel_DataTransfer_DataRequest_FailWithDisplayText_System_String_), que muestra un mensaje al usuario si se produce un error.

## <a name="delay-share-with-delegates"></a>Retrasar el uso compartido con funciones delegadas

A veces, puede que no tenga sentido preparar de inmediato los datos que el usuario quiere compartir. Por ejemplo, si la aplicación admite el envío de un archivo de imagen grande en varios formatos posibles, no resulta eficaz crear esas todas esas imágenes antes de que el usuario haga su selección.

Para solucionar este problema, un [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) puede contener una función delegada, es decir, una función a la que se llama cuando la aplicación receptora solicita datos. Recomendamos el uso de un delegado cuando los datos que un usuario quiera compartir hagan un uso intensivo de los recursos.

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
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelWidth * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## <a name="see-also"></a>Consulte también 

* [Comunicación entre aplicaciones](index.md)
* [Recibir datos](receive-data.md)
* [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackagePropertySet](/uwp/api/windows.applicationmodel.datatransfer.datapackagepropertyset)
* [Solicitud de consulta](/uwp/api/windows.applicationmodel.datatransfer.datarequest)
* [DataRequested](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)
* [FailWithDisplayText](/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
 
