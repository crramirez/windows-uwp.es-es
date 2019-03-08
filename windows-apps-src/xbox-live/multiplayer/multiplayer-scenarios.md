---
title: Escenarios multijugador
description: Obtenga información sobre los distintos escenarios multijugador y cómo Xbox Live admite estos escenarios.
ms.assetid: 470914df-cbb5-4580-b33a-edb353873e32
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 338c6e65846ca0ce1307e1e5843f37fe63ecf70a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618300"
---
# <a name="multiplayer-scenarios"></a>Escenarios multijugador
Hay muchos tipos diferentes de escenarios multijugador y elegir el escenario correcto puede aumentar la participación de Reproductor y el Reproductor base para su juego, lo que contribuye al ampliar la duración activa de su juego tanto como sea posible.

Incluso juegos son principalmente un solo jugador experiencias pueden beneficiarse de características competitivas de Xbox Live como marcadores, estadísticas o elementos de redes sociales.

En la lista siguiente describe algunos de los escenarios más comunes para varios jugadores que se pueden implementar con Xbox Live. La lista está ordenada en cuanto a complejidad y la cantidad de trabajo necesario para implementar y probar, y debe considerarse estos factores al decidir en escenarios de tipo que desea que su juego para admitir.

## <a name="comparative-indirect-play"></a>Comparativa Play (indirecto)
En este escenario, los jugadores compiten entre sí indirectamente, sin juega en directo en la misma instancia de un juego. Es ideal para juegos que están orientados a una experiencia de un solo jugador, pero contienen algunos aspectos de competencia que permiten que los participantes comparar cómo lo hicieron a otros jugadores.

La funcionalidad de ejemplo para este escenario incluye:

* Tablas de líderes en el que un jugador intenta lograr la mejor "score" en una categoría en relación con otros jugadores. Esto puede incluir un sistema de marcador social para competir con amigos solo.
* Logros y estadísticas, en la que un jugador desea ser capaz de comparar su progreso/rendimiento en relación con los de sus amigos y posiblemente brag sobre vencer una mención especial de especialmente complicada.
* "Fantasma" o "virtual participan varios jugadores", donde un jugador puede competir con una grabación de otro del jugador (o sus propios) rendimiento anterior, por ejemplo, una vuelta en un juego de carreras.

Este tipo de varios jugadores puede lograrse mediante el uso de los siguientes servicios de Xbox Live:

* Presencia
* Estadísticas
* Administrador de redes sociales
* Logros
* Marcadores
* Almacenamiento conectado

Este tipo de varios jugadores no requiere cualquiera de los servicios específicos de Xbox Live multijugador. Probar este escenario requiere varias Xbox Live cuentas solo.

## <a name="local-play-living-room-play"></a>Local Play (reproducir sala de estar)
Este tipo de varios jugadores se basa en dos o más de los jugadores un juego juntos (frente a entre sí, o forma cooperativa) en un único dispositivo. Un título puede usar una única pantalla para todos los jugadores o una experiencia de pantalla de la división de cada jugador. Como alternativa, en juegos en función de a su vez, puede usar un enfoque de "asiento activa" donde cada jugador toma el control del juego en su turno.

En la consola Xbox One varios jugadores pueden iniciar sesión en una única consola. Cada jugador está asociado a un controlador. Actualmente, los dispositivos Windows 10 solo admiten inicio de sesión de una sola cuenta de Xbox Live, pero Microsoft está investigando este cambio en una futura actualización.

Aunque es posible diseñar un estilo única de reproducción de varios jugadores mediante servicios multicapa de Xbox Live, es posible que convenga tener en cuenta este escenario como un subconjunto de un escenario de varios jugadores más amplio que incorpora las experiencias en línea, como el adicional inversión necesaria es mínima en comparación con la devolución potencial de ampliación del escenario de varios jugadores.

Este tipo de varios jugadores puede usar servicios similares como el escenario anterior:

* Presencia
* Estadísticas
* Administrador de redes sociales
* Logros
* Marcadores
* Almacenamiento conectado

