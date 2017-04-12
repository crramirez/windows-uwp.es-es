---
author: jwmsft
description: "Modifica el comportamiento de la compilación de XAML, por ejemplo, los campos para referencias a objetos con nombre se definen con el acceso público en lugar del comportamiento privado predeterminado."
title: Atributo xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: cad84be24836bc6a33a4ab08f1ca4fa2d9e97512
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="xfieldmodifier-attribute"></a>Atributo x:FieldModifier

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Modifica el comportamiento de la compilación de XAML, por ejemplo, los campos para referencias a objetos con nombre se definen con el acceso **public** en lugar del comportamiento **private** predeterminado.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:FieldModifier="public".../>
```

## <a name="dependencies"></a>Dependencias

El [atributo x:Name](x-name-attribute.md) también debe proporcionarse en el mismo elemento.

## <a name="remarks"></a>Observaciones

El valor del atributo **x:FieldModifier** variará según el lenguaje de programación. Los valores válidos son **private**, **public**, **protected**, **internal** o **friend**. En el caso de C#, Microsoft Visual Basic o las extensiones de componentes de Visual C++ (C++/CX), puedes proporcionar el valor de cadena "public" o "Public"; recuerda que el analizador no fuerza la coincidencia de mayúsculas y minúsculas en este valor de atributo.

El acceso **Private** es el valor predeterminado.

**x:FieldModifier** solo es relevante para elementos con el [atributo x:Name](x-name-attribute.md), ya que ese nombre se usa para hacer referencia al campo una vez que se hace público.

**Nota** El lenguaje XAML de Windows Runtime no admite los elementos **x:ClassModifier** ni **x:Subclass**.

