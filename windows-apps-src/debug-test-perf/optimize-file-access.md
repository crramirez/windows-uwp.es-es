---
author: jwmsft
ms.assetid: 40122343-1FE3-4160-BABE-6A2DD9AF1E8E
title: Optimizar el acceso a archivos
description: "Crea aplicaciones para la Plataforma universal de Windows (UWP) que puedan obtener acceso al sistema de archivos de forma eficaz, y así evitar problemas de rendimiento debido a la latencia de discos o a los ciclos de memoria o de la CPU."
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 00ad06179ed4a77cb3e5144df12d5490f79a5df2
ms.sourcegitcommit: ec18e10f750f3f59fbca2f6a41bf1892072c3692
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2017
---
# <a name="optimize-file-access"></a>Optimizar el acceso a archivos

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Crea aplicaciones para la Plataforma universal de Windows (UWP) que puedan obtener acceso al sistema de archivos de forma eficaz, y así evitar problemas de rendimiento debido a la latencia de discos o a los ciclos de memoria o de la CPU

Cuando quieras obtener acceso a una colección grande de archivos y a valores de propiedad diferentes de las propiedades típicas Name, FileType y Path, para hacerlo, crea [**QueryOptions**](https://msdn.microsoft.com/library/windows/apps/BR207995) y llama a [**SetPropertyPrefetch**](https://msdn.microsoft.com/library/windows/apps/hh973319). El método **SetPropertyPrefetch** puede mejorar considerablemente el rendimiento de las aplicaciones que muestran una colección de elementos obtenidos del sistema de archivos como, por ejemplo, una colección de imágenes. En los ejemplos siguientes se muestran algunas maneras de acceder a varios archivos.

En el primer ejemplo se usa [**Windows.Storage.StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/BR227273) para recuperar información sobre el nombre de un conjunto de archivos. Con este enfoque se logra un buen rendimiento, ya que en el ejemplo solo se tiene acceso a la propiedad Name.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> 
> for (int i = 0; i < files.Count; i++)
> {
>     // do something with the name of each file
>     string fileName = files[i].Name;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) =
>     Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> 
> For i As Integer = 0 To files.Count - 1
>     ' do something with the name of each file
>     Dim fileName As String = files(i).Name
> Next i
> ```

En el segundo ejemplo, se usa [**Windows.Storage.StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/BR227273) y, a continuación, se recuperan las propiedades de imagen de cada archivo. Con este enfoque se logra un rendimiento deficiente.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> for (int i = 0; i < files.Count; i++)
> {
>     ImageProperties imgProps = await files[i].Properties.GetImagePropertiesAsync();
> 
>     // do something with the date the image was taken
>     DateTimeOffset date = imgProps.DateTaken;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) = Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> For i As Integer = 0 To files.Count - 1
>     Dim imgProps As ImageProperties =
>         Await files(i).Properties.GetImagePropertiesAsync()
> 
>     ' do something with the date the image was taken
>     Dim dateTaken As DateTimeOffset = imgProps.DateTaken
> Next i
> ```

En el tercer ejemplo, se usa [**QueryOptions**](https://msdn.microsoft.com/library/windows/apps/BR207995) para obtener información sobre un conjunto de archivos. Con este enfoque se logra un rendimiento mucho mejor que en el ejemplo anterior.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> // Set QueryOptions to prefetch our specific properties
> var queryOptions = new Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, null);
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView, 100,
>         ThumbnailOptions.ReturnOnlyIfCached);
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties, 
>        new string[] {"System.Size"});
> 
> StorageFileQueryResult queryResults = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions);
> IReadOnlyList<StorageFile> files = await queryResults.GetFilesAsync();
> 
> foreach (var file in files)
> {
>     ImageProperties imageProperties = await file.Properties.GetImagePropertiesAsync();
> 
>     // Do something with the date the image was taken.
>     DateTimeOffset dateTaken = imageProperties.DateTaken;
> 
>     // Performance gains increase with the number of properties that are accessed.
>     IDictionary<String, object> propertyResults =
>         await file.Properties.RetrievePropertiesAsync(
>               new string[] {"System.Size" });
> 
>     // Get/Set extra properties here
>     var systemSize = propertyResults["System.Size"];
> }
> ```
> ```vb
> ' Set QueryOptions to prefetch our specific properties
> Dim queryOptions = New Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, Nothing)
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView,
>             100, Windows.Storage.FileProperties.ThumbnailOptions.ReturnOnlyIfCached)
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties,
>                                  New String() {"System.Size"})
> 
> Dim queryResults As StorageFileQueryResult = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions)
> Dim files As IReadOnlyList(Of StorageFile) = Await queryResults.GetFilesAsync()
> 
> 
> For Each file In files
>     Dim imageProperties As ImageProperties = Await file.Properties.GetImagePropertiesAsync()
> 
>     ' Do something with the date the image was taken.
>     Dim dateTaken As DateTimeOffset = imageProperties.DateTaken
> 
>     ' Performance gains increase with the number of properties that are accessed.
>     Dim propertyResults As IDictionary(Of String, Object) =
>         Await file.Properties.RetrievePropertiesAsync(New String() {"System.Size"})
> 
>     ' Get/Set extra properties here
>     Dim systemSize = propertyResults("System.Size")
> 
> Next file
> ```
Si realizas varias operaciones en objetos Windows.Storage, como `Windows.Storage.ApplicationData.Current.LocalFolder`, crea una variable local para hacer referencia a ese origen de almacenamiento de modo que no tengas que recrear objetos intermedios cada vez que tengas acceso a él.

## <a name="stream-performance-in-c-and-visual-basic"></a>Rendimiento de flujos en C# y Visual Basic

### <a name="buffering-between-uwp-and-net-streams"></a>Almacenar en búfer entre flujos UWP y .NET

En muchos escenarios, quizás quieras convertir una secuencia de UWP (como [**Windows.Storage.Streams.IInputStream**](https://msdn.microsoft.com/library/windows/apps/BR241718) o [**IOutputStream**](https://msdn.microsoft.com/library/windows/apps/BR241728)) en una secuencia de .NET ([**System.IO.Stream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.aspx)). Por ejemplo, esto es útil cuando escribes una aplicación para UWP y quieres usar código .NET ya existente que funcione en secuencias con el sistema de archivos de UWP. Para ello, las API de .NET para aplicaciones de la Tienda Windows proporcionan métodos de extensión que te permiten convertir tipos de secuencias entre .NET y UWP. Para más información, consulta [**WindowsRuntimeStreamExtensions**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.aspx).

Cuando conviertes una secuencia de UWP en una de .NET, creas, de forma correcta, un adaptador para la secuencia subyacente de UWP. En algunas circunstancias, invocar métodos en flujos de UWP en tiempo de ejecución tiene asociado un coste. Esto puede afectar a la velocidad de la aplicación, especialmente en escenarios donde realizas varias operaciones pequeñas y frecuentes de lectura o escritura.

Para aumentar la velocidad de la aplicación, los adaptadores de flujos de UWP contienen un búfer de datos. La siguiente muestra de código indica pequeñas lecturas consecutivas usando un adaptador de flujos de UWP con un tamaño de búfer predeterminado.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFile file = await Windows.Storage.ApplicationData.Current
>     .LocalFolder.GetFileAsync("example.txt");
> Windows.Storage.Streams.IInputStream windowsRuntimeStream = 
>     await file.OpenReadAsync();
> 
> byte[] destinationArray = new byte[8];
> 
> // Create an adapter with the default buffer size.
> using (var managedStream = windowsRuntimeStream.AsStreamForRead())
> {
> 
>     // Read 8 bytes into destinationArray.
>     // A larger block is actually read from the underlying 
>     // windowsRuntimeStream and buffered within the adapter.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> 
>     // Read 8 more bytes into destinationArray.
>     // This call may complete much faster than the first call
>     // because the data is buffered and no call to the 
>     // underlying windowsRuntimeStream needs to be made.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> }
> ```
> ```vb
> Dim file As StorageFile = Await Windows.Storage.ApplicationData.Current -
> .LocalFolder.GetFileAsync("example.txt")
> Dim windowsRuntimeStream As Windows.Storage.Streams.IInputStream =
>     Await file.OpenReadAsync()
> 
> Dim destinationArray() As Byte = New Byte(8) {}
> 
> ' Create an adapter with the default buffer size.
> Dim managedStream As Stream = windowsRuntimeStream.AsStreamForRead()
> Using (managedStream)
> 
>     ' Read 8 bytes into destinationArray.
>     ' A larger block is actually read from the underlying 
>     ' windowsRuntimeStream and buffered within the adapter.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
>     ' Read 8 more bytes into destinationArray.
>     ' This call may complete much faster than the first call
>     ' because the data is buffered and no call to the 
>     ' underlying windowsRuntimeStream needs to be made.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
> End Using
> ```

