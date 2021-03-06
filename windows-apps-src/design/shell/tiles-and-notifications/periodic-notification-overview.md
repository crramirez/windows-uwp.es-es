---
description: Las notificaciones periódicas, también denominadas notificaciones de sondeo, actualizan los iconos y los distintivos a intervalos fijos mediante la descarga de contenido directamente desde un servicio de nube.
title: Introducción a las notificaciones periódicas
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5860c4e45a25b0d141e1b2c57421dd2640d5fbb4
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033788"
---
# <a name="periodic-notification-overview"></a>Introducción a las notificaciones periódicas
 


Las notificaciones periódicas, también denominadas notificaciones de sondeo, actualizan los iconos y los distintivos a intervalos fijos mediante la descarga de contenido directamente desde un servicio de nube. Para usar las notificaciones periódicas, el código de tu aplicación cliente debe proporcionar dos elementos de información:

-   El identificador uniforme de recursos (URI) de una ubicación web para que Windows sondee las actualizaciones de icono o de distintivo para tu aplicación
-   La frecuencia con la que debe sondearse ese URI

Las notificaciones periódicas permiten a la aplicación obtener actualizaciones activas de icono con niveles mínimos de inversión de clientes y servicio de nube. También son un excelente método para distribuir el mismo contenido a una audiencia amplia.

**Nota** Para obtener más información, descarga [Push and periodic notifications sample (Muestra de notificaciones de inserción y periódicas)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Push%20and%20periodic%20notifications%20client-side%20sample%20(Windows%208)) para Windows 8.1 y vuelve a usar su código fuente en la aplicación de Windows 10.

 

## <a name="how-it-works"></a>Cómo funciona


Las notificaciones periódicas requieren que la aplicación hospede un servicio de nube. Todos los usuarios que tienen la aplicación instalada sondean el servicio de forma periódica. En cada intervalo de sondeo, por ejemplo, una vez cada hora, Windows envía una solicitud HTTP GET al URI, descarga el contenido del icono o distintivo solicitado (como XML) que se suministra en la respuesta a la solicitud, y muestra el contenido en el icono de la aplicación.

Ten en cuenta que las actualizaciones periódicas no se pueden usar como notificaciones del sistema. Las notificaciones del sistema se entregan mejor mediante notificaciones [programadas](/previous-versions/windows/apps/hh465417(v=win.10)) o [de inserción](/previous-versions/windows/apps/hh868252(v=win.10)).

## <a name="uri-location-and-xml-content"></a>Ubicación del URI y contenido XML


Se puede usar cualquier dirección web HTTP o HTTPS válida como el URI que se sondeará.

