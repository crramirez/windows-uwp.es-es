---
title: Responder a los comentarios de los clientes
description: Puedes responder directamente a los comentarios que los clientes dejan en el Centro de opiniones.
author: JnHs
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 04983b80-2a18-4ace-93d3-e8c33c04bfb9
ms.localizationpriority: medium
ms.openlocfilehash: 5da9e96bace29dc33874d5b8c3e4ac846eddeb63
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7437286"
---
# <a name="respond-to-customer-feedback"></a>Responder a los comentarios de los clientes

Puedes usar el [informe de comentarios](feedback-report.md) para revisar los comentarios que los clientes de Windows 10 dejaron sobre tu aplicación en el Centro de opiniones y luego responder directamente a los comentarios. Puedes publicar tus respuestas en el Centro de opiniones para que todos lo vean (como comentarios individuales o al actualizar el estado de un comentario y agregar una descripción), para indicar a los clientes sobre nuevas funciones o correcciones de errores o para pedir comentarios más específicos sobre cómo mejorar la aplicación. También puedes enviar tu respuesta por correo electrónico directamente al cliente que envió el comentario.

> [!TIP]
> Puedes animar a los clientes a que dejen comentarios con la API de comentarios en el [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) para agregar un control que permite que los clientes [inicien el Centro de opiniones desde tu aplicación para UWP](../monetize/launch-feedback-hub-from-your-app.md) directamente. Ten en cuenta que cualquier cliente que haya descargado tu aplicación en un dispositivo con Windows 10 que admita el Centro de opiniones tiene la posibilidad de dejar comentarios sobre ella directamente mediante la aplicación Centro de opiniones. Por este motivo, es posible que veas comentarios de los clientes en este informe, aunque no hayas solicitado específicamente que te envíen comentarios desde dentro de tu aplicación.

Para proporcionar una respuesta a cualquier comentario, haz clic en el vínculo **Responder a los comentarios** que aparece junto al comentario en tu **informe de comentarios**.

[El centro de partners](https://partner.microsoft.com/dashboard) admite tres opciones para responder a los clientes que proporcionan comentarios sobre la aplicación. Independientemente de la opción que elijas, ten en cuenta que hay un límite de 1000 caracteres para cada respuesta.

## <a name="public-comments-in-feedback-hub"></a>Comentarios públicos del Centro de opiniones

De manera predeterminada, el botón de radio de **Comentario** se selecciona después de hacer clic en **Responder a los comentarios**. Para publicar una respuesta pública a los comentarios del cliente, deja este botón seleccionado. Escribe tu comentario en el cuadro y, a continuación, haz clic en **Enviar**.

Se mostrará el comentario que hayas escrito como un comentario en el Centro de opiniones, junto con los comentarios enviados por otros clientes. Tu nombre de editor y nombre de la aplicación se mostrarán junto con tu comentario para identificarte como desarrollador. No hay ningún límite en el número de comentarios que puedes escribir para un comentario, pero ten en cuenta que no puedes editar ni eliminar comentarios después enviarlos. Los cinco comentarios más recientes a un comentario se mostrarán en tu **informe de comentarios** (también en el Centro de opiniones). Cuando hay más de cinco comentarios, puedes hacer clic en **Mostrar todos los comentarios** para verlos todos en el Centro de opiniones.


## <a name="private-responses-via-email"></a>Respuestas privadas por correo electrónico

Si prefieres no publicar una respuesta pública, puedes activar la casilla **Enviar comentario como correo electrónico** para enviar una respuesta privada directamente al cliente (si se ha proporcionado una dirección de correo electrónico y no ha optado por no recibir respuestas por correo electrónico). Al hacerlo, Microsoft envía un correo electrónico al cliente en tu nombre. El correo electrónico contendrá los comentarios originales, así como la respuesta que escribas.

Después de activar la casilla **Enviar comentario como correo electrónico**, escribe el comentario y, a continuación, haz clic en **Enviar**. Ten en cuenta que debes proporcionar una dirección de correo electrónico en el campo **Correo electrónico de contacto de soporte técnico** cuando uses esta opción. De manera predeterminada, usamos la dirección de correo electrónico que proporcionaste en la información de contacto de tu cuenta. Si prefieres usar una dirección de correo electrónico diferente, puedes actualizar el campo **Correo electrónico de contacto de soporte técnico** para usar uno diferente. El cliente que recibe la respuesta podrá responder directamente a esta dirección de correo electrónico.


## <a name="public-status-updates-and-descriptions-in-feedback-hub"></a>Actualizaciones de estado públicas y descripciones del Centro de opiniones

Una tercera opción para una respuesta pública es establecer el estado de un comentario para informar a los clientes que estás trabajando en el problema o que ya se ha solucionado. Cuando actualizas el estado de un comentario, se muestra junto con los comentarios del Centro de opiniones.

Para usar esta opción, selecciona el botón de radio **Actualizar estado**. A continuación, selecciona una de las siguientes opciones:

- **Investigando**: eres consciente de un problema y lo estás investigando.
- **Trabajando en ello**: estás en proceso de solución de un problema o agregando una característica solicitada.
- **Completado**: has publicado una actualización para solucionar el problema o agregar una característica solicitada.

Junto con la actualización de estado, puedes escribir un comentario para proporcionar más información, como una estimación de cuándo crees que se habrá solucionado un problema o más información sobre los últimos cambios. Esta descripción aparecerá en la parte superior de la lista de comentarios (y el informe de comentarios mostrará el estado actual y la descripción).

Con la opción **Actualizar estado** puedes cambiar el estado cuando quieras (además de proporcionar descripciones actualizadas para cada cambio de estado). Siempre que cambie el estado de un comentario, se actualizará el estado en el Centro de opiniones para que los clientes que visualicen la respuesta vean el estado más reciente.


## <a name="guidelines-for-responses"></a>Orientación para las respuestas

Independientemente del método que uses para responder a los comentarios de un cliente, debes seguir estas directrices para todas las respuestas.
- Las respuestas no pueden tener más de 1000 caracteres.
- No puedes ofrecer a los usuarios ningún tipo de compensación, incluidos productos de aplicaciones digitales, por sus comentarios públicos.
- No incluyas ningún contenido de marketing ni anuncios en la respuesta. Recuerda que la persona que deje un comentario ya es tu cliente.
- No promuevas otras aplicaciones o servicios en la respuesta.
- La respuesta debe estar directamente relacionada con la aplicación y el comentario específicos.
- No incluyas comentarios irreverentes, agresivos, personales ni maliciosos en tu respuesta. Sé siempre educado y recuerda que los clientes que estén contentos serán probablemente los mayores promotores de tu aplicación.

> [!NOTE]
> Los clientes pueden notificar a Microsoft sobre la recepción de una respuesta inapropiada de un desarrollador a un comentario. Pueden también optar por no recibir las respuestas de los comentarios por correo electrónico.

La relación con tus clientes depende de ti. Microsoft no se involucra en las discusiones entre los desarrolladores y los clientes. No obstante, si crees que el contenido de los comentarios de un cliente sobre tu producto es inadecuado, envía una [incidencia de soporte técnico](http://go.microsoft.com/fwlink/p/?LinkID=401178).
