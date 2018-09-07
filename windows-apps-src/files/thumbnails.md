---
author: QuinnRadich
Description: How to use thumbnail images to help users preview files in UWP apps.
title: Directrices para las imágenes en miniatura en aplicaciones para UWP
label: Thumbnail images
template: detail.hbs
ms.author: quradic
ms.date: 01/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df1eec58d936ba4f03e1eadae534abf0620b1a39
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "3659786"
---
# <a name="thumbnail-images"></a>Imágenes en miniatura

En estas directrices, se describe cómo usar las imágenes en miniatura para ayudar a los usuarios a mostrar vistas previas de los archivos mientras navegan por tu aplicación para UWP. 

> **API importantes**: [ThumbnailMode enum](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)

## <a name="should-my-app-include-thumbnails"></a>¿Debo incluir miniaturas en la aplicación?

Si tu aplicación permite a los usuarios explorar archivos, puedes mostrar imágenes en miniatura para ayudar a los usuarios a ver vistas previas de esos archivos. 

Usa miniaturas para: 
- Mostrar vistas previas de muchos elementos de una colección de galería (como archivos y carpetas). Por ejemplo, una galería de fotos debería usar miniaturas para ofrecer a los usuarios una pequeña vista de cada imagen mientras examinan sus archivos de fotos.

    ![galería de vídeo](images/thumbnail-gallery.png)

- Mostrar una vista previa de un elemento individual de una lista (como un archivo específico). Por ejemplo, es posible que el usuario quiera ver más información sobre un archivo, incluida una miniatura más grande para obtener una mejor vista previa, antes de decidir si quiere abrir el archivo. 

    ![vista previa de vídeo](images/thumbnail-preview.png)

## <a name="dos-and-donts"></a>Qué hacer y qué no hacer
- Especifica el [modo de miniatura](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) (PicturesView, VideosView, DocumentsView, MusicView, ListView o SingleItem) al recuperar miniaturas. Así te aseguras de que las imágenes en miniatura están optimizadas para mostrar el tipo de archivos que los usuarios quieren ver. 
    - Usa el modo SingleItem para recuperar una miniatura para un solo elemento, independientemente del tipo de archivo. Los demás modos de miniatura están pensados para mostrar vistas previas de varios archivos. 

- Muestra imágenes genéricas de marcador de posición en lugar de miniaturas mientras las miniaturas se cargan. Usar los marcadores de posición de esta forma hace que tu aplicación parezca más reactiva, ya que los usuarios pueden interactuar con vistas previas antes de que se carguen las miniaturas. 

    Las imágenes de marcador de posición deben:
    * Ser específicas del tipo de elemento al que representan. Por ejemplo, las carpetas, imágenes y vídeos deben tener sus propios marcadores de posición especializados. 
    * Ser del mismo tamaño y relación de aspecto que la imagen en miniatura a la que representan. 
    * Mostrarse hasta que la imagen en miniatura se haya cargado. 

- Usa imágenes de marcador de posición con etiquetas de texto para representar carpetas y grupos de archivos para diferenciarlos de archivos individuales.

- Si no puedes recuperar una miniatura, muestra una imagen de marcador de posición. 

- Muestra la información del archivo adicional cuando proporciones vistas previas para documentos y archivos de imagen. Los usuarios pueden identificar así la información clave sobre un archivo que tal vez no se pueda obtener fácilmente solo de la imagen en miniatura. Por ejemplo, para un archivo de música, puedes mostrar el nombre del artista junto con una miniatura que muestre la carátula del álbum. 

- No muestres información adicional sobre los archivos de imágenes y de vídeo. En la mayoría de los casos, una imagen en miniatura es suficiente para los usuarios que exploran imágenes y vídeos. 

## <a name="additional-usage-guidelines"></a>Directrices de uso adicionales
[Modos de miniatura](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) recomendados y sus funciones:

