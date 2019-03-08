---
title: Metadatos de la interfaz de usuario de la API de arena
description: Contiene una lista completa de los metadatos de la interfaz de usuario para las API de Arena de Xbox.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, arena, torneo, experiencia de usuario
ms.localizationpriority: medium
ms.openlocfilehash: 63151d2c584218090f98a3c5f8089bf24bdb9b22
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659770"
---
# <a name="arena-apis-a-comprehensive-list-of-ui-metadata"></a>API de arena: Una lista completa de los metadatos de la interfaz de usuario

Las API de Arena devolver los metadatos para presentar los detalles del torneo, coincidencia y el Reproductor dentro de un juego. Esta es una lista completa.

DETALLES DEL TORNEO  | DETALLES DE COINCIDENCIA | DETALLES DEL EQUIPO  | DETALLES DEL REGISTRO
--- | --- | --- | ---
Nombre del organizador | Hora de finalización, la fecha y hora que este torneo ha alcanzado el estado de "Canceled" o "Completado". Se establece automáticamente cuando se actualiza un torneo. | Coincidencia anterior (indica el resultado de juegos de torneo, clasificación, hora de finalización, hora de inicio de coincidencia, es un Adiós, descripción, opuestas identificadores de equipo) | Estado de registro (unknown, pendiente, retirado, rechazado, registrado, completado)
Nombre del torneo (máximo 128 caracteres) | Es un Bye   | Estado de equipo (unknown, registrada, en lista de espera, en espera, protegido, reproducción en curso, completado) | Motivo de registro (miembro desconocido, cerrado, ya está registrado, torneo completa, equipo eliminado, completado del torneo)
Descripción del torneo (1000 caracteres como máximo) | Torneo resultado juego Estados (ningún concurso o canceladas, win, pérdida, draw, rank, sin mostrar) | Motivo de que el equipo está en un estado completado (rechazado, expulsar, eliminado, completado) | Registra el número mínimo y máximo de equipos
Hora de inicio y fin del registro | Estado de arbitraje: Completado, none, cancelado, no hay resultados, los resultados parciales | Fecha de registro, la fecha y hora que se registró un equipo. |
Compruebe en el tiempo de inicio y fin | Coincide con descriptivo 'etiqueta': ("final, 1 de calor") | Pendiente de equipo | Es el registro abierto
Hora de inicio y finalización de reproducción | Hora de inicio | Nombre de equipo | Comprobación bajo licencia Open
Tiene un premio | Id. de equipo opuestos | Clasificación final de equipo | Hora de inicio y fin del registro
Tamaño del equipo mín./máx. | | URI de continuación, vuelve a la interfaz de usuario de ámbito de los miembros del equipo | Compruebe en el tiempo de inicio y fin
Modo de juego | | Actual coincide con los metadatos (descripción, hora de inicio, Bye, opuestas identificadores de equipo) | Recuento de los equipos registrados
Estilo de torneo (eliminación única, round robin) | | Resumen del equipo (el estado del equipo, la clasificación) |
Es el registro abierto | | Nombres de jugadores |
Comprobación bajo licencia Open | | |
Recuento de los equipos registrado | | |
Se trata de abrir | | |
