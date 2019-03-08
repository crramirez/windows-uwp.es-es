---
title: Plantillas de sesión de varios jugadores
description: Obtenga información acerca de las plantillas de sesión de varios jugadores de Xbox Live.
ms.assetid: 178c9863-0fce-4e6a-9147-a928110b53a2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox one, plantilla de sesión para varios jugadores,
ms.localizationpriority: medium
ms.openlocfilehash: 0bbe4f6a3afe2d39fb18b4d4bad13e2aa91d246e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599440"
---
# <a name="multiplayer-session-templates"></a>Plantillas de sesión de varios jugadores

En este tema ofrece una breve descripción de las plantillas de sesión de varios jugadores y proporciona varios ejemplos de plantillas que puede copiar y modificar para las sesiones de varios jugadores.

Una plantilla de sesión de varios jugadores es un plano que se utiliza para crear una sesión de varios jugadores. Todas las sesiones se deben crear en función de una plantilla predefinida. Una plantilla define constantes que serán el mismo para cualquier sesión que se crea a partir de la plantilla. Una vez que un juego, crea una sesión desde una plantilla, el juego se puede agregar y modificar datos adicionales a la sesión, pero no se puede modificar las constantes que se definieron en la plantilla.

 Para obtener más información, consulte [Session Overview](../multiplayer-appendix/mpsd-session-details.md).

La lista de plantillas de sesión que se aplican a un identificador de configuración de servicio (¿SCID), así como el contenido de plantillas de sesión específico, se puede recuperar desde el directorio de sesión participan varios jugadores (MPSD).


## <a name="about-session-templates"></a>Acerca de las plantillas de sesión

Una plantilla de la sesión utiliza el mismo formato como una solicitud HTTP PUT para crear o modificar una sesión. La diferencia es que la plantilla se limita a las constantes (no hay miembros, servidores o propiedades). Las constantes pueden ser una de estas constantes sesión, incluida una sección personalizada y la gama completa de las constantes del sistema.

### <a name="session-template-versions"></a>Versiones de plantillas de sesión

Las plantillas de sesión definidas en este tema se basan en la versión de contrato de plantilla 107. Cuando se utilicen para crear una nueva plantilla, asegúrese de que 107 se introduce como la versión del contrato.

Si usa XSAPI y ver las solicitudes resultantes en el depurador, es posible que tenga en cuenta que las solicitudes de usan la versión de contrato de plantilla 105. MPSD eficazmente "actualiza" estas solicitudes a la versión 107 en tiempo de ejecución.

> **Nota:** Es admisible para usar una versión diferente del contrato en la solicitud del que se usa en la plantilla de sesión.

Si es necesario, puede cambiar una plantilla de sesión de la versión 104/105 a versión 107. Para obtener instrucciones, consulte las instrucciones de migración en [problemas al adaptar su títulos comunes para varios jugadores 2015](../multiplayer-appendix/common-issues-when-adapting-multiplayer.md).


## <a name="session-template-default-values"></a>Valores predeterminados de plantilla de sesión

Cada sesión creado a partir de una plantilla de sesión se inicia como una copia de la plantilla. Al crear la sesión se pueden proporcionar valores que no incluye la plantilla. Cuando no se establece ningún otro valor, se proporcionan los valores predeterminados en algunos casos. Por ejemplo, el conjunto predeterminado de tiempos de espera para la versión del contrato 107 es:

```json
    {
      "constants": {
        "system": {
          "reservedRemovalTimeout": 30000,
          "inactiveRemovalTimeout": 0,
          "readyRemovalTimeout": 180000,
          "sessionEmptyTimeout": 0
        }
      }
    }
```
Puede forzar un valor que debe permanecer sin establecer especificando null. Esto invalida cualquier configuración predeterminada y evita que el valor que se establece al crear la sesión. Por ejemplo, para quitar el vacío tiempo de espera, lo que permite sesiones continuar indefinidamente, incluso sin ningún miembro, agregue lo siguiente a la plantilla de sesión:
```json
    {
      "constants": {
        "system": {
          "sessionEmptyTimeout": null
        }
      }
    }
```
> **Importante:** Las constantes que se establecen a través de una plantilla no se puede cambiar a través de operaciones de escritura a MPSD. Para cambiar los valores, debe crear y enviar una nueva plantilla con los cambios necesarios.


