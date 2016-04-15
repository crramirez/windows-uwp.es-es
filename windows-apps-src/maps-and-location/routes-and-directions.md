---
title: Mostrar rutas e indicaciones en un mapa
description: Solicita rutas e indicaciones y muéstralas en tu aplicación.
ms.assetid: BBB4C23A-8F10-41D1-81EA-271BE01AED81
---

# Mostrar rutas e indicaciones en un mapa


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Solicita rutas e indicaciones y muéstralas en tu aplicación.

**Sugerencia** Para obtener más información sobre el uso de mapas en la aplicación, descarga la muestra siguiente del [repositorio de muestras universales de Windows](http://go.microsoft.com/fwlink/p/?LinkId=619979) que encontrarás en GitHub.

-   [Muestra de mapa en la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

**Sugerencia**  Si los mapas no son una característica principal de tu aplicación, considera la posibilidad de iniciar la aplicación Mapas de Windows en su lugar. Puedes usar los esquemas URI `bingmaps:`, `ms-drive-to:` y `ms-walk-to:` para iniciar la aplicación de mapas de Windows para mostrar mapas específicos e indicaciones paso a paso. Para obtener más información, consulta [Iniciar la aplicación Mapas de Windows](https://msdn.microsoft.com/library/windows/apps/mt228341).

 

## Una introducción a los resultados de MapRouteFinder


Así es como están relacionadas las clases de rutas e indicaciones:

-   La clase [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938) tiene métodos que obtienen rutas e indicaciones.
-   Estos métodos devuelven un [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939).
-   [
            **MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) contiene un objeto [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937). Obtén acceso a este objeto a través de la propiedad [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940) de **MapRouteFinderResult**.
-   [
            **MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) contiene una colección de objetos [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955). Obtén acceso a esta colección a través de la propiedad [**Legs**](https://msdn.microsoft.com/library/windows/apps/dn636973) de **MapRoute**.
-   Cada [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) contiene una colección de objetos [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961). Accede a esta colección a través de la propiedad [**Maneuvers**](https://msdn.microsoft.com/library/windows/apps/dn636959) de **MapRouteLeg**.

## Mostrar indicaciones


Obtén una ruta e indicaciones para ir a pie o en coche mediante una llamada a los métodos de la clase [**MapRouteFinder**](https://msdn.microsoft.com/library/windows/apps/dn636938); por ejemplo, [**GetDrivingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636943) o [**GetWalkingRouteAsync**](https://msdn.microsoft.com/library/windows/apps/dn636953). El objeto [**MapRouteFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn636939) contiene un objeto [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) al que puedes tener acceso mediante su propiedad [**Route**](https://msdn.microsoft.com/library/windows/apps/dn636940).

Cuando solicitas una ruta, puedes especificar lo siguiente:

-   Puedes proporcionar un punto inicial y un punto final o puedes proporcionar una serie de puntos de trayecto para calcular la ruta.
-   Puedes especificar optimizaciones: por ejemplo, minimizar la distancia.
-   Puedes especificar restricciones: por ejemplo, evitar autopistas.

El objeto [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) calculado tiene propiedades que proporcionan el tiempo necesario para recorrer la ruta, la longitud de la ruta y la colección de objetos [**MapRouteLeg**](https://msdn.microsoft.com/library/windows/apps/dn636955) que contienen los tramos de la ruta. Cada objeto **MapRouteLeg** contiene una colección de objetos [**MapRouteManeuver**](https://msdn.microsoft.com/library/windows/apps/dn636961). El objeto **MapRouteManeuver** contiene indicaciones a las que puedes tener acceso mediante su propiedad [**InstructionText**](https://msdn.microsoft.com/library/windows/apps/dn636964).

**Importante** Debes especificar una clave de autenticación de mapas para poder usar los servicios de mapa. Para obtener más información, consulta [Solicitar una clave de autenticación de mapas](authentication-key.md).

 

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

## Mostrar rutas


Para mostrar un objeto [**MapRoute**](https://msdn.microsoft.com/library/windows/apps/dn636937) en un [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), construye una [**MapRouteView**](https://msdn.microsoft.com/library/windows/apps/dn637122) con el objeto **MapRoute**. A continuación, agrega la **MapRouteView** a la colección [**Routes**](https://msdn.microsoft.com/library/windows/apps/dn637047) del **MapControl**.

**Importante**  Tienes que especificar una clave de autenticación de mapas para poder usar los servicios de mapa o el control de mapa. Para obtener más información, consulta [Solicitar una clave de autenticación de mapas](authentication-key.md).

 

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

En este ejemplo se muestra lo siguiente en un [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) denominado **MapWithRoute**.

![Control de mapa con ruta visualizada.](images/routeonmap.png)

## Temas relacionados

* [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/)
* [Muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Directrices de diseño para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo de compilación de 2015: Aprovechamiento de mapas y ubicación entre teléfonos, tabletas y equipos en tus aplicaciones de Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Ejemplo de aplicación de tráfico de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)



<!--HONumber=Mar16_HO1-->


