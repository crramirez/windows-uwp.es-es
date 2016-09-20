---
author: jwmsft
description: Identifica exclusivamente los elementos que se crean y a los cuales se hace referencia como recursos, y que existen dentro de un ResourceDictionary.
title: Atributo xKey
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.sourcegitcommit: ba620bc89265cbe8756947e1531759103c3cafef
ms.openlocfilehash: 00d801dc3ebb8894f8e21ba0c1b9f3aecc981f30

---

# Atributo x:Key

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Identifica exclusivamente los elementos que se crean y a los cuales se hace referencia como recursos, y que existen dentro de un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794).

## Uso del atributo XAML

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## Uso del atributo XAML (**ResourceDictionary** implícito)

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## Valores de XAML

| Término | Descripción |
|------|-------------|
| objeto | Cualquier objeto que se pueda compartir. Consulta [Referencias a ResourceDictionary y a recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273). |
| stringKeyValue | Se usa una cadena verdadera como clave, que debe cumplir con la gramática _XamlName_. Consulta "Gramática XamlName", a continuación. | 

##   Gramática XamlName

A continuación se muestra la gramática normativa de una cadena que se usa como clave en la implementación de XAML en la Plataforma universal de Windows (UWP):

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Los caracteres están restringidos al intervalo ASCII inferior y, más específicamente, a letras mayúsculas y minúsculas del alfabeto romano, dígitos y el carácter de subrayado (\_).
-   No se admite el intervalo de caracteres Unicode.
-   Un nombre no puede comenzar por un dígito.

## Observaciones

Los elementos secundarios de un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) generalmente incluyen un atributo **x:Key** que especifica un valor de clave único dentro de ese diccionario. El procesador XAML hace cumplir la exclusividad de la clave en tiempo de carga. Los valores **x:Key** que no son únicos generarán errores o excepciones en el analizador. Si la [extensión de marcado {StaticResource}](staticresource-markup-extension.md) lo solicita, una clave no resuelta también generará excepciones de análisis XAML.


            **x:Key** y [x:Name](x-name-attribute.md) no son conceptos idénticos. 
            **x:Key** se usa exclusivamente en los diccionarios de recursos. x:Name se usa para todas las áreas de XAML. Una llamada a [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) usando el valor de clave no recuperará un recurso con clave.

Ten en cuenta que, en la sintaxis implícita que se muestra, el objeto [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) está implícito en la forma en que el procesador XAML produce un objeto nuevo para rellenar una colección [**Resources**](https://msdn.microsoft.com/library/windows/apps/br208740).

El código que equivale a especificar **x:Key** es cualquier operación que use una clave con el [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) subyacente. Por ejemplo, un **x:Key** aplicado en el marcado para un recurso es equivalente al valor del parámetro *key* de **Insert** cuando se agrega un recurso a un **ResourceDictionary**.

Un elemento de un diccionario de recursos puede pasar por alto un valor de **x:Key** si se trata de un [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849) o un [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/br209391) dirigido; en cualquiera de estos casos, la clave implícita del elemento de recurso es el valor de **TargetType** interpretado como una cadena. Para obtener más información, consulta [Inicio rápido: controles de estilo](https://msdn.microsoft.com/library/windows/apps/hh465498) y [Referencias a ResourceDictionary y a los recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).




<!--HONumber=Jun16_HO4-->


