---
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: Crear, escribir y leer archivos
description: Obtenga información sobre cómo crear, escribir y leer un archivo con los objetos FileIO, StorageFolder y StorageFile de la Plataforma universal de Windows (UWP).
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 89bdd0793c7af26ccd56bc2494ab6d60a6574df0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168389"
---
# <a name="create-write-and-read-a-file"></a>Crear, escribir y leer archivos

**API importantes**

-   [**Clase StorageFolder**](/uwp/api/windows.storage.storagefolder)
-   [**Clase StorageFile**](/uwp/api/windows.storage.storagefile)
-   [**Clase FileIO**](/uwp/api/windows.storage.fileio)

Leer y escribir un archivo mediante un objeto [**StorageFile**](/uwp/api/windows.storage.storagefile).

> [!NOTE]
> Para obtener una muestra completa, consulte [Muestra de acceso a archivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess).

## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica de las aplicaciones para Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md). Para obtener información sobre cómo escribir aplicaciones asincrónicas en C++/WinRT, consulte [Operaciones simultáneas y asincrónicas con C++/WinRT](../cpp-and-winrt-apis/concurrency.md). Para aprender a escribir aplicaciones asincrónicas en C++/CX, consulte [Programación asincrónica en C++/CX](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

-   **Aprenda a obtener el archivo que quiere leer o en el que quiere escribir o ambos**

    Puedes aprender a obtener un archivo al usar un selector de archivos en [Abrir archivos y carpetas con un selector](quickstart-using-file-and-folder-pickers.md).

## <a name="creating-a-file"></a>Crear de un archivo

Aquí se muestra cómo se crea un archivo en la carpeta local de la aplicación. Si ya existe, se reemplaza.

```csharp
// Create sample file; replace if exists.
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.CreateFileAsync("sample.txt",
        Windows.Storage.CreationCollisionOption.ReplaceExisting);
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    // Create a sample file; replace if exists.
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    co_await storageFolder.CreateFileAsync(L"sample.txt", Windows::Storage::CreationCollisionOption::ReplaceExisting);
}
```

```cpp
// Create a sample file; replace if exists.
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
concurrency::create_task(storageFolder->CreateFileAsync("sample.txt", CreationCollisionOption::ReplaceExisting));
```

```vb
' Create sample file; replace if exists.
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.CreateFileAsync("sample.txt", CreationCollisionOption.ReplaceExisting)
```

## <a name="writing-to-a-file"></a>Escritura en un archivo

A continuación se describe cómo escribir en un archivo editable en el disco mediante la clase [**StorageFile**](/uwp/api/windows.storage.storagefile). El primer paso común de cada una de las maneras de escribir en un archivo (a menos que vayas a escribir en el archivo inmediatamente después de crearlo) es obtener el archivo con [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync).

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.CreateFileAsync(L"sample.txt", Windows::Storage::CreationCollisionOption::ReplaceExisting) };
    // Process sampleFile
}
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile) 
{
    // Process file
});
```

```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**Escribir texto en un archivo**

Escriba texto en el archivo llamando al método [**FileIO.WriteTextAsync**](/uwp/api/windows.storage.fileio.writetextasync).

```csharp
await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow");
```

```cppwinrt
// MainPage.h
#include <winrt/Windows.Storage.h>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    // Write text to the file.
    co_await Windows::Storage::FileIO::WriteTextAsync(sampleFile, L"Swift as a shadow");
}
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile) 
{
    //Write text to a file
    create_task(FileIO::WriteTextAsync(sampleFile, "Swift as a shadow"));
});
```

```vb
Await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow")
```

**Escritura de bytes en un archivo con un búfer (2 pasos)**

