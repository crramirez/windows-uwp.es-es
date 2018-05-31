---
title: Introducción a Xbox Integrated Multiplayer
author: KevinAsgari
description: Obtén información sobre Xbox Integrated Multiplayer (XIM), una solución integral de multijugador/redes/chat para juegos de Xbox Live.
ms.assetid: edbb28e6-1b6c-4f12-a9c6-fa8961de99a8
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5af9a5c386dc620c19183747750081ea1bd5a66d
ms.sourcegitcommit: db09dcb08da5995c46c2729896e56be3774ee5ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2018
ms.locfileid: "1563404"
---
# <a name="xbox-integrated-multiplayer-overview"></a>Introducción a multijugador integrado de Xbox

 Xbox Integrated Multiplayer (XIM) es una interfaz independiente para agregar fácilmente comunicación de red y chat en tiempo real multijugador a tu juego a través de la potencia de los servicios de Xbox Live. Está orientado en torno a algunos conceptos clave:

 - **Red XIM** - Una representación lógica de un conjunto de usuarios interconectados que participan en una experiencia multijugador concreta, así como un estado básico que describe dicha colección. Los participantes solo pueden estar solo en una sola red XIM, pero pueden moverse sin problemas de una red XIM conceptual a otra.
 - `xim_player` - Un objeto que representa un usuario humano individual que ha iniciado sesión en un dispositivo local o remoto y que participa en una red XIM. Un usuario físico único que se une, sale y vuelve a unirse a la misma red XIM se considera que está en dos instancias de jugador independientes.
 - `xim_authority` - Un objeto que representa una entidad única con el permiso y la responsabilidad para administrar una vista coherente del estado específico del juego en una red XIM para todos los participantes. El uso de este objeto es opcional. Su funcionalidad tampoco está disponible en esta versión de software.
 - `xim_state_change` - Una estructura que representa una notificación para el dispositivo local en relación con un cambio asincrónico en algún aspecto de la red XIM.
 - `xim::start/finish_processing_state_changes` - El par de métodos llamados por la aplicación cada trama de la interfaz de usuario para realizar operaciones asíncronas, para recuperar resultados para ser gestionados en forma de estructuras `xim_state_change` y para liberar los recursos asociados al terminar.
 - **Asociación** - El proceso opcional de detectar jugadores remotos adicionales con intereses o niveles de habilidad similares para participar en una red XIM sin necesidad de una relación social existente.

En un nivel muy alto, la aplicación usa una biblioteca para configurar un conjunto de usuarios en el dispositivo local que se va a trasladar a una red XIM como nuevos jugadores. Cuando las instancias de la aplicación en dispositivos remotos hacen algo con sus propios usuarios y cada estado de procesamiento continuo de inicio + finalización cambia en cada trama de interfaz de usuario, cada instancia que participa proporciona actualizaciones de `xim_state_change` que describen el `xim_player` local y remoto que se une a dicha red XIM.

Dentro de la red XIM, la aplicación puede enviar sus propios mensajes de datos específicos del juego, como las actualizaciones de movimiento del avatar. Estos pueden destinarse a otros jugadores, o un `xim_authority` automáticamente seleccionado para que resida en uno de los dispositivos participantes (basándose en la mejor calidad de la red, estabilidad, reputación de jugador y otros factores) de forma que la aplicación en dicho dispositivo de `xim_authority` pueda devolver mensajes a jugadores, combinándolos normalmente para eficacia de la red y para arbitrar conflictos. Todos los mensajes recibidos se entregan a las aplicaciones como un `xim_state_change` que indica el origen previsto y el destino o destinos locales.

La comunicación de voz y de chat con texto también se proporcionan automáticamente entre todos los jugadores donde la configuración de comunicación y la configuración de la aplicación lo permita. Para jugadores que hayan habilitado el texto a voz o conversión texto a voz, XIM realizará de forma transparente esta traducción para proporcionar mensajes de texto para chart que representen el audio de voz entrante y reproduzca el audio de voz sintetizada para mensajes de texto de chat, respectivamente.

