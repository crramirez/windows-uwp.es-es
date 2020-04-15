---
Description: Obtenga información sobre cómo programar una notificación del sistema local para que aparezca en otro momento.
title: Programar una notificación del sistema
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: Windows 10, UWP, notificación del sistema programada, scheduledtoastnotification, Inicio rápido, introducción, ejemplo de código, tutorial
ms.localizationpriority: medium
ms.openlocfilehash: 07339cf793bdada51f79d70d9e9e6b6d4a41851b
ms.sourcegitcommit: 017f2f1492f3220da0fae8b4c99de7206a185dff
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2020
ms.locfileid: "81386434"
---
# <a name="schedule-a-toast-notification"></a>Programar una notificación del sistema

Las notificaciones del sistema programadas le permiten programar una notificación para que aparezca en un momento posterior, independientemente de si la aplicación se está ejecutando en ese momento. Esto resulta útil para escenarios como la visualización de recordatorios u otras tareas de seguimiento para el usuario, donde el tiempo y el contenido de la notificación se conoce por antemano.

Tenga en cuenta que las notificaciones del sistema programadas tienen una ventana de entrega de 5 minutos. Si el equipo se apaga durante el tiempo de entrega programado y permanece desactivado durante más de 5 minutos, la notificación se "quitará", ya que ya no es relevante para el usuario. Si necesita la entrega garantizada de notificaciones independientemente de cuánto tiempo se desactive el equipo, se recomienda usar una tarea en segundo plano con un desencadenador de tiempo, tal como se muestra en [este ejemplo de código](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off).

> [!IMPORTANT]
> Las aplicaciones de escritorio (tanto los paquetes dispersos como los MSIX y los de Win32 clásico) tienen pasos ligeramente diferentes para enviar notificaciones y controlar la activación. Siga las instrucciones que se indican a continuación, pero reemplace `ToastNotificationManager` por la clase `DesktopNotificationManagerCompat` de la documentación de las [aplicaciones de escritorio](toast-desktop-apps.md) .

> **API importantes**: [clase ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>Requisitos previos

Para comprender completamente este tema, será útil lo siguiente...

* Conocimientos prácticos sobre los términos y conceptos relacionados con las notificaciones del sistema. Para obtener más información, consulte la información [General del sistema y del centro de actividades](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/).
* Estar familiarizado con el contenido de la notificación del sistema de Windows 10. Para obtener más información, consulta la [documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md).
* Un proyecto de aplicación para UWP de Windows 10


## <a name="install-nuget-packages"></a>Instalación de paquetes NuGet

Te recomendamos instalar los dos siguientes paquetes NuGet en tu proyecto. Nuestro ejemplo de código usará estos paquetes.

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/): genera cargas de notificaciones del sistema mediante objetos en lugar de XML sin formato.
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/): genera y analiza las cadenas de consulta con C#.


## <a name="add-namespace-declarations"></a>Agregar declaraciones de espacios de nombres

`Windows.UI.Notifications` incluye las API del sistema.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="construct-the-toast-content"></a>Construcción del contenido del sistema

En Windows 10, el contenido de la notificación del sistema se describe con un lenguaje adaptable que permite una gran flexibilidad con el aspecto de la notificación. Consulta la [documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md) para obtener más información.

Gracias a la biblioteca de notificaciones, la generación del contenido XML es sencilla. Si no instalas la biblioteca de notificaciones desde NuGet, tienes que construir el XML manualmente, lo que deja lugar a errores.

Siempre debes establecer la propiedad **Launch** para que cuando el usuario pulse el cuerpo de la notificación del sistema y se inicie tu aplicación, tu aplicación sepa qué contenido debe mostrar.

```csharp
// In a real app, these would be initialized with actual data
string title = "ASTR 170B1";
string content = "You have 3 items due today!";

// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = title
                },
     
                new AdaptiveText()
                {
                    Text = content
                }
            }
        }
    },
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewClass" },
        { "classId", "3910938180" }
 
    }.ToString()
};
```

## <a name="create-the-scheduled-toast"></a>Creación del sistema programado

Una vez que haya inicializado el contenido del sistema, cree un nuevo [ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification) y pase el XML del contenido y el tiempo que desea que se entregue la notificación.

```csharp
// Create the scheduled notification
var toast = new ScheduledToastNotification(toastContent.GetXml(), DateTime.Now.AddSeconds(5));
```


## <a name="provide-a-primary-key-for-your-toast"></a>Proporcionar una clave principal para la notificación del sistema

Si desea cancelar, quitar o reemplazar la notificación programada mediante programación, debe usar la propiedad de etiqueta (y, opcionalmente, la propiedad de grupo) para proporcionar una clave principal para la notificación. Después, puede usar esta clave principal en el futuro para cancelar, quitar o reemplazar la notificación.

Para ver más detalles sobre reemplazar o quitar las notificaciones del sistema ya entregadas, consulta [Inicio rápido: administración de notificaciones del sistema en el Centro de actividades (XAML)](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10)).

Etiqueta y Grupo combinados actúan como clave principal compuesta. Grupo es el identificador más genérico, donde puedes asignar grupos como "wallPosts", "messages", "friendRequests", messages. Y, a continuación, Tag debe identificar de manera única la propia notificación desde dentro del grupo. Al usar un grupo genérico, puedes quitar todas las notificaciones de ese grupo mediante la [API RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_).

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="schedule-the-notification"></a>Programación de la notificación

Por último, cree un [ToastNotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier) y llame a AddToSchedule (), pasando la notificación del sistema programada.

```csharp
// And your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="cancel-scheduled-notifications"></a>Cancelar notificaciones programadas

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
* [Clase ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)
