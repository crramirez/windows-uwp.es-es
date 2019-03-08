---
title: Novedades para el SDK de Xbox Live, junio de 2015
description: Novedades para el SDK de Xbox Live, junio de 2015
ms.assetid: 354bcd47-2564-4dd5-89e3-242bca462b35
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a42d0fb0a3cb457a60a0542bfc5966893d00f18b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627880"
---
# <a name="whats-new-for-the-xbox-live-sdk---june-2015"></a>Novedades para el SDK de Xbox Live, junio de 2015

La versión de junio de Xbox Live SDK incluye las siguientes actualizaciones:

## <a name="os-and-tool-support"></a>Sistema operativo y la herramienta de soporte técnico ##
El SDK de Xbox Live ahora es compatible con la última compilación de Insider de Windows 10 y Visual Studio 2015 RC.

## <a name="title-callable-ui-apis"></a>API de la interfaz de usuario que se puede llamar de título

| Nota |
|------|
| En esta sección solo se aplica a los títulos de UWP, deben hacer referencia a la clase Windows.Xbox.UI.SystemUI títulos XDK para TCUI  |

El SDK de Xbox Live ahora contiene API que admiten la interfaz de usuario que se puede llamar de título (TCUI) para mostrar la interfaz de usuario en un dispositivo Windows 10 PC/Mobile del contenedor.

Estas API solo están disponibles para aplicaciones UWP.

Puede llamar a los contenedores TCUI desde las clases de title_callable_ui (C++) y TitleCallableUI (WinRT).

Las acciones se incluyen las interfaces de usuario:
* IU del selector de Reproductor.
* IU del selector de invitación para jugar.
* Una tarjeta de perfil del Reproductor la interfaz de usuario
* Una interfaz de usuario de confianza de agregar o quitar
* Un logros título juegos de mostrar la interfaz de usuario

Ver el nuevo *TCUI* como ejemplo de uso de las nuevas API. Puede encontrar el ejemplo en {*raíz del SDK de código fuente*} \Samples\TCUI.

## <a name="new-authentication-model-for-uwp-apps"></a>Nuevo modelo de autenticación para aplicaciones UWP
El SDK de Xbox Live ahora admite el uso de una cuenta de Microsoft (MSA) para identificar el Reproductor de Xbox Live en un dispositivo Windows 10 PC/Mobile.

Ahora puede iniciar sesión en un usuario mediante el uso de las clases de xbox_live_user (C++) y XboxLiveUser (WinRT).

## <a name="new-api-for-writing-events-in-uwp-apps"></a>Nueva API para escribir eventos en aplicaciones para UWP

| Nota |
|------|
| En esta sección solo se aplica a los títulos UWP.  Los desarrolladores XDK deben hacer referencia a la [eventos de juego](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/game-events) artículo para obtener información específica de XDK  |

El nuevo EventsService (WinRT) y las clases de events_service (C++) le permiten escribir eventos en el juego que se pueden actualizar las estadísticas de usuario, logros, tablas de clasificación, etcetera. Estas nuevas clases son solo para aplicaciones UWP.

## <a name="breaking-change-to-event-handlers"></a>Cambio importante para los controladores de eventos ##
Todos los controladores de eventos en el SDK de C++ han cambiado de una sola `void set_*_handler()` método a un par de `function_context add_*_handler()` y `void remove_*_handler(function_context context)` métodos.

Cada `add_*_handler()` método ahora devuelve un `function_context` objeto. Cuando se quita el controlador de eventos, debe pasar el `function_context` objeto.

Por ejemplo:
```
function_context subscriptionLostContext = xbox_live_context()->multiplayer_service().add_multiplayer_subscription_lost_handler(...);

localUser->xbox_live_context()->multiplayer_service().remove_multiplayer_subscription_lost_handler(subscriptionLostContext);
```

## <a name="new-apis-for-managing-multiplayer-scenarios"></a>Nuevas API para administrar escenarios multijugador
El SDK de Xbox Live ahora incluye un conjunto de APIs de C++ para administrar escenarios comunes de varios jugadores. Las API se incluyen en el espacio de nombres xbox.services.experimental.multiplayer.

Estas API están en el espacio de nombres experimental, lo que significa que es probables que cambie según los comentarios.  Animamos a los desarrolladores a usarlas y proporcionar comentarios.

Ver el nuevo *MultiplayerManager* como ejemplo de uso de estas nuevas API. Puede encontrar el ejemplo en {*raíz del SDK de código fuente*} \Samples\MultiplayerManager.
