---
description: Describe la compatibilidad en el nivel de lenguaje de XAML para Windows Runtime para determinados tipos de datos de Common Language Runtime (CLR) y otros lenguajes de programación como C++.
title: Tipos de datos intrínsecos de XAML
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 19f297e731225d28f92f63ad9359bba70f0f0b32
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340449"
---
# <a name="xaml-intrinsic-data-types"></a>Tipos de datos intrínsecos de XAML


El lenguaje XAML para Windows Runtime proporciona compatibilidad en el nivel de lenguaje para varios tipos de datos que son tipos primitivos de uso frecuente en Common Language Runtime (CLR) y otros lenguajes de programación como C++.

El lugar más común donde verás los usos del tipo de datos intrínseco XAML es cuando los recursos se definen en un diccionario de recursos XAML. Aquí es posible que definas constantes, como por ejemplo, números que usarás para varios valores. O es posible que uses una animación con guión gráfico que use una cadena o un valor booleano para la animación. Así pues, necesitarás un elemento de objeto XAML que represente la cadena o un valor booleano para rellenar el fotograma clave de la definición de [**ObjectAnimationUsingKeyFrames**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames). Las plantillas XAML predeterminadas de Windows Runtime usan estas dos técnicas.

El lenguaje XAML para Windows Runtime proporciona compatibilidad en el nivel de lenguaje para estos tipos.

| Primitivo de XAML | Descripción |
|-------|-------------|
| **x:Boolean**  | Para la compatibilidad con CLR, se corresponde con [**Boolean**](https://docs.microsoft.com/dotnet/api/system.boolean). XAML analiza los valores de **x:Boolean** sin distinguir entre mayúsculas y minúsculas. Ten en cuenta que "x:Bool" no es una alternativa aceptada. |
| **x:String**   | Para la compatibilidad con CLR, se corresponde con [**String**](https://docs.microsoft.com/dotnet/api/system.string). La codificación predeterminada de la cadena es la codificación XML adyacente. |
| **x:Double**   | Para la compatibilidad con CLR, se corresponde con [**Double**](https://docs.microsoft.com/dotnet/api/system.double). Además de los valores numéricos, la sintaxis del texto para **x:Double** permite el token "NaN", que es la forma en la que el valor "Auto" del comportamiento de diseño se puede almacenar como el valor de un recurso. En los token se distingue entre mayúsculas y minúsculas. Es posible usar notación científica como, por ejemplo, "1+E06" para `1,000,000`. |
| **x:Int32**    | Para la compatibilidad con CLR, se corresponde con [**Int32**](https://docs.microsoft.com/dotnet/api/system.int32). **x:Int32** se trata como si tuviera signo y es posible incluir el símbolo menos ("-") en un entero negativo. En XAML, la ausencia de un signo en la sintaxis de texto implica un valor con signo positivo. |

Estos tipos primitivos de lenguaje XAML generalmente son los únicos casos en los que definirás un elemento de objeto que usa el prefijo **x:** en el código XAML. Todas las otras características del lenguaje XAML se suelen usar en forma de atributo, o como extensión de marcado.

**Tenga en cuenta**@no__t Convención 1By, los primitivos del lenguaje para XAML y todos los demás elementos del lenguaje XAML se muestran con el prefijo "x:". Así es como suelen usarse los elementos del lenguaje XAML en el marcado en el mundo real. Esta convención se sigue en la documentación de XAML y también en la especificación XAML.

## <a name="other-xaml-primitives"></a>Otros tipos primitivos de XAML

La especificación XAML 2009 indica otros tipos primitivos en el nivel de lenguaje XAML, como **x:Uri** y **x:Single**. A menos que se indique en la tabla de este tema, los otros tipos primitivos de lenguaje XAML, tal y como los definen otros vocabularios XAML o la especificación XAML 2009, no se admiten actualmente en el XAML para Windows Runtime.

**Tenga en cuenta**  Dates y Times (las propiedades que usan [**DateTime**](https://docs.microsoft.com/uwp/api/Windows.Foundation.DateTime) o [**DateTimeOffset**](https://docs.microsoft.com/dotnet/api/system.datetimeoffset), [**TimeSpan**](https://docs.microsoft.com/uwp/api/Windows.Foundation.TimeSpan) o [**System. TimeSpan**](https://docs.microsoft.com/dotnet/api/system.timespan)) no se pueden establecer con un primitivo de XAML. Por lo general, estas propiedades son imposibles de establecer en XAML porque en el analizador de XAML de Windows Runtime no hay ningún comportamiento predeterminado de conversión desde cadenas para fechas y horas. Para los valores de inicialización de cualquier propiedad de fecha y hora, tendrás que usar código subyacente que se ejecute al cargarse una página o un elemento.

## <a name="related-topics"></a>Temas relacionados

* [Introducción a XAML](xaml-overview.md)
* [Guía de sintaxis de XAML](xaml-syntax-guide.md)
* [Animaciones con guion gráfico](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
 

