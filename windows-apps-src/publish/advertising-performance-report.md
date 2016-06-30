---
author: jnHs
Description: "Para ver los datos de rendimiento de las unidades de anuncio en tus aplicaciones, usa los informes de rendimiento de la publicidad de nivel de aplicación y de nivel de cuenta en el panel del Centro de desarrollo de Windows."
title: Informe de rendimiento de la publicidad
ms.assetid: 32E555C3-C34D-4503-82BB-4C3F5CAE4500
translationtype: Human Translation
ms.sourcegitcommit: 93e12837aec151b0cd1fa711c9e04081d74a3962
ms.openlocfilehash: 1617005cff264a89eb66e326e2bedf9f6641a3da

---

# Informe de rendimiento de la publicidad


Para ver los datos de rendimiento de las unidades de anuncios en tus aplicaciones, puedes usar los siguientes informes en el panel del Centro de desarrollo de Windows:

-   [Informe de rendimiento de la publicidad en el nivel de la aplicación](advertising-performance-report.md#app-level-advertising-performance-report). En este informe se proporcionan los datos de rendimiento de las unidades de anuncios de Microsoft en la aplicación seleccionada en el panel.
-   [Informe de rendimiento de la publicidad en el nivel de la cuenta](advertising-performance-report.md#account-level-advertising-performance-report). Este informe proporciona datos de rendimiento detallados de las unidades de anuncios de Microsoft y los anuncios de la comunidad de todas las aplicaciones que están registradas en tu cuenta de desarrollador.
-   [Informe de rendimiento de la publicidad en el nivel del panel](advertising-performance-report.md#dashboard-level-advertising-performance-report). Este informe en la página **Información general del panel** proporciona un resumen de los datos de rendimiento de las unidades de anuncios de Microsoft en todas las aplicaciones que están registradas en tu cuenta de desarrollador.

De manera predeterminada, los informes se filtran según el rendimiento de los últimos 30 días, en todos los dispositivos. Para cambiar estos filtros, haz clic en **Filtros de página** y elige un intervalo de tiempo diferente o un tipo de dispositivo concreto. 

> **Nota** Puede haber diferencias entre los informes de rendimiento de publicidad del Centro de desarrollo y de pubCenter. Los datos de rendimiento de la publicidad del Centro de desarrollo se agregan en función de la hora UTC (no de la zona horaria concreta), mientras que los informes de pubCenter se agregan según tu zona horaria.

En las siguientes secciones se proporcionan más detalles sobre estos informes.

## Informe de rendimiento de la publicidad en el nivel de la aplicación

Esta página muestra en el panel los datos de rendimiento en forma de gráfico, mapamundi y tabla para las unidades de anuncios de Microsoft en la aplicación seleccionada actualmente. Para ver este informe, selecciona una de las aplicaciones en el panel y haz clic en **Análisis**&gt;**Rendimiento de publicidad** en el panel de navegación.

Los datos se obtienen del seguimiento que realizamos de las siguientes métricas de rendimiento de los anuncios de la aplicación:

-   **Ingresos estimados**: la cantidad estimada de dinero que has recibido de los anuncios que se ejecutan en la aplicación.
-   **eCPM**: rentabilidad por mil impresiones.
-   **Solicitudes**: el número de veces que una solicitud de anuncio se envió desde la aplicación.
-   **Impresiones**: el número de veces que se mostró un anuncio en la aplicación.
-   **Velocidad de relleno**: el porcentaje de solicitudes de anuncio enviadas desde la aplicación en que se mostró un anuncio.
-   **Clics**: el número de veces que alguien hizo clic en un anuncio de la aplicación.
-   **CTR**: tasa de clics, es decir, el número de veces que se ha hecho clic en un anuncio dividido por el número de impresiones.

Para revisar todas estas métricas de rendimiento para las unidades de anuncios en tu aplicación, consulta la tabla debajo del gráfico y de las vistas de mapa.

Para analizar los datos de una de estas métricas en un gráfico o una vista de mapamundi, haz clic en **Gráfico** o **Mapa**. Haz clic en los encabezados situados sobre el gráfico o el mapa para cambiar entre las diferentes métricas. De manera predeterminada, en el gráfico y las vistas de mapa se muestran los datos de rendimiento para todas las unidades de anuncios en la aplicación, pero puedes hacer clic en **Elegir unidades de anuncios** para seleccionar hasta seis unidades de anuncio individuales para compararlas.

En la vista de mapa, los tonos más oscuros representan los valores más altos y tonos más claros representan valores más bajos. Puedes mantener el mouse sobre un país o región específico en el mapa para analizar el valor de la métrica seleccionada. También puedes hacer zoom en cualquier área del mapa para ver datos de los países más pequeños.

## Informe de rendimiento de la publicidad en el nivel de la cuenta

Esta página muestra los datos de rendimiento de las unidades de anuncios de Microsoft y los anuncios de la comunidad usados en las aplicaciones que están registradas en tu cuenta de desarrollador. Para ver este informe, ve a la página de información general del panel y haz clic en **Rendimiento de publicidad** en el panel de navegación.

Esta página incluye las siguientes secciones.

### Microsoft Advertising

Este informe proporciona los datos de rendimiento de todas las unidades de anuncios de Microsoft usadas en tus aplicaciones. Este informe también incluye los datos de rendimiento de las unidades de anuncio de pubCenter que no se asignaron correctamente a las aplicaciones del Centro de desarrollo.

Este informe muestra las siete mismas métricas de rendimiento y las mismas vistas (gráfico, mapamundi y tabla) que el informe de rendimiento de publicidad en el nivel de la aplicación descrito anteriormente. Puedes aplicar los siguientes filtros a este informe:

-   **Todas las unidades de anuncio**. Cuando se selecciona este filtro, puedes elegir mostrar los datos de todas las unidades de anuncio o de hasta seis unidades de anuncio específicas.
-   **Todas las aplicaciones**. Cuando se selecciona este filtro, puedes elegir mostrar los datos de todas las aplicaciones o de hasta seis aplicaciones específicas.
-   **Aplicación individual**. Cuando se selecciona una aplicación, puedes elegir mostrar los datos de todas las unidades de anuncio usadas por la aplicación o de hasta seis unidades de anuncio específicas usadas por la aplicación.

Si has creado unidades de anuncios para una aplicación con pubCenter de Microsoft, es posible que no se todas se hayan asignado correctamente a las aplicaciones en el Centro de desarrollo. En este informe, estas unidades de anuncio se asocian con los nombres de las aplicaciones que especificaste en pubCenter, con la cadena **(pubCenter)** anexa al nombre de la aplicación.

Si creaste unidades de anuncios para una aplicación con pubCenter y no ves datos sobre ellos en ninguna página de informe, haz clic en **Vincular una cuenta de pubCenter** para vincular esa cuenta a tu cuenta del Centro de desarrollo. Escribe la dirección de correo electrónico asociada a esta cuenta de pubCenter y tu código de vinculación de cuenta. Para encontrar el código de vinculación de cuenta, inicia sesión en la cuenta de pubCenter y ve a la página **Mi información**.

Para obtener más información sobre la migración de cuentas de pubCenter al Centro de desarrollo, consulta el tema [Integración del Centro de desarrollo y pubCenter](pubcenter-dev-center-integration.md).

### Anuncios de la comunidad Microsoft

En esta sección se proporcionan los datos de rendimiento en forma de gráfico y mapamundi de los anuncios de la comunidad en la aplicación seleccionada actualmente en el panel. Para obtener más información sobre los anuncios de la comunidad, consulta [Acerca de los anuncios de la comunidad](about-community-ads.md).

Los datos se obtienen del seguimiento que realizamos de las siguientes métricas de rendimiento de los anuncios en tu aplicación:

-   **Solicitudes**: el número de veces que se envió una solicitud de anuncio de la comunidad desde la aplicación.
-   **Velocidad de relleno**: el porcentaje de solicitudes de anuncio de la comunidad enviadas desde la aplicación en que se mostró un anuncio.
-   **Clics**: el número de veces que alguien hizo clic en un anuncio de la comunidad en la aplicación.
-   **CTR**: ratio de click-through, es decir, el número de veces que se hizo clic en un anuncio de la comunidad, dividido por el número de impresiones.
-   **Créditos obtenidos**: el número de créditos por anuncios de la comunidad que has obtenido de esta aplicación. Para obtener más información acerca de cómo se obtienen los créditos, consulta [Acerca de los anuncios de la comunidad](about-community-ads.md).
-   **Créditos usados**: el número de créditos por anuncios de la comunidad que has usado para esta aplicación. Para obtener más información acerca de cómo se usan los créditos, consulta [Acerca de los anuncios de la comunidad](about-community-ads.md).

Para analizar los datos de una de estas métricas en un gráfico o una vista de mapamundi, haz clic en **Gráfico** o **Mapa**. Haz clic en los encabezados situados sobre el gráfico o el mapa para cambiar entre las diferentes métricas. En la vista de mapa, los tonos más oscuros representan los valores más altos y tonos más claros representan valores más bajos. Puedes mantener el mouse sobre un país o región específico en el mapa para analizar el valor de la métrica seleccionada. También puedes hacer zoom en cualquier área del mapa para ver datos de los países más pequeños.

## Informe de rendimiento de la publicidad en el nivel del panel

La sección **Rendimiento de publicidad** en la página **Información general del panel** muestra un resumen de los datos de rendimiento de todas las unidades de anuncios de Microsoft usadas en tus aplicaciones. Este informe es similar a la sección **Microsoft Advertising** en el informe de rendimiento de publicidad en el nivel de cuenta, con las siguientes diferencias:

-   Proporciona los datos únicamente en forma de gráfico. Para obtener una versión en tabla de los datos, usa el informe de rendimiento de publicidad en el nivel de la cuenta.
-   Los únicos filtros proporcionados son para todas las aplicaciones o para aplicaciones individuales. Para filtrar por unidades de anuncio, usa el informe de rendimiento de publicidad en el nivel de la cuenta.


 

 



<!--HONumber=Jun16_HO4-->