<table>
<tr>
<th> Mostrar vistas previas para</th>
<th> Modos de miniatura </th>
<th> Funciones de las imágenes en miniatura recuperadas </th>
</tr>
<tr>
<td> Imágenes<br /> Vídeos </td>
<td> PicturesView <br />VideosView </td>
<td> <b>Tamaño:</b> medio, preferiblemente con un mínimo de 190 (si el tamaño de la imagen es de 190 x 130) <br />
<b>Relación de aspecto:</b> uniforme, relación de aspecto amplia de aproximadamente 0,7 (190 x 130 si el tamaño es 190) <br />
Recortado para vistas previas. <br /> 
Adecuada para alinear imágenes en una cuadrícula por su relación de aspecto uniforme.  </td>
</tr>
<tr>
<td> Documentos<br />Música </td>
<td> DocumentsView <br />MusicView <br /> ListView</td>
<td> <b>Tamaño:</b> pequeña, preferiblemente de 40 x 40 píxeles como mínimo <br />
<b>Relación de aspecto:</b> uniforme, relación de aspecto cuadrada  <br />
Adecuada para obtener una vista previa de la carátula del álbum debido a la relación de aspecto cuadrado. <br /> 
Los documentos tienen el mismo aspecto que tendrían en una ventana de selector de archivos (usa los mismos iconos). </td>
</tr>
</tr>
<tr>
<td> Cualquier elemento individual (independientemente del tipo de archivo) </td>
<td> SingleItem </td>
<td> <b>Tamaño:</b> pequeña, preferiblemente de 40 x 40 píxeles como mínimo <br />
<b>Relación de aspecto:</b> uniforme, relación de aspecto cuadrada  <br />
Adecuada para obtener una vista previa de la carátula del álbum debido a la relación de aspecto cuadrado. <br /> 
Los documentos tienen el mismo aspecto que tendrían en una ventana de selector de archivos (usa los mismos iconos). </td>
</tr>
</table>

