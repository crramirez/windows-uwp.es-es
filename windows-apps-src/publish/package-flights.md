---
description: Puedes usar paquetes piloto para distribuir paquetes que solo se entregan a un grupo de prueba limitado.
title: Paquetes piloto
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
ms.date: 03/07/2019
ms.topic: article
keywords: Windows 10, UWP, vuelos
ms.localizationpriority: medium
ms.openlocfilehash: e86fe69a0fa95ade82c5ed55acff907667b08f0f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029979"
---
# <a name="package-flights"></a>Paquetes piloto

Puede usar paquetes piloto para distribuir paquetes específicos a un grupo limitado de evaluadores. Los paquetes que ya ha publicado en la tienda se usarán para los demás clientes, por lo que su experiencia no se verá afectada.

Con paquetes piloto, solo los paquetes son diferentes; los detalles de la lista de tiendas serán los mismos para todos los clientes. Cualquier persona del grupo de vuelos recibirá los paquetes que se incluyen en el paquete piloto, mientras que los clientes que no están en el grupo de vuelos seguirán recibiendo los paquetes normales (sin vuelo).  Si más adelante decide que desea que los paquetes de un paquete piloto estén disponibles para todos los clientes, puede usarlos fácilmente en un envío sin vuelo. Tenga en cuenta que el paquete piloto debe pasar el [proceso de certificación](the-app-certification-process.md), exactamente igual que cualquier envío.

Al configurar los vuelos de paquete, puede especificar las personas que deben obtener paquetes específicos agregándolos a un **grupo de usuarios conocidos** (a veces denominado grupo de vuelos). Cualquier persona de un grupo piloto que use un dispositivo con una versión de Windows 10 compatible con paquetes piloto (la compilación 10586 o posterior de Windows.Desktop, la compilación 10586.63 o posterior de Windows.Mobile, o Xbox One) obtendrá paquetes de los paquetes piloto que designes para ese grupo en concreto. (Los vuelos del paquete pueden incluir paquetes destinados a cualquier versión del sistema operativo, incluidos Windows 8.1/Windows Phone 8,1 o anterior si la aplicación publicada anteriormente ya los admite). Cualquier persona que no se haya agregado a uno de los grupos de vuelos o que use un dispositivo que no sea compatible con paquetes piloto recibirá paquetes del envío sin vuelo.

> [!IMPORTANT] 
> En los dispositivos móviles y de escritorio, los usuarios de los grupos de vuelos obtendrán los paquetes en el vuelo automáticamente cada vez que se proporcionen actualizaciones. Sin embargo, los **usuarios de los grupos de vuelos que usen dispositivos Xbox deberán buscar actualizaciones manualmente** para obtener los paquetes más recientes, asegurándose de que estén iniciadas en su dispositivo con su cuenta de Microsoft (con la dirección de correo electrónico asociada que incluyó en el grupo de usuarios conocidos).

