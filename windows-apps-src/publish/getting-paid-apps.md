---
Description: Obtenga información sobre cómo recibir los pagos de sus aplicaciones, complementos (productos en la aplicación) y beneficios publicitarios.
title: Recepción del pago
ms.assetid: 37D1EF45-C4A8-4849-8819-3D4A4898215C
ms.date: 05/29/2020
ms.topic: article
keywords: Windows 10, UWP, pagos, ventas de aplicaciones, continuaciones de aplicaciones, pagos, gastos de tienda, retención de pago, porcentaje
ms.localizationpriority: medium
ms.openlocfilehash: 5e2b67984c43d799f0f4e3a1c6662b57bdc6ae9d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167399"
---
# <a name="getting-paid"></a>Recepción del pago
Esta es una información importante sobre cómo recibir el pago de las aplicaciones, los complementos y los ingresos publicitarios.

> [!IMPORTANT]
> Antes de que pueda recibir dinero de las ventas de aplicaciones en el Microsoft Store, debe [configurar la cuenta de pago y rellenar los formularios fiscales necesarios](setting-up-your-payout-account-and-tax-forms.md).

> [!NOTE]
> Si busca soporte técnico en relación con los pagos, incluidos la configuración de cuentas de pago, los pagos que faltan, la puesta en espera de los pagos o cualquier otra cosa, póngase en contacto con el servicio de soporte técnico [aquí](https://developer.microsoft.com/windows/support).

## <a name="store-fee"></a>Honorarios de Store

Cuando te [registras para obtener una cuenta de desarrollador](https://developer.microsoft.com/store/register), aceptas el [Acuerdo para desarrolladores de aplicaciones](/legal/windows/agreements/app-developer-agreement). En este contrato se explica la relación entre usted y Microsoft como perteneciente a la venta de aplicaciones en el Microsoft Store, incluida la cuota de la tienda que Microsoft cobra por cada venta realizada.

Las comisiones están establecidas oficialmente en el [Acuerdo para desarrolladores de aplicaciones](/legal/windows/agreements/app-developer-agreement). Revise siempre ese documento si tiene alguna pregunta.

La cuota de la tienda se aplica a todas las ventas de aplicaciones recopiladas por el Microsoft Store, incluidos los complementos.


## <a name="price-tiers"></a>Franjas de precio

Los niveles de precio seleccionados establecen el [precio de venta](set-and-schedule-app-pricing.md#base-price) en todos los países en los que elige distribuir la aplicación. También puede usar otras características de precios, como  [elegir precios diferentes para distintos mercados](set-and-schedule-app-pricing.md#override-base-price-for-specific-markets) o [poner la aplicación en venta](put-apps-and-add-ons-on-sale.md).

Puedes ofrecer tu aplicación de forma gratuita o elegir un precio que los clientes deben pagar para adquirir tu aplicación. Las franjas de precio comienzan en los 0,99 USD, con incrementos adicionales (1,09 USD, 1,19 USD, etc.). Los incrementos entre las franjas de precio aumentan a medida que el precio es mayor.

> [!NOTE] 
> Estos niveles de precios también se aplican a los complementos que se ofrecen desde la aplicación.

Cada franja de precio tiene un valor correspondiente en cada una de las monedas que ofrece la Store. Usamos estos valores para ayudarte a vender tus aplicaciones a un punto de precio comparable mundial. Sin embargo, debido a los cambios en las tasas de cambio, el importe exacto de las ventas puede variar ligeramente de una moneda a otra. Las tasas de cambio se calculan mensualmente. Según el momento en que se realizó la transacción, se aplica la tasa de cambio adecuada. La tasa de cambio y el intervalo de fechas para el que se aplicó se indican en el informe de pago en las columnas exchangeRate y exchangeRateDate, respectivamente.

También tiene la opción de especificar el precio que quiera de forma libre en la moneda local de un mercado específico. Al hacerlo, el precio no se ajustará (incluso si cambian las tasas de conversión) a menos que envíe una actualización con un precio nuevo. 

Tenga en cuenta que el precio que seleccione puede incluir impuestos de ventas o al valor añadido que los clientes deben pagar. Para más información, consulta [detalles de impuestos para aplicaciones de pago](tax-details-for-paid-apps.md) para obtener más información.


## <a name="payout-reporting"></a>Informes de pago

Puede obtener acceso a los detalles sobre la información de pago y descargar informes en el **Resumen de pago** del [Centro de partners](https://partner.microsoft.com/dashboard). Para obtener más información sobre lo que se muestra aquí y cómo clasificamos el dinero que gana, consulte el [Resumen de pago](payout-summary.md).


## <a name="payout-timeframe"></a>Plazo de pago

Los pagos se realizan mensualmente (siempre que se cumpla el umbral de pago aplicable y no haya retenido el pago tal y como se describe a continuación). Normalmente enviaremos todos los pagos cuyo vencimiento sea en un mes específico antes del día 15 de ese mes. Tenga en cuenta que los pagos suelen tardar entre 3 y 10 días laborables adicionales para llegar a su cuenta de pago. Para obtener más información, consulte [Umbrales de pago, métodos e intervalos de tiempo](payment-thresholds-methods-and-timeframes.md).


##  <a name="payout-hold-status"></a>Estado de retención de pagos

De forma predeterminada, enviaremos pagos mensualmente, como se describió anteriormente. No obstante, tiene la opción de retener los pagos de un programa, lo que nos impediría enviar pagos a su cuenta. Si decide retener los pagos, se siguen registrando los ingresos que genera y se le proporcionan los detalles en el **Resumen de pagos**. Sin embargo, no enviaremos ningún pago a la cuenta hasta que quite la suspensión.

Para retener los pagos, vaya a la **Configuración del desarrollador**. En **Pago e impuestos**, en la sección **Asignación de perfiles fiscales y de pago**, busque el programa para el que quiere suspender los pagos. Haga clic en la casilla **Retener mi pago** para retener los pagos de este programa. Puede cambiar el estado de retención de pagos en cualquier momento, pero tenga en cuenta que su decisión afectará a su próximo pago mensual. Por ejemplo, si quiere retener los pagos de abril, asegúrese de establecer el estado de retención de pagos en **Activado** antes del final de marzo.

Una vez que haya establecido el estado de la retención de pagos en **Activado**, todos los pagos de este programa se pondrán en espera hasta que desplace el control deslizante de nuevo a **Desactivado**. Cuando lo haya hecho, se le incluirá en el siguiente ciclo de pago mensual (siempre que se hayan cumplido los umbrales de pago aplicables). Por ejemplo, si ha retenido los pagos pero quiere que se genere un pago en junio, asegúrese de alternar el estado de retención de pagos a **Desactivado** antes de finales de mayo.

> [!NOTE]
> El **Estado de retención del pago** se aplica individualmente a cada programa (Microsoft Store, publicidad, Azure Marketplace, etc.). Si quiere retener los pagos de todos los programas, debe retenerlos en cada programa de forma individual.


 

 