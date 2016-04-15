---
ms.assetid: 4C59D5AC-58F7-4863-A884-E9E54228A5AD
title: Enumerar y consultar archivos y carpetas
description: Permite tener acceso a los archivos que se encuentran en carpetas, bibliotecas, dispositivos o ubicaciones de red. También puedes consultar los archivos y las carpetas que hay en una ubicación si creas consultas de archivos y carpetas.
---
# Enumerar y consultar archivos y carpetas


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Permite tener acceso a los archivos que se encuentran en carpetas, bibliotecas, dispositivos o ubicaciones de red. También puedes consultar los archivos y las carpetas que hay en una ubicación si creas consultas de archivos y carpetas.

**Nota**  Consulta también [Muestra de enumeración de carpetas](http://go.microsoft.com/fwlink/p/?linkid=619993).

 
## Requisitos previos

-   **Comprender la programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**

    Puedes aprender a escribir aplicaciones asincrónicas en C# o Visual Basic. Consulta [Llamar a API asincrónicas en C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Programación asincrónica en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

-   **Permisos de acceso a la ubicación**

    Por ejemplo, el código de estos ejemplos requiere la funcionalidad **picturesLibrary**, pero es posible que la ubicación requiera una funcionalidad distinta o ninguna. Para más información, consulta [Permisos de acceso de archivos](file-access-permissions.md).

## Enumerar archivos y carpetas en una ubicación

> **Nota**  Recuerda declarar la funcionalidad **picturesLibrary**.

En este ejemplo, se usa primero el método [**StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276) para obtener todos los archivos en la carpeta raíz de la propiedad [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) (no en las subcarpetas) y se muestra el nombre de cada archivo. Después, se usa el método [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227280) para obtener todas las subcarpetas en **PicturesLibrary** y se muestra el nombre de cada subcarpeta.

<!--BUGBUG: IAsyncOperation<IVectorView<StorageFolder^>^>^  causes build to flake out-->
> [!div class="tabbedCodeSnippets"] 
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Platform::Collections;
> using namespace concurrency;
> using namespace std;
> 
> // Be sure to specify the Pictures Folder capability in the appxmanifext file.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> 
> // Use a shared_ptr so that the string stays in memory
> // until the last task is complete.
> auto outputString = make_shared<wstring>();
> *outputString += L"Files:\n";
> 
> // Get a read-only vector of the file objects
> // and pass it to the continuation. 
> create_task(picturesFolder->GetFilesAsync())        
>     // outputString is captured by value, which creates a copy 
>     // of the shared_ptr and increments its reference count.
>     .then ([outputString] (IVectorView\<StorageFile^>^ files)
> {        
>     for ( unsigned int i = 0 ; i < files->Size; i++)
>     {
>         *outputString += files->GetAt(i)->Name->Data();
>         *outputString += L"\n";
>     }
> })
>     // We need to explicitly state the return type 
>     // here: -> IAsyncOperation<...>
>     .then([picturesFolder]() -> IAsyncOperation\<IVectorView\<StorageFolder^>^>^ 
> {
>     return picturesFolder->GetFoldersAsync();
> })
>     // Capture "this" to access m_OutputTextBlock from within the lambda.
>     .then([this, outputString](IVectorView\<StorageFolder^>^ folders)
> {        
>     *outputString += L"Folders:\n";
> 
>     for ( unsigned int i = 0; i < folders->Size; i++)
>     {
>         *outputString += folders->GetAt(i)->Name->Data();
>         *outputString += L"\n";
>     }
> 
>     // Assume m_OutputTextBlock is a TextBlock defined in the XAML.
>     m_OutputTextBlock->Text = ref new String((*outputString).c_str());
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
> 
> IReadOnlyList<StorageFile> fileList = 
>     await picturesFolder.GetFilesAsync();
> 
> outputText.AppendLine("Files:");
> foreach (StorageFile file in fileList)
> {
>     outputText.Append(file.Name + "\n");
> }
> 
> IReadOnlyList<StorageFolder> folderList = 
>     await picturesFolder.GetFoldersAsync();
>            
> outputText.AppendLine("Folders:");
> foreach (StorageFolder folder in folderList)
> {
>     outputText.Append(folder.DisplayName + "\n");
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
> 
> Dim fileList As IReadOnlyList(Of StorageFile) =
>     Await picturesFolder.GetFilesAsync()
> 
> outputText.AppendLine("Files:")
> For Each file As StorageFile In fileList
> 
>     outputText.Append(file.Name & vbLf)
> 
> Next file
> 
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await picturesFolder.GetFoldersAsync()
> 
> outputText.AppendLine("Folders:")
> For Each folder As StorageFolder In folderList
> 
>     outputText.Append(folder.DisplayName & vbLf)
> 
> Next folder
> ```


> **Nota**  En C# o Visual Basic, recuerda poner la palabra clave **async** en la declaración del método de cualquier método en el que uses el operador **await**.
 

Como alternativa, puedes usar el método [**GetItemsAsync**](https://msdn.microsoft.com/library/windows/apps/br227286) para obtener todos los elementos (archivos y subcarpetas) en una ubicación en particular. En el siguiente ejemplo se usa el método **GetItemsAsync** para obtener todos los archivos y subcarpetas en la carpeta raíz de [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) (no en las subcarpetas). Después, en el ejemplo se muestra el nombre de cada archivo y subcarpeta. Si el elemento es una subcarpeta, el ejemplo anexa `"folder"` al nombre.

> [!div class="tabbedCodeSnippets"] 
> ```cpp
> // See previous example for comments, namespace and #include info.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> auto outputString = make_shared<wstring>();
> 
> create_task(picturesFolder->GetItemsAsync())        
>     .then ([this, outputString] (IVectorView<IStorageItem^>^ items)
> {        
>     for ( unsigned int i = 0 ; i < items->Size; i++)
>     {
>         *outputString += items->GetAt(i)->Name->Data();
>         if(items->GetAt(i)->IsOfType(StorageItemTypes::Folder))
>         {
>             *outputString += L"  folder\n";
>         }
>         else
>         {
>             *outputString += L"\n";
>         }
>         m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     }
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
> 
> IReadOnlyList<IStorageItem> itemsList = 
>     await picturesFolder.GetItemsAsync();
> 
> foreach (var item in itemsList)
> {
>     if (item is StorageFolder)
>     {
>         outputText.Append(item.Name + " folder\n");
> 
>     }
>     else
>     {
>         outputText.Append(item.Name + "\n");
> 
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
> 
> Dim itemsList As IReadOnlyList(Of IStorageItem) =
>     Await picturesFolder.GetItemsAsync()
> 
> For Each item In itemsList
> 
>     If TypeOf item Is StorageFolder Then
> 
>         outputText.Append(item.Name & " folder" & vbLf)
> 
>     Else
> 
>         outputText.Append(item.Name & vbLf)
> 
>     End If
> 
> Next item
> ```

## Query files in a location and enumerate matching files

In this example we query for all the files in the [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) grouped by the month, and this time the example recurses into subfolders. En primer lugar, se llama a [**StorageFolder.CreateFolderQuery**](https://msdn.microsoft.com/library/windows/apps/br227262) y se pasa el valor [**CommonFolderQuery.GroupByMonth**](https://msdn.microsoft.com/library/windows/apps/br207957) al método. Esto nos da un objeto [**StorageFolderQueryResult**](https://msdn.microsoft.com/library/windows/apps/br208066).

Después, llamamos a [**StorageFolderQueryResult.GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br208074), que devuelve objetos [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que representan carpetas virtuales. En este caso, la agrupación se realiza por mes, por lo que cada carpeta virtual representa un grupo de archivos del mismo mes.

> [!div class="tabbedCodeSnippets"] 
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Windows::Storage::Search;
> using namespace concurrency;
> using namespace Platform::Collections;
> using namespace Windows::Foundation::Collections;
> using namespace std;
> 
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> 
> StorageFolderQueryResult^ queryResult = 
>     picturesFolder->CreateFolderQuery(CommonFolderQuery::GroupByMonth);
> 
> // Use shared_ptr so that outputString remains in memory
> // until the task completes, which is after the function goes out of scope.
> auto outputString = std::make_shared<wstring>();
> 
> create_task( queryResult->GetFoldersAsync()).then([this, outputString] (IVectorView<StorageFolder^>^ view) 
> {        
>     for ( unsigned int i = 0; i < view->Size; i++)
>     {
>         create_task(view->GetAt(i)->GetFilesAsync()).then([this, i, view, outputString](IVectorView<StorageFile^>^ files)
>         {
>             *outputString += view->GetAt(i)->Name->Data();
>             *outputString += L"(";
>             *outputString += to_wstring(files->Size);
>             *outputString += L")\r\n";
>             for (unsigned int j = 0; j < files->Size; j++)
>             {
>                 *outputString += L"     ";
>                 *outputString += files->GetAt(j)->Name->Data();
>                 *outputString += L"\r\n";
>             }
>         }).then([this, outputString]()
>         {
>             m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>         });
>     }    
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> 
> StorageFolderQueryResult queryResult = 
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth);
>         
> IReadOnlyList<StorageFolder> folderList = 
>     await queryResult.GetFoldersAsync();
> 
> StringBuilder outputText = new StringBuilder();
> 
> foreach (StorageFolder folder in folderList)
> {
>     IReadOnlyList<StorageFile> fileList = await folder.GetFilesAsync();
> 
>     // Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name + " (" + fileList.Count + ")");
> 
>     foreach (StorageFile file in fileList)
>     {
>         // Print the name of the file.
>         outputText.AppendLine("   " + file.Name);
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
> 
> Dim queryResult As StorageFolderQueryResult =
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth)
> 
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await queryResult.GetFoldersAsync()
> 
> For Each folder As StorageFolder In folderList
> 
>     Dim fileList As IReadOnlyList(Of StorageFile) =
>         Await folder.GetFilesAsync()
> 
>     ' Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name & " (" & fileList.Count & ")")
> 
>     For Each file As StorageFile In fileList
> 
>         ' Print the name of the file.
>         outputText.AppendLine("   " & file.Name)
> 
>     Next file
> 
> Next folder
> ```

The output of the example looks similar to the following.

``` syntax
July ‎2015 (2)
   MyImage3.png
   MyImage4.png
‎December ‎2014 (2)
   MyImage1.png
   MyImage2.png
```



<!--HONumber=Mar16_HO1-->


