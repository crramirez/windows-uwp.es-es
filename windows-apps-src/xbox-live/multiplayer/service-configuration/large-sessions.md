---
title: Sesiones de gran tamaño
description: Obtenga información sobre el uso de sesiones de gran tamaño con la plataforma de varios jugadores de Xbox Live.
ms.date: 07/11/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, sesión de varios jugadores, grande, reproductores recientes
ms.localizationpriority: medium
ms.openlocfilehash: dcd7b27bc0aea8b8406e7eccb420140cf32c1ed5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618550"
---
# <a name="large-sessions"></a>Sesiones de gran tamaño

Si necesita una sesión de varios jugadores que puede administrar a más de 100 miembros, deberá usar lo que se conoce como una sesión de gran tamaño. Este escenario es más común para los juegos multijugador en línea de (MMO) y difusiones (donde la mayoría de los miembros son espectadores), pero pueden tener aplicaciones a otros estilos de juegos.

En algunas circunstancias, es posible que también desea usar sesiones grandes incluso cuando se trabaja con grupos más pequeños de jugadores. Si desea que varios actores para estar en la misma sesión, pero no necesariamente no relacionados entre sí si no encuentran entre sí en juego, puede usar la propiedad "encuentra" de las sesiones de gran tamaño.

Las sesiones de gran tamaño no son compatibles actualmente con [Xbox integrado participan varios jugadores (XIM)](../xbox-integrated-multiplayer.md) o [Manager participan varios jugadores (MPM)](../multiplayer-manager.md), por lo que debe usar las API de 2015 de varios jugadores con llamadas directas a los varios jugadores Directorio de servicio (MPSD).

Sesiones grandes se tratan de forma ligeramente distinta a las sesiones normales:

* Contiene menos información que las sesiones normales, pero son más eficaces.
* Admite a hasta 10 000 miembros.
* No puede suscribirse a una sesión de gran tamaño.
* No hay ninguna inclusión automática en las listas de jugadores recientes para los miembros de una sesión de gran tamaño.

## <a name="recent-players"></a>Jugadores recientes

Una de las características de Xbox Live es que cuando los jugadores de Xbox Live jugar a juegos multijugador con nuevas personas, después el juego pueden ver estos agentes en su panel de la **jugadores recientes** lista. Si un jugador tenía una gran experiencia con un nuevo reproductor en un juego, es posible que desean jugar con ellos de nuevo, o agregarlos como friend. Si tuvieran una experiencia deficiente con un Reproductor, quizá deseen evitarlos en el futuro, o informe allí el comportamiento erróneo después de que el juego.

Con las sesiones normales, Xbox Live agrega automáticamente los jugadores en la misma sesión a la lista de los jugadores recientes. Si utiliza las sesiones de gran tamaño sin embargo, debe realizar algunos pasos adicionales para asegurarse de que las listas de Reproductor recientes se rellenan correctamente.

## <a name="set-up-a-large-session"></a>Configurar una sesión de gran tamaño

Para configurar las sesiones de mayor tamaño, agregar `“large”: true` a la sección de capacidades en la plantilla de sesión. Que permite establecer el `maxMembersCount` hasta 10 000. Una plantilla de sesión, como el debería funcionar a continuación:

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 2000,
            "visibility": "open",
            "capabilities": {
                "gameplay": true,
                "large": true
            },
            "timeouts": {
                "inactive": 0,
                "ready": 180000,
                "sessionEmpty": 0
            }
        },
        "custom": { }
    }
}
```

## <a name="working-with-large-sessions"></a>Trabajar con sesiones de gran tamaño

Al escribir las sesiones de gran tamaño en MPSD, le recomendamos que para que no supere los 10 escrituras por segundo. Esto suele ser una sesión de Reproductor de 1000 con una escritura cada 2 minutos en promedio por Reproductor (por ejemplo, unión o abandonarlos).

Otras propiedades no deben mantenerse en las sesiones de gran tamaño.

### <a name="associating-players-from-the-same-large-session"></a>Asociación de la misma sesión de gran tamaño de los jugadores

Al recuperar una gran sesión de MPSD, la lista de miembros no viene con la respuesta y, de hecho, no hay ninguna manera de obtener la lista completa. En su lugar, si el llamador está en la sesión, su registro de miembro será el único en la colección "miembros", etiquetadas como "me" (al igual que en la solicitud).

Esto significa que los miembros de los clientes sólo podrá actualizar su propia entrada en la sesión y se basarán en el servidor para proporcionar un identificador común que Xbox Live puede utilizar para asociar los jugadores que reproduce juntos.

Hay dos maneras para indicar que los usuarios en una sesión reproduzcan juntos (para actualizar la reputación y el estado de los jugadores recientes).

#### <a name="1-persistent-groups"></a>1. Grupos persistentes

Si un grupo de personas se halle juntos de forma continuada, posiblemente con personas que entren y salgan de ella, puede asignar al grupo un nombre (por ejemplo, un guid: siguiendo las mismas reglas de nomenclatura que las sesiones normales).  Como cada miembro viene y va desde el grupo, debe agregar o quitar el nombre del grupo a su propia propiedad "groups", que es una matriz de cadenas:

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "groups": [ "boffins-posse" ]
                }
            }
        }
    }
}
```

#### <a name="2-brief-encounters"></a>2. Encuentra breve

Si las dos personas tienen un encuentro con un solo uso breve, el juego en su lugar, puede usar la matriz "encuentra". Conceda a cada encuentren un nombre y, después, el encuentro con ambos (o todas) participantes escribiría el nombre a su propia propiedad "detecta":

```json
{
    "members": {
        "me": {
            "properties": {
                "system": {
                    "encounters": [ "trade.0c7bbbbf-1e49-40a1-a354-0a9a9e23d26a" ]
                }
            }
        }
    }
}
```

Puede usar el mismo nombre de "grupos" y "detecta": por ejemplo, si un jugador "comercia con" un grupo, las personas en el grupo no deben hacer nada (suponiendo que el nombre del grupo que agregan anteriormente a sus grupos de"") y la persona que tenía el encuentro con sería u cargar en su lista de "detecta" el nombre del grupo. Esto hará que el usuario individual ver a todos los miembros del grupo como reproductores recientes y viceversa.

Recuento de encuentros ya fueron un miembro del grupo de 30 segundos. Puesto que los encuentra se considera eventos de uso único, la matriz "encuentra" inmediatamente siempre procesa y, a continuación, se borra de la sesión.  Nunca aparecerán en una respuesta.  (La matriz de "grupos" se queda bloqueado en torno a hasta que se puede modificar o quitar, o el miembro abandona la sesión).
