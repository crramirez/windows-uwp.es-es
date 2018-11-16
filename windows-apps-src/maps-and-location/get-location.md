---
author: PatrickFarley
title: Obtener la ubicación del usuario
description: Busca la ubicación del usuario y responde a los cambios de ubicación. La configuración de privacidad de la aplicación Configuración administra el acceso a la ubicación del usuario. En este tema también se muestra cómo comprobar si la aplicación tiene permisos para acceder a la ubicación del usuario.
ms.assetid: 24DC9A41-8CC1-48B0-BC6D-24BF571AFCC8
ms.author: pafarley
ms.date: 11/28/2017
ms.topic: article
keywords: windows 10, uwp, mapa, map, ubicación, location, funcionalidad de ubicación, location capability
ms.localizationpriority: medium
ms.openlocfilehash: 2187bafa9fd2b4fdce049f3ef11d4e6766613de3
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "6834486"
---
# <a name="get-the-users-location"></a>Obtener la ubicación del usuario




Busca la ubicación del usuario y responde a los cambios de ubicación. La configuración de privacidad de la aplicación Configuración administra el acceso a la ubicación del usuario. En este tema también se muestra cómo comprobar si la aplicación tiene permisos para acceder a la ubicación del usuario.

**Sugerencia** Para obtener más información sobre cómo obtener acceso a la ubicación del usuario desde la aplicación, descarga la muestra siguiente del [repositorio Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) de GitHub.

-   [Muestra de mapa en la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="enable-the-location-capability"></a>Habilitar la funcionalidad de ubicación


1.  En el **Explorador de soluciones**, haz doble clic en **package.appxmanifest** y selecciona la pestaña **Funcionalidades**.
2.  En la lista **Funcionalidades**, activa la casilla **Ubicación**. De esta forma, se agrega la funcionalidad de dispositivo `location` al archivo de manifiesto del paquete.

```XML
  <Capabilities>
    <!-- DeviceCapability elements must follow Capability elements (if present) -->
    <DeviceCapability Name="location"/>
  </Capabilities>
```

## <a name="get-the-current-location"></a>Obtener la ubicación actual


En esta sección se describe cómo detectar la ubicación geográfica del usuario mediante las API del espacio de nombres [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603).

### <a name="step-1-request-access-to-the-users-location"></a>Paso 1: Solicitar acceso a la ubicación del usuario

A menos que la aplicación tiene la funcionalidad de ubicación aproximada (ver nota), debes solicitar acceso a la ubicación del usuario mediante el método de [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) antes de intentar acceder a la ubicación. Debes llamar al método **RequestAccessAsync** desde el subproceso de la interfaz de usuario, y la aplicación debe estar en primer plano. Hasta que el usuario no conceda permiso a la aplicación, esta no podrá acceder a la información de ubicación del usuario.\*

```csharp
using Windows.Devices.Geolocation;
...
var accessStatus = await Geolocator.RequestAccessAsync();
```



El método [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) pide permiso al usuario para acceder a su ubicación. Solo se pide confirmación al usuario una vez (por aplicación). Cuando el usuario concede o deniega el permiso por primera vez, este método ya no vuelve a solicitarlo. Para ayudar al usuario a cambiar los permisos de ubicación una vez se han solicitado, se recomienda proporcionar un vínculo a la configuración de ubicación, tal como se muestra más adelante en este tema.

>Nota: La característica de ubicación aproximada permite que la aplicación obtener una ubicación deliberadamente ofuscada (imprecisa) sin obtener permiso explícito del usuario (el cambio de ubicación de todo el sistema debe seguir estando **en**, sin embargo). Para obtener información sobre cómo usar la ubicación aproximada de tu aplicación, consulta el método [**AllowFallbackToConsentlessPositions**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Geolocation.Geolocator.AllowFallbackToConsentlessPositions) en la clase [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/windows.devices.geolocation.geolocator.aspx) .

### <a name="step-2-get-the-users-location-and-register-for-changes-in-location-permissions"></a>Paso 2: Obtener la ubicación del usuario y registrar los cambios en los permisos de ubicación

El método [**GetGeopositionAsync**](https://msdn.microsoft.com/library/windows/apps/hh973536) realiza una lectura única de la ubicación actual. En este ejemplo se usa una instrucción **switch** con **accessStatus** (obtenido del ejemplo anterior), para que actúe solamente cuando se permita el acceso a la ubicación del usuario. Si se permite el acceso a la ubicación del usuario, el código crea un objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534), registra los cambios en los permisos de ubicación y solicita la ubicación del usuario.

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

El objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) desencadena el evento [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) para indicar que se ha modificado la configuración de ubicación del usuario. Ese evento pasa el estado correspondiente mediante la propiedad **Status** del argumento (de tipo [**PositionStatus**](https://msdn.microsoft.com/library/windows/apps/br225599)). Ten en cuenta que no se llama a este método desde el subproceso de interfaz de usuario, y que el objeto [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) invoca los cambios de la interfaz de usuario.

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


Esta sección describe cómo usar el evento [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) para recibir actualizaciones de la ubicación del usuario durante un período de tiempo. Dado que el usuario puede revocar el acceso a la ubicación en cualquier momento, es importante llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) y usar el evento [**StatusChanged**](https://msdn.microsoft.com/library/windows/apps/br225542) según se indica en la sección anterior.

En esta sección se supone que ya se ha habilitado la funcionalidad de ubicación y que has llamado a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn859152) desde el subproceso de la interfaz de usuario de la aplicación en primer plano.

### <a name="step-1-define-the-report-interval-and-register-for-location-updates"></a>Paso 1: Definir el intervalo de informes y registrarse para obtener actualizaciones de ubicación

En este ejemplo, se usa una instrucción **switch** con **accessStatus** (del ejemplo anterior) para que actúe solamente cuando se permita el acceso a la ubicación del usuario. Si se permite el acceso a la ubicación del usuario, el código crea un objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534), especifica el tipo de seguimiento y registra las actualizaciones de ubicación.

El objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) puede desencadenar el evento [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) en función de un cambio de posición (seguimiento basado en la distancia) o un cambio en el tiempo (seguimiento periódico).

