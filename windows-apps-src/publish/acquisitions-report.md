---
description: El informe de adquisiciones del centro de Partners le permite ver quién ha adquirido e instalado su aplicación, junto con los detalles demográficos y de la plataforma.
title: Informe de adquisiciones
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, adquisiciones, ventas de aplicaciones, descargas de aplicaciones, instalaciones, embudo, adquisición, conversiones, canales, vistas de páginas de la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: cabdbd1433da64c927fe5e8326b7906095573497
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033288"
---
# <a name="acquisitions-report"></a>Informe de adquisiciones


El informe de **adquisiciones** del [centro de Partners](https://partner.microsoft.com/dashboard) le permite ver quién ha adquirido e instalado su aplicación, junto con detalles demográficos y de plataforma, y muestra información sobre cómo los clientes de Windows 10 (incluida Xbox) han llegado a la lista de la aplicación. También puede ver datos de adquisición casi en tiempo real durante la última hora o el período de 72 horas. 

Puede ver estos datos en el centro de Partners o [descargar el informe](download-analytic-reports.md) para verlo sin conexión. Como alternativa, puede recuperar estos datos mediante programación con la API de [REST de Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

En este informe, una **adquisición** significa que un nuevo cliente ha obtenido una licencia para su aplicación (independientemente de que se le cobre o que la haya ofrecido gratis). Una **instalación** hace referencia a la aplicación que se instala en un dispositivo de Windows 10.

> [!IMPORTANT]
> El informe de **adquisiciones** no incluye datos sobre reembolsos, inversiones, contracargos, etc. Para hacer una estimación de la aplicación, visite [Resumen de pago](payout-summary.md). En la sección **Reservado** , haz clic en el vínculo **Descargar transacciones reservadas** .
>
> Excepto en el caso de los datos de la vista de página (como se describe a continuación), este informe no incluye los datos relacionados con los clientes que adquieren una aplicación sin haber iniciado sesión en un cuenta de Microsoft.


## <a name="apply-filters"></a>Aplicación de filtros

Cerca de la parte superior de la página, puede seleccionar el período de tiempo para el que desea mostrar los datos. La selección predeterminada es **30D** (30 días), pero puede optar por mostrar los datos de 3, 6 o 12 meses, o de un intervalo de datos personalizado que especifique. Los datos casi en tiempo real se mostrarán para todas las opciones (excepto en los datos **acumulados** de la aplicación). Los períodos de tiempo **1H** y **72H** solo se aplican a la pestaña diariamente de la **aplicación** del gráfico **adquisiciones** y a la pestaña **adquisiciones** del gráfico **Markets** . 

También puede expandir **filtros** para filtrar todos los datos de esta página por mercado o por tipo de dispositivo.

-   **Market** : el filtro predeterminado es **todos los mercados** , pero puede limitar los datos a las adquisiciones en uno o varios mercados.
-   **Tipo de dispositivo** : la opción predeterminada es **Todos los dispositivos** . Si desea mostrar los datos de las adquisiciones de un solo tipo de dispositivo determinado (como PC, consola o tableta), puede elegir uno específico aquí.

La información de todos los gráficos que se enumeran a continuación reflejará el intervalo de fechas y los filtros que haya seleccionado. Algunas secciones también permiten aplicar filtros adicionales.


## <a name="acquisitions"></a>Adquisiciones

En el gráfico **adquisiciones** se muestra el número de adquisiciones diarias o semanales (un nuevo cliente que obtiene una licencia para su aplicación) durante el período de tiempo seleccionado. (Si usa **aplicar filtros** para Mostrar datos durante más tiempo, los datos de adquisición se agruparán por semana). En este gráfico solo se incluyen las adquisiciones realizadas por los clientes que han iniciado sesión con un cuenta de Microsoft válido. 

De forma predeterminada, se muestra la vista diaria de la **aplicación** , que incluye datos casi en tiempo real. También puede ver el número de duración de las adquisiciones de la aplicación seleccionando **aplicación acumulativa** . Muestra el total acumulado de todas las adquisiciones desde que la aplicación se publicó por primera vez.

Las **ventas brutas** de la aplicación (a partir del 2016 de octubre) también están disponibles en este gráfico, que muestra la cantidad total obtenida de las ventas de las aplicaciones (en USD). Tenga en cuenta que esta cantidad no tiene en cuenta los reembolsos, las inversiones, los contracargos, etc.

Opcionalmente, puede filtrar los resultados si la adquisición se originó en el cliente o en el almacén basado en web o en la versión del sistema operativo.

> [!NOTE]
> También puede recuperar mediante programación estos datos mediante el método [Get App Adquisitions](../monetize/get-app-acquisitions.md) de nuestra API de [REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

En la vista diaria de la **aplicación** , cuando se selecciona el período de tiempo **30D** , puede ver marcadores de círculo. Representan un aumento o una disminución significativos en un valor determinado que creemos que querrá conocer. La fecha en la que aparece el círculo representa el final de la semana en el que se ha detectado un aumento o una disminución significativos en comparación con la semana anterior. Para ver más detalles sobre lo que ha cambiado, mantenga el mouse sobre el círculo.  

> [!TIP]
> Puede ver más información relacionada con los cambios significativos en los últimos 30 días del [Informe Insights](insights-report.md).

## <a name="installs"></a>Instala .

El gráfico de **instalaciones** muestra el número de veces que hemos detectado que los clientes han instalado correctamente la aplicación en dispositivos Windows 10 (incluidas las consolas de Xbox One) durante el período de tiempo seleccionado. Se muestra el número total, junto con un gráfico que muestra las instalaciones por día o semana (en función de la duración seleccionada). Opcionalmente, puede filtrar los resultados por una versión específica del paquete.

El total de instalación incluye:
-   **Se instala en varios dispositivos Windows 10.** Por ejemplo, si el mismo cliente instala la aplicación en dos equipos con Windows 10 y una consola Xbox One, cuenta como tres instalaciones.
-   **Vuelve a instalar.** Por ejemplo, si un cliente instala hoy mismo su aplicación, desinstala la aplicación mañana y, a continuación, vuelve a instalar la aplicación el mes siguiente, que cuenta como dos instalaciones.

El total de la instalación no incluye ni refleja:
-   **Se instala en dispositivos que no son de Windows 10.** Si la aplicación es compatible con versiones anteriores del sistema operativo, como Windows 8. x o Windows Phone 8. x, no contamos con ninguna instalación en esos dispositivos.
-   **Desinstala.** Cuando un cliente desinstala la aplicación de su dispositivo, no la resta del número total de instalaciones.
-   **Actualizaciones.** Por ejemplo, si un cliente instala hoy mismo la aplicación y, a continuación, instala una actualización de la aplicación una semana posterior, solo cuenta como una instalación.
-   **Preinstala.** Si un cliente compra un dispositivo que tiene la aplicación preinstalada, no se cuenta como una instalación.
-   **Instalaciones iniciadas por el sistema.** Si Windows instala la aplicación automáticamente por alguna razón, no la contamos como una instalación.

> [!NOTE]
> También puede recuperar mediante programación estos datos mediante el método [Get App Installs](../monetize/get-app-installs.md) en nuestra API de [REST de Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="acquisition-funnel"></a>Embudo de adquisición

El **embudo de adquisición** muestra cuántos clientes completaron cada paso del embudo, desde ver la página de la tienda hasta el uso de la aplicación, junto con la tasa de conversión. Estos datos pueden ayudarle a identificar las áreas en las que podría querer invertir más para aumentar las adquisiciones, instalaciones o uso.

> [!IMPORTANT]
> El **embudo de adquisición** muestra solo los datos de los clientes de Windows 10 (incluida Xbox) en los últimos 90 días.

Los pasos del embudo son los siguientes:

- **Vistas de página** : este número representa el total de vistas de la lista de tiendas de la aplicación, incluidas las personas que no han iniciado sesión con un cuenta de Microsoft. Esto no incluye los datos de los clientes que han optado por no proporcionar esta información a Microsoft.
- **Adquisiciones** : el número de clientes nuevos que obtuvieron una licencia para su aplicación (cuando inició sesión con su cuenta de Microsoft) en el plazo de 48 horas a la hora de ver la lista de la tienda.
- **Instala** : el número de clientes que instalaron la aplicación después de adquirirla.
- **Uso** : el número de clientes que usaron la aplicación después de instalarla.

Opcionalmente, puede filtrar los resultados por sexo o grupo de edad, así como por el ID. de campaña personalizado.

> [!NOTE]
> También puede recuperar mediante programación estos datos mediante el método de [datos de canal de adquisición obtener aplicación](../monetize/get-acquisition-funnel-data.md) de la [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="markets"></a>Mercados

El gráfico de **mercados** muestra el número total de adquisiciones o instalaciones durante el período de tiempo seleccionado para cada mercado en el que la aplicación está disponible. Puede elegir si desea mostrar los datos de las **adquisiciones** o **instalaciones** .

Puede ver estos datos en un formulario de **mapa** visual o alternar la configuración para verlo en un formulario de **tabla** . El formulario de tabla mostrará cinco mercados a la vez, ordenados alfabéticamente o por el número más alto o más bajo de adquisiciones o instalaciones. También puede descargar los datos para ver la información de todos los mercados juntos.


## <a name="customer-demographic"></a>Grupo demográfico de clientes

El gráfico **Grupo demográfico de clientes** muestra información demográfica sobre las personas que adquirieron la aplicación. Puedes ver cuántas adquisiciones (durante el período de tiempo seleccionado) han realizado las personas de un determinado grupo de edad divididas por sexo.

> [!NOTE]
> Algunos clientes han optado por no compartir esta información. Si no podemos determinar el grupo de edad o el sexo, la adquisición se clasifica como **Desconocida** .

 

## <a name="app-page-views-and-conversions-by-channel"></a>Vistas de página de la aplicación y conversiones por canal

El gráfico de **vistas y conversiones de la página de aplicaciones por canal** le permite ver cómo llegaron los clientes de Windows 10 a la lista de la aplicación durante el período de tiempo seleccionado.

En este gráfico, un *canal* hace referencia al método en el que un cliente llegó a la página de lista de la aplicación (por ejemplo, examinando y buscando en la tienda, un vínculo desde un sitio web externo, un vínculo de una de sus campañas personalizadas, etc.). Se incluyen los siguientes tipos de canal:

-   **Tráfico de la Tienda:** el cliente estaba explorando o buscando dentro de la Tienda cuando vio la descripción de la aplicación.
-   **Campaña personalizada:** el cliente siguió un vínculo que usa un [identificador de campaña personalizado](create-a-custom-app-promotion-campaign.md).
-   **Otro:** El cliente siguió un vínculo externo (sin ningún identificador de campaña personalizado) de un sitio web a la lista de la aplicación o el cliente siguió un vínculo de un motor de búsqueda a la lista de la aplicación.

Una *vista de página* significa que un cliente vio la página de descripción de la aplicación de la Tienda, bien a través de la Tienda web o desde la aplicación de la Tienda de Windows 10. Esto incluye las vistas de personas que no han iniciado sesión con un cuenta de Microsoft. Algunos clientes han optado por no proporcionar esta información a Microsoft.

Una *conversión* significa que un cliente (que inicia sesión con una cuenta de Microsoft) ha obtenido recientemente una licencia para la aplicación (independientemente de que se le cobre o que la haya ofrecido gratis).

La vista de página y los números de conversión no son recuentos de clientes únicos. Para obtener información sobre la tasa de conversión, vea el gráfico de [embudo de adquisición](#acquisition-funnel) .

> [!NOTE]
> Los clientes pueden llegar a la lista de la aplicación haciendo clic en una campaña personalizada que no ha creado. Se marcan todas las vistas de página dentro de una sesión con el identificador de campaña desde el que el cliente entró por primera vez en el almacén. Después, vamos a asignar las conversiones a ese identificador de campaña para todas las adquisiciones en un plazo de 24 horas. Por este motivo, es posible que vea un mayor número de conversiones totales que las conversiones totales de los identificadores de campaña, y puede que tenga conversiones o conversiones de complementos con cero vistas de página.

> [!NOTE]
> También puede recuperar mediante programación estos datos mediante el método [obtener conversiones de aplicaciones por canal](../monetize/get-app-conversions-by-channel.md) en nuestra API de [REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="app-page-views-and-conversions-by-campaign-id"></a>Vistas de página de la aplicación y conversiones por identificador de campaña

El gráfico de las **vistas y conversiones de la página de la aplicación por ID. de campaña** le permite realizar un seguimiento de las conversiones y las vistas de página, como se describió anteriormente, para cada una de las [campañas de promoción personalizadas](create-a-custom-app-promotion-campaign.md). Se muestran los identificadores de campaña principales y puede usar los filtros para excluir o incluir determinados identificadores de campaña.

## <a name="total-campaign-conversions"></a>Total de conversiones de campaña

El gráfico de **conversiones de campaña total** muestra el número total de conversiones de aplicaciones y complementos de todas las campañas personalizadas durante el período de tiempo seleccionado.





 

 
