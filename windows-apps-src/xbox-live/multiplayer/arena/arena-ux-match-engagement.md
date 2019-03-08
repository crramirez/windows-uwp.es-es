---
title: Contratación de coincidencia
description: Describe las fases de la experiencia de usuario de jugadores progresa a través de una experiencia de torneo.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, arena, torneo, experiencia de usuario
ms.localizationpriority: medium
ms.openlocfilehash: edc81ff7c692c4789855d341917c54acf4d24594
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598360"
---
# <a name="match-engagement"></a>Contratación de coincidencia

Después de que un jugador está registrado y busca un torneo, está listo para jugar. Puede ser un participante en una de cuatro fases. Cada fase contiene un único conjunto de criterios guiar a los usuarios a través de la experiencia de juego torneo. Las cuatro fases son:

1.  Listo (ámbito de la interfaz de usuario o en el juego)
2.  (En el juego)
3.  Resultados (ámbito de la interfaz de usuario o en el juego)
4.  Final (ámbito de la interfaz de usuario o en el juego)

> [!NOTE]  
> Reproducción es la única fase de los cuatro que no se admiten en la interfaz de usuario de Arena. Como mínimo, el título debe redirigir a participantes a la interfaz de usuario de ámbito al final de cada coincidencia (cuando es el escenario de juego a través de), para que puedan ver los resultados.

###### <a name="diagram-the-four-stages-of-participant-progression-after-check-in"></a>Diagrama: Las cuatro fases del participante progresión, después de protegerlo.

![Diagrama de flujo de contratación de coincidencia](../../images/arena/arena-ux-match-flow.png)

![Barra de flujo de contratación de coincidencia](../../images/arena/arena-ux-match-flow-bar.png)

## <a name="1-ready-waiting-for-match"></a>1. Listo ("esperando coincidencia")

![Esperando al diagrama de coincidencia](../../images/arena/arena-ux-flow-ready.png)

### <a name="user-impact"></a>Impacto en el usuario

En esta fase, el jugador espera reproducir en una coincidencia, pero aún no se conoce el oponente. Esta fase se desencadena cuando se ha iniciado un torneo y el participante está esperando su sesión de coincidencia comenzar. Este período de espera podría deberse a que:

* El período en el repositorio todavía está abierto.
* No se ha iniciado la siguiente ronda.
* El participante tiene un bye actual de ida y vuelta.

### <a name="when-a-bye-is-needed"></a>Cuando se necesita un bye

Un bye puede ocurrir en cualquier ciclo de un torneo, aunque se expanden horizontalmente para intentar evitar la propagación.

Cada vez que la eliminación de una único o doble eliminación torneo implica varios jugadores o equipos que no es una potencia de 2 (es decir, 4, 8, 16, 32, 64, 128 o 256), deben concederse bytes. Un bye simplemente significa que un equipo no tiene que participan en la primera ronda del torneo y en su lugar obtiene un pase gratuito para la segunda ronda.

### <a name="discovery"></a>Detección

![Captura de pantalla de detección de equipo](../../images/arena/arena-ux-team-discovery.png)

En la fase de la lista, los participantes conozca su estado en estos lugares:

* Arena UI: Página de detalles del torneo
* En el juego: Lista de torneo, promoción, introductoria de coincidencia

> [!TIP]  
> **Recomendaciones de la experiencia del usuario**  
> Indicar que el Reproductor está esperando su siguiente coincidencia en el evento. Ejemplos: "Pendiente coincide con" o "Ha recibido un bye" notificación.  
>
> Si se admite esta fase, proporcionan un método para volver a la página de detalles del torneo.  
>
> Dar el contexto del evento: Nombre torneo, estado, estilo, descripción, inicio/fin de tiempo hora, etcetera.


###### <a name="ui-example-in-game-multiplayer-lobby"></a>Ejemplo de la interfaz de usuario: En el juego multijugador introductoria

![Captura de pantalla de torneo introductoria](../../images/arena/arena-ux-tournament-lobby.png)

Siempre presente el torneo de estado y razón en una posición coherente, donde se promueve una coincidencia de torneo o los detalles de la coincidencia se presentan.

Se trata de instrucciones críticas para jugadores cuando deciden debe:

* Para dejar el juego para obtener más información.
* ¿Qué pasos son necesarios para mientras se encuentra en un torneo de progreso.

