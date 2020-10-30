---
description: Establezca restricciones en el modo en que se puede detectar y adquirir la aplicación, incluido si las personas pueden encontrar la aplicación en la tienda o ver la lista de la tienda.
title: Elegir las opciones de visibilidad
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, visibilidad, público privado, disponible, reconocible
ms.localizationpriority: medium
ms.openlocfilehash: 3873afb39a8f28cd887617ef36c606a16e5b9bb5
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030018"
---
# <a name="choose-visibility-options"></a>Elegir las opciones de visibilidad


La sección **visibilidad** de la [Página precios y disponibilidad](set-app-pricing-and-availability.md) le permite establecer restricciones en el modo en que la aplicación se puede detectar y adquirir. Esto le ofrece la opción de especificar si los usuarios pueden encontrar la aplicación en la tienda o ver la lista de la tienda.

Hay dos secciones independientes en la sección de visibilidad: **audiencia** y **detectabilidad** . 

## <a name="audience"></a>Público

La sección audiencia le permite especificar si desea restringir la visibilidad de su envío a una audiencia específica que defina.


### <a name="public-audience"></a>Público público

De forma predeterminada, la lista de tiendas de la aplicación será visible para un **público público** . Esto es adecuado para la mayoría de los envíos, a menos que quiera limitar quién puede ver la lista de la aplicación a personas específicas. También puede usar las opciones de la sección [detectabilidad](#discoverability) para restringir la detección si lo desea.

> [!IMPORTANT]
> Si envía un producto con esta opción establecida **en público público, no** puede elegir **audiencia privada** en un envío posterior.


### <a name="private-audience"></a>Público privado

Si quiere que la lista de la aplicación solo esté visible para las personas seleccionadas que especifique, elija **audiencia privada** . Con esta opción, la aplicación no se podrá detectar ni estar disponible para ningún usuario que no sea de los usuarios de los grupos que especifique. Esta opción se usa a menudo para las [pruebas beta](beta-testing-and-targeted-distribution.md), ya que le permite distribuir la aplicación a los evaluadores sin que nadie más pueda obtener la aplicación, o incluso ver la lista de la tienda (aunque pueda escribir en la dirección URL de la lista de la tienda).

Al elegir **público privado** , debe especificar al menos un grupo de personas que deben obtener la aplicación. Puede elegir entre un grupo de [usuarios conocido](create-known-user-groups.md)existente, o puede seleccionar **crear un nuevo grupo** para definir un nuevo grupo. Deberá escribir las direcciones de correo electrónico asociadas al cuenta de Microsoft de cada persona que le gustaría incluir en el grupo. Para obtener más información, consulte [crear grupos de usuarios conocidos](create-known-user-groups.md).

Una vez publicado el envío, las personas del grupo que especifique podrán ver la lista de la aplicación y descargar la aplicación, siempre y cuando hayan iniciado sesión con el cuenta de Microsoft asociado a la dirección de correo electrónico que escribió y ejecuten Windows 10, versión 1607 o posterior (incluida Xbox One). Sin embargo, las personas que no están en su audiencia privada no podrán ver la lista de la aplicación ni descargar la aplicación, independientemente de la versión del sistema operativo que ejecuten. Puede publicar envíos actualizados en el público privado, que se distribuirá a los miembros de la audiencia de la misma manera que una actualización de la aplicación normal (pero aún no estará disponible para los usuarios que no estén en el público privado, a menos que cambie la selección de audiencia). 

Si planea poner la aplicación a disposición de un público público en una determinada fecha y hora, puede activar la casilla **hacer que este producto sea público** al crear el envío. Escriba la fecha y hora (en UTC) en las que desea que el producto esté disponible para el público. Tenga en cuenta lo siguiente:

- La fecha y la hora que seleccione se aplicarán a todos los mercados. Si desea personalizar la programación de versión para distintos mercados, no use este cuadro. En su lugar, cree un nuevo envío que cambie la configuración a público **público** y, a continuación, use las opciones de [programación](configure-precise-release-scheduling.md) para especificar la temporización de la versión.
- No se aplica a la Microsoft Store para la empresa o Microsoft Store para la educación que escriba una fecha para **que este producto sea público** . Para que podamos ofrecer tu aplicación a estos clientes a través de licencias de la organización, tendrás que crear un nuevo envío con público **público** seleccionado (y [licencias de organización](organizational-licensing.md) habilitadas).
- Después de la fecha y la hora que seleccione, todos los envíos futuros utilizarán **audiencias públicas** .

Si no especifica una fecha y una hora para que la aplicación esté disponible para un público público, siempre puede hacerlo más adelante mediante la creación de un nuevo envío y el cambio de la configuración de la audiencia de público **privado** a público **público** . Al hacerlo, tenga en cuenta que la aplicación puede pasar por un proceso de certificación adicional, por lo que debe estar preparado para solucionar los nuevos problemas de certificación que puedan surgir. 

Estos son algunos aspectos importantes que hay que tener en cuenta a la hora de elegir la opción de distribuir la aplicación a un público privado:
- Los usuarios de su público privado podrán obtener la aplicación mediante un vínculo específico a la lista de la tienda de la aplicación que les obliga a iniciar sesión con su cuenta de Microsoft para poder verla. Este vínculo se proporciona al seleccionar **audiencia privada** . También puede encontrarla en la página identidad de la [aplicación](view-app-identity-details.md) en **dirección URL si la aplicación solo está visible para determinadas personas (requiere autenticación)** . Asegúrese de proporcionar a los evaluadores este vínculo, no la dirección URL normal a la lista de tiendas.  
- A menos que elija una opción en **detectabilidad** que lo impida, las personas de su público privado podrán encontrar su aplicación buscando en la aplicación Microsoft Store. Sin embargo, la lista Web no se podrá detectar a través de la búsqueda, ni siquiera para los usuarios de esa audiencia. 
- No podrá [configurar fechas de lanzamiento en la sección Programación](configure-precise-release-scheduling.md) de la **Página precios y disponibilidad** , ya que la aplicación no se lanzará a los clientes que no pertenecen a su público privado.
- Otras selecciones que realice se aplicarán a los usuarios de esta audiencia. Por ejemplo, si elige un precio distinto de **gratis** , las personas de la audiencia privada tendrán que pagar ese precio para adquirir la aplicación. 
- Si quiere distribuir distintos paquetes a diferentes personas de su público privado, después de su envío inicial, puede usar [paquetes piloto](package-flights.md) para distribuir diferentes actualizaciones de paquetes a subconjuntos de su público privado. Puede crear grupos de usuarios conocidos adicionales para definir quién debe obtener un vuelo de paquete específico.
- Puede editar la pertenencia de los grupos de usuarios conocidos en la audiencia privada. Sin embargo, tenga en cuenta que si quita a alguien que se encontraba en el grupo y descargó previamente la aplicación, podrá seguir usando la aplicación, pero no recibirá las actualizaciones que proporcione (a menos que elija **público público** más adelante).
- La aplicación no estará disponible a través de la Microsoft Store para empresas o Microsoft Store para educación, independientemente de la configuración de la licencia de su organización, incluso de personas de su público privado.
- Aunque el almacén se asegurará de que la aplicación solo esté visible y disponible para los usuarios que han iniciado sesión con una cuenta de Microsoft que ha agregado a su público privado, no podemos evitar que esas personas compartan información o capturas de pantallas fuera de su público privado. Cuando la confidencialidad sea crítica, asegúrese de que la audiencia privada solo incluye a las personas en las que confía no compartir los detalles de la aplicación con otros usuarios.
- Asegúrese de que los evaluadores sepan cómo proporcionarle sus comentarios. Probablemente no querrá que dejen comentarios en el centro de comentarios, ya que cualquier otro cliente podría ver ese comentario. Considere la posibilidad de incluir un vínculo para que envíe correo electrónico o proporcione comentarios de alguna otra manera.
- Todas las revisiones escritas por personas de su público privado estarán disponibles para su visualización. Sin embargo, estas revisiones no se publicarán en la descripción de la tienda de la aplicación, incluso después de que el envío se mueva a público **público** . Puede leer las revisiones escritas por su público privado viendo el [Informe de revisiones](reviews-report.md), pero no puede descargar estos datos ni usar la [API de Microsoft Store Analytics](../monetize/access-analytics-data-using-windows-store-services.md) para acceder mediante programación a estas revisiones.
- Al migrar una aplicación de **público privado** a público **público** , la **fecha de lanzamiento** que se muestra en la lista de la tienda será la fecha en que se publicó por primera vez en el público público.

## <a name="discoverability"></a>Detectabilidad

Las selecciones en la sección **detectabilidad** indican cómo los clientes pueden detectar y adquirir su aplicación. 

> [!IMPORTANT]
> Si selecciona **público privado** , las selecciones de **detectabilidad** solo se aplican a las personas de su público privado. Los clientes que no están en los grupos especificados no podrán obtener la aplicación ni siquiera ver su lista. 


### <a name="make-this-product-available-and-discoverable-in-the-store"></a>Hacer que este producto esté disponible y reconocible en la tienda

Ésta es la opción predeterminada. Deje esta opción seleccionada si quiere que la aplicación se muestre en la tienda para que los clientes la encuentren a través del vínculo directo de la aplicación o de otros métodos, como la búsqueda, la exploración y la inclusión en listas de seleccionada. 

### <a name="make-this-product-available-but-not-discoverable-in-the-store"></a>Hacer que este producto esté disponible, pero no se puede detectar en el almacén

Al seleccionar esta opción, la aplicación no puede encontrarse en la tienda por los clientes que busquen o exploren. la única manera de llegar a la lista de la aplicación es mediante un vínculo directo. 

> [!TIP]
> Si no desea que la lista sea visible para el público, incluso con un vínculo directo, elija **audiencia privada** en la sección **audiencia** , tal como se ha descrito anteriormente.

También debe elegir una de las siguientes opciones para especificar cómo se puede adquirir la aplicación:


>[!IMPORTANT]
> Cada una de estas opciones limita las versiones del sistema operativo en las que los clientes pueden adquirir su aplicación. Lea detenidamente las descripciones para asegurarse de que sabe qué versiones del sistema operativo se admiten. 

- **Solo vínculo directo: cualquier cliente con un vínculo directo a la lista del producto puede descargarlo, excepto en Windows 8. x.** Cualquier cliente que obtenga la lista de la aplicación a través de un vínculo directo puede descargarla en dispositivos que ejecuten Windows 10 o en dispositivos que ejecuten Windows Phone 8,1 o una versión anterior (pero no en dispositivos que ejecuten Windows 8. x).
- **Detener la adquisición: cualquier cliente con un vínculo directo puede ver la lista de la tienda del producto, pero solo puede descargarla si poseía el producto antes o tiene un código promocional y está usando un dispositivo de Windows 10.** Incluso si un cliente tiene un vínculo directo, no puede descargar la aplicación a menos que tenga un [código promocional](generate-promotional-codes.md) y esté usando un dispositivo de Windows 10. Si un cliente tiene un código promocional, puede usarlo para obtener la aplicación de forma gratuita (solo en Windows 10), aunque no la esté ofreciendo a ningún otro cliente. Además de usar un código promocional, no hay ninguna manera de que nadie pueda obtener su aplicación.

> [!TIP]
> Si quiere dejar de ofrecer una aplicación a los clientes nuevos, puede seleccionar hacer que la **aplicación no esté disponible** en su página de información general. Después de confirmar que desea que la aplicación no esté disponible, en unas horas dejará de estar visible en el almacén y ningún cliente nuevo podrá obtenerla (a menos que tenga un [código promocional](generate-promotional-codes.md) y se encuentre en un dispositivo Windows 10). Esta acción invalidará las selecciones de **visibilidad** en el envío. Para que la aplicación esté disponible para los nuevos clientes de nuevo (según las selecciones de **visibilidad** ), puede hacer clic en **crear aplicación disponible** en la página de información general en cualquier momento. Para obtener más información, consulta [Quitar una aplicación de la Tienda](guidance-for-app-package-management.md#removing-an-app-from-the-store).




