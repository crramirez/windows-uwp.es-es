---
author: jwmsft
description: En un marcado XAML, especifica un modo predeterminado para x:Bind.
title: Atributo xDefaultBindMode
ms.author: jimwalk
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2696cb46591757421795b15083ea7fdab54943c5
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7307082"
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
