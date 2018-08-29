---
author: jnHs
Description: You can use package flights to distribute packages that are only given to a limited test group.
title: Paquetes piloto
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
ms.author: wdg-dev-content
ms.date: 6/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, distribución de paquetes piloto
ms.localizationpriority: medium
ms.openlocfilehash: d5f43173c85bc8a696d7dbc9967e704f79db2b3f
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2918716"
---
# <a name="package-flights"></a>Paquetes piloto

Puedes usar paquetes piloto para distribuir paquetes específicos a un grupo de evaluadores limitado. Los paquetes que ya has publicado en la tienda se usará para otros clientes, por lo que no se interrumpa su experiencia.

Con paquetes piloto, solo los paquetes son diferentes; la detalles del listado de la tienda serán los mismos para todos los clientes. Cualquier persona del grupo piloto recibirán los paquetes que se incluyen en el paquete piloto, mientras que los clientes que no están en el grupo piloto de fin de seguir mostrando los paquetes normales (versión final).  Si más adelante decides que quieres que los paquetes de un paquete piloto estén disponibles para todos los clientes, puede utilizar estos paquetes mismo fácilmente en un envío de versión final. Ten en cuenta que los paquetes piloto debe pasar el [proceso de certificación](the-app-certification-process.md), al igual que cualquier envío.

Cuando configuras paquetes piloto, puedes especificar qué personas deben obtener paquetes específicos agregándolos a un **grupo de usuarios conocido** (que a veces se denomina grupo piloto). Cualquier persona de un grupo piloto que use un dispositivo con una versión de Windows 10 compatible con paquetes piloto (la compilación 10586 o posterior de Windows.Desktop, la compilación 10586.63 o posterior de Windows.Mobile, o Xbox One) obtendrá paquetes de los paquetes piloto que designes para ese grupo en concreto. (Los paquetes piloto pueden incluir paquetes destinados a cualquier versión del sistema operativo, incluidas Windows 8.1 o Windows Phone 8.1 o versiones anterior). Cualquier persona que no se haya agregado a uno de tus grupos piloto o que use un dispositivo no compatible con los paquetes piloto, obtendrá paquetes del envío de versión final.

> [!IMPORTANT] 
> En los dispositivo móviles y de escritorio, las personas incluidas en tus grupos piloto obtendrán automáticamente los paquetes de tu piloto cada vez que proporciones actualizaciones. Sin embargo, los **integrantes de tus grupos piloto que usan dispositivos de Xbox, tendrán que consultar manualmente si hay actualizaciones** para obtener los paquetes más recientes y asegurarse de iniciar sesión en el dispositivo con su cuenta de Microsoft (aquella que tenga la dirección de correo asociada que incluiste en el grupo de usuarios conocido).

