---
title: Prácticas recomendadas de ATR
description: Obtenga información sobre los procedimientos recomendados al usar el servicio de la actividad de Xbox Live en tiempo real.
ms.assetid: 543b78e3-d06b-4969-95db-2cb996a8bbd3
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, actividad en tiempo real
ms.localizationpriority: medium
ms.openlocfilehash: 7733aab9330c316ad5938cf9a2ef763e06f19b9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613570"
---
# <a name="real-time-activity-rta-best-practices"></a>Prácticas recomendadas de tiempo real (ATR) de la actividad
Estas recomendaciones le ayudarán a sacar el máximo partido de uso de su título de ATR.


## <a name="the-basics"></a>Los conceptos básicos

ATR utiliza las sesiones de WebSocket para crear una conexión persistente con el cliente. Eso es cómo el servicio proporcionará las estadísticas para usted. Un cliente debe enviar una solicitud de conexión autenticada y ATR utilizará el token proporcionado en la solicitud para comprobar la conexión puede realizarse y, a continuación, establecerla.

Una vez que se ha establecido la conexión, la aplicación puede realizar solicitudes para suscribirse a las estadísticas determinadas. En una suscripción es correcta, ATR devolverá el valor actual y algunos metadatos adicionales, como el tipo de estadística, como parte de la carga de respuesta. A continuación, ATR reenviará las actualizaciones que se producen a la estadística, mientras que el cliente esté suscrito.

Cuando el título no requiere actualizaciones en tiempo real en una estadística, debe finalizar su suscripción para esa estadística.


## <a name="handling-disconnects"></a>Desconecta el control

Títulos deben tener en cuenta que cuando expira el token de autenticación del usuario, su sesión se terminará por el servicio. El título debe ser capaz de detectar cuando se produce este tipo de evento, volver a conectarse y volver a suscribirse a todas las estadísticas que previamente se ha suscrito.

Se cierran las conexiones de ATR transcurridas dos horas por diseño, lo que hará que el cliente para volver a conectar. Esto se hace porque el token de autenticación para la conexión se almacena en caché para ahorrar ancho de banda de mensaje. Finalmente, ese token expirará. Al cerrar la conexión y forzar al cliente a volver a conectar el cliente se ve obligado a actualizar el token de autenticación.

También podría se desconectan de un cliente debido a los ISP de un usuario tiene problemas o cuando se suspende el proceso para el título. Cuando esto sucede, se genera un evento de WebSocket para informar al cliente. En general, es mejor práctica para poder controlar desconecta del servicio.

> [!WARNING]
> Si un cliente usa ATR para sesiones de varios jugadores y se desconecta de treinta segundos, el [multijugador Directory(MPSD) sesión](../multiplayer/multiplayer-appendix/multiplayer-session-directory.md) detecta que la sesión de ATR se cierra e inicia el usuario cierre la sesión. Tiene el cliente de ATR para detectar cuándo se cierra la conexión e iniciar una reconexión y volver a suscribirse antes de que el MPSD finaliza la sesión.

## <a name="managing-subscriptions"></a>Administración de suscripciones

En relación con la administración de suscripciones cuando expira un token, el título debe realizar el seguimiento en absoluto veces que se han realizado las suscripciones. Tras la correcta suscripción, ATR devuelve un identificador único para cada estadística suscrito. En todas las actualizaciones futuras a esa estadística, el identificador se utilizará en lugar del nombre de la estadística. Esto se hace para ahorrar ancho de banda. Los clientes son responsables de mantener su propia asignación de las estadísticas para los identificadores de suscripción de ATR.


## <a name="unsubscribing"></a>Cancelar la suscripción

Tener suscripciones no utilizadas no se recomienda. El servicio limita el número de suscripciones que puede tener un usuario por título en un momento dado. Si se suscribe a todo el contenido y cualquier otra cosa, puede alcanzar ese límite, y esto puede impedir que la suscripción a algunas estadísticas importantes. (Para obtener más información acerca de los límites de suscripción, consulte **limita**, más adelante en este tema.)

Por ejemplo, el título de la sólo tiene una suscripción a una escena determinados. Cuando el usuario escribe ese escena, el título debe suscribirse. Cuando el usuario abandona dicha escena, esas estadísticas debería cancelar la suscripción a. De forma similar, hay un mensaje de cancelación de suscripción-all que puede usarse si no es necesario realizar ninguna suscripción.

Después de cancelar la suscripción, ya no se utilizará el identificador de suscripción para esa estadística.


## <a name="awareness-of-latent-items-in-the-queue"></a>Reconocimiento de latentes elementos en la cola

Al cancelar la suscripción de una estadística, es posible que haya una actualización para la estadística todavía en el proceso de llegar a su cliente. Por lo tanto, incluso si solo ha cancelado la suscripción el título de una estadística, tenga en cuenta que todavía puede obtener una o dos actualizaciones relacionadas con esa estadística.

La recomendación es pasar por alto las actualizaciones de estadísticas cuando no se reconoce el identificador de suscripción.


## <a name="ignore-messages-you-do-not-understand"></a>Ignorar los mensajes que no comprende

Es posible que se actualizará el protocolo de mensajes. Para mantener las aplicaciones independientes de los nuevos mensajes, se recomienda que el título simplemente descartar tipos de mensaje desconocido.


## <a name="throttles"></a>Limitaciones

ATR impone ciertas limitaciones en el servicio:

-   UserStats: 1000
-   Presencia: 2500

Si un cliente alcanza el límite de limitación que podrá recibir un error como parte de una llamada de suscribirse o cancelar la suscripción o se desconectará. En cualquier caso, para obtener más información acerca de la limitación de limitación que se ha llegado a se estén disponibles para el cliente junto con el mensaje de error o mensaje de desconexión.

Al desarrollar su título, tenga en cuenta estos conceptos. Si está realizando algo extremo, puede tener una experiencia de aplicación degradada debido a que el servicio podría estar limitando las llamadas. Derecha ahora, el servicio permite suscripciones de 1000 a las estadísticas por cada instancia de una aplicación. Además de eso, una instancia de una aplicación también puede suscribirse a la longitud de un usuario de la lista de todas las personas para las actualizaciones de presencia. Este número se está optimizando, por lo que puede cambiar en futuras versiones.
