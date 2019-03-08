---
title: Configuración del servicio de Xbox Live
description: Obtenga información sobre cómo configurar el servicio para su juego de Xbox Live.
ms.assetid: 631c415b-5366-4a84-ba4f-4f513b229c32
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, configuración del servicio
ms.localizationpriority: medium
ms.openlocfilehash: d12c66e61a189c13ddbcd96dd99caa351206ecf6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640910"
---
# <a name="xbox-live-service-configuration"></a>Configuración del servicio de Xbox Live

## <a name="what-is-service-configuration"></a>¿Qué es la configuración del servicio?

Puede que esté familiarizado con algunas de las características de Xbox Live, como [logros](achievements-2017/achievements.md), [marcadores](leaderboards-and-stats-2017/leaderboards.md) y [contactos](multiplayer/multiplayer-concepts.md#smartmatch-matchmaking).

En caso de que no están, se explicará brevemente marcadores como ejemplo. Marcadores permiten que los reproductores ver un valor que representa un logro, en comparación con otros jugadores.  Por ejemplo las puntuaciones más altas de un videojuego, tiempos de vuelta en un juego de carreras o headshots en una acción en primera persona. Pero a diferencia de una máquina recreativa que solo muestra las puntuaciones más altas de los jugadores que hayan jugado en esa máquina física, con Xbox Live se pueden mostrar las puntuaciones más altas de todo el mundo.

Pero para que ello, deberá configurar algunas opciones de un solo uso para que Xbox Live sepa sobre el marcador. Por ejemplo, si los valores se deben ordenar en orden ascendente o descendente de valor y qué parte de los datos debe ordenar.

Esta configuración se realiza en [centro de partners](https://partner.microsoft.com/dashboard) la mayoría del tiempo. Pero algunos desarrolladores usan [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com).

Si eres un desarrollo de su título como parte del programa de creadores de Xbox Live, usa [centro de partners](https://partner.microsoft.com/dashboard), y puede leer [Introducción a trabajar con Xbox Live](get-started-with-creators/get-started-with-xbox-live-creators.md) para aprender a configurar la aplicación.

Si eres un ID@Xbox desarrollador o trabajar con un publicador que es un Partner de Microsoft, lea en.

## <a name="choose-your-development-portal"></a>Elija su portal de desarrollo

Como se mencionó anteriormente, hay dos portales diferentes que pueden usarse para configurar servicios de Xbox Live. Centro de partners en [ https://partner.microsoft.com/dashboard ](https://partner.microsoft.com/dashboard) y el Portal de desarrollo de Xbox (XDP) en [ https://xdp.xboxlive.com ](https://xdp.xboxlive.com).

Centro de partners se recomienda para todos los títulos a partir de ahora, pero para determinadas características, es posible que aún desea usar XDP. En esta sección le ayudará a aconsejar a dónde se configura su título.

Puede encontrar información acerca de las páginas de configuración de servicio específico según su portal elegido:

* [Configuración del centro de partners](configure-xbl/windows-dev-center.md)
* [Configuración del Portal de desarrollo de Xbox](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/atoc-service-configuration) : para tener acceso a este vínculo, debe tener una cuenta de Microsoft (MSA) que se ha habilitado para el acceso completo de Xbox Live.

Si ya tiene un título configurado, puede desplazarse hacia abajo para [obtener los identificadores de](#get_ids) para obtener información sobre cómo obtener los identificadores distintos necesarios para configurar el título.

### <a name="pcmobile-uwp-game-only"></a>Juego de PC/Mobile UWP solo
Centro de partners se recomienda para configurar y administrar los juegos UWP que se ejecutan solo en equipos con Windows 10 y dispositivos móviles Windows 10.

#### <a name="using-xdp-to-configure-uwp-titles"></a>Uso de XDP para configurar los títulos UWP

Es posible que desee usar XDP para configurar los títulos UWP si tiene uno de los siguientes requisitos:

1. Usa Arena.
2. Tiene la configuración de permisos, grupos y usuarios existentes en XDP que desea seguir usando.
3. Está usando las herramientas que solo funcionan en XDP como la herramienta de torneos o el Visor del historial de sesión de directorio de sesión de varios jugadores.
4. Está desarrollando un título que tendrá las play multiplataforma entre una versión UWP PC/mobile del mismo juego y el juego de Xbox XDK uno basado.

Si no está en una de esas categorías, a continuación, debe usar Centro de partners. En caso contrario, puede ver a continuación para que saber cómo usar XDP para configurar un título UWP.

Usar XDP para configurar servicios de Xbox Live para las aplicaciones de UWP tiene algunas advertencias importantes:

* **Una vez que se publica la configuración del servicio de un juego Xbox Live CERT/Retail en XDP, no hay ninguna volviendo!** Para la configuración del servicio Xbox Live debe permanecer en XDP durante la vida útil del título del juego de juegos.
* **No hay ninguna ruta de migración de XDP al centro de partners.** Si inicia la configuración de Xbox Live en XDP, manualmente debe crearla en el centro de partners si desea moverla.

Dadas estas dos consideraciones, se recomienda usar Centro de partners para juegos de PC o móvil, a menos que se clasifican en alguna de las categorías que se ha descrito anteriormente.

### <a name="cross-play-between-xbox-one-and-pcmobile-games"></a>Cross-play entre los juegos de Xbox One y PC/Mobile ###
Juegos de dispositivo cruzado entre Xbox One y el PC, conocido como entre-play, es una experiencia de presentación de Windows 10. En este escenario, las versiones de un juego de Xbox One y PC comparten la misma configuración de Xbox Live.

Este escenario también tratan el caso de que tiene un juego de Xbox uno XDK existente y desea agregar compatibilidad cruzada-play para una versión UWP de su juego.

Para poder implementar entre-play, haga lo siguiente:

* **Use XDP para configurar y publica tu juego XDK.** Centro de partners no es compatible con juegos de Xbox uno XDK en este momento.
* **Usar una única configuración de servicio de Xbox Live que creaste en XDP para el XDK y las versiones UWP de un juego.** XDP ahora tiene nuevas características que permiten un juego compartir una única configuración de servicio de Xbox Live entre la versión XDK y la versión UWP de un juego.
* **Usar el centro de partners para ingerir y publique su juego para UWP.** Sin embargo, no utilice centro de partners para configurar los servicios de Xbox Live, como su juego va a usar la configuración del servicio que creó en XDP.
* **No se divide la configuración del servicio de Xbox Live entre XDP y centro de partners.** XDP y centro de partners no están relacionados entre sí y publicación de una configuración de servicio desde una fuente sobrescribe la configuración publicada desde otro origen. Esto podría provocar que los datos de usuario se perderá (logros que faltan, borrados juegos guardados, etc.) que puede crear una mala experiencia del usuario. Por este motivo, **nos requieren que el 100% de la configuración del servicio de Xbox Live se realiza en XDP para entre play XDK + UWP juegos.**

Hay más detalles sobre este proceso, incluidos los elementos que son *no* autoservicio en el [Introducción a entre jugar a juegos](get-started-with-partner/get-started-with-cross-play-games.md) guía

### <a name="separate-versions-of-xbox-one-and-pcmobile-games-that-are-not-cross-play"></a>Versiones independientes de juegos de Xbox One y PC/Mobile que no están entre-play
Puede decidir mantener separado de la versión de PC/Mobile del juego mismo su versión de un juego de Xbox One. En este caso, cree dos productos independientes y siga las instrucciones para Xbox uno XDK solo y el juego para UWP de PC/Mobile sólo respectivamente.

No se puede usar la misma configuración de servicio para ambas versiones en este caso, y debe crear manualmente la configuración del servicio para cada versión independiente de su juego, en XDP o en el centro de partners de forma adecuada.

<a name="get_ids"></a>

## <a name="get-your-ids"></a>Obtener los identificadores

Para habilitar los servicios de Xbox Live, deberá obtener varios identificadores para configurar el kit de desarrollo y su título. Éstos se pueden obtener mediante la configuración del servicio de Xbox Live.

Si no tiene actualmente un título en XDP o centro de partners, consulte la sección anterior [portales de la configuración del servicio de Xbox Live](#xbox_live_portals) para obtener instrucciones.

### <a name="critical-ids"></a>Identificadores de críticos

Hay tres identificadores que son fundamentales para el desarrollo de aplicaciones para Xbox One y títulos: el identificador de espacio aislado, el identificador del título y el ¿SCID.

Si bien es necesario tener un identificador de espacio aislado para usar un kit de desarrollo, el identificador de título y ¿SCID no son necesarios para el desarrollo inicial, pero son necesarios para cualquier uso de servicios de Xbox Live. Por lo tanto, recomendamos que obtenga los tres a la vez.

#### <a name="sandbox-ids"></a>Identificadores de espacio aislado

El espacio aislado proporciona aislamiento de contenido para el kit de desarrollo durante el desarrollo, asegúrese de que tiene un entorno limpio para desarrollar y probar su título. El identificador de espacio aislado identifica el espacio aislado. Una consola solo puede tener acceso un recinto de seguridad en cualquier momento, aunque un recinto de seguridad puede tener acceso a múltiples consolas.

Los identificadores de espacio aislado distinguen mayúsculas de minúsculas.

**Centro de partners**

Si está configurando su título en el centro de partners, obtenga el identificador de espacio aislado en la página de configuración de raíz "Xbox Live" tal como se muestra a continuación:

![](images/getting_started/devcenter_sandbox_id.png)

**XDP**

Si va a configurar su título en XDP, obtendrá el identificador de espacio aislado en la página información general del producto tal como se muestra a continuación:

![](images/getting_started/xdp_sandbox_id.png)

#### <a name="service-configuration-id-scid"></a>Id. de configuración de servicio (¿SCID)

Como parte del desarrollo, creará un host de otras características en línea, logros y eventos. Estos forman parte de la configuración del servicio y requieran la ¿SCID para el acceso.

SCIDs distinguen mayúsculas de minúsculas.

**Centro de partners**

Para recuperar su ¿SCID en Centro de partners, vaya a la sección de servicios de Xbox Live y vaya a *el programa de instalación de Xbox Live* . ¿Su SCID se muestra en la tabla que se muestra a continuación:

![](images/getting_started/devcenter_scid.png)

**XDP**

Para recuperar su ¿SCID en XDP, vaya a la sección "Configuración del producto" bajo el título y verá el identificador de título y ¿SCID.

![](images/getting_started/xdp_scid.png)

#### <a name="title-id"></a>Id. de título

El identificador del título identifica el título de los servicios de Xbox Live. Sirve a lo largo de los servicios para permitir que los usuarios tener acceso a contenido en vivo de su título, sus estadísticas de usuario, logros etc. y para habilitar la funcionalidad de varios jugadores en vivo.

Los identificadores de título pueden ser distingue mayúsculas de minúsculas, dependiendo de cómo y dónde se usan.

**Centro de partners**

El identificador del título en el centro de partners se encuentra en la misma tabla como el ¿SCID en el *el programa de instalación de Xbox Live* página.

**XDP**

El identificador de título en XDP se obtiene desde la misma ubicación como la ¿SCID es.

<a name="xbox_live_portals"></a>
