---
Description: Cuando envías un complemento, las opciones de la página Precios y disponibilidad determinan lo que se cobrará por tu complemento y cómo se debe ofrecer a los clientes.
title: Establecer los precios y la disponibilidad de los complementos
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, complementos, iap, precio
ms.localizationpriority: medium
ms.openlocfilehash: 062337c82d2567d15b0eff1767ab157618da257e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597810"
---
# <a name="set-add-on-pricing-and-availability"></a>Establecer los precios y la disponibilidad de los complementos

Al enviar un complemento de [centro de partners](https://partner.microsoft.com/dashboard), las opciones de la **precios y disponibilidad** página determinar cuánto a cobrar a los clientes para su complemento y cómo debe ofrecer a los clientes.

## <a name="markets"></a>Mercados

De manera predeterminada, tu complemento figurará en todos los mercados posibles, incluidos los mercados futuros que podamos agregar posteriormente, al precio base.

Sin embargo, al igual que con una aplicación, tienes la opción de elegir los mercados en los que quieres ofrecer el complemento. En la mayoría de los casos, querrás elegir el mismo conjunto de mercados que la aplicación, pero tienes la flexibilidad para realizar los cambios necesarios. 

Para obtener información y una lista completa de los mercados disponibles, consulta [Definir la selección de mercados](define-pricing-and-market-selection.md).

## <a name="visibility"></a>Visibilidad

Puedes determinar si debes ofrecer el complemento para su compra a los clientes. 

La opción predeterminada es **Puede mostrarse en la descripción de la Tienda del producto principal**. Deja esta opción activada para los complementos que estarán disponibles para cualquier cliente. 

En el caso delos complementos que no quieres que estén ampliamente disponibles, selecciona **Oculto en la Tienda** y una de las siguientes opciones:

-   **Disponible para su compra desde dentro del producto principal solo**: Si elige esta opción permite que cualquier cliente pueda adquirir el complemento desde dentro de la aplicación, pero el complemento no se mostrarán en la lista de la aplicación Store ni sea detectable en el Store. Úsalo solo cuando la oferta no esté ampliamente disponible, por ejemplo durante los períodos iniciales de pruebas internas.
-   **Detener la adquisición: Cualquier cliente con un vínculo directo puede ver Store del producto en la lista, pero solo puede descargar si pertenece el producto antes de, o tienen un código promocional y está usando un dispositivo Windows 10. Este complemento no se muestra en el listado del producto padre**: Si se selecciona esta opción, significa que el complemento no se mostrará en la descripción de la aplicación y no lo podrán comprar clientes nuevos. Sin embargo, **esta opción no se admite para los clientes en Windows 8.1 o versiones anteriores**. Si la aplicación publicada anteriormente está disponible en Windows 8.1 o versiones anteriores, el complemento aún estará disponible para su compra a los clientes. Para dejar de ofrecer el complemento a los clientes en Windows 8.1 o versiones anteriores, deberá actualizar la aplicación para quitar el código que ofrece el complemento y, después, publicar un nuevo envío de la aplicación. Esto se recomienda incluso si la aplicación de destino no es Windows 8.1 o versiones anteriores; es una mejor experiencia para los clientes si nunca les ofrece un complemento que se ha desactivado para que no esté disponible.
    
 > [!NOTE] 
 > Elegir la opción **Detener la compra** o enviar una actualización de aplicaciones que quite el complemento del código de la aplicación no afectan a los clientes que ya hayan comprado el complemento, independientemente de su sistema operativo.


## <a name="schedule"></a>Programa

De manera predeterminada (a menos que hayas seleccionado una de la opciones **Oculto en la Tienda** en la sección **Visibilidad**), el complemento estará disponible para todos los clientes tan pronto como supere la certificación y complete el proceso de publicación. Para elegir otras fechas, seleccione **Mostrar opciones** para expandir la sección. 

Para obtener más información, consulta [Configurar la programación de lanzamiento precisa](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Precios

Debe seleccionar un precio base para el complemento (a menos que haya seleccionado el **detener adquisición** opción el **visibilidad** sección). La selección predeterminada es **gratis**, por lo que si desea que se cobran por el complemento, asegúrese de elegir uno de los niveles de precios disponible (comenzando por.99 USD).

También puedes programar los cambios de precio para indicar la fecha y la hora en las que el precio del complemento debería cambiar. Además, tienes la opción de personalizar estos cambios para mercados específicos. 

> [!TIP]
> Para complementos de suscripción, no puede elevar el precio después de publicar el complemento, seleccionando un precio base más alto en un envío posterior o mediante la programación de un cambio de precio que aumenta el precio. Puede seleccionar un precio inferior con cualquiera de estos métodos, pero una vez que se reduce el precio no podrá generar mayor que ese nuevo precio. Por este motivo, es especialmente importante para asegurarse de que se seleccione el nivel de precios adecuado para complementos de suscripción. 

Para obtener más información, consulta [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Precio de oferta

Si quieres ofrecer tu complemento a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta. Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](put-apps-and-add-ons-on-sale.md).



