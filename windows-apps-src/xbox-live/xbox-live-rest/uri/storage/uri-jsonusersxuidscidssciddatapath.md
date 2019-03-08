---
title: /json/users/xuid({xuid})/scids/{scid}/data/{path}
assetID: c2745955-5e52-ea2b-7389-cb85202e01c3
permalink: en-us/docs/xboxlive/rest/uri-jsonusersxuidscidssciddatapath.html
description: " /json/users/xuid({xuid})/scids/{scid}/data/{path}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 275842420b73abc6c2fd8a8fafa265777e2fa00f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637160"
---
# <a name="jsonusersxuidxuidscidssciddatapath"></a>/json/users/xuid({xuid})/scids/{scid}/data/{path}
Muestra información de archivo en una ruta de acceso especificada. Es el dominio para estos URI `titlestorage.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| entero de 64 bits sin signo| El usuario Xbox ID (XUID) del Reproductor que realiza la solicitud.| 
| scid| guid| el identificador de la configuración de servicio para buscar.| 
| ruta de acceso| string| La ruta de acceso a los elementos de datos para devolver. Se devuelven todos los directorios y subdirectorios de búsqueda de coincidencias. Caracteres válidos son letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_) y barra diagonal (/). Puede estar vacío. Longitud máxima de 256.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET](uri-jsonusersxuidscidssciddatapath-get.md)

&nbsp;&nbsp;Muestra información de archivo en una ruta de acceso especificada.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Primario 

[URI de almacenamiento de información de título](atoc-reference-storagev2.md)

   