La respuesta del servidor de nube incluye el contenido descargado. El contenido que devuelve el URI debe cumplir la especificación del esquema XML de [icono](adaptive-tiles-schema.md) o [distintivo](/uwp/schemas/tiles/badgeschema/schema-root), y debe usar la codificación UTF-8. Puedes usar encabezados HTTP definidos para especificar el [tiempo de caducidad](#expiration-of-tile-and-badge-notifications) o la etiqueta de la notificación.

## <a name="polling-behavior"></a>Comportamiento del sondeo


Para comenzar a sondear llama a uno de estos métodos:

-   [**StartPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (icono)
-   [**StartPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.BadgeUpdater#Windows_UI_Notifications_BadgeUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (distintivo)
-   [**StartPeriodicUpdateBatch**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (icono)

Cuando se llama a uno de estos métodos, se sondea inmediatamente el URI y el icono o distintivo se actualiza con el contenido recibido. Después de este sondeo inicial, Windows continúa proporcionando actualizaciones con el intervalo solicitado. El sondeo continúa hasta que lo detienes explícitamente (con [**TileUpdater.StopPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.TileUpdater.StopPeriodicUpdate)), se desinstala tu aplicación o, en el caso de un icono secundario, se quita el icono. En caso contrario, Windows sigue sondeando las actualizaciones del icono o distintivo aunque la aplicación no vuelva a iniciarse nunca.

### <a name="the-recurrence-interval"></a>Intervalo de periodicidad

El intervalo de periodicidad se especifica como un parámetro de los métodos enumerados anteriormente. Ten en cuenta que aunque Windows intenta sondear según lo solicitado, el intervalo no es preciso. El intervalo de sondeo solicitado se puede retrasar hasta 15 minutos a elección de Windows.

### <a name="the-start-time"></a>Hora de inicio

Puedes especificar, opcionalmente, una hora específica del día para empezar el sondeo. Tomemos como ejemplo una aplicación que cambia el contenido de su icono una vez al día. En este caso, recomendamos que el sondeo se realice cerca de la hora en la que actualizas el servicio de nube. Por ejemplo, si un sitio de compras diario publica las ofertas del día a las 8 de la mañana, realiza el sondeo de nuevo contenido de icono poco después de las 8 de la mañana.

Si proporcionas una hora de inicio, la primera llamada al método sondea el contenido inmediatamente. Después, el sondeo periódico comienza a los 15 minutos de la hora de inicio indicada.

### <a name="automatic-retry-behavior"></a>Comportamiento de reintentos automáticos

El URI solo se sondea si el dispositivo está en línea. Si la red está disponible pero no se puede establecer contacto con el URI por algún motivo, esta iteración del intervalo de sondeo se omite y el URI se sondeará de nuevo en el siguiente intervalo. Si el dispositivo está en estado apagado, suspendido o hibernado cuando se alcanza un intervalo de sondeo, el URI se sondea cuando el dispositivo se enciende o se reactiva.

### <a name="handling-app-updates"></a>Control de actualizaciones de aplicaciones

Si publica una actualización de la aplicación que cambia el URI de sondeo, debe agregar una [tarea en segundo plano de desencadenador de tiempo](../../../launch-resume/run-a-background-task-on-a-timer-.md) diario que llama a StartPeriodicUpdate con el nuevo URI para asegurarse de que los mosaicos usan el nuevo URI. De lo contrario, si los usuarios reciben la actualización de la aplicación, pero no inician la aplicación, sus iconos seguirán usando el URI anterior, que puede que no se muestren si el URI no es válido o si la carga devuelta hace referencia a imágenes locales que ya no existen.

## <a name="expiration-of-tile-and-badge-notifications"></a>Expiración de las notificaciones de icono y de distintivo


De forma predeterminada, las notificaciones de icono y de distintivo periódicas expiran tres días después del momento en que se descargaron. Cuando una notificación expira, el contenido se quita del distintivo, el icono o la cola y no se vuelve a mostrar al usuario. Por lo tanto, se recomienda establecer un tiempo de caducidad explícito en todas las notificaciones de icono y de distintivo periódicas, con un tiempo apropiado para tu aplicación o notificación, para garantizar que el contenido persistirá solo mientras sea relevante. El tiempo de caducidad explícito resulta esencial para contenido con una vida útil definida. También garantiza la eliminación de contenido obsoleto si el servicio de nube se vuelve inaccesible, o si el usuario se desconecta de la red durante un período de tiempo prolongado.

El servicio de nube establece una hora y una fecha de caducidad para una notificación incluyendo el encabezado X-WNS-Expires HTTP en la carga de respuesta. El encabezado HTTP X-WNS-Expires sigue el [formato de fecha de HTTP](https://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.3.1). Para obtener más información, consulta [**StartPeriodicUpdate**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) o [**StartPeriodicUpdateBatch**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_).

Por ejemplo, durante un día de gran actividad en el mercado de valores, puedes establecer la caducidad para la actualización del precio de unas acciones en el doble del intervalo de sondeo (por ejemplo, una hora después de la recepción si estás sondeando cada media hora). Otro ejemplo sería una aplicación de noticias, que podría determinar que un día es un tiempo de caducidad apropiado para una actualización diaria del icono de noticias.

## <a name="periodic-notifications-in-the-notification-queue"></a>Notificaciones periódicas en la cola de notificaciones


Puedes usar actualizaciones de icono periódicas con el [ciclo de notificaciones](/previous-versions/windows/apps/hh781199(v=win.10)). De manera predeterminada, un icono de la pantalla Inicio muestra el contenido de una sola notificación hasta que es reemplazada por una notificación nueva. Cuando el ciclo de notificaciones está habilitado, se mantienen hasta cinco notificaciones en la cola y el icono las recorre cíclicamente.

Si la cola ha alcanzado su capacidad de cinco notificaciones, la próxima notificación nueva reemplazará la notificación más antigua de la cola. No obstante, si se establecen etiquetas en las notificaciones, puede afectar a la directiva de reemplazo de la cola. Una etiqueta es una cadena específica de la aplicación, y que no distingue entre mayúsculas y minúsculas, de hasta 16 caracteres especificada en el encabezado HTTP [X-WNS-Tag](/previous-versions/windows/apps/hh465435(v=win.10)) de la carga de respuesta. Windows compara la etiqueta de una notificación entrante con las etiquetas de todas las notificaciones que ya están en la cola. Si se encuentra una coincidencia, la nueva notificación reemplaza la notificación de la cola que tiene la misma etiqueta. Si no se encuentra ninguna coincidencia, se aplica la regla de reemplazo predeterminado y la nueva notificación reemplaza la notificación más antigua de la cola.

Puedes usar la cola y las etiquetas de notificaciones para implementar diferentes escenarios de notificación. Por ejemplo, una aplicación de bolsa podría enviar cinco notificaciones, cada una en relación con una cotización diferente y etiquetada con el nombre de la cotización. Esto permite que la cola nunca contenga dos notificaciones para la misma cotización; la más antigua se considera obsoleta.

Para obtener más información, vea [usar la cola de notificaciones](/previous-versions/windows/apps/hh781199(v=win.10)).

### <a name="enabling-the-notification-queue"></a>Habilitar la cola de notificaciones

Para implementar una cola de notificaciones, habilita primero la cola para tu icono (consulta [Cómo usar la cola de notificación con notificaciones locales](/archive/blogs/tiles_and_toasts/quickstart-how-to-use-the-tile-notification-queue-with-local-notifications)). Solo es necesario realizar una vez la llamada para habilitar la cola durante la vida útil de la aplicación, pero no pasa nada si se realiza cada vez que la aplicación se inicia.

### <a name="polling-for-more-than-one-notification-at-a-time"></a>Sondeo de más de una notificación al mismo tiempo

Debes proporcionar un URI único para cada notificación que quieras que Windows descargue para tu icono. Con el método [**StartPeriodicUpdateBatch**](/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_), puedes proporcionar hasta cinco URI al mismo tiempo para usarlos con la cola de notificaciones. Cada URI se sondea para una única carga de notificación, al mismo tiempo o casi al mismo tiempo. Cada URI sondeado puede devolver su propio tiempo de caducidad y valor de etiqueta.

## <a name="related-topics"></a>Temas relacionados


* [Directrices sobre notificaciones periódicas]()
* [Cómo configurar notificaciones periódicas para distintivos](/previous-versions/windows/apps/hh761476(v=win.10))
* [Cómo configurar notificaciones periódicas para iconos](/previous-versions/windows/apps/hh761476(v=win.10))
 
