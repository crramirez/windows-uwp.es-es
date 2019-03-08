---
ms.assetid: 4C59D5AC-58F7-4863-A884-E9E54228A5AD
title: Enumerar y consultar archivos y carpetas
description: Permite tener acceso a los archivos que se encuentran en carpetas, bibliotecas, dispositivos o ubicaciones de red. También puedes consultar los archivos y las carpetas que hay en una ubicación si creas consultas de archivos y carpetas.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: b561e08227664f723802ffc0ee3f0e16bc34a5cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613930"
---
# <a name="enumerate-and-query-files-and-folders"></a>Enumerar y consultar archivos y carpetas

Permite tener acceso a los archivos que se encuentran en carpetas, bibliotecas, dispositivos o ubicaciones de red. También puedes consultar los archivos y las carpetas que hay en una ubicación si creas consultas de archivos y carpetas.

Para obtener información sobre cómo almacenar los datos de la aplicación para la Plataforma universal de Windows, consulta la clase [ApplicationData](/uwp/api/windows.storage.applicationdata).

> [!NOTE]
> Para obtener un ejemplo completo, vea el [ejemplo de enumeración de carpeta](https://go.microsoft.com/fwlink/p/?linkid=619993).

## <a name="prerequisites"></a>Requisitos previos

-   **Comprender la programación asincrónica para las aplicaciones de la plataforma Universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic). Para obtener información sobre cómo escribir aplicaciones asincrónicas en C++ / c++ / WinRT, consulte [simultaneidad y operaciones asincrónicas con C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency). Para obtener información sobre cómo escribir aplicaciones asincrónicas en C++ / c++ / CX, consulte [programación asincrónica en C++ / c++ / CX](/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

-   **Permisos de acceso a la ubicación**

    Por ejemplo, el código de estos ejemplos requiere la funcionalidad **picturesLibrary**, pero es posible que la ubicación requiera una funcionalidad distinta o ninguna. Para más información, consulta [Permisos de acceso de archivos](file-access-permissions.md).

## <a name="enumerate-files-and-folders-in-a-location"></a>Enumerar archivos y carpetas en una ubicación

> [!NOTE]
> No olvide declarar el **picturesLibrary** capacidad.

En este ejemplo se usa primero el [ **StorageFolder.GetFilesAsync** ](/uwp/api/windows.storage.storagefolder.getfilesasync) método para obtener todos los archivos de la carpeta raíz de la [ **KnownFolders.PicturesLibrary** ](/uwp/api/windows.storage.knownfolders.pictureslibrary) (no en las subcarpetas) y el nombre de cada archivo de lista. A continuación, usamos el [ **StorageFolder.GetFoldersAsync** ](/uwp/api/windows.storage.storagefolder.getfoldersasync) método para obtener todas las subcarpetas la **PicturesLibrary** y el nombre de cada subcarpeta de la lista.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
StringBuilder outputText = new StringBuilder();

IReadOnlyList<StorageFile> fileList = await picturesFolder.GetFilesAsync();

outputText.AppendLine("Files:");
foreach (StorageFile file in fileList)
{
    outputText.Append(file.Name + "\n");
}

IReadOnlyList<StorageFolder> folderList = await picturesFolder.GetFoldersAsync();
           
outputText.AppendLine("Folders:");
foreach (StorageFolder folder in folderList)
{
    outputText.Append(folder.DisplayName + "\n");
}
```

```cppwinrt
// MainPage.h
// In MainPage.xaml: <TextBlock x:Name="OutputTextBlock"/>
#include <winrt/Windows.Storage.h>
#include <sstream>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    // Be sure to specify the Pictures Folder capability in your Package.appxmanifest.
    Windows::Storage::StorageFolder picturesFolder{
        Windows::Storage::KnownFolders::PicturesLibrary()
    };

    std::wstringstream outputString;
    outputString << L"Files:" << std::endl;

    for (auto const& file : co_await picturesFolder.GetFilesAsync())
    {
        outputString << file.Name().c_str() << std::endl;
    }

    outputString << L"Folders:" << std::endl;
    for (auto const& folder : co_await picturesFolder.GetFoldersAsync())
    {
        outputString << folder.Name().c_str() << std::endl;
    }

    OutputTextBlock().Text(outputString.str().c_str());
}
```

```cpp
#include <ppltasks.h>
#include <string>
#include <memory>

using namespace Windows::Storage;
using namespace Platform::Collections;
using namespace concurrency;
using namespace std;

// Be sure to specify the Pictures Folder capability in the appxmanifext file.
StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;

// Use a shared_ptr so that the string stays in memory
// until the last task is complete.
auto outputString = make_shared<wstring>();
*outputString += L"Files:\n";

