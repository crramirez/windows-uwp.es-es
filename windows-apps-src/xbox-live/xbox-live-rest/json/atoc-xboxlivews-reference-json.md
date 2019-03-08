---
title: Referencia de objeto de notación de objetos JavaScript (JSON)
assetID: 8efcc6f3-d88a-1b15-bcfc-d79a24581b0a
permalink: en-us/docs/xboxlive/rest/atoc-xboxlivews-reference-json.html
description: " Referencia de objeto de notación de objetos JavaScript (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c46557e3fb837bebccbb1039fb416f3e9787af2a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626560"
---
# <a name="javascript-object-notation-json-object-reference"></a>Referencia de objeto de notación de objetos JavaScript (JSON)
 
JavaScript Object Notation (JSON) es una notación ligera basadas en estándares, orientada a objetos para encapsular los datos en la web.
 
Servicios de Xbox Live define objetos JSON que se usan en solicitudes y respuestas del servicio. Esta sección proporciona información de referencia acerca de cada objeto JSON que puede usar con servicios de Xbox Live.
 
<a id="ID4EHB"></a>

 
## <a name="in-this-section"></a>En esta sección

[Mención especial (JSON)](json-achievementv2.md)

&nbsp;&nbsp;Un objeto de logro (versión 2).

[ActivityRecord (JSON)](json-activityrecord.md)

&nbsp;&nbsp;Una cadena formateada y localizada sobre presencia enriquecida de uno o varios usuarios.

[ActivityRequest (JSON)](json-activityrequest.md)

&nbsp;&nbsp;Una solicitud para obtener información acerca de la presencia enriquecida de uno o varios usuarios.

[AggregateSessionsResponse (JSON)](json-aggregatesessionsresponse.md)

&nbsp;&nbsp;Contiene los datos agregados para las sesiones de idoneidad de un usuario.

[BatchRequest (JSON)](json-batchrequest.md)

&nbsp;&nbsp;Una matriz de propiedades con el que se va a filtrar la información de presencia, como usuarios, dispositivos y los títulos.

[DeviceEndpoint (JSON)](json-deviceendpoint.md)

[DeviceRecord (JSON)](json-devicerecord.md)

&nbsp;&nbsp;Información acerca de un dispositivo, incluido su tipo y los títulos activos en él.

[Comentarios (JSON)](json-feedback.md)

&nbsp;&nbsp;Contiene información de comentarios sobre un reproductor.

[GameClip (JSON)](json-gameclip.md)

[GameClipsServiceErrorResponse (JSON)](json-gameclipsserviceerrorresponse.md)

&nbsp;&nbsp;Una parte opcional de la respuesta a la /clips/ /scids/ {¿scid} de {ownerId} / Users / {gameClipId} / URI/formato / {gameClipUriType} API.

[GameClipThumbnail (JSON)](json-gameclipthumbnail.md)

&nbsp;&nbsp;Contiene la información relacionada con una vista en miniatura individual. Puede haber varios tamaños de cada clip, y se deja el cliente para seleccionar el adecuado para su presentación.

[GameClipUri (JSON)](json-gameclipuri.md)

[GameMessage (JSON)](json-gamemessage.md)

&nbsp;&nbsp;Un objeto JSON que define los datos para un mensaje en cola de mensajes de la sesión de juego.

[GameResult (JSON)](json-gameresult.md)

&nbsp;&nbsp;Un objeto JSON que representa los datos que se describen los resultados de una sesión de juego.

[GameSession (JSON)](json-gamesession.md)

&nbsp;&nbsp;Un objeto JSON que representa los datos del juego para una sesión de varios jugadores.

[GameSessionSummary (JSON)](json-gamesessionsummary.md)

&nbsp;&nbsp;Un objeto JSON que representa los datos de resumen para una sesión de juego.

[GetClipResponse (JSON)](json-getclipresponse.md)

&nbsp;&nbsp;Ajusta el clip de juego.

[HopperStatsResults (JSON)](json-hopperstatsresults.md)

&nbsp;&nbsp;Un objeto JSON que representa las estadísticas para un hopper.

