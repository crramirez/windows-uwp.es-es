---
author: jwmsft
description: "Modifica el comportamiento de la compilación de XAML, por ejemplo, los campos para referencias a objetos con nombre se definen con el acceso público en lugar del comportamiento privado predeterminado."
title: Atributo xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
translationtype: Human Translation
ms.sourcegitcommit: 3144758352b99f8c145a3c7be8a6c43d6a002104
ms.openlocfilehash: f812c9498d5519aac8ab750f0c55423966a63464

---

# Atributo x:FieldModifier

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Modifica el comportamiento de la compilación de XAML, por ejemplo, los campos para referencias a objetos con nombre se definen con el acceso **public** en lugar del comportamiento **private** predeterminado.

## Uso del atributo XAML

``` syntax
<object x:FieldModifier="public".../>
```

## Dependencias

El [atributo x:Name](x-name-attribute.md) también debe proporcionarse en el mismo elemento.

## Observaciones

El valor del atributo **x:FieldModifier** variará según el lenguaje de programación. Los valores válidos son **private**, **public**, **protected**, **internal** o **friend**. En el caso de C#, Microsoft Visual Basic o las extensiones de componentes de Visual C++ (C++/CX), puedes proporcionar el valor de cadena "public" o "Public"; recuerda que el analizador no fuerza la coincidencia de mayúsculas y minúsculas en este valor de atributo.

El acceso **Private** es el valor predeterminado.

**x:FieldModifier** solo es relevante para elementos con el [atributo x:Name](x-name-attribute.md), ya que ese nombre se usa para hacer referencia al campo una vez que se hace público.

**Nota** El lenguaje XAML de Windows Runtime no admite los elementos **x:ClassModifier** ni **x:Subclass**.




<!--HONumber=Aug16_HO3-->


