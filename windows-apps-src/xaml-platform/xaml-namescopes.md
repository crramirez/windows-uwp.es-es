---
description: Un ámbito de nombres XAML almacena relaciones entre los nombres de objetos definidos en XAML y sus equivalentes de instancia. Este concepto es similar al significado más amplio del término ámbito de nombres en otros lenguajes y tecnologías de programación.
title: Ámbitos de nombres XAML
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e4fa430cdd6c2f7b47576478ec30f0f139b80363
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173759"
---
# <a name="xaml-namescopes"></a>Ámbitos de nombres XAML


Un *ámbito de nombres XAML* almacena relaciones entre los nombres de objetos definidos en XAML y sus equivalentes de instancia. Este concepto es similar al significado más amplio del término *ámbito* de nombres en otras tecnologías y lenguajes de programación.

## <a name="how-xaml-namescopes-are-defined"></a>Cómo se definen los ámbitos de nombres XAML

Los nombres de los ámbitos de nombres XAML permiten al código de usuario hacer referencia a los objetos que se declararon inicialmente en XAML. El resultado interno del análisis de XAML es que el tiempo de ejecución crea un conjunto de objetos que conservan algunas o todas las relaciones que esos objetos tenían en las declaraciones XAML. Estas relaciones se mantienen como propiedades de objeto específicas de los objetos creados, o se exponen a los métodos de utilidad en las API del modelo de programación.

El uso más común de un nombre en un ámbito de nombres XAML es como referencia directa a una instancia de objeto, que se habilita en el pase de compilación del marcado como una acción de compilación del proyecto, combinada con un método **InitializeComponent** generado en las plantillas de clase parcial.

También puedes usar el método de utilidad [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) en tiempo de ejecución para devolver una referencia a los objetos que se definieron con un nombre en el marcado XAML.

### <a name="more-about-build-actions-and-xaml"></a>Más información sobre las acciones de compilación y XAML

Técnicamente, lo que sucede es que el código XAML se somete a un pase del compilador de marcado al mismo tiempo que el código XAML y la clase parcial que define para el código subyacente se compilan juntos. Cada elemento de objeto con un **Name** o [atributo x:Name](x-name-attribute.md) definido en el marcado genera un campo interno con un nombre que coincide con el nombre XAML. Este campo inicialmente está vacío. Después, la clase genera un método **InitializeComponent** que solo se llama cuando todo el código XAML está cargado. Dentro de la lógica de **InitializeComponent**, cada campo interno se rellena con el valor devuelto [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) para la cadena de nombre equivalente. Para observar esta infraestructura por ti mismo, examina los archivos ".g" (generados) que se crean para cada página XAML en la subcarpeta /obj de un proyecto de aplicación de Windows Runtime después de la compilación. También puedes considerar a los campos y el método **InitializeComponent** como miembros de los ensamblados resultantes si reflexionas sobre ellos o examinas el contenido del lenguaje de la interfaz.

**Nota:**    Específicamente para las aplicaciones de Visual C++ extensiones de componentes (C++/CX), no se crea un campo de respaldo para una referencia de **x:Name** para el elemento raíz de un archivo XAML. Si necesitas hacer referencia al objeto raíz desde el código subyacente en C++/CX, usa otras API o un cruce seguro de árbol. Por ejemplo, puedes llamar a [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) para un elemento secundario con nombre conocido y, después, llamar a [**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent).

## <a name="creating-objects-at-run-time-with-xamlreaderload"></a>Creación de objetos en tiempo de ejecución con XamlReader.Load

El lenguaje XAML también se puede usar como la entrada de cadena del método [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load), que actúa de manera similar a la operación de análisis de origen XAML inicial. **XamlReader.Load** crea un nuevo árbol desconectado de objetos en tiempo de ejecución. Después, el árbol desconectado puede conectarse a algún punto del árbol de objetos principal. Debes conectar explícitamente el árbol de objetos creado; para ello, agrégalo a una colección de propiedades de contenido como **Children**, o establece otra propiedad que acepte un valor de objeto (por ejemplo, si cargas un nuevo [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) para un valor de propiedad [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill)).

### <a name="xaml-namescope-implications-of-xamlreaderload"></a>Implicaciones del ámbito de nombres XAML de XamlReader.Load 

El ámbito de nombres XAML preliminar que se define con el nuevo árbol de objetos creado por [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) evalúa los nombres definidos en el código XAML proporcionado para comprobar su exclusividad. Si los nombres del código XAML proporcionado no son internamente exclusivos en este punto, **XamlReader.Load** inicia una excepción. El árbol de objetos desconectado no intenta combinar su ámbito de nombres XAML con el ámbito de nombres XAML de la aplicación principal, si está conectado al árbol de objetos de la aplicación principal. Después de conectar los árboles, la aplicación contará con un árbol de objetos unificado, pero con ámbitos de nombres XAML discretos en su interior. Las divisiones se producen en los puntos de conexión entre los objetos, donde estableces que alguna propiedad sea el valor devuelto de una llamada **XamlReader.Load**.

El problema de tener ámbitos de nombres XAML discretos y desconectados es que las llamadas al método [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname), así como las referencias a objetos administrados directos, ya no operan con un ámbito de nombres XAML unificado. En su lugar, el objeto en particular para el que se llama a **FindName** determina el ámbito, siendo este el ámbito de nombres XAML dentro del cual se encuentra el objeto llamador. En el caso de una referencia a objetos administrados directos, el ámbito se determina mediante la clase donde existe el código. Por lo general, el código subyacente para la interacción en tiempo de ejecución de una "página" de contenido de la aplicación existe en la clase parcial que respalda la "página" raíz y, por lo tanto, el ámbito de nombres XAML es el ámbito de nombres XAML raíz.

