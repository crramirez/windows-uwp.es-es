---
title: Novedades de Xbox Live
description: Novedades de Xbox Live SDK
ms.date: 10/23/2018
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33e5b6afbf0d60679bfce1789be2d965fd881f1c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614120"
---
# <a name="whats-new-for-xbox-live"></a>Novedades para Xbox Live
También puede comprobar el [historial de confirmaciones de GitHub de Xbox Live API](https://github.com/Microsoft/xbox-live-api/commits/master) para ver todos los cambios recientes de código a las API de Xbox Live.

## <a name="in-this-article"></a>En este artículo

* [Junio de 2018](#june-2018)
* [Agosto de 2017](#august-2017)
* [Julio de 2017](#july-2017)
* [Junio de 2017](#june-2017)
* [Mayo de 2017](#may-2017)
* [Abril de 2017](#april-2017)
* [Marzo de 2017](#march-2017)
* [Archived](#archived)

## <a name="june-2018"></a>Junio de 2018

### <a name="xbox-live-features"></a>Características de Xbox Live

#### <a name="c-api-layer-for-xsapi"></a>Capa de API de C para XSAPI

API de C ahora están disponibles para algunas características de Xbox Live. El nuevo nivel de API proporciona una serie de ventajas para las características admitidas, incluidos administración de memoria personalizado administración de subprocesos manual para las tareas asincrónicas y una nueva biblioteca HTTP.

Para obtener más información, consulte [API de C de Xbox Live](../xsapi-flat-c.md).

## <a name="august-2017"></a>Agosto de 2017

### <a name="xbox-live-features"></a>Características de Xbox Live

#### <a name="in-game-clubs"></a>En el juego clubes

Los desarrolladores pueden crear ahora "en el juego clubes". Gimnasios en juego diferencian clubes de Xbox estándar que son totalmente personalizables por un desarrollador y se puede usar tanto dentro como fuera del juego. Como desarrollador de juegos, puede usar para crear rápidamente cualquier tipo de escenarios de grupo persistente dentro de sus juegos, como los equipos, clans, squads, oficios, etc. que coinciden con sus propios requisitos.

Xbox live miembros pueden tener acceso a gimnasios en juego fuera su juego en cualquier experiencia de Xbox para mantenerse conectados entre sí y a su juego mediante el uso de características de club como chat, fuente, LFG y Mixer libremente en la consola Xbox, PC o dispositivos de iOS o Android.

Las API están disponibles para crear y administrar en el juego tréboles directamente desde dentro de su juego. Estas API existen en el espacio de nombres xbox::services::clubs.

## <a name="july-2017"></a>Julio de 2017

### <a name="xbox-live-features"></a>Características de Xbox Live

#### <a name="tournaments"></a>Torneos

Se agregaron nuevas API para admitir torneos. Ahora puede usar la clase xbox::services::tournaments::tournament_service para acceder al servicio de torneos desde su título.
Estos nuevos torneo API habilitar los escenarios siguientes:

* Consultar el servicio para encontrar todos los torneos existentes para el título de la actual.
* Recuperar los detalles de un torneo del servicio.
* Consultar el servicio para recuperar una lista de equipos para un torneo.
* Recuperar los detalles de los equipos de un torneo del servicio.
* Seguimiento de cambios para torneos y los equipos mediante el uso de las suscripciones de la actividad en tiempo Real (ATR).

## <a name="june-2017"></a>Junio de 2017

### <a name="xbox-live-features"></a>Características de Xbox Live

#### <a name="game-chat-2"></a>Chat de juego 2

Ahora está disponible una versión actualizada y mejorada de Chat de juego. Para obtener más información, consulte el [juego Chat 2 Introducción](../multiplayer/chat/game-chat-2-overview.md).

### <a name="xbox-live-tools"></a>Herramientas de Xbox Live

#### <a name="xbox-live-powershell-module"></a>Módulo de PowerShell de Xbox Live

* Se han agregado módulos de PowerShell para que resulte más fácil cambiar los espacios aislados en el equipo de desarrollo. Para obtener más información, consulte [herramientas](../tools/tools.md)

#### <a name="bug-fixes"></a>Correcciones de errores

* Varias correcciones de errores. Compruebe el [historial de confirmaciones de GitHub](https://github.com/Microsoft/xbox-live-api/commits/master) para obtener una lista completa.

## <a name="may-2017"></a>Mayo de 2017

### <a name="xbox-services-apis"></a>API de servicios de Xbox

#### <a name="multiplayer"></a>Varios jugadores

* Consultar los identificadores de búsqueda que ahora incluye las propiedades de sesión personalizada en la respuesta.

#### <a name="bug-fixes"></a>Correcciones de errores

* Se ha corregido "incorrecta" json"que se va a devolver en lugar de un código de error HTTP válido.

## <a name="april-2017"></a>Abril de 2017

### <a name="xbox-services-apis"></a>API de servicios de Xbox

#### <a name="visual-studio-2017"></a>Visual Studio 2017

Las API de Xbox Live se han actualizado a la compatibilidad con Visual Studio 2017, los títulos de Xbox One y de plataforma Universal de Windows (UWP).

#### <a name="tournaments"></a>Torneos

Se agregaron nuevas API para admitir torneos. Ahora puede usar el `xbox::services::tournaments::tournament_service` clase para tener acceso al servicio de torneos desde su título.

Estos nuevos torneo API habilitar los escenarios siguientes:

* Consultar el servicio para encontrar todos los torneos existentes para el título de la actual.
* Recuperar los detalles de un torneo del servicio.
* Consultar el servicio para recuperar una lista de equipos para un torneo.
* Recuperar los detalles de los equipos de un torneo del servicio.
* Seguimiento de cambios para torneos y los equipos mediante el uso de las suscripciones de la actividad en tiempo Real (ATR).

## <a name="march-2017"></a>Marzo de 2017

### <a name="xbox-services-api"></a>API de servicios de Xbox

#### <a name="data-platform-2017"></a>Plataforma de datos de 2017

Hemos introducido una API simplificada de estadísticas.  Tradicionalmente, debía enviar eventos correspondientes a stat reglas definidas en XDP o centro de partners y estos actualizan los valores stat en la nube.  Nos referimos a este modelo como estadísticas de 2013.

Con estadísticas de 2017, el título es ahora en el control de sus valores stat.  Basta con llamar a una API con el valor de estado más reciente y que se envía al servicio directamente sin necesidad de eventos.  Esto utiliza el nuevo `StatsManager` API y se puede leer más información en [estadísticas del Reproductor](../leaderboards-and-stats-2017/player-stats.md)

#### <a name="github"></a>GitHub

Xbox Live API (XSAPI) ahora está disponible en GitHub en [ https://github.com/Microsoft/xbox-live-api ](https://github.com/Microsoft/xbox-live-api).  Con los archivos binarios que vienen con el XDK o como NuGet paquetes se sigue recomendando, sin embargo es Bienvenidos a usar el origen de y agradecemos las contribuciones de código de origen.  

### <a name="xbox-live-creators-program"></a>Programa de creadores de Xbox Live

El programa de creadores de Xbox Live es un programa para desarrolladores que ofrece un subconjunto de funcionalidad de Xbox Live a una audiencia más amplia de desarrollador.  Si ya está en el ID@Xbox programa, esto no tendrá ningún impacto para usted.

Puede leer más acerca del programa en [visión general del programa para desarrolladores](../developer-program-overview.md).

### <a name="documentation"></a>Documentación

Hay los siguientes artículos nuevos:

| Artículo | Descripción |
|---------|-------------|
|[Configuración del servicio en directo de Xbox](../xbox-live-service-configuration.md) | Información actualizada acerca de cómo realizar la configuración del servicio para el título de Xbox Live
| [Configurar Xbox Live en Unity](../get-started-with-creators/configure-xbox-live-in-unity.md) | Información nueva sobre la instalación de Unity para los desarrolladores de programas de creadores de Xbox Live |
| [Espacios aislados de Xbox Live](../xbox-live-sandboxes.md) | Una guía simplificada para espacios aislados de Xbox Live y aislamiento de contenido |
| [Cuentas de prueba en vivo de Xbox](../xbox-live-test-accounts.md) | Información acerca de cómo probar el trabajo de las cuentas y cómo crearlos en el centro de partners |

## <a name="archived"></a>Archivado

* [Diciembre de 2016](1612-whats-new.md)
* [Noviembre de 2016](1611-whats-new.md)
* [Agosto de 2016](1608-whats-new.md)
* [Junio de 2016](1606-whats-new.md)
* [Abril de 2016](1603-whats-new.md)
* [Marzo de 2016](1603-whats-new.md)
* [Febrero de 2016](1602-whats-new.md)
* [Octubre de 2015](1510-whats-new.md)
* [Septiembre de 2015](1509-whats-new.md)
* [Agosto de 2015](1508-whats-new.md)
* [Junio de 2015](1506-whats-new.md)
