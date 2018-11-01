---
author: PatrickFarley
title: Realizar geocodificación y geocodificación inversa
description: En esta guía se muestra cómo puedes convertir direcciones en ubicaciones geográficas (geocodificación) y ubicaciones geográficas en direcciones (geocodificación inversa) mediante una llamada a los métodos de la clase MapLocationFinder del espacio de nombres Windows.Services.Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.author: pafarley
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, geocodificación, geocoding, mapa, map, ubicación, location
ms.localizationpriority: medium
ms.openlocfilehash: bdd956dece4435ceb8e14121ec2b545095af3a11
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5871942"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Realizar geocodificación y geocodificación inversa

En esta guía se muestra cómo puedes convertir direcciones en ubicaciones geográficas (geocodificación) y ubicaciones geográficas en direcciones (geocodificación inversa) mediante una llamada a los métodos de la clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) en el [**Windows.Services.Maps **](https://msdn.microsoft.com/library/windows/apps/dn636979)espacio de nombres.

> [!TIP]
> Para obtener más información sobre el uso de mapas en tu aplicación, descarga la muestra de [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) desde el [repositorio de muestras universales de Windows](hhttps://github.com/Microsoft/Windows-universal-samples) en GitHub.

Las clases implicados en geocodificación y geocodificación inversa se organizan como sigue.

-   La clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) contiene métodos que controlen la geocodificación ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) y ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)) de geocodificación inversa.
-   Estos métodos devuelven una instancia de [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) .
-   La propiedad de [**las ubicaciones**](https://msdn.microsoft.com/library/windows/apps/dn627552) de la [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) expone una colección de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) . 
-   [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos tienen una propiedad de [**dirección**](https://msdn.microsoft.com/library/windows/apps/dn636929) , que expone un objeto [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) que representa una dirección y una propiedad de [**punto**](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) , que expone un objeto [**Geopoint**](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) que represente una ubicación geográfica.

> [!IMPORTANT]
> Debes especificar una clave de autenticación de mapas antes de poder usar servicios de mapa. Para obtener más información, consulta [Solicitar una clave de autenticación de mapas](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obtener una ubicación (geocodificación)

En esta sección se muestra cómo convertir una dirección o nombre de un lugar en una ubicación geográfica (geocodificación).

1.  Llamar a una de las sobrecargas del método [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) de la clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) con un nombre de lugar o direcciones de calles.
2.  El método [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) devuelve un objeto [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) .
3.  Usa la propiedad de [**las ubicaciones**](https://msdn.microsoft.com/library/windows/apps/dn627552) de la [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) para exponer los objetos de [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) de una colección. Puede haber varios objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) porque el sistema puede encontrar varias ubicaciones que corresponden a la entrada especificada.

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

1.  Llama al método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) de la clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  El método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) devuelve un objeto [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) que contiene una colección de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) coincidentes.
3.  Usa la propiedad de [**las ubicaciones**](https://msdn.microsoft.com/library/windows/apps/dn627552) de la [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) para exponer los objetos de [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) de una colección. Puede haber varios objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) porque el sistema puede encontrar varias ubicaciones que corresponden a la entrada especificada.
4.  Obtener acceso a objetos [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) a través de la propiedad de [**dirección**](https://msdn.microsoft.com/library/windows/apps/dn636929) de cada [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549).

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

* [Muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Ejemplo de aplicación de tráfico de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [Directrices de diseño para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo: Aprovechar los mapas y ubicación por teléfono, tableta o PC en las aplicaciones de Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/)
* [Clase **MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [Método **FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [Método **FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)
