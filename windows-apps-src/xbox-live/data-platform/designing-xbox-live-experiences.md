---
title: Diseñar experiencias de Xbox Live
description: Obtenga información sobre cómo diseñar experiencias fantásticas para los miembros de Xbox Live planeación de las estadísticas del Reproductor, marcadores y logros por su título.
ms.assetid: d2a85621-7d02-4b11-a7fa-9ebaee0092d5
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, estadísticas, logros, tablas de clasificación, diseño
ms.localizationpriority: medium
ms.openlocfilehash: 0f080593727ec6d7ddd529b2ce976708174250ba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600790"
---
# <a name="designing-xbox-live-experiences"></a>Diseñar experiencias de Xbox Live

Este tema le guiará a través de un proceso de ejemplo de pensar y agregar marcadores, logros y estadísticas de usuario para su juego con las características de plataforma de datos de Xbox Live.

## <a name="example"></a>Ejemplo
Vamos a configurar un juego ficticio que se muestra cómo puede ayudarle a Data Platform iluminada increíbles experiencias de Xbox Live en el panel de Xbox One, la aplicación Xbox en Windows 10 y en su juego. En este escenario, vamos a trabajar con un imaginario racing juego llamado _carreras 2015_.

Para obtener el máximo partido de estas experiencias, seguiremos estos recomienda las fases de planeamiento:
1. Diseñar sus experiencias XBL
2. Diseñar las estadísticas de los escenarios que necesitan
3. Configurar marcadores, logros o estadísticas con las características según sea necesario


## <a name="1-design-your-xbox-live-experiences"></a>1. Diseñar sus experiencias de Xbox Live
Queremos _carrera 2015_ a sacar el máximo partido de Xbox Live, con el fin de mantener a los usuarios comprometidos dentro y fuera del juego. Las primeras preguntas para formular son:

1. ¿Qué queremos experiencia de la ficha de nuestro GameHub logros similares? (Estadísticas destacadas & marcadores sociales)
2. ¿Qué objetivos y las motivaciones queremos dar a los encargados de hacer en el juego? (Logros)
3. Las puntuaciones o estadísticas ¿queremos que los jugadores a usar para clasificar a sí mismos con otros jugadores en juego? (Marcadores)


### <a name="gamehubs---featured-statistics-and-social-leaderboards"></a>GameHubs - estadísticas destacadas y marcadores sociales
GameHubs aterrizaje experiencias donde los usuarios pueden aprender todo acerca de un juego. Como un desarrollador o del publicador, éste es el lugar perfecto para permitirle _showcase_ cómo gran y completa su juego consiste en Xbox a los usuarios no han comprado aún el juego. GameHubs también están diseñados para que se mostrará el progreso y atractivas métricas a jugadores que ya posea el título. En la pestaña logros, los usuarios pueden encontrar juego progreso y logros, estadísticas Hero y marcadores sociales.

Objetivo de esta sección es capturar las métricas más atractivas para el juego y exponerlos para proporcionar una imagen única de la experiencia de los jugadores con el juego y clasificarlos frente a sus amigos en un marcador social. Por ejemplo, la ficha Forza 6 logro podría ser algo parecido a esto:

![Imagen Gamehub](../images/omega/forza_gamehub.png)


Observará algunas de las estadísticas como controlado por millas y primer lugar finaliza tiene oro, plata o bronce decoraciones que ilustran donde se clasifica el Reproductor con sus amigos. Interactuar con cualquiera de estas estadísticas se mostrará un marcador completando similar al siguiente:

![Tabla de clasificación](../images/omega/progress_gamehub_lb.png)

 Para _carrera 2015_, creemos el siguiente es un buen conjunto de estadísticas que representan la experiencia de un jugador con el título como buen métricas de clasificación:
 * Vuelta más rápida
 * Wins de lugar más 1
 * Millas controlada por


### <a name="achievements"></a>Logros
Logros son un mecanismo de todo el sistema para dirigir y gratificante acciones en el juego de los usuarios de forma coherente en todos los juegos. Diseñar esto correctamente ayuda a guiar a los usuarios para experimentar el juego en su pleno y amplía la duración del juego.

Para _carreras 2015_, este es un subconjunto de los logros que queremos configurar:
* Finalizar 1 vuelta en menos de 60 segundos
* Finalizar 10 carreras en lugar de 1
* Completar al menos 1 carrera en cada una de las pistas
* Tienen un 5 automóvil
* Unidad para 10.000 millas
* ...


###  <a name="leaderboards"></a>Marcadores
Marcadores proporcionan una manera de jugadores calificar a sí mismos con otros jugadores acciones específicas que pueden lograr en el juego. Además de los marcadores sociales en la GameHubs, podemos configurar marcadores globales que se usará en el juego. Estos son algunos de los marcadores que queremos todos nuestros usuarios clasificar en:

* Tiempo de vuelta más rápido
 * Metadatos: seguimiento donde ocurrió esto
 * Metadatos: usa de automóvil
 * Metadatos: las condiciones meteorológicas
* Wins de lugar más 1
* La mayoría de los automóviles recopilados

## <a name="next-steps"></a>Pasos siguientes
Ahora que sabe cómo diseñar eficazmente las estadísticas, marcadores y logros, ahora puede comenzar a empezar a implementar estas opciones en el título.  Las secciones siguientes describen el proceso de extremo a otro a partir de la configuración en el centro de partners.

Consulte [estadísticas del Reproductor](../leaderboards-and-stats-2017/player-stats.md)
