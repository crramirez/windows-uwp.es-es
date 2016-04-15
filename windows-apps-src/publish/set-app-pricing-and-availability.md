---
Description: La página Precios y disponibilidad del proceso de envío de la aplicación permite determinar cuánto costará tu aplicación, independientemente de si ofreces una prueba gratuita y cómo, cuándo y dónde estará disponible para los clientes.
title: Establecer los precios y la disponibilidad de las aplicaciones
ms.assetid: 37BE7C25-AA74-43CD-8969-CBA3BD481575
---

# Establecer los precios y la disponibilidad de las aplicaciones


La página **Precios y disponibilidad** del [proceso de envío de la aplicación](app-submissions.md) permite determinar cuánto costará tu aplicación, independientemente de si ofreces una prueba gratuita y cómo, cuándo y dónde estará disponible para los clientes. A continuación, te guiaremos por las opciones de esta página y lo que debes tener en cuenta al escribir la información.

## Precio base


El primer elemento de esta página te permite seleccionar un precio base de la aplicación. Puedes elegir ofrecerla de forma gratuita o puede seleccionar una de las franjas de precios disponibles. Es necesario especificar un precio base para enviar la aplicación.

Para más información, consulta [Definir la selección de precios y mercados](define-pricing-and-market-selection.md).

## Prueba gratuita


Muchos desarrolladores eligen permitir que los clientes prueben su aplicación de forma gratuita con la funcionalidad de prueba proporcionada por la Tienda. De manera predeterminada, una aplicación no estará disponible como prueba gratuita, pero si quieres ofrecer una, seleccione un valor de la lista desplegable **Prueba gratuita**.