1.  En primer lugar, llame a [**CryptographicBuffer.ConvertStringToBinary**](/uwp/api/windows.security.cryptography.cryptographicbuffer.convertstringtobinary) para obtener un búfer con los bytes (basados en una cadena) que desea escribir en el archivo.

    ```csharp
    var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
    ```

    ```cppwinrt
    // MainPage.h
    #include <winrt/Windows.Security.Cryptography.h>
    #include <winrt/Windows.Storage.h>
    #include <winrt/Windows.Storage.Streams.h>
    ...
    Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
    {
        Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
        auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
        // Create the buffer.
        Windows::Storage::Streams::IBuffer buffer{
            Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
                L"What fools these mortals be", Windows::Security::Cryptography::BinaryStringEncoding::Utf8)};
        // The code in step 2 goes here.
    }
    ```

    ```cpp
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        // Create the buffer
        IBuffer^ buffer = CryptographicBuffer::ConvertStringToBinary
        ("What fools these mortals be", BinaryStringEncoding::Utf8);
    });
    ```

    ```vb
    Dim buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        "What fools these mortals be",
        Windows.Security.Cryptography.BinaryStringEncoding.Utf8)
    ```

2.  A continuación, escriba los bytes del búfer en el archivo llamando al método [**FileIO.WriteBufferAsync**](/uwp/api/windows.storage.fileio.writebufferasync).

    ```csharp
    await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer);
    ```

    ```cppwinrt
    co_await Windows::Storage::FileIO::WriteBufferAsync(sampleFile, buffer);
    ```
    
    ```cpp
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        // Create the buffer
        IBuffer^ buffer = CryptographicBuffer::ConvertStringToBinary
        ("What fools these mortals be", BinaryStringEncoding::Utf8);      
        // Write bytes to a file using a buffer
        create_task(FileIO::WriteBufferAsync(sampleFile, buffer));
    });
    ```
    
    ```vb
    Await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer)
    ```

**Escritura de texto en un archivo con flujo (4 pasos)**

1.  En primer lugar, abre el archivo llamando al método [**StorageFile.OpenAsync**](/uwp/api/windows.storage.storagefile.openasync). Cuando se complete la operación de apertura, devolverá un flujo del contenido del archivo.

    ```csharp
    var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
    ```
    
    ```cppwinrt
    // MainPage.h
    #include <winrt/Windows.Storage.h>
    #include <winrt/Windows.Storage.Streams.h>
    ...
    Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
    {
        Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
        auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
        Windows::Storage::Streams::IRandomAccessStream stream{ co_await sampleFile.OpenAsync(Windows::Storage::FileAccessMode::ReadWrite) };
        // The code in step 2 goes here.
    }
    ```
    
    ```cpp
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        create_task(sampleFile->OpenAsync(FileAccessMode::ReadWrite)).then([sampleFile](IRandomAccessStream^ stream)
        {
            // Process stream
        });
    });
    ```
    
    ```vb
    Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
    ```

2.  A continuación, obtenga un flujo de salida llamando al método [**IRandomAccessStream.GetOutputStreamAt**](/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat) desde el `stream`. Si usa C#, entonces coloque esto en una instrucción **using** para administrar la duración del flujo de salida. Si usa [ C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md), a continuación puede controlar su duración al incluirlo en un bloque, o establecerlo en `nullptr` cuando haya terminado con él.

    ```csharp
    using (var outputStream = stream.GetOutputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
    ```
    
    ```cppwinrt
    Windows::Storage::Streams::IOutputStream outputStream{ stream.GetOutputStreamAt(0) };
    // The code in step 3 goes here.
    ```
    
    ```cpp
    // Add to "Process stream" in part 1
    IOutputStream^ outputStream = stream->GetOutputStreamAt(0);
    ```
    
    ```vb
    Using outputStream = stream.GetOutputStreamAt(0)
    ' We'll add more code here in the next step.
    End Using
    ```

