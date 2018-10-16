---
author: andrewleader
Description: Learn how to use Notification Listener to access all of the user's notifications.
title: Agente de escucha de notificaciones
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tiles
template: detail.hbs
ms.author: mijacobs
ms.date: 06/13/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, agente de escucha de notificaciones, usernotificationlistener, documentación, acceso a las notificaciones
ms.localizationpriority: medium
ms.openlocfilehash: f4d8cb9ef7589bd8f0c56586ab8fcfec7c1f01e3
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "4689610"
---
# <a name="notification-listener-access-all-notifications"></a>Agente de escucha de notificaciones: acceder a todas las notificaciones

El agente de escucha de notificación permite acceder a las notificaciones de un usuario. Los relojes inteligentes y otros dispositivos transportables pueden usar el agente de escucha de notificaciones para enviar las notificaciones del teléfono al dispositivo transportable. Las aplicaciones de automatización domésticas pueden usar el agente de escucha de notificaciones para realizar acciones específicas cuando se reciban notificaciones, como hacer que parpadeen las luces cuando se recibe una llamada. 

> [!IMPORTANT]
> **Requiere la Actualización de aniversario**: Debes utilizar el SDK 14393 y estar ejecutando la compilación 14393 o superior para usar el agente de escucha de notificaciones.


> **API importantes**: [clase UserNotificationListener](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.Management.UserNotificationListener), [clase UserNotificationChangedTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.UserNotificationChangedTrigger)


## <a name="enable-the-listener-by-adding-the-user-notification-capability"></a>Habilitar el agente de escucha con el añadido de la capacidad Notificaciones de usuario 

Para usar el agente de escucha de notificaciones, debes agregar la capacidad Agente de escucha de notificaciones de usuario en el manifiesto de la aplicación.

1. En Visual Studio, en el Explorador de soluciones, haz doble clic en el archivo `Package.appxmanifest` para abrir el diseñador de manifiestos.
2. Abre la pestaña Capacidades.
3. Marca la capacidad **Agente de escucha de notificaciones de usuario**.


## <a name="check-whether-the-listener-is-supported"></a>Comprobar si se admite el agente de escucha

Si la aplicación admite versiones anteriores de Windows 10, debes utilizar la [clase ApiInformation](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation) para comprobar si se admite el agente de escucha.  Si el agente de escucha no se admite, evita ejecutar llamadas a las API del agente de escucha.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Notifications.Management.UserNotificationListener"))
{
    // Listener supported!
}
 
else
{
    // Older version of Windows, no Listener
}
```


## <a name="requesting-access-to-the-listener"></a>Solicitud de acceso al agente de escucha

Como el agente de escucha permite el acceso a las notificaciones del usuario, los usuarios deben conceder permiso a la aplicación para acceder a sus notificaciones. Durante la experiencia de primera ejecución de la aplicación, debes solicitar acceso para usar el agente de escucha de notificaciones. Si quieres, puedes mostrar una interfaz de usuario preliminar que explique por qué la aplicación necesita acceder a las notificaciones del usuario antes de llamar a [RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.RequestAccessAsync), de modo que el usuario comprenda por qué debe permitir el acceso.

```csharp
// Get the listener
UserNotificationListener listener = UserNotificationListener.Current;
 
// And request access to the user's notifications (must be called from UI thread)
UserNotificationListenerAccessStatus accessStatus = await listener.RequestAccessAsync();
 
switch (accessStatus)
{
    // This means the user has granted access.
    case UserNotificationListenerAccessStatus.Allowed:
 
        // Yay! Proceed as normal
        break;
 
    // This means the user has denied access.
    // Any further calls to RequestAccessAsync will instantly
    // return Denied. The user must go to the Windows settings
    // and manually allow access.
    case UserNotificationListenerAccessStatus.Denied:
 
        // Show UI explaining that listener features will not
        // work until user allows access.
        break;
 
    // This means the user closed the prompt without
    // selecting either allow or deny. Further calls to
    // RequestAccessAsync will show the dialog again.
    case UserNotificationListenerAccessStatus.Unspecified:
 
        // Show UI that allows the user to bring up the prompt again
        break;
}
```

El usuario puede revocar el acceso en cualquier momento a través de la Configuración de Windows. Por lo tanto, la aplicación siempre debe comprobar el estado del acceso a través d método [GetAccessStatus](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetAccessStatus) antes de ejecutar cualquier código que use el agente de escucha de notificaciones. Si el usuario revoca el acceso, las API tendrán un error de forma silenciosa en lugar de generar una excepción (por ejemplo, la API para obtener todas las notificaciones simplemente devolverá una lista vacía).


## <a name="access-the-users-notifications"></a>Acceder a las notificaciones del usuario

Con el agente de escucha de notificaciones, puedes obtener una lista de las notificaciones actuales del usuario. Solo tienes que llamar al método [GetNotificationsAsync](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetNotificationsAsync) y especificar el tipo de notificaciones que deseas obtener (actualmente, el único tipo de notificaciones admitido son las notificaciones del sistema).

```csharp
// Get the toast notifications
IReadOnlyList<UserNotification> notifs = await listener.GetNotificationsAsync(NotificationKinds.Toast);
```


## <a name="displaying-the-notifications"></a>Visualización de las notificaciones

Cada notificación se representa como una [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification), que proporciona información sobre la aplicación de la que procede la notificación, la hora a la que se creó la notificación, el identificador de la notificación y la propia notificación.

```csharp
public sealed class UserNotification
{
    public AppInfo AppInfo { get; }
    public DateTimeOffset CreationTime { get; }
    public uint Id { get; }
    public Notification Notification { get; }
}
```

La propiedad [AppInfo](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.AppInfo) proporciona la información que necesitas para mostrar la notificación.

> [!NOTE]
> Recomendamos rodear todo el código para procesar una sola notificación en un bloque try/catch, por si se produce una excepción inesperada cuando vayas a capturar una sola notificación. No debe ser imposible mostrar otras notificaciones solo porque se haya producido un problema en una notificación específica.

```csharp
// Select the first notification
UserNotification notif = notifs[0];
 
