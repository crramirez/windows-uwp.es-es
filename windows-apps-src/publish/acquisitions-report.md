---
author: jnHs
Description: The Acquisitions report in Partner Center lets you see who has acquired and installed your app, along with demographic and platform details.
title: Informe Adquisiciones
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, adquisiciones, ventas de aplicaciones, descargas de aplicaciones, instalaciones, embudo, adquisición, conversiones, canal, vistas de página de la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: bbd84b32a4e94687ff50f553137a86885094a7d6
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6032726"
---
# <a name="acquisitions-report"></a>Informe Adquisiciones


El informe de **adquisiciones** del [Centro](https://partner.microsoft.com/dashboard) de partners te permite ver quién ha adquirido e instalado la aplicación, junto con datos demográficos y los detalles de la plataforma y se muestra información sobre cómo los clientes en Windows 10 (incluyendo Xbox) han llegado a la descripción de la aplicación. También puedes ver cerca de los datos de compra en tiempo real para el último período o de 72 horas. 

Puedes ver estos datos en el centro de partners, o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión. Como alternativa, puedes recuperar mediante programación estos datos con nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

En este informe, una **adquisición** significa que un cliente nuevo ha obtenido una licencia para tu aplicación (tanto si cobras dinero como si la ofreces gratis). **Instalar** hace referencia a que la aplicación se está instalando en un dispositivo Windows10.

> [!IMPORTANT]
> El informe **Adquisiciones** no incluye datos sobre devoluciones, inversiones, anulaciones, etc. Para calcular las ganancias por la aplicación, visita [Resumen de pago](payout-summary.md). En la sección **Reservado**, haz clic en el vínculo **Descargar transacciones reservadas**.
>
> A excepción de los datos de la vista de página (tal y como se describe a continuación), este informe no incluye datos relacionados con los clientes que adquieran una aplicación sin iniciar sesión en una cuenta de Microsoft.


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **30D** (30 días), pero también puedes mostrar los datos durante 3, 6 o 12 meses o durante un intervalo de fechas personalizado que especifiques. Casi en tiempo real se mostrarán datos de todas las opciones (excepto en los datos de la **aplicación acumulativa** ). El tiempo **1 H** y **72 H** períodos solo se aplican a la pestaña **aplicación diariamente** del gráfico de **adquisiciones** y a la pestaña de **adquisiciones** del gráfico de **mercados** . 

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página por mercado o por tipo de dispositivo.

-   **Mercado**: el filtro predeterminado es **Todos los mercados**, pero puedes limitar los datos a adquisiciones de uno o varios mercados.
-   **Tipo de dispositivo**: la opción predeterminada es **Todos los dispositivos**. Si quieres mostrar datos de adquisiciones de un determinado tipo de dispositivo únicamente (como PC, consola o tableta), aquí puedes elegir uno específico.

La información de todos los gráficos que aparecen a continuación reflejará el intervalo de fechas y los filtros que hayas seleccionado. Algunas secciones también te permiten aplicar filtros adicionales.


## <a name="acquisitions"></a>Adquisiciones

En el gráfico **Adquisiciones** se muestra el número de adquisiciones diarias o semanales (un nuevo cliente que obtiene una licencia para tu aplicación) durante el período de tiempo seleccionado. (Si usas **Aplicar filtros** para mostrar los datos durante más tiempo, los datos de adquisición se agruparán por semana). En este gráfico solo se incluyen las adquisiciones realizadas por los clientes que han iniciado sesión con una cuenta de Microsoft válida. 

De manera predeterminada, mostramos la vista de la **aplicación todos los días** , que incluye cerca de datos en tiempo real. También puedes ver el número de adquisiciones del ciclo de vida de la aplicación seleccionando **Aplicación acumulativa**. Muestra el total acumulado de todas las adquisiciones desde que la aplicación se publicó por primera vez.

**Ventas brutas** para la aplicación (de octubre de 2016 - presente) también están disponibles en este gráfico, que muestra el importe total acumulado de ventas de la aplicación (en USD). Ten en cuenta que no tiene en cuenta esta cantidad para los reembolsos, devoluciones, anulación, etcetera.

También puedes filtrar los resultados por si la adquisición se originó desde el cliente, la Tienda web o la versión del sistema operativo.

> [!NOTE]
> También puedes recuperar mediante programación estos datos mediante el método [obtener los datos de las adquisiciones de la aplicación](../monetize/get-app-acquisitions.md) en nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

En la vista de la **aplicación todos los días** , cuando el **30D** período de tiempo seleccionado, puede ver los marcadores de círculo. Estos representan un aumento significativo o disminución un valor determinado que creemos que querrás saber sobre. La fecha en el que se muestra el círculo representa el final de la semana en el que hemos detectado un aumento significativo o una disminución en comparación con la semana anterior. Para ver más detalles sobre qué ha cambiado, mantén el puntero encima del círculo.  

> [!TIP]
> Puedes ver más detalles relacionados con los cambios importantes a través de los últimos 30 días en el [informe de información](insights-report.md).

## <a name="installs"></a>Instalaciones

En el gráfico **Instalaciones** se muestra cuántas veces hemos detectado que los clientes han instalado correctamente la aplicación en dispositivos Windows10 (incluidas las consolas Xbox One) durante el período de tiempo seleccionado. Se muestra el número total, junto con un gráfico que muestra las instalaciones por día o semana (en función de la duración que hayas seleccionado). También puedes filtrar los resultados por una determinada versión del paquete.

El total de instalaciones incluye lo siguiente:
-   **Instalaciones en varios dispositivos Windows10.** Por ejemplo, si el mismo cliente instala tu aplicación en dos equipos con Windows10 y una Consola Xbox One, cuenta como tres instalaciones.
-   **Reinstalaciones.** Por ejemplo, si un cliente instala la aplicación hoy, la desinstala mañana y la vuelve a instalar al mes siguiente, cuenta como dos instalaciones.

El total de instalaciones no incluye ni refleja lo siguiente:
-   **Instalaciones en dispositivos que no usan Windows10.** Si la aplicación es compatible con las versiones anteriores del sistema operativo, tales como Windows8.x o WindowsPhone8.x, no contamos ninguna instalación en dichos dispositivos.
-   **Desinstalaciones.** Si un cliente desinstala la aplicación de su dispositivo, no la restamos al número total de instalaciones.
-   **Actualizaciones.** Por ejemplo, si un usuario instala la aplicación hoy e instala una actualización de la aplicación una semana después, solo se cuenta como una instalación.
-   **Preinstalaciones.** Si un cliente compra un dispositivo que tenga la aplicación preinstalada, no lo contamos como una instalación.
-   **Instalaciones iniciadas por el sistema.** Si Windows instala la aplicación automáticamente por algún motivo, no lo contamos como una instalación.

> [!NOTE]
> También puedes recuperar mediante programación estos datos mediante el método de [obtención de las instalaciones de la aplicación](../monetize/get-app-installs.md) en nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="acquisition-funnel"></a>Embudo de adquisiciones

El **Embudo de adquisiciones** te muestra cuántos clientes realizaron cada paso del embudo, desde la visualización de la página de la Tienda hasta el uso de la aplicación, junto con la tasa de conversión. Estos datos pueden ayudarte a identificar las áreas donde podrías invertir más para aumentar tu adquisiciones, instalaciones o uso.

> [!IMPORTANT]
> El **Embudo de adquisiciones** solo muestra los datos de clientes de Windows10 (incluida la Xbox) durante los últimos 90 días.

Los pasos del embudo son:

- **Vistas de página**: este número representa el total de vistas de la descripción de la Tienda de tu aplicación, incluidas las personas que no inician sesión con una cuenta de Microsoft. No se incluyen los datos de clientes que han decidido no proporcionar esta información a Microsoft.
- **Adquisiciones**: el número de clientes nuevos que han obtenido una licencia para la aplicación (que iniciaron sesión con su cuenta de Microsoft) en un plazo de 48 horas desde que visualizaron la descripción de la Tienda.
- **Instalaciones**: el número de clientes que han instalado la aplicación después de adquirirla.
- **Uso**: el número de clientes que han utilizado la aplicación después de instalarla.

También puedes filtrar los resultados por género o grupo de edad, así como por identificador de campaña personalizada.

> [!NOTE]
> También puedes recuperar mediante programación estos datos mediante el método [obtener datos de embudo de adquisiciones de aplicaciones](../monetize/get-acquisition-funnel-data.md) en nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="markets"></a>Mercados

En el gráfico **Mercados** se muestra el número total de adquisiciones o instalaciones durante el período de tiempo seleccionado en cada mercado en el que tu aplicación está disponible. Puedes optar por mostrar los datos de **Adquisiciones** o **Instalaciones**.

Puedes ver estos datos en forma de **Mapa** visual o cambiar la configuración para verlos en forma de **Tabla**. La tabla mostrará cinco mercados a la vez, ordenados alfabéticamente o por mayor o menor número de adquisiciones o instalaciones. También puedes descargar los datos para ver la información de todos los mercados de forma conjunta.


## <a name="customer-demographic"></a>Grupo demográfico de clientes

El gráfico **Grupo demográfico de clientes** muestra información demográfica sobre las personas que adquirieron la aplicación. Puedes ver cuántas adquisiciones (durante el período de tiempo seleccionado) realizaron las personas de un determinado grupo de edad divididas por sexo.

> [!NOTE]
> Algunos clientes han optado por no compartir esta información. Si no podemos determinar el grupo de edad o el sexo, la adquisición se clasifica como **Desconocida**.

 

## <a name="app-page-views-and-conversions-by-channel"></a>Vistas de página de la aplicación y conversiones por canal

El gráfico de **vistas de página y conversiones por canal** te permite ver cómo los clientes de Windows 10 llegaron hasta la descripción de la aplicación durante el período de tiempo seleccionado.

En este gráfico, un *canal* hace referencia al método por el que un cliente llegó a la página de descripción de la aplicación (por ejemplo, explorando y buscando en la Tienda, un vínculo de un sitio web externo, un vínculo de una de las campañas personalizadas, etc.). Se incluyen los siguientes tipos de canal:

-   **Tráfico de la Tienda:** El cliente estaba explorando o buscando dentro de la Tienda cuando vio la descripción de la aplicación.
-   **Campaña personalizada:** El cliente ha seguido un vínculo que usa un [identificador de campaña personalizado](create-a-custom-app-promotion-campaign.md).
-   **Otro:** El cliente ha seguido un vínculo externo (sin ningún identificador de campaña personalizado) desde un sitio web hasta la descripción de la aplicación o el cliente ha seguido un vínculo desde un motor de búsqueda a la descripción de la aplicación.

Una *vista de página* significa que un cliente vio la página de descripción de la tienda de la aplicación, a través de la tienda Web o desde la aplicación de la tienda en Windows 10. Se incluyen vistas por personas que no inician sesión con una cuenta de Microsoft. Algunos clientes han decidido no proporcionar esta información a Microsoft.

Una *conversión* significa que un cliente (que ha iniciado sesión con una cuenta de Microsoft) acaba de obtener una licencia para tu aplicación (tanto si cobras dinero como si la ofreces gratis).

Las vistas de página y números de conversión no son recuentos de clientes únicos. Para obtener información sobre la tasa de conversión, consulta el gráfico [Embudo de adquisiciones](#acquisition-funnel).

> [!NOTE]
> Los clientes pudieron llegar a la descripción de la aplicación haciendo clic en una campaña personalizada que no hayas creado tú. Marcamos cada vista de página dentro de una sesión con el identificador de campaña desde el que el cliente entró a la Tienda por primera vez. Luego atribuimos conversiones a dicho identificador de campaña para todas las adquisiciones en un plazo de 24 horas. Por este motivo, puedes ver un número de total de conversiones mayor que el total de conversiones de los identificadores de campañas. También es posible que tengas conversiones o conversiones de complementos que tengan cero vistas de página.

> [!NOTE]
> También puedes recuperar mediante programación estos datos mediante el método [obtener conversiones de aplicaciones por canal](../monetize/get-app-conversions-by-channel.md) en nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="app-page-views-and-conversions-by-campaign-id"></a>Vistas de página y conversiones de la aplicación por identificador de campaña

El gráfico **Vistas de página y conversiones de la aplicación por identificador de campaña** te permite realizar un seguimiento de conversiones y vistas de página, tal y como se ha descrito anteriormente, para cada una de tus [campañas de promoción personalizadas](create-a-custom-app-promotion-campaign.md). Se muestran los identificadores de campañas principales y puedes usar los filtros para excluir o incluir identificadores de campañas específicos.

## <a name="total-campaign-conversions"></a>Total de conversiones de campaña

En el gráfico **Total de conversiones de campaña** se muestra el número total de conversiones de la aplicación y de sus complementos desde todas las campañas personalizadas durante el período de tiempo seleccionado.





 

 
