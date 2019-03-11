---
description: Identifica de forma única elementos de objeto para acceder al objeto con instancia desde el código subyacente o el código general.
title: Atributo xName
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ef1a6047a7c462961f40ae8913881125e2331bb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618270"
---
# <a name="xname-attribute"></a>Atributo x:Name


Identifica de forma única elementos de objeto para acceder al objeto con instancia desde el código subyacente o el código general. Una vez aplicado a un modelo de programación de respaldo, se puede considerar que **x:Name** es equivalente a la variable que contiene una referencia de objeto, tal como la devuelve un constructor.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:Name="XAMLNameValue".../>
```

## <a name="xaml-values"></a>Valores de XAML

| Término | Descripción |
|------|-------------|
| XAMLNameValue | Una cadena que cumple con las restricciones de la gramática XamlName. |

##  <a name="xamlname-grammar"></a> Gramática XamlName

La siguiente es la gramática normativa de una cadena que se usa como clave en esta implementación de XAML:

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Los caracteres se restringen al intervalo ASCII menor y más concretamente en mayúsculas del alfabeto latino y letras minúsculas, dígitos y el carácter de subrayado (\_) caracteres.
-   No se admite el intervalo de caracteres Unicode.
-   Un nombre no puede comenzar por un dígito. Algunas implementaciones de la herramienta anteponen un carácter de subrayado (\_) en una cadena si el usuario proporciona un dígito como carácter inicial o la herramienta de autogenera **x: Name** valores según otros valores que contienen dígitos.

## <a name="remarks"></a>Observaciones

El **x:Name** especificado se convierte en el nombre de un campo que se crea en el código subyacente cuando se procesa el código XAML, y ese campo contiene una referencia al objeto. El proceso de creación de este campo se realiza mediante los pasos de destino de MSBuild, que son los responsables de unir las clases parciales de un archivo XAML con su código subyacente. Este comportamiento no está necesariamente especificado por el lenguaje XAML; es la implementación particular que la programación de la plataforma universal de Windows (UWP) para XAML aplica para usar **x:Name** en sus modelos de programación y aplicación.

Cada **x:Name** definido debe ser único dentro de un ámbito de nombres XAML. Por lo general, un ámbito de nombres XAML se define en el nivel de elemento raíz de una página cargada y contiene todos los elementos situados bajo ese elemento en una sola página XAML. Los ámbitos de nombres XAML adicionales se definen mediante cualquier plantilla de control o plantilla de datos definida en esa página. En tiempo de ejecución, se crea otro ámbito de nombres XAML para la raíz del árbol de objetos que se crea a partir de una plantilla de control aplicada y mediante los árboles de objetos creados a partir de una llamada a [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048). Para obtener más información, consulta [Ámbitos de nombres XAML](xaml-namescopes.md).

Las herramientas de diseño suelen generar valores **x:Name** automáticamente para los elementos cuando se incorporan a la superficie de diseño. El esquema de generación automática varía según el diseñador que uses, pero un esquema típico consiste en generar una cadena que comience con el nombre de la clase que respalda el elemento, seguida de un entero anticipado. Por ejemplo, si presentas el primer elemento [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) al diseñador, es posible que veas que, en el código XAML, el valor del atributo **x:Name** de este elemento sea "Button1".

**x:Name** no se puede establecer en la sintaxis de elementos de propiedad de XAML ni en el código usando [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361). **x:Name** solo se puede establecer con la sintaxis de atributo XAML en los elementos.

**Tenga en cuenta**  específicamente para C++ / c++ / CX aplicaciones, un campo de respaldo para una **x: Name** referencia no se creó para el elemento raíz de un archivo XAML o una página. Si necesitas hacer referencia al objeto raíz desde el código subyacente en C++, usa otras API o un cruce seguro de árbol. Por ejemplo, puedes llamar a [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715) para un elemento secundario con nombre conocido y, después, llamar a [**Parent**](https://msdn.microsoft.com/library/windows/apps/br208739).

### <a name="xname-and-other-name-properties"></a>x:Name y otras propiedades de Name

Algunos tipos usados en XAML de UWP también tienen una propiedad denominada **Name**. Por ejemplo, [**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) y [**TextElement.Name**](https://msdn.microsoft.com/library/windows/apps/hh702125).

Si **Name** está disponible como propiedad que puede establecerse en un elemento, se pueden usar **Name** y **x:Name** indistintamente en XAML; sin embargo, se genera un error si ambos atributos están especificados en el mismo elemento. También hay casos en los que hay una propiedad **Name** pero es de solo lectura (como [**VisualState.Name**](https://msdn.microsoft.com/library/windows/apps/br209031)). En ese caso, siempre usas **x:Name** para denominar ese elemento en XAML y **Name** de solo lectura existe en escenarios de código menos comunes.

**Nota** El elemento   [**FrameworkElement.Name**](https://msdn.microsoft.com/library/windows/apps/br208735) no debería usarse para cambiar valores establecidos originalmente por **x:Name**, aunque hay algunas excepciones a esa regla general. En los escenarios típicos, la creación y la definición de ámbitos de nombres XAML es una operación del procesador XAML. La modificación de **FrameworkElement.Name** en tiempo de ejecución puede producir una alineación de nombres incorrecta entre el ámbito de nombres XAML y el campo privado, lo que es difícil de rastrear en el código subyacente.

### <a name="xname-and-xkey"></a>x:Name y x:Key

**x:Name** puede aplicarse como un atributo a los elementos de un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) para actuar como un sustituto del [atributo x:Key](x-key-attribute.md). (Se trata de una regla que todos los elementos de un **ResourceDictionary** debe tener un atributo x: Key o x: Name.) Esto es común para [amplía su información animaciones](https://msdn.microsoft.com/library/windows/apps/mt187354). Para obtener más información, consulta la sección de [Referencias a ResourceDictionary y a los recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).

