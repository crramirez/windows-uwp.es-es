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
ms.openlocfilehash: 9e2605917027d7be96ac86421e83082aa5f368f9
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2017
---
# <a name="map-style-sheet-reference"></a>Referencia de hoja de estilo de mapa

Puedes crear hojas de estilo de mapa mediante la notación de objetos JavaScript (JSON).

Por ejemplo, deberías usar la siguiente notación JSON para hacer que las zonas de agua aparezcan en color rojo, las etiquetas de agua aparezcan en verde y las zonas de tierra aparezcan en azul:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000", "labelColor":"#00FF00"}}
    }
```
También podrías usar la notación JSON para quitar todas las etiquetas y los puntos de un mapa.

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

En este tema se muestran las entradas y [propiedades](#properties) JSON que puedes usar para personalizar la apariencia de tus mapas.

<span id="entries" />
## <a name="entries"></a>Entradas
En esta tabla se usan caracteres ">" para representar los niveles de la jerarquía de entradas.   

| Nombre                         | Grupo de propiedades            | Descripción    |
|------------------------------|---------------------------|----------------|
| version                      | [Version](#version)       | La versión de hoja de estilo que quieres usar. |
| settings                     | [Settings](#settings)     | La configuración que se aplica a toda la hoja de estilo. |
| mapElement                   | [MapElement](#mapelement) | La entrada principal para todas las entradas de mapa. |
| > baseMapElement             | [MapElement](#mapelement) | La entrada principal para todas las entradas que no sean del usuario. |
| >> area                      | [MapElement](#mapelement) | Las zonas de uso de tierra (no deben confundirse con la entrada de estructura). |
| >>> airport                  | [MapElement](#mapelement) | Zonas que abarcan un aeropuerto. |
| >>> cemetery                 | [MapElement](#mapelement) | Zonas de cementerios. |
| >>> continent                | [MapElement](#mapelement) | Zonas de continentes enteros. |
| >>> education                | [MapElement](#mapelement) |  |
| >>> indigenousPeoplesReserve | [MapElement](#mapelement) |  |
| >>> island                   | [MapElement](#mapelement) | Etiquetas en zonas de isla. |
| >>> medical                  | [MapElement](#mapelement) | Zonas de tierra que se usan para fines médicos (por ejemplo: un campus de hospital). |
| >>> military                 | [MapElement](#mapelement) | Zonas de bases militares. |
| >>> nautical                 | [MapElement](#mapelement) | Zonas de tierra que se usan para fines marítimos. |
| >>> neighborhood             | [MapElement](#mapelement) | Etiquetas en zonas definidas como vecindarios. |
| >>> runway                   | [MapElement](#mapelement) | Zonas de tierra que están cubiertas por una pista. |
| >>> sand                     | [MapElement](#mapelement) | Zonas de arena, como playas. |
| >>> shoppingCenter           | [MapElement](#mapelement) | Zonas de suelo asignadas para centros comerciales. |
| >>> stadium                  | [MapElement](#mapelement) | Zona de un estadio. |
| >>> vegetation               | [MapElement](#mapelement) | Bosques, prados, etc. |
| >>>> forest                  | [MapElement](#mapelement) | Zonas de tierra forestada. |
| >>>> golfCourse              | [MapElement](#mapelement) |  |
| >>>> park                    | [MapElement](#mapelement) | Zonas de parque. |
| >>>> playingField            | [MapElement](#mapelement) | Campos extraídos, como un campo de fútbol o pista de tenis. |
| >>>> reserve                 | [MapElement](#mapelement) | Zonas de reservas naturales. |
| >> point                     | [PointStyle](#pointstyle) | Todas las características de punto que se representan con un icono de algún tipo. |
| >>> naturalPoint             | [PointStyle](#pointstyle) |  |
| >>>> peak                    | [PointStyle](#pointstyle) | Iconos que representan cumbres. |
| >>>>> volcanicPeak           | [PointStyle](#pointstyle) | Iconos que representan picos de volcán. |
| >>>> waterPoint              | [PointStyle](#pointstyle) | Iconos que representan ubicaciones de agua, como una cascada. |
| >>> pointOfInterest          | [PointStyle](#pointstyle) | Restaurantes, hospitales, escuelas, puertos deportivos, zonas de esquí, etc. |
| >>>> business                | [PointStyle](#pointstyle) | Restaurantes, hospitales, escuelas, etc. |
| >>>>> foodPoint              | [PointStyle](#pointstyle) | Restaurantes, cafés, etc. |
| >>> populatedPlace           | [PointStyle](#pointstyle) | Iconos que representan el tamaño de un lugar poblado (por ejemplo: una ciudad o pueblo). |
| >>>> capital                 | [PointStyle](#pointstyle) | Iconos que representan la capital de un lugar poblado. |
| >>>>> adminDistrictCapital   | [PointStyle](#pointstyle) | Iconos que representan la capital de un estado o provincia. |
| >>>>> countryRegionCapital   | [PointStyle](#pointstyle) | Iconos que representan la capital de un país o región. |
| >>> roadShield               | [PointStyle](#pointstyle) | Carteles que representan el nombre compacto de una carretera. (Por ejemplo: I-5). Usa solo valores de paleta si estableces la propiedad **ImageFamily** de la entrada de configuración en un valor *Palette*. |
| >>> roadExit                 | [PointStyle](#pointstyle) | Iconos que representan salidas, normalmente desde una autopista de acceso controlado. |
| >>> transit                  | [PointStyle](#pointstyle) | Iconos que representan paradas de autobús, paradas de tren, aeropuertos, etc. |
| >> political                 | [BorderedMapElement](#borderedmapelement) | Regiones políticas, como países, regiones y estados o provincias. |
| >>> countryRegion            | [BorderedMapElement](#borderedmapelement) |  |
| >>> adminDistrict            | [BorderedMapElement](#borderedmapelement) | Admin1, estados, provincias, etc. |
| >>> district                 | [BorderedMapElement](#borderedmapelement) | Admin2, condados, etc. |
| >> structure                 | [MapElement](#mapelement) | Edificios y otras estructuras edificadas similares. |
| >>> building                 | [MapElement](#mapelement) |  |
| >>>> educationBuilding       | [MapElement](#mapelement) |  |
| >>>> medicalBuilding         | [MapElement](#mapelement) |  |
| >>>> transitBuilding         | [MapElement](#mapelement) |  |
| >> transportation            | [TwoToneLineStyle](#twotonelinestyle) | Líneas que forman parte de la red de transporte (por ejemplo: carreteras, trenes y transbordadores). |
| >>> road                     | [TwoToneLineStyle](#twotonelinestyle) | Líneas que representan todas las carreteras. |
| >>>> controlledAccessHighway | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>>> highSpeedRamp          | [TwoToneLineStyle](#twotonelinestyle) | Líneas que representan rampas. Estas tablas normalmente aparecen junto a autopistas de acceso controlado. |
| >>>> highway                 | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> majorRoad               | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> arterialRoad            | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> street                  | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>>> ramp                   | [TwoToneLineStyle](#twotonelinestyle) | Líneas que representan la entrada y salida de una autopista. |
| >>>>> unpavedStreet          | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>>> tollRoad                | [TwoToneLineStyle](#twotonelinestyle) | Carreteras con peaje. |
| >>> railway                  | [TwoToneLineStyle](#twotonelinestyle) | Líneas de ferrocarriles. |
| >>> trail                    | [TwoToneLineStyle](#twotonelinestyle) | Caminos por parques o senderos. |
| >>> waterRoute               | [TwoToneLineStyle](#twotonelinestyle) | Líneas de ruta de transbordador. |
| >> water                     | [MapElement](#mapelement) | Todo lo que parezca a agua. Esto incluye océanos y riachuelos. |
| >>> river                    | [MapElement](#mapelement) | Ríos, riachuelos u otros recorridos de agua.  Ten en cuenta que esto puede ser una línea o polígono y puede conectarse a masas de agua que no sean ríos. |
| > routeMapElement            | [MapElement](#mapelement) | Todas las entradas de enrutamiento bajo esta entrada. |
| >> routeLine                 | [TwoToneLineStyle](#twotonelinestyle) | El estilo de todas las líneas de ruta. |
| >>> walkingRoute             | [TwoToneLineStyle](#twotonelinestyle) |  |
| >>> drivingRoute             | [TwoToneLineStyle](#twotonelinestyle) |  |
| > userMapElement             | [MapElement](#mapelement) | Todas las entradas de usuario bajo esta entrada. |
| >> userPoint                 | [PointStyle](#pointstyle) | El estilo de los puntos de usuario predeterminados. |
| >> userLine                  | [MapElement](#mapelement) | El estilo de las líneas de usuario predeterminadas. |

<span id="properties" />
## <a name="properties"></a>Propiedades

En esta sección se describen las propiedades que puedes usar para cada entrada.

<span id="version" />
### <a name="version-properties"></a>Propiedades de la versión

| Propiedad                     | Tipo    | Descripción                                                                                                           |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| version                      | Cadena  | Versión de la hoja de estilo de destino. Se usa para la aplicación. "1.0" para el valor predeterminado "1.*" para actualizaciones menores de características adicionales. |

<span id="settings" />
### <a name="settings-properties"></a>Propiedades de la configuración

| Propiedad                     | Tipo    | Descripción                                                                                                                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| atmosphereVisible            | Bool    | Marcador que indica si la atmósfera aparece en el control 3D.                                                                                                                                                     |
| fogColor                     | Color   | El valor de color ARGB de la niebla de distancia que aparece en el control 3D.                                                                                                                                                    |
| glowColor                    | Color   | El valor de color ARGB que se podría aplicarse para etiquetar resplandor y resplandor de icono.                                                                                                                                                     |
| imageFamily                  | Cadena  | El nombre de la imagen establecida para usar con este estilo. Establece este valor en *Default* para carteles que usan colores fijos basados en el cartel del mundo real. Establece este valor en *Palette* para carteles que usan colores de paleta configurables. |
| landColor                    | Color   | El valor de color ARGB de la tierra antes de que se dibuje algún elemento en ella.                                                                                                                                                     |
| logosVisible                 | Bool    | Un marcador que indica si los elementos que tienen una propiedad **Organization** deben dibujar los logotipos adecuados o usar un icono genérico.                                                                                         |
| officialColorVisible         | Bool    | Un marcador que indica si los elementos que tienen una propiedad de color oficial (por ejemplo, las líneas de transporte público de China) deben dibujar ese color. Por ejemplo, desactiva este valor para un mapa en blanco y negro.                               |
| rasterRegionsVisible         | Bool    | Un marcador que indica si se deben o no dibujar regiones de trama en las que las representaciones no se hacen por vectores (por ejemplo: Japón y Corea del sur).                                                                                                |
| shadedReliefVisible          | Bool    | Un marcador que indica si se va a dibujar o no el sombreado de elevación en el mapa.                                                                                                                                                  |
| shadedReliefDarkColor        | Color   | El color del lado oscuro del relieve sombreado.  El canal alfa representa el valor alfa máximo. value.                                                                                                                            |
| shadedReliefLightColor       | Color   | El color del lado claro del relieve sombreado.  El canal alfa representa el valor alfa máximo. value.                                                                                                                           |
| spaceColor                   | Color   | El valor de color ARGB para la zona que rodea el mapa.                                                                                                                                                                               |
| useDefaultImageColors        | Bool    | Un marcador que indica si se deben usar los colores originales en SVG en lugar de buscar la entrada de la paleta de colores en una imagen.                                                                                |

<span id="mapelement" />
### <a name="mapelement-properties"></a>Propiedades de MapElement

| Propiedad                     | Tipo    | Descripción                                                                                                                 |
|------------------------------|---------|-----------------------------------------------------------------------------------------------------------------------------|
| fillColor                    | Color   | El color que se usa para rellenar polígonos, el fondo de los iconos de punto y para el centro de líneas si se han dividido. |
| fontFamily                   | Cadena  |                                                                                                                             |
| labelColor                   | Color   |                                                                                                                             |
| labelOutlineColor            | Color   |                                                                                                                             |
| labelScale                   | Flotante   | La cantidad según la cual se aplicará escala a los tamaños de etiqueta predeterminados. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño.            |
| labelVisible                 | Bool    |                                                                                                                             |
| strokeColor                  | Color   | El color que se usará para el contorno de polígonos, el contorno alrededor de los iconos de punto y el color de las líneas.                   |
| strokeWidthScale             | Flotante   | La cantidad según la cual se aplicará escala al trazado de las líneas. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño.            |
| visible                      | Bool    |                                                                                                                             |

<span id="borderedmap" />
### <a name="borderedmapelement"></a>BorderedMapElement

Este grupo de propiedades hereda los valores del grupo de propiedades [MapElement](#mapelement).

| Propiedad                     | Tipo    | Descripción                                                           |
|------------------------------|---------|-----------------------------------------------------------------------|
| borderOutlineColor           | Color   | El color de línea secundaria o de marco del borde de un polígono rellenado. |
| borderStrokeColor            | Color   | El color de la línea principal del borde de un polígono rellenado.             |
| borderVisible                | Bool    |                                                                       |
| borderWidthScale             | Flotante   |                                                                       |

<span id="pointstyle" />
### <a name="pointstyle-properties"></a>Propiedades de PointStyle

Este grupo de propiedades hereda los valores del grupo de propiedades [MapElement](#mapelement).

| Propiedad                     | Tipo    | Descripción                                                                                                        |
|------------------------------|---------|--------------------------------------------------------------------------------------------------------------------|
| iconColor                    | Color   | El color del glifo que se muestra en el medio de un icono de punto.                                                        |
| scale                        | Flotante   | La cantidad según la cual se aplicará escala al tamaño de todo el punto. Por ejemplo, usa *1* para el valor predeterminado y *2* para el doble del tamaño. |
| stemColor                    | Color   | El color de la barra que sale de la parte inferior del icono en modo 3D.                                             |
| stemOutlineColor             | Color   | El color del contorno alrededor de la barra que sale de la parte inferior del icono en modo 3D.                          |

<span id="twotonelinestyle" />
### <a name="twotonelinestyle-properties"></a>Propiedades de TwoToneLineStyle

Este grupo de propiedades hereda los valores del grupo de propiedades [MapElement](#mapelement).

| Propiedad                     | Tipo    | Descripción                                                                                          |
|------------------------------|---------|------------------------------------------------------------------------------------------------------|
| overwriteColor               | Bool    |  Hace que el valor alfa del elemento **FillColor** sobrescriba el elemento **StrokeColor** en lugar de combinarse con él. |
