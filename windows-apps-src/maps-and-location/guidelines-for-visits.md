---
Description: Aprende a usar la eficaz característica Visits Tracking para realizar cómodamente el seguimiento de la ubicación.
title: Directrices para usar Visits tracking
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.date: 05/18/2017
ms.topic: article
keywords: Windows 10, UWP, mapa, ubicación, webVisit, webvisits
ms.localizationpriority: medium
ms.openlocfilehash: 3b1766d0f883fa42b005908dcc63102e97ff0d4f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162519"
---
# <a name="guidelines-for-using-visits-tracking"></a>Directrices para usar Visits tracking

La característica visitas optimiza el proceso de seguimiento de la ubicación para que sea más eficaz para los propósitos prácticos de muchas aplicaciones. Una visita se define como un área geográfica significativa que el usuario entra y sale. Las visitas son similares a las [geovallas](guidelines-for-geofencing.md) , ya que permiten que la aplicación reciba una notificación solo cuando el usuario entra o sale de determinadas áreas de interés, lo que elimina la necesidad de realizar un seguimiento continuo de la ubicación, lo que puede ser un consumo de batería. Sin embargo, a diferencia de las geovallas, las áreas de visita se identifican dinámicamente en el nivel de plataforma y no es necesario definirlas explícitamente en aplicaciones individuales. Además, la selección de las visitas a las que se realizará el seguimiento de una aplicación se controla mediante una configuración de granularidad única, en lugar de suscribirse a lugares individuales.

## <a name="preliminary-setup"></a>Configuración preliminar

Antes de continuar, asegúrese de que la aplicación pueda acceder a la ubicación del dispositivo. Tendrá que declarar la `Location` funcionalidad en el manifiesto y llamar al método **[geolocator. RequestAccessAsync](/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)** para asegurarse de que los usuarios proporcionen los permisos de ubicación de la aplicación. Vea [obtener la ubicación del usuario](get-location.md) para obtener más información sobre cómo hacerlo. 

No olvide agregar el `Geolocation` espacio de nombres a la clase. Esto será necesario para que todos los fragmentos de código de esta guía funcionen.

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>Comprobar la última visita
La manera más sencilla de usar la característica de seguimiento de visitas es recuperar el último cambio conocido del estado relacionado con la visita. Un cambio de estado es un evento de registro de plataforma en el que el usuario especifica o sale de una ubicación de importancia, hay un movimiento significativo desde el último informe o se pierde la ubicación del usuario (vea la enumeración **[VisitStateChange](/uwp/api/windows.devices.geolocation.visitstatechange)** ). Los cambios de estado se representan mediante instancias de **[webVisit](/uwp/api/windows.devices.geolocation.geovisit)** . Para recuperar la instancia de **webVisit** para el último cambio de estado registrado, simplemente use el método designado en la clase **[GeovisitMonitor](/uwp/api/windows.devices.geolocation.geovisitmonitor)** .

> [!NOTE]
> La comprobación de la última visita registrada no garantiza que el sistema realice actualmente visitas. Para realizar el seguimiento de las visitas a medida que se producen, debe supervisarlas en primer plano o registrarse para el seguimiento en segundo plano (consulte las secciones siguientes).

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>Analizar una instancia de webVisit (opcional)
El método siguiente convierte toda la información almacenada en una instancia de **webVisit** en una cadena fácilmente legible. Se puede usar en cualquiera de los escenarios de esta guía para proporcionar comentarios sobre las visitas que se están inindicando.

```csharp
private string ParseGeovisit(Geovisit visit){
    string visitString = null;

    // Use a DateTimeFormatter object to process the timestamp. The following
    // format configuration will give an intuitive representation of the date/time
    Windows.Globalization.DateTimeFormatting.DateTimeFormatter formatterLongTime;
    
    formatterLongTime = new Windows.Globalization.DateTimeFormatting.DateTimeFormatter(
        "{hour.integer}:{minute.integer(2)}:{second.integer(2)}", new[] { "en-US" }, "US", 
        Windows.Globalization.CalendarIdentifiers.Gregorian, 
        Windows.Globalization.ClockIdentifiers.TwentyFourHour);
    
    // use this formatter to convert the timestamp to a string, and add to "visitString"
    visitString = formatterLongTime.Format(visit.Timestamp);

    // Next, add on the state change type value
    visitString += " " + visit.StateChange.ToString();

    // Next, add the position information (if any is provided; this will be null if 
    // the reported event was "TrackingLost")
    if (visit.Position != null) {
        visitString += " (" +
        visit.Position.Coordinate.Point.Position.Latitude.ToString() + "," +
        visit.Position.Coordinate.Point.Position.Longitude.ToString() + 
        ")";
    }

    return visitString;
}
```

