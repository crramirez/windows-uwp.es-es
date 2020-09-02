---
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: En este artículo se muestra cómo leer y escribir propiedades de metadatos de imagen y cómo incluir geoetiquetas en archivos mediante la clase de utilidad GeotagHelper.
title: Metadatos de imagen
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c020a2ca66c81bee81813402e546fc01ce77c7f3
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362568"
---
# <a name="image-metadata"></a>Metadatos de imagen



En este artículo se muestra cómo leer y escribir propiedades de metadatos de imagen y cómo geoetiquetar archivos mediante la clase de utilidad [**GeotagHelper**](/uwp/api/Windows.Storage.FileProperties.GeotagHelper) .

## <a name="image-properties"></a>Propiedades de la imagen

La propiedad [**StorageFile.Properties**](/uwp/api/windows.storage.storagefile.properties) devuelve un objeto [**StorageItemContentProperties**](/uwp/api/Windows.Storage.FileProperties.StorageItemContentProperties) que proporciona acceso a información relacionada con el contenido del archivo. Obtén las propiedades específicas de la imagen mediante una llamada a [**GetImagePropertiesAsync**](/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.getimagepropertiesasync). El objeto [**ImageProperties**](/uwp/api/Windows.Storage.FileProperties.ImageProperties) devuelto expone los miembros que contienen los campos básicos de metadatos de la imagen, como el título de la imagen y la fecha de captura.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetGetImageProperties":::

Para acceder a un conjunto de metadatos de archivo más grande, usa el Sistema de propiedades de Windows, un conjunto de propiedades de metadatos de archivo que se pueden recuperar con un identificador de cadena único. Crea una lista de cadenas y agrega el identificador a cada propiedad que quieras recuperar. El método [**ImageProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.imageproperties.retrievepropertiesasync) toma esta lista de cadenas y devuelve un diccionario de pares de clave y valor, donde la clave es el identificador de propiedad y el valor es el valor de propiedad.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetGetWindowsProperties":::

-   Para obtener una lista completa de las propiedades de Windows, incluidos los identificadores y el tipo de cada propiedad, consulte [propiedades de Windows](/windows/desktop/properties/props).

-   Algunas propiedades solo son compatibles con ciertos códecs de imagen y contenedores de archivos. Para obtener una lista de los metadatos de imagen admitidos para cada tipo de imagen, vea [directivas de metadatos de fotos](/windows/desktop/wic/photo-metadata-policies).

-   Dado que las propiedades que no son compatibles pueden devolver un valor nulo cuando se recuperan, siempre debes comprobar los valores nulos antes de usar un valor de metadatos devueltos.

## <a name="geotag-helper"></a>Geolocalizar auxiliares

GeotagHelper es una clase de utilidad que facilita el etiquetado de imágenes con datos geográficos, directamente mediante las API [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation), sin tener que analizar o construir manualmente el formato de metadatos.

Si ya tienes un objeto [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) que represente la ubicación que quieres etiquetar en la imagen, ya sea desde un uso anterior de la API de ubicación geográfica o desde otro origen, puedes establecer los datos de geoetiqueta con una llamada a [**GeotagHelper.SetGeotagAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagasync) y pasando un elemento [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) y el elemento **Geopoint**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetSetGeoDataFromPoint":::

Para establecer los datos de geoetiqueta mediante el uso de la ubicación actual del dispositivo, crea un objeto [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) nuevo y llama a la API [**GeotagHelper.SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) pasando un elemento **Geolocator** y el archivo que se va a etiquetar.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetSetGeoDataFromGeolocator":::

-   Debes incluir la funcionalidad del dispositivo **location** en el manifiesto de la aplicación para usar la API [**SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync).

-   Debes llamar a [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) antes de llamar a [**SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) para asegurarse que el usuario haya concedido permiso a la aplicación para usar su ubicación.

-   Para más información sobre las API de geolocalización, consulta [Mapas y ubicación](../maps-and-location/index.md).

