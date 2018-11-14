---
author: PatrickFarley
Description: Learn how to use the powerful Visits Tracking feature for more practical location tracking.
title: Directrices para usar Visits tracking
ms.assetid: 0c101684-48a9-4592-9ed5-6c20f3b830f2
ms.author: pafarley
ms.date: 05/18/2017
ms.topic: article
keywords: windows 10, uwp, mapa, ubicación, geovisit, geovisits
ms.localizationpriority: medium
ms.openlocfilehash: 3a78b2689a10dff50696e5e65cc44f79a6f1ea7d
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "6142871"
---
# <a name="guidelines-for-using-visits-tracking"></a>Directrices para usar Visits tracking

La característica Visitas simplifica el proceso de seguimiento de ubicación para que sea más eficiente para los efectos prácticos de muchas aplicaciones. Una visita se define como un área geográfica importante en la que el usuario entra y sale. Las visitas son similares a [geovallas](guidelines-for-geofencing.md) en el sentido de que permiten a la aplicación recibir una notificación solamente cuando el usuario entra o sale de determinadas áreas de interés, eliminando la necesidad de seguimiento de ubicación continuo, lo que puede hacer que disminuya la duración de la batería. Sin embargo, a diferencia de las geovallas, las áreas de visitas se identifican de forma dinámica en el nivel de la plataforma y no deben definirse explícitamente por aplicaciones individuales. Además, la selección de las visitas de las que una aplicación realizará el seguimiento se controla mediante la definición de granularidad única, en lugar de suscribirse a lugares individuales.

## <a name="preliminary-setup"></a>Configuración preliminar

Antes de ir más allá, asegúrate de que tu aplicación puede tener acceso a la ubicación del dispositivo. Tendrás que declarar la funcionalidad `Location` en el manifiesto y llamar al método **[Geolocator.RequestAccessAsync](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geolocator.RequestAccessAsync)** para asegurarte de que los usuarios dan permisos a la ubicación de la aplicación. Consulta [Obtener la ubicación del usuario](get-location.md) para obtener más información sobre cómo hacerlo. 

Recuerda agregar el espacio de nombres `Geolocation` a la clase. Esto será necesario para que todos los fragmentos de código de esta guía funcionen.

```csharp
using Windows.Devices.Geolocation;
```

## <a name="check-the-latest-visit"></a>Comprobar la visita más reciente
La manera más sencilla de usar la característica Visits tracking es recuperar el último cambio de estado conocido relacionados con la visita. Un cambio de estado es un evento registrado por plataforma en el que el usuario entra o sale de una ubicación de significación, hay un movimiento importante desde el último informe o se pierde la ubicación del usuario (consulta la enumeración **[VisitStateChange](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.visitstatechange)**). Los cambios de estado se representan mediante instancias **[Geovisit](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisit)**. Para recuperar la instancia de **Geovisit** para el último cambio de estado registrado, solo tienes que usar el método designado en la clase **[GeovisitMonitor](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geovisitmonitor)**.

> [!NOTE]
> La comprobación de la última visita registrada no garantiza que se esté realizando un seguimiento actualmente de las visitas por parte del sistema. Para realizar un seguimiento de las visitas conforme se producen, debes supervisarlas en primer plano o registrarlas para realizar el seguimiento en segundo plano (consulta las secciones siguientes).

```csharp
private async void GetLatestStateChange() {
    // retrieve the Geovisit instance
    Geovisit latestVisit = await GeovisitMonitor.GetLastReportAsync();

    // Using the properties of "latestVisit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```

### <a name="parse-a-geovisit-instance-optional"></a>Analizar una instancia de Geovisit (opcional)
El siguiente método convierte toda la información almacenada en una instancia de **Geovisit** en una cadena fácilmente legible. Se puede usar en cualquiera de los escenarios de esta guía para ayudar a proporcionar comentarios para las visitas que se notifican.

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

## <a name="monitor-visits-in-the-foreground"></a>Supervisar las visitas en primer plano

