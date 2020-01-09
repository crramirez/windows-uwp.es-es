---
Description: Obtenga información sobre cómo crear notificaciones eficaces y centradas en el usuario que hacen que los usuarios sean productivos y satisfechos.
title: Instrucciones para la experiencia del usuario
label: Toast UX Guidance
template: detail.hbs
ms.date: 05/18/2018
ms.topic: article
keywords: Windows 10, UWP, notificación, colección, grupo, experiencia de usuario, guía de la experiencia del usuario, guía, acción, notificación del sistema, centro de actividades, notificaciones de no interrupción y efectivas, notificaciones no intrusivas, accionables, administrar, organizar
ms.localizationpriority: medium
ms.openlocfilehash: 9e970b5ae2019dcc0290dc6b0b72b48208eb5c12
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684599"
---
# <a name="toast-notification-ux-guidance"></a>Guía de experiencia del usuario de notificación del sistema
Las notificaciones son una parte necesaria de la vida moderna; ayudan a los usuarios a ser más productivos y a trabajar con aplicaciones y sitios web, así como mantenerse al día con las actualizaciones. Sin embargo, las notificaciones pueden pasar rápidamente de ser útiles a la sobreutilización e intrusivo si no están diseñadas de forma centrada en el usuario. Las notificaciones son un clic con el botón derecho fuera de la desactivación y es improbable que una vez desactivadas, se vuelvan a activar.  Por lo tanto, asegúrese de que las notificaciones son respetuosas con el espacio de pantalla y el tiempo del usuario, de modo que pueda mantener este canal de Engagement abierto.