Cuando un jugador deja de participar en la red XIM (por iniciativa propia o por problemas de conectividad de red), se proporcionan actualizaciones de `xim_state_change` a todas las instancias de la aplicación que indican que `xim_player` ha salido. Si el dispositivo seleccionado para ser la `xim_authority` sale, se informa a las instancias de la aplicación de que la autoridad ha comenzado a moverse y empezado a "volver a conciliar" cualquier estado específico del juego proporcionando opcionalmente un búfer de datos a la `xim_authority` de sustitución para fines de resincronización. La nueva autoridad puede interpretar los datos de reconciliación de todos los jugadores que quieras y a cambio proporcionar un búfer de datos "reconciliado" a todos los jugadores para completar el traslado de autoridad y el proceso de resincronización. El mismo procedimiento de reconciliación básico también se ofrece a dispositivos participantes nuevos de forma que la autoridad pueda proporcionarles convenientemente el estado actual del juego autorizado actual cuando sus jugadores se unen a la red XIM, sin código adicional. Ten en cuenta que los cambios de estado de la autoridad no se proporcionan actualmente en esta versión de software.

Una aplicación puede determinar la red XIM en la que participar a través de varias formas. A menudo la aplicación se inicia moviendo automáticamente los usuarios locales a una nueva red disponible para los amigos del usuario, donde los usuarios locales pueden enviar invitaciones o tienen su red XIM detectada como una actividad a la que se pueden unir (a través de tarjetas de jugador, por ejemplo). Una vez que estos usuarios socialmente detectados están listos, la aplicación puede iniciar el proceso de "asociación" de Xbox Live y trasladar todos los jugadores a una nueva red XIM que también contiene jugadores remotos "coincidentes" para rellenar las listas del equipo u oponentes según se desee. A continuación, cuando se completa esa experiencia multijugador, instancias de la aplicación pueden devolver sus jugadores locales, y, opcionalmente, los jugadores de asociación preliminar remotos originales también, a una nueva red XIM privada o en otra red XIM aleatoria que detectada a través de la asociación. El chat de texto y voz siguen estando disponibles en todo. Esta facilidad de trasladar jugadores de una red XIM a una red XIM es fundamental para la API y refleja las expectativas modernas de experiencias de juego buenas altamente sociales.

Para empezar, consulta [Utilización de XIM](xbox-integrated-multiplayer/using-xim.md).

## <a name="xims-relationship-to-other-modules"></a>Relación de XIM con otros módulos

XIM se ha diseñado para proporcionar una interfaz práctica integral para juegos con necesidades de multijugador básicas. Encapsula la funcionalidad de varios módulos, especialmente el módulo `multiplayer_manager` de la API de servicios de Xbox (XSAPI), la biblioteca `Microsoft::Xbox::GameChat` y la red multijugador segura `Windows::Networking::XboxLive`, como una API única simplificada. Esto reduce la cantidad típica normal de código, tareas y conceptos relacionados al crear juegos multijugador que no requieren el control o la flexibilidad máxima absoluta. No obstante, aplicaciones cuyos requisitos no se alinean con suposiciones de simplificación de XIM quizás quieran, en su lugar, usar estos componentes directamente.

Mientras XIM está pensado para eliminar la necesidad de administrar cosas como documentos de sesión de directorio de sesión multijugador (MPSD) o un transporte de red a través de componentes subyacentes, no impedirá su uso simultáneo o al lado de como parte de una lista de jugadores o una malla de comunicación separada. En este caso es responsabilidad de la aplicación garantizar el uso de los recursos de red cooperativa entre XIM y sus propios mecanismos. XIM actualmente se admite "fuera de las reservas de banda" para facilitar su uso como una solución de chat dedicada cuya lista de usuarios está controlada exclusivamente mediante entrada externa.

Xbox Live proporciona muchas otras funcionalidades valiosas para juegos multijugador pero menos involucradas directamente en la configuración de chat multijugador y comunicaciones de red y, por tanto, no encapsuladas por este módulo. Se aconseja que las aplicaciones busquen la API de servicios de Xbox (XSAPI) para reportes de jugador, logros, marcadores, almacenamiento y mucho más.


## <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>Usar XIM como una solución de chat dedicada a través de las reservas fuera de banda

La mayoría de las aplicaciones usan XIM para controlar todos los aspectos de unión de jugadores. Después de todo, el enfoque de ensamblar todas las funcionalidades necesarias para admitir escenarios multijugador comunes de forma integral es el motivo por el que se denomina "Xbox Integrated Multiplayer". Pero algunas aplicaciones que implementan sus propias soluciones de redes con los servidores de Internet también desearían las ventajas de la comunicación de chat de punto a punto de bajo costo, de establecimiento fiable y baja latencia. XIM reconoce esta necesidad y actualmente admite un modo de extensión para sacar provecho de su comunicación simplificada del mismo nivel para aumentar la administración de jugadores externos que sucede fuera de la API de XIM.

> Para obtener documentación detallada sobre el uso de XIM a través de reservas de fuera de banda, consulta [Reservas de XIM](xbox-integrated-multiplayer/xim-reservations.md).
