---
title: /json/users/batch/scids/{scid}/data/{pathAndFileName},json
assetID: 06ae159f-7739-1330-df15-871d260e6ba1
permalink: en-us/docs/xboxlive/rest/uri-jsonusersbatchscidssciddatapathandfilenametype.html
description: " /json/users/batch/scids/{scid}/data/{pathAndFileName},json"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b28b0faad1c321137344455bc7cd471f7cb6eba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598600"
---
# <a name="jsonusersbatchscidssciddatapathandfilenamejson"></a>/json/users/batch/scids/{scid}/data/{pathAndFileName},json
Descarga varios archivos de varios usuarios con el mismo nombre de archivo. Es el dominio para estos URI `titlestorage.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| guid| el identificador de la configuración de servicio para buscar.| 
| pathAndFileName| string| Ruta de acceso y nombre del elemento que se va a obtenerse. Los caracteres válidos para la parte de la ruta de acceso (hasta e incluida la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_) y la barra diagonal (/). La parte de la ruta de acceso puede estar vacía. Los caracteres válidos para la parte del nombre de archivo (todos los elementos después de la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_), punto (.) y guión (-). El nombre de archivo no puede estar vacío, terminar con un punto ni contener dos puntos consecutivos.| 
  
<a id="ID4E3B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[POST](uri-jsonusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;Descarga varios archivos de varios usuarios con el mismo nombre de archivo. Descarga el archivo viene determinada por el URI de la solicitud. El cuerpo de la solicitud contiene la lista de XUIDs de los usuarios cuyos archivos para descargar. El cuerpo de la respuesta será un mensaje MIME de varias partes, con cada elemento que representa un archivo para un usuario determinado con su propio conjunto de encabezados. Es posible que las partes de la respuesta a ser una combinación de operaciones correctas y erróneas.
 
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Primario 

[URI de almacenamiento de información de título](atoc-reference-storagev2.md)

   