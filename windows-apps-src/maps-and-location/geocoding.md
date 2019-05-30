---
title: Realizar geocodificación y geocodificación inversa
description: Esta guía muestra cómo convertir direcciones postales en ubicaciones geográficas (geocodificación) y convertir las ubicaciones geográficas a direcciones postales (geocodificación inversa) mediante una llamada a los métodos de la clase MapLocationFinder en el espacio de nombres Windows.Services.Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, geocodificación, geocoding, mapa, map, ubicación, location
ms.localizationpriority: medium
ms.openlocfilehash: 88687f6e7074ff7c914927a81a08720336b3c60c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371874"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Realizar geocodificación y geocodificación inversa

Esta guía muestra cómo convertir direcciones postales en ubicaciones geográficas (geocodificación) y convertir las ubicaciones geográficas en direcciones postales (geocodificación inversa) mediante una llamada a los métodos de la [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) clase en el [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps) espacio de nombres.

> [!TIP]
> Para más información sobre el uso de mapas en su aplicación, descargue el [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) ejemplo desde el [repositorio de ejemplos de Windows universal](h https://github.com/Microsoft/Windows-universal-samples) en GitHub.

Las clases implicadas en la codificación geográfica y geocodificación inversa se organizan como sigue.

-   El [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) clase contiene métodos que controlan la geocodificación ([**FindLocationsAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) y (decodificacióngeográficainversa[ **FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)).
-   Estos métodos devuelven un [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) instancia.
-   El [ **ubicaciones** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) propiedad de la [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) expone una colección de [  **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos. 
-   [**MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos tienen ambos un [ **dirección** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) propiedad, que expone un [ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) objeto que representa una dirección postal y un [ **punto** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) propiedad, que expone un [ **Geopoint** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) objeto que representa una ubicación geográfica.

> [!IMPORTANT]
> Debe especificar una clave de autenticación de asignaciones para poder usar los servicios de mapa. Para obtener más información, consulta [Solicitar una clave de autenticación de mapas](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obtener una ubicación (geocodificación)

En esta sección se muestra cómo convertir una dirección o nombre de un lugar a una ubicación geográfica (geocodificación).

1.  Llamar a una de las sobrecargas de los [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) método de la [ **MapLocationFinder** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) clase con un nombre de contexto o la calle dirección.
2.  El [ **FindLocationsAsync** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) método devuelve un [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) objeto.
3.  Use la [ **ubicaciones** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) propiedad de la [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) para exponer una colección [  **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos. Puede haber varios [ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos porque el sistema puede encontrar varias ubicaciones que corresponden a la entrada especificada.

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

Este código muestra los siguientes resultados en el cuadro de texto `tbOutputText`.

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## <a name="get-an-address-reverse-geocode"></a>Obtener una dirección (geocodificación inversa)

En esta sección se muestra cómo convertir una ubicación geográfica a una dirección (geocodificación inversa).

1.  Llama al método [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) de la clase [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder).
2.  El método [**FindLocationsAtAsync**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) devuelve un objeto [**MapLocationFinderResult**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) que contiene una colección de objetos [**MapLocation**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) coincidentes.
3.  Use la [ **ubicaciones** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinderresult.locations) propiedad de la [ **MapLocationFinderResult** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinderResult) para exponer una colección [  **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos. Puede haber varios [ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation) objetos porque el sistema puede encontrar varias ubicaciones que corresponden a la entrada especificada.
4.  Acceso [ **MapAddress** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapAddress) objetos a través de la [ **dirección** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.address) propiedad de cada uno [ **MapLocation** ](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocation).

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

Este código muestra los siguientes resultados en el cuadro de texto `tbOutputText`.

``` syntax
town = Redmond
```

## <a name="related-topics"></a>Temas relacionados

* [Ejemplo de mapa de UWP](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Ejemplo de aplicación de tráfico para UWP](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [Directrices de diseño para mapas](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Vídeo: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps](https://channel9.msdn.com/Events/Build/2015/2-757) (Aprovechamiento de mapas y ubicación entre teléfonos, tabletas y equipos en las aplicaciones de Windows)
* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [**MapLocationFinder** clase](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync** method](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync** method](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