// Get the app's display name
string appDisplayName = notif.AppInfo.DisplayInfo.DisplayName;
 
// Get the app's logo
BitmapImage appLogo = new BitmapImage();
RandomAccessStreamReference appLogoStream = notif.AppInfo.DisplayInfo.GetLogo(new Size(16, 16));
await appLogo.SetSourceAsync(await appLogoStream.OpenReadAsync());
```

El contenido de la propia notificación, como el texto de la notificación, se encuentra en la propiedad [Notification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification.Notification). Esta propiedad contiene la parte visual de la notificación. (Si conoces el envío de notificaciones en Windows, observarás que las propiedades [Visual](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification.Visual) y [Visual.Bindings](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationvisual.Bindings) del objeto [Notification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notification) se corresponden a lo que los desarrolladores envían al sacar una notificación).

Queremos buscar el enlace de la notificación del sistema (para el código sea a prueba de errores, debes comprobar que el enlace no sea null). Desde el enlace puedes obtener los elementos de texto. Puedes elegir mostrar todos los elementos de texto que quieras. (Lo ideal es que muestres todos). Puedes elegir tratar los elementos de texto de manera diferente; por ejemplo, tratar el primer elemento como el texto del título y los posteriores como el texto del cuerpo.

```csharp
// Get the toast binding, if present
NotificationBinding toastBinding = notif.Notification.Visual.GetBinding(KnownNotificationBindings.ToastGeneric);
 
if (toastBinding != null)
{
    // And then get the text elements from the toast binding
    IReadOnlyList<AdaptiveNotificationText> textElements = toastBinding.GetTextElements();
 
    // Treat the first text element as the title text
    string titleText = textElements.FirstOrDefault()?.Text;
 
    // We'll treat all subsequent text elements as body text,
    // joining them together via newlines.
    string bodyText = string.Join("\n", textElements.Skip(1).Select(t => t.Text));
}
```


## <a name="remove-a-specific-notification"></a>Quitar una notificación específica

Si el dispositivo transportable o el servicio permiten al usuario descartar notificaciones, puedes quitar la notificación real para que el usuario no la vea luego en su teléfono o equipo. Simplemente proporciona el identificador de la notificación (obtenido desde el objeto [UserNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.usernotification)) de la notificación que quieras quitar: 

```csharp
// Remove the notification
listener.RemoveNotification(notifId);
```


## <a name="clear-all-notifications"></a>Borrar todas las notificaciones.

El método [UserNotificationListener.ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) borra todas las notificaciones del usuario. Usa este método con precaución. Solo debes borrar todas las notificaciones si el dispositivo transportable o el servicio muestran TODAS las notificaciones. Si el dispositivo transportable o el servicio solo muestran ciertas notificaciones, cuando el usuario hace clic en el botón "Borrar notificaciones", solo espera que se eliminen esas notificaciones específicas; sin embargo, si se llama al método [ClearNotifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications), realmente se provocaría que se eliminaran todas las notificaciones, incluidas aquellas que no se estaban mostrando en el dispositivo transportable o el servicio.

```csharp
// Clear all notifications. Use with caution.
listener.ClearNotifications();
```


## <a name="background-task-for-notification-addeddismissed"></a>Tarea en segundo plano para una notificación agregada/descartada

Una forma común de permitir que una aplicación escuche notificaciones es configurar una tarea en segundo plano, de forma que sepas cuándo se ha agregado o descartado una notificación independientemente de si la aplicación se está ejecutando en ese momento.

Gracias al [modelo de proceso único](../../../launch-resume/create-and-register-an-inproc-background-task.md) agregado en la Actualización de aniversario, agregar tareas en segundo plano es bastante sencillo. En el código de la aplicación principal, tras haber obtenido el acceso del usuario al agente de escucha de notificaciones y obtener acceso para ejecutar tareas en segundo plano, simplemente tienes que registrar una nueva tarea en segundo plano y establecer el [UserNotificationChangedTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.usernotificationchangedtrigger) mediante el [tipo de notificación del sistema](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationkinds).

```csharp
// TODO: Request/check Listener access via UserNotificationListener.Current.RequestAccessAsync
 
