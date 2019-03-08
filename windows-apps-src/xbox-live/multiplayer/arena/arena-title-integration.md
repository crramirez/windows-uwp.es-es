---
title: Guía de integración de título de campo
description: Obtenga información sobre cómo puede crear un título en la compatibilidad de la plataforma de Arena de Xbox Live.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, arena, torneo
ms.localizationpriority: medium
ms.openlocfilehash: 891fa8da1ca6e26128e0a33d28a505a18e99662a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659890"
---
# <a name="arena-title-integration-guide"></a>Guía de integración de título de campo

## <a name="introduction"></a>Introducción

La plataforma de Arena para Xbox permite torneo organizadores (TOs) para crear y operar torneos para los títulos que admiten la Arena. Arena admite tanto una Xbox Live a que permite el publicador y torneos de ejecución de usuario, así como los procedimientos de otros fabricantes que se integren con ámbito. El estilo del torneo, como eliminación única o suizo, viene determinada por la línea.

En este tema le guía, como desarrollador de título, cómo implementar la compatibilidad de Arena en su título. Después de un juego de Arena habilitado, funcionará con cualquier formato de torneo admitidos seleccionado por la línea. LA línea organiza a coincidencias para el título de acuerdo con la estructura del torneo. El título de la Arena habilitado, a continuación, coloca los usuarios adecuados en cada coincidencia dentro del juego y notifica los resultados de coincidencia a la línea.

## <a name="overview"></a>Introducción

Xbox Arena usa conceptos familiares para el desarrollo de juegos multijugador Xbox. Si no está familiarizado con el sistema de varios jugadores de Xbox, se recomienda que revise dicha documentación antes de continuar. El proceso es similar de un usuario de aceptar la invitación a un juego multijugador. Cuando se trata de un usuario reproducir a una coincidencia en un torneo de Arena, una notificación del sistema aparece para informar al usuario. Acepte la notificación del sistema hace que el título recibir un evento de activación de protocolos estándar de Arena. Si ya no se está ejecutando el título, se inicia en este momento.

La coincidencia de torneo se coordina mediante el uso de una sesión de directorio de sesión de varios jugadores (MPSD). En el caso de Arena, se crea la sesión de la plataforma de Arena en lugar de por su título. Esto se realiza mediante una plantilla de sesión y la configuración de juego adicional que se han creado por el usuario como el publicador de título. Los participantes de la coincidencia de torneo ya habrá se ha unido a la sesión. Como el publicador de título, debe configurar la plantilla de sesión usada para Arena para admitir el arbitraje de informes de resultados. Se incluyen todos los requisitos para la sesión más adelante en este documento. También es gratuito crear las sesiones adicionales que necesita su título.

