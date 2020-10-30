---
description: Obtenga análisis detallados para las aplicaciones de Windows, en el centro de Partners o a través de otros métodos.
title: Analizar el rendimiento de las aplicaciones
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Analytics, informes, panel, aplicaciones, datos, métricas
ms.localizationpriority: medium
ms.openlocfilehash: 08ee9c5ddf729ac7a9026c36bc9fbddad0af5dc5
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032988"
---
# <a name="analyze-app-performance"></a>Analizar el rendimiento de las aplicaciones

Puede ver análisis detallados de las aplicaciones en el [centro de Partners](https://partner.microsoft.com/dashboard). Los gráficos y las estadísticas te permiten saber cómo están funcionando las aplicaciones, desde a cuántos clientes has llegado a cómo usan estos la aplicación y qué opinan de ella. También puede encontrar métricas sobre el estado de la aplicación, el uso de AD y mucho más.

Puede ver los informes analíticos directamente en el centro de Partners o [descargar los informes que necesita](download-analytic-reports.md) para analizar los datos sin conexión. También proporcionamos varias maneras de acceder a [los datos de análisis fuera del centro de Partners](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Visualización del análisis de claves para todas las aplicaciones

Para ver el análisis de claves de las aplicaciones más descargadas, expanda **analizar** y seleccione **información general** . De forma predeterminada, la página de información general muestra información sobre las cinco aplicaciones que tienen la mayoría de las adquisiciones de duración. Para elegir las distintas aplicaciones publicadas que se van a mostrar, seleccione **filtros** .

## <a name="view-individual-reports-for-each-app"></a>Visualización de informes individuales para cada aplicación

En esta sección encontrarás detalles sobre la información presentada en cada uno de los siguientes informes:

-   [Informe de adquisiciones](acquisitions-report.md)
-   [Informe de adquisiciones de complementos](add-on-acquisitions-report.md)
-   [Informe de uso](usage-report.md)
-   [Informe de estado](health-report.md)
-   [Informe Valoración](ratings-report.md)
-   [Informe Críticas](reviews-report.md)
-   [Informe de comentarios](feedback-report.md)
-   [Informe de análisis de Xbox](xbox-analytics-report.md)
-   [Informe de conocimientos](insights-report.md)
-   [Informe de rendimiento de la publicidad](advertising-performance-report.md)
-   [Informe de campaña publicitaria](/windows/uwp/publish/ad-campaign-report)


> [!NOTE]
> Según las funciones y la implementación específicas de la aplicación, es posible que no veas datos en todos estos informes.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Acceder a datos de análisis fuera del centro de Partners

Además de ver informes en el centro de Partners, puede acceder a análisis de aplicaciones de otras maneras.

### <a name="microsoft-store-analytics-api"></a>API de análisis de Microsoft Store

Use la [API de análisis](../monetize/access-analytics-data-using-windows-store-services.md) de la tienda para recuperar mediante programación los datos de análisis de las aplicaciones. Esta API de REST permite recuperar datos respecto a las adquisiciones de la aplicación y de los complementos, errores, clasificaciones de la aplicación y opiniones. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Paquete de contenido del Centro de desarrollo de Windows para Power BI

Use el [paquete de contenido del centro de desarrollo de Windows para Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar y supervisar los datos de análisis del centro de partners en Power BI. Power BI es un servicio de análisis de negocio en la nube que ofrece una vista única de los datos empresariales.

Usa los siguientes recursos para empezar a usar Power BI para acceder a los datos de análisis.

* [Suscribirse en Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Información sobre cómo usar Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Aprender a usar el paquete de contenido del Centro de desarrollo de Windows para Power BI para conectar con los datos de análisis](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Para conectarse al paquete de contenido del centro de desarrollo de Windows para Power BI, se recomienda especificar las credenciales de un directorio Azure AD que esté asociado a la cuenta del centro de Partners. Si usas las credenciales de tu cuenta de Microsoft, los datos de análisis de Power BI no se actualizarán automáticamente y tendrás que conectarte a Power BI para actualizarlos. Si su organización ya usa Microsoft 365 u otros servicios empresariales de Microsoft, ya tiene Azure AD. De lo contrario, puedes [obtenerlo de forma gratuita](https://account.azure.com/organization). Para obtener más información sobre la configuración de la asociación, consulte [asociar Azure Active Directory a la cuenta del centro de Partners](./associate-azure-ad-with-partner-center.md).
