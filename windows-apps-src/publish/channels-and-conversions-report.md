---
author: shawjohn
Description: "El informe Canales y conversiones del panel del Centro de desarrollo de Windows te permite ver cómo los clientes de Windows 10 han llegado hasta la descripción de tu aplicación."
title: Informe Canales y conversiones
ms.assetid: C359B9FB-A17B-4A8E-B8EE-19F2F98AA4FF
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, canales, conversiones, informe, análisis"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 60f639c6bad73273a6cc7f83cf65fdf321211ba1
ms.lasthandoff: 02/07/2017

---

# <a name="channels-and-conversions-report"></a>Informe Canales y conversiones


El informe **Canales y conversiones** del panel del Centro de desarrollo de Windows te permite ver cómo los clientes de Windows 10 han llegado hasta la descripción de tu aplicación. Te permite realizar un seguimiento de las [campañas de promoción personalizadas](create-a-custom-app-promotion-campaign.md) para la aplicación o sus complementos y ver cuántas de las visitas han generado nuevas adquisiciones. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión.

> **Importante** Este informe solo muestra los datos de vista y conversión de la página de los clientes de Windows 10.

 

En este informe, un *canal* hace referencia al método por el que un cliente llegó a la página de descripción de la aplicación (por ejemplo, explorando y buscando en la Tienda, un vínculo de un sitio web externo, un vínculo de una de las campañas personalizadas, etc.). Se incluyen los siguientes tipos de canal:

-   **Tráfico de la Tienda:** El cliente estaba explorando o buscando dentro de la Tienda cuando vio la descripción de la aplicación.
-   **Campaña personalizada:** El cliente ha seguido un vínculo que usa un [identificador de campaña personalizado](create-a-custom-app-promotion-campaign.md).
-   **Otro:** El cliente ha seguido un vínculo externo (sin ningún identificador de campaña personalizado) desde un sitio web hasta la descripción de la aplicación o el cliente ha seguido un vínculo desde un motor de búsqueda a la descripción de la aplicación.

Una *vista de página* significa que un cliente vio la página de descripción de la aplicación de la Tienda, bien a través de la Tienda web o desde la aplicación de la Tienda de Windows 10.

Una *conversión* significa que un cliente acaba de obtener una licencia para tu aplicación (tanto si cobras dinero como si la ofreces gratis) o para un complemento.

En este informe no se muestra una tasa de conversión dado que nuestras vistas de página y números de conversión no son recuentos de clientes únicos.

Los datos de conversión se proporcionan solo para las campañas personalizadas. Para otros tipos de canal, solo se incluyen en este informe los datos de la vista de página.

> **Nota**  Los clientes pueden llegar a la descripción de la aplicación haciendo clic en una campaña personalizada que no hayas creado tú. Para tener en cuenta esta posibilidad, marcamos cada vista de página dentro de una sesión con el identificador de campaña desde el que el usuario entra a la Tienda por primera vez. Luego se atribuye una conversión de la aplicación a ese identificador de campaña si se produce una adquisición de la aplicación en el plazo de 24 horas. Cuando ves el informe, es por esto que es posible que veas conversiones atribuidas a campañas con las que no estás familiarizado, por qué puedes ver un mayor número total de conversiones que en el desglose de conversiones, y por qué es posible que tengas conversiones o conversiones de complemento que tengan cero vistas de página. Puedes ver el desglose de conversiones por identificador de campaña para ver solo las conversiones atribuidas campañas creadas por ti a fin de evaluar su efectividad.


## <a name="apply-filters"></a>Aplicar filtros


Cerca de la parte superior de la página, puedes expandir **Aplicar filtros** para filtrar todos los datos de esta página por intervalo de fechas o por mercado.

-   **Fecha**: El filtro predeterminado es **Últimos 30 días**, pero puedes ampliarlo hasta **Últimos 12 meses**.
-   **Mercado**: El valor predeterminado es **Todos los mercados**. Puedes elegir un mercado específico si quieres que esta página solo muestre los detalles de clientes en ese mercado.
-   **Tipo de dispositivo**: El filtro predeterminado es **Todos los dispositivos**. Puedes elegir un tipo de dispositivo específico si quieres que esta página solo muestre los datos de clientes que usen ese tipo de dispositivo.

La información contenida en todos los gráficos enumerados a continuación reflejará el período de tiempo seleccionado en la sección **Aplicar filtros** y todos los filtros que hayas elegido aquí.

## <a name="app-page-views-and-conversions-by-channel"></a>Vistas de página de la aplicación y conversiones por canal


El gráfico de **vistas de página de la aplicación y conversiones por canal** muestra la frecuencia con la que se vio la página de descripción de la aplicación y cómo los clientes llegaron hasta ella. También muestra el número de conversiones de campañas personalizadas durante el período de tiempo seleccionado.

La pestaña **vistas de página** de este gráfico muestra el número de veces que se ha visto la página de descripción de la aplicación durante el período de tiempo seleccionado. Las vistas se agrupan según el tipo de canal por el que el cliente encontró la descripción de la aplicación.

La pestaña **conversiones** de este gráfico muestra el número de conversiones (nuevas adquisiciones) durante el período de tiempo seleccionado para los clientes que llegaron a la descripción de la aplicación a través de una campaña personalizada.

Para obtener información acerca de todas las adquisiciones de la aplicación, incluidas las que no se han producido a través de un vínculo de campaña personalizada y las de los clientes en otras versiones de sistema operativo, consulta el [informe de adquisiciones](acquisitions-report.md).

 

## <a name="total-campaign-conversions"></a>Total de conversiones de campaña


En el gráfico **Total de conversiones de campaña** se muestra el número total de conversiones de la aplicación y de sus complementos desde las campañas personalizadas durante el período seleccionado.

## <a name="app-page-views-and-conversions-by-campaign-id"></a>Vistas de página y conversiones de la aplicación por identificador de campaña


En el gráfico **Vistas de página y conversiones de la aplicación por identificador de campaña** se muestra el número de vistas de página y de conversiones para todos los [identificadores de campaña](create-a-custom-app-promotion-campaign.md) durante el período seleccionado.

##  <a name="add-on-conversions-by-campaign-id"></a>Conversiones de complementos por identificador de campaña


El gráfico **Conversiones de complemento por identificador de campaña** muestra el número de conversiones de complemento por identificador de campaña personalizado.

Cuando una instalación de aplicación se cuenta como conversión para una campaña personalizada, cualquier compra de un complemento en esa aplicación también se cuenta como conversiones de la misma campaña personalizada.

De manera predeterminada, el informe incluye cualquier complemento que tuviera una conversión proveniente de un vínculo con un identificador de campaña personalizado durante el período de tiempo seleccionado. Para ver los datos de un complemento concreto, selecciónalo desde **Filtros de sección**.

## <a name="conversions-breakdown-by-campaign-id"></a>Desglose de conversiones por identificador de campaña


En el gráfico **Desglose de conversiones** se muestran los detalles siguientes sobre las vistas de página y conversiones que se obtienen de las campañas personalizadas.

-   **Id.:** Muestra los identificadores de campaña específicos.
-   **Vistas de página:** Muestra el número de vistas de página marcadas con el identificador de campaña de la primera entrada del cliente en la Tienda.
-   **Conversiones de aplicaciones:** Muestra el número de conversiones de la aplicación obtenidas a partir de la campaña personalizada.
-   **Conversiones de complementos:** Muestra el número de conversiones de complementos obtenidas a partir de la campaña personalizada.


 

 

