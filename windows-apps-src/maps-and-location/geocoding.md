---
author: PatrickFarley
title: "Realizar geocodificación y geocodificación inversa"
description: "Puedes convertir direcciones en ubicaciones geográficas (geocodificación) y ubicaciones geográficas en direcciones (geocodificación inversa), si llamas a los métodos de la clase MapLocationFinder del espacio de nombres Windows.Services.Maps."
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, geocodificación, geocoding, mapa, map, ubicación, location"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 16be7bcafaf286a71e79fb4bca01511ddc7a1ae0
ms.lasthandoff: 02/07/2017

---

# <a name="perform-geocoding-and-reverse-geocoding"></a>Realizar geocodificación y geocodificación inversa


\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Puedes convertir direcciones en ubicaciones geográficas (geocodificación), y ubicaciones geográficas en direcciones (geocodificación inversa), si llamas a los métodos de la clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) del espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979).

**Sugerencia** Para obtener más información sobre el uso de mapas en la aplicación, descarga la muestra siguiente del [repositorio de muestras universales de Windows](http://go.microsoft.com/fwlink/p/?LinkId=619979) que encontrarás en GitHub.

-   [Muestra de mapa en la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

Aquí se explica la relación entre las clases de geocodificación y geocodificación inversa:

-   La clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) tiene métodos que llevan a cabo procedimientos de geocodificación ([**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)) y geocodificación inversa ([**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)).
-   Estos métodos devuelven la clase [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551).
-   A su vez, la clase [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) contiene una colección de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549). Puedes obtener acceso a esta colección a través de la propiedad [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) de **MapLocationFinderResult**.
-   Igualmente, cada objeto [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) contiene un objeto [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533). Puedes obtener acceso a este objeto a través de la propiedad [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) de cada objeto **MapLocation**.

**Importante** Es necesario especificar una clave de autenticación de mapas para poder usar los servicios de mapa. Para obtener más información, consulta [Solicitar una clave de autenticación de mapas](authentication-key.md).

 

## <a name="get-a-location-geocode"></a>Obtener una ubicación (geocodificación)


Convierte una dirección o el nombre de un lugar en una ubicación geográfica (geocodificación) mediante el siguiente procedimiento.

1.  Llama a una de las sobrecargas del método [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) de la clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  El método [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925) devuelve un objeto [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) que contiene una colección de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) coincidentes.
3.  Accede a esta colección a través de la propiedad [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) de la clase [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551).

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


Convierte una ubicación geográfica en una dirección (geocodificación inversa) mediante el siguiente procedimiento.

1.  Llama al método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) de la clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550).
2.  El método [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928) devuelve un objeto [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551) que contiene una colección de objetos [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549) coincidentes.
3.  Accede a esta colección a través de la propiedad [**Locations**](https://msdn.microsoft.com/library/windows/apps/dn627552) de la clase [**MapLocationFinderResult**](https://msdn.microsoft.com/library/windows/apps/dn627551).
4.  Accede al objeto [**MapAddress**](https://msdn.microsoft.com/library/windows/apps/dn627533) a través de la propiedad [**Address**](https://msdn.microsoft.com/library/windows/apps/dn636929) de cada clase [**MapLocation**](https://msdn.microsoft.com/library/windows/apps/dn627549).

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

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Directrices de diseño para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo de compilación de 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps (Aprovechamiento de mapas y ubicación entre teléfonos, tabletas y equipos en tus aplicaciones de Windows)](https://channel9.msdn.com/Events/Build/2015/2-757)
* [UWP traffic app sample (Ejemplo de aplicación de tráfico de UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619982)
* [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550)
* [**FindLocationsAsync**](https://msdn.microsoft.com/library/windows/apps/dn636925)
* [**FindLocationsAtAsync**](https://msdn.microsoft.com/library/windows/apps/dn636928)

