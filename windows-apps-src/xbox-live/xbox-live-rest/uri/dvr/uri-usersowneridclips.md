---
title: /users/{ownerId}/clips
assetID: cc200b89-dc63-9ab5-b037-7a910f46dae9
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclips.html
description: " /users/{ownerId}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cd711777bcdf0b073dd0821222049b03aa35a23c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599520"
---
# <a name="usersowneridclips"></a>/users/{ownerId}/clips
Lista de acceso de los clips del usuario. Los dominios para estos URI son `gameclipsmetadata.xboxlive.com` y `gameclipstransfer.xboxlive.com`, según la función del identificador URI en cuestión.
 
  * [Parámetros de URI](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| ownerId| string| Identidad de usuario del usuario cuyo recurso se está obteniendo acceso. Formatos admitidos: "me" o "xuid(123456789)". Longitud máxima: 16.| 
  
<a id="ID4EVB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/{ownerId}/clips)](uri-usersowneridclipsget.md)

&nbsp;&nbsp;Recuperar la lista de clips del usuario.
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Primario 

[URI Game DVR](atoc-reference-dvr.md)

   