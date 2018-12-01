---
Description: Learn to write code for a custom Panel class, implementing ArrangeOverride and MeasureOverride methods, and using the Children property.
MS-HAID: dev\_ctrl\_layout\_txt.boxpanel\_example\_custom\_panel
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: BoxPanel, un ejemplo de panel personalizado (aplicaciones de Windows)
ms.assetid: 981999DB-81B1-4B9C-A786-3025B62B74D6
label: BoxPanel, an example custom panel
template: detail.hbs
op-migration-status: ready
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 42b62e46c8adea771a1b7719d24e99f77f765039
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8332056"
---
# <a name="boxpanel-an-example-custom-panel"></a>BoxPanel, un ejemplo de panel personalizado

 

Aprende a escribir código para una clase [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) personalizada, con la implementación de métodos [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) y [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730), y el uso de la propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514). 

> **API importantes**: [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711), [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) 

En el código de ejemplo se muestra la implementación de un panel personalizado, pero no nos detenemos demasiado en explicar los conceptos de diseño que influyen en la forma en que un panel se puede personalizar para distintos escenarios de diseño. Si quieres más información sobre estos conceptos de diseño y su aplicación a escenarios concretos, consulta [Introducción a los paneles personalizados de XAML](custom-panels-overview.md).

Un *panel* es un objeto que ofrece un comportamiento de diseño para los elementos secundarios que contiene. Este comportamiento tiene lugar cuando se ejecuta el sistema de diseño XAML y se representa la interfaz de usuario de tu aplicación. Se pueden definir paneles personalizados para el diseño XAML derivando una clase personalizada de la clase [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). De igual modo, el comportamiento del panel se proporciona reemplazando los métodos [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) y [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) y suministrando una lógica que mida y organice los elementos secundarios. Este ejemplo deriva de **Panel**. Cuando se parte de **Panel**, los métodos **ArrangeOverride** y **MeasureOverride** carecen de un comportamiento de inicio. Tu código es el que proporciona la puerta a través de la cual los elementos secundarios se dan a conocer al sistema de diseño XAML y se representan en la interfaz de usuario. Por eso, es sumamente importante que el código tenga en cuenta todos los elementos secundarios y siga los patrones que el sistema de diseño espera.

## <a name="your-layout-scenario"></a>Escenario de diseño

Cuando defines un panel personalizado, estás definiendo un escenario de diseño.

Un escenario de diseño se expresa del modo siguiente:

-   Con el comportamiento del panel cuando tiene elementos secundarios.
-   Cuando el panel tiene limitaciones en su espacio propio.
-   La determinación por parte de la lógica del panel de todas las medidas, posiciones de colocación y tamaños que finalmente se convertirán en un diseño de interfaz de usuario representada de elementos secundarios.

