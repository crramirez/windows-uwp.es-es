---
author: jnHs
Description: "El panel del Centro de desarrollo de Windows te ofrece la opción de que tu aplicación esté disponible solo para ciertas personas, de forma que puedas hacer que la prueben los evaluadores antes de ponerla a disposición del público."
title: "Pruebas beta y distribución dirigida"
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 7dd0e346e6be147935503eeb0b685568bfce726c
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2017
---
# <a name="beta-testing-and-targeted-distribution"></a>Pruebas beta y distribución dirigida

No importa lo minucioso que hayas sido al probar la aplicación, no hay nada como una prueba real de su uso por parte de otras personas. El panel del Centro de desarrollo de Windows te ofrece opciones para hacer envío de aplicación disponible solo para ciertas personas, de forma que puedas hacer que la prueben los evaluadores antes de ponerla a disposición del público. Los evaluadores pueden descubrir problemas que hayas pasado por alto, como errores de ortografía, un flujo de aplicación confuso o incluso errores que pueden hacer que la aplicación se bloquee. Luego, tendrás la oportunidad de corregir esos problemas antes de lanzar el envío al público, lo que resulta en un producto final más pulido.

Ofrecemos varias formas de limitar la distribución de las aplicaciones a solo los evaluadores sin necesidad de crear una versión independiente de la aplicación con una identidad del paquete y nombre diferentes. (Por supuesto, puedes crear una aplicación separada para realizar las pruebas si lo prefieres. Si lo haces, asegúrate de asignarle un nombre distinto del que deseas usar como nombre final; es decir, distinto al nombre público de la aplicación).

El método de distribución a los evaluadores de la aplicación depende de los sistemas operativos que sean objetivo de la aplicación. A continuación encontrarás opciones que funcionan para Windows 10, Windows Phone 8.1 y versiones anteriores.

## <a name="making-your-app-available-to-testers-on-windows-10-devices"></a>Hacer que la aplicación esté disponible para los evaluadores en dispositivos de Windows 10

Proporcionamos dos opciones que te permiten limitar la distribución de tus aplicaciones solo a determinadas personas en dispositivos de Windows 10.

### <a name="package-flights"></a>Paquetes piloto

Si ya has publicado tu aplicación, puedes crear paquetes piloto para distribuir un conjunto de paquetes diferente a las personas que especifiques. Puedes incluso crear varios paquetes piloto para usar la misma aplicación con diferentes grupos de personas. Esta es una manera excelente de probar diferentes paquetes simultáneamente y puedes extraer paquetes de una versión piloto e incluirlos en el envío de la versión final si decides que los paquetes están listos para distribuirlos a todo el mundo.

Para obtener más información, consulta [Paquetes piloto](package-flights.md).

> [!NOTE]
> Para distribuir paquetes específicos a un conjunto aleatorio de tus clientes de Windows10 en un porcentaje establecido en lugar de a un grupo concreto de clientes específicos, puedes usar el [lanzamiento de paquete gradual](gradual-package-rollout.md). También puedes combinar el lanzamiento con tus paquetes piloto si quieres distribuir una actualización de forma gradual a uno de tus grupos piloto.

<span id="hide" />
### <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>Ocultar la aplicación en la Tienda y usar códigos promocionales

Si deseas limitar la distribución de una aplicación a solo un determinado grupo de evaluadores, **sin** antes publicar un envío que esté disponible de forma mayoritaria, puedes usar el mismo [proceso de envío de la aplicación](app-submissions.md) que en cualquier aplicación que envíes. Para permitir que solo determinados contactos obtengan la aplicación de forma gratuita y evitar que otros clientes vean su descripción o puedan descargarla, haz lo siguiente:

