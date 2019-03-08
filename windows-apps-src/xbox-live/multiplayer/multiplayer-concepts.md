---
title: Conceptos de varios jugadores de Xbox Live
description: Obtenga información sobre los conceptos comunes para varios jugadores utilizados por los sistemas para varios jugadores de Xbox Live.
ms.assetid: 1e765f19-1530-4464-b5cf-b00259807fd3
ms.date: 08/25/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores
ms.localizationpriority: medium
ms.openlocfilehash: 0e2ba32b2eaff757cfa6401952567f6b0163f632
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623800"
---
# <a name="xbox-live-multiplayer-concepts"></a>Conceptos de varios jugadores de Xbox Live

En este tema trata sobre una serie de varios jugadores términos y conceptos importantes que se usan con frecuencia en la documentación de Xbox Live. Tener una buena explicación de estos conceptos le ayudará a comprender el funcionamiento de Xbox Live para varios jugadores.

## <a name="multiplayer-session"></a>Sesión de varios jugadores

Una sesión de varios jugadores representa un grupo de usuarios de Xbox Live y propiedades asociadas a ellos. La sesión se crea y mantiene los títulos y se representa como un documento JSON seguro que se encuentra en la nube de Xbox Live. El propio documento sesión contiene información acerca de los usuarios de Xbox Live que están conectados a la sesión, cuántos puntos son metadatos disponibles, personalizado (para la sesión, así como para cada miembro de la sesión), y otra información relacionada con la sesión de juego.

Cada sesión se basa en una plantilla de sesión, que se definen por el desarrollador de juegos y se configuran en la configuración del servicio Xbox Live para una instancia de título.

Mientras un título puede crear y actualizar una sesión, no puede eliminar directamente una sesión.  En su lugar, una vez que se quitan todos los reproductores de una sesión, el servicio de varios jugadores de Xbox Live eliminará automáticamente la sesión después de un tiempo de espera especificado. Para obtener información detallada acerca de las sesiones, vea [detalles de la sesión MPSD](multiplayer-appendix/mpsd-session-details.md).

Los títulos pueden optar por usar varias sesiones, pero una implementación típica de varios jugadores usará dos sesiones:

* Introductoria sesión: se trata de una sesión que representa un grupo de amigos que quiere que permanezcan juntos en varias rondas, niveles, mapas, etc., del juego.
* Sesión de juegos: se trata de una sesión que representa las personas que se están reproduciendo en una instancia de sesión específico de un juego, como round, coincidencia, nivel, etcetera. Esta sesión puede incluir a miembros de varias sesiones introductoria que se han unido la instancia de la sesión, normalmente a través de un servicio de emparejamiento.

Este es un escenario de ejemplo: Sally quiere jugar con algunos participan varios jugadores con sus amigos, John y Lisa. Sally se inicia un juego y se invita a John y Lisa en su juego. Después de que se unen a, Sally, John y Lisa todos se encuentran en una sesión introductoria. En esta sesión, deciden reproducir en una coincidencia en línea con otras personas. El juego crea una sesión de juego y utiliza el servicio Xbox Live para rellenar las ranuras restantes del Reproductor con otros jugadores de Xbox Live.

Digamos que Bob y Joe son concordaron con ellos, y las cinco de ellas jueguen juntos el redondeo. Una vez finalizada la Ronda, Sally, John y Lisa deje la sesión de juego, pero siguen estando juntos en la sesión introductoria (sin Bob y Joe) y pueden optar por reproducir otra ronda, o cambiar a modo de juego diferente.

### <a name="session-member"></a>Miembro de la sesión

Un miembro de la sesión es un usuario de Xbox Live que forma parte de una sesión.

### <a name="arbiter"></a>Arbiter

Un árbitro es una consola o dispositivos que administra el estado de la sesión de un juego. Por ejemplo, el árbitro sería responsable de anunciar una sesión de juego en contactos con el fin de buscar varios jugadores.

El árbitro se establece el título. Puede ser el mismo que el host del juego, pero no tiene que ser el mismo.

### <a name="session-host"></a>Host de sesión

El host de sesión es la consola o dispositivo que ejecuta el juego reproducir simulación para títulos basado en una arquitectura de red de punto a punto basada en host. Esta consola o el dispositivo suele ser el mismo que el árbitro, pero no tiene que ser el mismo.

## <a name="multiplayer-service-session-directory"></a>Directorio de sesión de servicio para varios jugadores

