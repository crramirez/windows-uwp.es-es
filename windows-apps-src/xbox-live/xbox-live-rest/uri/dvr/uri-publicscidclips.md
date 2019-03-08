---
title: /public/scids/{scid}/clips
assetID: 55a1f0ae-08bb-6d1e-a1da-cbeb481c42ee
permalink: en-us/docs/xboxlive/rest/uri-publicscidclips.html
description: " /public/scids/{scid}/clips"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: db279d546780ed40158894f73ecb84687ef35ba6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627980"
---
# <a name="publicscidsscidclips"></a>/public/scids/{scid}/clips
Clips de acceso públicos. Este URI puede especificarse realmente de dos formas: `/public/scids/{scid}/clips` y `/public/titles/{titleId}/clips`. Consulta más adelante para más información. El dominio de este URI es `gameclipsmetadata.xboxlive.com`.
 
  * [Parámetros de URI](#ID4E1)
 
<a id="ID4E1"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| string| El identificador de configuración de servicio principal de los clips públicos.| 
| titleid| string| TitleId de los clips públicos. No se puede especificar en el mismo URI como el ¿SCID. Si se especifica, se usará para buscar el ¿SCID principal.| 
  
<a id="ID4E6B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/public/scids/{scid}/clips)](uri-publicscidclipsget.md)

&nbsp;&nbsp;Lista de clips públicas.
 
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Primario 

[URI de Marketplace](../marketplace/atoc-reference-marketplace.md)

   