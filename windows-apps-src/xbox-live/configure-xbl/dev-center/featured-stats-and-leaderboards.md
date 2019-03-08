---
title: Destacados estadísticas y marcadores de 2017
description: Aprenda a configurar Xbox Live destacados de estadísticas y tablas de líderes de 2017 en el centro de partners
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, juegos, uwp, windows 10, Xbox uno, con todas las características de estadísticas y marcadores, marcadores, estadísticas de 2017, centro de partners
ms.openlocfilehash: e152ea8aa745536f0b7843f4f7d372a79dc3223f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655110"
---
# <a name="configuring-featured-stats-and-leaderboards-2017-in-partner-center"></a>Configuración de estadísticas destacadas y 2017 marcadores en el centro de partners

Para que un juego interactuar con el servicio de estadísticas, debe definirse en un estado [centro de partners](https://partner.microsoft.com/dashboard). Todas las estadísticas con las características se mostrarán en el GameHub, lo que permite actuar automáticamente como un marcador. Almacenamos el valor sin formato, sin embargo, el juego será propietaria de la lógica para determinar si se debe proporcionar un nuevo valor.

![Captura de pantalla de la página logros en el centro de juego](../../images/dev-center/featured-stats-and-leaderboards/featured-stats-and-leaderboards-2.png) la imagen anterior muestra cuál será la apariencia estadísticas destacado en GameHub del título. Son las estadísticas con las características que se muestra dentro del cuadro rojo.

Con la plataforma de datos de 2017, solo deberá configurar un estado que se usa para un marcador social que se incluye en la página de un jugador GameHub.

Puede usar el centro de partners para configurar un destacado stat y marcador asociado a su juego. Agregar configuración haciendo lo siguiente:

1. Navegue hasta la **estadísticas destacados y marcadores** sección del título, ubicado en **servicios** > **Xbox Live**  >  **Destacados de estadísticas y marcadores**.
2. Haga clic en el **New** botón que se abrirá un formulario modal. Una vez rellenado, haga clic en **guardar**.

![Imagen del cuadro de diálogo nuevo destacado stat/marcador](../../images/dev-center/featured-stats-and-leaderboards/featured-stats.png)

El **nombre para mostrar** campo es lo que verán los usuarios en el GameHub. Esta cadena se puede localizar en la **localizar cadenas** de la configuración del servicio Xbox Live.

El **ID** campo es el nombre stat y cómo hará referencia a su estado al actualizarlo desde el código. Consulte la [actualizar estadísticas](../../leaderboards-and-stats-2017/player-stats-updating.md) para obtener más detalles.

El **formato** es el formato de datos de la estadística. Las opciones incluyen Integer, Decimal, porcentaje, intervalo de tiempo breve, timespan largo y String.

Cada **formato** opciones proporcionarán cierta información en formato en la lista desplegable cuando se selecciona o en los valores aceptables.

* Estadísticas de entero aceptan números enteros como 1, 2 o 100.
* Estadísticas Decimal aceptan números fraccionarios con dos posiciones decimales como 1.05 o 12.00
* Estadísticas de porcentaje aceptan números enteros entre 0 y 100. '%' se anexará al final del número entero. (por ejemplo, 0%, 100%)
* Estadísticas de intervalo de tiempo breve siguen el formato hh: mm: al igual que 10:02:30 y le pedirá que proporcione una unidad de tiempo para la estadística.   Las unidades de tiempo disponibles son milisegundos, segundos, minutos, horas y días.
* Stat timespan Long siguen el formato de Xd Xh Xm como 1d 2h 10m, esta stat le pide que proporcione una unidad de tiempo para la estadística.

El **ordenación** campo le permite cambiar el criterio de ordenación de la tabla de clasificación ascendente o descendente.

Tenga en cuenta los siguientes requisitos al configurar un destacado stat y marcador:

| Tipo de desarrollador | Requisitos | Límite |
|----------------|-------------|-------|
| Programa de creadores de Xbox Live | No hay ningún requisito para designar las estadísticas como con todas las características de estadísticas | 20 |
| ID@Xbox y los socios de Microsoft | Debe designar al menos 3 estadísticas destacados | 20 |

## <a name="next-steps"></a>Pasos siguientes

A continuación, deberá actualizar las estadísticas desde el código.  Consulte [actualizar estadísticas](../../leaderboards-and-stats-2017/player-stats-updating.md) para obtener más detalles.