El servicio de Xbox Live multijugador opera en la nube de Xbox Live y centraliza los metadatos de varios jugadores del sistema de un juego en varios clientes. El sistema que realiza el seguimiento de estos metadatos se conoce como el directorio de sesión de varios jugadores o MPSD para abreviar. MPSD se puede considerar como una biblioteca de sesiones activas de juego. Puede agregar su juego, buscar, modificar o quitar sesiones activas de su título. El MPSD también administra el estado de sesión y actualiza las sesiones cuando sea necesario.

MPSD permite títulos compartir la información básica necesaria para conectarse a un grupo de usuarios. Garantiza que la funcionalidad de la sesión sea coherente y sincronizados. Coordina con el shell y el sistema operativo de Xbox One consola envío/aceptar invitaciones y en la que se combina a través de la tarjeta de jugador.

### <a name="session-handles"></a>Identificadores de sesión

Una sesión se identifica en MPSD mediante una combinación de datos:

* La configuración del servicio del título del Id. (¿SCID)
* El nombre de la plantilla de sesión que se usó para crear la sesión
* El nombre de la sesión

Un identificador de sesión es un objeto JSON que contiene una referencia a una sesión específica que existe en MPSD. Identificadores de sesión permiten a los miembros Xbox Live unirse a sesiones existentes.

Cada identificador de sesión incluye un guid que identifica el identificador, lo que permite a los títulos de hacer referencia a la sesión mediante un guid único.

Hay varios tipos de identificadores de sesión:

* identificador de invitación
* identificador de búsqueda
* identificador de actividad
* identificador de correlación
* identificador de transferencia

#### <a name="invite-handle"></a>Identificador de invitación

Se pasa un identificador de invitación a un miembro al que están invitados a unirse a un juego. El identificador de invitación contiene información que permite la combinación de juegos del miembro invitado la sesión correcta.

#### <a name="search-handle"></a>Identificador de búsqueda

Un identificador de búsqueda incluye metadatos adicionales acerca de la sesión y permite que los títulos buscar las sesiones que cumplen los criterios seleccionados.

#### <a name="activity-handle"></a>Identificador de actividad

Un identificador de actividad permite a los miembros ver los otros miembros de su red social reproducción en curso y se pueden usar unirse a un juego de un amigo.

#### <a name="correlation-handle"></a>Identificador de correlación

Un identificador de correlación eficazmente funciona como un alias para una sesión, lo que permite un juego hacer referencia a una sesión solo con el Id. del identificador de correlación.

### <a name="transfer-handle"></a>Identificador de transferencia

Un identificador de transferencia se usa para mover los reproductores de una sesión a otra sesión.

### <a name="invites"></a>Invitaciones

Xbox Live ofrece un sistema de invitación que sea compatible con el servicio de varios jugadores. Permite que los reproductores invitar a otros jugadores a sus sesiones de juegos. Los jugadores invitados reciben una invitación para jugar y un título usa esta información para unirse a una sesión existente y la experiencia de varios jugadores. Los títulos de controlan el flujo de invitación y cuándo se pueden enviar invitaciones.

Invitaciones se pueden enviar a través del shell por el usuario o directamente desde el título. El texto de notificación para una invitación puede establecerse dinámicamente por un título para proporcionar más información al encargado de invitado. Invitaciones también pueden incluir datos adicionales para el título que no es visible para el Reproductor y pueden utilizarse para transportar la información adicional.

### <a name="join-in-progress"></a>Combinación en curso

Además de invitaciones, Xbox Live también proporciona una opción de shell para reproductores para unirse a una sesión activa del juego de amigos u otros reproductores conocidos. Esto permite a otra ruta de acceso en una sesión activa de juegos y también está controlada por el MPSD. Los títulos de controlan qué sesión para exponer de combinación en curso y cuando las sesiones son joinable.

### <a name="protocol-activation"></a>Activación de protocolos

Si Sally envía una invitación a Lisa para unir su juego, Lisa recibe una notificación en su dispositivo que puede elegir Aceptar o rechazar.

Si acepta la invitación, el sistema operativo intenta iniciar el juego, si el juego no está ya se está ejecutando y desencadena un evento de activación que contiene información sobre por qué se ha activado el juego y detalles adicionales (en el caso de una invitación, por ejemplo, el los detalles incluyen el Id. del Reproductor que invita, así como la sesión que el miembro ha sido invitado a).

El proceso de controlar este evento se conoce como activación de protocolos e indica que el juego debe avanzar de forma automática en un estado específico, que se detalla en los argumentos de evento de activación. Si el miembro se une a un juego multijugador, el Id. de identificador de sesión se especifica como uno de los argumentos.

