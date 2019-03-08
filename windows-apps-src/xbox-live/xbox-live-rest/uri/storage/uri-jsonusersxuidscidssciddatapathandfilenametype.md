---
title: /json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json
assetID: d004d715-d73c-693e-2a0b-ce64f482642b
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapathandfilenametype.html
description: " /json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ee2f21426f0fad1cdb84adba864aeebd0d8e1edc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614030"
---
# <a name="jsonusersxuidxuidscidssciddatapathandfilenamejson"></a>/json/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},json
Descarga, carga o elimina un archivo. Es el dominio para estos URI `titlestorage.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| entero de 64 bits sin signo| El usuario Xbox ID (XUID) del Reproductor que realiza la solicitud.| 
| scid| guid| el identificador de la configuración de servicio para buscar.| 
| pathAndFileName| string| Ruta de acceso y nombre del elemento que se va a obtenerse. Los caracteres válidos para la parte de la ruta de acceso (hasta e incluida la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_) y la barra diagonal (/). La parte de la ruta de acceso puede estar vacía. Los caracteres válidos para la parte del nombre de archivo (todos los elementos después de la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_), punto (.) y guión (-). El nombre de archivo no puede estar vacío, terminar con un punto ni contener dos puntos consecutivos.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[ELIMINAR](uri-jsonusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;Elimina un archivo. 

[GET](uri-jsonusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;Descarga un archivo.

[PUT](uri-jsonusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;Carga un archivo. No se admite la carga de varios bloque para los datos de tipo json. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Primario 

[URI de almacenamiento de información de título](atoc-reference-storagev2.md)

   