---
author: jnHs
Description: Si la aplicación usa los elementos AdMediatorControl o AdControl para mostrar anuncios en banner, puedes aumentar la velocidad de relleno de los anuncios y tus ingresos mostrando anuncios de filiales de Microsoft en la aplicación.
title: Paquetes piloto
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
---

# Paquetes piloto

Puedes usar paquetes piloto para distribuir paquetes que solo se entregan a un grupo de prueba limitado. 

Los paquetes piloto te permiten proporcionar paquetes diferentes para un conjunto designado de evaluadores sin interrumpir la experiencia de otros clientes. Solo los paquetes son diferentes; los detalles del listado de la Tienda serán los mismos para todos los clientes.

Ten en cuenta que los paquetes piloto deben pasar el [proceso de certificación](the-app-certification-process.md), al igual que un envío de versión final normal. Si más tarde decides que quieres que los paquetes de un paquete piloto estén disponibles para todos tus clientes, puedes extraer estos paquetes en tu envío de versión final, tal como se describe a continuación.

Cuando configuras paquetes piloto, puedes elegir qué personas deben obtener paquetes específicos. Para ello, agrégalas a un grupo piloto. Cualquier persona de un grupo piloto que use un dispositivo que ejecute una versión de Windows 10 compatible con los paquetes piloto (la compilación 10586 o posterior de Windows.Desktop, o la compilación 10586.63 o posterior de Windows.Mobile), obtendrá paquetes de los paquetes piloto que designes para ese grupo en concreto. Cualquier persona que no esté agregada a uno de tus grupos piloto o que use un dispositivo incompatible con los paquetes piloto, obtendrá paquetes del envío de versión final.

Después de publicar en envío para tu aplicación, verás una sección **Paquetes piloto** en la página Información general de la aplicación. Haz clic en **Nuevo paquete piloto** para empezar. Si no has configurado ningún grupo piloto aún, se te pedirá que crees uno antes de continuar.

## Crear un nuevo grupo piloto

Los grupos piloto te permiten especificar las personas a las que te gustaría incluir en el grupo. Para obtener tus paquetes piloto, cada persona debe estar autenticada en la Tienda con una cuenta de Microsoft asociada a la dirección de correo electrónico que proporciones y debe usar un dispositivo de Windows 10 (según se especifica más arriba) para descargar la aplicación.

Al crear un grupo piloto, debes asignarle un nombre. Cada grupo piloto debe contener al menos una dirección de correo electrónico, con un máximo de 10 000 direcciones de correo electrónico. Puedes escribir las direcciones de correo electrónico directamente en el campo (separadas por espacios, comas o punto y coma), o puede hacer clic en el vínculo **Importar .csv** para crear el grupo piloto a partir de una lista de direcciones de correo electrónico en un archivo .csv.

Haz clic en **Crear grupo** para guardar el grupo y continuar configurando el paquete piloto.

> **Importante** Asegúrate de obtener el permiso necesario de las personas que agregas a tu grupo piloto, y de que comprenden que obtendrán paquetes diferentes al envío de versión final. 
> Es posible que también quieras tener en cuenta el modo en que pueden enviarte comentarios sobre la aplicación aquellas personas que incluyas en el paquete piloto. Te sugerimos que [agregues un control en la aplicación para iniciar el Centro de opiniones](../monetize/launch-feedback-hub-from-your-app.md), para que así tus clientes puedan enviarte sus comentarios directamente; además, podrás leer sus comentarios en el [informe de comentarios](feedback-report.md) de la aplicación.

Para editar el grupo piloto más adelante, puedes hacer clic en **Exportar .csv** para guardar la información de tu grupo en un archivo .csv. Realiza los cambios que quieras en este archivo y haz clic en **Importar .csv** para usar la nueva versión para actualizar la pertenencia al grupo. Ten en cuenta que los cambios de pertenencia al grupo piloto pueden tardar hasta 30 minutos en implementarse.

