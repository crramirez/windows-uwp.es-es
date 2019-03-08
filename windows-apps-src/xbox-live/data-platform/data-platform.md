---
title: Plataforma de datos en directo de Xbox
description: Información general de Xbox Live Data Platform, que consta de los servicios para administrar marcadores, logros y estadísticas del Reproductor.
ms.assetid: a8bb7c4f-09fe-4dba-b3ce-1fab60453831
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, estadísticas, logros, tablas de clasificación, plataforma de datos
ms.localizationpriority: medium
ms.openlocfilehash: f9a14c508e265cdfc510896eb621466d03c66776
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634840"
---
# <a name="xbox-live-data-platform---stats-leaderboards-achievements"></a>Xbox Live datos plataforma: estadísticas, marcadores, logros

Escribir datos de juegos en la plataforma de datos de Xbox Live permite que el título para ejecutarse como un servicio. Además, la plataforma de datos de Xbox Live unidades de participación de los usuarios con el título con estadísticas, marcadores y logros y superficies con todas las características de estadísticas en el shell de consola y Xbox App.

Un ejemplo de un estado de un juego de carreras podría ser que las anticipaciones total se ejecutan en modo de arrastre franja, que puede usarse para crear un marcador para comparar la puntuación en la Comunidad y se puede reflejar en la cadena de presencia enriquecida en tiempo real (por ejemplo , "GoTeamEmily se reproduce el modo de arrastre franja: 523 carreras completadas"). Las carreras total en el modo de arrastre franja también se puede usar como criterio para contactos, lo que garantiza los jugadores con una experiencia similar es probable que deben coincidir entre sí.

El título puede mostrar marcadores basados en las estadísticas del Reproductor. Por ejemplo, el marcador puede ser una clasificación global de las carreras completado. Se llama a estos servicios mediante las API de Xbox Live directamente, o contenedores en un motor de juego, como Unity.

Puede designar determinadas estadísticas estadísticas como destacadas. Estadísticas destacadas se muestran en GameHub del título y comparan los jugadores con sus amigos.

Logros no cuentan con estadísticas y su título decide cuándo se desbloquea un logro. Por ejemplo, el título podría tener un logro para completar una carrera en menos de un minuto. El título de la realiza un seguimiento de los parámetros necesarios para desbloquear el logro. En este ejemplo sería hasta el título para medir la carrera de cuánto tiempo tardó y premio, a continuación, la realización. Normalmente, se convertirá en puntuación junto con la finalización de los logros. Depende de usted para decidir la cantidad de puntuación para cada mención especial.

## <a name="features"></a>Características ##
Los temas siguientes proporcionan más información acerca de las características de la plataforma de datos de Xbox Live:

* [Estadísticas del Reproductor](../leaderboards-and-stats-2017/player-stats.md)
* [Logros](../achievements-2017/achievements.md)
* [Marcadores](../leaderboards-and-stats-2017/leaderboards.md)