// Get a read-only vector of the file objects
// and pass it to the continuation.
create_task(picturesFolder->GetFilesAsync())        
   // outputString is captured by value, which creates a copy
   // of the shared_ptr and increments its reference count.
   .then ([outputString] (IVectorView\<StorageFile^>^ files)
   {        
       for ( unsigned int i = 0 ; i < files->Size; i++)
       {
           *outputString += files->GetAt(i)->Name->Data();
           *outputString += L"\n";
      }
   })
   // We need to explicitly state the return type
   // here: -IAsyncOperation<...>
   .then([picturesFolder]() -IAsyncOperation\<IVectorView\<StorageFolder^>^>^
   {
       return picturesFolder->GetFoldersAsync();
   })
   // Capture "this" to access m_OutputTextBlock from within the lambda.
   .then([this, outputString](IVectorView/<StorageFolder^>^ folders)
   {        
       *outputString += L"Folders:\n";

       for ( unsigned int i = 0; i < folders->Size; i++)
       {
          *outputString += folders->GetAt(i)->Name->Data();
          *outputString += L"\n";
       }

       // Assume m_OutputTextBlock is a TextBlock defined in the XAML.
       m_OutputTextBlock->Text = ref new String((*outputString).c_str());
    });
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim outputText As New StringBuilder

Dim fileList As IReadOnlyList(Of StorageFile) =
    Await picturesFolder.GetFilesAsync()

outputText.AppendLine("Files:")
For Each file As StorageFile In fileList

    outputText.Append(file.Name & vbLf)

Next file

Dim folderList As IReadOnlyList(Of StorageFolder) =
    Await picturesFolder.GetFoldersAsync()

outputText.AppendLine("Folders:")
For Each folder As StorageFolder In folderList

    outputText.Append(folder.DisplayName & vbLf)

Next folder
```

> [!NOTE]
> En C# o Visual Basic, recuerda poner la palabra clave **async** en la declaración del método de cualquier método en el que uses el operador **await**.

Como alternativa, puede usar el [ **clase StorageFolder.GetItemsAsync** ](/uwp/api/windows.storage.storagefolder.getitemsasync) método para obtener todos los elementos (archivos y subcarpetas) en una ubicación determinada. En el ejemplo siguiente se usa el **GetItemsAsync** método para obtener todos los archivos y subcarpetas en la carpeta raíz de la [ **KnownFolders.PicturesLibrary** ](/uwp/api/windows.storage.knownfolders.pictureslibrary) (no en las subcarpetas). Después, en el ejemplo se muestra el nombre de cada archivo y subcarpeta. Si el elemento es una subcarpeta, el ejemplo anexa `"folder"` al nombre.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
StringBuilder outputText = new StringBuilder();

IReadOnlyList<IStorageItem> itemsList = await picturesFolder.GetItemsAsync();

foreach (var item in itemsList)
{
    if (item is StorageFolder)
    {
        outputText.Append(item.Name + " folder\n");

    }
    else
    {
        outputText.Append(item.Name + "\n");
    }
}
```

```cppwinrt
// MainPage.h
// In MainPage.xaml: <TextBlock x:Name="OutputTextBlock"/>
#include <winrt/Windows.Storage.h>
#include <sstream>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    // Be sure to specify the Pictures Folder capability in your Package.appxmanifest.
    Windows::Storage::StorageFolder picturesFolder{
        Windows::Storage::KnownFolders::PicturesLibrary()
    };

    std::wstringstream outputString;

    for (Windows::Storage::IStorageItem const& item : co_await picturesFolder.GetItemsAsync())
    {
        outputString << item.Name().c_str();

        if (item.IsOfType(Windows::Storage::StorageItemTypes::Folder))
        {
            outputString << L" folder" << std::endl;
        }
        else
        {
            outputString << std::endl;
        }

        OutputTextBlock().Text(outputString.str().c_str());
    }
}
```

```cpp
// See previous example for comments, namespace and #include info.
StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
auto outputString = make_shared<wstring>();

create_task(picturesFolder->GetItemsAsync())        
    .then ([this, outputString] (IVectorView<IStorageItem^>^ items)
{        
    for ( unsigned int i = 0 ; i < items->Size; i++)
    {
        *outputString += items->GetAt(i)->Name->Data();
        if(items->GetAt(i)->IsOfType(StorageItemTypes::Folder))
        {
            *outputString += L"  folder\n";
        }
        else
        {
            *outputString += L"\n";
        }
        m_OutputTextBlock->Text = ref new String((*outputString).c_str());
    }
});
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim outputText As New StringBuilder

Dim itemsList As IReadOnlyList(Of IStorageItem) =
    Await picturesFolder.GetItemsAsync()

For Each item In itemsList

    If TypeOf item Is StorageFolder Then

        outputText.Append(item.Name & " folder" & vbLf)

    Else

        outputText.Append(item.Name & vbLf)

    End If

Next item
```

## <a name="query-files-in-a-location-and-enumerate-matching-files"></a>Consultar archivos en una ubicación y enumerar archivos coincidentes

