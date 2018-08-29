---
author: jnHs
Description: The Usage report in the Windows Dev Center dashboard lets you see how customers are using your app.
title: Informe Uso
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.author: wdg-dev-content
ms.date: 06/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, uso, evento personalizado, informe, telemetría, sesiones de usuario
ms.localizationpriority: medium
ms.openlocfilehash: 96d36ebbaa2b7f1a650e2b0f794a1976c1f525a6
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2910513"
---
# <a name="usage-report"></a>Informe Uso


El informe **Uso** del panel de información del Centro de desarrollo de Windows te permite ver cómo los clientes de Windows10 (incluida la Xbox) usan la aplicación y muestra información sobre los eventos personalizados que hayas definido. Puedes ver estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión.


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **30D** (30 días), pero también puedes mostrar los datos durante 3, 6 o 12 meses o durante un intervalo de fechas personalizado que especifiques.

También puedes expandir la opción **Filtros** para filtrarlos datos de esta página por versión de paquete, mercado o tipo de dispositivo.

-   **Versión del paquete**: el valor predeterminado es **Todas**. Si la aplicación incluye más de un paquete, puedes elegir uno concreto aquí.
-   **Mercado**: el filtro predeterminado es **Todos los mercados**, pero puedes limitar los datos a uno o varios mercados.
-   **Tipo de dispositivo**: el valor predeterminado es **Todos**, pero también puedes mostrar los datos de un determinado tipo de dispositivo (PC, consola, tableta, etc.).

La información de todos los gráficos enumerados a continuación reflejará el intervalo de fechas y los filtros que has seleccionado (a excepción de **Nuevos usuarios** en el gráfico **Uso**, que no aparecerá si se selecciona algún filtro). Algunas secciones también te permiten aplicar filtros adicionales.

> [!IMPORTANT]
> Este informe solo incluye los datos de uso de los clientes de Windows 10 (incluida la Xbox) que decidieron no proporcionar información de telemetría. Los datos de uso para juegos de Xbox se incluyen aquí, con independencia de si el cliente ha iniciado sesión o no en Xbox Live. 


## <a name="usage"></a>Uso

El gráfico **Uso** muestra detalles acerca de cómo los clientes están usando tu aplicación durante el período de tiempo seleccionado. Ten en cuenta que este gráfico no realiza un seguimiento de usuarios únicos de tu aplicación ni sesiones de usuario único (es decir, un usuario se representa en este gráfico tanto si usó la aplicación una vez como si lo hizo varias veces).

Este gráfico tiene cuatro pestañas independientes que se pueden ver, mostrando el uso por día o semana (en función de la duración que selecciones).

- **Usuarios**: muestra el número total de **sesiones de usuario** durante el período de tiempo seleccionado. Cada sesión de usuario representa un período de tiempo distinto, a partir de cuando se inicia la aplicación (inicio del proceso) y termina cuando finaliza (final del proceso) o después de un período de inactividad. Por este motivo, un cliente único podría tener varias sesiones de usuario en el mismo día o semana. El número total de **Usuarios activos** (cualquier cliente que use la aplicación ese día o semana) y **Nuevos usuarios** (un cliente que usó la aplicación por primera vez ese día o semana) también se muestran. Ten en cuenta que si has aplicado filtros a la página, no verás **Nuevos usuarios** en este gráfico.
- **Dispositivos**: muestra el número de dispositivos que usan cada día todos los usuarios para interactuar con la aplicación.
- **Duración**: muestra el total de horas de interacción (horas en las que un usuario usa la aplicación de forma activa).
- **Retención**: muestra el número total de **DAU/MAU** (usuarios activos diariamente/usuarios activos mensualmente) durante el período de tiempo seleccionado.

Cuando la **30D** se selecciona el período de tiempo, puedes ver los marcadores de círculo al ver las pestañas **a los usuarios**, **dispositivos**o la **duración** . Estos representan un aumento significativo o disminución un valor determinado que creemos que querrás saber sobre. La fecha en el que se muestra el círculo representa al final de la semana en el que hemos detectado un aumento significativo o una disminución en comparación con la semana anterior a que. Para ver más detalles sobre qué ha cambiado, mantén el puntero encima del círculo.  

> [!TIP]
> Puedes ver más detalles relacionados con los cambios importantes a través de los últimos 30 días en el [informe de información](insights-report.md).


## <a name="user-sessions"></a>Sesiones de usuario

El gráfico **Sesiones de usuario** muestra el número total de sesiones de usuario de la aplicación por mercado durante el período de tiempo seleccionado.

Al igual que con la información de **Sesiones de usuario** del gráfico **Uso** gráfico, una sesión de usuario representa un período de tiempo distinto cuando un usuario interactuó con la aplicación, y este gráfico no realiza un seguimiento de usuarios únicos de la aplicación.

Puedes ver estos datos en forma de **Mapa** visual o cambiar la configuración para verlos en forma de **Tabla**. La tabla mostrará cinco mercados a la vez, ordenados alfabéticamente o por mayor o menor número de sesiones de usuario. También puedes descargar los datos para ver la información de todos los mercados de forma conjunta.


## <a name="package-version"></a>Versión del paquete

El gráfico **Sesiones de usuario** muestra el número total de sesiones de usuario diarias de la aplicación por versión de paquete durante el período de tiempo seleccionado.

Al igual que con el gráfico **Sesiones de usuario**, una sesión de usuario representa un período de tiempo distinto cuando un usuario interactuó con la aplicación, y este gráfico no realiza un seguimiento de usuarios únicos de la aplicación.


## <a name="custom-events"></a>Eventos personalizados

El gráfico **Eventos personalizados** muestra el total de repeticiones de eventos personalizados que has definido para la aplicación. Puede incluir varias repeticiones para el mismo cliente. Puedes usar los filtros para seleccionar los eventos personalizados específicos para los que quieres ver estos datos.

Los eventos personalizados se implementan mediante el método [StoreServicesCustomEventLogger.Log](https://docs.microsoft.com/en-us/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) de [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md).

Para obtener más información, consulta [Registrar eventos personalizados para el Centro de desarrollo](../monetize/log-custom-events-for-dev-center.md).


## <a name="custom-events-breakdown"></a>Desglose de eventos personalizados

El gráfico **Desglose de eventos personalizados** muestra más detalles sobre la frecuencia con la que se producen los eventos personalizados. Esto puede ayudarte a determinar si los eventos se producen con más frecuencia para un mercado, tipo de dispositivo o versiones del paquete determinados.

Para cada evento, verás el nombre del evento y un número de eventos que se corresponden a una combinación específica del mercado del usuario, tipo de dispositivo y versión del paquete. Por lo general, verás un evento que muestra varias veces con distintas combinaciones de estos factores. 




 
