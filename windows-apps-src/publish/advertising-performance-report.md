---
author: jnHs
Description: "Para ver los datos de rendimiento de las unidades de anuncio en tus aplicaciones, usa los informes de rendimiento de la publicidad de nivel de aplicación y de nivel de cuenta en el panel del Centro de desarrollo de Windows."
title: Informe Rendimiento de la publicidad
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
ms.author: wdg-dev-content
ms.date: 07/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: c46533c6762ddd2f47a4dbd40c253bc3d8f346d7
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
# <a name="advertising-performance-report"></a>Informe Rendimiento de la publicidad


En el **Informe Rendimiento de la publicidad** se muestra el rendimiento de tus [unidades de anuncio](monetize-with-ads.md#available-ad-units), incluidos los anuncios de la comunidad y los anuncios de filial. En este informe se incluyen datos de varios proveedores de anuncios de aplicaciones para UWP que usan la [mediación de anuncios](monetize-with-ads.md#mediation). 

Para ver este informe, amplía **Analizar** en el menú de navegación izquierdo y luego selecciona **Rendimiento de los anuncios**. 

Para realizar un análisis con más detalle de los datos, ofrecemos un vínculo para **descargar informe** que puedes usar para descargar archivos CSV (valores separados por comas) que se pueden abrir en MicrosoftExcel o en otro programa. Como alternativa, puedes recuperar mediante programación estos datos mediante el método [Obtener los datos de rendimiento de los anuncios](../monetize/get-ad-performance-data.md) en la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).

Al visualizar los informes de rendimiento de la publicidad, ten en cuenta que los datos de los informes de los últimos tres días pueden cambiar, ya que recibimos y procesamos nuevos datos procedentes de varios orígenes. Asimismo, pueden producirse modificaciones de datos hasta de los 90 días anteriores.


## <a name="overall-performance"></a>Rendimiento general

En la parte superior del informe, puedes usar los siguientes filtros para ajustar el ámbito de los datos mostrados en el informe:

* **Fecha**: permite filtrar el informe por un período de tiempo predeterminado o un intervalo de fechas personalizado. De forma predeterminada, el informe muestra los datos de los últimos 30 días.
* **Agregación**: permite seleccionar cómo se agregan estos datos y cómo pueden seguir filtrándose. De forma predeterminada, el informe muestra los datos de todas las unidades de anuncios y verás un vínculo **Elegir unidades de anuncios** en la parte inferior de la sección, que te permite seleccionar hasta seis unidades para compararlas. También puedes cambiar la **Agregación** a **Todas las aplicaciones** o **Todos los proveedores de anuncios**. En tal caso, el vínculo de esta sección cambiará a **Elegir aplicaciones** o **Elegir proveedores de anuncios**, lo que te permite elegir hasta seis opciones para compararlas. También puedes agregar una aplicación determinada en la que utilizas anuncios.
* **Proveedores de anuncios**: permite filtrar el informe por datos de rendimiento para algunos proveedores de anuncios. Para obtener más información acerca de los proveedores de anuncios disponibles, consulta la sección [Mediación de anuncios](monetize-with-ads.md#mediation) en [Monetizar con anuncios](monetize-with-ads.md). De forma predeterminada, el informe muestra datos de todos los proveedores de anuncios. Esta opción se desactivará si has elegido **Todos los proveedores de anuncios** en la lista desplegable **Agregación**.
* **Dispositivo**: permite filtrar el informe por los datos de rendimiento de determinados tipos de dispositivos. De forma predeterminada, el informe muestra datos de todos los tipos de dispositivo.


## <a name="report-views"></a>Vistas del informe

Debajo de los filtros, el informe muestra los datos entre una variedad de métricas de rendimiento de anuncios en forma de gráfico, mapamundi y tabla para las unidades de anuncio usadas actualmente en la aplicación.

Para analizar los datos de una de estas métricas en un gráfico o una vista de mapamundi, haz clic en **Gráfico** o **Mapa**. De forma predeterminada, en las vistas de gráfico y mapa se muestran los datos de rendimiento de todas las unidades de anuncios o proveedores de anuncios, en función de las selecciones de la lista desplegable **Agregación**, pero tienes la opción de seleccionar hasta seis unidades de anuncios, aplicaciones o proveedores de anuncios individuales para compararlos.

En la vista de mapa, los tonos más oscuros representan los valores más altos y tonos más claros representan los valores más bajos. Puedes mantener el mouse sobre un país o región específico en el mapa para analizar el valor de la métrica seleccionada. También puedes acercar cualquier área del mapa para ver datos referentes a países más pequeños.

Para revisar todas las métricas de rendimiento para las unidades de anuncios en tu aplicación, consulta la tabla debajo de las vistas de gráfico y mapa.

> [!NOTE]
> Si habías creado unidades de anuncios para una aplicación con pubCenter de Microsoft, es posible que no todas se hayan asignado correctamente a las aplicaciones en el Centro de desarrollo. En este informe, estas unidades de anuncio se asocian con los nombres de las aplicaciones que especificaste en pubCenter, con la cadena **(pubCenter)** anexa al nombre de la aplicación.


## <a name="performance-metrics"></a>Métricas de rendimiento

Este informe puede incluir datos de las siguientes métricas de rendimiento. Las métricas que se muestran en el informe varían según el proveedor de anuncios.

|  Métrica  |  Descripción  |
|----------|---------------|
| Ingresos estimados  |  La cantidad estimada de dinero que has recibido de los anuncios que se ejecutan en la aplicación. |
| eCPM  |  Rentabilidad por mil impresiones. |
| Solicitudes  | El número de veces que se ha enviado una solicitud de anuncio desde la aplicación.  |
| Impresiones  | El número de veces que se ha mostrado un anuncio en la aplicación.  |
| Velocidad de relleno  | El porcentaje de solicitudes de anuncio enviadas desde la aplicación en las que se ha mostrado un anuncio.  |
| Clics  |  El número de veces que alguien ha hecho clic en un anuncio en la aplicación. |
| CTR  |  La tasa de clics, es decir, el número de veces que se ha hecho clic en un anuncio dividido por el número de impresiones. |
| Créditos ganados  | Para [anuncios de la Comunidad](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), esto indica el número de créditos que has ganado por el espacio de anuncios promocionales al mostrar anuncios de la comunidad en tu aplicación.  |
| Créditos gastados  | Para [anuncios de la Comunidad](https://docs.microsoft.com/windows/uwp/publish/about-community-ads), esto indica el número de créditos que has gastado en los anuncios para tu aplicación.  |


## <a name="affiliates-performance"></a>Rendimiento de filial

Si has [decidido participar en el programa de anuncios de filial de Microsoft](about-affiliate-ads.md), aquí podrás ver los datos de rendimiento de los anuncios de filial que aparecen en tu aplicación. Esta información se actualiza diariamente. 


En la parte superior del informe, puedes usar los siguientes filtros para ajustar el ámbito de los datos mostrados en el informe:
- **Fecha**: permite filtrar el informe por un período de tiempo predeterminado o un intervalo de fechas personalizado. De forma predeterminada, el informe muestra los datos de los últimos 30 días.
- **Dispositivo**: permite filtrar el informe por los datos de rendimiento de determinados tipos de dispositivos. De forma predeterminada, el informe muestra datos de todos los tipos de dispositivo.

De forma predeterminada, este informe proporciona un resumen (en forma de gráfico y tabla) de los datos de rendimiento de los anuncios de filial en todas las aplicaciones en las que has decidido participar en el programa de anuncios de filial de Microsoft. Puedes seleccionar **Elegir aplicaciones** para seleccionar hasta seis aplicaciones que podrás comparar.

Los datos de rendimiento de filial se obtienen del seguimiento que realizamos de las siguientes siete métricas de rendimiento de los anuncios de filial de la aplicación:

-   **Ingresos estimados (aprobados)**: La cantidad estimada de dinero que has recibido como comisión por las compras aprobadas que han realizado usuarios haciendo clic en los anuncios de filiales de la aplicación.
-   **Ingresos estimados (pendientes de aprobación)**: La cantidad estimada de dinero que podrías recibir como comisión por las compras pendientes de aprobación.
-   **Impresiones**: El número de veces que se ha mostrado un anuncio de filial en la aplicación.
-   **Clics**: El número de veces que alguien ha hecho clic en un anuncio de filial en la aplicación.
-   **CTR**: La tasa de clics; es decir, el número de veces que se ha hecho clic en un anuncio de filial dividido por el número de impresiones de anuncio de filial.
-   **Compras (aprobadas)**: El número de compras aprobadas que han realizado los usuarios haciendo clic en los anuncios de filiales en la aplicación.
-   **Compras (pendientes de aprobación)**: El número de compras pendientes de aprobación que han realizado los usuarios haciendo clic en anuncios de filiales en la aplicación.

> [!NOTE]
> Cuando un usuario compra un producto en la Tienda, se produce un período de espera de 45 días hasta que la compra puede aprobarse para el programa de anuncios de filial. Debido a este período de espera, los datos de **Ganancias estimadas (aprobadas)**, **Ganancias estimadas (pendientes de aprobación)**, **Compras (aprobadas)** y **Compras (pendientes de aprobación)** de un día determinado pueden cambiar cuando se aprueben o rechacen las compras.


 