-   En tu envío, en la sección de [Visibilidad](set-app-pricing-and-availability.md#visibility) de la página **Precios y disponibilidad**, selecciona **Make this product available but not discoverable in the Store**.  Elige la opción para **detener la compra: Los clientes con un vínculo directo podrán ver la descripción del producto en la Tienda, pero solo podrán descargarlo si ya lo tienen, tienen un código promocional y están usando un dispositivo Windows 10** Esto impide que se encuentre la aplicación en la Tienda mediante la búsqueda o la exploración.
-   Cuando la aplicación supere la certificación, [genera códigos promocionales](generate-promotional-codes.md) para ella y distribúyelos a los evaluadores. Puedes generar códigos que permiten hasta 1600 canjes para una misma aplicación en un período de seis meses. Estos códigos proporcionan a los evaluadores un vínculo directo a la descripción de la aplicación y les permite descargarla de forma gratuita, aunque hayas fijado un precio para ella cuando creaste el envío.

Después de distribuir los vínculos de código promocional a los evaluadores, pueden descargar la aplicación de forma gratuita y ofrecerte sus comentarios para mejorarla. Más adelante, cuando quieras poner la aplicación a disposición del público, crea un nuevo envío y cambia la opción **Visibilidad** a **Make this app available in the Store** (junto con cualquier otro cambio que quieras realizar).

Estas son cosas que se deben tener en cuenta al hacerlo:

-   Puedes proporcionar a los evaluadores una versión actualizada de la aplicación en cualquier momento mediante la creación de un nuevo envío. Asegúrate de mantener la opción **Visibilidad** establecida en **Make this product available but not discoverable in the Store** con la opción **Detener la compra**. Los evaluadores obtendrán la actualización cuando esta supere el proceso de certificación, pero nadie más podrá obtenerla.
-   Los evaluadores deben tener un dispositivo de Windows 10 en el que puedan instalar la aplicación. (Sin embargo, no es necesario que la aplicación incluya paquetes de Windows 10 para usar este método de evaluación).
-   Puedes crear más [códigos promocionales](generate-promotional-codes.md) en cualquier momento (hasta 1600 canjes por aplicación cada seis meses).
-   No se puede revocar el acceso a la aplicación cuando los evaluadores ya la han descargado. Podrán seguir usándola y recibirán cualquier actualización que publiques más adelante.
-   Debes determinar cómo quieres recopilar los comentarios de los evaluadores. Considera la posibilidad de proporcionar un vínculo en la aplicación beta que les permita enviar comentarios a través del correo electrónico o a través del [Centro de opiniones](../monetize/launch-feedback-hub-from-your-app.md).
-   Puedes revisar los [informes analíticos](analytics.md) referentes a tu aplicación, lo que incluye los informes de uso y estado y las clasificaciones o los comentarios de los evaluadores.
-   Al distribuir la aplicación a los evaluadores, puedes incluir complementos. Como probablemente no quieras cobrarles nada, asegúrate de que el precio de los complementos sea **Gratis** durante la fase de evaluación. Cuando la aplicación esté disponible para otros clientes, puedes crear un nuevo envío para cada complemento y cambiar su precio.


## <a name="other-methods-for-distributing-apps-to-testers"></a>Otros métodos para distribuir aplicaciones a los evaluadores

También puedes limitar la distribución de la aplicación solo a un grupo específico de personas con las opciones adicionales de la sección [Visibilidad](set-app-pricing-and-availability.md#visibility) de la página **Precios y disponibilidad** del envío de una aplicación. Ten en cuenta que estas opciones no funcionan para los clientes en todos las versiones de los sistemas operativos. Específicamente, ninguna de ellas funciona para los clientes que usen Windows 8 o Windows 8.1.

### <a name="targeted-distribution-to-customers-with-a-link-to-your-apps-listing"></a>Distribución dirigida a los clientes con un vínculo a la descripción de la aplicación

Con esta opción, solo aquellos con un vínculo directo a la descripción de la aplicación pueden descargarla. Puedes encontrar esta **URL** en la página [Identidad de la aplicación](view-app-identity-details.md) en el panel. Los clientes podrán encontrar la aplicación mediante búsqueda o exploración en la Tienda, pero aquellos que tengan el vínculo podrán descargarla en un dispositivo que ejecute Windows Phone 8.1 o versiones anteriores o en Windows 10. 

> [!NOTE]
> Para que los evaluadores puedan descargar la aplicación sin coste alguno, debes establecer su precio como **Gratis**.

Para usar esta opción, en la sección de **Make this product available but not discoverable in the Store** en la sección [Visibilidad](set-app-pricing-and-availability.md#visibility), de la página **Precio y disponibilidad**. A continuación, elige la opción para **Solo vínculo directo: Cualquier cliente con un vínculo directo a la descripción del producto puede descargarla, excepto en Windows 8.x.**.  


### <a name="targeted-distribution-to-customers-with-specified-email-addresses"></a>Distribución dirigida a clientes con direcciones de correo electrónico específicas

Para las pruebas **en Windows Phone8.1 y versiones anteriores únicamente**, esta opción ofrece una forma de limitar la distribución de la aplicación. Solo las personas cuya dirección de correo electrónico (asociada a sus cuentas Microsoft) escribas en el cuadro podrán descargar la aplicación mediante un vínculo directo a su descripción.

> [!IMPORTANT]
> Las personas con las direcciones de correo electrónico que especifiques solo podrán descargar la aplicación en dispositivos que ejecuten Windows Phone8.1 o versiones anteriores.
 
Puedes encontrar el vínculo directo de la aplicación en la página [Identidad de la aplicación](view-app-identity-details.md) del panel. Ningún cliente podrá encontrar la aplicación mediante la búsqueda o exploración en la Tienda y, aunque se disponga del vínculo a la descripción, no se podrá descargar salvo que se use para ello una cuenta de Microsoft asociada a una de las direcciones de correo electrónico que proporcionaras al enviar la aplicación.

> [!NOTE]
Si usas esta opción, aún puedes poner la aplicación a disposición de los evaluadores en dispositivos con Windows 10 [generando códigos promocionales](generate-promotional-codes.md), como se describió anteriormente. Cualquier persona que tenga uno de los códigos promocionales de la aplicación podrá descargarla en un dispositivo de Windows 10, aunque no hayas escrito aquí su dirección de correo.

Para usar esta opción, en la sección de **Make this product available but not discoverable in the Store** en la sección [Visibilidad](set-app-pricing-and-availability.md#visibility), de la página **Precio y disponibilidad**. A continuación, elige la opción para que **Individuals on Windows Phone 8.x only: Only people you specify below can download this product on a Windows Phone 8.x device. Anyone with a direct link and a promotional code may download the product on a Windows 10 device.** (Solo usuarios en Windows Phone 8.x: Solo las personas especificadas a continuación puede descargan este producto en un dispositivo con Windows Phone 8.x. Cualquier persona que tenga un vínculo directo y un código promocional puede descargar el producto en un dispositivo con Windows 10). 

Si eliges esta opción, debes tener en cuenta lo siguiente:

-   Esta opción solo se puede seleccionar si nunca antes has publicado la aplicación con la opción [Visibilidad](set-app-pricing-and-availability.md#visibility) establecida en **Make this app available in the Store**.
-   El precio de la aplicación debe ser **Gratis** para que los evaluadores puedan descargar la aplicación gratuitamente.
-   Los evaluadores solo pueden descargar la aplicación en Windows Phone 8.1 y versiones anteriores. Los evaluadores deben tener un dispositivo Windows Phone comercial para usar la aplicación, pero no es necesario que el dispositivo esté desbloqueado ni registrado.
-   Los evaluadores deberán tener una cuenta de Microsoft para obtener acceso a la Tienda Windows y descargar la aplicación. Tendrás que conocer el correo electrónico asociado con la cuenta de Microsoft de cada evaluador para agregarlos a la lista de evaluadores. Para crear una cuenta de Microsoft nueva, los evaluadores pueden ir a [Configuración de cuenta de Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=618945).
-   Puedes proporcionar hasta 10 000 direcciones de correo en el cuadro de texto.
-   Las direcciones de correo deben estar separadas por punto y coma.
-   Puedes agregar más direcciones más adelante, pero para ello será necesario crear un nuevo envío.
-   No se puede revocar el acceso a la aplicación cuando los evaluadores ya la han descargado. Una vez la hayan descargado, podrán seguir usándola y recibirán cualquier actualización que envíes.
