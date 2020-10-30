---
description: El informe de uso del centro de Partners le permite ver cómo los clientes usan la aplicación.
title: Informe de uso
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, uso, eventos personalizados, informes, telemetría, sesiones de usuario
ms.localizationpriority: medium
ms.openlocfilehash: 7eafc102ca1720aaa14c697fcbff3825436343f9
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034948"
---
# <a name="usage-report"></a>Informe de uso


El informe de **uso** del [centro de Partners](https://partner.microsoft.com/dashboard) le permite ver cómo los clientes de Windows 10 (incluido Xbox) usan la aplicación y muestra información acerca de los eventos personalizados que ha definido. Puede ver estos datos en el centro de Partners o [descargar el informe](download-analytic-reports.md) para verlo sin conexión.


## <a name="apply-filters"></a>Aplicación de filtros

Cerca de la parte superior de la página, puede seleccionar el período de tiempo para el que desea mostrar los datos. La selección predeterminada es **30D** (30 días), pero puede optar por mostrar los datos de 3, 6 o 12 meses, o de un intervalo de datos personalizado que especifique.

También puede expandir **filtros** para filtrar los datos de esta página por la versión del paquete, el mercado o el tipo de dispositivo.

-   **Versión del paquete** : el valor predeterminado es **Todas** . Si la aplicación incluye más de un paquete, puedes elegir uno concreto aquí.
-   **Market** : el filtro predeterminado es **todos los mercados** , pero puede limitar los datos a uno o varios mercados.
-   **Tipo de dispositivo** : la configuración predeterminada es **todo** , pero puede elegir Mostrar datos solo para un tipo de dispositivo específico (PC, consola, tableta, etc.).

La información de todos los gráficos que se enumeran a continuación reflejará el intervalo de fechas y los filtros que haya seleccionado (con la excepción de **los nuevos usuarios** en el gráfico de **uso** , que no aparecerán si se seleccionan filtros). Algunas secciones también permiten aplicar filtros adicionales.

> [!IMPORTANT]
> Este informe solo incluye los datos de uso de los clientes de Windows 10 (incluido Xbox) que no han optado por proporcionar información de telemetría. Los datos de uso de los juegos de Xbox se incluyen aquí independientemente de si el cliente se ha iniciado sesión en Xbox Live. 


## <a name="usage"></a>Uso

El gráfico de **uso** muestra detalles sobre cómo los clientes usan la aplicación durante el período de tiempo seleccionado. Tenga en cuenta que este gráfico no realiza un seguimiento de los usuarios únicos de la aplicación o de las sesiones de usuario único (es decir, un usuario se representa en este gráfico, independientemente de que use la aplicación una sola vez o varias veces).

Este gráfico tiene pestañas independientes que puede ver, mostrando el uso por día o semana (en función de la duración seleccionada).

- **Usuarios** : muestra el número total de **sesiones de usuario** durante el período de tiempo seleccionado. Cada sesión de usuario representa un período de tiempo distinto, que comienza cuando se inicia la aplicación (inicio de proceso) y finaliza cuando termina (fin de proceso) o después de un período de inactividad. Por este motivo, un solo cliente puede tener varias sesiones de usuario durante el mismo día o semana. También se muestra el número total de **usuarios activos** (cualquier cliente que use la aplicación ese día o semana) y **nuevos usuarios** (un cliente que usó la aplicación por primera vez ese día o semana). Tenga en cuenta que si ha aplicado filtros a la página, no verá **los nuevos usuarios** en este gráfico.
- **Dispositivos** : muestra el número de dispositivos diarios usados para interactuar con la aplicación por todos los usuarios.
- **Duración** : muestra las horas de interacción totales (horas en las que un usuario usa activamente la aplicación).
- **Engagement** : muestra el promedio de minutos de interacción por usuario (promedio de duración de todas las sesiones de usuario). 
- **Retención** : muestra el número total de **Dau/Mau** (usuarios activos diarios/usuarios activos mensuales) durante el período de tiempo seleccionado.

Cuando se selecciona el período de tiempo de **30D** , es posible que vea marcadores de círculo al ver los **usuarios** , los **dispositivos** o las pestañas de **duración** . Representan un aumento o una disminución significativos en un valor determinado que creemos que querrá conocer. La fecha en la que aparece el círculo representa el final de la semana en el que se ha detectado un aumento o una disminución significativos en comparación con la semana anterior. Para ver más detalles sobre lo que ha cambiado, mantenga el mouse sobre el círculo.  

> [!TIP]
> Puede ver más información relacionada con los cambios significativos en los últimos 30 días del [Informe Insights](insights-report.md).


## <a name="user-sessions"></a>Sesiones de usuario

El gráfico de **sesiones de usuario** muestra el número total de sesiones de usuario para la aplicación por mercado, durante el período de tiempo seleccionado.

Al igual que con la información de las **sesiones de usuario** en el gráfico de **uso** , una sesión de usuario representa un período de tiempo distinto cuando un cliente interactúa con la aplicación y este gráfico no realiza un seguimiento de usuarios únicos para la aplicación.

Puede ver estos datos en un formulario de **mapa** visual o alternar la configuración para verlo en un formulario de **tabla** . El formulario de tabla mostrará cinco mercados a la vez, ordenados alfabéticamente o por el número más alto o más bajo de sesiones de usuario. También puede descargar los datos para ver la información de todos los mercados juntos.


## <a name="package-version"></a>Versión del paquete

El gráfico de **sesiones de usuario** muestra el número total de sesiones de usuario diarias de la aplicación por versión del paquete durante el período de tiempo seleccionado.

Al igual que con el gráfico de **sesiones de usuario** , una sesión de usuario representa un período de tiempo distinto cuando un cliente interactúa con la aplicación y este gráfico no realiza un seguimiento de usuarios únicos para la aplicación.


## <a name="custom-events"></a>Eventos personalizados

El gráfico de **eventos personalizados** muestra el número total de repeticiones de los eventos personalizados que ha definido para la aplicación. Puede incluir varias repeticiones para el mismo cliente. Puede usar los filtros para seleccionar los eventos personalizados específicos para los que desea ver estos datos.

Los eventos personalizados se implementan mediante el método [StoreServicesCustomEventLogger.Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) de [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md).

Para obtener más información, consulte [registro de eventos personalizados para el centro de desarrollo](../monetize/log-custom-events-for-dev-center.md).


## <a name="custom-events-breakdown"></a>Desglose de eventos personalizados

En el gráfico de **desglose de eventos personalizados** se muestran más detalles sobre la frecuencia con que se produjeron los eventos personalizados. Esto puede ayudarle a determinar si los eventos se producen con más frecuencia para un mercado determinado, un tipo de dispositivo o versiones de paquete.

Para cada evento, verá el nombre del evento y un recuento de eventos que corresponden a una combinación específica del mercado del usuario, el tipo de dispositivo y la versión del paquete. Normalmente, verá un evento en la lista varias veces junto con diferentes combinaciones de estos factores. 




 
