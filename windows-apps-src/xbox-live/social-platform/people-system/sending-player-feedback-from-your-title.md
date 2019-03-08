---
title: Envío de comentarios del Reproductor desde su título
description: Obtenga información sobre cómo el título puede ayudar a promover experiencias positivas Reproductor mediante el envío de comentarios del Reproductor para el servicio de reputación de Xbox Live.
ms.assetid: 49f8eb44-1e31-4248-b645-9123df6f8689
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, reputación, comentarios del Reproductor
ms.localizationpriority: medium
ms.openlocfilehash: 3e8d82fc9b195f174bf7a46a8d21fb20b2df9551
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592240"
---
# <a name="sending-player-feedback-from-your-title"></a>Envío de comentarios del Reproductor desde su título
La mayoría de los miembros de Xbox Live son impresionantes, pero hay un pequeño porcentaje de "Manzanas incorrecta" que perjudican experiencias de juego de otras personas. Se identifican estos porcentajes pequeño de usuarios a través de usuario y el nombre de comentarios enviados. Ayudamos a proteger el resto de la población asegurándose de que estos "manzanas incorrecta" tengan una experiencia de varios jugadores limitada donde no interfieren con juegos de buena jugadores. Xbox depende en gran medida a los usuarios para notificar a otros usuarios a mantener el sistema de precisión, pero los títulos de Xbox One pueden participar directamente y ayudar drásticamente a mejorar la precisión de la población de reputación de usuarios.

## <a name="steps-to-submit-feedback-from-title-or-title-service"></a>Pasos para enviar comentarios en el título o el servicio de título
1. Agregar momentos de comentarios para el título o el servicio de título
2. Determinar el tipo correcto de comentarios
3. Llamar a las API de comentarios de reputación con comentarios

### <a name="adding-feedback-moments-to-title-or-title-service"></a>Agregar comentarios momentos a título o el servicio de título
Todos los jugadores han tenido malas experiencias con compañeros que sabotear sus propios lado, los jugadores que simplemente estar en lugar de reproducir activamente o tramposos que dañen sus juegos. Xbox Live permite a los usuarios notifican directamente estos reproductores problemáticos, pero no es perfecta comentarios del usuario. Los títulos pueden determinar fácilmente cosas sencillas, como reproductores inactivo en juego y salir al principio y a veces, incluso pueden determinar cuándo alguien es trampa. El título puede enviar comentarios en una amplia variedad de momentos de comentarios, que le ayudarán a mejorar la experiencia de todos los jugadores buena.

### <a name="determining-the-correct-feedback-type"></a>Determinar el tipo correcto de comentarios
El sistema de reputación tiene muchos tipos de comentarios que están diseñados para capturar las distintas formas en que un usuario puede justificar los comentarios. Totalmente aparecen en la tabla 1. La mayoría de ellos es negativa, pero es posible mejorar la reputación de un usuario con comentarios positivos también.

Interfaz de usuario del sistema de Xbox proporciona una manera para reproductores para enviar comentarios acerca de otros usuarios en el juego. Comentarios al usuario no tiene mucho peso, puesto que los usuarios son propensos a griefing entre sí cuando pierden. Los títulos pueden complementar ese sistema de la interfaz de usuario, que proporciona la interfaz de usuario para los usuarios para enviar directamente el comentario en el otro, pero en su lugar, es preferible que los títulos de enviar sus comentarios en nombre del título propio mediante el uso de comentarios de los socios. Comentarios de los socios son de plena confianza.

## <a name="example-partner-feedback-usage-scenarios"></a>Escenarios de uso de comentarios de ejemplo asociado

### <a name="user-quitting-a-game-in-the-middle-of-a-match"></a>Al salir de un juego en medio de una coincidencia de usuario
Un reproductor está perdiendo un juego y usa el menú del juego para salir del juego, abandonar sus compañeros de equipo. Cuando un título detecta este comportamiento puede notificar el comportamiento mediante **FairPlayQuitter.**

### <a name="idling-after-match-found-in-game"></a>Inactivo después de la coincidencia encontrada en el juego
Un Reproductor obtiene coincide con otros jugadores para reproducir, pero coherente significa inactivo en el juego en lugar de ayudar al equipo. El título puede notificar el comportamiento de Reproductor mediante **FairplayIdler.**

### <a name="killing-teammates-in-game"></a>Terminando compañeros de juego
Un reproductor en un juego está matando constantemente compañeros de equipo por diversión. Cuando un título detecta que un usuario es constantemente sacrificio de equipo puede notificar el Reproductor con **FairPlayKillsTeammates.**

### <a name="title-has-community-kickvote-feature"></a>Título tiene la característica de puesta en marcha/voto de comunidad
Un Reproductor ha sido elegido por los usuarios en la Ronda va a quitar de la sesión para el comportamiento incorrecto. Si el título quita ese jugador de la sesión puede informar al usuario con **FairPlayKicked.**

### <a name="helpful-player-community-vote"></a>Reproductor útil Comunidad voto
Después de unos ciclos de un juego, el título ofrece una opción para seleccionar a una persona que ha ayudado a la mayor parte. Cuando un título ve esta acción puede notificar el comportamiento mediante **PositiveHelpfulPlayer.**

