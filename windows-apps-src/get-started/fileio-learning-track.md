---
author: TylerMSFT
title: Trabajar con archivos
description: Aprende a trabajar con archivos en la Plataforma universal de Windows.
ms.author: twhitney
ms.date: 05/01/2018
ms.topic: article
keywords: introducción, uwp, windows 10, pista de aprendizaje, archivos, e/s de archivos, leer archivo, escribir archivo, crear archivo, escribir texto, leer texto
ms.localizationpriority: medium
ms.openlocfilehash: 02ac4d3f157343182097e67dc8825dde940302c3
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6666301"
---
# <a name="work-with-files"></a>Trabajar con archivos

Este tema aborda lo que necesitas saber para comenzar a leer y escribir en archivos en una aplicación de la Plataforma universal de Windows (UWP). Se presentan las API y los tipos principales, y se proporcionan enlaces que te ayudarán a obtener más información.

Este no es un tutorial. Si quieres ver un tutorial, consulta [Crear, escribir y leer un archivo](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) que, además de mostrar cómo crear, leer y escribir un archivo, muestra cómo usar los búferes y los flujos. Puede que estés interesado también en el [Ejemplo de acceso a archivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) que muestra cómo crear, leer, escribir, copiar y eliminar un archivo, así como cómo recuperar propiedades de archivo y recuerda un archivo o carpeta para que la aplicación pueda tener acceso a ellos nuevamente de forma fácil.

Examinaremos el código para escribir y leer texto desde un archivo y cómo acceder a las carpetas locales, de roaming y temporales de la aplicación.

## <a name="what-do-you-need-to-know"></a>Qué debes saber

Estos son los tipos principales que necesitas conocer para leer o escribir texto desde/en un archivo:

- [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) representa un archivo. Esta clase tiene propiedades que proporcionan información sobre el archivo y los métodos para crear, abrir, copiar, eliminar y cambiar el nombre de los archivos.
Puede usarse para tratar con rutas de acceso de cadenas. Hay algunas API de UWP que toman una ruta de acceso de cadena, pero con mayor frecuencia utilizarás un **StorageFile** para representar un archivo porque algunos archivos con los que trabajes en UWP es posible que no tengan una ruta de acceso, o puede que tengan una ruta de acceso difícil de usar. Usa [StorageFile.GetFileFromPathAsync()](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) para convertir una ruta de acceso de cadena en un **StorageFile**. 

- La clase [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) proporciona una manera fácil de leer y escribir texto. Esta clase también puede leer/escribir una matriz de bytes o el contenido de un búfer. Esta clase es muy similar a la clase [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio). La diferencia principal es que, en vez de tomar una ruta de acceso de cadena, como **PathIO**, toma un **StorageFile**.
- [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) representa una carpeta (directorio). Esta clase tiene métodos para crear archivos, consultar el contenido de una carpeta, crear, cambiar el nombre y eliminar carpetas y propiedades que proporcionan información sobre una carpeta. 

Formas habituales de obtener una **StorageFolder** incluyen:

- [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) que permite al usuario navegar a la carpeta que quiera usar.
- [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) que proporciona la **StorageFolder** específica para una de las carpetas locales para la aplicación como la carpeta local, de roaming y temporal.
- [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders) que proporciona la **StorageFolder** para bibliotecas conocidas como las bibliotecas de música o imágenes.

## <a name="write-text-to-a-file"></a>Escribir texto en un archivo

 En esta introducción, nos centraremos en un escenario simple: leer y escribir texto. Empecemos echando un vistazo a parte del código que usa la clase **FileIO** para escribir texto en un archivo.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.CreateFileAsync("test.txt",
        Windows.Storage.CreationCollisionOption.OpenIfExists);

await Windows.Storage.FileIO.WriteTextAsync(file, "Example of writing a string\r\n");

// Append a list of strings, one per line, to the file
var listOfStrings = new List<string> { "line1", "line2", "line3" };
await Windows.Storage.FileIO.AppendLinesAsync(file, listOfStrings); // each entry in the list is written to the file on its own line.
```

En primer lugar, identificamos dónde debe ubicarse el archivo. `Windows.Storage.ApplicationData.Current.LocalFolder` proporciona acceso a la carpeta local de datos, que se crea para tu aplicación cuando se instala. Consulta [Acceder al sistema de archivo](#access-the-file-system) para obtener más información acerca de las carpetas a las que la aplicación puede acceder.

A continuación, usamos **StorageFolder** para crear el archivo (o abrirlo si ya existe).

La clase **FileIO** proporciona una manera cómoda para escribir texto en el archivo. `FileIO.WriteTextAsync()` sustituye todo el contenido del archivo por el texto proporcionado. `FileIO.AppendLinesAsync()` anexa una colección de cadenas al archivo, escribiendo una cadena por línea.

## <a name="read-text-from-a-file"></a>Leer texto desde un archivo

Al igual que al escribir archivos, leer un archivo comienza especificando dónde se encuentra el archivo. Usaremos la misma ubicación que en el ejemplo anterior. A continuación, usaremos la clase **FileIO** para leer su contenido.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.GetFileAsync("test.txt");

string text = await Windows.Storage.FileIO.ReadTextAsync(file);
```

