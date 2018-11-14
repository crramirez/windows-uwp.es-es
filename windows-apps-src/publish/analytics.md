---
author: JnHs
Description: Get detailed analytics for your Windows apps, in Partner Center or via other methods.
title: Analizar el rendimiento de las aplicaciones
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, análisis, informes, panel, aplicaciones, datos, las métricas
ms.localizationpriority: medium
ms.openlocfilehash: 22d9a4d4b66091148bbb078abfb89237ab14ea87
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6199253"
---
# <a name="analyze-app-performance"></a>Analizar el rendimiento de las aplicaciones

Puedes ver análisis detallados de las aplicaciones en [El centro de partners](https://partner.microsoft.com/dashboard). Los gráficos y las estadísticas te permiten saber cómo están funcionando las aplicaciones, desde a cuántos clientes has llegado a cómo usan estos la aplicación y qué opinan de ella. También encontrarás métricas sobre el estado de la aplicación, el uso de los anuncios y mucho más.

Puedes ver informes analíticos directamente en el centro de partners o [descargar los informes que necesites](download-analytic-reports.md) para analizar los datos sin conexión. También proporcionamos varias maneras para que puedas tener [acceso a los datos de análisis fuera del centro de partners](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Ver análisis clave de todas las aplicaciones

Para ver un análisis clave sobre tus aplicaciones más descargadas, amplía **Analizar** y selecciona **Información general**. De forma predeterminada, la página de resumen muestra información sobre las cinco aplicaciones que tienen más adquisiciones de ciclo de vida. Para elegir que se muestren diferentes aplicaciones publicadas, selecciona **Filtros**.

## <a name="view-individual-reports-for-each-app"></a>Ver informes individuales de cada aplicación

En esta sección encontrarás detalles sobre la información presentada en cada uno de los siguientes informes:

-   [Informe Adquisiciones](acquisitions-report.md)
-   [Informe de adquisiciones de complementos](add-on-acquisitions-report.md)
-   [Informe de uso](usage-report.md)
-   [Informe Mantenimiento](health-report.md)
-   [Informe de clasificaciones](ratings-report.md)
-   [Informe de críticas](reviews-report.md)
-   [Informe de comentarios](feedback-report.md)
-   [Informe de Análisis de Xbox](xbox-analytics-report.md)
-   [Informe de información](insights-report.md)
-   [Informe de rendimiento de publicidad](advertising-performance-report.md)
-   [Informe Campaña publicitaria](promote-your-app-report.md)


> [!NOTE]
> Según las funciones y la implementación específicas de la aplicación, es posible que no veas datos en todos estos informes.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Acceder a los datos de análisis fuera del centro de partners

Además de ver los informes en el centro de partners, puedes acceder el análisis de la aplicación en un número de diferentes maneras.

### <a name="microsoft-store-analytics-api"></a>API de análisis de Microsoft Store

Usa la [API de análisis de Store](../monetize/access-analytics-data-using-windows-store-services.md) para recuperar datos de análisis de las aplicaciones mediante programación. Esta API de REST permite recuperar datos respecto a las adquisiciones de la aplicación y de los complementos, errores, clasificaciones de la aplicación y opiniones. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas procedentes de la aplicación o el servicio.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Paquete de contenido del Centro de desarrollo de Windows para Power BI

Usa el [paquete de contenido del centro de desarrollo de Windows para Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar y supervisar los datos de análisis del centro de partners en Power BI. Power BI es un servicio de análisis de negocio en la nube que ofrece una vista única de los datos empresariales.

Usa los siguientes recursos para empezar a usar Power BI para acceder a los datos de análisis.

* [Registrarse en Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Aprender a usar Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Aprender a usar el paquete de contenido del Centro de desarrollo de Windows para Power BI para conectar con los datos de análisis](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Para conectar con el paquete de contenido del centro de desarrollo de Windows para Power BI, te recomendamos que especifiques las credenciales de un directorio de Azure AD que está asociado con tu cuenta del centro de partners. Si usas las credenciales de tu cuenta de Microsoft, los datos de análisis de Power BI no se actualizarán automáticamente y tendrás que conectarte a Power BI para actualizarlos. Si la organización ya usa Office365 u otros servicios empresariales de Microsoft, ya tienes AzureAD. De lo contrario, puedes [obtenerlo de forma gratuita](http://go.microsoft.com/fwlink/p/?LinkId=703757). Para obtener más información acerca de cómo configurar la asociación, consulta [Asociar Azure Active Directory con tu cuenta del centro de partners](associate-azure-ad-with-dev-center.md).

### <a name="dev-center-app"></a>Aplicación del Centro de desarrollo

Instala la aplicación del [Centro de desarrollo](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) para ver rápidamente los detalles sobre el estado y el rendimiento de las aplicaciones en cualquier dispositivo Windows 10.