Para obtener un elemento GeoPoint que represente la ubicación geoetiquetada de un archivo de imagen, llama a [**GetGeotagAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.getgeotagasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetGetGeoData":::

## <a name="decode-and-encode-image-metadata"></a>Descodificar y codificar los metadatos de imagen

La forma más avanzada de trabajar con datos de imagen es leer y escribir las propiedades en el nivel de secuencia mediante un elemento [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) o [BitmapEncoder](bitmapencoder-options-reference.md). Para estas operaciones puedes usar las propiedades de Windows para especificar los datos de lectura o escritura, pero también puedes usar el lenguaje de consulta de metadatos proporcionado por Windows Imaging Component (WIC) para especificar la ruta de acceso a una propiedad solicitada.

La lectura de metadatos de imagen con esta técnica requiere que dispongas de un elemento [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) que se creó con la secuencia de archivos de imagen de origen. Para obtener información sobre cómo hacerlo, consulta [Creación de imágenes](imaging.md).

Cuando tengas el descodificador, crea una lista de cadenas y agrega una entrada nueva para cada propiedad de metadatos que quieras recuperar, usando la cadena de identificador de propiedad de Windows o una consulta de metadatos WIC. Llama al método [**BitmapPropertiesView.GetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) en el miembro [**BitmapProperties**](/uwp/api/Windows.Graphics.Imaging.BitmapProperties) del decodificador para solicitar las propiedades especificadas. Las propiedades se devuelven en un diccionario de pares clave y valor que contiene el nombre de la propiedad o la ruta de acceso y el valor de la propiedad.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetReadImageMetadata":::

-   Para obtener información sobre el lenguaje de consulta de metadatos de WIC y las propiedades admitidas, vea [las consultas de metadatos nativas de formato de imagen WIC](/windows/desktop/wic/-wic-native-image-format-metadata-queries).

-   Muchas propiedades de metadatos solo son compatibles con un subconjunto de tipos de imagen. [**GetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) no se realizará correctamente con el código de error 0x88982F41 si una de las propiedades solicitadas no es compatible con la imagen asociada con el descodificador y con el código 0x88982F81 si la imagen no admite metadatos. Las constantes asociadas a estos códigos de error son WINCODEC \_ Err \_ PROPERTYNOTSUPPORTED y WINCODEC \_ Err \_ UNSUPPORTEDOPERATION y se definen en el archivo de encabezado Winerror. h.
-   Como una imagen puede contener o no un valor para una propiedad concreta, usa el elemento **IDictionary.ContainsKey** para comprobar que una propiedad está presente en los resultados antes de intentar acceder a ella.

La escritura de metadatos de imagen en la secuencia requiere un elemento **BitmapEncoder** asociado al archivo de resultado de la imagen.

Crea un objeto [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) que contenga los valores de propiedad que quieras establecer. Crea un objeto [**BitmapTypedValue**](/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) para representar el valor de propiedad. Este objeto se usa un elemento **object** como el valor y miembro de la enumeración de [**PropertyType**](/uwp/api/Windows.Foundation.PropertyType) que define el tipo de valor. Agrega el elemento **BitmapTypedValue** a **BitmapPropertySet** y luego llama a [**BitmapProperties.SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) para que el codificador escriba las propiedades en la secuencia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetWriteImageMetadata":::

-   Para obtener información detallada sobre las propiedades que se admiten para los tipos de archivo de imagen, consulte [propiedades de Windows](/windows/desktop/properties/props), [directivas de metadatos de fotos](/windows/desktop/wic/photo-metadata-policies)y consultas de [metadatos nativas de formato de imagen de WIC](/windows/desktop/wic/-wic-native-image-format-metadata-queries).

-   [**SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) no se ejecutará correctamente y devolverá el código de error 0x88982F41 si la imagen asociada con el codificador no admite una de las propiedades solicitadas.

## <a name="related-topics"></a>Temas relacionados

* [Creación de imágenes](imaging.md)
 

 
