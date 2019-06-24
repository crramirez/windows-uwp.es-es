---
Description: Obtenga información sobre cómo crear notificaciones efectiva y centrado en los usuarios que hacen que los usuarios productivos y felices.
title: Guía de experiencia de usuario del sistema
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, uwp, notificación, colección, grupo, experiencia de usuario, directrices de ux, orientación, acción, notificaciones del sistema, centro de actividades, noninterruptive, notificaciones efectiva, las notificaciones no intrusivos, procesables, administrar, organizar
ms.localizationpriority: medium
ms.openlocfilehash: 327a2add84343be3b972f7bb1f232298e7ef92ad
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320729"
---
# <a name="toast-notification-ux-guidance"></a>Guía de UX de notificación del sistema
Las notificaciones son una parte necesaria de su ciclo de vida moderno ayudan a los usuarios ser más productivos y comprometida con aplicaciones y sitios Web, así como Manténgase al día con las actualizaciones. Sin embargo, las notificaciones rápidamente pueden activar desde útil overbearing y molestos si no están diseñadas de manera orientados al usuario. Las notificaciones son una con el botón secundario fuera de la que se ha desactivado y es poco probable que una vez que están desactivadas, se activará a intentarlo.  Por lo tanto, asegúrese de que las notificaciones son respetuosas del conjunto de espacio en la pantalla del usuario y la hora, por lo que puede mantener abierto este canal de engagement.

