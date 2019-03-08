---
title: GameClip (JSON)
assetID: 204cb702-4ce4-85a8-f231-3b4fb243405f
permalink: en-us/docs/xboxlive/rest/json-gameclip.html
description: " GameClip (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4cfc2ac0a635e4aacdc9eeefb5097c6bd946a518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646510"
---
# <a name="gameclip-json"></a>GameClip (JSON)
 
<a id="ID4EO"></a>

 
## <a name="gameclip"></a>GameClip
 
El objeto de clip de juego tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| <b>gameClipId</b>| string| El identificador asignado al clip de juego.| 
| <b>state</b>| GameClipState| El estado del juego clip en el sistema.| 
| <b>dateRecorded</b>| DateTime| La fecha y hora en que se inició la grabación, en UTC (formato ISO 8601).| 
| <b>lastModified</b>| DateTime| Última hora de modificación de sus metadatos, en UTC (formato ISO 8601) o el clip de juego.| 
| <b>userCaption</b>| string| Escrito por el usuario no localizado cadena para el clip de juego.| 
| <b>type</b>| GameClipTypes| El tipo de imagen. Puede ser varios valores y será delimitado por comas si es así.| 
| <b>source</b>| GameClipSource| ¿Cómo se originó el clip.| 
| <b>visibility</b>| GameClipVisibility| La visibilidad del clip juego una vez que se publica en el sistema.| 
| <b>durationInSeconds</b>| entero de 32 bits sin signo| La duración del juego clip en segundos.| 
| <b>scid</b>| string| ¿SCID al que está asociado el clip de juego.| 
| <b>rating</b>| número de punto flotante de precisión doble| La clasificación asociada con el clip de juego, en el intervalo de 0,0 a 5.0.| 
| <b>ratingCount</b>| entero de 32 bits sin signo| El número de veces que se ha clasificado este clip.| 
| <b>views</b>| entero de 32 bits sin signo| El número de vistas asociadas al clip de juego.| 
| <b>titleData</b>| string| La bolsa de propiedades de título específico.| 
| <b>titleData</b>| string| La bolsa de propiedades específicas de la consola.| 
| <b>thumbnails</b>| matriz de GameClipThumbnail| Matriz de objetos GameClipThumbnail.| 
| <b>gameClipUris</b>| matriz de GameClipUri| Matriz de objetos GameClipUri.| 
| <b>xuid</b>| string| XUID del propietario del clip juegos, serializa como una cadena.| 
| <b>clipName</b>| string| La versión localizada del nombre del clip, según la configuración regional de la solicitud, tal y como se puede buscar desde el sistema de administración de título.| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
     "id": "7ce5c1a7-1255-46d3-a90e-34a0e2dfab06",
     "xuid": "2716903703773872",
     "state": "Published", 
     "dateRecorded": "2012-12-23T12:00:00Z",
     "lastModified": "2012-10-31T10:45:00Z",
     "clipName": "Forza 4",
     "userCaption": "My awesome car flip",
     "type": "DeveloperInitiated, Achievement",
     "source": "TitleDirect",
     "visibility": "Default",
     "durationInSeconds": 30,
     "scid": "ecb5497e-76d4-4a8a-870d-e76a26306b7d",
     "rating": 1.0,
     "views": 5,
     "thumbnails": [
       {
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
     ],
     "gameClipUris": [
       {
         "uri": "https://gameclips.xbox.com/clips/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/clip.mp4",
         "fileSize": 1234565,
         "uriType": "Download",
         "expiration": "9999-12-31T23:59:59.9999999"
       }
     ]
   }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   