Para probar este escenario, requiere varias cuentas de Xbox Live y varios controladores en un único dispositivo.

##  <a name="online-play-with-friends"></a>Jugar en línea con amigos
Este escenario es la experiencia de varios jugadores en línea más tradicional. En este escenario, un miembro de Xbox Live quiere jugar con amigos solo, pero no quiere jugar con extraños. Amigos o están invitados o unirse a un juego en curso porque está en curso.

Para el control parental un título para varios jugadores más amplio (como se muestra más adelante) debe tener la capacidad para limitar el juego multijugador en línea a amigos sólo y recurrir a este escenario. Controles parentales que limitan las interacciones con desconocidos se aplican también a través del servicio de Xbox Live.

Este tipo de varios jugadores puede lograrse mediante el uso de los siguientes servicios de Xbox Live:

* Administrador de varios jugadores
* Presencia
* Estadísticas
* Administrador de redes sociales

Para probar este escenario, requiere varias cuentas de Xbox Live y varios dispositivos.

## <a name="online-play-through-a-list-of-game-sessions"></a>Jugar en línea a través de una lista de las sesiones de juego
En este escenario, un Reproductor puede examinar una lista de las sesiones de juego joinable en un juego y, a continuación, seleccionar cuál va a unir. El Reproductor también tiene la capacidad de crear instancias de la nueva sesión de juego al alojar un juego localmente. Estas instancias de juegos pueden permitir que las preferencias personalizadas (como el modo de juego, nivel o las reglas del juego). Dependiendo del diseño del título, las sesiones de juego pueden admitir restricciones, como requerir una contraseña para determinados niveles de habilidad/Reproductor o de combinación. Estas instancias de juegos de sesión también pueden ser totalmente pública u oculto, según cómo su juego implementa la sesión de exploración y la unión.

Este escenario permite que los reproductores seleccionar automáticamente una sesión de juego. Esto proporciona control para el Reproductor pero no hay ninguna garantía de que una sesión proporciona una buena experiencia. No se puede rellenar una sesión con el Reproductor correcto establecido en el nivel de aptitud interesantes. Una lista de sesiones es más eficaz cuando juegos tienen una pequeña comunidad multijugador activa.

Este tipo de varios jugadores puede usar servicios similares como el escenario anterior:

* Administrador de varios jugadores
* [Examinar sesiones](session-browse.md)
* Presencia
* Estadísticas
* Administrador de redes sociales

Para probar este escenario, requiere un gran conjunto de cuentas de Xbox Live y dispositivos para probar con precisión. Deben tener en cuenta los desarrolladores que dynamics Reproductor true para las listas de sesión sólo se puede probar con Reproductor grandes bases.

## <a name="online-play-though-looking-for-group"></a>Aunque buscando el grupo de jugar en línea
Este escenario es similar al escenario de la lista de sesiones, sin embargo, difiere de él en los puntos importantes. En lugar de una lista de juegos en el título de la plataforma proporciona funcionalidad para las sesiones de juego de lista fuera del juego. Estos anuncios de búsqueda para el grupo están pensados para proporcionar una experiencia más social e incluir el juego, aptitudes y restricciones de la relación de redes sociales. Esto permite que un juego proporcionar una experiencia mejorada a través de la sesión se enumera y también proporciona al creador de la sesión de control más.

El jugador que ha creado la sesión "atractivo para el grupo" puede aprobar o rechazar solicitudes para unirse a su grupo de otros jugadores. Esto permite que los jugadores a buscar a otros jugadores que comparten sus preferencias playstyle.

El servicio de Xbox Live LFG las novedades de 2016 y API de vista previa se espera que esté disponible para los títulos de mayo.

Este tipo de varios jugadores puede usar servicios similares como el escenario anterior:

* Administrador de varios jugadores
* Busca el grupo de Xbox
* Presencia
* Estadísticas
* Administrador de redes sociales
* Servicio LFG

Para probar este escenario, requiere un gran conjunto de cuentas de Xbox Live y dispositivos para probar.

