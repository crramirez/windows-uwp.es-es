---
Description: El informe de las clasificaciones en el centro de partners le permite ver cómo los clientes con la clasificación de la aplicación en el Store.
title: Informe Valoración
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, clasificación, estrellas de estrella, velocidad, clasificados
ms.localizationpriority: medium
ms.openlocfilehash: 9b507212cd660a025bc3d858fc2938cf59d4219f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640060"
---
# <a name="ratings-report"></a>Informe Valoración


El **clasificaciones** notificar en [centro de partners](https://partner.microsoft.com/dashboard) le permite ver cómo los clientes con la clasificación de la aplicación en el Store. 

Puede ver estos datos en el centro de partners, o [descargar el informe](download-analytic-reports.md) ver sin conexión. Como alternativa, puede recuperar mediante programación estos datos mediante el [obtener las revisiones de la aplicación](../monetize/get-app-reviews.md) método en el [analytics API de REST de Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).

> [!TIP]
> Para obtener una vista rápida de las valoraciones, clasificaciones y comentarios de los usuarios de todas las aplicaciones en los últimos 30 días, expande **Interactuar** en el menú de navegación izquierdo y selecciona **Valoraciones y comentarios**. 

## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar las valoraciones. La selección predeterminada es **Duración**, pero también puedes mostrar las valoraciones durante 30 días, 3 meses, 6 meses o 12 meses o durante un intervalo de fechas personalizado que especifiques. Ten en cuenta que los gráficos **Desglose de clasificaciones** y **Clasificación promedio con el tiempo** siempre mostrarán datos de los últimos 12 meses; estas opciones de período de tiempo no afectarán a dichos gráficos.

Puedes expandir la opción **Filtros** para filtrar las valoraciones mostradas en esta página mediante las siguientes opciones. Estos filtros no se aplicarán a los gráficos **Desglose de clasificaciones** y **Clasificación promedio con el tiempo**.

-   **Mercado**: El valor predeterminado es **todos los mercados**. Puedes elegir un mercado específico si quieres que esta página solo muestre clasificaciones de clientes de ese mercado.
-   **Tipo de dispositivo**: El filtro predeterminado es **todos los dispositivos**. Puedes elegir un tipo de dispositivo específico si quieres que esta página solo muestre clasificaciones aportadas por clientes que usen ese tipo de dispositivo.
-   **Versión del sistema operativo**: El valor predeterminado es **todas**. Puede elegir una versión específica del sistema operativo si desea que esta página para mostrar únicamente las clasificaciones izquierda por los clientes en esa versión del sistema operativo.
-   **Nombre de categoría**: El filtro predeterminado es **todas**. Puede elegir mostrar únicamente las clasificaciones asociadas con las revisiones que hemos identificado como pertenecientes a un determinado [revise la categoría de insight](reviews-report.md#insight-categories) para mostrar solo las revisiones que nos hemos asociado con esa categoría. 

> [!TIP]
> Si no ve la clasificación en la página, compruebe para asegurarse de que los filtros aún no lo ha excluido todas de la clasificación. Por ejemplo, si se filtra por un sistema operativo de destino que no es compatible con la aplicación, no verá ninguna clasificación.


## <a name="rating-breakdown"></a>Desglose de clasificación

El **clasificación desglose** del gráfico se muestra: 
- La clasificación promedio por estrellas de la aplicación.
- El número total de clasificaciones de la aplicación en los últimos 12 meses.
- El número total de clasificaciones para cada clasificación por estrellas.
- El número de clasificaciones para cada tipo de clasificación (nueva o revisada) por clasificación por estrellas en los últimos 12 meses.
 - **Clasificaciones nuevas** son clasificaciones que los clientes han enviado, pero que no han cambiado.
 - **Clasificaciones revisadas** son clasificaciones que el cliente ha cambiado de cualquier forma, incluso cambiar el texto de la valoración.

> [!TIP]
> La clasificación promedio que un cliente ve en la Tienda tiene en cuenta el mercado y el tipo de dispositivo del cliente, de modo que puede ser diferente de lo que aparece en este informe.


## <a name="average-rating"></a>Clasificación promedio

El **clasificación Media** gráfico muestra cómo ha cambiado la valoración media de la aplicación a través de los últimos 12 meses.

En lugar de calcular la media de todas las clasificaciones que queden durante los últimos 12 meses (como en el **desglose de valoraciones** gráfico), el **clasificación Media** gráfico muestra cómo los clientes de clasificación de la aplicación en una semana determinada. Esto puede ayudarte a identificar tendencias o determinar si las clasificaciones se vieron afectadas por actualizaciones u otros factores.

## <a name="rating-by-market"></a>Clasificación por mercado

El **calificación por mercado** gráfico muestra un desglose de las clasificaciones de promedio de cada mercado durante el período de tiempo seleccionado. De forma predeterminada, se muestran estos datos en un objeto visual **mapa** formulario, sino también se puede activar el control en la esquina superior derecha para verlo como un **tabla**.

El **tabla** vista muestra los mercados de cinco a la vez, ordenados alfabéticamente o por clasificación media mayor o menor. También puede descargar los datos de este gráfico para ver información de todos los mercados de forma conjunta.

También puede filtrar el gráfico por **clasificación**. De forma predeterminada se comprueban las revisiones de todas las clasificaciones por estrellas, pero se puede activar y desactivar clasificaciones específicas (de 1 a 5 estrellas) si desea ver solo las revisiones asociadas a determinada clasificación por estrellas.