Elige **La versión de prueba no expira nunca** para permitir a los clientes el acceso a la aplicación de forma gratuita indefinidamente. Querrás animarlos para que adquieran la versión completa, así que asegúrate de agregar código para [excluir o limitar funciones en la versión de prueba](https://msdn.microsoft.com/library/windows/apps/mt219685).

También tienes la opción de seleccionar una versión de prueba con límite de tiempo de **1 día**, **7 días**, **15 días** o **30 días**. Puedes seguir limitando funciones durante el período de prueba o puedes permitir a los clientes acceder a toda la funcionalidad durante ese período de tiempo.

> **Nota**  Las pruebas de tiempo limitado no se muestran a los clientes en Windows Phone 8.1 y versiones anteriores.

## Mercados y precios personalizados


De manera predeterminada, tu aplicación figurará en todos los mercados posibles al precio base. Puedes cambiar esta configuración para incluir o excluir mercados específicos y cambiar el precio de la aplicación en cualquier mercado en que la ofrezcas en la sección **Mercados y precios personalizados**. Para más información, consulta [Definir la selección de precios y mercados](define-pricing-and-market-selection.md).

## Precio de oferta


Si quieres ofrecer tu aplicación a un precio reducido durante un período de tiempo limitado, puedes crear y programar una oferta. Para obtener más información, consulta [Poner aplicaciones e IAP en oferta](put-apps-and-iaps-on-sale.md).

## Distribución y visibilidad


La sección **Distribución y visibilidad** te permite establecer restricciones sobre cómo se puede detectar y adquirir la aplicación.

El valor predeterminado es **Make this app available in the Store**. Esto significa que tu aplicación figurará en la Tienda para que los clientes puedan buscar a través del vínculo directo de la aplicación o por otros métodos, como la búsqueda, la exploración y la inclusión en listas selectas.

Si quieres ocultar la aplicación en la Tienda pero aún ponerla a disposición de determinadas personas, selecciona una de las siguientes opciones para limitar la disponibilidad de tu aplicación. Ten en cuenta que los clientes de Windows 8 y Windows 8.1 no podrán obtener la aplicación si eliges cualquiera de estas opciones.

-   **Ocultar esta aplicación y evitar la adquisición. Los clientes con un código promocional aún pueden descargarla en dispositivos Windows 10**: ningún cliente puede encontrar la aplicación en la Tienda a través de la búsqueda o exploración, pero puedes [generar códigos promocionales](generate-promotional-codes.md) para distribuirla entre personas concretas en Windows 10. Estas personas pueden usar el vínculo y el código para obtener la aplicación de forma gratuita, aunque ya no se la ofrezcas a ningún otro cliente.
-   **Ocultar la aplicación en la Tienda. Los clientes que dispongan de un vínculo directo a la descripción de la aplicación podrán continuar descargándola, salvo en Windows 8 y Windows 8.1**: ningún cliente puede encontrar la aplicación en la Tienda a través de la búsqueda o exploración, pero los clientes con el vínculo directo a la descripción de la aplicación pueden descargar esta en dispositivos que ejecutan Windows 10 o Windows Phone 8.1 y versiones anteriores.
-   **Ocultar esta aplicación y ponerla a la disposición de solo las personas que especifiques abajo, quienes podrán descargar esta aplicación en dispositivos Windows Phone 8.x. Se puede usar un código de promoción para descargar esta aplicación en dispositivos Windows 10**: ningún cliente puede encontrar la aplicación en la Tienda a través de la búsqueda o la exploración, y únicamente los clientes de Windows Phone 8.x cuyas direcciones de correo electrónico (asociadas a sus cuentas de Microsoft) hayas especificado en el cuadro (separadas por punto y coma) pueden descargar la aplicación mediante el vínculo directo a su descripción. También puedes [generar códigos promocionales](generate-promotional-codes.md) para distribuirla entre personas concretas en Windows 10. A menudo se usa esta opción para las [pruebas beta](beta-testing-and-targeted-distribution.md) en Windows Phone 8.1 y versiones anteriores. Ten en cuenta que esta opción solo se puede seleccionar si nunca antes has publicado la aplicación con la opción **Distribución y visibilidad** establecida en **Todo el mundo puede encontrar tu aplicación en la Tienda**.

> **Nota**  Para dejar de ofrecer completamente una aplicación a los clientes nuevos, haz clic en **Make app unavailable** desde la página Información general de la aplicación. Después de confirmar que quieres que la aplicación deje de estar disponible, en el plazo de unas horas dejará de estar visible en la Tienda y ningún cliente nuevo podrá acceder a ella de ninguna manera. Esta acción invalidará cualquiera de las opciones aquí seleccionadas: no estará disponible para los clientes nuevos de ninguna manera. Para que vuelva a estar disponible para los nuevos clientes, puedes hacer clic en **Make app available** desde la página Información general de la aplicación en cualquier momento. Para más información, consulta [Quitar una aplicación de la Tienda](guidance-for-app-package-management.md#removing-an-app-from-the-store).

## Familias de dispositivos Windows 10

Esta sección te permite indicar qué tipos de clientes de dispositivos Windows 10 pueden usarse para adquirir tu aplicación. (Si el paquete no se ejecuta en un determinado tipo de dispositivo, no se ofrece para su descarga en ese tipo de dispositivo).

> **Importante**  Para impedir por completo que una determinada familia de dispositivos Windows 10 obtenga tu aplicación, debes actualizar el elemento [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903) del manifiesto de la appx para especificar como destino únicamente la familia de dispositivos compatible (es decir, **Windows.Mobile** o **Windows.Desktop**), en lugar de dejar el valor **Windows.Universal** (de la familia de dispositivos universal) que Microsoft Visual Studio incluye de manera predeterminada en el manifiesto de appx.

De manera predeterminada, los cuadros **Móvil** y **Escritorio** estarán marcados. Se recomienda dejar estas casillas marcadas salvo que tengas un motivo específico para limitar el tipo de dispositivos Windows 10 que pueden comprar tu aplicación. Por ejemplo, puedes haber creado paquetes universales de Windows, pero sabes que todavía tienes que resolver algunos problemas de la aplicación en dispositivos móviles. Para evitar que los nuevos clientes descarguen la aplicación en dispositivos móviles Windows 10, puedes desmarcar la casilla **Móvil**. Si más adelante decides que la aplicación está lista para dispositivos móviles Windows 10, puedes crear un nuevo envío con la casilla **Móvil** marcada.

Si probaste la aplicación para garantizar que se ejecuta correctamente en Microsoft HoloLens, también puedes marcar el cuadro **Holográfico** para ofrecer la aplicación a clientes de HoloLens. Para obtener más información sobre cómo compilar, probar y publicar aplicaciones holográficas, consulta la [Introducción al desarrollo de Windows Holographic](http://dev.windows.com/holographic/development_overview).

Ten en cuenta que las selecciones que realices en esta sección se aplicarán a todos los paquetes de la aplicación, independientemente de la versión del sistema operativo de destino (Windows 10, Windows 8.x, Windows Phone 8.x, etc.). Sin embargo, afectan a la disponibilidad solo para los clientes que usan dispositivos de Windows 10 (no de Windows 8.x o Windows Phone 8.x).

También es importante tener en cuenta que las selecciones aquí realizadas se aplican solo a las nuevas adquisiciones. Cualquiera que ya tenga la aplicación puede seguir usándola y obtener cualquier actualización que envíes, aunque quites aquí su familia de dispositivos. Esto se aplica incluso a los clientes que adquieren la aplicación antes de actualizar a Windows 10. Por ejemplo, si tienes una aplicación publicada con paquetes de Windows Phone 8.1 y más adelante agregas un paquete de Windows 10 (UWP) con la misma aplicación y con la familia universal de dispositivos como destino, a los clientes móviles de Windows 10 que tuvieran el paquete de Windows Phone 8.1 se les ofrecerá actualizarse a este paquete de Windows 10 (UWP), aunque hayas desmarcado la casilla **Móvil** (ya que no se trata de una nueva compra, sino de una actualización). Sin embargo, si no proporcionas ningún paquete de Windows 10 (UWP) que tenga como destino las familias de dispositivos universal o móvil, tus clientes móviles de Windows 10 permanecerán en el paquete de Windows Phone 8.1.

Para obtener más información acerca de las familias de dispositivos, consulta [Guía de aplicaciones de la Plataforma universal de Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/dn894631) y [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

> **Nota**  También verás una casilla para indicar si quieres permitir que Microsoft ponga la aplicación a disposición de futuras familias de dispositivos Windows 10. Se recomienda dejar esta casilla marcada, de modo que la aplicación esté disponible para más clientes potenciales al incorporarse nuevas familias de dispositivos.

## Licencias organizativas


De manera predeterminada, tu aplicación se puede ofrecer a las organizaciones para su compra por volumen. En esta sección puedes indicar si lo permites, y en qué condiciones.

Para obtener más información, consulta [Opciones de licencias organizativas](organizational-licensing.md).

## Fecha de publicación


Puedes indicar cuándo se publicará la aplicación (o la actualización) seleccionando una opción en la sección **Fecha de publicación**.

-   Elige **Publish this submission as soon as it passes certification** para que este envío esté disponible en la Tienda tan pronto como sea posible.
-   Elige **Publicar este envío manualmente** si no quieres que tu envío se publique hasta que lo indiques. Puedes hacerlo desde la página de estado de certificación haciendo clic en **Publicar ahora** o seleccionando una fecha específica, como se describe a continuación.
-   Elige **No antes del \[fecha\]** para garantizar que el envío no se publique hasta una fecha determinada. Con esta opción, el envío se lanzará lo antes posible en la fecha que especifiques o después de ella. La fecha debe ser al menos 24 horas después del momento actual. Junto con la fecha, también puedes especificar la hora en que el envío debe comenzar a publicarse.
    > **Nota**  Los retrasos durante la certificación o la publicación podrían hacer que la fecha de lanzamiento real sea posterior a la que solicites. La Tienda Windows no puede garantizar que la aplicación (o la actualización) esté disponible en una fecha específica.

También puedes cambiar la fecha de lanzamiento después de enviar la aplicación, siempre y cuando aún no haya entrado en el paso **Publicar**.
 

 






<!--HONumber=Mar16_HO5-->


