---
author: jnHs
Description: Select the base price for an app and schedule price changes. You can also customize these options for specific markets.
title: Establecer y programar los precios de las aplicaciones
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, precios, precios de las aplicaciones, precio de la aplicación, vender aplicaciones, cambio de precio, precio personalizado, precio, precios, costo, reemplazar el precio base, precio de forma libre, forma libre
ms.localizationpriority: medium
ms.openlocfilehash: 372abfdb0de5567b7c7d262b298d264b086fe339
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5839340"
---
# <a name="set-and-schedule-app-pricing"></a>Establecer y programar los precios de las aplicaciones

La sección **Precios** de la página [Precios y disponibilidad](set-app-pricing-and-availability.md) te permite seleccionar el precio base para una aplicación. También puedes [programar los cambios de precio](#schedule-price-changes) para indicar la fecha y la hora en las que el precio de la aplicación debería cambiar. Además, tienes la opción de [invalidar el precio base para mercados específicos](#override-base-price-for-specific-markets), ya sea seleccionando una nueva franja de precios o escribiendo un precio de forma libre en la moneda local del mercado.

> [!NOTE]
> Aunque este tema hace referencia a las aplicaciones, la selección de precios para envíos de complementos utiliza el mismo proceso. Ten en cuenta que para los [complementos de suscripción](../monetize/enable-subscription-add-ons-for-your-app.md), el precio base que selecciones no se nunca puede aumentar (si cambiando el precio base o programar un cambio de precio), aunque se puede disminuir.

## <a name="base-price"></a>Precio base

Cuando selecciones el **Precio base** de la aplicación, este se usará en cada mercado en el que se venda la aplicación, a menos que reemplaces el precio base en cualquier mercado.

Puedes establecer el **precio base** en **Gratis** o puedes elegir una franja de precios, que establece el precio en todos los países donde elijas distribuir tu aplicación. Las franjas de precios empiezan a partir de 0,99USD, con franjas adicionales disponibles en incrementos (1,09USD, 1,19USD, etc.). Estos incrementos generalmente aumentan a medida que el precio sube. 

> [!NOTE]
> Estas franjas de precios se aplican también a complementos. 

Cada franja de precios tiene un valor correspondiente en cada una de las más de 60 divisas que ofrece la Tienda. Usamos estos valores para ayudarte a vender tus aplicaciones a un punto de precio comparable mundial. Puedes seleccionar el precio base en cualquier moneda y utilizaremos automáticamente el valor correspondiente para diferentes mercados. Ten en cuenta que a veces podemos ajustar el valor correspondiente en un determinado mercado para tener en cuenta cambios en las tasas de conversión de divisas.

En la sección **Precios**, haz clic en **view conversion table** para ver los precios correspondientes en todas las monedas. Esto también muestra un número de identificador asociado a cada franja de precios, que necesitarás si usas la [API de envío de la Microsoft Store](../monetize/manage-app-submissions.md#price-tiers) para introducir precios. Puedes hacer clic en **Descargar** para descargar una copia de la tabla de franja de precios como un archivo .csv.

Recuerda que la franja de precios que seleccionaste puede incluir el impuesto sobre el valor añadido o las ventas que tus clientes deben pagar. Para obtener más información sobre las implicaciones fiscales de la aplicación en los mercados seleccionados, consulta los [detalles de impuestos para aplicaciones de pago](tax-details-for-paid-apps.md). También deberías revisar las [consideraciones relativas a los precios para mercados concretos](define-pricing-and-market-selection.md#price-considerations-for-specific-markets).

> [!NOTE]
> Si eliges la opción **Detener la compra** en **hacer este producto disponible, pero no detectable en la tienda** en la sección [visibilidad](choose-visibility-options.md#discoverability) ), no podrás establecer precios para el envío (ya que nadie podrá comprar la aplicación a menos que usan un código promocional para obtener la aplicación de forma gratuita).

## <a name="schedule-price-changes"></a>Programar cambios de precio

También puedes programar uno o varios de los cambios de precio si quieres que el precio base de la aplicación cambie en una fecha y hora específica. 

> [!IMPORTANT]
> Los cambios en los precios solo se muestran a los clientes de dispositivos Windows10 (incluida la Xbox). Si la aplicación publicada anteriormente es compatible con versiones anteriores del sistema operativo, los cambios de precio no se aplicarán a estos clientes. Para los clientes en Windows 8, la aplicación siempre se ofrecerá a su **precio base** (y no a cualquier precio específico del mercado), incluso si has programado cambios de precio adicionales. Para los clientes en Windows 8.1 y en Windows Phone 8.1 y versiones anteriores, la aplicación siempre se ofrecerá a la primera franja de precios para el mercado del cliente.

Haz clic en **Programar un cambio de precio** para ver las opciones de cambio de precio. Elige la franja de precios que quieres usar (o especifica un precio de forma libre para invalidaciones de precio base de mercado único) y luego selecciona la fecha, la hora y la zona horaria.

Puedes hacer clic en volver a **programar un cambio de precio** para programar tantos cambios posteriores como quieras.

> [!NOTE]
> Los cambios de precios programados funcionan de una manera diferente al [Precio de oferta](put-apps-and-add-ons-on-sale.md). Cuando una aplicación está en oferta, el precio muestra un tachado en la Store y los clientes podrán comprar la aplicación en el precio de oferta durante el período que hayas seleccionado. Una vez que el periodo de oferta se acabe, ya no se aplicará el precio de venta y la aplicación estará disponible en su precio base (o un precio diferente que hayas especificado para ese mercado, si procede).
>
> Con un cambio de precio programado, puedes ajustar el precio para que sea mayor o menor. El cambio se llevará a cabo en la fecha especifica, pero no se mostrará como una oferta en la Store, o tiene cualquier formato especial aplicado; la aplicación solo tendrá un nuevo precio base. 


## <a name="override-base-price-for-specific-markets"></a>Reemplazar el precio base para mercados concretos

De manera predeterminada, las opciones que selecciones anteriormente, se aplicarán a todos los mercados en los que se ofrece la aplicación. Opcionalmente, puedes cambiar el precio de uno o más mercados, elegir una franja de precios diferente o escribir un precio de forma libre en la moneda local del mercado.

> [!IMPORTANT]
> Si la aplicación publicada anteriormente es compatible con Windows 8, estos clientes siempre verán la aplicación en su **precio Base**, incluso si seleccionas un precio diferente para el mercado.

Para cambiar el precio para mercados específicos, haz clic en **Select markets for base price override**. La ventana emergente de **Selección de mercado** aparecerá en la lista de todos los mercados que hayas elegido en los que la aplicación estará disponible. (Si se excluye algún mercado en la sección **Mercados**, esos mercados no estará disponibles.) 

Puedes invalidar el precio base para un mercado cada vez, o para un grupo de mercados juntos. Una vez que lo hayas hecho, para invalidar el precio base para un mercado adicional (o un grupo de mercado adicional), selecciona **Select markets for base price override** de nuevo y repite el proceso que se describe a continuación. Para quitar el precio de invalidación que ha especificado para un mercado (o grupo de mercado), haz clic en **Quitar**.


### <a name="override-the-base-price-for-a-single-market"></a>Invalidar el precio base para un mercado único

Para cambiar el precio para un solo mercado, selecciónalo y haz clic en **Crear**. A continuación, verás las mismas opciones **Precio base** y **Programar un cambio de precio** como se describen anteriormente, pero las selecciones que realices será específicas para ese mercado. Dado que va a reemplazar el precio base para un solo mercado, se mostrarán las franjas de precios en la moneda local de ese mercado. Puede hacer clic en **view conversion table** para ver los precios correspondientes en todas las monedas. 

La invalidación del precio base para un solo mercado también te ofrece la opción de especificar un precio de forma libre de tu elección en la moneda local del mercado. Puedes especificar cualquier precio que quieras (dentro de un intervalo mínimo y máximo), incluso si no se corresponde a una de las franjas de precios estándar. Este precio se usará solo para los clientes de Windows 10 (incluida la Xbox) del mercado seleccionado. 

> [!IMPORTANT]
> Si especifica un precio de forma libre, no se ajustará ese precio (incluso si cambian las tasas de conversión) a menos que envíes una actualización con un precio nuevo. 

### <a name="override-the-base-price-for-a-market-group"></a>Invalidar el precio base para un grupo de mercado

Para invalidar el precio base para varios mercados, se creará un *grupo de mercado*. Para ello, selecciona los mercados que quieras incluir y, opcionalmente, escribe un nombre del grupo. (Este nombre solo es para tu propia referencia y no será visible para los clientes). Cuando hayas terminado, haz clic en **Crear**. A continuación, verás las mismas opciones **Precio base** y **Programar un cambio de precio** como se describen anteriormente, pero las selecciones que realices serán específicas para ese grupo de mercado. Ten en cuenta que no se pueden usar precios de forma libre con grupos de mercado; deberás seleccionar una franja de precios.

Para cambiar los mercados que se incluyen en un grupo de mercado, haz clic en el nombre del grupo de mercado y agrega o quita los mercados que quieras; después, haz clic en **Aceptar** para guardar los cambios. 

> [!NOTE]
> Un mercado no puede pertenecer a varios grupos de mercados dentro de la sección **Precios**.