### <a name="under-the-hood"></a>En segundo plano
Si esta fase es compatible con el título, pasar a la fase de reproducción cuando la coincidencia está lista.

 
## <a name="2-playing"></a>2. Reproducción en curso

![Reproducir el diagrama de coincidencia](../../images/arena/arena-ux-flow-play.png)

### <a name="user-impact"></a>Impacto en el usuario

Se conoce la siguiente coincidencia-arriba del Reproductor y se ha creado la sesión de varios jugadores. Durante esta fase, el título mueve a los jugadores a través del flujo normal de juego (pantallas de instalación de coincidencia, introductoria de juegos etc.). Se trata de la fase que llegan los usuarios cuando la coincidencia está lista.

### <a name="discovery"></a>Detección

Hay dos formas de un participante del torneo de alerta y habilitarlas para unirse a una coincidencia de torneo desde fuera de un juego:

* **Notificación del sistema Xbox** : cuando aparezca la notificación, el participante puede contener el ![botón nexus](../../images/xbox-nexus.png) (botón Nexus) para iniciar el juego y escriba una coincidencia.

  ![Coincide con el del sistema listo](../../images/arena/arena-ux-match-ready-toast.png)

* Página de detalles del torneo de arena: puede seleccionar el participante **reproducir** para iniciar el juego y escriba una coincidencia
 
> [!TIP]  
> **Recomendación de la experiencia del usuario:** Confirme que la entrada de coincidencia.  
> Mostrar la interfaz de usuario que informa a y/o confirma que el participante está a punto de entrar directamente en una sala de coincidencia.

###### <a name="ui-example-match-entry-confirmation-dialog-box"></a>Ejemplo de la interfaz de usuario: cuadro de diálogo de confirmación de entrada coincide con

![Cuadro de diálogo de confirmación de entrada de coincidencia](../../images/arena/arena-ux-confirm-match-entry.png)

Mostrar esta interfaz de usuario una vez que elige el participante que escriba un torneo desde una ubicación fuera del juego. Haga esto una vez se ha lanzado un juego y antes de entrar en el vestíbulo de la coincidencia.

> [!TIP]  
> **Recomendación de la experiencia del usuario:** Ofrece una opción para cancelar.

Los participantes pueden decidir hay otras tareas que deben realizar para preparar el torneo. Es importante para proporcionarles la flexibilidad de cambiar de opinión. El título puede ofrecer opciones adicionales:

* Permanezca en el juego. (El participante se toma en el menú principal).
* Volver a la interfaz de usuario de Arena.
* Si el participante opta por dejar el juego, se recomienda que el título de informe al participante de que el tiempo restante antes de que se alcanzó el límite de tiempo de espera de pérdida.

###### <a name="ui-example-match-entry-confirmation--leave-game-warning"></a>Ejemplo de la interfaz de usuario: coincide con la confirmación de entrada: deje advertencia juegos

![Cuadro de diálogo de confirmación de pérdida de entrada de coincidencia](../../images/arena/arena-ux-confirm-match-cancel.png)

### <a name="playing-a-match"></a>Reproducción de una coincidencia

La hora de inicio para la coincidencia se define en la plantilla de sesión. Se establece como una fecha y hora en formato UTC estándar, por ejemplo, `2009-06-15T13:45:30.0900000Z`. El organizador del torneo puede crear la sesión como anticipación a la hora de inicio como desee, incluido simultáneamente con la hora de inicio. Coincidencia notificaciones se envían a los participantes de coincidencia a la hora de inicio para no activar títulos nunca antes de la hora de inicio. En general, trate la sesión de Arena del mismo modo trataría su sesión de varios jugadores del título. Sin embargo, hay algunas diferencias:

* El título no puede cambiar la lista de la sesión de juego.
* No existe el proceso de investigación habitual de los jugadores con velocidades de conexión problemática.
* El título debe participar en el arbitraje.

### <a name="match-lobby-details"></a>Detalles de lobby de coincidencia

Vestíbulo coincidencia torneo debe incluir los detalles que dejan claro que es un torneo, y que es diferente de una coincidencia de varios jugadores estándar. Ejemplos de estos detalles son tiempo, dependencia de equipo y redondea consecutivo y fases. Cuando finaliza la coincidencia, continúe con la fase 3 (resultados en espera) o devolver el Reproductor a la interfaz de usuario de Arena.

###### <a name="ui-example-match-lobby-details"></a>Ejemplo de la interfaz de usuario: Detalles de Lobby de coincidencia

