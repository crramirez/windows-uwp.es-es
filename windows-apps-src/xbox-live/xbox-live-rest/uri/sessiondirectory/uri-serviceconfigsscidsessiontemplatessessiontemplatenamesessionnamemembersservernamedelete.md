---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})
assetID: 39c921d1-a166-74b9-fcbc-ea3c0c58cc40
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservernamedelete.html
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 65da52284d49d4d9384685d073f13bd93b10a30b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658320"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameserversserver-name"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name})
Quita el servidor especificado de una sesión.

> [!IMPORTANT]
> Este método URI requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

  * [Parámetros de URI](#ID4ET)
  * [Códigos de estado HTTP](#ID4E5)
  * [Cuerpo de la solicitud](#ID4EFB)
  * [Cuerpo de respuesta](#ID4EOB)

<a id="ID4ET"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| scid| GUID| Identificador de configuración de servicio (¿SCID). Parte 1 de identificador de sesión.|
| sessionTemplateName| string| Nombre de la instancia actual de la plantilla de sesión. Parte 2 del identificador de sesión.|
| sessionName| GUID| Identificador único de la sesión. Parte 3 del identificador de sesión.|

<a id="ID4E5"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EFB"></a>


## <a name="request-body"></a>Cuerpo de la solicitud
Ver la estructura de la solicitud en [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md).  
<a id="ID4EOB"></a>


## <a name="response-body"></a>Cuerpo de la respuesta
Ver la estructura de respuesta en [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4E1B"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E3B"></a>


##### <a name="parent"></a>Primario

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)
