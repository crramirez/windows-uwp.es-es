---
description: Puedes usar la clase PropertyPath y la sintaxis de cadena para crear una instancia de un valor PropertyPath en XAML o en código.
title: Sintaxis de property-path
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5a3cac608bbc985bb8c0db0e0cece219b3c566a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155039"
---
# <a name="property-path-syntax"></a>Sintaxis de property-path


Puede usar la clase [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) y la sintaxis de cadena para crear instancias de un valor **PropertyPath** en XAML o en código. El enlace de datos usa los valores de **PropertyPath**. Una sintaxis similar se usa para seleccionar el destino de las animaciones de guión gráfico. Para los dos escenarios, una ruta de acceso de propiedades describe un cruce seguro de una o más relaciones de propiedades de objeto que finalmente se resuelven en una sola propiedad.

Puedes establecer una cadena de ruta de acceso de propiedades directamente en un atributo en XAML. Es más, puedes usar la misma sintaxis de cadena para construir una clase [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) que establezca una clase [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) en el código, o establecer un destino de animación en el código mediante [**SetTargetProperty**](/uwp/api/windows.ui.xaml.media.animation.storyboard.settargetproperty). Existen dos áreas de características distintas en Windows Runtime que usan una ruta de acceso de propiedades: el enlace de datos y el destino de animaciones. La selección del destino de animaciones no crea valores de Property-path syntax subyacentes en la implementación de Windows en tiempo de ejecución, sino que mantiene la información como una cadena, pero los conceptos de cruce seguro de propiedades de objetos son muy similares. El enlace de datos y la selección del destino de animaciones evalúan una ruta de acceso de propiedades de un modo ligeramente diferente, de forma que describiremos la sintaxis de la ruta de acceso de propiedad por separado para cada uno.

## <a name="property-path-for-objects-in-data-binding"></a>Ruta de acceso de propiedades para objetos en el enlace de datos

En Windows en tiempo de ejecución, puedes crear enlaces en el valor de destino de cualquier propiedad de dependencias. El valor de la propiedad de origen de un enlace de datos no tiene por qué ser una propiedad de dependencias; puede ser una propiedad en un objeto empresarial (por ejemplo, una clase escrita en un lenguaje Microsoft .NET o C++). O, el objeto de origen del valor de enlace puede ser una un objeto de dependencias existente ya definido por la aplicación. Pueden hacer referencia al origen un simple nombre de propiedad o un cruce seguro de las relaciones entre propiedades y objetos del gráfico del objeto empresarial.

Puedes crear un enlace con un valor de propiedad individual, o con una propiedad de destino que contenga listas o colecciones. Si el origen es una colección, o si la ruta de acceso especifica una propiedad de colección, el motor de enlace de datos hará que los elementos de la colección del origen coincidan con el destino de enlace; esto dará como resultado un comportamiento similar a llenar una clase [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) con una lista de elementos de una colección de orígenes de datos sin tener que adelantar los elementos específicos de esa colección.

### <a name="traversing-an-object-graph"></a>Atravesar un gráfico de objeto

El elemento de la sintaxis que denota el cruce seguro de una relación entre propiedades y objetos en el gráfico de objetos es el carácter de punto (**.**). Cada punto en una cadena de ruta de acceso de propiedades indica una división entre un objeto (lado izquierdo del punto) y una propiedad de ese objeto (lado derecho del punto). La cadena se evalúa de izquierda a derecha, lo que habilita el paso a través de varias relaciones de propiedades de objetos. Veamos un ejemplo:

``` syntax
"{Binding Path=Customer.Address.StreetAddress1}"
```

Esta ruta de acceso se evalúa del modo siguiente:

1.  Se busca en el objeto de contexto de datos (o en una propiedad [**Source**](/uwp/api/windows.ui.xaml.data.binding.source) especificada por la misma clase [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding)) una propiedad denominada "Customer".
2.  Se busca en el objeto que corresponde al valor de la propiedad "Customer" una propiedad denominada "Address".
3.  Se busca en el objeto que corresponde al valor de la propiedad "Address" una propiedad llamada "StreetAddress1".

