---
title: Problemas comunes de migración de varios jugadores 2015
description: Obtenga información sobre problemas comunes que puede surgir al adaptar su título 2014 multijugador a 2015 participan varios jugadores.
ms.assetid: 206f8fe4-c7aa-44b8-923b-18f679d8439f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a04370ee2f45534c88467700b9523c5a4ad11094
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615820"
---
# <a name="common-issues-when-adapting-your-multiplayer-2014-title-to-multiplayer-2015"></a>Problemas comunes al adaptar su título 2014 multijugador a 2015 multijugador

En este tema se describe problemas que se deben considerar al adaptar los títulos de varios jugadores de 2014 para varios jugadores de 2015.


## <a name="configuration-changes-to-make-for-2015-multiplayer"></a>Cambios de configuración para realizar varios jugadores de 2015

Esta sección describen los cambios para tener en cuenta al configurar las sesiones y plantillas para varios jugadores de 2015. Para obtener ejemplos de los elementos específicos que se analizan, consulte [MPSD sesión plantillas](multiplayer-session-directory.md).

### <a name="add-a-capability-for-active-member-connection"></a>Agregar una funcionalidad de conexión de miembro activo

La funcionalidad de connectionRequiredForActiveMembers permite la detección de desconexión y las características de suscripción de varios jugadores de 2015 de cambio de sesión. Agregar esta funcionalidad al objeto /constants/system/capabilities para todas las plantillas de sesión.


### <a name="add-a-system-constant-for-invite-protocol"></a>Agregue una constante de sistema para el protocolo de invitación

La constante de sistema inviteProtocol permite al destinatario de una invitación para recibir una notificación del sistema al título del remitente llama a la **MultiplayerService.SendInvitesAsync método** o **SystemUI.ShowSendGameInvitesAsync Método (IUser, IMultiplayerSessionReference, String)**. Agregar esta constante, establezca en "jugar" para el objeto /constants/system para todas las plantillas para las sesiones a la que el título invita a los jugadores.


## <a name="runtime-considerations-for-2015-multiplayer"></a>Consideraciones de tiempo de ejecución para varios jugadores de 2015

Títulos de 2015 participan varios jugadores deben:   Llame siempre a la **MultiplayerService.EnableMultiplayerSubscriptions método** antes de entrar en el área del código título varios jugadores. Esta llamada permite que ambas suscripciones a los cambios de la sesión y detección de la desconexión.
-   Asegúrese de usar el mismo **XboxLiveContext clase** objeto para todas las llamadas al mismo usuario. El contexto contiene el estado relacionadas con la administración de la conexión usada para las suscripciones de varios jugadores y detección de la desconexión.
-   Si hay varios usuarios locales, use otro **XboxLiveContext** objeto para cada usuario.


## <a name="migrating-a-session-template-from-contract-version-104105-to-107"></a>Migración de una plantilla de sesión desde la versión del contrato 104/105 a 107

La versión más reciente de contrato de plantilla de sesión es 107, con algunos cambios en el esquema utilizado para MPSD. Las plantillas que se han escrito para la versión de contrato de plantilla de sesión 104/105 deben actualizarse si se vuelven a publicar para usar la versión 107. En este tema se resume los cambios que realice en la migración de las plantillas a la versión más reciente. Las plantillas propiamente dichas se describen en [MPSD sesión plantillas](multiplayer-session-directory.md).

| Importante                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| No se puede cambiar la funcionalidad que se establece a través de una plantilla a través de operaciones de escritura a MPSD. Para cambiar los valores, debe crear y enviar una nueva plantilla con los cambios necesarios. Todos los elementos que no se establecen a través de una plantilla pueden cambiarse a través de operaciones de escritura a la MPSD. |


### <a name="changes-to-the-constantssystemmanagedinitialization-object"></a>Cambios realizados en el objeto /constants/system/managedInitialization

El objeto /constants/system/managedInitialization ha cambiado a /constants/system/memberInitialization. Estos son los cambios que realice a los pares nombre/valor para este objeto: autoEvaluate ha cambiado a externalEvaluation. Los cambios polaridad, excepto que false sigue siendo el predeterminado.
-   El valor predeterminado de membersNeededToStart cambia de 2 a 1.
-   El valor predeterminado de joinTimeout cambia de 5 segundos a 10 segundos.
-   El valor predeterminado de measurementTimeout cambia de 5 segundos a 30 segundos.


### <a name="changes-to-the-constantssystemtimeouts-object"></a>Cambios realizados en el objeto /constants/system/timeouts

Se quita el objeto /constants/system/timeouts, y se cambia el nombre y reubicados en /constants/system los tiempos de espera. Estos son los cambios que realice a los pares nombre/valor para este objeto:   El tiempo de espera de reserva pasa a ser reservedRemovalTimeout.
-   El tiempo de espera inactivo se convierte en inactiveRemovalTimeout. Su nuevo valor predeterminado es 0 (horas).
-   El tiempo de espera listo se convierte en readyRemovalTimeout.
-   El tiempo de espera sessionEmpty se convierte en sessionEmptyTimeout.

Los detalles de los tiempos de espera se presentan en [tiempos de espera de sesión](mpsd-session-details.md).


### <a name="change-to-the-game-play-capability"></a>Cambie a la capacidad de juego

El juego reproducir capacidad habilita reciente reproductores y reputación reporting. Ahora debe especificar el campo /constants/system/capabilities/gameplay como true en las sesiones que representan el juego real, en lugar de las sesiones de aplicación auxiliar, por ejemplo, coinciden y destacar las sesiones.


## <a name="see-also"></a>Consulte también

[Plantillas de sesión MPSD](mpsd-session-details.md)
