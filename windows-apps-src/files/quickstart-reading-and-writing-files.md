---
author: laurenhughes
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: Crear, escribir y leer archivos
description: Lee y escribe un archivo mediante un objeto StorageFile.
translationtype: Human Translation
ms.sourcegitcommit: 6822bb63ac99efdcdd0e71c4445883f4df5f471d
ms.openlocfilehash: 0709d9c9126dc4523eae58d5db8d9037a2fb618e

---

# <a name="create-write-and-read-a-file"></a>Crear, escribir y leer archivos


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Clase StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)
-   [**Clase StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)
-   [**Clase FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440)

Leer y escribir un archivo con un objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171).

> **Nota** Consulta también [File access sample (Muestra de acceso a archivos)](http://go.microsoft.com/fwlink/p/?linkid=619995).

## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica de las aplicaciones para Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Aprender a obtener el archivo del que deseas leer o en el que quieres escribir**

    Puedes aprender a obtener un archivo al usar un selector de archivos en [Abrir archivos y carpetas con un selector](quickstart-using-file-and-folder-pickers.md).

## <a name="creating-a-file"></a>Creación de un archivo

Aquí se muestra cómo se crea un archivo en la carpeta local de la aplicación. Si ya existe, se reemplaza.
> [!div class="tabbedCodeSnippets"]
```cs
// Create sample file; replace if exists.
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.CreateFileAsync("sample.txt",
        Windows.Storage.CreationCollisionOption.ReplaceExisting);
```
```vb
' Create sample file; replace if exists.
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.CreateFileAsync("sample.txt", CreationCollisionOption.ReplaceExisting)
```

## <a name="writing-to-a-file"></a>Escritura en un archivo


A continuación se describe cómo escribir en un archivo editable en el disco mediante la clase [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). El primer paso común de cada una de las maneras de escribir en un archivo (a menos que vayas a escribir en el archivo inmediatamente después de crearlo) es obtener el archivo con [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272).
> [!div class="tabbedCodeSnippets"]
```cs
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**Escribir texto en un archivo**

Escribe texto en el archivo llamando al método [**WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505) de la clase [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).
> [!div class="tabbedCodeSnippets"]
```cs
await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow");
```
```vb
Await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow")
```

**Escritura de bytes en un archivo con un búfer (2 pasos)**

1.  En primer lugar, llama a [**ConvertStringToBinary**](https://msdn.microsoft.com/library/windows/apps/br241385) para obtener un búfer con los bytes (basado en una cadena arbitraria) que deseas escribir en el archivo.
> [!div class="tabbedCodeSnippets"]
```cs
var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```
```vb
Dim buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
                    "What fools these mortals be",
                    Windows.Security.Cryptography.BinaryStringEncoding.Utf8)
```

2.  A continuación, escribe los bytes del búfer en el archivo llamando al método [**WriteBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701490) de la clase [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).
> [!div class="tabbedCodeSnippets"]
```cs
await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer);
```
```vb
Await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer)
```

**Escritura de texto en un archivo con un flujo (4 pasos)**

1.  En primer lugar, abre el archivo llamando al método [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851). Cuando se complete la operación de apertura, devolverá un flujo del contenido del archivo.
> [!div class="tabbedCodeSnippets"]
```cs
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```
```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  A continuación, obtén un flujo de salida llamando al método [**GetOutputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241738) de `stream`. Coloca esto en una instrucción **using** para administrar la duración del flujo de salida.
> [!div class="tabbedCodeSnippets"]
```cs
using (var outputStream = stream.GetOutputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
```
```vb
Using outputStream = stream.GetOutputStreamAt(0)
  ' We'll add more code here in the next step.
End Using
```

3.  Ahora, agrega este código en la instrucción **using** existente para escribir en el flujo de salida creando un nuevo objeto [**DataWriter**](https://msdn.microsoft.com/library/windows/apps/br208154) y llamando al método [**DataWriter.WriteString**](https://msdn.microsoft.com/library/windows/apps/br241642).
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataWriter = new Windows.Storage.Streams.DataWriter(outputStream))
    {
        dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    }
```
```vb
    Dim dataWriter As New DataWriter(outputStream)
    dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.")
```

4.  Por último, agrega este código (en la instrucción **using** interna) para guardar el texto en el archivo con [**StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) y cierra el flujo con [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br241729).
> [!div class="tabbedCodeSnippets"]
```cs
    await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
```
```vb
    Await dataWriter.StoreAsync()
        Await outputStream.FlushAsync()
```

## <a name="reading-from-a-file"></a>Leer desde un archivo


Aquí se muestra cómo puedes leer desde un archivo del disco mediante la clase [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171). El primer paso común para cada una de las maneras de leer archivos es obtener el archivo con [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272).
> [!div class="tabbedCodeSnippets"]
```cs
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**Leer texto desde un archivo**

Lee texto del archivo llamando al método [**ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482) de la clase [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).
> [!div class="tabbedCodeSnippets"]
```cs
string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
```
```vb
Dim text As String = Await Windows.Storage.FileIO.ReadTextAsync(sampleFile)
```

**Lectura de texto desde un archivo mediante un búfer (2 pasos)**

1.  En primer lugar, llama al método [**ReadBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701468) de la clase [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440).
> [!div class="tabbedCodeSnippets"]
```cs
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
```
```vb
Dim buffer = Await Windows.Storage.FileIO.ReadBufferAsync(sampleFile)
```

2.  A continuación, usa un objeto [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) para leer en primer lugar la longitud del búfer y después su contenido.
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
    {
        string text = dataReader.ReadString(buffer.Length);
    }
```
```vb
Dim dataReader As DataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer)
    Dim text As String = dataReader.ReadString(buffer.Length)
```

**Lectura de texto desde un archivo con un flujo (4 pasos)**

1.  Abre un flujo para el archivo llamando al método [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851). Devuelve un flujo del contenido del archivo cuando se completa la operación.
> [!div class="tabbedCodeSnippets"]
```cs
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```
```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  Obtén el tamaño del flujo para usarlo más adelante.
> [!div class="tabbedCodeSnippets"]
```cs
ulong size = stream.Size;
```
```vb
Dim size = stream.Size
```

3.  Obtén un flujo de entrada llamando al método [**GetInputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241737). Coloca esto en una instrucción **using** para administrar la duración de la secuencia. Especifica 0 cuando llames a **GetInputStreamAt** para establecer la posición al principio de la secuencia.
> [!div class="tabbedCodeSnippets"]
```cs
using (var inputStream = stream.GetInputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
```
```vb
Using inputStream = stream.GetInputStreamAt(0)
    ' We'll add more code here in the next step.
End Using
```

4.  Por último, agrega este código dentro de la instrucción **using** existente para obtener un objeto [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) en la secuencia. Luego, lee el texto con una llamada a [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135) y [**DataReader.ReadString**](https://msdn.microsoft.com/library/windows/apps/br208147).
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        uint numBytesLoaded = await dataReader.LoadAsync((uint)size);
        string text = dataReader.ReadString(numBytesLoaded);
    }
```
```vb
Dim dataReader As New DataReader(inputStream)
    Dim numBytesLoaded As UInteger = Await dataReader.LoadAsync(CUInt(size))
    Dim text As String = dataReader.ReadString(numBytesLoaded)
```

 

 



<!--HONumber=Dec16_HO1-->