![Pantalla de detalles de lobby de coincidencia](../../images/arena/arena-ux-match-lobby-details.png)

**Un** -reforzar que se trata de una coincidencia "Torneo" o "Ámbito".  
**B** -torneo detalle  
   1. Nombre del torneo (máximo 128 caracteres)  
   2. Estilo, de modo de juego  
   3. Round o número de fase  

**C** : ¿estás listo registrarte / participantes de botón de reproducción: es posible que ha escrito un torneo, pero requiere más tiempo antes de comenzar la sesión de coincidencia. Por ejemplo, una coincidencia posible requieren configuración, como la selección de caracteres. Esta capacidad puede informar a los miembros del equipo cuando estén listas.  
**D.** -temporizador Regresivo
  * El temporizador se basa en el **límite de tiempo de espera de pérdida** (establecida por el juego).
  * Esto advierte de los equipos que, si no se cumplen los requisitos mínimos del equipo en este momento, se perderá el juego y wins equipo activo.  

**E** : difusión recordatorio  
**F** -nombre del equipo y participantes

 
### <a name="under-the-hood"></a>En segundo plano

#### <a name="protocol-activation"></a>Activación de protocolos

La interfaz de usuario de Arena inicia las cosas mediante el envío de los participantes directamente en un título cuando un usuario está listo para reproducir una coincidencia, por ejemplo, cuando acepte el aviso "¿listos de coincidencia" enviado por el campo. Activación de protocolos puede producirse en dos veces: cuando se inicia el título, o cuando ya se está ejecutando. En ambos casos, la activación es similar a lo que sucede cuando se activa un título en respuesta a un usuario aceptar la invitación al juego.

* **Si el título que se va a iniciar** --la **Activated** evento se desencadena por primera vez cuando se inicia el título. Si esa activación inicial es una activación de protocolos de Arena, significa que se inició el título de un usuario intenta reproducir un torneo. El título debe omitir tan pronto como sea posible a la coincidencia, omitiendo el menú principal y las pantallas de inicio de sesión. El Id. de usuario de Xbox (XUID) del usuario que participan se proporciona en el URI de la activación y estar ya ha iniciado sesión.

* **Si ya se estaba ejecutando el título** --la **activación** también pueden activar eventos con una activación de protocolos de Arena mientras ya se está ejecutando el título. Si el título está inactivo cuando recibe la activación de protocolos, debe pasar inmediatamente a la coincidencia del torneo. Es seguro hacerlo porque se desencadena el evento de activación sólo mediante una acción explícita del usuario (que actúan en una notificación del sistema que conduce a la coincidencia o pasar a la coincidencia de la interfaz de usuario de Arena). Si el título recibe la activación de protocolo durante el juego, debe proporcionar al usuario la opción de dejar el juego (guardarlo en primer lugar, si es necesario) y escriba la coincidencia del torneo de la forma más expedita posible.

* **Si un participante omite la notificación del sistema de Xbox** : se recomienda que un título de Arena habilitado siempre consulta Arena durante el inicio para ver si el usuario está en una coincidencia activa. Si por lo tanto, el título debe presentar "confirmar la coincidencia de entrada" interfaz de usuario, para indicar al usuario que tienen una coincidencia y pregunte si desea volver a escribirla.  

  Esto garantiza que los participantes realizar la transición hacia una coincidencia del torneo correctamente, en caso de que pierda la coincidencia de notificación del sistema y basta con inician el juego (se esperaba en su coincidencia). O bien, si acaso el juego se bloquea y el participante simplemente vuelva a iniciarse el título a regresar a su coincidencia.

> [!NOTE]  
> El título debe comprobar el XUID en el URI de la activación de protocolos. Si no coincide con el Reproductor actual del título, el título también debe cambiar contextos de usuario.

#### <a name="forfeit-time-out"></a>Tiempo de espera de pérdida

Al menos uno de los jugadores debe estar activo en la sesión antes de la hora de pérdida, que es la hora de inicio más el tiempo de espera de pérdida (consulte el diagrama de reproducción en curso). Si nadie se ha unido a la sesión como activa antes de la hora de pérdida, la coincidencia se cancela y ambos equipos reciben una pérdida a través de arbitraje de Arena.
 
## <a name="3-results"></a>3. Results

![Resultados de la coincidencia de diagrama](../../images/arena/arena-ux-flow-results.png)

