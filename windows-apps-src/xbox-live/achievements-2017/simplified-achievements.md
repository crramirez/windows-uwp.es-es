---
title: Logros de 2017
description: Logros de 2017
ms.assetid: d424db04-328d-470c-81d3-5d4b82cb792f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bfc67f6aca27abf095a89c451111e6429bca82e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590990"
---
# <a name="achievements-2017"></a>Logros de 2017

El sistema de logros 2017 permite a los desarrolladores de juegos usar un modelo de llamada directo para desbloquear los logros por nuevos juegos de Xbox Live en Xbox One, Windows 10, 10 de Windows Phone, Android e iOS.

## <a name="introduction"></a>Introducción

Con la Xbox One, se introdujo un nuevo sistema Cloud-Powered logros que permite a los desarrolladores de juegos para controlar los datos de sus capacidades de Xbox Live, como estadísticas de usuario, logros, presencia enriquecida y participan varios jugadores, basta con enviar eventos de telemetría del juego. Esto abre paso a un gran número de nuevas ventajas: un único evento puede actualizar los datos de varias características de Xbox Live; Configuración de Xbox Live se encuentra en el servidor en lugar de en el cliente; y mucho más.

En los años posteriores al lanzamiento de Xbox One, hemos atendido estrechamente a los comentarios de desarrolladores de juegos y los desarrolladores constantemente comparten las siguientes:

1.  **Deseo desbloquear logros a través de un patrón que realiza la llamada directo.** Muchos desarrolladores crean juegos para varias plataformas, incluidas las versiones anteriores de Xbox, y los sistemas de logro similares en estas plataformas utilizan un método directo que realiza la llamada. Soporte directo desbloquee las llamadas en Xbox One y otras plataformas de Xbox de generación actual podrían aliviar sus necesidades de desarrollo de juegos multiplataforma y los costos de tiempo de desarrollo.

2.  **Minimizar la complejidad de la configuración.** Con el sistema Cloud-Powered logros, un logro desbloquear lógica debe configurarse en Xbox Live para que los servicios sepan cómo interpretar los datos de estadísticas del título y cuándo se debe desbloquear la consecución de un usuario. Esto se realiza mediante una nueva sección de reglas de mención especial de configuración de un logro que no existía anteriormente. Aunque tener desbloquear lógica en la nube puede ser bastante eficaz, este requisito de configuración adicional agrega complejidad en el diseño y creación de los logros de un título.

3.  **Difícil de solucionar.** Si bien el sistema Cloud-Powered logros presenta una variedad de funciones útiles, también puede ser más difícil para el juego a los desarrolladores para validar y solucionar problemas con sus logros, puesto que desbloquea el logro desencadenados indirectamente por reglas que residen en el servicio en lugar de controlada directamente por el juego en Sí.

Conviene tener en cuenta que los desarrolladores de juegos también repetidamente y han compartido los comentarios que se aprecian valor ciertas características que se introdujeron junto con el sistema Cloud-Powered logros:

1.  Nuevas características de experiencia de usuario como la progresión de logro, actualizaciones en tiempo real, las recompensas de arte concepto y registro se desbloquea en fuente de actividades.

2.  Mejoras de la configuración como una configuración de servicio en lugar de una configuración local que debe incluirse en el paquete de juego (es decir, gameconfig, XLAST, SPA, etc.) y la capacidad de editar fácilmente la mención especial de cadenas & imágenes una vez se haya enviado el juego.

Con los logros de 2017, estamos creando un reemplazo del sistema existente de logros Cloud-Powered futuros títulos de para usar que facilita aún más para los desarrolladores de juegos de Xbox configurar los logros, integrar y logros de desbloquea las actualizaciones en el código de juegos y validar los logros que funciona según lo previsto.

## <a name="whats-different-with-achievements-2017"></a>¿Qué es diferente con los logros de 2017

|                          | Sistema logros 2017        | Sistema de logros basadas en la nube      |
|--------------------------|---------------------------------------|----------------------------------------|
| Desbloqueo de desencadenador           | Directamente a través de la llamada de API                 | De forma indirecta mediante eventos de telemetría        |
| Desbloquear propietario             | Título                                 | Xbox Live                              |
| Configuración            | Cadenas, imágenes, las recompensas              | Las cadenas, imágenes, premios, desbloquear reglas \[estadísticas + eventos\]                    |
| Progresión              | Se admite <br>*directamente a través de la llamada de API*                | Se admite <br> *de forma indirecta mediante eventos de telemetría*       |
| Actividad en tiempo real (ATR) | Se admite                             | Se admite                              |
| Retos               | No se admite   | Se admite                      |

## <a name="title-requirements"></a>Requisitos de título

Los siguientes son los requisitos de cualquier título que va a usar el sistema 2017 logros.

