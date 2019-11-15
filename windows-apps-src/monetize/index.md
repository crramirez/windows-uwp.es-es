---
ms.assetid: 4e8cc0c0-b14c-472c-9e1c-4601d10289d2
description: Windows SDK, el SDK de Microsoft Advertising, Microsoft Store Services SDK y Microsoft Store proporcionan muchas características que permiten ganar más dinero con las aplicaciones y conseguir clientes gracias a la interacción con los usuarios.
title: Monetización, interacción y servicios de Store
ms.date: 11/29/2017
ms.topic: article
keywords: windows 10, uwp, monetize, engage, promote, Store services
ms.localizationpriority: medium
ms.openlocfilehash: 460179f7f57e17f78fdb3fd3bd289e761a8a7b4f
ms.sourcegitcommit: 2dba9b4e81151d14ca90d36341274a3b59926197
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74057460"
---
# <a name="monetization-engagement-and-store-services"></a>Monetización, interacción y servicios de Store

Windows SDK, el SDK de Microsoft Advertising, Microsoft Store Services SDK y Microsoft Store proporcionan características que permiten ganar más dinero con las aplicaciones y conseguir clientes gracias a la interacción con los usuarios. Los temas de esta sección te muestran cómo compilar estas funciones en tu aplicación.

Para información detallada sobre las cuotas de Microsoft Store y cómo recibir el pago del dinero generado por tu aplicación, consulta [Proceso de pago](../publish/getting-paid-apps.md).

## <a name="choose-a-pricing-model"></a>Elegir un modelo de precios

:::row:::
    :::column:::
        ![Cobra un precio por la aplicación](images/pricing-charge-price.png)
    :::column-end:::
    :::column span="2":::
**Cobra un precio por la aplicación**

Puedes cobrar un precio por adelantado por tu aplicación. Permitimos una amplia gama de niveles de precios, incluida la opción de establecer precios según mercado. Incluso puedes programar una oferta para reducir el precio de la aplicación durante un período de tiempo limitado.

[Establecer precios de las aplicaciones](../publish/set-app-pricing-and-availability.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Pruebas gratuitas](images/pricing-free-trial.png)
    :::column-end:::
    :::column span="2":::
**Pruebas gratuitas**

Puedes ofrecer una versión de prueba gratuita de la aplicación para lograr que la prueben más clientes. Para que los clientes quieran comprar la versión completa, puedes limitar las funciones de la versión de prueba (por ejemplo, incluyendo solo el primer nivel de un juego), mostrar anuncios o especificar una versión de prueba de tiempo limitado.

[Ofrecer una prueba](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Compras desde la aplicación](images/pricing-in-app-purchases.png)
    :::column-end:::
    :::column span="2":::
**Compras desde la aplicación**

Ya cobres un precio por la aplicación o la ofrezcas de manera gratuita, puedes convertir las compras desde tu aplicación en un flujo de ingresos constantes. Usa las compras desde la aplicación para permitir a los clientes actualizar de una versión gratuita a otra de pago de la aplicación, o para ofrecer complementos duraderos o consumibles para la venta dentro de la aplicación.

[Usar las compras desde la aplicación](in-app-purchases-and-trials.md)
    :::column-end:::
:::row-end:::

## <a name="monetize-your-app-with-ads"></a>Monetizar tu aplicación con anuncios

:::row:::
    :::column:::
        ![Anuncios para cada contexto](images/monetize-ads-every-context.png)
    :::column-end:::
    :::column span="2":::
**Anuncios para cada contexto**

Permitimos una amplia variedad de experiencias de anuncios que se adaptan a la mayoría de las necesidades, incluidos anuncios de banners, anuncios intersticiales (banner y vídeo), anuncios en vídeo lineales y anuncios nativos. Nuestra plataforma es compatible con los estándares OpenRTB, VAST 2.x, MRAID 2 y VPAID 3, y con MOAT e IAS.

