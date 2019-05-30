---
ms.assetid: 3FD2AA71-EF67-47B2-9332-3FFA5D3703EA
description: En este artículo se explica cómo cargar y guardar archivos de imagen mediante BitmapDecoder y BitmapEncoder, y cómo usar el objeto SoftwareBitmap para representar imágenes de mapa de bits.
title: Crear, editar y guardar imágenes de mapa de bits
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7c61a35f0ad35cf85afcba564eb676aa171b0243
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360842"
---
# <a name="create-edit-and-save-bitmap-images"></a>Crear, editar y guardar imágenes de mapa de bits



En este artículo se explica cómo cargar y guardar archivos de imagen mediante [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) y [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder), y cómo usar el objeto [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) para representar imágenes de mapa de bits.

La clase **SoftwareBitmap** es una API versátil que se puede crear desde varios orígenes, entre los que se incluyen archivos de imagen, objetos [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap), superficies de Direct3D y código. **SoftwareBitmap** permite convertir fácilmente entre los modos alfa y formatos de píxel diferentes, y también permite el acceso de bajo nivel a datos de píxel. Además, **SoftwareBitmap** es una interfaz común que utilizan varias características de Windows, entre las que se incluyen:

-   [**CapturedFrame** ](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.CapturedFrame) le permite obtener los fotogramas capturados por la cámara como una **SoftwareBitmap**.

