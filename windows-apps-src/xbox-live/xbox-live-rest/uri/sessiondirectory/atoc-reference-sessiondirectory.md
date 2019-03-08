---
title: URI de directorio de sesión
assetID: e3ba951d-b21f-0014-c358-2603d549d118
permalink: en-us/docs/xboxlive/rest/atoc-reference-sessiondirectory.html
description: " URI de directorio de sesión"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9492ff3272af830404a546c9b01d62178adbac96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651200"
---
# <a name="session-directory-uris"></a>URI de directorio de sesión

Esta sección proporciona detalles sobre las direcciones de identificador de recursos Universal (URI) y los métodos de protocolo de transporte de hipertexto (HTTP) asociados de servicios de Xbox Live para el directorio de sesión de varios jugadores (MPSD).


> [!NOTE] 
> Solo los títulos de juegos que se ejecutan en una consola Xbox 360, en un dispositivo Windows Phone o en Xbox.com pueden usar el URI del directorio de sesión.  


  * [Dominio](#ID4EUB)
  * [Versión del servicio](#ID4EZB)
  * [Las propiedades y los objetos del sistema](#ID4EAC)
  * [Identificadores](#ID4EBE)

<a id="ID4EUB"></a>


## <a name="domain"></a>Dominio
sessiondirectory.xboxlive.com  
<a id="ID4EZB"></a>


## <a name="service-version"></a>Versión del servicio

Los llamadores de estos URI de REST deben pasar el valor 104/105 o posterior para X-Xbl-contrato-Version, el encabezado HTTP que especifica la versión del servicio de servicios de detección de entretenimiento (EDS).

<a id="ID4EAC"></a>


## <a name="system-objects-and-properties"></a>Las propiedades y los objetos del sistema

Para la configuración de sus sesiones y plantillas, el MPSD usa un número de objetos JSON de sesión que se ajustan con esquemas fijos que el directorio exige e interpreta. Durante las llamadas a los métodos admitidos por el directorio de sesión distintos URI, estos objetos se validan y combinan, según los esquemas admitidos. Los objetos JSON principales asociados con la configuración de varios jugadores son:

   *  [MultiplayerActivityDetails (JSON)](../../json/json-multiplayeractivitydetails.md)
   *  [MultiplayerSession (JSON)](../../json/json-multiplayersession.md)
   *  [MultiplayerSessionReference (JSON)](../../json/json-multiplayersessionreference.md)
   *  [MultiplayerSessionRequest (JSON)](../../json/json-multiplayersessionrequest.md)


Objetos JSON asociados que se refiere específicamente con los juegos son:

   *  [GameMessage (JSON)](../../json/json-gamemessage.md)
   *  [GameResult (JSON)](../../json/json-gameresult.md)
   *  [GameSession (JSON)](../../json/json-gamesession.md)
   *  [GameSessionSummary (JSON)](../../json/json-gamesessionsummary.md)


<a id="ID4EBE"></a>


## <a name="handles"></a>Identificadores

Para solo multijugador de 2015, las sesiones pueden tener acceso a través de identificadores de sesión. Se han agregado varios URI para proporcionar la funcionalidad para admitir identificadores.  
<a id="ID4EFE"></a>


## <a name="in-this-section"></a>En esta sección

[/handles](uri-handles.md)

&nbsp;&nbsp;Admite una operación POST para establecer la sesión para la actividad del usuario actual que se mostrará en la experiencia de usuario de panel Xbox One e invitar a miembros de la sesión si es necesario.

[/handles/{handleId}](uri-handleshandleid.md)

&nbsp;&nbsp;Admite operaciones DELETE y GET para los identificadores de sesión especificados por identificador.

[/handles/{handleId}/session](uri-handleshandleidsession.md)

&nbsp;&nbsp;Admite operaciones GET y PUT para una sesión, utilizando la desreferenciación del identificador.

[/handles/query](uri-handlesquery.md)

&nbsp;&nbsp;Admite operaciones POST para crear consultas para los identificadores de sesión.

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)

&nbsp;&nbsp;Admite una operación POST para una consulta por lotes en el nivel de identificador de configuración de servicio.

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)

&nbsp;&nbsp;Admite una operación GET para recuperar un conjunto de documentos de la sesión.

[/serviceconfigs/{scid}/sessiontemplates](uri-serviceconfigsscidsessiontemplates.md)

&nbsp;&nbsp;Admite una operación GET para recuperar un conjunto de plantillas de sesión MPSD.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}](uri-serviceconfigsscidsessiontemplatessessiontemplatename.md)

&nbsp;&nbsp;Admite una operación GET para recuperar un conjunto de nombres de plantilla de sesión.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/batch](uri-serviceconfigscidsessiontemplatessessiontemplatenamebatch.md)

&nbsp;&nbsp;Admite una operación POST para crear una consulta por lotes en el nivel de la plantilla de sesión.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessions.md)

&nbsp;&nbsp;Admite una operación GET para recuperar un conjunto de plantillas de sesión con los nombres de la plantilla especificada.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)

&nbsp;&nbsp;Es compatible con operaciones GET y PUT para crear y recuperar las sesiones.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/{index}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersindex.md)

&nbsp;&nbsp;Admite una operación de eliminación para quitar al miembro de la sesión especificada.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/members/me](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnamemembersme.md)

&nbsp;&nbsp;Admite una operación de eliminación para quitar a los miembros de la sesión.

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}/servers/{server-name}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionnamemembersservername.md)

&nbsp;&nbsp;Admite una operación de eliminación para quitar el servidor especificado de una sesión.

<a id="ID4ESF"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EUF"></a>

   [URI de contactos](../matchtickets/atoc-reference-matchtickets.md)


<a id="ID4E1F"></a>


##### <a name="parent"></a>Primario

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)
