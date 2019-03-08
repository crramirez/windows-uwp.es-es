---
title: Novedades para el SDK de Xbox Live - junio de 2016
description: Novedades para el SDK de Xbox Live - junio de 2016
ms.assetid: 306e7fa8-14f0-437f-a991-6693f5c0d6a8
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d984d054d9e5fd7f9d34b42c1a224d53632e719
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595290"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2016"></a>Novedades para el SDK de Xbox Live - junio de 2016

Consulte la [What's New - abril de 2016](1604-whats-new.md) artículo para lo que se agregó en la versión de abril de 2016.

> **Nota:** Si está utilizando el Kit de desarrolladores de Xbox (XDK), el *What's New - abril de 2016* artículo trata sobre la nueva funcionalidad adicional que se agregó desde el XDK marzo lanzamiento.

## <a name="os-and-tool-support"></a>Sistema operativo y la herramienta de soporte técnico
El SDK de Xbox Live es compatible con Windows 10 RTM [versión 10.0.10240] y Visual Studio 2015 RTM [versión 14.0.23107.0].

## <a name="contextual-search"></a>Búsqueda contextual
Se han agregado las siguientes clases nuevas para el `contextual_search` API para recuperar los clips de juegos:

* `contextual_search_game_clip`
* `contextual_search_game_clip_stat`
* `contextual_search_game_clip_thumbnail`
* `contextual_search_game_clip_uri_info`
* `contextual_search_game_clips_result`

Consulte la documentación de referencia para obtener más información.

## <a name="networking"></a>Funciones de red
La consola Xbox Live SDK ahora incluye un nuevo de aserción que detecta si se crean websockets 5 o más en una instancia única del título por usuario.

Puede deshabilitar esta aserción mediante una llamada a `disable_asserts_for_maximum_number_of_websockets_activated()`.

> **Nota:** Administrador de redes sociales y el Administrador de varios jugadores utilizan un único websocket combinado si usa estas características en el título de la.

## <a name="tools"></a>Herramientas
* La consola Xbox Live resistencia complemento para Fiddler ahora se incluye en el SDK de Xbox Live.  
Es un complemento de Fiddler que permite a los desarrolladores bloquear de manera selectiva las llamadas a Xbox Live.
Con este complemento, los programadores pueden probar la resistencia a través de las interrupciones del servicio parcial en sus títulos de juegos.
La herramienta incluye una serie de Xbox Live tipos de error integrados y diferentes de servicio los puntos de conexión para la prueba.
Se admiten los títulos de todos los Xbox One y UWP.
