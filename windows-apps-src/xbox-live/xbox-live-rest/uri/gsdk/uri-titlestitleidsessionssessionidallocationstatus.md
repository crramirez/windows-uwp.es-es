---
title: /titles/{titleId}/sessions/{sessionId}/allocationStatus
assetID: 55611f4b-4ba4-fa9a-ce44-fcc4a6df1b35
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus.html
description: " /titles/{titleId}/sessions/{sessionId}/allocationStatus"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: aa66b81d1e363488a6969ab31d7091b695f09ae4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603310"
---
# <a name="titlestitleidsessionssessionidallocationstatus"></a>/titles/{titleId}/sessions/{sessionId}/allocationStatus
El identificador de título especificado y el Id. de sesión, de obtener el estado de la solicitud de vale. Los dominios para estos URI son `gameserverds.xboxlive.com` y `gameserverms.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EU)
  * [Nombre de host](#ID4EPB)
  * [Métodos válidos](#ID4EWB)
 
<a id="ID4EU"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Descripción| 
| --- | --- | 
| titleId| Id. del título que debe operar la solicitud.| 
| sessionId| el identificador de la sesión para buscar.| 
  
<a id="ID4EPB"></a>

 
## <a name="host-name"></a>Nombre de host
 
gameserverms.xboxlive.com
  
<a id="ID4EWB"></a>

 
## <a name="valid-methods"></a>Métodos válidos
  
[GET](uri-titlestitleidsessionssessionidallocationstatus-get.md)
 
&nbsp;&nbsp;Devuelve el estado de asignación de la sessionhost identificado por su Id. de sesión.
   