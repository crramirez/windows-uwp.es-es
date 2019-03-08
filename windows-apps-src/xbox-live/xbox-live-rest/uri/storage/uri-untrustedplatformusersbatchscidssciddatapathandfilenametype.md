---
title: /untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
assetID: c2909e75-e108-3555-c157-f2d552f10e9f
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersbatchscidssciddatapathandfilenametype.html
description: " /untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bc57ce4cd80a33b831be3eb3615489cec5a633b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599740"
---
# <a name="untrustedplatformusersbatchscidssciddatapathandfilenametype"></a>/untrustedplatform/users/batch/scids/{scid}/data/{pathAndFileName},{type}
Descarga varios archivos de varios usuarios con el mismo nombre de archivo. Es el dominio para estos URI `titlestorage.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| guid| el identificador de la configuración de servicio para buscar.| 
| pathAndFileName| string| Ruta de acceso y nombre del elemento que se va a obtenerse. Los caracteres válidos para la parte de la ruta de acceso (hasta e incluida la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_) y la barra diagonal (/). La parte de la ruta de acceso puede estar vacía. Los caracteres válidos para la parte del nombre de archivo (todos los elementos después de la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_), punto (.) y guión (-). El nombre de archivo no puede estar vacío, terminar con un punto ni contener dos puntos consecutivos.| 
| type| string| El formato de los datos. Los valores posibles son binario json.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[POST](uri-untrustedplatformusersbatchscidssciddatapathandfilenametype-post.md)

&nbsp;&nbsp;Descarga varios archivos de varios usuarios con el mismo nombre de archivo.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Primario 

[URI de almacenamiento de información de título](atoc-reference-storagev2.md)

   