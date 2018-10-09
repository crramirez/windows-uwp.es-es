---
author: jnHs
Description: Here’s some important info you’ll need to ensure that you receive payment for your apps, in-app products (IAPs), and advertising earnings.
title: Recibir pagos
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.author: wdg-dev-content
ms.date: 02/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, pagos, ventas de aplicaciones, ganancias por la aplicación, pago, tarifa de la store, suspensión de pago, porcentaje
ms.localizationpriority: medium
ms.openlocfilehash: 0c128bedd1c889f4c2dcf0565c7c10575eb75013
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "4465952"
---
# <a name="getting-paid"></a>Recibir pagos
Aquí encontrarás información importante que necesitarás para asegurarte de que recibes tanto los pagos de tus aplicaciones y complementos, como las ganancias de publicidad.

> [!IMPORTANT]
> Antes de que recibas dinero por las ventas de aplicaciones en la Microsoft Store, debes [configurar tu cuenta de pago y rellenar los formularios fiscales necesarios](setting-up-your-payout-account-and-tax-forms.md).

## <a name="store-fee"></a>Comisión de la Tienda

Cuando te [registras para obtener una cuenta de desarrollador](http://go.microsoft.com/fwlink/p/?LinkID=615100), aceptas el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Este acuerdo explica la relación que tienes con Microsoft en cuanto a la venta de aplicaciones en la Microsoft Store, incluida la comisión de la Store que Microsoft cobra por cada venta realizada.

En la mayoría de los casos, la comisión de la Tienda es del 30%. Las comisiones están establecidas oficialmente en el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Revisa siempre ese documento si tienes alguna duda.

La comisión de la Store se aplica a todas las ventas de aplicaciones realizadas a través de la Microsoft Store, incluidos los complementos.


## <a name="price-tiers"></a>Franjas de precios

La franja de precios que selecciones establece el [precio de ventas](set-and-schedule-app-pricing.md#base-price) en todos los países donde eliges distribuir tu aplicación. También puedes usar características de precios adicionales, tales como [elegir precios diferentes para distintos mercados](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) o [poner tu aplicación en oferta](put-apps-and-add-ons-on-sale.md).

Puedes ofrecer tu aplicación de forma gratuita o elegir un precio que los clientes deben pagar para adquirir tu aplicación. Las franjas de precios empiezan por 0,99USD, con incrementos adicionales (1,09USD, 1,19USD, etc.). Estos incrementos entre franjas de precios aumentan a medida que el precio sube.

> [!NOTE] 
> Estas franjas de precios también se aplican a cualquier complemento que ofrezcas desde la aplicación.

Cada franja de precios tiene un valor correspondiente en cada una de las divisas que ofrece la Tienda. Usamos estos valores para ayudarte a vender tus aplicaciones a un punto de precio comparable mundial. No obstante, debido a cambios en las tasas de cambio extranjeras, el importe de ventas exacto puede variar levemente de una moneda a otra.

También tienes la opción de especificar un precio de forma libre que elijas en la moneda local de un mercado específico. Al hacerlo, no se ajustará el precio (incluso si cambian las tasas de conversión) a menos que envíes una actualización con un precio nuevo. 

Recuerda que el precio que seleccionaste puede incluir el impuesto sobre el valor añadido o las ventas que tus clientes deben pagar. Para más información, consulta [detalles de impuestos para aplicaciones de pago](tax-details-for-paid-apps.md) para obtener más información.


## <a name="payout-reporting"></a>Informes de pago

Puedes obtener acceso a los detalles sobre la información de pago y descargar los informes en el **resumen de pago** del panel del Centro de desarrollo de Windows. Para obtener más información sobre la información que se muestra aquí, y sobre cómo que clasificamos el dinero que ganas, consulta [Resumen de pago](payout-summary.md).


## <a name="payout-timeframe"></a>Período de pago

Los pagos se realizan de forma mensual (siempre que hayas alcanzado el umbral de pago aplicable y no hayas seleccionado el pago en suspensión tal como se describe a continuación). Por lo general, enviaremos cualquier pago pendiente a día 15 de un mes determinado. Recuerda que los pagos suelen tardar entre 3 y 10 días laborables adicionales en acreditarse en tu cuenta de pago. Para obtener más información, consulta [Umbrales, métodos y plazos de pago](payment-thresholds-methods-and-timeframes.md).


##  <a name="payout-hold-status"></a>Estado de suspensión de pago

De manera predeterminada, te enviaremos pagos mensualmente como se describe más arriba. Sin embargo, tienes la opción de suspender los pagos, lo que evitará que te enviemos pagos a tu cuenta. Si optas por suspender los pagos, continuaremos registrando los ingresos que obtengas y proporcionándote detalles en tu **Resumen de pagos**. Sin embargo, no enviaremos los pagos a tu cuenta hasta que quitas la suspensión. 

Para seleccionar la opción de pagos en suspensión, ve a **Configuración de la cuenta**. En **Detalles financieros**, en la sección **Estado de suspensión de pagos**, cambia el control deslizante por **Activado**. Puedes cambiar el estado de suspensión de pagos en cualquier momento, pero ten en cuenta que tu decisión afectará al pago mensual siguiente. Por ejemplo, si quieres suspender el pago de abril, asegúrate de que estableces el estado de suspensión de pagos en **Activado** antes de final de marzo.

Una vez hayas establecido el estado de suspensión de pago en **Activado**, todos los pagos estarán en suspensión hasta que cambies el control deslizante por **Desactivado**. Al hacerlo, se te incluirá durante el próximo ciclo de pagos mensual (siempre que se hayas cumplido los umbrales de pago aplicables). Por ejemplo, si has tenido los pagos en suspensión, pero te gustaría que se generara un pago en junio, asegúrate de definir el estado de suspensión del pago en **Desactivado** antes de final de mayo.

> [!NOTE]
> La selección del **Estado de suspensión de pago** se aplica a **todas** las fuentes de ingresos que se pagan a través del Centro de desarrollo de Windows (Microsoft Store, publicidad, AzureMarketplace, etc.). No puedes seleccionar diferentes estados de suspensión para cada fuente de ingresos.


 

 




