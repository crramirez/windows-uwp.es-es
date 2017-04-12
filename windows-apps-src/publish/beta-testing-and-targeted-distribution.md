---
author: jnHs
Description: "El panel del Centro de desarrollo de Windows te ofrece la opción de que tu aplicación esté disponible solo para ciertas personas, de forma que puedas hacer que la prueben los evaluadores antes de ponerla a disposición del público."
title: "Pruebas beta y distribución dirigida"
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 20178a8048cadb733bf1e34996ad46067a8660f3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="beta-testing-and-targeted-distribution"></a>Pruebas beta y distribución dirigida


No importa lo minucioso que hayas sido al probar la aplicación, no hay nada como una prueba real de su uso por parte de otras personas. El panel del Centro de desarrollo de Windows te ofrece la opción de que tu aplicación esté disponible solo para ciertas personas, de forma que puedas hacer que la prueben los evaluadores antes de ponerla a disposición del público. Los evaluadores pueden descubrir problemas que hayas pasado por alto, como errores de ortografía, un flujo de aplicación confuso o incluso errores que pueden hacer que la aplicación se bloquee. Luego, tendrás la oportunidad de corregir esos problemas antes de lanzar la aplicación al público, lo que resulta en un producto final más pulido.

Ofrecemos varias formas de limitar la distribución de las aplicaciones a solo los evaluadores sin necesidad de crear una versión independiente de la aplicación con una identidad del paquete y nombre diferentes. (Por supuesto, puedes crear una aplicación separada para realizar las pruebas si lo prefieres. Si lo haces, asegúrate de asignarle un nombre distinto del que deseas usar como nombre final; es decir, distinto al nombre público de la aplicación).

El método de distribución a los evaluadores de la aplicación depende de los sistemas operativos que sean objetivo de la aplicación. A continuación encontrarás opciones que funcionan para Windows 10, Windows Phone 8.1 y versiones anteriores.

## <a name="making-your-app-available-to-testers-on-windows-10-devices"></a>Hacer que la aplicación esté disponible para los evaluadores en dispositivos de Windows 10

Proporcionamos dos opciones que te permiten limitar la distribución de tus aplicaciones solo a determinadas personas en dispositivos de Windows 10.

### <a name="package-flights"></a>Paquetes piloto

Si ya has publicado una versión de la aplicación, puedes crear paquetes piloto para distribuir un conjunto de paquetes diferente a las personas que especifiques. Puedes crear varios paquetes piloto para usar la misma aplicación con diferentes grupos de personas. Esta es una manera excelente de probar diferentes paquetes simultáneamente y te permite extraer paquetes de una versión piloto e incluirlos en el envío de la versión final si decides que están listos para distribuirlos a todos.

Para obtener más información, consulta [Paquetes piloto](package-flights.md).

> **Nota** Para distribuir paquetes a un conjunto aleatorio de tus clientes de Windows10 en un porcentaje especificado en lugar de a un grupo concreto de clientes específicos, puedes usar el [lanzamiento de paquete gradual](gradual-package-rollout.md). También puedes combinar el lanzamiento con tus paquetes piloto si quieres distribuir una actualización de forma gradual a uno de tus grupos piloto.

### <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>Ocultar la aplicación en la Tienda y usar códigos promocionales

Si deseas limitar la distribución de una aplicación a solo un determinado grupo de evaluadores, **sin** antes publicar un envío que esté disponible de forma mayoritaria, puedes usar el mismo [proceso de envío de la aplicación](app-submissions.md) que en cualquier aplicación que envíes. Para permitir que solo determinados contactos obtengan la aplicación de forma gratuita y evitar que otros clientes vean su descripción o puedan descargarla, haz lo siguiente:

