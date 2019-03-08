---
title: Configuración del servicio de creadores de Xbox Live
description: Obtenga información sobre la configuración del servicio de Xbox Live para el programa de creadores.
ms.assetid: 22b8f893-abb3-426e-9840-f79de0753702
ms.date: 10/03/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 76d25a48caadb908e30e6e1897c19178e2b837e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662240"
---
# <a name="xbox-live-service-configuration-for-the-creators-program"></a>Configuración del servicio de Xbox Live para el Programa de creadores

## <a name="what-is-service-configuration"></a>¿Qué es la configuración del servicio?

Puede que esté familiarizado con algunas de las características de Xbox Live, como [marcadores](../leaderboards-and-stats-2017/leaderboards.md) y [almacenamiento conectado](../storage-platform/connected-storage/connected-storage-technical-overview.md).

En caso de que no están, se explicará brevemente marcadores como ejemplo. Marcadores permiten que los reproductores ver un valor que representa un logro, en comparación con otros jugadores. Por ejemplo las puntuaciones más altas de un videojuego, tiempos de vuelta en un juego de carreras o headshots en una acción en primera persona. Pero a diferencia de una máquina recreativa que solo muestra las puntuaciones más altas de los jugadores que hayan jugado en esa máquina física, con Xbox Live se pueden mostrar las puntuaciones más altas de todo el mundo.

Pero para que ello, deberá configurar algunas opciones de un solo uso para que Xbox Live sepa sobre el marcador. Por ejemplo, si los valores se deben ordenar en orden ascendente o descendente de valor y qué parte de los datos debe ordenar.

Esta configuración se realiza en [centro de partners](https://partner.microsoft.com/dashboard) para el programa de creadores de Xbox Live y se puede leer [Introducción a trabajar con Xbox Live](get-started-with-xbox-live-creators.md) para aprender a configurar la aplicación.

## <a name="get-your-ids"></a>Obtener los identificadores

Para habilitar los servicios de Xbox Live, deberá obtener varios identificadores para configurar el entorno de desarrollo y el título. Éstos se pueden obtener mediante la actualización de la configuración del servicio Xbox Live.

Si actualmente no tiene un título en el centro de partners, consulte [creación y prueba un nuevo título de creadores](create-and-test-a-new-creators-title.md) para obtener instrucciones.

### <a name="critical-ids"></a>Identificadores de críticos

Hay tres identificadores que son fundamentales para el desarrollo de aplicaciones para Xbox One y títulos: el identificador de espacio aislado, el identificador del título y la configuración del servicio Id. (¿SCID).

Si bien es necesario tener un identificador de espacio aislado para configurar el entorno de desarrollo, el identificador de título y ¿SCID no son necesarios para el desarrollo inicial, pero son necesarios para cualquier uso de servicios de Xbox Live. Por lo tanto, recomendamos que obtenga los tres a la vez. Puede ver todos los identificadores de en la página de configuración de raíz "Xbox Live" tal como se muestra a continuación:

![](../images/getting_started/devcenter_sandbox_id.png)

#### <a name="sandbox-ids"></a>Identificadores de espacio aislado

El espacio aislado proporciona aislamiento de contenido para su entorno durante el desarrollo, asegurarse de que esté limpio para desarrollar y probar su título. El identificador de espacio aislado identifica el espacio aislado. Una consola solo puede tener acceso un recinto de seguridad en cualquier momento, aunque un recinto de seguridad puede tener acceso a múltiples consolas.

Los identificadores de espacio aislado distinguen mayúsculas de minúsculas.

#### <a name="service-configuration-id-scid"></a>Id. de configuración de servicio (¿SCID)

Como parte del desarrollo, creará las estadísticas, marcadores y muchas otras características en línea. Estos forman parte de la configuración del servicio y requieran la ¿SCID para el acceso. SCIDs distinguen mayúsculas de minúsculas.

#### <a name="title-id"></a>Id. de título

El identificador del título identifica el título de los servicios de Xbox Live. Sirve a lo largo de los servicios para permitir que los usuarios tener acceso a contenido en vivo de su título, sus estadísticas de usuario, logros etc. y para habilitar la funcionalidad de varios jugadores en vivo.

Los identificadores de título pueden ser distingue mayúsculas de minúsculas, dependiendo de cómo y dónde se usan.

## <a name="publish-your-xbox-live-service-configuration"></a>Publicar la configuración del servicio Xbox Live

Al realizar cambios en la configuración de Xbox Live para su juego, deberá publicar los cambios antes de que se recogen el resto de Xbox Live y pueden verse el juego. Cuando todavía está trabajando en su juego, publica en su propio espacio aislado de desarrollo. El espacio aislado de desarrollo permite trabajar en los cambios en su juego en un entorno aislado. Cuando se lance su juego al público, la configuración de Xbox Live se publicará automáticamente en el espacio aislado de venta directa.
De forma predeterminada, las consolas de una Xbox y equipos con Windows 10 están en el espacio aislado de venta directa.

En la página de configuración de Xbox Live, haga clic en el **prueba** botón para publicar la configuración actual de Xbox Live en su espacio aislado de desarrollo.

![](../images/creators_udc/creators_udc_xboxlive_config_test.png)