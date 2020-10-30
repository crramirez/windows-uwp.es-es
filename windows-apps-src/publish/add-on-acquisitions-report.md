---
description: El informe de adquisiciones de complementos del centro de Partners le permite ver el número de complementos que ha vendido, junto con detalles demográficos y de plataforma.
title: Informe de adquisiciones de complementos
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, ventas complementarias, adquisiciones de complementos, ventas de IAP, productos en la aplicación, IAPS, Complementos
ms.localizationpriority: medium
ms.openlocfilehash: f996fdcdff8664564379d99e529398e0f998e813
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033958"
---
# <a name="add-on-acquisitions-report"></a>Informe de adquisiciones de complementos


El informe de **adquisiciones de complementos** del [centro de Partners](https://partner.microsoft.com/dashboard) le permite ver cuántos complementos ha vendido, junto con detalles demográficos y de plataforma, y muestra información de conversión para los clientes de Windows 10 (incluida Xbox). También puede ver datos de adquisición casi en tiempo real durante la última hora o el período de 72 horas.

Puede ver estos datos en el centro de Partners o [descargar el informe](download-analytic-reports.md) para verlo sin conexión. Como alternativa, puede recuperar estos datos mediante programación con el método [Get Add-on Adquisitions](../monetize/get-in-app-acquisitions.md) de la API de [REST de Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md).

En este informe, una adquisición de complementos significa que un cliente ha adquirido un complemento de usted (o lo adquirió sin pagar, si lo ha ofrecido gratis). Varias compras del mismo complemento consumible realizadas por el mismo cliente se contabilizan como adquisiciones de complementos independientes.

> [!IMPORTANT]
> El informe de **adquisiciones del complemento** no incluye datos sobre reembolsos, inversiones, contracargos, etc. Para hacer una estimación de la aplicación, visite [Resumen de pago](payout-summary.md). En la sección **Reservado** , haz clic en el vínculo **Descargar transacciones reservadas** .


## <a name="apply-filters"></a>Aplicación de filtros

Cerca de la parte superior de la página, puede seleccionar el período de tiempo para el que desea mostrar los datos. La selección predeterminada es **30D** (30 días), pero puede optar por mostrar los datos de 3, 6 o 12 meses, o de un intervalo de datos personalizado que especifique. También puede seleccionar **1H** o **72H** para mostrar los datos de adquisición casi en tiempo real durante una hora o 72 horas; Estos períodos de tiempo solo se aplican a la pestaña de **Complementos diarios** del gráfico de **adquisiciones del complemento** y a la pestaña **adquisiciones** del gráfico **Markets** . 

También puede expandir **filtros** para filtrar todos los datos de esta página por complementos determinados, por mercado o por tipo de dispositivo.

-   **Complemento** : el filtro predeterminado es **todos los complementos** , pero puede limitar los datos a uno o varios de los complementos de la aplicación.
-   **Market** : el filtro predeterminado es **todos los mercados** , pero puede limitar los datos a las adquisiciones en uno o varios mercados.
-   **Tipo de dispositivo** : la opción predeterminada es **Todos los dispositivos** . Si desea mostrar los datos de las adquisiciones de un solo tipo de dispositivo determinado (como PC, consola o tableta), puede elegir uno específico aquí.

La información de todos los gráficos que se enumeran a continuación reflejará el intervalo de fechas y los filtros que haya seleccionado. Algunas secciones también permiten aplicar filtros adicionales.


## <a name="add-on-acquisitions"></a>Adquisiciones de complementos

El gráfico **Adquisiciones de complementos** muestra el número de adquisiciones diarias o semanales de tus complementos durante el período de tiempo seleccionado. (Si usa **aplicar filtros** para Mostrar datos durante más tiempo, los datos de adquisición se agruparán por semana).

También puede ver el número de duración de las adquisiciones de la aplicación seleccionando **complemento acumulativo** . Muestra el total acumulado de todas las adquisiciones desde que la aplicación se publicó por primera vez.

Opcionalmente, puede filtrar los resultados si la adquisición del complemento se originó en el cliente o en el almacén basado en web o en la versión del sistema operativo.


## <a name="customer-demographic"></a>Grupo demográfico de clientes

El gráfico de datos demográficos del **cliente** muestra información demográfica sobre las personas que han adquirido los complementos. Puedes ver cuántas adquisiciones (durante el período de tiempo seleccionado) han realizado las personas de un determinado grupo de edad divididas por sexo.

> [!NOTE]
> Algunos clientes han optado por no compartir esta información. Si no podemos determinar el grupo de edad o el sexo, la adquisición se clasifica como **Desconocida** .


## <a name="markets"></a>Mercados

El gráfico de **mercados** muestra el número total de adquisiciones de complementos durante el período de tiempo seleccionado para cada mercado en el que la aplicación está disponible. 

Puede ver estos datos en un formulario de **mapa** visual o alternar la configuración para verlo en un formulario de **tabla** . El formulario de tabla mostrará cinco mercados a la vez, ordenados alfabéticamente o por el número más alto o más bajo de adquisiciones o instalaciones. También puede descargar los datos para ver la información de todos los mercados juntos.


## <a name="add-on-page-views-and-conversions-by-campaign-id"></a>Vistas de página y conversiones de complementos por ID. de campaña

En el gráfico de las **vistas de página y las conversiones de los complementos por ID. de campaña** se muestra el número total de conversiones de complemento (adquisiciones) por identificador de campaña durante el período de tiempo seleccionado, lo que le ayuda a realizar un seguimiento de las conversiones y las vistas de página de los clientes de Windows 10 (incluida Xbox) para cada una de las [campañas](create-a-custom-app-promotion-campaign.md)de En este gráfico solo se muestran las conversiones de complementos.

> [!NOTE]
> Los clientes pueden llegar a la lista de la aplicación haciendo clic en una campaña personalizada que no ha creado. Se marcan todas las vistas de página dentro de una sesión con el identificador de campaña desde el que el cliente entró por primera vez en el almacén. Después, vamos a asignar las conversiones a ese identificador de campaña para todas las adquisiciones en un plazo de 24 horas. Por este motivo, es posible que vea un mayor número de conversiones totales que las conversiones totales de los identificadores de campaña, y puede que tenga conversiones o conversiones de complementos con cero vistas de página. 


## <a name="conversions-breakdown-by-campaign-id"></a>Desglose de conversiones por ID. de campaña

El gráfico **desglose de conversiones por ID. de campaña** le permite realizar un seguimiento de las conversiones y las vistas de página de los clientes de Windows 10 para cada una de las campañas de [promoción personalizadas](create-a-custom-app-promotion-campaign.md). Las conversiones de aplicaciones y complementos se muestran por identificador de campaña.

En este gráfico, una *vista de página* significa que un cliente ha visto la lista de la tienda de la aplicación. Una *conversión* significa que un cliente ha obtenido recientemente una licencia para la aplicación o el complemento (tanto si se le cobra dinero como si lo ha ofrecido gratis).

Tenga en cuenta que estas vistas de página y números de conversión no son recuentos de clientes únicos. 


## <a name="top-add-ons"></a>Complementos principales

En el gráfico de **Complementos superior** se muestra el número total de adquisiciones para cada uno de los complementos durante el período de tiempo seleccionado, de modo que puede ver cuáles son los complementos más populares. 



 

 
