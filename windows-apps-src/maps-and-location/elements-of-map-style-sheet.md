---
description: Entradas y propiedades de una hoja de estilo de mapa
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referencia de hoja de estilo de mapa
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, maps, mapas, map style sheet, hoja de estilo de mapa
ms.localizationpriority: medium
ms.openlocfilehash: b59e8c3c6d9c4c299e441964be1afb4e02051e23
ms.sourcegitcommit: 5264d7499ddbe21199a63d74a294206069f90f8b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2020
ms.locfileid: "78287451"
---
# <a name="map-style-sheet-reference"></a>Referencia de hoja de estilo de mapa

Las tecnologías de asignación de Microsoft usan _hojas de estilos de mapa_ para definir el aspecto de las asignaciones.  Una hoja de estilos de mapa se define mediante notación de objetos JavaScript (JSON) y se puede usar de varias maneras, como la [clase MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) de la aplicación de la tienda Windows, a través del método [MapStyleSheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) .

Las hojas de estilos se pueden crear de forma interactiva mediante la aplicación del [Editor de hojas de estilos de mapa](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) .

El siguiente JSON se puede usar para que las áreas de agua aparezcan en rojo, las etiquetas de agua aparecen en verde y las áreas de tierra aparecen en azul:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Este JSON se puede usar para quitar todas las etiquetas y los puntos de un mapa.

```json

    {"version":"1.*", "elements":{"mapElement":{"labelVisible":false},"point":{"visible":false}}}
```

A veces, el valor de una propiedad se transforma para generar el resultado final.  Por ejemplo, fillColor de vegetación tiene sombras ligeramente diferentes según el tipo de entidad que se está mostrando.  Este comportamiento se puede desactivar, usando por tanto el valor proporcionado preciso, mediante la propiedad ignoreTransform.

```json
    {"version":"1.*",
        "settings":{"shadedReliefVisible":false},
        "elements":{"vegetation":{"fillColor":{"value":"#999999","ignoreTransform":true}}}
    }
```

