---
title: /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
assetID: be789e70-517d-383e-ea35-b0c39e846081
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapathandfilenametype.html
description: " /trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 035f30279321b7b386f59e014fa6d185e6b71b57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603720"
---
# <a name="trustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
Descarga, carga o elimina un archivo. Es el dominio para estos URI `titlestorage.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| entero de 64 bits sin signo| El usuario Xbox ID (XUID) del Reproductor que realiza la solicitud.| 
| scid| guid| el identificador de la configuración de servicio para buscar.| 
| pathAndFileName| string| Ruta de acceso y nombre del elemento que se va a obtenerse. Los caracteres válidos para la parte de la ruta de acceso (hasta e incluida la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_) y la barra diagonal (/). La parte de la ruta de acceso puede estar vacía. Los caracteres válidos para la parte del nombre de archivo (todos los elementos después de la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_), punto (.) y guión (-). El nombre de archivo no puede estar vacío, terminar con un punto ni contener dos puntos consecutivos.| 
| type| string| El formato de los datos. Los valores posibles son binario o json.| 
  
<a id="ID4EOC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[ELIMINAR](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;Elimina un archivo. 

[GET](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;Descarga un archivo.

[PUT](uri-trustedplatformusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;Carga un archivo. Los datos se pueden cargar en una carga completa en la que se envían los datos y metadatos en un único mensaje, o como una carga de varios bloque en el que se envían los datos y metadatos en una serie de bloques más pequeños. Solo los archivos que tengan menos de 4 megabytes pueden enviarse como un solo mensaje. No se admite la carga de varios bloque para los datos de tipo json. 
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Primario 

[URI de almacenamiento de información de título](atoc-reference-storagev2.md)

   