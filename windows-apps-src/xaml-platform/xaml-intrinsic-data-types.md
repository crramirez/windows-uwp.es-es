---
author: jwmsft
description: "Describe la compatibilidad en el nivel de lenguaje de XAML para Windows Runtime para determinados tipos de datos de Common Language Runtime (CLR) y otros lenguajes de programación como C++."
title: "Tipos de datos intrínsecos de XAML"
ms.assetid: D50E6127-395D-4E27-BAA2-2FE627F4B711
ms.sourcegitcommit: 60e328ca8652baeb226e78f5a9d99fbf8c4f5208
ms.openlocfilehash: 479b900ca14497712f25a7825fde6775a3c1ab60

---

# Tipos de datos intrínsecos de XAML

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

El lenguaje XAML para Windows Runtime proporciona compatibilidad en el nivel de lenguaje para varios tipos de datos que son tipos primitivos de uso frecuente en Common Language Runtime (CLR) y otros lenguajes de programación como C++.

El lugar más común donde verás los usos del tipo de datos intrínseco XAML es cuando los recursos se definen en un diccionario de recursos XAML. Aquí es posible que definas constantes, como por ejemplo, números que usarás para varios valores. O es posible que uses una animación con guión gráfico que use una cadena o un valor booleano para la animación. Así pues, necesitarás un elemento de objeto XAML que represente la cadena o un valor booleano para rellenar el fotograma clave de la definición de [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320). Las plantillas XAML predeterminadas de Windows Runtime usan estas dos técnicas.

El lenguaje XAML para Windows Runtime proporciona compatibilidad en el nivel de lenguaje para estos tipos.

| Primitivo de XAML | Descripción |
|-------|-------------|
| **x:Boolean**  | Para la compatibilidad con CLR, se corresponde con [**Boolean**](https://msdn.microsoft.com/library/windows/apps/xaml/system.boolean.aspx). XAML analiza los valores de **x:Boolean** sin distinguir entre mayúsculas y minúsculas. Ten en cuenta que "x:Bool" no es una alternativa aceptada. |
| **x:String**   | Para la compatibilidad con CLR, se corresponde con [**String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx). La codificación predeterminada de la cadena es la codificación XML adyacente. |
| **x:Double**   | Para la compatibilidad con CLR, se corresponde con [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx). Además de los valores numéricos, la sintaxis del texto para **x:Double** permite el token "NaN", que es la forma en la que el valor "Auto" del comportamiento de diseño se puede almacenar como el valor de un recurso. En los token se distingue entre mayúsculas y minúsculas. Es posible usar notación científica como, por ejemplo, "1+E06" para `1,000,000`. |
| **x:Int32**    | Para la compatibilidad con CLR, se corresponde con [**Int32**](https://msdn.microsoft.com/library/windows/apps/xaml/system.int32.aspx). **x:Int32** se trata como si tuviera signo, y es posible incluir el símbolo menos ("-") para un entero negativo. En XAML, la ausencia de un signo en la sintaxis de texto implica un valor con signo positivo. |

Estos tipos primitivos de lenguaje XAML generalmente son los únicos casos en los que definirás un elemento de objeto que usa el prefijo **x:** en el código XAML. Todas las otras características del lenguaje XAML se suelen usar en forma de atributo, o como extensión de marcado.

**Nota** Por convención, los tipos primitivos del lenguaje XAML y todos los demás elementos del lenguaje XAML se muestran con el prefijo "x:". Así es como suelen usarse los elementos del lenguaje XAML en el marcado en el mundo real. Esta convención se sigue en la documentación de XAML y también en la especificación XAML.

## Otros tipos primitivos de XAML

La especificación XAML 2009 indica otros tipos primitivos en el nivel de lenguaje XAML, como **x:Uri** y **x:Single**. A menos que se indique en la tabla de este tema, los otros tipos primitivos de lenguaje XAML, tal y como los definen otros vocabularios XAML o la especificación XAML 2009, no se admiten actualmente en el XAML para Windows Runtime.

**Nota** Las fechas y horas (propiedades que usan [**DateTime**](https://msdn.microsoft.com/library/windows/apps/br206576) o [**DateTimeOffset**](https://msdn.microsoft.com/library/windows/apps/xaml/system.datetimeoffset.aspx), [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/br225996) o [**System.TimeSpan**](https://msdn.microsoft.com/library/windows/apps/xaml/system.timespan.aspx)) no se pueden establecer con un primitivo de XAML. Por lo general, estas propiedades son imposibles de establecer en XAML porque en el analizador de XAML de Windows Runtime no hay ningún comportamiento predeterminado de conversión desde cadenas para fechas y horas. Para los valores de inicialización de cualquier propiedad de fecha y hora, tendrás que usar código subyacente que se ejecute al cargarse una página o un elemento.

## Temas relacionados

* [Introducción a XAML](xaml-overview.md)
* [Guía de sintaxis XAML](xaml-syntax-guide.md)
* [Animaciones con guion gráfico](https://msdn.microsoft.com/library/windows/apps/mt187354)
 




<!--HONumber=Jun16_HO4-->


