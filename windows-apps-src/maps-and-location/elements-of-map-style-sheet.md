---
description: Entradas y propiedades de una hoja de estilo de mapa
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referencia de hoja de estilo de mapa
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, maps, mapas, map style sheet, hoja de estilo de mapa
ms.localizationpriority: medium
ms.openlocfilehash: f199e08f74ace4e6c8b123a701af19469b029aed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608420"
---
# <a name="map-style-sheet-reference"></a>Referencia de hoja de estilo de mapa

Usan las tecnologías de asignación de Microsoft _hojas de estilos de mapa_ para definir la apariencia de mapas.  Una hoja de estilos de mapa se define mediante JavaScript Object Notation (JSON) y se pueden usar en diversas maneras, como en una aplicación de Windows Store [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol) a través de la [MapStyleSheet.ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) método.

Se pueden crear hojas de estilos interactivamente mediante el [Editor de hojas de estilo de mapa](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) aplicación.

El siguiente JSON se puede usar para crear áreas de agua se muestran en rojo, agua etiquetas aparecen en verde y áreas de land aparecen en azul:

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

En este tema se muestran las entradas y [propiedades](#properties) JSON que puedes usar para personalizar la apariencia de tus mapas.  Estas propiedades también se pueden aplicar a los elementos de mapa de usuario a través de la [MapStyleSheetEntry](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapelement.mapstylesheetentry) propiedad.

<a id="entries" />

## <a name="entries"></a>Entradas
En esta tabla se usan caracteres ">" para representar los niveles de la jerarquía de entradas.  También muestra qué versiones de Windows son compatibles con cada entrada y que pasar por alto.

| Versión | Nombre de la versión de Windows |
|---------|----------------------|
|  1703   | Creators Update      |
|  1709   | Fall Creators Update |
|  1803   | Actualización de abril de 2018    |
|  1809   | Actualización de octubre de 2018  |

| Nombre                         | Grupo de propiedades            | 1703 | 1709 | 1803 | 1809 | Descripción    |
|------------------------------|---------------------------|------|------|------|------|----------------|
| version                      | [Versión](#version)       |  ✔   |  ✔   |  ✔   |  ✔   | La versión de hoja de estilo que quieres usar. |
| configuración                     | [Configuración](#settings)     |  ✔   |  ✔   |  ✔   |  ✔   | La configuración que se aplica a toda la hoja de estilo. |
| mapElement                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | La entrada principal para todas las entradas de mapa. |
| > baseMapElement             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | La entrada principal para todas las entradas que no sean del usuario. |
| >> area                      | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Utilice las áreas que describe la tierra.  Estos no se deben para confundir con los edificios físicos que están bajo la entrada de la estructura. |
| >>> airport                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan los aeropuertos. |
| >>> areaOfInterest           | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas en que hay una alta concentración de empresas o puntos interesantes. |
| >>> cemetery                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan cementerios. |
| >>> continent                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Etiquetas de área continente. |
| >>> education                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan las escuelas y otras instalaciones educativos. |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Las áreas que abarcan pueblos indígenas reserva. |
| >>> industrial               | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas que se usan para fines industriales. |
| >>> island                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Etiquetas de área de la isla. |
| >>> medical                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que se usan para fines médicos (por ejemplo: un campus de hospital). |
| >>> military                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan bases militares o tienen usos militares. |
| >>> nautical                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que se usan para fines relacionados náuticas. |
| >>> neighborhood             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Etiquetas de área del vecindario. |
| >>> runway                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que se utiliza como una pista de aterrizaje de un avión. |
| >>> sand                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas de arena, como playas. |
| >>> shoppingCenter           | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas de suelo asignadas para centros comerciales. |
| >>> stadium                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan estadios. |
| >>> underground              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Áreas subterráneas (por ejemplo: la superficie de una estación de metro). |
| >>> vegetation               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Bosques, prados, etc. |
| >>>> forest                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Zonas de tierra forestada. |
| >>>> golfCourse              | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan de golf. |
| >>>> park                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Áreas que abarcan los parques. |
| >>>> playingField            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Campos extraídos, como un campo de fútbol o pista de tenis. |
| >>>> reserve                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Reserva de las áreas que abarcan la naturaleza. |
| >> point                     | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Todas las características de punto que se dibujan con un icono de algún tipo. |
| >>> address                  | [PointStyle](#pointstyle) |      |      |  ✔   |  ✔   | Números de las etiquetas de la dirección. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan las características físicas. |
| >>>> peak                    | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan cumbres. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan picos de volcán. |
| >>>> waterPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan ubicaciones de agua, como una cascada. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan cualquier ubicación interesante. |
| >>>> business                | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan cualquier ubicación de la empresa. |
| >>>>> attractionPoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan atracciones turísticos como museos, zoológicos, etcetera. |
| >>>>> communityPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan las ubicaciones de uso general a la Comunidad. |
| >>>>> educationPoint         | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan las escuelas y otro educación relacionados con las ubicaciones. |
| >>>>> entertainmentPoint     | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan los lugares de entretenimiento como teatros, cines, etcetera. |
| >>>>> essentialServicePoint  | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan servicios esenciales como estacionamiento, bancos, gas, etcetera. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan los restaurantes, cafés, etcetera. |
| >>>>> lodgingPoint           | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan los hoteles y otras empresas de alojamiento. |
| >>>>> realEstatePoint        | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan las empresas de bienes raíces. |
| >>>>> shoppingPoint          | [PointStyle](#pointstyle) |      |  ✔   |  ✔   |  ✔   | Iconos que representan los hoteles y otras empresas de alojamiento. |
| >>> populatedPlace           | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan el tamaño de un lugar poblado (por ejemplo: una ciudad o pueblo). |
| >>>> capital                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan la capital de un lugar poblado. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan la capital de un estado o provincia. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan la capital de un país o región. |
| >>> roadShield               | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Carteles que representan el nombre compacto de una carretera. (Por ejemplo: I-5). Usa solo valores de paleta si estableces la propiedad **ImageFamily** de la entrada de configuración en un valor *Palette*. |
| >>> roadExit                 | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan salidas, normalmente desde una autopista de acceso controlado. |
| >>> transit                  | [PointStyle](#pointstyle) |  ✔   |  ✔   |  ✔   |  ✔   | Iconos que representan paradas de autobús, paradas de tren, aeropuertos, etc. |
| >> political                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Regiones políticas, como países, regiones y estados o provincias. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Los bordes de región del país y las etiquetas. |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin1, Estados, provincias, etc., los bordes y las etiquetas. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Admin2, provincias, etc., los bordes y las etiquetas. |
| >> structure                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edificios y otras estructuras edificadas similares. |
| >>> building                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edificios. |
| >>>> educationBuilding       | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edificios utilizados para educación. |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edificios destinadas aplicaciones médicas, como hospitales. |
| >>>> transitBuilding         | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Edificios en tránsito como aeropuertos. |
| >> transportation            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que forman parte de la red de transporte (por ejemplo: carreteras, trenes y transbordadores). |
| >>> road                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan todas las carreteras. |
| >>>> controlledAccessHighway | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan autopistas grandes acceso controlado. |
| >>>>> highSpeedRamp          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Las líneas que representan rampas de alta velocidad que normalmente se conectan a controlan autopistas de acceso. |
| >>>> highway                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan las autopistas. |
| >>>> majorRoad               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan las principales carreteras. |
| >>>> arterialRoad            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan las carreteras arterial. |
| >>>> street                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan calles. |
| >>>>> ramp                   | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan rampas que normalmente se conectan a autopistas. |
| >>>>> unpavedStreet          | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan calles unpaved. |
| >>>> tollRoad                | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan las carreteras que cuestan dinero a usar. |
| >>> railway                  | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas de ferrocarriles. |
| >>> trail                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Caminos por parques o senderos. |
| >>> existe un pasillo                  | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Existe un pasillo con privilegios elevados. |
| >>> waterRoute               | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas de ruta de transbordador. |
| >> water                     | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Todo lo que parezca a agua. Esto incluye océanos y riachuelos. |
| >>> river                    | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Ríos, riachuelos u otros recorridos de agua.  Ten en cuenta que esto puede ser una línea o polígono y puede conectarse a masas de agua que no sean ríos. |
| > routeMapElement            | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Todas las entradas relacionadas enrutamientos. |
| >> routeLine                 | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Línea de ruta entradas relacionadas. |
| >>> drivingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan rutas de conducción. |
| >>> scenicRoute              | [MapElement](#mapelement) |      |  ✔   |  ✔   |  ✔   | Líneas que representan rutas de conducción scenic. |
| >>> walkingRoute             | [MapElement](#mapelement) |  ✔   |  ✔   |  ✔   |  ✔   | Líneas que representan el recorrido de las rutas. |
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
| rasterRegionsVisible         | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Una marca que indica si se va a dibujar las regiones de mapa de bits que tienen una mejor representación de vectores (Japón y Corea). |
| shadedReliefVisible          | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Un marcador que indica si se va a dibujar o no el sombreado de elevación en el mapa. |
| shadedReliefDarkColor        | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color del lado oscuro del relieve sombreado.  Canal alfa representa el máximo valor alfa. |
| shadedReliefLightColor       | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El color del lado claro del relieve sombreado.  Canal alfa representa el máximo valor alfa. |
| shadowColor                  | Color   |      |      |      |  ✔   | El color de la sombra detrás de los iconos que usan las sombras. |
| spaceColor                   | Color   |  ✔   |  ✔   |  ✔   |  ✔   | El valor de color ARGB para la zona que rodea el mapa. |
| useDefaultImageColors        | Bool    |  ✔   |  ✔   |  ✔   |  ✔   | Una marca que indica si los colores en el formato SVG originales deben ser utilizado en lugar de buscar la entrada de la paleta de colores en una imagen. |

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
| borderWidthScale             | Flotante   |  ✔   |  ✔   |  ✔   |  ✔   | La cantidad por la que se escalan el trazo de bordes. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |

<a id="pointstyle" />

### <a name="pointstyle-properties"></a>Propiedades de PointStyle

Este grupo de propiedades hereda los valores del grupo de propiedades [MapElement](#mapelement).

| Propiedad                     | Tipo    | 1703 | 1709 | 1803 | 1809 | Descripción |
|------------------------------|---------|------|------|------|------|-------------|
| Fondo de la forma             | Flotante   |      |      |      |  ✔   | Forma que se usará como el fondo del icono, reemplazando cualquier forma que exista. |
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
