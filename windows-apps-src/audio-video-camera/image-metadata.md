---
author: laurenhughes
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: En este artículo se muestra cómo leer y escribir propiedades de metadatos de imagen y cómo incluir geoetiquetas en archivos mediante la clase de utilidad GeotagHelper.
title: Metadatos de imagen
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a3e2f10174412b49ce60f3da6a4bb73b2efc4411
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7288978"
---
# <a name="image-metadata"></a>Metadatos de imagen



En este artículo se muestra cómo leer y escribir propiedades de metadatos de imagen y cómo incluir geoetiquetas en archivos mediante la clase de utilidad [**GeotagHelper**](https://msdn.microsoft.com/library/windows/apps/dn903683) .

## <a name="image-properties"></a>Propiedades de imagen

La propiedad [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225) devuelve un objeto [**StorageItemContentProperties**](https://msdn.microsoft.com/library/windows/apps/hh770642) que proporciona acceso a información relacionada con el contenido del archivo. Obtén las propiedades específicas de la imagen mediante una llamada a [**GetImagePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770646). El objeto [**ImageProperties**](https://msdn.microsoft.com/library/windows/apps/br207718) devuelto expone los miembros que contienen los campos básicos de metadatos de la imagen, como el título de la imagen y la fecha de captura.

[!code-cs[GetImageProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetImageProperties)]

Para acceder a un conjunto de metadatos de archivo más grande, usa el Sistema de propiedades de Windows, un conjunto de propiedades de metadatos de archivo que se pueden recuperar con un identificador de cadena único. Crea una lista de cadenas y agrega el identificador a cada propiedad que quieras recuperar. El método [**ImageProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br207732) toma esta lista de cadenas y devuelve un diccionario de pares de clave y valor, donde la clave es el identificador de propiedad y el valor es el valor de propiedad.

[!code-cs[GetWindowsProperties](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetWindowsProperties)]

-   Para obtener una lista completa de propiedades de Windows, incluidos los identificadores y el tipo de cada propiedad, consulta [Propiedades de Windows](https://msdn.microsoft.com/library/windows/desktop/dd561977).

-   Algunas propiedades solo son compatibles con ciertos códecs de imagen y contenedores de archivos. Para obtener una lista de los metadatos de imagen compatibles con cada tipo de imagen, consulta [directivas de metadatos de fotos](https://msdn.microsoft.com/library/windows/desktop/ee872003).

-   Dado que las propiedades que no son compatibles pueden devolver un valor nulo cuando se recuperan, siempre debes verificar los valores nulos antes de usar un valor de metadatos devueltos.

## <a name="geotag-helper"></a>Geolocalizar auxiliares

GeotagHelper es una clase de utilidad que facilita el etiquetado de imágenes con datos geográficos, directamente mediante las API [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603), sin tener que analizar o construir manualmente el formato de metadatos.

Si ya tienes un objeto [**Geopoint**](https://msdn.microsoft.com/library/windows/apps/dn263675) que represente la ubicación que quieres etiquetar en la imagen, ya sea desde un uso anterior de la API de ubicación geográfica o desde otro origen, puedes establecer los datos de geoetiqueta con una llamada a [**GeotagHelper.SetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903685) y pasando un elemento [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) y el elemento **Geopoint**.

[!code-cs[SetGeoDataFromPoint](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromPoint)]

Para establecer los datos de geoetiqueta mediante el uso de la ubicación actual del dispositivo, crea un objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) nuevo y llama a la API [**GeotagHelper.SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686) pasando un elemento **Geolocator** y el archivo que se va a etiquetar.

[!code-cs[SetGeoDataFromGeolocator](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetSetGeoDataFromGeolocator)]

-   Debes incluir la funcionalidad del dispositivo **location** en el manifiesto de la aplicación para usar la API [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686).

-   Debes llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) antes de llamar a [**SetGeotagFromGeolocatorAsync**](https://msdn.microsoft.com/library/windows/apps/dn903686) para asegurarse que el usuario haya concedido permiso a la aplicación para usar su ubicación.

-   Para más información sobre las API de geolocalización, consulta [Mapas y ubicación](https://msdn.microsoft.com/library/windows/apps/mt219699).

Para obtener un elemento GeoPoint que represente la ubicación geoetiquetada de un archivo de imagen, llama a [**GetGeotagAsync**](https://msdn.microsoft.com/library/windows/apps/dn903684).

[!code-cs[GetGeoData](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetGetGeoData)]

## <a name="decode-and-encode-image-metadata"></a>Descodificar y codificar los metadatos de imagen

La forma más avanzada de trabajar con datos de imagen es leer y escribir las propiedades en el nivel de secuencia mediante un elemento [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) o [BitmapEncoder](bitmapencoder-options-reference.md). Para estas operaciones puedes usar las propiedades de Windows para especificar los datos de lectura o escritura, pero también puedes usar el lenguaje de consulta de metadatos proporcionado por Windows Imaging Component (WIC) para especificar la ruta de acceso a una propiedad solicitada.

La lectura de metadatos de imagen con esta técnica requiere que dispongas de un elemento [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) que se creó con la secuencia de archivos de imagen de origen. Para obtener información sobre cómo hacerlo, consulta [Creación de imágenes](imaging.md).

Cuando tengas el descodificador, crea una lista de cadenas y agrega una entrada nueva para cada propiedad de metadatos que quieras recuperar, usando la cadena de identificador de propiedad de Windows o una consulta de metadatos WIC. Llama al método [**BitmapPropertiesView.GetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226250) en el miembro [**BitmapProperties**](https://msdn.microsoft.com/library/windows/apps/br226248) del decodificador para solicitar las propiedades especificadas. Las propiedades se devuelven en un diccionario de pares clave y valor que contiene el nombre de la propiedad o la ruta de acceso y el valor de la propiedad.

[!code-cs[ReadImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetReadImageMetadata)]

-   Para obtener información sobre el lenguaje de consulta de metadatos WIC y las propiedades compatibles, consulta el tema [Consultas de metadatos nativos de formato de imagen WIC](https://msdn.microsoft.com/library/windows/desktop/ee719904).

-   Muchas propiedades de metadatos solo son compatibles con un subconjunto de tipos de imagen. [**GetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226250) no se ejecutará correctamente y generará un error con el código 0x88982F41 si la imagen asociada con el descodificador no admite una de las propiedades solicitadas y con el código 0x88982F81 si la imagen no admite metadatos. Las constantes asociadas con estos códigos de error son WINCODEC\_ERR\_PROPERTYNOTSUPPORTED y WINCODEC\_ERR\_UNSUPPORTEDOPERATION y se definen en el archivo de encabezado winerror.h.
-   Como una imagen puede contener o no un valor para una propiedad concreta, usa el elemento **IDictionary.ContainsKey** para comprobar que una propiedad está presente en los resultados antes de intentar acceder a ella.

La escritura de metadatos de imagen en la secuencia requiere un elemento **BitmapEncoder** asociado al archivo de resultado de la imagen.

Crea un objeto [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) que contenga los valores de propiedad que quieras establecer. Crea un objeto [**BitmapTypedValue**](https://msdn.microsoft.com/library/windows/apps/hh700687) para representar el valor de propiedad. Este objeto se usa un elemento **object** como el valor y miembro de la enumeración de [**PropertyType**](https://msdn.microsoft.com/library/windows/apps/br225871) que define el tipo de valor. Agrega el elemento **BitmapTypedValue** a **BitmapPropertySet** y luego llama a [**BitmapProperties.SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) para que el codificador escriba las propiedades en la secuencia.

[!code-cs[WriteImageMetadata](./code/ImagingWin10/cs/MainPage.xaml.cs#SnippetWriteImageMetadata)]

-   Para obtener información detallada sobre las propiedades que se admiten para cada tipo de archivo de imagen, consulta [Propiedades de Windows](https://msdn.microsoft.com/library/windows/desktop/dd561977), [Directivas de metadatos de fotos](https://msdn.microsoft.com/library/windows/desktop/ee872003) y [Consultas de metadatos nativos de formato de imagen WIC](https://msdn.microsoft.com/library/windows/desktop/ee719904).

-   [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) no se ejecutará correctamente y devolverá el código de error 0x88982F41 si la imagen asociada con el codificador no admite una de las propiedades solicitadas.

## <a name="related-topics"></a>Temas relacionados

* [Creación de imágenes](imaging.md)
 

 




