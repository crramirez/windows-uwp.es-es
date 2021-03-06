---
description: Vea instrucciones y procedimientos recomendados para usar geovallas con el fin de proporcionar experiencias contextuales geográficamente en la aplicación.
title: Directrices para usar geovallas en aplicaciones
ms.assetid: F817FA55-325F-4302-81BE-37E6C7ADC281
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, mapa, ubicación, geovallado
ms.localizationpriority: medium
ms.openlocfilehash: e4d033673acbb4a8b7fd558d9e6c4f8329d79bf5
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297715"
---
# <a name="guidelines-for-geofencing-apps"></a>Directrices para usar geovallas en aplicaciones

> [!NOTE]
> [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y MAP Services refieren una clave de autenticación de Maps denominada [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Para obtener más información sobre cómo obtener y establecer una clave de autenticación de Maps, consulte [solicitar una clave de autenticación de Maps](authentication-key.md).

**API importantes**

-   [**Clase Geofence (XAML)**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence)
-   [**Clase Geolocator (XAML)**](/uwp/api/Windows.Devices.Geolocation.Geolocator)

Sigue estos procedimientos recomendados para el uso de [**geovallas**](/uwp/api/Windows.Devices.Geolocation.Geofencing) en una aplicación.

## <a name="recommendations"></a>Recomendaciones


-   Si la aplicación necesitará acceso a Internet cuando se produzca un evento [**Geofence**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence), comprueba si hay acceso a Internet antes de crear la geovalla.
    -   Si la aplicación no tiene acceso a Internet, puedes pedir al usuario que se conecte a Internet antes de configurar la geovalla.
    -   Si no es posible el acceso a Internet, evita el consumo de energía necesario para las comprobaciones de la ubicación de geovalla.
-   Asegúrate de que las notificaciones de geovalla sean relevantes, mediante la comprobación de la marca de tiempo y la ubicación actual cuando un evento de geovalla indique que se ha cambiado al estado [**Entered**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) o al estado **Exited**. Consulta **Comprobar la marca de tiempo y la ubicación actual** para más información.
(#timestamp) a continuación para obtener más información.
-   Crea excepciones para administrar los casos en los que un dispositivo no pueda acceder a la información local y notifica al usuario si fuese necesario. Puede que no esté disponible la información de ubicación porque los permisos estén desactivados, el dispositivo no disponga de un adaptador GPS, la señal GPS esté bloqueada o la señal Wi-Fi no sea lo suficientemente intensa.
-   En general, no es necesario escuchar los eventos de geovalla en primer y segundo plano al mismo tiempo. Sin embargo, si la aplicación debe escuchar eventos de geovalla tanto en primer como en segundo plano:

    -   Llama al método [**ReadReports**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.readreports) para averiguar si se ha producido un evento.
    -   Anula el registro del agente de escucha de eventos en primer plano cuando la aplicación no esté visible para el usuario y vuelve a registrarlo cuando se haga visible de nuevo.

    Consulta [Agentes de escucha en primer plano y en segundo plano](#background-and-foreground-listeners), donde podrás ver ejemplos de código y más información.

-   No uses más de 1.000 geovallas por aplicación. El sistema admite miles de geovallas por aplicación, pero para mantener un buen rendimiento de la aplicación y reducir su uso de memoria, no uses más de 1000.
-   No crees una geovalla con un radio inferior a 50 metros. Si tu aplicación debe usar una geovalla con un radio pequeño, aconseja a los usuarios que usen la aplicación en un dispositivo con radio GPS para garantizar el mejor rendimiento.

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales

### <a name="checking-the-time-stamp-and-current-location"></a>Comprobar la marca de tiempo y la ubicación actual

Cuando un evento indica un cambio a un estado [**Entered**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) o **Exited**, comprueba la marca de tiempo del evento y tu ubicación actual. Varios factores, como que el sistema no disponga de recursos suficiente para iniciar una tarea en segundo plano, que el usuario no vea la notificación o que el dispositivo esté en modo de espera (en Windows), pueden afectar al momento en que el usuario realmente procesa el evento. Por ejemplo, se podría producir la siguiente secuencia:

-   La aplicación crea una geovalla y la supervisa para comprobar si se producen eventos enter y exit.
-   El usuario mueve el dispositivo dentro de la geovalla, lo que provoca que se inicie un evento enter.
-   La aplicación envía una notificación al usuario indicando que está dentro de la geovalla.
-   El usuario estaba ocupado y no ve la notificación hasta 10 minutos más tarde.
-   Durante ese retraso de 10 minutos, el usuario ha salido de la geovalla.

Gracias a la marca de tiempo, puedes saber que la acción ocurrió en el pasado. Gracias a la ubicación actual, puedes ver que el usuario ahora está fuera de la geovalla. Según la funcionalidad de tu aplicación, quizás quieras filtrar este evento.

### <a name="background-and-foreground-listeners"></a>Agentes de escucha en primer plano y en segundo plano

Por lo general, una aplicación no necesita escuchar los eventos [**Geofence**](/uwp/api/Windows.Devices.Geolocation.Geofencing.Geofence) en una tarea en primer plano y en segundo plano al mismo tiempo. El método más limpio para tratar un caso en que pudieras necesitarlo es dejar que la tarea en segundo plano administre las notificaciones. Si configuras agentes de escucha de geovalla en primer plano y en segundo plano, no estarás seguro de cuál se iniciará primero y, por lo tanto, debes llamar siempre al método [**ReadReports**](/uwp/api/windows.devices.geolocation.geofencing.geofencemonitor.readreports) para averiguar si se ha producido un evento.

Si has configurado agentes de escucha de geovalla en primer plano y en segundo plano, debes anular el registro del agente de escucha de eventos en primer plano cuando la aplicación no esté visible para el usuario y volver a registrarlo cuando se haga visible de nuevo. Estos son algunos ejemplos de código que realiza el registro para el evento de visibilidad.

```csharp
    Windows.UI.Core.CoreWindow coreWindow;    

    // This needs to be set before InitializeComponent sets up event registration for app visibility
    coreWindow = CoreWindow.GetForCurrentThread();
    coreWindow.VisibilityChanged += OnVisibilityChanged;
```

```javascript
 document.addEventListener("visibilitychange", onVisibilityChanged, false);
```

Cuando la visibilidad cambia, puedes habilitar o deshabilitar los controladores de eventos en primer plano tal y como se indica aquí.

```csharp
private void OnVisibilityChanged(CoreWindow sender, VisibilityChangedEventArgs args)
{
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (args.Visible)
    {
        // register for foreground events
        GeofenceMonitor.Current.GeofenceStateChanged += OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged += OnGeofenceStatusChanged;
    }
    else
    {
        // unregister foreground events (let background capture events)
        GeofenceMonitor.Current.GeofenceStateChanged -= OnGeofenceStateChanged;
        GeofenceMonitor.Current.StatusChanged -= OnGeofenceStatusChanged;
    }
}
```

```javascript
function onVisibilityChanged() {
    // NOTE: After the app is no longer visible on the screen and before the app is suspended
    // you might want your app to use toast notification for any geofence activity.
    // By registering for VisibiltyChanged the app is notified when the app is no longer visible in the foreground.

    if (document.msVisibilityState === "visible") {
        // register for foreground events
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.addEventListener("statuschanged", onGeofenceStatusChanged);
    } else {
        // unregister foreground events (let background capture events)
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("geofencestatechanged", onGeofenceStateChanged);
        Windows.Devices.Geolocation.Geofencing.GeofenceMonitor.current.removeEventListener("statuschanged", onGeofenceStatusChanged);
    }
}
```

### <a name="sizing-your-geofences"></a>Determinar el tamaño de las geovallas

Aunque el GPS puede proporcionar la información de ubicación más precisa, las geovallas también pueden usar Wi-Fi y otros sensores de ubicación para determinar la posición actual del usuario. Sin embargo, estos otros métodos pueden afectar al tamaño de las geovallas que puedes crear. Si el nivel de precisión es bajo, no será de utilidad crear geovallas pequeñas. En general, se recomienda no crear una geovalla con un radio inferior a 50 metros. Además, las tareas en segundo plano de geovallas solo se ejecutan periódicamente en Windows; si utilizas una geovalla pequeña, existe la posibilidad de que se te pase por alto un evento [**Enter**](/uwp/api/Windows.Devices.Geolocation.Geofencing.GeofenceState) o **Exit** por completo.

Si tu aplicación debe usar una geovalla con un radio pequeño, aconseja a los usuarios que usen la aplicación en un dispositivo con radio GPS para garantizar el mejor rendimiento.

## <a name="related-topics"></a>Temas relacionados


* [Configurar una geovalla](./set-up-a-geofence.md)
* [Obtener la ubicación actual](./get-location.md)
* [Ejemplo de ubicación de UWP (geolocation)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
 

 