En cada uno de estos pasos, el valor se trata como un objeto. El tipo de resultado se comprueba solo cuando se aplica el enlace a una propiedad específica. Este ejemplo daría un error si "Address" fuera solo un valor de cadena que no indicara qué parte de la cadena corresponde a la dirección. Por lo general, el enlace apunta a los valores de propiedades anidados específicos de un objeto empresarial que tiene una estructura de información conocida y deliberada.

### <a name="rules-for-the-properties-in-a-data-binding-property-path"></a>Reglas de las propiedades en una ruta de acceso de propiedades de enlace de datos

-   Todas las propiedades a las que hace referencia una ruta de acceso de propiedades deben ser públicas en el objeto empresarial de origen.
-   La propiedad final (la propiedad que es la última propiedad con nombre de la ruta de acceso) debe ser pública y mutable; no se puede enlazar con valores estáticos.
-   La propiedad final debe ser de lectura y escritura si esta ruta de acceso se usa como información de la propiedad [**Path**](/uwp/api/windows.ui.xaml.data.binding.path) para un enlace bidireccional.

### <a name="indexers"></a>Indizadores

Una ruta de acceso de propiedades para el enlace de datos puede incluir referencias a propiedades indexadas. Esto te permite habilitar el enlace a listas y vectores ordenadas, o a diccionarios y mapas. Use corchetes " \[ \] " para indicar una propiedad indizada. El contenido de estos corchetes puede ser un entero (para la lista ordenada) o una cadena sin comillas (para los diccionarios). También puedes enlazar con un diccionario en el que la clave sea un entero. Puedes usar propiedades indizadas diferentes en la misma ruta de acceso con un punto separando la propiedad del objeto.

Por ejemplo, imagina que tienes un objeto empresarial en el que hay una lista denominada "Teams" (lista ordenada) en la cual, cada equipo consta de un diccionario denominado "Players" donde se puede encontrar a cada integrante según su apellido. Una ruta de acceso de propiedad de ejemplo a un reproductor específico en el segundo equipo es: "Teams \[ 1 \] . Jugadores \[ Smith \] ". (Debes usar 1 para indicar el segundo elemento en "Teams" porque la lista tiene un índice de cero).

**Nota:**    La compatibilidad con la indización de los orígenes de datos de C++ es limitada; consulte [enlace de datos en profundidad](../data-binding/data-binding-in-depth.md).

### <a name="attached-properties"></a>Propiedades adjuntas

Las rutas de acceso de propiedades pueden incluir referencias a propiedades adjuntas. Como el nombre identificador de una propiedad adjunta ya incluye un punto, deberás encerrar el nombre de la propiedad adjunta entre paréntesis para que el punto no se considere un paso de propiedad de objeto. Por ejemplo, la cadena dedicada a especificar que quieres usar [**Canvas.ZIndex**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) como ruta de acceso de enlace es "(Canvas.ZIndex)". Para obtener más información sobre las propiedades adjuntas, consulta [Introducción a las propiedades adjuntas](attached-properties-overview.md).

### <a name="combining-property-path-syntax"></a>Combinación de sintaxis de ruta de acceso de propiedades

Puedes combinar varios elementos de la sintaxis de ruta de acceso de propiedades en una sola cadena. Por ejemplo, puedes definir una ruta de acceso de propiedades que haga referencia a una propiedad adjunta indizada si tu origen de datos tuviera dicha propiedad.

### <a name="debugging-a-binding-property-path"></a>Depuración de una ruta de acceso de propiedades de enlace