En este ejemplo se consulta para todos los archivos en el [ **KnownFolders.PicturesLibrary** ](/uwp/api/windows.storage.knownfolders.pictureslibrary) agrupados por mes, y esta vez la recorre de ejemplo en las subcarpetas. En primer lugar, se llama a [**StorageFolder.CreateFolderQuery**](/uwp/api/windows.storage.storagefolder.createfolderquery) y se pasa el valor [**CommonFolderQuery.GroupByMonth**](/uwp/api/windows.storage.search.commonfolderquery) al método. Esto nos da un objeto [**StorageFolderQueryResult**](/uwp/api/windows.storage.search.storagefolderqueryresult).

Después, llamamos a [**StorageFolderQueryResult.GetFoldersAsync**](/uwp/api/windows.storage.search.storagefolderqueryresult.getfoldersasync), que devuelve objetos [**StorageFolder**](/uwp/api/windows.storage.storagefolder) que representan carpetas virtuales. En este caso, la agrupación se realiza por mes, por lo que cada carpeta virtual representa un grupo de archivos del mismo mes.

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;

StorageFolderQueryResult queryResult =
    picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth);
        
IReadOnlyList<StorageFolder> folderList =
    await queryResult.GetFoldersAsync();

StringBuilder outputText = new StringBuilder();

foreach (StorageFolder folder in folderList)
{
    IReadOnlyList<StorageFile> fileList = await folder.GetFilesAsync();

    // Print the month and number of files in this group.
    outputText.AppendLine(folder.Name + " (" + fileList.Count + ")");

    foreach (StorageFile file in fileList)
    {
        // Print the name of the file.
        outputText.AppendLine("   " + file.Name);
    }
}
```

```cppwinrt
// MainPage.h
// In MainPage.xaml: <TextBlock x:Name="OutputTextBlock"/>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Search.h>
#include <sstream>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    // Be sure to specify the Pictures Folder capability in your Package.appxmanifest.
    Windows::Storage::StorageFolder picturesFolder{
        Windows::Storage::KnownFolders::PicturesLibrary()
    };

    Windows::Storage::Search::StorageFolderQueryResult queryResult{
        picturesFolder.CreateFolderQuery(Windows::Storage::Search::CommonFolderQuery::GroupByMonth)
    };

    std::wstringstream outputString;

    for (Windows::Storage::StorageFolder const& folder : co_await queryResult.GetFoldersAsync())
    {
        auto files{ co_await folder.GetFilesAsync() };
        outputString << folder.Name().c_str() << L" (" << files.Size() << L")" << std::endl;

        for (Windows::Storage::StorageFile const& file : files)
        {
            outputString << L"    " << file.Name().c_str() << std::endl;
        }
    }

    OutputTextBlock().Text(outputString.str().c_str());
}
```

```cpp
#include <ppltasks.h>
#include <string>
#include <memory>
using namespace Windows::Storage;
using namespace Windows::Storage::Search;
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace std;

StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;

StorageFolderQueryResult^ queryResult =
    picturesFolder->CreateFolderQuery(CommonFolderQuery::GroupByMonth);

// Use shared_ptr so that outputString remains in memory
// until the task completes, which is after the function goes out of scope.
auto outputString = std::make_shared<wstring>();

create_task( queryResult->GetFoldersAsync()).then([this, outputString] (IVectorView<StorageFolder^>^ view)
{        
    for ( unsigned int i = 0; i < view->Size; i++)
    {
        create_task(view->GetAt(i)->GetFilesAsync()).then([this, i, view, outputString](IVectorView<StorageFile^>^ files)
        {
            *outputString += view->GetAt(i)->Name->Data();
            *outputString += L" (";
            *outputString += to_wstring(files->Size);
            *outputString += L")\r\n";
            for (unsigned int j = 0; j < files->Size; j++)
            {
                *outputString += L"     ";
                *outputString += files->GetAt(j)->Name->Data();
                *outputString += L"\r\n";
            }
        }).then([this, outputString]()
        {
            m_OutputTextBlock->Text = ref new String((*outputString).c_str());
        });
    }    
});
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim outputText As New StringBuilder

Dim queryResult As StorageFolderQueryResult =
    picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth)

Dim folderList As IReadOnlyList(Of StorageFolder) =
    Await queryResult.GetFoldersAsync()

For Each folder As StorageFolder In folderList

    Dim fileList As IReadOnlyList(Of StorageFile) =
        Await folder.GetFilesAsync()

    ' Print the month and number of files in this group.
    outputText.AppendLine(folder.Name & " (" & fileList.Count & ")")

    For Each file As StorageFile In fileList

        ' Print the name of the file.
        outputText.AppendLine("   " & file.Name)

    Next file

Next folder
```

El resultado del ejemplo tiene un aspecto similar al siguiente.

```syntax
July ‎2015 (2)
   MyImage3.png
   MyImage4.png
‎December ‎2014 (2)
   MyImage1.png
   MyImage2.png
```
