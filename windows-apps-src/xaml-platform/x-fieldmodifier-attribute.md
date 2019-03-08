---
description: Modifica el comportamiento de la compilación de XAML, por ejemplo, los campos para referencias a objetos con nombre se definen con el acceso público en lugar del comportamiento privado predeterminado.
title: Atributo xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 751cda36fc58d0e6add9204327a74ec947c9fc53
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660920"
---
# <a name="xfieldmodifier-attribute"></a>Atributo x:FieldModifier


Modifica el comportamiento de la compilación de XAML, por ejemplo, los campos para referencias a objetos con nombre se definen con el acceso **public** en lugar del comportamiento **private** predeterminado.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>Dependencias

El [atributo x:Name](x-name-attribute.md) también debe proporcionarse en el mismo elemento.

## <a name="remarks"></a>Observaciones

El valor del atributo **x:FieldModifier** variará según el lenguaje de programación. Los valores válidos son **private**, **public**, **protected**, **internal** o **friend**. Para C#, extensiones de componentes de Microsoft Visual Basic o Visual C++ (C++ / c++ / CX), puede proporcionar la cadena de valor "public" o "Público". el analizador no obliga a mayúsculas y minúsculas en el valor de este atributo.

El acceso **Private** es el valor predeterminado.

**x:FieldModifier** solo es relevante para elementos con el [atributo x:Name](x-name-attribute.md), ya que ese nombre se usa para hacer referencia al campo una vez que se hace público.

**Tenga en cuenta**  no es compatible con Windows en tiempo de ejecución XAML **x: ClassModifier** o **x: Subclass**.

