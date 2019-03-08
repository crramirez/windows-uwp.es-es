---
title: Introducción al sistema de varios jugadores
description: Proporciona una introducción de alto nivel para el sistema de 2015 de varios jugadores de Xbox Live.
ms.assetid: d025bd2b-2ca4-4ba9-9394-4950d96ad264
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, 2015 multijugador
ms.localizationpriority: medium
ms.openlocfilehash: 4a739aaa650a7086dfe58b1b8ca170e15b3b2ef0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629420"
---
# <a name="introduction-to-the-multiplayer-system"></a>Introducción al sistema de varios jugadores

| Nota                                                                                                                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| En este artículo es para uso avanzado de API.  Como punto de partida, eche un vistazo a la [API del Administrador de varios jugadores](../multiplayer-manager.md) que simplifican enormemente el desarrollo.  Comunique su madre si encuentra un escenario no admitido en el Administrador de varios jugadores. |

Este documento contiene las siguientes secciones
* Acerca del sistema para varios jugadores
* Las arquitecturas, Interfaces y componentes para varios jugadores
* Entidades compatibles con varios jugadores de 2015
* Terminología de varios jugadores
* Novedades de varios jugadores de 2015
* Diferencias entre la consola Xbox 360 y Xbox MPSD una sesión funciones

## <a name="about-the-multiplayer-system"></a>Acerca del sistema para varios jugadores


2015 participan varios jugadores optimiza el uso directo de MPSD y las sesiones de juego. Proporciona una mejor compatibilidad con varias sesiones simultáneas para un solo título o el usuario y actualiza la API de servicios de Xbox (XSAPI) para activar los títulos para:

-   Anunciar la actividad actual y la disponibilidad para las combinaciones de un usuario.
-   Enviar invitaciones a las sesiones, junto con las cadenas de contexto de usuario visible (especificado por el título).
-   Detectar y unirse a sesiones a través del código de título.
-   Mantener las conexiones de socket web a MPSD para que puedan recibir notificaciones breves (derivaciones hombro) en la sesión cambia, por ejemplo, las actualizaciones que reflejan las suscripciones para cambiar los eventos y los cambios de estado de conexión. MPSD también usa conexiones de socket web para detectar rápidamente y actuar de desconexión del cliente.
-   Use SmartMatch contactos.

## <a name="multiplayer-components-interfaces-and-architectures"></a>Las arquitecturas, Interfaces y componentes para varios jugadores

### <a name="components-of-2015-multiplayer"></a>Componentes de varios jugadores de 2015

Varios jugadores es un sistema que consta de varios componentes. Es lo suficientemente flexible como para permitir que otros componentes, como servidores dedicados y sistemas de contactos externos.
#### <a name="multiplayer-session-directory-mpsd"></a>Directorio de sesión multijugador (MPSD)


Directorio de sesión de varios jugadores (MPSD) es un servicio que contiene una colección de sesiones. Una sesión se define como un documento protegido que se encuentran en la nube y que representa un grupo de personas un juego. Para obtener más información MPSD, consulte [directorio de sesión de varios jugadores (MPSD)](multiplayer-session-directory.md).


#### <a name="multiplayer-apis"></a>API de varios jugadores

Varios jugadores 2015 proporciona una API de WinRT participan varios jugadores, que se implementan a través de la **Microsoft.Xbox.Services.Multiplayer Namespace**. Sus componentes incluyen el **MultiplayerService clase**, definir un servicio web que ajusta MPSD.

También incluye una API de REST de varios jugadores. Define los objetos URI y JSON que son llamados por los métodos de API de WinRT. Puede usar la funcionalidad REST en las llamadas directas de los títulos, pero se recomienda para tener acceso a MPSD indirectamente a través de la API de WinRT. Para obtener más información, consulte **MPSD una llamada a**.

#### <a name="xbox-party-system"></a>Sistema de terceros de Xbox

En el modo multijugador 2015, el sistema de terceros de Xbox admite sólo las partes de chat como entidades externas. Los títulos pueden interactuar con el sistema de terceros para detectar la pertenencia de las partes de chat. Para obtener más información, consulte **partes compatibles con varios jugadores 2015**.

El sistema de terceros ahora es compatible con los juegos directamente a través de la sesión MPSD, en lugar de usar una entidad de juego. Es el título a usar sesiones para habilitar operaciones tales como la interacción del miembro, extracción de los jugadores en un juego como espacio disponible y compromiso de los usuarios durante los períodos de espera.


### <a name="2015-multiplayer-interfaces"></a>Interfaces de varios jugadores de 2015

