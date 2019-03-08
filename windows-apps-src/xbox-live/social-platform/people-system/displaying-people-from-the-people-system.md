---
title: Mostrar personas desde el sistema de personas
description: Obtenga información acerca del flujo de código para mostrar las personas con los sistemas de personas de Xbox Live.
ms.assetid: c97b699f-ebc2-4f65-8043-e99cca8cbe0c
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 512de51f2a0e30a9b41a5e49f3dc3ababe30fc4d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658310"
---
# <a name="display-people-from-the-people-system"></a>Mostrar personas desde el sistema de personas

Servicios a través de Xbox Live devuelvan solamente los datos que pertenecen a ese servicio y devuelven solo XUID referencias a los usuarios; Por ejemplo, el servicio de personas solo posee y devuelve el XUIDs que se encuentran en la lista de personas de un usuario y cierta información básica sobre cada una de esas XUIDs (por ejemplo, el estado preferido). El servicio de presencia posee los datos sobre la información de estado de conexión de XUIDs. El servicio de marcadores posee información de clasificación en listas de XUIDs. Mostrar información de nombre y el nombre de jugador nunca se devuelve desde cualquier servicio que no sean el servicio de perfil y, por lo tanto, es necesario para representar las listas de personas en las experiencias de llamar a varios servicios.

El patrón general que realiza la llamada para el servicio de API realizar un recorrido de redondeo y obtener primero una lista de XUIDs desde el servicio que mejor se puede filtrar u ordenar la lista y, a continuación, asegúrese de que las llamadas de ida y vuelta simultáneas a otros servicios necesarios para obtener los metadatos adicionales necesarias para cada XIUD. En el caso de las imágenes, deban un tercer ida y vuelta de llamadas para obtener imágenes desde las direcciones URL de imagen.

Para reducir el número de ciclos de ida y necesaria para obtener datos acerca de la lista de personas de un usuario, una de las personas *moniker* se introduce a los servicios relevantes. Esta nueva característica permite que los llamadores expresar de forma abstracta para el servicio principal que el servicio debe obtener la lista de usuarios del usuario desde el servicio de las personas y, a continuación, usar ese conjunto de XUIDs para definir el ámbito de la devolución.

Siguientes son algunos escenarios de flujo de llamada de ejemplo que ilustran cómo obtener los títulos de los datos de servicios relacionados con las personas:

-   Lista de usuarios actualmente en el juego
-   Lista de personas del usuario actual que están en línea
-   Tablero de puntuaciones global que contiene usuarios aleatorios
-   Tabla de clasificación de las personas del usuario


## <a name="list-of-users-currently-in-game"></a>Lista de usuarios actualmente en el juego

| Tiene el título  | Objetivo  | Campo para representar  | Flujo de llamadas
|-------------------------------------------------|----------------------------------------------------|--------------------|--------------------------------------|
| Lista de XUIDs aleatorios de otros usuarios en el juego | Para representar la información mínima para cada uno de los otros usuarios | GameDisplayName \[perfil\] | Llame al perfil con la lista de XUIDs. |


## <a name="list-of-the-current-users-people-who-are-online"></a>Lista de personas del usuario actual que están en línea

## <a name="title-has"></a>Tiene el título:
XUID del usuario actual

## <a name="goal"></a>Objetivo
Para representar enriquecido lista de usuarios en línea que se encuentran en la lista de personas del usuario actual

## <a name="field-to-render-owning-service"></a>Campo para representar \[posee el servicio\]
* Indicador favoritos [usuarios]
* Imagen para mostrar [Profile]
* GameDisplayName [Profile]
* Estado de conexión básico (bola verde) [presencia]

## <a name="call-flow"></a>Flujo de llamadas
1. Llame a su presencia, pasando el moniker de personas a obtener el XUIDs y el estado en línea para cada una de las personas del usuario.
1. En paralelo:
 1. Llame al perfil, pasar de la lista completa de XUIDs para obtener el nombre para mostrar y la URL de la imagen para cada uno.
 1. Llamar a las personas, pasa la lista de XUIDs para averiguar si alguno es Favoritos del usuario.
1. Después de llamar al perfil:
 1. Obtener imágenes para cada dirección URL de imagen

## <a name="global-leaderboard-containing-random-users"></a>Tablero de puntuaciones global que contiene usuarios aleatorios

## <a name="title-has"></a>Tiene el título:
El identificador/nombre de la tabla de líderes

## <a name="goal"></a>Objetivo
Para representar la información básica para cada usuario en la tabla de líderes

## <a name="field-to-render-owning-service"></a>Campo para representar [servicio propietario]
* Indicador favoritos [usuarios]
* GameDisplayName [Profile]
* Rango [marcadores]
* Puntuación [marcadores]

## <a name="call-flow"></a>Flujo de llamadas
1. Marcadores de llamada para obtener la XUIDs, rank y las puntuaciones de la tabla de clasificación determinado.
1. En paralelo:
 1. Llame al perfil, pasa la lista de XUIDs para obtener el nombre para mostrar y la URL de la imagen para cada uno.
 1. Llamar a las personas, pasa la lista de XUIDs para averiguar si alguno es Favoritos del usuario.

## <a name="leaderboard-of-users-people"></a>Tabla de clasificación de las personas del usuario

## <a name="title-has"></a>Tiene el título:
* El identificador/nombre de la tabla de líderes
* XUID del usuario actual

## <a name="goal"></a>Objetivo
Para representar la información básica para cada usuario en la tabla de líderes

## <a name="field-to-render-owning-service"></a>Campo para representar [servicio propietario]
* Indicador favoritos [usuarios]
* GameDisplayName [Profile]
* Rango [marcadores]
* Puntuación [marcadores]

## <a name="call-flow"></a>Flujo de llamadas
1. Llamar a las tablas de líderes pasando el moniker de personas a obtener el XUIDs, rank y las puntuaciones para el marcador determinado limitado a la lista de personas del usuario.
1. En paralelo:
 1. Llame al perfil, pasa la lista de XUIDs para obtener el nombre para mostrar y la URL de la imagen para cada uno.
 1. Llamar a las personas, pasa la lista de XUIDs para averiguar si alguno es Favoritos del usuario.
