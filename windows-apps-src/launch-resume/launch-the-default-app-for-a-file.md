---
title: Iniciar la aplicación predeterminada de un archivo
description: Aprenda a usar el TEM Windows.Sys. API del iniciador para iniciar el controlador predeterminado para un archivo que la aplicación no puede controlar.
ms.assetid: BB45FCAF-DF93-4C99-A8B5-59B799C7BD98
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 34aee5b2e2f04b7e5d72a7bc31d2cfafc1bcb6fb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167849"
---
# <a name="launch-the-default-app-for-a-file"></a>Iniciar la aplicación predeterminada de un archivo

**API importantes**

-   [**Windows.System.Launcher.LaunchFileAsync**](/uwp/api/windows.system.launcher.launchfileasync)

Aprende cómo iniciar la aplicación predeterminada de un archivo. Muchas aplicaciones necesitan funcionar con archivos que no pueden controlar. Por ejemplo, las aplicaciones de correo electrónico reciben una gran variedad de tipos de archivo y necesitan una forma de iniciar estos archivos en sus controladores predeterminados. Los siguientes pasos te mostrarán cómo usar la API [**Windows.System.Launcher**](/uwp/api/Windows.System.Launcher) para iniciar el controlador predeterminado para un archivo que no puede controlar la aplicación.

## <a name="get-the-file-object"></a>Obtener un objeto de archivo

Primero, obtén un objeto [**Windows.Storage.StorageFile**](/uwp/api/Windows.Storage.StorageFile) para el archivo.

Si el archivo está incluido en el paquete de la aplicación, puedes usar la propiedad [**Package.InstalledLocation**](/uwp/api/windows.applicationmodel.package.installedlocation) para obtener un objeto [**Windows.Storage.StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) y el método [**Windows.Storage.StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) para obtener el objeto [**StorageFile**](/uwp/api/Windows.Storage.StorageFile).

Si el archivo está en una carpeta conocida, puedes usar las propiedades de la clase [**Windows.Storage.KnownFolders**](/uwp/api/Windows.Storage.KnownFolders) para obtener una clase [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) y el método [**GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) para obtener el objeto [**StorageFile**](/uwp/api/Windows.Storage.StorageFile).

## <a name="launch-the-file"></a>Inicia el archivo

Windows proporciona varias opciones distintas para iniciar el controlador predeterminado de un archivo. Estas opciones se explican en este gráfico y en las secciones que siguen.

| Opción | Método | Descripción |
|--------|--------|-------------|
| Inicio predeterminado | [**LaunchFileAsync(IStorageFile)**](/uwp/api/windows.system.launcher.launchfileasync) | Inicia el archivo especificado con el controlador predeterminado. |
| Abrir con inicio | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) | Inicia el archivo especificado dejando que el usuario elija el controlador mediante el cuadro de diálogo Abrir con. |
| Iniciar con una reserva de aplicación recomendada | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) | Inicia el archivo especificado con el controlador predeterminado. Si el sistema no tiene ningún controlador instalado, recomienda al usuario una aplicación de la tienda. |
| Inicio con una vista deseada permanente | [**LaunchFileAsync(IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) (solo Windows) | Inicia el archivo especificado con el controlador predeterminado. Especifica una preferencia para que permanezca en la pantalla después de iniciar y solicitar un tamaño de ventana específico. [**LauncherOptions.DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) no se admite en la familia de dispositivos móviles. |

### <a name="default-launch"></a>Inicio predeterminado

Llama al método [**Windows.System.Launcher.LaunchFileAsync(IStorageFile)**](/uwp/api/windows.system.launcher.launchfileasync) para iniciar la aplicación predeterminada. En este ejemplo, se usa el método [**Windows.Storage.StorageFolder.GetFileAsync**](/uwp/api/windows.storage.storagefolder.getfileasync) para iniciar un archivo de imagen, test.png, incluido en el paquete de la aplicación.

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);
   
   if (file != null)
   {
      // Launch the retrieved file
      var success = await Windows.System.Launcher.LaunchFileAsync(file);

      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()
   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)
   
   If file IsNot Nothing Then
      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="open-with-launch"></a>Abrir con inicio