[InitialUploadRequest (JSON)](json-initialuploadrequest.md)

&nbsp;&nbsp;El cuerpo de un clip de juego POST cargue la solicitud.

[InitialUploadResponse (JSON)](json-initialuploadresponse.md)

[inventoryItem (JSON)](json-inventoryitem.md)

&nbsp;&nbsp;El artículo de inventario de core representa el elemento estándar en el que se puede conceder un derecho.

[LastSeenRecord (JSON)](json-lastseenrecord.md)

&nbsp;&nbsp;Información acerca de cuándo el sistema vio por última vez un usuario, está disponible cuando el usuario no tiene ningún DeviceRecord válido.

[MatchTicket (JSON)](json-matchticket.md)

&nbsp;&nbsp;Un objeto JSON que representa un vale de coincidencia, utilizado los reproductores para buscar otros jugadores a través del directorio de sesión de varios jugadores (MPSD).

[MediaAsset (JSON)](json-mediaasset.md)

&nbsp;&nbsp;Los recursos multimedia asociados con el cumplimiento o sus recompensas.

[MediaRecord (JSON)](json-mediarecord.md)

[MediaRequest (JSON)](json-mediarequest.md)

[MultiplayerActivityDetails (JSON)](json-multiplayeractivitydetails.md)

&nbsp;&nbsp;Un objeto JSON que representa el **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**.

[MultiplayerSessionReference (JSON)](json-multiplayersessionreference.md)

&nbsp;&nbsp;Un objeto JSON que representa el **MultiplayerSessionReference**. 

[MultiplayerSessionRequest (JSON)](json-multiplayersessionrequest.md)

&nbsp;&nbsp;Pasa el objeto de solicitud JSON para una operación en un **MultiplayerSession** objeto.

[MultiplayerSession (JSON)](json-multiplayersession.md)

&nbsp;&nbsp;Un objeto JSON que representa el **MultiplayerSession**. 

[PagingInfo (JSON)](json-paginginfo.md)

&nbsp;&nbsp;Contiene información de paginación de resultados que se devuelven en páginas de datos.

[PeopleList (JSON)](json-peoplelist.md)

&nbsp;&nbsp;Colección de [persona](json-person.md) objetos.

[PermissionCheckBatchRequest (JSON)](json-permissioncheckbatchrequest.md)

&nbsp;&nbsp;Colección de objetos PermissionCheckBatchRequest.

[PermissionCheckBatchResponse (JSON)](json-permissioncheckbatchresponse.md)

&nbsp;&nbsp;Comprobación los resultados de un permiso de batch para obtener una lista de valores de permiso para varios usuarios.

[PermissionCheckBatchUserResponse (JSON)](json-permissioncheckbatchuserresponse.md)

&nbsp;&nbsp;Comprobación los motivos de un permiso de lote para la lista de valores de permiso para un usuario de destino único.

[PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)

&nbsp;&nbsp;Los resultados de una comprobación de un único usuario en una configuración de permisos solo con un usuario de destino único.

[PermissionCheckResult (JSON)](json-permissioncheckresult.md)

&nbsp;&nbsp;Los resultados de una comprobación de un único usuario en una configuración de permisos solo con un usuario de destino único.

[Persona (JSON)](json-person.md)

&nbsp;&nbsp;Metadatos sobre una sola persona en el sistema de personas.

[PersonSummary (JSON)](json-personsummary.md)

&nbsp;&nbsp;Colección de [persona (JSON)](json-person.md) objetos.

[Player (JSON)](json-player.md)

&nbsp;&nbsp;Contiene los datos de un reproductor en una sesión de juego.

[PresenceRecord (JSON)](json-presencerecord.md)

&nbsp;&nbsp;Datos sobre la presencia en línea de un solo usuario.

[Profile (JSON)](json-profile.md)

&nbsp;&nbsp;La configuración del perfil personal para un usuario.

[Progresión (JSON)](json-progression.md)

&nbsp;&nbsp;Progresión del usuario a desbloquear el logro.

[Propiedad (JSON)](json-property.md)

