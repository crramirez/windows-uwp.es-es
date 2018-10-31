---
author: PatrickFarley
Description: Follow these best practices for geofencing in your app.
title: Directrices para usar geovallas en aplicaciones
ms.assetid: F817FA55-325F-4302-81BE-37E6C7ADC281
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, mapa, map, ubicación, location, geovalla, geofencing
ms.localizationpriority: medium
ms.openlocfilehash: 86104f00ed0189290fd0cd718042573d9d592cc3
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5822165"
---
# <a name="guidelines-for-geofencing-apps"></a>Directrices para usar geovallas en aplicaciones




**API importantes**

-   [**Clase Geofence (XAML)**](https://msdn.microsoft.com/library/windows/apps/dn263587)
-   [**Clase Geolocator (XAML)**](https://msdn.microsoft.com/library/windows/apps/br225534)

Sigue estos procedimientos recomendados para el uso de [**geovallas**](https://msdn.microsoft.com/library/windows/apps/dn263744) en una aplicación.

## <a name="recommendations"></a>Recomendaciones


-   Si la aplicación necesitará acceso a Internet cuando se produzca un evento [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587), comprueba si hay acceso a Internet antes de crear la geovalla.
    -   Si la aplicación no tiene acceso a Internet, puedes pedir al usuario que se conecte a Internet antes de configurar la geovalla.
    -   Si no es posible el acceso a Internet, evita el consumo de energía necesario para las comprobaciones de la ubicación de geovalla.
-   Asegúrate de que las notificaciones de geovalla sean relevantes, mediante la comprobación de la marca de tiempo y la ubicación actual cuando un evento de geovalla indique que se ha cambiado al estado [**Entered**](https://msdn.microsoft.com/library/windows/apps/dn263660) o al estado **Exited**. Consulta **Comprobar la marca de tiempo y la ubicación actual** para más información.
(#timestamp) abajo para más información.
-   Crea excepciones para administrar los casos en los que un dispositivo no pueda acceder a la información local y notifica al usuario si fuese necesario. Puede que no esté disponible la información de ubicación porque los permisos estén desactivados, el dispositivo no disponga de un adaptador GPS, la señal GPS esté bloqueada o la señal Wi-Fi no sea lo suficientemente intensa.
-   En general, no es necesario escuchar los eventos de geovalla en primer y segundo plano al mismo tiempo. Sin embargo, si la aplicación debe escuchar eventos de geovalla tanto en primer como en segundo plano:

    -   Llama al método [**ReadReports**](https://msdn.microsoft.com/library/windows/apps/dn263633) para averiguar si se ha producido un evento.
    -   Anula el registro del agente de escucha de eventos en primer plano cuando la aplicación no esté visible para el usuario y vuelve a registrarlo cuando se haga visible de nuevo.

    Consulta [Agentes de escucha en primer plano y en segundo plano](#background-and-foreground-listeners), donde podrás ver ejemplos de código y más información.

-   No uses más de 1.000 geovallas por aplicación. El sistema admite miles de geovallas por aplicación, pero para mantener un buen rendimiento de la aplicación y reducir su uso de memoria, no uses más de 1000.
-   No crees una geovalla con un radio inferior a 50 metros. Si tu aplicación debe usar una geovalla con un radio pequeño, aconseja a los usuarios que usen la aplicación en un dispositivo con radio GPS para garantizar el mejor rendimiento.

## <a name="additional-usage-guidance"></a>Instrucciones de uso adicionales

### <a name="checking-the-time-stamp-and-current-location"></a>Comprobar la marca de tiempo y la ubicación actual

Cuando un evento indica un cambio a un estado [**Entered**](https://msdn.microsoft.com/library/windows/apps/dn263660) o **Exited**, comprueba la marca de tiempo del evento y tu ubicación actual. Varios factores, como que el sistema no disponga de recursos suficiente para iniciar una tarea en segundo plano, que el usuario no vea la notificación o que el dispositivo esté en modo de espera (en Windows), pueden afectar al momento en que el usuario realmente procesa el evento. Por ejemplo, se podría producir la siguiente secuencia:

-   La aplicación crea una geovalla y la supervisa para comprobar si se producen eventos enter y exit.
-   El usuario mueve el dispositivo dentro de la geovalla, lo que provoca que se inicie un evento enter.
-   La aplicación envía una notificación al usuario indicando que está dentro de la geovalla.
-   El usuario estaba ocupado y no ve la notificación hasta 10 minutos más tarde.
-   Durante ese retraso de 10 minutos, el usuario ha salido de la geovalla.

Gracias a la marca de tiempo, puedes saber que la acción ocurrió en el pasado. Gracias a la ubicación actual, puedes ver que el usuario ahora está fuera de la geovalla. Según la funcionalidad de tu aplicación, quizás quieras filtrar este evento.

### <a name="background-and-foreground-listeners"></a>Agentes de escucha en primer plano y en segundo plano

Por lo general, una aplicación no necesita escuchar los eventos [**Geofence**](https://msdn.microsoft.com/library/windows/apps/dn263587) en una tarea en primer plano y en segundo plano al mismo tiempo. El método más limpio para tratar un caso en que pudieras necesitarlo es dejar que la tarea en segundo plano administre las notificaciones. Si configuras agentes de escucha de geovalla en primer plano y en segundo plano, no estarás seguro de cuál se iniciará primero y, por lo tanto, debes llamar siempre al método [**ReadReports**](https://msdn.microsoft.com/library/windows/apps/dn263633) para averiguar si se ha producido un evento.

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

Aunque el GPS puede proporcionar la información de ubicación más precisa, las geovallas también pueden usar Wi-Fi y otros sensores de ubicación para determinar la posición actual del usuario. Sin embargo, estos otros métodos pueden afectar al tamaño de las geovallas que puedes crear. Si el nivel de precisión es bajo, no será de utilidad crear geovallas pequeñas. En general, se recomienda no crear una geovalla con un radio inferior a 50 metros. Además, las tareas en segundo plano de geovallas solo se ejecutan periódicamente en Windows; si utilizas una geovalla pequeña, existe la posibilidad de que se te pase por alto un evento [**Enter**](https://msdn.microsoft.com/library/windows/apps/dn263660) o **Exit** por completo.

Si tu aplicación debe usar una geovalla con un radio pequeño, aconseja a los usuarios que usen la aplicación en un dispositivo con radio GPS para garantizar el mejor rendimiento.

## <a name="related-topics"></a>Temas relacionados


* [Configurar una geovalla](https://msdn.microsoft.com/library/windows/apps/mt219702)
* [Obtener la ubicación actual](https://msdn.microsoft.com/library/windows/apps/mt219698)
* [Ejemplo de ubicación de UWP (geolocation)](http://go.microsoft.com/fwlink/p/?linkid=533278)
 

 
