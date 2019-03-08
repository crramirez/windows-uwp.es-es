---
title: Reputación
description: Obtenga información sobre cómo Xbox Live, se usa el servicio de reputación para fomentar el juego positivo.
ms.assetid: f8966184-5db7-4cab-93ca-9a0250a6077d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, reputación, plataforma social
ms.localizationpriority: medium
ms.openlocfilehash: 4e7763294458f19509d5b797a6fa10e03678abee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641820"
---
# <a name="reputation"></a>Reputación

Reputación es una estadística de usuario, al igual que cualquier otra y se incluye en las llamadas para recuperar todas las estadísticas de un usuario y su uso en informes y seguimiento. La reputación de sí mismo está representada por la **clase reputación**. El **ReputationService clase**representa el servicio de reputación. El URI correspondiente se describen en **reputación URI**.

> [!IMPORTANT]
> Estadísticas de reputación son globales para el servicio, no asociado a un título específico. La configuración del servicio para el servicio Id. (¿SCID) es el ¿SCID global que usa para acceder a las estadísticas de reputación.


## <a name="features-of-the-reputation-service"></a>Características del servicio de reputación

El servicio de reputación:

-   Controla las quejas y comentarios de la misma manera. Entidades enviar comentarios acerca de un usuario, y estos comentarios afecta a la reputación del usuario. A continuación, se pueden reenviar los comentarios al equipo de cumplimiento para tomar medidas.
-   Permite a los usuarios enviar comentarios sobre otros usuarios. Títulos pueden enviar comentarios automáticamente.
-   Permite a los títulos de acceso directo a la API. Un usuario puede enviar comentarios directamente desde dentro de un juego y desde el contexto del área de juego donde el usuario está actualmente.
-   Identificadores de mala reputación por afectar a lo que los usuarios pueden hacer en Xbox Live y dentro de los títulos. Por lo tanto los usuarios deben Eche un vistazo a su reputación y act más adecuadamente durante la reproducción en línea.
-   Permite comentarios positivos, así como comentarios negativos. Los usuarios que ayuden a la Comunidad de Xbox o comunidad de un título pueden ser recompensa por sus esfuerzos y puede incluso se les privilegios especiales.
-   Se deriva de una sola, reputación general, representado por la **Reputation.OverallReputation propiedad**. Se deriva de los siguientes tipos de reputación:

    -   Devolver la jugada. Representado por la **Reputation.FairplayReputation propiedad**.
    -   Comunicaciones. Representado por la **Reputation.CommunicationsReputation propiedad**.
    -   Contenido generado por el usuario (contenido generado por usuarios). Representado por la **Reputation.UserGeneratedContentReputation propiedad**.

Consulte **ResetReputation (JSON)** para obtener más información.


## <a name="usage-flow-for-the-reputation-service"></a>Flujo de uso para el servicio de reputación

El flujo total para el servicio de reputación tiene dos fases: reporting reputación y filtrado de reputación de contactos.


## <a name="reporting-reputation"></a>Reputación de informes

Cuando se ha notificado suficientes votos negativos para un usuario específico, Establece el título del **Reputation.OverallReputation propiedad** para indicar una mala reputación para ese usuario (atributo JSON OverallReputationIsBad). Con el tiempo sin quejas, mejora la reputación de un usuario con lentitud y es posible que un usuario con una vez mala reputación para lograr una buena reputación.


## <a name="reputation-filtered-matchmaking"></a>Filtrado de reputación de contactos

Para el filtrado de reputación de contactos, especificado por requisito Xbox (XR), el título recupera el Reproductor **Reputation.OverallReputation propiedad**. Este valor es una puntuación que indica el estado de la reputación general del Reproductor.

> [!NOTE]
> Si el título está buscando el atributo OverallReputationIsBad en un archivo JSON y no lo encuentra, es seguro suponer que el usuario tiene una buena reputación. Esto puede ocurrir con las nuevas cuentas, hasta que se ha procesado los comentarios del usuario. Los reproductores con los comentarios de otros usuarios siempre tendrá las estadísticas de reputación y un valor para el **Reputation.OverallReputation** propiedad.