### <a name="high-quality-ugc-user-generated-content"></a>Alta calidad por (contenido generados por el usuario)
Un título tiene una escena en el que un Reproductor puede elegir el mejor diseño para un vehículo. Cuando un título ve esta acción puede notificar el comportamiento mediante **PositiveHighQualityUGC.**

### <a name="skilled-player"></a>Reproductor cualificado
Después de unos ciclos de un juego, el título ofrece una opción para seleccionar a una persona que es el MVP fue el mejor Reproductor. Cuando un título ve un consistenly jugador gana puede notificar el estado de MVP el comportamiento mediante **PositiveSkilledPlayer.**

### <a name="user-reports-questionable-ugc-in-title"></a>Usuario informa cuestionables contenido generado por usuarios en el título
Un título tiene una escena en el que un Reproductor puede ver los diseños del vehículo. Un Reproductor de avisos de un diseño ofensivo y desea notificarlo. Cuando un título ve esta acción puede informar al culpable con **UserContentInappropriateUGC.**

### <a name="title-wants-to-request-an-xbl-ban-review-of-a-player-in-their-title"></a>Título quiere solicitar una revisión de prohibir XBL de un reproductor en su título
Administrador de un título de la Comunidad ha observado un Reproductor de reputación de baja que constantemente causa problemas en su juego. Puede solicitar un título de una directiva de XBL y revisión de equipo de cumplimiento mediante **FairPlayUserBanRequest.**

## <a name="complete-behavior-feedback-options"></a>Complete las opciones de comentarios del comportamiento
En la tabla siguiente se enumera los tipos de comentarios que puede usar para enviar comentarios del usuario en nombre de su título. El servicio de reputación es flexible y puede agregar fácilmente nuevos tipos de comentarios si cree que estos no cumplen las necesidades de su título. Informe a su administrador de cuentas si gustaría ver un nuevo tipo de comentarios agregado.

Tabla 1: Los comentarios de los socios distintos tipos de admite el servicio de reputación.

**Tipos de comentarios de FairPlay**               | **Descripción**
----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------
FairplayKillsTeammates                    | Notificar un jugador que se está terminando intencionadamente sus compañeros de equipo
FairplayCheater                           | Informe de un jugador que sin duda sería trampa
FairplayTampering                         | Notificar un jugador que sin duda ha meddled con el disco de juego o se lo contrario alterado el juego software o hardware
FairplayUserBanRequest                    | Informar de un jugador que piensa que ha obtenido una suspensión
FairplayConsoleBanRequest                 | Informe de una consola que cree que debe quedar prohibida conectaran a Xbox Live
FairplayUnsporting                        | Notificar un jugador que es ciertamente atractivo de conducta unsportsmanlike
FairplayIdler                             | Informar de un jugador que entra en multijugador coincide, pero no se reproduce activamente
FairplayLeaderboardCheater                | Notificar un jugador que sin duda ha hecho trampas que aparezca en una tabla de clasificación
**Tipos de comentarios de las comunicaciones**         |
CommsInappropriateVideo                   | Notificar un jugador que se va a inadecuado en chat de vídeo
**Tipos de comentarios del contenido generado por el usuario** |
UserContentInappropriateUGC               | Notificar una inadecuado parte del contenido que crea un reproductor en el título
UserContentReviewRequest                  | Informar proactivamente una parte del contenido para que revise el equipo XBLPET
UserContentReviewRequestBroadcast         | Informar proactivamente una difusión para que revise el equipo XBLPET
UserContentReviewRequestGameDVR           | Notifique un clip GameDVR proactivamente para que revise el equipo XBLPET
UserContentReviewRequestScreenshot        | Informar proactivamente una captura de pantalla para que revise el equipo XBLPET
**Votos positivos**                     |
PositiveSkilledPlayer                     | Si los usuarios pueden votar para determinar un MVP, informar de un reproductor cualificado cuando determinado que el Reproductor merece votos positivos
PositiveHelpfulPlayer                     | Si un juego proporciona la interfaz de usuario para un reproductor notificar que el otro no útil, notifique el Reproductor útil
PositiveHighQualityUGC                    | Si un juego proporciona la interfaz de usuario para un reproductor complementar el contenido de otro usuario, notificar el contenido de forma positiva

## <a name="feedback-api-calls"></a>Llamadas de API de comentarios
Los títulos pueden usar dos estrategias para llamar al servicio de reputación. El método preferido es para usuarios de informes en el agregado de un servicio asociado con un token de servicio para la autenticación. Títulos también pueden notificar a los usuarios directamente desde el cliente. La API de cliente cuenta con tecnología de lucha contra el fraude basada en lo que requiere varios informes en un usuario que se considere válido. Ambas API es por lotes y puede notificar varios usuarios simultáneamente.

El título puede usar las siguientes API de Xbox Live para enviar comentarios del Reproductor reputación:

Idioma | API
-------- | --------------------------------------------------------------------------------------------
WinRT    | Microsoft::Xbox::Services::Social::ReputationService.SubmitBatchReputationFeedbackAsync(...)
C++      | xbox::services::social::reputation_service.submit_batch_reputation_feedback(...)

