---
Description: Centro de partners ofrece varias opciones para permitir que los evaluadores a probar la aplicación antes de que se ofrece al público.
title: Pruebas beta y distribución dirigida
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, prueba beta, distribución limitada, beta, versiones beta, pruebas, evaluadores
ms.localizationpriority: medium
ms.openlocfilehash: 1c0a4d1053c35a831458c832131659b9cb888259
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63787178"
---
# <a name="beta-testing-and-targeted-distribution"></a>Pruebas beta y distribución dirigida

No importa lo minucioso que hayas sido al probar la aplicación, no hay nada como una prueba real de su uso por parte de otras personas. Los evaluadores pueden descubrir problemas que hayas pasado por alto, como errores de ortografía, un flujo de aplicación confuso o incluso errores que pueden hacer que la aplicación se bloquee. Luego, tendrás la oportunidad de corregir esos problemas antes de lanzar el envío al público, lo que resulta en un producto final más pulido. 

Centro de partners ofrece varias opciones para permitir que los evaluadores a probar la aplicación antes de que se ofrece al público.

Sea cual sea el método que elijas, estas son algunas cosas que debes tener en cuenta al probar la versión beta de tu aplicación.

- No puedes revocar el acceso a la aplicación cuando un evaluador ya la haya descargado. Podrán seguir usándola y recibirán cualquier actualización que publiques más adelante.
- Debes determinar cómo quieres recopilar los comentarios de los evaluadores. Considera la posibilidad de ofrecer un vínculo que permita a tus evaluadores enviar comentarios por correo electrónico (o a través del [Centro de opiniones](../monetize/launch-feedback-hub-from-your-app.md), caso de no ser necesaria la confidencialidad. 
- Puedes revisar los [informes analíticos](analytics.md) referentes a tu aplicación, lo que incluye los informes de uso y estado y las clasificaciones o los comentarios de los evaluadores.
- Al distribuir la aplicación a los evaluadores, puedes incluir complementos. Dado que probablemente no quieras cobrarles dinero por un complemento, puedes [generar códigos promocionales](generate-promotional-codes.md) y distribuirlos a los evaluadores para permitirles agregar el complemento de forma gratuita, o puedes establecer el precio para el complemento al valor **Gratis** durante las pruebas (a continuación, antes de que la aplicación esté disponible para otros clientes, crea un nuevo envío para el complemento y cambia su precio). Ten en cuenta que cada complemento solo se puede comprar una vez por cada cuenta de Microsoft, por lo que el mismo evaluador no podrá probar el proceso de adquisición del complemento más de una vez. 
- Puedes proporcionar a los evaluadores una versión actualizada de la aplicación en cualquier momento, creando un nuevo envío con nuevos paquetes. Los evaluadores obtendrán la actualización cuando supere el proceso de certificación, al igual que obtuvieron el paquete original, pero nadie más podrá obtenerla (a menos que realices cambios adicionales, como mover una aplicación de **Audiencia privada** a **Audiencia pública** o cambiar la pertenencia a grupos que puedan obtenerla).

## <a name="private-audience"></a>Audiencia privada

Si quieres permitir que los evaluadores usen tu aplicación antes de que esté disponible para otras personas y asegurarte de que nadie más pueda ver su descripción, usa la opción **Audiencia privada** en [visibilidad](choose-visibility-options.md) (en la página **Precios y disponibilidad** de tu envío). Este es el único método que te permite distribuir tu aplicación a los evaluadores mientras que impide por completo que el resto pueda ver una descripción de Store de la aplicación, incluso si pudieran escribirla en su vínculo directo. 

El **audiencia privada** opción solo puede usarse cuando ya no publica la aplicación a una audiencia pública. Puede usar esta opción con las aplicaciones destinadas a cualquier versión del sistema operativo, pero los evaluadores deben ejecutar Windows 10, versión 1607 o superior (incluido Xbox One) y deben haber iniciado sesión con la cuenta Microsoft asociada con la dirección de correo electrónico que proporcione.

Para obtener más información, consulta [Audiencia privada](choose-visibility-options.md#audience).


## <a name="package-flights"></a>Paquetes piloto

Si ya has publicado tu aplicación, puedes crear paquetes piloto para distribuir un conjunto de paquetes diferente a las personas que especifiques. Puedes incluso crear varios paquetes piloto para usar la misma aplicación con diferentes grupos de personas. Esta es una manera excelente de probar diferentes paquetes simultáneamente y puedes extraer paquetes de una versión piloto e incluirlos en el envío de la versión final si decides que los paquetes están listos para distribuirlos a todo el mundo.

Los paquetes piloto pueden usarse con aplicaciones destinadas a cualquier versión de sistema operativo, pero los evaluadores solo pueden obtener la aplicación si ejecutan la compilación Windows.Desktop 10586.63 o posterior, la compilación Windows.Mobile 10586.63 o posterior, o Xbox One.

Para obtener más información, consulta [Paquetes piloto](package-flights.md).


<span id="hide" />

## <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>Ocultar la aplicación en la Tienda y usar códigos promocionales

Esta opción ofrece otra forma de limitar la distribución de una aplicación a solo un determinado grupo de evaluadores, evitando que nadie de la detección de la aplicación en el Store (o adquirirlos sin un código promocional). Sin embargo, a diferencia de la opción de audiencia pública, podría ser posible para cualquiera ver la descripción de tu aplicación si dispone del vínculo directo. Si la confidencialidad es fundamental para el envío, se recomienda por el contrario publicar para una audiencia privada.

La ocultación de la aplicación y el uso de códigos promocionales pueden usarse con aplicaciones destinadas a cualquier versión del sistema operativo, pero los evaluadores solo podrán obtener la aplicación si ejecutan Windows 10.

Para usar esta opción:

- En la sección **Visibilidad** de la página **Precios y disponibilidad**, en [Detectabilidad](choose-visibility-options.md#discoverability), selecciona **Hacer disponible este producto, pero no descubrible, en Store**. Elija la opción de **detener adquisición: Cualquier cliente con un vínculo directo puede ver la lista de Store del producto, pero solo puede descargar si pertenece el producto antes de, o tienen un código promocional y está usando un dispositivo Windows 10**. 
- Cuando la aplicación supere la certificación, [genera códigos promocionales](generate-promotional-codes.md) para ella y distribúyelos a los evaluadores. Puedes generar códigos que permiten hasta 1600 canjes para una misma aplicación en un período de seis meses. Estos códigos proporcionan a los evaluadores un vínculo directo a la descripción de la aplicación y les permite descargarla de forma gratuita, aunque hayas fijado un precio para ella cuando creaste el envío.
- Cuando estés listo para poner la aplicación a disposición del público, crea un nuevo envío y cambia la opción **Visibilidad** a **Hacer disponible y descubrible esta aplicación en Store** (junto con cualquier otro cambio que quieras realizar).


## <a name="targeted-distribution-with-a-link-to-your-apps-listing"></a>Distribución dirigida con un vínculo a la descripción de la aplicación

A diferencia de las opciones descritas anteriormente, esta opción funciona para los clientes de Windows Phone 8.1, así como de Windows 10 (pero no en Windows 8 .x). Ningún cliente podrá encontrar la aplicación mediante búsqueda o exploración en Store, pero quienes dispongan del vínculo directo a su descripción de Store podrán descargarla en un dispositivo que ejecute Windows Phone 8.1 (o versiones anteriores) o Windows 10. No olvides que, para que tus evaluadores puedan descargar la aplicación sin costo, debes fijar su precio al valor **Gratis**.

Para usar esta opción:
- En la sección **Visibilidad** de la página **Precios y disponibilidad**, en [Detectabilidad](choose-visibility-options.md#discoverability), selecciona **Hacer disponible este producto, pero no descubrible, en Store**. Elija la opción de **solo vínculo directo: Cualquier cliente con un vínculo directo a la publicación del producto puede descargarlo, excepto en Windows 8.x.**.
- Después de que tu producto se haya publicado, distribuye el vínculo (la dirección **URL** de la [Página de identidad de la aplicación](view-app-identity-details.md)) a los evaluadores para que puedan probarla.
- Cuando estés listo para poner la aplicación a disposición del público, crea un nuevo envío y cambia la opción **Visibilidad** a **Hacer disponible y descubrible esta aplicación en Store** (junto con cualquier otro cambio que quieras realizar).

> [!IMPORTANT]
> A partir del 31 de octubre de 2018, los productos recién creada no incluyen los paquetes destinados a Windows Phone 8.x o versiones anteriores. Para obtener más información, consulte este [entrada de blog](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

## <a name="targeted-distribution-to-windows-phone-customers-with-specified-email-addresses"></a>Distribución dirigida a clientes de Windows Phone con direcciones de correo electrónico específicas

> [!IMPORTANT]
> Esta opción no está disponible para nuevos envíos. Si hubieras seleccionado previamente esta opción para una aplicación destinada a Windows Phone 8.1 o versiones anteriores, podrás seguir usándola para esa aplicación. Puedes realizar cambios en la lista de evaluadores (hasta 10.000) creando un nuevo envío. 

Con esta opción, las personas con las direcciones de correo electrónico que hayas especificado podrán descargar la aplicación (solo en dispositivos que ejecuten Windows Phone 8.1 o versiones anteriores) mediante el uso del vínculo directo a su descripción. Ningún otro cliente podrá descargar la aplicación, aunque disponga del vínculo, y no podrán encontrar la aplicación en Store ni buscando ni explorando. Para que los evaluadores puedan descargar la aplicación, deberás darles su vínculo (la **dirección URL** de la [página de identidad de la aplicación](view-app-identity-details.md)), y deberán iniciar sesión con una cuenta de Microsoft asociada a una dirección de correo electrónico que hayas facilitado. También puede hacer la aplicación disponible para los evaluadores en dispositivos Windows 10 por [generar códigos promocionales](generate-promotional-codes.md); cualquier usuario con uno de los códigos promocionales de la aplicación puede descargar en un dispositivo Windows 10, incluso si no ha especificado su correo electrónico aquí.