// TODO: Request/check background task access via BackgroundExecutionManager.RequestAccessAsync
 
// If background task isn't registered yet
if (!BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals("UserNotificationChanged")))
{
    // Specify the background task
    var builder = new BackgroundTaskBuilder()
    {
        Name = "UserNotificationChanged"
    };
 
    // Set the trigger for Listener, listening to Toast Notifications
    builder.SetTrigger(new UserNotificationChangedTrigger(NotificationKinds.Toast));
 
    // Register the task
    builder.Register();
}
```

A continuación, en App.xaml.cs, reemplaza el método [OnBackgroundActivated](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.OnBackgroundActivated) si aún no lo has hecho y usa una instrucción switch en el nombre de la tarea para determinar cuál de las varios desencadenadores de tarea en segundo plano se ha invocado.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "UserNotificationChanged":
            // Call your own method to process the new/removed notifications
            // The next section of documentation discusses this code
            await MyWearableHelpers.SyncNotifications();
            break;
    }
 
    deferral.Complete();
}
```

La tarea en segundo plano es simplemente una "llamada de atención": no proporciona ninguna información sobre qué notificación específica se ha agregado o quitado. Cuando se desencadene la tarea en segundo plano, debes sincronizar las notificaciones en el dispositivo transportable para que reflejen las notificaciones en la plataforma. Esto garantiza que si se produce un error en la tarea en segundo plano, las notificaciones en el dispositivo transportable se pueden recuperar la próxima vez que se ejecute la tarea en segundo plano.

`SyncNotifications` es un método que implementas; la siguiente sección muestra cómo hacerlo. 


## <a name="determining-which-notifications-were-added-and-removed"></a>Determinar qué notificaciones se han agregado o quitado

En el método `SyncNotifications`, para determinar qué notificaciones se han agregado o quitado (sincronización de las notificaciones con el dispositivo transportable), tienes que calcular la diferencia entre la colección actual de notificaciones y las notificaciones que haya en la plataforma.

```csharp
// Get all the current notifications from the platform
IReadOnlyList<UserNotification> userNotifications = await listener.GetNotificationsAsync(NotificationKinds.Toast);
 
// Obtain the notifications that our wearable currently has displayed
IList<uint> wearableNotificationIds = GetNotificationsOnWearable();
 
// Copy the currently displayed into a list of notification ID's to be removed
var toBeRemoved = new List<uint>(wearableNotificationIds);
 
// For each notification in the platform
foreach (UserNotification userNotification in userNotifications)
{
    // If we've already displayed this notification
    if (wearableNotificationIds.Contains(userNotification.Id))
    {
        // We want to KEEP it displayed, so take it out of the list
        // of notifications to remove.
        toBeRemoved.Remove(userNotification.Id);
    }
 
    // Othwerise it's a new notification
    else
    {
        // Display it on the Wearable
        SendNotificationToWearable(userNotification);
    }
}
 
// Now our toBeRemoved list only contains notification ID's that no longer exist in the platform.
// So we will remove all those notifications from the wearable.
foreach (uint id in toBeRemoved)
{
    RemoveNotificationFromWearable(id);
}
```


## <a name="foreground-event-for-notification-addeddismissed"></a>Evento en primer plano para una notificación agregada/descartada

> [!IMPORTANT] 
> Problema conocido: el evento en primer plano provocarán un bucle de CPU en las versiones recientes de Windows y previamente no funcionó antes de que. NO uses el evento en primer plano. En una próxima actualización de Windows, se corrige esto.

En lugar de usar el evento en primer plano, usa el código que se muestra anteriormente para una tarea en segundo plano de [modelo de proceso único](../../../launch-resume/create-and-register-an-inproc-background-task.md) . La tarea en segundo plano, también podrás recibir notificaciones evento cambio mientras la aplicación está cerrada o se está ejecutando.

```csharp
// Subscribe to foreground event (DON'T USE THIS)
listener.NotificationChanged += Listener_NotificationChanged;
 
private void Listener_NotificationChanged(UserNotificationListener sender, UserNotificationChangedEventArgs args)
{
    // NOTE: This event WILL CAUSE CPU LOOPS, DO NOT USE. Use the background task instead.
}
```


## <a name="how-to-fix-delays-in-the-background-task"></a>Cómo solucionar los retrasos en la tarea en segundo plano

Al probar la aplicación, es posible que observes que a veces la tarea en segundo plano se retrasa y no se desencadena durante varios minutos. Para arreglarlo, puedes pedir al usuario que vaya a la Configuración del sistema -> Sistema -> Batería -> Uso de la batería por aplicación, busque la aplicación en la lista, la seleccione y la cambie a "Siempre permitido en segundo plano". Después, la tarea en segundo plano debe desencadenarse siempre aproximadamente un segundo después de recibirse la notificación.