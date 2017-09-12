---
author: JnHs
Description: "Segmentos específicos de destino de los clientes con contenido personalizado para aumentar la interacción, retención y monetización."
title: "Usar ofertas dirigidas para maximizar la interacción y las conversiones"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ofertas de destino, ofertas, notificaciones
ms.openlocfilehash: b30add7a1aa55ffd852bff57fcebb7a5f40e114b
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2017
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Usar ofertas dirigidas para maximizar la interacción y las conversiones

Segmentos específicos de destino de los clientes con contenido personalizado y atractivo para aumentar la interacción, retención y monetización. En algunos casos, las ofertas de destino pueden asociarse a una promoción patrocinada por Microsoft, que paga una bonificación a los desarrolladores por cada conversión correcta, lo que te permite ganar aún más.

> [!IMPORTANT]
> Las ofertas dirigidas solo pueden usarse con aplicaciones para UWP que incluyen complementos.

## <a name="targeted-offer-overview"></a>Información general de ofertas dirigidas

En un nivel elevado, deberás seguir tres pasos para usar las ofertas dirigidas:

1. **Crear la oferta en el panel de información.** Navegar hasta la página para **Interactuar > Ofertas dirigidas** para crear ofertas. A continuación, se describe más información sobre este proceso.
2. **Implementar la experiencia de la oferta desde la aplicación.** Usa la *tienda de Windows de destino ofrece API* en el código de tu aplicación para recuperar las ofertas disponibles para un usuario determinado y reclamar la oferta (si está asociada con una promoción). También tendrás que crear la experiencia en la aplicación para la oferta de destino. Para obtener más información, consulta [Administrar ofertas dirigidas usando los servicios de la Tienda](../monetize/manage-targeted-offers-using-windows-store-services.md).
3. **Envía tu aplicación a la Tienda.** La aplicación debe publicarse con la experiencia de oferta en la aplicación para que las ofertas estén disponibles para los clientes.

Después de completar estos pasos, los clientes que usen la aplicación verán las ofertas que están disponibles en ese momento, en función de su pertenencia a los segmentos asociados a dichas ofertas. Ten en cuenta que aunque nos esforzamos todo lo que podemos para mostrar todas las ofertas disponibles a tus clientes, pueden producirse errores ocasionales que afectan a la disponibilidad de ofertas.


## <a name="to-create-and-send-a-targeted-offer"></a>Crear y enviar una oferta dirigida

Sigue estos pasos para crear una oferta dirigida en el panel de información. 

1.  En el panel de información del Centro de desarrollo de Windows, expande **Interactuar** en el panel de navegación izquierdo y luego selecciona **Ofertas dirigidas**.
2.  En la página **Ofertas dirigidas**, revisa las ofertas disponibles. Selecciona **Crear nueva oferta** para cualquier oferta que quieras implementar. 

    > [!NOTE]
    > Las ofertas disponibles que verás pueden variar con el tiempo y se basan en criterios de cuenta. Inicialmente, verás una o más ofertas con los criterios de segmento predefinidos. Pronto, te permitiremos crear ofertas dirigidas en función de los [segmentos de clientes que definas](create-customer-segments.md).

3.  En la nueva fila que aparecerá debajo de las ofertas disponibles, elige el producto (aplicación) para el que estará disponible la oferta. Luego selecciona el complemento que quieres asociar a la oferta. 
4.  Repite los pasos 2 y 3 si quieres crear más ofertas. Puedes implementar el mismo tipo de oferta más de una vez para la misma aplicación, siempre y cuando selecciones complementos diferentes para cada uno de ellas. Además, puedes asociar el mismo complemento con más de un tipo de oferta.
5.  Cuando hayas terminado de crear ofertas, haz clic en **Guardar**.

Después de implementar las ofertas, puedes ver el número de total de conversiones para cada oferta en la página **Ofertas dirigidas** en el panel de información. 

Si decides no usar una oferta o si ya no quieres seguir usándola, haz clic en **Eliminar**.

> [!IMPORTANT]
> Asegúrate de haber publicado código para recuperar las ofertas disponibles para un usuario determinado y para crear la experiencia en la aplicación. Para obtener más información, consulta [Administrar ofertas dirigidas usando los servicios de la Tienda](../monetize/manage-targeted-offers-using-windows-store-services.md).
> 
> Al considerar el contenido de las ofertas dirigidas, ten en cuenta que, al igual que con todo el contenido de aplicación, el contenido de tus ofertas debe cumplir con las [Directivas de contenido](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies) de la Tienda.
> 
> También ten cuenta que, si un cliente que usa tu aplicación (y ha iniciado sesión con su cuenta de Microsoft en el momento de determinar la pertenencia a un segmento) y más adelante da su dispositivo a otra persona para que lo use, esta última puede ver las ofertas dirigidas al cliente original. 

