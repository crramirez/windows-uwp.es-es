---
Description: Aprenda a usar el agente de escucha de notificaciones para tener acceso a todas las notificaciones del usuario.
title: Agente de escucha de notificaciones
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tiles
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: Windows 10, UWP, agente de escucha de notificaciones, usernotificationlistener, documentación, notificaciones de acceso
ms.localizationpriority: medium
ms.openlocfilehash: dc2afb36337439cd115273cd9df8ee1cb2eb3741
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169189"
---
# <a name="notification-listener-access-all-notifications"></a>Agente de escucha de notificaciones: acceso a todas las notificaciones

El agente de escucha de notificaciones proporciona acceso a las notificaciones de un usuario. Smartwatches y otros ponibles pueden usar el agente de escucha de notificaciones para enviar las notificaciones del teléfono al dispositivo portátil. Las aplicaciones de automatización de inicio pueden usar el agente de escucha de notificaciones para realizar acciones específicas cuando se reciben notificaciones, como hacer parpadear las luces cuando se recibe una llamada. 

> [!IMPORTANT]
> **Requiere actualización de aniversario**: debe tener como destino el SDK 14393 y ejecutar la compilación 14393 o superior para usar el agente de escucha de notificaciones.


> **API importantes**: [clase UserNotificationListener](/uwp/api/Windows.UI.Notifications.Management.UserNotificationListener), [clase UserNotificationChangedTrigger](/uwp/api/Windows.ApplicationModel.Background.UserNotificationChangedTrigger)


## <a name="enable-the-listener-by-adding-the-user-notification-capability"></a>Habilitación del agente de escucha mediante la adición de la funcionalidad de notificación de usuario 

Para usar el agente de escucha de notificaciones, debe agregar la capacidad del agente de escucha de notificaciones de usuario al manifiesto de la aplicación.

1. En el Explorador de soluciones de Visual Studio, haga doble clic en el `Package.appxmanifest` archivo para abrir el diseñador de manifiestos.
2. Abre la pestaña Capacidades.
3. Compruebe la capacidad del **agente de escucha de notificaciones de usuario** .


## <a name="check-whether-the-listener-is-supported"></a>Comprobar si se admite el agente de escucha

Si la aplicación admite versiones anteriores de Windows 10, debe usar la [clase ApiInformation](/uwp/api/Windows.Foundation.Metadata.ApiInformation) para comprobar si se admite el agente de escucha.  Si no se admite el agente de escucha, evite ejecutar llamadas a las API de agente de escucha.

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


## <a name="requesting-access-to-the-listener"></a>Solicitar acceso al agente de escucha

Dado que el agente de escucha permite el acceso a las notificaciones del usuario, los usuarios deben conceder permiso a su aplicación para acceder a las notificaciones. Durante la primera ejecución de la aplicación, debe solicitar acceso para usar el agente de escucha de notificaciones. Si lo desea, puede mostrar una interfaz de usuario preliminar que explica por qué la aplicación necesita acceder a las notificaciones del usuario antes de llamar a [RequestAccessAsync](/uwp/api/windows.ui.notifications.management.usernotificationlistener.RequestAccessAsync), de modo que el usuario comprenda por qué deben permitir el acceso.

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

El usuario puede revocar el acceso en cualquier momento a través de la configuración de Windows. Por lo tanto, la aplicación debe comprobar siempre el estado de acceso a través del método [GetAccessStatus](/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetAccessStatus) antes de ejecutar el código que usa el agente de escucha de notificaciones. Si el usuario revoca el acceso, las API producirán un error de forma silenciosa en lugar de producir una excepción (por ejemplo, la API para obtener todas las notificaciones simplemente devolverá una lista vacía).


## <a name="access-the-users-notifications"></a>Acceder a las notificaciones del usuario

Con el agente de escucha de notificaciones, puede obtener una lista de las notificaciones actuales del usuario. Simplemente llame al método [GetNotificationsAsync](/uwp/api/windows.ui.notifications.management.usernotificationlistener.GetNotificationsAsync) y especifique el tipo de notificaciones que desea obtener (actualmente, el único tipo de notificaciones admitidas son las notificaciones del sistema).

```csharp
// Get the toast notifications
IReadOnlyList<UserNotification> notifs = await listener.GetNotificationsAsync(NotificationKinds.Toast);
```


## <a name="displaying-the-notifications"></a>Mostrar las notificaciones

Cada notificación se representa como una [UserNotification](/uwp/api/windows.ui.notifications.usernotification), que proporciona información sobre la aplicación de la que procede la notificación, la hora a la que se creó la notificación, el identificador de la notificación y la propia notificación.

```csharp
public sealed class UserNotification
{
    public AppInfo AppInfo { get; }
    public DateTimeOffset CreationTime { get; }
    public uint Id { get; }
    public Notification Notification { get; }
}
```

La propiedad [Appinfo](/uwp/api/windows.ui.notifications.usernotification.AppInfo) proporciona la información que necesita para mostrar la notificación.

> [!NOTE]
> Se recomienda rodear todo el código para procesar una única notificación en un try/catch, en caso de que se produzca una excepción inesperada al capturar una notificación única. No debe realizar ninguna notificación por completo para mostrar otras notificaciones solo debido a un problema con una notificación específica.

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

El contenido de la propia notificación, como el texto de notificación, se incluye en la propiedad de [notificación](/uwp/api/windows.ui.notifications.usernotification.Notification) . Esta propiedad contiene la parte visual de la notificación. (Si está familiarizado con el envío de notificaciones en Windows, observará que las propiedades [Visual](/uwp/api/windows.ui.notifications.notification.Visual) y [visual. bindings](/uwp/api/windows.ui.notifications.notificationvisual.Bindings) del objeto de [notificación](/uwp/api/windows.ui.notifications.notification) corresponden a lo que los desarrolladores envían cuando extraen una notificación).

