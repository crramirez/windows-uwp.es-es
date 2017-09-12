---
author: JnHs
Description: "Obtén análisis detallados de las aplicaciones de Windows en el panel de información o a través de otros métodos."
title: "Analizar el rendimiento de la aplicación"
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.author: wdg-dev-content
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, análisis, informes, panel, aplicaciones"
ms.openlocfilehash: 57e4a30258fa25411bb461cac56aa18d2f74981d
ms.sourcegitcommit: a93b1da07b386a682435de58a8129d7b4ee90c14
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2017
---
# <a name="analyze-app-performance"></a>Analizar el rendimiento de la aplicación

Puedes ver análisis detallados de las aplicaciones en el panel del Centro de desarrollo de Windows. Los gráficos y las estadísticas te permiten saber cómo están funcionando las aplicaciones, desde a cuántos clientes has llegado a cómo usan estos la aplicación y qué opinan de ella. También encontrarás métricas sobre el estado de la aplicación, el uso de los anuncios y mucho más.

Puedes visualizar informes de análisis en el panel de información o [descargar los informes que necesites](download-analytic-reports.md) para analizar los datos sin conexión. También te ofrecemos varias formas de [acceder a los datos de análisis sin usar el panel](#no-dashboard).

## <a name="view-key-analytics-for-all-your-apps"></a>Ver análisis clave de todas las aplicaciones

Para ver un análisis clave sobre tus aplicaciones más descargadas, amplía **Analizar** y selecciona **Información general**. De forma predeterminada, la página **Información general de análisis** muestra información acerca de las cinco aplicaciones que tienen más adquisiciones de ciclo de vida. Para elegir diferentes aplicaciones publicadas que mostrar, selecciona **Filtros**.

## <a name="view-individual-reports-for-each-app"></a>Ver informes individuales de cada aplicación

En esta sección encontrarás detalles sobre la información presentada en cada uno de los siguientes informes:

-   [Informe Adquisiciones](acquisitions-report.md)
-   [Informe Adquisiciones de complementos](add-on-acquisitions-report.md)
-   [Informe Uso](usage-report.md)
-   [Informe Estado](health-report.md)
-   [Informe Críticas](reviews-report.md)
-   [Informe Comentarios](feedback-report.md)
-   [Informe Rendimiento de la publicidad](advertising-performance-report.md)
-   [Informe Campaña publicitaria](promote-your-app-report.md)

> [!NOTE]
> Según las funciones y la implementación específicas de la aplicación, es posible que no veas datos en todos estos informes.

<span id="no-dashboard"/>
## <a name="access-analytics-data-without-using-the-dev-center-dashboard"></a>Acceder a los datos de análisis sin usar el panel de información del Centro de desarrollo

Además de los informes de análisis del panel de información, hay otras formas de acceder a los datos de análisis.

### <a name="windows-store-analytics-api"></a>API de análisis de la Tienda Windows

Usa la [API de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md) para recuperar datos de análisis de las aplicaciones mediante programación. Esta API de REST permite recuperar datos respecto a las adquisiciones de la aplicación y de los complementos, errores, clasificaciones de la aplicación y opiniones. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas procedentes de la aplicación o el servicio.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Paquete de contenido del Centro de desarrollo de Windows para Power BI

Usa el [paquete de contenido del Centro de desarrollo de Windows para Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar y supervisar los datos de análisis del Centro de desarrollo en Power BI. Power BI es un servicio de análisis de negocio en la nube que ofrece una vista única de los datos empresariales.

Usa los siguientes recursos para empezar a usar Power BI para acceder a los datos de análisis.

* [Registrarse en Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Aprender a usar Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Aprender a usar el paquete de contenido del Centro de desarrollo de Windows para Power BI para conectar con los datos de análisis](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Para conectar con el paquete de contenido del Centro de desarrollo de Windows para Power BI, te recomendamos que especifiques las credenciales de un directorio de Azure AD asociado con tu cuenta del Centro de desarrollo. Si usas las credenciales de tu cuenta de Microsoft, los datos de análisis de Power BI no se actualizarán automáticamente y tendrás que conectarte a Power BI para actualizarlos. Si la organización ya usa Office365 u otros servicios empresariales de Microsoft, ya tienes AzureAD. De lo contrario, puedes [obtenerlo de forma gratuita](http://go.microsoft.com/fwlink/p/?LinkId=703757). Para obtener más información acerca de cómo asociar tu cuenta del Centro de desarrollo con Azure AD, consulta [Administrar usuarios de la cuenta](manage-account-users.md).

### <a name="dev-center-app"></a>Aplicación del Centro de desarrollo

Instala la aplicación del [Centro de desarrollo](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) para ver rápidamente los detalles sobre el estado y el rendimiento de las aplicaciones en cualquier dispositivo Windows 10.