Como alternativa, el título puede usar los siguientes métodos directos de REST:

API          | Dirección URL                                                      | Requisitos de autenticación
------------ | -------------------------------------------------------- | ---------------------------------------
Servicio de publicación | https://reputation.xboxlive.com/users/batchfeedback      | S-token con notificaciones de espacio aislado y de asociado
POST del cliente  | https://reputation.xboxlive.com/users/batchtitlefeedback | Xtoken con notificaciones de título y el espacio aislado

## <a name="feedback-object"></a>Objeto de comentarios
El objeto de comentarios tiene la siguiente especificación de la versión más reciente, 101. Ambas API espera un lote de los siguientes objetos.

Miembro       | Tipo   | Descripción
------------ | ------ | -----------------------------------------------------------------------------------------------------------------------------------------------------------------
sessionRef   | object | Un objeto que describe la sesión MPSD estos comentarios se relaciona con, o null.
feedbackType | string | El tipo de comentarios. Los valores posibles se definen en la enumeración ReputationFeedbackType
textReason   | string | Se ha enviado el texto proporcionado por el usuario que agregó al remitente para explicar el motivo de los comentarios. Esto es muy valiosa para nuestro equipo de cumplimiento de directivas.
evidenceId   | string | El identificador de un recurso que puede usarse como prueba de los comentarios que se envían, por ejemplo, un archivo de vídeo registrados durante el juego, o un elemento de fuente de actividades.
titleID      | Cadena | El identificador del título del título reproducido; sólo es necesario si no está presente en el símbolo (token).
targetXuid   | Cadena | XUID del Reproductor que está notificando.

Por ejemplo:

```json
POST https://reputation.xboxlive.com/users/batchtitlefeedback
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId" : null,
            "sessionRef": {
  "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
  "templateName": "CaptureFlag5",
  "name": "Title56932",
           },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Title detected this player killing team members 19 times",
            "evidenceId": null
        }
    ]
}
```

## <a name="feedback-qa"></a>Comentarios, preguntas y respuestas

### <a name="q-can-i-send-a-hint-to-the-system-to-help-with-humans-that-might-be-looking-at-the-player-report"></a>Q: ¿Puedo enviar una sugerencia en el sistema de ayuda con los seres humanos que es posible que se consulten el informe del Reproductor?
R: Sí, y resulta muy útil! Use el parámetro "textReason" para el ejecutor de que en última instancia, se examinará los comentarios enviados. Por ejemplo, para un idler, puede agregar un motivo por el texto que dice "Hemos no recibido ninguna entrada de usuario de este Reproductor después de los primeros cinco segundos del juego". Motivo de este texto puede ser muy valiosa para los agentes de cumplimiento XBLPET, así que asegúrese de que el motivo por el texto descriptivo y útil.

### <a name="q-should-i-worry-about-how-often-i-send-in-feedback-on-a-user"></a>Q: ¿Me preocupan con qué frecuencia enviar los comentarios en un usuario?
R: Títulos deben llamar el servicio de reputación cuando estén seguros de que un usuario ha obtenido comentarios. El sistema tiene varios cierres de seguridad para evitar que los títulos y los usuarios puedan exceso afectarían a los usuarios.

### <a name="q-can-i-adjust-the-weight-of-the-feedback-being-sent"></a>Q: ¿Se puede ajustar el peso de los comentarios que se envían?
R: No, el servicio de reputación determina el espesor de los comentarios. Siempre nos estamos ajustando los pesos para mejorar el sistema.

### <a name="q-can-i-undo-feedback-ive-sent-on-a-user"></a>Q: ¿Puedo deshacer los comentarios que he enviado en un usuario?
R: No, los comentarios son final. Si cree que el título tiene un error y envía comentarios erróneos, háganoslo saber y se le lista negra el título hasta que corrija el error.

### <a name="q-can-i-see-the-feedback-sent-for-my-title-from-users"></a>Q: ¿Ver los comentarios enviados para Mi título de los usuarios?
R: Actualmente no está disponible esa información de autoservicio. Puede pedir a su cuenta de administrador y tienen datos por título, podemos compartir. Pronto nos gustaría poner esto directamente a su disposición y también muestran la combinación de usuarios con mala reputación utilizando su título.

### <a name="q-when-i-send-in-console-or-user-ban-review-request-how-do-i-know-what-happened"></a>Q: Cuando se envían en la consola o solicitud de revisión de prohibir usuario ¿cómo saber qué ha ocurrido?
R: Actualmente, la información de la revisión se envía al equipo XBL directiva y cumplimiento, pero actualmente no se actualizan en la revisión de prohibición.

### <a name="q-is-there-a-reputation-score-per-title"></a>Q: ¿Hay una puntuación de reputación por título?
R: No. Actualmente hay subpuntuaciones de reputación de fairplay, comunicaciones y el contenido generado por el usuario, pero no por título. Podemos agregar esta característica en el futuro si no hay suficientes a petición, así que el Administrador de cuentas saber si desea solicitar esa característica.
