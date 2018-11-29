---
Description: When submitting an add-on, the options on the Pricing and availability page determine what to charge for your add-on and how it should be offered to customers.
title: Establecer los precios y la disponibilidad de los complementos
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, complementos, iap, precio
ms.localizationpriority: medium
ms.openlocfilehash: 062337c82d2567d15b0eff1767ab157618da257e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "7983062"
---
# <a name="set-add-on-pricing-and-availability"></a>Establecer los precios y la disponibilidad de los complementos

Cuando envías un complemento en [El centro de partners](https://partner.microsoft.com/dashboard), las opciones de la página **precios y disponibilidad** determinan cuánto cargo en los clientes de tu complemento y cómo se debe ofrecer a los clientes.

## <a name="markets"></a>Mercados

De manera predeterminada, tu complemento figurará en todos los mercados posibles, incluidos los mercados futuros que podamos agregar posteriormente, al precio base.

Sin embargo, al igual que con una aplicación, tienes la opción de elegir los mercados en los que quieres ofrecer el complemento. En la mayoría de los casos, querrás elegir el mismo conjunto de mercados que la aplicación, pero tienes la flexibilidad para realizar los cambios necesarios. 

Para obtener información y una lista completa de los mercados disponibles, consulta [Definir la selección de mercados](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilidad

Puedes determinar si debes ofrecer el complemento para su compra a los clientes. 

La opción predeterminada es **Puede mostrarse en la descripción de la Tienda del producto principal**. Deja esta opción activada para los complementos que estarán disponibles para cualquier cliente. 

En el caso delos complementos que no quieres que estén ampliamente disponibles, selecciona **Oculto en la Tienda** y una de las siguientes opciones:

-   **Disponible para su compra desde el producto principal solo**: elegir esta opción permite a los clientes comprar el complemento desde dentro de la aplicación, pero el complemento no se muestran en la descripción de la tienda de la aplicación o detectable en Store. Usa esta opción solo cuando la oferta no esté ampliamente disponible, por ejemplo, durante los períodos iniciales de pruebas internas.
-   **Detener la compra: Los clientes con un vínculo directo podrán ver la descripción de la aplicación en la Tienda, pero solo podrán descargarla si ya tienen el producto, tienen un código promocional y están usando un dispositivo Windows 10. Este complemento no se muestra en la descripción del producto principal**: elegir esta opción significa que el complemento no se mostrará en la descripción de la aplicación y los nuevos clientes no podrán comprar el complemento. Sin embargo, **esta opción no se admite para los clientes en Windows8.1 o versiones anteriores**. Si la aplicación publicada anteriormente está disponible en Windows8.1 o versiones anteriores, el complemento seguirá estando disponible para su compra para dichos clientes. Para dejar de ofrecer el complemento a los clientes en Windows8.1 o versiones anteriores, tendrás que actualizar la aplicación para quitar el código que ofrece el complemento, a continuación, publicar un nuevo envío de la aplicación. Esto se recomienda incluso si la aplicación no destinarse Windows8.1 o versiones anteriores; es una mejor experiencia para los clientes si nunca les ofreces un complemento que has optado por que no esté disponible.
    
 > [!NOTE] 
 > Elegir la opción **Detener la compra** o enviar una actualización de aplicaciones que quite el complemento del código de la aplicación no afectan a los clientes que ya hayan comprado el complemento, independientemente de su sistema operativo.


## <a name="schedule"></a>Programación

De manera predeterminada (a menos que hayas seleccionado una de la opciones **Oculto en la Tienda** en la sección **Visibilidad**), el complemento estará disponible para todos los clientes tan pronto como supere la certificación y complete el proceso de publicación. Para elegir otras fechas, selecciona **Mostrar opciones** para expandir la sección. 

Para obtener más información, consulta [Configurar la programación de lanzamiento precisa](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Precios

Debes seleccionar un precio base para el complemento (a menos que hayas seleccionado la opción de **Detener la compra** en la sección **visibilidad** ). La selección predeterminada es **gratuito**, por lo tanto, si quieres cobrar dinero por el complemento, asegúrate de elegir una de las franjas de precios disponibles (a partir de.99 USD).

También puedes programar los cambios de precio para indicar la fecha y la hora en las que el precio del complemento debería cambiar. Además, tienes la opción de personalizar estos cambios para mercados específicos. 

> [!TIP]
> Complementos de suscripción, no puedes Subir el precio de después de publicar el complemento, al seleccionar un precio base superior en envíos posteriores o mediante programación un cambio de precio que aumenta el precio. Puedes seleccionar un precio más bajo usar cualquiera de estos métodos, pero una vez que se reduce el precio no podrás generarlo mediante ese precio nuevo. Por este motivo, es especialmente importante para asegurarse de seleccionar la franja de precios adecuados para los complementos de suscripción. 

Para obtener más información, consulta [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Precio de oferta

Si quieres ofrecer tu complemento a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta. Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](put-apps-and-add-ons-on-sale.md).



