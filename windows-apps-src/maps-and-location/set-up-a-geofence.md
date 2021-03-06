---
title: Configurar una geovalla
description: Configura una geovalla en tu aplicación y aprende a administrar las notificaciones en primer y segundo plano.
ms.assetid: A3A46E03-0751-4DBD-A2A1-2323DB09BDBA
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, mapa, ubicación, geovallas, notificaciones
ms.localizationpriority: medium
ms.openlocfilehash: ca6dad1a96f37e3a308ad10c84293a8d49fb0329
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297596"
---
# <a name="set-up-a-geofence"></a>Configurar una geovalla

> [!NOTE]
> [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y MAP Services refieren una clave de autenticación de Maps denominada [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Para obtener más información sobre cómo obtener y establecer una clave de autenticación de Maps, consulte [solicitar una clave de autenticación de Maps](authentication-key.md).

Configure una [**geovalla**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) en la aplicación y obtenga información sobre cómo controlar las notificaciones en primer plano y en segundo plano.

**Sugerencia** Para obtener más información sobre el acceso a la ubicación en tu aplicación, descarga la muestra siguiente del [repositorio de muestras universales de Windows](https://github.com/Microsoft/Windows-universal-samples) en GitHub.

-   [Ejemplo de mapa de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="enable-the-location-capability"></a>Habilitar la funcionalidad de ubicación


1.  En el **Explorador de soluciones**, haz doble clic en **package.appxmanifest** y selecciona la pestaña **Funcionalidades**.
2.  En la lista **Funcionalidades**, activa **Ubicación**. De esta forma, se agrega la funcionalidad de dispositivo `Location` al archivo de manifiesto del paquete.

```xml
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="set-up-a-geofence"></a>Configurar una geovalla


### <a name="step-1-request-access-to-the-users-location"></a>Paso 1: Solicitar acceso a la ubicación del usuario

**Importante** Debes solicitar permiso para obtener acceso a la ubicación del usuario mediante el método [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) antes de intentar acceder a esta. Debes llamar al método **RequestAccessAsync** desde el subproceso de la interfaz de usuario, y la aplicación debe estar en primer plano. Hasta que el usuario no conceda permiso a la aplicación, esta no podrá acceder a la información de ubicación del usuario.

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```

El método [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) pide permiso al usuario para acceder a su ubicación. Solo se pide confirmación al usuario una vez (por aplicación). Cuando el usuario concede o deniega el permiso por primera vez, este método ya no vuelve a solicitarlo. Para ayudar al usuario a cambiar los permisos de ubicación una vez se han solicitado, se recomienda proporcionar un vínculo a la configuración de ubicación, tal como se muestra más adelante en este tema.

### <a name="step-2-register-for-changes-in-geofence-state-and-location-permissions"></a>Paso 2: Registrar cambios en los permisos de estado y ubicación de una geovalla

En este ejemplo, se usa una instrucción **switch** con **accessStatus** (del ejemplo anterior) para que actúe solamente cuando se permita el acceso a la ubicación del usuario. Si se permite el acceso a la ubicación del usuario, el código accede a las geovallas actuales, y registra los cambios de estado de las geovallas y los cambios en los permisos de ubicación.

**Sugerencia** Si usas una geovalla, supervisa los cambios en los permisos de ubicación mediante el evento [**StatusChanged**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.statuschanged) de la clase GeofenceMonitor, en lugar del evento StatusChanged de la clase Geolocator. Un [**GeofenceMonitorStatus**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitorStatus) de **Deshabilitado** es equivalente a un [**PositionStatus**](/uwp/api/Windows.Devices.Geolocation.PositionStatus) deshabilitado (ambos indican que la aplicación no tiene permiso para acceder a la ubicación del usuario).

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        geofences = GeofenceMonitor.Current.Geofences;

        FillRegisteredGeofenceListBoxWithExistingGeofences();
        FillEventListBoxWithExistingEvents();

        // Register for state change events.
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
        break;


    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access denied.", NotifyType.ErrorMessage);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        break;
}
```

Luego, cuando navegues fuera de la aplicación en primer plano, anula el registro de las escuchas de eventos.

```csharp
protected override void OnNavigatingFrom(NavigatingCancelEventArgs e)
{
    GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
    GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;

    base.OnNavigatingFrom(e);
}
```

### <a name="step-3-create-the-geofence"></a>Paso 3: Crear la geovalla

Ahora ya estás listo para definir y configurar un objeto [**Geovalla**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence). Puedes elegir entre diferentes sobrecargas de constructor según tus necesidades. En el constructor de geovalla más básico, especifica únicamente [**Id**](/uwp/api/windows.devices.geolocation.geofencing.geofence.id) y [**Geoshape**](/uwp/api/windows.devices.geolocation.geofencing.geofence.geoshape) como se muestra aquí.

```csharp
// Set the fence ID.
string fenceId = "fence1";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set a circular region for the geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle);

```

Puedes ajustar aún más la geovalla mediante uno de los otros constructores. En el siguiente ejemplo, el constructor de geovalla especifica estos parámetros adicionales:

-   [**MonitoredStates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates): indica de qué eventos de geovalla quieres recibir notificaciones para entrar en la región definida, salir de la región definida o eliminar la geovalla.
-   [**SingleUse**](/uwp/api/windows.devices.geolocation.geofencing.geofence.singleuse): quita la geovalla cuando se alcanzan todos los estados de la geovalla que se está supervisando.
-   [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime): indica cuánto tiempo debe estar el usuario dentro o fuera del área definida antes de desencadenar eventos de entrada o salida.
-   [**StartTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.starttime): indica cuándo comenzar a supervisar la geovalla.
-   [**Duration**](/uwp/api/windows.devices.geolocation.geofencing.geofence.duration): indica el período durante el cual se va a supervisar la geovalla.

```csharp
// Set the fence ID.
string fenceId = "fence2";

// Define the fence location and radius.
BasicGeoposition position;
position.Latitude = 47.6510;
position.Longitude = -122.3473;
position.Altitude = 0.0;
double radius = 10; // in meters

// Set the circular region for geofence.
Geocircle geocircle = new Geocircle(position, radius);

// Remove the geofence after the first trigger.
bool singleUse = true;

// Set the monitored states.
MonitoredGeofenceStates monitoredStates =
                MonitoredGeofenceStates.Entered |
                MonitoredGeofenceStates.Exited |
                MonitoredGeofenceStates.Removed;

// Set how long you need to be in geofence for the enter event to fire.
TimeSpan dwellTime = TimeSpan.FromMinutes(5);

// Set how long the geofence should be active.
TimeSpan duration = TimeSpan.FromDays(1);

// Set up the start time of the geofence.
DateTimeOffset startTime = DateTime.Now;

// Create the geofence.
Geofence geofence = new Geofence(fenceId, geocircle, monitoredStates, singleUse, dwellTime, startTime, duration);
```

Después de crearlo, Recuerde registrar la nueva [**geovalla**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) en el monitor.

```csharp
// Register the geofence
try {
   GeofenceMonitor.Current.Geofences.Add(geofence);
} catch {
   // Handle failure to add geofence
}
```

### <a name="step-4-handle-changes-in-location-permissions"></a>Paso 4: Controlar cambios en los permisos de ubicación

El objeto [**GeofenceMonitor**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitor) desencadena el evento [**StatusChanged**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.statuschanged) para indicar que se ha modificado la configuración de ubicación del usuario. Ese evento pasa el estado correspondiente mediante la propiedad **sender.Status** del argumento (de tipo [**GeofenceMonitorStatus**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceMonitorStatus)). Ten en cuenta que no se llama a este método desde el subproceso de interfaz de usuario, y que el objeto [**Distribuidor**](/uwp/api/Windows.UI.Core.CoreDispatcher) invoca los cambios de la interfaz de usuario.

```csharp
using Windows.UI.Core;
...
public async void OnGeofenceStatusChanged(GeofenceMonitor sender, object e)
{
   await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
   {
    // Show the location setting message only if the status is disabled.
    LocationDisabledMessage.Visibility = Visibility.Collapsed;

    switch (sender.Status)
    {
     case GeofenceMonitorStatus.Ready:
      _rootPage.NotifyUser("The monitor is ready and active.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.Initializing:
      _rootPage.NotifyUser("The monitor is in the process of initializing.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NoData:
      _rootPage.NotifyUser("There is no data on the status of the monitor.", NotifyType.ErrorMessage);
      break;

     case GeofenceMonitorStatus.Disabled:
      _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

      // Show the message to the user to go to the location settings.
      LocationDisabledMessage.Visibility = Visibility.Visible;
      break;

     case GeofenceMonitorStatus.NotInitialized:
      _rootPage.NotifyUser("The geofence monitor has not been initialized.", NotifyType.StatusMessage);
      break;

     case GeofenceMonitorStatus.NotAvailable:
      _rootPage.NotifyUser("The geofence monitor is not available.", NotifyType.ErrorMessage);
      break;

     default:
      ScenarioOutput_Status.Text = "Unknown";
      _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
      break;
    }
   });
}
```

## <a name="set-up-foreground-notifications"></a>Configurar notificaciones en primer plano


Una vez creadas las geovallas, tendrás que agregar la lógica para controlar lo que sucede cuando se produce un evento de geovalla. Dependiendo de los [**MonitoredStates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) que hayas configurado, es posible que recibas un evento cuando:

-   El usuario entre en una región de interés.
-   El usuario salga de una región de interés.
-   La geovalla expire o se elimine. Ten en cuenta que una aplicación en segundo plano no se activa en caso de evento de eliminación.

Puedes escuchar los eventos directamente desde tu aplicación cuando se está ejecutando, o bien registrarte en una tarea en segundo plano para recibir notificaciones en segundo plano cuando se produzca un evento.

### <a name="step-1-register-for-geofence-state-change-events"></a>Paso 1: Registrarse para eventos de cambio de estado de geovalla

Para que tu aplicación reciba una notificación en primer plano del cambio en el estado de una geovalla, debes registrar un controlador de eventos. Normalmente esto se configura al crear la geovalla.

```csharp
private void Initialize()
{
    // Other initialization logic

    GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
}

```

### <a name="step-2-implement-the-geofence-event-handler"></a>Paso 2: Implementar el controlador de eventos de geovalla

El paso siguiente es implementar los controladores de eventos. La acción que se siga aquí dependerá de la finalidad para la que tu aplicación use la geovalla.

```csharp
public async void OnGeofenceStateChanged(GeofenceMonitor sender, object e)
{
    var reports = sender.ReadReports();

    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        foreach (GeofenceStateChangeReport report in reports)
        {
            GeofenceState state = report.NewState;

            Geofence geofence = report.Geofence;

            if (state == GeofenceState.Removed)
            {
                // Remove the geofence from the geofences collection.
                GeofenceMonitor.Current.Geofences.Remove(geofence);
            }
            else if (state == GeofenceState.Entered)
            {
                // Your app takes action based on the entered event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
            else if (state == GeofenceState.Exited)
            {
                // Your app takes action based on the exited event.

                // NOTE: You might want to write your app to take a particular
                // action based on whether the app has internet connectivity.

            }
        }
    });
}



```

## <a name="set-up-background-notifications"></a>Configurar notificaciones en segundo plano


Una vez creadas las geovallas, tendrás que agregar la lógica para controlar lo que sucede cuando se produce un evento de geovalla. Dependiendo de los [**MonitoredStates**](/uwp/api/windows.devices.geolocation.geofencing.geofence.monitoredstates) que hayas configurado, es posible que recibas un evento cuando:

-   El usuario entre en una región de interés.
-   El usuario salga de una región de interés.
-   La geovalla expire o se elimine. Ten en cuenta que una aplicación en segundo plano no se activa en caso de evento de eliminación.

Para escuchar un evento de geovalla en segundo plano

-   Declara la tarea en segundo plano en el manifiesto de la aplicación.
-   Registra la tarea en segundo plano en tu aplicación. Si la aplicación necesita acceso a Internet, por ejemplo, para acceder a un servicio de nube, puedes establecer una marca para ello cuando se desencadene el evento. También puedes establecer una marca para asegurarte de que el usuario está presente cuando el evento se desencadena con el fin de garantizar que el usuario recibirá la notificación.
-   Mientras tu aplicación se ejecuta en primer plano, pide al usuario que conceda permisos de ubicación a tu aplicación.

### <a name="step-1-register-for-geofence-state-change-events"></a>Paso 1: Registrarse para eventos de cambio de estado de geovalla

En el manifiesto de la aplicación, en la pestaña **Declaraciones**, agrega una declaración para una tarea de ubicación en segundo plano. Para ello, siga estos pasos:

-   Agrega una declaración del tipo **Tareas en segundo plano**.
-   Establezca un tipo de tarea de propiedad de **Ubicación**.
-   Establece un punto de entrada en tu aplicación al que se llamará cuando se genere el evento.

### <a name="step-2-register-the-background-task"></a>Paso 2: Registrar la tarea en segundo plano

El código de este paso registra la tarea de geovalla en segundo plano. Recuerde que cuando se creó la geovalla, hemos comprobado los permisos de ubicación.

```csharp
async private void RegisterBackgroundTask(object sender, RoutedEventArgs e)
{
    // Get permission for a background task from the user. If the user has already answered once,
    // this does nothing and the user must manually update their preference via PC Settings.
    BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

    // Regardless of the answer, register the background task. Note that the user can use
    // the Settings app to prevent your app from running background tasks.
    // Create a new background task builder.
    BackgroundTaskBuilder geofenceTaskBuilder = new BackgroundTaskBuilder();

    geofenceTaskBuilder.Name = SampleBackgroundTaskName;
    geofenceTaskBuilder.TaskEntryPoint = SampleBackgroundTaskEntryPoint;

    // Create a new location trigger.
    var trigger = new LocationTrigger(LocationTriggerType.Geofence);

    // Associate the location trigger with the background task builder.
    geofenceTaskBuilder.SetTrigger(trigger);

    // If it is important that there is user presence and/or
    // internet connection when OnCompleted is called
    // the following could be called before calling Register().
    // SystemCondition condition = new SystemCondition(SystemConditionType.UserPresent | SystemConditionType.InternetAvailable);
    // geofenceTaskBuilder.AddCondition(condition);

    // Register the background task.
    geofenceTask = geofenceTaskBuilder.Register();

    // Associate an event handler with the new background task.
    geofenceTask.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);

    BackgroundTaskState.RegisterBackgroundTask(BackgroundTaskState.LocationTriggerBackgroundTaskName);

    switch (backgroundAccessStatus)
    {
    case BackgroundAccessStatus.Unspecified:
    case BackgroundAccessStatus.Denied:
        rootPage.NotifyUser("This app is not allowed to run in the background.", NotifyType.ErrorMessage);
        break;

    }
}


```

### <a name="step-3-handling-the-background-notification"></a>Paso 3: Administrar la notificación en segundo plano

La acción que realices para notificar al usuario dependerá de lo que haga tu aplicación, pero puedes mostrar una notificación del sistema, reproducir un sonido o actualizar un icono dinámico. El código de este paso administra la notificación.

```csharp
async private void OnCompleted(IBackgroundTaskRegistration sender, BackgroundTaskCompletedEventArgs e)
{
    if (sender != null)
    {
        // Update the UI with progress reported by the background task.
        await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            try
            {
                // If the background task threw an exception, display the exception in
                // the error text box.
                e.CheckResult();

                // Update the UI with the completion status of the background task.
                // The Run method of the background task sets the LocalSettings.
                var settings = ApplicationData.Current.LocalSettings;

                // Get the status.
                if (settings.Values.ContainsKey("Status"))
                {
                    rootPage.NotifyUser(settings.Values["Status"].ToString(), NotifyType.StatusMessage);
                }

                // Do your app work here.

            }
            catch (Exception ex)
            {
                // The background task had an error.
                rootPage.NotifyUser(ex.ToString(), NotifyType.ErrorMessage);
            }
        });
    }
}