Ten en cuenta que no se distribuirán paquetes piloto a través de la [Tienda Microsoft para Empresas](https://businessstore.microsoft.com/store) y la [Tienda Microsoft para Educación](https://educationstore.microsoft.com/store). Esto se debe a que las personas de tus grupos de usuario conocidos deben iniciar sesión con sus cuentas de Microsoft para poder recibir un paquete piloto. Todas aquellas adquisiciones realizadas a través de la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación recibirán los paquetes de la versión final.

> [!TIP]
> Los paquetes piloto ofrecen paquetes solo a los clientes seleccionados que especificas. Para distribuir paquetes a una selección aleatoria de los clientes en un porcentaje especificado, puedes usar el [lanzamiento de paquete gradual](gradual-package-rollout.md). También puedes combinar el lanzamiento con tus paquetes piloto si quieres distribuir una actualización de forma gradual a uno de tus grupos de piloto.
>
> A diferencia de los paquetes piloto, las selecciones de lanzamiento de paquete gradual son aplicables a los clientes que adquieran la aplicación a través de la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación. 

> [!TIP]
> Ten en cuenta el modo en que podrán enviarte comentarios sobre la aplicación aquellas personas que incluyas en el paquete piloto. Te sugerimos que [agregues un control en la aplicación para iniciar el Centro de opiniones](../monetize/launch-feedback-hub-from-your-app.md), para que así tus clientes puedan enviarte sus comentarios directamente; además, podrás leer sus comentarios en el [informe de comentarios](feedback-report.md) de la aplicación.


## <a name="create-a-new-package-flight"></a>Crear un nuevo paquete piloto

Después de publicar en envío para tu aplicación, verás una sección **Paquetes piloto** en la página Información general de la aplicación. Haz clic en **Nuevo paquete piloto** para empezar.

Si no has creado ningún grupo de usuario conocido aún, se te pedirá que crees uno antes de continuar. Para obtener más información, consulta [Crear grupos de usuarios conocidos](create-known-user-groups.md). Puedes crear un nuevo grupo de usuarios conocido directamente desde esta página seleccionando **Crear un grupo piloto**.

En la página de creación del paquete piloto, tendrás que introducir un nombre para dicho paquete y especificar al menos un grupo piloto. Cuando termines, selecciona **Crear piloto**. No podrás cambiar estos detalles más adelante (aunque si no estás satisfecho con lo que has introducido, puedes eliminar este paquete piloto y crear uno nuevo en su lugar).

> [!NOTE]
> Si tienes más de un paquete piloto, deberás asignar una clasificación a cada uno de ellos. Para obtener más información, vea [Agregar y clasificar paquetes piloto adicionales](#add-and-rank-additional-package-flights) más adelante.


## <a name="specify-packages-to-include-in-your-package-flight"></a>Especifica los paquetes que se incluirán en tu paquete piloto

Después de guardar los detalles del paquete piloto, verás la página de información general. Haz clic en **Paquetes** para especificar los paquetes que te gustaría incluir en el paquete piloto. Puedes incluir paquetes destinados a cualquier versión del sistema operativo, incluidas Windows 10, Windows 8.x y Windows Phone 8.x o versiones anteriores.

Tienes la opción de seleccionar paquetes que estaban asociados con un envío publicado anterior (un envío de versión final o uno de tus otros paquetes piloto, si tienes más de uno). Si necesitas cargar nuevos paquetes que se usará para este paquete piloto, puedes cargarlos aquí (con el [mismo proceso que al cargar paquetes de la aplicación en un envío de versión final normal](upload-app-packages.md)). Haz clic en **Guardar** cuando termines de especificar los paquetes que se incluirán en este paquete piloto.

Si la aplicación admite varias familias de dispositivos, asegúrate de que incluir paquetes para admitir el mismo conjunto de familias de dispositivos en tu piloto. Las personas en los grupos piloto **solo** podrán obtener paquetes de ese piloto. No podrán acceder a los paquetes de otros pilotos ni del envío de versión final. 

Además, recuerda la descripción de la tienda y disponibilidad de familia de dispositivos se basa en el envío de versión final. Los clientes de los grupos piloto solo podrán descargar la aplicación en una familia de dispositivos que sea compatible con el envío de versión final. Para obtener más información, consulta [Compatibilidad con familias de dispositivos](#device-family-support). 


## <a name="gradual-package-rollout"></a>Lanzamiento gradual del paquete

De manera predeterminada, los paquetes de tu envío estarán disponibles para todos los integrantes del grupo de piloto al mismo tiempo. Para cambiar esto, puedes activar la casilla que indica **Los lanzamientos se actualizan gradualmente una vez publicado el envío (solo a clientes de Windows 10). **. Puedes elegir un porcentaje de personas del grupo de piloto para que reciban los paquetes del nuevo envío, de modo que puedas supervisar los comentarios y los datos analíticos para asegurarte de estar convencido sobre la actualización antes de lanzarla más ampliamente al resto del grupo de piloto. Puedes aumentar el porcentaje (o detener la actualización) en cualquier tiempo sin necesidad de crear un nuevo envío de tu paquete piloto. 

> [!IMPORTANT]
> Cuando lanzar gradualmente los paquetes en un paquete piloto, las personas que no se incluyen en el porcentaje que recibe los nuevos paquetes recibirán los paquetes de envío del paquete piloto anterior (a menos que haya un paquete con mayor clasificación disponible para ellos).

Para obtener más información, consulta [Gradual package rollout (Lanzamiento de paquete gradual)](gradual-package-rollout.md).


## <a name="configure-additional-package-flight-options"></a>Configurar las opciones de paquete piloto adicionales

De manera predeterminada, tu paquete piloto se publicará y estará disponible para tu grupo de piloto tan pronto como se complete el proceso de certificación. Si quieres cambiar la [fecha de publicación](set-app-pricing-and-availability.md#publish-date) o agregar [Notas para la certificación](notes-for-certification.md), puedes hacerlo en la sección **Opciones de piloto**. Haz clic en **Guardar** para volver a la página de información general del paquete piloto. 


## <a name="submit-your-package-flight-to-the-store"></a>Enviar el paquete piloto a la Tienda

Cuando hayas especificado los paquetes y hayas configurado las opciones necesarias, haz clic en **Enviar a la Tienda**. A continuación, el paquete piloto pasará por el [proceso de certificación de la aplicación](the-app-certification-process.md). Ten en cuenta que los paquetes incluidos en el paquete piloto deben cumplir con las [Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies), al igual que con todos los envíos.

Las personas de los grupos piloto asociados con ese paquete piloto que ya tengan tu aplicación, obtendrán ahora una actualización con los paquetes que has incluido en el paquete piloto. Si esas personas todavía no tienen la aplicación, obtendrán los paquetes del paquete piloto cuando la instalen. 

> [!NOTE]
> Las personas que tengan un paquete que solo esté disponible en un paquete piloto pueden asignar a la aplicación una clasificación de estrellas y dejar opiniones, aunque estas no se mostrarán a otros clientes. (Esto no incluye los paquetes 7.x o 8.0 XAP heredados; otros clientes podrán ver las calificaciones y opiniones que hayan escrito los miembros de tus grupos piloto mediante esos paquetes). Puedes ver los comentarios y las clasificaciones de todos los clientes, incluyendo aquellos que pertenecen a los grupos piloto, en los informes que encontrarás en la sección **Opiniones** y **Comentarios** de la aplicación.


## <a name="device-family-support"></a>Compatibilidad con familias de dispositivos

En la mayoría de los casos, querrás incluir paquetes que admitan el mismo conjunto de familias de dispositivos compatibles con tu envío de versión final. La disponibilidad de familias de dispositivos para una aplicación siempre se basará en el envío de versión final, así un cliente esté en un grupo piloto o no.

**Si el envío de versión final admite una familia de dispositivos que no es compatible con tu paquete piloto**, las personas del grupo de piloto no podrán descargar la aplicación en esa familia de dispositivos. Por ejemplo, si tu envío de versión final incluye paquetes Mobile y Desktop y luego creas un paquete piloto que solo incluye un paquete Mobile, las personas del grupo piloto solo podrán descargar la aplicación en dispositivos móviles, aunque tengas un paquete de escritorio disponible para los clientes que no están en el piloto. Incluso si solo usas el paquete piloto para probar los cambios en el paquete Mobile, debes incluir el paquete Desktop de tu envío de versión final en el paquete piloto para que los clientes en el grupo piloto puedan descargar la aplicación en dispositivos de escritorio.

**Si el paquete piloto es compatible con una familia de dispositivos que el envío de versión final no admite**, nadie podrá descargar la aplicación en esa familia de dispositivos, así estén o no en el grupo de piloto. Por ejemplo, si tu envío de versión final solo incluye un paquete Mobile y luego creas un paquete piloto que incluye paquetes Mobile y Desktop, las personas del grupo piloto aún solo podrán descargar la aplicación en dispositivos móviles. El paquete de escritorio no puede ofrecerse a nadie, incluso a personas del grupo piloto. Si quieres que un paquete de escritorio esté a disposición de las personas del grupo piloto, primero debes actualizar el envío de versión final para que incluya un paquete de escritorio. Para una mejor experiencia para todos los clientes de la aplicación, tu envío de versión final debe admitir las mismas familias de dispositivos que el paquete piloto. 

> [!NOTE]
> Los paquetes agregados a los paquetes piloto pueden admitir cualquier versión de sistema operativo (o cualquier compilación de Windows 10), pero, como se indicó anteriormente, las personas incluidas en los grupos de piloto deben usar un dispositivo con una versión de Windows 10 que admita paquetes piloto (compilación 10586 o posterior de Windows.Desktop; compilación 10586.63 o posterior de Windows.Mobile) con el fin de obtener paquetes del paquete piloto.


## <a name="update-or-modify-your-package-flight"></a>Actualizar o modificar el paquete piloto

Para crear un nuevo envío de un paquete piloto que ya has publicado, haz clic en **Update** junto al nombre del piloto en la página de información general de la aplicación. Puedes cargar nuevos paquetes (y quitar los que no necesites), al igual que con un envío de versión final. Realiza los cambios necesarios y, a continuación, haz clic en **Enviar a la Tienda** para enviar el paquete piloto actualizado a través del [proceso de certificación de la aplicación](the-app-certification-process.md).

Para modificar un paquete piloto sin crear ni enviar una actualización nueva, haz clic en **Modificar** junto al nombre del paquete piloto. Esto te permite cambiar detalles como los grupos piloto, el nombre y la clasificación, sin necesidad de que paquete piloto vuelva a pasar por el proceso de certificación. Ten en cuenta que si tienes una actualización en curso, o si el paquete piloto no se ha publicado aún, no verás la opción **Modificar** . 


## <a name="add-and-rank-additional-package-flights"></a>Agregar y clasificar paquetes piloto adicionales

Puedes crear varios paquetes piloto de la misma aplicación, para distribuir varios paquetes distintos a conjuntos diferentes de clientes. 

Después de crear tu primer paquete piloto, puedes crear otro siguiendo el proceso descrito anteriormente. La única diferencia es que si ya habías creado un paquete piloto, deberás especificar el orden de prioridad de todos los paquetes piloto en la sección **Clasificación**. Esto permite a la tienda determinar qué paquete proporcionar a un cliente individual, si están en más de uno de tus grupos piloto. Las personas que estén en tus grupos piloto siempre obtendrán el paquete piloto disponible que tenga la clasificación más alta, aunque un paquete piloto de clasificación inferior contenga paquetes con un número de versión mayor.

De manera predeterminada, el nuevo paquete piloto tendrá la clasificación más alta. Si quieres cambiar su clasificación, puedes bajarla (o crear una copia de seguridad) para colocarla en la ubicación correcta entre los demás paquetes piloto.

Ten en cuenta que el envío de versión final siempre están clasificado el menor (1). Es decir, aquellas personas que no estén en ninguno de tus grupos piloto solo podrán obtener paquetes del envío de versión final a través de la Tienda. Las personas de un grupo piloto siempre obtendrán paquetes del paquete de clasificación más alta piloto disponible para ellos (pero nunca el envío de versión final, ya que no tiene la clasificación más baja). Esto proporciona flexibilidad para determinar cómo distribuir los paquetes a las personas que puedan ser miembros de más de uno de tus grupos piloto.

Por ejemplo, supongamos que quieres crear dos paquetes piloto además de tu envío de versión final normal: uno que sea relativamente estable y esté listo para probar con un público amplio y otro del que no estés seguro y que prefieras limitar a unos pocos evaluadores. Podrías crear un grupo piloto denominado Evaluadores e incluirlo en un paquete piloto denominado Piloto para evaluadores. Luego, podrías crear un grupo piloto denominado Entusiastas con más miembros e incluirlo en otro paquete piloto denominado Piloto para entusiastas. Si asignas al paquete Piloto para evaluadores un clasificación más alta que al paquete Piloto para entusiastas, podrás usar los paquetes que te inspiren confianza en el paquete Piloto para entusiastas y dejar los paquetes más arriesgados para los evaluadores del paquete Piloto para evaluadores. Los miembros del grupo Evaluadores siempre obtendrán los paquetes que proporciones en el paquete Piloto para evaluadores, aunque pertenezcan también al grupo de entusiastas. (Más adelante, si parece que los paquetes del paquete Piloto para evaluadores funcionan bien, puedes actualizar el paquete Piloto para entusiastas y usar los paquetes que originalmente se distribuían al paquete Piloto para evaluadores. Eventualmente, puedes usar estos paquetes en tu envío de versión final).


## <a name="make-packages-from-a-package-flight-available-to-all-your-customers"></a>Hacer que los paquetes de un paquete piloto estén disponibles para todos los clientes

Si decides que uno o varios de los paquetes incluidos en un paquete piloto publicado deben estar disponibles para los clientes que no están en un grupo piloto, puedes actualizar el envío de versión final para usar esos paquetes, sin necesidad de volver a cargar los mismos paquetes completamente. 

Al crear el envío, en la página [**Paquetes**](upload-app-packages.md) verás una lista desplegable con la opción para copiar los paquetes de uno de tus paquetes piloto. Selecciona el paquete piloto que tiene los paquetes que quieres extraer. A continuación, podrás seleccionar varios o todos los paquetes, para incluirlos en el envío de versión final.

Ten en cuenta que se aplicarán todas las mismas reglas de validación del paquete, aunque se usen paquetes de un envío publicado anteriormente. 


## <a name="delete-a-package-flight"></a>Eliminar un paquete piloto

Para eliminar un paquete piloto que ya no quieras admitir, haz clic en su nombre desde la página Información general de la aplicación. En la página de información general del piloto, haz clic en **Modificar** y luego haz clic en el vínculo **Eliminar** para eliminar el paquete piloto. (Si tienes en curso un envío sin publicar del paquete piloto, primero tendrás que eliminar ese envío). Para que se complete esta operación, pueden pasar hasta 30 minutos.

Cuando eliminas un paquete piloto, los clientes que tenían paquetes que distribuiste en ese paquete piloto obtendrán una actualización de la aplicación si existe una versión más reciente del paquete (o tan pronto como esté disponible un paquete de este tipo). Si desinstalan la aplicación y vuelven a instalarla más tarde, esta se tratará como una nueva adquisición y obtendrán la versión más reciente que esté disponible. 
