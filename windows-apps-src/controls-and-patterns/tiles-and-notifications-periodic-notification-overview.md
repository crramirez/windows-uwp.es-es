---
author: mijacobs
Description: "Las notificaciones periódicas, también denominadas notificaciones de sondeo, actualizan los iconos y los distintivos a intervalos fijos mediante la descarga de contenido directamente desde un servicio de nube."
title: "Introducción a las notificaciones periódicas"
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
label: TBD
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 37858532004f477672f2c19adad93a22ca989f39

---
# Introducción a las notificaciones periódicas
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


Las notificaciones periódicas, también denominadas notificaciones de sondeo, actualizan los iconos y los distintivos a intervalos fijos mediante la descarga de contenido directamente desde un servicio de nube. Para usar las notificaciones periódicas, el código de tu aplicación cliente debe proporcionar dos elementos de información:

-   El identificador uniforme de recursos (URI) de una ubicación web para que Windows sondee las actualizaciones de icono o de distintivo para tu aplicación
-   La frecuencia con la que debe sondearse ese URI

Las notificaciones periódicas permiten a la aplicación obtener actualizaciones activas de icono con niveles mínimos de inversión de clientes y servicio de nube. También son un excelente método para distribuir el mismo contenido a una audiencia amplia.

**Nota** Para obtener más información, descarga [Push and periodic notifications sample (Muestra de notificaciones de inserción y periódicas)](http://go.microsoft.com/fwlink/p/?linkid=231476) para Windows 8.1 y vuelve a usar su código fuente en la aplicación de Windows 10.

 

## Cómo funciona


Las notificaciones periódicas requieren que la aplicación hospede un servicio de nube. Todos los usuarios que tienen la aplicación instalada sondean el servicio de forma periódica. En cada intervalo de sondeo, por ejemplo, una vez cada hora, Windows envía una solicitud HTTP GET al URI, descarga el contenido del icono o distintivo solicitado (como XML) que se suministra en la respuesta a la solicitud, y muestra el contenido en el icono de la aplicación.

Ten en cuenta que las actualizaciones periódicas no se pueden usar como notificaciones del sistema. Las notificaciones del sistema se entregan mejor mediante notificaciones [programadas](https://msdn.microsoft.com/library/windows/apps/hh465417) o [de inserción](https://msdn.microsoft.com/library/windows/apps/xaml/hh868252).

## Ubicación del URI y contenido XML


Se puede usar cualquier dirección web HTTP o HTTPS válida como el URI que se sondeará.

La respuesta del servidor de nube incluye el contenido descargado. El contenido que devuelve el URI debe cumplir la especificación del esquema XML de [icono](tiles-and-notifications-adaptive-tiles-schema.md) o [distintivo](https://msdn.microsoft.com/library/windows/apps/br212851), y debe usar la codificación UTF-8. Puedes usar encabezados HTTP definidos para especificar el [tiempo de caducidad](#expiry) o la [etiqueta](#taggo) de la notificación.

## Comportamiento del sondeo


Para comenzar a sondear llama a uno de estos métodos:

-   [**StartPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701684) (icono)
-   [**StartPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701611) (distintivo)
-   [**StartPeriodicUpdateBatch**](https://msdn.microsoft.com/library/windows/apps/hh967945) (icono)

Cuando se llama a uno de estos métodos, se sondea inmediatamente el URI y el icono o distintivo se actualiza con el contenido recibido. Después de este sondeo inicial, Windows continúa proporcionando actualizaciones con el intervalo solicitado. El sondeo continúa hasta que lo detienes explícitamente (con [**TileUpdater.StopPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701697)), se desinstala tu aplicación o, en el caso de un icono secundario, se quita el icono. En caso contrario, Windows sigue sondeando las actualizaciones del icono o distintivo aunque la aplicación no vuelva a iniciarse nunca.

### Intervalo de periodicidad

El intervalo de periodicidad se especifica como un parámetro de los métodos enumerados anteriormente. Ten en cuenta que aunque Windows intenta sondear según lo solicitado, el intervalo no es preciso. El intervalo de sondeo solicitado se puede retrasar hasta 15 minutos a elección de Windows.

### Hora de inicio

Puedes especificar, opcionalmente, una hora específica del día para empezar el sondeo. Tomemos como ejemplo una aplicación que cambia el contenido de su icono una vez al día. En este caso, recomendamos que el sondeo se realice cerca de la hora en la que actualizas el servicio de nube. Por ejemplo, si un sitio de compras diario publica las ofertas del día a las 8 de la mañana, realiza el sondeo de nuevo contenido de icono poco después de las 8 de la mañana.

Si proporcionas una hora de inicio, la primera llamada al método sondea el contenido inmediatamente. Después, el sondeo periódico comienza a los 15 minutos de la hora de inicio indicada.

### Comportamiento de reintentos automáticos

El URI solo se sondea si el dispositivo está en línea. Si la red está disponible pero no se puede establecer contacto con el URI por algún motivo, esta iteración del intervalo de sondeo se omite y el URI se sondeará de nuevo en el siguiente intervalo. Si el dispositivo está en estado apagado, suspendido o hibernado cuando se alcanza un intervalo de sondeo, el URI se sondea cuando el dispositivo se enciende o se reactiva.

## Expiración de las notificaciones de icono y de distintivo


De forma predeterminada, las notificaciones de icono y de distintivo periódicas expiran tres días después del momento en que se descargaron. Cuando una notificación expira, el contenido se quita del distintivo, el icono o la cola y no se vuelve a mostrar al usuario. Por lo tanto, se recomienda establecer un tiempo de caducidad explícito en todas las notificaciones de icono y de distintivo periódicas, con un tiempo apropiado para tu aplicación o notificación, para garantizar que el contenido persistirá solo mientras sea relevante. El tiempo de caducidad explícito resulta esencial para contenido con una vida útil definida. También garantiza la eliminación de contenido obsoleto si el servicio de nube se vuelve inaccesible, o si el usuario se desconecta de la red durante un período de tiempo prolongado.

El servicio de nube establece una hora y una fecha de caducidad para una notificación incluyendo el encabezado X-WNS-Expires HTTP en la carga de respuesta. El encabezado HTTP X-WNS-Expires sigue el [formato de fecha de HTTP](http://go.microsoft.com/fwlink/p/?linkid=253706). Para obtener más información, consulta [**StartPeriodicUpdate**](https://msdn.microsoft.com/library/windows/apps/hh701684) o [**StartPeriodicUpdateBatch**](https://msdn.microsoft.com/library/windows/apps/hh967945).

Por ejemplo, durante un día de gran actividad en el mercado de valores, puedes establecer la caducidad para la actualización del precio de unas acciones en el doble del intervalo de sondeo (por ejemplo, una hora después de la recepción si estás sondeando cada media hora). Otro ejemplo sería una aplicación de noticias, que podría determinar que un día es un tiempo de caducidad apropiado para una actualización diaria del icono de noticias.

## Notificaciones periódicas en la cola de notificaciones


Puedes usar actualizaciones de icono periódicas con el [ciclo de notificaciones](https://msdn.microsoft.com/library/windows/apps/hh781199). De manera predeterminada, un icono de la pantalla Inicio muestra el contenido de una sola notificación hasta que es reemplazada por una notificación nueva. Cuando el ciclo de notificaciones está habilitado, se mantienen hasta cinco notificaciones en la cola y el icono las recorre cíclicamente.

Si la cola ha alcanzado su capacidad de cinco notificaciones, la próxima notificación nueva reemplazará la notificación más antigua de la cola. No obstante, si se establecen etiquetas en las notificaciones, puede afectar a la directiva de reemplazo de la cola. Una etiqueta es una cadena específica de la aplicación, y que no distingue entre mayúsculas y minúsculas, de hasta 16 caracteres especificada en el encabezado HTTP [X-WNS-Tag](https://msdn.microsoft.com/library/windows/apps/hh465435.aspx#pncodes_x_wns_tag) de la carga de respuesta. Windows compara la etiqueta de una notificación entrante con las etiquetas de todas las notificaciones que ya están en la cola. Si se encuentra una coincidencia, la nueva notificación reemplaza la notificación de la cola que tiene la misma etiqueta. Si no se encuentra ninguna coincidencia, se aplica la regla de reemplazo predeterminado y la nueva notificación reemplaza la notificación más antigua de la cola.

Puedes usar la cola y las etiquetas de notificaciones para implementar diferentes escenarios de notificación. Por ejemplo, una aplicación de bolsa podría enviar cinco notificaciones, cada una en relación con una cotización diferente y etiquetada con el nombre de la cotización. Esto permite que la cola nunca contenga dos notificaciones para la misma cotización; la más antigua se considera obsoleta.

Para obtener más información, consulta [Uso de la cola de notificaciones](https://msdn.microsoft.com/library/windows/apps/hh781199).

### Habilitar la cola de notificaciones

Para implementar una cola de notificaciones, habilita primero la cola para tu icono (consulta [Cómo usar la cola de notificación con notificaciones locales](https://msdn.microsoft.com/library/windows/apps/hh465429)). Solo es necesario realizar una vez la llamada para habilitar la cola durante la vida útil de la aplicación, pero no pasa nada si se realiza cada vez que la aplicación se inicia.

### Sondeo de más de una notificación al mismo tiempo

Debes proporcionar un URI único para cada notificación que quieras que Windows descargue para tu icono. Con el método [**StartPeriodicUpdateBatch**](https://msdn.microsoft.com/library/windows/apps/hh967945), puedes proporcionar hasta cinco URI al mismo tiempo para usarlos con la cola de notificaciones. Cada URI se sondea para una única carga de notificación, al mismo tiempo o casi al mismo tiempo. Cada URI sondeado puede devolver su propio tiempo de caducidad y valor de etiqueta.

## Temas relacionados


* [Directrices sobre notificaciones periódicas](https://msdn.microsoft.com/library/windows/apps/hh761461)
* [Cómo configurar notificaciones periódicas para distintivos](https://msdn.microsoft.com/library/windows/apps/hh761476)
* [Cómo configurar notificaciones periódicas para iconos](https://msdn.microsoft.com/library/windows/apps/hh761476)
 

 







<!--HONumber=Aug16_HO3-->


