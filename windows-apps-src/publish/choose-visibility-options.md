---
author: jnHs
Description: Set restrictions on how your app can be discovered and acquired, including whether people can find your app in the Store or see its Store listing at all.
title: Elegir las opciones de visibilidad
ms.author: wdg-dev-content
ms.date: 08/10/2018
ms.topic: article
keywords: windows 10, uwp, visibilidad, audiencia privada, disponible, descubrible
ms.localizationpriority: medium
ms.openlocfilehash: 8a83f1ea4547e60547e427cedd5ad5338e450762
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5687691"
---
# <a name="choose-visibility-options"></a>Elegir las opciones de visibilidad


La sección **Visibility** de la página [Precios y disponibilidad](set-app-pricing-and-availability.md) te permite establecer restricciones sobre cómo se puede detectar y adquirir la aplicación. Esto te ofrece la opción de especificar si las personas pueden encontrar tu aplicación en Store o ver su descripción de Store completamente.

Hay dos secciones separadas en la sección Visibilidad: **Audiencia** y **Detectabilidad**. 

## <a name="audience"></a>Audiencia

La sección Audiencia te permite especificar si quieres restringir la visibilidad de tu envío a una audiencia específica que definas.


### <a name="public-audience"></a>Audiencia pública

De manera predeterminada, la descripción de Store de las aplicaciones será visible para una **Audiencia pública**. Esto es apropiado para la mayoría de los envíos, a menos que quieras limitar quién puede ver la lista de aplicaciones a personas concretas. También puedes usar las opciones de la sección [Detectabilidad](#discoverability) para restringir la detectabilidad si lo deseas.

> [!IMPORTANT]
> Si envías un producto con esta opción establecida en **Audiencia pública**, no puedes elegir **Audiencia privada** en envíos posteriores.


### <a name="private-audience"></a>Audiencia privada

Si quieres que la descripción de tu aplicación sea visible solo a personas seleccionadas que especifiques, elige **Audiencia privado**. Con esta opción, la aplicación no estará descubrible ni disponible para todos aquellas personas que no sean de los grupos que especifiques. A menudo se usa esta opción para [pruebas beta](beta-testing-and-targeted-distribution.md), ya que te permite distribuir tu aplicación a los evaluadores sin que nadie más pueda obtener la aplicación, ni siquiera ver su descripción de Store (incluso si pudieran escribirla en su dirección URL de descripción de Store).

Cuando eliges **Audiencia privada**, tendrás que especificar al menos un grupo de personas que deberían obtener la aplicación. Puedes elegir entre un [grupo de usuarios conocidos](create-known-user-groups.md) existente, o puedes seleccionar **Crear un nuevo grupo** para definir un nuevo grupo. Deberás escribir las direcciones de correo asociadas con la cuenta de Microsoft de cada persona que te gustaría incluir en el grupo. Para obtener más información, consulta [Crear grupos de usuarios conocidos](create-known-user-groups.md).

Después de publicar el envío, las personas del grupo que especifiques podrán ver la descripción de la aplicación y descargarla, siempre y cuando hayan iniciado sesión con la cuenta de Microsoft asociada con el correo electrónico que hayas introducido y estén ejecutando Windows 10, versión 1607 o posterior (incluyendo Xbox One). Sin embargo, las personas que no estén en tu audiencia privada no podrán ver la lista de aplicaciones ni descargar la aplicación, independientemente de qué versión del sistema operativo estén ejecutando. Puedes publicar envíos actualizados para la audiencia privada, que se distribuirán a los miembros de esa audiencia de la misma forma que una actualización normal de la aplicación (pero seguirán sin estar disponibles para todos los que no estén en tu audiencia privada, a menos que cambies la selección de audiencia). 

Si tienes previsto hacer la aplicación disponible a una audiencia pública en una determinada fecha y hora, puedes seleccionar la casilla denominada **Hacer este producto público el** al crear el envío. Escribe la fecha y hora (en hora UTC) en que te gustaría que el producto pasara a estar disponible para el público. No olvides lo siguiente:

- La fecha y hora que selecciones se aplicará a todos los mercados. Si quieres personalizar la programación del lanzamiento para diferentes mercados, no uses esta casilla. En su lugar, crea un nuevo envío que cambie la configuración a **Audiencia pública** y, a continuación, usa las opciones de [Programación](configure-precise-release-scheduling.md) para especificar los tiempos de tu lanzamiento.
- Especificar una fecha para **Hacer este producto público el** no se aplica a Microsoft Store para Empresas y/o a Microsoft Store para Educación. Para permitirnos que ofrecer tu aplicación a estos clientes mediante licencias de organización, tendrás que crear un nuevo envío con **Audiencia pública** seleccionada (y [licencias de organización](organizational-licensing.md) habilitado).
- Después de la fecha y hora que selecciones, todos los futuros envíos usarán **Audiencia pública**.

Si no especificas una fecha y hora para que la aplicación esté disponible para una audiencia privada, siempre podrás hacerlo más adelante, creando un nuevo envío y cambiando configuración de audiencia a **Audiencia privada** a **Audiencia pública**. Al hacerlo, ten en cuenta que la aplicación puede pasar por un proceso de certificación adicional, por lo que deberás estar preparado para solucionar cualquier nuevo problema de certificación que pueda surgir. 

Estas son algunas cosas importantes que debes tener en cuenta al elegir distribuir la aplicación a un público privado:
- Las personas de tu audiencia privada podrán obtener la aplicación mediante un vínculo específico a la descripción de Store de tu aplicación que les requerirá iniciar sesión con su cuenta de Microsoft para poder verla. Este vínculo se proporciona cuando se selecciona **Audiencia privada**. También puedes encontrarlo en tu página [Identidad de la aplicación](view-app-identity-details.md), en la dirección **URL si tu aplicación solo es visible para determinadas personas (requiere autenticación)**. Asegúrate de proporcionar a los evaluadores este vínculo, no la dirección URL normal de la descripción de Store.  
- A menos que elijas una opción en **Detectabilidad** que lo evite, las personas de tu audiencia privada podrán encontrar tu aplicación buscando en la aplicación de Microsoft Store. Sin embargo, la descripción web no se podrá detectar mediante búsqueda, incluso para las personas de esa audiencia. 
- No podrás [configurar las fechas de lanzamiento en la sección Programación](configure-precise-release-scheduling.md) de la **Página de precios y disponibilidad**, dado que la aplicación no se distribuirá a clientes de fuera de tu audiencia privada.
- El resto de selecciones que hagas se aplicarán a las personas de esta audiencia. Por ejemplo, si eliges un precio que no sea **Gratis**, las personas de tu audiencia privada tendrán que pagar dicho precio para poder adquirir la aplicación. 
- Si quieres distribuir paquetes diferentes a distintas personas de tu audiencia privada después de tu envío inicial, puedes utilizar [paquetes piloto](package-flights.md) para distribuir diferentes actualizaciones de paquetes a subconjuntos de tu audiencia privada. Puedes crear grupos de usuarios conocidos adicionales para definir quiénes deberían obtener un paquete piloto específico.
- Puedes editar la pertenencia de los grupos de usuarios conocidos en tu audiencia privada. Sin embargo, ten en cuenta que, si quitas a alguien que estaba en el grupo y hubiera descargado anteriormente tu aplicación, aún podrá usar la aplicación, pero no podrá seguir usando las actualizaciones que proporciones (a menos que elijas **Audiencia pública** en fecha posterior).
- La aplicación no estará disponible a través de Microsoft Store para Empresas y/o Microsoft Store para Educación, independientemente de la configuración de licencias de organización, incluso para las personas de tu público privado.
- Mientras que Store garantizará que tu aplicación solo será visible y estará disponible para las personas que hayan iniciado sesión con una cuenta de Microsoft que hayas agregado a tu audiencia privada, no podemos impedir que esas personas compartan información o capturas de pantalla fuera de tu audiencia privada. Cuando la confidencialidad es fundamental, asegúrate de que tu audiencia privada incluya solo las personas que confíes que no compartirán detalles sobre la aplicación con otras personas.
- Asegúrate de que los evaluadores sepan cómo enviarte sus comentarios. Probablemente no quieras que dejen comentarios en el Centro de opiniones, porque cualquier otro cliente podría verlos. Plantéate incluir un vínculo para que ellos envíen correos electrónicos o proporcionen comentarios de cualquier otra forma.
- Cualquier opinión escrita por personas de tu audiencia privada estará disponible para que puedas verla. Sin embargo, estas reseñas no se publicarán en la descripción de Store de tu aplicación, incluso después de que el envío se pase a **Audiencia pública**. Puedes leer críticas escritas de tu audiencia privada viendo el [Informe de críticas](reviews-report.md) en el Centro de desarrollo, pero no se pueden descargar estos datos ni usar la [API de análisis de Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md) para acceder programáticamente a estas críticas.
- Al mover una aplicación de **Audiencia privada** a **Audiencia pública**, la **Fecha de lanzamiento** que se mostrará en la descripción de Store será la fecha en la que se publicó por primera vez para la audiencia pública.

## <a name="discoverability"></a>Detectabilidad

Las selecciones de la sección **Detectabilidad** indican cómo los clientes pueden descubrir y comprar tu aplicación. 

> [!IMPORTANT]
> Si seleccionas **Audiencia privada**, tus selecciones de **Detectabilidad** solo se aplican a las personas de tu audiencia privada. Los clientes que no estén en los grupos que hayas especificado no podrán obtener la aplicación, ni siquiera ver su la descripción. 


### <a name="make-this-product-available-and-discoverable-in-the-store"></a>Hacer ese producto disponible y detectable en Store

Esta es la opción predeterminada. Deja esta opción seleccionada si quieres que tu aplicación que se mostrarán en la tienda para los clientes puedan buscar a través del vínculo directo de la aplicación o por otros métodos, como la búsqueda, la exploración y la inclusión en listas selectas. 

### <a name="make-this-product-available-but-not-discoverable-in-the-store"></a>Hacer ese producto disponible pero no detectable en Store

Cuando se selecciona esta opción, la aplicación no pueden encontrarla en Store los clientes ni buscando ni explorando; la única manera de acceder a la descripción de tu aplicación es un vínculo directo. 

> [!TIP]
> Si no quieres que la descripción sea visible para el público, incluso aunque dispongan de un vínculo directo, elige **Audiencia privada** en la sección **Audiencia** sección, como se describió anteriormente.

También debes elegir una de las siguientes opciones para especificar cómo se puede adquirir tu aplicación:


>[!IMPORTANT]
> Cada una de estas opciones limita las versiones de sistema operativo en el que los clientes pueden comprar tu aplicación. Lee las descripciones con cuidado para asegurarte de que conoces qué versiones del sistema operativo son compatibles. 

- **Solo vínculo directo: Cualquier cliente con un vínculo directo a la descripción del producto puede descargarla, excepto en Windows 8.x.** Cualquier cliente que obtenga la descripción de la aplicación a través de un vínculo directo puede descargarla en dispositivos que ejecuten Windows 10, o en dispositivos que ejecuten Windows Phone 8.1 o versiones anteriores (pero no en dispositivos que ejecuten Windows 8.x).
- **Detener la compra: Los clientes con un vínculo directo podrán ver la descripción de la aplicación en la Store, pero solo podrán descargarla si ya tienen el producto, tienen un código promocional y están usando un dispositivo Windows 10.** Incluso si un cliente tiene un vínculo directo, no se puede descargar la aplicación a no ser que tenga un [código promocional](generate-promotional-codes.md) y esté usando un dispositivo Windows 10. Si un cliente dispone de un código promocional, puede usarlo para obtener tu aplicación de forma gratuita (solo en Windows 10), aunque no se la ofrezcas a ningún otro cliente. Además de usar un código promocional, no existe manera alguna de que otra persona pueda acceder a tu aplicación.

> [!TIP]
> Si quieres dejar de ofrecer una aplicación a los nuevos clientes, puedes seleccionar **Hacer la aplicación no disponible** desde su página de información general. Después de confirmar que quieres que la aplicación deje de estar disponible, en el plazo de unas horas dejará de estar visible en Microsoft Store y ningún cliente nuevo podrá acceder a ella (a no ser que tengan un [código promocional](generate-promotional-codes.md) y usen un dispositivo Windows 10). Esta acción anulará las selecciones de **Visibilidad** en el envío. Para que la aplicación vuelva a estar disponible para los nuevos clientes (por selecciones de **Visibilidad**), puedes hacer clic en **Make app available** desde la página de información general en cualquier momento. Para obtener más información, consulta [Quitar una aplicación de Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).