Tenga en cuenta que el paquete piloto no se distribuirá a través [de Microsoft Store para empresas](https://businessstore.microsoft.com/store) y [Microsoft Store para educación](https://educationstore.microsoft.com/store). Esto se debe a que los usuarios de los grupos de usuarios conocidos deben iniciar sesión con sus cuentas de Microsoft para recibir un paquete piloto. Todas las adquisiciones realizadas a través de Microsoft Store para empresas o Microsoft Store para educación recibirán los paquetes no vuelos.

> [!TIP]
> Los vuelos de paquetes ofrecen paquetes solo a los clientes seleccionados que especifique. Para distribuir paquetes a una selección aleatoria de los clientes en un porcentaje especificado, puedes usar el [lanzamiento de paquete gradual](gradual-package-rollout.md). También puedes combinar el lanzamiento con tus paquetes piloto si quieres distribuir una actualización de forma gradual a uno de tus grupos de piloto.
>
> A diferencia de los vuelos de paquetes, las selecciones de lanzamiento de paquetes graduales se aplican a los clientes que adquieren su aplicación a través de Microsoft Store para la empresa y Microsoft Store para educación. 

> [!TIP]
> Tenga en cuenta cómo las personas del paquete piloto podrán proporcionar su entrada sobre la aplicación. Te sugerimos que [agregues un control en la aplicación para iniciar el Centro de opiniones](../monetize/launch-feedback-hub-from-your-app.md), para que así tus clientes puedan enviarte sus comentarios directamente; además, podrás leer sus comentarios en el [informe de comentarios](feedback-report.md) de la aplicación.


## <a name="create-a-new-package-flight"></a>Crear un nuevo paquete piloto

Después de publicar en envío para tu aplicación, verás una sección **Paquetes piloto** en la página Información general de la aplicación. Haz clic en **Nuevo paquete piloto** para empezar.

Si aún no ha creado ningún grupo de usuarios conocidos, se le pedirá que cree uno antes de continuar. Para obtener más información, consulte [crear grupos de usuarios conocidos](create-known-user-groups.md). Puede crear un nuevo grupo de usuarios conocido directamente desde esta página. para ello, seleccione **crear un grupo de vuelos** .

En la página creación de paquetes piloto, deberá escribir un nombre para el vuelo y especificar al menos un grupo de vuelos. Una vez hecho esto, seleccione **crear vuelo** . No podrá cambiar estos detalles más adelante (aunque si no está satisfecho con lo que ha escrito, puede eliminar este vuelo y crear uno nuevo para usar en su lugar).

> [!NOTE]
> Si tiene más de un paquete piloto, deberá asignar un rango a cada uno de ellos. Para obtener más información, vea [Agregar y clasificar los vuelos de paquete adicionales](#add-and-rank-additional-package-flights) a continuación.


## <a name="specify-packages-to-include-in-your-package-flight"></a>Especifica los paquetes que se incluirán en tu paquete piloto

Después de guardar los detalles del paquete piloto, verás la página de información general. Haz clic en **Paquetes** para especificar los paquetes que te gustaría incluir en el paquete piloto. Puede incluir paquetes que tengan como destino cualquier versión del sistema operativo que admita la aplicación.

Tienes la opción de seleccionar paquetes que estaban asociados con un envío publicado anterior (un envío de versión final o uno de tus otros paquetes piloto, si tienes más de uno). Si necesita cargar paquetes nuevos para usarlos con este paquete piloto, puede cargarlos aquí (con el [mismo proceso que cuando carga paquetes de aplicaciones en un envío normal sin vuelo](upload-app-packages.md)). Haz clic en **Guardar** cuando termines de especificar los paquetes que se incluirán en este paquete piloto.

Si la aplicación admite varias familias de dispositivos, asegúrate de que incluir paquetes para admitir el mismo conjunto de familias de dispositivos en tu piloto. Las personas en los grupos piloto **solo** podrán obtener paquetes de ese piloto. No podrán acceder a los paquetes de otros pilotos ni del envío de versión final. 

Recuerde también que la disponibilidad de la familia de dispositivos y la información de la tienda se basa en el envío sin vuelo. Los clientes de los grupos piloto solo podrán descargar la aplicación en una familia de dispositivos que sea compatible con el envío de versión final. Para obtener más información, consulta [Compatibilidad con familias de dispositivos](#device-family-support). 


## <a name="gradual-package-rollout"></a>Lanzamiento gradual del paquete

De manera predeterminada, los paquetes de tu envío estarán disponibles para todos los integrantes del grupo de piloto al mismo tiempo. Para cambiar esto, puedes activar la casilla que indica **Los lanzamientos se actualizan gradualmente una vez publicado el envío (solo a clientes de Windows 10).** . Puedes elegir un porcentaje de personas del grupo de piloto para que reciban los paquetes del nuevo envío, de modo que puedas supervisar los comentarios y los datos analíticos para asegurarte de estar convencido sobre la actualización antes de lanzarla más ampliamente al resto del grupo de piloto. Puede aumentar el porcentaje (o detener la actualización) en cualquier tiempo sin necesidad de crear un nuevo envío de tu paquete piloto. 

> [!IMPORTANT]
> Cuando se implementan gradualmente paquetes en un paquete piloto, las personas que no están incluidas en el porcentaje que obtiene los paquetes nuevos obtendrán los paquetes del envío de paquetes piloto anterior (a menos que haya disponible un vuelo de mayor clasificación).

Para obtener más información, consulta [Gradual package rollout (Lanzamiento gradual del paquete)](gradual-package-rollout.md).


## <a name="configure-additional-package-flight-options"></a>Configurar las opciones de paquete piloto adicionales

De manera predeterminada, tu paquete piloto se publicará y estará disponible para tu grupo de piloto tan pronto como se complete el proceso de certificación. Si desea cambiar la [fecha de publicación](set-app-pricing-and-availability.md#publish-date), puede hacerlo en la sección **Opciones de vuelo** . Haz clic en **Guardar** para volver a la página de información general del paquete piloto. 


## <a name="submit-your-package-flight-to-the-store"></a>Enviar el paquete piloto a la Tienda

Cuando hayas especificado los paquetes y hayas configurado las opciones necesarias, haz clic en **Enviar a la Tienda** . A continuación, el paquete piloto pasará por el [proceso de certificación de la aplicación](the-app-certification-process.md). Tenga en cuenta que los paquetes incluidos en el paquete piloto deben cumplir las [directivas de Microsoft Store](store-policies.md), al igual que con todos los envíos.

Las personas de los grupos piloto asociados con ese paquete piloto que ya tengan tu aplicación, obtendrán ahora una actualización con los paquetes que has incluido en el paquete piloto. Si esas personas todavía no tienen la aplicación, obtendrán los paquetes del paquete piloto cuando la instalen. 

> [!NOTE]
> Las personas que tienen un paquete que solo está disponible en un paquete piloto pueden dar a la aplicación una clasificación por estrellas y dejar críticas, pero sus clasificaciones y revisiones no se mostrarán a otros clientes. (Esto excluye los paquetes antiguos de los 7. x o 8,0 XAP; las clasificaciones y las revisiones que dejan los miembros de los grupos de vuelos con esos paquetes serán visibles para otros clientes). Puede ver las clasificaciones y los comentarios de todos los clientes, incluidos los de los grupos de vuelos, en los informes de **opiniones** y **comentarios** de la aplicación.


## <a name="device-family-support"></a>Compatibilidad con familias de dispositivos

En la mayoría de los casos, querrás incluir paquetes que admitan el mismo conjunto de familias de dispositivos compatibles con tu envío de versión final. La disponibilidad de familias de dispositivos para una aplicación siempre se basará en el envío de versión final, así un cliente esté en un grupo piloto o no.

**Si el envío de versión final admite una familia de dispositivos que no es compatible con tu paquete piloto** , las personas del grupo de piloto no podrán descargar la aplicación en esa familia de dispositivos. Por ejemplo, si tu envío de versión final incluye paquetes Mobile y Desktop y luego creas un paquete piloto que solo incluye un paquete Mobile, las personas del grupo piloto solo podrán descargar la aplicación en dispositivos móviles, aunque tengas un paquete de escritorio disponible para los clientes que no están en el piloto. Incluso si solo usas el paquete piloto para probar los cambios en el paquete Mobile, debes incluir el paquete Desktop de tu envío de versión final en el paquete piloto para que los clientes en el grupo piloto puedan descargar la aplicación en dispositivos de escritorio.

**Si el paquete piloto es compatible con una familia de dispositivos que el envío de versión final no admite** , nadie podrá descargar la aplicación en esa familia de dispositivos, así estén o no en el grupo de piloto. Por ejemplo, si tu envío de versión final solo incluye un paquete Mobile y luego creas un paquete piloto que incluye paquetes Mobile y Desktop, las personas del grupo piloto aún solo podrán descargar la aplicación en dispositivos móviles. El paquete de escritorio no puede ofrecerse a nadie, incluso a personas del grupo piloto. Si quieres que un paquete de escritorio esté a disposición de las personas del grupo piloto, primero debes actualizar el envío de versión final para que incluya un paquete de escritorio. Para una mejor experiencia para todos los clientes de la aplicación, tu envío de versión final debe admitir las mismas familias de dispositivos que el paquete piloto. 

> [!NOTE]
> Los paquetes agregados a su paquete piloto pueden admitir cualquier versión del sistema operativo (o cualquier compilación de Windows 10), pero como se indicó anteriormente, los usuarios de los grupos de vuelos deben usar un dispositivo que ejecute una versión de Windows 10 que admita paquetes piloto (Windows. Desktop Build 10586 o posterior; Windows. Mobile Build 10586,63 o posterior) para obtener paquetes del paquete piloto.


## <a name="update-or-modify-your-package-flight"></a>Actualizar o modificar el paquete piloto

Para crear un nuevo envío de un paquete piloto que ya ha publicado, haga clic en **Actualizar** junto al nombre del vuelo en la página de información general de la aplicación. Puedes cargar nuevos paquetes (y quitar los que no necesites), al igual que con un envío de versión final. Realiza los cambios necesarios y, a continuación, haz clic en **Enviar a la Tienda** para enviar el paquete piloto actualizado a través del [proceso de certificación de la aplicación](the-app-certification-process.md).

Para modificar un paquete piloto sin crear ni enviar una actualización nueva, haz clic en **Modificar** junto al nombre del paquete piloto. Esto te permite cambiar detalles como los grupos piloto, el nombre y la clasificación, sin necesidad de que paquete piloto vuelva a pasar por el proceso de certificación. Tenga en cuenta que si tiene una actualización en curso o si aún no se ha publicado el paquete piloto, no verá la opción **modificar** . 


## <a name="add-and-rank-additional-package-flights"></a>Agregar y clasificar paquetes piloto adicionales

Puedes crear varios paquetes piloto de la misma aplicación, para distribuir varios paquetes distintos a conjuntos diferentes de clientes. 

Después de crear tu primer paquete piloto, puedes crear otro siguiendo el proceso descrito anteriormente. La única diferencia es que si ya habías creado un paquete piloto, deberás especificar el orden de prioridad de todos los paquetes piloto en la sección **Clasificación** . Esto permite que el almacén determine qué paquete debe proporcionar a cualquier cliente individual si se encuentran en más de uno de los grupos de vuelos. Las personas que estén en tus grupos piloto siempre obtendrán el paquete piloto disponible que tenga la clasificación más alta, aunque un paquete piloto de clasificación inferior contenga paquetes con un número de versión mayor.

De manera predeterminada, el nuevo paquete piloto tendrá la clasificación más alta. Si quieres cambiar su clasificación, puedes bajarla (o crear una copia de seguridad) para colocarla en la ubicación correcta entre los demás paquetes piloto.

Tenga en cuenta que el envío sin vuelo siempre tiene una clasificación mínima (#1). Es decir, aquellas personas que no estén en ninguno de tus grupos piloto solo podrán obtener paquetes del envío de versión final a través de la Tienda. Las personas de un grupo de vuelos siempre recibirán los paquetes del paquete piloto de la categoría más alta que estén a su disposición (pero nunca el envío sin vuelo, ya que tiene el rango más bajo). Esto proporciona flexibilidad para determinar cómo distribuir los paquetes a las personas que puedan ser miembros de más de uno de tus grupos piloto.

Por ejemplo, supongamos que quieres crear dos paquetes piloto además de tu envío de versión final normal: uno que sea relativamente estable y esté listo para probar con un público amplio y otro del que no estés seguro y que prefieras limitar a unos pocos evaluadores. Podrías crear un grupo piloto denominado Evaluadores e incluirlo en un paquete piloto denominado Piloto para evaluadores. Luego, podrías crear un grupo piloto denominado Entusiastas con más miembros e incluirlo en otro paquete piloto denominado Piloto para entusiastas. Si asignas al paquete Piloto para evaluadores un clasificación más alta que al paquete Piloto para entusiastas, podrás usar los paquetes que te inspiren confianza en el paquete Piloto para entusiastas y dejar los paquetes más arriesgados para los evaluadores del paquete Piloto para evaluadores. Los miembros del grupo Evaluadores siempre obtendrán los paquetes que proporciones en el paquete Piloto para evaluadores, aunque pertenezcan también al grupo de entusiastas. (Más adelante, si parece que los paquetes del paquete Piloto para evaluadores funcionan bien, puedes actualizar el paquete Piloto para entusiastas y usar los paquetes que originalmente se distribuían al paquete Piloto para evaluadores. Eventualmente, puedes usar estos paquetes en tu envío de versión final).


## <a name="make-packages-from-a-package-flight-available-to-all-your-customers"></a>Hacer que los paquetes de un paquete piloto estén disponibles para todos los clientes

Si decides que uno o varios de los paquetes incluidos en un paquete piloto publicado deben estar disponibles para los clientes que no están en un grupo piloto, puedes actualizar el envío de versión final para usar esos paquetes, sin necesidad de volver a cargar los mismos paquetes completamente. 

Al crear el envío, en la página [**Paquetes**](upload-app-packages.md) verás una lista desplegable con la opción para copiar los paquetes de uno de tus paquetes piloto. Selecciona el paquete piloto que tiene los paquetes que quieres extraer. A continuación, podrás seleccionar varios o todos los paquetes, para incluirlos en el envío de versión final.

Ten en cuenta que se aplicarán todas las mismas reglas de validación del paquete, aunque se usen paquetes de un envío publicado anteriormente. 


## <a name="delete-a-package-flight"></a>Eliminar un paquete piloto

Para eliminar un paquete piloto que ya no quieras admitir, haz clic en su nombre desde la página Información general de la aplicación. En la página de información general del piloto, haz clic en **Modificar** y luego haz clic en el vínculo **Eliminar** para eliminar el paquete piloto. (Si tienes en curso un envío sin publicar del paquete piloto, primero tendrás que eliminar ese envío). Para que se complete esta operación, pueden pasar hasta 30 minutos.

Cuando eliminas un paquete piloto, los clientes que tenían paquetes que distribuiste en ese paquete piloto obtendrán una actualización de la aplicación si existe una versión más reciente del paquete (o tan pronto como esté disponible un paquete de este tipo). Si desinstalan la aplicación y vuelven a instalarla más tarde, esta se tratará como una nueva adquisición y obtendrán la versión más reciente que esté disponible. 