## <a name="reputation-as-an-indicator-of-behavior"></a>Reputación como un indicador de comportamiento

Reputación proporciona un indicador de cómo se comporta el usuario en el sistema. ¿Por ejemplo, es la persona que un reproductor descriptivo o no? Comentarios de otros miembros de la sesión determina la reputación de un jugador. Cada usuario tiene una puntuación de reputación que viaja con esa persona en cualquier lugar en Xbox One. Se expone de forma destacada en sitios de redes sociales que pueden ver amigos para que la presión del mismo nivel puede aplicar a un reproductor se comporten mejor. Por ejemplo:

-   Está en la tarjeta de perfil de cada usuario. Cualquier persona en el sistema puede mirar el perfil del usuario con un solo clic.
-   Se muestra en los juegos multijugador. Cuando los usuarios jueguen juntos en línea, reputación del grupo es igual a la reputación del Reproductor con la reputación más bajo en el grupo. El grupo sólo coincide con otras personas con reputación similar. Otros jugadores use reputación para decidir qué tipo de reproductores están en juego y decidir si desea unirse al juego.


## <a name="key-elements-of-reputation"></a>Elementos clave de la reputación

Un título puede determinar si un usuario ha alcanzado un, evite Me nivel para devolver la jugada, las comunicaciones o categorías de contenido generado por el usuario (contenido generado por usuarios). El usuario ha alcanzado evitar Me para la categoría asociada si cualquiera de las siguientes propiedades se establece en 1:

-   **Propiedad Reputation.OverallReputation**. El atributo JSON asociado es OverallReputationIsBad.
-   **Propiedad Reputation.FairplayReputation**. El atributo JSON asociado es FairplayReputationIsBad.
-   **Propiedad Reputation.CommunicationsReputation**. El atributo JSON asociado es CommsReputationIsBad.
-   **Propiedad Reputation.UserGeneratedContentReputation**. El atributo JSON asociado es UserContentReputationIsBad.


## <a name="good-game-reports"></a>Juegos buenos informes

Además de informar sobre los usuarios para acciones incorrectas, los usuarios pueden también enviar entre sí buena informes juegos, que se pueden crear en títulos como voto para el Reproductor de gran valor.


## <a name="feedback-history"></a>Historial de votos

Historial de votos notifica información como el tiempo cuando el usuario se notificaba el último y cuál fue la persona notificado para, por ejemplo, "recientemente recibido comentarios de estilo de comunicación".


## <a name="xbox-requirement-for-reputation"></a>Requisito de Xbox para reputación

Requisito (XR de Xbox) 068, contactos filtrado por reputación, requiere la separación de los reproductores con mala reputación de aquellos con alta reputación. Esto se hace examinando la **Reputation.OverallReputation** el valor de un reproductor y atributo de OverallReputationIsBad del jugador en estadísticas de usuario.

El título puede recuperar pasando "OverallReputation" reputación de un usuario a la *statisticName* parámetro de la **UserStatisticsService.GetSingleUserStatisticsAsync método (String, String, String)**. Las llamadas al método WinRT **obtener (/users/xuid({xuid})/scids/{scid}/stats)** como se muestra en el siguiente cuerpo JSON.

```json
    {
      "requestedusers": [
        "2533274792693551"
      ],
      "requestedscids": [
        {
          "scid": "7492baca-c1b4-440d-a391-b7ef364a8d40",
          "requestedstats": [
            "OverallReputationIsBad",
            "FairplayReputationIsBad",
            "CommsReputationIsBad",
            "UserContentReputationIsBad"
          ]
        }
      ]
    }
```

Cuando los comentarios de un usuario de otros jugadores alcanza un nivel bajo, OverallReputationIsBad se establece en 1 para el usuario. Las personas para quien **Reputation.OverallReputation** es 1 solo debe coincidir con otras personas tener **OverallReputation** establecido en 1. De forma predeterminada, cuando se inserta una coincidencia, por lo general no tienen que tratar con las personas con mala reputación. Los títulos, opcionalmente, pueden permitir que un reproductor con una buena reputación participar en el para que coincidan con los reproductores de reputación de baja.