También puedes leer cada línea del archivo en cadenas individuales de una colección con `IList<string> contents = await Windows.Storage.FileIO.ReadLinesAsync(sampleFile);`

## <a name="access-the-file-system"></a>Acceso al sistema de archivos

En la plataforma UWP, el acceso a la carpeta está restringido para garantizar la integridad y la privacidad de los datos del usuario. 

### <a name="app-folders"></a>Carpetas de la aplicación

Cuando se instala una aplicación para UWP, se crean varias carpetas en c:\users\<nombre usuario>\AppData\Local\Packages\<identificador paquete aplicación>\ para almacenar, entre otras cosas, los archivos locales, de roaming y temporales de la aplicación. La aplicación no tiene que declarar las funcionalidades para acceder a estas carpetas, y estas carpetas no están accesibles para otras aplicaciones. Estas carpetas también se eliminarán al desinstalar la aplicación.

Estas son algunas de las carpetas de la aplicación que normalmente se utilizan:

- **LocalState**: para los datos locales para el dispositivo actual. Cuando se hace una copia de seguridad del dispositivo, los datos de este directorio se guardan en una imagen de copia de seguridad en OneDrive. Si el usuario restablece o reemplaza el dispositivo, se restaurarán los datos. Accede a esta carpeta con `Windows.Storage.ApplicationData.Current.LocalFolder.` Guarda los datos locales que no deseas en la copia de seguridad de OneDrive en la **LocalCacheFolder**, a la que puedes acceder con `Windows.Storage.ApplicationData.Current.LocalCacheFolder`.

- **RoamingState**: para los datos que se deben replicar en todos los dispositivos donde esté instalada la aplicación. Windows limita la cantidad de datos de roaming; por lo tanto, guarda aquí solo la configuración de usuario y pequeños archivos. Accede a la carpeta de roaming con `Windows.Storage.ApplicationData.Current.RoamingFolder`.

- **TempState**: para los datos que puedan eliminarse cuando se esté ejecutando la aplicación. Accede a esta carpeta con `Windows.Storage.ApplicationData.Current.TemporaryFolder`.

### <a name="access-the-rest-of-the-file-system"></a>Acceder al resto del sistema de archivos

Una aplicación para UWP debe declarar su propósito para acceder a una biblioteca de usuario específica agregando a su manifiesto la capacidad correspondiente. A continuación, se pregunta al usuario la fecha de instalación de aplicación para comprobar que autorizan el acceso a la biblioteca especificada. En caso contrario, la aplicación no está instalada. Hay funcionalidades para acceder a las imágenes, vídeos y bibliotecas de música. Consulta [Declaración de funcionalidades de la aplicación](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) para obtener la lista completa. Para obtener una **StorageFolder** para estas bibliotecas, usa la clase [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders).

#### <a name="documents-library"></a>Biblioteca de documentos

