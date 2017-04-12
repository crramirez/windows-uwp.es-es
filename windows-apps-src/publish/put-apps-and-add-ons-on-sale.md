---
author: jnHs
Description: "Puedes promocionar tu aplicación o complemento en la Tienda Windows al ponerlos en oferta durante un tiempo limitado."
title: Poner aplicaciones y complementos en oferta
ms.assetid: 71ABA960-0CDC-4E35-A1C8-1D34B6673817
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 7c88e927a88dbef928deedd6f92b2aa7b60da1cf
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="put-apps-and-add-ons-on-sale"></a>Poner aplicaciones y complementos en oferta

Puedes promocionar tu aplicación o complemento en la Tienda Windows al ponerlos en oferta durante un tiempo limitado.

Cuando programes una oferta para reducir temporalmente el precio de tu aplicación o complemento, los clientes que consulten la descripción de la Tienda verán que el precio ha bajado y podrán comprarlos al precio reducido durante el período que hayas seleccionado. Si reduces el precio a **Gratis**, podrán descargarla sin pagar nada durante el periodo de oferta.

> **Nota**  El precio de oferta solo se muestra a tus clientes de Windows 10. En otros sistemas operativos, los clientes verán el precio normal de la aplicación o el complemento. Siempre puedes cambiar un precio eligiendo una franja de precios diferente en un nuevo envío, pero no se mostrará como una oferta por tiempo limitado.

## <a name="scheduling-a-sale"></a>Programar una oferta

Las ofertas se programan como parte del envío de una aplicación o un complemento. Si quieres programar una oferta para una aplicación o un complemento que ya se ha publicado, tendrás que crear un nuevo envío, aunque ese sea el único cambio que quieras realizar.

**Para programar una oferta**

1.  En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta**.
2.  Haz clic en **Nueva oferta**.
3.  Escribe la fecha y la hora del inicio y final del período de oferta. Las horas se muestran en hora UTC.

   > **Nota**  Para los complementos, no puedes programar ofertas que se superpongan entre sí.

4.  Elige el precio de oferta en la lista desplegable. Puedes elegir cualquier precio, incluido **Gratis**.
5.  Si quieres especificar precios personalizados para esta oferta, haz clic en **Mostrar las opciones de precio de mercado personalizadas**. Puedes establecer precios de oferta personalizados por mercado (o excluir mercados específicos de la oferta) aquí. Para más información, consulta [Definir la selección de precios y mercados](define-pricing-and-market-selection.md).

    > **Nota**  Las selecciones de mercado que realices en la sección **Precio de oferta** no afectarán a los mercados en los que se ofrece la aplicación. Estas selecciones solo determinan si se ofrece un precio de oferta y en qué mercados. Si estableces un precio de oferta para un mercado en el que la aplicación no está disponible, esto no hará que la aplicación esté disponible en ese mercado.

6.  Haz clic en **Listo** para guardar la oferta programada.
7.  Haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, haz clic en **Enviar a la Tienda** en el resumen del envío.

> **Nota**  Es posible seleccionar una franja de precios mayor que el precio base de la aplicación. Sin embargo, el precio de oferta solo se mostrará a los clientes si es inferior al precio normal de la aplicación en ese mercado. Seleccionar un precio de oferta mayor que el precio base de la aplicación puede ser apropiado si ya has establecido en determinados mercados precios personalizados que superan el precio base y quieres reducir temporalmente el precio en dichos mercados (siendo el precio de venta todavía mayor que el precio base de la aplicación). Si tus selecciones resultan en que el precio de la aplicación aumente en un mercado concreto, dicho precio (superior) no se mostrará en ese mercado: los clientes seguirán viendo la aplicación con su precio anterior (inferior). También se mostrará a los clientes el menor precio disponible si programas distintas ofertas superpuestas con diferentes precios.

## <a name="changing-or-canceling-a-scheduled-sale"></a>Cambiar o cancelar una oferta programada


Para revisar o cancelar una oferta previamente programada para una aplicación o un complemento, debes crear y realizar un nuevo envío para la Tienda.

**Para editar una oferta programada**

1.  En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta**.
2.  Busca la oferta que quieres actualizar y haz clic en su precio para editarla.
3.  Realiza los cambios y luego haz clic en **Listo**.
4.  Haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, haz clic en **Enviar a la Tienda** en el resumen del envío.

Cuando el envío supere el proceso de certificación, los cambios surtirán efecto (aunque la oferta ya haya comenzado).

> **Sugerencia**  Para volver a usar una oferta completada en un nuevo envío, puedes editar sus fechas de inicio y fin. Esto es especialmente útil si configuraste una oferta con precios complicados de mercado personalizado.
 
**Para cancelar una oferta programada**

1.  En la página **Precios y disponibilidad** de un envío en curso de una aplicación o un complemento, ve a la sección **Precio de oferta**.
2.  Encuentra la oferta que quieres cancelar y haz clic en **Eliminar** para quitarla.
3.  Haz clic en **Guardar** en la parte inferior de la página **Precios y disponibilidad** y, después, haz clic en **Enviar a la Tienda** en el resumen del envío.

Si la oferta no ha comenzado en el momento de completarse el proceso de certificación, nunca llegará a entrar en efecto. Si eliminas una oferta que ya ha finalizado, simplemente se quitará de tu página **Precios y disponibilidad**.

> **Importante**  Dado que los clientes pueden ver la fecha de finalización programada en la descripción de la Tienda que se muestra la aplicación, no es recomendable eliminar una oferta una vez iniciada. Si eliminas una oferta que ya está en curso, terminará cuando el envío complete el proceso de certificación, lo que puede resultar frustrante para los clientes potenciales.

