---
author: jnHs
Description: "En el informe Adquisiciones de complementos del panel del Centro de desarrollo de Windows puedes ver cuántos complementos has vendido, junto con los detalles demográficos y de plataforma."
title: Informe Adquisiciones de complementos
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 189b15728fd5013a1f976803ffb0e55f57e95142
ms.lasthandoff: 02/07/2017

---

# <a name="add-on-acquisitions-report"></a>Informe Adquisiciones de complementos


En el informe **Adquisiciones de complementos** del panel del Centro de desarrollo de Windows puedes ver cuántos complementos has vendido, junto con los detalles demográficos y de plataforma. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Como alternativa, puedes recuperar mediante programación estos datos mediante el método [obtener los datos de las adquisiciones de complementos](../monetize/get-in-app-acquisitions.md) en la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).

En este informe, la adquisición de un complemento significa que un cliente te ha comprado un complemento. Varias compras del mismo complemento consumible realizadas por el mismo cliente se contabilizan como adquisiciones de complementos independientes.

> **Importante**: El informe de **adquisiciones de complementos** no incluye datos sobre reembolsos, devoluciones, anulaciones, etc. Para calcular las ganancias de la aplicación, visita [Resumen de pago](payout-summary.md). En la sección **Reservado**, haz clic en el vínculo **Descargar transacciones reservadas**.

## <a name="apply-filters"></a>Aplicar filtros


Cerca de la parte superior de la página, puedes expandir **Aplicar filtros** para filtrar todos los datos de esta página por un intervalo de fechas o por un tipo de dispositivo. También puedes filtrar para mostrar solo los datos de un complemento específico.

-   **Fecha**: El filtro predeterminado es **Últimos 30 días**, pero puedes ampliarlo hasta **Últimos 12 meses**.
-   **Complemento**: El filtro predeterminado es **Todos los complementos**. Si quieres mostrar datos de adquisición solamente de uno de los complementos, aquí puedes elegir uno concreto.
-   **Tipo de dispositivo**: La opción predeterminada es **Todos los dispositivos**. Si quieres mostrar los datos de adquisiciones de complementos de un determinado tipo de dispositivo únicamente, aquí puede elegir uno específico.

La información de los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros**.

La información de todos los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros**. De manera predeterminada, se incluyen los datos de todos los dispositivos a menos que hayas usado **Aplicar filtros** para elegir solo uno.

## <a name="add-on-acquisitions"></a>Adquisiciones de complementos


El gráfico **Adquisiciones de complementos** muestra el número de adquisiciones diarias o semanales de tus complementos durante el período de tiempo seleccionado. (Si usas **Aplicar filtros** para filtrar los datos durante más tiempo, los datos se agruparán por semanas).

También puedes ver el número de adquisiciones durante ciclo de vida respecto a tus complementos. Muestra el total acumulado de todas las adquisiciones desde que la aplicación se publicó por primera vez.

El gráfico también muestra el precio que un cliente pagó por adquirir el complemento.

También puedes filtrar los resultados por mercado o por la versión del SO.

## <a name="top-add-ons"></a>Complementos principales

El gráfico **Complementos principales** muestra el número total de adquisiciones de cada uno de los complementos durante el período de tiempo seleccionado por mercado. De manera predeterminada, se muestra el complemento con más adquisiciones en la parte superior seguido del resto en orden descendente. Si quieres invertir el orden, alterna la flecha situada en la columna **Adquisiciones** del gráfico.

## <a name="markets"></a>Mercados

El gráfico **Mercados** muestra el número total de adquisiciones de complementos durante el período de tiempo seleccionado por mercado. De manera predeterminada, se muestra el mercado con más adquisiciones en la parte superior seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Adquisiciones** del gráfico.

## <a name="customer-demographic"></a>Grupo demográfico de clientes

El gráfico **Grupo demográfico de clientes** muestra información demográfica sobre las personas que han adquirido el complemento. Puedes ver cuántas adquisiciones (durante el período de tiempo seleccionado) han realizado las personas de un determinado grupo de edad divididas por sexo.

> **Nota** Algunos clientes han optado por no compartir esta información. Si no podemos determinar el grupo de edad o el sexo, la adquisición se clasifica como **Desconocida**.

## <a name="os-version"></a>Versión de SO

En el gráfico **Versión de SO** se muestra el número total de adquisiciones según el sistema operativo del cliente (o mediante la [adquisición por volumen por parte de organizaciones](organizational-licensing.md)). Es posible que en algunos casos no podamos determinar esta información. En ese caso, la versión del SO se muestra como **Desconocida**.

 

 

