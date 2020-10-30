---
description: Obtenga información sobre cómo programar una notificación del sistema local para que aparezca en otro momento.
title: Programar una notificación del sistema
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: Windows 10, UWP, notificación del sistema programada, scheduledtoastnotification, Inicio rápido, introducción, ejemplo de código, tutorial
ms.localizationpriority: medium
ms.openlocfilehash: 2a138458634f0246d7e6bed9d6d65c2479dac3c9
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030698"
---
# <a name="schedule-a-toast-notification"></a>Programar una notificación del sistema

Las notificaciones del sistema programadas le permiten programar una notificación para que aparezca en un momento posterior, independientemente de si la aplicación se está ejecutando en ese momento. Esto resulta útil para escenarios como la visualización de recordatorios u otras tareas de seguimiento para el usuario, donde el tiempo y el contenido de la notificación se conoce por antemano.

Tenga en cuenta que las notificaciones del sistema programadas tienen una ventana de entrega de 5 minutos. Si el equipo se apaga durante el tiempo de entrega programado y permanece desactivado durante más de 5 minutos, la notificación se "quitará", ya que ya no es relevante para el usuario. Si necesita la entrega garantizada de notificaciones independientemente de cuánto tiempo se desactive el equipo, se recomienda usar una tarea en segundo plano con un desencadenador de tiempo, tal como se muestra en [este ejemplo de código](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off).

> [!IMPORTANT]
> Las aplicaciones de escritorio (tanto los paquetes dispersos como MSIX y el escritorio clásico) tienen pasos ligeramente diferentes para enviar notificaciones y controlar la activación. Siga las instrucciones que se indican a continuación, pero reemplace `ToastNotificationManager` por la `DesktopNotificationManagerCompat` clase de la documentación de las [aplicaciones de escritorio](toast-desktop-apps.md) .

> **API importantes** : [clase ScheduledToastNotification](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>Requisitos previos

Para entender completamente este tema, le resultará útil lo siguiente...

* Conocimiento práctico de los términos y conceptos de las notificaciones del sistema. Para obtener más información, consulte la información [General del sistema y del centro de actividades](/archive/blogs/tiles_and_toasts/toast-notification-and-action-center-overview-for-windows-10).
* Familiaridad con el contenido de las notificaciones del sistema de Windows 10. Para obtener más información, consulte [documentación del contenido del sistema](adaptive-interactive-toasts.md).
* Un proyecto de aplicación para UWP de Windows 10


## <a name="step-1-install-nuget-package"></a>Paso 1: instalar el paquete NuGet

Instale el [paquete NuGet Microsoft. Toolkit. UWP. notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/). Nuestro ejemplo de código usará este paquete. Al final del artículo, proporcionaremos los fragmentos de código "sin formato" que no usan ningún paquete NuGet. Este paquete le permite crear notificaciones del sistema sin usar XML.


## <a name="step-2-add-namespace-declarations"></a>Paso 2: agregar declaraciones de espacio de nombres

`Windows.UI.Notifications` incluye las API del sistema.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-schedule-the-notification"></a>Paso 3: programación de la notificación

Usaremos una notificación simple basada en texto que recordará a un estudiante sobre el uso que han vencido hoy en día. Construya la notificación y programe la programación.

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("itemsDueToday", ToastActivationType.Foreground)
    .AddText("ASTR 170B1")
    .AddText("You have 3 items due today!");
    .GetToastContent();

    
// Create the scheduled notification
var toast = new ScheduledToastNotification(content.GetXml(), DateTime.Now.AddSeconds(5));


// Add your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="provide-a-primary-key-for-your-toast"></a>Proporcionar una clave principal para la notificación del sistema

Si desea cancelar, quitar o reemplazar la notificación programada mediante programación, debe usar la propiedad de etiqueta (y, opcionalmente, la propiedad de grupo) para proporcionar una clave principal para la notificación. Después, puede usar esta clave principal en el futuro para cancelar, quitar o reemplazar la notificación.

Para ver más detalles sobre cómo reemplazar o quitar notificaciones del sistema ya entregadas, consulte [Inicio rápido: administrar notificaciones del sistema en el centro de actividades (XAML)](/previous-versions/windows/apps/dn631260(v=win.10)).

Etiqueta y grupo combinados actúan como clave principal compuesta. Grupo es el identificador más genérico, donde puede asignar grupos como "wallPosts", "Messages", "friendRequests", etc. Y, a continuación, la etiqueta debe identificar de forma única la notificación en el grupo. Mediante el uso de un grupo genérico, puede quitar todas las notificaciones de ese grupo mediante la [API de RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="cancel-scheduled-notifications"></a>Cancelación de notificaciones programadas

Para cancelar una notificación programada, primero debe recuperar la lista de todas las notificaciones programadas.

A continuación, busque la notificación del sistema programada que coincida con la etiqueta (y, opcionalmente, agrupe) que especificó anteriormente y llame a RemoveFromSchedule ().

```csharp
// Create the toast notifier
ToastNotifier notifier = ToastNotificationManager.CreateToastNotifier();

// Get the list of scheduled toasts that haven't appeared yet
IReadOnlyList<ScheduledToastNotification> scheduledToasts = notifier.GetScheduledToastNotifications();

// Find our scheduled toast we want to cancel
var toRemove = scheduledToasts.FirstOrDefault(i => i.Tag == "18365" && i.Group == "ASTR 170B1");
if (toRemove != null)
{
    // And remove it from the schedule
    notifier.RemoveFromSchedule(toRemove);
}
```


## <a name="activation-handling"></a>Control de activación

Consulte la documentación de [envío de un sistema local](send-local-toast.md) para obtener más información sobre el control de la activación. La activación de una notificación del sistema programada se administra igual que la activación de una notificación del sistema local.


## <a name="adding-actions-inputs-and-more"></a>Agregar acciones, entradas, etc.

Vea los documentos sobre cómo [enviar una notificación del sistema local](send-local-toast.md) para obtener más información sobre temas avanzados, como acciones y entradas. Las acciones y las entradas funcionan igual en las notificaciones del sistema locales que en los trabajos programados.


## <a name="resources"></a>Recursos

* [Documentación del contenido del sistema](adaptive-interactive-toasts.md)
* [Clase ScheduledToastNotification](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)
