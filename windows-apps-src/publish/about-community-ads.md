---
Description: Puedes promocionar tu aplicación de manera cruzada con las aplicaciones publicadas por otros desarrolladores. Esta función se denomina anuncios de la comunidad.
title: Acerca de los anuncios de la comunidad
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 48d63dae76113c70273f6ff2cba46348b1e24a10
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507019"
---
# <a name="about-community-ads"></a>Acerca de los anuncios de la comunidad

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Si la aplicación [muestra anuncios intersticiales en banner o banner](../monetize/display-ads-in-your-app.md), puede realizar promociones cruzadas de la aplicación con otros desarrolladores con aplicaciones en el Microsoft Store de forma gratuita. Esta función se denomina *anuncios de la comunidad*.  

Así es cómo funciona este programa:

* Una vez que haya optado por los anuncios de la comunidad, como se describe a continuación, puede [crear una campaña de anuncios de la comunidad gratuita](create-an-ad-campaign-for-your-app.md). La aplicación compartirá el espacio de publicidad promocional con otros desarrolladores que también participen en los anuncios de la comunidad. Tu aplicación mostrará anuncios de las aplicaciones publicadas por otros desarrolladores que participan en los anuncios de la comunidad y sus aplicaciones mostrarán anuncios de tu aplicación.
* Puedes conseguir créditos por el espacio de anuncios promocionales en otras aplicaciones si muestras anuncios de la comunidad en tu aplicación. Los créditos se calculan según el siguiente proceso:
  * Para cada país o región en la que haya una aplicación que ofrece anuncios de la comunidad disponible, el valor de eCPM (costo eficaz por mil impresiones) a la tasa de mercado actual del país o la región se multiplica por el número de solicitudes de anuncios de la comunidad que realiza tu aplicación en ese país o región. Este valor corresponde a los créditos que has obtenido por tu aplicación en ese país o región.
  * El total de créditos obtenidos durante un período de tiempo determinado equivale a la suma de todos los créditos obtenidos en cada país o región por cada una de tus aplicaciones que ofrecen anuncios de la comunidad.
* Los créditos se dividen de forma equitativa entre todas las campañas de anuncios de la comunidad activas y se convierten en impresiones de anuncios para tu aplicación en función de los valores de eCPM a la tasa del mercado actual de los países o regiones a los que están destinadas tus campañas de anuncios de la comunidad.
* Para hacer un seguimiento del rendimiento de los anuncios de la comunidad en tu aplicación, consulta el [Informe Rendimiento de la publicidad](advertising-performance-report.md).

### <a name="opt-in-to-community-ads"></a>Participar en anuncios de la comunidad

Antes de poder crear una campaña de ad de la comunidad para una de las aplicaciones, debes participar en la página **monetizar** &gt; **anuncios en la aplicación** del [centro de Partners](https://partner.microsoft.com/dashboard).

Para participar en anuncios de la comunidad para una aplicación para UWP:

1. Seleccione una unidad de anuncio que esté usando en la aplicación y desplácese hacia abajo hasta **configuración de mediación**.
2. Si está seleccionada la opción **permitir que Microsoft optimice la configuración** , los anuncios de la comunidad se habilitan automáticamente para la unidad de anuncio. En caso contrario, seleccione la configuración de línea de base o una configuración específica del mercado en el menú desplegable **destino** y, a continuación, active la casilla ADS de la **comunidad de Microsoft** en la lista **otras redes de ad** .

    > [!NOTE]
    > Puede usar los campos **peso** para especificar la relación de los anuncios que desea mostrar desde las redes de pago y otras redes de anuncios, incluidos los anuncios de la comunidad.

No tienes que volver a publicar la aplicación después de realizar las selecciones. Una vez que hayas participado, podrás seleccionar **Anuncio de la comunidad (gratuito)** como el tipo de campaña al [crear una campaña publicitaria](create-an-ad-campaign-for-your-app.md).

### <a name="related-topics"></a>Temas relacionados

* [Anuncios en aplicaciones](in-app-ads.md)
* [Crear una campaña de AD para la aplicación](create-an-ad-campaign-for-your-app.md)
