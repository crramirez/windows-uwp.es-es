---
title: /serviceconfigs/{scid}/hoppers/{name}/stats
assetID: 56bb4398-445b-e8c5-a4ce-1651576ee7e7
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestats.html
description: " /serviceconfigs/{scid}/hoppers/{name}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 04376505b76296e052ea431f2a4e5fcfeac7b9e4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613940"
---
# <a name="serviceconfigsscidhoppersnamestats"></a>/serviceconfigs/{scid}/hoppers/{name}/stats

Admite una operación GET para recuperar las estadísticas para un hopper.

> [!IMPORTANT]
> Este identificador URI está pensado para su uso con el contrato 103 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 103 o posterior en cada solicitud.

<a id="ID4ER"></a>


## <a name="domain"></a>Dominio
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>Observaciones
Este URI es compatible con lo valores xuid, gt y me para el identificador de propietario en la configuración del usuario de destino. Solo el creador de un vale puede eliminar el vale o recuperar el estado de la dirección URI.  
<a id="ID4E6"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| scid| GUID| El identificador de configuración de servicio (¿SCID) para la sesión.|
| name| string| El nombre de la hopper.|

<a id="ID4EEC"></a>


## <a name="valid-methods"></a>Métodos válidos

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](uri-serviceconfigsscidhoppershoppernamestatsget.md)

&nbsp;&nbsp;Obtiene las estadísticas para un hopper.

<a id="ID4EQC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4ESC"></a>


##### <a name="parent"></a>Primario  

[URI de contactos](atoc-reference-matchtickets.md)
