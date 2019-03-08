---
Description: El informe de adquisiciones en el centro de partners le permite ver quién se adquiere e instala la aplicación, junto con datos demográficos y los detalles de la plataforma.
title: Informe de adquisiciones
ms.assetid: 21126362-F3CD-4006-AD3F-82FC88E3B862
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, adquisiciones, ventas de aplicaciones, descargas de aplicaciones, instalaciones, embudo, adquisición, conversiones, canal, vistas de página de la aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 33d5885c5161793807bf32f62ff2df4bab5b2c1d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653070"
---
# <a name="acquisitions-report"></a>Informe de adquisiciones


El **adquisiciones** notificar en [centro de partners](https://partner.microsoft.com/dashboard) le permite ver quién se adquiere e instala la aplicación, junto con datos demográficos y detalles de la plataforma y muestra información acerca de cómo los clientes en Windows 10 (incluidas Xbox) han llegado a la lista de la aplicación. También puede ver cerca de los datos de adquisición en tiempo real para el último período o setenta y dos horas. 

Puede ver estos datos en el centro de partners, o [descargar el informe](download-analytic-reports.md) ver sin conexión. Como alternativa, puedes recuperar mediante programación estos datos con nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

En este informe, una **adquisición** significa que un cliente nuevo ha obtenido una licencia para tu aplicación (tanto si cobras dinero como si la ofreces gratis). **Instalar** hace referencia a que la aplicación se está instalando en un dispositivo Windows 10.

> [!IMPORTANT]
> El **adquisiciones** informe no incluye datos sobre reembolsos, reversiones, contracargos, etcetera. Para calcular sus ganancias de la aplicación, visite [resumen de pagos](payout-summary.md). En la sección **Reservado**, haz clic en el vínculo **Descargar transacciones reservadas**.
>
> A excepción de los datos de la vista de página (tal y como se describe a continuación), este informe no incluye datos relacionados con los clientes que adquieran una aplicación sin iniciar sesión en una cuenta de Microsoft.


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **30D** (30 días), pero también puedes mostrar los datos durante 3, 6 o 12 meses o durante un intervalo de fechas personalizado que especifiques. Casi en tiempo real, los datos se mostrarán todas las opciones (excepto en **aplicación acumulativa** datos). El **1H** y **72H** períodos de tiempo solo se aplican a la **aplicación diariamente** pestaña de la **adquisiciones** gráfico y a la  **Adquisiciones** pestaña de la **mercados** gráfico. 

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página por mercado o por tipo de dispositivo.

-   **Mercado**: El filtro predeterminado es **todos los mercados**, pero puede limitar los datos a las adquisiciones en uno o varios mercados.
-   **Tipo de dispositivo**: El valor predeterminado es **todos los dispositivos**. Si quieres mostrar datos de adquisiciones de un determinado tipo de dispositivo únicamente (como PC, consola o tableta), aquí puedes elegir uno específico.

La información de todos los gráficos que aparecen a continuación reflejará el intervalo de fechas y los filtros que hayas seleccionado. Algunas secciones también te permiten aplicar filtros adicionales.


## <a name="acquisitions"></a>Adquisiciones

En el gráfico **Adquisiciones** se muestra el número de adquisiciones diarias o semanales (un nuevo cliente que obtiene una licencia para tu aplicación) durante el período de tiempo seleccionado. (Si usas **Aplicar filtros** para mostrar los datos durante más tiempo, los datos de adquisición se agruparán por semanas). Solo las adquisiciones realizadas por los clientes que han iniciado sesión con una cuenta válida de Microsoft se incluyen en este gráfico. 

De forma predeterminada, se muestran los **aplicación diariamente** vista, que incluye cerca de datos en tiempo real. También puedes ver el número de adquisiciones del ciclo de vida de la aplicación seleccionando **Aplicación acumulativa**. Muestra el total acumulado de todas las adquisiciones desde que la aplicación se publicó por primera vez.

**Ventas brutas** para la aplicación (desde octubre de 2016 - presente) también están disponibles en este gráfico, que muestra la cantidad total acumulada de ventas de la aplicación (en dólares estadounidenses). Tenga en cuenta que esta cantidad no tiene en cuenta para los reembolsos, reversiones, anulación, etcetera.

También puedes filtrar los resultados por si la adquisición se originó desde el cliente, la Tienda web o la versión del sistema operativo.

> [!NOTE]
> También puedes recuperar mediante programación estos datos mediante el método [obtener los datos de las adquisiciones de la aplicación](../monetize/get-app-acquisitions.md) en nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

En el **aplicación diariamente** vista, cuando el **d. 30** está seleccionado el período de tiempo, puede ver los marcadores de círculo. Estos representan un aumento significativo o disminuyen en un valor determinado que creemos que desea conocer. La fecha en la que aparece el círculo representa el final de la semana en el que se ha detectado un significativo aumento o disminución en comparación con la semana antes de que. Para ver más detalles sobre lo que ha cambiado, mantenga el mouse sobre el círculo.  

> [!TIP]
> Puede ver más información relacionada con cambios significativos durante los últimos 30 días en el [informe Insights](insights-report.md).

## <a name="installs"></a>Instalaciones

En el gráfico **Instalaciones** se muestra cuántas veces hemos detectado que los clientes han instalado correctamente la aplicación en dispositivos Windows 10 (incluidas las consolas Xbox One) durante el período de tiempo seleccionado. Se muestra el número total, junto con un gráfico que muestra las instalaciones por día o semana (en función de la duración que hayas seleccionado). También puedes filtrar los resultados por una determinada versión del paquete.

El total de instalaciones incluye lo siguiente:
-   **Se instala en varios dispositivos de Windows 10.** Por ejemplo, si el mismo cliente instala tu aplicación en dos equipos con Windows 10 y una Consola Xbox One, cuenta como tres instalaciones.
-   **Vuelve a instalar.** Por ejemplo, si un cliente instala la aplicación hoy, la desinstala mañana y la vuelve a instalar al mes siguiente, cuenta como dos instalaciones.

El total de instalaciones no incluye ni refleja lo siguiente:
-   **Se instala en dispositivos que no sean Windows 10.** Si la aplicación es compatible con las versiones anteriores del sistema operativo, tales como Windows 8.x o Windows Phone 8.x, no contamos ninguna instalación en dichos dispositivos.
-   **Desinstala.** Si un cliente desinstala la aplicación de su dispositivo, no la restamos al número total de instalaciones.
-   **Actualizaciones.** Por ejemplo, si un usuario instala la aplicación hoy e instala una actualización de la aplicación una semana después, solo se cuenta como una instalación.
-   **Preinstala.** Si un cliente compra un dispositivo que tenga la aplicación preinstalada, no lo contamos como una instalación.
-   **Instalaciones iniciadas por el sistema.** Si Windows instala la aplicación automáticamente por algún motivo, no lo contamos como una instalación.

> [!NOTE]
> También puedes recuperar mediante programación estos datos mediante el método de [obtención de las instalaciones de la aplicación](../monetize/get-app-installs.md) en nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="acquisition-funnel"></a>Embudo de adquisiciones

El **Embudo de adquisiciones** te muestra cuántos clientes realizaron cada paso del embudo, desde la visualización de la página de la Tienda hasta el uso de la aplicación, junto con la tasa de conversión. Estos datos pueden ayudarte a identificar las áreas donde podrías invertir más para aumentar tu adquisiciones, instalaciones o uso.

> [!IMPORTANT]
> El **Embudo de adquisiciones** solo muestra los datos de clientes de Windows 10 (incluida la Xbox) durante los últimos 90 días.

Los pasos del embudo son:

- **Vistas de página**: Este número representa el número total de vistas de lista de la aplicación Store, incluidas personas que no ha iniciado sesión con una cuenta de Microsoft. No se incluyen los datos de clientes que han decidido no proporcionar esta información a Microsoft.
- **Adquisiciones**: El número de los clientes nuevos que ha había obtenido una licencia a la aplicación (si inició sesión con su cuenta de Microsoft) en 48 horas de visualización de su lista de Store.
- **Instala**: El número de clientes que ha instalado la aplicación después de adquirir.
- **uso**: El número de clientes que utilizaron la aplicación tras la instalación.

También puedes filtrar los resultados por género o grupo de edad, así como por identificador de campaña personalizada.

> [!NOTE]
> También puedes recuperar mediante programación estos datos mediante el método [obtener datos de embudo de adquisiciones de aplicaciones](../monetize/get-acquisition-funnel-data.md) en nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="markets"></a>Mercados

En el gráfico **Mercados** se muestra el número total de adquisiciones o instalaciones durante el período de tiempo seleccionado en cada mercado en el que tu aplicación está disponible. Puedes optar por mostrar los datos de **Adquisiciones** o **Instalaciones**.

Puedes ver estos datos en forma de **Mapa** visual o cambiar la configuración para verlos en forma de **Tabla**. La tabla mostrará cinco mercados a la vez, ordenados alfabéticamente o por mayor o menor número de adquisiciones o instalaciones. También puedes descargar los datos para ver la información de todos los mercados de forma conjunta.


## <a name="customer-demographic"></a>Grupo demográfico de clientes

El gráfico **Grupo demográfico de clientes** muestra información demográfica sobre las personas que adquirieron la aplicación. Puedes ver cuántas adquisiciones (durante el período de tiempo seleccionado) han realizado las personas de un determinado grupo de edad divididas por sexo.

> [!NOTE]
> Algunos clientes han optado por no compartir esta información. Si no podemos determinar el grupo de edad o el sexo, la adquisición se clasifica como **Desconocida**.

 

## <a name="app-page-views-and-conversions-by-channel"></a>Vistas de página de la aplicación y conversiones por canal

El **conversiones por canal y vistas de página de aplicación** gráfico le permite ver cómo llegaron a los clientes en Windows 10 en la lista de la aplicación en el período de tiempo seleccionado.

En este gráfico, un *canal* hace referencia al método por el que un cliente llegó a la página de descripción de la aplicación (por ejemplo, explorando y buscando en la Tienda, un vínculo de un sitio web externo, un vínculo de una de las campañas personalizadas, etc.). Se incluyen los siguientes tipos de canal:

-   **Tráfico de Store:** El cliente se exploración o la búsqueda en el Store cuando ven el anuncio de la aplicación.
-   **Campaña personalizado:** El cliente seguido un vínculo que usa un [Id. de campaña personalizado](create-a-custom-app-promotion-campaign.md).
-   **Otro:** El cliente había seguido un vínculo externo (sin ningún Id. de campaña personalizados) desde un sitio Web para publicar la aplicación o el cliente había seguido un vínculo de un motor de búsqueda a la lista de la aplicación.

Un *vista de página* significa que un cliente ve la página de listado de la aplicación Store, mediante el Store basadas en web o desde el app Store en Windows 10. Se incluyen vistas por personas que no inician sesión con una cuenta de Microsoft. Algunos clientes han decidido no proporcionar esta información a Microsoft.

Una *conversión* significa que un cliente (que ha iniciado sesión con una cuenta de Microsoft) acaba de obtener una licencia para tu aplicación (tanto si cobras dinero como si la ofreces gratis).

Las vistas de página y números de conversión no son recuentos de clientes únicos. Para obtener información sobre la tasa de conversión, consulta el gráfico [Embudo de adquisiciones](#acquisition-funnel).

> [!NOTE]
> Los clientes pudieron llegar a la descripción de la aplicación haciendo clic en una campaña personalizada que no hayas creado tú. Marcamos cada vista de página dentro de una sesión con el identificador de campaña desde el que el cliente entró a la Tienda por primera vez. Luego atribuimos conversiones a dicho identificador de campaña para todas las adquisiciones en un plazo de 24 horas. Por este motivo, puedes ver un número de total de conversiones mayor que el total de conversiones de los identificadores de campañas. También es posible que tengas conversiones o conversiones de complementos que tengan cero vistas de página.

> [!NOTE]
> También puedes recuperar mediante programación estos datos mediante el método [obtener conversiones de aplicaciones por canal](../monetize/get-app-conversions-by-channel.md) en nuestra [API de REST de análisis](../monetize/access-analytics-data-using-windows-store-services.md).

## <a name="app-page-views-and-conversions-by-campaign-id"></a>Vistas de página de la aplicación y conversiones por identificador de campaña

El gráfico **Vistas de página y conversiones de la aplicación por identificador de campaña** te permite realizar un seguimiento de conversiones y vistas de página, tal y como se ha descrito anteriormente, para cada una de tus [campañas de promoción personalizadas](create-a-custom-app-promotion-campaign.md). Se muestran los identificadores de campañas principales y puedes usar los filtros para excluir o incluir identificadores de campañas específicos.

## <a name="total-campaign-conversions"></a>Total de conversiones de campaña

En el gráfico **Total de conversiones de campaña** se muestra el número total de conversiones de la aplicación y de sus complementos desde todas las campañas personalizadas durante el período de tiempo seleccionado.





 

 
