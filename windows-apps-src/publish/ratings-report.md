---
author: jnHs
Description: "El informe Clasificaciones del panel del Centro de desarrollo de Windows te permite ver la distribución de cómo los clientes clasifican la aplicación en la Tienda Windows."
title: Informe Clasificaciones
ms.assetid: CAFEC20B-04FB-48C8-B663-1238C0B85ECD
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: e0258bc9402772d0a036b32563348d11acd0fdb7

---

# Informe Clasificaciones


El informe **Clasificaciones** del panel del Centro de desarrollo de Windows te permite ver la distribución de cómo los clientes clasifican la aplicación en la Tienda Windows. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Como alternativa, puedes recuperar mediante programación estos datos con la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).

En este informe, una clasificación significa el número de estrellas (de 1 a 5) que un cliente proporcionó a la aplicación al clasificarla en la Tienda. En el informe **Clasificaciones** no se incluye información sobre los comentarios individuales que se dejan como opiniones; estos están disponibles en el [informe de opiniones](reviews-report.md).

## Aplicar filtros


Cerca de la parte superior de la página, puedes expandir **Aplicar filtros** para filtrar todos los datos de esta página por intervalo de fechas o por mercado.

-   
            **Fecha**: el filtro predeterminado es **Últimos 30 días**, pero puedes ampliarlo hasta **Últimos 12 meses**.
-   
            **Mercado**: el filtro predeterminado es **Todos los mercados**. Puedes elegir un mercado específico si quieres que esta página solo muestre clasificaciones de clientes de ese mercado.
-   
            **Tipo de dispositivo**: el filtro predeterminado es **Todos los dispositivos**. Puedes elegir un tipo de dispositivo específico si quieres que esta página solo muestre clasificaciones aportadas por clientes que usen ese tipo de dispositivo.

La información contenida en todos los gráficos enumerados a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros** y todos los filtros que hayas elegido aquí.

## Clasificación promedio


El gráfico **Clasificación promedio** muestra la clasificación promedio de la aplicación durante el período de tiempo seleccionado.

## Número de clasificaciones


El gráfico **número de clasificaciones** muestra el número total de clasificaciones de la aplicación durante el período de tiempo seleccionado.

## Clasificaciones nuevas y revisadas


El gráfico **Clasificaciones nuevas y revisadas** muestra el número de clasificaciones para cada tipo de clasificación (nueva o revisada) durante el período de tiempo seleccionado.

-   
            **Clasificaciones nuevas** son clasificaciones que los clientes han enviado y que no han cambiado.
-   
            **Clasificaciones revisadas** son clasificaciones que el cliente ha cambiado.

>
            **Nota** Una clasificación aparecerá aquí como revisada incluso si el cliente solo cambió o agregó el texto o el título de la crítica y dejó igual la clasificación.

## Clasificación promedio con el tiempo


El gráfico **Clasificación promedio con el tiempo** muestra cómo ha cambiado la clasificación promedio de la aplicación durante el período de tiempo seleccionado.

En lugar de calcular el promedio de todas las valoraciones realizadas durante el período de tiempo seleccionado (como en el gráfico **Clasificación media**), el gráfico de **clasificación media en el tiempo** muestra cómo clasificaron la aplicación los clientes en un día o semana determinados en una semana durante el período. Esto resulta útil para identificar tendencias o determinar si las clasificaciones se vieron afectadas por actualizaciones u otros factores.

Si has filtrado la información por **Últimos 30 días** o **Últimos 3 meses**, el gráfico muestra tu clasificación media por día. Si has filtrado por **Últimos 6 meses** o **Últimos 12 meses**, el gráfico muestra tu clasificación media por semana (en ese caso, se considera que empieza una nueva semana el lunes y la clasificación promedio que se muestra es de la semana anterior).

## Mercados


El gráfico **Mercados** muestra la clasificación promedio y el número de clasificaciones durante el período de tiempo seleccionado por mercado.

> 
            **Nota** Si usaste **Filtros de página** para especificar un mercado concreto, no verás este gráfico en el informe **Clasificaciones**. Para ver el gráfico, cambia **Filtros de página** para mostrar todos los mercados.

De manera predeterminada, mostramos el mercado que tenía más críticas y se sigue en orden descendente, pero puede invertir el orden alternando la flecha situada en la columna **Número de clasificaciones** del gráfico. También puedes ordenar los datos por **Clasificación promedio** o **Mercado** haciendo clic en las columnas.

> 
            **Nota** Es probable que veas un número diferente de clasificaciones al comparar el informe **Clasificaciones** del Centro de desarrollo de Windows con el informe Críticas de la aplicación móvil más antigua del Centro de desarrollo. Esto se debe a que la aplicación solo muestra los datos de las opiniones que dejaron los clientes en Windows Phone 8.1 y versiones anteriores. También puede ser que Microsoft haya eliminado opiniones de la Tienda Windows que se identificaron como spam, inadecuadas, ofensivas o que infringían las directivas de otra forma. Esperamos que esta acción dé como resultado una mejor experiencia del cliente.

 

 



<!--HONumber=Jun16_HO4-->