Varios jugadores 2015 usa varias interfaces a otros componentes de Xbox.
#### <a name="xbox-secure-sockets"></a>Sockets seguros de Xbox

Multijugador 2015 admite la comunicación de red de bajo nivel entre los dispositivos de uso de sockets seguros de Xbox y Winsock. Las funciones de red usa seguridad de protocolo de Internet (IPSec) para permitir que los títulos proporcionar la asociación de dispositivos segura. Consulte **información general de redes** para obtener detalles de la consola Xbox característica sockets de seguros.


#### <a name="xbox-live-real-time-activity-service"></a>Servicio de Xbox Live actividad en tiempo real

Usa varios jugadores 2015 el **servicio de la actividad en tiempo real** permitir que los títulos para suscribirse a MPSD sesión cambia y habilitar la detección automática del cliente se desconecta. Se proporciona más información en [servicio de la actividad en tiempo real (ATR)](../../real-time-activity-service/real-time-activity-service.md).


#### <a name="xbox-live-matchmaking-service"></a>Servicio Xbox Live

El **servicio** forms grupos desde reproductores, según sus preferencias y datos y la información proporcionada durante la SmartMatch contactos. Para obtener detalles de varios jugadores de uso de este servicio, consulte [SmartMatch contactos](smartmatch-matchmaking.md).


#### <a name="xbox-live-reputation-service"></a>Servicio de reputación en vivo de Xbox

El *servicio de reputación* administra las estadísticas de usuario acerca de la reputación y durante el filtrado de reputación de contactos. Se usa por 2015 participan varios jugadores a través de SmartMatch contactos. Para obtener más información, consulte [reputación](../../social-platform/people-system/reputation.md).


#### <a name="xbox-live-compute"></a>Proceso de Xbox Live

Xbox Live Compute proporciona en la nube con varios jugadores de 2015 de títulos de la potencia de procesamiento. Para obtener más información sobre el uso de Xbox Live Compute, consulte [usar Xbox Live Compute en el modo multijugador](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer).


### <a name="typical-2015-multiplayer-architectures"></a>Arquitecturas de varios jugadores típicas de 2015

Esta sección presenta las arquitecturas de varios jugadores más habituales de 2015.
#### <a name="peer-to-peer-architecture"></a>Arquitectura de punto a punto

En la arquitectura de punto a punto, el título usa MPSD y SmartMatch contactos para detectar direcciones del mismo nivel. Las direcciones, a continuación, se usan para conectar los equipos del mismo nivel con Xbox de sockets seguros. Para obtener más información, consulte *Introducción a Winsock en Xbox One*.

![Diagrama de arquitectura de punto a punto](../../images/multiplayer/Mult2015ArchPeer.png)


#### <a name="client-server-architecture"></a>Arquitectura de cliente / servidor

En la arquitectura cliente / servidor con varios jugadores, el título usa MPSD y SmartMatch contactos para detectar las direcciones de servidor dedicado. A continuación, se usan para conectarse a un servidor dedicado con Xbox de sockets seguros. Para obtener más información, consulte *Introducción a Winsock en Xbox One*.

| Nota                                                                         |
|-------------------------------------------------------------------------------------------|
| Las instancias de Xbox Live Compute pueden usarse como servidores en la arquitectura cliente / servidor. |

![Diagrama de arquitectura cliente-servidor](../../images/multiplayer/Mult2015ArchClientServer.png)


## <a name="parties-supported-by-2015-multiplayer"></a>Entidades compatibles con varios jugadores de 2015
Varios jugadores 2015 para Xbox One no exponen la entidad"juego" como una construcción de nivel de sistema. Sin embargo, es compatible con la entidad"chat" en el nivel de sistema, como se muestra en 2014 participan varios jugadores.

| Nota                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------|
| Los títulos pueden implementar las experiencias de usuario similares a las implementa con la entidad de juegos, pero con sesiones MPSD en su lugar. |


### <a name="chat-party"></a>Entidad de chat

Una entidad de chat es un grupo de personas que están conversando con entre sí, administrado por el usuario a través de la aplicación de terceros una Xbox. Los usuarios pueden ser en una entidad de chat mientras se reproduce en una sesión de juego o mientras se realiza otra actividad de la consola. Sin embargo, hay sin vínculos entre los usuarios de la entidad de chat y otras actividades para esos usuarios.

La entidad de chat se expone mediante el *PartyChat clase*, lo que permite que el título examinar el estado de la entidad de chat. Por ejemplo, un título puede enumerar los miembros de la entidad de chat con la *PartyChat.GetPartyChatViewAsync método*.


### <a name="implementing-features-similar-to-those-related-to-the-game-party"></a>Implementación de características similares a las relacionadas con la entidad de juegos