En estos ejemplos se muestra cómo varían las imágenes en miniatura recuperadas según el tipo de archivo y el modo de miniatura:
<div class="mx-responsive-img">
<table>
<tr>
<th>Tipo de elemento</th>
<th>Cuando se recupera usando: <ul><li>PicturesView <li>VideosView</ul></th>
<th>Cuando se recupera usando: <ul><li>DocumentsView <li>MusicView <li>ListView</ul></th>
<th>Cuando se recupera usando: <ul><li>SingleItem</ul></th>
<tr>
<tr>
<td>Imagen</td>
<td>La imagen en miniatura usa la relación de aspecto original del archivo. <br />
<img src="images/thumbnail-pic-picvidmode.png" alt="Picture thumbnail in picture or video mode"/></td>
<td>La miniatura se recorta para obtener una relación de aspecto cuadrada. <br />
<img src="images/thumbnail-pic-doclistmusic-modes.png" alt="Picture thumbnail in documents, music, or list modes"/></td>
<td>La imagen en miniatura usa la relación de aspecto original del archivo.<br />
<img src="images/thumbnail-pic-single-mode.png" alt="Picture thumbnail in single mode"/> </td>
</tr>
<tr>
<td>Vídeo</td>
<td>La miniatura tiene un icono que la diferencia de las imágenes. <br />
<img src="images/thumbnail-vid-picvid-modes.png" alt="Video thumbnail in picture or video mode"/></td>
<td>La miniatura se recorta para obtener una relación de aspecto cuadrada. <br />
<img src="images/thumbnail-vid-doclistmusic-modes.png" alt="Video thumbnail in documents, music, or list mode"/> </td>
<td>La imagen en miniatura usa la relación de aspecto original del archivo. <br />
<img src="images/thumbnail-vid-single-mode.png" alt="Video thumbnail in single mode"/></td>
</tr>
<tr>
<td>Música</td>
<td>La miniatura es un icono sobre un fondo del tamaño adecuado. El color de fondo se determina por el color de fondo de la ventana de la aplicación. <br />
<img src="images/thumbnail-music-picvid-modes.png" alt="Music thumbnail in picture or video mode"/></td>
<td>Si el archivo tiene una carátula del álbum, la miniatura será la carátula.  <br />
<img src="images/thumbnail-music-doclistmusic-modes.png" alt="Music thumbnail in documents, music, or list mode"/> <br />
En otro caso, la miniatura será un icono sobre un fondo del tamaño adecuado.</td>
<td>Si el archivo tiene una carátula del álbum, la miniatura es la carátula con la relación de aspecto original del archivo.  <br />
<img src="images/thumbnail-music-single-mode.png" alt="Music thumbnail in single mode"/> <br />
En otro caso, la miniatura será un icono. </td>
</tr>
<tr>
<td>Documento</td>
<td>La miniatura es un icono sobre un fondo del tamaño adecuado. El color de fondo se determina por el color de fondo de la ventana de la aplicación. <br />
<img src="images/thumbnail-docs-picvid-modes.png" alt="Document thumbnail in picture or video mode"/></td>
<td>La miniatura es un icono sobre un fondo del tamaño adecuado. El color de fondo se determina por el color de fondo de la ventana de la aplicación. <br />
<img src="images/thumbnail-doc-doclistmusic-modes.png" alt="Document thumbnail in documents, music, or list mode"/></td>
<td>La miniatura del documento, si existe una. <br />
<img src="images/thumbnail-doc1-single-mode.png" alt="Document thumbnail in single mode"/><br />
En otro caso, la miniatura será un icono. <br />
<img src="images/thumbnail-doc2-single-mode.png" alt="Document thumbnail icon in single mode"/></td>
</tr>
<tr>
<td>Carpeta</td>
<td>Si hay un archivo de imagen en la carpeta, se usa la miniatura de la imagen.  <br />
<img src="images/thumbnail-dir-picvid-modes.png" alt="Folder thumbnail in picture or video mode"/> <br />
De lo contrario, no se recupera ninguna miniatura.</td>
<td>No se recupera ninguna imagen en miniatura.</td>
<td>La miniatura es el icono de carpeta.<br />
<img src="images/thumbnail-dir-single-mode.png" alt="Folder icon thumbnail in single mode"/></td>
</tr>
<tr>
<td>Grupo de archivos</td>
<td>Si hay un archivo de imagen en la carpeta, se usa la miniatura de la imagen.<br />
<img src="images/thumbnail-grp-picvid-modes.png" alt="File group thumbnail in picture or video mode"/> <br /> De lo contrario, no se recupera ninguna imagen en miniatura. </td>
<td>Si hay un archivo que tiene una carátula de álbum entre los archivos del grupo, la miniatura es la carátula del álbum. <br />
<img src="images/thumbnail-grp-doclistmusic-modes.png" alt="File group thumbnail in documents, music or list mode"/> <br />De lo contrario, no se recupera ninguna imagen en miniatura. </td>
<td>Si hay un archivo que tiene una carátula de álbum entre los archivos del grupo, la miniatura es la carátula del álbum y usa la relación de aspecto original del archivo. <br />
<img src="images/thumbnail-grp1-single-mode.png" alt="File group thumbnail in picture or video mode"/> <br />En otro caso, la miniatura es un icono que representa un grupo de archivos. <br />
<img src="images/thumbnail-grp2-single-mode.png" alt="File group icon in single mode"/> 
</td>
</tr>
</table>
</div>

## <a name="related-topics"></a>Artículos relacionados
- [Enumeración ThumbnailMode](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)
- [Clase StorageItemThumbnail](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemThumbnail)
- [Clase StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)
- [Muestra de miniatura de archivo y carpeta (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileThumbnails)
- [Vista de lista y de cuadrícula](../design/controls-and-patterns/lists.md)