## Crear un nuevo paquete piloto

Después de crear tu primer grupo piloto, verás una página donde puedes agregar detalles para completar el proceso de configuración. Deberás asignar un nombre al paquete piloto y especificar al menos un grupo piloto. Si quieres configurar un nuevo grupo, puedes hacerlo desde esta página.

Haz clic en **Crear piloto** una vez que hayas escrito el nombre y seleccionado los grupos piloto. No podrás cambiar estos detalles más adelante (aunque siempre puedes eliminar y crear un nuevo paquete piloto para usarlo en lugar del primero).

> Nota: si tienes más de un paquete piloto, deberás asignar una clasificación a cada uno de ellos. Para obtener más información, consulta a continuación Agregar y clasificar paquetes piloto adicionales.

## Especifica los paquetes que se incluirán en tu paquete piloto

Después de guardar los detalles del paquete piloto, verás la página de información general. Haz clic en **Paquetes** para especificar los paquetes que te gustaría incluir en el paquete piloto.

Tienes la opción de seleccionar paquetes que estaban asociados con un envío publicado anterior (un envío de versión final, o uno de tus otros paquetes piloto si tienes más de uno). Si necesitas [cargar nuevos paquetes](upload-app-packages.md) para usarlos en este paquete piloto, puedes cargarlos aquí (puedes recurrir al mismo proceso que al cargar paquetes de aplicaciones en un envío de versión final normal). Haz clic en **Guardar** cuando termines de especificar los paquetes que se incluirán en este paquete piloto.

Si la aplicación admite varias familias de dispositivos, asegúrate de que incluir paquetes para admitir el mismo conjunto de familias de dispositivos en tu piloto. Las personas en los grupos piloto **solo** podrán obtener paquetes de ese piloto. No podrán acceder a los paquetes de otros pilotos ni del envío de versión final. 