Como la ruta de acceso de la propiedad se interpreta mediante un motor de enlace y depende de la información que pueda estar presente solo en tiempo de ejecución, deberás depurar con frecuencia una ruta de acceso de propiedades del enlace sin poder depender de la compatibilidad convencional de las opciones tiempo de diseño o del tiempo de compilación que encontrarás en las herramientas de desarrollo. En muchos casos, el resultado en tiempo de ejecución de error al resolver una ruta de acceso de propiedades es un valor en blanco sin errores, porque ese es el comportamiento de reserva que tiene la resolución de enlaces. Afortunadamente, Microsoft Visual Studio proporciona un modo de salida de depuración que puede aislar la parte de una ruta de acceso de propiedades que especifica un origen de enlace que no se pudo resolver. Para más información sobre el uso de esta herramientas de desarrollo, consulta la sección "Depuración" del tema [Enlace de datos en profundidad](../data-binding/data-binding-in-depth.md#debugging).

## <a name="property-path-for-animation-targeting"></a>Ruta de acceso de propiedades para selección de destino de animaciones

Las animaciones dependen de la selección del destino de una propiedad de dependencias en la que los valores de guión gráfico se aplican cuando se ejecuta la animación. Para identificar el objeto en el que existe la propiedad que se va a animar, la animación selecciona como destino un elemento con el nombre ([atributo x:Name](x-name-attribute.md)). Con frecuencia es necesario definir una ruta de acceso de propiedades que comience con el objeto identificado como [**Storyboard.TargetName**](/dotnet/api/system.windows.media.animation.storyboard.targetname) y que termine con el valor de propiedad de dependencias particular en el que debe aplicarse la animación. La ruta de acceso de propiedades se usa como valor de la propiedad [**Storyboard.TargetProperty**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms616983(v=vs.95)).

Para obtener más información sobre cómo definir animaciones en XAML, consulta [Animaciones con guion gráfico](../design/motion/storyboarded-animations.md).

## <a name="simple-targeting"></a>Selección de destino simple

Si vas a animar una propiedad que ya existe en el propio objeto de destino y al tipo de esa propiedad se le puede aplicar directamente una animación (en vez de aplicársela a una subpropiedad del valor de una propiedad), entonces podrás asignar un nombre a la propiedad que se va a animar sin una cualificación adicional. Por ejemplo, si seleccionas como destino una subclase [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) como [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), y aplicas una estructura [**Color**](/uwp/api/Windows.UI.Color) animada a la propiedad [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill), la ruta de acceso de la propiedad podrá ser "Fill".

## <a name="indirect-property-targeting"></a>Selección indirecta del destino de propiedades

Puedes animar una propiedad que es una subpropiedad del objeto de destino. Dicho de otro modo, si hay una propiedad del objeto de destino que es un objeto en sí mismo, y ese objeto tiene propiedades, deberás definir una ruta de acceso de propiedades que explique cómo pasar por esa relación de propiedades de objeto. Siempre que especifiques el objeto donde deseas animar una subpropiedad, puedes escribir el nombre de la propiedad entre paréntesis y especificar la propiedad en el formato *typename*.*propertyname*. Por ejemplo, para especificar que quieres ver el valor de objeto de la propiedad [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) de un objeto de destino, primero debes especificar "(UIElement.RenderTransform)" en la ruta de acceso de la propiedad. Recuerda que esto no es aún una ruta de acceso completa, ya que no hay animaciones que se puedan aplicar a un valor de la clase [**Transform**](/uwp/api/Windows.UI.Xaml.Media.Transform) directamente. Así pues, en este ejemplo completarás la ruta de acceso de la propiedad de forma que la propiedad final sea una propiedad de una subclase **Transform** a la cual pueda animar un valor **Double**: "(UIElement.RenderTransform).(CompositeTransform.TranslateX)"

## <a name="specifying-a-particular-child-in-a-collection"></a>Especificar un elemento secundario en particular de una colección

Para especificar un elemento secundario de una propiedad de colección, puedes usar un indexador numérico. Use corchetes " \[ \] " en torno al valor de índice de entero. Ten en cuenta que puedes hacer referencia solo a listas ordenadas y no a diccionarios. Como una colección no es un valor que se pueda animar, el uso de un indizador nunca puede ser la propiedad final en una ruta de acceso de propiedades.

Por ejemplo, para especificar que desea animar el primer color de detención de color de un [**LinearGradientBrush**](/uwp/api/Windows.UI.Xaml.Media.LinearGradientBrush) que se aplica a la propiedad de [**fondo**](/uwp/api/windows.ui.xaml.controls.control.background) de un control, esta es la ruta de acceso de la propiedad: "(control. Background). (GradientBrush. GradientStops) \[ 0 \] . ( GradientStop. color) ". Ten en cuenta que el indexador no es el último paso de la ruta de acceso, sino que el último paso debe hacer referencia, en particular, a la propiedad [**GradientStop.Color**](/uwp/api/windows.ui.xaml.media.gradientstop.color) del elemento 0 que se encuentra en la colección, para aplicarle un valor animado [**Color**](/uwp/api/Windows.UI.Color).

## <a name="animating-an-attached-property"></a>Animar una propiedad adjunta

No suele ser habitual, pero es posible animar una propiedad adjunta siempre que esta tenga un valor que coincida con un tipo de animación. Como el nombre identificador de una propiedad adjunta ya incluye un punto, deberás encerrar el nombre de la propiedad adjunta entre paréntesis para que el punto no se considere un paso de propiedad de objeto. Por ejemplo, la cadena para especificar que quieres animar la propiedad adjunta [**Grid.Row**](/dotnet/api/system.windows.controls.grid.row) en un objeto, usa la ruta de acceso de propiedad "(Grid.Row)".

**Nota:**    En este ejemplo, el valor de [**Grid. Row**](/dotnet/api/system.windows.controls.grid.row) es un tipo de propiedad **Int32** . Debido a ello, no podrás animarlo con una animación **Double**. En cambio, sí que puedes definir una clase [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) que tenga componentes [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) en los cuales la propiedad [**ObjectKeyFrame.Value**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.value) esté establecida como un entero "0" o "1".

## <a name="rules-for-the-properties-in-an-animation-targeting-property-path"></a>Reglas de las propiedades en una ruta de acceso de propiedades de selección de destino de animaciones

-   El punto inicial supuesto de la ruta de acceso de propiedades es el objeto identificado por una propiedad [**Storyboard.TargetName**](/dotnet/api/system.windows.media.animation.storyboard.targetname).
-   Todos los objetos y las propiedades a los que se hace referencia en la ruta de acceso de propiedad deben ser públicos.
-   La propiedad final (la propiedad que es la última propiedad con nombre de la ruta de acceso) debe ser pública, de escritura y de dependencias.
-   La propiedad final debe ser un tipo de propiedad que sea capaz de animarse mediante una de las amplias clases de tipos de animaciones (por ejemplo, de tipo [**Color**](/uwp/api/Windows.UI.Color), **Double**, [**Point**](/uwp/api/Windows.Foundation.Point), [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames), etc.).

## <a name="the-propertypath-class"></a>Clase PropertyPath

La clase [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) es el tipo de propiedad subyacente de [**Binding.Path**](/uwp/api/windows.ui.xaml.data.binding.path) del escenario de enlace.

La mayor parte de las veces, puedes aplicar una clase [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) en XAML sin tener usar ningún código en absoluto. Pero en algunos casos, deberás definir un objeto **PropertyPath** mediante código y asignarlo a una propiedad en tiempo de ejecución.

[**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath) tiene un constructor [**PropertyPath(String)**](/uwp/api/windows.ui.xaml.propertypath.-ctor), pero no tiene un constructor predeterminado. La cadena que pases a ese constructor deberá ser una cadena definida mediante la sintaxis de ruta de acceso de propiedades, tal como ya hemos explicado. Además, es también la misma cadena que usarías para asignar [**Path**](/uwp/api/windows.ui.xaml.data.binding.path) como atributo XAML. Ten en cuenta que la única API de la clase **PropertyPath** es la propiedad [**Path**](/uwp/api/windows.ui.xaml.propertypath.path), la cual es de solo lectura. Puedes usar esta propiedad como la cadena de construcción de otra instancia **PropertyPath**.

## <a name="related-topics"></a>Temas relacionados

* [Enlace de datos en profundidad](../data-binding/data-binding-in-depth.md)
* [Animaciones con guion gráfico](../design/motion/storyboarded-animations.md)
* [Extensión de marcado {Binding}](binding-markup-extension.md)
* [**PropertyPath**](/uwp/api/Windows.UI.Xaml.PropertyPath)
* [**Enlace**](/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**Binding constructor (Constructor Binding)**](/uwp/api/windows.ui.xaml.data.binding.-ctor)
* [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)