---
author: laurenhughes
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: Permisos de acceso de archivos
description: Las aplicaciones pueden obtener acceso a determinadas ubicaciones del sistema de archivos de manera predeterminada. Asimismo, también pueden tener acceso a otras ubicaciones mediante el selector de archivos o declarando funcionalidades.
ms.author: lahugh
ms.date: 06/28/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8699ee06da545e3b34711f496a887fd7aa2c935
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6986898"
---
# <a name="file-access-permissions"></a>Permisos de acceso de archivos

Las aplicaciones universales de Windows pueden obtener acceso a determinadas ubicaciones del sistema de archivos de manera predeterminada. Las aplicaciones también pueden tener acceso a otras ubicaciones mediante el selector de archivos o declarando funcionalidades.

## <a name="the-locations-that-all-apps-can-access"></a>Ubicaciones a las que pueden tener acceso todas las aplicaciones

Al crear una aplicación nueva, puedes obtener acceso a las siguientes ubicaciones del sistema de archivos de manera predeterminada:

### <a name="application-install-directory"></a>Directorio de instalación de la aplicación
La carpeta donde está instalada la aplicación en el sistema del usuario.

Hay dos formas principales para acceder a los archivos y carpetas en tu aplicación directorio de instalación:

1. Puedes recuperar una clase [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que represente el directorio de instalación de la aplicación como, por ejemplo:

```csharp
Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
```

```javascript
var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
```

```cppwinrt
#include <winrt/Windows.Storage.h>
...
Windows::Storage::StorageFolder installedLocation{ Windows::ApplicationModel::Package::Current().InstalledLocation() };
```

```cpp
Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
```

Entonces podrás tener acceso a los archivos y carpetas del directorio mediante los métodos de la clase [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230). En el ejemplo, **StorageFolder** se almacena en la variable `installDirectory`. En la [muestra de información del paquete de la aplicación](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package) de GitHub, puedes obtener más información sobre cómo trabajar con el paquete de la aplicación y el directorio de instalación.

2. Puedes recuperar un archivo directamente del directorio de instalación de la aplicación mediante un URI de aplicación como:

```cs
using Windows.Storage;            
StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
```

```javascript
Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
    function(file) {
        // Process file
    }
);
```

```cppwinrt
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    Windows::Storage::StorageFile file{
        co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{L"ms-appx:///file.txt"})
    };
    // Process file
}
```

```cpp
auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
getFileTask.then([](StorageFile^ file) 
{
    // Process file
});
```

Cuando finalice el método [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741), devolverá una clase [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa al archivo `file.txt` en el directorio de instalación de la aplicación (`file`, en el ejemplo).

El prefijo "ms-appx:///" del URI hace referencia al directorio de instalación de la aplicación. Puedes aprender más sobre cómo usar el URI de aplicación en [How to use URIs to reference content (Cómo usar los URI para hacer referencia al contenido)](https://msdn.microsoft.com/library/windows/apps/hh781215).

Además, a diferencia de lo que sucede con otras ubicaciones, también puedes tener acceso a los archivos del directorio de instalación de la aplicación si usas las API [Win32 y COM para las aplicaciones de la Plataforma universal de Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/br205757) y algunas de las [funciones de la biblioteca estándar de C/C++ de Microsoft Visual Studio](http://msdn.microsoft.com/library/hh875057.aspx).

El directorio de instalación de la aplicación es una ubicación de solo lectura. No se puede obtener acceso al directorio de instalación mediante el selector de archivos.

### <a name="application-data-locations"></a>Ubicaciones de datos de aplicación
Son las carpetas en las que la aplicación puede almacenar datos. Estas carpetas (locales, móviles o temporales) se crean al instalar la aplicación.

Hay dos formas principales para tener acceso a archivos y carpetas desde ubicaciones de datos de la aplicación:

1.  Usa las propiedades de [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587) para recuperar una carpeta de datos de la aplicación.

Por ejemplo, puedes usar [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) para recuperar una clase [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que represente la carpeta local de la aplicación de esta manera:

```cs
using Windows.Storage;
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
```

```javascript
var localFolder = Windows.Storage.ApplicationData.current.localFolder;
```

```cppwinrt
Windows::Storage::StorageFolder storageFolder{
    Windows::Storage::ApplicationData::Current().LocalFolder()
};
```

```cpp
using namespace Windows::Storage;
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
```

Si quieres obtener acceso a la carpeta móvil o temporal de la aplicación, usa la propiedad [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) o, en su lugar, [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629).

Tras recuperar una clase [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que represente una ubicación de datos de la aplicación, podrás obtener acceso a los archivos y carpetas del directorio mediante los métodos de **StorageFolder**. En el ejemplo, estos objetos **StorageFolder** se almacenan en la variable `localFolder`. Puedes obtener más información sobre cómo usar las ubicaciones de datos de la aplicación en la ayuda de la página [ApplicationData class](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata) (Clase ApplicationData) y si descargas la [muestra de datos de aplicación](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) de GitHub.

2. Por ejemplo, puedes recuperar un archivo directamente de la carpeta local de la aplicación mediante un URI de aplicación, tal como se muestra a continuación:

```cs
using Windows.Storage;
StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appdata:///local/file.txt"));
```

```javascript
Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
    function(file) {
        // Process file
    }
);
```

```cppwinrt
Windows::Storage::StorageFile file{
    co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{ L"ms-appdata:///local/file.txt" })
};
// Process file
```

```cpp
using Windows::Storage;
auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
getFileTask.then([](StorageFile^ file) 
{
    // Process file
});
```

Cuando finalice el método [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741), devolverá una clase [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa al archivo `file.txt` en la carpeta local de la aplicación (`file`, en el ejemplo).

El prefijo "ms-appdata:///local/" del URI hace referencia a la carpeta local de la aplicación. Para obtener acceso a los archivos de las carpetas móvil o temporal de la aplicación, usa "ms-appdata:///roaming/" o "ms-appdata:///temporary/" en su lugar. Puedes obtener más información sobre cómo usar las URI de aplicación en el tema [Cómo cargar recursos de archivos](https://msdn.microsoft.com/library/windows/apps/hh781229).

Asimismo, a diferencia de lo que sucede con otras ubicaciones, también puedes tener acceso a los archivos de las ubicaciones de datos de tu aplicación si usas las API [Win32 y COM para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/br205757) y algunas de las funciones de la biblioteca estándar de C/C++ de Visual Studio.

No se puede tener acceso a las carpetas locales, móviles o temporales mediante el selector de archivos.

### <a name="removable-devices"></a>Dispositivos extraíbles
Igualmente, la aplicación puede obtener acceso a algunos de los archivos en los dispositivos conectados de manera predeterminada. Esto es una alternativa en caso de que la aplicación use la [extensión de reproducción automática](https://msdn.microsoft.com/library/windows/apps/xaml/hh464906.aspx#autoplay) para iniciarse automáticamente cuando los usuarios conecten al sistema un dispositivo, como una cámara o una unidadUSB. Los archivos a los que la aplicación puede acceder se limitan a los tipos de archivo específicos que se definan a través de declaraciones de asociación de tipo de archivo en el manifiesto de la aplicación.

Sobra decir que también puedes tener acceso a los archivos y carpetas de un dispositivo extraíble llamando al selector de archivos (mediante [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) y [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)), y permitiendo que el usuario escoja los archivos y carpetas a los que la aplicación puede tener acceso. En [Abrir archivos y carpetas con un selector](quickstart-using-file-and-folder-pickers.md) encontrarás más información sobre el uso del selector de archivos.

> [!NOTE]
> Para más información sobre el acceso a una tarjeta SD u otros dispositivos extraíbles, consulta [Acceso a la tarjeta SD](access-the-sd-card.md).

## <a name="locations-that-uwp-apps-can-access"></a>Ubicaciones a las que pueden tener acceso las aplicaciones para UWP
### <a name="users-downloads-folder"></a>Carpeta descargas del usuario

Es la carpeta donde se guardan de forma predeterminada los archivos que se descargan.

De manera predeterminada, tu aplicación puede acceder únicamente a los archivos y carpetas en la carpeta Descargas del usuario que la aplicación haya creado. No obstante, también puedes obtener acceso a ellos llamando a un selector de archivos ([**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) o [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)), lo que permite que los usuarios puedan tener acceso a los archivos o carpetas y seleccionarlos para que la aplicación los use.

- Puedes crear un archivo en la carpeta Descargas del usuario del siguiente modo:

```cs
using Windows.Storage;
StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
```

```javascript
Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
    function(newFile) {
        // Process file
    }
);
```

```cppwinrt
Windows::Storage::StorageFile newFile{
    co_await Windows::Storage::DownloadsFolder::CreateFileAsync(L"file.txt")
};
// Process file
```

```cpp
using Windows::Storage;
auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
createFileTask.then([](StorageFile^ newFile)
{
    // Process file
});
```

[**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761) se sobrecarga para que puedas especificar qué debe hacer el sistema en caso de que ya haya un archivo con el mismo nombre en la carpeta Descargas. Cuando estos métodos se completen, devuelven una clase [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) que representa el archivo que se ha creado. Este archivo se llama `newFile` en el ejemplo.

- Puedes crear una subcarpeta en la carpeta Descargas del usuario del siguiente modo:

```cs
using Windows.Storage;
StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
```

```javascript
Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
    function(newFolder) {
        // Process folder
    }
);
```

```cppwinrt
Windows::Storage::StorageFolder newFolder{
    co_await Windows::Storage::DownloadsFolder::CreateFolderAsync(L"New Folder")
};
// Process folder
```

```cpp
using Windows::Storage;
auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
createFolderTask.then([](StorageFolder^ newFolder)
{
    // Process folder
});
```

[**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763) se sobrecarga para que puedas especificar qué debe hacer el sistema en caso de que ya haya una subcarpeta con el mismo nombre en la carpeta Descargas. Cuando estos métodos se completen, devuelven una clase [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) que representa la subcarpeta que se ha creado. Este archivo se llama `newFolder` en el ejemplo.

Si creas un archivo o carpeta en la carpeta Descargas, te recomendamos que agregues dicho elemento a la propiedad [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457), para que así la aplicación pueda acceder a él sin problemas.

## <a name="accessing-additional-locations"></a>Acceder a más ubicaciones

Además de a las ubicaciones predeterminadas, las aplicaciones pueden obtener acceso a archivos y carpetas adicionales mediante la declaración de funcionalidades en el manifiesto de la aplicación (consulta [Declaraciones de funcionalidades de aplicación](https://msdn.microsoft.com/library/windows/apps/mt270968)), o bien mediante la llamada a un selector de archivos para permitir que el usuario elija los archivos y carpetas a los que puede tener acceso la aplicación (consulta [Apertura de archivos y carpetas con un selector](quickstart-using-file-and-folder-pickers.md)).

Las aplicaciones que declaran la extensión [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) tienen permisos del sistema de archivos del directorio desde el que se inician desde la ventana de la consola y hacia abajo.

En la siguiente tabla se incluyen otras ubicaciones a las que puedes tener acceso si declaras una funcionalidad (o varias) y usas la API de [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) correspondiente:

| Ubicación | Funcionalidad | API de Windows.Storage |
|----------|------------|---------------------|
| Todos los archivos a los que tiene acceso el usuario. Por ejemplo: documentos, imágenes, fotos, descargas, escritorio, OneDrive, etc. | broadFileSystemAccess<br><br>Es una funcionalidad restringida. La primera vez que la utilices, el sistema solicitará al usuario que permita el acceso. Se puede configurar el acceso en Configuración > Privacidad > Sistema de archivos. Si envías una aplicación a la Store que declare esta funcionalidad, deberás proporcionar descripciones adicionales sobre por qué tu aplicación necesita esta funcionalidad y cómo pretende usarla.<br>Esta funcionalidad funciona para las API del espacio de nombres [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) | n/d |
| Documentos | DocumentsLibrary <br><br>Nota: debes incluir en el manifiesto de la aplicación aquellas asociaciones de tipo de archivo que declaren los tipos de archivo concretos a los que la aplicación tiene acceso en esta ubicación. <br><br>Usa esta funcionalidad si tu aplicación:<br>- Facilita el acceso sin conexión entre plataformas al contenido específico de OneDrive mediante direcciones URL o id. de recursos de OneDrive válidos.<br>-Guarda los archivos abiertos en el OneDrive del usuario automáticamente y sin conexión | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| Música     | MusicLibrary <br>Consulta también [Archivos y carpetas de las bibliotecas de música, imágenes y vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| Imágenes  | PicturesLibrary<br> Consulta también [Archivos y carpetas de las bibliotecas de música, imágenes y vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| Vídeos    | VideosLibrary<br>Consulta también [Archivos y carpetas de las bibliotecas de música, imágenes y vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md). | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| Dispositivos extraíbles  | RemovableDevices <br><br>Nota: debes incluir en el manifiesto de la aplicación aquellas asociaciones de tipo de archivo que declaren los tipos de archivo concretos a los que la aplicación tiene acceso en esta ubicación. <br><br>Consulta también [Acceso a la tarjeta SD](access-the-sd-card.md). | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| Bibliotecas de Grupo Hogar  | Se necesita al menos una de las siguientes funcionalidades. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| Dispositivos de servidores multimedia (DLNA) | Se necesita al menos una de las siguientes funcionalidades. <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) |
| Carpetas UNC (convención de nomenclatura universal) | Se necesita una combinación de las siguientes funcionalidades. <br><br>Funcionalidad de redes doméstica y de trabajo: <br>- PrivateNetworkClientServer <br><br>Y al menos una funcionalidad de Internet y redes públicas: <br>- InternetClient <br>- InternetClientServer <br><br>Además, si procede, la funcionalidad de credenciales de dominio:<br>- EnterpriseAuthentication <br><br>Nota: debes incluir en el manifiesto de la aplicación aquellas asociaciones de tipo de archivo que declaren los tipos de archivo concretos a los que la aplicación tiene acceso en esta ubicación. | Una carpeta se recupera mediante: <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>Un archivo se recupera mediante: <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |

**Ejemplo**

En este ejemplo, se agrega la funcionalidad restringida `broadFileSystemAccess`. Además de especificar la funcionalidad, se debe agregar el espacio de nombres `rescap` y también se debe agregar a `IgnorableNamespaces`:

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp uap5 rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> Para obtener una lista completa de funcionalidades de la aplicación, consulta [Declaraciones de funcionalidades de las aplicaciones](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).
