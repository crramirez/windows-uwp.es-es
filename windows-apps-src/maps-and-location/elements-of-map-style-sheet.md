---
author: normesta
description: Entradas y propiedades de una hoja de estilo de mapa
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referencia de hoja de estilo de mapa
ms.author: normesta
ms.date: 03/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, maps, mapas, map style sheet, hoja de estilo de mapa
ms.localizationpriority: medium
ms.openlocfilehash: f0a657ada755b77abe8ffef6a38bfa1f9ece8fcd
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "4353402"
---
# <a name="map-style-sheet-reference"></a>Referencia de hoja de estilo de mapa

Tecnologías de asignación de Microsoft usan _hojas de estilo de mapa_ para definir la apariencia de mapas.  Una hoja de estilo de mapa se define mediante la notación de objetos JavaScript (JSON) y puede usarse en diversas formas que incluya en la aplicación de la tienda Windows [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) a través del método [MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) .

Hojas de estilo pueden crearse de forma interactiva mediante la aplicación de [Editor de hojas de estilo de mapa](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) .

Se puede usar la siguiente notación JSON para hacer que las zonas de agua aparezcan en color rojo, las etiquetas de agua aparezcan en verde y tierra aparezcan en azul:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Este JSON puede usarse para quitar todas las etiquetas y los puntos de un mapa.

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

