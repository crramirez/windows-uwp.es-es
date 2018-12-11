---
Description: The Pricing and availability page of the app submission process lets you determine how much your app will cost, whether you'll offer a free trial, and how, when, and where it will be available to customers.
title: Establecer los precios y la disponibilidad de las aplicaciones
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, precio, disponible, descubrible, prueba gratuita, pruebas, prueba, aplicaciones, fecha de lanzamiento
ms.localizationpriority: medium
ms.openlocfilehash: d5fa6c3e23516a5255f8bd3252f6ded233101625
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8874664"
---
# <a name="set-app-pricing-and-availability"></a>Establecer los precios y la disponibilidad de las aplicaciones


La página **Precios y disponibilidad** del [proceso de envío de la aplicación](app-submissions.md) permite determinar cuánto costará tu aplicación, independientemente de si ofreces una prueba gratuita y cómo, cuándo y dónde estará disponible para los clientes. A continuación, te guiaremos a través de las opciones de esta página y todo lo que debes tener en cuenta al escribir la información.


## <a name="markets"></a>Mercados

La Microsoft Store llega a clientes de más de 200 países y regiones de todo el mundo. De manera predeterminada, te ofreceremos la aplicación en todos los mercados posibles. Si lo prefieres, puedes elegir los mercados específicos en los que quieres ofrecer tu aplicación. 

Para más información, consulta [Definir la selección de precios y mercados](define-pricing-and-market-selection.md).


## <a name="visibility"></a>Visibilidad

La sección **Visibilidad** te permite establecer restricciones sobre cómo tu aplicación puede ser descubierta y adquirida, incluyendo si las personas pueden encontrar tu aplicación en Store o ver su descripción de Store completa.

Para obtener más información, consulta [Elegir opciones de visibilidad](choose-visibility-options.md).


## <a name="schedule"></a>Programación

De manera predeterminada (a menos que hayas seleccionado una de la opciones **Hacer disponible esta aplicación, pero no descubrible, en Store** en la sección [Visibilidad](choose-visibility-options.md#discoverability), la aplicación estará disponible para todos los clientes tan pronto como supere la certificación y complete el proceso de publicación. Para elegir otras fechas, seleccione **Mostrar opciones** para expandir la sección. 

Para obtener más información, consulta [Configurar la programación de lanzamiento precisa](configure-precise-release-scheduling.md).


## <a name="pricing"></a>Precios

Necesitas seleccionar un precio base de la aplicación (a menos que hayas seleccionado la opción **Detener adquisición** en **Hacer disponible esta aplicación, pero no descubrible, en Store** en la sección [Visibilidad](choose-visibility-options.md#discoverability)), eligiendo entre **Gratis** y uno de los niveles de precio disponibles. También puedes programar los cambios de precio para indicar la fecha y la hora en las que el precio de la aplicación debería cambiar. Además, tienes la opción de personalizar estos cambios para mercados específicos. 

Para obtener más información, consulta [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md).


## <a name="free-trial"></a>Prueba gratuita

Muchos desarrolladores eligen permitir que los clientes prueben su aplicación de forma gratuita con la funcionalidad de prueba proporcionada por la Store. De manera predeterminada, **Sin prueba gratuita** está seleccionada, y no habrá ninguna versión de prueba de la aplicación. Si quieres ofrecer una versión de prueba, puedes seleccionar un valor en la lista desplegable **Evaluación gratuita**.

Hay dos tipos de prueba que puedes elegir y tienes la opción para configurar la fecha y hora en las que la versión de prueba empieza y acaba.

### <a name="time-limited"></a>Límite de tiempo

Elige **Límite de tiempo** para permitir que los clientes prueben tu aplicación gratis durante un determinado número de días: **1 día**, **7 días**, **15 días**, o **30 días**. Puedes limitar características agregando código para [excluir o limitar funciones en la versión de prueba](../monetize/in-app-purchases-and-trials.md), o puedes permitir que los clientes accedan a toda las funcionalidades durante ese período de tiempo. 
> [!NOTE]
> Las versiones de prueba con tiempo limitado no se muestran a los clientes con Windows 10, compilación 10.0.10586 o versiones anteriores o a clientes con Windows Phone 8.1 y versiones anteriores.

### <a name="unlimited"></a>Sin límite

Elige **Sin límite** para permitir a los clientes el acceso a la aplicación de forma gratuita indefinidamente. Querrás animarlos para que adquieran la versión completa, así que asegúrate de agregar código para [excluir o limitar funciones en la versión de prueba](../monetize/in-app-purchases-and-trials.md).

### <a name="start-and-end-dates"></a>Fechas de inicio y finalización

De manera predeterminada, la versión de prueba estará disponible en cuanto se publique tu aplicación y se ofrecerá siempre. Si lo deseas, puedes especificar la fecha y hora en la que la versión de prueba debería empezar a ofrecerse y cuando debería detenerse. 

>[!NOTE]
> Estas fechas solo se aplican para los clientes de Windows 10 (incluida la Xbox). Si la aplicación está disponible para los clientes con versiones anteriores del sistema operativo, se ofrecerá la prueba a estos clientes siempre que tu producto esté disponible. 

Para establecer las fechas de cuándo se debe ofrecer la prueba a los clientes en Windows 10, cambia el menú desplegable **Comienza en** o **finaliza en** a **hasta** y, a continuación, elige la fecha y la hora. Si lo haces, puede elegir **UTC** para que el tiempo que selecciones sea el Horario universal coordinado (UTC), o elige **Local** para que estas horas se usen en cada zona asociada a un mercado. (Ten en cuenta para los mercados que tengan más de una zona horaria, se usará solo una zona horaria para ese mercado. Para los Estados Unidos, la zona horario se usa). Puedes seleccionar **personalización para mercados específicos** si deseas establecer fechas diferentes en cualquier mercado.


## <a name="sale-pricing"></a>Precio de oferta

Si quieres ofrecer tu aplicación a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta.

Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](put-apps-and-add-ons-on-sale.md).


## <a name="organizational-licensing"></a>Licencias organizativas

De manera predeterminada, tu aplicación se puede ofrecer a las organizaciones para su compra por volumen. En esta sección puedes indicar si lo permites, y en qué condiciones.

Para obtener más información, consulta [Opciones de licencias organizativas](organizational-licensing.md).


## <a name="publish-date"></a>Fecha de publicación

Anteriormente, la sección **Fecha de publicación** aparecía en esta página. Esta funcionalidad ahora puede encontrarse en la página [Opciones de envío](manage-submission-options.md), en la sección **Opciones de suspensión de publicación**. (Ten en cuenta que, para controlar cuándo debe publicarse la aplicación en Store, te recomendamos que uses la sección [Programación](configure-precise-release-scheduling.md) de la página **Precios y disponibilidad**).