-   Para realizar el seguimiento basado en la distancia, establece la propiedad [**MovementThreshold**](https://msdn.microsoft.com/library/windows/apps/br225539).
-   Para realizar el seguimiento periódico, establece la propiedad [**ReportInterval**](https://msdn.microsoft.com/library/windows/apps/br225541).

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

El objeto [**Geolocator**](https://msdn.microsoft.com/library/windows/apps/br225534) desencadena el evento [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/br225540) para indicar que se ha modificado la ubicación del usuario o que ha pasado el tiempo, en función de cómo lo hayas configurado. Ese evento pasa la ubicación correspondiente mediante la propiedad **Position** del argumento (de tipo [**Geoposition**](https://msdn.microsoft.com/library/windows/apps/br225543)). En este ejemplo, no se llama al método desde el subproceso de la interfaz de usuario, y el objeto [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) invoca los cambios de la interfaz de usuario.

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

Como alternativa, la aplicación puede llamar al método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para iniciar la aplicación **Configuración** desde el código. Para obtener más información, consulta [Iniciar la aplicación Configuración de Windows](https://msdn.microsoft.com/library/windows/apps/mt228342).

```csharp
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-location"));
```

## <a name="troubleshoot-your-app"></a>Solucionar problemas de tu aplicación


Para que la aplicación pueda acceder a la ubicación del usuario, la opción **Ubicación** debe estar habilitada en el dispositivo. En la aplicación **Configuración**, comprueba que la siguiente **configuración de privacidad de ubicación** esté activada:

-   **Ubicación para este dispositivo …** está **activada (no es aplicable en Windows 10 Mobile)**
-   La configuración de servicios de ubicación, **"Ubicación"**, está **activada**
-   En **Elige las aplicaciones que pueden usar tu ubicación**, la aplicación está establecida como **activada**

## <a name="related-topics"></a>Temas relacionados

* [Muestra de ubicación geográfica para UWP](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [Directrices de diseño para geovallas](https://msdn.microsoft.com/library/windows/apps/dn631756)
* [Directrices de diseño para aplicaciones con reconocimiento de ubicación](https://msdn.microsoft.com/library/windows/apps/hh465148)