3.  Ahora agregue este código (si usa C#, en la instrucción existente **using**)para escribir en el flujo de salida creando un nuevo objeto [**DataWriter**](/uwp/api/windows.storage.streams.datawriter) y llamando al método [**DataWriter.WriteString**](/uwp/api/windows.storage.streams.datawriter.writestring).

    ```csharp
    using (var dataWriter = new Windows.Storage.Streams.DataWriter(outputStream))
    {
        dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    }
    ```
    
    ```cppwinrt
    Windows::Storage::Streams::DataWriter dataWriter;
    dataWriter.WriteString(L"DataWriter has methods to write to various types, such as DataTimeOffset.");
    // The code in step 4 goes here.
    ```
    
    ```cpp
    // Added after code from part 2
    DataWriter^ dataWriter = ref new DataWriter(outputStream);
    dataWriter->WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    ```
    
    ```vb
    Dim dataWriter As New DataWriter(outputStream)
    dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.")
    ```

4.  Por último, agregue este código (si usa C#, en la instrucción interna **using**) para guardar el texto en el archivo con [**DataWriter.StoreAsync**](/uwp/api/windows.storage.streams.datawriter.storeasync) y cierre el flujo con [**IOutputStream.FlushAsync**](/uwp/api/windows.storage.streams.ioutputstream.flushasync).

    ```csharp
    await dataWriter.StoreAsync();
    await outputStream.FlushAsync();
    ```
    
    ```cppwinrt
    dataWriter.StoreAsync();
    outputStream.FlushAsync();
    ```
    
    ```cpp
    // Added after code from part 3
    dataWriter->StoreAsync();
    outputStream->FlushAsync();
    ```
    
    ```vb
    Await dataWriter.StoreAsync()
    Await outputStream.FlushAsync()
    ```

**Procedimientos recomendados para escribir en archivos**

Para obtener más información e instrucciones de prácticas recomendadas, consulte [Procedimientos recomendados para escribir en archivos](best-practices-for-writing-to-files.md).
    
## <a name="reading-from-a-file"></a>Leer desde un archivo

Aquí se muestra cómo puedes leer desde un archivo del disco mediante la clase [**StorageFile**](/uwp/api/Windows.Storage.StorageFile). El primer paso común para cada una de las maneras de leer archivos es obtener el archivo con [**StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync).

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```

```cppwinrt
Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
// Process file
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    // Process file
});
```

```vb
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**Leer texto desde un archivo**

Lee texto del archivo llamando al método [**FileIO.ReadTextAsync**](/uwp/api/windows.storage.fileio.readtextasync).

```csharp
string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
```

```cppwinrt
Windows::Foundation::IAsyncOperation<winrt::hstring> ExampleCoroutineAsync()
{
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    co_return co_await Windows::Storage::FileIO::ReadTextAsync(sampleFile);
}
```

```cpp
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    return FileIO::ReadTextAsync(sampleFile);
});
```

```vb
Dim text As String = Await Windows.Storage.FileIO.ReadTextAsync(sampleFile)
```

**Lectura de texto desde un archivo mediante un búfer (2 pasos)**

1.  En primer lugar, llame al método [**FileIO.ReadBufferAsync**](/uwp/api/windows.storage.fileio.readbufferasync).

    ```csharp
    var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    Windows::Storage::Streams::IBuffer buffer{ co_await Windows::Storage::FileIO::ReadBufferAsync(sampleFile) };
    // The code in step 2 goes here.
    ```
    
    ```cpp
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        return FileIO::ReadBufferAsync(sampleFile);
    
    }).then([](Streams::IBuffer^ buffer)
    {
        // Process buffer
    });
    ```
    
    ```vb
    Dim buffer = Await Windows.Storage.FileIO.ReadBufferAsync(sampleFile)
    ```

2.  A continuación, usa un objeto [**DataReader**](/uwp/api/windows.storage.streams.datareader) para leer en primer lugar la longitud del búfer y después su contenido.

    ```csharp
    using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
    {
        string text = dataReader.ReadString(buffer.Length);
    }
    ```
    
    ```cppwinrt
    auto dataReader{ Windows::Storage::Streams::DataReader::FromBuffer(buffer) };
    winrt::hstring bufferText{ dataReader.ReadString(buffer.Length()) };
    ```
    
    ```cpp
    // Add to "Process buffer" section from part 1
    auto dataReader = DataReader::FromBuffer(buffer);
    String^ bufferText = dataReader->ReadString(buffer->Length);
    ```
    
    ```vb
    Dim dataReader As DataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer)
    Dim text As String = dataReader.ReadString(buffer.Length)
    ```