Para obtener la versión más reciente de XRs que se aplican a la reputación, consulte [requisitos de Xbox](https://developer.xboxlive.com/en-us/platform/certification/requirements/Pages/home.aspx) en juego Developer Network (GDN).


## <a name="creating-users-with-bad-overall-reputation-for-testing"></a>Creación de usuarios con mala reputación general para las pruebas

Para las pruebas, pueden configurar los usuarios con muy mala reputación para probar el filtrado de implementación para un título de coincidencia. Para establecer un valor específico para un usuario, el título de la prueba o la herramienta llama **POST (/users/xuid({xuid})/resetreputation)** con un archivo JSON que define los valores de reputación específica del usuario. Consulte **ResetReputation (JSON)** para obtener más información.

Por ejemplo, el siguiente cuerpo JSON establece reputación de devolver la jugada de un usuario en un valor muy bajo:

```json
    {
      "fairplayReputation": 5,
      "commsReputation": 75,
      "userContentReputation": 75
    }
```

Los asociados pueden hacer esta llamada para todos los espacios aislados, excepto la venta directa. Esta solicitud establece una reputación base las puntuaciones de usuario y las ponderaciones del Reproductor de votos positivos se todos se pone a cero. Reputación real del usuario después de esta llamada consta de estas puntuaciones de bases además de bonificación de ambassador del jugador, más extra de seguidores del Reproductor. Este mecanismo, crea un usuario con una puntuación baja y **Reputation.OverallReputation** establecido en 1 para probar la densidad XR de filtro de coincidencia. El título puede obtener la puntuación de reputación de usuario para el usuario desde el servicio de las estadísticas de usuario, como se describe en la sección "Requisitos de Xbox para reputación" de este tema.


## <a name="resetting-a-users-reputation-to-the-defaults"></a>Restableciendo la reputación de un usuario a los valores predeterminados

El título establece el atributo OverallReputationIsBad para indicar que el usuario tiene una buena reputación. Llama a **POST (/ usuarios/me/resetreputation)** y establece todos los valores a 75. Una sola llamada a **/users/xuid({xuid})/deleteuserdata** se puede usar para restablecer varios identificadores de usuario de Xbox. El cuerpo de solicitud es:

```json
    {
      "xuids": [
        "2814659110958830"
      ]
    }
```

## <a name="sending-feedback-from-titles"></a>Envío de comentarios de los títulos

Los títulos pueden enviar fair play comentarios acerca de los jugadores de coincidencias. Esto se realiza directamente desde el título o desde el servicio de título en lotes. Puede utilizar el título del **ReputationService.SubmitReputationFeedbackAsync método** o directa de los siguientes métodos REST:

|                      |                                                             |
|----------------------|-------------------------------------------------------------|
| POST del cliente          | https://reputation.xboxlive.com/users/xuid{XUID}/feedback |
| Servicio POST (lote) | https://reputation.xboxlive.com:10443/users/batchfeedback   |

Consulte **comentarios (JSON)** para un cuerpo JSON de ejemplo para la presentación y para las definiciones de los campos permitidos.


## <a name="types-of-feedback-allowed"></a>Tipos de comentarios permitidos

Se definen las categorías de comentarios que se pueden registrar en **comentarios (JSON)**.


## <a name="when-titles-should-send-feedback"></a>Cuando los títulos deben enviar comentarios

Un título debe enviar comentarios negativos cuando un evento afecta negativamente a la experiencia de Reproductor en un juego. Como norma general, el título debe enviar solo comentarios por tipo, por round reproduce. Por ejemplo, el título debe:

1.  Sólo puede enviar un elemento de comentarios FairPlayKillsTeammates para que una persona por round kill 3 o más miembros del equipo, en lugar de enviar un evento cada vez que la persona mata a un compañero de equipo.
2.  Enviar el comentario FairplayQuitter cuando alguien intencionados cierra una coincidencia al principio.
3.  Enviar el elemento de comentarios FairplayUnsporting, una vez por cada carrera, cuando un usuario está impulsando hacia atrás en un juego de carreras.