> **API importantes**: [Paquete de nuget de notificaciones de Kit de herramientas de comunidad de Windows](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Nos hemos analizado nuestra telemetría de Windows, así como otros primero y casos prácticos de terceros para proponer cuatro reglas en torno a lo que hace que un caso de notificación excelente.  Estamos seguros de estas reglas son aplicables a todo el mundo, independientemente de la plataforma y le ayudará a las notificaciones tienen un impacto positivo en los usuarios.

## <a name="1-actionable-notifications"></a>1. Notificaciones que requieren acción
Las notificaciones que requieren acción permiten a los usuarios ser productivos sin tener que abrir la aplicación.  Aunque es excelente para que la aplicación se inicia, esto no es la única medida de éxito y permitiendo a los usuarios tomar realizar pequeñas tareas sin tener que ir a la aplicación pueden ser una herramienta muy eficaz en deleitar a los usuarios.

![Notificación que requieren acción con el cuadro de texto de entrada y los botones para establecer avisos y responder a la notificación](images/actionable-notification-example01.png)

Anteriormente es un ejemplo de una notificación que aprovecha las acciones. La sensación de tareas de acabado es un sentimiento positivo universalmente e incorporar esa sensación de la aplicación o sitio Web mediante el envío de notificaciones que tienen contenido útil en ellos. Las notificaciones que requieren acción también pueden ayudar a aumentar la productividad, tanto en escenarios empresariales y para consumidores, reduciendo el tiempo a los usuarios de la acción que recorren para realizar estas tareas más pequeñas. Se recomienda incluir las acciones que realice periódicamente los usuarios o las cosas que intentan formar a los usuarios hacer.  Entre algunos ejemplos se incluyen:
* Gusto, gestión, marcar o protagonistas de contenido
* Aprobar o denegar los informes de gastos, tiempo libre, permisos, etcetera.
* Inline responder a mensajes de correos electrónicos, grupo chats, comentarios, etcetera.
* Completar pedidos con [actualización pendiente](toast-pending-update.md)
* Establecer alertas o avisos para otro momento, así como reserva potencialmente el tiempo en un calendario

Las notificaciones que requieren acción son una herramienta muy eficaz para ayudar a los usuarios a sentir productivo, realizar tareas y tener una experiencia excelente y eficiente con su aplicación o sitio Web.  ¡Existen muchas oportunidades aquí! Si necesita ayuda, intercambiando ideas, no dude en contacto con el equipo de notificaciones de windows.

## <a name="2-timing-and-urgency"></a>2. Control de tiempo y la urgencia
¡Al contrario de lo que a menudo pensamos sobre las notificaciones, en tiempo real no es necesariamente mejor! Le instamos a los desarrolladores piense en el usuario y si la notificación que están enviando es urgente información o no. Los usuarios pueden sobrecargarse con demasiada información fácilmente y obtener frustrados si se se interrumpe mientras se están intentando foco. Windows proporciona unas cuantas opciones para a evaluar la tendencia a la intrusión de las notificaciones que se va a enviar:

**Notificaciones sin procesar:** Uso de [notificaciones sin procesar](raw-notification-overview.md) puede ser beneficioso por diversos motivos, especialmente cuando se trata de ot minimizar las interrupciones en el usuario.  Envío de notificaciones sin procesar se activará la aplicación en segundo plano, para poder evaluar si la notificación de que tiene sentido para entregar inmediatamente en el contexto de la aplicación. Si es algo que cree que debe mostrarse al usuario inmediatamente, puede aparecer un [toast local](send-local-toast.md) desde allí.  Si es algo que el usuario no es necesario ver en este momento, puede crear un [programado del sistema](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) que se activará en un momento posterior.


**Registros fantasma del sistema:** también pueden activar una notificación que omita aparecerán en la esquina inferior derecha de la pantalla y, en su lugar enviar la notificación directamente al centro de actividades. Esto se consigue estableciendo la [SupressPopup propiedad](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) en True. Aunque podría haber algunos escepticismo en torno a no aparecerán las notificaciones fuera de centro de actividades, vemos una 2-3 veces superior extrae de engagement para notificaciones del sistema que se encuentran en el centro de actividades a través del sistema.  Los usuarios son más capacidad de respuesta cuando esté listos para recibir notificaciones y puede controlar cuando se interrumpen, motivo por el contenido en el centro de actividades puede ser mucho más eficaz noninvasively notifique a los usuarios.

## <a name="3-clear-out-the-clutter"></a>3. Limpiar el desorden
Pueden conservar las notificaciones en el centro de actividades durante un tiempo relativamente largo (el valor predeterminado es tres días).  Es imprescindible que asegurarse de que el contenido que se encuentra aquí es relevante y actualizada cada vez que el usuario abre el centro de actividades. Son desperdiciar espacio de pantalla del usuario y ocupar ranuras que podrían usarse para algo más actualizadas.  Supongamos que el usuario instala la aplicación de administración de correo electrónico y recibe diez correos electrónicos y diez notificaciones junto con los mensajes de correo electrónico.  Dependiendo de su experiencia deseada, es posible que considere la posibilidad de desactivar las notificaciones si el usuario leer el correo electrónico correspondiente, o puede abrir la aplicación como una forma de quitar el desorden anterior del centro de actividades.

Tenemos una serie de [ToastNotificationHistory](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotificationhistory) API que le permiten ver qué contenido se encuentra en el centro de actividades, así como administración estas notificaciones. Respetar a espacio en la pantalla del usuario y tenga cuidado de que solo se muestran contenido relevante y actual para los usuarios.

## <a name="4-keeping-organized"></a>4. Mantener organizada
Como se mencionó anteriormente, conservar el contenido en el centro de actividades durante tres días.  Para ayudar a los usuarios elegir la información que buscan rápidamente, organizar las notificaciones en el centro de acción con [encabezados](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/toast-headers) o [colecciones](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection). Puede ver un ejemplo de un encabezado a continuación.

![Ejemplos de notificación del sistema con los encabezados de la etiqueta "Camping!!"](images/toast-headers-action-center.png)

Agrupar estas notificaciones de una manera para que el contenido relevante permanece juntos (es decir, piense en descomponer las ligas deportivas diferentes en una aplicación de deportes u ordenar mensajes por chat de grupo). Las colecciones son una manera más obvia de notificaciones de grupo, mientras que los encabezados son más sutiles, pero ambos permiten a los usuarios evaluar y elegir las notificaciones más rápidamente.

## <a name="other-resources"></a>Otros recursos
Estos cuatro puntos anteriores son instrucciones que hemos encontrado efectiva a través de nuestro propio análisis de telemetría y primero y tercero experimentos. Tenga en cuenta, sin embargo, que estas instrucciones son justamente eso: directrices.  Estamos seguros de estas reglas le ayudarán a aumentar la participación y productividad de las notificaciones, pero nada puede sustituir pensar centrado en el usuario y de aprendizaje con sus propios datos.  

Si envía notificaciones a la aplicación UWP hoy en día, puede ver análisis sobre qué ha ocurrido con las notificaciones en [centro de partners](https://partner.microsoft.com/dashboard)! Estos datos se presentan gratuita cuando se usa el [Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) o el [WNS API](https://docs.microsoft.com/en-us/windows/uwp/design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview). Estas métricas le proporcionará más información sobre lo que ocurre con las notificaciones en la plataforma windows, así como la que los usuarios interactúan con las notificaciones. Obtener acceso a estos datos, vaya al menú en el lado izquierdo interactuar > notificaciones, y, a continuación, haga clic en la pestaña "Analizar" dentro de la página de notificaciones.  Esto se encuentra en el mismo lugar, vaya a enviar notificaciones desde el centro de partners.

## <a name="related-topics"></a>Temas relacionados

* [Contenido de notificación del sistema](adaptive-interactive-toasts.md)
* [Notificaciones sin procesar](raw-notification-overview.md)
* [Actualización pendiente](toast-pending-update.md)
* [Biblioteca de notificaciones en GitHub (parte del Kit de herramientas de comunidad de Windows)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