Este comportamiento de búfer es conveniente en la mayoría de los escenarios en que se convierte un flujo de UWP en uno de .NET. Sin embargo, en algunos escenarios quizás necesites retocar el comportamiento de búfer para aumentar el rendimiento.

### <a name="working-with-large-data-sets"></a>Trabajar con grandes conjuntos de datos

Cuando leas o escribas grandes conjuntos de datos, podrás aumentar tu velocidad de lectura o escritura si proporcionas un tamaño de búfer mayor a los métodos de extensión [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx), [**AsStreamForWrite**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforwrite.aspx) y [**AsStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx). Esto le da al adaptador de secuencias un búfer interno de mayor tamaño. Por ejemplo, cuando se pasa un flujo de un archivo grande a un analizador XML, el analizador puede hacer varias lecturas secuenciales pequeñas del flujo. Un búfer grande puede reducir la cantidad de llamadas a la secuencia de UWP y aumentar el rendimiento.

> **Nota**  Debes tener cuidado cuando configures un tamaño de búfer que sea mayor que aproximadamente 80KB, ya que esto puede causar la fragmentación del montón del colector de elementos no utilizados (consulta [Mejorar el rendimiento de la recolección de elementos no utilizados](improve-garbage-collection-performance.md)). El siguiente ejemplo de código crea un adaptador de secuencias administradas con un búfer de 81.920 bytes.