El título usa la sesión para configurar los resultados de coincidencia y el informe. Responder a la activación de protocolos e interactuar con la sesión MPSD son el nivel mínimo de integración requerido por los títulos de Arena. Exploración y el registro para torneos se admiten a través de la interfaz de usuario de Arena de Xbox. Opcionalmente, los títulos pueden obtener información adicional sobre el torneo desde el servicio de concentrador torneos, lo que permite una mejor experiencia en el título. Por ejemplo, mediante el centro de torneos, su título puede mostrar contexto como los nombres de equipo y detectar nuevas sesiones de coincidencia para un usuario. Este flujo de datos se representan mediante las flechas verdes en el diagrama siguiente y se describe en detalle en la [experimentar requisitos y procedimientos recomendados](#experience-requirements-and-best-practices) sección. Las flechas descoloridas indican que la referencia desde el equipo a la sesión cambia con el tiempo, como el usuario se desplaza de coincidencia en coincidencia dentro del torneo.

![](../../images/arena/tournament-flow.png)


La activación de protocolos de Arena URI contiene información sobre el torneo, la sesión para la coincidencia y un vínculo profundo que el título puede invocar cuando se sitúa sobre la coincidencia. El vínculo profundo devuelve al usuario a la interfaz de usuario de Arena de Xbox. Estos componentes URI se describen con más detalle en la [protocolo activación](#1protocol-activation) sección de [requisitos básicos para la integración de Arena](#basic-requirements-for-arena-integration).

## <a name="basic-requirements-for-arena-integration"></a>Requisitos básicos para la integración de Arena

Esta sección proporciona orientación técnica y los detalles para el título de la integración con los requisitos mínimos para admitir la Arena. Este estilo de integración aprovecha el flujo de datos representado por las flechas naranjas en el siguiente diagrama de información general.

![](../../images/arena/arena-data-flow.png)

El título es activados en el protocolo de la interfaz de usuario de Arena de Xbox. Esto se pueda originar en una notificación del sistema, la página de detalles para el torneo, o cualquier otro punto de entrada para la coincidencia. Los pasos que se producen cuando un usuario que participa en una coincidencia son:

1.  El título es activados en el protocolo de la interfaz de usuario de Arena de Xbox.
2.  El título de utiliza los parámetros de activación para reproducir a una coincidencia única.
3.  Cuando finaliza la coincidencia, el título notifica los resultados a la sesión de juego MPSD.
4.  El título proporciona al usuario la opción para volver a la interfaz de usuario de Arena.

Las secciones siguientes se pasan por cada uno de estos pasos con detalle.

### <a name="1--protocol-activation"></a>1.  Activación de protocolos

La interfaz de usuario de Arena inicia las cosas mediante la activación protocolo su título cuando está lista para el usuario una coincidencia. Hay dos casos: el título se puede iniciar en este momento, o que ya esté ejecutando. Ambos casos son similares a lo que sucede cuando se activa un título en respuesta a un usuario aceptar la invitación a un juego multijugador.

#### <a name="if-your-title-is-launched-at-the-time-of-protocol-activation"></a>Si el título se inicia en el momento de la activación de protocolos

El **Activated** evento se desencadena por primera vez cuando se inicia su título. Si esa activación inicial es una activación de protocolos de Arena, se inició el título de un usuario intenta reproducir un torneo. Tienen el título omitir tan pronto como sea posible a la coincidencia, omitiendo las pantallas de menú e inicio de sesión. El Id. de usuario de Xbox (XUID) del usuario participante se proporciona en el URI de activación y ya debería iniciar sesión.

#### <a name="if-your-title-is-already-running"></a>Si ya se está ejecutando el título

El **Activated** también pueden activar eventos con una activación de protocolos de Arena mientras su título ya se está ejecutando. El **activación** evento se desencadena únicamente por una acción explícita del usuario (que actúan en una notificación del sistema que indica la coincidencia o pasar a la coincidencia de la interfaz de usuario de la Arena Xbox). Si ha estado esperando su título en una pantalla de menú, tiene pasar inmediatamente a la coincidencia del torneo. Si su título recibe la activación de protocolo durante el juego, tiene conceder al usuario la opción de dejar el juego y escriba la coincidencia del torneo de la forma más expedita puede. Conceda al usuario una oportunidad de guardar su juego si es necesario.

Activaciones de protocolo se entregan en el título a través de la `CoreApplicationView.Activated` eventos. Si el `IActivatedEventArgs.Kind` propiedad de los argumentos de evento se establece en `Protocol`, la activación es una activación de protocolos y los argumentos que se pueden convertir en el `ProtocolActivatedEventArgs` (clase), donde el URI de la activación de protocolos está disponible a través de la `Uri` propiedad.

Tener el título de comprobar la XUID en el URI de la activación de protocolos. Si el jugador actual no coincide, el título también deberá cambiar contextos de usuario.

#### <a name="the-protocol-activation-uri"></a>El URI de la activación de protocolos

La plantilla para el URI es:

```URI
ms-xbl-multiplayer://tournament?action=joinGame&joinerXuid={memberId}&organizer={organizerId}&tournamentId={tournamentId}&teamId={teamId}&scid={scid}&templateName={templateName}&name={name}&returnUri={returnUri}&returnPfn={returnPfn}
```

Para los títulos de ERA en la consola, el esquema de activación es ligeramente diferente:

```
ms-xbl-{titleIdHex}://
```

Este es el mismo que para las invitaciones. Para este propósito, el identificador de título debe ser ocho caracteres hexadecimales, por lo que se incluyen ceros iniciales, si es necesario.

En primer lugar asegúrese de que el `Uri.Host` (el nombre inmediatamente después del separador "://") es "torneo". Eso es cómo se diferencian de las activaciones de protocolo de Arena de activaciones de otras características, como invitaciones al juego.

Los argumentos de cadena de consulta será la dirección URL codificada y distinguen entre mayúsculas y minúsculas. El título no debe depende del orden de los parámetros y debe omitir los parámetros no reconocidos.


Paramater(s) | Descripción
--- | ---
**action** | La única acción admitida es "joinGame". Si el **acción** falta o se especifica otro valor para él, pasar por alto la activación.
**joinerXuid** | El **joinerXuid** es el XUID del usuario con sesión iniciada que está intentando reproducir en una coincidencia del torneo. El título debe permitir cambiar al contexto de este usuario. Si el **joinerXuid** no está firmado, el título debe solicitar al usuario que lo muestre el selector de cuenta. XUIDs siempre se expresan en notación decimal.
**organizerId, tournamentId** | El **organizerId** y **tournamentId**, al combinarse, forman el identificador único para el torneo que está asociada la coincidencia. Usar este identificador para recuperar la información más detallada sobre el torneo desde el concentrador torneos, si elige que se muestre en el título.
**teamId** | El **teamId** es un identificador único para el equipo en el contexto del torneo de la que el usuario (especificado por el **joinerXuid** parámetro) es un miembro de. Al igual que el **organizerId** y **tournamentId** parámetros, puede usar el **teamId** para, opcionalmente, recuperar información sobre el equipo desde el concentrador torneos.
**scid, templateName, name** | Juntos, que identifican la sesión. Estos son los mismos tres parámetros en la ruta de acceso MPSD URI a la sesión:</br> </br>`https://sessiondirectory.xboxlive.com/serviceconfigs/{scid}/sessiontemplates/{templateName}/sessions{name}`.</br></br>En XSAPI, son los tres parámetros para el `multiplayer_session_reference `constructor.
**returnUri, returnPfn** | El **returnUri** es un URI de activación de protocolos para el usuario regresará a la interfaz de usuario de Arena de Xbox. El **returnPfn** parámetro puede o no pueden estar presente. Si es así, es el nombre de familia de productos (PFN) para la aplicación que se va a controlar el **returnUri**. Para el código de ejemplo que muestra cómo utilizar estos valores, consulte [devolver a la interfaz de usuario de la Arena Xbox](#4returning-to-the-xbox-arena-ui).

### <a name="2--playing-the-match"></a>2.  Reproducción de la coincidencia

Cuando se crea la sesión MPSD el organizador del torneo, el usuario está configurado como un miembro de esa sesión inactivo. El título debe establecer inmediatamente el Reproductor a activo mediante `multiplayer_session::join()`. Esto indica a Xbox Live y los demás usuarios de la coincidencia que el usuario está en el título y listo para reproducirse.

Es la hora de inicio para la coincidencia en la sesión en `/servers/arbitration/constants/system/startTime` como un valor de fecha y hora en formato UTC estándar (por ejemplo, "2009-06-15T13:45:30.0900000Z"). (Hora de inicio también está disponible en XSAPI como `multiplayer_session_arbitration_server::arbitration_start_time()`, empezando en el XDK 1703). LA puede crear la sesión como anticipación a la hora de inicio como desee (incluido simultáneamente con la hora de inicio). Coincidencia notificaciones se envían a los participantes de coincidencia a la hora de inicio para no activar títulos nunca antes de la hora de inicio. La hora de inicio también es la primera hora a la que MPSD permitirá que se comuniquen resultados para la coincidencia.

El título se examina cada miembro `/member/{index}/constants/system/team` propiedad (`multiplayer_session_member::team_id()`) para averiguar lo que cada usuario de equipo se encuentra en.

El título también usa la sesión para determinar la configuración de coincidencia, como los mapas y modo. Estos valores específicos de título pueden establecerse en la plantilla de sesión o como parte de la definición del torneo como constantes personalizadas. (Para obtener más información, consulte [configurando un título para el campo](#configuring-a-title-for-arena).) A continuación se muestra un ejemplo:

```json
{
    "constants": {
        "custom": {
            "enableCheats": false,
            "bestOf": 3,
            "map": "winter-fall",
            "mode": "capture-the-throne"
        }
    }
}
```

Puede recuperar esta configuración de la sesión como un objeto de JavaScript Object Notation (JSON) utilizando el `multiplayer_session_constants::session_custom_constants_json()` API.

En general, el título debe tratar la sesión de Arena del mismo modo que trataría su propia sesión MPSD. Por ejemplo, puede crear controladores y suscríbase para recibir notificaciones de ATR. Sin embargo, hay algunas diferencias. Por ejemplo, el título no puede cambiar la lista de la sesión de juego o usar las características de calidad de servicio (QoS) de la sesión, y ésta debe participar en el arbitraje. Se proporcionan los detalles completos de la sesión más adelante en este documento.

### <a name="3--reporting-results"></a>3.  Informar de los resultados

Los resultados de la coincidencia se notifican al ámbito y la a través de la sesión mediante el uso de una característica denominada arbitraje. Arbitraje es un marco para usar una sesión para reproducir de forma segura una coincidencia y un resultado del informe.

La sesión proporcionada para el título en el paso de activación de protocolos será una sesión arbitrarios, lo que significa que tiene una escala de tiempo fijo que exige el marco de arbitraje. Este diagrama muestra la escala de tiempo de arbitraje.

![](../../images/arena/arbitration-timeline.png)

Al menos uno de los jugadores debe estar activo en la sesión antes de la hora de pérdida, que es la hora de inicio (`multiplayer_session_arbitration_server::arbitration_start_time()`) más el tiempo de espera de pérdida. Es el tiempo de espera de la pérdida de la sesión como un número de milisegundos en `/constants/system/arbitration/forfeitTimeout` (`multiplayer_session_constants::forfeit_timeout()`). Si no se ha unido a la sesión como activa antes de la hora de pérdida, se cancelará la coincidencia.

Es el tiempo de espera de arbitraje en milisegundos en `/constants/system/arbitration/arbitrationTimeout` (`multiplayer_session_constants::arbitration_timeout()`). Tiempo de arbitraje es la hora de inicio más el valor de espera para el arbitraje y representa el tiempo que los jugadores deben ha completado a la coincidencia y notificado los resultados. Este valor se establece en la plantilla de sesión o el modo de juego por el publicador. Configúrelo para permitir que tanto tiempo como el título de la que se necesita para que se puede completar la coincidencia.

El título puede notificar los resultados en cualquier momento entre la hora de inicio y la hora de arbitraje. En cualquier momento entre el momento de la pérdida y la hora de arbitraje, después de cada miembro activo de la sesión ha enviado los resultados se produzca el arbitraje. Por ejemplo, si un solo miembro está activo en la sesión en el momento de la pérdida, que puede (y debe) expongan un resultado y se producirá el arbitraje. Independientemente de cuántos resultados están disponibles en el momento de arbitraje, arbitraje se producirá si no es así ya. Si no hay resultados se envían cuando se alcanza el tiempo de arbitraje, todos los participantes en la coincidencia tienen una pérdida.

También es posible que un servidor de juegos forzar el arbitraje en cualquier momento escribiendo simplemente un resultado arbitrarios.

Si un usuario está en una sesión que ya ha sido arbitrated (bien porque ha expirado el tiempo de espera de arbitraje, un servidor de juegos arbitra la sesión o el usuario se une en tiempo de ejecución), el título de finaliza la coincidencia y muestra el resultado arbitrarios al usuario.

Resultados de arbitraje siempre incluyen el resultado de cada equipo. Cuando un jugador escribe un resultado en la sesión, incluye no solo los resultados de su equipo, pero el conjunto completo de resultados para cada equipo.

Los resultados en JSON tiene este aspecto.

```json
"results": {
    "red": {
        "outcome": "rank",
        "ranking": 1
    },
    "blue": {
        "outcome": "rank",
        "ranking": 2
    }
}
```

En este ejemplo, el equipo de los identificadores es "rojo" y "azul". El equipo rojo llegó primero y el equipo azul en segundo lugar. Otros posibles valores para **resultado** son:

* "win"
* "pérdida"
* "draw"
* "noshow"

Si **resultado** algo distinto de "rank", la clasificación se omite para ese equipo.

Los usuarios individuales escribir los resultados de coincidencia en `/members/{index}/properties/system/arbitration` (`multiplayer_session::set_current_user_member_arbitration_results()`). El ejemplo siguiente muestra cómo un título puede construir y enviar un conjunto de resultados, suponiendo que el título no es en función de equipo (es decir, todos los jugadores están en los equipos de uno).

```c++
void Sample::SubmitResultsForArbitration()
{
    std::unordered_map<string_t, tournament_team_result> results;

    for (auto& member : arbitratedSession->members())
    {
        tournament_team_result teamResult;
        teamResult.set_state(tournament_game_result_state::rank);
        teamResult.set_ranking(memberRank);

        results.insert(std::pair<string_t, tournament_team_result>(
            member->team_id(),
            teamResult));
    }

    arbitratedSession->set_current_user_member_arbitration_results(results);
    xboxLiveContext->multiplayer_service().write_session(
arbitratedSession,
multiplayer_session_write_mode::update_existing)
    .then([](xbox_live_result<std::shared_ptr<multiplayer_session>> sessionResult)
    {
        if (sessionResult.err())
        {
            // Handle error.
        }
        else
        {
            // Update local session cache.
        }
    });
}
```

Después de que se produzca el arbitraje, MPSD coloca los resultados finales en `/servers/arbitration/properties/system` (`multiplayer_session::arbitration_server()`), junto con algunas otras propiedades como se muestra aquí.

```json
{
    "resultState": "completed”,
    "resultSource": "arbitration",
    "resultConfidenceLevel": 75,

    "results": { ... }
}
```

Posibilidades de **resultState** incluyen:

* "completed": Todos los jugadores activos informó de un resultado.
* "partialresults": Algunos pero no todos los jugadores activos informó de un resultado antes de tiempo de espera de arbitraje.
* “noresults”: Ninguno de los jugadores activos informó de un resultado antes de tiempo de espera de arbitraje.
* "Cancelado": Ninguna de jugadores activos llegaron antes el tiempo de espera de pérdida.

En el caso de "/noresults" y "Cancelado", se omiten los resultados.

El **resultSource** es "arbitraje" cuando MPSD realiza el arbitraje basándose en los resultados notificados por los usuarios individuales. Puede escribir un servidor de juegos en /servers/arbitration/properties/system, omitiendo el arbitraje. En ese caso, indica un **resultSource** de "servidor". También es posible que un servidor de juegos para volver a escribir (es decir, corregir o ajustar) póngala conjuntos de resultados, en los que **resultSource** "ajustar".

El **resultConfidenceLevel** es un entero entre 0 y 100 que indica el nivel de consenso en el resultado. 100 significa que todos los reproductores de acuerdo. 0 significa que se ha producido ningún consenso y un resultado se ha elegido básicamente de forma aleatoria de los que se envió. Cuando un servidor de juegos establece los resultados de arbitraje, Establece el **resultConfidenceLevel**-normalmente a 100, a menos que por alguna razón incluso no está seguro (por ejemplo, si está informando los resultados de su propio reproductor por el servidor de juegos procedimiento de votación). Establecer el **resultConfidenceLevel** también cuando los resultados se ajustan para reflejar la confianza en los resultados ajustadas.

Cuando el **resultSource** es "arbitraje" (y el **resultState** es "completed" o "partialresults"), los resultados proporcionados son una copia exacta de al menos uno de los resultados notificados por el Reproductor.

### <a name="4--returning-to-the-xbox-arena-ui"></a>4.  Volver a la interfaz de usuario de Arena Xbox

Cuando la coincidencia es a través de (o posiblemente en respuesta a solicitud de un jugador que se desea abandonar a la coincidencia en curso), presentan una opción para que el Reproductor volver a la interfaz de usuario de la Arena Xbox desde donde se unirán a la coincidencia. Esto puede realizarse desde una pantalla de resultados de la coincidencia posterior a la o cualquier otra interfaz de usuario final del juego.

Para volver a la interfaz de usuario de Arena, invocar el **returnUri** que se pasó desde el URI de la activación de protocolos por utilizando el `Windows::System::Launcher` clase. No olvide incluir el contexto de usuario.

La API de inicio se expone de forma ligeramente diferente a los juegos ERA que a los juegos de la plataforma Universal de Windows (UWP). La versión de la ERA de la API no permite el PFN que se proporcione, por lo que se puede omitir el PFN incluso si está presente en el URI de activación.

Este es el código de ejemplo para una ERA devolver al usuario a la interfaz de usuario de Arena.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, Windows::Xbox::System::User ^currentUser)
{
    auto options = ref new LauncherOptions();
    options->Context = currentUser;

    Launcher::LaunchUriAsync(returnUri, options);
}
```

Juegos UWP pueden establecer el **TargetApplicationPackageFamilyName** propiedad **LauncherOptions** para el PFN devuelto, si se ha proporcionado en el URI de la activación de protocolos. Que garantiza que se inicie la aplicación específica y que el usuario se dirige a la Store si la aplicación ya no está instalada.

Este es el código de ejemplo para una aplicación para UWP devolver al usuario a la interfaz de usuario de Arena.

```c++
void Sample::LaunchReturnUi(Uri ^returnUri, String ^returnPfn, User ^currentUser)
{
    auto options = ref new LauncherOptions();

    if (returnPfn != nullptr)
    {
        options->TargetApplicationPackageFamilyName = returnPfn;
    }

    Launcher::LaunchUriForUserAsync(currentUser, returnUri, options);
}
```

Después de invocar la interfaz de usuario de Arena, su título continúa ejecutándose, posiblemente en esa misma pantalla, esperando a otro evento de activación de protocolo. De este modo, si el jugador tiene otra coincidencia para reproducir, el título está listo para ir. Esto acelera la experiencia del usuario, como el Reproductor entra de coincidencia en coincidencia, cambiar entre el título y la interfaz de usuario de Arena Xbox.

### <a name="handling-errors-and-edge-cases"></a>Controlar errores y casos extremos

El flujo en la sección anterior describe la ruta de acceso maestra mediante la reproducción de una coincidencia. Hay muchos lugares donde podría obtener comportamientos inesperados, o es posible que surjan problemas. En esta sección se analiza una serie de posibles errores o casos extremos y proporciona recomendaciones para controlarlos en su título.

#### <a name="game-session-not-found"></a>No se encontró la sesión juegos

Si el título se inicia con una referencia de la sesión para una coincidencia y, a continuación, el cliente no recibe un error al solicitar esa sesión de MPSD, presentar un error para el usuario lo que significa que no se puede unir a la coincidencia.

#### <a name="user-attempts-to-join-a-match-that-has-been-played"></a>Usuario intenta unirse a una coincidencia que se han jugado

Si un usuario intenta unirse a una coincidencia después de que se han notificado los resultados, bloquear a dicho cliente inicie una nueva coincidencia y presentar al usuario con un error. Puede comprobar si se han registrado los resultados recorriendo en iteración los miembros de la sesión para ver si alguno tiene un `/members/{index}/arbitrationStatus` (`multiplayer_session_member::tournament_arbitration_status`) establecido en "completar".

#### <a name="game-clients-unable-to-establish-a-p2p-connection"></a>Clientes de juegos no se puede establecer una conexión de p2p

Si el título usa conexiones de persona a persona (p2p) entre los clientes para varios jugadores y no tiene soporte técnico para las retransmisiones, puede haber instancias donde los usuarios que coinciden son no se puede establecer una conexión de p2p.

Si el cliente es capaz de recuperar la sesión para la coincidencia, pero no se puede conectar a otros clientes, informar un resultado "draw" para cada equipo en `/members/{index}/properties/system/arbitration/results` (`multiplayer_session::set_current_user_member_arbitration_results()`). Esto indica que la coincidencia no ha tenido lugar a Xbox Live. También impide que las vulnerabilidades de seguridad donde los usuarios pueden avanzar a través de un torneo obligando a un error de conexión de p2p.

#### <a name="game-client-disconnects-mid-match"></a>Juego cliente desconecta coincidencia intermedio

Uno de los escenarios de error más común es cuando los clientes se desconectan mientras se reproduce una coincidencia. Según el estado de la coincidencia y la implementación, hay unas cuantas opciones para controlar este caso:

* Si la coincidencia de torneo es uno frente a uno, o si todos los miembros de un equipo se desconexión de los demás clientes o el servicio de juegos, se recomienda notificar una "pérdida" para el equipo que ya no está conectado. Esto evita que el caso en el que los usuarios pueden forzar una desconexión de una coincidencia de pérdida de para impedir que el resultado se notifica.
* Si la coincidencia distingue entre los equipos con varios miembros y desconectarse de un subconjunto de los clientes para un equipo intermedio coincidencia, puede elegir finalizar la búsqueda de coincidencias en ese momento o permitir que continúe con los restantes usuarios. Si decide continuar con la coincidencia, opcionalmente, también puede permitir a los usuarios desconectados para volver a unirse a la coincidencia.

#### <a name="full-teams-not-present"></a>Equipos completos no está presentes

Si el título es compatible con las coincidencias con los equipos de dos o más, puede encontrar casos donde algunos miembros de un equipo no se pudo iniciar el título de su coincidencia. En este caso, puede elegir permitir que a los miembros del equipo para reproducir sin su miembro del equipo y, opcionalmente, permitir que ese miembro del equipo para unirse a la coincidencia en curso.

Como alternativa, puede aplicar automáticamente un resultado. Si lo hace, esperar hasta la hora de forfeitTimeout para brindar una oportunidad para unirse a la coincidencia antes de aplicar el resultado de todos los participantes. Esto le permite ajustar la ventana con los cambios realizados en el modo de juego para el torneo en lugar de tener que actualizar el título.

#### <a name="length-of-match-exceeds-arbitrationtimeout"></a>Longitud de coincidencia supera arbitrationTimeout

Establezca el valor de **arbitrationTimeout** sea mayor que la longitud máxima de tiempo, posiblemente, puede tardar una coincidencia. Dicho eso, el título de la posible característica modos en que las longitudes de coincidencia posible son demasiado largas o en el que no hay ningún máximo. En estos casos, considere cualquiera:

* Play torneo limitación a los modos de sólo con una longitud fija máxima de tiempo.
* Para informar al usuario de la fecha límite y los informes de un resultado antes de la **arbitrationTimeout** según un factor de desempate en la coincidencia.

Dado que **arbitrationTimeout** caducidad se desencadenará un resultado automática para la coincidencia, no espere el completo **arbitrationTimeout** período antes de informar de un resultado a partir del título.  En su lugar, en un búfer de tiempo de compilación (por ejemplo, **arbitrationTimeout** -1000 ms), o usar un valor independiente para controlar la longitud máxima de coincidencia.

#### <a name="other-cases"></a>Otros casos

Para otros casos de error, la regla general es informar al usuario del error y el usuario regresará a la interfaz de usuario de Arena de Xbox.

## <a name="experience-requirements-and-best-practices"></a>Requisitos de experiencia y procedimientos recomendados

Porque Arena presenta solo el título con coincidencias individuales, se pueden introducir nuevos formatos con el tiempo sin tener que actualizar el título. Por lo tanto, la experiencia de usuario de la línea de base para un campo integrado título debe ser simple y lo suficientemente flexible para trabajar con la competencia de varios formatos.

Si lo desea, puede usar datos desde el centro de torneo en el título para que torneos más integrada parte de la experiencia de juego.  Esta funcionalidad puede encontrarse en 'xbox::services::tournaments'.  Con estas API tienen la capacidad de:
* Detalles del torneo expuesta en el título de la interfaz de usuario (nombre de torneo, nombre de equipo, etcetera.)
* Proporcionar una detección para torneos en su título
* Mantener a los usuarios en su título entre las coincidencias de Arena

## <a name="configuring-a-title-for-arena"></a>Configuración de un título de campo

Para habilitar un título para Arena, son necesarios algunos pasos adicionales cuando se configura en el Portal para desarrolladores de Xbox (XDP) o [centro de partners](https://partner.microsoft.com/dashboard).

### <a name="enabling-arena-for-your-title"></a>Habilitación de Arena para el título

Para habilitar la Arena, vaya a la página de configuración de servicio para el título en XDP y seleccione 'Campo'.

![](../../images/arena/arena-configure-xdp.png)

En este caso, tendrá varias opciones:

* **Arena habilitado** : Active esta casilla para habilitar el ámbito de este espacio aislado.
* **Características de arena** : esta sección incluyen las casillas de verificación para habilitar torneos generados por el usuario en el espacio aislado y para habilitar el juego, lo que permite a los usuarios en varias plataformas para participar en el mismos torneos cruzado.
* **Plataformas de arena** – le permite seleccionar las plataformas en la que se pueden reproducir torneos del título.
* **Activos de torneo** : (anteriormente en la sección "Participan varios jugadores y contactos".) Estas son las imágenes del torneo del título.

También se puede habilitar la arena en el centro de partners en el **torneo** menú en el servicio Xbox Live.

![Menú de arena en el centro de partners](../../images/arena/Arena_On_WDC.JPG)

Debe publicar la configuración del servicio para que los cambios surtan efecto. Configuración de ámbito de autoservicio no se admite a través de UDC. Si usas UDC de configuración del servicio, trabajar con su cuenta de director de desarrollo para incorporar con ámbito.

### <a name="setting-up-game-modes"></a>Configurar modos de juego

Modos de juego son una característica de Xbox Live que permite que el publicador preconfigurar la configuración para una coincidencia del torneo. Estos modos de juego se usan durante la creación del torneo para establecer las reglas aplicadas por su título para el torneo y Xbox Live. Como el publicador de título, necesita al menos un modo de juego para habilitar torneos generados por el usuario para el título. Elementos del modo de juego incluyen:

#### <a name="supports-ugt"></a>¿Admite UGT?

Modos de juego pueden habilitarse para su uso con torneos generados por el usuario (UGTs). Si el título está configurado para admitir UGT, puede marcar los modos de juego para que pueden seleccionarse por usuarios de Xbox Live al crear torneos del título.

#### <a name="team-size"></a>Tamaño del equipo

Este es el número de usuarios que pueden participar en el torneo como un equipo. Para un título de usuario único o un torneo, el modo de juego tiene un tamaño de equipo de uno. Campo no admite actualmente tamaños variables de equipo para torneos.

#### <a name="forfeittimeout"></a>forfeitTimeout

Este es el número de milisegundos, después de hora, antes de la coincidencia se cancele si no mostrar de los jugadores compensando de inicio de la coincidencia (es decir, si ningún reproductores propios establecido en activo en la sesión). Establezca esta opción en un período de tiempo que es al menos lo suficientemente largo como para que un jugador inicie su título y entrar en la coincidencia de torneo desde la hora verán la notificación del sistema.

#### <a name="arbitrationtimeout"></a>arbitrationTimeout

Este es el número de milisegundos, después de hora, después del cual no se aceptará ningún resultado más de inicio de la coincidencia. Establézcalo en el tiempo de espera de pérdida más posiblemente podría tardar una cantidad de tiempo mayor que la coincidencia más larga. De este modo, incluso si los encargados de empezar a reproducir justo antes de la hora de pérdida, aún tiene tiempo suficiente para finalizar a la coincidencia. Si se muestra ningún resultado en el momento de la **arbitrationTimeout**, los participantes de la coincidencia de todos los reciben una pérdida. Xbox Arena también espera hasta que el **arbitrationTimeout** para todos los miembros activos a los resultados del informe antes de comenzar el arbitraje.

Este tiempo de espera actúa como un mecanismo de seguridad en caso de algo sale mal y no se notifican un resultado para un reproductor. En lugar de estancamiento el torneo todo, este tiempo de espera coloca un límite superior en la cantidad de tiempo que esperará el torneo. Por ese motivo, que no debería establecerse demasiado alto, porque determina la longitud máxima del torneo de cada ronda.

#### <a name="name"></a>Nombre

Esto es el nombre de usuario para el modo de juego. Este valor es localizable.

#### <a name="description"></a>Descripción

Esta es la descripción de cara al usuario para el modo de juego. Este valor es localizable.

#### <a name="custom"></a>Personalizado

El **personalizado** sección para el modo de juego es una bolsa de propiedades donde los desarrolladores pueden insertar los valores de configuración específicos de título para la coincidencia del torneo. Los valores definidos como parte de la sección personalizada se escriben en la sesión MPSD de coincidencia en `/properties/custom/`.

Actualmente, no se admite la instalación en modo de juego a través de XDP o UDC. Para crear los modos de juego para el título, póngase en contacto con su administrador de cuentas de desarrollador.

### <a name="requirements-for-the-session-template"></a>Requisitos para la plantilla de sesión

Como desarrollador de título, debe proporcionar la plantilla de sesión para el campo para su uso al crear sesiones de coincidencias. Hay ciertas expectativas sobre el contenido de la plantilla.

```json
{
    "version": 1,
    "inviteProtocol": "tournamentGame",
    "memberInitialization": null,

    "capabilities": {
        "gameplay": true,
        "arbitration": true,

        "large": false,
        "broadcast": false,
        "blockBadMsaReputation": false
    },

    "maxMembersCount": {maxMembersCount},
}
```

Otras propiedades que no se muestran aquí se pueden establecer que desee.

Las sesiones de arena siempre son la versión 1. Establecer el **inviteProtocol** a "tournamentGame" permite que las notificaciones de coincidencia listos para enviarse a los participantes del torneo. **memberInitialization** se establece en nulo para deshabilitar QoS. La capacidad de juego debe establecerse porque la sesión representa a una coincidencia, y la capacidad de arbitraje es necesaria para informes de resultados. El **grandes**, **difusión**, y **blockBadMsaReputation** capacidades deben deshabilitarse porque interfieren con el funcionamiento de la sesión.

El título puede especificar su propia configuración en la sección de la plantilla de configuración que tienen un valor fijo para todos los torneos que usan la plantilla personalizada. Este es un ejemplo.

```json
        "custom": {
            "enableCheats": false,
            "bestOf": 3
        }
```

Por último, el **maxMembersCount** se requiere la configuración del sistema. Establézcalo en el número total de reproductores que puedan reproducir en una coincidencia del torneo. El número de jugadores puede restringirse aún más mediante la configuración del modo de juego, así que asegúrese de que el valor establecido en la sesión representa el número total más alto de reproductores admite para el título. Por ejemplo, si el número máximo de jugadores es compatible con su juego para una coincidencia es 5 vs. 5, establezca **maxMembersCount** a 10. Esta plantilla MPSD puede usarse para admitir las coincidencias de cualquier número de jugadores hasta el **maxMembersCount**.
