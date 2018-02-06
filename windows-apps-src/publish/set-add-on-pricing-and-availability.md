---
author: jnHs
Description: When submitting an add-on, the options on the Pricing and availability page determine what to charge for your add-on and how it should be offered to customers.
title: Establecer los precios y la disponibilidad de los complementos
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, complementos, iap, precio
ms.localizationpriority: high
ms.openlocfilehash: 8eb2321ec6d2bc8602438e2dc66ca1d96690a3f8
ms.sourcegitcommit: 446fe2861651f51a129baa80791f565f81b4f317
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2018
---
# <a name="set-add-on-pricing-and-availability"></a>Establecer los precios y la disponibilidad de los complementos


Cuando envías un complemento, las opciones de la página **Precios y disponibilidad** determinan lo que se cobrará por tu complemento y cómo se debe ofrecer a los clientes.

## <a name="markets"></a>Mercados

De manera predeterminada, tu complemento figurará en todos los mercados posibles, incluidos los mercados futuros que podamos agregar posteriormente, al precio base.

Sin embargo, al igual que con una aplicación, tienes la opción de elegir los mercados en los que quieres ofrecer el complemento. En la mayoría de los casos, querrás elegir el mismo conjunto de mercados que la aplicación, pero tienes la flexibilidad para realizar los cambios necesarios. 

Para obtener información y una lista completa de los mercados disponibles, consulta [Definir la selección de mercados](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilidad

Puedes determinar si debes ofrecer el complemento para su compra a los clientes. 

La opción predeterminada es **Puede mostrarse en la descripción de la Tienda del producto principal**. Deja esta opción activada para los complementos que estarán disponibles para cualquier cliente. 

En el caso delos complementos que no quieres que estén ampliamente disponibles, selecciona **Oculto en la Tienda** y una de las siguientes opciones:

-   **Disponible para la compra solo desde el producto principal**: elegir esta opción permite a los clientes comprar el complemento desde tu aplicación, pero el complemento no se mostrará en la descripción de la Tienda de la aplicación. Usa esta opción solo cuando la oferta no esté ampliamente disponible, por ejemplo, durante los períodos iniciales de pruebas internas.
-   **Detener la compra: Los clientes con un vínculo directo podrán ver la descripción de la aplicación en la Tienda, pero solo podrán descargarla si ya tienen el producto, tienen un código promocional y están usando un dispositivo Windows 10. Este complemento no se muestra en la descripción del producto principal**: elegir esta opción significa que el complemento no se mostrará en la descripción de la aplicación y los nuevos clientes no podrán comprar el complemento. Sin embargo, **esta opción no se admite para los clientes en Windows 8.1 o versiones anteriores**. Si la aplicación está disponible en Windows 8.1 o versiones anteriores, el complemento seguirá estando disponible para su compra para estos clientes. Para dejar de ofrecer el complemento a los clientes en Windows 8.1 o versiones anteriores, debes actualizar la aplicación para quitar el código que ofrece el complemento y publicar un nuevo envío de la aplicación. Esto se recomienda incluso si la aplicación no está destinada a Windows 8.1 o una versión anterior; es una experiencia mejor para tus clientes si nunca les ofreces un complemento respecto al cual has optado por que no esté disponible.
    
 > [!NOTE] 
 > Elegir la opción **Detener la compra** o enviar una actualización de aplicaciones que quite el complemento del código de la aplicación no afectan a los clientes que ya hayan comprado el complemento, independientemente de su sistema operativo.


## <a name="schedule"></a>Programación

De manera predeterminada (a menos que hayas seleccionado una de la opciones **Oculto en la Tienda** en la sección **Visibilidad**), el complemento estará disponible para todos los clientes tan pronto como supere la certificación y complete el proceso de publicación. Para elegir otras fechas, selecciona **Mostrar opciones** para expandir la sección. 

Para obtener más información, consulta [Configurar la programación de lanzamiento precisa](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Precios

Debes seleccionar un precio base del complemento (a menos que hayas seleccionado la opción **Detener la compra** de la sección **Visibilidad**), seleccionando **Gratis** o una de las franjas de precios disponibles (a partir de 0,99USD).

También puedes programar los cambios de precio para indicar la fecha y la hora en las que el precio del complemento debería cambiar. Además, tienes la opción de personalizar estos cambios para mercados específicos. 

Para obtener más información, consulta [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Precio de oferta

Si quieres ofrecer tu complemento a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta. Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](put-apps-and-add-ons-on-sale.md).


## <a name="publish-date"></a>Fecha de publicación

De manera predeterminada, tu envío comenzará el proceso de publicación en cuanto supere la certificación, a menos que hayas configurado las fechas en la [sección **Programación**](#schedule) descrita anteriormente. 

Para controlar cuándo debe publicarse el complemento en la Tienda, usa la sección **Programación**. Para la mayoría de los envíos, debes usar esa sección para programar el lanzamiento y dejar la sección **Fecha de publicación** establecida en la opción predeterminada, **Publish this submission as soon as it passes certification**. Esto evitará que el envío se publique antes de las fechas establecidas en la sección **Programación**. Las fechas que seleccionaste en la sección **Programación** determinarán cuándo el complemento estará disponible para los clientes en la Tienda.

Si no deseas establecer una fecha de lanzamiento todavía y prefieres que el envío permanezca sin publicar hasta que decidas iniciar manualmente el proceso de publicación, puedes elegir **Publicar este envío manualmente**. Elegir esta opción, significa que la selección no se publicará hasta que tú lo indiques. Cuando el complemento supere la certificación, puedes publicarlo seleccionando **Publicar ahora** en la página de estado de certificación, o seleccionando una fecha específica, como se describe a continuación.

Elige **No antes del \[fecha\]** para garantizar que el envío no se publique hasta una fecha determinada. Con esta opción, el envío se lanzará lo antes posible en la fecha que especifiques o después de ella. La fecha debe ser al menos 24 horas después del momento actual. Junto con la fecha, también puedes especificar la hora en que el envío debe comenzar a publicarse.
 
> [!NOTE]
> Los retrasos durante la certificación o la publicación podrían hacer que la fecha de lanzamiento real sea posterior a la que solicites. Microsoft Store no puede garantizar que el complemento (o la actualización) esté disponible en una fecha específica.  



 




