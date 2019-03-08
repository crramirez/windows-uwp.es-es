---
title: /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me
assetID: a6c2fa17-8bed-d0df-d7ff-db1aa60f44b3
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.html
description: " /serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2b0ecadb543434383ef32c7fd2a749760d5bcc98
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608460"
---
# <a name="serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme"></a>/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me
Admite una operación de eliminación para quitar a los miembros de la sesión.
<a id="ID4EO"></a>


## <a name="domain"></a>Dominio
sessiondirectory.xboxlive.com  
<a id="ID4ET"></a>

 
## <a name="remarks"></a>Observaciones

Todas las operaciones de recursos de miembros de sesión requieren autorización de notificación de un usuario del Id. de usuario de Xbox (XUID).

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| scid| GUID| Identificador de configuración de servicio (¿SCID). Parte 1 de identificador de sesión.|
| sessionTemplateName| string| Nombre de la instancia actual de la plantilla de sesión. Parte 2 del identificador de sesión.|
| sessionName| GUID| Identificador único de la sesión. Parte 3 del identificador de sesión.|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>Métodos válidos

[DELETE (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me)](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersmedelete.md)

&nbsp;&nbsp;Quita a un miembro de una sesión.

<a id="ID4EYC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E1C"></a>


##### <a name="parent"></a>Primario

[URI del directorio de sesión](atoc-reference-sessiondirectory.md)
