---
Description: You can define custom panels for XAML layout by deriving a custom class from the Panel class.
MS-HAID: dev\_ctrl\_layout\_txt.xaml\_custom\_panels\_overview
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Introducción a los paneles personalizados de XAML
ms.assetid: 0CD395CD-E2AB-429D-BB49-56A71C5CC35D
label: XAML custom panels overview (Windows apps)
template: detail.hbs
op-migration-status: ready
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9999ebb121916a7804546784ea98ac4e0f4222e5
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8854846"
---
# <a name="xaml-custom-panels-overview"></a>Introducción a los paneles personalizados de XAML

 

Un *panel* es un objeto que ofrece un comportamiento de diseño para los elementos secundarios que contiene, cuando el diseño del sistema de lenguaje XAML se ejecuta y se representa la interfaz de usuario de tu aplicación. 


> **API importantes**: [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711), [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)

Se pueden definir paneles personalizados para el diseño XAML derivando una clase personalizada de la clase [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). De igual modo, el comportamiento del panel se proporciona reemplazando los métodos [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) y [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) y suministrando una lógica que mida y organice los elementos secundarios.

## <a name="the-panel-base-class"></a>La clase base **Panel**


Para definir una clase de panel personalizada, puedes derivar directamente de la clase [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) o derivar de una de las clases de panel práctico que no están selladas, como [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) o [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635). Es más fácil derivar de **Panel**, porque puede resultar difícil evitar la lógica de diseño existente de un panel que ya tiene un comportamiento de diseño. Además, un panel con comportamiento podría tener ya propiedades que no son relevantes para las características de diseño de tu panel.

Tu panel personalizado hereda estas API de [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511):

-   La propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514).
-   Las propiedades [**Background**](https://msdn.microsoft.com/library/windows/apps/br227512), [**ChildrenTransitions**](https://msdn.microsoft.com/library/windows/apps/br227515) e [**IsItemsHost**](https://msdn.microsoft.com/library/windows/apps/br227517) y los identificadores de las propiedades de dependencia. Ninguna de estas propiedades son virtuales, por lo que normalmente no se invalidan ni se reemplazan. Normalmente no necesitas estas propiedades para escenarios de paneles personalizados, ni siquiera para leer valores.
-   Los métodos de invalidación de diseño [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) y [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711). Originalmente fueron definidos por [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706). La clase [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) base no los invalida, pero los paneles prácticos como [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) tienen implementaciones de invalidación que se implementan como código nativo y son ejecutadas por el sistema. La mayor parte del trabajo necesario para definir un panel personalizado se dedica a proporcionar implementaciones nuevas (o adicionales) de **ArrangeOverride** y **MeasureOverride**.
-   Todas las demás API de [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706), [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) y [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), como [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height), [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) etc. Algunas veces se hace referencia a los valores de estas propiedades en las invalidaciones de diseño, pero no son virtuales por lo que normalmente no se invalidan ni se reemplazan.

Lo que nos ocupa aquí es describir los conceptos relativos a XAML para que puedas tener en cuenta todas las posibilidades sobre cómo se puede y debe comportar un panel personalizado en el diseño. Si prefieres ir directamente a ver un ejemplo de implementación de panel personalizado, consulta el tema [BoxPanel, un ejemplo de panel personalizado](boxpanel-example-custom-panel.md).

## <a name="the-children-property"></a>La propiedad **Children**


La propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) es relevante para un panel personalizado porque todas las clases derivadas de [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) usan la propiedad **Children** como lugar donde almacenar los elementos secundarios que contienen en una colección. **Children** está designada como propiedad de contenido XAML de la clase **Panel**, y todas las clases derivadas de **Panel** pueden heredar el comportamiento de la propiedad de contenido XAML. Si una propiedad se designa como propiedad de contenido XAML, significa que el marcado XAML puede omitir un elemento de propiedad cuando se especifica esa propiedad en el marcado y que los valores se establecen como elementos secundario de marcado inmediatos (el "contenido"). Por ejemplo, si derivas una clase llamada **CustomPanel** de **Panel** que no define ningún comportamiento nuevo, puedes seguir usando este marcado:

```XAML
<local:CustomPanel>
  <Button Name="button1"/>
  <Button Name="button2"/>
</local:CustomPanel>
```

Cuando un analizador XAML lee este marcado, se sabe que [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) es la propiedad de contenido XAML de todos los tipos derivados de [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511), por lo que el analizador agregará los dos elementos [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265) al valor [**UIElementCollection**](https://msdn.microsoft.com/library/windows/apps/br227633) de la propiedad **Children**. La propiedad de contenido XAML facilita una relación optimizada entre elementos primarios y secundarios en el marcado XAML de una definición de interfaz de usuario. Para obtener más información sobre las propiedades de contenido XAML y sobre cómo se rellenan las propiedades de colección cuando se analiza el código XAML, consulta la [Guía de sintaxis XAML](https://msdn.microsoft.com/library/windows/apps/mt185596).

El tipo de colección que mantiene el valor de la propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) es la clase [**UIElementCollection**](https://msdn.microsoft.com/library/windows/apps/br227633). **UIElementCollection** es una colección fuertemente tipada que usa [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) como tipo de elemento obligatorio. **UIElement** es un tipo base del que heredan cientos de tipos de elementos prácticos de interfaz de usuario, por lo que aquí la aplicación del tipo es deliberadamente menos estricta. Sí obliga a que no puedas tener un objeto [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) como elemento secundario directo de un objeto [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511) y eso suele significar que solo se encontrarán como elementos secundarios en un **Panel** los elementos que serán visibles en la interfaz de usuario y que participen en el diseño.

Normalmente, un panel personalizado acepta cualquier elemento secundario [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) mediante una definición XAML, simplemente usando las características de la propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) tal cual. En un escenario avanzado, podrías admitir una comprobación adicional del tipo de los elementos secundarios cuando iteras por la colección en las invalidaciones de diseño.

Además de recorrer la colección [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) en las invalidaciones, la lógica del panel también podría verse influida por `Children.Count`. Podrías tener una lógica que asigne espacio basándose, al menos parcialmente, en el número de elementos en lugar de en los tamaños deseados y en otras características de los elementos individuales.

## <a name="overriding-the-layout-methods"></a>Métodos de invalidación de diseño


En el modelo básico para los métodos de invalidación de diseño ([**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) y [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)) se debe procesar una iteración en todos los elementos secundarios y llamar al método de diseño específico de cada elemento secundario. El primer ciclo de diseño comienza cuando el sistema de diseño XAML establece el aspecto visual de la ventana raíz. Debido a que cada elemento primario invoca el diseño en su elemento secundario, esto propaga una llamada a los métodos de diseño para todos los posibles elementos de interfaz de usuario que se supone que forman parte de un diseño. En el diseño XAML, hay dos fases: medir y después, organizar.

No se obtiene ningún comportamiento integrado de los métodos de diseño para [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) y [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) de la clase base [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). Los elementos que hay en [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) no se representarán automáticamente como parte del árbol visual XAML. Depende de ti hacer que los elementos sean conocidos para el proceso de diseño; para ello, invoca métodos de diseño para cada uno de los elementos que encuentras en **Children** mediante un pase de diseño en tus implementaciones de **MeasureOverride** y **ArrangeOverride**.

No hay motivos para llamar a las implementaciones base en las invalidaciones de diseño de Windows a menos que tengas tu propia herencia. Los métodos nativos del comportamiento de diseño (si existe) se ejecutan de todos modos, y el comportamiento nativo se producirá aunque no se llame a la implementación base desde las invalidaciones.

Durante el pase de medición, la lógica de diseño consulta el tamaño deseado de cada elemento secundario llamando al método [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) para ese elemento secundario. Al llamar al método **Measure**, se establece el valor de la propiedad [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921). El valor devuelto de [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) es el tamaño deseado para el propio panel.

