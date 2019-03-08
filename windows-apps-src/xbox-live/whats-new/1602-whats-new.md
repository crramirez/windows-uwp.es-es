---
title: Novedades para el SDK de Xbox Live - febrero de 2016
description: Novedades para el SDK de Xbox Live - febrero de 2016
ms.assetid: 7ff34ea4-f07d-4584-98e4-13977994ccca
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c13b2893397a0d84e8919146435ece338bc1afde
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594740"
---
# <a name="whats-new-for-the-xbox-live-sdk---february-2016"></a>Novedades para el SDK de Xbox Live - febrero de 2016

Consulte la [What's New - octubre de 2015](1510-whats-new.md) artículo para las nuevas funciones de 1510

## <a name="os-and-tool-support"></a>Sistema operativo y la herramienta de soporte técnico
El SDK de Xbox Live es compatible con Windows 10 RTM [versión 10.0.10240] y Visual Studio 2015 RTM [versión 14.0.23107.0].

## <a name="throttling"></a>limitación
- Limitación específica pronto se lanzarán a más servicios de Xbox Live.  API de servicio de Xbox (XSAPI) controlar los reintentos automáticamente y le informa de las llamadas que están limitadas durante el desarrollo.  Pueden encontrar más detalles en el [mejores prácticas de una llamada a Xbox Live](../using-xbox-live/best-practices/best-practices-for-calling-xbox-live.md) artículo en la documentación.

## <a name="leaderboards"></a>Marcadores
- Tablas de clasificación de varias columnas ahora se pueden acceder mediante la API GetLeaderboard. Si proporciona un vector de los nombres de las columnas adicionales, el vector de columnas en el resultado se rellenen si existen esas columnas.

## <a name="documentation"></a>Documentación
- [Application Insights](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/application-insights) documentación está aquí.  Puede usar Application Insights con una cuenta gratuita de Azure para ver los eventos de la plataforma de datos en casi en tiempo real.  Actualmente, esta funcionalidad solo está disponible para aplicaciones de UWP que se ejecutan en Windows 10 en el escritorio.
- Documentación actualizada en la herramienta de eventos comunes de Xbox para desarrolladores de UWP que se traba sobre cómo generar contenedores para el envío de eventos de la plataforma de datos.  Tenga en cuenta que esto es opcional y puede seguir usar la API de WriteInGameEvent si lo prefiere.
- Uso de Fiddler para depurar eventos de la plataforma de datos y asegúrese de que se envían correctamente.  Esto es solo para eventos UWP.
- Información sobre cómo recopilar registros de la herramienta Analizador de seguimiento en vivo está disponible.  Consulte la [analizar las llamadas a servicios de Xbox Live](../tools/analyze-service-calls.md) artículo.
