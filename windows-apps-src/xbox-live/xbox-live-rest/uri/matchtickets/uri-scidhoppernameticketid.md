---
title: /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}
assetID: 25deb7fe-859c-01d2-d14f-455a36c08a7c
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketid.html
description: " /serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9722d06af64c5fd24f53485a1bcfe765f89b08cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627320"
---
# <a name="serviceconfigsscidhoppershoppernameticketsticketid"></a>/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}

Admite una operación de eliminación de un vale de coincidencia.

> [!IMPORTANT]
> Este identificador URI está pensado para su uso con el contrato 103 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 103 o posterior en cada solicitud.

<a id="ID4ER"></a>


## <a name="domain"></a>Dominio
momatch.xboxlive.com  
<a id="ID4EW"></a>


## <a name="remarks"></a>Observaciones
Este URI es compatible con lo valores xuid, gt y me para el identificador de propietario en la configuración del usuario de destino.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| scid| GUID| El identificador de configuración de servicio (¿SCID) para la sesión.|
| name| string| El nombre de la hopper.|
| ticketId| GUID| El Id. de vale.|

<a id="ID4EJC"></a>


## <a name="valid-methods"></a>Métodos válidos

[DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})](uri-scidhoppernameticketiddelete.md)

&nbsp;&nbsp;Quita un vale de coincidencia.

<a id="ID4ETC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EVC"></a>


##### <a name="parent"></a>Primario  

[URI de contactos](atoc-reference-matchtickets.md)
