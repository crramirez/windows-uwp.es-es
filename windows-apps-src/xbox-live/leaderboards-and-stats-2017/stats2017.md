---
title: Estadísticas de los jugadores
description: Introducción a las estadísticas de 2017
ms.date: 07/02/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, estadísticas del Reproductor, marcadores, estadísticas de 2017
ms.localizationpriority: medium
ms.openlocfilehash: eabcc655afa5a9d58ca8ef45569d099e503ebc32
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650870"
---
# <a name="stats-2017"></a>Estadísticas de 2017

Estadísticas 2017 permite a los desarrolladores configurar estadísticas para jugadores individuales, lo que significa progreso y habilidad en juego. Estas estadísticas son una herramienta de redes sociales que permitirá a los jugadores sea más competitivo con sus amigos y el título de la mayor comunidad, así como dar a conocer algunas de las capacidades y los desafíos que tiene el título para ofrecer. Si tiene un juego de carreras con estadísticas destacadas, como desfase más largo y mejor tiempo de bloqueo, se puede comunicar el tipo de juego de carreras pueden esperar que los jugadores. Ver cómo pila jugadores con sus amigos y la Comunidad en general les proporcionará más incentivo para comprar y reproducir el título. Los jugadores verá estadísticas destacadas en GameHub del título. Estadísticas destacadas periódicamente también pueden aparecer en anclado bloques de contenido que los usuarios pueden agregar a su vista principal.

Estadísticas 2017 funciona aceptando valores stat como pares clave / valor en el título de un jugador y almacenar ese valor stat para que se puede mostrar en un momento posterior. Estadísticas 2017 está pensado para admitir escenarios de marcador de Xbox Live guardando estadísticas acerca de los jugadores individuales para que se puedan comparar y clasificados en una tabla de clasificación más adelante. Estadísticas 2017 acepta valores le enviados con poca o ninguna validación para que sea el título para controlar toda la lógica que determina el valor correcto para una estadística.

## <a name="configured-stats-and-featured-leaderboards"></a>Estadísticas configuradas y marcadores destacadas

Las estadísticas se configuran en el [panel del centro de desarrollo de Windows](https://developer.microsoft.com/en-us/dashboard/windows/overview). Para configurar estadísticas que ya debe tener un título configurado. Si no tiene un título configurado puede aprender a hacerlo leyendo [crear y probar el título del nuevo creador](../get-started-with-creators/create-and-test-a-new-creators-title.md).  Las estadísticas que configure en el centro de partners aparecerá en GameHub del título como se muestra en la imagen:

El *característica estadísticas* se resaltan en amarillo en la imagen siguiente.
![Marcador de página Social Club oficial](../images/omega/gamehub_featuredstats.png)


En Xbox One, los usuarios pueden anclar juegos directamente a su pantalla principal para encontrar rápidamente información relevante, contenido controlado por la Comunidad y publicaciones de desarrollador. Las estadísticas que configure también pueden aparecer como parte del contenido de principal anclado. Las estadísticas mostrarán el Reproductor junto con un amigo una clasificación ligeramente superior, animándoles a reproducir su recorrido hacia la tabla de líderes. Tablas de clasificación de contenido anclado aparecería como se resalta en la siguiente imagen.

![Halo 5 anclado marcador](../images/stats/Halo_5_Pinned_Leaderboard.png)

Comparan estadísticas destacadas en curso juego basado en estadísticas configuradas con otros amigos que también desempeñan el título. Esto fomenta más tiempo de reproducción del título cuando amigos compitan por las puntuaciones más altas o simplemente se uniría a través de una experiencia compartida. Marcadores Stat destacadas son marcadores mensual que se restablecen el primer día del mes.

Los desarrolladores están limitados a tener no más de 20 estadísticas destacadas de su puesto, pero no hay ningún requisito para que los desarrolladores incluir estadísticas destacados en su título.

## <a name="further-reading"></a>Lecturas adicionales
Aprenda a configurar las estadísticas con [configurar estadísticas destacados y marcadores 2017](../configure-xbl/dev-center/featured-stats-and-leaderboards.md) Obtenga información sobre cómo actualizar los valores de stat con [2017 actualización de estadísticas](player-stats-updating.md)