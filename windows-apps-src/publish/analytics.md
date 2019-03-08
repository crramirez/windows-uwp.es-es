---
Description: Obtenga análisis detallados para las aplicaciones de Windows, en el centro de partners, o mediante otros métodos.
title: Analizar el rendimiento de las aplicaciones
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, análisis, informes, panel, las aplicaciones, datos, las métricas
ms.localizationpriority: medium
ms.openlocfilehash: 6f76b1f897c345fb71beec8e37e592165922b2ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625450"
---
# <a name="analyze-app-performance"></a>Analizar el rendimiento de las aplicaciones

Puede ver análisis detallado de las aplicaciones en [centro de partners](https://partner.microsoft.com/dashboard). Los gráficos y las estadísticas te permiten saber cómo están funcionando las aplicaciones, desde a cuántos clientes has llegado a cómo usan estos la aplicación y qué opinan de ella. También encontrarás métricas sobre el estado de la aplicación, el uso de los anuncios y mucho más.

Puede ver los informes de análisis directamente en el centro de partners o [descargar los informes que necesita](download-analytic-reports.md) para analizar los datos sin conexión. También se proporcionan varias maneras para que pueda [acceder a los datos de análisis fuera del centro de partners](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Ver análisis clave de todas las aplicaciones

Para ver un análisis clave sobre tus aplicaciones más descargadas, amplía **Analizar** y selecciona **Información general**. De forma predeterminada, la página de resumen muestra información sobre las cinco aplicaciones que tienen más adquisiciones de ciclo de vida. Para elegir diferentes aplicaciones publicadas que mostrar, selecciona **Filtros**.

## <a name="view-individual-reports-for-each-app"></a>Ver informes individuales de cada aplicación

En esta sección encontrarás detalles sobre la información presentada en cada uno de los siguientes informes:

-   [Informe Adquisiciones](acquisitions-report.md)
-   [Informe Adquisiciones de complementos](add-on-acquisitions-report.md)
-   [Informe de uso](usage-report.md)
-   [Informe de mantenimiento](health-report.md)
-   [Informe de clasificación](ratings-report.md)
-   [Informe Críticas](reviews-report.md)
-   [Informe Comentarios](feedback-report.md)
-   [Informe de análisis de Xbox](xbox-analytics-report.md)
-   [Informe de perspectivas](insights-report.md)
-   [Informe de rendimiento de publicidad](advertising-performance-report.md)
-   [Informe Campaña publicitaria](promote-your-app-report.md)


> [!NOTE]
> Según las funciones y la implementación específicas de la aplicación, es posible que no veas datos en todos estos informes.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Acceder a los datos de análisis fuera del centro de partners

Además de ver informes en el centro de partners, puede tener acceso a análisis de aplicaciones de otras maneras.

### <a name="microsoft-store-analytics-api"></a>API de análisis de Microsoft Store

Usa la [API de análisis de Store](../monetize/access-analytics-data-using-windows-store-services.md) para recuperar datos de análisis de las aplicaciones mediante programación. Esta API de REST permite recuperar datos respecto a las adquisiciones de la aplicación y de los complementos, errores, clasificaciones de la aplicación y opiniones. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Paquete de contenido del Centro de desarrollo de Windows para Power BI

Use la [paquete de contenido del centro de desarrollo de Windows para Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar y supervisar los datos de análisis de centro de partners de Power BI. Power BI es un servicio de análisis de negocio en la nube que ofrece una vista única de los datos empresariales.

Usa los siguientes recursos para empezar a usar Power BI para acceder a los datos de análisis.

* [Registrarse en Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Aprenda a usar Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Obtenga información sobre cómo usar el paquete de contenido del centro de desarrollo de Windows para Power BI para conectarse a los datos de análisis](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Para conectar con el paquete de contenido del centro de desarrollo de Windows para Power BI, se recomienda que especifique las credenciales de un directorio de Azure AD que está asociado con la cuenta del centro de partners. Si usas las credenciales de tu cuenta de Microsoft, los datos de análisis de Power BI no se actualizarán automáticamente y tendrás que conectarte a Power BI para actualizarlos. Si la organización ya usa Office 365 u otros servicios empresariales de Microsoft, ya tienes Azure AD. De lo contrario, puedes [obtenerlo de forma gratuita](https://go.microsoft.com/fwlink/p/?LinkId=703757). Para obtener más información acerca de cómo configurar la asociación, vea [asociar Azure Active Directory con su cuenta de centro de partners](associate-azure-ad-with-dev-center.md).
