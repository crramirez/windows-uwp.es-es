---
description: Puede promover la aplicación o el complemento en el Microsoft Store poniéndolo en venta durante un tiempo limitado.
title: Poner aplicaciones y complementos en oferta
ms.assetid: 71ABA960-0CDC-4E35-A1C8-1D34B6673817
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5bd1a5f6961e3b1caaab29a6a3fb9ecef2e9d6ef
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035008"
---
# <a name="put-apps-and-add-ons-on-sale"></a>Poner aplicaciones y complementos en oferta

Puede promover la aplicación o el complemento en el Microsoft Store poniéndolo en venta durante un tiempo limitado. Puede elegir ofrecer el producto en un plan de tarifa inferior o con un descuento basado en porcentajes. Además, puede elegir si desea ofrecer la venta a todos los usuarios o convertirla en una oferta exclusiva para los clientes que poseen uno de sus otros productos.

> [!NOTE]
> Los precios de venta no se admiten para los complementos de suscripción.

Al usar la sección **precios de venta** de la página de **precios y disponibilidad** de un envío para reducir temporalmente el precio de la aplicación o el complemento, los clientes que vean la lista de la tienda verán un precio de tachado que indica que se ha reducido el precio (en lugar de un [cambio de precio programado](set-and-schedule-app-pricing.md#schedule-price-changes), lo que puede reducir o aumentar el precio sin mostrarlo como un cambio en el almacén 

Durante el período de tiempo durante el cual el producto está en venta, los clientes podrán adquirirlo a un precio inferior durante el período de tiempo que haya seleccionado. Si reduces el precio a **Gratis** , podrán descargarla sin pagar nada durante el periodo de oferta.

> [!IMPORTANT]
> Los precios de venta solo se muestran a los clientes en dispositivos Windows 10, incluido Xbox One. Las ventas que solo se ofrecen a los propietarios de uno de los otros productos solo se muestran a los clientes de Windows 10, versión 1607 o posterior.
> 
> En otros sistemas operativos, los clientes verán el precio normal de la aplicación o el complemento y no podrán comprarlo a precio de venta. Siempre puedes cambiar un precio eligiendo una franja de precios diferente en un nuevo envío, pero no se mostrará como una oferta por tiempo limitado.


## <a name="scheduling-a-sale"></a>Programar una oferta

Las ofertas se programan como parte del envío de una aplicación o un complemento. Si quieres programar una oferta para una aplicación o un complemento que ya se ha publicado, tendrás que crear un nuevo envío, aunque ese sea el único cambio que quieras realizar.

**Para programar una oferta**

1. En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta** .
2. Seleccione **Mostrar opciones** y, a continuación, seleccione **nueva venta** .
3. Aparecerá la ventana emergente de **selección de mercado** , que le permite crear un *grupo de marketing* que especificará los mercados en los que se debe ofrecer la venta. Puede hacer clic en **seleccionar todo** para ofrecer la venta a todos los mercados en los que la aplicación esté disponible, seleccionar un mercado individual o seleccionar varios mercados. Opcionalmente, puede escribir un nombre para el grupo de marketing. Cuando haya realizado las selecciones, haga clic en **crear** . (Para editar los mercados del grupo más adelante, haga clic en su nombre).

   > [!NOTE]
   > Las selecciones de mercado que realice en la sección precios de venta no afectarán a los mercados en los que se ofrece la aplicación; estas selecciones solo determinan si se ofrece un precio de venta y en qué mercados. Si estableces un precio de oferta para un mercado en el que la aplicación no está disponible, esto no hará que la aplicación esté disponible en ese mercado.
4. Elija una de las siguientes opciones para especificar el tipo de descuento:
   - **Precio** : Use esta opción para seleccionar un plan de tarifa inferior en el que se ofrecerá la aplicación. Puede cambiar la lista desplegable moneda para seleccionar el precio en la moneda que prefiera. (El precio se convertirá en el nivel correspondiente para cada moneda. Para obtener más información, consulte [precios](set-app-pricing-and-availability.md)).
   - **Porcentaje** : Use esta opción para seleccionar el porcentaje de un descuento que se aplicará a la aplicación. El mismo porcentaje de descuento se usa para todas las monedas.
5. En la fila ofrecido a, elija una de las opciones disponibles, entre las **que se** incluyen:
   - **Todos** : la venta se ofrecerá a todos los clientes.
   - **Propietarios de** : la venta se ofrecerá a los clientes que ya poseen una de sus aplicaciones. Puede seleccionar entre las aplicaciones publicadas en la lista desplegable que aparece. Para que esta opción esté disponible, debe tener una o varias aplicaciones publicadas.

  > [!IMPORTANT]
  > Si selecciona **propietarios de** , la venta solo estará visible para los clientes de Windows 10, versión 1607 o posterior.

   - **Grupo de usuarios conocidos** : la venta se ofrecerá a las personas del [grupo de usuarios conocidos](create-known-user-groups.md) que seleccione. Ya debe haber creado el grupo de usuarios conocido para que esta opción esté disponible.
   - **Segmento** : la venta se ofrecerá a las personas del segmento de cliente que seleccione. Puede usar un  [segmento que ya ha creado](create-customer-segments.md) aquí. También puede elegir los **primeros pagadores** para ofrecer la venta solo a los clientes que nunca han comprado nada en la tienda. Aquí ofrecemos este segmento porque hemos descubierto que, una vez que un cliente realiza la compra de la primera tienda, a menudo continúan realizando más compras, por lo que puede ser un grupo excelente para atraer los precios de venta.
6. Escribe la fecha y la hora del inicio y final del período de oferta. Elija una de las siguientes opciones de zona horaria:
   - **UTC** : la hora que seleccione será la hora universal coordinada (UTC), de modo que la venta se realice al mismo tiempo en todo el mundo.
   - **Local** : la hora que seleccione será la usada en cada zona horaria asociada a un mercado. (Tenga en cuenta que para los mercados que incluyen más de una zona horaria, solo se utilizará una zona horaria en ese mercado. En el Estados Unidos, se usa la zona horaria oriental).
7. Para programar una venta adicional, seleccione **nueva venta** . En caso contrario, seleccione **Guardar** en la parte inferior de la **Página precios y disponibilidad** y, luego, seleccione **Enviar a la tienda** en la información general del envío.

> [!NOTE]
> Es posible seleccionar un plan de tarifa superior al precio base de la aplicación. Sin embargo, el precio de oferta solo se mostrará a los clientes si es inferior al precio normal de la aplicación en ese mercado.
>
> Seleccionar un precio de oferta mayor que el precio base de la aplicación puede ser apropiado si ya has establecido en determinados mercados precios personalizados que superan el precio base y quieres reducir temporalmente el precio en dichos mercados (siendo el precio de venta todavía mayor que el precio base de la aplicación). Si tus selecciones resultan en que el precio de la aplicación aumente en un mercado concreto, dicho precio (superior) no se mostrará en ese mercado: los clientes seguirán viendo la aplicación con su precio anterior (inferior). También se mostrará a los clientes el menor precio disponible si programas distintas ofertas superpuestas con diferentes precios.

## <a name="changing-or-canceling-a-scheduled-sale"></a>Cambiar o cancelar una oferta programada

Para revisar o cancelar una oferta previamente programada para una aplicación o un complemento, debes crear y realizar un nuevo envío para la Tienda.

**Para editar una oferta programada**

1.  En la página **precios y disponibilidad** de una aplicación en curso o un envío de complemento, vaya a la sección **precios de venta** .
2.  Busque la venta que desea actualizar y luego realice los cambios.
3.  Haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, haz clic en **Enviar a la Tienda** en el resumen del envío.

Después de que el envío pase por el proceso de certificación, los cambios surtirán efecto.

> [!IMPORTANT]
> Si ya se ha iniciado una venta, no podrá editar la fecha de inicio. Aunque puede editar la fecha de finalización, se recomienda que no edite una venta para que finalice antes de la fecha de finalización original. Puede ser frustrante para sus clientes potenciales si termina una venta antes de la fecha de publicación originalmente (puesto que los clientes ven la fecha de finalización programada al ver la lista de la tienda de la aplicación).

 **Para cancelar una venta que todavía no se ha iniciado**

1.  En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta** .
2.  Busque la venta que desea cancelar y haga clic en **quitar** .
3.  Haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, haz clic en **Enviar a la Tienda** en el resumen del envío. Siempre que la venta no se haya iniciado en el momento en que el nuevo envío complete el proceso de certificación, la venta eliminada no se ejecutará.