## <a name="monitor-visits-in-the-foreground"></a>Supervisar visitas en primer plano

La clase **GeovisitMonitor** utilizada en la sección anterior también controla el escenario de escucha de cambios de estado durante un período de tiempo. Para ello, puede crear una instancia de esta clase, registrar un método de controlador para su evento y llamar al `Start` método.

```csharp
// this GeovisitMonitor instance will belong to the class scope
GeovisitMonitor monitor;

public void RegisterForVisits() {

    // Create and initialize a new monitor instance.
    monitor = new GeovisitMonitor();
    
    // Attach a handler to receive state change notifications.
    monitor.VisitStateChanged += OnVisitStateChanged;
    
    // Calling the start method will start Visits tracking for a specified scope:
    // For higher granularity such as venue/building level changes, choose Venue.
    // For lower granularity in the range of zipcode level changes, choose City.
    monitor.Start(VisitMonitoringScope.Venue);
}
```

En este ejemplo, el `OnVisitStateChanged` método controlará los informes de visita entrantes. La instancia de **webVisit** correspondiente se pasa a través del parámetro de evento.

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
Cuando la aplicación haya terminado de supervisar los cambios de estado relacionados con la visita, debe detener el monitor y anular el registro de los controladores de eventos. Esto también debe hacerse siempre que la aplicación se suspenda o se cierre.

```csharp
public void UnregisterFromVisits() {
    
    // Stop the monitor to stop tracking Visits. Otherwise, tracking will
    // continue until the monitor instance is destroyed.
    monitor.Stop();
    
    // Remove the handler to stop receiving state change events.
    monitor.VisitStateChanged -= OnVisitStateChanged;
}
```

## <a name="monitor-visits-in-the-background"></a>Supervisar visitas en segundo plano

También puede implementar la supervisión de la visita en una tarea en segundo plano, por lo que la actividad relacionada con la visita puede controlarse en el dispositivo incluso cuando la aplicación no está abierta. Este es el método recomendado, ya que es más versátil y tiene una eficacia energética. 

En esta guía se usará el modelo de [creación y registro de una tarea en segundo plano fuera de proceso](../launch-resume/create-and-register-a-background-task.md), en la que los archivos de aplicación principales residen en un proyecto y el archivo de tarea en segundo plano en un proyecto independiente en la misma solución. Si no está familiarizado con la implementación de tareas en segundo plano, se recomienda que siga estas instrucciones principalmente, y que realice las sustituciones necesarias a continuación para crear una tarea en segundo plano para la administración de visitas.

> [!NOTE]
> En los siguientes fragmentos de código, algunas funciones importantes, como el control de errores y el almacenamiento local, están ausentes por motivos de simplicidad. Para una implementación sólida de la administración de visitas en segundo plano, consulte la [aplicación de ejemplo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation).


En primer lugar, asegúrese de que la aplicación ha declarado permisos de tarea en segundo plano. En el `Application/Extensions` elemento del archivo *Package. appxmanifest* , agregue la siguiente extensión (agregue un `Extensions` elemento si aún no existe ninguno).

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

Después, en la definición de la clase de tarea en segundo plano, pegue el código siguiente. El `Run` método de esta tarea en segundo plano simplemente pasará los detalles del desencadenador (que contienen la información de visita) a un método independiente.

