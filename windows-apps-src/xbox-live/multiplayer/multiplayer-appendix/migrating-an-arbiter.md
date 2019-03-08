---
title: Migrar un árbitro
description: Obtenga información sobre cómo migrar a un árbitro en Xbox Live participan varios jugadores de 2015
ms.assetid: 9fb5d2c0-d548-4a22-b64e-6b215f78d22e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, arbiter, 2015 multijugador
ms.localizationpriority: medium
ms.openlocfilehash: 7e250841fb0731a903c0e9c5bdeeecd42f07ac84
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629710"
---
# <a name="migrating-an-arbiter"></a>Migrar un árbitro

En algún momento durante una sesión completa, debe seleccionar un árbitro nuevo mediante la migración de arbiter. Hay dos tipos de migración:

-   **Migración de arbiter estable**
-   **Migración de arbiter de conmutación por error**

El diagrama de flujo siguiente se muestra cómo migrar a un árbitro.

![](../../images/multiplayer/Multiplayer_2015_HostMigration.png)

## <a name="graceful-arbiter-migration"></a>Migración de Arbiter estable

En la migración correcta de arbiter, el árbitro saliente puede ayudar con la tarea de migración y determinar a un árbitro nuevo. Este tipo de migración usa la configuración de un árbitro como se describe en [Cómo: Establecer un árbitro para una sesión MPSD](multiplayer-how-tos.md).


## <a name="failover-arbiter-migration"></a>Migración de Arbiter de conmutación por error

En una migración de arbiter conmutación por error, se pierde la conexión con el árbitro anterior y los pares restantes deben determinar a un árbitro nuevo para la sesión. Migración de arbiter de conmutación por error también establece el host del token del dispositivo y controla los códigos de estado HTTP/412 estable solo como árbitro hace. Sin embargo, existen varios enfoques para seleccionar a un árbitro nueva durante una migración de arbiter de conmutación por error.
## <a name="select-arbiter-using-the-host-candidate-list"></a>Seleccione el árbitro mediante la lista de candidatos de Host

Puede configurar MPSD para proporcionar una lista de candidatos de host ordenada en función de métricas de calidad de servicio que se evalúan durante ciertas operaciones de contactos. El cliente puede utilizar esta lista para determinar a un árbitro nuevo. Para aprovechar las ventajas de esta lista durante la migración de arbiter, cada elemento del mismo nivel puede:

1.  Identifica la posición de la lista del árbitro anterior.
2.  Evaluar la consola en la lista siguiente.
3.  Si la consola es la consola local, puede usar como nuevo el árbitro.
4.  Si la consola ya no está presente en la sesión de varios jugadores o se ha desconectado de sus pares, pasar a la siguiente candidata en la lista y evalúelo como en los pasos anteriores.
5.  Si alcanza el final de la lista con ningún nuevo árbitro seleccionado, utilice un enfoque expansivo para la selección de arbiter, que puede interrumpir la conectividad. Vea "Usar selección de Arbiter expansivo".

| Nota                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| No se recomienda para crear una lista de candidatos de host en el juego después de contactos a través de sondeos de QoS en título explícitos. Si este mecanismo es absolutamente necesario, tener el cliente usa el token del dispositivo host en lugar de la información de usuario, por ejemplo, Xbox Id. de usuario, para determinar a los candidatos de arbiter. |


### <a name="select-arbiter-using-peer-voting"></a>Seleccione el árbitro con mismo nivel de votación

Si existe conectividad completa entre todos los elementos del mismo nivel, pueden usar los mensajes del mismo nivel votar y seleccione a un árbitro nuevo. El árbitro nuevo, a continuación, actualiza el token del dispositivo host para la sesión mediante una actualización sincronizada. Vea [Cómo: Actualizar una sesión de varios jugadores](multiplayer-how-tos.md).


### <a name="use-greedy-arbiter-selection"></a>Usar selección de Arbiter expansiva

A veces no está disponible ninguna lista de candidatos de host o, por ejemplo, conectividad de calidad de servicio no es necesario para las responsabilidades de arbiter puro. En estos casos, el cliente puede usar un enfoque de selección de arbiter expansiva. En este caso, debe establecer un punto el árbitro nueva tan pronto como detecta que el árbitro original ha abandonado la sesión de juego, devuelto por la **MultiplayerSessionChanged** eventos. Todos los demás pares ven un código de estado HTTP/412 al intentar establecer el host de token del dispositivo, suponiendo que ningún otro cambio en la sesión se realiza en este momento. Solo un elemento del mismo nivel se realiza correctamente al seleccionar el árbitro nuevo.


## <a name="see-also"></a>Consulte también

[Detalles de la sesión MPSD](mpsd-session-details.md)