```

## <a name="change-the-privacy-settings"></a>Cambiar la configuración de privacidad


Si la configuración de privacidad de la ubicación no permite que la aplicación acceda a la ubicación del usuario, se recomienda proporcionar un vínculo práctico a la **configuración de privacidad de la ubicación** en la aplicación **Configuración**. En este ejemplo, se usa un control de hipervínculo para navegar al URI `ms-settings:privacy-location`.

```xml
<!--Set Visibility to Visible when access to the user's location is denied. -->  
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access Location. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-location">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the location privacy settings."/>
</TextBlock>
```

Como alternativa, la aplicación puede llamar al método [**LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) para iniciar la aplicación **Configuración** desde el código. Para obtener más información, consulta [Iniciar la aplicación Configuración de Windows](../launch-resume/launch-settings-app.md).

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="test-and-debug-your-app"></a>Probar y depurar la aplicación


Las tareas para probar y depurar aplicaciones de geovalla pueden resultar todo un desafío porque dependen de la ubicación del dispositivo. Aquí describimos varios métodos para probar geovallas en primer y en segundo plano.

**Para depurar una aplicación de geovalla**

1.  Mueve físicamente el dispositivo a nuevas ubicaciones.
2.  Intenta acceder a una geovalla creando una región de geovalla que incluya tu ubicación física actual, de modo que ya estés dentro de la geovalla. El evento "geofenced entered" (ingreso en geovalla) se desencadenará inmediatamente.
3.  Usa el emulador de Microsoft Visual Studio para simular ubicaciones del dispositivo.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-foreground"></a>Probar y depurar una aplicación de geovalla que se ejecuta en primer plano

**Para probar la aplicación de geovalla que se ejecuta en primer plano**

1.  Compila tu aplicación en Visual Studio.
2.  Inicia la aplicación en el emulador de Visual Studio.
3.  Usa estas herramientas para simular varias ubicaciones dentro y fuera de tu región de geovalla. Asegúrate de esperar lo suficiente una vez transcurrido el tiempo que especifica la propiedad [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) para desencadenar el evento. Ten en cuenta que debes aceptar la solicitud para habilitar permisos de ubicación para la aplicación. Para obtener más información sobre cómo simular ubicaciones, consulte [establecer la geolocalización simulada del dispositivo](/previous-versions/hh441475(v=vs.120)#BKMK_Set_the_simulated_geo_location_of_the_device).
4.  También puedes usar el emulador para calcular el tamaño de las vallas y los tiempos de permanencia aproximados que deben detectarse a velocidades diferentes.

### <a name="test-and-debug-a-geofencing-app-that-is-running-in-the-background"></a>Probar y depurar una aplicación de geovalla que se ejecuta en segundo plano

**Para probar la aplicación de geovalla que se ejecuta en segundo plano**

1.  Compila tu aplicación en Visual Studio. Ten en cuenta que la aplicación debe establecer el tipo de tarea en segundo plano de **Ubicación**.
2.  Primero, implementa la aplicación localmente.
3.  Cierra la aplicación que se está ejecutando localmente.
4.  Inicia la aplicación en el emulador de Visual Studio. Ten en cuenta que la simulación de geovallas en segundo plano solo puede hacerse en una aplicación a la vez en el emulador. No inicies varias aplicaciones de geovalla en el emulador.
5.  En el emulador, simula varias ubicaciones dentro y fuera de tu región de geovalla. Asegúrate de esperar lo suficiente una vez transcurrido el [**DwellTime**](/uwp/api/windows.devices.geolocation.geofencing.geofence.dwelltime) para desencadenar el evento. Ten en cuenta que debes aceptar la solicitud para habilitar permisos de ubicación para la aplicación.
6.  Usa Visual Studio para desencadenar la tarea de ubicación en segundo plano. Para obtener más información sobre cómo desencadenar tareas en segundo plano en Visual Studio, consulte [Cómo desencadenar tareas en segundo plano](/previous-versions/hh974425(v=vs.120)#BKMK_Trigger_background_tasks).

## <a name="troubleshoot-your-app"></a>Solucionar problemas de tu aplicación


Antes de que la aplicación pueda acceder a la ubicación, la opción **Ubicación** debe estar habilitada en el dispositivo. En la aplicación **Configuración**, comprueba que la siguiente **configuración de privacidad de ubicación** esté activada:

-   La opción **Ubicación para este dispositivo** está **activada** (no es aplicable en Windows 10 Mobile)
-   La configuración de servicios de ubicación, **"Ubicación"**, está **activada**
-   En **Elegir las aplicaciones que pueden usar tu ubicación**, la aplicación está establecida en el valor **activado**

## <a name="related-topics"></a>Temas relacionados

* [Ejemplo de ubicación geográfica de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Directrices de diseño para geovallas](./guidelines-for-geofencing.md)
* [Directrices de diseño para las aplicaciones con reconocimiento de ubicación](./guidelines-and-checklist-for-detecting-location.md)