## <a name="example-session-templates"></a>Plantillas de sesión de ejemplo
En esta sección se muestra varios ejemplos de plantillas de sesión para distintos propósitos y topologías de red. Examine cada plantilla antes de elegir uno más adecuado para su cliente. Puede, a continuación, copie y pegue la plantilla en la configuración del servicio, potencialmente después de realizar los cambios necesarios.

### <a name="standard-lobby-session"></a>Sesión introductoria estándar
Puede usar la siguiente plantilla como una plantilla de inicio para crear una sesión introductoria para su juego:

* Cambiar el `maxMembersCount` valor para el número máximo de reproductores que desee admitir en su sesión introductoria.  
* Si el título no admite diferentes plataformas (por ejemplo, una Xbox One y un PC) de los jugadores reproduciendo juntos, puede quitar el `crossPlay` elemento.  
* Puede cambiar los demás valores, pero los valores siguientes son valores correctos para empezar si no está seguro de lo que necesita.


```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            },
        },
        "custom": {}
    }
}
```

### <a name="standard-game-session-without-matchmaking"></a>Sesión de juego estándar sin contactos
Puede usar la siguiente plantilla como una plantilla de inicio para crear una sesión de juego para su juego, si su juego no incluye los contactos anónimo y no requiere a más de 100 miembros.

Tenga en cuenta que los valores solo nuevos especificados de la plantilla de sesión introductoria estándar son los siguientes:
* `constants.system.inviteProtocol : "game"`
* `constants.system.capabilities.gameplay : true`

