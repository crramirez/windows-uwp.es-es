---
title: Plataforma de almacenamiento en vivo de Xbox
description: Obtenga información acerca de la Xbox Live plataforma de almacenamiento, que incluye el almacenamiento conectado y título.
ms.assetid: 3c92549c-65fd-4d26-a693-3aded8bae498
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8104c0945fb6987611e71e2ccb0fada2b746c03c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660200"
---
# <a name="xbox-live-storage-platform---connected-storage-title-storage"></a>Plataforma de almacenamiento en vivo de Xbox - conectado almacenamiento título

Xbox Live permite que los anunciantes almacenar datos de título globales y datos específicos del Reproductor en la nube.

## <a name="in-this-section"></a>En esta sección

[Almacenamiento conectados](connected-storage/connected-storage-overview.md) datos almacenados mediante la API de almacenamiento por usuario conectado, automáticamente se desplaza para que los usuarios en PC y múltiples consolas Xbox One y también está disponibles para su uso sin conexión. Use este servicio para permitir el juego continuar sin problemas al reiniciar un título después de cambiar entre dispositivos. Debe usar el servicio de almacenamiento conectado que guarde con frecuencia los datos de progreso, como inventario, estado del juego y la ubicación actual en juego. El servicio de almacenamiento conectado es el servicio de almacenamiento de más errores con tolerancia a errores en la nube y es menos susceptible a errores de energía y red.

[Almacenamiento de Xbox Live título](xbox-live-title-storage/xbox-live-title-storage.md) servicio la Xbox Live título Storage proporciona una manera de almacenar y compartir datos del juego y activos de título en la nube. Los juegos que se ejecutan en todas las plataformas pueden usar en línea. Este servicio proporciona mayor control sobre la visibilidad de los datos para el consumidor, así como global por datos del título además de los datos por usuario. Almacenamiento de título es ideal para almacenar estadísticas del Reproductor, la clasificación del jugador y activos de título, como material gráfico desbloquear de manera independiente y nuevas asignaciones.