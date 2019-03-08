---
title: Novedades de las API de Xbox Live - abril de 2017
description: Novedades de las API de Xbox Live - abril de 2017
ms.assetid: ''
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, arena, torneos
ms.localizationpriority: medium
ms.openlocfilehash: a9256bfce05c14a0bfa63c7ec656dd899648d844
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651310"
---
# <a name="whats-new-for-the-xbox-live-apis---april-2017"></a>Novedades de las API de Xbox Live - abril de 2017

Consulte la [What's New - marzo de 2017](1703-whats-new.md) artículo para lo que se agregó en la versión de marzo de 2017.

## <a name="xbox-services-apis"></a>API de servicios de Xbox

### <a name="visual-studio-2017"></a>Visual Studio 2017

Las API de Xbox Live se han actualizado a la compatibilidad con Visual Studio 2017, los títulos de Xbox One y de plataforma Universal de Windows (UWP).

### <a name="tournaments"></a>Torneos

Se agregaron nuevas API para admitir torneos. Ahora puede usar el `xbox::services::tournaments::tournament_service` clase para tener acceso al servicio de torneos desde su título.

Estos nuevos torneo API habilitar los escenarios siguientes:

* Consultar el servicio para encontrar todos los torneos existentes para el título de la actual.
* Recuperar los detalles de un torneo del servicio.
* Consultar el servicio para recuperar una lista de equipos para un torneo.
* Recuperar los detalles de los equipos de un torneo del servicio.
* Seguimiento de cambios para torneos y los equipos mediante el uso de las suscripciones de la actividad en tiempo Real (ATR).
