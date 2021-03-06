---
description: Identifica exclusivamente los elementos que se crean y a los cuales se hace referencia como recursos, y que existen dentro de un ResourceDictionary.
title: Atributo xKey
ms.assetid: 141FC5AF-80EE-4401-8A1B-17CB22C2277A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 45cdc3bcf766cd110498e357052da150ea96fa21
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161699"
---
# <a name="xkey-attribute"></a>Atributo x:Key


Identifica exclusivamente los elementos que se crean y a los cuales se hace referencia como recursos, y que existen dentro de un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary).

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<ResourceDictionary>
  <object x:Key="stringKeyValue".../>
</ResourceDictionary>
```

## <a name="xaml-attribute-usage-implicit-resourcedictionary"></a>Uso del atributo XAML (**ResourceDictionary** implícito)

``` syntax
<object.Resources>
  <object x:Key="stringKeyValue".../>
</object.Resources>
```

## <a name="xaml-values"></a>Valores de XAML

| Término | Descripción |
|------|-------------|
| object | Cualquier objeto que se pueda compartir. Consulta [Referencias a ResourceDictionary y a recursos XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md). |
| stringKeyValue | Se usa una cadena verdadera como clave, que debe cumplir con la gramática _XamlName_. Consulta "Gramática XamlName", a continuación. | 

##  <a name="xamlname-grammar"></a> Gramática XamlName

A continuación se muestra la gramática normativa de una cadena que se usa como clave en la implementación de XAML en la Plataforma universal de Windows (UWP):

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Los caracteres están restringidos al intervalo ASCII inferior y, más específicamente, a letras mayúsculas y minúsculas del alfabeto latino, dígitos y el carácter de subrayado ( \_ ).
-   No se admite el intervalo de caracteres Unicode.
-   Un nombre no puede comenzar por un dígito.

## <a name="remarks"></a>Observaciones

Los elementos secundarios de un [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) generalmente incluyen un atributo **x:Key** que especifica un valor de clave único dentro de ese diccionario. El procesador XAML hace cumplir la exclusividad de la clave en tiempo de carga. Los valores **x:Key** que no son únicos generarán errores o excepciones en el analizador. Si la [extensión de marcado {StaticResource}](staticresource-markup-extension.md) lo solicita, una clave no resuelta también generará excepciones de análisis XAML.

**x:Key** y [x:Name](x-name-attribute.md) no son conceptos idénticos. **x:Key** se usa exclusivamente en los diccionarios de recursos. x:Name se usa para todas las áreas de XAML. Una llamada a [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) mediante el valor de clave no recuperará un recurso con clave. Es posible que los objetos definidos en un diccionario de recursos tengan el elemento **x:Key**, **x:Name** o ambos. No es necesario que la clave y el nombre coincidan.

Ten en cuenta que, en la sintaxis implícita que se muestra, el objeto [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) está implícito en la forma en que el procesador XAML produce un objeto nuevo para rellenar una colección [**Resources**](/uwp/api/windows.ui.xaml.frameworkelement.resources).

El código que equivale a especificar **x:Key** es cualquier operación que use una clave con el [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) subyacente. Por ejemplo, un **x:Key** aplicado en el marcado para un recurso es equivalente al valor del parámetro *key* de **Insert** cuando se agrega un recurso a un **ResourceDictionary**.

Un elemento de un diccionario de recursos puede pasar por alto un valor de **x:Key** si se trata de un [**Style**](/uwp/api/Windows.UI.Xaml.Style) o un [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) dirigido; en cualquiera de estos casos, la clave implícita del elemento de recurso es el valor de **TargetType** interpretado como una cadena. Para obtener más información, consulta [Inicio rápido: controles de estilo](/previous-versions/windows/apps/hh465498(v=win.10)) y [Referencias a ResourceDictionary y a los recursos XAML](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).