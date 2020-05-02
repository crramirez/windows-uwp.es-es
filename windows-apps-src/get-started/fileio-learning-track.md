---
title: Trabajar con archivos
description: Obtén información sobre cómo trabajar con archivos en la Plataforma universal de Windows.
ms.date: 05/01/2018
ms.topic: article
keywords: introducción;uwp;windows 10;pista de aprendizaje;archivos;e/s de archivos;leer archivo;escribir archivo;crear archivo;escribir texto;leer texto;get started;learning track;files;file io;read file;write file;create file;write text;read text
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1270d49cc8746b2793b1414306f9ee842cb40f40
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82166251"
---
# <a name="work-with-files"></a>Trabajar con archivos

Este tema aborda lo que necesitas saber para comenzar a leer y escribir en archivos en una aplicación de la Plataforma universal de Windows (UWP). Se presentan las API y los tipos principales y se proporcionan vínculos que te ayudarán a obtener más información.

Este no es un tutorial. Si quieres ver un tutorial, consulta [Crear, escribir y leer un archivo](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) que, además de mostrar cómo crear, leer y escribir un archivo, muestra cómo usar los búferes y las secuencias. Quizá también te interese el [Ejemplo de acceso a archivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) que muestra cómo crear, leer, escribir, copiar y eliminar un archivo, así como los métodos para recuperar las propiedades de un archivo y recordar un archivo o carpeta para que la aplicación pueda tener acceso a ellos nuevamente de forma fácil.

Examinaremos el código para escribir y leer texto desde un archivo y cómo acceder a las carpetas locales, de itinerancia y temporales de la aplicación.

## <a name="what-do-you-need-to-know"></a>¿Qué debes saber?

Estos son los tipos principales que necesitas conocer para leer o escribir texto desde/en un archivo:

- [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) representa un archivo. Esta clase tiene propiedades que proporcionan información sobre el archivo y los métodos para crear, abrir, copiar, eliminar y cambiar el nombre de los archivos.
Puede usarse para tratar con rutas de acceso de cadenas. Hay algunas API de UWP que toman una ruta de acceso de cadena, pero con mayor frecuencia utilizarás un **StorageFile** para representar a un archivo. Esto se debe a que algunos de los archivos con los trabajarás en UWP pueden no tener una ruta de acceso, o puede que sea una ruta de acceso difícil de usar. Usa [StorageFile.GetFileFromPathAsync()](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) para convertir una ruta de acceso de cadena en un **StorageFile**. 

- La clase [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) proporciona una manera fácil de leer y escribir texto. Esta clase también puede leer/escribir una matriz de bytes o el contenido de un búfer. Esta clase es muy similar a la clase [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio). La diferencia principal es que, en vez de tomar una ruta de acceso de cadena (como **PathIO**) toma un **StorageFile**.
- [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) representa una carpeta (directorio). Esta clase tiene métodos para crear archivos, consultar el contenido de una carpeta o para crear, cambiar el nombre de las carpetas y eliminarlas. Además, tiene propiedades que proporcionan información sobre una carpeta. 

Entre las formas habituales de obtener un **StorageFolder** se incluyen:

- [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker), que permite al usuario navegar a la carpeta que quiera usar.
- [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) que proporciona la **StorageFolder** específica para alguna de las carpetas locales de la aplicación, como las carpetas local, de itinerancia y temporal.
- [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders) que proporciona la **StorageFolder** para bibliotecas conocidas como las bibliotecas de música o imágenes.

## <a name="write-text-to-a-file"></a>Escribir texto en un archivo

 En esta introducción, nos centraremos en un escenario simple: leer y escribir texto. Empecemos echando un vistazo a la parte del código que usa la clase **FileIO** para escribir texto en un archivo.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.CreateFileAsync("test.txt",
        Windows.Storage.CreationCollisionOption.OpenIfExists);

await Windows.Storage.FileIO.WriteTextAsync(file, "Example of writing a string\r\n");

