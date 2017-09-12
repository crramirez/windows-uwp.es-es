---
author: jnHs
Description: "El informe de mediación de anuncios te permite ver la velocidad de relleno efectiva y las velocidades de relleno respectivas para las redes de anuncios que estás usando."
title: "Informe Mediación de anuncios"
ms.assetid: 18A33928-B9F2-4F76-9A9C-F01FEE42FEA1
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 0c08c1061231b033dc5401c77085e16ea7001bb1
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2017
---
# <a name="ad-mediation-report"></a>Informe Mediación de anuncios

Este informe muestra datos de la mediación de anuncios para las aplicaciones de Windows 8.x o Windows Phone 8.x que usan un **AdMediatorControl** desde el [SDK de Microsoft Advertising para Windows and Windows Phone 8.x](http://aka.ms/store-8-sdk) para mediar entre anuncios de banner de varias redes de anuncios. El informe de mediación de anuncios para estas aplicaciones te permite ver la velocidad de relleno efectiva y las velocidades de relleno respectivas para las redes de anuncios que estás usando. También muestra las velocidades de adopción de las configuraciones de mediación y proporciona visibilidad de los errores notificados por las redes de anuncios y el mediador. Puedes visualizar estos datos en tu panel de información o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión.

> [!NOTE]
> El informe **Mediación de anuncios** solo está disponible si utilizas un control **AdMediatorControl** en tu aplicación para Windows8.x o WindowsPhone8.x. Para obtener más información, consulta [este artículo](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359). Para una aplicación para UWP que usa [mediación de anuncios](monetize-with-ads.md#mediation) en un control **AdControl** o **InterstitialAd**, usa el [Informe Rendimiento de la publicidad](advertising-performance-report.md) para revisar los datos de rendimiento de las redes de anuncios.

## <a name="page-filters"></a>Filtros de página

Cerca de la parte superior de la página, puede expandir **Filtros de página** para filtrar todos los datos de esta página por intervalo de fechas o por mercado.

-   **Fecha**: El filtro predeterminado es **Últimos 30 días**, pero puedes ampliarlo hasta **Últimos 12 meses**.
-   **Mercado**: El valor predeterminado es **Todos los mercados**. Puedes elegir un mercado específico si quieres que esta página solo muestre clasificaciones de clientes en ese mercado.
-   **Plataforma**: El valor predeterminado es **Todas las plataformas**. Si la aplicación está destinada a varias plataformas, puedes elegir una plataforma específica.

La información de todos los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en **Filtros de página**. De manera predeterminada, esto incluirá datos de todos los mercados y plataformas en que se muestra la aplicación, a menos que hayas usado **Filtros de página** para especificar un mercado o una plataforma en concreto.

## <a name="ad-mediation-performance"></a>Rendimiento de la mediación de anuncios

El gráfico **Rendimiento de la mediación de anuncios** muestra el promedio de velocidad de relleno total durante el período de tiempo seleccionado. Se trata del promedio de velocidad de relleno en todas las sesiones de usuario, independientemente de la configuración de mediación o de la frecuencia con que se llamó a redes de anuncios.

Puedes hacer clic en el encabezado **Solicitudes de mediación** para ver el promedio de solicitudes de mediación individuales o hacer clic en **Anuncios entregados** para ver el promedio total de anuncios entregados.

## <a name="ad-provider-fill-rates"></a>Velocidades de relleno del proveedor de anuncios

El gráfico **Velocidades de relleno del proveedor de anuncios** muestra la velocidad de relleno promedio de cada una de tus redes de anuncios durante el período de tiempo seleccionado.

La información de cada red de anuncios se muestra agrupada para que puedas comprar el rendimiento de cada una.

## <a name="unique-users-per-mediation-configuration"></a>Usuarios únicos por configuración de mediación

El gráfico **Usuarios únicos por configuración de mediación** muestra el número total de usuarios únicos que recibieron cada versión de la configuración de mediación durante el período de tiempo seleccionado.

## <a name="errors-by-ad-network"></a>Errores por red de anuncios

El gráfico **Errores por red de anuncios** muestra el número total de solicitudes y errores para cada una de las redes de anuncios, junto con el porcentaje de solicitudes que provocaron un error.

## <a name="errors-by-type"></a>Errores por tipo

El gráfico **Errores por tipo** muestra los errores específicos que experimentó cada red de anuncios. También se muestra el porcentaje del total de errores de la red que representa un error específico para que tengas una idea de qué errores se producen frecuentemente por red de anuncios.

 

 
