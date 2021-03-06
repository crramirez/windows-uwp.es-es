---
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: Archivos, carpetas y bibliotecas
description: Obtén información sobre la lectura y escritura de la configuración de la aplicación, los selectores de archivos y carpetas y las ubicaciones de espacios aislados como, por ejemplo, la biblioteca de vídeos y música.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 714185f943e5bb3d417128011684fcc367fd7f20
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175419"
---
 # <a name="files-folders-and-libraries"></a>Archivos, carpetas y bibliotecas


Usa las API de los espacios de nombres [Windows.Storage](/uwp/api/Windows.Storage), [Windows.Storage.Streams](/uwp/api/Windows.Storage.Streams) y [Windows.Storage.Pickers](/uwp/api/Windows.Storage.Pickers) para leer y escribir texto y otros formatos de datos en archivos, además de para administrar archivos y carpetas. En esta sección también aprenderás sobre la lectura y escritura de la configuración de la aplicación, los selectores de archivos y carpetas, así como las ubicaciones de espacios aislados como, por ejemplo, como la biblioteca de vídeos o música.

| Tema | Descripción  |
|-------|--------------|
| [Enumerar y consultar archivos y carpetas](quickstart-listing-files-and-folders.md) | Permite tener acceso a los archivos que se encuentran en carpetas, bibliotecas, dispositivos o ubicaciones de red. También puedes consultar los archivos y las carpetas que hay en una ubicación si creas consultas de archivos y carpetas. |
| [Crear, escribir y leer archivos](quickstart-reading-and-writing-files.md) | Lee y escribe un archivo con un objeto [StorageFile](/uwp/api/Windows.Storage.StorageFile). |
| [Procedimientos recomendados para escribir en archivos](best-practices-for-writing-to-files.md) | Conoce los procedimientos recomendados para usar diversos métodos de escritura de archivos de las clases [FileIO](/uwp/api/windows.storage.fileio) y [PathIO](/uwp/api/windows.storage.pathio). |
| [Obtención de las propiedades del archivo](quickstart-getting-file-properties.md) | Obtén las propiedades de nivel superior, básicas y extendidas de un archivo representado mediante un objeto [StorageFile](/uwp/api/Windows.Storage.StorageFile). |
| [Abrir archivos y carpetas con un selector](quickstart-using-file-and-folder-pickers.md) | Para obtener acceso a archivos y carpetas, permite al usuario interactuar con un selector. Puedes usar [FolderPicker](/uwp/api/Windows.Storage.Pickers.FolderPicker) para obtener acceso a una carpeta. |
| [Guardar un archivo con un selector](quickstart-save-a-file-with-a-picker.md) | Usa [FileSavePicker](/uwp/api/Windows.Storage.Pickers.FileSavePicker) para permitir a los usuarios especificar el nombre y la ubicación donde desean que tu aplicación guarde un archivo. |
| [Acceso a contenido de Grupo Hogar](quickstart-accessing-homegroup-content.md) | Obtén acceso al contenido almacenado en la carpeta Grupo Hogar, que incluye imágenes, música y vídeos. |
| [Determinación de la disponibilidad de los archivos de Microsoft OneDrive](quickstart-determining-availability-of-microsoft-onedrive-files.md) | Determina si un archivo de Microsoft OneDrive está disponible mediante la propiedad [StorageFile.IsAvailable](/uwp/api/windows.storage.storagefile.isavailable). |
| [Archivos y carpetas de las bibliotecas de música, imágenes y vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | Agrega carpetas existentes de música, imágenes o vídeos a las bibliotecas correspondientes. También puedes quitar carpetas de bibliotecas y obtener la lista de carpetas de una biblioteca para detectar archivos de vídeos, música y fotos almacenados. |
| [Seguimiento de los archivos y carpetas usados recientemente](how-to-track-recently-used-files-and-folders.md) | Realiza un seguimiento de los archivos a los que el usuario accede con mayor frecuencia; para ello, agrégalos a la lista de elementos usados recientemente (MRU) de la aplicación. La plataforma administra la lista MRU automáticamente ordenando los elementos según la hora del último acceso y eliminando los más antiguos cuando se alcanza el límite de 25 elementos en la lista. Todas las aplicaciones tienen sus propias listas de MRU. |
| [Realizar un seguimiento de los cambios del sistema de archivos en segundo plano](change-tracking-filesystem.md) | Realiza el seguimiento de los cambios en el sistema de archivos, incluso cuando no se está ejecutando la aplicación.|
| [Acceso a la tarjeta SD](access-the-sd-card.md) | Puedes almacenar datos no esenciales y tener acceso a ellos en una tarjeta microSD opcional, especialmente en los dispositivos móviles de bajo coste que tienen un almacenamiento interno limitado. |
| [Permisos de acceso a archivos](file-access-permissions.md) | Las aplicaciones pueden obtener acceso a determinadas ubicaciones del sistema de archivos de manera predeterminada. Asimismo, las aplicaciones también pueden tener acceso a otras ubicaciones mediante el selector de archivos o declarando funcionalidades. |
| [Acceso rápido a las propiedades de archivos de UWP](fast-file-properties.md) | Recopila de forma eficaz una lista de archivos y sus propiedades de una biblioteca para usarlos en una aplicación para UWP. |

## <a name="related-samples"></a>Muestras relacionadas
[Ejemplo de enumeración de carpetas](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FolderEnumeration)

[Ejemplo de acceso a archivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)

[Ejemplo de selector de archivos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker)
 

 