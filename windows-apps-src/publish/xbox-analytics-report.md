---
author: jnHs
Description: The Xbox analytics report in the Windows Dev Center dashboard shows you statistics about how your customers are engaging with the Xbox features in your product.
title: Informe de análisis de Xbox
ms.author: wdg-dev-content
ms.date: 06/04/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, análisis de xbox, análisis dinámicos de xbox, estadística de xbox
ms.localizationpriority: medium
ms.openlocfilehash: 9e69c41ec2ae6dface93b9f3148e699e448faa18
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "5522854"
---
# <a name="xbox-analytics-report"></a>Informe de análisis de Xbox

En el informe de **análisis de Xbox** del Panel del centro de desarrollo de Windows se muestran estadísticas sobre la manera en que tus clientes interactúan con las características de Xbox en tu juego. También proporciona información sobre el estado del servicio para ayudarte a solucionar errores de cliente.

> [!IMPORTANT]
> Solo verás este informe si publicas un juego para Xbox o un juego que usa servicios de Xbox Live. Para ello, debe pasar por el [proceso de aprobación de concepto](../gaming/concept-approval.md), que incluye juegos publicados por [los partners de Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) y juegos enviados a través de la [ ID@Xbox programa](../xbox-live/developer-program-overview.md#id). Juegos publicados a través del [Programa de creadores de Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) no son visibles actualmente en este informe.

Para ver el informe de **Análisis de Xbox** en el menú de navegación izquierdo para tu juego, expande **Análisis** y selecciona **Análisis de Xbox**.  Puedes ver estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión.


## <a name="overview-tab"></a>Pestaña Introducción

Las secciones de la pestaña **Introducción** muestran información sobre quiénes son tus jugadores y cómo están interactuando con las características de Xbox Live.

Para muchas de estas estadísticas, también mostramos el **promedio de Xbox** para que puedas ver fácilmente cómo interactúan los clientes con Xbox en comparación con el cliente promedio de Xbox.

> [!NOTE]
> Estas estadísticas son de clientes que están conectados a Xbox Live, no todos los clientes de Xbox.


### <a name="concurrent-usage"></a>Uso simultáneo

En esta sección se muestran datos de uso casi en tiempo real (con latencia de entre 5 y 15 minutos) sobre el promedio de clientes que usan tu juego cada minuto u hora. Para elegir el intervalo de tiempo (desde **Última hora** hasta **Últimos 7 días**), selecciona el icono de filtro de la esquina superior derecha de esta sección.


### <a name="gamerscore-distribution"></a>Distribución de puntuación de jugador

En esta sección se muestra información sobre la puntuación de jugador de tus clientes. Puedes seleccionar **Todos los juegos** para ver la distribución de la puntuación total a través de tus clientes o seleccionar **Este juego** para ver la distribución de la puntuación de jugador obtenida solo a través de tu juego.


### <a name="achievement-unlocks"></a>Desbloqueos de logros

En esta sección se muestra el número total de clientes que han desbloqueado cada logro del intervalo de tiempo especificado. Para elegir el intervalo de tiempo (**Último día**, **Últimos 30 días** o **Duración**), selecciona el icono de filtro de la esquina superior derecha de esta sección.


### <a name="game-statistics"></a>Estadísticas de juegos

En esta sección se incluyen pestañas que puedes seleccionar para mostrar datos diferentes para los clientes de tu juego. Ten en cuenta que las estadísticas de esta sección hacen referencia al uso de características en general y no dentro de tu producto específico.

- En la pestaña **Social usage** se muestran los datos relacionados con la forma de interacción social de tus clientes.
   - **Game invites** muestra el porcentaje de tus clientes que han enviado invitaciones (para cualquier juego).
   - **Charla de grupo** muestra el porcentaje de los clientes que usan la charla de grupo (para cualquier juego).
   - **Mensajes de texto** muestra el porcentaje de los clientes que envían mensajes a través del Shell de Xbox (para cualquier juego).
- La pestaña **Streaming usage** muestra los porcentajes de clientes de tu juego que ven o hacen streaming del juego (para cualquier juego) en Twitch y YouTube.
- La pestaña **Game DVR usage** muestra datos relacionados con el modo en que los clientes graban y ven un juego. Puedes ver los porcentajes de clientes que han visto y cargado clips de juegos y capturas de pantalla del juego (para cualquier juego).


### <a name="friends-and-followers"></a>Amigos y seguidores

En esta sección se muestra **Median number of friends** y **Median number of followers** para los clientes que usan tu juego.


### <a name="accessory-usage"></a>Uso de accesorios

En este gráfico se muestran los porcentajes de los clientes de tu juego que usan unidades de disco duro externas y que usan Mandos Inalámbricos Xbox Elite (en Xbox).

Estos datos no indican los clientes que han instalado tu producto en unidades de disco duro externas o usan un mando Elite para jugar. Se refiere a cuántos de los clientes de tu producto usan estas características en general.


### <a name="connection-type"></a>Tipo de conexión

En este gráfico se muestran los porcentajes de los clientes de tu producto que usan conexiones a Internet **Por cable** frente a las conexiones a Internet de tipo **Inalámbrico** (en Xbox).


## <a name="xbox-live-service-health-tab"></a>Pestaña Estado del servicio de XboxLive

Las secciones de la **pestaña Estado del servicio de XboxLive** te ayudan a comprender el impacto de los errores de cliente de Xbox Live, incluida la limitación de velocidad. También te permite explorar por extremo y código de estado para obtener información que te ayuda a resolver estos problemas y te mantiene informado sobre la disponibilidad de servicio de Xbox Live específica de las llamadas de tu producto.

> [!NOTE]
> Al revisar esta información y resolver problemas, te recomendamos establecer prioridades en la limitación de la velocidad, ya que esos errores suelen tener el mayor impacto en el cliente.


### <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la pestaña, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **30D** (30 días), pero también puedes mostrar los datos durante **7D** (7 días) o durante un intervalo de fechas personalizado que especifiques (de no más de 30 días). Para un intervalo de fechas personalizado, ten en cuenta que todos los gráficos recortarán el intervalo del gráfico al primer y último día de los datos proporcionados dentro del intervalo de fechas que escribas.

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página por versión de paquete, tipo de dispositivo o espacio aislado.
- **Versión del paquete**: el filtro predeterminado es **Todas las versiones**, pero puedes limitar los datos de estado del servicio a una versión específica del paquete.
- **Tipo de dispositivo**: el valor predeterminado es **Todos los dispositivos**, pero también puedes limitar los datos de estado del servicio a un determinado tipo de dispositivo.
- **Espacio aislado**: el valor predeterminado es **VENTA AL DETALLE**, pero puedes limitar los datos de estado del servicio a un espacio aislado específico.

La información de todos los gráficos que aparecen a continuación reflejará el intervalo de fechas y los filtros que hayas seleccionado. Algunas secciones también te permiten aplicar filtros adicionales.


### <a name="client-errors-by-service"></a>Errores de cliente por servicio

El gráfico **Client errors by service** muestra el número de errores de cliente diarios (4xx) por cada servicio de Xbox Live durante el período de tiempo seleccionado.

También puedes ver solo los errores de limitación de velocidad seleccionando **Rate limiting**. Muestra el número de errores de limitación de velocidad diaria (429) y de exención de limitación de velocidad (429E) en cada servicio de Xbox Live durante el período de tiempo seleccionado.

> [!NOTE]
> Un código de estado 429E se devolvió realmente de manera correcta como código de estado 200, pero habría tenido límite de velocidad si el servicio estuviera teniendo un volumen alto en el momento, por lo que recomendamos que lo trate exactamente igual como si hubiera aplicado (429).

De manera predeterminada, este gráfico muestra los seis servicios principales por número de errores. Puedes seleccionar el icono de filtro en la esquina superior derecha de esta sección para elegir diferentes servicios. Puedes ver errores para un máximo de seis servicios a la vez.

> [!NOTE]
> La leyenda solo muestra el prefijo distintivo para cada servicio (por ejemplo, **presence** en lugar de **presence.xboxlive.com**). Encontrarás la dirección de servicio completo en la tabla **Client errors by endpoint** que se encuentra debajo de la pestaña **Estado del servicio de XboxLive**.


### <a name="service-availability"></a>Disponibilidad de servicio

En el gráfico **Disponibilidad del servicio** muestra la disponibilidad diaria en cada servicio de Xbox Live durante el período de tiempo seleccionado. Esto se calcula como *1-(errores de servidor totales (5xx)/respuesta totales)* y es específico de tu producto, no de Xbox Live en su conjunto.

De manera predeterminada, este gráfico muestra los seis servicios que han tenido la menor disponibilidad. Puedes seleccionar el icono de filtro en la esquina superior derecha de esta sección para elegir diferentes servicios. Puedes ver la disponibilidad para un máximo de seis servicios a la vez.

> [!NOTE]
> La leyenda solo muestra el prefijo distintivo para cada servicio (por ejemplo, **presence** en lugar de **presence.xboxlive.com**). Encontrarás la dirección de servicio completo en la tabla **Client errors by endpoint** que se encuentra debajo de la pestaña **Estado del servicio de XboxLive**.


### <a name="client-errors-by-endpoint"></a>Errores de cliente por extremo

En la tabla **Errores de cliente por extremo** se muestra el número de errores de cliente diarios (4xx) desglosados por cada servicio de Xbox Live, extremo y código de estado durante el período de tiempo seleccionado. De manera predeterminada, la tabla se ordena por el número total de respuestas de servicio en orden descendente, pero puedes cambiar el criterio de ordenación haciendo clic en cualquiera de los encabezados de columna.

También puedes ver solo los errores de limitación de velocidad seleccionando **Rate limiting**. Muestra el número de errores de limitación de velocidad diaria (429) y de exención de limitación de velocidad (429E) en cada servicio de Xbox Live, extremo y código de estado durante el período de tiempo seleccionado.

> [!NOTE]
> Un código de estado 429E se devolvió realmente de manera correcta como código de estado 200, pero habría tenido límite de velocidad si el servicio estuviera teniendo un volumen alto en el momento, por lo que recomendamos que lo trate exactamente igual como si hubiera aplicado (429).










 

 