Se trata de la fase cuando un jugador ha completado a la coincidencia y se han notificado los resultados a ser arbitrated. En este momento, se ha aún no se ha determinado si el equipo está todavía en el torneo. Mientras tanto, la experiencia de pantalla de "juego final" debe continuar informa sobre los resultados de la sesión (al igual que en cualquier coincidencia multijugador).

Los resultados del torneo, ningún concurso (no hay ningún juego), Win, las pérdidas, Draw, rango o No mostrar, seguirá.

Es posible que esta fase se omite si el resultado está disponible inmediatamente.

### <a name="reporting-results"></a>Informar de los resultados

Los resultados de la coincidencia se notifican al campo y el organizador del torneo a través de la sesión mediante el uso de una característica denominada *arbitraje*. Arbitraje es un marco para usar una sesión de coincidencia para reproducir de forma segura una coincidencia y un resultado del informe.

Cuando se inicia una sesión de coincidencia, se considera una sesión arbitrarios. Tiene una escala de tiempo fijo, lo que exige el marco de arbitraje.
 
### <a name="arbitration-time-out"></a>Tiempo de espera de arbitraje

El tiempo de espera de arbitraje es la cantidad máxima de tiempo, después de la coincidencia de hora de inicio, que los participantes deben reproducir y notificar los resultados. Este valor se establece en la plantilla de sesión de eventos de ejecución de organizador de torneo y en el modo de juego multijugador de Arena para UGT y eventos de franquicia de ejecución, lo que puede ser todo el tiempo que necesita su título.

El título puede notificar los resultados en cualquier momento entre la hora de inicio y la hora de arbitraje. En cualquier momento entre el momento de la pérdida y la hora de arbitraje, después de cada miembro activo de la sesión ha enviado los resultados se produzca el arbitraje. Por ejemplo, si un solo miembro está activo en la sesión en el momento de la pérdida, que puede (y debe) expongan un resultado y se producirá el arbitraje. Independientemente de cuántos resultados están disponibles en el momento de arbitraje, arbitraje se producirá si no es así ya.

Si un usuario pasa a estar en una sesión que ya se han arbitrated (porque ha expirado el tiempo de espera de arbitraje, un servidor de juegos arbitra la sesión o el usuario se une en tiempo de ejecución), el título debe finalizar la búsqueda de coincidencias y mostrar el resultado arbitrarios al usuario.

Resultados de arbitraje siempre incluyen el resultado de cada equipo. Cuando se notifica la puntuación de un jugador individual, incluye no solo los resultados de su equipo, pero el conjunto completo de resultados para cada equipo.
Arbitraje comienza automáticamente tan pronto como el primer cliente informa de los resultados a Xbox Live. Finaliza en cuanto todos los clientes informan de resultados. Puede haber instancias donde los participantes de reproducción que superan el tiempo de arbitraje especificado en la plantilla de sesión. Esto supone un riesgo de que una coincidencia está obligada a finalizar antes de que los participantes están listos.

> [!TIP]  
> **Recomendación de la experiencia del usuario:** Proporcione un período de tiempo fijo de sesión de coincidencia.
>
> Límites de tiempo predefinidos en sesiones de coincidencia se reducirá el riesgo de problemas que se producen en tiempo de espera de arbitraje.

Hay dos opciones para evitar una coincidencia finalice antes de que se realizan los participantes de reproducción:

* Establecer un período de tiempo más amplio de tiempo de espera de arbitraje.
* Si un período de tiempo fijo entra en conflicto con el estilo de juego de su título, pruebe la solución siguiente:
   1.   Determinar que no se han notificado los resultados y que el tiempo de espera de arbitraje está a punto de expirar (por ejemplo, se dejan los cinco minutos).
   2.   Mostrar la interfaz de usuario que alerta a los participantes de su juego está a punto de finalizar en X minutos. Esta opción se puede implementar para todos los torneos y, si el tiempo de espera de arbitraje es más amplio, la mayoría de los participantes nunca lo verá.

### <a name="returning-to-the-arena-ui-by-using-a-dedicated-a-button-press"></a>Devolver la interfaz de usuario de Arena utilizando una dedicado presionar un botón

Si el título no expuesta resultados del torneo o progresión de fase en juego, se recomienda que el participante tienen una manera de volver a la interfaz de usuario que ofrece esta información. Podría tratarse de la interfaz de usuario de campo o una aplicación de organizador del torneo de terceros. Idealmente, esta prestación aparecería después de la coincidencia es a través de, o posiblemente en respuesta a solicitud de un jugador que se desea abandonar a la coincidencia en curso. Esto puede realizarse desde una pantalla de informe final del juego.

