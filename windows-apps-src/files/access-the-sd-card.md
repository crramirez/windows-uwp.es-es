---
ms.assetid: CAC6A7C7-3348-4EC4-8327-D47EB6E0C238
title: Acceso a la tarjeta SD
description: Puedes almacenar datos no esenciales y tener acceso a ellos en una tarjeta microSD opcional, especialmente en los dispositivos móviles de bajo coste que tienen un almacenamiento interno limitado.
ms.date: 03/08/2017
ms.topic: article
keywords: windows 10, uwp, tarjeta sd, almacenamiento
ms.localizationpriority: medium
ms.openlocfilehash: 4573e0959cf9d4af9b3cef8ffbbce14847a9e521
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369495"
---
# <a name="access-the-sd-card"></a>Acceso a la tarjeta SD



Puedes almacenar datos no esenciales y tener acceso a ellos en una tarjeta microSD, especialmente en los dispositivos móviles de bajo costo que tienen un almacenamiento interno limitado y cuentan con una ranura para una tarjeta SD.

En la mayoría de casos, debes especificar la funcionalidad **removableStorage** en el archivo de manifiesto de la aplicación para que esta pueda almacenar archivos en la tarjeta SD y pueda tener acceso a ellos. Por lo general, también tienes que realizar el registro para administrar el tipo de archivos que tu aplicación almacena y a los que tu aplicación tiene acceso.

Puedes almacenar archivos en la tarjeta SD opcional y tener acceso a ellos mediante los métodos siguientes:
- Selectores de archivos.
- Las API de [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage).

## <a name="what-you-can-and-cant-access-on-the-sd-card"></a>Elementos a los que puedes acceder y elementos a los que no puedes acceder en la tarjeta SD

### <a name="what-you-can-access"></a>Elementos a los que puedes acceder

- Tu aplicación únicamente puede leer y escribir en archivos de los tipos de archivo para cuya administración se haya registrado la aplicación en el archivo de manifiesto de la aplicación.
- Tu aplicación también puede crear y administrar carpetas.

### <a name="what-you-cant-access"></a>Elementos a los que no puedes acceder

- Tu aplicación no puede ver carpetas del sistema ni puede acceder a ellas ni a los archivos que contienen.
- Tu aplicación no puede ver archivos marcados con el atributo Oculto. El atributo Hidden normalmente se usa para reducir el riesgo de eliminación accidental de datos.
- Tu aplicación no puede ver la biblioteca de documentos ni acceder a ella mediante [**KnownFolders.DocumentsLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.documentslibrary). Sin embargo, puedes acceder a la biblioteca de documentos de la tarjeta SD recorriendo el sistema de archivos.

## <a name="security-and-privacy-considerations"></a>Consideraciones de seguridad y privacidad

Cuando una aplicación guarda archivos en una ubicación global de la tarjeta SD, estos archivos no se cifran y normalmente otras aplicaciones pueden tener acceso a ellos.

- Mientras la tarjeta SD esté en el dispositivo, otras aplicaciones que se hayan registrado para administrar los mismos tipos de archivo pueden acceder a tus archivos.
- Cuando la tarjeta SD se extrae del dispositivo y se abre en tu PC, los archivos que contiene pueden verse en el Explorador de archivos y otras aplicaciones pueden acceder a ellos.

Sin embargo, cuando una aplicación instalada en la tarjeta SD guarda archivos en su [**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder), estos archivos se cifran y otras aplicaciones no pueden tener acceso a ellos.

## <a name="requirements-for-accessing-files-on-the-sd-card"></a>Requisitos para tener acceso a archivos en la tarjeta SD

Por lo general, para acceder a los archivos de la tarjeta SD, tienes que especificar lo siguiente:

1.  Debes especificar la funcionalidad **removableStorage** en el archivo de manifiesto de la aplicación.
2.  También debes registrarte para controlar las extensiones de archivo asociadas al tipo de elemento multimedia al que quieres acceder.

Usa el método anterior para tener acceso, también, a archivos multimedia de la tarjeta SD sin necesidad de hacer referencia a una carpeta conocida, como **KnownFolders.MusicLibrary** o para tener acceso a archivos multimedia que estén almacenados fuera de las carpetas de la biblioteca multimedia.

Para tener acceso a los archivos multimedia de las bibliotecas multimedia (Música, Fotos o Vídeos) mediante el uso de carpetas conocidas, solo tienes que especificar la funcionalidad asociada en el archivo de manifiesto de la aplicación: **musicLibrary**, **picturesLibrary** o **videoLibrary**. No es necesario especificar la funcionalidad **removableStorage**. Para obtener más información, consulta [Archivos y carpetas en las bibliotecas de música, imágenes y vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md).

