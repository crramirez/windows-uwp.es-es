---
title: Estadísticas de los jugadores
description: Obtenga información sobre cómo configurar las estadísticas del Reproductor en Xbox Live.
ms.assetid: 5ec7cec6-4296-483d-960d-2f025af6896e
ms.date: 07/30/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, estadísticas del Reproductor, marcadores
ms.localizationpriority: medium
ms.openlocfilehash: e8b88f3db93f98789ada3c3b3dfe74195925657e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597310"
---
# <a name="player-stats"></a>Estadísticas del Reproductor

Como se describe en [información general sobre la plataforma de datos](../data-platform/data-platform.md), las estadísticas son piezas clave de la información que desea realizar un seguimiento sobre un reproductor. Por ejemplo *Head capturas* o *tiempo más rápido de vuelta*. Las estadísticas se utilizan para generar marcadores en una serie de escenarios que permitirá que los reproductores para comparar su esfuerzo y conocimientos con sus amigos y cada otro reproductor en comunidad de un título. Configura las estadísticas se muestran en un título [concentrador juego](../data-platform/designing-xbox-live-experiences.md) donde un reproductor verá cómo clasifican con sus amigos que también se hayan reproducido el título del marcador. Se llama a las estadísticas que se muestran en juego concentrador de un título **característica estadísticas**, también conocido como **Hero estadísticas**. Además de aparecer en el centro de juegos, desde la actualización de Xbox One 1806 también periódicamente pueden aparecer estadísticas destacado en anclados bloques de contenido que los usuarios pueden agregar a su vista principal. Esto permite un acceso rápido a marcadores sociales de directamente desde la vista principal. Es importante recordar que las estadísticas con las características son uno de varios elementos posibles mostrados en anclado bloques de contenido, por lo que es posible que no siempre verlos usuarios si Servicios de contenido de Microsoft determinan que otro elemento de contenido podría ser más atractivo.

## <a name="stats-2013-and-2017"></a>Estadísticas de 2013 y 2017

Actualmente hay dos implementaciones para estadísticas de Xbox Live, estadísticas de 2013 y estadísticas de 2017. Ambos están disponibles para ID@Xbox y administra los desarrolladores asociado. Los desarrolladores de programas de creadores de Xbox Live solo pueden usar 2017 de estadísticas y por lo que pueden pasar por alto las estadísticas de 2013.

Los desarrolladores de programas de creadores de Xbox Live pueden ir directamente a la [documento estadísticas 2017](stats2017.md).

Estos dos implementaciones operan sobre los principios fundamentalmente distintos. Cuando se usa estadísticas 2013 envía **eventos** para el servicio de Xbox Live que contiene cierta información sobre una acción de un usuario ha realizado. La información en estos **eventos** se usa para actualizar las estadísticas en consecuencia. El servicio realizar un seguimiento de y actualizar todos los valores de estadísticas, lo que el origen de datos para los valores de estadística para un reproductor o el grupo de jugadores en estadísticas de 2013. Por otro lado, en 2017 estadísticas enviará el valor real de stat propio para el servidor. En el servidor se encarga de estadísticas 2017 poca o ninguna validación en el valor enviado a él por lo que es hasta el título para realizar un seguimiento de los valores correctos stat y ser el origen de datos para los valores de estadística. Se recomienda realizar un seguimiento y almacenar las estadísticas en la nube con la [plataforma de almacenamiento de Xbox Live](../storage-platform/storage-platform.md) si decide implementar estadísticas 2017. Estadísticas de 2017 se puede considerar como un servicio de informes. Enviar el estado correcto para su juego en el servidor, stat, a continuación, se colocan en el servidor y esperar que se muestren en la solicitud o actualizado.

## <a name="a-brief-history"></a>Una breve historia

Con el lanzamiento de Xbox One, Xbox Live presentó a un nuevo modelo de estadísticas controlado por eventos, estadísticas de Reproductor de 2013. Esto ofrece una gran cantidad de beneficios: un solo evento de un juego puede actualizar los datos de varias características de Xbox Live, como tablas de clasificación y logros; Configuración de Xbox Live se encuentra en el servidor en lugar de en el cliente; y mucho más. En los años posteriores al lanzamiento de Xbox One, hemos tenido en cuenta estrechamente a los comentarios de desarrolladores de juegos y los desarrolladores constantemente solicita un servicio de estadísticas más sencillo que podría omitir la complejidad que conlleva un evento controlado por el sistema, así como permitir ellos para que use los métodos que ya estaban practicando de seguimiento de estadísticas. Según los comentarios de desarrollador se creó una nueva versión simplificada de estadísticas que colocaría control de la lógica de estadísticas de vuelta en las manos del desarrollador. Que sistema 2017 estadísticas, un servicio que simplemente toma el valor pasado que el título de otorgar a los desarrolladores de control de la lógica de cómo se determina un valor stat.