En caso de Laura, acepte la invitación debe automáticamente, inicie el juego (si es necesario) y unirse a ella a la misma sesión de juego como Sally, sin necesidad de realizar ninguna acción adicional de Lisa.

Puede de activación de protocolo se desencadena al aceptar una invitación, unirse a otro miembro del jugador a través de su tarjeta de perfil o al hacer clic en un profundo vinculado logro.

## <a name="smartmatch-matchmaking"></a>SmartMatch contactos

SmartMatch es el nombre del servicio Xbox Live para contactos anónimo. Este servicio hace corresponder los jugadores del juego mismo configurable según un conjunto de reglas de coincidencia.

El servicio trabaja en estrecha colaboración con el MPSD y utiliza las sesiones de contactos de entrada y salida. Contactos se realizan en el servicio, que permite que los títulos de proporcionar fácilmente otras experiencias durante el flujo de contactos, por ejemplo solo jugador dentro del título.

Individuos o grupos que desean escribir contactos creación una sesión de vale de coincidencia, a continuación, solicitan el servicio para encontrar otros jugadores con la que se va a configurar una coincidencia. Esto da como resultado la creación de un archivo temporal "vale de coincidencia" que residen en el servicio (en un hopper coincidencia) durante un período de tiempo.

El servicio elige sesiones para reproducir en función de la configuración de la regla, las estadísticas almacenadas para cada jugador y cualquier información adicional proporcionado en el momento de la solicitud de coincidencia. El servicio, a continuación, crea una sesión de destino de coincidencia que contiene todos los jugadores que han coincidido y notifica a los títulos de los usuarios de la coincidencia.

Cuando la sesión de destino esté lista, títulos pueden realizar la calidad de servicio (QoS) comprobaciones para confirmar que el grupo puede jueguen juntos o únase a los jugadores a la sesión para iniciar el juego. Durante la calidad de servicio en el proceso y matchmade juego, títulos de mantener el estado de sesión al día en MPSD y recibir notificaciones de MPSD acerca de los cambios a la sesión. Estos cambios incluyen los usuarios o la baja y cambia al árbitro de sesión.

### <a name="match-ticket-session"></a>Coincide con la sesión de vale

Una sesión de vale coincidencia representa a los clientes de los jugadores que desean realizar una coincidencia. Normalmente, se crea basándose en un grupo de los jugadores que están en una sala de juntas, o en otras agrupaciones específicas de título de los jugadores. En algunos casos, la sesión de vale podría ser una sesión ya está en curso que está buscando más jugadores.

### <a name="match-ticket"></a>Vale de coincidencia

Envío de una sesión de vale para contactos da como resultado la creación de un vale de coincidencia que realiza un seguimiento el intento de contactos. Los atributos pueden agregarse para el vale, por ejemplo, mapa de juego o nivel del Reproductor. Estos, junto con los atributos de los jugadores en la sesión de vale, se usan para determinar a la coincidencia.

### <a name="hoppers"></a>Hoppers

Hoppers son lugares lógicos donde se recopilan y se especificó al principio de contactos vales de coincidencia. Solo los vales en el mismo hopper pueden coincidir. Un título puede tener varios hoppers pero solo puede iniciar los contactos en uno cada vez. Por ejemplo, un título puede crear uno hopper qué Reproductor de habilidad es el elemento más importante para la coincidencia. Puede usar otro hopper en el que solo se confrontan reproductores si se ha comprado el mismo contenido descargable.

Configurar hoppers para contactos en la configuración del servicio. TBD.

## <a name="quality-of-service-qos"></a>Calidad de servicio (QoS)

Cuando los jugadores jugar a un juego multijugador en línea, la calidad del juego se ve afectada por la calidad de la comunicación de red entre los dispositivos que hospedan los juegos. Puede dar lugar a una red baja en las experiencias de juego indeseables como lag o conexión quita debido al ancho de banda suficiente o la latencia.

QoS hace referencia a medir la fortaleza de una conexión en línea (ancho de banda y latencia) entre los reproductores para asegurarse de que todos los jugadores tienen una calidad suficiente de la conexión de red. Esto es especialmente importante para reproductores que se hacen coincidir durante contactos para garantizar una buena experiencia debido a la red. Resulta menos aplicable para invitaciones donde amigos jueguen juntos y están normalmente dispuestos a aceptar las consecuencias de una mala conexión.

Puede configurar la sesión para controlar la calidad de servicio automáticamente basándose en criterios específicos, o puede controlar su juego medir la calidad de servicio cada vez que alguien une a la sesión.