-   En tu envío, en la página **Precios y disponibilidad**, elige **Ocultar esta aplicación e impedir la compra. Los clientes con un vínculo directo podrán ver la descripción de la aplicación, pero solo podrán descargarla si tienen un código promocional y están usando un dispositivo Windows10** en la sección [Distribución y visibilidad](set-app-pricing-and-availability.md#distribution-and-visibility). Esto impide que se encuentre la aplicación en la Tienda mediante la búsqueda o la exploración.
-   Cuando la aplicación supere la certificación, [genera códigos promocionales](generate-promotional-codes.md) para ella y distribúyela a los evaluadores. Puedes generar hasta 250 códigos promocionales para una misma aplicación en un período de seis meses. Estos códigos proporcionan a los evaluadores un vínculo directo a la descripción de la aplicación y les permite descargarla de forma gratuita, aunque hayas fijado un precio para ella cuando creaste el envío.

Después de distribuir los vínculos de código promocional a los evaluadores, pueden descargar la aplicación de forma gratuita y ofrecerte sus comentarios para mejorarla. Más adelante, cuando quieras poner la aplicación a disposición del público, tan solo crea un nuevo envío y cambia la opción **Distribución y visibilidad** a **Make this app available in the Store** (junto con cualquier otro cambio que quieras realizar).

Estas son cosas que se deben tener en cuenta al hacerlo:

-   Puedes proporcionar a los evaluadores una versión actualizada de la aplicación en cualquier momento mediante la creación de un nuevo envío. Asegúrate de mantener la opción de **Distribución y visibilidad** establecida en **Ocultar esta aplicación e impedir la compra. Los clientes con un vínculo directo podrán ver la descripción de la aplicación, pero solo podrán descargarla si tienen un código promocional y están usando un dispositivo Windows10**. Los evaluadores obtendrán la actualización cuando esta supere el proceso de certificación, pero nadie más podrá obtenerla.
-   Los evaluadores deben tener un dispositivo de Windows 10 en el que puedan instalar la aplicación. (Sin embargo, no es necesario que la aplicación incluya paquetes de Windows 10 para usar este método de evaluación).
-   Puedes crear más [códigos promocionales](generate-promotional-codes.md) en cualquier momento (hasta 500 códigos cada seis meses).
-   No se puede revocar el acceso a la aplicación cuando los evaluadores ya la han descargado. Podrán seguir usándola y recibirán cualquier actualización que publiques más adelante.
-   Debes determinar cómo quieres recopilar los comentarios de los evaluadores. Considera la posibilidad de proporcionar un correo electrónico o un vínculo a un sitio web en la aplicación beta, de modo que los evaluadores puedan proporcionar comentarios con facilidad.
-   Puedes revisar [informes analíticos](analytics.md) referentes a tu aplicación, lo que incluye las clasificaciones o los comentarios de los evaluadores.
-   Al distribuir la aplicación a los evaluadores, puedes incluir complementos. Como probablemente no quieras cobrarles nada, asegúrate de que el precio de los complementos sea Gratis durante la fase de evaluación. Cuando la aplicación esté disponible para otros clientes, puedes crear un nuevo envío para cada complemento y cambiar su precio.

## <a name="other-methods-for-distributing-apps-to-testers"></a>Otros métodos para distribuir aplicaciones a los evaluadores

También puedes limitar la distribución de la aplicación solo a un grupo específico de personas con las opciones adicionales de la sección [Distribución y visibilidad](set-app-pricing-and-availability.md#distribution-and-visibility) de la página **Precios y disponibilidad** del envío de una aplicación. Ten en cuenta que estas opciones no funcionan para los clientes en todos los sistemas operativos. Las opciones anteriores son las recomendadas para probar tu aplicación en dispositivos de Windows 10.

Si eliges una de las opciones anteriores, siempre puedes enviar una actualización y establecer la opción [Distribución y visibilidad](set-app-pricing-and-availability.md#distribution-and-visibility) en **Make this app available in the Store** cuando estés preparado para terminar el periodo de pruebas y hacer que la aplicación esté ampliamente disponible. No es necesario cambiar el nombre de la aplicación y crear una totalmente distinta (a menos que prefieras hacerlo).

### <a name="targeted-distribution-to-customers-with-a-link-to-your-apps-listing"></a>Distribución dirigida a los clientes con un vínculo a la descripción de la aplicación

Con esta opción, solo aquellos con un vínculo directo a la descripción de la aplicación pueden descargarla. Puedes encontrar esta **URL** en la página [Identidad de la aplicación](view-app-identity-details.md) en el panel. Ningún cliente podrá encontrar la aplicación mediante la búsqueda o la exploración en la Tienda, pero cualquiera que tenga el vínculo podrá descargarla. (Ten en cuenta que el precio debe ser **Gratis** para que los evaluadores puedan descargarla gratuitamente).

Para usar esta opción, al enviar la aplicación selecciona **Ocultar la aplicación en la Tienda. Los clientes que dispongan de un vínculo directo a la descripción de la aplicación podrán continuar descargándola, salvo en Windows 8 y Windows 8.1.** en la sección [Distribución y visibilidad](set-app-pricing-and-availability.md#distribution-and-visibility) de la página **Precios y disponibilidad**.

> **Importante** Esta opción no funciona para los evaluadores con Windows8 ni Windows8.1.

### <a name="targeted-distribution-to-customers-with-specified-email-addresses"></a>Distribución dirigida a clientes con direcciones de correo electrónico específicas

Para las pruebas **en Windows Phone8.1 y versiones anteriores únicamente**, esta opción ofrece una forma de limitar la distribución de la aplicación. Solo las personas cuya dirección de correo electrónico (asociada a sus cuentas Microsoft) escribas en el cuadro podrán descargar la aplicación mediante un vínculo directo a su descripción.

> **Importante** Las personas con las direcciones de correo electrónico que especifiques solo podrán descargar la aplicación en dispositivos que ejecuten Windows Phone8.1 o versiones anteriores.
 
Puedes encontrar el vínculo directo de la aplicación en la página [Identidad de la aplicación](view-app-identity-details.md) del panel. Ningún cliente podrá encontrar la aplicación mediante la búsqueda o exploración en la Tienda y, aunque se disponga del vínculo a la descripción, no se podrá descargar salvo que se use para ello una cuenta de Microsoft asociada a una de las direcciones de correo electrónico que proporcionaras al enviar la aplicación.

> **Nota** Si usas esta opción, aún puedes poner la aplicación a disposición de los evaluadores en dispositivos Windows 10 [generando códigos promocionales](generate-promotional-codes.md), como se ha descrito anteriormente. Cualquier persona que tenga uno de los códigos promocionales de la aplicación podrá descargarla en un dispositivo de Windows 10, aunque no hayas escrito aquí su dirección de correo.

Para usar esta opción, al enviar la aplicación selecciona **Ocultar esta aplicación y ponerla a la disposición de solo las personas que especifiques abajo, quienes podrán descargar esta aplicación en dispositivos Windows Phone 8.x. Se puede usar un código de promoción para descargar esta aplicación en dispositivos Windows10** de la sección [Distribución y visibilidad](set-app-pricing-and-availability.md#distribution-and-visibility) de la página **Precios y disponibilidad**.

Si eliges esta opción, debes tener en cuenta lo siguiente:

-   Esta opción solo se puede seleccionar si nunca antes has publicado la aplicación con la opción [Distribución y visibilidad](set-app-pricing-and-availability.md#distribution-and-visibility) establecida en **Make this app available in the Store**.
-   El precio de la aplicación debe ser **Gratis** para que los evaluadores puedan descargar la aplicación gratuitamente.
-   Los evaluadores solo pueden descargar la aplicación en Windows Phone 8.1 y versiones anteriores. Los evaluadores deben tener un dispositivo Windows Phone comercial para usar la aplicación, pero no es necesario que el dispositivo esté desbloqueado ni registrado.
-   Los evaluadores deberán tener una cuenta de Microsoft para obtener acceso a la Tienda Windows y descargar la aplicación. Tendrás que conocer el correo electrónico asociado con la cuenta de Microsoft de cada evaluador para agregarlos a la lista de evaluadores. Para crear una cuenta de Microsoft nueva, los evaluadores pueden ir a [Configuración de cuenta de Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=618945).
-   Puedes proporcionar hasta 10 000 direcciones de correo en el cuadro de texto.
-   Las direcciones de correo deben estar separadas por punto y coma.
-   Puedes agregar más direcciones más adelante, pero para ello será necesario crear un nuevo envío.
-   No se puede revocar el acceso a la aplicación cuando los evaluadores ya la han descargado. Una vez la hayan descargado, podrán seguir usándola y recibirán cualquier actualización que envíes.