## <a name="simple-matchmaking"></a>Contactos simple
En este escenario, un reproductor (o grupo de reproductores) busca otros jugadores (que pueden o no pueden conocerse al jugador) para un juego en línea. En lugar de seleccionar los amigos, el juego proporciona un flujo de contactos sencilla que permite a los reproductores para unirse a las sesiones de juego con otros jugadores. El flujo de contactos en este escenario es sencillo: reproductores de buscar y encontrar otros jugadores sin ninguna configuración de contactos importantes. Este escenario funciona mejor con una mayor audiencia en línea.

Para que coincida con los reproductores juntos correctamente, deben intentar los contactos respetar la reputación y bloquear las listas en Xbox Live. El servicio de Xbox Live SmartMatch controla automáticamente estas restricciones. Teniendo en cuenta las restricciones como estas ayuda a garantizar una más segura y mejor experiencia para reproductores.

Una parte importante de contactos son redes comprobaciones de calidad de servicio (QoS). Estas comprobaciones aseguran de que la conectividad de red entre dos jugadores es suficiente para una experiencia de juego buena. Esto es diferente en el jugar en línea con el escenario de amigos, ya que es posible repetir los contactos y buscar otros jugadores en el caso de una conexión de red en mal estado.

Este tipo de varios jugadores puede usar servicios similares como el escenario anterior:

* Administrador de varios jugadores
* Presencia
* Estadísticas
* Administrador de redes sociales
* SmartMatch contactos

Para probar este escenario, requiere un gran conjunto de cuentas de Xbox Live y dispositivos para probar con precisión. Deben tener en cuenta los desarrolladores que dynamics Reproductor true para las listas de sesión sólo se puede probar con Reproductor grandes bases. Existen herramientas simplificar la condición de la red y las pruebas SmartMatch.

## <a name="skill-based-matchmaking"></a>Contactos basado en conocimientos
Contactos basado en conocimientos es una versión más elegante de escenario simple de contactos. En este escenario que incluye el servicio los conjuntos de reglas más avanzadas, como aptitudes, nivel del Reproductor y otras propiedades específicas de los juegos. El servicio usa las reglas que usan estos coinciden con los parámetros para buscar una sesión de más alta calidad para el Reproductor. Según el diseño de juegos parámetros de coincidencia se configuran directamente por el Reproductor o se establecen automáticamente el título.

Xbox Live SmartMatch proporciona un conjunto de reglas que puede usarse para contactos basado en la aptitud. El servicio de SmartMatch usa una característica de Xbox Live llamada TrueSkill para ayudar a evaluar la aptitud relativa de un reproductor.

Este tipo de varios jugadores puede usar servicios similares como el escenario anterior:

* Administrador de varios jugadores
* Presencia
* Estadísticas
* Administrador de redes sociales
* SmartMatch contactos

Para probar este escenario, requiere un gran conjunto de cuentas de Xbox Live y dispositivos. Deben tener en cuenta los desarrolladores que dynamics Reproductor true para las listas de sesión sólo se puede probar con Reproductor grandes bases. Las herramientas están disponibles para simplificar la condición de la red, TrueSkill y SmartMatch las pruebas.

## <a name="tournaments"></a>Torneos
Torneos permiten jugadores cualificados competir entre sí en un formato organizado. Normalmente, torneos se hospedan y organizados por organizaciones de terceros independientes.

Xbox Live se ha agregado una nueva característica en 2016 para permitir que los títulos de usar contactos de Xbox Live junto con los organizadores del torneo de terceros aprobadas. Esta característica permite a los títulos de integrar sin problemas los torneos con la experiencia de shell Xbox, sin necesidad de que los jugadores puedan registrarse en sitios Web externos.

Este tipo de varios jugadores puede lograrse mediante servicios de Xbox Live, como:

* Presencia
* Estadísticas
* Redes sociales
* Reputación
* Varios jugadores
* SmartMatch contactos
* Torneos

Para probar este escenario, requiere un gran conjunto de cuentas de Xbox Live y dispositivos.