Si llamas a [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) para obtener un objeto con nombre del ámbito de nombres XAML raíz, el método no encontrará los objetos en un ámbito de nombres XAML discreto creado por [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load). A la inversa, si llamas a **FindName** desde un objeto obtenido a partir del ámbito de nombres XAML discreto, el método no encontrará objetos con nombre en el ámbito de nombres XAML raíz.

Este problema del ámbito de nombres XAML discreto solo afecta a la búsqueda de objetos por nombre en los ámbitos de nombres XAML cuando se usa la llamada [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname).

Puedes usar varias técnicas para obtener referencias a los objetos definidos en un ámbito de nombres XAML diferente:

-   Recorra todo el árbol en pasos discretos con propiedades de colección y [**elementos primarios**](/uwp/api/windows.ui.xaml.frameworkelement.parent) que se sabe que existen en la estructura de árbol de objetos (por ejemplo, la colección devuelta por [**panel. Children**](/uwp/api/windows.ui.xaml.controls.panel.children)).
-   Si haces la llamada desde un ámbito de nombres XAML discreto y deseas el ámbito de nombres XAML raíz, siempre es fácil obtener una referencia a la ventana principal que se muestra actualmente. Puedes obtener la raíz visual (el elemento XAML raíz, también conocido como origen del contenido) de la ventana de la aplicación actual en una línea de código con la llamada `Window.Current.Content`. Después puedes convertirla a [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) y llamar a [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) desde este ámbito.
-   Si estás llamando desde el ámbito de nombres XAML raíz y deseas un objeto que esté dentro de un ámbito de nombres XAML discreto, lo más conveniente es que planifiques de antemano el código y conserves una referencia al objeto devuelto por [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) y agregado al árbol de objetos principal. Este objeto ahora es un objeto válido para llamadas [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) dentro del ámbito de nombres XAML discreto. Podrías mantener este objeto disponible como variable global o pasarlo con parámetros del método.
-   Para evitar por completo las consideraciones acerca de los nombres y los ámbitos de nombres XAML, examina el árbol visual. La API [**VisualTreeHelper**](/uwp/api/Windows.UI.Xaml.Media.VisualTreeHelper) te permite atravesar el árbol visual en términos de objetos principales y colecciones secundarias exclusivamente según la posición y el índice.

## <a name="xaml-namescopes-in-templates"></a>Ámbitos de nombres XAML en plantillas

En XAML, las plantillas ofrecen la posibilidad de volver a usar y aplicar contenido de una manera sencilla, pero las plantillas también pueden incluir elementos con nombres definidos en el nivel de plantilla.  Esa misma plantilla se puede usar varias veces en una página. Por esta razón, las plantillas definen sus propios ámbitos de nombres XAML sin importar la página contenedora donde se aplica el estilo o la plantilla. Considere este ejemplo:

```xml
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  >
  <Page.Resources>
    <ControlTemplate x:Key="MyTemplate">
      ....
      <TextBlock x:Name="MyTextBlock" />
    </ControlTemplate>
  </Page.Resources>
  <StackPanel>
    <SomeControl Template="{StaticResource MyTemplate}" />
    <SomeControl Template="{StaticResource MyTemplate}" />
  </StackPanel>
</Page>
```

Aquí se aplica la misma plantilla a dos controles diferentes. Si las plantillas no tenían ámbitos de nombres XAML discretos, el nombre "MyTextBlock" que se usó en la plantilla provocaría un conflicto de nombres. Todas las instancias de la plantilla tienen su propio ámbito de nombres XAML, por lo que, en este ejemplo, el ámbito de nombres XAML de todas las plantillas de instancia contendrían exactamente un nombre. Sin embargo, el ámbito de nombres XAML raíz no contiene el nombre de ninguna de las plantillas.

Debido a la separación de los ámbitos de nombres XAML, buscar elementos con nombre dentro de una plantilla desde el ámbito de la página donde se aplicó la plantilla requiere una técnica diferente. En lugar de llamar a [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) en algún objeto del árbol de objetos, primero se obtiene el objeto al que se aplicó la plantilla y después se llama a [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild). Si eres el autor del control y estás generando una convención por la que un determinado elemento con nombre de una plantilla aplicada es el destino para un comportamiento definido por el propio control, puedes usar el método **GetTemplateChild** desde el código de implementación del control. El método **GetTemplateChild** está protegido, por lo que solo el autor del control tiene acceso a él. Además, existen convenciones que los autores de controles deben seguir para asignar nombres a las partes y a las partes de las plantillas, y notificarlas como valores de atributo que se aplican a la clase del control. Esta técnica permite que los usuarios del control puedan detectar los nombres de partes importantes; además, es posible que deseen aplicar una plantilla distinta, para lo que sería necesario reemplazar las partes con nombre a fin de mantener la funcionalidad del control.

## <a name="related-topics"></a>Temas relacionados

* [Introducción a XAML](xaml-overview.md)
* [Atributo x:Name](x-name-attribute.md)
* [Inicio rápido: plantillas de control](/previous-versions/windows/apps/hh465374(v=win.10))
* [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)
* [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)
 