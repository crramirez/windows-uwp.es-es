---
author: jnHs
Description: "Puedes ver análisis detallados de las aplicaciones en el panel del Centro de desarrollo de Windows."
title: "Análisis"
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.sourcegitcommit: dfaf348956b19746aa5332aeb7ad5cbc4b224e8c
ms.openlocfilehash: 8922a53da8b1bc97bef7faf49d0e412a26127188

---

# Análisis

Puedes ver análisis detallados de las aplicaciones en el panel del Centro de desarrollo de Windows. Los gráficos y las estadísticas te permiten saber cómo están funcionando las aplicaciones: p. ej., puedes saber a cuántos clientes has llegado o cómo usan estos la aplicación y qué opinan de ella. También encontrarás información sobre el estado de la aplicación, el uso de los anuncios y mucho más. Visualiza los informes en el panel o [descarga los informes que necesites](download-analytic-reports.md) para analizar los datos sin conexión. También te ofrecemos varias formas de [acceder a los datos de análisis sin usar el panel](#no-dashboard).

> 
            **Nota**
            &nbsp;&nbsp;Además de los informes de panel, puedes acceder mediante programación a algunos datos de análisis mediante la [API de REST de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md).

## Análisis para todas las aplicaciones


En la página de introducción del panel también se incluye una vista de resumen que recopila información sobre todas las aplicaciones. Las estadísticas que se muestran en la página de introducción varían en función de las aplicaciones.

Cuando [descargas informes analíticos](download-analytic-reports.md), también tienes la opción de descargar informes sobre todas tus aplicaciones. Ten en cuenta que tendrás que acceder a la página **Descargar informes** de la sección **Análisis** de una de las aplicaciones, pero también podrás descargar los datos de todas las demás.

## Informes disponibles para cada aplicación


En esta sección encontrarás detalles sobre la información presentada en cada uno de los siguientes informes:

-   [Informe de adquisiciones](acquisitions-report.md)
-   [Informe de estado](health-report.md)
-   [Informe de clasificaciones](ratings-report.md)
-   [Informe de críticas](reviews-report.md)
-   [Informe de comentarios](feedback-report.md)
-   [Informe de uso](usage-report.md)
-   [Informe de adquisiciones de IAP](iap-acquisitions-report.md)
-   [Informe de mediación de anuncios](ad-mediation-report.md)
-   [Informe de rendimiento de la publicidad](advertising-performance-report.md)
-   [Informe de anuncios de instalación de aplicaciones](app-install-ads-reports.md)
-   [Informe de canales y conversiones](channels-and-conversions-report.md)

> 
            **Nota**
            &nbsp;&nbsp;Según las funciones y la implementación específicas de la aplicación, es posible que no veas datos en todos estos informes.

## Filtros de página y de sección

Cada informe incluye filtros que puedes usar para explorar en profundidad los datos. En la parte superior de la página verás **Aplicar filtros**. Puedes usar estos filtros para limitar o ampliar el ámbito de todos los gráficos y la información de la página.

Dentro de cada gráfico determinado, también puedes ver los filtros de sección individuales. Estos filtros limitan los datos que se muestran solo para ese gráfico determinado.

Los filtros específicos varían según el informe. En los temas de esta sección se explica qué filtros están disponibles y se describen los otros datos de la página para cada informe.

<span id="no-dashboard"/>
## Acceder a los datos de análisis sin usar el panel del Centro de desarrollo

Además de los informes de análisis del panel, hay otras formas de acceder a los datos de análisis.

### API de análisis de la Tienda Windows

Usa la [API de análisis de la Tienda Windows](../monetize/access-analytics-data-using-windows-store-services.md) para recuperar datos de análisis para las aplicaciones mediante programación. Esta API de REST permite recuperar los datos de adquisiciones, errores, clasificaciones y opiniones de la aplicación de IAP y de la aplicación. Esta API usa Azure Active Directory (Azure AD) para autenticar las llamadas provenientes de la aplicación o el servicio.

### Paquete de contenido del Centro de desarrollo de Windows para Power BI

Usa el [paquete de contenido del Centro de desarrollo de Windows para Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) para explorar y supervisar los datos de análisis del Centro de desarrollo en Power BI. Power BI es un servicio de análisis de negocio en la nube que ofrece una vista única de los datos empresariales.

Usa los siguientes recursos para empezar a usar Power BI para acceder a los datos de análisis.

* [Registrarse en Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Aprender a usar Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Aprender a usar el paquete de contenido del Centro de desarrollo de Windows para Power BI para conectar con los datos de análisis](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> 
            **Nota**
            &nbsp;&nbsp;Para conectar con el paquete de contenido del Centro de desarrollo de Windows para Power BI, te recomendamos que especifiques las credenciales de un directorio de Azure AD asociado con tu cuenta del Centro de desarrollo. Si usas las credenciales de cuenta de Microsoft, los datos de análisis de Power BI no se actualizarán automáticamente y tendrás que iniciar sesión en Power BI para actualizarlos. Si la organización ya usa Office365 u otros servicios empresariales de Microsoft, ya tienes AzureAD. De lo contrario, puedes [obtenerlo de forma gratuita](http://go.microsoft.com/fwlink/p/?LinkId=703757). Para obtener más información acerca de cómo asociar tu cuenta del Centro de desarrollo con Azure AD, consulta [Administrar usuarios de la cuenta](manage-account-users.md).

### Aplicación del Centro de desarrollo

Instala la aplicación del [Centro de desarrollo](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws) para ver rápidamente los detalles sobre el estado y el rendimiento de las aplicaciones en cualquier dispositivo Windows 10. 



<!--HONumber=Jun16_HO4-->


