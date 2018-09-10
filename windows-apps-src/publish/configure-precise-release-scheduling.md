---
author: jnHs
Description: You can set the precise date and time that your app should become available in the Store, giving you greater flexibility and the ability to customize dates for different markets.
title: Configurar la programación precisa del lanzamiento
ms.author: wdg-dev-content
ms.date: 05/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, programación, fecha de lanzamiento, fechas, inicio
ms.localizationpriority: medium
ms.openlocfilehash: 84466f907bad7e38506e1bf81b89eb631675093c
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "3821339"
---
# <a name="configure-precise-release-scheduling"></a>Configurar la programación precisa del lanzamiento

La sección **Programación** en la página [Precios y disponibilidad](set-app-pricing-and-availability.md) te permite establecer la fecha y hora precisas a la que la aplicación debe estar disponible en la Store, ofreciéndote más flexibilidad y la capacidad de personalizar fechas para los diferentes mercados.

> [!NOTE]
> Aunque este tema hace referencia a las aplicaciones, la programación de lanzamiento de envíos de complementos utiliza el mismo proceso.

Además, puedes establecer una fecha en la que el producto dejará de estar disponible en la Store. Recuerda que esto significa que el producto ya no podrá encontrarse en la Store mediante búsqueda o exploración, pero los clientes con un vínculo directo podrán ver la descripción del producto en la Store. Solo pueden descargarlo si ya tienen el producto o si tienen un [código promocional](generate-promotional-codes.md) y estás usando un dispositivo Windows 10.

