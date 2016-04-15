---
Description: En el informe de adquisiciones de IAP del panel del Centro de desarrollo de Windows puedes ver cuántos IAP has vendido, junto con los detalles demográficos y de plataforma.
title: Informe de adquisiciones de IAP
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
---

# Informe de adquisiciones de IAP


En el informe de **adquisiciones de IAP** del panel del Centro de desarrollo de Windows puedes ver cuántos IAP has vendido, junto con los detalles demográficos y de plataforma. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Como alternativa, puedes recuperar mediante programación estos datos con la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).

En este informe, una adquisición de IAP significa que un cliente te ha comprado un IAP. Varias compras del mismo IAP consumible realizadas por el mismo cliente se contabilizan como adquisiciones de IAP independientes.

> **Importante**: En el informe **Adquisiciones de IAP** no se incluyen datos sobre reembolsos, devoluciones, anulaciones, etc. Para calcular las ganancias por la aplicación, visita [Resumen de pago](payout-summary.md). En la sección **Reservado**, haz clic en el vínculo **Descargar transacciones reservadas**.

## Aplicar filtros


Cerca de la parte superior de la página, puedes expandir **Aplicar filtros** para filtrar todos los datos de esta página por intervalo de fechas o por tipo de dispositivo. También puedes filtrar para mostrar solo los datos de un IAP específico.

-   **Fecha**: el filtro predeterminado es **Últimos 30 días**, pero puedes ampliarlo hasta **Últimos 12 meses**.
-   **IAP**: el filtro predeterminado es **Todos los IAP**. Si quieres mostrar datos de adquisición solamente de uno de los IAP, aquí puedes elegir uno concreto.
-   **Tipo de dispositivo**: la opción predeterminada es **Todos los dispositivos**. Si quieres mostrar los datos de adquisiciones de IAP de un determinado tipo de dispositivo únicamente, puede elegir uno específico aquí.

La información de los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros**.

La información de todos los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros**. De manera predeterminada, se incluyen los datos de todos los dispositivos a menos que hayas usado la opción **Aplicar filtros** para elegir solo uno.

## Adquisiciones de IAP


El gráfico **Adquisiciones de IAP** muestra el número de adquisiciones diarias o semanales de tus IAP durante el período de tiempo seleccionado. (Si usas **Aplicar filtros** para filtrar los datos durante más tiempo, los datos se agruparán por semana).

También puedes ver el número de adquisiciones del ciclo de vida de tus IAP. Muestra el total acumulado de todas las adquisiciones desde que la aplicación se publicó por primera vez.

El gráfico también muestra el precio que un cliente pagó por adquirir el IAP.

También puedes filtrar los resultados por mercado o por la versión del SO.

## IAP principales


El gráfico **IAP principales** muestra el número total de adquisiciones para cada uno de los IAP durante el período de tiempo seleccionado por mercado. De manera predeterminada, se muestra el IAP con más adquisiciones en la parte superior seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Adquisiciones** del gráfico.

## Mercados


El gráfico **Mercados** muestra el número total de adquisiciones de IAP durante el período de tiempo seleccionado por mercado. De manera predeterminada, se muestra el mercado con más adquisiciones en la parte superior seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Adquisiciones** del gráfico.

## Grupo demográfico de clientes


El gráfico **Grupo demográfico de clientes** muestra información demográfica sobre las personas que adquirieron la aplicación. Puedes ver cuántas adquisiciones (durante el período de tiempo seleccionado) realizaron las personas de un determinado grupo de edad divididas por sexo.

> **Nota**: Algunos clientes han optado por no compartir esta información. Si no podemos determinar el grupo de edad o el sexo, la adquisición se clasifica como **Desconocida**.

## Versión de SO


En el gráfico **Versión de SO** se muestra el número total de adquisiciones según el sistema operativo del cliente (o mediante la [adquisición por volumen por parte de organizaciones](organizational-licensing.md)). Es posible que en algunos casos no podamos determinar esta información. En ese caso, la versión del SO se muestra como **Desconocida**.

 

 


<!--HONumber=Mar16_HO1-->