1.  **Debe ser un nuevo título (lanzado).** Los títulos que ya se han publicado y usan el sistema Cloud-Powered logros son aptos. Para obtener más información, consulte [¿por qué no títulos existentes "migración" en el nuevo sistema logros 2017?](#_Why_can’t_existing)

2.  **Debe usar el XDK de agosto de 2016 o versiones más recientes.** La API Update_Achievement se publicó en el XDK de agosto de 2016.

3.  **Debe ser un título XDK o UWP.** El sistema logros 2017 no está disponible para plataformas heredadas, incluidos Xbox 360, Windows 8.x o una versión anterior, ni Windows Phone 8 o una versión anterior.

## <a name="updateachievement-api"></a>Update_Achievement API

Una vez que se configuran sus logros a través de XDP o [UDC](../configure-xbl/dev-center/achievements-in-udc.md) y publicado en su espacio aislado de desarrollo, su título puede desbloquearlos mediante una llamada a la API Update_Achievement.

La API está disponible en el XDK y el SDK de Xbox Live.

### <a name="api-signature"></a>Firma de API

La firma de la API es como sigue:

```c++
/// <summary>
    /// Allow achievement progress to be updated and achievements to be unlocked.  
    /// This API will work even when offline. On PC and Xbox One, updates will be posted by the system when connection is re-established even if the title isn't running
    /// </summary>
    /// <param name="xboxUserId">The Xbox User ID of the player.</param>
    /// <param name="titleId">The title ID.</param>
    /// <param name="serviceConfigurationId">The service configuration ID (SCID) for the title.</param>
    /// <param name="achievementId">The achievement ID as defined by XDP or Partner Center.</param>
    /// <param name="percentComplete">The completion percentage of the achievement to indicate progress.
    /// Valid values are from 1 to 100. Set to 100 to unlock the achievement.  
    /// Progress will be set by the server to the highest value sent</param>
    /// <remarks>
    /// Returns a task<T> object that represents the state of the asynchronous operation.
    ///
    /// This method calls V2 GET /users/xuid({xuid})/achievements/{scid}/update
    /// </remarks>
    _XSAPIIMP pplx::task<xbox::services::xbox_live_result<void>> update_achievement(
        _In_ const string_t& xboxUserId,
        _In_ uint32_t titleId,
        _In_ const string_t& serviceConfigurationId,
        _In_ const string_t& achievementId,
        _In_ uint32_t percentComplete
        );
```

`xbox::services::xbox_live_result<T>` es la llamada de valor devuelta para todas las llamadas de API de C++ Xbox Live Services.

Para obtener más información, consulte la charla Xfest 2015, "XSAPI: C++, no hay excepciones!"<br>
[vídeo](https://go.microsoft.com/?linkid=9888207) |  [diapositivas](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest_2015/Xbox_Live_Track/XSAPI_Cpp_No_Exceptions.pptx)

### <a name="unlocking-via-updateachievement-api"></a>Desbloquear mediante Update_Achievement API

Para desbloquear un logro, establezca el *percentComplete* a 100.

Si el usuario está en línea, la solicitud se enviará inmediatamente al servicio logros de Xbox Live y desencadenará las experiencias de usuario siguientes:

-   El usuario recibirá una notificación logro desbloqueado;

-   El logro especificado aparecerá como desbloqueado en la lista del usuario mención especial para el título;

-   El logro desbloqueado se agregarán a la fuente de actividades del usuario.

> *Nota: No habrá ninguna diferencia visible en experiencias de usuario logros que usan el sistema logros 2017 y los logros Cloud-Powered.*

Si el usuario está sin conexión, la solicitud de desbloqueo se pondrá en cola localmente en el dispositivo del usuario. Cuando el dispositivo del usuario ha restablecido la conectividad de red, la solicitud se realizará automáticamente se envían al servicio logros: tenga en cuenta: no es necesaria el juego para desencadenar esta, ninguna acción y las experiencias de usuario anterior se producirá como se describe.

### <a name="updating-completion-progress-via-updateachievement-api"></a>Actualización del progreso de finalización a través de API Update_Achievement

Para actualizar el progreso de un usuario de desbloquear un logro, establezca el *percentComplete* adecuado número entero entre 1 y 100.

Solo puede aumentar el progreso de una mención especial. Si *percentComplete* está establecido en un número menor que el logro 's última *percentComplete* valor, se omitirá la actualización. Por ejemplo, si la consecución *percentComplete* previamente se haya establecido a 75, enviar una actualización con un valor de 25 se omitirá y la realización se mostrará como un 75% completado.

Si *percentComplete* está establecido en 100, se desbloqueará el logro.

Si *percentComplete* se establece en un número mayor que 100, la API se comportará como si se establece en 100 exactamente.

## <a name="frequently-asked-questions"></a>Preguntas frecuentes

### <a name="span-idwhyarechallenges-classanchorspancan-i-ship-my-title-using-the-achievements-2017-system-yet"></a><span id="_Why_are_Challenges" class="anchor"></span>¿Puedo enviar mi título aún utilizando el sistema logros 2017?

Por supuesto. Todos los nuevos títulos son esperada y recomendamos que haga uso de los logros de 2017 sistema en lugar del sistema Cloud-Powered logros.

### <a name="why-are-challenges-not-supported-in-the-achievements-2017-system"></a>¿Por qué no se admiten los desafíos en el sistema logros 2017?

Datos de uso en los juegos de Xbox ha demostrado que la implementación actual y la presentación de los desafíos no satisfacer una necesidad para la mayoría de los desarrolladores de juegos. Se continuará la recopilación de entrada de desarrollador y comentarios en este espacio y centrar sus esfuerzos proporcionar características futuras que están más en punto con las necesidades del desarrollador. La Xbox Arena recién lanzado es una dirección nuevo, pero de forma similar, de juegos de un ejemplo de una característica que introduce nuevas capacidades de competencia para Xbox.

### <a name="can-i-still-add-new-achievements-every-calendar-quarter-if-my-title-is-using-the-achievements-2017-system"></a>¿Todavía puedo agregar nuevo logros cada trimestre del calendario si mi título está usando el sistema logros 2017?

Sí. No se modifica la directiva logros.

### <a name="span-idwhycantexisting-classanchorspanwhy-cant-existing-titles-migrate-onto-the-new-achievements-2017-system"></a><span id="_Why_can’t_existing" class="anchor"></span>¿Por qué no títulos existentes "migración" en el nuevo sistema logros 2017?

Para la mayoría de los títulos de existentes, 'migración' en el sistema logros 2017 no se limita a simplemente actualizando sus configuraciones de servicio e intercambio de escrituras de evento por logros desbloquear llamadas, aunque estos cambios solo sería muy costoso y llevaría a un riesgo importante de error y un comportamiento imprevisto que podría dar lugar a los logros irreparablemente interrumpirse constantemente. En su lugar, la mayoría de los títulos existentes también tienen los usuarios con los datos existentes. Intenta convertir un juego en vivo que ya está usando los logros Cloud-Powered sistema podría no sólo es un esfuerzo muy costoso, para el desarrollador y la Xbox, sino significativamente en peligro los perfiles o experiencias de juego de los usuarios existentes.

### <a name="if-my-title-was-released-using-the-cloud-powered-achievements-system-can-any-future-dlc-for-the-title-switch-to-achievements-2017"></a>¿Si Mi título se publicó con el sistema Cloud-Powered logros, puede cambiar cualquier futura DLC para el título a logros 2017?

Todos los logros por un título deben usar el mismo sistema logros. Usa cualquier sistema logros logros del juego base es el sistema que se debe utilizar para todos los logros futuras para el título.

### <a name="while-testing-achievements-in-my-dev-sandbox-can-i-mix-and-match-between-using-the-achievements-2017-system-and-the-cloud-powered-achievements-system"></a>Durante las pruebas logros en mi espacio aislado de desarrollo, ¿se pueden mezclar y coincidencia entre el uso del sistema logros 2017 y el sistema Cloud-Powered logros?

No. Todos los logros por un título deben usar el mismo sistema logros.

### <a name="does-achievements-2017-also-include-offline-unlocks"></a>¿Logros 2017 también incluye sin conexión desbloquea?

Si el título desbloquea un logro mientras el dispositivo está sin conexión, la API Update_Achievement automáticamente poner en cola las solicitudes de desbloqueo sin conexión y también lo hará la sincronización automática de Xbox Live cuando el dispositivo ha restablecido la conectividad de red, similar a la actual Experiencia de basadas en la nube logros del sistema sin conexión. Logros desbloquea will no se producen mientras el usuario está sin conexión.

### <a name="i-see-a-new-achievementupdate-event-in-xdp-if-my-title-uses-that-event-does-that-mean-it-has-achievements-2017"></a>Veo un nuevo evento "AchievementUpdate" XDP. ¿Si Mi título utiliza ese evento, significa tiene logros 2017?

El *AchievementUpdate* evento base requiere Xbox Live para fines de back-end. Se puede ignorarlo. Si el título configura un evento con este tipo de evento base, se omitirán esas operaciones de escritura de eventos por Xbox Live. Los títulos que se basan en el sistema Cloud-Powered logros deben continuar configurar sus eventos con los otros tipos de evento base. No necesitan configurar los títulos que se basan en el sistema logros 2017 *cualquier* eventos para fines de cumplimiento.
