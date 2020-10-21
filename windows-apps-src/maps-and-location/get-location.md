---
title: Obtener la ubicación del usuario
description: Busca la ubicación del usuario y responde a los cambios de ubicación. La configuración de privacidad de la aplicación Configuración administra el acceso a la ubicación del usuario. En este tema también se muestra cómo comprobar si la aplicación tiene permisos para acceder a la ubicación del usuario.
ms.assetid: 24DC9A41-8CC1-48B0-BC6D-24BF571AFCC8
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, asignación, ubicación y capacidad de ubicación
ms.localizationpriority: medium
ms.openlocfilehash: 8a60b5003310fdba046b624e61007761ef5e0f20
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297764"
---
# <a name="get-the-users-location"></a>Obtener la ubicación del usuario

> [!NOTE]
> [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y MAP Services refieren una clave de autenticación de Maps denominada [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Para obtener más información sobre cómo obtener y establecer una clave de autenticación de Maps, consulte [solicitar una clave de autenticación de Maps](authentication-key.md).

Busca la ubicación del usuario y responde a los cambios de ubicación. La configuración de privacidad de la aplicación Configuración administra el acceso a la ubicación del usuario. En este tema también se muestra cómo comprobar si la aplicación tiene permisos para acceder a la ubicación del usuario.

**Sugerencia** Para obtener más información sobre cómo obtener acceso a la ubicación del usuario en la aplicación, descarga la muestra siguiente de [Windows-universal-samples repo (Repositorio de muestras universales de Windows)](https://github.com/Microsoft/Windows-universal-samples) que encontrarás en GitHub.

-   [Ejemplo de mapa de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="enable-the-location-capability"></a>Habilitar la funcionalidad de ubicación


1.  En el **Explorador de soluciones**, haz doble clic en **package.appxmanifest** y selecciona la pestaña **Funcionalidades**.
2.  En la lista **capacidades** , active la casilla **Ubicación**. De esta forma, se agrega la funcionalidad de dispositivo `location` al archivo de manifiesto del paquete.

```XML
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="get-the-current-location"></a>Obtener la ubicación actual


En esta sección se describe cómo detectar la ubicación geográfica del usuario mediante las API del espacio de nombres [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation).

### <a name="step-1-request-access-to-the-users-location"></a>Paso 1: Solicitar acceso a la ubicación del usuario

A menos que la aplicación tenga la funcionalidad de ubicación aproximada (vea la nota), debe solicitar acceso a la ubicación del usuario mediante el método [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) antes de intentar acceder a la ubicación. Debes llamar al método **RequestAccessAsync** desde el subproceso de la interfaz de usuario, y la aplicación debe estar en primer plano. Hasta que el usuario no conceda permiso a la aplicación, esta no podrá acceder a la información de ubicación del usuario.\*

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```



El método [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) pide permiso al usuario para acceder a su ubicación. Solo se pide confirmación al usuario una vez (por aplicación). Cuando el usuario concede o deniega el permiso por primera vez, este método ya no vuelve a solicitarlo. Para ayudar al usuario a cambiar los permisos de ubicación una vez se han solicitado, se recomienda proporcionar un vínculo a la configuración de ubicación, tal como se muestra más adelante en este tema.

>Nota: la característica de ubicación aproximada permite a la aplicación obtener una ubicación intencionadamente ofuscada (imprecisa) sin obtener el permiso explícito del usuario (sin embargo, el modificador de ubicación del sistema todavía debe estar **activado**). Para obtener información sobre cómo se usan las ubicaciones gruesas en la aplicación, vea el método [**AllowFallbackToConsentlessPositions**](/uwp/api/windows.devices.geolocation.geolocator.allowfallbacktoconsentlesspositions) en la clase [**geolocator**](/uwp/api/windows.devices.geolocation.geolocator) .

### <a name="step-2-get-the-users-location-and-register-for-changes-in-location-permissions"></a>Paso 2: Obtener la ubicación del usuario y registrar los cambios en los permisos de ubicación

El método [**GetGeopositionAsync**](/uwp/api/windows.devices.geolocation.geolocator.getgeopositionasync) realiza una lectura única de la ubicación actual. En este ejemplo se usa una instrucción **switch** con **accessStatus** (obtenido del ejemplo anterior), para que actúe solamente cuando se permita el acceso a la ubicación del usuario. Si se permite el acceso a la ubicación del usuario, el código crea un objeto [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator), registra los cambios en los permisos de ubicación y solicita la ubicación del usuario.

```csharp
switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);

        // If DesiredAccuracy or DesiredAccuracyInMeters are not set (or value is 0), DesiredAccuracy.Default is used.
        Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = _desireAccuracyInMetersValue };

        // Subscribe to the StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;

        // Carry out the operation.
        Geoposition pos = await geolocator.GetGeopositionAsync();

        UpdateLocationData(pos);
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        UpdateLocationData(null);
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecified error.", NotifyType.ErrorMessage);
        UpdateLocationData(null);
        break;
}
```

### <a name="step-3-handle-changes-in-location-permissions"></a>Paso 3: Controlar cambios en los permisos de ubicación

El objeto [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) desencadena el evento [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) para indicar que se ha modificado la configuración de ubicación del usuario. Ese evento pasa el estado correspondiente mediante la propiedad **Status** del argumento (de tipo [**PositionStatus**](/uwp/api/Windows.Devices.Geolocation.PositionStatus)). Ten en cuenta que no se llama a este método desde el subproceso de interfaz de usuario, y que el objeto [**Distribuidor**](/uwp/api/Windows.UI.Core.CoreDispatcher) invoca los cambios de la interfaz de usuario.

```csharp
using Windows.UI.Core;
...
async private void OnStatusChanged(Geolocator sender, StatusChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        // Show the location setting message only if status is disabled.
        LocationDisabledMessage.Visibility = Visibility.Collapsed;

        switch (e.Status)
        {
            case PositionStatus.Ready:
                // Location platform is providing valid data.
                ScenarioOutput_Status.Text = "Ready";
                _rootPage.NotifyUser("Location platform is ready.", NotifyType.StatusMessage);
                break;

            case PositionStatus.Initializing:
                // Location platform is attempting to acquire a fix.
                ScenarioOutput_Status.Text = "Initializing";
                _rootPage.NotifyUser("Location platform is attempting to obtain a position.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NoData:
                // Location platform could not obtain location data.
                ScenarioOutput_Status.Text = "No data";
                _rootPage.NotifyUser("Not able to determine the location.", NotifyType.ErrorMessage);
                break;

            case PositionStatus.Disabled:
                // The permission to access location data is denied by the user or other policies.
                ScenarioOutput_Status.Text = "Disabled";
                _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);

                // Show message to the user to go to location settings.
                LocationDisabledMessage.Visibility = Visibility.Visible;

                // Clear any cached location data.
                UpdateLocationData(null);
                break;

            case PositionStatus.NotInitialized:
                // The location platform is not initialized. This indicates that the application
                // has not made a request for location data.
                ScenarioOutput_Status.Text = "Not initialized";
                _rootPage.NotifyUser("No request for location is made yet.", NotifyType.StatusMessage);
                break;

            case PositionStatus.NotAvailable:
                // The location platform is not available on this version of the OS.
                ScenarioOutput_Status.Text = "Not available";
                _rootPage.NotifyUser("Location is not available on this version of the OS.", NotifyType.ErrorMessage);
                break;

            default:
                ScenarioOutput_Status.Text = "Unknown";
                _rootPage.NotifyUser(string.Empty, NotifyType.StatusMessage);
                break;
        }
    });
}
```

## <a name="respond-to-location-updates"></a>Responder a actualizaciones de ubicación


Esta sección describe cómo usar el evento [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) para recibir actualizaciones de la ubicación del usuario durante un período de tiempo. Dado que el usuario puede revocar el acceso a la ubicación en cualquier momento, es importante llamar a [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) y usar el evento [**StatusChanged**](/uwp/api/windows.devices.geolocation.geolocator.statuschanged) según se indica en la sección anterior.

En esta sección se supone que ya se ha habilitado la funcionalidad de ubicación y que has llamado a [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync) desde el subproceso de la interfaz de usuario de la aplicación en primer plano.

### <a name="step-1-define-the-report-interval-and-register-for-location-updates"></a>Paso 1: Definir el intervalo de informes y registrarse para obtener actualizaciones de ubicación

En este ejemplo, se usa una instrucción **switch** con **accessStatus** (del ejemplo anterior) para que actúe solamente cuando se permita el acceso a la ubicación del usuario. Si se permite el acceso a la ubicación del usuario, el código crea un objeto [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator), especifica el tipo de seguimiento y registra las actualizaciones de ubicación.

El objeto [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) puede desencadenar el evento [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) en función de un cambio de posición (seguimiento basado en la distancia) o un cambio en el tiempo (seguimiento periódico).

-   Para realizar el seguimiento basado en la distancia, establece la propiedad [**MovementThreshold**](/uwp/api/windows.devices.geolocation.geolocator.movementthreshold).
-   Para realizar el seguimiento periódico, establece la propiedad [**ReportInterval**](/uwp/api/windows.devices.geolocation.geolocator.reportinterval).

Si no se establece ninguna propiedad, se devuelve una posición cada segundo (equivalente a `ReportInterval = 1000`). En este caso, se usa un intervalo de informes de 2 segundos (`ReportInterval = 2000`).
```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();

switch (accessStatus)
{
    case GeolocationAccessStatus.Allowed:
        // Create Geolocator and define perodic-based tracking (2 second interval).
        _geolocator = new Geolocator { ReportInterval = 2000 };

        // Subscribe to the PositionChanged event to get location updates.
        _geolocator.PositionChanged += OnPositionChanged;

        // Subscribe to StatusChanged event to get updates of location status changes.
        _geolocator.StatusChanged += OnStatusChanged;

        _rootPage.NotifyUser("Waiting for update...", NotifyType.StatusMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        StartTrackingButton.IsEnabled = false;
        StopTrackingButton.IsEnabled = true;
        break;

    case GeolocationAccessStatus.Denied:
        _rootPage.NotifyUser("Access to location is denied.", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Visible;
        break;

    case GeolocationAccessStatus.Unspecified:
        _rootPage.NotifyUser("Unspecificed error!", NotifyType.ErrorMessage);
        LocationDisabledMessage.Visibility = Visibility.Collapsed;
        break;
}
```

### <a name="step-2-handle-location-updates"></a>Paso 2: Controlar actualizaciones de la ubicación

El objeto [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) desencadena el evento [**PositionChanged**](/uwp/api/windows.devices.geolocation.geolocator.positionchanged) para indicar que se ha modificado la ubicación del usuario o que ha pasado el tiempo, en función de cómo lo hayas configurado. Ese evento pasa la ubicación correspondiente mediante la propiedad **Position** del argumento (de tipo [**Geoposition**](/uwp/api/Windows.Devices.Geolocation.Geoposition)). En este ejemplo, no se llama al método desde el subproceso de la interfaz de usuario, y el objeto [**Dispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) invoca los cambios de la interfaz de usuario.

```csharp
using Windows.UI.Core;
...
async private void OnPositionChanged(Geolocator sender, PositionChangedEventArgs e)
{
    await Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
    {
        _rootPage.NotifyUser("Location updated.", NotifyType.StatusMessage);
        UpdateLocationData(e.Position);
    });
}
```

## <a name="change-the-location-privacy-settings"></a>Cambiar la configuración de privacidad de ubicación


Si la configuración de privacidad de la ubicación no permite que la aplicación acceda a la ubicación del usuario, se recomienda proporcionar un vínculo práctico a la **configuración de privacidad de la ubicación** en la aplicación **Configuración**. En este ejemplo, se usa un control de hipervínculo para navegar al URI `ms-settings:privacy-location`.

```xml
<!--Set Visibility to Visible when access to location is denied -->  
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

## <a name="troubleshoot-your-app"></a>Solucionar problemas de tu aplicación


Para que la aplicación pueda acceder a la ubicación del usuario, la opción **Ubicación** debe estar habilitada en el dispositivo. En la aplicación **Configuración**, comprueba que la siguiente **configuración de privacidad de ubicación** esté activada:

-   La opción **Ubicación para este dispositivo** está **activada** (no es aplicable en Windows 10 Mobile)
-   La configuración de servicios de ubicación, **"Ubicación"**, está **activada**
-   En **Elegir las aplicaciones que pueden usar tu ubicación**, la aplicación está establecida en el valor **activado**

## <a name="related-topics"></a>Temas relacionados

* [Ejemplo de ubicación geográfica de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Directrices de diseño para geovallas](./guidelines-for-geofencing.md)
* [Directrices de diseño para las aplicaciones con reconocimiento de ubicación](./guidelines-and-checklist-for-detecting-location.md)