En el modo multijugador de 2014, los fabricantes de juegos había servido varios propósitos. Permite un título al:

-   Anunciar la actividad actual y la disponibilidad para las combinaciones de un usuario
-   Enviar invitaciones (invitaciones) a las sesiones
-   Detectar y unirse a sesiones
-   Recibir notificaciones en ciertos cambios en las sesiones registradas en la entidad
-   Usar SmartMatch contactos
-   Mantener un grupo de personas juntos en varias sesiones de juegos

Multijugador 2015 es compatible con todas las características anteriores directamente a través de sesiones MPSD.

## <a name="multiplayer-terminology"></a>Terminología de varios jugadores


| Término                                 | Descripción|
| --- | --- |
| Reproductor de activo                        | Un reproductor que se ha establecido en el estado activo dentro de la sesión. El título establece un reproductor en este estado cuando el Reproductor está participando en un juego. Para obtener más información, consulte [Estados de sesión de usuario](mpsd-session-details.md).                                                                                                                                                                                                                                                                                          |
| Arbiter                              | La consola solo en una sesión de juego que administra el estado de la sesión (MPSD) del directorio de sesión de varios jugadores para un juego, por ejemplo, en el anuncio de la sesión de juego a contactos, buscar más jugadores. El árbitro se establece el título. El árbitro no es siempre el host del juego. Consulte [sesión árbitro](mpsd-session-details.md).                                                                                                                                                                            |
| Juego organizado                        | Un tipo de juego que se crea únicamente a través de un solo Reproductor invitar a otros jugadores se unan a, sin ninguna intervención de contactos.                                                                                                                                                                                                                                                                                                                                                                                                    |
| Entidad de chat                           | Un grupo de personas que están conversando juntos. Podrían exponerse a las personas en la misma actividad o es posible que se ha colaborado en actividades diferentes, como juegos, música o aplicaciones. Consulte **partes compatibles con varios jugadores 2015**.                                                                                                                                                                                                                                                                                 |
| Invitación de juegos                          | Una invitación para participar en una sesión de juego.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Sesión de juego                         | Una sesión en el que los usuarios realmente están reproduciendo juntos. Todos los escenarios multijugador, por ejemplo, contactos o unirse a un área de recepción, en última instancia, terminan en una sesión de juego. La sesión a menudo se anuncia como las actividades actuales de los usuarios para habilitar las combinaciones. También se utiliza para compilar la reciente lista de los jugadores. Consulte [detalles de la sesión MPSD](mpsd-session-details.md).                                                                                                                                                           |
| Host de sesión de juego                    | La consola donde se ejecuta el juego reproducir simulación para títulos basado en una arquitectura de red de punto a punto basada en host. Esta consola suele ser el mismo que el árbitro, pero no tiene que ser el mismo.                                                                                                                                                                                                                                                                                                                            |
| Identificador (o identificador de sesión)           | Una referencia a una sesión MPSD que tiene el estado adicional y el comportamiento asociado con él. Consulte [MPSD controla a las sesiones](multiplayer-session-directory.md).                                                                                                                                                                                                                                                                                                                                                                    |
| Reproductor inactivo                      | Un reproductor que se ha establecido en el estado inactivo dentro de la sesión. El título establece un reproductor en este estado cuando un juego se suspende o no está inactiva, según se define en el título. En algunos casos, MPSD también puede establecer un reproductor como inactiva, pero es principalmente la responsabilidad del título para hacerlo. Para obtener más información, consulte [Estados de sesión de usuario](mpsd-session-details.md).                                                                                                                           |
| Hopper                               | Un Hopper es una colección controlado por la lógica de coincidencia vales. Un título puede tener varios hoppers, pero solo los vales en el mismo hopper pueden coincidir. Por ejemplo, un título puede crear uno hopper qué Reproductor de habilidad es el elemento más importante para la coincidencia. Puede usar otro hopper en el que solo se confrontan reproductores si se ha comprado el mismo contenido descargable. Para obtener más información sobre cómo encajan las hoppers en el flujo de trabajo SmartMatch, consulte [SmartMatch operaciones de tiempo de ejecución](smartmatch-matchmaking.md) |
| Únase a en curso                     | El concepto de unirse a otro reproductor 's juego una vez iniciado el juego. Los jugadores pueden unirse a juego de un amigo a través de la tarjeta jugador de amigo. El título, a continuación, puede mover estos agentes a la sesión de juego en el momento adecuado.                                                                                                                                                                                                                                                                                                      |
| Introducción a la sesión                        | Una sesión de aplicación auxiliar para jugadores invitados que están esperando para unirse a una sesión de. Consulte [detalles de la sesión MPSD](mpsd-session-details.md).                                                                                                                                                                                                                                                                                                                                                                                       |
| Sesión de destino de coincidencia                 | Una sesión de coincidencia se configure durante SmartMatch contactos para representar a la coincidencia. Consulte [SmartMatch contactos](smartmatch-matchmaking.md).                                                                                                                                                                                                                                                                                                                                                                                              |
| Sesión de vale de coincidencia                 | Una coincidencia preliminar establece una sesión durante SmartMatch contactos. Consulte [SmartMatch contactos](smartmatch-matchmaking.md).                                                                                                                                                                                                                                                                                                                                                                                                         |
| Sesión MPSD                         | Un documento protegido que reside en el directorio de sesión de varios jugadores (MPSD) dentro de la nube de Xbox Live. Contiene un grupo de usuarios que podrían estar conectado mientras se ejecuta un título en Xbox One, junto con metadatos sobre los usuarios y su juego. Consulte [detalles de la sesión MPSD](mpsd-session-details.md).                                                                                                                                                                                                                  |
| Directorio de sesión multijugador (MPSD) | El servicio en la nube que el sistema de varios jugadores que se usa para almacenar y recuperar las sesiones. Consulte [(MPSD) del directorio de sesión de varios jugadores](multiplayer-session-directory.md).                                                                                                                                                                                                                                                                                                                                                                |
| Aplicaciones de terceros                            | Un sistema Xbox One ajustar la aplicación que permite a los usuarios ver y administrar sus entidades.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Sesión de servidor                       | Una sesión de creado por el procesamiento de Xbox Live Compute. Consulte [detalles de la sesión MPSD](mpsd-session-details.md).                                                                                                                                                                                                                                                                                                                                                                                                            |
| Pulse hombro                         | Notificación de MPSD para un título que se ha producido un cambio potencialmente interesante en el servicio. La derivación de hombros es un recordatorio rápido, a menudo menos informativos que una notificación normal. Consulte [MPSD Cambiar control de notificación y detección de la desconexión](multiplayer-session-directory.md).                                                                                                                                                                                                                     |
| Asociación de SmartMatch               | Una Xbox Live contactos capacidad disponible para los títulos de Xbox One, implementados por el servicio. Con MPSD y contactos, el título realiza una solicitud que debe coincidir y se notifica posteriormente que se ha encontrado un grupo coincidente. Consulte [SmartMatch contactos](smartmatch-matchmaking.md).                                                                                                                                                                                                                                  |