// Append a list of strings, one per line, to the file
var listOfStrings = new List<string> { "line1", "line2", "line3" };
await Windows.Storage.FileIO.AppendLinesAsync(file, listOfStrings); // each entry in the list is written to the file on its own line.
```

En primer lugar, identificamos dónde debe ubicarse el archivo. `Windows.Storage.ApplicationData.Current.LocalFolder` proporciona acceso a la carpeta local de datos, que se crea para tu aplicación cuando se instala. Consulta [Acceder al sistema de archivos](#access-the-file-system) para obtener más información acerca de las carpetas a las que la aplicación puede acceder.

A continuación, usamos **StorageFolder** para crear el archivo (o abrirlo si ya existe).

La clase **FileIO** proporciona una forma práctica de escribir texto en el archivo. `FileIO.WriteTextAsync()` sustituye todo el contenido del archivo por el texto proporcionado. `FileIO.AppendLinesAsync()` anexa una colección de cadenas al archivo, escribiendo una cadena por línea.

## <a name="read-text-from-a-file"></a>Leer texto desde un archivo

Al igual que al escribir archivos, el primer paso para leer un archivo es especificar dónde se encuentra. Usaremos la misma ubicación que en el ejemplo anterior. A continuación, usaremos la clase **FileIO** para leer su contenido.

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.GetFileAsync("test.txt");

string text = await Windows.Storage.FileIO.ReadTextAsync(file);
```

También puedes leer cada línea del archivo en cadenas individuales de una colección con `IList<string> contents = await Windows.Storage.FileIO.ReadLinesAsync(sampleFile);`.

## <a name="access-the-file-system"></a>Acceder al sistema de archivos

En la plataforma UWP, el acceso a la carpeta está restringido para garantizar la integridad y la privacidad de los datos del usuario. 

### <a name="app-folders"></a>Carpetas de la aplicación

Cuando se instala una aplicación para UWP, se crean varias carpetas en c:\users\\<nombre usuario>\AppData\Local\Packages\\<identificador del paquete de aplicación>\ para almacenar, entre otras cosas, los archivos locales, de itinerancia y temporales de la aplicación. La aplicación no tiene que declarar las funcionalidades para acceder a estas carpetas, y otras aplicaciones no pueden acceder a estas carpetas. Estas carpetas también se eliminarán al desinstalar la aplicación.

Estas son algunas de las carpetas de la aplicación que normalmente se utilizan:

- **LocalState**: para los datos locales del dispositivo actual. Cuando se hace una copia de seguridad del dispositivo, los datos de este directorio se guardan en una imagen de copia de seguridad en OneDrive. Si el usuario restablece o reemplaza el dispositivo, se restaurarán los datos. Accede a esta carpeta con `Windows.Storage.ApplicationData.Current.LocalFolder.` Guarda los datos locales para los que no quieres una copia de seguridad de OneDrive en la carpeta **LocalCacheFolder**, a la que puedes acceder con `Windows.Storage.ApplicationData.Current.LocalCacheFolder`.

- **RoamingState**: para los datos que se deben replicar en todos los dispositivos donde esté instalada la aplicación. Windows limita la cantidad de datos de itinerancia; así que guarda en esta ubicación solo la configuración de usuario y archivos pequeños. Accede a la carpeta de itinerancia con `Windows.Storage.ApplicationData.Current.RoamingFolder`.

- **TempState**: para los datos que pueden eliminarse cuando la aplicación no se está ejecutando. Accede a esta carpeta con `Windows.Storage.ApplicationData.Current.TemporaryFolder`.

### <a name="access-the-rest-of-the-file-system"></a>Acceder al resto del sistema de archivos

