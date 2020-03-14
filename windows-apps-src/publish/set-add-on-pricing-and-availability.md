---
Description: Cuando envías un complemento, las opciones de la página Precios y disponibilidad determinan lo que se cobrará por tu complemento y cómo se debe ofrecer a los clientes.
title: Establecer los precios y la disponibilidad de los complementos
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, complementos, iap, precio
ms.localizationpriority: medium
ms.openlocfilehash: 803164c395602313bcb84331e30376efd6832731
ms.sourcegitcommit: 912146681b1befc43e6db6e06d1e3317e5987592
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79295728"
---
# <a name="set-add-on-pricing-and-availability"></a>Establecer los precios y la disponibilidad de los complementos

Al enviar un complemento en el centro de [Partners](https://partner.microsoft.com/dashboard), las opciones de la página **precios y disponibilidad** determinan la cantidad de carga de los clientes para el complemento y cómo se debe ofrecer a los clientes.

## <a name="markets"></a>Mercados

De manera predeterminada, tu complemento figurará en todos los mercados posibles, incluidos los mercados futuros que podamos agregar posteriormente, al precio base.

Sin embargo, al igual que con una aplicación, tienes la opción de elegir los mercados en los que quieres ofrecer el complemento. En la mayoría de los casos, querrás elegir el mismo conjunto de mercados que la aplicación, pero tienes la flexibilidad para realizar los cambios necesarios. 

Para obtener información y una lista completa de los mercados disponibles, consulta [Definir la selección de mercados](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilidad

Puedes determinar si debes ofrecer el complemento para su compra a los clientes. 

La opción predeterminada es **Puede mostrarse en la descripción de la Tienda del producto principal**. Deja esta opción activada para los complementos que estarán disponibles para cualquier cliente. 

En el caso delos complementos que no quieres que estén ampliamente disponibles, selecciona **Oculto en la Tienda** y una de las siguientes opciones:

-   **Solo está disponible para su compra en el producto primario**: la elección de esta opción permite que cualquier cliente compre el complemento desde dentro de la aplicación, pero el complemento no se mostrará en la lista de la tienda de la aplicación ni se podrá detectar en el almacén. Úsalo solo cuando la oferta no esté ampliamente disponible, por ejemplo durante los períodos iniciales de pruebas internas.
-   **Detener la compra: Los clientes con un vínculo directo podrán ver la descripción de la aplicación en la Tienda, pero solo podrán descargarla si ya tienen el producto, tienen un código promocional y están usando un dispositivo Windows 10. Este complemento no se muestra en la descripción del producto principal**: elegir esta opción significa que el complemento no se mostrará en la descripción de la aplicación y los nuevos clientes no podrán comprar el complemento. Sin embargo, **esta opción no se admite para los clientes de Windows 8.1 o versiones anteriores**. Si la aplicación publicada anteriormente está disponible en Windows 8.1 o versiones anteriores, el complemento seguirá estando disponible para su compra a esos clientes. Para dejar de ofrecer el complemento a los clientes en Windows 8.1 o versiones anteriores, debe actualizar la aplicación para quitar el código que ofrece el complemento y luego publicar un nuevo envío para la aplicación. Esto se recomienda incluso si la aplicación no tiene como destino Windows 8.1 o versiones anteriores; es una mejor experiencia para los clientes si nunca les ofrece un complemento que ha optado por no estar disponible.
    
 > [!NOTE] 
 > La elección de la opción **detener adquisición** o el envío de una actualización de la aplicación que quite el complemento de la aplicación no impedirá que los clientes usen el complemento si ya lo han comprado. Las suscripciones existentes no se renovarán y, posteriormente, se cancelarán una vez finalizado el período actual.


## <a name="schedule"></a>Programa

De manera predeterminada (a menos que hayas seleccionado una de la opciones **Oculto en la Tienda** en la sección **Visibilidad**), el complemento estará disponible para todos los clientes tan pronto como supere la certificación y complete el proceso de publicación. Para elegir otras fechas, seleccione **Mostrar opciones** para expandir la sección. 

Para obtener más información, consulta [Configurar la programación de lanzamiento precisa](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Precio

Debe seleccionar un precio base para el complemento (a menos que haya seleccionado la opción **detener adquisición** en la sección **visibilidad** ). La selección predeterminada es **gratuita**, por lo que si desea cobrar dinero por el complemento, asegúrese de elegir uno de los planes de tarifa disponibles (a partir de 99 USD).

También puedes programar los cambios de precio para indicar la fecha y la hora en las que el precio del complemento debería cambiar. Además, tienes la opción de personalizar estos cambios para mercados específicos. 

> [!TIP]
> En el caso de los complementos de suscripción, no puede aumentar el precio después de publicar el complemento, ya sea seleccionando un precio base mayor en un envío posterior o programando un cambio de precio que aumente el precio. Puede seleccionar un precio más bajo con cualquiera de estos métodos, pero una vez que se reduce el precio, no podrá aumentarlo por encima del precio nuevo. Por este motivo, es especialmente importante asegurarse de seleccionar el nivel de precios adecuado para los complementos de suscripción. 

Para obtener más información, consulta [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Precio de oferta

Si quieres ofrecer tu complemento a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta. Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](put-apps-and-add-ons-on-sale.md).