## <a name="whats-new-in-2015-multiplayer"></a>Novedades de varios jugadores de 2015

| Precaución                                                                                                                                                                                                                                               |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Al usar varios jugadores de 2015, es importante tener en cuenta que las clases relacionadas con la entidad en el modo multijugador 2014 no se recomienda ya. Mezclar funcionalidad multijugador 2015 con las clases relacionadas con la entidad provoca un comportamiento incoherentes y nunca se debe intentar. |


### <a name="new-concepts-in-2015-multiplayer"></a>Nuevos conceptos de varios jugadores de 2015


#### <a name="web-socket-connections-to-mpsd"></a>Conexiones de Socket Web a MPSD

MPSD ahora permite títulos mantener las conexiones de socket web con él. Estas conexiones permiten a los clientes recibir notificaciones cuando cambian las sesiones. Para obtener más información, consulte [desconectar detección y control de notificación de cambio de MPSD](multiplayer-session-directory.md).


#### <a name="mpsd-session-handles"></a>Identificadores de sesión MPSD

2015 participan varios jugadores agrega compatibilidad para los identificadores de sesión MPSD, que son las referencias a las sesiones que pueden incluir datos con tipo. Para obtener más información, consulte [MPSD controla a las sesiones](multiplayer-session-directory.md).


### <a name="summary-of-new-2015-multiplayer-winrt-api-functionality"></a>Resumen de las nuevas funciones de API de WinRT para varios jugadores de 2015

Nueva funcionalidad multijugador de API de WinRT se basa en el XSAPI existente, para facilitar que la transición para títulos ya usa el multijugador 2014 en combinación con la API de fabricantes de juegos.

Agrega varios jugadores 2015 el *MultiplayerActivityDetails clase*, que representa los detalles de la actividad actual de un usuario, por ejemplo, la sesión en el que el usuario puede unir.

Se ha agregado nueva funcionalidad a la *MultiplayerService clase*. Algunos ejemplos son los métodos y propiedades al trabajar con actividades de usuarios y grupos de redes sociales, recuperación de las sesiones que usan varios filtros y controladores, envío de invitaciones de juego y sesión lee y escribe con los manipuladores.

