---
Description: El informe de adquisiciones de complemento en el centro de partners le permite ver cómo muchos complementos que ha vendido, junto con datos demográficos y los detalles de la plataforma.
title: Informe de adquisiciones de complementos
ms.assetid: F2DF9188-0A98-4AC3-81C0-3E2C37B15582
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, ventas de complementos, adquisiciones de complementos, ventas de iap, productos desde la aplicación, IAP, complementos
ms.localizationpriority: medium
ms.openlocfilehash: 8027276779dac59f0745dd8053ee73cf1615e630
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625840"
---
# <a name="add-on-acquisitions-report"></a>Informe de adquisiciones de complementos


El **adquisiciones de complemento** notificar en [centro de partners](https://partner.microsoft.com/dashboard) permite ver cómo muchos complementos que ha vendido, junto con datos demográficos y los detalles de la plataforma y muestra información de conversión para los clientes de Windows 10 ( incluido Xbox). También puede ver cerca de los datos de adquisición en tiempo real para el último período o setenta y dos horas.

Puede ver estos datos en el centro de partners, o [descargar el informe](download-analytic-reports.md) ver sin conexión. Como alternativa, puedes recuperar mediante programación estos datos mediante el método [obtener los datos de las adquisiciones de complementos](../monetize/get-in-app-acquisitions.md) en la [API de REST de análisis de la Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md).

En este informe, una adquisición de complemento significa que un cliente te ha comprado un complemento (o lo ha adquirido sin tener que pagar, si lo ofrecías de forma gratuita). Varias compras del mismo complemento consumible realizadas por el mismo cliente se contabilizan como adquisiciones de complementos independientes.

> [!IMPORTANT]
> El **adquisiciones de complemento** informe no incluye datos sobre reembolsos, reversiones, contracargos, etcetera. Para calcular sus ganancias de la aplicación, visite [resumen de pagos](payout-summary.md). En la sección **Reservado**, haz clic en el vínculo **Descargar transacciones reservadas**.


## <a name="apply-filters"></a>Aplicar filtros

Cerca de la parte superior de la página, puedes seleccionar el período de tiempo durante el que quieres mostrar los datos. La selección predeterminada es **30D** (30 días), pero también puedes mostrar los datos durante 3, 6 o 12 meses o durante un intervalo de fechas personalizado que especifiques. También puede seleccionar **1H** o **72H** para mostrar los datos de adquisición casi en tiempo real para cada una hora o 72 horas; estos períodos de tiempo solo se aplican a la **complemento diariamente** ficha de la **adquisiciones de complemento** gráfico y a la **adquisiciones** pestaña de la **mercados** gráfico. 

También puedes expandir la opción **Filtros** para filtrar todos los datos de esta página por complementos específicos, por mercado o por tipo de dispositivo.

-   **Complemento**: El filtro predeterminado es **todos los complementos**, pero puede limitar los datos a uno o varios de los complementos de la aplicación.
-   **Mercado**: El filtro predeterminado es **todos los mercados**, pero puede limitar los datos a las adquisiciones en uno o varios mercados.
-   **Tipo de dispositivo**: El valor predeterminado es **todos los dispositivos**. Si quieres mostrar datos de adquisiciones de un determinado tipo de dispositivo únicamente (como PC, consola o tableta), aquí puedes elegir uno específico.

La información de todos los gráficos que aparecen a continuación reflejará el intervalo de fechas y los filtros que hayas seleccionado. Algunas secciones también te permiten aplicar filtros adicionales.


## <a name="add-on-acquisitions"></a>Adquisiciones de complementos

El gráfico **Adquisiciones de complementos** muestra el número de adquisiciones diarias o semanales de tus complementos durante el período de tiempo seleccionado. (Si usas **Aplicar filtros** para mostrar los datos durante más tiempo, los datos de adquisición se agruparán por semanas).

También puedes ver el número de adquisiciones del ciclo de vida de la aplicación seleccionando **Complemento acumulativo**. Muestra el total acumulado de todas las adquisiciones desde que la aplicación se publicó por primera vez.

También puedes filtrar los resultados por si la adquisición del complemento se originó desde el cliente, la Tienda web o la versión del sistema operativo.


## <a name="customer-demographic"></a>Grupo demográfico de clientes

En el gráfico **Grupo demográfico de clientes** se muestra información demográfica sobre las personas que han adquirido los complementos. Puedes ver cuántas adquisiciones (durante el período de tiempo seleccionado) han realizado las personas de un determinado grupo de edad divididas por sexo.

> [!NOTE]
> Algunos clientes han optado por no compartir esta información. Si no podemos determinar el grupo de edad o el sexo, la adquisición se clasifica como **Desconocida**.


## <a name="markets"></a>Mercados

En el gráfico **Mercados** se muestra el número total de adquisiciones de complementos durante el período de tiempo seleccionado en cada mercado en el que tu aplicación está disponible. 

Puedes ver estos datos en forma de **Mapa** visual o cambiar la configuración para verlos en forma de **Tabla**. La tabla mostrará cinco mercados a la vez, ordenados alfabéticamente o por mayor o menor número de adquisiciones o instalaciones. También puedes descargar los datos para ver la información de todos los mercados de forma conjunta.


## <a name="add-on-page-views-and-conversions-by-campaign-id"></a>Vistas de página de complementos y conversiones de la aplicación por identificador de campaña

En el gráfico **Vistas de página de complementos y conversiones de la aplicación por identificador de campaña** se muestra el número total de conversiones de complementos (adquisiciones) por identificador de campaña durante el período de tiempo seleccionado, lo que te permite realizar un seguimiento de las conversiones y vistas de página de los clientes de Windows 10 (incluido Xbox) para cada una de tus [campañas de promoción personalizadas](create-a-custom-app-promotion-campaign.md). En este gráfico se muestran solo las conversiones de complementos.

> [!NOTE]
> Los clientes pudieron llegar a la descripción de la aplicación haciendo clic en una campaña personalizada que no hayas creado tú. Marcamos cada vista de página dentro de una sesión con el identificador de campaña desde el que el cliente entró a la Tienda por primera vez. Luego atribuimos conversiones a dicho identificador de campaña para todas las adquisiciones en un plazo de 24 horas. Por este motivo, puedes ver un número de total de conversiones mayor que el total de conversiones de los identificadores de campañas. También es posible que tengas conversiones o conversiones de complementos que tengan cero vistas de página. 


## <a name="conversions-breakdown-by-campaign-id"></a>Desglose de conversiones por identificador de campaña

El gráfico **Desglose de conversiones por identificador de campaña** te permite realizar un seguimiento de conversiones y vistas de página de los clientes de Windows 10 para cada una de tus [campañas de promoción personalizadas](create-a-custom-app-promotion-campaign.md). Las conversiones de aplicaciones y complementos se muestran por identificador de campaña.

En este gráfico, una *vista de página* significa que un cliente ha visualizado la descripción de la Tienda de la aplicación. Una *conversión* significa que un cliente acaba de obtener una licencia para la aplicación o complemento (tanto si cobras dinero como si los ofreces gratis).

Ten en cuenta que estas vistas de página y números de conversión no son recuentos de clientes únicos. 


## <a name="top-add-ons"></a>Complementos principales

En el gráfico **Complementos principales** se muestra el número total de adquisiciones de cada uno de los complementos durante el período de tiempo seleccionado, de modo que puedes ver los complementos que son más populares. 



 

 
