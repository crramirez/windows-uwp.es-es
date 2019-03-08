---
title: Procedimientos recomendados para modo sin conexión
description: Obtenga información sobre los procedimientos recomendados para controlar los escenarios sin conexión con los títulos de Xbox Live habilitado.
ms.assetid: 6290dd67-1145-4fe2-8ada-c3a29a9ad29a
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, sin conexión
ms.localizationpriority: medium
ms.openlocfilehash: 5680b045873ebf6eed1422cb915f3f53df748790
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618730"
---
# <a name="best-practices-for-offline"></a>Procedimientos recomendados para modo sin conexión

Xbox One está diseñado como un dispositivo conectado, y las mejores experiencias, como juegos multijugador y streaming de vídeo, requieren conectividad. Sin embargo, Xbox One admiten muchas formas de reproducción sin conexión. En este tema se describe cómo controlar la reproducción sin conexión.

## <a name="overview"></a>Introducción

Un número creciente de los consumidores en todo el mundo tiene acceso a Internet. Sin embargo, hay lugares del mundo donde la conectividad es imprevisible, y hay veces cuando fallan los enrutadores, fibra todavía se corta, bloqueos de los servidores o quitar servicios inalámbricos.

Para admitir el conjunto más amplio de consumidores y experiencias, Xbox One permite para casos comunes donde la conectividad a Internet de vez en cuando se pierde o no está disponible por completo. El juego se informa de errores de conectividad, y es libre de elegir cómo responder: si el juego completo que sigue, degradar a un modo sin conexión, o final juego por completo.

## <a name="normal-online-operation"></a>Operación en línea normal

En general, las consolas Xbox One operan en un estado completamente conectado, donde el usuario tiene una conexión a Internet constante y el acceso completo a Xbox Live y los servicios de terceros. Este estado de conexión permite al servicio de Xbox Live validar periódicamente el estado de la consola, para proporcionar actualizaciones y realizar otros servicios en segundo plano que se benefician de los juegos y los usuarios.

Se recomienda que asumir que los usuarios tengan conectividad en línea la mayoría del tiempo.

## <a name="offline-play-principles"></a>Principios de reproducción sin conexión

Hay casos donde no está disponible una conexión en línea. Para la reproducción sin conexión, Xbox One está diseñado con los siguientes principios en mente:

-   Lo más importante, seguir jugando a los usuarios a pesar de los problemas de conectividad.

-   Seguir jugando a los usuarios incluso si no tienen conexión en absoluto.

-   Asegúrese de reproducción sin conexión sencilla y predecible para los usuarios, manteniendo el espíritu de una experiencia siempre conectados.

## <a name="offline-modes"></a>Modos sin conexión

Hay dos alto nivel *pérdida de conectividad* escenarios:

-   Pérdida completa del servicio de Internet

-   Pérdida de uno o varios servicios en línea

En cada uno de estos modos, hay una variedad de situaciones que pueden surgir. Se explican a continuación con ejemplos de escenarios sin conexión comunes que afectan al comportamiento del juego.

## <a name="offline-scenario-no-internet-service-upon-game-start"></a>Escenario sin conexión: ningún servicio de Internet en el inicio de juegos

Juegos pueden declarar a sí mismos como uno de los tres tipos:

-   *Xbox Live necesario*: todos los modos de juego requieren conectividad a Internet.

-   *Xbox Live Gold necesario*: todos los modos de juego requieren conectividad a Internet y una suscripción a Xbox Live Gold.

-   *Xbox Live no es necesario*: el juego tiene al menos un modo de reproducción que no requiere conectividad a Internet. Técnicamente, este tipo no se declara explícitamente en el manifiesto de aplicación. Una aplicación que no declara a sí mismo como uno de los dos primeros tipos se considera "No es necesario, de Xbox Live" o sin conexión se admite.

Cuando se inicia un juego por el usuario y la consola está sin conexión, el sistema comprueba la declaración de conectividad de juego en el manifiesto de aplicación. Si el juego requiere conectividad (ya sea de los primeros dos casos anteriores), el sistema muestra un mensaje al usuario y no inicia el título automáticamente.

Si la consola está sin conexión, el sistema solo iniciará títulos que tengan al menos un modo de reproducción que no requiere conectividad. En otras palabras, el sistema no iniciará "Xbox Live necesarios" o "Required de Xbox Live Gold" títulos.

## <a name="offline-scenario-connectivity-lost-during-gameplay"></a>Escenario sin conexión: se perdió la conectividad durante el juego

Si ya se está ejecutando un juego cuando se pierde la conectividad, se notificará el título por el sistema. Si el juego no usa los servicios en línea, continúe la sesión sin interrupción. Si el juego esté usando activamente los servicios en línea, cambie a un modo donde ya no son necesarios dichos servicios o informar al Reproductor que finaliza la sesión de juegos debido al estado sin conexión.

Los títulos que se declaran "Required de Xbox Live" o "Required de Xbox Live Gold" se suspenderá automáticamente por el sistema cuando la consola pierde toda la conectividad de red durante un período de tiempo y el sistema proporciona automáticamente un mensaje de error al usuario.

Como con cualquier otro escenario que implica la suspensión de juego, debe guardar el estado por lo que el usuario no perder datos y poder volver rápidamente a ese estado después de la recuperación de su conectividad.