**Lectura de texto desde un archivo con un flujo (4 pasos)**

1.  Abre un flujo para el archivo llamando al método [**StorageFile.OpenAsync**](/uwp/api/windows.storage.storagefile.openasync). Devuelve un flujo del contenido del archivo cuando se completa la operación.

    ```csharp
    var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder storageFolder{ Windows::Storage::ApplicationData::Current().LocalFolder() };
    auto sampleFile{ co_await storageFolder.GetFileAsync(L"sample.txt") };
    Windows::Storage::Streams::IRandomAccessStream stream{ co_await sampleFile.OpenAsync(Windows::Storage::FileAccessMode::Read) };
    // The code in step 2 goes here.
    ```
    
    ```cpp
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        create_task(sampleFile->OpenAsync(FileAccessMode::Read)).then([sampleFile](IRandomAccessStream^ stream)
        {
            // Process stream
        });
    });
    ```
    
    ```vb
    Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.Read)
    ```

2.  Obtén el tamaño del flujo para usarlo más adelante.

    ```csharp
    ulong size = stream.Size;
    ```
    
    ```cppwinrt
    uint64_t size{ stream.Size() };
    // The code in step 3 goes here.
    ```
    
    ```cpp
    // Add to "Process stream" from part 1
    UINT64 size = stream->Size;
    ```
    
    ```vb
    Dim size = stream.Size
    ```

3.  Obtenga un flujo de entrada llamando al método [**IRandomAccessStream.GetInputStreamAt**](/uwp/api/windows.storage.streams.irandomaccessstream.getinputstreamat). Coloca esto en una instrucción **using** para administrar la duración de la secuencia. Especifica 0 cuando llames a **GetInputStreamAt** para establecer la posición al principio de la secuencia.

    ```csharp
    using (var inputStream = stream.GetInputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    ```
    
    ```cppwinrt
    Windows::Storage::Streams::IInputStream inputStream{ stream.GetInputStreamAt(0) };
    Windows::Storage::Streams::DataReader dataReader{ inputStream };
    // The code in step 4 goes here.
    ```
    
    ```cpp
    // Add after code from part 2
    IInputStream^ inputStream = stream->GetInputStreamAt(0);
    auto dataReader = ref new DataReader(inputStream);
    ```
    
    ```vb
    Using inputStream = stream.GetInputStreamAt(0)
    ' We'll add more code here in the next step.
    End Using
    ```

4.  Por último, agrega este código dentro de la instrucción **using** existente para obtener un objeto [**DataReader**](/uwp/api/windows.storage.streams.datareader) en la secuencia. Luego, lee el texto con una llamada a [**DataReader.LoadAsync**](/uwp/api/windows.storage.streams.datareader.loadasync) y [**DataReader.ReadString**](/uwp/api/windows.storage.streams.datareader.readstring).

    ```csharp
    using (var dataReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        uint numBytesLoaded = await dataReader.LoadAsync((uint)size);
        string text = dataReader.ReadString(numBytesLoaded);
    }
    ```
    
    ```cppwinrt
    unsigned int cBytesLoaded{ co_await dataReader.LoadAsync(size) };
    winrt::hstring streamText{ dataReader.ReadString(cBytesLoaded) };
    ```
    
    ```cpp
    // Add after code from part 3
    create_task(dataReader->LoadAsync(size)).then([sampleFile, dataReader](unsigned int numBytesLoaded)
    {
        String^ streamText = dataReader->ReadString(numBytesLoaded);
    });
    ```
    
    ```vb
    Dim dataReader As New DataReader(inputStream)
    Dim numBytesLoaded As UInteger = Await dataReader.LoadAsync(CUInt(size))
    Dim text As String = dataReader.ReadString(numBytesLoaded)
    ```

## <a name="see-also"></a>Consulta también

- [Procedimientos recomendados para escribir en archivos](best-practices-for-writing-to-files.md)