En este tema se muestran las entradas y [propiedades](#properties) JSON que puedes usar para personalizar la apariencia de tus mapas.  Estas propiedades también se pueden aplicar a los elementos de mapa de usuario a través de la propiedad [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) .

<a id="entries" />

## <a name="entries"></a>Entradas
En esta tabla se usan caracteres ">" para representar los niveles de la jerarquía de entradas.  También muestra las versiones de Windows que admiten cada entrada y que lo omiten.

| Versión | Nombre de la versión de Windows |
|---------|----------------------|
|  1703   | Creators Update      |
|  1709   | Actualización Fall Creators Update |
|  1803   | Actualización de abril de 2018    |
|  1809   | Actualización de octubre de 2018  |
|  1903   | Actualización de mayo de 2019      |

| Name                               | Grupo de propiedades            | 1703 | 1709 | 1803 | 1809 | 1903 | Descripción    |
|------------------------------------|---------------------------|------|------|------|------|------|----------------|
| version                            | [Versión](#version)       |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | La versión de hoja de estilo que quieres usar. |
| configuración                           | [Configuración](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | La configuración que se aplica a toda la hoja de estilo. |
| mapElement                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | La entrada principal para todas las entradas de mapa. |
| > baseMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | La entrada principal para todas las entradas que no sean del usuario. |
| >> area                            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que describen el uso terrestre.  No deben confundirse con los edificios físicos que se encuentran en la entrada de la estructura. |
| >>> airport                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan aeropuertos. |
| >>> areaOfInterest                 | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Áreas en que hay una alta concentración de empresas o puntos interesantes. |
| >>> cemetery                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan Cemeteries. |
| >>> continent                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Etiquetas del área del continente. |
| >>> education                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan escuelas y otras instalaciones educativas. |
| >>> indigenousPeoplesReserve       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan reservas de pueblos indígenas. |
| >>> industrial                     | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que se usan con fines industriales. |
| >>> island                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Etiquetas de área de isla. |
| >>> medical                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que se usan para fines médicos (por ejemplo, un campus hospitalario). |
| >>> military                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan bases militares o que tienen usos militares. |
| >>> nautical                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que se usan para fines relacionados con la náutica. |
| >>> neighborhood                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Etiquetas de área de entorno. |
| >>> runway                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que se usan como aterrizaje de avión. |
| >>> sand                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zonas de arena, como playas. |
| >>> shoppingCenter                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zonas de suelo asignadas para centros comerciales. |
| >>> stadium                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan estadios. |
| >>> underground                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Áreas subterráneas (por ejemplo: la superficie de una estación de metro). |
| >>> vegetation                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bosques, prados, etc. |
| >>>> forest                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Zonas de tierra forestada. |
| >>>> golfCourse                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan los cursos de golf. |
| >>>> park                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan parques. |
| >>>> playingField                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Campos extraídos, como un campo de fútbol o pista de tenis. |
| >>>> reserve                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan las reservas de naturaleza. |
| > > frozenWater                     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Agua congelada, como Glacier. |
| >> point                           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Todas las características de punto que se dibujan con un icono de algún tipo. |
| >>> address                        | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   |  ✔   | Etiquetas de números de dirección. |
| >>> naturalPoint                   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan características naturales. |
| >>>> peak                          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan cumbres. |
| >>>>> volcanicPeak                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan picos de volcán. |
| >>>> waterPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan ubicaciones de agua, como una cascada. |
| >>> pointOfInterest                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan cualquier ubicación interesante. |
| >>>> business                      | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan cualquier ubicación empresarial. |
| > > > > > attractionPoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan atracciones turísticas, como museos, zoológicos, etc. |
| > > > > > > amusementPlacePoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de lugar de diversión. |
| > > > > > > aquariumPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de acuario. |
| > > > > > > artGalleryPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de la galería de arte. |
| > > > > > > campPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de campamento. |
| > > > > > > fishingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de pesca. |
| > > > > > > gardenPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de jardín. |
| > > > > > > hikingPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de excursión. |
| > > > > > > libraryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de la biblioteca. |
| > > > > > > museumPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de Museo. |
| > > > > > > naturalPlacePoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de ubicación natural. |
| > > > > > > parkPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de estacionamiento. |
| > > > > > > restAreaPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono del área de REST. |
| > > > > > > touristAttractionPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de atracción turística. |
| > > > > > > zooPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de Zoo. |
| > > > > > communityPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan las ubicaciones de uso general de la comunidad. |
| > > > > > > conventionCenterPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono del centro de convenciones. |
| > > > > > > financialPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono financiero. |
| > > > > > > governmentPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de gobierno. |
| > > > > > > informationTechnologyPoint  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de tecnología de la información. |
| > > > > > > palacePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de Palacio. |
| > > > > > > pollingStationPoint         | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de la estación de sondeo. |
| > > > > > > researchPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de investigación. |
| > > > > > educationPoint               | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan escuelas y otras ubicaciones relacionadas con la educación. |
| > > > > > entertainmentPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan lugares de entretenimiento, como cines, cines, etc. |
| > > > > > > artsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de arte. |
| > > > > > > bowlingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de bolos. |
| > > > > > > casinoPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de Casino. |
| > > > > > > golfCoursePoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de curso de golf. |
| > > > > > > gymPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de gimnasio. |
| > > > > > > marinaPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de Marina. |
| > > > > > > movieTheaterPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de cine en película. |
| > > > > > > nightClubPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono del club nocturno. |
| > > > > > > recreationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de recreo. |
| > > > > > > skatingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de patinaje. |
| > > > > > > skiAreaPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono del área de Ski. |
| > > > > > > stadiumPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono del grupo de natación. |
| > > > > > > swimmingPoolPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono del grupo de natación. |
| > > > > > > theaterPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de teatro. |
| > > > > > > wineryPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de bodega. |
| > > > > > essentialServicePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan servicios esenciales como estacionamiento, bancos, gas, etc. |
| > > > > > > aTMPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de ATM. |
| > > > > > > automobileRentalPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de alquiler de automóviles. |
| > > > > > > automobileRepairPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de reparación de automóviles. |
| > > > > > > bankPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de banco. |
| > > > > > > clinicPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de la sesión. |
| > > > > > > electricChargingStationPoint| [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de la estación de carga eléctrica. |
| > > > > > > fireStationPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de FireStation. |
| > > > > > > gasStationPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de GasStation. |
| > > > > > > groceryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de supermercado. |
| > > > > > > hospitalPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de hospital. |
| > > > > > > laundryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de lavandería. |
| > > > > > > liquorAndWineStorePoint     | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono del licor y del almacén del vino. |
| > > > > > > mailPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono mail. |
| > > > > > > marketPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de mercado. |
| > > > > > > parkingPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de estacionamiento. |
| > > > > > > petsPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de mascotas. |
| > > > > > > pharmacyPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de farmacia. |
| > > > > > > policePoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de policía. |
| > > > > > > postalServicePoint          | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono del servicio postal. |
| > > > > > > professionalPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono servicio profesional. |
| > > > > > > toiletPoint                 | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de inodoro. |
| > > > > > > veterinarianPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de veterinario. |
| >>>>> foodPoint                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan restaurantes, cafés, etc. |
| > > > > > > barAndGrillPoint            | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de barra y rejilla. |
| > > > > > > barPoint                    | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de barra. |
| > > > > > > breweryPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de cerveza. |
| > > > > > > cafePoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de cafetería. |
| > > > > > > iceCreamShopPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de helado. |
| > > > > > > restaurantPoint             | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de restaurante. |
| > > > > > > teaShopPoint                | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de TeaShop. |
| > > > > > lodgingPoint                 | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan Hoteles y otras empresas de alojamiento. |
| > > > > > > gotelPoint                  | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de Hotel. |
| > > > > > realEstatePoint              | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan empresas inmobiliarias. |
| > > > > > shoppingPoint                | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan Hoteles y otras empresas de alojamiento. |
| > > > > > > automobileDealerPoint       | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de concesionario de automóviles. |
| > > > > > > beautyAndSpaPoint           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de belleza y Spa. |
| > > > > > > bookstorePoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de librería. |
| > > > > > > departmentStorePoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de almacén de departamento. |
| > > > > > > electronicsShopPoint        | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de la tienda electrónica. |
| > > > > > > golfShopPoint               | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de Golf Shop. |
| > > > > > > homeApplianceShopPoint      | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de tienda del dispositivo doméstico. |
| > > > > > > mallPoint                   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de centro comercial. |
| > > > > > > phoneShopPoint              | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Icono de la tienda telefónica. |
| >>> populatedPlace                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan el tamaño de un lugar poblado (por ejemplo: una ciudad o pueblo). |
| >>>> capital                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan la capital de un lugar poblado. |
| >>>>> adminDistrictCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan la capital de un estado o provincia. |
| > > > > > > majorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Iconos que representan el capital principal de un estado o provincia. |
| > > > > > > minorAdminDistrictCapital   | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Iconos que representan el capital secundario de un estado o provincia. |
| >>>>> countryRegionCapital         | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan la capital de un país o región. |
| > > > > majorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Iconos que representan el tamaño de la posición rellenada principal. |
| > > > > minorPopulatedPlace           | [PointStyle](#pointstyle) |      |      |      |      |  ✔   | Iconos que representan el tamaño de la posición rellenada menor. |
| >>> roadShield                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Carteles que representan el nombre compacto de una carretera. (Por ejemplo: I-5). Usa solo valores de paleta si estableces la propiedad **ImageFamily** de la entrada de configuración en un valor *Palette*. |
| >>> roadExit                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan salidas, normalmente desde una autopista de acceso controlado. |
| >>> transit                        | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan paradas de autobús, paradas de tren, aeropuertos, etc. |
| >> political                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Regiones políticas, como países, regiones y estados o provincias. |
| >>> countryRegion                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Bordes y etiquetas de la región del país. |
| >>> adminDistrict                  | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, Estados, provincias, etc., bordes y etiquetas. |
| >>> district                       | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, condados, etc., bordes y etiquetas. |
| >> structure                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Edificios y otras estructuras edificadas similares. |
| >>> building                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Oficinas. |
| >>>> educationBuilding             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Edificios utilizados para la educación. |
| >>>> medicalBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Edificios usados para fines médicos como hospitales. |
| >>>> transitBuilding               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Edificios usados para el tránsito, como aeropuertos. |
| >> transportation                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que forman parte de la red de transporte (por ejemplo: carreteras, trenes y transbordadores). |
| >>> road                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan todas las carreteras. |
| >>>> controlledAccessHighway       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan carreteras de acceso grandes y controladas. |
| >>>>> highSpeedRamp                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan rampas de alta velocidad que normalmente se conectan a la autopista de acceso controlado. |
| >>>> highway                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan carreteras. |
| >>>> majorRoad                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan carreteras principales. |
| >>>> arterialRoad                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan carreteras arterial. |
| >>>> street                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan calles. |
| >>>>> ramp                         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan rampas que normalmente se conectan a la autopista. |
| >>>>> unpavedStreet                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan unpaved calles. |
| >>>> tollRoad                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan carreteras que cuestan el uso de dinero. |
| >>> railway                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas de ferrocarriles. |
| >>> trail                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Caminos por parques o senderos. |
| > > > Pasarela                        | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Pasarela elevado. |
| >>> waterRoute                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas de ruta de transbordador. |
| >> water                           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Todo lo que parezca a agua. Esto incluye océanos y riachuelos. |
| >>> river                          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Ríos, riachuelos u otros recorridos de agua.  Ten en cuenta que esto puede ser una línea o polígono y puede conectarse a masas de agua que no sean ríos. |
| > routeMapElement                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Todas las entradas relacionadas con el enrutamiento. |
| >> routeLine                       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Entradas relacionadas con la línea de ruta. |
| >>> drivingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan rutas de conducción. |
| >>> scenicRoute                    | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan rutas de conducción de Scenic. |
| >>> walkingRoute                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan rutas de recorrido. |
| > userMapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Todas las entradas de usuario. |
| >> userBillboard                   | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   |  ✔   | El estilo de las instancias [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) predeterminadas. |
| >> userLine                        | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El estilo de las instancias [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) predeterminadas. |
| >> userModel3D                     | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   |  ✔   | El estilo de las instancias [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) predeterminadas.  Esto va dirigido principalmente al juste de renderAsSurface. |
| >> userPoint                       | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El estilo de las instancias [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) predeterminadas. |

<a id="properties" />

## <a name="properties"></a>Propiedades

En esta sección se describen las propiedades que puedes usar para cada entrada.

<a id="version" />

### <a name="version-properties"></a>Propiedades de la versión

| Propiedad                     | Tipo    | Descripción                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | String  | Versión de la hoja de estilo de destino. Se usa para la aplicación. "1.0" para el valor predeterminado "1.*" para actualizaciones menores de características adicionales. |

<a id="settings" />

### <a name="settings-properties"></a>Propiedades de la configuración

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | 1903 | Descripción |
|------------------------------|---------|------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Marcador que indica si la atmósfera aparece en el control 3D. |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   |  ✔   | Una marca que indica si se debe o no mostrar texturas en edificios 3D simbólicos que tienen texturas. |
| fogColor                     | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El valor de color ARGB de la niebla de distancia que aparece en el control 3D. |
| glowColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El valor de color ARGB que se podría aplicarse para etiquetar resplandor y resplandor de icono. |
| imageFamily                  | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El nombre de la imagen establecida para usar con este estilo. Establece este valor en *Default* para carteles que usan colores fijos basados en el cartel del mundo real. Establece este valor en *Palette* para carteles que usan colores de paleta configurables. |
| landColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El valor de color ARGB de la tierra antes de que se dibuje algún elemento en ella. |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Un marcador que indica si los elementos que tienen una propiedad **Organization** deben dibujar los logotipos adecuados o usar un icono genérico. |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Un marcador que indica si los elementos que tienen una propiedad de color oficial (por ejemplo, las líneas de transporte público de China) deben dibujar ese color. Por ejemplo, desactiva este valor para un mapa en blanco y negro. |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Marca que indica si se dibujan o no las regiones de tramas en las que tienen una mejor representación que los vectores (Japón y Corea). |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Un marcador que indica si se va a dibujar o no el sombreado de elevación en el mapa. |
| shadowColor                  | Color   |      |      |      |  ✔   |  ✔   | Color de los iconos de sombra detrás que utilizan sombras. |
| spaceColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El valor de color ARGB para la zona que rodea el mapa. |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Marca que indica si se deben usar los colores originales del SVG en lugar de buscar la entrada de la paleta para los colores de una imagen. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>Propiedades de MapElement

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | 1903 | Descripción |
|------------------------------|---------|------|------|------|------|------|-------------|
| backgroundScale              | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Cantidad en que se debe escalar el elemento en segundo plano de un icono.  Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| fillColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El color que se usa para rellenar polígonos, el fondo de los iconos de punto y para el centro de líneas si se han dividido. |
| fontFamily                   | String  |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| fontWeight                   | String  |      |      |      |      |  ✔   | Densidad de un tipo de letra, en cuanto a la luminosidad o la intensidad de los trazos. "**Light**", "**normal**", "**Semibold**" y "**Bold**" se pueden establecer |
| iconColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El color del glifo que se muestra en el medio de un icono de punto. |
| iconScale                    | Flotante   |      |  ✔   |  ✔   |  ✔   |  ✔   | Cantidad en que se debe escalar el glifo de un icono.  Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| labelColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | La cantidad según la cual se aplicará escala a los tamaños de etiqueta predeterminados. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Hace que el valor alfa del elemento **FillColor** sobrescriba el elemento **StrokeColor** en lugar de combinarse con él. |
| escala                        | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | La cantidad según la cual se aplicará escala al tamaño de todo el punto. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| strokeColor                  | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El color que se usará para el contorno de polígonos, el contorno alrededor de los iconos de punto y el color de las líneas. |
| strokeWidthScale             | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | La cantidad según la cual se aplicará escala al trazado de las líneas. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Este grupo de propiedades hereda los valores del grupo de propiedades [MapElement](#mapelement).

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | 1903 | Descripción |
|------------------------------|---------|------|------|------|------|------|-------------|
| borderOutlineColor           | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El color de línea secundaria o de marco del borde de un polígono rellenado. |
| borderStrokeColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El color de la línea principal del borde de un polígono rellenado. |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Cantidad por la que se escala el trazo de los bordes. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propiedades de PointStyle

Este grupo de propiedades hereda los valores del grupo de propiedades [MapElement](#mapelement).

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | 1903 | Descripción |
|------------------------------|---------|------|------|------|------|------|-------------|
| shadowVisible                | Bool    |      |      |      |      |  ✔   | Marca que indica si la sombra del icono debe estar visible o no. |
| forma: fondo             | String  |      |      |      |      |  ✔   | Forma que se va a usar como fondo del icono, reemplazando cualquier forma que exista en ella. |
| Icono de forma                   | String  |      |      |      |      |  ✔   | Forma que se va a usar como glifo de primer plano del icono, reemplazando cualquier forma que exista allí. |
| stemAnchorRadiusScale        | Flotante   |      |      |  ✔   |  ✔   |  ✔   | Cantidad en que se debe escalar el punto de anclaje del eje de un icono.  Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| stemColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El color de la barra que sale de la parte inferior del icono en modo 3D. |
| stemHeightScale              | Flotante   |      |      |  ✔   |  ✔   |  ✔   | Cantidad en que se debe escalar la longitud del eje de un icono.  Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble de longitud. |
| stemOutlineColor             | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | El color del contorno alrededor de la barra que sale de la parte inferior del icono en modo 3D. |
| stemWidthScale               | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   |  ✔   | Cantidad en que se debe escalar la anchura del eje de un icono.  Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble de longitud. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Este grupo de propiedades hereda los valores del grupo de propiedades [MapElement](#mapelement).

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | 1903 | Descripción |
|------------------------------|---------|------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   |  ✔   | Una marca que indica que un modelo 3D se debe representar como un edificio, sin fundido de profundidad contra el suelo. |
