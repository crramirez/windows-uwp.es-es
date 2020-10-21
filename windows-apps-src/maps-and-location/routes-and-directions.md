---
title: Mostrar rutas e indicaciones en un mapa
description: Obtenga información sobre cómo recuperar rutas y direcciones mediante la clase MapRouteFinder y mostrarlas en un MapControl en una aplicación Plataforma universal de Windows (UWP).
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, ruta, mapa, ubicación, direcciones
ms.localizationpriority: medium
ms.openlocfilehash: 4171598f47d28942adb56860452a8ec49cb5e2c2
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297598"
---
# <a name="display-routes-and-directions-on-a-map"></a>Mostrar rutas e indicaciones en un mapa

> [!NOTE]
> [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y MAP Services refieren una clave de autenticación de Maps denominada [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Para obtener más información sobre cómo obtener y establecer una clave de autenticación de Maps, consulte [solicitar una clave de autenticación de Maps](authentication-key.md).

Solicita rutas e indicaciones y muéstralas en tu aplicación.

>[!Note]
>Para obtener más información sobre el uso de mapas en su aplicación, descargue el [ejemplo de mapa de plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl).
>Si la asignación no es una característica básica de la aplicación, considere la posibilidad de iniciar la aplicación Windows Maps en su lugar. Puedes usar los esquemas URI `bingmaps:`, `ms-drive-to:` y `ms-walk-to:` para iniciar la aplicación Mapas de Windows para mostrar mapas específicos e indicaciones paso a paso. Para obtener más información, consulta [Iniciar la aplicación Mapas de Windows](../launch-resume/launch-maps-app.md).

 
## <a name="an-intro-to-maproutefinder-results"></a>Introducción a los resultados de MapRouteFinder


Así es como están relacionadas las clases de rutas e indicaciones:

* La clase [**MapRouteFinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder) tiene métodos que obtienen rutas e indicaciones. Estos métodos devuelven un [**MapRouteFinderResult**](/uwp/api/Windows.Services.Maps.MapRouteFinderResult).

* La clase [**MapRouteFinderResult**](/uwp/api/Windows.Services.Maps.MapRouteFinderResult) contiene un objeto [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute). Obtenga acceso a este objeto a través de la propiedad [**Route**](/uwp/api/windows.services.maps.maproutefinderresult.route) de **MapRouteFinderResult**.

* La clase [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) contiene una colección de objetos [**MapRouteLeg**](/uwp/api/Windows.Services.Maps.MapRouteLeg). Obtenga acceso a esta colección a través de la propiedad [**piernass**](/uwp/api/windows.services.maps.maproute.legs) de **MapRoute**.

* Cada clase [**MapRouteLeg**](/uwp/api/Windows.Services.Maps.MapRouteLeg) contiene una colección de objetos [**MapRouteManeuver**](/uwp/api/Windows.Services.Maps.MapRouteManeuver). Obtenga acceso a esta colección a través de la propiedad [**maniobras**](/uwp/api/windows.services.maps.maprouteleg.maneuvers) de **MapRouteLeg**.

Obtenga una ruta de conducción o de caminar llamando a los métodos de la clase [**MapRouteFinder**](/uwp/api/Windows.Services.Maps.MapRouteFinder) . Por ejemplo, [**GetDrivingRouteAsync**](/uwp/api/windows.services.maps.maproutefinder.getdrivingrouteasync) o [**GetWalkingRouteAsync**](/uwp/api/windows.services.maps.maproutefinder.getwalkingrouteasync).

Cuando solicitas una ruta, puedes especificar lo siguiente:

* Puedes proporcionar un punto inicial y un punto final o puedes proporcionar una serie de puntos de trayecto para calcular la ruta.

    Los puntos de *interrupción de detención* agregan segmentos de ruta adicionales, cada uno con su propio itinerario. Para especificar los puntos de *interrupción de detención* , use cualquiera de las sobrecargas de [**GetDrivingRouteFromWaypointsAsync**](/uwp/api/windows.services.maps.maproutefinder.getwalkingroutefromwaypointsasync) .

    *A través* de puntos de trayecto define ubicaciones intermedias entre puntos de *interrupción de detención* . No agregan segmentos de ruta.  Son simplemente puntos de acceso que una ruta debe atravesar. Para especificar a *través* de puntos de trayecto, use cualquiera de las sobrecargas de [**GetDrivingRouteFromEnhancedWaypointsAsync**](/uwp/api/windows.services.maps.maproutefinder.getdrivingroutefromenhancedwaypointsasync) .

* Puede especificar las optimizaciones (por ejemplo, minimizar la distancia).

* Puede especificar restricciones (por ejemplo, evitar carreteras).

## <a name="display-directions"></a>Mostrar indicaciones

El objeto [**MapRouteFinderResult**](/uwp/api/Windows.Services.Maps.MapRouteFinderResult) contiene un objeto [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) al que puedes tener acceso mediante su propiedad [**Route**](/uwp/api/windows.services.maps.maproutefinderresult.route).

El objeto [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) calculado tiene propiedades que proporcionan el tiempo necesario para recorrer la ruta, la longitud de la ruta y la colección de objetos [**MapRouteLeg**](/uwp/api/Windows.Services.Maps.MapRouteLeg) que contienen los tramos de la ruta. Cada objeto **MapRouteLeg** contiene una colección de objetos [**MapRouteManeuver**](/uwp/api/Windows.Services.Maps.MapRouteManeuver). El objeto **MapRouteManeuver** contiene indicaciones a las que puedes tener acceso mediante su propiedad [**InstructionText**](/uwp/api/windows.services.maps.maproutemaneuver.instructiontext).

>[!IMPORTANT]
>Debe especificar una clave de autenticación de Maps para poder usar Map Services. Para obtener más información, consulta [Solicitar una clave de autenticación de mapas](authentication-key.md).

 

```csharp
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void button_Click(object sender, RoutedEventArgs e)
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() {Latitude=47.643,Longitude=-122.131};

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() {Latitude = 47.604,Longitude= -122.329};

   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      System.Text.StringBuilder routeInfo = new System.Text.StringBuilder();

      // Display summary info about the route.
      routeInfo.Append("Total estimated time (minutes) = ");
      routeInfo.Append(routeResult.Route.EstimatedDuration.TotalMinutes.ToString());
      routeInfo.Append("\nTotal length (kilometers) = ");
      routeInfo.Append((routeResult.Route.LengthInMeters / 1000).ToString());

      // Display the directions.
      routeInfo.Append("\n\nDIRECTIONS\n");

      foreach (MapRouteLeg leg in routeResult.Route.Legs)
      {
         foreach (MapRouteManeuver maneuver in leg.Maneuvers)
         {
            routeInfo.AppendLine(maneuver.InstructionText);
         }
      }

      // Load the text box.
      tbOutputText.Text = routeInfo.ToString();
   }
   else
   {
      tbOutputText.Text =
            "A problem occurred: " + routeResult.Status.ToString();
   }
}
```

En este ejemplo se muestran los siguientes resultados en el cuadro de texto `tbOutputText`.

``` syntax
Total estimated time (minutes) = 18.4833333333333
Total length (kilometers) = 21.847

DIRECTIONS
Head north on 157th Ave NE.
Turn left onto 159th Ave NE.
Turn left onto NE 40th St.
Turn left onto WA-520 W.
Enter the freeway WA-520 from the right.
Keep left onto I-5 S/Portland.
Keep right and leave the freeway at exit 165A towards James St..
Turn right onto James St.
You have reached your destination.
```

## <a name="display-routes"></a>Mostrar rutas


Para mostrar un objeto [**MapRoute**](/uwp/api/Windows.Services.Maps.MapRoute) en un [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl), construye una [**MapRouteView**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapRouteView) con el objeto **MapRoute**. A continuación, agregue **MapRouteView** a la colección [**Routes**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.routes) de la **clase MapControl**.

>[!IMPORTANT]
>Debe especificar una clave de autenticación de Maps para poder usar los servicios de mapa o el control de mapa. Para obtener más información, consulta [Solicitar una clave de autenticación de mapas](authentication-key.md).

 

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
   // Start at Microsoft in Redmond, Washington.
   BasicGeoposition startLocation = new BasicGeoposition() { Latitude = 47.643, Longitude = -122.131 };

   // End at the city of Seattle, Washington.
   BasicGeoposition endLocation = new BasicGeoposition() { Latitude = 47.604, Longitude = -122.329 };


   // Get the route between the points.
   MapRouteFinderResult routeResult =
         await MapRouteFinder.GetDrivingRouteAsync(
         new Geopoint(startLocation),
         new Geopoint(endLocation),
         MapRouteOptimization.Time,
         MapRouteRestrictions.None);

   if (routeResult.Status == MapRouteFinderStatus.Success)
   {
      // Use the route to initialize a MapRouteView.
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      // Add the new MapRouteView to the Routes collection
      // of the MapControl.
      MapWithRoute.Routes.Add(viewOfRoute);

      // Fit the MapControl to the route.
      await MapWithRoute.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
   }
}
```

Este ejemplo muestra lo siguiente en una [**clase MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) denominada **MapWithRoute**.

![control de mapa con la ruta visualizada.](images/routeonmap.png)

Esta es una versión de este ejemplo que usa un punto de trayecto *a través* de entre dos puntos de *interrupción de detención* :

```csharp
using System;
using Windows.Devices.Geolocation;
using Windows.Services.Maps;
using Windows.UI;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Maps;
...
private async void ShowRouteOnMap()
{
  Geolocator locator = new Geolocator();
  locator.DesiredAccuracyInMeters = 1;
  locator.PositionChanged += Locator_PositionChanged;

  BasicGeoposition point1 = new BasicGeoposition() { Latitude = 47.649693, Longitude = -122.144908 };
  BasicGeoposition point2 = new BasicGeoposition() { Latitude = 47.6205, Longitude = -122.3493 };
  BasicGeoposition point3 = new BasicGeoposition() { Latitude = 48.649693, Longitude = -122.144908 };

  // Get Driving Route from point A  to point B thru point C
  var path = new List<EnhancedWaypoint>();

  path.Add(new EnhancedWaypoint(new Geopoint(point1), WaypointKind.Stop));
  path.Add(new EnhancedWaypoint(new Geopoint(point2), WaypointKind.Via));
  path.Add(new EnhancedWaypoint(new Geopoint(point3), WaypointKind.Stop));

  MapRouteFinderResult routeResult =  await MapRouteFinder.GetDrivingRouteFromEnhancedWaypointsAsync(path);

  if (routeResult.Status == MapRouteFinderStatus.Success)
  {
      MapRouteView viewOfRoute = new MapRouteView(routeResult.Route);
      viewOfRoute.RouteColor = Colors.Yellow;
      viewOfRoute.OutlineColor = Colors.Black;

      myMap.Routes.Add(viewOfRoute);

      await myMap.TrySetViewBoundsAsync(
            routeResult.Route.BoundingBox,
            null,
            Windows.UI.Xaml.Controls.Maps.MapAnimationKind.None);
  }
}
```

## <a name="related-topics"></a>Temas relacionados

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Ejemplo de mapa de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Directrices de diseño para mapas](./display-maps.md)
* [Vídeo de compilación de 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps (Aprovechar los mapas y la ubicación entre teléfonos, tabletas y equipos en tus aplicaciones Windows)](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Ejemplo de aplicación de tráfico para UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