Una aplicación para UWP debe declarar su intención de acceder a una biblioteca de usuario específica agregando a su manifiesto la funcionalidad correspondiente. A continuación, se pide la confirmación del usuario durante la instalación de la aplicación para comprobar que autoriza el acceso a la biblioteca especificada. En caso contrario, la aplicación no se instala. Hay funcionalidades para acceder a las imágenes, vídeos y bibliotecas de música. Consulta [Declaraciones de funcionalidades de la aplicación](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) para obtener la lista completa. Para obtener una **StorageFolder** para estas bibliotecas, usa la clase [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders).

#### <a name="documents-library"></a>Biblioteca de documentos

Aunque hay una funcionalidad para acceder a la biblioteca de documentos del usuario, está restringida; es decir que Microsoft Store rechazará a las aplicaciones que declaren esa funcionalidad a menos que sigas el proceso para obtener aprobación especial. No está prevista para uso general. En su lugar, usa el archivo o los selectores de archivo (consulta [Abrir archivos y carpetas con un selector](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) y [Guardar un archivo con un selector](https://docs.microsoft.com/windows/uwp/files/quickstart-save-a-file-with-a-picker)) que permiten al usuario navegar a la carpeta o archivo. Cuando el usuario navega a una carpeta o archivo, implícitamente ha dado permiso de acceso a la aplicación y el sistema permite el acceso.

#### <a name="general-access"></a>Acceso general

Como alternativa, la aplicación puede declarar la funcionalidad [broadFileSystem](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) restringida en su manifiesto, lo que también requiere la aprobación de Microsoft Store. A continuación, la aplicación puede acceder a cualquier archivo al que el usuario tenga acceso sin la intervención de un selector de archivos o carpetas.

Para obtener una lista completa de las ubicaciones a las que las aplicaciones pueden tener acceso, consulta [Permisos de acceso de archivo](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).

## <a name="useful-apis-and-docs"></a>API y documentos útiles

Este es un resumen rápido de las API y otra documentación de utilidad que te ayudarán a empezar a trabajar con archivos y carpetas.

### <a name="useful-apis"></a>API útiles

| API | Descripción |
|------|---------------|
|  [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) | Proporciona información sobre el archivo, además de los métodos para crear, abrir, copiar, eliminar y cambiar el nombre de los archivos. |
| [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) | Proporciona información sobre la carpeta, métodos para crear archivos, y métodos para crear, abrir, eliminar y cambiar el nombre de los archivos. |
| [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) |  Proporciona una forma fácil de leer y escribir texto. Esta clase también puede leer/escribir una matriz de bytes o el contenido de un búfer. |
| [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) | Proporciona una manera sencilla de leer/escribir texto desde/a un archivo después de haber facilitado la ruta de acceso de cadena del archivo. Esta clase también puede leer/escribir una matriz de bytes o el contenido de un búfer. |
| [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) & [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) |  Leer y escribir búferes, bytes, enteros, GUID, TimeSpan y más desde/hacia una secuencia. |
| [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) | Proporciona acceso a las carpetas creadas para la aplicación, como la carpeta local, la carpeta de itinerancia y la carpeta de archivos temporales. |
| [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) |  Permite al usuario elegir una carpeta y devuelve una **StorageFolder** para ella. De esta forma, se obtiene acceso a las ubicaciones a las que la aplicación no puede acceder de manera predeterminada. |
| [Windows.Storage.Pickers.FileOpenPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker) | Permite al usuario elegir un archivo para abrirlo y devuelve una clase **StorageFile** para él. De esta forma, se obtiene acceso al archivo al que la aplicación no puede acceder de manera predeterminada. |
| [Windows.Storage.Pickers.FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker) | Permite al usuario elegir el nombre de archivo, la extensión y la ubicación de almacenamiento de un archivo. Devuelve una **StorageFile** . De esta forma, se guarda un archivo en una ubicación a la que la aplicación no puede acceder de manera predeterminada. |
|  Espacio de nombres [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Cubre las secuencias de lectura y escritura. En especial, fíjate en las clases [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) y [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) que leen y escriben búferes, bytes, enteros, GUID, TimeSpan y mucho más. |

### <a name="useful-docs"></a>Documentos útiles

| Tema | Descripción |
|-------|----------------|
| [Windows.Storage Namespace](https://docs.microsoft.com/uwp/api/windows.storage) | Documentos de referencia de API. |
| [Archivos, carpetas y bibliotecas](https://docs.microsoft.com/windows/uwp/files/) | Documentos conceptuales. |
| [Crear, escribir y leer archivos](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) | Cubre la creación, lectura y escritura de texto, datos binarios y secuencias. |
| [Getting started storing app data locally](https://blogs.windows.com/buildingapps/2016/05/10/getting-started-storing-app-data-locally/#pCbJKGjcShh5DTV5.97) (Introducción al almacenamiento local de datos de la aplicación) | Además de cubrir los procedimientos recomendados para guardar los datos locales, aborda el propósito de la carpeta LocalSettings y LocalCache. |
| [Getting Started with Roaming App Data](https://blogs.windows.com/buildingapps/2016/05/03/getting-started-with-roaming-app-data/#RgjgLt5OkU9DbVV8.97) (Introducción a los datos de aplicación de itinerancia) | Una serie de dos partes sobre cómo usar los datos de aplicación de itinerancia. |
| [Directrices para datos de aplicaciones de itinerancia](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | Sigue estas directrices de datos de itinerancia cuando diseñes tu aplicación. |
| [Almacenar y recuperar la configuración y otros datos de aplicación](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | Proporciona una introducción a varios almacenes de datos de aplicación, como las carpetas locales, de itinerancia y temporales. Consulta la sección [Datos de movilidad](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#roaming-data) para obtener directrices e información adicional acerca de la escritura de datos que se transfieren entre dispositivos. |
| [Permisos de acceso a archivos](https://docs.microsoft.com/windows/uwp/files/file-access-permissions) | Información sobre las ubicaciones del sistema de archivo a las que puede acceder tu aplicación. |
| [Abrir archivos y carpetas con un selector](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) | Muestra cómo acceder a archivos y carpetas, permite al usuario decidir a través de una interfaz de usuario de selector. |
| [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | Tipos usados para leer y escribir secuencias. |
| [Archivos y carpetas de las bibliotecas de música, imágenes y vídeos](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries) | Aborda cómo quitar carpetas de las bibliotecas, además del método para obtener la lista de carpetas de una biblioteca a fin de detectar archivos de vídeos, música y fotos almacenados. |

## <a name="useful-code-samples"></a>Ejemplos de código útiles

| Ejemplo de código | Descripción |
|-----------------|---------------|
| [Application data sample](https://docs.microsoft.com/samples/microsoft/windows-universal-samples/applicationdata/) (ejemplo de datos de aplicaciones) | Muestra cómo almacenar y recuperar datos que son específicos de cada usuario utilizando las API de datos de aplicaciones. |
| [Ejemplo de acceso a archivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) | Muestra cómo crear, leer, escribir, copiar y eliminar un archivo. |
| [Ejemplo de selector de archivos](https://docs.microsoft.com/samples/microsoft/windows-universal-samples/filepicker/) | Muestra cómo acceder a archivos y carpetas al permitir que el usuario los elija con la interfaz de usuario. Además, muestra cómo guardar un archivo para que el usuario pueda especificar el nombre, el tipo de archivo y la ubicación de un archivo que va a guardarse. |
| [JSON sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Json) (ejemplo de JSON) | Muestra cómo codificar y descodificar objetos, matrices, cadenas, números y booleanos de notación de objetos JavaScript (JSON) mediante el [Espacio de nombres Windows.Data.Json](https://docs.microsoft.com/uwp/api/Windows.Data.Json). |
| [Ejemplos de código adicionales](https://developer.microsoft.com/windows/samples) | Elige **Files, folder, and libraries** (Archivos, carpeta y bibliotecas) en la lista desplegable de categorías. |