La clase **GeovisitMonitor** usada en la sección anterior también controla el escenario de escucha de los cambios de estado durante un período de tiempo. Puedes hacer esto creando una instancia de esta clase, registrando un método de controlador para su evento y llamando al método `Start`.

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

En este ejemplo, el método `OnVisitStateChanged` controlará informes de visitas entrantes. La instancia de **Geovisit** correspondiente se pasa a través del parámetro del evento.

```csharp
private void OnVisitStateChanged(GeoVisitWatcher sender, GeoVisitStateChangedEventArgs args) {
    Geovisit visit = args.Visit;
    
    // Using the properties of "visit", parse out the time that the state 
    // change was recorded, the device's location when the change was recorded,
    // and the type of state change.
}
```
Cuando la aplicación termina la supervisión de los cambios de estado relacionados con la visita, deberá detener el monitor y anular el registro de los controladores de eventos. Esto también debe realizarse siempre que la aplicación se suspenda o se cierre.

```csharp
public void UnregisterFromVisits() {
    
    // Stop the monitor to stop tracking Visits. Otherwise, tracking will
    // continue until the monitor instance is destroyed.
    monitor.Stop();
    
    // Remove the handler to stop receiving state change events.
    monitor.VisitStateChanged -= OnVisitStateChanged;
}
```

## <a name="monitor-visits-in-the-background"></a>Supervisar las visitas en segundo plano

También puedes implementar la supervisión de visitas en una tarea en segundo plano, para que la actividad relacionada con la visita pueda controlarse en el dispositivo, incluso cuando la aplicación no esté abierta. Este es el método recomendado, ya que es más versátil y de mayor eficiencia energética. 

Esta guía usará el modelo de [Crear y registrar una tarea en segundo plano fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task), en el que los archivos de la aplicación principal se encuentran en un proyecto y el archivo de tarea en segundo plano se encuentra en un proyecto independiente en la misma solución. Si no estás familiarizado con la implementación de tareas en segundo plano, se recomienda que sigas esa orientación principalmente, realizando las sustituciones necesarias para crear una tarea en segundo plano de control de visitas.

> [!NOTE]
> En los siguientes fragmentos, falta alguna funcionalidad importante como el control de errores y el almacenamiento local para lograr una mayor sencillez. Para una implementación sólida del control de visitas en segundo plano, consulta la [aplicación de muestra](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation).


En primer lugar, asegúrate de que la aplicación ha declarado permisos de tareas en segundo plano. En el elemento `Application/Extensions` de tu archivo *Package.appxmanifest*, agrega la siguiente extensión (agregar un elemento `Extensions` si no existe ya uno).

```xml
<Extension Category="windows.backgroundTasks" EntryPoint="Tasks.VisitBackgroundTask">
    <BackgroundTasks>
        <Task Type="location" />
    </BackgroundTasks>
</Extension>
```

A continuación, en la definición de la clase de tarea en segundo plano, pega el siguiente código. El método `Run` de esta tarea en segundo plano simplemente pasará los detalles del desencadenador (que contienen la información de visitas) a un método independiente.

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

Define el método `GetVisitReports` en alguna parte de esta misma clase.

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

A continuación, en el proyecto principal de la aplicación, deberás realizar el registro de esta tarea en segundo plano. Crea un método de registro al que se pueda llamar por alguna acción del usuario o que se llame cada vez que se active la clase.

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

Esto establece que una clase de tarea en segundo plano denominada `VisitBackgroundTask` en el espacio de nombres `Tasks` hará algo con el tipo de desencadenador `location`. 

Tu aplicación ahora debe ser capaz de registrar la tarea en segundo plano de control de visitas y esta tarea debe activarse siempre que el dispositivo registre un cambio de estado relacionado con la visita. Deberás rellenar la lógica de la clase de tarea en segundo plano para determinar qué hacer con esta información de cambio de estado.

## <a name="related-topics"></a>Temas relacionados
* [Crear y registrar una tarea en segundo plano fuera de proceso](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
* [Obtener la ubicación del usuario](get-location.md)
* [Espacio de nombres Windows.Devices.Geolocation](https://docs.microsoft.com/uwp/api/windows.devices.geolocation)