```json
{
   "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 8,
            "visibility": "open",
            "inviteProtocol": "game",
            "capabilities": {
                "connectivity": true,
                "connectionRequiredForActiveMembers": true,
                "gameplay" : true,
                "crossPlay": true,
                "userAuthorizationStyle": true
            }
        },
        "custom": {}
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-while-letting-the-multiplayer-service-handle-quality-of-service-checks"></a>Agregar contactos a una plantilla de sesión de juego, permitiendo que el servicio de varios jugadores controlar la calidad de servicio comprueba.

Puede agregar lo siguiente `memberInitialization` elemento json a la plantilla de juego con el fin de agregar compatibilidad para los contactos.

Cuando crea su hopper SmartMatch, use esta plantilla como la plantilla de la sesión de destino para su hopper.

```json
{
   "constants": {
        "system": {
            "memberInitialization": {
               "joinTimeout": 20000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        }
    }
}
```

### <a name="add-matchmaking-to-a-game-session-template-where-quality-of-service-checks-are-handled-by-a-title-managed-data-center"></a>Agregar contactos a una plantilla de sesión de juego, donde la calidad de servicio comprueba se controlan mediante un centro de datos administrados de título.



```json
{
   "constants": {
        "system": {
            "peerToHostRequirements": {  
                "latencyMaximum": 250,
                "bandwidthDownMinimum": 256,
                "bandwidthUpMinimum": 256,
                "hostSelectionMetric": "latency"
            },
            "memberInitialization": {
               "joinTimeout": 15000,
               "measurementTimeout": 15000,
               "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}
```

### <a name="basic-session-template-for-client-server-game-session"></a>Plantilla de sesión básico para la sesión de cliente / servidor

Puede usar esta plantilla para un título que no lleva a cabo la comunicación punto a punto y no usa Xbox Live Compute, pero en su lugar, tiene que los clientes conectarse a un servidor hospedado de terceros.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true,
          },
        },
        "custom": {}
      }
    }
```

### <a name="lobbysmartmatch-ticket-session-template-for-peer-based-networking"></a>Plantilla de sesión introductoria/SmartMatch vale para redes basadas en mismo nivel

Use esta plantilla para crear una sesión introductoria o una sesión de vale SmartMatch que solo se usará para enviar un grupo de jugadores en contactos. La plantilla no es que se usará para configurar una sesión de juego. Está pensado para su uso por los clientes mediante una topología de red punto a punto o del mismo nivel para alojar.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 10,
          "visibility": "open",
          "capabilities": {
            "connectionRequiredForActiveMembers": true,
          },
          "memberInitialization": {
            "membersNeededToStart": 1
          },
        },
        "custom": {}
      }
    }
```

### <a name="quality-of-service-qos-templates"></a>Calidad de las plantillas de servicio (QoS)

Si el cliente está usando los contactos y evaluar la calidad de servicio, debe agregar algunas constantes a la plantilla de sesión para informar a MPSD coordinarse con el cliente para administrar los usuarios unirse a la sesión. Esta coordinación valida la calidad del estado de conexión antes de informar a los usuarios que está listo para iniciar el juego. En el caso de los juegos de cliente / servidor, la coordinación valida calidad de la conexión antes de que un grupo de jugadores entre contactos.

#### <a name="peer-to-host-game-session-template-with-qos"></a>Plantilla de sesión de juego al host del mismo nivel con QoS

El ejemplo siguiente presenta una plantilla de la sesión de juego al host del mismo nivel con QoS.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "inviteProtocol": "game",
          "capabilities": {
            "connectivity": true,
            "connectionRequiredForActiveMembers": true,
            "gameplay" : true
          },
          "memberInitialization": {
            "membersNeededToStart": 2
          },
          "peerToHostRequirements": {
            "latencyMaximum": 350,
            "bandwidthDownMinimum": 1000,
            "bandwidthUpMinimum": 1000,
            "hostSelectionMetric": "latency"
          }
        },
        "custom": { }
      }
    }
```

#### <a name="peer-to-peer-game-session-template-with-qos"></a>Plantilla de punto a punto de partida con QoS

El siguiente es un ejemplo de una plantilla de punto a punto de partida con QoS.
```json
    {
    "constants": {
      "system": {
        "version": 1,
        "maxMembersCount": 12,
        "visibility": "open",
        "inviteProtocol": "game",
        "capabilities": {
          "connectivity": true,
          "connectionRequiredForActiveMembers": true,
          "gameplay" : true
        },
        "memberInitialization": {
          "membersNeededToStart": 2
        },
        "peerToPeerRequirements": {
          "latencyMaximum": 250,
          "bandwidthMinimum": 10000
        }
      },
      "custom": { }
     }
    }
```

#### <a name="client-server-xbox-live-compute-lobbymatchmaking-session-template-with-qos"></a>Plantilla de sesión de cliente / servidor (Xbox Live Compute) introductoria/contactos con QoS

Use esta plantilla para crear una sesión introductoria o una sesión de contactos con QoS. Esta plantilla no pretende utilizarse para configurar una sesión de juego.
```json
    {
      "constants": {
        "system": {
          "version": 1,
          "maxMembersCount": 12,
          "visibility": "open",
          "memberInitialization": {
            "membersNeededToStart": 1
          }
        },
        "custom": {}
      }
    }
```

#### <a name="session-template-for-crossplay-between-xbox-one-and-windows-10"></a>Plantilla de sesión para crossplay entre Xbox One y Windows 10

Use esta plantilla para habilitar crossplay multijugador entre Xbox One y Windows 10. La capacidad de userAuthorizationStyle permite el acceso a Windows 10. La funcionalidad opcional crossPlay significa que el título es compatible con las interacciones, como invitaciones y combinación en curso entre plataformas.
```json
    {
      "constants": {
        "system": {
          "capabilities": {
            "crossPlay": true,
            "userAuthorizationStyle": true
          },
        },
        "custom": {}
      }
    }
```
