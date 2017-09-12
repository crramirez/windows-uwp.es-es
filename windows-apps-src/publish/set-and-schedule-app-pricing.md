---
author: jnHs
Description: "Selecciona el precio base para cambiar para una aplicación y programa el cambio de precio. También puedes personalizar estas opciones para mercados específicos."
title: Establecer y programar los precios de las aplicaciones
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.openlocfilehash: 65d2fa64e4d442da42b8c546693c61eda817aa18
ms.sourcegitcommit: 968187e803a866b60cda0528718a3d31f07dc54c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2017
---
# <a name="set-and-schedule-app-pricing"></a>Establecer y programar los precios de las aplicaciones

La sección **Precios** de la página [Precios y disponibilidad](set-app-pricing-and-availability.md) te permite seleccionar el precio base para una aplicación. También puedes [programar los cambios de precio](#schedule-price-changes) para indicar la fecha y la hora en las que el precio de la aplicación debería cambiar. Además, tienes la opción de [personalizar estos cambios para mercados específicos](#customize-pricing-for-specific-markets). 

> [!NOTE]
> Aunque este tema hace referencia a las aplicaciones, la selección d precios para envíos de complementos utiliza el mismo proceso.

## <a name="base-price"></a>Precio base

Cuando selecciones el **Precio base** de la aplicación, este se usará en cada mercado en el que se venda la aplicación, a menos que especifiques un precio personalizado para varios mercados determinados.

Puedes establecer el **precio base** a **Gratis**, o puedes elegir una franja de precios, que establece el precio de venta en todos los países donde elijas distribuir tu aplicación. Las franjas de precios empiezan a partir de 0,99 USD, con franjas adicionales disponibles en incrementos (1,09 USD, 1,19 USD, etc.). Estos incrementos generalmente aumentan a medida que el precio sube. 

> [!NOTE]
> Estas franjas de precios se aplican también a complementos. 

Cada franja de precios tiene un valor correspondiente en cada una de las más de 60 divisas que ofrece la Tienda. Usamos estos valores para ayudarte a vender tus aplicaciones a un punto de precio comparable mundial. Puedes seleccionar el precio base en cualquier moneda y utilizaremos automáticamente el valor correspondiente para diferentes mercados.

En la sección **Precios**, haz clic en **Ver la tabla** para ver los precios correspondientes en todas las monedas. Esto también muestra un número de identificador asociado a cada franja de precios, que necesitarás si usas la [API de envío de la Tienda Windows](../monetize/manage-app-submissions.md#price-tiers) para introducir precios. Puedes hacer clic en **Descargar** para descargar una copia de la tabla de franja de precios como un archivo .csv.

Recuerda que la franja de precios que seleccionaste puede incluir el impuesto sobre el valor añadido o las ventas que tus clientes deben pagar. Para obtener más información sobre las implicaciones fiscales de la aplicación en los mercados seleccionados, consulta los [detalles de impuestos para aplicaciones de pago](tax-details-for-paid-apps.md). También deberías revisar las [consideraciones relativas a los precios para mercados concretos](define-pricing-and-market-selection.md#price-considerations-for-specific-markets).

## <a name="schedule-price-changes"></a>Cambio de precio de programación

También puedes programar uno o varios de los cambios de precio si quieres que el precio base de la aplicación cambie en una fecha y hora específica. 

> [!IMPORTANT]
> Los cambios en los precios solo se muestran a los clientes de Windows 10. Si la aplicación es compatible con versiones anteriores del sistema operativo, los cambios de precios no se aplicarán. 
>
> - Para los clientes en Windows 8, la aplicación siempre se ofrecerá a su **precio base** (y no a cualquier precio específico del mercado), incluso si has programado cambios de precio adicionales. 
> - Para los clientes en Windows 8.1 y en Windows Phone 8.1 y versiones anteriores, la aplicación siempre se ofrecerá al precio inicial para el mercado del cliente, incluso si has programado cambios de precio adicionales en ese mercado.
> 
> Ten esto en cuenta al programar cambios en los precios. Por ejemplo, si inicialmente lanzas la aplicación a un precio más bajo y, a continuación, programas una fecha en la que debería aumentar el precio, los clientes con versiones anteriores del sistema operativo que adquieren la aplicación pagarán un precio más bajo (original).

Haz clic en **Programar un cambio de precio** para ver las opciones de cambio de precio. Elige la franja de precios que te gustaría usar y luego selecciona la fecha, hora y zona horaria.

Puedes hacer clic en **programar un nuevo cambio de precio** para programar tantos cambios posteriores como quieras.

> [!NOTE]
> Los cambios de precios programados funcionan de una manera diferente al [Precio de oferta](put-apps-and-add-ons-on-sale.md). Cuando una aplicación está en oferta, los clientes que consultan la descripción de la Tienda verán que el precio ha bajado y podrán comprarla a precio reducido durante el período que hayas seleccionado. Una vez que el periodo de oferta se acabe, se volverá al precio base.
>
> Con un cambio de precio programado, puedes ajustar el precio para que sea mayor o menor. El cambio se llevará a cabo en la fecha especifica, pero no se mostrará como una oferta en la Tienda. La aplicación solo tendrá un nuevo precio base. 

## <a name="customize-pricing-for-specific-markets"></a>Personalización de los precios para mercados específicos

De manera predeterminada, las opciones que selecciones anteriormente, se aplicarán a todos los mercados en los que se ofrece la aplicación. Para personalizar los precios para mercados específicos, selecciona **Personalización para mercados específicos**. La ventana emergente de **Selección de mercado** aparecerá en la lista de todos los mercados que hayas elegido en los que la aplicación estará disponible. Si se excluye algún mercado en la sección Mercados, no se mostrarán aquí. 

> [!IMPORTANT]
> Los clientes de Windows 8 siempre verán la aplicación con su **precio base**, incluso si seleccionas un precio diferente para el mercado.

Para agregar un precio de mercado personalizado, selecciónalo y haz clic en **Guardar**. A continuación, verás las mismas opciones **Precio base** y **Programar un cambio de precio** como se describen anteriormente, pero las selecciones que realices será específicas para ese mercado.

Para agregar un precio personalizado para varios mercados, se creará un *grupo de mercado*. Para ello, selecciona los mercados que quieras incluir y escribe un nombre del grupo. (Este nombre solo es para tu propia referencia y no será visible para los clientes). Cuando hayas terminado, haz clic en **Guardar**. A continuación, verás las mismas opciones **Precio base** y **Programar un cambio de precio** como se describen anteriormente, pero las selecciones que realices serán específicas para ese grupo de mercado.

> [!NOTE]
> Un mercado no puede pertenecer a más de un grupo de mercados que usas en la sección Precio.

Para agregar un precio personalizado para un mercado adicional, o un grupo de mercados adicionales, selecciona **Personalización para mercados específicos** de nuevo y repite estos pasos. Para cambiar los mercados que se incluyen en un grupo de mercado, selecciona su nombre. Para quitar el precio personalizado para un grupo de mercado (o mercado individual), haz clic en **Quitar**.



