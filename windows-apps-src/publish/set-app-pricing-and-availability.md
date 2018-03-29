---
author: jnHs
Description: The Pricing and availability page of the app submission process lets you determine how much your app will cost, whether you'll offer a free trial, and how, when, and where it will be available to customers.
title: Establecer los precios y la disponibilidad de las aplicaciones
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
ms.author: wdg-dev-content
ms.date: 11/22/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: high
ms.openlocfilehash: 3e94aadefb418aa7cefa90b8f274868c80f3e480
ms.sourcegitcommit: 11edca90aaf7856c762e68903483079d30ad3877
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2018
---
# <a name="set-app-pricing-and-availability"></a>Establecer los precios y la disponibilidad de las aplicaciones


La página **Precios y disponibilidad** del [proceso de envío de la aplicación](app-submissions.md) permite determinar cuánto costará tu aplicación, independientemente de si ofreces una prueba gratuita y cómo, cuándo y dónde estará disponible para los clientes. A continuación, te guiaremos a través de las opciones de esta página y todo lo que debes tener en cuenta al escribir la información.


## <a name="markets"></a>Mercados

La Microsoft Store llega a clientes de más de 200 países y regiones de todo el mundo. De manera predeterminada, te ofreceremos la aplicación en todos los mercados posibles. Si lo prefieres, puedes elegir los mercados específicos en los que quieres ofrecer tu aplicación. 

Para más información, consulta [Definir la selección de precios y mercados](define-pricing-and-market-selection.md).


## <a name="visibility"></a>Visibilidad

La sección **Visibilidad** te permite establecer restricciones sobre cómo se puede detectar y adquirir la aplicación.

El valor predeterminado es **Make this product available and discoverable in the Store**. Esto significa que tu aplicación figurará en la Tienda para que los clientes puedan buscar a través del vínculo directo de la aplicación o por otros métodos, como la búsqueda, la exploración y la inclusión en listas selectas. 

Si quieres ocultar la aplicación en la Tienda pero que esté disponible para determinadas personas, selecciona **Mostrar opciones**, expande la sección y, a continuación, selecciona **Make this product available but not discoverable in the Store**. Esto significa que ningún cliente podrá encontrar la aplicación en la Tienda a través de la búsqueda o exploración, independientemente de su versión del sistema operativo. También debes elegir una de las siguientes versiones para determinar cómo se puede adquirir tu aplicación.

>[!IMPORTANT]
> Cada una de estas opciones limita las versiones de sistema operativo en el que los clientes pueden comprar tu aplicación. Lee las descripciones con cuidado para asegurarte de que conoces qué versiones del sistema operativo son compatibles. Ten en cuenta que los clientes de Windows 8 y Windows 8.1 no podrán obtener la aplicación si eliges una de las opciones de **Make this product available but not discoverable in the Store**. 

- **Solo vínculo directo: Cualquier cliente con un vínculo directo a la descripción del producto puede descargarla, excepto en Windows 8.x.** Cualquier cliente que obtenga la descripción de la aplicación a través de un vínculo directo puede descargarla en dispositivos que ejecuten Windows 10 o Windows Phone 8.1 y versiones anteriores. Los clientes en Windows 8.x no pueden obtener la aplicación con esta opción.
- **Detener la compra: Los clientes con un vínculo directo podrán ver la descripción de la aplicación en la Tienda, pero solo podrán descargarla si ya tienen el producto, tienen un código promocional y están usando un dispositivo Windows 10.** Incluso si un cliente tiene un vínculo directo, no se puede descargar la aplicación a no ser que tenga un [código promocional](generate-promotional-codes.md) y esté usando un dispositivo Windows 10. Al proporcionar un código a un cliente, este puede usar el vínculo y el código para tu aplicación de forma gratuita (solo en Windows 10), aunque no se la ofrezcas a ningún otro cliente. Además de usar un código promocional, no hay otra manera de acceder a tu aplicación.
- **Solo usuarios en Windows Phone 8.x: Solo las personas especificadas a continuación puede descargan este producto en un dispositivo con Windows Phone 8.x. Cualquier persona que tenga un vínculo directo y un código promocional puede descargar el producto en un dispositivo con Windows 10** Esta opción puede que no aparezca para todos los envíos. Solo se aplica si tienes paquetes que puedan ejecutarse en Windows Phone 8.x. Solo los clientes cuya dirección de correo electrónico (asociada a sus cuentas Microsoft) escribas en el cuadro (separadas por punto y comas) podrán descargar la aplicación en Windows Phone 8.x mediante un vínculo directo a su descripción. También puedes generar códigos promocionales para distribuirla entre personas concretas en Windows 10 como se ha descrito anteriormente. 

