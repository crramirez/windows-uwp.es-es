---
title: API de Xbox Live C
description: Obtenga información sobre el modelo de API de C sin formato que puede usar para interactuar con el servicio Xbox Live.
ms.date: 06/05/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, c, xsapi
ms.localizationpriority: medium
ms.openlocfilehash: a1c73661b561d586f9e28957c7caa6a1b1f9cb03
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597530"
---
# <a name="introduction-to-the-xbox-live-c-apis"></a>Introducción a las API de Xbox Live C

En junio de 2018, se agregó una nueva capa de API de C sin formato a XSAPI. Este nuevo nivel de API soluciona algunos problemas que se produjeron con las capas de C++ y la API de WinRT.

La API de C no cubre todas las características XSAPI aún, pero están trabajando características adicionales. Todos los 3 API capas, C, C++ y WinRT seguirá siendo compatible y se agregaron características adicionales con el tiempo.

> [!NOTE]
> Las API de C solo funciona actualmente con los títulos que usar el Kit de desarrollo de Xbox (XDK). No admiten juegos UWP en este momento.

## <a name="features-covered-by-the-c-apis"></a>Características de las API de C

Actualmente, la API de C admite las siguientes características y servicios:

- Logros
- Presencia
- Perfil
- Redes sociales
- Administrador de redes sociales

## <a name="benefits-of-the-c-api-for-xsapi"></a>Ventajas de la API de C para XSAPI

- Permite que los títulos controlar las asignaciones de memoria al llamar a XSAPI.
- Permite que los títulos tener control total del subproceso que controla al llamar a XSAPI.
- Usa una nueva biblioteca HTTP, libHttpClient, diseñada para desarrolladores de juegos.

Puede usar las API de C junto con XSAPI de C++, pero no se obtienen las ventajas enumeradas anteriormente con las APIs de C++.

### <a name="managing-memory-allocations"></a>Administrar asignaciones de memoria

Con la nueva API de C, ahora puede especificar una devolución de llamada de función XSAPI llamará siempre que intenta asignar memoria. Si no especifica las devoluciones de llamada de función, XSAPI usará rutinas de asignación de memoria estándar.

Para especificar manualmente sus rutinas de memoria, puede hacer lo siguiente:

- Al principio del juego:
  - Llame a `XblMemSetFunctions(memAllocFunc, memFreeFunc)` para especificar las devoluciones de llamada de asignación para asignar y liberar memoria.
  - Llame a `XblInitialize()` para inicializar la instancia de la biblioteca.  
- Mientras se ejecuta el juego:
  - Llamar a cualquiera de las nuevas API de C en XSAPI que asignar o liberar memoria, producirá XSAPI llamar a la memoria especificada en las devoluciones de llamada del control.  
- Cuando se cierra el juego:
  - Llame a `XblCleanup()` para recuperar todos los recursos asociados con la biblioteca XSAPI.
  - Limpiar el Administrador de memoria personalizado de su juego.

### <a name="managing-asynchronous-threads"></a>Administración de subprocesos asincrónicos

La API de C se introduce un nuevo subproceso asincrónico llamada patrón que permite a los desarrolladores control total sobre el modelo de subprocesos. Para obtener más información, consulte [las llamadas asincrónicas de nivel C de planos de patrón de llamada para XSAPI](flatc-async-patterns.md).

## <a name="migrating-code-to-use-c-xsapi"></a>Migración de código para usar C XSAPI

Las XSAPI C APIs puede utilizarse junto con las API de C++ XSAPI en un proyecto, por lo que recomendamos que migre una característica a la vez.

Las API de C y C++ APIs son contenedores finos simplemente en torno a una base común, solo con los puntos de entrada diferentes, por lo que no debe cambiar la funcionalidad. Sin embargo, solo las API de C puede aprovechar las ventajas de la memoria personalizado y el subproceso de características de administración.

> [!IMPORTANT]
> No se pueden mezclar XSAPI WinRT APIs con las API de C.

## <a name="where-to-view-the-c-apis"></a>WHERE ver la API de C

- [Archivos de encabezado de la API de C](https://github.com/Microsoft/xbox-live-api/tree/master/Include/xsapi-c)
- [Código de ejemplo con las nuevas API de C](https://github.com/Microsoft/xbox-live-api/tree/master/InProgressSamples/Social/Xbox/C)
- [libHttpClient](https://github.com/Microsoft/libHttpClient)