-   [**VideoFrame** ](https://docs.microsoft.com/uwp/api/Windows.Media.VideoFrame) le permite obtener un **SoftwareBitmap** representación de un **VideoFrame**.

-   [**FaceDetector** ](https://docs.microsoft.com/uwp/api/Windows.Media.FaceAnalysis.FaceDetector) le permite detectar caras en una **SoftwareBitmap**.

El código de ejemplo de este artículo usa las API de los siguientes espacios de nombres.

[!code-cs[Namespaces](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetNamespaces)]

## <a name="create-a-softwarebitmap-from-an-image-file-with-bitmapdecoder"></a>Crear un objeto SoftwareBitmap desde un archivo de imagen con BitmapDecoder

Para crear un elemento [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) desde un archivo, obtén una instancia de [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) que contenga los datos de la imagen. En este ejemplo se usa un elemento [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) para permitir al usuario seleccionar un archivo de imagen.

[!code-cs[PickInputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickInputFile)]

Llama al método [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) del objeto **StorageFile** para obtener un flujo de acceso aleatorio que contenga los datos de la imagen. Llama al método estático [**BitmapDecoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.createasync) para obtener una instancia de la clase [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) para el flujo especificado. Llama a [**GetSoftwareBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapdecoder.getsoftwarebitmapasync) para obtener un objeto [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) que contenga la imagen.

[!code-cs[CreateSoftwareBitmapFromFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromFile)]

## <a name="save-a-softwarebitmap-to-a-file-with-bitmapencoder"></a>Guardar un elemento SoftwareBitmap en un archivo con BitmapEncoder

Para guardar un elemento **SoftwareBitmap** en un archivo, obtén una instancia de **StorageFile** en la que se guardará la imagen. En este ejemplo se usa un elemento [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) para permitir al usuario seleccionar un archivo de salida.

[!code-cs[PickOutputFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetPickOutputFile)]

Llama al método [**OpenAsync**](https://docs.microsoft.com/uwp/api/windows.storage.istoragefile.openasync) del objeto **StorageFile** para obtener un flujo de acceso aleatorio en el que se escribirá la imagen. Llama al método estático [**BitmapEncoder.CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync) para obtener una instancia de la clase [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) para el flujo especificado. El primer parámetro para **CreateAsync** es un GUID que representa el códec que se debe usar para codificar la imagen. La clase **BitmapEncoder** expone una propiedad que contiene el identificador de cada códec compatible con el codificador, como por ejemplo [**JpegEncoderId**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.jpegencoderid).

Usa el método [**SetSoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.setsoftwarebitmap) para establecer la imagen que se va a codificar. Puedes establecer los valores de la propiedad [**BitmapTransform**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTransform) para aplicar transformaciones básicas a la imagen mientras se codifica. La propiedad [**IsThumbnailGenerated**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.isthumbnailgenerated) determina si el codificador genera una miniatura. Ten en cuenta que no todos los formatos de archivo admiten las miniaturas, por lo que si usas esta característica, debes capturar el error de operación no compatible que se produce si las miniaturas no se admiten.

Llama a [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) para hacer que el codificador escriba los datos de la imagen en el archivo especificado.

[!code-cs[SaveSoftwareBitmapToFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSaveSoftwareBitmapToFile)]

Puedes especificar opciones de codificación adicionales al crear el elemento **BitmapEncoder** mediante la creación de un nuevo objeto [**BitmapPropertySet**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) y rellenándolo con uno o varios objetos [**BitmapTypedValue**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) que representen la configuración del codificador. Para obtener una lista de opciones de codificador admitidas, consulta el [tema de referencia de opciones de BitmapEncoder](bitmapencoder-options-reference.md).

[!code-cs[UseEncodingOptions](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetUseEncodingOptions)]

## <a name="use-softwarebitmap-with-a-xaml-image-control"></a>Usar SoftwareBitmap con un control de imagen XAML

Para mostrar una imagen dentro de una página XAML con el control [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image), define primero un control **Image** en la página XAML.

[!code-xml[ImageControl](./code/ImagingWin10/cs/MainPage.xaml#SnippetImageControl)]

Actualmente, el control **Image** solo admite imágenes que usan la codificación BGRA8 y el canal alfa premultiplicado o ningún canal alfa. Antes de intentar mostrar una imagen, asegúrate de que tiene el formato correcto y, si no es así, usa el método estático [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.windows) de **SoftwareBitmap** para convertir la imagen al formato compatible.

Crea un nuevo objeto [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource). Establece el contenido del objeto de origen mediante una llamada a [**SetBitmapAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) y pásale un elemento **SoftwareBitmap**. Después puedes establecer la propiedad [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) del control **Image** al objeto **SoftwareBitmapSource** recién creado.

[!code-cs[SoftwareBitmapToWriteableBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSoftwareBitmapToWriteableBitmap)]

También puedes usar **SoftwareBitmapSource** para establecer un elemento **SoftwareBitmap** como la propiedad [**ImageSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource) de una clase [**ImageBrush**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush).

## <a name="create-a-softwarebitmap-from-a-writeablebitmap"></a>Crear un objeto SoftwareBitmap desde un objeto WriteableBitmap

Puedes crear un objeto **SoftwareBitmap** desde un objeto **WriteableBitmap** existente mediante una llamada a [**SoftwareBitmap.CreateCopyFromBuffer**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfrombuffer) y facilitando la propiedad **PixelBuffer** del objeto **WriteableBitmap** para establecer los datos de píxel. El segundo argumento permite solicitar un formato de píxel para el objeto **WriteableBitmap** recién creado. Puedes usar las propiedades [**PixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelwidth) y [**PixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.pixelheight) del objeto **WriteableBitmap** para especificar las dimensiones de la nueva imagen.

[!code-cs[WriteableBitmapToSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteableBitmapToSoftwareBitmap)]

## <a name="create-or-edit-a-softwarebitmap-programmatically"></a>Crear o editar un objeto SoftwareBitmap mediante programación

Hasta ahora, este tema ha explicado cómo se trabaja con archivos de imagen. También puedes crear un nuevo objeto **SoftwareBitmap** mediante programación en código y usar la misma técnica para acceder y modificar los datos de píxel de **SoftwareBitmap**.

**SoftwareBitmap** usa la interoperabilidad COM para exponer el búfer sin procesar que contiene los datos de píxel.

Para usar la interoperabilidad COM, debes incluir una referencia al espacio de nombres **System.Runtime.InteropServices** del proyecto.

[!code-cs[InteropNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetInteropNamespace)]

Inicializa la interfaz COM [**IMemoryBufferByteAccess**](https://docs.microsoft.com/previous-versions//mt297505(v=vs.85)) agregando el código siguiente dentro del espacio de nombres.

[!code-cs[COMImport](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCOMImport)]

Crea un nuevo objeto **SoftwareBitmap** con el formato de píxel y el tamaño que deseas. O bien, usa un objeto **SoftwareBitmap** existente del que quieras editar los datos de píxel. Llama a [**SoftwareBitmap.LockBuffer**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer) para obtener una instancia de la clase [**BitmapBuffer**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapBuffer) que represente el búfer de datos de píxel. Convierte el elemento **BitmapBuffer** a la interfaz COM **IMemoryBufferByteAccess** y luego llama a [**IMemoryBufferByteAccess.GetBuffer**](https://docs.microsoft.com/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) para rellenar una matriz de bytes con los datos. Usa el método [**BitmapBuffer.GetPlaneDescription**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.getplanedescription) para obtener un objeto [**BitmapPlaneDescription**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPlaneDescription) que te ayudará a calcular el desplazamiento del búfer de cada píxel.

[!code-cs[CreateNewSoftwareBitmap](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateNewSoftwareBitmap)]

Como este método tiene acceso el búfer sin procesar subyacente a los tipos de Windows Runtime, se debe declarar con la palabra clave **unsafe**. También debes configurar el proyecto en Microsoft Visual Studio para permitir la compilación del código no seguro; para ello, abre la página **Propiedades** del proyecto, haz clic en la página de propiedades de **Compilación** y selecciona la casilla **Permitir código no seguro**.

## <a name="create-a-softwarebitmap-from-a-direct3d-surface"></a>Crear un objeto SoftwareBitmap desde una superficie de Direct3D

Para crear un objeto **SoftwareBitmap** desde una superficie de Direct3D, debes incluir el espacio de nombres [**Windows.Graphics.DirectX.Direct3D11**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11) en el proyecto.

[!code-cs[Direct3DNamespace](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetDirect3DNamespace)]

Llama a [**CreateCopyFromSurfaceAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.createcopyfromsurfaceasync) para crear un nuevo objeto **SoftwareBitmap** desde la superficie. Como su nombre indica, el nuevo objeto **SoftwareBitmap** tiene una copia independiente de los datos de imagen. Las modificaciones al objeto **SoftwareBitmap** no tendrán ningún efecto en la superficie de Direct3D.

[!code-cs[CreateSoftwareBitmapFromSurface](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetCreateSoftwareBitmapFromSurface)]

## <a name="convert-a-softwarebitmap-to-a-different-pixel-format"></a>Convertir un objeto SoftwareBitmap a un formato de píxel diferente

La clase **SoftwareBitmap** proporciona el método estático [**Convert**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.windows) que permite crear fácilmente un nuevo objeto **SoftwareBitmap** que use el modo alfa y el formato de píxel que se especifique desde un objeto **SoftwareBitmap** existente. Ten en cuenta que el mapa de bits recién creado tiene una copia de los datos de imagen. Las modificaciones en el nuevo mapa de bits no afectarán al mapa de bits de origen.

[!code-cs[Convert](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetConvert)]

## <a name="transcode-an-image-file"></a>Transcodificar un archivo de imagen

Puedes transcodificar un archivo de imagen directamente desde un elemento [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) a un elemento [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapEncoder). Crear un elemento [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) desde el archivo que se va a transcodificar. Crea un nuevo objeto **BitmapDecoder** desde el flujo de entrada. Crea un nuevo elemento [**InMemoryRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) para el codificador en el que se va a escribir y llama a [**BitmapEncoder.CreateForTranscodingAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync) pasándole el flujo en memoria y el objeto descodificador. Las opciones de codificación no son compatibles al realizar la operación de transcodificación; en su lugar deberías usar [**CreateAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.createasync). Las propiedades del archivo de imagen de entrada que no se establecieron específicamente en el codificador se escribirán sin cambios en el archivo de salida. Llama a [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) para que el codificador codifique en el flujo en memoria. Por último, busca la secuencia de archivos y la secuencia en memoria al principio y llama a [**CopyAsync**](https://docs.microsoft.com/uwp/api/windows.storage.streams.randomaccessstream.copyasync) para escribir la secuencia en memoria en la secuencia de archivos.

[!code-cs[TranscodeImageFile](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetTranscodeImageFile)]

## <a name="related-topics"></a>Temas relacionados

* [Referencia de las opciones de BitmapEncoder](bitmapencoder-options-reference.md)
* [Metadatos de imagen](image-metadata.md)
 

 