Aunque hay una funcionalidad para acceder a la biblioteca de documentos del usuario, esa funcionalidad está restringida lo que significa que una aplicación que la declara será rechazada por Microsoft Store, a menos que sigas un proceso para obtener aprobación especial. No está prevista para uso general. En su lugar, usa el archivo o los selectores de archivo (consulta [Abrir archivos y carpetas con un selector](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) y [Guardar un archivo con un selector](https://docs.microsoft.com/windows/uwp/files/quickstart-save-a-file-with-a-picker)) que permiten al usuario navegar a la carpeta o archivo. Cuando el usuario navega a una carpeta o archivo, implícitamente han dado permiso de acceso a la aplicación y el sistema permite el acceso.

#### <a name="general-access"></a>Acceso general

Como alternativa, la aplicación puede declarar la funcionalidad [broadFileSystem](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) restringida en su manifiesto, lo que también requiere la aprobación de Microsoft Store. A continuación, la aplicación puede acceder a cualquier archivo al que el usuario tenga acceso sin la intervención de un archivo o selector de archivo.

Para obtener una lista completa de las ubicaciones a las que las aplicaciones pueden tener acceso, consulta [Permisos de acceso de archivo](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).

## <a name="useful-apis-and-docs"></a>API y documentos de utilidad

Este es un resumen rápido de las API y otra documentación de utilidad que te ayudará a comenzar con archivos y carpetas.

### <a name="useful-apis"></a>API útiles

| API | Descripción |
|------|---------------|
|  [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) | Proporciona información sobre el archivo y los métodos para crear, abrir, copiar, eliminar y cambiar el nombre de los archivos. |
| [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) | Proporciona información sobre la carpeta, métodos para crear archivos y métodos para crear, abrir, eliminar y cambiar el nombre de los archivos. |
| [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) |  Proporciona una manera fácil de leer y escribir texto. Esta clase también puede leer/escribir una matriz de bytes o el contenido de un búfer. |
| [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) | Proporciona una manera sencilla de leer/escribir texto desde/a o un archivo después de haber facilitado la ruta de acceso de cadena del archivo. Esta clase también puede leer/escribir una matriz de bytes o el contenido de un búfer. |
| [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) & [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) |  Leer y escribir búferes, bytes, enteros, GUID, TimeSpan y más desde/hacia una secuencia. |
| [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) | Proporciona acceso a las carpetas creadas para la aplicación, como la carpeta local, la carpeta de roaming y la carpeta de archivos temporales. |
| [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) |  Permite al usuario elegir una carpeta y devuelve una **StorageFolder** para ella. Esto es cómo obtener acceso a las ubicaciones a las que la aplicación no puede acceder de forma predeterminada. |
| [Windows.Storage.Pickers.FileOpenPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker) | Permite al usuario elegir un archivo para abrirlo y devuelve un **StorageFile** para él. Esto es cómo obtener acceso a un archivo al que la aplicación no puede acceder de forma predeterminada. |
| [Windows.Storage.Pickers.FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker) | Permite al usuario elegir el nombre de archivo, la extensión y la ubicación de almacenamiento de un archivo. Devuelve un **StorageFile **. Esto es cómo guardar un archivo en una ubicación a la que la aplicación no puede acceder de forma predeterminada. |
|  [Espacio de nombres Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Cubre las secuencias de lectura y escritura. En especial, fíjate en las clases [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) y [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) que leen y escriben búferes, bytes, enteros, GUID, TimeSpan y mucho más. |

### <a name="useful-docs"></a>Documentos útiles

| Tema | Descripción |
|-------|----------------|
| [Espacio de nombres Windows.Storage](https://docs.microsoft.com/uwp/api/windows.storage) | Documentos de referencia de API. |
| [Archivos, carpetas y bibliotecas](https://docs.microsoft.com/windows/uwp/files/) | Documentos conceptuales. |
| [Crear, escribir y leer archivos](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) | Cubre la creación, lectura y escritura de texto, datos binarios y secuencias. |
| [Introducción sobre almacenamiento local de datos de la aplicación](https://blogs.windows.com/buildingapps/2016/05/10/getting-started-storing-app-data-locally/#pCbJKGjcShh5DTV5.97) | Además de cubrir los procedimientos recomendados para guardar los datos locales, aborda el propósito de la carpeta LocalSettings y LocalCache. |
| [Introducción a datos de aplicación de roaming](https://blogs.windows.com/buildingapps/2016/05/03/getting-started-with-roaming-app-data/#RgjgLt5OkU9DbVV8.97) | Una serie de dos partes sobre cómo usar los datos de aplicaciones de roaming. |
| [Directrices para perfiles móviles de datos de la aplicación](http://msdn.microsoft.com/library/windows/apps/hh465094) | Sigue estas directrices de datos de roaming cuando diseñes tu aplicación. |
| [Almacenar y recuperar la configuración y otros datos de aplicación](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | Proporciona una visión general de varios almacenes de datos de aplicación, como las carpetas locales, de roaming y temporales. Consulta la sección [Datos de roaming](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#roaming-data) para obtener directrices e información adicional acerca de la escritura de datos que se transfieren entre dispositivos. |
| [Permisos de acceso de archivos](https://docs.microsoft.com/windows/uwp/files/file-access-permissions) | Información sobre las ubicaciones del sistema de archivo a las que puede acceder tu aplicación. |
| [Abrir archivos y carpetas con un selector](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) | Muestra cómo acceder a archivos y carpetas, permite al usuario decidir a través de una interfaz de usuario de selector. |
| [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Tipos usados para leer y escribir secuencias. |
| [Archivos y carpetas de las bibliotecas de música, imágenes y vídeos](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries) | Aborda cómo quitar carpetas de bibliotecas y obtener la lista de carpetas de una biblioteca para detectar archivos de vídeos, música y fotos almacenados. |

## <a name="useful-code-samples"></a>Muestras de código útiles

| Ejemplo de código | Descripción |
|-----------------|---------------|
| [Ejemplo de los datos de la aplicación](https://code.msdn.microsoft.com/windowsapps/ApplicationData-sample-fb043eb2) | Muestra cómo almacenar y recuperar datos que son específicos de cada usuario utilizando las API de los datos de la aplicación. |
| [Muestra de acceso a archivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) | Muestra cómo crear, leer, escribir, copiar y eliminar un archivo. |
| [Muestra de selector de archivos](http://code.msdn.microsoft.com/windowsapps/File-picker-sample-9f294cba) | Muestra cómo acceder a archivos y carpetas, permitiéndole al usuario elegirlos con la interfaz de usuario y cómo guardar un archivo para que el usuario pueda especificar el nombre, el tipo de archivo y la ubicación de un archivo para guardar. |
| [Ejemplo de JSON](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Json) | Muestra cómo codificar y descodificar objetos de notación de objetos JavaScript (JSON), matrices, cadenas, números y booleanos mediante el [Espacio de nombres Windows.Data.Json](https://docs.microsoft.com/uwp/api/Windows.Data.Json). |
| [Ejemplos de código adicionales](https://developer.microsoft.com//windows/samples) | Elige **Archivos, carpeta y bibliotecas** en la lista desplegable de categorías. |