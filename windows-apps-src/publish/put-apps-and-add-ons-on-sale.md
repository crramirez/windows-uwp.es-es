---
author: jnHs
Description: You can promote your app or add-on in the Microsoft Store by putting it on sale for a limited time.
title: Poner a la venta aplicaciones y complementos
ms.assetid: 71ABA960-0CDC-4E35-A1C8-1D34B6673817
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a25980c964e399931a088e281e959df3095ffe9a
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "5472566"
---
# <a name="put-apps-and-add-ons-on-sale"></a>Poner a la venta aplicaciones y complementos

Puedes promocionar tu aplicación o complemento en Microsoft Store poniéndola a la venta durante un tiempo limitado. Puedes elegir ofrecer el producto en una franja de precios inferior o con un descuento basado en porcentajes.

> [!NOTE]
> No se admite el precio de oferta para complementos de suscripción.

Cuando usas la sección **Precio de oferta** de la página **Precios y disponibilidad** de un envío para reducir temporalmente el precio de la aplicación o complemento, los clientes que consulten la descripción de la Tienda verán el precio tachado, lo que indica que se ha reducido el precio (a diferencia de un [cambio de precio programado](set-and-schedule-app-pricing.md#schedule-price-changes), que puede aumentar o disminuir el precio sin que se muestre como un cambio en la Tienda). 

Durante el período de tiempo que tu producto esté rebajado, los clientes podrán comprarlo a un precio inferior durante el período de tiempo seleccionado. Si reduces el precio a **Gratis**, podrán descargarlo sin pagar nada durante el periodo de oferta.

> [!IMPORTANT]
> Precio de oferta solo se muestra a los clientes en los dispositivos Windows 10, incluida la Xbox One. Las ofertas ofrecidas a los propietarios de uno de tus otros productos solo se mostrarán a los clientes con dispositivos Windows10, versión 1607 o posterior.
> 
> En otros sistemas operativos, los clientes verán el precio normal de la aplicación o el complemento y no podrán adquirirlos al precio de oferta. Siempre puedes cambiar un precio eligiendo una franja de precios diferente en un nuevo envío, pero no se mostrará como una oferta por tiempo limitado.


## <a name="scheduling-a-sale"></a>Programar una oferta

Las ofertas se programan como parte del envío de una aplicación o un complemento. Si quieres programar una oferta para una aplicación o un complemento que ya se ha publicado, tendrás que crear un nuevo envío, aunque ese sea el único cambio que quieras realizar.

**Para programar una oferta**

1. En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta**.
2. Selecciona **Mostrar opciones** y, a continuación, selecciona **Nueva oferta**.
3. La ventana emergente **Selección de mercado** aparecerá y te permitirá crear un *grupo mercado* que especificará los mercados en los que se debe ofrecer la oferta. Puedes hacer clic en **Seleccionar todo** para ofrecer la oferta en todos los mercados en los que la aplicación esté disponible, seleccionar un mercado individual o varios mercados. De manera opcional, puedes escribir un nombre para tu grupo de mercado. Cuando hayas realizado tu selección, haz clic en **Crear**. (Para modificar los mercados en el grupo más adelante, haz clic en el nombre).

   > [!NOTE]
   > Las selecciones de mercado que realices en la sección Precio de oferta no afectarán a los mercados en los que se ofrece la aplicación. Estas selecciones solo determinan si se ofrece un precio de oferta y en qué mercados. Si estableces un precio de oferta para un mercado en el que la aplicación no está disponible, esto no hará que la aplicación esté disponible en ese mercado.
4. Elige una de las siguientes opciones para especificar el tipo de descuento:
   - **Precio**: Usa esta opción para seleccionar una franja de precios más bajos a los que se ofrecerá la aplicación. Puedes cambiar la moneda de la lista desplegable para seleccionar el precio en la moneda que prefieras. (El precio se convertirá en el nivel correspondiente para cada moneda. Para obtener información, consulta [Precio](set-app-pricing-and-availability.md)).
   - **Porcentaje**: Usa esta opción para seleccionar el porcentaje del descuento que se aplicará a la aplicación. El mismo porcentaje de descuento se usa para todas las monedas.
5. En la fila **Se ofrece a**, elige una de las opciones disponibles, entre las que se incluyen:
   - **Todos los usuarios**: la oferta se ofrecerá a todos los clientes.
   - **Los propietarios de**: la oferta se ofrecerá a los clientes que ya poseen una de las aplicaciones. Puedes seleccionar entre tus aplicaciones publicadas en la lista desplegable que aparecerá. Debes tener una o más aplicaciones publicadas para que esta opción esté disponible.

  > [!IMPORTANT]
  > Si seleccionas **Los propietarios de**, la oferta solo estará visible para los clientes de Windows10, versión 1607 o posterior.

   - **Grupo de usuarios conocido**: la oferta se ofrecerá a las personas en el [grupo de usuarios conocido](create-known-user-groups.md) que selecciones. Debes haber creado el grupo de usuarios conocido para que esta opción esté disponible.
   - **Segmento**: la oferta se ofrecerá a las personas en el segmento de clientes que selecciones. Puedes usar un [segmento que ya has creado](create-customer-segments.md) aquí. También puedes elegir **Comprador por primera vez** para ofrecer la venta solo a los clientes que nunca hayan comprado nada en la Tienda. Ofrecemos este segmento aquí porque nos dimos cuenta que una vez que un cliente realiza su primera compra en la Tienda, sigue realizando más, así que este puede ser un buen grupo para tentar con un precio de oferta.
6. Escribe la fecha y la hora del inicio y final del período de oferta. Elige una de las siguientes opciones de zonas horarias:
   - **UTC**: El tiempo que selecciones será el Horario universal coordinado (UTC), para que la oferta se produzca al mismo tiempo en todos los lugares.
   - **Local**: El tiempo que selecciones se usará en cada zona horaria asociada con un mercado. (Ten en cuenta para los mercados que tengan más de una zona horaria, se usará solo una zona horaria para ese mercado. Para los Estados Unidos, se usará la zona horario del Este).
7. Para programar una venta adicional, selecciona **Nueva oferta**. En caso contrario, haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, selecciona **Enviar a la Tienda** en el resumen del envío.

> [!NOTE]
> Es posible seleccionar una franja de precios mayor que el precio base de la aplicación. Sin embargo, el precio de oferta solo se mostrará a los clientes si es inferior al precio normal de la aplicación en ese mercado.
>
> Seleccionar un precio de oferta mayor que el precio base de la aplicación puede ser apropiado si ya has establecido en determinados mercados precios personalizados que superan el precio base y quieres reducir temporalmente el precio en dichos mercados (siendo el precio de venta todavía mayor que el precio base de la aplicación). Si tus selecciones resultan en que el precio de la aplicación aumente en un mercado concreto, dicho precio (superior) no se mostrará en ese mercado: los clientes seguirán viendo la aplicación con su precio anterior (inferior). También se mostrará a los clientes el menor precio disponible si programas distintas ofertas superpuestas con diferentes precios.

## <a name="changing-or-canceling-a-scheduled-sale"></a>Cambiar o cancelar una oferta programada

Para revisar o cancelar una oferta previamente programada para una aplicación o un complemento, debes crear y realizar un nuevo envío para la Tienda.

**Para editar una oferta programada**

1.  En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta**.
2.  Encuentra la oferta que quieras actualizar y, luego, realiza los cambios.
3.  Haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, haz clic en **Enviar a la Tienda** en el resumen del envío.

Cuando el envío supere el proceso de certificación, los cambios surtirán efecto.

> [!IMPORTANT]
> Si ya ha iniciado una oferta, no podrás editar la fecha de inicio. Aunque puedes editar la fecha de finalización, te recomendamos que no edites una oferta para que finalice antes de su fecha final original. Puede resultar frustrante para los clientes potenciales si finalizas una oferta antes de la fecha en que se publicó originalmente (dado que los clientes verán la fecha de finalización programada al ver descripción de la aplicación en la Tienda).

 **Cancelar una oferta que no se ha iniciado todavía**

1.  En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta**.
2.  Encuentra la oferta que quieres cancelar y haz clic en **Eliminar**.
3.  Haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, haz clic en **Enviar a la Tienda** en el resumen del envío. Si la oferta no ha comenzado en el momento en el que el nuevo envío complete el proceso de certificación, la oferta eliminada no llegará a entrar en efecto.




