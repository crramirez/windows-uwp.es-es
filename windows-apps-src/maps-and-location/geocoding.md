---
title: Realizar geocodificación y geocodificación inversa
description: Esta guía muestra cómo convertir direcciones postales en ubicaciones geográficas (geocodificación) y convertir las ubicaciones geográficas a direcciones postales (geocodificación inversa) mediante una llamada a los métodos de la clase MapLocationFinder en el espacio de nombres Windows.Services.Maps.
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, geocodificación, geocoding, mapa, map, ubicación, location
ms.localizationpriority: medium
ms.openlocfilehash: a30ca89242b15866019fffc6972bdae7086f3f7e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637630"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>Realizar geocodificación y geocodificación inversa

Esta guía muestra cómo convertir direcciones postales en ubicaciones geográficas (geocodificación) y convertir las ubicaciones geográficas en direcciones postales (geocodificación inversa) mediante una llamada a los métodos de la [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) clase en el [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979) espacio de nombres.

> [!TIP]
> Para más información sobre el uso de mapas en su aplicación, descargue el [MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl) ejemplo desde el [repositorio de ejemplos de Windows universal](h https://github.com/Microsoft/Windows-universal-samples) en GitHub.

Las clases implicadas en la codificación geográfica y geocodificación inversa se organizan como sigue.

-   El [ **MapLocationFinder** ](https://msdn.microsoft.com/library/windows/apps/dn627550) clase contiene métodos que controlan la geocodificación ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) y (decodificacióngeográficainversa[ **FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)).
-   Estos métodos devuelven un [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) instancia.
-   El [ **ubicaciones** ](https://msdn.microsoft.com/library/windows/apps/dn627552) propiedad de la [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) expone una colección de [  **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos. 
-   [**MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos tienen ambos un [ **dirección** ](https://msdn.microsoft.com/library/windows/apps/dn636929) propiedad, que expone un [ **MapAddress** ](https://msdn.microsoft.com/library/windows/apps/dn627533) objeto que representa una dirección postal y un [ **punto** ](https://docs.microsoft.com/uwp/api/windows.services.maps.maplocation.point) propiedad, que expone un [ **Geopoint** ](https://docs.microsoft.com/uwp/api/windows.devices.geolocation.geopoint) objeto que representa una ubicación geográfica.

> [!IMPORTANT]
> Debe especificar una clave de autenticación de asignaciones para poder usar los servicios de mapa. Para obtener más información, consulta [Solicitar una clave de autenticación de mapas](authentication-key.md).

## <a name="get-a-location-geocode"></a>Obtener una ubicación (geocodificación)

En esta sección se muestra cómo convertir una dirección o nombre de un lugar a una ubicación geográfica (geocodificación).

1.  Llamar a una de las sobrecargas de los [ **FindLocationsAsync** ](https://msdn.microsoft.com/library/windows/apps/dn636925) método de la [ **MapLocationFinder** ](https://msdn.microsoft.com/library/windows/apps/dn627550) clase con un nombre de contexto o la calle dirección.
2.  El [ **FindLocationsAsync** ](https://msdn.microsoft.com/library/windows/apps/dn636925) método devuelve un [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) objeto.
3.  Use la [ **ubicaciones** ](https://msdn.microsoft.com/library/windows/apps/dn627552) propiedad de la [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) para exponer una colección [  **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos. Puede haber varios [ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos porque el sistema puede encontrar varias ubicaciones que corresponden a la entrada especificada.

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

1.  Llama al método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) de la clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  El método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) devuelve un objeto [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) que contiene una colección de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) coincidentes.
3.  Use la [ **ubicaciones** ](https://msdn.microsoft.com/library/windows/apps/dn627552) propiedad de la [ **MapLocationFinderResult** ](https://msdn.microsoft.com/library/windows/apps/dn627551) para exponer una colección [  **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos. Puede haber varios [ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549) objetos porque el sistema puede encontrar varias ubicaciones que corresponden a la entrada especificada.
4.  Acceso [ **MapAddress** ](https://msdn.microsoft.com/library/windows/apps/dn627533) objetos a través de la [ **dirección** ](https://msdn.microsoft.com/library/windows/apps/dn636929) propiedad de cada uno [ **MapLocation** ](https://msdn.microsoft.com/library/windows/apps/dn627549).

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

* [Ejemplo de asignación de UWP](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Ejemplo de aplicación UWP tráfico](https://go.microsoft.com/fwlink/p/?LinkId=619982)
* [Instrucciones de diseño de mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo: Aprovechamiento de mapas y ubicación en el teléfono, tableta y PC en sus aplicaciones de Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Centro para desarrolladores de Bing Maps](https://www.bingmapsportal.com/)
* [**MapLocationFinder** clase](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync** method](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync** method](https://msdn.microsoft.com/library/windows/apps/dn636928)