Teniendo esto en mente, el `BoxPanel` que mostramos aquí es para un escenario en particular. Como queremos dar prioridad al código en este ejemplo, todavía no vamos a explicar el escenario en detalle, sino que nos centraremos en los pasos necesarios y en los patrones de codificación. Si primero quieres más información sobre el escenario, ve directamente a ["El escenario para `BoxPanel`"](#the-scenario-for-boxpanel) y, luego, regresa al código.

## <a name="start-by-deriving-from-panel"></a>Derivar de **Panel** para empezar

Empezaremos derivando una clase personalizada [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). Probablemente, la forma más sencilla de hacerlo es definir un archivo de código independiente para esta clase. Para hacerlo, usamos las opciones del menú contextual **Agregar** | **Nuevo elemento** | **Clase** de un proyecto en el **Explorador de soluciones** de Microsoft Visual Studio. Asigna el nombre `BoxPanel` a la clase (y al archivo).

El archivo de plantilla de una clase no comienza con una gran cantidad de instrucciones **using**, ya que no está destinado específicamente a aplicaciones para la Plataforma universal de Windows (UWP). Por lo tanto, agrega primero las instrucciones **using**. El archivo de plantilla también empieza con algunas instrucciones **using** que probablemente no necesitas y que se pueden eliminar. A continuación te sugerimos una lista de instrucciones **using** que pueden resolver tipos que necesitarás en un código de panel personalizado típico:

```CSharp
using System;
using System.Collections.Generic; // if you need to cast IEnumerable for iteration, or define your own collection properties
using Windows.Foundation; // Point, Size, and Rect
using Windows.UI.Xaml; // DependencyObject, UIElement, and FrameworkElement
using Windows.UI.Xaml.Controls; // Panel
using Windows.UI.Xaml.Media; // if you need Brushes or other utilities
```

Ahora que ya puedes resolver [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), conviértela en la clase base de `BoxPanel` Además, haz que `BoxPanel` sea público:

```CSharp
public class BoxPanel : Panel
{
}
```

En el nivel de clase, define algunos valores **int** y **double** que vayan a compartir algunas de las funciones de tu lógica, pero que no sea preciso exponer como API pública. En el ejemplo, son estos: `maxrc`, `rowcount`, `colcount`, `cellwidth`, `cellheight`, `maxcellheight`, `aspectratio`.

Después de hacerlo, el archivo de código completo tiene el siguiente aspecto (quitando los comentarios sobre **using**, ahora que ya sabemos por qué estaban ahí):

```CSharp
using System;
using System.Collections.Generic;
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

public class BoxPanel : Panel 
{
    int maxrc, rowcount, colcount;
    double cellwidth, cellheight, maxcellheight, aspectratio;
}
```

A partir de aquí, te enseñaremos una definición de miembro cada vez, ya sea una invalidación de método o algo auxiliar, como una propiedad de dependencia. Estos elementos se pueden agregar al esqueleto anterior en cualquier orden, y no volveremos a mostrar las instrucciones **using** o la definición del ámbito de la clase en los fragmentos de código hasta que mostremos el código final.

## **<a name="measureoverride"></a>MeasureOverride**


```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize;
    // Determine the square that can contain this number of items.
    maxrc = (int)Math.Ceiling(Math.Sqrt(Children.Count));
    // Get an aspect ratio from availableSize, decides whether to trim row or column.
    aspectratio = availableSize.Width / availableSize.Height;

    // Now trim this square down to a rect, many times an entire row or column can be omitted.
    if (aspectratio > 1)
    {
        rowcount = maxrc;
        colcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
    } 
    else 
    {
        rowcount = (maxrc > 2 && Children.Count < maxrc * (maxrc - 1)) ? maxrc - 1 : maxrc;
        colcount = maxrc;
    }

    // Now that we have a column count, divide available horizontal, that's our cell width.
    cellwidth = (int)Math.Floor(availableSize.Width / colcount);
    // Next get a cell height, same logic of dividing available vertical by rowcount.
    cellheight = Double.IsInfinity(availableSize.Height) ? Double.PositiveInfinity : availableSize.Height / rowcount;
           
    foreach (UIElement child in Children)
    {
        child.Measure(new Size(cellwidth, cellheight));
        maxcellheight = (child.DesiredSize.Height > maxcellheight) ? child.DesiredSize.Height : maxcellheight;
    }
    return LimitUnboundedSize(availableSize);
}
```

El patrón necesario de una implementación de [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) es el bucle que pasa por cada elemento de [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514). Llama siempre al método [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) en cada uno de estos elementos. **Measure** tiene un parámetro de tipo [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995). Lo que se estás pasando aquí es el tamaño que el panel va a tener disponible para el elemento secundario en cuestión. Por lo tanto, antes de poder efectuar el bucle y empezar a llamar a **Measure**, necesitaremos saber la cantidad de espacio que cada celda puede dedicar. En el propio método **MeasureOverride** tenemos el valor *availableSize*. Se trata del tamaño que el elemento principal del panel usó cuando llamó a **Measure**, que era causante de que este **MeasureOverride** se llamara en primer lugar. Así, una lógica típica consistiría en concebir un esquema en el que cada elemento secundario divida el espacio de todo el *availableSize* del panel. Luego, cada división de tamaño se pasaría a **Measure** en cada elemento secundario.

La forma en la que `BoxPanel` divide el tamaño es bastante sencilla: divide su espacio en una serie de cuadros que se controla en gran medida mediante el número de elementos. El tamaño de los cuadros se establece a partir del recuento de filas y columnas y del tamaño disponible. Hay veces en las que una fila o una columna de un cuadrado no es necesaria y se desecha, de modo que el panel pasa a ser más un rectángulo que un cuadrado en cuanto a su relación fila:columna. Para más información sobre cómo se ha llegado hasta esta lógica, ve a ["El escenario para BoxPanel"](#the-scenario-for-boxpanel).

¿Qué es lo que hace el paso de medición? Establece el valor de la propiedad de solo lectura [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) en cada elemento en el que se haya llamado a [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952). Tener un valor de **DesiredSize** posiblemente sea importante al llegar al paso de organización, ya que **DesiredSize** indica cuál puede o debe ser el tamaño al organizar y en la representación final. Incluso si no usas **DesiredSize** en tu lógica, el sistema seguirá necesitándolo.

Este panel se puede usar cuando el componente de altura de *availableSize* no esté enlazado. Si esto es así, el panel no tiene una altura conocida que dividir. En este caso, la lógica del paso de medición informa a cada elemento secundario de que todavía carece de una altura enlazada, y lo hace pasando un elemento [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) a la llamada de [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) de los elementos secundarios en los que [**Size.Height**](https://msdn.microsoft.com/library/windows/apps/hh763910) es infinito. Esto puede hacerse. Cuando se llama a **Measure**, la lógica consiste en que [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) se establece en el mínimo de lo siguiente: lo que se pasó a **Measure**, o bien el tamaño natural de dicho elemento de factores como [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) y [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) expresamente definidos.

> [!NOTE]
> La lógica interna de [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) presenta el mismo comportamiento: **StackPanel** pasa un valor de dimensión infinito a [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) en los elementos secundarios, lo que indica que no hay ninguna limitación en ellos en cuanto a dimensión de orientación. Normalmente, **StackPanel** establece su tamaño dinámicamente para dar cabida a todos los elementos secundarios de una pila que crece en esa dimensión.

Sin embargo, el panel en sí no puede devolver un objeto [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) con un valor infinito de [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730); esto generaría una excepción durante el diseño. Por lo tanto, parte de la lógica irá dirigida a averiguar la altura máxima que cada elemento secundario necesita para, luego, usar esa altura como altura de celda en caso de que esta no se haya obtenido ya de las propias limitaciones de tamaño del panel. Aquí te mostramos la función auxiliar `LimitUnboundedSize` a la que se hizo referencia en el código anterior, que toma la altura de celda máxima y la usa para dar al panel una altura finita que devolver, al tiempo que garantiza que `cellheight` sea un número finito antes de que se inicie el paso de organización:

```CSharp
// This method is called only if one of the availableSize dimensions of measure is infinite.
// That can happen to height if the panel is close to the root of main app window.
// In this case, base the height of a cell on the max height from desired size
// and base the height of the panel on that number times the #rows.

Size LimitUnboundedSize(Size input)
{
    if (Double.IsInfinity(input.Height))
    {
        input.Height = maxcellheight * colcount;
        cellheight = maxcellheight;
    }
    return input;
}
```

## **<a name="arrangeoverride"></a>ArrangeOverride**

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
     int count = 1
     double x, y;
     foreach (UIElement child in Children)
     {
          x = (count - 1) % colcount * cellwidth;
          y = ((int)(count - 1) / colcount) * cellheight;
          Point anchorPoint = new Point(x, y);
          child.Arrange(new Rect(anchorPoint, child.DesiredSize));
          count++;
     }
     return finalSize;
}
```

El patrón necesario de una implementación de [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) es el bucle a través de cada elemento en [**Panel.Children**](https://msdn.microsoft.com/library/windows/apps/br227514). Llama siempre al método [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) en cada uno de estos elementos.

Fíjate en que no hay tantos cálculos como en [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730). Es lo normal. El tamaño de los elementos secundarios ya se conoce por la propia lógica de **MeasureOverride** del panel, o bien por el valor de [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) de cada elemento secundario que se estableció durante el paso de medición. No obstante, todavía nos queda decidir la ubicación en la que cada elemento secundario va a aparecer en el panel. En un panel normal, los elementos secundarios deben representarse en una posición diferente. Un panel que crea elementos superpuestos no es lo más conveniente en un escenario normal (si bien no es descartable que puedan crearse paneles que se solapen a propósito, si es realmente lo que se pretende en el escenario).

Este panel se organiza siguiendo el concepto de filas y columnas. El número de filas y columnas ya está calculado (era necesario para la medición), así que ahora la forma de las filas y columnas, además de los tamaños conocidos de cada celda, contribuyen a la lógica para definir una posición de representación (`anchorPoint`) para cada elemento del panel. Ese [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), junto con el valor de [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) que ya conocemos de la medición, se usan como los dos componentes que construyen un [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994). **Rect** es el tipo de entrada de [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914).

A veces, los paneles necesitan recortar su contenido. De hacerlo, el tamaño recortado es aquel que está presente en [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921), dado que la lógica de [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) lo establece como el mínimo de lo que se ha pasado a **Measure**, o cualquier otro factor de tamaño natural. Lo normal, pues, es que no sea necesario comprobar expresamente los recortes de tamaño durante el método [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914); el recorte sencillamente se producirá en función del pase de **DesiredSize** a cada llamada de **Arrange**.

No siempre hay que hacer un recuento mientras se avanza por el bucle, si toda la información necesaria para definir la posición de representación se conoce por otros métodos. Por ejemplo, en la lógica de diseño de [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), la posición de la colección [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) es irrelevante. Toda la información necesaria para ubicar cada elemento en un **Canvas** se conoce con la lectura de los valores [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) y [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) de elementos secundarios como parte de la lógica de organización. Resulta que la lógica `BoxPanel` necesita un recuento que comparar con *colcount*, de modo que se sepa cuándo empezar una nueva fila y desplazar el valor de *y*.

Es normal que el objeto *finalSize* de entrada y el objeto [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) que se devuelven de una implementación de [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) sean iguales. Para obtener más información sobre los motivos de esto, consulta la sección "**ArrangeOverride**" en [Introducción a los paneles personalizados de XAML](custom-panels-overview.md).

## <a name="a-refinement-controlling-the-row-vs-column-count"></a>Un ajuste: controlar el recuento de filas y columnas

Este panel se podría compilar y utilizar tal cual está ahora. Sin embargo, le vamos a agregar un ajuste más. En el código que acabas de ver, la lógica coloca la fila o columna extra en el lado más largo dentro de la relación de aspecto. Pero, si quieres que haya un mayor control de las formas de las celdas, probablemente lo más conveniente sea decantarse por un conjunto de celdas de 4x3 en lugar de 3x4, incluso cuando la relación de aspecto del panel sea “vertical”. Por lo tanto, agregaremos una propiedad de dependencia opcional que el usuario del panel puede definir para controlar este comportamiento. A continuación te mostramos la definición de la propiedad de dependencia, que es muy básica:

```CSharp
public static readonly DependencyProperty UseOppositeRCRatioProperty =
   DependencyProperty.Register("UseOppositeRCRatio", typeof(bool), typeof(BoxPanel), null);

public bool UseSquareCells
{
    get { return (bool)GetValue(UseOppositeRCRatioProperty); }
    set { SetValue(UseOppositeRCRatioProperty, value); }
}
```

Y así es como el uso de `UseOppositeRCRatio` repercute en la lógica de medición. Realmente, lo único que hace es cambiar el modo en que `rowcount` y `colcount` se derivan de `maxrc` y la verdadera relación de aspecto y, a causa de esto, se producen las correspondientes diferencias de tamaño en cada celda. Cuando `UseOppositeRCRatio` es **true**, invierte el valor de la verdadera relación de aspecto antes de usarla para los recuentos de filas y columnas.

```CSharp
if (UseOppositeRCRatio) { aspectratio = 1 / aspectratio;}
```

## <a name="the-scenario-for-boxpanel"></a>El escenario para BoxPanel

El escenario particular para `BoxPanel` es un panel en el que uno de los principales factores determinantes de cómo se divide el espacio consiste en conocer el número de elementos secundarios y dividir el espacio disponible existente del panel. La forma de los paneles es rectangular por naturaleza. Muchos paneles funcionan dividiendo ese espacio rectangular en más rectángulos, que es lo que [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) hace para sus celdas. En el caso de **Grid**, el tamaño de las celdas se establece por medio de los valores de [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) y [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606), mientras que los elementos declaran la celda exacta en la que se van a situar mediante las propiedades adjuntas [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) y [**Grid.Column**](https://msdn.microsoft.com/library/windows/apps/hh759774). Para lograr un buen diseño a partir de un elemento **Grid**, normalmente es necesario conocer el número de elementos secundarios de antemano, de modo que haya suficientes celdas y cada elemento secundario defina sus propiedades adjuntas para caber en su propia celda.

Pero, ¿y si el número de elementos secundarios es dinámico? Es totalmente factible: el código de tu aplicación puede agregar elementos a colecciones, como respuesta a cualquier condición de tiempo de ejecución dinámica que consideres que sea lo suficientemente importante como para actualizar la interfaz de usuario. Si usas enlaces de datos a objetos de negocios/colecciones de respaldo, la obtención de esas actualizaciones y la actualización de la interfaz de usuario se controla automáticamente, de modo que esta es a menudo la técnica preferida (consulta el tema de [Introducción al enlace de datos](https://msdn.microsoft.com/library/windows/apps/mt210946)).

Sin embargo, no todos los escenarios de aplicaciones se prestan al enlace de datos. A veces, es necesario crear elementos de interfaz de usuario en el tiempo de ejecución y hacerlos visibles. `BoxPanel`  corresponde a este escenario. Un número variable de elementos secundarios no es un problema para `BoxPanel`, dado que usa el recuento de elementos secundarios en sus cálculos y ajusta los elementos secundarios tanto nuevos como existentes en un nuevo diseño para que todos tengan cabida.

Un escenario avanzado para extender más aún `BoxPanel` (no se muestra aquí) sería incluir los elementos secundarios dinámicos y usar un [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) de un elemento secundario como factor principal para asignar el tamaño de las celdas individuales. Este escenario podría usar tamaños de fila o columna variables o formas que no sean de cuadrícula para que el espacio "desperdiciado" sea menor. Esto requiere una estrategia que permita contener en un solo rectángulo varios rectángulos de diversos tamaños y relaciones de aspecto para lograr el tamaño mínimo y un resultado estético. `BoxPanel`  no hace eso, sino que usa una técnica más sencilla para dividir el espacio. `BoxPanel`La técnica consiste en averiguar el mínimo cuadrado que sea mayor que el recuento de elementos secundarios. Así, por ejemplo, nueve elementos encajarían en un cuadrado de 3x3, 10 elementos necesitan un cuadrado de 4x4. No obstante, con frecuencia se pueden ajustar elementos y, al mismo tiempo, quitar una fila o una columna del cuadrado inicial para ahorrar espacio. En el ejemplo del recuento=10, esto encaja en un rectángulo de 4x3 o de 3x4.

Te estarás preguntando por qué el panel no elige 5x2 para diez elementos, ya que así el número de elementos encajaría a la perfección. Pero, en la práctica, los paneles tienen forma de rectángulos que rara vez presentan una relación de aspecto con una orientación muy marcada. La técnica de los mínimos cuadrados es una forma de influir en la lógica de tamaño para que funcione correctamente con las formas de diseño típicas y no fomentar los cambios de tamaño cuando las formas de celda presentan relaciones de aspecto extrañas.

## <a name="related-topics"></a>Artículos relacionados

**Referencia**

* [**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
* [**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
* [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)

**Conceptos**

* [Alineación, margen y espaciado](alignment-margin-padding.md)