[Explorar las opciones de anuncios](../publish/create-an-ad-campaign-for-your-app.md)
[instalar el SDK de anuncios](https://aka.ms/ads-sdk-uwp)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Servicio de mediación de anuncios](images/monetize-ad-mediation-service.png)
    :::column-end:::
    :::column span="2":::
**Servicio de mediación de anuncios**

Maximiza los ingresos publicitarios de tus aplicaciones usando el servicio de mediación de anuncios de Microsoft para ofrecer anuncios de múltiples redes de anuncios populares. Puedes configurar los ajustes de mediación en el Centro de partners, sin necesidad de tocar una sola línea de código. Si dejas que configuremos la mediación por ti, nuestros algoritmos de aprendizaje automático te ayudarán a maximizar los ingresos por anuncios en los mercados que tu aplicación admita.

[Usar el servicio de anuncios](https://aka.ms/admediationblog)
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
        ![Análisis](images/monetize-analytics-pie-chart.png)
    :::column-end:::
    :::column span="2":::
**Análisis**

Los informes de análisis detallados te permiten ver el rendimiento de los anuncios en las aplicaciones, facilitándote la información necesaria para maximizar los ingresos por anuncios. También proporcionamos una API de RESTful que puedes usar para obtener estos datos programáticamente.

[Revisar el rendimiento](../publish/advertising-performance-report.md)
    :::column-end:::
:::row-end:::

## <a name="other-monetization-opportunities"></a>Otras oportunidades de monetización

![Otras oportunidades de monetización](images/monetize-other-opportunities.png)

¿Buscas otras formas de aumentar la rentabilidad? Ten en cuenta estas opciones.

 Tema                | Descripción                 |
|--------------------|-----------------------------|
| [Programa de afiliados de Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=617665) | Consigue comisiones vinculando productos de Microsoft desde la aplicación, el blog, la página web u otras comunicaciones. Puedes vincular productos a aplicaciones, juegos, música, películas, hardware, accesorios y otros productos que se vendan en Microsoft Store.
| [Experimentación A/B](https://go.microsoft.com/fwlink/p/?LinkId=722784) | Ejecuta pruebas A/B en tus aplicaciones para medir la eficacia de los cambios de características en algunos clientes antes de habilitar los cambios de forma generalizada.
| [Conectar con clientes con Microsoft Store Services SDK](microsoft-store-services-sdk.md) | Microsoft Store Services SDK proporciona bibliotecas y herramientas que puedes usar para agregar características a las aplicaciones que te ayudarán a atraer a los clientes. Estas características incluyen notificaciones dirigidas, pruebas A/B e inicio del Centro de opiniones desde la aplicación.
| [Iniciar el Centro de opiniones desde la aplicación](launch-feedback-hub-from-your-app.md) | Agrega código a tus aplicaciones para UWP para dirigir a los clientes de Windows 10 al Centro de opiniones, donde pueden enviar sus problemas y sugerencias, así como votar. A continuación, administra estas opiniones en el [Informe de comentarios](../publish/feedback-report.md) del Centro de partners. Esta característica requiere Microsoft Store Services SDK. 
| [Configurar la aplicación para notificaciones de inserción dirigidas](configure-your-app-to-receive-dev-center-notifications.md) | Registra un canal de notificaciones para que tu aplicación para UWP pueda recibir [notificaciones de inserción del Centro de partners](../publish/send-push-notifications-to-your-apps-customers.md) y realizar un seguimiento de la tasa de inicios de la aplicación derivados de estas notificaciones. Esta característica requiere Microsoft Store Services SDK.
| [Registrar eventos personalizados para el Centro de partners](log-custom-events-for-dev-center.md) | Registra eventos personalizados de tu aplicación para UWP y examina los eventos en el [informe de uso](../publish/usage-report.md) del Centro de partners. Esta característica requiere Microsoft Store Services SDK.
| [Solicitar valoraciones y opiniones](request-ratings-and-reviews.md) | Anima a los clientes a valorar la aplicación u opinar sobre ella mostrando mediante programación una interfaz de usuario de valoración y opinión.
| [Servicios de Microsoft Store](using-windows-store-services.md) | Obtén información sobre cómo usar las API de RESTful para automatizar envíos a la Tienda, obtener acceso a datos de análisis de las aplicaciones y automatizar otras tareas relacionadas con la Tienda.
| [Agregar características de prueba comercial (RDX) a la aplicación](retail-demo-experience.md) | Incluye un modo de prueba comercial en la aplicación de Windows para que los clientes que prueben equipos PC y dispositivos en la zona de ventas puedan comenzar de inmediato.

## <a name="monetization-analytics"></a>Análisis de rentabilidad

![Análisis de rentabilidad](images/monetize-analytics.png)

Mantente al día del rendimiento de la aplicación en Store usando estos informes.

- [Resumen de pago](../publish/payout-summary.md)
- [Informe Adquisiciones](../publish/acquisitions-report.md)
- [Informe Adquisiciones de complementos](../publish/add-on-acquisitions-report.md)
- [Informe de rendimiento de publicidad](../publish/advertising-performance-report.md)
- [Obtener datos de análisis mediante nuestra API de REST](access-analytics-data-using-windows-store-services.md)
- [Crear segmentos de clientes](../publish/create-customer-segments.md)
- [Informe Comentarios](../publish/feedback-report.md)
- [Informe de uso](../publish/usage-report.md)