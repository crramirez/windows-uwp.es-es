---
author: jwmsft
description: Modifica el comportamiento de la compilación de XAML, por ejemplo, los campos para referencias a objetos con nombre se definen con el acceso público en lugar del comportamiento privado predeterminado.
title: Atributo xFieldModifier
ms.assetid: 6FBCC00B-848D-4454-8B1F-287CA8406DDF
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

El valor del atributo **x:FieldModifier** variará según el lenguaje de programación. La cadena que se use dependerá de cómo implemente cada lenguaje su **CodeDomProvider** y del tipo de convertidores que devuelvan para definir el significado de **TypeAttributes.Public** y **TypeAttributes.NotPublic**. En el caso de C#, Microsoft Visual Basic o las extensiones de componentes de Visual C++ (C++/CX), puedes proporcionar el valor de cadena "public" o "Public"; el analizador no fuerza la coincidencia de mayúsculas y minúsculas en este valor de atributo.

También puedes especificar **NonPublic** (**internal** en C# o C++/CX, **Friend** en Visual Basic) pero no es frecuente. El acceso interno no tiene ninguna aplicación en el modelo de generación de código XAML de Windows Runtime. El acceso privado es el predeterminado.

**x:FieldModifier** solo es relevante para elementos con el [atributo x:Name](x-name-attribute.md), porque ese nombre se usa para hacer referencia al campo una vez que se hace público.

**Nota** El XAML de Windows Runtime no admite **x:ClassModifier** ni **x:Subclass**.



<!--HONumber=May16_HO2-->


