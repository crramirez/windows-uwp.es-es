---
Description: Para ver los datos de rendimiento de las unidades de anuncios de las aplicaciones, use el informe de rendimiento de publicidad del centro de Partners.
title: Informe de rendimiento de la publicidad
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bb9c6a43b9411b2297cc4cbf35dfdb2177e5dab7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155379"
---
# <a name="advertising-performance-report"></a>Informe de rendimiento de la publicidad


El **Informe de rendimiento de publicidad** del [centro de Partners](https://partner.microsoft.com/dashboard) muestra el rendimiento de las [unidades de anuncios](in-app-ads.md) , incluidos los anuncios de la comunidad. Este informe incluye datos de varios proveedores de AD en aplicaciones UWP que usan la [mediación de anuncios](in-app-ads.md#mediation).

Para ver este informe, expanda **analizar** en el menú de navegación izquierdo y, a continuación, seleccione **rendimiento de ad**. Puede ver estos datos en el centro de Partners o descargar los datos del informe para verlos sin conexión haciendo clic en los iconos de flecha de la página. Como alternativa, puede recuperar estos datos mediante programación con el método [Get ad performance Data](../monetize/get-ad-performance-data.md) en nuestra API de [REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

Al ver los informes de rendimiento de anuncios, tenga en cuenta que los datos de informes de los últimos tres días pueden cambiar a medida que reciben y procesan datos nuevos de diversos orígenes. Además, las reactivaciones de datos pueden producir hasta 90 días en el pasado.

## <a name="apply-filters"></a>Aplicación de filtros

Cerca de la parte superior de la página, puede seleccionar el período de tiempo para el que desea mostrar los datos. La selección predeterminada es 30D (30 días), pero puede optar por mostrar los datos de 3, 6 o 12 meses, o de un intervalo de datos personalizado que especifique.

También puede expandir **filtros** para filtrar todos los datos de esta página por unidad de anuncio, aplicación, proveedor de AD y tipo de dispositivo. Puede elegir entre las siguientes opciones:

* **Agregación**: elija cómo se agregan los datos del informe y cómo se pueden filtrar más. De forma predeterminada, este filtro se establece en **todas las unidades de anuncios**. Opcionalmente, puede cambiar este filtro a **todas las aplicaciones** o a **todos los proveedores de ad**, o bien puede optar por agregarlo por una aplicación específica en la que use anuncios.
* **Proveedores de ad**: filtre el informe por los datos de rendimiento de determinados [proveedores de ad](in-app-ads.md#paid-networks). De forma predeterminada, el informe muestra los datos de todos los proveedores de ad. Esta opción se deshabilitará si selecciona **todos los proveedores de ad** en el menú desplegable **agregación** .
* **Dispositivo**: filtre el informe por los datos de rendimiento de determinados tipos de dispositivos. De forma predeterminada, el informe muestra los datos de todos los tipos de dispositivos.

## <a name="overall-performance"></a>Rendimiento general

En esta sección se muestran las métricas de rendimiento de AD en un gráfico o una vista del mapa del mundo para las unidades de anuncios, las aplicaciones y los proveedores de anuncios seleccionados en los filtros de informe.

Para cambiar a una vista diferente de los datos, haga clic en **gráfico** o en **mapa** en la parte superior de la sección **rendimiento general** . En la vista de mapa, los tonos más oscuros representan los valores más altos y tonos más claros representan valores más bajos. Puedes mantener el mouse sobre un país o región específico en el mapa para analizar el valor de la métrica seleccionada. También puedes acercar cualquier área del mapa para ver datos referentes a países más pequeños.

Para refinar los datos que se muestran en el gráfico o el mapa, haga clic en el icono de filtro junto a la lista desplegable **gráfico** o **mapa** . Este filtro permite elegir entre hasta seis unidades de anuncios, aplicaciones o proveedores de ad diferentes para realizar la comparación en el gráfico o la vista del mapa. El tipo de datos que puede elegir en este filtro depende de lo que haya seleccionado para el filtro de nivel superior de **agregación** en la parte superior del informe.


## <a name="overall-performance-breakdown"></a>Desglose del rendimiento general

En esta sección se muestra una tabla que contiene todas las métricas de rendimiento de ad del conjunto de datos especificado por los filtros que ha seleccionado en el informe.

## <a name="performance-metrics"></a>Métricas de rendimiento

El informe de **rendimiento de publicidad** incluye datos de las siguientes métricas de rendimiento. Algunas métricas solo se muestran para determinados proveedores de ad.

|  Métrica  |  Descripción  |
|----------|---------------|
| Ingresos estimados  |  La cantidad de dinero calculada que ha recibido de los anuncios que se ejecutan en la aplicación. |
| eCPM  |  Costo efectivo por miles de impresiones. |
| Requests  | El número de veces que se envió una solicitud de ad desde la aplicación.  |
| Impresiones  | El número de veces que se mostró un anuncio en la aplicación.  |
| Velocidad de relleno  | El porcentaje de solicitudes de ad enviadas desde la aplicación en la que se mostró un anuncio.  |
| Clics  |  El número de veces que alguien hizo clic en un anuncio en la aplicación. |
| CALEND  |  Tasa de clics, es decir, el número de veces que se hizo clic en un anuncio, dividido por el número de impresiones. |
| Visualización | Porcentaje de impresiones de anuncios que se pueden ver en la aplicación. Para obtener más detalles sobre cómo se calcula este valor, vea [optimizar la capacidad de visualización de las unidades de anuncios](../monetize/optimize-ad-unit-viewability.md). |
| Créditos obtenidos  | Si está ejecutando una campaña de ad de la [comunidad](./about-community-ads.md) , indica el número de créditos que ha obtenido para el espacio de anuncios promocional mostrando anuncios de la comunidad en la aplicación.  |
| Créditos invertidos  | Si está ejecutando una campaña de ad de la [comunidad](./about-community-ads.md) , indica el número de créditos que ha empleado en los anuncios de la aplicación.  |

## <a name="related-topics"></a>Temas relacionados

* [Anuncios en aplicaciones](in-app-ads.md)
* [Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising](../monetize/display-ads-in-your-app.md)
* [Optimizar la visualización de las unidades de anuncios](../monetize/optimize-ad-unit-viewability.md)


 