## <a name="offline-scenario-when-a-single-xbox-live-service-is-down"></a>Escenario sin conexión: cuando un único servicio Xbox Live está inactivo

Hay otras situaciones en que los servicios en línea específicos no estén disponibles, aunque la conectividad a Internet está bien.

Por ejemplo, un único servicio de Xbox Live podría estar sin conexión durante un breve período de tiempo. En este caso, la llamada a específico del servicio expirará o notificar un error en el juego. Debe administrar correctamente los Estados de servicio sin conexión tal como se deben controlar aquellas situaciones en la consola Xbox 360 o en Windows.

Como mínimo, el juego no debe bloquearse o de bloqueo. Si el juego no puede continuar sin que el servicio, notificar la situación al usuario y permitir al usuario continuar en otra área del juego, donde el servicio no es necesario.

En el caso mejor, continuar emparejamiento y caché de datos para enviar faxes más adelante (si el juego estaba escribiendo en el servicio) o para hacer razonables suposiciones acerca de los datos (si el juego estaba leyendo desde el servicio).

## <a name="offline-scenario-when-a-third-party-service-is-down"></a>Escenario sin conexión: cuando un servicio de terceros está inactivo

Si su juego se basa en un servicio de terceros en línea, su juego también debe ser resistente si se produce un error de ese servicio. Si se produce un error en el servicio, a continuación, el juego no debe producir o de bloqueo. Puede informar del error de servicio al usuario si no puede continuar el juego. Idealmente, debe continuar el juego o debe permitir al usuario continuar en un área del juego que no requiere el servicio en línea.

## <a name="offline-scenario-when-xbox-live-cloud-compute-is-down"></a>Escenario sin conexión: cuando Xbox Live compute de la nube está inactivo

Uno de la Xbox One exhibe es poder de la nube. Algunos juegos pueden confiar completamente en un servicio conectado siempre como el proceso de la nube de Xbox Live, que permite tener acceso a recursos informáticos adicionales o servidores de juegos siempre disponible. Este modo siempre conectado se permiten tanto recomienda donde mejora la experiencia de los jugadores.

Si el juego utiliza este modo, se recomienda que el juego sea resistente a las interrupciones de servicio (de varios segundos y hasta un minuto) que son debido a cualquier pérdida total de conectividad a Internet o de un servicio de nube determinado. Sin embargo, el juego no debe tener un modo sin conexión. Si el juego realmente necesita conectividad y la conectividad no está disponible, a continuación, informar al usuario y finalizar la sesión de juego.

## <a name="xbox-requirements"></a>Requisitos de Xbox

El requisito más importante cuando se trabaja con escenarios sin conexión es estabilidad del juego. Ya sea la pérdida de conectividad completa o simplemente la pérdida de un servicio específico en línea, su juego debe de bloqueo no, bloqueos o causar que el usuario pierde el estado. El juego debe tener un sistema eficaz para controlar los escenarios de tiempo de espera de red y para controlar los errores devueltos por cualquier API que tiene acceso a un servicio en línea.

Juegos no son necesarios para admitir la reproducción sin conexión. Si su juego simplemente no puede continuar debido a una pérdida de conectividad de servicios, notificar al usuario, finalizar la sesión de juego y volver al menú principal o estado interactivo inicial.

## <a name="best-practices"></a>Procedimiento recomendado

Estos son los procedimientos recomendados para tratar con situaciones sin conexión:

-   Juegos suponer que los usuarios tengan conectividad en línea la mayoría del tiempo de diseño.

-   Si tiene sentido para el diseño del juego, considere la posibilidad de diseñar los modos de juego que permiten al usuario tener una experiencia agradable, incluso si la consola está en un estado sin conexión.

-   Servicios dejan de estar disponibles. Las conexiones pueden producir un error. Compilación sólido control de errores para las API que puede que el tiempo de espera, o las condiciones de error de informe cuando un servicio en línea está inactivo o cuando se pierde la conectividad a Internet. Siempre que sea posible, mantener a los usuarios reproducir a pesar de estos problemas.

-   Cumplir los requisitos de Xbox (XRs). No deje de responder o bloquearse.

-   Al recibir una notificación de suspensión PLM título, guardar estado para que el usuario no perder datos y poder volver rápidamente a ese estado cuando se reanuda el juego.

-   Marca el título de la forma adecuada en el manifiesto de aplicación. Sólo marcar el título como "Xbox Live required" si todos los modos de juego requieren conectividad.

-   Juegos de Xbox One se pueden usar y se basan en servicios en línea en todos los modos de juego. Si el juego simplemente no puede continuar debido a una pérdida de conectividad de servicios, notificar al usuario, finalizar la sesión de juego y volver al menú principal o estado interactivo inicial.

-   Ayuda relacionada con los Estados sin conexión y no confíe en el servicio de Ayuda de Xbox para mensajes de error. El servicio de Ayuda de Xbox requiere una conexión a Xbox Live.

## <a name="conclusion"></a>Conclusión

Xbox One está diseñado para un mundo conectado siempre. Sin embargo, en el camino a esa visión, la consola permite reproductores para disfrutar de escenarios sin conexión. Juegos deben ser robustos ante los errores de conectividad. Para juegos con experiencias de juego sin conexión, permitir que sus jugadores para disfrutar de estas experiencias al máximo. Para los juegos que siempre están en línea cuando se pierde la conectividad de red, devolver correctamente al usuario a un estado sin conexión.