Queremos buscar el enlace del sistema (para el código de prueba de errores, debe comprobar que el enlace no sea nulo). En el enlace, puede obtener los elementos de texto. Puede optar por mostrar tantos elementos de texto como desee. (Idealmente, debería mostrarlos todos). Puede optar por tratar los elementos de texto de forma diferente. por ejemplo, trate el primero como texto del título y los elementos subsiguientes como texto del cuerpo.

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

Si su portátil o servicio permite al usuario descartar las notificaciones, puede quitar la notificación real para que el usuario no la vea más adelante en su teléfono o equipo. Simplemente proporcione el identificador de notificación (obtenido del objeto [UserNotification](/uwp/api/windows.ui.notifications.usernotification) ) de la notificación que desea quitar: 

```csharp
// Remove the notification
listener.RemoveNotification(notifId);
```


## <a name="clear-all-notifications"></a>Borrar todas las notificaciones

El método [UserNotificationListener. ClearNotifications](/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) borra todas las notificaciones del usuario. Use este método con precaución. Solo debe borrar todas las notificaciones si su portátil o servicio muestra todas las notificaciones. Si su portátil o servicio solo muestra ciertas notificaciones, cuando el usuario hace clic en el botón "borrar notificaciones", el usuario solo espera que se quiten las notificaciones específicas; sin embargo, si se llama al método [ClearNotifications](/uwp/api/windows.ui.notifications.management.usernotificationlistener.ClearNotifications) , realmente se eliminarían todas las notificaciones, incluidas las que el servicio o el portátil no mostraban.

```csharp
// Clear all notifications. Use with caution.
listener.ClearNotifications();
```


## <a name="background-task-for-notification-addeddismissed"></a>Tarea en segundo plano para la notificación agregada o descartada

Una forma habitual de permitir que una aplicación escuche notificaciones es configurar una tarea en segundo plano para que pueda saber cuándo se ha agregado o descartado una notificación independientemente de si la aplicación se está ejecutando actualmente.

Gracias al [modelo de proceso único](../../../launch-resume/create-and-register-an-inproc-background-task.md) agregado en la actualización de aniversario, agregar tareas en segundo plano es bastante sencillo. En el código de la aplicación principal, después de obtener el acceso del usuario al agente de escucha de notificaciones y obtener acceso para ejecutar las tareas en segundo plano, simplemente registre una nueva tarea en segundo plano y establezca [UserNotificationChangedTrigger](/uwp/api/windows.applicationmodel.background.usernotificationchangedtrigger) con el [tipo de notificación del sistema](/uwp/api/windows.ui.notifications.notificationkinds).

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

Después, en el App.xaml.cs, invalide el método [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.OnBackgroundActivated) si todavía no lo ha hecho, y use una instrucción switch en el nombre de la tarea para determinar qué desencadenadores de tareas en segundo plano se invocaron.

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "UserNotificationChanged":
            // Call your own method to process the new/removed notifications
            // The next section of documentation discusses this code
            await MyWearableHelpers.SyncNotifications();
            break;
    }
 
    deferral.Complete();
}
```

La tarea en segundo plano es simplemente una "TAP de hombro": no proporciona ninguna información sobre la notificación específica que se ha agregado o quitado. Cuando se desencadene la tarea en segundo plano, debe sincronizar las notificaciones en el portátil para que reflejen las notificaciones en la plataforma. Esto garantiza que, si se produce un error en la tarea en segundo plano, las notificaciones en el portátil se pueden recuperar la próxima vez que se ejecute la tarea en segundo plano.

`SyncNotifications` es un método que se implementa; en la sección siguiente se muestra cómo. 


## <a name="determining-which-notifications-were-added-and-removed"></a>Determinar qué notificaciones se agregaron y quitaron

En el `SyncNotifications` método, para determinar qué notificaciones se han agregado o quitado (sincronizando las notificaciones con el portátil), tiene que calcular la diferencia entre la colección de notificaciones actual y las notificaciones de la plataforma.

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
 
    // Otherwise it's a new notification
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


## <a name="foreground-event-for-notification-addeddismissed"></a>Evento de primer plano para la notificación agregada o descartada

> [!IMPORTANT] 
> Problema conocido: en las compilaciones anteriores a la compilación 17763/octubre 2018 actualización/versión 1809, el evento de primer plano producirá un bucle de CPU o no funcionó. Si necesita compatibilidad con esas compilaciones anteriores, utilice en su lugar la tarea en segundo plano.

También puede escuchar notificaciones de un controlador de eventos en memoria...

```csharp
// Subscribe to foreground event
listener.NotificationChanged += Listener_NotificationChanged;
 
private void Listener_NotificationChanged(UserNotificationListener sender, UserNotificationChangedEventArgs args)
{
    // Your code for handling the notification
}
```


## <a name="howto-fixdelays-in-the-background-task"></a>Cómo corregir retrasos en la tarea en segundo plano

Al probar la aplicación, es posible que observe que la tarea en segundo plano se retrasa en ocasiones y no se desencadena durante varios minutos. Para corregir el retraso, pida al usuario que vaya a la configuración del sistema: > batería del > del sistema > uso de la batería por la aplicación, busque la aplicación en la lista, selecciónela y establézcala en "siempre permitido en segundo plano".Después, la tarea en segundo plano debe desencadenarse siempre en torno a un segundo de la notificación recibida.