&nbsp;&nbsp;Contiene los datos de la propiedad proporcionados por el cliente para los criterios de solicitud de contactos.

[QueryClipsResponse (JSON)](json-queryclipsresponse.md)

&nbsp;&nbsp;Encapsula la lista de clips de juego devueltos junto con información de paginación de la lista.

[quotaInfo (JSON)](json-quota.md)

&nbsp;&nbsp;Contiene información de cuota de un grupo de título.

[Requisito (JSON)](json-requirement.md)

&nbsp;&nbsp;Los criterios de desbloqueo de la realización y lo lejos que el usuario está en la superación de ellos.

[ResetReputation (JSON)](json-resetreputation.md)

&nbsp;&nbsp;Contiene las puntuaciones de reputación bases nueva a la que se deben cambiar las puntuaciones de un usuario existente.

[Reward (JSON)](json-reward.md)

&nbsp;&nbsp;La recompensa asociada con la realización.

[RichPresenceRequest (JSON)](json-richpresencerequest.md)

&nbsp;&nbsp;Solicitud para obtener información acerca de qué se debe usar la información de presencia enriquecida.

[Depuracuión puede contener (JSON)](json-serviceerror.md)

&nbsp;&nbsp;Contiene información sobre un error devuelto cuando el error en la llamada al servicio.

[ServiceErrorResponse (JSON)](json-serviceerrorresponse.md)

&nbsp;&nbsp;Cuando se produce un error de servicio, se devolverá un código de error HTTP adecuado. Opcionalmente, el servicio también puede incluir un objeto ServiceErrorResponse tal como se define a continuación. En entornos de producción, se pueden incluidos menos datos.

[SessionEntry (JSON)](json-sessionentry.md)

&nbsp;&nbsp;Contiene los datos de una sesión de idoneidad.

[TitleAssociation (JSON)](json-titleassociation.md)

&nbsp;&nbsp;Un título que está asociado con el logro.

[TitleBlob (JSON)](json-titleblob.md)

&nbsp;&nbsp;Contiene información sobre un título de almacenamiento.

[TitleRecord (JSON)](json-titlerecord.md)

&nbsp;&nbsp;Información sobre un título, incluido su nombre y una marca de tiempo de última modificación.

[TitleRequest (JSON)](json-titlerequest.md)

&nbsp;&nbsp;Solicitud para obtener información sobre un título.

[UpdateMetadataRequest (JSON)](json-updatemetadatarequest.md)

&nbsp;&nbsp;Los metadatos que se deben actualizar de un clip.

[Usuario (JSON)](json-user.md)

&nbsp;&nbsp;Contiene los datos de la tabla de clasificación de usuario.

[UserClaims (JSON)](json-userclaims.md)

&nbsp;&nbsp;Devuelve información sobre el usuario autenticado actual.

[UserList (JSON)](json-userlist.md)

&nbsp;&nbsp;Una colección de [usuario](json-user.md) objetos.

[UserSettings (JSON)](json-usersettings.md)

&nbsp;&nbsp;Devuelve la configuración de usuario autenticado actual.

[UserTitle (JSON)](json-usertitlev2.md)

&nbsp;&nbsp;Contiene los datos de título del usuario.

[VerifyStringResult (JSON)](json-verifystringresult.md)

&nbsp;&nbsp;Como resultado de códigos correspondientes a cada cadena enviada a [/system/strings/validate](../uri/stringserver/uri-systemstringsvalidate.md).

[XuidList (JSON)](json-xuidlist.md)

&nbsp;&nbsp;Lista de XUIDs en el que se va a realizar una operación.
 
<a id="ID4ENH"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EPH"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de RESTful de servicios de Xbox Live](../atoc-xboxlivews-reference.md)

  
<a id="ID4EZH"></a>

 
##### <a name="external-links-ecma-international-standard-262-ecmascript-language-specificationhttpswwwecma-internationalorgpublicationsfilesecma-stecma-262pdf"></a>Vínculos externos [estándar internacional de ECMA 262: Especificación del lenguaje ECMAScript](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-262.pdf)

   