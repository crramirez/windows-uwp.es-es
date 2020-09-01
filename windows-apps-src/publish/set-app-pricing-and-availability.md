---
Description: La página Precios y disponibilidad del proceso de envío de la aplicación permite determinar cuánto costará tu aplicación, independientemente de si ofreces una prueba gratuita y cómo, cuándo y dónde estará disponible para los clientes.
title: Establecer los precios y la disponibilidad de las aplicaciones
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, precio, disponible, reconocible, evaluación gratuita, pruebas, prueba, aplicaciones, fecha de lanzamiento
ms.localizationpriority: medium
ms.openlocfilehash: 9956463471b310835aedf517817878d526cc810d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164109"
---
# <a name="set-app-pricing-and-availability"></a>Establecer los precios y la disponibilidad de las aplicaciones


La página **Precios y disponibilidad** del [proceso de envío de la aplicación](app-submissions.md) permite determinar cuánto costará tu aplicación, independientemente de si ofreces una prueba gratuita y cómo, cuándo y dónde estará disponible para los clientes. A continuación, te guiaremos por las opciones de esta página y lo que debes tener en cuenta al escribir la información.


## <a name="markets"></a>Mercados

El Microsoft Store llega a los clientes en más de 240 países y regiones de todo el mundo. De forma predeterminada, ofreceremos la aplicación en todos los mercados posibles. Si lo prefiere, puede elegir los mercados específicos en los que le gustaría ofrecer la aplicación. 

Para obtener más información, consulte [definir la selección del mercado](./define-market-selection.md).


## <a name="visibility"></a>Visibilidad

La sección **visibilidad** le permite establecer restricciones en el modo en que se puede detectar y adquirir la aplicación, incluso si las personas pueden encontrar la aplicación en la tienda o ver la lista de la tienda.

Para obtener más información, consulta [Elegir las opciones de visibilidad](choose-visibility-options.md).


## <a name="schedule"></a>Programación

De forma predeterminada (a menos que haya seleccionado una de las opciones de **hacer que esta aplicación esté disponible pero no se pueda detectar en las** opciones del almacén en la sección de [visibilidad](choose-visibility-options.md#discoverability) ), la aplicación estará disponible para los clientes en cuanto pase la certificación y complete el proceso de publicación. Para elegir otras fechas, seleccione **Mostrar opciones** para expandir esta sección. 

Para obtener más información, consulte Configuración de la [programación de versiones precisa](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Precios

Se le pedirá que seleccione un precio base para la aplicación (a menos que haya seleccionado la opción **detener adquisición** en **hacer que esta aplicación esté disponible pero no sea reconocible en el almacén** en la sección [visibilidad](choose-visibility-options.md#discoverability) ), eligiendo **gratis** o uno de los planes de tarifa disponibles. También puede programar cambios de precios para indicar la fecha y la hora a las que debe cambiar el precio de la aplicación. Además, tiene la opción de personalizar estos cambios para mercados específicos. Microsoft actualiza periódicamente los precios recomendados para tener en cuenta las fluctuaciones de moneda en los distintos mercados. Cuando cambia el precio recomendado, el área de precios mostrará un indicador de advertencia si los precios que ha seleccionado no están alineados con los nuevos valores recomendados. Los precios de los productos no cambiarán, tiene el control sobre cuándo y si desea actualizar estos precios. 

Para obtener más información, consulta [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md).


## <a name="free-trial"></a>Evaluación gratuita

Muchos desarrolladores eligen permitir que los clientes prueben su aplicación de forma gratuita con la funcionalidad de prueba proporcionada por la Tienda. De forma predeterminada, no se selecciona **ninguna evaluación gratuita** y no se realizará ninguna evaluación de la aplicación. Si desea ofrecer una versión de prueba, puede seleccionar un valor en la lista desplegable **evaluación gratuita** .

Hay dos tipos de pruebas que puede elegir y tiene la opción de configurar la fecha y la hora en que la evaluación debe iniciarse y dejar de ofrecerse.

### <a name="time-limited"></a>Limitado por tiempo

Elija **tiempo limitado** para permitir que los clientes prueben su aplicación de forma gratuita durante un determinado número de días: **1 día**, **7 días**, **15 días**o **30 días**. Puede limitar las características agregando código para [excluir o limitar características en la versión de prueba](../monetize/in-app-purchases-and-trials.md), o puede permitir que los clientes tengan acceso a toda la funcionalidad durante ese período de tiempo. 
> [!NOTE]
> Las pruebas de tiempo limitado no se muestran a los clientes de Windows 10 Build 10.0.10586 o una versión anterior, o a los clientes de Windows Phone 8,1 y versiones anteriores.

### <a name="unlimited"></a>Sin límite

Elija **ilimitado** para permitir que los clientes accedan a la aplicación de forma gratuita. Querrás animarlos para que adquieran la versión completa, así que asegúrate de agregar código para [excluir o limitar funciones en la versión de prueba](../monetize/in-app-purchases-and-trials.md).

### <a name="start-and-end-dates"></a>Fechas de inicio y fin

De forma predeterminada, la versión de prueba estará disponible tan pronto como se publique la aplicación y nunca se dejará de ofrecer. Si lo desea, puede especificar la fecha y la hora en que se debe empezar a ofrecer la evaluación y cuándo debe dejar de ofrecerse. 

>[!NOTE]
> Estas fechas solo se aplican a los clientes de Windows 10 (incluida Xbox). Si la aplicación está disponible para los clientes en versiones anteriores del sistema operativo, la evaluación se ofrecerá a los clientes durante el tiempo que esté disponible el producto. 

Para establecer las fechas en las que se debe ofrecer la versión de evaluación a los clientes de Windows 10, cambie la lista desplegable **comienza en** y/o **finaliza en en** y, a **continuación, elija**la fecha y la hora. Si lo hace, puede elegir la hora **UTC** para que la hora seleccionada sea hora universal coordinada (UTC), o bien elegir **local** , de modo que estas horas se usen en cada zona horaria asociada a un mercado. (Tenga en cuenta que para los mercados que incluyen más de una zona horaria, solo se utilizará una zona horaria en ese mercado. En el Estados Unidos, se usa la zona horaria oriental). Puede seleccionar **personalizar para mercados específicos** si desea establecer fechas diferentes para cualquier mercado...


## <a name="sale-pricing"></a>Precio de oferta

Si quieres ofrecer tu aplicación a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta.

Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](put-apps-and-add-ons-on-sale.md).


## <a name="organizational-licensing"></a>Licencias organizativas

De manera predeterminada, tu aplicación se puede ofrecer a las organizaciones para su compra por volumen. En esta sección puedes indicar si lo permites, y en qué condiciones.

Para obtener más información, consulta [Opciones de licencias organizativas](organizational-licensing.md).


## <a name="publish-date"></a>Fecha de publicación

Anteriormente, la sección **publicar fecha** aparecía en esta página. Esta funcionalidad se puede encontrar ahora en la página [Opciones de envío](manage-submission-options.md) , en la sección Opciones de la suspensión de la **publicación** . (Tenga en cuenta que para controlar cuándo se debe publicar la aplicación en la tienda, se recomienda usar la sección [programación](configure-precise-release-scheduling.md) de la página **precios y disponibilidad** ).