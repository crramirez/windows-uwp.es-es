---
author: jnHs
Description: The Payout summary shows you details about the money you’ve earned with your apps and add-ons. It also lets you know when you’ll receive payments and how much you'll be paid.
title: Resumen de pago
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.author: wdg-dev-content
ms.date: 02/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, resumen de pago, extracto, pagos, ganancias, pagos, pago, beneficios
ms.localizationpriority: medium
ms.openlocfilehash: 1d9845fdbd9c8dad77c8599a04850a47573858e8
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "4360161"
---
# <a name="payout-summary"></a>Resumen de pago


En el apartado **Resumen de pago** se muestran los detalles sobre el dinero que has ganado con tus aplicaciones y complementos. También se muestra información que te permite saber cuándo recibirás los pagos y cuánto te pagarán.

Si usas publicidad para ganar dinero, también podrás ver la información de pago acerca de las ganancias de publicidad en **Resumen de pago**. Se te mostrará la aplicación que ha generado estas ganancias, o bien "sin asignar" para las unidades de anuncio que se usan en varias aplicaciones o que no se pueden asignar a una aplicación específica. 

Si vendes productos en Azure Marketplace, también verás información sobre los pagos efectuados en **Resumen de pago**. Para obtener más detalles sobre el pago en Azure Marketplace, consulta la página [Microsoft Azure Marketplace Participation Policies (Directivas de participación de Microsoft Azure Marketplace)](http://go.microsoft.com/fwlink/p/?LinkId=722436) y el documento [Microsoft Azure Marketplace Publisher Agreement (Acuerdo del publicador de Microsoft Azure Marketplace)](http://go.microsoft.com/fwlink/p/?LinkID=699560 ). Puedes encontrar más información sobre cómo generar informes de pagos de Azure Marketplace [aquí](http://go.microsoft.com/fwlink/p/?LinkID=722439).

> [!NOTE]
> Para ser apto para el pago, las ganancias deben alcanzar el [umbral de pago](payment-thresholds-methods-and-timeframes.md) aplicable. Si las ganancias son inferiores al umbral de pago, estas permanecerán en la categoría **Reservado** hasta que se alcance el umbral. Para obtener información detallada sobre el umbral de pago correspondiente a las ganancias por la aplicación, consulta el [Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Para las ganancias de publicidad, el umbral de pago es de 50USD (o su equivalente en moneda local). 
>
> Los pagos se realizan mensualmente (siempre que se haya alcanzado cualquier umbral de pago aplicable). Por lo general, enviaremos cualquier pago pendiente a día 15 de un mes determinado. Recuerda que los pagos suelen tardar entre 3 y 10 días laborables adicionales en acreditarse en tu cuenta de pago. Para obtener más información, consulta [Umbrales, métodos y plazos de pago](payment-thresholds-methods-and-timeframes.md).

Para ver tu **Resumen de pago**, haz clic en el icono **Pago** que aparece junto a la esquina superior derecha del Centro de desarrollo y, a continuación, selecciona **Resumen de pago**.

## <a name="current-proceeds-and-payments"></a>Ganancias y pagos actuales

En la parte superior de la página, encontrarás el apartado **Ganancias y pagos actuales**, que contiene tres secciones: **Reservado**, **Próximo pago** y **Pago más reciente**.

-   En **Reservado** se muestra el importe que ha acumulado tu cuenta, pero que aún no se ha programado para el pago, incluidas las ganancias por publicidad. (Las ganancias de Azure Marketplace no aparecen en la sección **Reservado**; si solo participas en Azure Marketplace, verás que aparece el valor 0,00USD en esta sección). Las ganancias obtenidas por las ventas de tus aplicaciones más recientes, se mantienen en un estado pendiente durante 30 días aproximadamente antes de que puedas cobrarlas. Después, se programan las ganancias para el pago en el mes siguiente (suponiendo que se haya alcanzado el [umbral de pago](payment-thresholds-methods-and-timeframes.md)). Cuando se intenta un pago, tu saldo reservado disminuirá en el importe del pago y verás el importe reflejado en **Próximo pago**. Ten en cuenta que el importe que se muestra en **Reservado** es una estimación, porque los tipos de cambio para las ventas en otras monedas pueden variar antes de la creación del pago. Puedes observar cómo tu saldo reservado cambia ligeramente al principio de cada mes. Tu saldo reservado se actualiza mensualmente para reflejar los tipos de cambio mensuales y así representa una estimación más precisa. Puedes hacer clic en **Ver detalles** para ver información adicional, o en el vínculo **Descargar las transacciones reservadas** para ver un archivo .csv de todas las transacciones que estén en la categoría **Reservado**.
-   En **Próximo pago** se muestra el número de pagos que se harán en un futuro, el importe de tus próximos pagos y las fechas de creación de los pagos. Si tus ganancias aptas aún no han alcanzado el [umbral de pago](payment-thresholds-methods-and-timeframes.md), no se muestra el próximo pago aquí. Selecciona **Ver detalles** para ver información adicional, como los importes de pago y su origen de ingresos correspondiente. Cuando se muestre un importe en la sección **Próximo pago**, verás un vínculo temporal a **Descargar transacciones**.  Si haces clic en el vínculo, puedes ver un archivo .csv que incluye todas las transacciones que componen los próximos pagos.  Ten en cuenta que, si el importe que consta en la sección **Próximo pago** pasa a la sección **Pago más reciente**, el vínculo **Descargar transacciones** ya no se muestra más.
-   En **Pago más reciente** se muestra el importe del último pago intentado. Si el pago se realizó correctamente, el vínculo **Ver detalles** aparecerá en color azul y puedes hacer clic en él para ver los detalles de cada pago. Ten en cuenta que si intentamos hacer varios pagos y solo uno de ellos se realiza correctamente, aquí solo se mostrará el importe del pago correcto. Si se produce un error en uno o más pagos, el vínculo **Ver detalles** aparecerá en color rojo y se mostrará el número de pagos con error. Puedes hacer clic en **Ver detalles** para ver más detalles sobre el problema a fin de poder solucionarlo.

## <a name="proceeds-by-app-and-adjustments"></a>Ganancias por la aplicación y ajustes


En esta sección se divide la información de resumen para que puedas ver información específica de cada aplicación. Si has ganado dinero con la publicidad, aquí se muestra la cantidad total de las ganancias de publicidad con un elemento de línea única.

Al revisar esta sección, puedes determinar qué aplicaciones han ganado el dinero que se encuentra actualmente en las categorías **Reservado** o **Pago más reciente**. También puedes ver el importe total recibido de cada aplicación. En caso de que necesites realizar [ajustes](#proceeds-by-app-and-adjustments) en tu saldo de cuenta, puedes verlos aquí también. (Ten en cuenta que los ajustes para las ganancias de publicidad no se muestran aquí.)

## <a name="payment-statements"></a>Extractos de pago


En esta sección puedes ver los extractos de todos los pagos mensuales que se han realizado correctamente, así como la cantidad total de dinero que se te ha pagado.

En la sección **Total pagado hasta la fecha** se muestra el importe total que se te ha pagado para todas tus ventas. Haz clic en **Ver detalles** para ver los importes que procedían de cada fuente de ingresos.

Debajo de la sección **Total pagado hasta la fecha**, verás los últimos tres extractos de manera predeterminada. Para ver el extracto completo (de los pagos correctos), haz clic en **Vista**. Puedes acceder a los extractos de pagos históricos a través del cuadro desplegable.

En la parte superior de cada extracto, verás el importe total de tu pago mensual. Justo debajo, en los **Pagos emitidos**, verás un resumen de cómo se calculó el importe del pago.

Debajo, en la sección **Proceeds breakdown**, puedes ver detalles sobre cuánto dinero se ganó por mercado y por fuente de ingresos (por ejemplo, Microsoft Store, Windows Store 8, Windows Phone Store, etc.) por aplicación. También verás detalles acerca de los [ajustes](#proceeds-by-app-and-adjustments) realizados, incluida la fecha, el importe y el motivo del ajuste.

Ten en cuenta que en las secciones anteriores solo se muestra información sobre tus ganancias (y ajustes) por ventas de aplicaciones; si ganaste dinero mediante publicidad, verás una sección independiente de Microsoft Advertising con detalles sobre los pagos y conversiones de divisas.

## <a name="adjustments"></a>Ajustes


| Categoría de ajuste     | Descripción                                                                                                |
|-------------------------|------------------------------------------------------------------------------------------------------------|
| Ajuste de compensación | Los ajustes realizados en tu saldo de pago que no pertenezcan a otras categorías de ajuste enumeradas |
| Saldo histórico        | Saldos de pago de un sistema de pagos histórico                                                             |
| Transferencia de impuestos              | Ajuste de impuestos relacionados con las ventas en Corea                                                                   |

 

## <a name="downloading-payment-transactions"></a>Descargar transacciones de pago


En la parte superior de cada extracto, verás un vínculo para **Descargar transacciones**. Haz clic en este vínculo para obtener un archivo .csv con información detallada sobre cada una de las transacciones incluidas en tu pago.

En la siguiente tabla se describen los campos que aparecen en el archivo .csv. Ten en cuenta que los campos exactos que puedes ver variar a medida que seguimos actualizando el informe.

| Nombre del campo              | Descripción                                                                                                                              |
|-------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| Fuente de ingresos          | La fuente de tus ingresos, que se basa en el lugar en el que se ha producido la transacción (por ejemplo, Microsoft Store, Windows Phone Store, Windows Store 8, publicidad, etc.) |
| Id. de pedido                |  Un identificador de pedido exclusivo. Este id. permite identificar las transacciones de compra con sus respectivas transacciones que no son de compra (como las devoluciones, la anulaciones, etc.). Ambas tendrá el mismo id. de pedido. Además, en el caso de un cargo dividido en el que se hayan usado varios métodos de pago para una sola compra, te permitirá vincular las transacciones de compra.                                                                                                          |
| Id. de transacción          |       Identificador único de la transacción.  |
| Fecha y hora de transacción   | Fecha y hora en que se realizó la transacción (hora UTC).                                                                                        |
| Id. de producto principal       | Identificador único del producto principal. Ten en cuenta que, si no hay un producto principal para la transacción, el identificador del producto principal es igual al identificador del producto. |
| Id. del producto              | Identificador único del producto.                                                                                                               |
| Nombre del producto principal     | Nombre del producto principal. Ten en cuenta que, si no hay un producto principal para la transacción, el nombre del producto principal es igual al nombre del producto.   |
| Nombre del producto            | Nombre del producto.                                                                                                                     |
| Tipo de producto            | Tipo de producto (por ejemplo aplicación, complemento, juego, etc.)                                                                                        |
| Cantidad                | Si el valor de Fuente de ingresos es la Microsoft Store para Empresas, Cantidad representa el número de licencias adquiridas. Para otras fuentes de ingresos, el valor de Cantidad siempre será 1. Nota: Aunque una misma transacción esté dividida en dos artículos de línea debido al uso de dos métodos de pago distintos, cada artículo de línea mostrará un valor de 1 en Cantidad.    |
| Tipo de transacción        | Tipo de transacción (por ejemplo, compra, reembolso, inversión, anulación, etc.)                                                                |
| Método de pago          | Instrumento de pago del cliente usado para la transacción (por ejemplo, tarjeta, facturación del operador de telefonía móvil, PayPal, etc.)                                 |
| País o región        | País o región donde se realizó la transacción.                                                                                            |
| Proveedor o vendedor local | Proveedor o vendedor local del registro.                                                                                                          |
| Moneda de transacción    | Moneda de la transacción.                                                                                                              |
| Importe de la transacción      | Importe de la transacción.                                                                                                                |
| Impuestos enviados            | Importe de impuestos enviados (de ventas, de uso o impuestos sobre bienes y servicios o IVA)                                                                                    |
| Ingresos netos            | Importe de la transacción menos impuestos.                                                                                                     |
| Honorarios de la Tienda               | Porcentaje de los ingresos netos retenido por Microsoft en concepto de honorarios por hacer que la aplicación o el complemento esté disponible en la Tienda.                        |
| Ganancias por la aplicación            | Ingresos netos menos los honorarios de la Tienda.                                                                                                         |
| Impuestos retenidos          | Importe de impuestos retenido. (No se incluye en el archivo .csv **Reservado**).                                                                  |
| Pago                 | Ganancias por la aplicación menos la retención de impuestos aplicables (es el importe que aparece en Moneda de transacción). (No se incluye en el archivo .csv **Reservado**). |
| Tipo de cambio                 | Tipo de cambio usado para convertir la moneda de la transacción a la moneda de pago.                                                           |
| Moneda de pago        | Moneda en que se realiza el pago.                                                                                                         |
| Pago convertido       | Importe del pago convertido a la moneda de pago con el tipo de cambio.                                                                           |
| Modelo de remesa de impuestos         | Parte responsable de la remesa de impuestos (de ventas, de uso o impuestos sobre bienes y servicios o IVA)                                                                     |
| Fecha y hora de elegibilidad   | Fecha y hora en las que las ganancias por las transacciones sean aptas para el pago (hora UTC). Cuando se crea un pago, incluye las ganancias de las transacciones que tienen una fecha y hora de elegibilidad anterior a la fecha de creación del pago. (Solo se incluye en el archivo .csv **Reservado**).                                                                     |
| Cargos                 | Muestra un desglose de todos los detalles de los cargos agregados en la columna Importe de la transacción. (Solo se incluye para Azure Marketplace; no se incluye en el archivo .csv **Reservado**).          |

 

 

 




