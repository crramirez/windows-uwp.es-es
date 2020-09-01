---
description: Aprenda a usar el atributo x:DefaultBindMode en el marcado XAML para especificar un modo predeterminado para x:Bind que no sea OneTime.
title: atributo xDefaultBindMode
ms.date: 02/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 817b89dc8f3ab7952cdbfc53489eb1afb12024f2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166879"
---
# <a name="xdefaultbindmode-attribute"></a>Atributo x:DefaultBindMode

En el marcado XAML, especifica un modo predeterminado para x:Bind.

**x:DefaultBindMode** está disponible a partir de Windows 10, versión 1607 (actualización de aniversario), versión de SDK 14393.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:DefaultBindMode="OneTime \| OneWay \| TwoWay" .../>
```

## <a name="remarks"></a>Observaciones

[x:Bind](x-bind-markup-extension.md) tiene un modo predeterminado de **onetime**. Esto se eligió por motivos de rendimiento, ya que el uso de **OneWay** hace que se genere más código para establecer el enlace y controlar la detección de cambios. Puede usar **x:DefaultBindMode** para cambiar el modo predeterminado de x:BIND para un segmento específico del árbol de marcado. El modo especificado se aplica a cualquier expresión x:Bind en ese elemento y sus elementos secundarios, que no especifican explícitamente un modo como parte del enlace.

## <a name="related-topics"></a>Temas relacionados

* [extensión de marcado x:Bind](x-bind-markup-extension.md)