Además, recuerda que la información de la descripción de la Tienda procede de tu envío de versión final, incluidas las familias de dispositivos son compatibles con la aplicación. Los clientes de los grupos piloto solo podrán descargar la aplicación en una familia de dispositivos que sea compatible con el envío de versión final. Para obtener más información, consulta [Compatibilidad con familias de dispositivos](#device-family-support). 

## Configurar las opciones de paquete piloto adicionales

De manera predeterminada, tu paquete piloto se publicará y estará disponible para tu grupo piloto tan pronto como se complete el proceso de certificación. Si quieres cambiar la [fecha de publicación](set-app-pricing-and-availability.md#publish-date), o agregar [Notas para la certificación](notes-for-certification.md), puedes hacerlo en la sección **Opciones**. Haz clic en **Guardar** para volver a la página de información general del paquete piloto. 

## Enviar el paquete piloto a la Tienda

Cuando hayas especificado los paquetes y hayas configurado las opciones necesarias, haz clic en **Enviar a la Tienda**. A continuación, el paquete piloto pasará por el [proceso de certificación de la aplicación](the-app-certification-process.md). Ten en cuenta que los paquetes incluidos en el paquete piloto deben cumplir con las [Directivas de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx), como sucede con todos los envíos.

Las personas de los grupos piloto asociados con ese paquete piloto que ya tengan tu aplicación, obtendrán ahora una actualización con los paquetes que has incluido en el paquete piloto. Si esas personas todavía no tienen la aplicación, obtendrán los paquetes del paquete piloto cuando la instalen. 

> Nota: aquellas personas que tengan un paquete que solo esté disponible en un paquete piloto, pueden asignar a la aplicación una clasificación de estrellas y dejar opiniones, aunque estas no se mostrarán a otros clientes. (Esto no incluye los paquetes 7.x o 8.0 XAP heredados; otros clientes podrán ver las calificaciones y opiniones que hayan escrito los miembros de tus grupos piloto mediante esos paquetes). Puedes ver los comentarios de todos los clientes, incluyendo aquellos que pertenecen a los grupos piloto, en los informes que encontrarás en la sección Calificaciones y opiniones de la aplicación.

## Compatibilidad con familias de dispositivos

En la mayoría de los casos, querrás incluir paquetes que admitan el mismo conjunto de familias de dispositivos compatibles con tu envío de versión final. La disponibilidad de familias de dispositivos para una aplicación siempre se basará en el envío de versión final, así un cliente esté en un grupo piloto o no.

**Si el envío de versión final admite una familia de dispositivos que no es compatible con tu paquete piloto**, las personas del grupo piloto no podrán descargar la aplicación en esa familia de dispositivos. Por ejemplo, si tu envío de versión final incluye paquetes Mobile y Desktop y luego creas un paquete piloto que solo incluye un paquete Mobile, las personas del grupo piloto solo podrán descargar la aplicación en dispositivos móviles, aunque tengas un paquete de escritorio disponible para los clientes que no están en el piloto. Incluso si solo usas el paquete piloto para probar los cambios en el paquete Mobile, debes incluir el paquete Desktop de tu envío de versión final en el paquete piloto para que los clientes en el grupo piloto puedan descargar la aplicación en dispositivos de escritorio.

**Si el paquete piloto es compatible con una familia de dispositivos que el envío de versión final no admite**, nadie podrá descargar la aplicación en esa familia de dispositivos, así estén en el grupo piloto o no. Por ejemplo, si tu envío de versión final solo incluye un paquete Mobile y luego creas un paquete piloto que incluye paquetes Mobile y Desktop, las personas del grupo piloto aún solo podrán descargar la aplicación en dispositivos móviles. El paquete de escritorio no puede ofrecerse a nadie, incluso a personas del grupo piloto. Si quieres que un paquete de escritorio esté a disposición de las personas del grupo piloto, primero debes actualizar el envío de versión final para que incluya un paquete de escritorio. Para una mejor experiencia para todos los clientes de la aplicación, tu envío de versión final debe admitir las mismas familias de dispositivos que el paquete piloto. 

**Nota**  Los paquetes agregados a los paquetes piloto pueden admitir cualquier versión de sistema operativo (o cualquier compilación de Windows 10), pero, como se indicó anteriormente, las personas en los grupos piloto debe usar un dispositivo con una versión de Windows 10 que admita los paquete piloto (Windows.Desktop compilación 10586 o posterior; Windows.Mobile compilación 10586.63 o posterior) con el fin de obtener paquetes del paquete piloto.

## Actualizar o modificar el paquete piloto

Para crear un envío de un paquete piloto ya existente, haz clic en **Actualizar** junto al nombre del paquete piloto, en la página Información general de la aplicación. Puedes cargar nuevos paquetes (y quitar los que no necesites), al igual que con un envío de versión final. Realiza los cambios necesarios y, a continuación, haz clic en **Enviar a la Tienda** para enviar el paquete piloto actualizado a través del [proceso de certificación de la aplicación](the-app-certification-process.md).

Para modificar un paquete piloto sin crear ni enviar una actualización nueva, haz clic en **Modificar** junto al nombre del paquete piloto. Esto te permite cambiar detalles como los grupos piloto, el nombre y la clasificación, sin necesidad de que paquete piloto vuelva a pasar por el proceso de certificación.

## Agregar y clasificar paquetes piloto adicionales

Puedes crear varios paquetes piloto de la misma aplicación, para distribuir varios paquetes distintos a conjuntos diferentes de clientes. 

Después de crear tu primer paquete piloto, puedes crear otro siguiendo el proceso descrito anteriormente. La única diferencia es que si ya habías creado un paquete piloto, deberás especificar el orden de prioridad de todos los paquetes piloto en la sección **Clasificación**. Esto permite a la Tienda determinar qué paquete se debe proporcionar a un cliente individual, aunque esté en más de uno de tus grupos piloto. Las personas que estén en tus grupos piloto siempre obtendrán el paquete piloto disponible que tenga la clasificación más alta, aunque un paquete piloto de clasificación inferior contenga paquetes con un número de versión mayor.

De manera predeterminada, el nuevo paquete piloto tendrá la clasificación más alta. Si quieres cambiar su clasificación, puedes bajarla (o crear una copia de seguridad) para colocarla en la ubicación correcta entre los demás paquetes piloto.

Ten en cuenta que el envío de versión final siempre tendrá la clasificación más baja. Es decir, aquellas personas que no estén en ninguno de tus grupos piloto solo podrán obtener paquetes del envío de versión final a través de la Tienda. Las personas de un grupo piloto siempre obtendrán paquetes del paquete piloto de clasificación más alta que tengan a su disposición (pero nunca el envío de versión final). Esto proporciona flexibilidad para determinar cómo distribuir los paquetes a las personas que puedan ser miembros de más de uno de tus grupos piloto.

Por ejemplo, supongamos que quieres crear dos paquetes piloto además de tu envío de versión final normal: uno que sea relativamente estable y esté listo para probar con un público amplio y otro del que no estés seguro y que prefieras limitar a unos pocos evaluadores. Podrías crear un grupo piloto denominado Evaluadores e incluirlo en un paquete piloto denominado Piloto para evaluadores. Luego, podrías crear un grupo piloto denominado Entusiastas con más miembros e incluirlo en otro paquete piloto denominado Piloto para entusiastas. Si asignas al paquete Piloto para evaluadores un clasificación más alta que al paquete Piloto para entusiastas, podrás usar los paquetes que te inspiren confianza en el paquete Piloto para entusiastas y dejar los paquetes más arriesgados para los evaluadores del paquete Piloto para evaluadores. Los miembros del grupo Evaluadores siempre obtendrán los paquetes que proporciones en el paquete Piloto para evaluadores, aunque pertenezcan también al grupo de entusiastas. (Más adelante, si parece que los paquetes del paquete Piloto para evaluadores funcionan bien, puedes actualizar el paquete Piloto para entusiastas y usar los paquetes que originalmente se distribuían al paquete Piloto para evaluadores. Eventualmente, puedes usar estos paquetes en tu envío de versión final).

## Hacer que los paquetes de un paquete piloto estén disponibles para todos los clientes

Si decides que uno o varios de los paquetes incluidos en un paquete piloto publicado deben estar disponibles para los clientes que no están en un grupo piloto, puedes actualizar el envío de versión final para usar esos paquetes, sin necesidad de volver a cargar los mismos paquetes completamente. 

Al crear el envío, en la página [**Paquetes**](upload-app-packages.md) verás una lista desplegable con la opción para copiar los paquetes de uno de tus paquetes piloto. Selecciona el paquete piloto que tiene los paquetes que quieres extraer. A continuación, podrás seleccionar varios o todos los paquetes, para incluirlos en el envío de versión final.

Ten en cuenta que se aplicarán todas las mismas reglas de validación del paquete, aunque se usen paquetes de un envío publicado anteriormente. 

## Eliminar un paquete piloto

Para eliminar un paquete piloto que ya no quieras admitir, haz clic en su nombre desde la página Información general de la aplicación. En la página de información general del piloto, haz clic en **Modificar** y luego haz clic en el vínculo **Eliminar** para eliminar el paquete piloto. (Si tienes en curso un envío sin publicar del paquete piloto, primero tendrás que eliminar ese envío). Para que se complete esta operación, pueden pasar hasta 30 minutos.

Cuando eliminas un paquete piloto, los clientes que tenían paquetes que distribuiste en ese paquete piloto obtendrán una actualización de la aplicación si existe una versión más reciente del paquete (o tan pronto como esté disponible un paquete de este tipo). Si desinstalan la aplicación y vuelven a instalarla más tarde, esta se tratará como una nueva adquisición y obtendrán la versión más reciente que esté disponible. 


<!--HONumber=May16_HO2-->


