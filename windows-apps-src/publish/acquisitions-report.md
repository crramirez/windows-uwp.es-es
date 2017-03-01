---
author: jnHs
Description: "El informe Adquisiciones del panel del Centro de desarrollo de Windows te permite ver quién ha adquirido la aplicación, junto con datos demográficos y de plataforma."
title: Informe Adquisiciones
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: a668c3d03c11ac4c6c27cddeefafeb3c42caf1e3
ms.lasthandoff: 02/07/2017

---

# <a name="acquisitions-report"></a>Informe Adquisiciones


El informe **Adquisiciones** del panel del Centro de desarrollo de Windows te permite ver quién ha adquirido la aplicación, junto con datos demográficos y de plataforma. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Como alternativa, puedes recuperar mediante programación estos datos mediante el método [obtener los datos de las adquisiciones de la aplicación](../monetize/get-app-acquisitions.md) en la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).

En este informe, una adquisición significa que un cliente nuevo ha obtenido una licencia para tu aplicación (tanto si cobras dinero como si la ofreces gratis).

> **Importante**: El informe **Adquisiciones** no incluye datos sobre reembolsos, devoluciones, anulaciones, etc. Para calcular las ganancias por la aplicación, visita [Resumen de pago](payout-summary.md). En la sección **Reservado**, haz clic en el vínculo **Descargar transacciones reservadas**.



## <a name="apply-filters"></a>Aplicar filtros


Cerca de la parte superior de la página, puedes expandir **Aplicar filtros** para filtrar todos los datos de esta página por intervalo de fechas o por tipo de dispositivo.

-   **Fecha**: El filtro predeterminado es **Últimos 30 días**, pero puedes ampliarlo hasta **Últimos 12 meses**.
-   **Tipo de dispositivo**: La opción predeterminada es **Todos los dispositivos**. Si quieres mostrar los datos de adquisiciones de un determinado tipo de dispositivo únicamente, puede elegir uno específico aquí.

La información de los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros**.

La información de todos los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros**. De manera predeterminada, se incluyen los datos de todos los dispositivos a menos que hayas usado **Aplicar filtros** para elegir solo uno.

## <a name="acquisitions"></a>Adquisiciones


El gráfico **Adquisiciones** muestra el número de adquisiciones diarias o semanales de la aplicación durante el período de tiempo seleccionado. (Si usas **Aplicar filtros** para filtrar los datos durante más tiempo, los datos se agruparán por semana).

También puedes ver el número de adquisiciones del ciclo de vida de la aplicación. Muestra el total acumulado de todas las adquisiciones desde que la aplicación se publicó por primera vez.

También puedes filtrar los resultados por mercado o por la versión del SO.

## <a name="customer-demographic"></a>Grupo demográfico de clientes


El gráfico **Grupo demográfico de clientes** muestra información demográfica sobre las personas que adquirieron la aplicación. Puedes ver cuántas adquisiciones (durante el período de tiempo seleccionado) realizaron las personas de un determinado grupo de edad divididas por sexo.

> **Nota** Algunos clientes han optado por no compartir esta información. Si no podemos determinar el grupo etario o el sexo, la adquisición se clasifica como **Desconocida**.

 

## <a name="markets"></a>Mercados


El gráfico **Mercados** muestra el número total de adquisiciones durante el período de tiempo seleccionado por mercado. De manera predeterminada, se muestra el mercado con más adquisiciones en la parte superior seguido del resto en orden descendente. Puedes invertir el orden alternando la flecha situada en la columna **Adquisiciones** del gráfico.

## <a name="os-version"></a>Versión de SO


El gráfico **Versión de SO** muestra el número total de adquisiciones según el sistema operativo del cliente (o mediante la [adquisición por volumen por parte de organizaciones](organizational-licensing.md)). Es posible que en algunos casos no podamos determinar esta información. En ese caso, la versión del SO se muestra como **Desconocida**.



 

 

