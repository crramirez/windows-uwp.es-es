---
Description: Seleccione el precio base para una aplicación y programe los cambios de precios. También puede personalizar estas opciones para mercados específicos.
title: Establecer y programar los precios de las aplicaciones
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, precios, precios de la aplicación, precio de la aplicación, aplicaciones de venta, cambio de precio, precio personalizado, precio, precios, costo, invalidar precio base, precio de forma libre, forma libre
ms.localizationpriority: medium
ms.openlocfilehash: 451a22ffef2d8062de7bf7d29d921db7197987b5
ms.sourcegitcommit: c2e4bbe46c7b37be1390cdf3fa0f56670f9d34e9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92253809"
---
# <a name="set-and-schedule-app-pricing"></a>Establecer y programar los precios de las aplicaciones

La sección de **precios** de la página [precios y disponibilidad](set-app-pricing-and-availability.md) le permite seleccionar el precio base de una aplicación. También puede [programar cambios de precios](#schedule-price-changes) para indicar la fecha y la hora a las que debe cambiar el precio de la aplicación. Además, tiene la opción de [invalidar el precio base para mercados específicos](#override-base-price-for-specific-markets), ya sea seleccionando un nuevo plan de precios o introduciendo un precio de forma libre en la moneda local del mercado.

> [!NOTE]
> Aunque este tema hace referencia a las aplicaciones, la selección de precios para los envíos de complementos usa el mismo proceso. Tenga en cuenta que en el caso de los [Complementos de suscripción](../monetize/enable-subscription-add-ons-for-your-app.md), el precio base que seleccione no se puede aumentar nunca (ya sea cambiando el precio base o programando un cambio de precio), aunque puede reducirse.

## <a name="base-price"></a>Precio base

Al seleccionar el **precio base**de la aplicación, ese precio se usará en todos los mercados en los que se venda la aplicación, a menos que se invalide el precio base en los mercados.

Puede establecer el **precio base** en **gratis**o puede elegir un plan de tarifa disponible, que establece el precio en todos los países en los que elige distribuir la aplicación. Los niveles de precios comienzan a las 0,99 USD, con niveles adicionales disponibles en incrementos crecientes (1,09 USD, 1,19 USD, etc.). Los incrementos generalmente aumentan a medida que el precio es mayor. 

> [!NOTE]
> Estos niveles de precios también se aplican a los complementos. 

Cada plan de tarifa tiene un valor correspondiente en cada una de las más de 60 monedas ofrecidas por el almacén. Usamos estos valores para ayudarte a vender tus aplicaciones a un punto de precio comparable mundial. Puede seleccionar el precio base en cualquier divisa y usaremos automáticamente el valor correspondiente para los distintos mercados. Tenga en cuenta que, a veces, se puede ajustar el valor correspondiente en un determinado mercado para tener en cuenta los cambios en las tasas de conversión de moneda.

En la sección **precios** , haga clic en **ver tabla de conversión** para ver los precios correspondientes en todas las monedas. También se muestra un número de identificación asociado a cada nivel de precios, que necesitará si usa la [API de envío de Microsoft Store](../monetize/manage-app-submissions.md#price-tiers) para introducir los precios. Puede hacer clic en **Descargar** para descargar una copia de la tabla de nivel de precios como archivo. csv.

Recuerda que la franja de precios que seleccionaste puede incluir el impuesto sobre el valor añadido o las ventas que tus clientes deben pagar. Para obtener más información sobre las implicaciones fiscales de la aplicación en los mercados seleccionados, consulta los [detalles de impuestos para aplicaciones de pago](tax-details-for-paid-apps.md). También debe revisar las [consideraciones de precios para mercados específicos](define-market-selection.md#price-considerations-for-specific-markets).

> [!NOTE]
> Si eliges la opción **detener adquisición** en **hacer que este producto esté disponible pero no se puede detectar en la tienda** de la sección [visibilidad](choose-visibility-options.md#discoverability) ), no podrás establecer los precios para tu envío (ya que nadie podrá adquirir la aplicación a menos que use un código promocional para obtener la aplicación de forma gratuita).

## <a name="schedule-price-changes"></a>Programar cambios de precios

Opcionalmente, puede programar uno o varios cambios de precios si desea que el precio base de la aplicación cambie en una fecha y hora específicas. 

> [!IMPORTANT]
> Los cambios de precio solo se muestran a los clientes en dispositivos Windows 10 (incluida Xbox). Si la aplicación publicada anteriormente es compatible con versiones anteriores del sistema operativo, los cambios de precio no se aplicarán a los clientes. En el caso de los clientes de Windows 8, la aplicación siempre se ofrecerá a su **precio base** (y no a ningún precio específico del mercado), incluso si programa cambios adicionales en el precio. En el caso de los clientes de Windows 8.1 y en Windows Phone 8,1 y versiones anteriores, la aplicación siempre se ofrecerá en el primer nivel de precios para el mercado del cliente.

Haga clic en **programar un cambio de precio** para ver las opciones de cambio de precio. Elija el nivel de precios que desea usar (o escriba un precio libre para las invalidaciones de precios de base de un solo mercado) y, a continuación, seleccione la fecha, la hora y la zona horaria.

Puede volver a hacer clic en **programar un cambio de precio** para programar tantos cambios posteriores como desee.

> [!NOTE]
> Los cambios de precio programados funcionan de manera diferente a los [precios de venta](put-apps-and-add-ons-on-sale.md). Al poner una aplicación en venta, el precio se muestra con un tachado en el almacén y los clientes podrán comprar la aplicación con el precio de venta durante el período de tiempo que haya seleccionado. Una vez que el período de venta esté activo, ya no se aplicará el precio de venta y la aplicación estará disponible a su precio base (u otro precio que haya especificado para ese mercado, si procede).
>
> Con un cambio de precio programado, puede ajustar el precio para que sea mayor o menor. El cambio se realizará en la fecha que especifique, pero no se mostrará como una venta en el almacén ni se aplicará ningún formato especial. la aplicación solo tendrá un nuevo precio. 


## <a name="override-base-price-for-specific-markets"></a>Invalidar precio base para mercados específicos

De forma predeterminada, las opciones seleccionadas anteriormente se aplicarán a todos los mercados en los que se ofrece la aplicación. Opcionalmente, puede cambiar el precio de uno o más mercados, eligiendo un plan de tarifa diferente o introduciendo un precio de forma libre en la moneda local del mercado.

> [!IMPORTANT]
> Si la aplicación publicada previamente es compatible con Windows 8, esos clientes siempre verán la aplicación a su **precio base**, aunque Seleccione un precio diferente para su mercado.

Para cambiar el precio de mercados específicos, haga clic en **seleccionar mercados para invalidar el precio base**. Aparecerá la ventana emergente de **selección de mercado** , donde se muestran todos los mercados en los que ha elegido hacer que la aplicación esté disponible. (Si ha excluido los mercados de la sección **mercados** , estos mercados no estarán disponibles). 

Puede invalidar el precio base de un mercado a la vez o para un grupo de mercados juntos. Una vez hecho esto, puede invalidar el precio base para un mercado adicional (o un grupo de mercado adicional) seleccionando de nuevo los **mercados de la anulación de precio base** y repitiendo el proceso que se describe a continuación. Para quitar los precios de invalidación que ha especificado para un mercado (o un grupo de marketing), haga clic en **quitar**.


### <a name="override-the-base-price-for-a-single-market"></a>Invalidar el precio base para un solo mercado

Para cambiar el precio de un solo mercado, selecciónelo y haga clic en **crear**. A continuación, verá el mismo **precio base** y **programará** las opciones de cambio de precio como se describió anteriormente, pero las selecciones que realice serán específicas para ese mercado. Dado que solo está invalidando el precio base de un mercado, los planes de tarifa se mostrarán en la moneda local del mercado. Puede hacer clic en **ver tabla de conversión** para ver los precios correspondientes en todas las monedas. 

La invalidación del precio base de un solo mercado también le ofrece la opción de especificar un precio gratuito de su elección en la moneda local del mercado. Puede especificar el precio que desee (dentro de un intervalo mínimo y máximo), incluso si no se corresponde con uno de los planes de tarifa estándar. Este precio solo se usará para los clientes de Windows 10 (incluido Xbox) en el mercado seleccionado. 

> [!IMPORTANT]
> Si especifica un precio de forma libre, dicho precio no se ajustará (incluso si cambian las tasas de conversión) a menos que envíe una actualización con un nuevo precio. 

### <a name="override-the-base-price-for-a-market-group"></a>Invalidar el precio base de un grupo de marketing

Para invalidar el precio base de varios mercados, creará un *grupo de marketing*. Para ello, seleccione los mercados que quiera incluir y, opcionalmente, escriba un nombre para el grupo. (Este nombre es solo para su referencia y no será visible para ningún cliente). Cuando haya terminado, haga clic en **crear**. A continuación, verá el mismo **precio base** y **programará** las opciones de cambio de precio como se describió anteriormente, pero las selecciones que realice serán específicas para ese grupo de mercado. Tenga en cuenta que los precios de forma libre no se pueden usar con grupos de marketing; deberá seleccionar un plan de tarifa disponible.

Para cambiar los mercados incluidos en un grupo de marketing, haga clic en el nombre del grupo de marketing, agregue o quite los mercados que desee y, a continuación, haga clic en **Aceptar** para guardar los cambios. 

> [!NOTE]
> Un mercado no puede pertenecer a varios grupos de marketing dentro de la sección de **precios** .





