---
Description: El informe de uso en el centro de partners le permite ver cómo los clientes usan la aplicación.
title: Informe de uso
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, uso, evento personalizado, informe, telemetría, sesiones de usuario
ms.localizationpriority: medium
ms.openlocfilehash: 0d0be1399ebc00ffda57ecf27a72be994fa994ce
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610910"
---
# <a name="usage-report"></a>Informe de uso


El **uso** notificar en [centro de partners](https://partner.microsoft.com/dashboard) le permite ver cómo los clientes en Windows 10 (incluido Xbox) usan la aplicación y muestra información sobre los eventos personalizados que haya definido. Puede ver estos datos en el centro de partners, o [descargar el informe](download-analytic-reports.md) ver sin conexión.


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **30D** (30 días), pero también puedes mostrar los datos durante 3, 6 o 12 meses o durante un intervalo de fechas personalizado que especifiques.

También puedes expandir la opción **Filtros** para filtrarlos datos de esta página por versión de paquete, mercado o tipo de dispositivo.

-   **Versión del paquete**: El valor predeterminado es **todas**. Si la aplicación incluye más de un paquete, puedes elegir uno concreto aquí.
-   **Mercado**: El filtro predeterminado es **todos los mercados**, pero puede limitar los datos a uno o varios de los mercados.
-   **Tipo de dispositivo**: El valor predeterminado es **todas**, pero puede optar por mostrar los datos de un único tipo de dispositivo específico (PC, consola, tableta, etcetera.).

La información de todos los gráficos enumerados a continuación reflejará el intervalo de fechas y los filtros que has seleccionado (a excepción de **Nuevos usuarios** en el gráfico **Uso**, que no aparecerá si se selecciona algún filtro). Algunas secciones también te permiten aplicar filtros adicionales.

> [!IMPORTANT]
> Este informe solo incluye los datos de uso de los clientes de Windows 10 (incluida la Xbox) que decidieron no proporcionar información de telemetría. Los datos de uso para juegos de Xbox se incluyen aquí, con independencia de si el cliente ha iniciado sesión o no en Xbox Live. 


## <a name="usage"></a>Uso

El gráfico **Uso** muestra detalles acerca de cómo los clientes están usando tu aplicación durante el período de tiempo seleccionado. Ten en cuenta que este gráfico no realiza un seguimiento de usuarios únicos de tu aplicación ni sesiones de usuario único (es decir, un usuario se representa en este gráfico tanto si usó la aplicación una vez como si lo hizo varias veces).

Este gráfico tiene pestañas independientes que se pueden ver, que muestra el uso por día o semana (según la duración que ha seleccionado).

- **Usuarios**: Muestra el número total de **las sesiones de usuario** durante el período de tiempo seleccionado. Cada sesión de usuario representa un período de tiempo distinto, a partir de cuando se inicia la aplicación (inicio del proceso) y termina cuando finaliza (final del proceso) o después de un período de inactividad. Por este motivo, un cliente único podría tener varias sesiones de usuario en el mismo día o semana. El número total de **Usuarios activos** (cualquier cliente que use la aplicación ese día o semana) y **Nuevos usuarios** (un cliente que usó la aplicación por primera vez ese día o semana) también se muestran. Ten en cuenta que si has aplicado filtros a la página, no verás **Nuevos usuarios** en este gráfico.
- **Dispositivos**: Muestra el número de dispositivos diarios que se usa para interactuar con la aplicación todos los usuarios.
- **Duración**: Muestra las horas de compromiso total (horas que un usuario esté usando activamente la aplicación).
- **Engagement**: Muestra los minutos de engagement Media por usuario (duración media de todas las sesiones de usuario). 
- **Retención**: Muestra el número total de **MAU/dau Acumulados** (usuarios activos diariamente usuarios/mensuales activos) en el período de tiempo seleccionado.
- **Predicción de abandono**: Muestra cuántos usuarios se predecir es probable que detener mediante la aplicación lo antes posible, en función de su uso reciente.

Cuando el **d. 30** está seleccionado el período de tiempo, puede ver los marcadores de círculo al ver el **usuarios**, **dispositivos**, o **duración** pestañas. Estos representan un aumento significativo o disminuyen en un valor determinado que creemos que desea conocer. La fecha en la que aparece el círculo representa el final de la semana en el que se ha detectado un significativo aumento o disminución en comparación con la semana antes de que. Para ver más detalles sobre lo que ha cambiado, mantenga el mouse sobre el círculo.  

> [!TIP]
> Puede ver más información relacionada con cambios significativos durante los últimos 30 días en el [informe Insights](insights-report.md).


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




 
