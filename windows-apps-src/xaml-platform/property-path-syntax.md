---
author: jwmsft
description: Puedes usar la clase PropertyPath y la sintaxis de cadena para crear una instancia de un valor PropertyPath en XAML o en código.
title: Sintaxis de Property-path
ms.assetid: FF3ECF47-D81F-46E3-BE01-C839E0398025
---

# Sintaxis de Property-path

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Puedes usar la clase [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) y la sintaxis de cadena para crear una instancia de un valor **PropertyPath** en XAML o en código. El enlace de datos usa los valores de **PropertyPath**. Una sintaxis similar se usa para seleccionar el destino de las animaciones de guión gráfico. Pero la selección de destino de las animaciones no crea valores de Property-path syntax subyacentes, sino que mantiene la información como una cadena. Para los dos escenarios, una ruta de acceso de propiedades describe un cruce seguro de una o más relaciones de propiedades de objeto que finalmente se resuelven en una sola propiedad.

Puedes establecer una cadena de ruta de acceso de propiedades directamente en un atributo en XAML. Es más, puedes usar la misma sintaxis de cadena para construir una clase [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) que establezca una clase [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820) en el código, o establecer un destino de animación en el código mediante [**SetTargetProperty**](https://msdn.microsoft.com/library/windows/apps/br210503). Existen dos áreas de características distintas en Windows Runtime que usan una ruta de acceso de propiedades: el enlace de datos y el destino de animaciones. La selección del destino de animaciones no crea valores de Property-path syntax subyacentes en la implementación de Windows en tiempo de ejecución, sino que mantiene la información como una cadena, pero los conceptos de cruce seguro de propiedades de objetos son muy similares. El enlace de datos y la selección del destino de animaciones evalúan una ruta de acceso de propiedades de un modo ligeramente diferente, de forma que describiremos la sintaxis de la ruta de acceso de propiedad por separado para cada uno.

## Ruta de acceso de propiedades para objetos en el enlace de datos

En Windows en tiempo de ejecución, puedes crear enlaces en el valor de destino de cualquier propiedad de dependencias. El valor de la propiedad de origen de un enlace de datos no tiene por qué ser una propiedad de dependencias; puede ser una propiedad en un objeto empresarial (por ejemplo, una clase escrita en un lenguaje Microsoft .NET o C++). O, el objeto de origen del valor de enlace puede ser una un objeto de dependencias existente ya definido por la aplicación. Pueden hacer referencia al origen un simple nombre de propiedad o un cruce seguro de las relaciones entre propiedades y objetos del gráfico del objeto empresarial.

Puedes crear un enlace con un valor de propiedad individual, o con una propiedad de destino que contenga listas o colecciones. Si el origen es una colección, o si la ruta de acceso especifica una propiedad de colección, el motor de enlace de datos hará que los elementos de la colección del origen coincidan con el destino de enlace; esto dará como resultado un comportamiento similar a llenar una clase [**ListBox**](https://msdn.microsoft.com/library/windows/apps/br242868) con una lista de elementos de una colección de orígenes de datos sin tener que adelantar los elementos específicos de esa colección.

### Atravesar un gráfico de objeto

El elemento de la sintaxis que denota el cruce seguro de una relación entre propiedades y objetos en el gráfico de objetos es el carácter de punto (**.**). Cada punto en una cadena de ruta de acceso de propiedades indica una división entre un objeto (lado izquierdo del punto) y una propiedad de ese objeto (lado derecho del punto). La cadena se evalúa de izquierda a derecha, lo que habilita el paso a través de varias relaciones de propiedades de objetos. Veamos un ejemplo:

``` syntax
<Binding Path="Customer.Address.StreetAddress1"
```

Esta ruta de acceso se evalúa del modo siguiente:

1.  Se busca en el objeto de contexto de datos (o en una propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br209832) especificada por la misma clase [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820)) una propiedad denominada "Customer".
2.  Se busca en el objeto que corresponde al valor de la propiedad "Customer" una propiedad denominada "Address".
3.  Se busca en el objeto que corresponde al valor de la propiedad "Address" una propiedad llamada "StreetAddress1".

En cada uno de estos pasos, el valor se trata como un objeto. El tipo de resultado se comprueba solo cuando se aplica el enlace a una propiedad específica. Este ejemplo daría un error si "Address" fuera solo un valor de cadena que no indicara qué parte de la cadena corresponde a la dirección. Por lo general, el enlace apunta a los valores de propiedades anidados específicos de un objeto empresarial que tiene una estructura de información conocida y deliberada.

### Reglas de las propiedades en una ruta de acceso de propiedades de enlace de datos

-   Todas las propiedades a las que hace referencia una ruta de acceso de propiedades deben ser públicas en el objeto empresarial de origen.
-   La propiedad final (la propiedad que es la última propiedad con nombre de la ruta de acceso) debe ser pública y mutable; no se puede enlazar con valores estáticos.
-   La propiedad final debe ser de lectura y escritura si esta ruta de acceso se usa como información de la propiedad [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) para un enlace bidireccional.

### Indexadores

Una ruta de acceso de propiedades para el enlace de datos puede incluir referencias a propiedades indexadas. Esto te permite habilitar el enlace a listas y vectores ordenadas, o a diccionarios y mapas. Usa corchetes "\[\]" para indicar una propiedad indexada. El contenido de estos corchetes puede ser un entero (para la lista ordenada) o una cadena sin comillas (para los diccionarios). También puedes enlazar con un diccionario en el que la clave sea un entero. Puedes usar propiedades indizadas diferentes en la misma ruta de acceso con un punto separando la propiedad del objeto.

Por ejemplo, imagina que tienes un objeto empresarial en el que hay una lista denominada "Teams" (lista ordenada) en la cual, cada equipo consta de un diccionario denominado "Players" donde se puede encontrar a cada integrante según su apellido. Un ejemplo de ruta de acceso de propiedades a un integrante específico del segundo equipo sería: "Teams\[1\].Players\[Smith\]". (Debes usar 1 para indicar el segundo elemento en "Teams" porque la lista tiene un índice de cero).

**Nota** La compatibilidad con la indexación de los orígenes de datos de C++ es limitada; consulta [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

### Propiedades adjuntas

Las rutas de acceso de propiedades pueden incluir referencias a propiedades adjuntas. Como el nombre identificador de una propiedad adjunta ya incluye un punto, deberás encerrar el nombre de la propiedad adjunta entre paréntesis para que el punto no se considere un paso de propiedad de objeto. Por ejemplo, la cadena dedicada a especificar que quieres usar [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773) como ruta de acceso de enlace es "(Canvas.ZIndex)". Para obtener más información sobre las propiedades adjuntas, consulta [Introducción a las propiedades adjuntas](attached-properties-overview.md).

### Combinación de sintaxis de ruta de acceso de propiedades

Puedes combinar varios elementos de la sintaxis de ruta de acceso de propiedades en una sola cadena. Por ejemplo, puedes definir una ruta de acceso de propiedades que haga referencia a una propiedad adjunta indizada si tu origen de datos tuviera dicha propiedad.

### Depuración de una ruta de acceso de propiedades de enlace

Como la ruta de acceso de la propiedad se interpreta mediante un motor de enlace y depende de la información que pueda estar presente solo en tiempo de ejecución, deberás depurar con frecuencia una ruta de acceso de propiedades del enlace sin poder depender de la compatibilidad convencional de las opciones tiempo de diseño o del tiempo de compilación que encontrarás en las herramientas de desarrollo. En muchos casos, el resultado en tiempo de ejecución de error al resolver una ruta de acceso de propiedades es un valor en blanco sin errores, porque ese es el comportamiento de reserva que tiene la resolución de enlaces. Afortunadamente, Microsoft Visual Studio proporciona un modo de salida de depuración que puede aislar la parte de una ruta de acceso de propiedades que especifica un origen de enlace que no se pudo resolver. Para más información sobre el uso de esta herramientas de desarrollo, consulta la sección "Depuración" del tema [Enlace de datos en profundidad](../data-binding/data-binding-in-depth.md#debugging).

## Ruta de acceso de propiedades para selección de destino de animaciones

Las animaciones dependen de la selección del destino de una propiedad de dependencias en la que los valores de guión gráfico se aplican cuando se ejecuta la animación. Para identificar el objeto en el que existe la propiedad que se va a animar, la animación selecciona como destino un elemento con el nombre ([atributo x:Name](x-name-attribute.md)). Con frecuencia es necesario definir una ruta de acceso de propiedades que comience con el objeto identificado como [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823) y que termine con el valor de propiedad de dependencias particular en el que debe aplicarse la animación. La ruta de acceso de propiedades se usa como valor de la propiedad [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/hh759824).

Para obtener más información sobre cómo definir animaciones en XAML, consulta [Animaciones con guion gráfico](https://msdn.microsoft.com/library/windows/apps/mt187354).

## Selección de destino simple

Si vas a animar una propiedad que ya existe en el propio objeto de destino y al tipo de esa propiedad se le puede aplicar directamente una animación (en vez de aplicársela a una subpropiedad del valor de una propiedad), entonces podrás asignar un nombre a la propiedad que se va a animar sin una cualificación adicional. Por ejemplo, si seleccionas como destino una subclase [**Shape**](https://msdn.microsoft.com/library/windows/apps/br243377) como [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/br243371), y aplicas una estructura [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) animada a la propiedad [**Fill**](https://msdn.microsoft.com/library/windows/apps/br243378), la ruta de acceso de la propiedad podrá ser "Fill".

## Selección indirecta del destino de propiedades

Puedes animar una propiedad que es una subpropiedad del objeto de destino. Dicho de otro modo, si hay una propiedad del objeto de destino que es un objeto en sí mismo, y ese objeto tiene propiedades, deberás definir una ruta de acceso de propiedades que explique cómo pasar por esa relación de propiedades de objeto. Siempre que especifiques el objeto donde deseas animar una subpropiedad, puedes escribir el nombre de la propiedad entre paréntesis y especificar la propiedad en el formato *typename*.*propertyname*. Por ejemplo, para especificar que quieres ver el valor de objeto de la propiedad [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/br208980) de un objeto de destino, primero debes especificar "(UIElement.RenderTransform)" en la ruta de acceso de la propiedad. Recuerda que esto no es aún una ruta de acceso completa, ya que no hay animaciones que se puedan aplicar a un valor de la clase [**Transform**](https://msdn.microsoft.com/library/windows/apps/br243006) directamente. Así pues, en este ejemplo completarás la ruta de acceso de la propiedad de forma que la propiedad final sea una propiedad de una subclase **Transform** a la cual pueda animar un valor **Double**: "(UIElement.RenderTransform).(CompositeTransform.TranslateX)"

## Especificar un elemento secundario en particular de una colección

Para especificar un elemento secundario de una propiedad de colección, puedes usar un indexador numérico. Usa corchetes "\[\]" alrededor del valor del índice de entero. Ten en cuenta que puedes hacer referencia solo a listas ordenadas y no a diccionarios. Como una colección no es un valor que se pueda animar, el uso de un indizador nunca puede ser la propiedad final en una ruta de acceso de propiedades.

Por ejemplo, para especificar que quieres animar el primer punto de parada de color en una clase [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/br210108) que se aplique a la propiedad [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) de un control, esta es la ruta de acceso de la propiedad que debes usar: "(Control.Background).(GradientBrush.GradientStops)\[0\].(GradientStop.Color)". Ten en cuenta que el indexador no es el último paso de la ruta de acceso, sino que el último paso debe hacer referencia, en particular, a la propiedad [**GradientStop.Color**](https://msdn.microsoft.com/library/windows/apps/br210094) del elemento 0 que se encuentra en la colección, para aplicarle un valor animado [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723).

## Animar una propiedad adjunta

No suele ser habitual, pero es posible animar una propiedad adjunta siempre que esta tenga un valor que coincida con un tipo de animación. Como el nombre identificador de una propiedad adjunta ya incluye un punto, deberás encerrar el nombre de la propiedad adjunta entre paréntesis para que el punto no se considere un paso de propiedad de objeto. Por ejemplo, la cadena para especificar que quieres animar la propiedad adjunta [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) en un objeto, usa la ruta de acceso de propiedad "(Grid.Row)".

**Nota** En este ejemplo, el valor de [**Grid.Row**](https://msdn.microsoft.com/library/windows/apps/hh759795) es un tipo de propiedad **Int32**. Debido a ello, no podrás animarlo con una animación **Double**. En cambio, sí que puedes definir una clase [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320) que tenga componentes [**DiscreteObjectKeyFrame**](https://msdn.microsoft.com/library/windows/apps/br243132) en los cuales la propiedad [**ObjectKeyFrame.Value**](https://msdn.microsoft.com/library/windows/apps/br210344) esté establecida como un entero "0" o "1".

## Reglas de las propiedades en una ruta de acceso de propiedades de selección de destino de animaciones

-   El punto inicial supuesto de la ruta de acceso de propiedades es el objeto identificado por una propiedad [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/hh759823).
-   Todos los objetos y las propiedades a los que se hace referencia en la ruta de acceso de propiedad deben ser públicos.
-   La propiedad final (la propiedad que es la última propiedad con nombre de la ruta de acceso) debe ser pública, de escritura y de dependencias.
-   La propiedad final debe ser un tipo de propiedad que sea capaz de animarse mediante una de las amplias clases de tipos de animaciones (por ejemplo, de tipo [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723), **Double**, [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/br210320), etc.).

## Clase PropertyPath

La clase [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) es el tipo de propiedad subyacente de [**Binding.Path**](https://msdn.microsoft.com/library/windows/apps/br209830) del escenario de enlace.

La mayor parte de las veces, puedes aplicar una clase [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259) en XAML sin tener usar ningún código en absoluto. Pero en algunos casos, deberás definir un objeto **PropertyPath** mediante código y asignarlo a una propiedad en tiempo de ejecución.

[
              **PropertyPath**
            ](https://msdn.microsoft.com/library/windows/apps/br244259) tiene un constructor [**PropertyPath(String)**](https://msdn.microsoft.com/library/windows/apps/br244261), pero no tiene un constructor predeterminado. La cadena que pases a ese constructor deberá ser una cadena definida mediante la sintaxis de ruta de acceso de propiedades, tal como ya hemos explicado. Además, es también la misma cadena que usarías para asignar [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) como atributo XAML. Ten en cuenta que la única API de la clase **PropertyPath** es la propiedad [**Path**](https://msdn.microsoft.com/library/windows/apps/br244260), la cual es de solo lectura. Puedes usar esta propiedad como la cadena de construcción de otra instancia **PropertyPath**.

## Temas relacionados

* [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946)
* [Animaciones con guion gráfico](https://msdn.microsoft.com/library/windows/apps/mt187354)
* [Extensión de marcado {Binding}](binding-markup-extension.md)
* [**PropertyPath**](https://msdn.microsoft.com/library/windows/apps/br244259)
* [**Binding (Enlace)**](https://msdn.microsoft.com/library/windows/apps/br209820)
* [**Binding constructor (Constructor Binding)**](https://msdn.microsoft.com/library/windows/apps/br209825)
* [**DataContext**](https://msdn.microsoft.com/library/windows/apps/br208713)



<!--HONumber=May16_HO2-->


