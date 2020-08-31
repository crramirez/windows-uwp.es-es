---
Description: Dirija segmentos específicos de sus clientes con contenido personalizado para aumentar la implicación, la retención y la monetización.
title: Usar ofertas de destino para maximizar el compromiso y las conversiones
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, ofertas de destino, ofertas, notificaciones
ms.localizationpriority: medium
ms.openlocfilehash: ff2c3049154ffcc18164b8084bc9e34a9121905d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155309"
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>Usar ofertas de destino para maximizar el compromiso y las conversiones

Dirija segmentos específicos de sus clientes con contenido atractivo y personalizado para aumentar la implicación, la retención y la monetización.

> [!IMPORTANT]
> Las ofertas de destino solo se pueden usar con aplicaciones UWP que incluyen complementos.

## <a name="targeted-offer-overview"></a>Introducción a la oferta de destino

En un nivel alto, debe hacer tres cosas para usar las ofertas de destino:

1. **Cree la oferta en el [centro de Partners](https://partner.microsoft.com/dashboard).** Vaya a la página **participar en > ofertas de destino** para crear ofertas. A continuación se describe más información sobre este proceso.
2. **Implemente la experiencia de la oferta en la aplicación.** Use la *API de ofertas de destino de Microsoft Store* en el código de la aplicación para recuperar las ofertas disponibles para un usuario determinado. También necesitará crear la experiencia en la aplicación para la oferta de destino. Para obtener más información, consulte [Administración de ofertas de destino con los servicios](../monetize/manage-targeted-offers-using-windows-store-services.md)de la tienda.
3. **Envíe la aplicación a la tienda.** La aplicación debe publicarse con la experiencia de oferta de la aplicación en vigor para que las ofertas estén disponibles para los clientes.

Después de completar estos pasos, los clientes que usen su aplicación verán las ofertas que están disponibles en ese momento, en función de su pertenencia a los segmentos asociados a las ofertas. Tenga en cuenta que, aunque haremos todo lo posible para mostrar todas las ofertas disponibles a sus clientes, en ocasiones puede haber problemas que ofrezcan disponibilidad.


## <a name="to-create-and-send-a-targeted-offer"></a>Para crear y enviar una oferta de destino

1.  En el [centro de Partners](https://partner.microsoft.com/dashboard), expanda **participar** en el menú de navegación izquierdo y seleccione ofertas de **destino**.
2.  En la página **ofertas de destino** , revise las ofertas disponibles. Seleccione **crear nueva oferta** para cualquier oferta que desee implementar.

    > [!NOTE]
    > Las ofertas disponibles que verá pueden variar con el tiempo y según los criterios de la cuenta.

3.  En la nueva fila que aparece debajo de las ofertas disponibles, elija el producto (aplicación) en el que estará disponible la oferta. A continuación, seleccione el complemento que desea asociar a la oferta.
4.  Repita los pasos 2 y 3 Si desea crear ofertas adicionales. Puede implementar el mismo tipo de oferta más de una vez para la misma aplicación, siempre que seleccione diferentes complementos para cada oferta. Además, puede asociar el mismo complemento a más de un tipo de oferta.
5.  Cuando haya terminado de crear ofertas, haga clic en **Guardar**.

Una vez que haya implementado las ofertas, puede volver a la página de **ofertas de destino** del centro de partners para ver las conversiones totales de cada oferta.

Si decide no usar una oferta (o si ya no desea seguir usando, haga clic en **eliminar.**

> [!IMPORTANT]
> Asegúrese de haber publicado el código para recuperar las ofertas disponibles para un usuario determinado y para crear la experiencia en la aplicación. Para obtener más información, consulte [Administración de ofertas de destino con los servicios](../monetize/manage-targeted-offers-using-windows-store-services.md)de la tienda.
>
> Al considerar el contenido de las ofertas de destino, tenga en cuenta que, al igual que con todo el contenido de la aplicación, el contenido de las ofertas debe cumplir las [directivas de contenido](/legal/windows/agreements/store-policies)de la tienda.
>
> Tenga en cuenta también que si un cliente que usa la aplicación (y ha iniciado sesión con su cuenta de Microsoft en el momento en que se determina la pertenencia al segmento) da su dispositivo a otro usuario para que lo use, la otra persona podrá ver las ofertas destinadas al cliente original.