Durante el pase de organización, se determinan las posiciones y los tamaños de los elementos secundarios en el espacio x-y, y se prepara la composición del diseño para su representación. El código debe llamar a [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) para cada elemento secundario de [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) de modo que el sistema de diseño detecte que el elemento pertenece al diseño. La llamada **Arrange** es previa a la composición y representación; informa al sistema de diseño dónde va el elemento cuando la composición se envía para su representación.

Muchas propiedades y valores influyen en el modo de funcionamiento de la lógica en tiempo de ejecución. Una manera de considerar el proceso de diseño es que los elementos sin secundarios (normalmente los elementos más profundamente anidados en la interfaz de usuario) son los que primero pueden finalizar las mediciones. No tienen ninguna dependencia de elementos secundarios que influya en su tamaño deseado. Podrían tener sus propios tamaños deseados y estos son sugerencias de tamaño hasta que el diseño se produce en realidad. Después, el pase de medición continúa recorriendo hacia arriba el árbol visual hasta que el elemento raíz tenga sus mediciones y se puedan finalizar todas las mediciones.

El diseño candidato debe caber en la ventana actual de la aplicación o algunas partes de la interfaz de usuario se recortarán. Los paneles suelen ser el lugar donde se determina la lógica del recorte. La lógica del panel puede determinar qué tamaño está disponible desde la implementación de [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730), y puede tener que forzar restricciones de tamaño en los elementos secundarios y dividir el espacio entre los elementos secundarios para que todo quepa lo mejor posible. El resultado ideal del diseño es aquel que usa varias propiedades de todas las partes del diseño pero que al mismo tiempo cabe dentro de la ventana de la aplicación. Eso requiere una buena implementación de la lógica de diseño de los paneles, así como un diseño razonable de la interfaz de usuario en la parte de cualquier código de la aplicación que cree una interfaz de usuario que use ese panel. Ningún diseño de panel se verá bien si el diseño general de la interfaz de usuario incluye más elementos secundarios de los que puedan caber en la aplicación.

Lo que en gran parte hace que el sistema de diseño funcione es que cualquier elemento basado en [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) ya tiene algo de su comportamiento inherente cuando actúa como elemento secundario en un contenedor. Por ejemplo, hay varias API de **FrameworkElement** que informan del comportamiento de diseño o que son necesarias para realizar el trabajo de diseño. Estos son:

-   [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) (en realidad es una propiedad [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911))
-   [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) y [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709)
-   [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) y [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width)
-   [**Margen**](https://msdn.microsoft.com/library/windows/apps/br208724)
-   Evento [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722)
-   [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208720) y [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/br208749)
-   Métodos [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) y [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
-   Métodos [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) y [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952): estos tienen implementaciones nativas definidas en el nivel de [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) que controlan la acción de diseño en el nivel de elemento.

## **<a name="measureoverride"></a>MeasureOverride**


El método [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) tiene un valor devuelto que el sistema de diseño usa como valor inicial de [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) para el propio panel cuando su elemento primario en el diseño llama al método [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) para el panel. Las opciones de la lógica dentro del método son tan importantes como lo que devuelve, y la lógica suele influir en el valor que se devuelve.

Todas las implementaciones [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) deben recorrer en bucle [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) y llamar al método [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) para cada elemento secundario. Al llamar al método **Measure**, se establece el valor de la propiedad [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921). Esto podría informar de cuánto espacio necesita el propio panel, y la manera de dividir ese espacio entre los elementos o de dimensionarlo para un elemento secundario determinado.

Esta es una estructura muy básica de un método [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730):

```CSharp
protected override Size MeasureOverride(Size availableSize)
{
    Size returnSize; //TODO might return availableSize, might do something else
     
    //loop through each Child, call Measure on each
    foreach (UIElement child in Children)
    {
        child.Measure(new Size()); // TODO determine how much space the panel allots for this child, that's what you pass to Measure
        Size childDesiredSize = child.DesiredSize; //TODO determine how the returned Size is influenced by each child's DesiredSize
        //TODO, logic if passed-in Size and net DesiredSize are different, does that matter?
    }
    return returnSize;
}
```

Los elementos suelen tener un tamaño natural en el momento en que están listos para el diseño. Después del pase de medición, [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) podría indicar ese tamaño natural si el valor de *availableSize* que se pasó para [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) era menor. Si el tamaño natural es mayor que el valor *availableSize* que se pasó para **Measure**, **DesiredSize** se reduce a *availableSize*. Así se comporta la implementación interna de **Measure** y tus invalidaciones de diseño deben tener en cuenta ese comportamiento.

Algunos elementos no tienen un tamaño natural porque tienen valores **Auto** para [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) y [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width). Esos elementos usan el valor completo de *availableSize*, porque eso es lo que representa un valor **Auto**: dimensionar un elemento con el máximo tamaño disponible, que el elemento primario inmediato de diseño comunica con una llamada a [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) con *availableSize*. En la práctica, una interfaz de usuario siempre se dimensiona en alguna medida (aunque sea la ventana de nivel superior). Finalmente, el pase de medición resuelve todos los valores **Auto** en las restricciones de los elementos primarios y todos los elementos con un valor **Auto** obtienen mediciones reales (que puedes obtener comprobando [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) y [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707), una vez terminado el diseño).

Está permitido pasar un tamaño a [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) que tenga al menos una dimensión infinita, para indicar que el panel puede intentar dimensionarse a sí mismo para ajustarse a la medida de su contenido. Todos los elementos secundarios que se miden establecen su valor [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) con su tamaño natural. Después, durante el pase de organización, el panel normalmente se organiza usando ese tamaño.

Los elementos de texto como [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) tienen un valor [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) y [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) calculado según su cadena de texto y sus propiedades de texto, aunque no se haya establecido ningún valor [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) o [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width). Tu panel lógico debe respetar estas dimensiones. El recorte de texto es una experiencia de la interfaz de usuario especialmente desaconsejable.

Aunque tu implementación no use las medidas de tamaño deseado, lo mejor es llamar al método [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) en cada elemento secundario, porque hay comportamientos internos y nativos que activa el método **Measure** al que se está llamando. Para que un elemento participe en el diseño, es necesario llamar a **Measure** para cada elemento secundario durante el pase de medición y al método [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) durante el pase de organización. Al llamar a estos métodos se establecen las marcas internas en el objeto y se rellenan los valores (como la propiedad [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921)) que la lógica de diseño del sistema necesita cuando crea el árbol visual y representa la interfaz de usuario.

El valor devuelto [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) se basa en la lógica del panel que interpreta el valor [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) o en otras consideraciones de tamaño para cada uno de los elementos secundarios de [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) cuando se llama a [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) para ellos. Qué hacer con los valores de **DesiredSize** de los elementos secundarios y cómo debe usarlos el valor devuelto de **MeasureOverride** depende de la interpretación de tu propia lógica. Normalmente no incorporas los valores sin modificarlos porque la entrada de **MeasureOverride** suele ser un tamaño disponible fijo que sugiere el elemento primario del panel. Si excedes ese tamaño, el propio panel podría recortarse. Normalmente comparas el tamaño total del elemento secundario con el tamaño disponible del panel y realizas los ajustes necesarios.

### <a name="tips-and-guidance"></a>Sugerencias e instrucciones

-   Lo ideal es que un panel personalizado sea apropiado para ser el primer elemento visual real de una composición de interfaz de usuario, quizás en un nivel inmediatamente inferior a [**Page**](https://msdn.microsoft.com/library/windows/apps/br227503), [**UserControl**](https://msdn.microsoft.com/library/windows/apps/br227647) u otro elemento que sea la raíz de la página XAML. En implementaciones [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730), no devuelvas el valor [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) de entrada de forma rutinaria sin examinar los valores. Si el valor **Size** de retorno contiene un valor **Infinity**, podría lanzar excepciones en la lógica de diseño en tiempo de ejecución. Un valor de **Infinity** podría proceder de la ventana de aplicación principal, que se puede desplazar y, por lo tanto, no tiene un alto máximo. Otro contenido que se puede desplazar podría tener el mismo comportamiento.
-   Otro error común en las implementaciones [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) es devolver un nuevo valor [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) predeterminado (los valores de alto y de ancho son 0). Podrías comenzar con ese valor que quizás sea el valor correcto si tu panel determina que no debe representarse ninguno de los elementos secundarios. No obstante, un valor **Size** predeterminado hace que el host no dimensione el panel correctamente. Dado que no solicita ningún espacio en la interfaz de usuario, no obtiene ningún espacio y no se representa. Por lo demás, todo el código del panel podría funcionar bien pero seguirás sin ver el panel ni el contenido si se compone con un alto cero y un ancho cero.
-   En las invalidaciones, evita la tentación de convertir los elementos secundarios en [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) y usar las propiedades que se calculan como resultado del diseño, especialmente [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) y [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707). En la mayoría de los escenarios habituales, es posible basar la lógica en el valor [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) del elemento secundario y no necesitarás las propiedades relacionadas [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) o [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) de un elemento secundario. En casos especiales en los que conozcas el tipo de elemento y tengas información adicional, por ejemplo, el tamaño natural de un archivo de imagen, puedes usar esa información especializada porque no es un valor que los sistemas de diseño alteren de forma activa. Incluir propiedades calculadas por el diseño como parte de la lógica de diseño aumenta de forma importante el riesgo de definir accidentalmente un bucle de diseño. Estos bucles pueden provocar una condición en la que no se puede crear un diseño y el sistema puede lanzar [**LayoutCycleException**](https://msdn.microsoft.com/library/windows/apps/hh673799) si el bucle es irrecuperable.
-   Los paneles suelen dividir su espacio disponible entre varios elementos secundarios, aunque la forma exacta de dividir el espacio varía. Por ejemplo, [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) implementa una lógica de diseño que usa sus valores [**RowDefinition**](https://msdn.microsoft.com/library/windows/apps/br227606) y [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/br209324) para dividir el espacio en las celdas **Grid** y admite valores tanto de píxel como de variación de tamaño proporcional. En el caso de los valores de píxel, el tamaño disponible para cada elemento secundario ya se conoce, y es lo que se pasa como tamaño de entrada de [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) con estilo de cuadrícula.
-   Los propios paneles pueden insertar espacio reservado para el espaciado entre los elementos. Si lo haces, debes exponer las medidas como una propiedad distinta a [**Margin**](https://msdn.microsoft.com/library/windows/apps/br208724) o cualquier propiedad **Padding**.
-   Los elementos podrían tener valores para sus propiedades [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) y [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) basadas en un pase de diseño anterior. Si los valores cambian, el código de la interfaz de usuario de la aplicación puede colocar controladores para [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) en los elementos si hay una lógica especial para ejecutar, pero la lógica del panel normalmente no necesita comprobar los cambios con control de eventos. El sistema de diseño ya determina cuándo volver a ejecutar el diseño porque una propiedad relacionada con el diseño ha cambiado de valor, y se llama automáticamente a los métodos [**MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730) o [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) de un panel en las circunstancias apropiadas.

## **<a name="arrangeoverride"></a>ArrangeOverride**


El método [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) tiene un valor devuelto [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) que el sistema de diseño usa para representar el panel cuando su elemento primario en el diseño llama al método [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) para el panel. Es habitual que el valor *finalSize* de entrada y el valor **ArrangeOverride** devuelto de **Size** sean iguales. Si no lo son, significa que el panel está intentando dimensionarse con un tamaño diferente del que los demás participantes del diseño aseguran que está disponible. El tamaño final se obtiene después de haber ejecutado previamente el pase de medición del diseño por todo el código del panel, por lo que no es habitual que se devuelva un tamaño diferente: significa que has pasado por alto la lógica deliberadamente.

No devuelvas un valor [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995) con un componente **Infinity**. Si intentas usar **Size**, se produce una excepción en el diseño interno.

Todas las implementaciones [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) deben recorrer en bucle [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) y llamar al método [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) para cada elemento secundario. Al igual que [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952), **Arrange** no tiene un valor devuelto. A diferencia de **Measure**, ninguna propiedad calculada se establece como resultado (sin embargo, el elemento en cuestión normalmente activa un evento [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722)).

Esta es una estructura muy básica de un método [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711):

```CSharp
protected override Size ArrangeOverride(Size finalSize)
{
    //loop through each Child, call Arrange on each
    foreach (UIElement child in Children)
    {
        Point anchorPoint = new Point(); //TODO more logic for topleft corner placement in your panel
       // for this child, and based on finalSize or other internal state of your panel
        child.Arrange(new Rect(anchorPoint, child.DesiredSize)); //OR, set a different Size 
    }
    return finalSize; //OR, return a different Size, but that's rare
}
```

El pase de organización del diseño podría realizarse sin un pase de medición previo. Sin embargo, esto solo sucede cuando el sistema de diseño ha determinado que no ha cambiado ninguna propiedad que pueda afectar a las mediciones anteriores. Por ejemplo, si una alineación cambia, no es necesario volver a medir ese elemento en particular porque su [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) no cambiaría cuando cambie su opción de alineación. Por otra parte, si [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) cambia en cualquier elemento de un diseño, se debe pasar una nueva medición. El sistema de diseño detecta automáticamente los cambios de medición reales, invoca de nuevo el pase de medición y después ejecuta otro pase de organización.

La entrada de [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) toma un valor [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994). La manera más habitual de construir este valor **Rect** es usar el constructor que tiene una entrada [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) y una entrada [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995). **Point** es el punto donde se colocará la esquina superior izquierda del cuadro de límite del elemento. **Size** es la dimensión que se usará para representar ese elemento en particular. Se suele usar el valor de [**DesiredSize**](https://msdn.microsoft.com/library/windows/apps/br208921) para ese elemento como este valor de **Size**, porque el propósito del pase de medición del diseño era establecer el valor de **DesiredSize** para todos los elementos implicados en el diseño. (El pase de medición determina el tamaño vertical de los elementos de manera iterativa para que el sistema de diseño pueda optimizar la manera en que se colocan los elementos cuando llega al pase de organización).

Lo que suele variar entre las implementaciones [**ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711) es la lógica con la que el panel determina el componente [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870) con el que organiza cada elemento. Un panel de posicionamiento absoluto como [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) usa la información de colocación explícita que obtiene de cada elemento mediante los valores [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) y [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772). Un panel de división del espacio como [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) tendría operaciones matemáticas que dividen el espacio disponible en celdas y cada celda tendría un valor x-y donde el contenido debe colocarse y organizarse. Un panel adaptable como [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635) podría expandirse automáticamente para ajustarse al contenido en la dimensión de su orientación.

Además de lo que puedes controlar y pasar directamente a [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914), existen también otros factores que influyen en la posición de los elementos en el diseño. Estos factores proceden de la implementación nativa interna de **Arrange** que es común a todos los tipos derivados de [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706) y que otros tipos aumentan, como los elementos de texto. Por ejemplo, unos elementos pueden tener margen y alineación, y otros pueden tener espaciado. Estas propiedades suelen interactuar. Para obtener más información, consulta [alineación, margen y espaciado](alignment-margin-padding.md).

## <a name="panels-and-controls"></a>Paneles y controles


Evita colocar funcionalidad en un panel personalizado que debería crearse en su lugar como un control personalizado. El rol de un panel es presentar el contenido de los elementos secundarios que pueda haber en él, como una función de diseño que se produce automáticamente. El panel podría agregar decoraciones al contenido (de forma similar al modo en que [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) agrega el borde alrededor del elemento que presenta), o bien realizar otros ajustes relacionados con el diseño, como el espaciado. En cualquier caso, eso es lo más que deberías ampliar el resultado del árbol visual aparte de notificar y usar la información del elemento secundario.

Si hay alguna interacción que sea accesible para el usuario, debes escribir un control personalizado, no un panel. Por ejemplo, un panel no debe agregar ventanillas de desplazamiento para el contenido que presenta, aunque el objetivo sea evitar el recorte, porque las barras de desplazamiento y las miniaturas, entre otras, son partes de control interactivo. (Después de todo, el contenido podría tener barras de desplazamiento, pero debes dejar eso a la lógica del elemento secundario. No lo fuerces agregando desplazamiento como una operación de diseño). Podrías crear un control y escribir también un panel personalizado que juegue un papel importante en el árbol visual de ese control cuando se trate de presentar contenido en dicho control. Pero el control y el panel deben ser objetos de código distintos.

Un motivo por el que la distinción entre control y panel es importante, es la automatización de la interfaz de usuario de Microsoft y la accesibilidad. Los paneles proporcionan un comportamiento de diseño visual, no lógico. Normalmente, la apariencia visual de un elemento de la interfaz de usuario no es algo importante en escenarios de accesibilidad. La accesibilidad consiste en mostrar las partes de una aplicación que son importantes desde un punto de vista lógico para entender una interfaz de usuario. Cuando se necesita interacción, los controles deben exponer las posibilidades de interacción a la infraestructura de automatización de la interfaz de usuario. Para obtener más información, consulta [Personalizar sistemas de automatización del mismo nivel](https://msdn.microsoft.com/library/windows/apps/mt297667).

## <a name="other-layout-api"></a>Otras API de diseño


Hay otras API que forman parte del sistema de diseño, pero no se declaran mediante [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511). Pueden usarse en una implementación de panel o en un control personalizado que use paneles.

-   [**UpdateLayout**](https://msdn.microsoft.com/library/windows/apps/br208989), [**InvalidateMeasure**](https://msdn.microsoft.com/library/windows/apps/br208930) e [**InvalidateArrange**](https://msdn.microsoft.com/library/windows/apps/br208929) son métodos que inician un pase de diseño. Es posible que **InvalidateArrange** no inicie un pase de medición, pero los otros dos sí lo hacen. No llames nunca a estos métodos desde la invalidación de un método de diseño porque provocarán un bucle de diseño casi con total seguridad. Normalmente, el código de control tampoco necesita llamarlos. La mayoría de los aspectos de diseño se inician automáticamente cuando se detectan cambios en las propiedades de diseño definidas por el entorno, como [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) y otras.
-   [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) es un evento que se activa cuando cambia algún aspecto del elemento. No es específico de los paneles; el evento se define mediante [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706).
-   [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br208742) es un evento que se activa solo cuando finalizan los pases de diseño e indica que [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) o [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) han cambiado como resultado. Este es otro evento [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706). Hay casos en los que [**LayoutUpdated**](https://msdn.microsoft.com/library/windows/apps/br208722) se activa, pero **SizeChanged** no. Por ejemplo, puede que el contenido interno se reorganice, pero que el tamaño del elemento no cambie.


## <a name="related-topics"></a>Temas relacionados

**Referencia**
* [**FrameworkElement.ArrangeOverride**](https://msdn.microsoft.com/library/windows/apps/br208711)
* [**FrameworkElement.MeasureOverride**](https://msdn.microsoft.com/library/windows/apps/br208730)
* [**Panel**](https://msdn.microsoft.com/library/windows/apps/br227511)

**Conceptos**
* [Alineación, margen y espaciado](alignment-margin-padding.md)
