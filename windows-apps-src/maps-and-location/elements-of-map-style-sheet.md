---
description: Las entradas y propiedades de una hoja de estilos de mapa
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Referencia de hoja de estilo de mapa
ms.date: 03/19/2017
ms.topic: article
keywords: Windows 10, UWP, mapas, hoja de estilos de mapa
ms.localizationpriority: medium
ms.openlocfilehash: ec30fd5d63dfa522ef721d2d8a2e4950e6e8e854
ms.sourcegitcommit: c9edb164356c0843d8a9b19ab43707d486e392e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2020
ms.locfileid: "85377936"
---
# <a name="map-style-sheet-reference"></a>Referencia de hoja de estilo de mapa

Las tecnologías de asignación de Microsoft usan [hojas de estilos de mapa](https://docs.microsoft.com/BingMaps/styling/map-style-sheets) para definir el aspecto de las asignaciones, incluidas las que se muestran en el [MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)de una aplicación de la tienda Windows.  Una hoja de estilos de mapa se define mediante notación de objetos JavaScript (JSON) a través del método [MapStyleSheet. ParseFromJson](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) .

Una hoja de estilos consta de [entradas](https://docs.microsoft.com/BingMaps/styling/map-style-sheet-entries) cuyas propiedades se reemplazan para cambiar la apariencia final del mapa.

Por ejemplo, el siguiente JSON se puede usar para que las áreas de agua aparezcan en rojo, las etiquetas de agua aparecen en verde y las áreas de tierra aparecen en azul:

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

Las hojas de estilos se pueden crear de forma interactiva mediante la aplicación del [Editor de hojas de estilos de mapa](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) .
