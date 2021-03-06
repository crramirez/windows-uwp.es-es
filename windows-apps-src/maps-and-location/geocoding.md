---
title: Realizar geocodificación y geocodificación inversa
description: En esta guía se muestra cómo convertir direcciones de calles en ubicaciones geográficas (geocodificación) y convertir ubicaciones geográficas en direcciones postales (geocodificación inversa) mediante una llamada a los métodos de la clase MapLocationFinder en el espacio de nombres Windows. Services. Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 10/20/2020
ms.topic: article
keywords: Windows 10, UWP, geocodificación, mapa, ubicación
ms.localizationpriority: medium
ms.openlocfilehash: 992a9902081f0655885383ef90ea02ed1e79f13a
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297703"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Realizar geocodificación y geocodificación inversa

> [!NOTE]
> [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y MAP Services refieren una clave de autenticación de Maps denominada [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Para obtener más información sobre cómo obtener y establecer una clave de autenticación de Maps, consulte [solicitar una clave de autenticación de Maps](authentication-key.md).

En esta guía se muestra cómo convertir direcciones de calles en ubicaciones geográficas (geocodificación) y convertir ubicaciones geográficas en direcciones postales (geocodificación inversa) mediante una llamada a los métodos de la clase [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) en el espacio de nombres [**Windows. Services. Maps**](/uwp/api/Windows.Services.Maps) .

> [!TIP]
> Para obtener más información sobre cómo usar Maps en la aplicación, descargue el ejemplo de [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) del [repositorio de ejemplos de Windows universal](hhttps://github.com/Microsoft/Windows-universal-samples) en github.

Las clases implicadas en la geocodificación y la geocodificación inversa se organizan de la siguiente manera.

-   La clase [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) contiene métodos que controlan la geocodificación ([**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) y la geocodificación inversa ([**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)).
-   Estos métodos devuelven una instancia de [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) .
-   La propiedad [**locations**](/uwp/api/windows.services.maps.maplocationfinderresult.locations) del [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) expone una colección de objetos [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) . 
-   Los objetos [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) tienen una propiedad [**Address**](/uwp/api/windows.services.maps.maplocation.address) , que expone un objeto [**MapAddress**](/uwp/api/Windows.Services.Maps.MapAddress) que representa una dirección postal, y una propiedad [**Point**](/uwp/api/windows.services.maps.maplocation.point) , que expone un objeto [**geopoint**](/uwp/api/windows.devices.geolocation.geopoint) que representa una ubicación geográfica.

> [!IMPORTANT]
> Debe especificar una clave de autenticación de Maps para poder usar Map Services. Para obtener más información, consulta [Solicitar una clave de autenticación de mapas](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obtener una ubicación (geocodificación)

En esta sección se muestra cómo convertir una dirección postal o un nombre de lugar en una ubicación geográfica (geocodificación).

1.  Llame a una de las sobrecargas del método [**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) de la clase [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) con un nombre de lugar o una dirección postal.
2.  El método [**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync) devuelve un objeto [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) .
3.  Use la propiedad [**locations**](/uwp/api/windows.services.maps.maplocationfinderresult.locations) de [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) para exponer una colección de objetos [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) . Puede haber varios objetos [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) porque el sistema puede encontrar varias ubicaciones que corresponden a la entrada especificada.

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

En esta sección se muestra cómo convertir una ubicación geográfica en una dirección (geocodificación inversa).

1.  Llama al método [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) de la clase [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder).
2.  El método [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) devuelve un objeto [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) que contiene una colección de objetos [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) coincidentes.
3.  Use la propiedad [**locations**](/uwp/api/windows.services.maps.maplocationfinderresult.locations) de [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) para exponer una colección de objetos [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) . Puede haber varios objetos [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) porque el sistema puede encontrar varias ubicaciones que corresponden a la entrada especificada.
4.  Acceder a los objetos [**MapAddress**](/uwp/api/Windows.Services.Maps.MapAddress) a través de la propiedad [**Address**](/uwp/api/windows.services.maps.maplocation.address) de cada [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation).

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

* [Ejemplo de mapa de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Ejemplo de aplicación de tráfico para UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [Directrices de diseño para mapas](./display-maps.md)
* [Vídeo: uso de mapas y ubicaciones en el teléfono, la tableta y el PC en las aplicaciones de Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Clase **MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [Método **FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [Método **FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)