## <a name="how-stats-are-handled-in-2013-and-2017"></a>Cómo se controlan las estadísticas en 2013 y 2017

Echemos un vistazo a configurar y actualizar las estadísticas de 2013 y 2017. Supongamos que va a crear estadísticas de algunos rpg genérico y desea realizar un seguimiento de que termine.

En 2013 estadísticas enviaría el título de un *evento* que contiene información sobre una acción realizada por un reproductor. En este caso la acción se se lo que quiera un orc mientras el Reproductor tenía un filo equipado. Parte de la información contenida en este evento podría ser que se realizó una acción slay, el slayed lo era un orc, el tipo de combatir era melee y la suya usan era un filo. Estadísticas de 2013 se ejecutará esta información a través de un número de reglas configuradas por el usuario, el desarrollador, en el [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts) o [centro de partners](https://partner.microsoft.com/dashboard), y actualizar estadísticas, también configuradas por el usuario, en función de el evento. En 2013 estadísticas el servicio realizará el seguimiento del cuál debería ser el valor para las estadísticas slaying. Puede actualizar el orc slay con eventos filo varias estadísticas, como, número de derribos, número de orcs slain y número de derribos filo.

En las estadísticas de 2017, enviará los valores reales para las estadísticas. Para orc de slay con ejemplo filo, el título de mantener el seguimiento del número de derribos general filo derribos y orcs slain individualmente y el servicio de envío el número actualizado para cada estadística. El servicio tiene las comprobaciones de validación mínima para asegurarse de que está enviando un número que tenga sentido, por lo que es absolutamente hasta el título de la estadística correcta de envío. Aunque puede usar el servicio de 2017 de estadísticas para recuperar los valores de las estadísticas de al principio de una sesión de juego, no debe usar el servicio de 2017 de estadísticas para confirmar el valor de un estado mientras la sesión está en curso.

A continuación puede ver una ilustración de cómo funcionan los dos tipos de estadísticas.
![Ilustración de la diferencia de estadísticas](../images/stats/Stats2013-7DiagramColored.jpg)

## <a name="a-few-more-notes"></a>Algunas notas más

Para ID@Xbox y los desarrolladores pueden socio administrado hay algunas diferencias más debe tener en cuenta para decidir entre 2013 estadísticas y estadísticas de 2017 para el título.

Estadísticas de 2013 permite más vistas de marcador.
Estadísticas de 2013 permite que más fácilmente profundizar en los metadatos de las estadísticas. En nuestro orc matar la estadística general se mantiene el seguimiento del número de derribos de ejemplo. Estadísticas 2013 le permite explorar en profundidad en las estadísticas para lo que se ha eliminado y con qué arma, así como cualquier otro kill define la información que puede configurar.

Estadísticas de 2013 tiene una stat nativo de minutos jugados. Estadísticas de 2013 permite el que acceso a tiempo de reproducción de un jugador de juego sin configurar la estadística. Cualquier estadística de reproducción tendría que realizar un seguimiento de su título en estadísticas de 2017.

Estadísticas de 2013 tiene una frecuencia de actualización casi en tiempo real.
El sistema de eventos de estadísticas 2013 actualiza las estadísticas casi al instante. En estadísticas 2017 hay un tiempo de espera de cinco minutos antes de que actualice las estadísticas se vacían en el servicio. El desarrollador puede vaciar manualmente las estadísticas, pero todavía se reducirán al mínimo de 30 segundos entre vaciados.

Estadísticas de 2013 puede admitir otros servicios de Xbox Live.
Con estadísticas 2013 puede utilizar las estadísticas para desbloquear los logros y tomar decisiones de contactos. Estadísticas 2017 solo se utiliza para generar marcadores.

## <a name="further-reading"></a>Obtener más información

Para obtener una explicación más detallada de las estadísticas de 2013 puede leer el [sólo la documentación de 2013 de estadísticas asociada a](https://developer.microsoft.com/en-us/games/xbox/docs/xboxlive/xbox-live-partners/event-driven-data-platform/user-stats).
Para una explicación más detallada de las estadísticas de 2017, lea el [estadísticas 2017 documentación](stats2017.md).