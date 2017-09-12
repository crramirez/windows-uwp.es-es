---
author: tbd
description: En un marcado XAML, especifica un modo predeterminado para x:Bind.
title: "Extensión de marcado xDefaultBindMode"
ms.assetid: 
ms.author: 
ms.date: 
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 0fa037b4c59566cb1b9bacd4d2e36520a86c508d
ms.sourcegitcommit: ba0d20f6fad75ce98c25ceead78aab6661250571
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2017
---
# <a name="xdefaultbindmode-markup-extension"></a>Extensión de marcado {x:DefaultBindMode}

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En un marcado XAML, especifica un modo predeterminado para x:Bind.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>Comentarios

x: Bind tiene un modo predeterminado de OneTime, y esto se escogió por motivos de rendimiento, ya que usar OneTime provocará que se genere más código para enlazar y gestionar la detección del cambio. **x:DefaultBindMode** puede usarse para cambiar el modo predeterminado de x:Bind para un segmento específico del árbol de marcado. El modo seleccionado aplicará las expresiones de x:Bind en ese elemento y sus elementos secundarios, que no especifican explícitamente un modo como parte del enlace.

## <a name="related-topics"></a>Temas relacionados

* [**x:Bind**](https://docs.microsoft.com/en-us/windows/uwp/xaml-platform/x-bind-markup-extension)
