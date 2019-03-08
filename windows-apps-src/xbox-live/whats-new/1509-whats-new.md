---
title: Novedades para el SDK de Xbox Live, septiembre de 2015
description: Novedades para el SDK de Xbox Live, septiembre de 2015
ms.assetid: 84b82fde-f6f3-4dc2-b2df-c7c7313a2cc3
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c618a738dc2670d3d3de1fa2f4c4108c24130eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602280"
---
# <a name="whats-new-for-the-xbox-live-sdk---september-2015"></a>Novedades para el SDK de Xbox Live, septiembre de 2015

Consulte la [What's New - agosto de 2015](1508-whats-new.md) artículo para lo que se agregó en la versión de agosto de 2015.

La versión de septiembre de Xbox Live SDK incluye las siguientes actualizaciones:

## <a name="os-and-tool-support"></a>Sistema operativo y la herramienta de soporte técnico ##
El SDK de Xbox Live es compatible con Windows 10 RTM [versión 10.0.10240] y Visual Studio 2015 RTM [versión 14.0.23107.0].

## <a name="contextual-search-apis"></a>API de búsqueda contextual
* Habilitar el título o la aplicación para buscar las difusiones de sus juegos con estadísticas en tiempo real de su elección.
* Consulte las nuevas API en Microsoft::Xbox::Services::ContextualSearch

## <a name="app-insights-for-events"></a>Application Insights para eventos

| Nota |
|------|
| App Insights solo se aplica a los títulos UWP.  Si está desarrollando un título XDK, esta sección no se aplica a usted |

<p/>

* Se pueden ver los eventos escritos mediante write_in_game_event() mediante AppInsights
* Estará disponible en esta documentación en el futuro, mientras tanto póngase en contacto con su madre para obtener acceso

## <a name="logging"></a>Registro
* service_call_logging_config in xbox::services::experimental
* Para iniciar y detener los seguimientos mediante xbTrace.exe en la consola, se debe llamar a register_for_protocol_activation en la clase service_call_logging_config.  Hacer esta llamada una vez durante la inicialización del juego.

## <a name="resync-for-rta"></a>Resincronización de la ATR
* Resincronización puede producirse cuando el servicio de ATR considera que la información de los usuarios puede ser obsoleta
* Títulos deben llamar a las llamadas HTTP correspondientes para las suscripciones que están suscritos a
* No es necesario volver a suscribirse títulos
* Se ha agregado xbox::services::real_time_activity_service::add_resync_handler
* Removed xbox::services::real_time_activity_service::remove_resync_handler
* Se ha agregado http_status_429_too_many_requests
* Esta condición de error se verán cuando se está limitando un título para el envío de demasiadas solicitudes de http

## <a name="documentation"></a>Documentación
* Migración a Xbox Live Services API 2.0
* Control de errores
* Autenticación activa de Xbox en Windows 10
* Búsqueda contextual
