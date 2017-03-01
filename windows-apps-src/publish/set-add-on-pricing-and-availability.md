---
author: jnHs
Description: "Cuando envías un complemento, las opciones de la página Precios y disponibilidad determinan lo que se cobrará por tu complemento y cómo se debe ofrecer a los clientes."
title: Establecer los precios y la disponibilidad de los complementos
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e16cd437a1ee2b53a263ad29cd8e93b3d2e40bed
ms.lasthandoff: 02/07/2017

---

# <a name="set-add-on-pricing-and-availability"></a>Establecer los precios y la disponibilidad de los complementos


Cuando envías un complemento, las opciones de la página **Precios y disponibilidad** determinan lo que se cobrará por tu complemento y cómo se debe ofrecer a los clientes.

## <a name="base-price"></a>Precio base


Debes seleccionar un precio base para tu complemento. Estas franjas de precios son las mismas que las franjas de precios para las aplicaciones, que comienzan a partir de 0,99 USD. También tienes la opción de ofrecer tu complemento de forma gratuita.

## <a name="markets-and-custom-prices"></a>Mercados y precios personalizados


De manera predeterminada, tu complemento figurará en todos los mercados posibles, incluidos los mercados futuros que podamos agregar posteriormente, al precio base.

Sin embargo, al igual que con una aplicación, tienes la opción de elegir los mercados en los que quieres ofrecer el complemento. En la mayoría de los casos, querrás elegir el mismo conjunto de mercados que la aplicación, pero tienes la flexibilidad para realizar los cambios necesarios. También puedes establecer precios personalizados, de modo que puedas cobrar precios diferentes para el complemento en distintos mercados.

Para obtener información y una lista completa de los mercados disponibles, consulta [Definir la selección de precios y mercados](define-pricing-and-market-selection.md).

## <a name="sale-pricing"></a>Precio de oferta


Si quieres ofrecer tu complemento a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta. Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](put-apps-and-add-ons-on-sale.md).

## <a name="distribution-and-visibility"></a>Distribución y visibilidad


Puedes determinar si debes ofrecer el complemento para su compra a los clientes. Elija una de las siguientes opciones:

-   **Disponible para la compra. Se puede mostrar en la descripción de tu aplicación:** este es el valor predeterminado y se recomienda a menos que quieras restringir el acceso al complemento. Deja esta opción activada para los complementos que estarán disponibles para cualquier cliente.
-   **Disponible para la compra. No se muestra en la descripción de tu aplicación:** elegir esta opción permite a los clientes comprar el complemento desde tu aplicación, pero el complemento no se mostrará en la descripción de la aplicación de la Tienda. Úsalo solo cuando la oferta no esté ampliamente disponible, por ejemplo durante los períodos iniciales de pruebas internas.
-   **Ya no está disponible para la compra. No se muestra en la descripción de la aplicación.** Si se selecciona esta opción, significa que el complemento no se mostrará en la descripción de la aplicación y no lo podrán comprar clientes nuevos. Sin embargo, **esta opción no se admite para los clientes en Windows 8.1 o versiones anteriores**. Si la aplicación está disponible en Windows 8.1 o versiones anteriores, el complemento seguirá estando disponible para su compra para estos clientes. Para dejar de ofrecer el complemento a los clientes en Windows 8.1 o versiones anteriores, debes actualizar la aplicación para quitar el código que ofrece el complemento y publicar un nuevo envío de la aplicación. Esto se recomienda incluso si la aplicación no está destinada a Windows 8.1 o una versión anterior; es una experiencia mejor para tus clientes si nunca les ofreces un complemento respecto al cual has optado por que no esté disponible.
    
 > **Nota** Elegir esta configuración o enviar una actualización de aplicaciones que quite el complemento del código de la aplicación no afecta a los clientes que ya hayan comprado el complemento, independientemente de su sistema operativo.


## <a name="publish-date"></a>Fecha de publicación

Puedes indicar cuándo debe publicarse el envío del complemento (o la actualización) seleccionando una opción en la sección **Fecha de publicación**.

-   Elige **Publicar tan pronto como el complemento supere la certificación** para que este envío esté disponible en la Tienda tan pronto como sea posible.
-   Elige **Publicar este complemento manualmente** si no quieres que tu envío se publique hasta que lo indiques. Puedes hacerlo desde la página de estado de certificación haciendo clic en **Publicar ahora** o seleccionando una fecha específica, como se describe a continuación.
-   Elige **No antes del \[fecha\]** para garantizar que el envío no se publique hasta una fecha determinada. Con esta opción, el envío se lanzará lo antes posible en la fecha que especifiques o después de ella. La fecha debe ser al menos 24 horas después del momento actual. Junto con la fecha, también puedes especificar la hora en que el envío debe comenzar a publicarse.

 > **Nota** Los retrasos durante la certificación o la publicación podrían hacer que la fecha de lanzamiento real sea posterior a la que solicites. La Tienda Windows no puede garantizar que el complemento (o la actualización) esté disponible en una fecha específica.
 

 





