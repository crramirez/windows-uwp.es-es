---
title: Configuración de estadísticas y tablas de líderes de 2017
description: Obtenga información sobre cómo configurar Xbox Live destacados de estadísticas y marcadores en el centro de partners con 2017 de plataforma de datos.
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ea2baf4bc27e6d1cfd5beb9ef0386acda72a39d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598860"
---
# <a name="configuring-featured-stats-or-leaderboards-in-partner-center-with-data-platform-2017"></a>Configuración de estadísticas destacadas o marcadores en el centro de partners con una plataforma de datos de 2017

Con la plataforma de datos de 2017, solo deberá configurar un stat en dos casos:

* La estadística se usa como base para un tablero de puntuaciones global, o
* El estado es un stat Reproductor destacadas que se mostrará en la página concentrador de juego.

En cualquier caso, debe configurar las estadísticas y marcadores juntos. Cada tabla de clasificación se basa en un estado, y no se puede configurar un estado sin configurar también una tabla de clasificación asociado global, incluso si no va a usar un marcador para un stat destacado del Reproductor.

No es necesario configurar las estadísticas que no son estadísticas Reproductor destacadas y no usa un marcador global.

## <a name="configure-a-global-leaderboard-and-an-associated-player-stat"></a>Configurar un tablero de puntuaciones global y un stat Reproductor asociado

En primer lugar, vaya a la sección de estadísticas del Reproductor para el título como se muestra a continuación.

![](../images/omega/dev_center_player_stats_creators.png)

A continuación, haga clic en el `Create your first featured stat/leaderboard` botón y, a continuación, rellene este cuadro de diálogo y haga clic en Guardar.

![](../images/omega/dev_center_player_stats_creators_leaderboard.png)

El `Display name` campo es lo que verán los usuarios en el GameHub.  Esta cadena se puede localizar en la `Localize strings` sección.  Haga clic en `Show Options` en la `Localize strings` sección para ver detalles sobre cómo localizar estas cadenas.

El `ID` campo es el nombre stat y será cómo hará referencia a su estado al actualizarlo desde el código de título.   Consulte la [actualizar estadísticas](player-stats-updating.md) sección para obtener más detalles.

El `Format` es el formato de datos de la estadística.

El `Display Logic` ofrece la elección entre `Player progression`, `Personal best`, y `Counter`:
- Progresión del Reproductor: Representa un nivel individual del Reproductor o la progresión del juego.  El último valor establecido es lo que verán los usuarios.  Por ejemplo, colocar en ciclo actual.
- Mejor registro personal. Representa el mejor resultado actual que ha publicado un reproductor. El valor máximo o mínimo establecido según el criterio de ordenación es lo que verán los usuarios.  Por ejemplo, vuelta más rápida.
- Contador: Se pueden agregar a otro reproductor para calcular un número acumulativo.  

El `Sort` campo le permite cambiar el criterio de ordenación del marcador.

También puede realizar esta stat un `Featured Stat`, pero al hacer clic en `Feature on players' profiles`.  

## <a name="configure-featured-stats"></a>Configurar estadísticas destacadas

Al definir un stat Reproductor, tiene la opción de comprobación de `Featured Stat`.  Tenga en cuenta los siguientes requisitos

| Tipo de desarrollador | Requisitos |
|----------------|-------------|
| Programa de creadores de Xbox Live | No hay ningún requisito para designar las estadísticas como con todas las características de estadísticas.  Aunque está limitado a un máximo de 10 |
| ID@Xbox y los socios de Microsoft | Debe designar entre 3 y 10 estadísticas destacados |

## <a name="next-steps"></a>Pasos siguientes

A continuación, deberá actualizar la estadística desde el código de título.  Consulte [actualizar estadísticas](player-stats-updating.md) para obtener más detalles.