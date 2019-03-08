---
title: Varios jugadores integrada de Xbox
description: Obtén información sobre Xbox Integrated Multiplayer (XIM), una solución integral de multijugador/redes/chat para juegos de Xbox Live.
ms.assetid: edbb28e6-1b6c-4f12-a9c6-fa8961de99a8
ms.date: 01/25/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores integrada de xbox
localizationpriority: medium
ms.openlocfilehash: aa82b1042f710d81d83b98767802c7538fd53fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641780"
---
# <a name="xbox-integrated-multiplayer-xim"></a>Xbox Integrated Multiplayer (XIM)

- [Información general](#overview)
- [Conceptos](#concepts)
- [Características](#features)
- [Relación con otros módulos.](#relationship-to-other-modules)

## <a name="overview"></a>Introducción

Xbox Integrated Multiplayer (XIM) es una interfaz independiente para agregar fácilmente comunicación de red y chat en tiempo real multijugador a tu juego a través de la potencia de los servicios de Xbox Live. La interfaz XIM no requiere un proyecto para elegir entre compilar con C++ / c++ / CX y C++ tradicionales; se puede usar con cualquiera. La implementación no produce excepciones como medio de error no irrecuperable reporting por lo que puede consumirlos fácilmente desde proyectos sin excepciones si lo prefiere.

Para empezar, consulta [Utilización de XIM](xbox-integrated-multiplayer/using-xim.md). Si usas C#, consulte [utilizando XIM C# ](xbox-integrated-multiplayer/using-xim-cs.md).

## <a name="concepts"></a>Conceptos

XIM se centra en unos cuantos conceptos clave:

- Red XIM: una representación lógica de un conjunto de usuarios interconectados que participan en un determinado experiencia multijugador, así como el estado básico que describe esa colección. Los participantes solo pueden estar solo en una sola red XIM, pero pueden moverse sin problemas de una red XIM conceptual a otra.
- Contactos: el proceso opcional de detectar reproductores remotos adicionales con los mismos intereses o niveles de conocimientos para participar en una red XIM sin necesidad de una relación de redes sociales existente.
- Consulta - el proceso opcional de detectar XIM redes sin necesidad de una relación de redes sociales existente entre los participantes.
- `xim_player` -Un objeto que representa un usuario individual usuario inician sesión en un dispositivo local o remoto y que participan en una red XIM. Un usuario físico único que se une, sale y vuelve a unirse a la misma red XIM se considera que está en dos instancias de jugador independientes.
- `xim_state_change` -Una estructura que representa una notificación en el dispositivo local con respecto a un cambio asincrónico en algún aspecto de la red XIM.
- `xim::start_processing_state_changes` y `xim::finish_processing_state_changes` -el par de métodos de la aplicación llama cada fotograma de la interfaz de usuario para realizar operaciones asincrónicas, para recuperar los resultados que se tratan de forma de `xim_state_change` estructuras y, a continuación para liberar los recursos asociados cuando termine.

En un nivel muy alto, la aplicación de juego utiliza la biblioteca XIM para configurar un conjunto de los usuarios que inicien sesión en el dispositivo local que desea mover a una red XIM como reproductores de nuevo. Las llamadas de aplicación `xim::start_processing_state_changes` y `xim::finish_processing_state_changes` cada fotograma de la interfaz de usuario. Como instancias de la aplicación en dispositivos remotos, agrega a los usuarios en una red XIM, se proporcionan todas las instancias participantes `xim_state_change` actualizaciones que describe el equipo local y remoto `xim_player`s unirse a esa red XIM. Cuando un jugador deja de participar en la red XIM (por iniciativa propia o por problemas de conectividad de red), se proporcionan actualizaciones de `xim_state_change` a todas las instancias de la aplicación que indican que `xim_player` ha salido.

Una aplicación puede determinar la red XIM en la que participar a través de varias formas. A menudo la aplicación se inicia moviendo automáticamente los usuarios locales a una nueva red disponible para los amigos del usuario, donde los usuarios locales pueden enviar invitaciones o tienen su red XIM detectada como una actividad a la que se pueden unir (a través de tarjetas de jugador, por ejemplo). Una vez que estos usuarios socialmente detectados están listos, la aplicación puede iniciar el proceso de "asociación" de Xbox Live y trasladar todos los jugadores a una nueva red XIM que también contiene jugadores remotos "coincidentes" para rellenar las listas del equipo u oponentes según se desee. A continuación, cuando se completa esa experiencia multijugador, instancias de la aplicación pueden devolver sus jugadores locales, y, opcionalmente, los jugadores de asociación preliminar remotos originales también, a una nueva red XIM privada o en otra red XIM aleatoria que detectada a través de la asociación. El chat de texto y voz siguen estando disponibles en todo. Esta facilidad de trasladar jugadores de una red XIM a una red XIM es fundamental para la API y refleja las expectativas modernas de experiencias de juego buenas altamente sociales.

En lugar de un modelo cliente / servidor, una red XIM es lógicamente una malla conectada por completo de dispositivos del mismo nivel. Como se describe en la sección de este documento, cualquier reproductor puede enviar directamente a cualquier otro a través de la API. Todos los métodos que afectan al estado de la red XIM como un todo pueden invocar cualquier dispositivo que participan.

XIM utiliza la resolución de conflictos de last-write-wins simple si la aplicación en caso contrario, no impide que a más de un participante de modificar el estado de la mismo red XIM eficazmente al mismo tiempo. Esto significa que XIM no impone ningún concepto de rol para "host" o "servidor". También XIM no restringe las aplicaciones usen sus propios conceptos, como la compatibilidad con migración de roles definidos por la aplicación a otro participante cuando un jugador sale de una red XIM.

## <a name="features"></a>Características

- Proporciona el juego con la comunicación de chat de voz y texto que se observa y respeta la configuración de privacidad del Reproductor

    La comunicación de voz y de chat con texto también se proporcionan automáticamente entre todos los jugadores donde la configuración de comunicación y la configuración de la aplicación lo permita. Para jugadores que hayan habilitado el texto a voz o conversión texto a voz, XIM realizará de forma transparente esta traducción para proporcionar mensajes de texto para chart que representen el audio de voz entrante y reproduzca el audio de voz sintetizada para mensajes de texto de chat, respectivamente.

- Permite el juego enviar sus propios mensajes de datos específicos del juego

    Dentro de la red XIM, la aplicación puede enviar sus propios mensajes de datos específicos del juego, como las actualizaciones de movimiento del avatar. Todos los mensajes recibidos se entregan a las aplicaciones como un `xim_state_change` que indica el origen previsto y el destino o destinos locales.

- Funciones como una solución de chat dedicado a través de las reservas de direcciones fuera de banda

    Para obtener documentación detallada sobre el uso de XIM a través de reservas de fuera de banda, consulta [Reservas de XIM](xbox-integrated-multiplayer/xim-reservations.md).

- Sin excepciones y puede usarse con cualquier C / c++ / CX o C++ tradicional

## <a name="relationship-to-other-modules"></a>Relación con otros módulos.

XIM se ha diseñado para proporcionar una interfaz práctica integral para juegos con necesidades de multijugador básicas. Encapsula la funcionalidad de varios módulos, especialmente el módulo `multiplayer_manager` de la API de servicios de Xbox (XSAPI), la biblioteca `xbox::services::game_chat_2` y la red multijugador segura `Windows::Networking::XboxLive`, como una API única simplificada. Esto reduce la cantidad típica normal de código, tareas y conceptos relacionados al crear juegos multijugador que no requieren el control o la flexibilidad máxima absoluta. No obstante, aplicaciones cuyos requisitos no se alinean con suposiciones de simplificación de XIM quizás quieran, en su lugar, usar estos componentes directamente.

Mientras XIM está pensado para eliminar la necesidad de administrar cosas como documentos de sesión de directorio de sesión multijugador (MPSD) o un transporte de red a través de componentes subyacentes, no impedirá su uso simultáneo o al lado de como parte de una lista de jugadores o una malla de comunicación separada. En este caso es responsabilidad de la aplicación garantizar el uso de los recursos de red cooperativa entre XIM y sus propios mecanismos. XIM actualmente se admite "fuera de las reservas de banda" para facilitar su uso como una solución de chat dedicada cuya lista de usuarios está controlada exclusivamente mediante entrada externa.

Xbox Live proporciona muchas otras funcionalidades valiosas para juegos multijugador pero menos involucradas directamente en la configuración de chat multijugador y comunicaciones de red y, por tanto, no encapsuladas por este módulo. Las aplicaciones, se recomienda buscar marcadores, logros de Reproductor, almacenamiento y mucho más a la API de servicios de Xbox (XSAPI).