El *MultiplayerSession clase* agrega funcionalidad para trabajar con tipos de cambio de sesión, las suscripciones de notificaciones, comparación de la sesión y establecer una sesión como cerrado.

El *MultiplayerSessionReference clase* es cambiar para heredar **IMultiplayerSessionReference**, para permitir llamadas entre-espacio de nombres. La clase también tiene la ruta de acceso URI nuevos métodos de análisis.

| Nota                                                                                                                                                                                                                                                                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Para admitir los eventos y las suscripciones a notificaciones, 2015 participan varios jugadores agrega funcionalidad a la *Microsoft.Xbox.Services.RealTimeActivity Namespace*. También se incluyen en la nueva funcionalidad es *SystemUI.ShowSendGameInvitesAsync método*, que se usa para mostrar la interfaz de usuario de invitación juegos para varios jugadores de 2015. |

## <a name="differences-between-xbox-360-and-xbox-one-mpsd-session-functions"></a>Diferencias entre la consola Xbox 360 y Xbox MPSD una sesión funciones

| Función | Xbox 360 | Xbox One |
|---|---|---|
| **Obtener información de sesión de juego** | XSessionGetDetails, XSessionSearchByID o título hace seguimiento. | Título solicita información de la sesión de MPSD. |
|**Migrar el host** | Cuando es necesario, título llama XSessionMigrateHost. | Dependiendo de la causa de la migración, título podría ser capaz de asignar un nuevo host para la sesión o puede crear una nueva sesión MPSD. |
| **Varias sesiones de Reproductor** | Difícil de controlar más de una sesión a la vez, por ejemplo, XNetReplaceKey frente a XNetUnregisterKey. | Sesión basada en el servicio realiza la administración de una sesión más limpio y simplifica la administración de varias sesiones. |
| **Signouts y desconecta** | Título que se tiene que administrar se desconecta y cierre de sesión de manera diferente con XCloseHandle o XSessionDelete. | MPSD simplifica signouts y desconectar el control y el tiempo de espera establecido en la configuración de los juegos. |
| **Contactos** | Consultas de contactos basado en cliente | Contactos basado en el servicio que permite a una mejor coincidencia de calidad y más fácil de contactos en segundo plano en el título. |


### <a name="sessions"></a>Sesiones

En la consola Xbox 360, una sesión había representada por una instancia de juego. Los usuarios buscan sesiones en el servicio y notifican las estadísticas al final de una sesión.

En Xbox One, una sesión es más genérica y representa un grupo de los jugadores. Una sesión es necesaria para cualquier conexión de red entre las consolas y contiene información que se debe compartir entre todos los usuarios en la sesión. Algunos ejemplos de esta información incluyen el número de jugadores permitido en la sesión, la dirección de cada consola en la sesión y los datos personalizados del juego segura.


### <a name="xbox-matchmaking"></a>Contactos de Xbox

En la consola Xbox 360, títulos realizan contactos mediante la configuración de un esquema de atributos y un conjunto de consultas para buscar a través de esos atributos. En tiempo de ejecución, el título había seleccionado cualquier host una sesión o busque una.

En Xbox One, contactos está basado en el servidor y, títulos y los reproductores ya no deciden si hospedar o buscar. En su lugar, cada grupo creado previamente de jugadores crea una sesión de "ticket" y envía dicha sesión para el servicio. A continuación, el servicio busca otras sesiones y combina los grupos para formar una nueva sesión de "target". Los clientes se le notificará la coincidencia y realizan la calidad de servicio (QoS) para validar la conectividad con otros miembros de la sesión antes de iniciar el juego.


### <a name="xbox-live-compute-service"></a>Servicio de Xbox Live Compute

El servicio Xbox Live Compute es una nueva oferta en Xbox One. Permite a los desarrolladores aprovechar la potencia de proceso flexible de la nube y permite escenarios multijugador más grandes que eran posibles en una red punto a punto. Para obtener más información sobre el servicio Xbox Live Compute, consulte la documentación de Xbox Live Compute en la documentación XDK.


## <a name="see-also"></a>Consulte también

[Directorio de sesión de varios jugadores (MPSD)](multiplayer-session-directory.md)

[SmartMatch contactos](smartmatch-matchmaking.md)

[Servicio de la actividad en tiempo real (ATR)](../../real-time-activity-service/real-time-activity-service.md)

[Reputación](../../social-platform/people-system/reputation.md)

[Uso de Xbox Live Compute en varios jugadores (requiere acceso de socio administrado)](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/xbox-live-compute/using-xbox-live-compute-in-multiplayer)
