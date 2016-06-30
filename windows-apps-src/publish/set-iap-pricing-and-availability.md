---
author: jnHs
Description: "Cuando envías un IAP, las opciones de la página Precios y disponibilidad determinan lo que se cobrará por tu IAP y cómo se debe ofrecer a los clientes."
title: Establecer precios y disponibilidad de IAP
ms.assetid: B3D4B753-716B-460B-A3B1-ED5712ECD694
ms.sourcegitcommit: 52816584a9afbd6c8e213a182bae18732f082aef
ms.openlocfilehash: 0e6c58f2d892f213d2de53c3cb9b97b1e8152137

---

# Establecer precios y disponibilidad de IAP


Cuando envías un IAP, las opciones de la página **Precios y disponibilidad** determinan lo que se cobrará por tu IAP y cómo se debe ofrecer a los clientes.

## Precio base


Debes seleccionar un precio base para tu IAP. Estas franjas de precios son las mismas que las franjas de precios para las aplicaciones, que comienzan a partir de 0,99 USD. También tienes la opción de ofrecer tu IAP de forma gratuita.

## Mercados y precios personalizados


De manera predeterminada, tu IAP figurará en todos los mercados posibles, incluidos los mercados futuros que podamos agregar posteriormente, al precio base.

Sin embargo, al igual que con una aplicación, tienes la opción de elegir los mercados en los que quieres ofrecer el IAP. En la mayoría de los casos, querrás elegir el mismo conjunto de mercados que la aplicación, pero tienes la flexibilidad para realizar los cambios necesarios. También puedes establecer precios personalizados, de modo que puedas cobrar precios diferentes para el IAP en distintos mercados.

Para obtener información y una lista completa de los mercados disponibles, consulta [Definir la selección de precios y mercados](define-pricing-and-market-selection.md).

## Precio de oferta


Si quieres ofrecer tu IAP a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta. Para obtener más información, consulta [Poner aplicaciones e IAP en oferta](put-apps-and-iaps-on-sale.md).

## Distribución y visibilidad


Puedes determinar si debes ofrecer el IAP para su compra a los clientes. Elija una de las siguientes opciones:

-   **Disponible para la compra. Se puede mostrar en la descripción de tu aplicación:** este es el valor predeterminado y se recomienda a menos que quieras restringir el acceso al IAP. Deja esta opción activada para los IAP que estarán disponibles para cualquier cliente.
-   **Disponible para la compra. No se muestra en la descripción de tu aplicación:** elegir esta opción permite a los clientes comprar el IAP desde tu aplicación, pero el IAP no se mostrará en la descripción de la aplicación de la Tienda. Úsalo solo cuando la oferta no esté ampliamente disponible, por ejemplo durante los períodos iniciales de pruebas internas.
-   **Ya no está disponible para la compra. No se muestra en la descripción de la aplicación.** Si se selecciona esta opción, significa que el IAP no se mostrará en la descripción de la aplicación y no lo podrán comprar clientes nuevos. Sin embargo, **esta opción no se admite para los clientes en Windows 8.1 o versiones anteriores**. Si la aplicación está disponible en Windows 8.1 o versiones anteriores, el IAP seguirá estando disponible para su compra para estos clientes. Para dejar de ofrecer el IAP a los clientes en Windows 8.1 o versiones anteriores, debes actualizar la aplicación para quitar el código que ofrece el IAP y publicar un nuevo envío de la aplicación. Esto se recomienda incluso si la aplicación no está destinada a Windows 8.1 o una versión anterior; es una experiencia mejor para tus clientes si nunca les ofreces un IAP respecto al cual has optado por que no esté disponible.
    
 > **Nota** Elegir esta configuración o enviar una actualización de aplicaciones que quite el IAP del código de la aplicación no afecta a los clientes que ya hayan comprado el IAP, independientemente de su sistema operativo.


## Fecha de publicación

Puedes indicar cuándo se publicará el IAP (o la actualización) si seleccionas una opción en la sección **Fecha de publicación**.

-   Elige **Publicar tan pronto como el IAP supere la certificación** para que este envío esté disponible en la Tienda tan pronto como sea posible.
-   Elige **Publicar este IAP manualmente** si no quieres que tu envío se publique hasta que lo indiques. Puedes hacerlo desde la página de estado de certificación haciendo clic en **Publicar ahora** o seleccionando una fecha específica, como se describe a continuación.
-   Elige **No antes del \[fecha\]** para garantizar que el envío no se publique hasta una fecha determinada. Con esta opción, el envío se lanzará lo antes posible en la fecha que especifiques o después de ella. La fecha debe ser al menos 24 horas después del momento actual. Junto con la fecha, también puedes especificar la hora en que el envío debe comenzar a publicarse.

 > **Nota** Los retrasos durante la certificación o la publicación podrían hacer que la fecha de lanzamiento real sea posterior a la que solicites. La Tienda Windows no puede garantizar que el IAP (o la actualización) esté disponible en una fecha específica.
 

 







<!--HONumber=Jun16_HO4-->


