---
Description: Puedes promocionar tu aplicación o complemento en Microsoft Store poniéndola a la venta durante un tiempo limitado.
title: Poner aplicaciones y complementos en oferta
ms.assetid: 71ABA960-0CDC-4E35-A1C8-1D34B6673817
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3ec71453cd03359dc6e1b72af2f6a43805bcb5ff
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63788361"
---
# <a name="put-apps-and-add-ons-on-sale"></a>Poner aplicaciones y complementos en oferta

Puedes promocionar tu aplicación o complemento en Microsoft Store poniéndola a la venta durante un tiempo limitado. Puedes elegir ofrecer el producto en una franja de precios inferior o con un descuento basado en porcentajes. Y puede elegir si desea ofrecer la venta a todos los usuarios o realizar una oferta exclusiva para los clientes que dispone de uno de los otros productos.

> [!NOTE]
> Precios de venta no se admiten para complementos de suscripción.

Cuando usas la sección **Precio de oferta** de la página **Precios y disponibilidad** de un envío para reducir temporalmente el precio de la aplicación o complemento, los clientes que consulten la descripción de la Tienda verán el precio tachado, lo que indica que se ha reducido el precio (a diferencia de un [cambio de precio programado](set-and-schedule-app-pricing.md#schedule-price-changes), que puede aumentar o disminuir el precio sin que se muestre como un cambio en la Tienda). 

Durante el período de tiempo que tu producto esté rebajado, los clientes podrán comprarlo a un precio inferior durante el período de tiempo seleccionado. Si reduces el precio a **Gratis**, podrán descargarla sin pagar nada durante el periodo de oferta.

> [!IMPORTANT]
> Precios de venta solo se muestran a los clientes en dispositivos Windows 10, incluidos Xbox One. Ventas que ofrecer sólo a los propietarios de uno de los otros productos se muestran solo a los clientes de Windows 10, versión 1607 o posterior.
> 
> En otros sistemas operativos, los clientes verán el precio normal de la aplicación o el complemento y no podrán adquirirlos al precio de oferta. Siempre puedes cambiar un precio eligiendo una franja de precios diferente en un nuevo envío, pero no se mostrará como una oferta por tiempo limitado.


## <a name="scheduling-a-sale"></a>Programar una oferta

Las ofertas se programan como parte del envío de una aplicación o un complemento. Si quieres programar una oferta para una aplicación o un complemento que ya se ha publicado, tendrás que crear un nuevo envío, aunque ese sea el único cambio que quieras realizar.

**Para programar una venta**

1. En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta**.
2. Selecciona **Mostrar opciones** y, a continuación, selecciona **Nueva oferta**.
3. La ventana emergente **Selección de mercado** aparecerá y te permitirá crear un *grupo mercado* que especificará los mercados en los que se debe ofrecer la oferta. Puedes hacer clic en **Seleccionar todo** para ofrecer la oferta en todos los mercados en los que la aplicación esté disponible, seleccionar un mercado individual o varios mercados. De manera opcional, puedes escribir un nombre para tu grupo de mercado. Cuando hayas realizado tu selección, haz clic en **Crear**. (Para modificar los mercados en el grupo más adelante, haz clic en el nombre).

   > [!NOTE]
   > Las selecciones de mercado que realices en la sección Precio de oferta no afectarán a los mercados en los que se ofrece la aplicación. Estas selecciones solo determinan si se ofrece un precio de oferta y en qué mercados. Si estableces un precio de oferta para un mercado en el que la aplicación no está disponible, esto no hará que la aplicación esté disponible en ese mercado.
4. Elige una de las siguientes opciones para especificar el tipo de descuento:
   - **Precio**: Utilice esta opción para seleccionar un plan de precios inferior a la que se ofrecerán la aplicación. Puedes cambiar la moneda de la lista desplegable para seleccionar el precio en la moneda que prefieras. (El precio se convertirá en el nivel correspondiente para cada moneda. Para obtener información, consulta [Precio](set-app-pricing-and-availability.md)).
   - **Porcentaje**: Use esta opción para seleccionar el porcentaje para obtener un descuento que se aplicará a la aplicación. El mismo porcentaje de descuento se usa para todas las monedas.
5. En la fila **Se ofrece a**, elige una de las opciones disponibles, entre las que se incluyen:
   - **Todo el mundo**: Se ofrecerán la venta a todos los clientes.
   - **Los propietarios de**: La venta se ofrecerán a los clientes que ya poseen una de las aplicaciones. Puedes seleccionar entre tus aplicaciones publicadas en la lista desplegable que aparecerá. Debes tener una o más aplicaciones publicadas para que esta opción esté disponible.

  > [!IMPORTANT]
  > Si seleccionas **Los propietarios de**, la oferta solo estará visible para los clientes de Windows 10, versión 1607 o posterior.

   - **Grupo de usuarios conocidos**: Se ofrecerán la venta a las personas de la [grupo de usuarios conocidos](create-known-user-groups.md) seleccione. Debes haber creado el grupo de usuarios conocido para que esta opción esté disponible.
   - **Segmento**: Se ofrecerán la venta a las personas en el segmento de clientes que seleccione. Puedes usar un [segmento que ya has creado](create-customer-segments.md) aquí. También puedes elegir **Comprador por primera vez** para ofrecer la venta solo a los clientes que nunca hayan comprado nada en la Tienda. Ofrecemos este segmento aquí porque nos dimos cuenta que una vez que un cliente realiza su primera compra en la Tienda, sigue realizando más, así que este puede ser un buen grupo para tentar con un precio de oferta.
6. Escribe la fecha y la hora del inicio y final del período de oferta. Elige una de las siguientes opciones de zonas horarias:
   - **UTC**: La hora que seleccione será el tiempo de hora Universal coordinada (UTC), para que se produzca la venta al mismo tiempo en todas partes.
   - **Local**: La hora que seleccione se usará en cada zona horaria asociada con un mercado. (Ten en cuenta para los mercados que tengan más de una zona horaria, se usará solo una zona horaria para ese mercado. Para los Estados Unidos, se usará la zona horario del Este).
7. Para programar una venta adicional, selecciona **Nueva oferta**. En caso contrario, haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, selecciona **Enviar a la Tienda** en el resumen del envío.

> [!NOTE]
> Es posible seleccionar una franja de precios mayor que el precio base de la aplicación. Sin embargo, el precio de oferta solo se mostrará a los clientes si es inferior al precio normal de la aplicación en ese mercado.
>
> Seleccionar un precio de oferta mayor que el precio base de la aplicación puede ser apropiado si ya has establecido en determinados mercados precios personalizados que superan el precio base y quieres reducir temporalmente el precio en dichos mercados (siendo el precio de venta todavía mayor que el precio base de la aplicación). Si tus selecciones resultan en que el precio de la aplicación aumente en un mercado concreto, dicho precio (superior) no se mostrará en ese mercado: los clientes seguirán viendo la aplicación con su precio anterior (inferior). También se mostrará a los clientes el menor precio disponible si programas distintas ofertas superpuestas con diferentes precios.

## <a name="changing-or-canceling-a-scheduled-sale"></a>Cambiar o cancelar una oferta programada

Para revisar o cancelar una oferta previamente programada para una aplicación o un complemento, debes crear y realizar un nuevo envío para la Tienda.

**Para editar una venta programada**

1.  En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta**.
2.  Encuentra la oferta que quieras actualizar y, luego, realiza los cambios.
3.  Haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, haz clic en **Enviar a la Tienda** en el resumen del envío.

Cuando el envío supere el proceso de certificación, los cambios surtirán efecto.

> [!IMPORTANT]
> Si ya ha iniciado una oferta, no podrás editar la fecha de inicio. Aunque puedes editar la fecha de finalización, te recomendamos que no edites una oferta para que finalice antes de su fecha final original. Puede resultar frustrante para los clientes potenciales si finalizas una oferta antes de la fecha en que se publicó originalmente (dado que los clientes verán la fecha de finalización programada al ver descripción de la aplicación en la Tienda).

 **Para cancelar una venta que aún no ha iniciado**

1.  En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta**.
2.  Encuentra la oferta que quieres cancelar y haz clic en **Eliminar**.
3.  Haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, haz clic en **Enviar a la Tienda** en el resumen del envío. Si la oferta no ha comenzado en el momento en el que el nuevo envío complete el proceso de certificación, la oferta eliminada no llegará a entrar en efecto.




