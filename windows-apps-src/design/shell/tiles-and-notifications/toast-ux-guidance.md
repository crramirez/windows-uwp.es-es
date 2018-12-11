---
Description: Learn how to create effective and user-focused notifications that make your users productive and happy.
title: Instrucciones de experiencia de usuario de notificación del sistema
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, uwp, notificación, colecciones, grupo, experiencia del usuario, instrucciones de experiencia del usuario, instrucciones, acción, notificación del sistema, el centro de actividades, noninterruptive, notificaciones efectiva, las notificaciones no intrusivos, accionables, administrar, organizar
ms.localizationpriority: medium
ms.openlocfilehash: 878df85db9ab0e33db06a86ddb726f07dc28f013
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8918854"
---
# <a name="toast-notification-ux-guidance"></a>Instrucciones de experiencia de usuario de notificación del sistema
Las notificaciones son una parte necesaria de la vida moderna; que ayudan a los usuarios a ser más productivos y establecido con las aplicaciones y sitios Web, así como mantenerse actual con las actualizaciones. Sin embargo, las notificaciones pueden activar rápidamente desde útil overbearing e intrusivo si no se han diseñado de forma centrado en el usuario. Las notificaciones son una contextual desde que se ha desactivado y es poco probable que una vez que están desactivados, se activará nuevamente.  Por lo tanto, asegúrate de que las notificaciones son respetuosas de espacio de pantalla del usuario y el tiempo, para poder mantener este canal de participación abierta.