```csharp
using Windows.ApplicationModel.Background;

namespace Tasks {
    
    public sealed class VisitBackgroundTask : IBackgroundTask {
        
        public void Run(IBackgroundTaskInstance taskInstance) {
            
            // get a deferral
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
            
            // this task's trigger will be a Geovisit trigger
            GeovisitTriggerDetails triggerDetails = taskInstance.TriggerDetails as GeovisitTriggerDetails;

            // Handle Visit reports
            GetVisitReports(triggerDetails);         

            finally {
                deferral.Complete();
            }
        }        
    }
}
```

Defina el `GetVisitReports` método en alguna parte de esta misma clase.

```csharp
private void GetVisitReports(GeovisitTriggerDetails triggerDetails) {

    // Read reports from the triggerDetails. This populates the "reports" variable 
    // with all of the Geovisit instances that have been logged since the previous
    // report reading.
    IReadOnlyList<Geovisit> reports = triggerDetails.ReadReports();

    foreach (Geovisit report in reports) {
        // Using the properties of "visit", parse out the time that the state 
        // change was recorded, the device's location when the change was recorded,
        // and the type of state change.
    }

    // Note: depending on the intent of the app, you many wish to store the
    // reports in the app's local storage so they can be retrieved the next time 
    // the app is opened in the foreground.
}
```

Después, en el proyecto principal de la aplicación, deberá realizar el registro de esta tarea en segundo plano. Cree un método de registro al que pueda llamar alguna acción del usuario o al que se llama cada vez que se activa la clase.

```csharp
// a reference to this registration should be declared at the class level
private IBackgroundTaskRegistration visitTask = null;

// The app must call this method at some point to allow future use of 
// the background task. 
private async void RegisterBackgroundTask(object sender, RoutedEventArgs e) {
    
    string taskName = "MyVisitTask";
    string taskEntryPoint = "Tasks.VisitBackgroundTask";

    // First check whether the task in question is already registered
    foreach (var task in BackgroundTaskRegistration.AllTasks) {
        if (task.Value.Name == taskName) {
            // if a task is found with the name of this app's background task, then
            // return and do not attempt to register this task
            return;
        }
    }
    
    // Attempt to register the background task.
    try {
        // Get permission for a background task from the user. If the user has 
        // already responded once, this does nothing and the user must manually 
        // update their preference via Settings.
        BackgroundAccessStatus backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();

        switch (backgroundAccessStatus) {
            case BackgroundAccessStatus.AlwaysAllowed:
            case BackgroundAccessStatus.AllowedSubjectToSystemPolicy:
                // BackgroundTask is allowed
                break;

            default:
                // notify user that background tasks are disabled for this app
                //...
                break;
        }

        // Create a new background task builder
        BackgroundTaskBuilder visitTaskBuilder = new BackgroundTaskBuilder();

        visitTaskBuilder.Name = exampleTaskName;
        visitTaskBuilder.TaskEntryPoint = taskEntryPoint;

        // Create a new Visit trigger
        var trigger = new GeovisitTrigger();

        // Set the desired monitoring scope.
        // For higher granularity such as venue/building level changes, choose Venue.
        // For lower granularity in the range of zipcode level changes, choose City. 
        trigger.MonitoringScope = VisitMonitoringScope.Venue; 

        // Associate the trigger with the background task builder
        visitTaskBuilder.SetTrigger(trigger);

        // Register the background task
        visitTask = visitTaskBuilder.Register();      
    }
    catch (Exception ex) {
        // notify user that the task failed to register, using ex.ToString()
    }
}
```

Esto establece que una clase de tarea en segundo plano llamada `VisitBackgroundTask` en el espacio de nombres `Tasks` hará algo con el `location` tipo de desencadenador. 

La aplicación ahora debe ser capaz de registrar la tarea en segundo plano de la administración de visitas y esta tarea debe activarse cada vez que el dispositivo registre un cambio de estado relacionado con la visita. Tendrá que rellenar la lógica en la clase de tarea en segundo plano para determinar qué hacer con esta información de cambio de estado.

## <a name="related-topics"></a>Temas relacionados
* [Crear y registrar una tarea en segundo plano fuera del proceso](../launch-resume/create-and-register-a-background-task.md)
* [Obtener la ubicación del usuario](get-location.md)
* [Espacio de nombres Windows. Devices. geolocation](/uwp/api/windows.devices.geolocation)