> **API importantes**: [paquete Nuget de notificaciones de Windows Community Toolkit](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

Hemos analizado la telemetría de Windows, así como otros casos prácticos de terceros y de terceros, para presentar cuatro reglas sobre lo que crea una excelente historia de notificaciones.  Estamos seguros de que estas reglas se aplican universalmente, independientemente de la plataforma, y ayudarán a las notificaciones a tener un impacto positivo en los usuarios.

## <a name="1-actionable-notifications"></a>1. notificaciones accionables
Las notificaciones procesables permiten a los usuarios ser productivos sin necesidad de abrir la aplicación.  Aunque es excelente que se inicie la aplicación, esta no es la única medida de éxito y permitir que los usuarios realicen tareas pequeñas sin tener que ir a la aplicación puede ser una herramienta muy eficaz para aclarar los usuarios.

![Notificación accionable con el cuadro de texto de entrada y botones para establecer recordatorios y responder a la notificación](images/actionable-notification-example01.png)

Lo anterior es un ejemplo de una notificación que aprovecha las acciones. La sensación de finalizar las tareas es una sensación universal y puede llevarla a su aplicación o sitio web mediante el envío de notificaciones que tienen contenido accionable en ellas. Las notificaciones accionables también pueden ayudar a aumentar la productividad, tanto en escenarios empresariales como de consumidor, al reducir el tiempo que los usuarios llevan a cabo para llevar a cabo estas tareas más pequeñas. Se recomienda incluir las acciones que los usuarios llevan a cabo regularmente o las cosas que está intentando entrenar a los usuarios.  Algunos ejemplos incluyen:
* Contenido de gusto, agregando, marcado o protagonistas
* Aprobación o denegación de informes de gastos, tiempo de inactividad, permisos, etc.
* Respuesta en línea a mensajes, correos electrónicos, chats de grupo, comentarios, etc.
* Completar pedidos mediante la [actualización pendiente](toast-pending-update.md)
* Configuración de alertas o recordatorios en otro momento, así como el tiempo de reserva potencial en un calendario

Las notificaciones procesables son una herramienta muy eficaz para ayudar a los usuarios a sentirse productivas, realizar tareas y tener una experiencia excelente y eficaz con la aplicación o el sitio Web.  Hay muchas oportunidades aquí. Si desea ayudar a compartir ideas, no dude en ponerse en contacto con el equipo de notificaciones de Windows.

## <a name="2-timing-and-urgency"></a>2. temporización y urgencia
Al contrario de lo que a menudo pensamos en las notificaciones, el tiempo real no es necesariamente el mejor. Instamos a los desarrolladores a pensar en el usuario y si la notificación que envían es una información urgente o no. Los usuarios se pueden sobrecargar fácilmente con demasiada información y hacerse frustración si se interrumpen mientras intentan centrarse. Windows proporciona algunas opciones para considerar la intrusión de las notificaciones que se envían:

**Notificaciones sin procesar:** El uso de [notificaciones sin procesar](raw-notification-overview.md) puede ser beneficioso por muchas razones, especialmente cuando se trata de una reducción de las interrupciones al usuario.  El envío de notificaciones sin procesar reactivará la aplicación en segundo plano, por lo que puede evaluar si la notificación tiene sentido que se entregue inmediatamente en el contexto de la aplicación. Si es algo que cree que se debe mostrar al usuario de inmediato, puede extraer una [notificación del sistema local](send-local-toast.md) desde allí.  Si es algo que el usuario no necesita ver en este momento, puede crear una [notificación programada](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/09/30/quickstart-sending-an-alarm-in-windows-10/) que se activará en un momento posterior.


Notificaciones **fantasma:** también puede activar una notificación que omitirá la presentación en la esquina inferior derecha de la pantalla y, en su lugar, enviará la notificación directamente al centro de actividades. Esto se logra estableciendo la [propiedad SupressPopup](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotification.suppresspopup) en true. Aunque es posible que haya algunas escepticismo en torno a no sacar las notificaciones fuera del centro de actividades, vemos un compromiso de 2-3 veces mayor para los notificaciones que viven en el centro de actividades sobre la notificación del sistema.  Los usuarios tienen mayor capacidad de respuesta cuando están preparados para recibir notificaciones y pueden controlar cuándo se interrumpen, por lo que el contenido del centro de actividades puede ser mucho más eficaz para los usuarios que notifican de forma no invasiva a los usuarios.

## <a name="3-clear-out-the-clutter"></a>3. Desactive la confusión
Las notificaciones pueden persistir en el centro de actividades durante bastante tiempo (el valor predeterminado es tres días).  Es imperativo asegurarse de que el contenido que vive aquí esté actualizado y sea relevante cada vez que el usuario abra el centro de actividades. Está desperdiciando el espacio de la pantalla del usuario y ocupando ranuras que podrían usarse para algo más actualizado.  Supongamos que el usuario instala la aplicación de administración de correo electrónico y recibe diez correos electrónicos y diez notificaciones junto con esos correos electrónicos.  En función de la experiencia deseada, puede considerar la posibilidad de borrar esas notificaciones si el usuario ha leído el correo electrónico correspondiente o si ha abierto la aplicación como una forma de quitar el desorden anterior del centro de actividades.

Tenemos una serie de API de [ToastNotificationHistory](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotificationhistory) que le permiten ver qué contenido está en el centro de actividades, así como administrar estas notificaciones. Sea respetuoso con el espacio de la pantalla del usuario y tenga cuidado de que solo muestre contenido relevante y actual a los usuarios.

## <a name="4-keeping-organized"></a>4. mantener organizado
Como se mencionó anteriormente, el contenido del centro de actividades se conserva durante tres días.  Para ayudar a los usuarios a seleccionar la información que buscan rápidamente, organice las notificaciones en el centro de actividades mediante [encabezados](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-headers) o [recopilaciones](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastcollection). Puede ver un ejemplo de un encabezado a continuación.

![Ejemplos del sistema con encabezados con la etiqueta ' Camping!! '](images/toast-headers-action-center.png)

Agrupe estas notificaciones de forma que el contenido correspondiente permanezca en conjunto (es decir, piense en separar las distintas ligas deportivas en una aplicación deportiva u ordene los mensajes por chat de grupo). Las colecciones son una manera más obvia de agrupar las notificaciones, mientras que los encabezados son más sutiles, pero ambos permiten a los usuarios evaluar y seleccionar las notificaciones más rápidamente.



Estos cuatro puntos anteriores son instrucciones que hemos encontrado en el análisis de la telemetría y en los experimentos de primer y de terceros. No obstante, tenga en cuenta que estas instrucciones son solo las siguientes: instrucciones.  Estamos seguros de que estas reglas le ayudarán a aumentar el compromiso y la productividad de las notificaciones, pero nada puede sustituir el pensamiento centrado en el usuario y aprender de sus propios datos.  

## <a name="related-topics"></a>Temas relacionados

* [Contenido del sistema](adaptive-interactive-toasts.md)
* [Notificaciones sin procesar](raw-notification-overview.md)
* [Actualización pendiente](toast-pending-update.md)
* [Biblioteca de notificaciones en GitHub (parte del kit de herramientas de la comunidad de Windows)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
