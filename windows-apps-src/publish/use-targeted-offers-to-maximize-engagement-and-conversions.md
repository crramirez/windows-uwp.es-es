---
Description: Segmentos específicos de destino de los clientes con contenido personalizado para aumentar la interacción, retención y monetización.
title: Usar ofertas dirigidas para maximizar la interacción y las conversiones
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, ofertas de destino, ofertas, notificaciones
ms.localizationpriority: medium
ms.openlocfilehash: e894e81b560f071cf42198f09aad0b2b82eae2cd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606160"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Usar ofertas dirigidas para maximizar la interacción y las conversiones

Segmentos específicos de destino de los clientes con contenido personalizado y atractivo para aumentar la interacción, retención y monetización.

> [!IMPORTANT]
> Las ofertas dirigidas solo pueden usarse con aplicaciones para UWP que incluyen complementos.

## <a name="targeted-offer-overview"></a>Información general de ofertas dirigidas

En un nivel elevado, deberás seguir tres pasos para usar las ofertas dirigidas:

1. **Cree la oferta en [centro de partners](https://partner.microsoft.com/dashboard).** Navegar hasta la página para **Interactuar > Ofertas dirigidas** para crear ofertas. A continuación, se describe más información sobre este proceso.
2. **Implementar la experiencia de la oferta en la aplicación.** Use la *ofrece una API de Microsoft Store como destino* en código de la aplicación para recuperar las ofertas disponibles para un usuario determinado. También tendrás que crear la experiencia en la aplicación para la oferta de destino. Para obtener más información, consulta [Administrar ofertas dirigidas usando los servicios de la Tienda](../monetize/manage-targeted-offers-using-windows-store-services.md).
3. **Enviar la aplicación a la Store.** La aplicación debe publicarse con la experiencia de oferta en la aplicación para que las ofertas estén disponibles para los clientes.

Después de completar estos pasos, los clientes que usen la aplicación verán las ofertas que están disponibles en ese momento, en función de su pertenencia a los segmentos asociados a dichas ofertas. Ten en cuenta que aunque nos esforzamos todo lo que podemos para mostrar todas las ofertas disponibles a tus clientes, pueden producirse errores ocasionales que afectan a la disponibilidad de ofertas.


## <a name="to-create-and-send-a-targeted-offer"></a>Crear y enviar una oferta dirigida

1.  En [centro de partners](https://partner.microsoft.com/dashboard), expanda **interactuar** en el menú de navegación izquierdo, seleccione **destino ofertas**.
2.  En la página **Ofertas dirigidas**, revisa las ofertas disponibles. Selecciona **Crear nueva oferta** para cualquier oferta que quieras implementar.

    > [!NOTE]
    > Las ofertas disponibles que verás pueden variar con el tiempo y se basan en criterios de cuenta.

3.  En la nueva fila que aparecerá debajo de las ofertas disponibles, elige el producto (aplicación) para el que estará disponible la oferta. Luego selecciona el complemento que quieres asociar a la oferta.
4.  Repite los pasos 2 y 3 si quieres crear más ofertas. Puedes implementar el mismo tipo de oferta más de una vez para la misma aplicación, siempre y cuando selecciones complementos diferentes para cada uno de ellas. Además, puedes asociar el mismo complemento con más de un tipo de oferta.
5.  Cuando hayas terminado de crear ofertas, haz clic en **Guardar**.

Después de implementar las ofertas, puede volver a la **destino ofertas** página en el centro de partners para ver las conversiones total para cada oferta.

Si decides no usar una oferta o si ya no quieres seguir usándola, haz clic en **Eliminar**.

> [!IMPORTANT]
> Asegúrate de haber publicado código para recuperar las ofertas disponibles para un usuario determinado y para crear la experiencia en la aplicación. Para obtener más información, consulta [Administrar ofertas dirigidas usando los servicios de la Tienda](../monetize/manage-targeted-offers-using-windows-store-services.md).
>
> Al considerar el contenido de las ofertas dirigidas, ten en cuenta que, al igual que con todo el contenido de aplicación, el contenido de tus ofertas debe cumplir con las [Directivas de contenido](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies) de la Tienda.
>
> También ten cuenta que, si un cliente que usa tu aplicación (y ha iniciado sesión con su cuenta de Microsoft en el momento de determinar la pertenencia a un segmento) y más adelante da su dispositivo a otra persona para que lo use, esta última puede ver las ofertas dirigidas al cliente original.