De manera predeterminada (a menos que hayas seleccionado uno de la opciones **Make this app available but not discoverable in the Store** en la sección [Visibilidad](choose-visibility-options.md#discoverability), la aplicación estará disponible para todos los clientes tan pronto como supere la certificación y complete el proceso de publicación. Para elegir otras fechas, seleccione **Mostrar opciones** para expandir la sección.

Ten en cuenta que no podrás configurar las fechas en la sección **Programación** si has seleccionado una de las opciones **Make this app available but not discoverable in the Store**, en la sección [Visibilidad](choose-visibility-options.md#discoverability), porque la aplicación no se lanzará a los clientes, así que no hay ninguna fecha de lanzamiento que configurar.

> [!IMPORTANT]
> Las fechas que especifiques en la sección Programación solo se aplican a los clientes de Windows 10.
>
>Si la aplicación es compatible con versiones anteriores del sistema operativo, los clientes con esas versiones de sistema operativo verán la descripción de la aplicación en cuanto se supere la certificación y se complete el proceso de publicación, incluso si has seleccionado una fecha de lanzamiento más adelante. Cualquier fecha para **detener la compra** que selecciones no se aplicará a los clientes; todavía podrán comprar la aplicación (a menos que envíes una actualización con una nueva selección en la sección [Visibilidad](choose-visibility-options.md#discoverability), o si seleccionas **Make app unavailable** desde la página **Introducción a la aplicación**).


## <a name="base-schedule"></a>Programación de base

Las opciones seleccionadas para la programación de base se aplicarán a todos los mercados en los que la aplicación esté disponible, a menos que agregues fechas posteriores para mercados específicos (o grupos de mercado) seleccionando [Personalización para mercados específicos](#customize-the-schedule-for-specific-markets).

Verás dos opciones aquí: **Lanzamiento** y **Detener la compra**. 

## <a name="release"></a>Lanzamiento

En la lista desplegable de **Lanzamiento**, puedes establecer cuando quieres que tu aplicación esté disponible en la Store. Esto significa que la aplicación es reconocible en la Store mediante búsqueda o exploración, y que los clientes pueden ver su descripción en la Store y comprar la aplicación.

>[!NOTE]
> Una vez que la aplicación se ha publicado y está disponible en la Store, ya no podrás seleccionar una fecha de **lanzamiento** (ya que la aplicación se habrá publicado).

Estas son las opciones que se puede configurar para la programación del **lanzamiento** de un producto:
- **Tan pronto como sea posible**: El producto se publicará tan pronto como se certifique y se publique. Esta es la opción predeterminada.
- **En**: El producto se publicará en la fecha y hora que selecciones. Tienes dos opciones adicionales:
   - **UTC**: El tiempo que selecciones será el Horario universal coordinado (UTC), para que la aplicación se lance al mismo tiempo en todos los lugares.
   - **Local**: El tiempo que selecciones se usará en cada zona horaria asociada con un mercado. (Ten en cuenta para los mercados que tengan más de una zona horaria, se usará solo una zona horaria para ese mercado. Para los Estados Unidos, se usará la zona horario del Este).
- **No programada**: La aplicación no estará disponible en la Store. Si eliges esta opción, puede realizar la aplicación disponible en la Store creando más tarde un nuevo envío y eligiendo una de las otras opciones.


## <a name="stop-acquisition"></a>Detener la compra

En el menú desplegable **Detener la compra**, puedes establecer una fecha y hora en la que quieras establecer que los clientes nuevos ya no compren la aplicación o descubran su descripción. Esto puede ser útil si desea controlar con precisión cuando una aplicación ya no se ofrecerán a los clientes nuevos, como cuando se coordinar disponibilidad entre más de uno de tus aplicaciones.

De manera predeterminada, **Detener la compra** siempre está establecida en "Nunca". Para cambiar esta opción, selecciona **En** en la lista desplegable y especifica una fecha y hora, como se ha descrito anteriormente. En la fecha y hora que selecciones, los clientes ya no podrán comprar la aplicación.

Es importante comprender que esta opción tiene el mismo impacto que seleccionar **Make this app discoverable but not available** en la sección [Visibilidad](choose-visibility-options.md#discoverability) y eligiendo **Detener la compra: Los clientes con un vínculo directo podrán ver la descripción del producto en la Store, pero solo podrán descargarlo si ya lo tienen, tienen un código promocional y están usando un dispositivo Windows 10.** Para dejar de ofrecer completamente una aplicación a los clientes nuevos, haz clic en **Make app unavailable** desde la página Información general de la aplicación. Para obtener más información, consulta [Quitar una aplicación de la Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).

> [!TIP]
> Si seleccionas una fecha para **detener la compra** y, más adelante decides que te gustaría que la aplicación estuviera disponible de nuevo, puedes crear un nuevo envío y cambiar la opción **impedir la compra** a **Nunca**. La aplicación volverá a estar disponible una vez que tu envío actualizado se publique.

## <a name="customize-the-schedule-for-specific-markets"></a>Personalizar la programación para mercados concretos 

De manera predeterminada, las opciones que selecciones anteriormente, se aplicarán a todos los mercados en los que se ofrece la aplicación. Para personalizar los precios para mercados específicos, haz clic en **Personalización para mercados específicos**. La ventana emergente de **Selección de mercado** aparecerá en la lista de todos los mercados en los que hayas elegido que la aplicación estará disponible. Si has excluido algún mercado en la sección [Mercados](define-pricing-and-market-selection.md), no se mostrarán aquí. 

Para agregar una programación para un mercado, selecciónalo y haz clic en **Guardar**. A continuación, verás las mismas opciones **Lanzamiento** y **Detener la compra** descritas anteriormente, pero las selecciones que realice solo se aplicarán a ese mercado.

Para agregar una programación que se aplique a varios mercados, se creará un *grupo de mercado*. Para ello, selecciona los mercados que quieras incluir y escribe un nombre del grupo. (Este nombre solo es para tu propia referencia y no será visible para los clientes). Por ejemplo, si quieres crear un grupo de mercado para Norteamérica, puedes seleccionar **Canadá**, **México**, y **Estados Unidos**y asígnale el nombre **Norteamérica** u otro nombre que elijas. Cuando hayas terminado de crear el grupo de mercado, haz clic en **Guardar**. A continuación, verás las mismas opciones **Lanzamiento** y **Detener la compra** descritas anteriormente, pero las selecciones que realice solo se aplicarán a ese grupo de mercado.

Para agregar una programación personalizada para un mercado adicional, o un grupo de mercados adicionales, haz clic en **Personalización para mercados específicos** de nuevo y repite estos pasos. Para cambiar los mercados que se incluyen en un grupo de mercado, selecciona su nombre. Para quitar la programación personalizada para un grupo de mercado (o mercado individual), haz clic en **Quitar**.

> [!NOTE]
> Un mercado no puede pertenecer a más de un grupo de mercado que usas en la sección **Programación**. 