> **API importantes**: [paquete de nuget de notificaciones del Kit de herramientas de comunidad de Windows](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Te hemos analizado nuestra telemetría de Windows, así como otros primero y casos prácticos de terceros, para elaborar cuatro reglas en torno a lo que hace una historia de notificación excelente.  Estamos seguros de estas reglas son universalmente aplicables, independientemente de la plataforma y ayudan a las notificaciones a tener un impacto positivo en los usuarios.

## <a name="1-actionable-notifications"></a>1. accionables notificaciones
Notificaciones accionables permiten a los usuarios a ser productivos sin tener que abrir la aplicación.  Si bien es excelente para que la aplicación se inicia, no es la única medición de éxito y realizar pequeño permitiendo a los usuarios realizar tareas sin tener que ir a la aplicación pueden ser una herramienta muy eficaz en mimar a los usuarios.

![Notificación accionable con el cuadro de entrada de texto y los botones para establecer avisos y responder a la notificación](images/actionable-notification-example01.png)

Anterior es un ejemplo de una notificación que saca provecho de las acciones. La sensación de tareas de acabado es una sensación universalmente positiva y esa sensación de que puede aportar a tu aplicación o el sitio Web mediante el envío de notificaciones que tienen contenido accionable en ellos. Notificaciones accionables también pueden ayudar a aumentar la productividad, tanto en escenarios empresariales y del consumidor, reduciendo el tiempo a los usuarios de la acción pasar por para realizar estas tareas más pequeñas. Te recomendamos que incluyas cosas que estás intentando formar a los usuarios hacer o acciones que los usuarios realizar con regularidad.  Algunos ejemplos incluyen:
* Gusto, favoriting, marcar o intérprete contenido
* Aprobar o deniega informes de gastos, tiempo desactivado, permisos, etcetera.
* En línea responder a mensajes, mensajes de correo electrónico, charlas de grupo, comentarios, etcetera.
* Completar los pedidos con [actualización pendiente](toast-pending-update.md)
* Establecer alertas o avisos para otro momento, así como reserva potencialmente el tiempo en un calendario

Las notificaciones de accionables son una herramienta muy eficaz para ayudar a los usuarios a ser productivos, realizar las tareas y tener una experiencia excelente y eficaz con la aplicación o el sitio Web.  ¡Hay una gran cantidad de oportunidades aquí! Si desea obtener ayuda ideas de lluvia de ideas, no dudes en contacto con el equipo de notificaciones de windows.

## <a name="2-timing-and-urgency"></a>2. temporización y urgencia
¡Al contrario de lo que a menudo pensamos sobre las notificaciones, en tiempo real no es necesariamente mejor! Recomendamos a los desarrolladores de reflexión sobre el usuario y si la notificación que envían es urgente información o no. Los usuarios pueden sobrecargar fácilmente con demasiada información y frustrarte si se está interrumpen mientras está intentando de foco. Windows proporciona varias opciones para cómo a tener en cuenta la tendencia a la intrusión de las notificaciones que se va a enviar:

**Notificaciones sin procesar:** Usar [las notificaciones sin procesar](raw-notification-overview.md) puede ser útil por muchas razones, especialmente cuando se trate o minimizar las interrupciones para el usuario.  Enviar notificaciones sin procesar se reactive la aplicación en segundo plano, por lo que puedes evaluar si la notificación tenga sentido para entregar inmediatamente en el contexto de la aplicación. Si es algo que crees que se deben mostrar al usuario al instante, es posible que aparezca una [notificación del sistema local](send-local-toast.md) desde allí.  Si es algo que el usuario no tiene que ver ahora, puedes crear una [notificación del sistema programada](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) que se activará en un momento posterior.


**Ghost notificación del sistema:** también puede generar una notificación que omitiremos aparecerán en la esquina inferior derecha de la pantalla y, en su lugar enviar la notificación directamente en el centro de actividades. Esto se logra estableciendo la [propiedad SupressPopup](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) en True. Aunque es posible que haya algunos escepticismo alrededor no aparición notificaciones fuera del centro de actividades, vemos un 2-3 veces mayor participación para las notificaciones del sistema que se encuentran en el centro de actividades a través de extrae del sistema.  Los usuarios responden mejor cuando están listos para recibir notificaciones y controlar cuándo se interrumpen, motivo por el contenido en el centro de actividades puede ser mucho más eficaz de noninvasively notificar a los usuarios.

## <a name="3-clear-out-the-clutter"></a>3. borrar el desorden
Las notificaciones pueden persistir en el centro de actividades durante un tiempo bastante (el valor predeterminado es tres días).  Es fundamental que asegurarse de que el contenido que se encuentra aquí es relevante y actualizado cada vez que el usuario abre el centro de actividades. Son una pérdida de espacio de pantalla del usuario y ocupando ranuras que se pueden usar para algo más actualizada.  Supongamos que el usuario instala la aplicación de administración de correo electrónico y recibe diez notificaciones junto con los mensajes de correo electrónico y mensajes de correo diez electrónico.  Dependiendo de la experiencia deseada, podrías considerar la posibilidad de borrar las notificaciones si el usuario tiene leer el correo electrónico correspondiente, o abrir la aplicación como una manera de quitar el desorden antiguo desde el centro de actividades.

Se tiene una serie de [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) API que te permiten ver qué contenido se encuentra en el centro de actividades, así como administración estas notificaciones. Respetar del espacio de pantalla del usuario y se encargará de que solo se muestran contenido actual y relevante a los usuarios.

## <a name="4-keeping-organized"></a>4. manteniendo organizado
Como se mencionó anteriormente, se conserva el contenido en el centro de actividades de tres días.  Para ayudar a los usuarios elegir un vistazo a la información que buscan rápidamente, organizar las notificaciones en el centro de actividades con [encabezados](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) o [colecciones](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection). Puedes ver un ejemplo de un encabezado a continuación.

![Ejemplos de notificación del sistema con encabezados con la etiqueta 'Camping!!'](images/toast-headers-action-center.png)

Estas notificaciones en una forma de grupo para que el contenido relevante permanece entre sí (es decir, piensa en la separación de ligas deportes diferentes en una aplicación de deportes u ordenar mensajes por grupo de chat). Las colecciones son una manera más evidente a las notificaciones de grupo, mientras que los encabezados son más sutiles, pero ambas permiten a los usuarios clasificar y seleccionar las notificaciones de manera más rápida.

## <a name="other-resources"></a>Otros recursos
Estos cuatro puntos anteriores son instrucciones que hemos encontrado eficaces a través de nuestro propio análisis de telemetría y primero y experimentos de terceros. Ten en cuenta, sin embargo, que estas directrices son simplemente eso: directrices.  Estamos seguros de estas reglas ayudará a aumentar la interacción y la productividad de las notificaciones, pero nada puede sustituir thinking centrado en el usuario y el aprendizaje de sus propios datos.  

Si enviar notificaciones a tu aplicación para UWP hoy en día, puedes ver análisis en qué ha ocurrido con las notificaciones del [Centro](https://partner.microsoft.com/dashboard)de partners! Estos datos viene libres al usar el [SDK de servicios de Store](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) o las [API de WNS](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview). Estas métricas te proporcionará más información sobre lo que sucede con las notificaciones en la plataforma de windows, así como el comportamiento de los usuarios interactúan con las notificaciones. Obtener acceso a estos datos, ve al menú en el lado izquierdo interactuar > notificaciones, a continuación, al hacer clic en la pestaña "Analizar" dentro de la página de notificaciones.  Esto se encuentra en el mismo lugar que vas a enviar notificaciones del centro de partners.

## <a name="related-topics"></a>Temas relacionados

* [Contenido de notificaciones del sistema](adaptive-interactive-toasts.md)
* [Notificaciones sin procesar](raw-notification-overview.md)
* [Actualización pendiente](toast-pending-update.md)
* [Biblioteca de notificaciones en GitHub (parte del Kit de herramientas de Comunidad Windows)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
