---
title: Comentarios (JSON)
assetID: 726117c1-f01b-18c0-3b75-a7a7d27d84a2
permalink: en-us/docs/xboxlive/rest/json-feedback.html
description: " Comentarios (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9cc1cb4ecc12219d54af1c4ab420671f2bbfa81f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644770"
---
# <a name="feedback-json"></a>Comentarios (JSON)
Contiene información de comentarios sobre un reproductor.
<a id="ID4EN"></a>


## <a name="feedback"></a>Comentarios

El objeto de comentarios tiene la especificación siguiente.

| Miembro| Tipo| Descripción|
| --- | --- | --- |
| sessionRef| object | Un objeto que describe la sesión MPSD estos comentarios se relaciona con, o null. |
| feedbackType| string | El tipo de comentarios. Los valores posibles se definen en el <b>Microsoft.Xbox.Services.Social.ReputationFeedbackType</b>. |
| textReason| string| Se ha enviado el texto proporcionado por el usuario que agregó al remitente para explicar el motivo de los comentarios. |
| voiceReasonId| string| El identificador de un archivo proporcionado por el usuario de voz de Kinect que era el remitente agregado para explicar el motivo de los comentarios enviados (Base-64). |
| evidenceId| string| El identificador de un recurso que puede usarse como prueba de los comentarios que se envían, por ejemplo, un archivo de vídeo grabado disfrutando del juego. |

<a id="ID4EVC"></a>


### <a name="feedback-types"></a>Tipos de comentarios

La columna "Enviando" indica quién puede enviar los comentarios.

   * "User" significa se puede enviar mediante la consola mediante un XToken para la autenticación, por lo tanto, la API puede aceptar **SubmitFeedback**.
   * "Asociado" significa se puede enviar mediante un socio mediante un certificado de notificaciones, por lo tanto, la API puede aceptar **SubmitBatchFeedback**.
   * "Privacy" significa que solo el servicio de privacidad de SLS puede enviar los comentarios.
   * "" Significa que los comentarios se generan internamente por el servicio de reputación de SLS para auditoría y no se puede enviar cualquier llamador.

| Tipo| Enviado por| Notas|
| --- | --- | --- | --- | --- | --- |
| CommsAbusiveVoice| Usuario| Los usuarios enviar comentarios a las comunicaciones de voz inadecuado de informes desde dentro de un título y de la interfaz Xbox. |
| CommsInappropriateVideo| Usuarios, socios| Los usuarios y socios envían sus comentarios para informar de vídeo inadecuado desde dentro de un título y de la interfaz Xbox. |
| CommsMuted| Privacidad| Cuando un usuario silencia otro reproductor, privacidad envía esta información al servicio de reputación. |
| CommsPhishing| Usuario| Los usuarios enviar este comentario a un mensaje de "phishing". |
| CommsPictureMessage| Usuario| El servicio de la Bandeja de entrada llama al servicio de reputación, que actualiza la reputación del remitente basada en la comunicación de una imagen e informa de los comentarios al equipo de cumplimiento. |
| CommsSpam| Usuario| Los usuarios enviar este comentario a un mensaje de correo basura. |
| CommsTextMessage| Usuario| El servicio de la Bandeja de entrada llama al servicio de reputación, que actualiza la reputación del remitente e informa de los comentarios al equipo de cumplimiento. **Nota:** La UI Inbox debe tener un botón para permitir que los usuarios marcar un mensaje. |
  | CommsVoiceMessage | Usuario | El servicio de la Bandeja de entrada llama al servicio de reputación, que actualiza la reputación del remitente basada en la comunicación de un mensaje de voz e informa de los comentarios al equipo de cumplimiento.  |
  | FairPlayBlock | Privacidad | Privacidad envía esta información al servicio de reputación cuando un usuario bloquee otro reproductor.  |
  | FairPlayCheater | Usuarios, socios | Los títulos que determinan que un usuario es trampa pueden enviar este comentario sin intervención del usuario.  |
  | FairPlayConsoleBanRequest | Partner | Un socio envía estos comentarios como una recomendación para prohibir una consola Xbox Live.  |
  | FairPlayIdler | Usuarios, socios | Los títulos que determinan si un usuario es el acrónimo inactivo a propósito en un juego, suelen redondear después ronda, pueden enviar este comentario sin intervención del usuario.  |
  | FairPlayKicked | Usuarios, socios | Los títulos que detectan que un usuario se ha votado fuera de una partida (iniciada) pueden enviar este comentario sin intervención del usuario.  |
  | FairPlayKillsTeammates | Usuarios, socios | Los títulos que se pueden determinar automáticamente cuando un reproductor killls su compañero de equipo puede enviar este comentario sin intervención del usuario.  |
  | FairPlayQuitter | Usuarios, socios | Los títulos que determinan que un usuario salga al principio de un juego pueden enviar este comentario sin intervención del usuario.  |
  | FairPlayTampering | Usuarios, socios | Los títulos que determinan que un usuario ha manipulado de contenido en el disco pueden enviar este comentario sin intervención del usuario.  |
  | FairPlayUnblock | Privacidad | Privacidad envía esta información al servicio de reputación cuando un usuario desbloquea otro reproductor.  |
  | FairPlayUserBanRequest | Partner | Un socio envía estos comentarios como una recomendación para prohibir que un usuario de Xbox Live.  |
  | InternalAmbassadorScoreUpdated | Ninguno | Se trata de un tipo interno de comentarios no para su uso por los autores de llamadas.  |
  | InternalReputationReset | Ninguno | Se trata de un tipo interno de comentarios no para su uso por los autores de llamadas.  |
  | InternalReputationUpdated | Ninguno | Se trata de un tipo interno de comentarios no para su uso por los autores de llamadas.  |
  | PositiveHelpfulPlayer | Usuarios, socios | Los usuarios y socios envían este comentario envíen información positiva sobre útiles colegas de los jugadores de juegos, foros y así sucesivamente.  |
  | PositiveHighQualityUGC | Usuarios, socios | Los usuarios y socios envían este comentario para indicar que los títulos deben permitir a los usuarios enviar comentarios positivos en el contenido generado compartido que por usuarios desde dentro del juego, por ejemplo, ajuste las configuraciones en Forza.  |
  | PositiveSkilledPlayer | Usuarios, socios | Los usuarios y socios envían este comentario para indicar que los títulos pueden permitir a los usuarios votar un MVP al final de una sesión MPSD.  |
  | UserContentGamerpic | Usuario | Los usuarios enviar este comentario a una imagen jugador inadecuada directamente desde la tarjeta jugador de informes.  |
  | UserContentGamertag | Usuario | Los usuarios enviar este comentario a un jugador inadecuado directamente desde la tarjeta jugador de informes.  |
  | UserContentInappropriateUGC | Usuarios, socios | Los usuarios y socios envían este comentario para indicar que los títulos deben permitir a los usuarios marcar inadecuado compartido contenido generado por usuarios desde dentro del juego, por ejemplo, los trabajos de paint en Forza.  |
  | UserContentPersonalInfo | Usuario | Los usuarios enviar este comentario a una biografía y otra información personal directamente desde la tarjeta jugador de informes.  |

<a id="ID4EFEAC"></a>


## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo


```json
{
"sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Halo556932",
  },
  "feedbackType": "CommsAbusiveVoice",
  "textReason": "He called me a doodoo-head!",
  "voiceReasonId": null,
  "evidenceId": null
}

```


<a id="ID4EOEAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EQEAC"></a>


##### <a name="parent"></a>Primario

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)
