---
author: JnHs
Description: Target specific segments of your customers with personalized content to increase engagement, retention, and monetization.
title: Usar ofertas dirigidas para maximizar la interacción y las conversiones
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, ofertas de destino, ofertas, notificaciones
ms.localizationpriority: medium
ms.openlocfilehash: 0f6e1f8119522cd0441157131362d860feff3410
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7282702"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Usar ofertas dirigidas para maximizar la interacción y las conversiones

Segmentos específicos de destino de los clientes con contenido personalizado y atractivo para aumentar la interacción, retención y monetización.

> [!IMPORTANT]
> Las ofertas dirigidas solo pueden usarse con aplicaciones para UWP que incluyen complementos.

## <a name="targeted-offer-overview"></a>Información general de ofertas dirigidas

En un nivel elevado, deberás seguir tres pasos para usar las ofertas dirigidas:

1. **Crear la oferta en [El centro de partners](https://partner.microsoft.com/dashboard).** Navegar hasta la página para **Interactuar > Ofertas dirigidas** para crear ofertas. A continuación, se describe más información sobre este proceso.
2. **Implementar la experiencia de la oferta desde la aplicación.** Usar la *API de ofertas dirigidas de Microsoft Store* en el código de la aplicación para recuperar las ofertas disponibles para un usuario determinado. También tendrás que crear la experiencia en la aplicación para la oferta de destino. Para obtener más información, consulta [Administrar ofertas dirigidas usando los servicios de la Tienda](../monetize/manage-targeted-offers-using-windows-store-services.md).
3. **Envía tu aplicación a la Tienda.** La aplicación debe publicarse con la experiencia de oferta en la aplicación para que las ofertas estén disponibles para los clientes.

Después de completar estos pasos, los clientes que usen la aplicación verán las ofertas que están disponibles en ese momento, en función de su pertenencia a los segmentos asociados a dichas ofertas. Ten en cuenta que aunque nos esforzamos todo lo que podemos para mostrar todas las ofertas disponibles a tus clientes, pueden producirse errores ocasionales que afectan a la disponibilidad de ofertas.


## <a name="to-create-and-send-a-targeted-offer"></a>Crear y enviar una oferta dirigida

1.  En el [Centro de partners](https://partner.microsoft.com/dashboard), expande **interactuar** en el menú de navegación izquierdo y luego selecciona **dirigido ofrece**.
2.  En la página **Ofertas dirigidas**, revisa las ofertas disponibles. Selecciona **Crear nueva oferta** para cualquier oferta que quieras implementar.

    > [!NOTE]
    > Las ofertas disponibles que verás pueden variar con el tiempo y se basan en criterios de cuenta.

3.  En la nueva fila que aparecerá debajo de las ofertas disponibles, elige el producto (aplicación) para el que estará disponible la oferta. Luego selecciona el complemento que quieres asociar a la oferta.
4.  Repite los pasos 2 y 3 si quieres crear más ofertas. Puedes implementar el mismo tipo de oferta más de una vez para la misma aplicación, siempre y cuando selecciones complementos diferentes para cada uno de ellas. Además, puedes asociar el mismo complemento con más de un tipo de oferta.
5.  Cuando hayas terminado de crear ofertas, haz clic en **Guardar**.

Después de implementar las ofertas, puedes volver a la página **ofertas de destino** en el centro de partners para ver el total de conversiones para cada oferta.

Si decides no usar una oferta o si ya no quieres seguir usándola, haz clic en **Eliminar**.

> [!IMPORTANT]
> Asegúrate de haber publicado código para recuperar las ofertas disponibles para un usuario determinado y para crear la experiencia en la aplicación. Para obtener más información, consulta [Administrar ofertas dirigidas usando los servicios de la Tienda](../monetize/manage-targeted-offers-using-windows-store-services.md).
>
> Al considerar el contenido de las ofertas dirigidas, ten en cuenta que, al igual que con todo el contenido de aplicación, el contenido de tus ofertas debe cumplir con las [Directivas de contenido](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies) de la Tienda.
>
> También ten cuenta que, si un cliente que usa tu aplicación (y ha iniciado sesión con su cuenta de Microsoft en el momento de determinar la pertenencia a un segmento) y más adelante da su dispositivo a otra persona para que lo use, esta última puede ver las ofertas dirigidas al cliente original.