En este tema se muestran las entradas y [propiedades](#properties) JSON que puedes usar para personalizar la apariencia de tus mapas.  Estas propiedades también pueden aplicarse a los elementos de mapa de usuario a través de la propiedad [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) .

<a id="entries" />

## <a name="entries"></a>Entradas
En esta tabla se usan caracteres ">" para representar los niveles de la jerarquía de entradas.  También muestra qué versiones de Windows admiten cada entrada y que pasar por alto.

| Version | Nombre de la versión de Windows |
|---------|----------------------|
|  1703   | Creators Update      |
|  1709   | Fall Creators Update |
|  1803   | Actualización de abril de 2018    |
|  1809   | Actualización de octubre de 2018  |

| Nombre                         | Grupo de propiedades            | 1703 | 1709 | 1803 | 1809 | Descripción    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [Version](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | La versión de hoja de estilo que quieres usar. |
| settings                     | [Settings](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | La configuración que se aplica a toda la hoja de estilo. |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | La entrada principal para todas las entradas de mapa. |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | La entrada principal para todas las entradas que no sean del usuario. |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Usan áreas que describe la tierra.  Estos no se deben confundirse con los edificios físicos que están en la entrada de la estructura. |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que abarcan aeropuerto. |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas en que hay una alta concentración de empresas o puntos interesantes. |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que abarcan cementerios. |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Etiquetas de área Continent. |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que abarcan las escuelas y otras instalaciones educativas. |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que abarcan pueblos casos naturales. |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Zonas que se usan para fines industriales. |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Etiquetas de área de isla. |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Las áreas que se usan para fines médicos (por ejemplo: un campus de hospital). |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que abarcan bases militares o tienen usos militares. |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que se usan para fines marítimos de relacionados. |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Etiquetas de área del barrio. |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que se usa como una pista de avión. |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas de arena, como playas. |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas de suelo asignadas para centros comerciales. |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que abarcan estadios. |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas subterráneas (por ejemplo: la superficie de una estación de metro). |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bosques, prados, etc. |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas de tierra forestada. |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que abarcan de golf. |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que abarcan parques. |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Campos extraídos, como un campo de fútbol o pista de tenis. |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas que abarcan reservas naturales. |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Todas las características de punto que se dibujan con un icono de algún tipo. |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | Las etiquetas de los números de la dirección. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan las características naturales. |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan cumbres. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan picos de volcán. |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan ubicaciones de agua, como una cascada. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan cualquier ubicación interesante. |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan cualquier locaiton de empresas. |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan atracciones turística como museos, zoos, etcetera. |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan ubicaciones de uso general a la Comunidad. |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan las escuelas y otras actividades de enseñanza relacionadas con ubicaciones. |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan lugares de entretenimiento, como teatros, salas de cine, etcetera. |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan servicios esenciales como estacionamiento, bancos, gas, etcetera. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan restaurantes, cafés, etcetera. |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan hoteles y otras empresas de presentación. |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan las empresas inmobiliaria. |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan hoteles y otras empresas de presentación. |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan el tamaño de un lugar poblado (por ejemplo: una ciudad o pueblo). |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan la capital de un lugar poblado. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan la capital de un estado o provincia. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan la capital de un país o región. |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Carteles que representan el nombre compacto de una carretera. (Por ejemplo: I-5). Usa solo valores de paleta si estableces la propiedad **ImageFamily** de la entrada de configuración en un valor *Palette*. |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan salidas, normalmente desde una autopista de acceso controlado. |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan paradas de autobús, paradas de tren, aeropuertos, etc. |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Regiones políticas, como países, regiones y estados o provincias. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bordes de la región de país y las etiquetas. |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, Estados, provincias, etc., los bordes y las etiquetas. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, condados, etc., los bordes y las etiquetas. |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edificios y otras estructuras edificadas similares. |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edificios. |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edificios utilizados para educación. |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edificios que se usan para fines médicos como hospitales. |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edificios en tránsito, como aeropuertos. |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que forman parte de la red de transporte (por ejemplo: carreteras, trenes y transbordadores). |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan todas las carreteras. |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan autopistas de acceso de gran tamaño y controlado. |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan rampas de alta velocidad que normalmente se conectan a controlan autopistas de acceso. |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan autopistas. |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan carreteras principales. |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan carreteras arterial. |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan calles. |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan rampas que normalmente se conectan a autopistas. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan unpaved calles. |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan carreteras con peaje. |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas de ferrocarriles. |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Caminos por parques o senderos. |
| >>> existe un pasillo                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Existe un pasillo con privilegios elevados. |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas de ruta de transbordador. |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Todo lo que parezca a agua. Esto incluye océanos y riachuelos. |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Ríos, riachuelos u otros recorridos de agua.  Ten en cuenta que esto puede ser una línea o polígono y puede conectarse a masas de agua que no sean ríos. |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Todas las entradas enrutamiento relacionadas. |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | La ruta de línea relacionados con las entradas. |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan las rutas de conducción. |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Líneas que representan las rutas de conducción pintorescas. |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan walking rutas. |
| > userMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Todas las entradas de usuario. |
| >> userBillboard             | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | El estilo de las instancias [MapBillboard](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapbillboard) predeterminadas. |
| >> userLine                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | El estilo de las instancias [MapPolyline](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mappolyline) predeterminadas. |
| >> userModel3D               | [MapElement3D](#mapelement3d) |      |  ✔   |  ✔   |  ✔   | El estilo de las instancias [MapModel3D](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d) predeterminadas.  Esto va dirigido principalmente al juste de renderAsSurface. |
| >> userPoint                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | El estilo de las instancias [MapIcon](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapicon) predeterminadas. |

<a id="properties" />

## <a name="properties"></a>Propiedades

En esta sección se describen las propiedades que puedes usar para cada entrada.

<a id="version" />

### <a name="version-properties"></a>Propiedades de la versión

| Propiedad                     | Tipo    | Descripción                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | Cadena  | Versión de la hoja de estilo de destino. Se usa para la aplicación. "1.0" para el valor predeterminado "1.*" para actualizaciones menores de características adicionales. |

<a id="settings" />

### <a name="settings-properties"></a>Propiedades de la configuración

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descripción |
|------------------------------|---------|------|------|------|------|-------------|
| atmosphereVisible            | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Marcador que indica si la atmósfera aparece en el control 3D. |
| buildingTexturesVisible      | Bool    |      |      |  ✔   |  ✔   | Una marca que indica si se debe o no mostrar texturas en edificios 3D simbólicos que tienen texturas. |
| fogColor                     | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El valor de color ARGB de la niebla de distancia que aparece en el control 3D. |
| glowColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El valor de color ARGB que se podría aplicarse para etiquetar resplandor y resplandor de icono. |
| imageFamily                  | Cadena  |  ✔   |  ✔   |  ✔   |  ✔   | El nombre de la imagen establecida para usar con este estilo. Establece este valor en *Default* para carteles que usan colores fijos basados en el cartel del mundo real. Establece este valor en *Palette* para carteles que usan colores de paleta configurables. |
| landColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El valor de color ARGB de la tierra antes de que se dibuje algún elemento en ella. |
| logosVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Un marcador que indica si los elementos que tienen una propiedad **Organization** deben dibujar los logotipos adecuados o usar un icono genérico. |
| officialColorVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Un marcador que indica si los elementos que tienen una propiedad de color oficial (por ejemplo, las líneas de transporte público de China) deben dibujar ese color. Por ejemplo, desactiva este valor para un mapa en blanco y negro. |
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Un marcador que indica si se debe o no dibujar regiones de trama donde tienen una representación mejor que vectores (Japón y Corea del sur). |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Un marcador que indica si se va a dibujar o no el sombreado de elevación en el mapa. |
| shadedReliefDarkColor        | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color del lado oscuro del relieve sombreado.  Canal alfa representa el valor alfa máximo. |
| shadedReliefLightColor       | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color del lado claro del relieve sombreado.  Canal alfa representa el valor alfa máximo. |
| shadowColor                  | Color   |      |      |      |  ✔️   | El color de la sombra detrás de los iconos que usan las sombras. |
| spaceColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El valor de color ARGB para la zona que rodea el mapa. |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Un marcador que indica si los colores originales en SVG deben ser usados en lugar de buscar la entrada de la paleta de colores en una imagen. |

<a id="mapelement" />

### <a name="mapelement-properties"></a>Propiedades de MapElement

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descripción |
|------------------------------|---------|------|------|------|------|-------------|
| backgroundScale              | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   | Cantidad en que se debe escalar el elemento en segundo plano de un icono.  Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| fillColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color que se usa para rellenar polígonos, el fondo de los iconos de punto y para el centro de líneas si se han dividido. |
| fontFamily                   | Cadena  |  ✔   |  ✔   |  ✔   |  ✔   |  |
| iconColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color del glifo que se muestra en el medio de un icono de punto. |
| iconScale                    | Flotante   |      |  ✔   |  ✔   |  ✔   | Cantidad en que se debe escalar el glifo de un icono.  Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| labelColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelOutlineColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   |  |
| labelScale                   | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   | La cantidad según la cual se aplicará escala a los tamaños de etiqueta predeterminados. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| labelVisible                 | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| overwriteColor               | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Hace que el valor alfa del elemento **FillColor** sobrescriba el elemento **StrokeColor** en lugar de combinarse con él. |
| scale                        | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   | La cantidad según la cual se aplicará escala al tamaño de todo el punto. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| strokeColor                  | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color que se usará para el contorno de polígonos, el contorno alrededor de los iconos de punto y el color de las líneas. |
| strokeWidthScale             | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   | La cantidad según la cual se aplicará escala al trazado de las líneas. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| visible                      | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |

<a id="borderedmap" />

### <a name="borderedmapelement"></a>BorderedMapElement

Este grupo de propiedades hereda los valores del grupo de propiedades [MapElement](#mapelement).

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descripción |
|------------------------------|---------|------|------|------|------|-------------|
| borderOutlineColor           | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color de línea secundaria o de marco del borde de un polígono rellenado. |
| borderStrokeColor            | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color de la línea principal del borde de un polígono rellenado. |
| borderVisible                | Bool    |  ✔   |  ✔   |  ✔   |  ✔   |  |
| borderWidthScale             | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   | El importe por el cual se aplicará escala al trazado de los bordes. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propiedades de PointStyle

Este grupo de propiedades hereda los valores del grupo de propiedades [MapElement](#mapelement).

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descripción |
|------------------------------|---------|------|------|------|------|-------------|
| Fondo de la forma             | Flotante   |      |      |      |  ✔️   | Forma que se usará como el fondo del icono: reemplazar cualquier forma que existe. |
| stemAnchorRadiusScale        | Flotante   |      |      |  ✔   |  ✔   | Cantidad en que se debe escalar el punto de anclaje del eje de un icono.  Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| stemColor                    | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color de la barra que sale de la parte inferior del icono en modo 3D. |
| stemHeightScale              | Flotante   |      |      |  ✔   |  ✔   | Cantidad en que se debe escalar la longitud del eje de un icono.  Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble de longitud. |
| stemOutlineColor             | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color del contorno alrededor de la barra que sale de la parte inferior del icono en modo 3D. |
| stemWidthScale               | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   | Cantidad en que se debe escalar la anchura del eje de un icono.  Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble de longitud. |

<a id="mapelement3d" />

### <a name="mapelement3d"></a>MapElement3D

Este grupo de propiedades hereda los valores del grupo de propiedades [MapElement](#mapelement).

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descripción |
|------------------------------|---------|------|------|------|------|------------|
| renderAsSurface              | Bool    |      |      |  ✔   |  ✔   | Una marca que indica que un modelo 3D se debe representar como un edificio, sin fundido de profundidad contra el suelo. |
