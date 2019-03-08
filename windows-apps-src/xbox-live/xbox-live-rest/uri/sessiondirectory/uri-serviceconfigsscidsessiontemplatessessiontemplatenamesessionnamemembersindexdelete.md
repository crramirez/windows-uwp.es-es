---
title: DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})
assetID: 00aa2f3d-69a6-6d68-e99b-aad4b102aba3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindexdelete.html
description: " DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fbe1942d32ee8facc83f1c723cee2aedaa1078d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618230"
---
# <a name="delete-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersindex"></a>DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index})
Quita a los miembros especificados de una sesión.

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
<a id="ID4EYB"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E1B"></a>


##### <a name="parent"></a>Primario

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)