> [!TIP]  
> **Recomendación de la experiencia del usuario**  
> Dedique un botón del dispositivo como una forma rápida de jugadores devolver en el centro de Arena en la interfaz Xbox.  
>
> ![Botón de controlador de concentrador de arena](../../images/arena/arena-ux-arena-hub-button.png)

### <a name="redirect-participants-at-the-end-of-a-match"></a>Participantes de la redirección al final de una coincidencia

Cuando la coincidencia completan de los participantes, es importante que proporciona el título de borrar la dirección en los pasos siguientes. Esto incluye un método para revisar el redondeo de torneo o resultados de la fase (si no presenta en el juego).

> [!TIP]  
> **Recomendación de la experiencia del usuario:** Utilice una superposición emergente.
>
> Esta interfaz de usuario aparece cuando torneo resultados están en y la experiencia de juego final ha finalizado.

### <a name="recommended-ui-data"></a>Datos de la interfaz de usuario recomendadas:

* Nombre del torneo
* Corchete siguiente
* Detalles de fase o round siguientes
* Pendiente de equipo
* Resultados arbitrarios
* Vínculo profundo a los detalles del torneo
* Opción para mantener el juego

### <a name="under-the-hood"></a>En segundo plano

Después de invocar la interfaz de usuario de Arena, debe continuar el título de la ejecución, posiblemente en esa misma pantalla, esperando a otro evento de activación de protocolos. A continuación, si el jugador tiene otra coincidencia para reproducir, el título estará listo para ir. Puede acelerar la experiencia del usuario como el Reproductor entra de coincidencia en coincidencia, cambiar entre el título y la interfaz de usuario de Arena.

###### <a name="ui-example-tournament-arbitrated-resultswinning-team"></a>Ejemplo de la interfaz de usuario: Torneo arbitrated resultados: ganar team

![Ha ganado la pantalla](../../images/arena/arena-ux-won-game-redirect.png)

###### <a name="ui-example-end-of-tournament-resultsmatch-ended"></a>Ejemplo de la interfaz de usuario: Los resultados finales del torneo, coincidencia finalizada 

![Ha perdido la pantalla](../../images/arena/arena-ux-lost-game-redirect.png)

## <a name="4-end"></a>4. Fin

![Final del diagrama de coincidencia](../../images/arena/arena-ux-flow-end.png)

La fase final de un flujo de torneo se produce cuando ha finalizado el torneo de sí mismo y se notifican los resultados finales.

Los participantes le informa de que el torneo sobre o que se ha eliminado el Reproductor.

### <a name="discovery"></a>Detección

La interfaz de usuario de Arena muestra los resultados y se celebra la Campeones:

* Página de detalles del torneo
* Resultados compartidos por los participantes de su fuente de actividades Reproductor o clubes
* Resultados del torneo registrados al perfil de un jugador

El título puede surgir torneo resultados en el juego:

* Después de la experiencia de final de juego para la Ronda final el participante completado.
* Anuncie **torneos mi**, en una característica de torneo Examinar.


###### <a name="ui-example-end-of-tournament-resultstournament-ended"></a>Ejemplo de la interfaz de usuario: Final de resultados del torneo, torneo finalizada

![Torneo a través de la pantalla](../../images/arena/arena-ux-tournament-completed.png)

* Si se admite esta fase, proporcionan una opción para volver a la interfaz de usuario de Arena o una aplicación de organizador del torneo.
* Mostrar el nombre del evento.
* Mostrar el nombre de equipo, si el equipo tiene más de un miembro.
* Mostrar permanente del equipo en el evento, si está disponible. Si no está disponible ningún permanente (o si lo desea, además de la situación), el título debe indicar que la participación del jugador en el evento ha llegado a su fin.
* El título sugiere pasos (por ejemplo, "Ver secuencias en directo", "Encontrar otro torneo,", "permanecen en el juego").
* El título proporciona un motivo por qué ha finalizado la interacción del equipo:
  * Rechazado: el equipo no cumplen los requisitos para escribir el torneo.
  * Eliminado, el equipo ha perdido la coincidencia del torneo.
  * Expulsar: el equipo se ha quitado de un torneo activo.
  * Completado, el equipo ha completado la Ronda final del torneo.
