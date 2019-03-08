---
title: InitialUploadRequest (JSON)
assetID: 8b8bce98-cb5f-bbaf-5564-9be2f58d749b
permalink: en-us/docs/xboxlive/rest/json-initialuploadrequest.html
description: " InitialUploadRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2fbb2417f743d97b487ad16abd241fcb5eea62bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646640"
---
# <a name="initialuploadrequest-json"></a>InitialUploadRequest (JSON)
El cuerpo de un clip de juego POST cargue la solicitud. 
<a id="ID4EN"></a>

 
## <a name="initialuploadrequest"></a>InitialUploadRequest
 
El objeto InitialUploadRequest tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| <b>greatestMomentId</b>| string| El identificador de cadena para el texto que se usará como el nombre para el clip. Esto se administra y localizado en el archivo de configuración para el título por el desarrollador del título.| 
| <b>userCaption</b>| string| Opcional. Obtener nombre alternativo escrito por el usuario para juegos clip hasta una longitud máxima de 250 caracteres.| 
| <b>sessionRef</b>| string| Opcional. Referencia de sesión de juego durante el cual se realizó la grabación.| 
| <b>dateRecorded</b>| DateTime| La hora en que se inició la grabación, en formato UTC. En el búfer como una cadena en formato ISO 8601 (consulte <a href="https://www.w3.org/TR/NOTE-datetime">formatos de fecha y hora</a> para obtener más información).| 
| <b>durationInSeconds</b>| entero de 32 bits sin signo| La longitud del clip en segundos.| 
| <b>expectedBlocks</b>| entero de 32 bits sin signo| Opcional. Número de bloques en la que se va a dividir el archivo. Se omite si el archivo se transmitirá en una sola solicitud.| 
| <b>fileSize</b>| entero de 32 bits sin signo| Tamaño del archivo en bytes del vídeo que se cargará.| 
| <b>type</b>| [Enumeración GameClipType](../enums/gvr-enum-gamecliptypes.md)| El tipo de clip, se serializa como un valor de cadena de la enumeración que está delimitada por comas.| 
| <b>source</b>| [Enumeración GameClipSource](../enums/gvr-enum-gameclipsource.md)| Especifica cómo se originó el clip, serializar como un valor de cadena de la enumeración.| 
| <b>visibility</b>| [Enumeración GameClipVisibility](../enums/gvr-enum-gameclipvisibility.md)| Especifica la visibilidad del clip juego una vez que se publica en el sistema.| 
| <b>titleData</b>| string| Opcional. Bolsa de propiedades específicas del título asociado con este clip. Almacena y se devuelve como-es. Los desarrolladores de título pueden usar este campo para conservar sus propios metadatos acerca de un clip.| 
| <b>titleData</b>| string| Opcional. Bolsa de propiedades para las propiedades específicas de la consola de asociada con este clip. Almacena y se devuelve como-es. Plataforma de la consola puede usar este campo para conservar sus propios metadatos acerca de un clip.| 
| <b>systemProperties</b>| string| Opcional. Bolsa de propiedades para las propiedades específicas de la consola de asociada con este clip. Almacena y devuelve tal cual. Plataforma de la consola puede usar este campo para conservar sus propios metadatos acerca de un clip.| 
| <b>usersInSession</b>| matriz de cadena| Opcional. Una lista de los usuarios de la sesión actual.| 
| <b>thumbnailSource</b>| [Enumeración ThumbnailSource](../enums/gvr-enum-thumbnailsource.md)| Opcional. El origen de la miniatura.| 
| <b>thumbnailOffsetMillseconds</b>| entero de 32 bits con signo| Especifica el desplazamiento (en milisegundos) para el desplazamiento miniaturas generadas. Solo se especificó cuando <b>thumbnailSource</b> está establecido como desplazamiento.| 
| <b>savedByUser</b>| Valor booleano| Opcional. Establece el clip se guarden en la cuota del usuario en lugar de almacenamiento de FIFO. El valor predeterminado es false.| 
  
<a id="ID4ERH"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
   "greatestMomentId": "123abc",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
    
```

  
<a id="ID4E1H"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E3H"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   