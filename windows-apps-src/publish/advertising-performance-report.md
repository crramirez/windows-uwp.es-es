---
author: jnHs
Description: To view performance data for the ad units in your apps, use the advertising performance report on the Windows Dev Center dashboard.
title: Informe Rendimiento de la publicidad
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.author: wdg-dev-content
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a1a82c91aeafa253427d8e526b38b8ac304591a2
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "2855709"
---
# <a name="advertising-performance-report"></a>Informe Rendimiento de la publicidad


En el **Informe Rendimiento de la publicidad** se muestra el rendimiento de tus [unidades de anuncio](in-app-ads.md), incluidos los anuncios de la comunidad. En este informe se incluyen datos de varios proveedores de anuncios de aplicaciones para UWP que usan la [mediación de anuncios](in-app-ads.md#mediation).

Para ver este informe, amplía **Analizar** en el menú de navegación izquierdo y luego selecciona **Rendimiento de los anuncios**. Puedes visualizar estos datos en tu panel o descargar los datos del informe para consultarlo sin conexión haciendo clic en los iconos de flecha en la página. También puedes recuperar mediante programación estos datos mediante el método [Obtener los datos de rendimiento de los anuncios](../monetize/get-ad-performance-data.md) en nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

Al visualizar los informes de rendimiento de la publicidad, ten en cuenta que los datos de los informes de los últimos tres días pueden cambiar, ya que recibimos y procesamos nuevos datos procedentes de varios orígenes. Asimismo, pueden producirse modificaciones de datos hasta de los 90 días anteriores.

## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es 30D (30 días), pero también puedes mostrar los datos durante 3, 6 o 12 meses o durante un intervalo de fechas personalizado que especifiques.

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página por unidad de anuncios, aplicación, proveedor de anuncios y tipo de dispositivo. Puedes elegir entre las siguientes opciones:

* **Agregación**: permite elegir cómo se agregan los datos del informe y cómo pueden seguir filtrándose. De manera predeterminada, este filtro está establecido en **Todas las unidades de anuncio**. También puedes cambiar este filtro a **Todas las aplicaciones** o a **Todos los proveedores de anuncios**, o puedes agregar los datos por una determinada aplicación en la que uses anuncios.
* **Proveedores de anuncios**: permite filtrar el informe por datos de rendimiento para algunos [proveedores de anuncios](in-app-ads.md#paid-networks). De manera predeterminada, el informe muestra datos de todos los proveedores de anuncios. Esta opción se desactivará si has elegido **Todos los proveedores de anuncios** en la lista desplegable **Agregación**.
* **Dispositivo**: permite filtrar el informe por los datos de rendimiento de determinados tipos de dispositivos. De manera predeterminada, el informe muestra datos de todos los tipos de dispositivo.

## <a name="overall-performance"></a>Rendimiento general

En esta sección se muestran métricas de rendimiento de anuncios en una vista de gráfico o mapamundi para las unidades de anuncio, aplicaciones y proveedores de anuncios que has seleccionado en los filtros de informe.

Para cambiar a una vista diferente de los datos, haz clic en **Gráfico** o **Mapa** en la parte superior de la sección **Rendimiento general**. En la vista de mapa, los tonos más oscuros representan los valores más altos y los tonos más claros representan los valores más bajos. Puedes mantener el mouse sobre un país o región específico en el mapa para analizar el valor de la métrica seleccionada. También puedes acercar cualquier área del mapa para ver datos referentes a países más pequeños.

Puedes restringir los datos mostrados en el gráfico o mapa haciendo clic en el icono de filtro situado junto al menú desplegable **Gráfico** o **Mapa**. Este filtro te permite elegir hasta seis unidades de anuncio, aplicaciones o proveedores de anuncios diferentes para compararlos en la vista de gráfico o mapa. El tipo de datos que puedes elegir en este filtro depende de lo que hayas seleccionado para el filtro de nivel superior de **Agregación** en la parte superior del informe.


## <a name="overall-performance-breakdown"></a>Desglose del rendimiento general

En esta sección se muestra una tabla que incluye todas las métricas de rendimiento de anuncios para el conjunto de datos especificado mediante los filtros que has seleccionado en el informe.

## <a name="performance-metrics"></a>Métricas de rendimiento

El informe **Rendimiento de publicidad** incluye los datos de las siguientes métricas de rendimiento. Algunas métricas se muestran solo para determinados proveedores de anuncios.

|  Métrica  |  Descripción  |
|----------|---------------|
| Ingresos estimados  |  La cantidad estimada de dinero que has recibido de los anuncios que se ejecutan en la aplicación. |
| eCPM  |  Rentabilidad por mil impresiones. |
| Solicitudes  | El número de veces que se ha enviado una solicitud de anuncio desde la aplicación.  |
| Impresiones  | El número de veces que se ha mostrado un anuncio en la aplicación.  |
| Velocidad de relleno  | El porcentaje de solicitudes de anuncio enviadas desde la aplicación en las que se ha mostrado un anuncio.  |
| Clics  |  El número de veces que alguien ha hecho clic en un anuncio en la aplicación. |
| CTR  |  La tasa de clics, es decir, el número de veces que se ha hecho clic en un anuncio dividido por el número de impresiones. |
| Capacidad de visualización | El porcentaje de impresiones de anuncios que se pueden ver en su aplicación. Para obtener más información acerca de cómo se calcula este valor, consulte [optimizar la capacidad de visualización de las unidades de ad](../monetize/optimize-ad-unit-viewability.md). |
| Créditos ganados  | Si estás realizando una campaña de [anuncios de la Comunidad](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), esto indica el número de créditos que has ganado por el espacio de anuncios promocionales al mostrar anuncios de la comunidad en tu aplicación.  |
| Créditos gastados  | Si estás realizando una campaña de [anuncios de la Comunidad](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), esto indica el número de créditos que has gastado en los anuncios para tu aplicación.  |

## <a name="related-topics"></a>Temas relacionados

* [Anuncios desde la aplicación](in-app-ads.md)
* [Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising](../monetize/display-ads-in-your-app.md)
* [Optimizar la visualización de tus unidades de anuncios](../monetize/optimize-ad-unit-viewability.md)


 