## <a name="accessing-files-on-the-sd-card"></a>Acceso a los archivos de la tarjeta SD

### <a name="getting-a-reference-to-the-sd-card"></a>Obtención de una referencia a la tarjeta SD

La carpeta [**KnownFolders.RemovableDevices**](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.removabledevices) es la clase [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) de raíz lógica para el conjunto de dispositivos extraíbles actualmente conectados al dispositivo. Si está presente una tarjeta SD, la primera (y única) clase **StorageFolder** en la carpeta **KnownFolders.RemovableDevices** representa la tarjeta SD.

Usa código como el siguiente para determinar la presencia de una tarjeta SD y para obtener una referencia a ella como [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder).

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    // An SD card is present and the sdCard variable now contains a reference to it.
}
else
{
    // No SD card is present.
}
```

> [!NOTE]
> Si el lector de tarjetas SD es un lector integrado (por ejemplo, una ranura en tu portátil o PC), puede que no sea accesible a través de KnownFolders.RemovableDevices.

### <a name="querying-the-contents-of-the-sd-card"></a>Consulta del contenido de la tarjeta SD

La tarjeta SD puede contener muchas carpetas y archivos que no se reconocen como carpetas conocidas y que no se pueden consultar mediante el uso de una ubicación de [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders). Para buscar archivos, tu aplicación debe enumerar el contenido de la tarjeta recorriendo el sistema de archivos de forma recursiva. Usa [**GetFilesAsync (CommonFileQuery.DefaultQuery)** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) y [**GetFoldersAsync (CommonFolderQuery.DefaultQuery)** ](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfoldersasync) para obtener el contenido de la tarjeta SD de un modo eficaz.

Te recomendamos que uses un subproceso en segundo plano para recorrer la tarjeta SD. Una tarjeta SD puede contener varios gigabytes de datos.

Tu aplicación también solicitará al usuario que elija carpetas específicas mediante el selector de carpetas.

Si accedes al sistema de archivos de la tarjeta SD con una ruta de acceso que derivaste de [**KnownFolders.RemovableDevices**](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.removabledevices), los métodos que figuran a continuación se comportan de la siguiente manera.

-   El método [**GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) devuelve la unión de las extensiones de archivo que hayas registrado para controlar y las extensiones de archivo asociadas a las funcionalidades de biblioteca multimedia que hayas especificado.
-   El método [**GetFileFromPathAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) produce un error si no realizaste el registro necesario para controlar la extensión de archivo que tiene el archivo al que estás intentando acceder.

## <a name="identifying-the-individual-sd-card"></a>Identificación de la tarjeta SD individual

Cuando la tarjeta SD se monta por primera vez, el sistema operativo genera un identificador único para la tarjeta. Almacena este id. en un archivo de la carpeta WPSystem, en la raíz de la tarjeta. Una aplicación puede utilizar este id. para determinar si reconoce la tarjeta. Si una aplicación reconoce la tarjeta, la aplicación puede ser capaz de posponer determinadas operaciones que se completaron anteriormente. Sin embargo, el contenido de la tarjeta puede haber cambiado desde que la aplicación accedió a la tarjeta por última vez.

Por ejemplo, piensa en una aplicación que indiza libros electrónicos. Si la aplicación ha analizado anteriormente la presencia de archivos de libros electrónicos en toda la tarjeta SD y ha creado un índice de los libros electrónicos, puede mostrar la lista inmediatamente si vuelve a insertarse la tarjeta y la aplicación reconoce la tarjeta. Por separado, puede iniciar un subproceso de en segundo plano de baja prioridad para buscar nuevos libros electrónicos. También puede controlar un error de búsqueda de un libro electrónico que existía anteriormente cuando el usuario intenta acceder al libro electrónico eliminado.

El nombre de la propiedad que contiene este identificador es **WindowsPhone.ExternalStorageId**.

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    var allProperties = sdCard.Properties;
    IEnumerable<string> propertiesToRetrieve = new List<string> { "WindowsPhone.ExternalStorageId" };

    var storageIdProperties = await allProperties.RetrievePropertiesAsync(propertiesToRetrieve);

    string cardId = (string)storageIdProperties["WindowsPhone.ExternalStorageId"];

    if (...) // If cardID matches the cached ID of a recognized card.
    {
        // Card is recognized. Index contents opportunistically.
    }
    else
    {
        // Card is not recognized. Index contents immediately.
    }
}
```

 

 
