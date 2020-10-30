---
description: Cuando envías un complemento, las opciones de la página Precios y disponibilidad determinan lo que se cobrará por tu complemento y cómo se debe ofrecer a los clientes.
title: Establecer los precios y la disponibilidad de los complementos
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, complementos, IAP, precio
ms.localizationpriority: medium
ms.openlocfilehash: 5aa63307a232eed0e3fdb2e6d1355665d964212b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029978"
---
# <a name="set-add-on-pricing-and-availability"></a>Establecer los precios y la disponibilidad de los complementos

Al enviar un complemento en el centro de [Partners](https://partner.microsoft.com/dashboard), las opciones de la página **precios y disponibilidad** determinan la cantidad de carga de los clientes para el complemento y cómo se debe ofrecer a los clientes.

## <a name="markets"></a>Mercados

De manera predeterminada, tu complemento figurará en todos los mercados posibles, incluidos los mercados futuros que podamos agregar posteriormente, al precio base.

Sin embargo, al igual que con una aplicación, tienes la opción de elegir los mercados en los que quieres ofrecer el complemento. En la mayoría de los casos, querrás elegir el mismo conjunto de mercados que la aplicación, pero tienes la flexibilidad para realizar los cambios necesarios. 

Para obtener más información y una lista completa de los mercados disponibles, consulte [definir la selección del mercado](./define-market-selection.md).

## <a name="visibility"></a>Visibilidad

Puedes determinar si debes ofrecer el complemento para su compra a los clientes. 

La opción predeterminada se **puede mostrar en la lista de tiendas del producto principal** . Deja esta opción activada para los complementos que estarán disponibles para cualquier cliente. 

En el caso de complementos que no quiere que estén disponibles en general, seleccione **oculto en el almacén** y una de las siguientes opciones:

-   **Solo está disponible para su compra en el producto primario** : la elección de esta opción permite que cualquier cliente compre el complemento desde dentro de la aplicación, pero el complemento no se mostrará en la lista de la tienda de la aplicación ni se podrá detectar en el almacén. Úsalo solo cuando la oferta no esté ampliamente disponible, por ejemplo durante los períodos iniciales de pruebas internas.
-   **Detener la adquisición: cualquier cliente con un vínculo directo puede ver la lista de la tienda del producto, pero solo puede descargarla si poseía el producto antes o tiene un código promocional y está usando un dispositivo de Windows 10. Este complemento no se muestra en la lista del producto primario** : Si elige esta opción, el complemento no se mostrará en la lista de la aplicación y ningún cliente nuevo podrá comprar el complemento. Sin embargo, **esta opción no se admite para los clientes en Windows 8.1 o versiones anteriores** . Si la aplicación publicada anteriormente está disponible en Windows 8.1 o versiones anteriores, el complemento seguirá estando disponible para su compra a esos clientes. Para dejar de ofrecer el complemento a los clientes en Windows 8.1 o versiones anteriores, debes actualizar la aplicación para quitar el código que ofrece el complemento y publicar un nuevo envío de la aplicación. Esto se recomienda incluso si la aplicación no está destinada a Windows 8.1 o una versión anterior; es una experiencia mejor para tus clientes si nunca les ofreces un complemento respecto al cual has optado por que no esté disponible.
    
 > [!NOTE] 
 > La elección de la opción **detener adquisición** o el envío de una actualización de la aplicación que quite el complemento de la aplicación no impedirá que los clientes usen el complemento si ya lo han comprado. Las suscripciones existentes no se renovarán y, posteriormente, se cancelarán una vez finalizado el período actual.


## <a name="schedule"></a>Programación

De forma predeterminada (a menos que haya seleccionado una de las opciones de **almacenamiento ocultas** en la sección **visibilidad** ), el complemento estará disponible para los clientes en cuanto pase la certificación y complete el proceso de publicación. Para elegir otras fechas, seleccione **Mostrar opciones** para expandir esta sección. 

Para obtener más información, consulte Configuración de la [programación de versiones precisa](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Precios

Debe seleccionar un precio base para el complemento (a menos que haya seleccionado la opción **detener adquisición** en la sección **visibilidad** ). La selección predeterminada es **gratuita** , por lo que si desea cobrar dinero por el complemento, asegúrese de elegir uno de los planes de tarifa disponibles (a partir de 99 USD).

También puede programar cambios de precios para indicar la fecha y la hora a las que debe cambiar el precio del complemento. Además, tiene la opción de personalizar estos cambios para mercados específicos. 

> [!TIP]
> En el caso de los complementos de suscripción, no puede aumentar el precio después de publicar el complemento, ya sea seleccionando un precio base mayor en un envío posterior o programando un cambio de precio que aumente el precio. Puede seleccionar un precio más bajo con cualquiera de estos métodos, pero una vez que se reduce el precio, no podrá aumentarlo por encima del precio nuevo. Por este motivo, es especialmente importante asegurarse de seleccionar el nivel de precios adecuado para los complementos de suscripción. 

Para obtener más información, consulta [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md).


## <a name="sale-pricing"></a>Precio de oferta

Si quieres ofrecer tu complemento a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta. Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](put-apps-and-add-ons-on-sale.md).