> [!div class="tabbedCodeSnippets"]
```csharp
// Create a stream adapter with an 80 KB buffer.
Stream managedStream = nativeStream.AsStreamForRead(bufferSize: 81920);
```
```vb
' Create a stream adapter with an 80 KB buffer.
Dim managedStream As Stream = nativeStream.AsStreamForRead(bufferSize:=81920)
```

Los métodos [**Stream.CopyTo**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.copyto.aspx) y [**CopyToAsync**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.stream.copytoasync.aspx) también asignan un búfer local para copiar entre secuencias. Al igual que con el método de extensión [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforread.aspx), podrás obtener un mejor rendimiento para copiar secuencias grandes, al invalidar el tamaño del búfer predeterminado. El siguiente ejemplo de código demuestra cómo cambiar el tamaño del búfer predeterminado de una llamada a **CopyToAsync**.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> MemoryStream destination = new MemoryStream();
> // copies the buffer into memory using the default copy buffer
> await managedStream.CopyToAsync(destination);
> 
> // Copy the buffer into memory using a 1 MB copy buffer.
> await managedStream.CopyToAsync(destination, bufferSize: 1024 * 1024);
> ```
> ```vb
> Dim destination As MemoryStream = New MemoryStream()
> ' copies the buffer into memory using the default copy buffer
> Await managedStream.CopyToAsync(destination)
> 
> ' Copy the buffer into memory using a 1 MB copy buffer.
> Await managedStream.CopyToAsync(destination, bufferSize:=1024 * 1024)
> ```

Este ejemplo usa un búfer de 1 MB, que es mayor que el de 80 KB previamente recomendado. El uso de este tamaño de búfer puede mejorar el rendimiento de la operación de copia de varios conjuntos grandes de datos (es decir, varios cientos de megabytes). Si embargo, este búfer se asigna al montón de objetos grandes y podría degradar el rendimiento de la recolección de elementos no usados. Solo debes usar tamaños más grandes de búfer cuando mejoren notablemente el rendimiento de tu aplicación.

Cuando trabajas con una gran cantidad de secuencias al mismo tiempo, es posible que quieras reducir o eliminar la sobrecarga de la memoria del búfer. Puedes especificar un búfer menor o establecer el parámetro *bufferSize* en 0 para desactivar por completo el almacenamiento en búfer de ese adaptador de secuencias. Aún puedes lograr un buen rendimiento sin el almacenamiento en búfer, si realizas lecturas o escrituras grandes en el flujo administrado.

### <a name="performing-latency-sensitive-operations"></a>Realizar operaciones sensibles a la latencia

Probablemente quieras evitar el almacenamiento en búfer, en el caso de escrituras y lecturas de latencia baja y en el caso de que no quieras leer en bloques grandes de un flujo subyacente de UWP. Por ejemplo, podrías querer escrituras y lecturas de latencia baja si estás usando el flujo de comunicaciones de red.

En una aplicación de chat, podrías usar un flujo en una interfaz de red para enviar y recibir mensajes. En este caso quieres enviar mensajes tan pronto estén listos, en lugar de esperar a que el búfer se llene. Si estableces el tamaño del búfer en 0 cuando llames a los métodos de extensión [**AsStreamForRead**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforread.aspx), [**AsStreamForWrite**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstreamforwrite.aspx) y [**AsStream**](https://msdn.microsoft.com/library/windows/apps/xaml/system.io.windowsruntimestreamextensions.asstream.aspx), el adaptador resultante no asignará un búfer y todas las llamadas manipularán directamente la secuencia de UWP subyacente.


