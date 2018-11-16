---
author: laurenhughes
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: Archivos, carpetas y bibliotecas
description: Obtén información sobre la lectura y escritura de la configuración de la aplicación, los selectores de archivos y carpetas y las ubicaciones de espacios aislados como, por ejemplo, la biblioteca de vídeos y música.
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 80c6292c12568d7f1017ca67707689ce9539c804
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6980372"
---
 # <a name="files-folders-and-libraries"></a>Archivos, carpetas y bibliotecas


Usa las API de los espacios de nombres [Windows.Storage](https://msdn.microsoft.com/library/windows/apps/br227346), [Windows.Storage.Streams](https://msdn.microsoft.com/library/windows/apps/br241791) y [Windows.Storage.Pickers](https://msdn.microsoft.com/library/windows/apps/br207928) para leer y escribir texto y otros formatos de datos en archivos, además de para administrar archivos y carpetas. En esta sección también aprenderás sobre la lectura y escritura de la configuración de la aplicación, los selectores de archivos y carpetas, así como las ubicaciones de espacios aislados como, por ejemplo, como la biblioteca de vídeos o música.

| Tema | Descripción  |
|-------|--------------|
| [Enumerar y consultar archivos y carpetas](quickstart-listing-files-and-folders.md) | Permite tener acceso a los archivos que se encuentran en carpetas, bibliotecas, dispositivos o ubicaciones de red. También puedes consultar los archivos y las carpetas que hay en una ubicación si creas consultas de archivos y carpetas. |
| [Crear, escribir y leer archivos](quickstart-reading-and-writing-files.md) | Lee y escribe un archivo con un objeto [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171). |
| [Obtener las propiedades de archivos](quickstart-getting-file-properties.md) | Obtén las propiedades de nivel superior, básicas y extendidas de un archivo representado mediante un objeto [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171). |
| [Abrir archivos y carpetas con un selector](quickstart-using-file-and-folder-pickers.md) | Para obtener acceso a archivos y carpetas, permite al usuario interactuar con un selector. Puedes usar [FolderPicker](https://msdn.microsoft.com/library/windows/apps/br207881) para obtener acceso a una carpeta. |
| [Guardar un archivo con un selector](quickstart-save-a-file-with-a-picker.md) | Usa [FileSavePicker](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir a los usuarios especificar el nombre y la ubicación donde desean que tu aplicación guarde un archivo. |
| [Acceso a contenido de Grupo Hogar](quickstart-accessing-homegroup-content.md) | Obtén acceso al contenido almacenado en la carpeta Grupo Hogar, que incluye imágenes, música y vídeos. |
| [Determinar la disponibilidad de los archivos de Microsoft OneDrive](quickstart-determining-availability-of-microsoft-onedrive-files.md) | Determina si un archivo de Microsoft OneDrive está disponible mediante la propiedad [StorageFile.IsAvailable](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx). |
| [Archivos y carpetas de las bibliotecas de música, imágenes y vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | Agrega carpetas existentes de música, imágenes o vídeos a las bibliotecas correspondientes. También puedes quitar carpetas de bibliotecas y obtener la lista de carpetas de una biblioteca para detectar archivos de vídeos, música y fotos almacenados. |
| [Realizar un seguimiento de los archivos y carpetas usados recientemente](how-to-track-recently-used-files-and-folders.md) | Realiza un seguimiento de los archivos a los que el usuario accede con mayor frecuencia agregándolos a la lista de elementos usados recientemente (MRU) de la aplicación. La plataforma administra la lista MRU automáticamente ordenando los elementos según la hora del último acceso y eliminando los más antiguos cuando se alcanza el límite de 25 elementos en la lista. Todas las aplicaciones tienen sus propias listas de MRU. |
| [Acceder a la tarjeta SD](access-the-sd-card.md) | Puedes almacenar y tener acceso a datos no esenciales en una tarjeta microSD, especialmente en los dispositivos móviles de bajo costo que tienen un almacenamiento interno limitado. |
| [Permisos de acceso a archivos](file-access-permissions.md) | Las aplicaciones pueden obtener acceso a determinadas ubicaciones del sistema de archivos de manera predeterminada. Asimismo, también pueden tener acceso a otras ubicaciones mediante el selector de archivos o declarando funcionalidades. |
| [Acceso rápido a las propiedades de archivo en UWP](fast-file-properties.md) | Recopila de forma eficaz una lista de archivos y sus propiedades de una biblioteca para usarlos en una aplicación para UWP. |

## <a name="related-samples"></a>Muestras relacionadas
[Muestra de enumeración de carpetas](http://go.microsoft.com/fwlink/p/?linkid=619993)

[Muestra de acceso a archivos](http://go.microsoft.com/fwlink/p/?linkid=619995)

[Muestra de selector de archivos](http://go.microsoft.com/fwlink/p/?linkid=619994)
 

 
