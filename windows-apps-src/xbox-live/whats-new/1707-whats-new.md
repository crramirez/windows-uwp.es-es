---
title: Novedades en las API de Xbox Live - julio de 2017
description: Novedades en las API de Xbox Live - julio de 2017
ms.assetid: ''
ms.date: 07/16/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, novedades, julio de 2017
ms.localizationpriority: medium
ms.openlocfilehash: 47db721a98b71fd6f4b5175a88ddccd00048d8d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618830"
---
# <a name="whats-new-for-the-xbox-live-apis---july-2017"></a>Novedades de las API de Xbox Live - julio de 2017

Consulte la [What's New - junio de 2017](1706-whats-new.md) artículo para lo que se agregó en la versión de junio de 2017.

También puede comprobar el [historial de confirmaciones de GitHub de Xbox Live API](https://github.com/Microsoft/xbox-live-api/commits/master) para ver todos los cambios recientes de código a las API de Xbox Live.

## <a name="xbox-live-features"></a>Características de Xbox Live

### <a name="multiplayer-updates"></a>Actualizaciones de varios jugadores

Consultar los identificadores de actividad e identificadores de búsqueda ahora incluye las propiedades de sesión personalizada en la respuesta.

### <a name="tournaments"></a>Torneos

Se agregaron nuevas API para admitir torneos. Ahora puede usar la clase xbox::services::tournaments::tournament_service para acceder al servicio de torneos desde su título.
Estos nuevos torneo API habilitar los escenarios siguientes:
* Consultar el servicio para encontrar todos los torneos existentes para el título de la actual.
* Recuperar los detalles de un torneo del servicio.
* Consultar el servicio para recuperar una lista de equipos para un torneo.
* Recuperar los detalles de los equipos de un torneo del servicio.
* Seguimiento de cambios para torneos y los equipos mediante el uso de las suscripciones de la actividad en tiempo Real (ATR).