> [!TIP]
> Si quieres dejar de ofrecer una aplicación a los clientes nuevos, puedes seleccionar **Make app unavailable** desde su página de información general. Después de confirmar que quieres que la aplicación deje de estar disponible, en el plazo de unas horas dejará de estar visible en Microsoft Store y ningún cliente nuevo podrá acceder a ella (a no ser que tengan un [código promocional](generate-promotional-codes.md) y usen un dispositivo Windows 10). Esta acción anulará las selecciones de **Visibilidad** en el envío. Para que la aplicación vuelva a estar disponible para los nuevos clientes (por selecciones de **Visibilidad**), puedes hacer clic en **Make app available** desde la página de información general en cualquier momento. Para obtener más información, consulta [Quitar una aplicación de la Tienda](guidance-for-app-package-management.md#removing-an-app-from-the-store).


## <a name="schedule"></a>Programación

De manera predeterminada (a menos que hayas seleccionado uno de la opciones **Make this app available but not discoverable in the Store** en la sección **Visibilidad**, la aplicación estará disponible para todos los clientes tan pronto como supere la certificación y complete el proceso de publicación. Para elegir otras fechas, seleccione **Mostrar opciones** para expandir la sección. 

Para obtener más información, consulta [Configurar la programación de lanzamiento precisa](configure-precise-release-scheduling.md).


## <a name="display-release-date"></a>Mostrar fecha de lanzamiento

De manera predeterminada, la fecha de lanzamiento de la aplicación será la fecha en la que aparezca en la Tienda. Si quieres reemplazarla y proporcionar una fecha de lanzamiento personalizada, activa la casilla de esta sección y, a continuación, escribe la fecha de lanzamiento deseada. Ten en cuenta que la fecha de lanzamiento no siempre se muestra en la descripción de la Tienda.


## <a name="pricing"></a>Precios

Necesitas seleccionar un precio base de la aplicación (a menos que hayas seleccionado una de las opciones **Make this app available but not discoverable in the Store** de la sección [Visibilidad](#visibility), y seleccionar **Gratis** o una de las franjas de precios disponibles. También puedes programar los cambios de precio para indicar la fecha y la hora en las que el precio de la aplicación debería cambiar. Además, tienes la opción de personalizar estos cambios para mercados específicos. 

Para obtener más información, consulta [Establecer y programar los precios de las aplicaciones](set-and-schedule-app-pricing.md).


## <a name="free-trial"></a>Prueba gratuita

Muchos desarrolladores eligen permitir que los clientes prueben su aplicación de forma gratuita con la funcionalidad de prueba proporcionada por la Tienda. De manera predeterminada, **Sin prueba gratuita** está seleccionada, y no habrá ninguna versión de prueba de la aplicación. Si quieres ofrecer una versión de prueba, puedes seleccionar un valor en la lista desplegable **Evaluación gratuita**.

Hay dos tipos de prueba que puedes elegir y tienes la opción para configurar la fecha y hora en las que la versión de prueba empieza y acaba.

### <a name="time-limited"></a>Límite de tiempo

Elige **Límite de tiempo** para permitir que los clientes prueben tu aplicación gratis durante un determinado número de días: **1 día**, **7 días**, **15 días**, o **30 días**. Puedes limitar características agregando código para [excluir o limitar funciones en la versión de prueba](../monetize/in-app-purchases-and-trials.md), o puedes permitir que los clientes accedan a toda las funcionalidades durante ese período de tiempo. 
> [!NOTE]
> Las versiones de prueba con tiempo limitado no se muestran a los clientes con Windows 10, compilación 10.0.10586 o versiones anteriores o a clientes con Windows Phone 8.1 y versiones anteriores.

### <a name="unlimited"></a>Sin límite

Elige **Sin límite** para permitir a los clientes el acceso a la aplicación de forma gratuita indefinidamente. Querrás animarlos para que adquieran la versión completa, así que asegúrate de agregar código para [excluir o limitar funciones en la versión de prueba](../monetize/in-app-purchases-and-trials.md).

### <a name="start-and-end-dates"></a>Fechas de inicio y finalización

De manera predeterminada, la versión de prueba estará disponible en cuanto se publique tu aplicación y se ofrecerá siempre. Si lo deseas, puedes especificar la fecha y hora en la que la versión de prueba debería empezar a ofrecerse y cuando debería detenerse. 

>[!NOTE]
> Estas fechas solo se aplican para los clientes de Windows 10 (incluida la Xbox). Si la aplicación está disponible para los clientes con versiones anteriores del sistema operativo, se ofrecerá la prueba a estos clientes siempre que tu producto esté disponible. 

Para establecer las fechas de cuándo se debe ofrecer la prueba a los clientes en Windows 10, cambia el menú desplegable **Comienza en** o **finaliza en** a **hasta** y, a continuación, elige la fecha y la hora. Si lo haces, puede elegir **UTC** para que el tiempo que selecciones sea el Horario universal coordinado (UTC), o elige **Local** para que estas horas se usen en cada zona asociada a un mercado. (Ten en cuenta para los mercados que tengan más de una zona horaria, se usará solo una zona horaria para ese mercado. Para los Estados Unidos, se usará la zona horario del Este). 

>[!NOTE]
> A diferencia de la sección [Programación](configure-precise-release-scheduling.md), las fechas que selecciones para tu **evaluación gratuita** no se puede personalizar para mercados específicos. 



## <a name="sale-pricing"></a>Precio de oferta

Si quieres ofrecer tu aplicación a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta.

Para obtener más información, consulta [Poner aplicaciones y complementos en oferta](put-apps-and-add-ons-on-sale.md).


## <a name="organizational-licensing"></a>Licencias organizativas

De manera predeterminada, tu aplicación se puede ofrecer a las organizaciones para su compra por volumen. En esta sección puedes indicar si lo permites, y en qué condiciones.

Para obtener más información, consulta [Opciones de licencias organizativas](organizational-licensing.md).


## <a name="publish-date"></a>Fecha de publicación

De manera predeterminada, tu envío comenzará el proceso de publicación en cuanto supere la certificación, a menos que hayas configurado las fechas en la sección [**Programación**](#schedule) descrita anteriormente. 

Para controlar cuándo debe publicarse la aplicación en la Tienda, usa la sección **Programación**. Para la mayoría de los envíos, debes usar esa sección para programar el lanzamiento de la aplicación y dejar la sección **Fecha de publicación** establecida en la opción predeterminada, **Publish this submission as soon as it passes certification**. Esto evitará que el envío se publique antes de las fechas establecidas en la sección **Programación**. Las fechas que seleccionaste en la sección **Programación** determina cuando la aplicación estará disponible para los clientes en la Tienda.

Si no deseas establecer una fecha de lanzamiento todavía y prefieres que el envío permanezca sin publicar hasta que decidas iniciar manualmente el proceso de publicación, puedes elegir **Publicar este envío manualmente.** Elegir esta opción, significa que la selección no se publicará hasta que tú lo indiques. Cuando la aplicación supere la certificación, puedes publicarla seleccionando **Publicar ahora** en la página de estado de certificación, o seleccionando una fecha específica, como se describe a continuación.

Elige **No antes del \[fecha\]** para garantizar que el envío no se publique hasta una fecha determinada. Con esta opción, el envío se lanzará lo antes posible en la fecha que especifiques o después de ella. La fecha debe ser al menos 24 horas después del momento actual. Junto con la fecha, también puedes especificar la hora en que el envío debe comenzar a publicarse.
 
> [!NOTE]
> Los retrasos durante la certificación o la publicación podrían hacer que la fecha de lanzamiento real sea posterior a la que solicites. La Microsoft Store no puede garantizar que la aplicación (o la actualización) esté disponible en una fecha específica.  

También puedes cambiar la fecha de lanzamiento después de enviar la aplicación, siempre y cuando aún no haya entrado en el paso Publicar. 

