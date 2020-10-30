---
description: El informe de análisis de Xbox del centro de Partners muestra estadísticas sobre cómo los clientes intervienen con las características de Xbox del producto.
title: Informe de análisis de Xbox
ms.date: 03/21/2019
ms.topic: article
keywords: Windows 10, UWP, análisis de Xbox, análisis de Xbox Live, estadísticas de Xbox
ms.localizationpriority: medium
ms.openlocfilehash: bbe57fa444c4cb43e24944378a49b33f61883866
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034878"
---
# <a name="xbox-analytics-report"></a>Informe de análisis de Xbox

El informe de **análisis de Xbox** del [centro de Partners](https://partner.microsoft.com/dashboard) muestra estadísticas sobre cómo los clientes intervienen con las características de Xbox del juego. También proporciona información sobre el estado del servicio para ayudarle a solucionar los errores de los clientes.

> [!IMPORTANT]
> Solo verá este informe si está publicando un juego para Xbox o un juego que usa los servicios de Xbox Live. Para ello, debe seguir el [proceso de aprobación del concepto](../gaming/concept-approval.md), que incluye juegos publicados por asociados de [Microsoft](/gaming/xbox-live/developer-program-overview#microsoft-partners) y juegos enviados a través del [ ID@Xbox programa](/gaming/xbox-live/developer-program-overview#id). Los juegos publicados a través del [programa de creadores de Xbox Live](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) no están visibles actualmente en este informe.

Puede ver el informe de **análisis de Xbox** en el menú de navegación izquierdo del juego expandiendo el **análisis y** seleccionando el análisis de **Xbox** .  Puede ver estos datos en el centro de Partners o [descargar el informe](download-analytic-reports.md) para verlo sin conexión.


## <a name="overview-tab"></a>Pestaña Información general

Las secciones de la pestaña **información general** muestran información sobre quién son sus jugadores y cómo intervienen con las características de Xbox Live.

Para muchas de estas estadísticas, también se muestra la **media** de la Xbox para que pueda ver fácilmente cómo los clientes interactúan con Xbox en comparación con el cliente de Xbox promedio.

> [!NOTE]
> Estas estadísticas son de clientes que están conectados a Xbox Live, no a todos los clientes de Xbox.


### <a name="concurrent-usage"></a>Uso simultáneo

En esta sección se muestran datos de uso casi en tiempo real (con una latencia de 5-15 minutos) sobre el promedio de clientes que juegan el juego cada minuto o hora. Puede elegir el intervalo de tiempo (de la **última hora** hasta los **últimos 7 días** ) seleccionando el icono de filtro en la esquina superior derecha de esta sección.


### <a name="gamerscore-distribution"></a>Distribución de Gamerscore

En esta sección se muestra información sobre el Gamerscore de los clientes. Puede seleccionar **todos los juegos** para ver la distribución del total de Gamerscore en todos los clientes o seleccionar **este juego** para ver la distribución de Gamerscore obtenida solo a través del juego.


### <a name="achievement-unlocks"></a>Desbloqueos de logros

En esta sección se muestra el número total de clientes que han desbloqueado cada logro en el intervalo de tiempo especificado. Puede elegir el intervalo de tiempo (el **último día** , los **últimos 30 días** o la **duración** ) seleccionando el icono de filtro en la esquina superior derecha de esta sección.


### <a name="game-statistics"></a>Estadísticas del juego

En esta sección se incluyen pestañas que puede seleccionar para Mostrar datos diferentes para los clientes del juego. Tenga en cuenta que las estadísticas de esta sección hacen referencia al uso de características en general y no dentro de su producto específico.

- En la pestaña **uso social** se muestran los datos relacionados con el modo en que los clientes interactúan con la social.
   - Los **invitados del juego** muestran el porcentaje de los clientes que han enviado invitados (para cualquier juego).
   - **Chat de entidad** muestra el porcentaje de los clientes que usan el chat de terceros (para cualquier juego).
   - **Mensajes de texto** muestra el porcentaje de los clientes que envían mensajes a través de la consola Xbox (para cualquier juego).
- En la pestaña **uso de streaming** se muestran los porcentajes de los clientes del juego que ven o transmiten por secuencias (para cualquier juego) en Twitch y YouTube.
- En la pestaña **uso de DVR de juego** se muestran datos relacionados con el modo en que los clientes registran y ven el juego. Puede ver los porcentajes de los clientes que han visto y cargado clips de juegos y capturas de pantallas de juego (para cualquier juego).


### <a name="friends-and-followers"></a>Amigos y seguidores

En esta sección se muestra el **número medio de amigos** y el **número medio de seguidores** para los clientes que juegan su juego.


### <a name="accessory-usage"></a>Uso del accesorio

Este gráfico muestra los porcentajes de los clientes del juego que usan unidades de disco duro externas y que usan controladores inalámbricos de Xbox Elite (en Xbox).

Estos datos no significan que los clientes que instalaron el producto en discos duros externos o usaran un controlador Elite mientras lo reproducen. En general, se refiere a cuántos de los clientes del producto utilizan estas características.


### <a name="connection-type"></a>Tipo de conexión

Este gráfico muestra los porcentajes de los clientes del producto que usan conexiones de Internet **cableadas** frente a **redes inalámbricas** (en Xbox).


## <a name="xbox-live-service-health-tab"></a>Pestaña Estado del servicio Xbox Live

Las secciones de la **pestaña Estado del servicio Xbox Live** le ayudan a comprender el impacto de los errores de cliente de Xbox Live, incluida la limitación de velocidad. También le permite profundizar en el punto de conexión y el código de estado para obtener información que le ayude a resolver estos problemas, y le mantiene informado sobre la disponibilidad del servicio Xbox Live específico para las llamadas del producto.

> [!NOTE]
> Al revisar esta información y solucionar problemas, se recomienda establecer la prioridad de la limitación de velocidad, ya que estos errores suelen tener el mayor impacto en el cliente.


### <a name="apply-filters"></a>Aplicación de filtros

Cerca de la parte superior de la pestaña, puede seleccionar el período de tiempo para el que desea mostrar los datos. La selección predeterminada es **30D** (30 días), pero puede elegir mostrar los datos de **7D** (7 días) o un intervalo de fechas personalizado que especifique (no más de 30 días). En el caso de un intervalo de fechas personalizado, tenga en cuenta que todos los gráficos recortarán el intervalo del gráfico hasta el primer y último día de los datos proporcionados en el intervalo de fechas especificado.

También puede expandir **filtros** para filtrar todos los datos de esta página por la versión del paquete, el tipo de dispositivo o el espacio aislado.
- **Versión del paquete** : el filtro predeterminado es **todas las versiones** , pero puede limitar los datos de estado del servicio a una versión específica del paquete.
- **Tipo de dispositivo** : la configuración predeterminada es **todos los dispositivos** , pero puede limitar los datos de estado del servicio a un tipo de dispositivo específico.
- **Espacio aislado** : el valor predeterminado es **venta directa** , pero puede limitar los datos de estado del servicio a un espacio aislado específico.

La información de todos los gráficos que se enumeran a continuación reflejará el intervalo de fechas y los filtros que haya seleccionado. Algunas secciones también permiten aplicar filtros adicionales.


### <a name="client-errors-by-service"></a>Errores de cliente por servicio

El gráfico **errores de cliente por servicio** muestra el número de errores de cliente (4xx) diarios en cada servicio de Xbox Live durante el período de tiempo seleccionado.

También puede ver solo los errores de limitación de velocidad seleccionando **limitación de velocidad** . Esto muestra el número de errores de límite de velocidad diaria (429) y de limitación de velocidad (429E) en cada servicio de Xbox Live durante el período de tiempo seleccionado.

> [!NOTE]
> Un código de estado 429E se devolvió correctamente como un código de estado 200, pero se habría limitado la velocidad si el servicio experimentase un volumen elevado en el tiempo, por lo que se recomienda tratarlo exactamente igual que si se aplicara (429).

De forma predeterminada, este gráfico muestra los seis primeros servicios por recuento de errores. Puede seleccionar el icono de filtro en la esquina superior derecha de esta sección para elegir distintos servicios. Puede ver los errores de hasta seis servicios a la vez.

> [!NOTE]
> La leyenda solo muestra el prefijo distintivo de cada servicio (por ejemplo, **presencia** en lugar de **Presence.XboxLive.com** ). Puede encontrar la dirección de servicio completa en la tabla **errores de cliente por extremo** en la parte inferior de la pestaña **Estado del servicio Xbox Live** .


### <a name="service-availability"></a>Disponibilidad del servicio

El gráfico de **disponibilidad del servicio** muestra la disponibilidad diaria en cada servicio de Xbox Live durante el período de tiempo seleccionado. Se calcula como *1-(total de errores de servidor (5xx)/total de respuestas)* y es específico de su producto, no de Xbox Live en su totalidad.

De forma predeterminada, este gráfico muestra los seis servicios que han experimentado la disponibilidad más baja. Puede seleccionar el icono de filtro en la esquina superior derecha de esta sección para elegir distintos servicios. Puede ver la disponibilidad de hasta seis servicios a la vez.

> [!NOTE]
> La leyenda solo muestra el prefijo distintivo de cada servicio (por ejemplo, **presencia** en lugar de **Presence.XboxLive.com** ). Puede encontrar la dirección de servicio completa en la tabla **errores de cliente por extremo** en la parte inferior de la pestaña **Estado del servicio Xbox Live** .


### <a name="client-errors-by-endpoint"></a>Errores de cliente por extremo

La tabla **errores de cliente por punto de conexión** muestra el número de errores de cliente (4xx) diarios desglosados por cada servicio, punto de conexión y código de estado de Xbox Live durante el período de tiempo seleccionado. De forma predeterminada, la tabla se ordena por el número total de respuestas de servicio en orden descendente, pero puede cambiar el criterio de ordenación haciendo clic en cualquiera de los encabezados de columna.

También puede ver solo los errores de limitación de velocidad seleccionando **limitación de velocidad** . Esto muestra el número de errores de límite de velocidad diaria (429) y de limitación de velocidad (429E) en cada servicio de Xbox Live, punto de conexión y código de estado durante el período de tiempo seleccionado.

> [!NOTE]
> Un código de estado 429E se devolvió correctamente como un código de estado 200, pero se habría limitado la velocidad si el servicio experimentase un volumen elevado en el tiempo, por lo que se recomienda tratarlo exactamente igual que si se aplicara (429).










 

 