Llama al método [**Windows.System.Launcher.LaunchFileAsync(IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) con [**LauncherOptions.DisplayApplicationPicker**](/uwp/api/windows.system.launcheroptions.displayapplicationpicker) establecido en **true** para iniciar la aplicación que el usuario seleccione en el cuadro de diálogo **Abrir con**.

Recomendamos que uses el cuadro de diálogo **Abrir con**, cuando el usuario quiera seleccionar una aplicación que no sea la aplicación predeterminada para un archivo en particular. Por ejemplo, si la aplicación permite al usuario iniciar un archivo de imagen, el controlador predeterminado probablemente sea una aplicación de visualización. A veces, es posible que el usuario quiera editar la imagen en lugar de verla. Usa la opción **Abrir con** junto con un comando alternativo de la **barra de la aplicación** o de un menú contextual, para permitir que el usuario abra el cuadro de diálogo **Abrir con** y selecciona la aplicación de edición en estos tipos de escenarios.

![se inicia el cuadro de diálogo Abrir con de un archivo .png el cuadro de diálogo contiene una casilla que especifica si la elección del usuario se debe usar para todos los archivos .png o solo para este archivo .png. el cuadro de diálogo contiene cuatro opciones de la aplicación para iniciar el archivo y un vínculo 'más opciones'.](images/checkboxopenwithdialog.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
      string imageFile = @"images\test.png";
      
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the option to show the picker
      var options = new Windows.System.LauncherOptions();
      options.DisplayApplicationPicker = true;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.png"

   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the option to show the picker
      Dim options = Windows.System.LauncherOptions()
      options.DisplayApplicationPicker = True

      ' Launch the retrieved file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the option to show the picker
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DisplayApplicationPicker(true);

        // Launch the retrieved file
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the option to show the picker
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DisplayApplicationPicker = true;

         // Launch the retrieved file
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

**Iniciar con una reserva de aplicación recomendada**

En algunos casos, el usuario podría no tener instalada una aplicación para administrar el archivo que estás iniciando. Si esto sucede, de manera predeterminada, Windows ofrece un vínculo al usuario para que busque una aplicación apropiada en la Tienda. Si quieres recomendar al usuario qué aplicación comprar en este escenario, debes incluir la recomendación junto con el archivo que estás iniciando. Para ello, llama al método [**Windows.System.Launcher.launchFileAsync(IStorageFile, LauncherOptions)**](/uwp/api/windows.system.launcher.launchfileasync#Windows_System_Launcher_LaunchFileAsync_Windows_Storage_IStorageFile_Windows_System_LauncherOptions_) con [**LauncherOptions.PreferredApplicationPackageFamilyName**](/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname) establecido con el nombre de familia de paquete correspondiente a la aplicación de la Tienda que quieras recomendar. Después, establece [**LauncherOptions.PreferredApplicationDisplayName**](/uwp/api/windows.system.launcheroptions.preferredapplicationdisplayname) en el nombre de dicha aplicación. Windows usará esta información para reemplazar la opción general de buscar una aplicación por una opción específica para comprar la aplicación recomendada en la Tienda.

> [!NOTE]
> Debe establecer ambas opciones para recomendar una aplicación. Si se establece una, pero la otra no, se producirá un error.

![se inicia el cuadro de diálogo Abrir con de un archivo .contoso dado que .contoso no tiene un controlador instalado en el equipo, el cuadro de diálogo contiene una opción con el icono de la Tienda y un texto que indica al usuario cuál es el controlador correcto de la Tienda. el cuadro de diálogo también contiene un vínculo ‘más opciones’.](images/howdoyouwanttoopen.png)

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.contoso";

   // Get the image file from the package's image directory
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the recommended app
      var options = new Windows.System.LauncherOptions();
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      // Launch the retrieved file pass in the recommended app
      // in case the user has no apps installed to handle the file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```vb
async Sub DefaultLaunch()

   ' Path to the file in the app package to launch
   Dim imageFile = "images\test.contoso"

   ' Get the image file from the package's image directory
   Dim file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile)

   If file IsNot Nothing Then
      ' Set the recommended app
      Dim options = Windows.System.LauncherOptions()
      options.PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
      options.PreferredApplicationDisplayName = "Contoso File App";

      ' Launch the retrieved file pass in the recommended app
      ' in case the user has no apps installed to handle the file
      Dim success = await Windows.System.Launcher.LaunchFileAsync(file)

      If success Then
         ' File launched
      Else
         ' File launch failed
      End If
   Else
      ' Could not find file
   End If
End Sub
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the recommended app
        Windows::System::LauncherOptions launchOptions;
        launchOptions.PreferredApplicationPackageFamilyName(L"Contoso.FileApp_8wknc82po1e");
        launchOptions.PreferredApplicationDisplayName(L"Contoso File App");

        // Launch the retrieved file, and pass in the recommended app
        // in case the user has no apps installed to handle the file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.contoso"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the recommended app
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->PreferredApplicationPackageFamilyName = "Contoso.FileApp_8wknc82po1e";
         launchOptions->PreferredApplicationDisplayName = "Contoso File App";
         
         // Launch the retrieved file pass, and in the recommended app
         // in case the user has no apps installed to handle the file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

### <a name="launch-with-a-desired-remaining-view-windows-only"></a>Inicio con una vista deseada permanente (solo Windows)

Las aplicaciones de origen que llaman a [**LaunchFileAsync**](/uwp/api/windows.system.launcher.launchfileasync) pueden solicitar que permanezcan en pantalla después de iniciarse un archivo. Windows intenta compartir de manera predeterminada todo el espacio disponible entre la aplicación de origen y la aplicación de destino que controla el archivo. Las aplicaciones de origen pueden usar la propiedad [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) para indicar al sistema operativo que prefieren que la ventana de la aplicación ocupe más o menos espacio del que hay disponible. También puedes usar el elemento **DesiredRemainingView** para indicar que la aplicación de origen no necesita permanecer en pantalla después de iniciar un archivo, y que la aplicación de destino puede sustituirla por completo. Esta propiedad especifica únicamente el tamaño de ventana preferido de la aplicación que llama; no especifica el comportamiento de ninguna otra aplicación que también esté en pantalla al mismo tiempo.

> [!NOTE]
> Windows tiene en cuenta varios factores diferentes cuando determina el tamaño de ventana final de la aplicación de origen, por ejemplo, la preferencia de la aplicación de origen, el número de aplicaciones en pantalla, la orientación de la pantalla, etc. Aunque establezcas la propiedad [**DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview), no se garantiza un comportamiento de ventanas específico para la aplicación de origen.

**Familia de dispositivos móviles:  **[**LauncherOptions. DesiredRemainingView**](/uwp/api/windows.system.launcheroptions.desiredremainingview) no se admite en la familia de dispositivos móviles.

```csharp
async void DefaultLaunch()
{
   // Path to the file in the app package to launch
   string imageFile = @"images\test.png";
   
   var file = await Windows.ApplicationModel.Package.Current.InstalledLocation.GetFileAsync(imageFile);

   if (file != null)
   {
      // Set the desired remaining view
      var options = new Windows.System.LauncherOptions();
      options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

      // Launch the retrieved file
      bool success = await Windows.System.Launcher.LaunchFileAsync(file, options);
      if (success)
      {
         // File launched
      }
      else
      {
         // File launch failed
      }
   }
   else
   {
      // Could not find file
   }
}
```

```cppwinrt
Windows::Foundation::IAsyncAction MainPage::DefaultLaunch()
{
    auto installFolder{ Windows::ApplicationModel::Package::Current().InstalledLocation() };

    Windows::Storage::StorageFile file{ co_await installFolder.GetFileAsync(L"images\\test.png") };

    if (file)
    {
        // Set the desired remaining view.
        Windows::System::LauncherOptions launchOptions;
        launchOptions.DesiredRemainingView(Windows::UI::ViewManagement::ViewSizePreference::UseLess);

        // Launch the retrieved file.
        bool success = co_await Windows::System::Launcher::LaunchFileAsync(file, launchOptions);
        if (success)
        {
            // File launched
        }
        else
        {
            // File launch failed
        }
    }
    else
    {
        // Could not find file
    }
}
```

```cpp
void MainPage::DefaultLaunch()
{
   auto installFolder = Windows::ApplicationModel::Package::Current->InstalledLocation;

   concurrency::task<Windows::Storage::StorageFile^> getFileOperation(installFolder->GetFileAsync("images\\test.png"));
   getFileOperation.then([](Windows::Storage::StorageFile^ file)
   {
      if (file != nullptr)
      {
         // Set the desired remaining view.
         auto launchOptions = ref new Windows::System::LauncherOptions();
         launchOptions->DesiredRemainingView = Windows::UI::ViewManagement::ViewSizePreference::UseLess;

         // Launch the retrieved file.
         concurrency::task<bool> launchFileOperation(Windows::System::Launcher::LaunchFileAsync(file, launchOptions));
         launchFileOperation.then([](bool success)
         {
            if (success)
            {
               // File launched
            }
            else
            {
               // File launch failed
            }
         });
      }
      else
      {
         // Could not find file
      }
   });
}
```

## <a name="remarks"></a>Observaciones

La aplicación no puede seleccionar qué aplicación se inicia, sino que es el usuario quien la determina. El usuario puede seleccionar una aplicación para la Plataforma universal de Windows (UWP) o una aplicación de escritorio de Windows.

Cuando inicies un archivo, tu aplicación debe ser la aplicación en primer plano, es decir, visible para el usuario. Este requisito permite asegurar que el usuario permanezca en control. Para cumplir este requisito, asegúrate de enlazar todos los inicios de archivo directamente a la interfaz de usuario de la aplicación. Lo más probable es que el usuario tenga que realizar siempre alguna acción para iniciar el archivo.

No puedes iniciar tipos de archivo que contengan código o script, como archivos .exe, .msi o .js, si el sistema operativo los ejecuta automáticamente. Esta restricción protege a los usuarios contra archivos potencialmente malintencionados que podrían modificar el sistema. Puedes usar este método para iniciar tipos de archivo, como archivos .docx, que pueden contener script, si los ejecuta una aplicación que aísla el script. Las aplicaciones como Microsoft Word impiden que el script de los archivos .docx modifique el sistema.

Si intentas iniciar un tipo de archivo restringido, se producirá un error en el inicio y se invocará a la devolución de llamada de error. Si tu aplicación controla una gran cantidad de tipos de archivo y esperas que este error pueda detectarse, conviene que proveas al usuario de una experiencia de reserva. Por ejemplo, puedes ofrecerle la posibilidad de guardar el archivo en el escritorio para poder abrirlo desde allí.

## <a name="related-topics"></a>Temas relacionados

### <a name="tasks"></a>Tareas

* [Iniciar la aplicación predeterminada para un URI](launch-default-app.md)
* [Administrar la activación de archivos](handle-file-activation.md)

### <a name="guidelines"></a>Directrices

* [Directrices sobre tipos de archivo y URI](../files/index.md)

### <a name="reference"></a>Referencia

* [**Windows.Storage.StorageFile**](/uwp/api/Windows.Storage.StorageFile)
* [**Windows.System.Launcher.LaunchFileAsync**](/uwp/api/windows.system.launcher.launchfileasync)