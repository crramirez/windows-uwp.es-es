---
description: En un marcado XAML, especifica un modo predeterminado para x:Bind.
title: Atributo xDefaultBindMode
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c8917b09f04206a5466797f48414defeb35baf5e
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7970079"
---
# <a name="xdefaultbindmode-attribute"></a>Atributo x:DefaultBindMode

En un marcado XAML, especifica un modo predeterminado para x:Bind.

**x:DefaultBindMode** está disponible a partir de Windows 10, versión 1607 (Actualización de aniversario), versión 14393 del SDK.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>Comentarios

[x:Bind](x-bind-markup-extension.md) tiene un modo predeterminado de **OneTime**. Esto se escogió por motivos de rendimiento, ya que usar **OneWay** provocará que se genere más código para enlazar y gestionar la detección del cambio. Puedes usar **x:DefaultBindMode** para cambiar el modo predeterminado de x:Bind para un segmento específico del árbol de marcado. El modo especificado ser aplica a las expresiones de x:Bind en ese elemento y sus elementos secundarios, que no especifican explícitamente un modo como parte del enlace.

## <a name="related-topics"></a>Temas relacionados

* [Extensión de marcado x:Bind](x-bind-markup-extension.md)
