---
description: El informe de clasificaciones del centro de Partners le permite ver cómo los clientes clasificaron la aplicación en la tienda.
title: Informe Valoración
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, clasificación, tasa, estrella, estrellas, valorado
ms.localizationpriority: medium
ms.openlocfilehash: 4d1d0f9ceaa660245c537c4caf1f1770f1c5be6d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030348"
---
# <a name="ratings-report"></a>Informe Valoración


El informe de **clasificaciones** del [centro de Partners](https://partner.microsoft.com/dashboard) le permite ver cómo los clientes clasificaron la aplicación en la tienda. 

Puede ver estos datos en el centro de Partners o [descargar el informe](download-analytic-reports.md) para verlo sin conexión. Como alternativa, puede recuperar estos datos mediante programación con el método [Get App Reviews](../monetize/get-app-reviews.md) en la [API de REST de Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

> [!TIP]
> Para ver una visión rápida de las revisiones, las clasificaciones y los comentarios de los usuarios de todas las aplicaciones en los últimos 30 días, expanda **participar** en el menú de navegación izquierdo y seleccione **revisiones y comentarios.** 

## <a name="apply-filters"></a>Aplicación de filtros

Cerca de la parte superior de la página, puede seleccionar el período de tiempo para el que desea mostrar las revisiones. La selección predeterminada es **vigencia** , pero puede elegir que se muestren las revisiones durante 30 días, 3 meses, 6 meses o 12 meses, o para un intervalo de datos personalizado que especifique. Tenga en cuenta que el desglose de las **clasificaciones** y la **clasificación media** de los gráficos de tiempo siempre mostrarán los datos de los últimos 12 meses; Estas opciones de período de tiempo no afectarán a esos gráficos.

Puede expandir los **filtros** para filtrar las revisiones que se muestran en esta página por las siguientes opciones. Estos filtros no se aplicarán a las **clasificaciones de desglose** y promedio de clasificación en los gráficos de **tiempo** .

-   **Mercado** : el valor predeterminado es **Todos los mercados** . Puedes elegir un mercado específico si quieres que esta página solo muestre clasificaciones de clientes de ese mercado.
-   **Tipo de dispositivo** : el filtro predeterminado es **Todos los dispositivos** . Puedes elegir un tipo de dispositivo específico si quieres que esta página solo muestre clasificaciones aportadas por clientes que usen ese tipo de dispositivo.
-   **Versión del sistema operativo** : el valor predeterminado es **todos** . Puede elegir una versión específica del sistema operativo Si desea que esta página solo muestre las clasificaciones que los clientes han dejado en esa versión del sistema operativo.
-   **Nombre de categoría** : el filtro predeterminado es **todos** . Puede elegir mostrar solo las clasificaciones asociadas a las revisiones que hemos identificado como pertenecientes a una [categoría de información de revisión](reviews-report.md#insight-categories) específica para mostrar solo las revisiones que hemos asociado a esa categoría. 

> [!TIP]
> Si no ve ninguna clasificación en la página, asegúrese de que los filtros no hayan excluido todas las clasificaciones. Por ejemplo, si filtra por un sistema operativo de destino que no es compatible con la aplicación, no verá ninguna clasificación.


## <a name="rating-breakdown"></a>Desglose de clasificación

En el gráfico de **desglose de clasificación** se muestra: 
- Puntuación media de la estrella de clasificación de la aplicación.
- El número total de clasificaciones de la aplicación en los últimos 12 meses.
- El número total de clasificaciones para cada clasificación por estrellas.
- El número de clasificaciones para cada tipo de clasificación (nueva o revisada) por clasificación por estrellas en los últimos 12 meses.
 - Las **clasificaciones nuevas** son clasificaciones que los clientes han enviado pero no han cambiado en absoluto.
 - Las **clasificaciones revisadas** son clasificaciones modificadas por el cliente de cualquier manera, incluso cambiando el texto de la revisión.

> [!TIP]
> La clasificación promedio que un cliente ve en el almacén tiene en cuenta el mercado y el tipo de dispositivo del cliente, por lo que puede diferir de lo que se ve en este informe.


## <a name="average-rating"></a>Clasificación promedio

El gráfico de **clasificación promedio** muestra cómo ha cambiado la clasificación promedio de la aplicación en los últimos 12 meses.

En lugar de calcular el promedio de todas las clasificaciones que quedan durante los últimos 12 meses (como en el gráfico de **desglose de clasificaciones** ), el gráfico de **clasificación promedio** muestra cómo los clientes clasificaron la aplicación en una semana determinada. Esto puede ayudarle a identificar tendencias o determinar si las clasificaciones se vieron afectadas por actualizaciones u otros factores.

## <a name="rating-by-market"></a>Clasificación por mercado

El gráfico **clasificación por mercado** muestra un desglose de las clasificaciones medias en cada mercado durante el período de tiempo seleccionado. De forma predeterminada, se muestran estos datos en un formulario de **mapa** visual, pero también se puede alternar el control en la esquina superior derecha para verlo como una **tabla** .

En la vista de **tabla** se muestran cinco mercados a la vez, ordenados alfabéticamente o por valoración media mayor o menor. También puede descargar los datos de este gráfico para ver la información de todos los mercados juntos.

También puede filtrar este gráfico por **clasificación** . De forma predeterminada, se comprueban las revisiones con todas las clasificaciones por estrellas, pero puede activar y desactivar las clasificaciones específicas (de 1 a 5 estrellas) si solo desea ver las revisiones asociadas a las clasificaciones por estrellas determinadas.
