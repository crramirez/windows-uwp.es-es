---
description: A continuación explicamos las reglas de la sintaxis XAML y la terminología que describe las restricciones o alternativas que existen en la sintaxis XAML.
title: Guía de sintaxis XAML
ms.assetid: A57FE7B4-9947-4AA0-BC99-5FE4686B611D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e988582877a6aa4ca3cf88ba0a5d98aceb56939e
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7693356"
---
# <a name="xaml-syntax-guide"></a>Guía de sintaxis XAML


A continuación explicamos las reglas de la sintaxis XAML y la terminología que describe las restricciones o alternativas que existen en la sintaxis XAML. Este tema te resultará muy útil si no tienes experiencia en el uso del lenguaje XAML, si deseas recordar la terminología o las partes de la sintaxis, o si sientes curiosidad acerca del funcionamiento del lenguaje XAML y quieres conocerlo mejor.

## <a name="xaml-is-xml"></a>XAML es XML

El lenguaje de marcado de aplicaciones extensible (XAML) tiene una sintaxis básica basada en XML. Por lo tanto, por definición, todo código XAML válido debe ser código XML válido. Sin embargo, XAML también tiene sus propios conceptos de sintaxis que amplían XML. Si bien una entidad XML determinada podría ser válida en XML sin formato, esa sintaxis podría tener un significado diferente y más completo como XAML. En este tema se explican estos conceptos de sintaxis XAML.

## <a name="xaml-vocabularies"></a>Vocabularios XAML

Un área en la que XAML difiere de la mayoría de usos de XML es que XAML no suele exigir un esquema, como un archivo XSD. Esto se debe a que XAML está pensado para ser extensible. Precisamente eso es lo que indica la "X" de la sigla XAML. Una vez que XAML se analiza, se espera que los elementos y atributos a los que haces referencia en XAML existan en alguna representación de código de respaldo, ya sea en los tipos principales definidos por Windows en tiempo de ejecución o en los tipos que se extienden o se basan en Windows en tiempo de ejecución. A veces, la documentación del SDK hace referencia a los tipos ya integrados en Windows en tiempo de ejecución y que se pueden usar en XAML como el *vocabulario XAML* para Windows en tiempo de ejecución. Microsoft Visual Studio te ayuda a producir marcado que sea válido dentro de este vocabulario XAML. Visual Studio también puede incluir tus tipos personalizados para el uso de XAML siempre que el origen de esos tipos esté referenciado correctamente en el proyecto. Para obtener más información sobre XAML y los tipos personalizados, consulta [Espacios de nombres XAML y asignación de espacios de nombres](xaml-namespaces-and-namespace-mapping.md).

##  <a name="declaring-objects"></a>Declaración de objetos

Los programadores a menudo piensan en términos de objetos y miembros, mientras que un lenguaje de marcado se conceptualiza como elementos y atributos. En el sentido más básico, un elemento que declares en un marcado XAML se convierte en un objeto en una representación de objetos en tiempo de ejecución de respaldo. Para crear un objeto en tiempo de ejecución para tu aplicación, declaras un elemento XAML en el marcado XAML. El objeto se crea cuando Windows en tiempo de ejecución carga el XAML.

Un archivo XAML siempre tiene exactamente un elemento que funciona como raíz y que declara un objeto que será la raíz conceptual de parte de la estructura de programación, como una página, o el gráfico de objeto de toda la definición en tiempo de ejecución de una aplicación.

En lo que respecta a la sintaxis XAML, hay tres formas de declarar objetos en XAML:

-   **Directamente, mediante la sintaxis de elementos de objeto:** se usan etiquetas de apertura y cierre y así poder crear instancias de un objeto como un elemento de formulario XML. Puedes usar esta sintaxis para declarar objetos raíz o para crear objetos anidados que establecen valores de propiedad.
-   **Indirectamente, mediante la sintaxis de atributo:** se usa un valor de cadena insertado que contiene instrucciones sobre la creación de un objeto. El analizador XAML utiliza esta cadena para establecer el valor de una propiedad en un valor de referencia recién creado. Solo se admite en determinados objetos y propiedades comunes.
-   Usando una extensión de marcado.

Esto no significa siempre puedas elegir cualquiera de las sintaxis para crear un objeto en un vocabulario XAML. Algunos objetos solo se pueden crear con la sintaxis de elemento de objeto. Algunos objetos solo se pueden crear si se establecen inicialmente en un atributo. De hecho, los objetos que pueden crearse tanto con la sintaxis de elemento de objeto como de atributo son en comparación relativamente infrecuentes en los vocabularios XAML. Aunque ambas formas de sintaxis sean posibles, una de ellas será más común por cuestión de estilo.
También hay técnicas que puedes usar en XAML para hacer referencia a objetos existentes en lugar de crear valores nuevos. Los objetos existentes podrían definirse en otras áreas de XAML, o podrían existir implícitamente a través de ciertos comportamientos de la plataforma y sus modelos de aplicación o programación.

### <a name="declaring-an-object-by-using-object-element-syntax"></a>Declarar un objeto mediante el uso de una sintaxis de elemento de objeto

Para declarar un objeto con sintaxis de elemento de objeto, escribes etiquetas como esta: `<objectName>  </objectName>`, donde *objectName* es el nombre de tipo del objeto cuya instancia deseas crear. Este es el uso del elemento de objeto para declarar un objeto [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267):

```xml
<Canvas>
</Canvas>
```

Si el objeto no contiene otros objetos, puedes declarar el elemento de objeto usando una etiqueta de autocierre en lugar de un par de apertura/cierre:  `<Canvas />`

### <a name="containers"></a>Contenedores

Muchos objetos que se usan como elementos de interfaz de usuario, como [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), pueden contener otros objetos. Se los suele denominar contenedores. El siguiente ejemplo muestra un contenedor **Canvas** que contiene un elemento: un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<Canvas>
  <Rectangle />
</Canvas>
```

### <a name="declaring-an-object-by-using-attribute-syntax"></a>Declarar un objeto mediante la sintaxis de atributo

Dado que este comportamiento está vinculado a la configuración de propiedades, lo describiremos con más detalle en próximas secciones.

### <a name="initialization-text"></a>Texto de inicialización

En algunos objetos, puedes declarar valores nuevos mediante texto interno que se usa como valores de inicialización para la creación. En XAML, esta técnica y sintaxis se denomina *texto de inicialización*. Conceptualmente, el texto de inicialización se asemeja a llamar a un constructor que tenga parámetros. El texto de inicialización resulta útil para establecer los valores iniciales de ciertas estructuras.

A menudo usas una sintaxis de elemento de objeto con texto de inicialización si deseas un valor de estructura con un **x:Key**, de manera que puede existir en un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794). Podrías hacerlo si compartes ese valor de estructura entre varias propiedades de destino. En algunas estructuras, no puedes usar la sintaxis de atributo para establecer los valores de la estructura: el texto de inicialización es la única manera de producir un recurso [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/br242343), [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864), [**GridLength**](https://msdn.microsoft.com/library/windows/apps/br208754) o [**Color**](https://msdn.microsoft.com/library/windows/apps/hh673723) útil que pueda compartirse.

Este ejemplo abreviado usa el texto de inicialización para especificar valores para un [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864); en este caso, especificando los valores que establecen **Left** y **Right** en 20, y **Top** y **Bottom** en 10. Este ejemplo muestra el **Thickness** que se crea como recurso con clave y después la referencia a ese recurso. Para obtener más información sobre el texto de inicialización de [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864), consulta [**Thickness**](https://msdn.microsoft.com/library/windows/apps/br208864).

```xml
<UserControl ...>
  <UserControl.Resources>
    <Thickness x:Key="TwentyTenThickness">20,10</Thickness>
    ....
  </UserControl.Resources>
  ...
  <Grid Margin="{StaticResource TwentyTenThickness}">
  ...
  </Grid>
</UserControl ...>
```

**Nota**algunas estructuras no se pueden declarar como elementos de objeto. No se admite el texto de inicialización y no pueden usarse como recursos. Debes usar una sintaxis de atributo para establecer las propiedades en estos valores en XAML. Estos tipos son: [**Duration**](https://msdn.microsoft.com/library/windows/apps/br242377), [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/br210411), [**Point**](https://msdn.microsoft.com/library/windows/apps/br225870), [**Rect**](https://msdn.microsoft.com/library/windows/apps/br225994) y [**Size**](https://msdn.microsoft.com/library/windows/apps/br225995).

## <a name="setting-properties"></a>Establecer propiedades

Puedes usar la sintaxis de elemento de objeto para establecer las propiedades de los objetos que declares. Existen varias formas de establecer propiedades en XAML:

-   Mediante la sintaxis de atributo.
-   Mediante la sintaxis de elemento de propiedad.
-   Mediante la sintaxis de elemento en la que el contenido (texto interno o elementos secundarios) establece la propiedad de contenido XAML de un objeto.
-   Mediante una sintaxis de colección (que suele ser la sintaxis de colección implícita).

Al igual que en la declaración de objetos, esta lista no implica que todas las propiedades se puedan establecer con cualquiera de las técnicas. Algunas propiedades admiten solo una de las técnicas.
Algunas propiedades admiten más de una forma; por ejemplo, hay propiedades que pueden usar sintaxis de elemento de propiedad o sintaxis de atributo. Lo que es posible depende de la propiedad y del tipo de objeto que use la propiedad. En la referencia de la API de Windows en tiempo de ejecución, verás los posibles usos de XAML en la sección **Sintaxis**. A veces, existe un uso alternativo que podría funcionar pero que sería más detallado. Esos usos detallados no se muestran siempre, ya que intentamos mostrarte los procedimientos recomendados o escenarios de la vida real para el uso de esa propiedad en XAML. La orientación de la sintaxis XAML se proporciona en las secciones **Uso de XAML** de las páginas de referencia de las propiedades que se pueden establecer en XAML.

Hay algunas propiedades en objetos que no se pueden establecer en XAML de ninguna manera y que solo pueden establecerse mediante código. Normalmente se trata de propiedades más adecuadas para trabajar en el código subyacente y no en XAML.

Una propiedad de solo lectura no se puede establecer en XAML. Incluso en el código, el tipo propietario debería admitir alguna otra forma de establecerla, como una sobrecarga de constructor, un método auxiliar o compatibilidad con propiedades calculadas. Una propiedad calculada depende de los valores de otras propiedades configurables y, en ocasiones, también de un evento con control integrado; estas características están disponibles en el sistema de propiedades de dependencia. Para obtener más información sobre el modo en que las propiedades de dependencia son útiles para la compatibilidad con propiedades calculadas, consulta [Introducción a las propiedades de dependencia](dependency-properties-overview.md).

La sintaxis de colección en XAML da la sensación de que estás estableciendo una propiedad de solo lectura, pero en realidad no es así. Consulta la sección "Establecer una propiedad con una sintaxis de colección" más adelante en este tema.

### <a name="setting-a-property-by-using-attribute-syntax"></a>Establecer una propiedad con una sintaxis de atributo

Establecer un valor de atributo es el medio típico por el cual estableces un valor de propiedad en un lenguaje de marcado, por ejemplo XML o HTML. El proceso de establecimiento de atributos XAML es similar al modo en que se establecen valores de atributo en XML. El nombre de atributo se especifica en cualquier punto entre las etiquetas después del nombre de elemento, separado de este por al menos un espacio en blanco. El nombre de atributo está seguido de un signo igual. El valor de atributo queda encerrado entre comillas. Las comillas pueden ser dobles o simples, siempre que coincidan para encerrar el valor. El valor de atributo en sí mismo debe expresarse como una cadena. La cadena a menudo contiene numerales, pero para XAML todos los valores de atributo son valores de cadena hasta que el analizador XAML interviene y realiza algún tipo de conversión de valores básica.

Este ejemplo usa una sintaxis de atributo de cuatro atributos para establecer las propiedades [**Name**](https://msdn.microsoft.com/library/windows/apps/br208735), [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) y [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) de un objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

```xml
<Rectangle Name="rectangle1" Width="100" Height="100" Fill="Blue" />
```

### <a name="setting-a-property-by-using-property-element-syntax"></a>Establecer una propiedad con una sintaxis de elemento de propiedad

Se pueden establecer varias propiedades de un objeto con una sintaxis de elemento de propiedad. Un elemento de propiedad tiene el siguiente aspecto: `<`*object*`.`*property*`>`.

Para usar una sintaxis de elemento de propiedad, creas elementos de propiedad XAML para la propiedad que deseas establecer. En XML estándar, este elemento se consideraría un elemento que tiene un punto en su nombre. Sin embargo, en XAML, el punto en el nombre de elemento identifica el elemento como un elemento de propiedad, con *property* como un miembro de *object* en la implementación de un modelo de objetos de respaldo. Para usar la sintaxis de elemento de propiedad, se debe poder especificar un elemento de objeto para "rellenar" las etiquetas de elemento de propiedad. Un elemento de propiedad siempre tendrá algún contenido (elemento único, varios elementos o texto interno); no tiene sentido tener un elemento de propiedad de autocierre.

En la siguiente gramática, *property* es el nombre de la propiedad que deseas establecer y *propertyValueAsObjectElement* es un elemento de objeto único que se espera que satisfaga los requisitos de tipo de valor de la propiedad.

`<`*object*`>`

`<`*object*`.`*property*`>`

*propertyValueAsObjectElement*

`</`*object*`.`*property*`>`

`</`*object*`>`

El siguiente ejemplo usa la sintaxis de elemento de propiedad para establecer el [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) de un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) con un elemento de objeto [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962). (Dentro del **SolidColorBrush**, [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) se establece como un atributo). El resultado analizado de este XAML es idéntico al ejemplo de XAML anterior que establecía **Fill** mediante la sintaxis de atributo.

```xml
<Rectangle
  Name="rectangle1"
  Width="100" 
  Height="100"
> 
  <Rectangle.Fill> 
    <SolidColorBrush Color="Blue"/> 
  </Rectangle.Fill>
</Rectangle>
```

### <a name="xaml-vocabularies-and-object-oriented-programming"></a>Vocabularios XAML y programación orientada a objetos

Las propiedades y los eventos tal y como aparecen como miembros XAML de un tipo XAML de Windows Runtime se suelen heredar de los tipos base. Observa este ejemplo: `<Button Background="Blue" .../>`. La propiedad [**Background**](https://msdn.microsoft.com/library/windows/apps/br209395) no es una propiedad declarada de inmediato en la clase [**Button**](https://msdn.microsoft.com/library/windows/apps/br209265). En lugar de ello, **Background** se hereda de la clase base [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390). De hecho, si observas el tema de referencia de **Button**, verás que las listas de miembros contienen al menos un miembro heredado de cada una de las clases de una cadena de clases base sucesivas: [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736), [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390), [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/br208706), [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911), [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). En la lista **Properties** , todas las propiedades de lectura y escritura y las propiedades de colección se heredan en un vocabulario XAML. También se heredan los eventos (como los diversos eventos [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)).

Si usas la referencia de Windows Runtime como orientación de XAML, el nombre de elemento que se muestra en una sintaxis o incluso en un código de ejemplo a veces corresponde al tipo que define originalmente la propiedad, ya que el tema de referencia lo comparten todos los tipos posibles que lo heredan de una clase base. Si usas IntelliSense para XAML de Visual Studio en el editor XML, IntelliSense y sus listas desplegables fusionan eficazmente la herencia y proporcionan una lista precisa de atributos que pueden establecerse en cuanto se empieza a usar un elemento de objeto para una instancia de clase.

### <a name="xaml-content-properties"></a>Propiedades de contenido XAML

Algunos tipos definen una de sus propiedades de manera tal que la propiedad habilita una sintaxis de contenido XAML. Para la propiedad de contenido XAML de un tipo, puedes omitir el elemento de propiedad de esa propiedad cuando lo especifiques en XAML. O bien, puedes establecer la propiedad en un valor de texto interno proporcionando directamente el texto interno dentro de las etiquetas del elemento de objeto del tipo propietario. Las propiedades de contenido XAML admiten la sintaxis de marcado directa para esa propiedad y reducen el anidamiento para que el XAML sea más legible para las personas.

Si hay una sintaxis de contenido XAML disponible, esa sintaxis se mostrará en las secciones "XAML" de **Sintaxis** de esa propiedad en la documentación de referencia de Windows Runtime. Por ejemplo, la página de propiedades [**Child**](https://msdn.microsoft.com/library/windows/apps/br209258) de [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250) muestra una sintaxis de contenido XAML en lugar de una sintaxis de elemento de propiedad para establecer el valor **Border.Child** de objeto único de un **Border**, como en este ejemplo::

```xml
<Border>
  <Button .../>
</Border>
```

Si la propiedad que se declara como la propiedad de contenido XAML es el tipo **Object** o el tipo **String**, la sintaxis de contenido XAML admite lo que es básicamente texto interno en el modelo de documento XML: una cadena entre las etiquetas de apertura y cierre de objeto. Por ejemplo, la página de propiedades [**Text**](https://msdn.microsoft.com/library/windows/apps/br209676) de [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) muestra una sintaxis de contenido XAML que tiene un valor de texto interno para establecer **Text**, pero la cadena "Text" no aparece nunca en el marcado. Observa el siguiente ejemplo de uso:

```xml
<TextBlock>Hello!</TextBlock>
```

Si hay una propiedad de contenido XAML para una clase, se indica en la sección "Atributos" del tema de referencia de la clase. Busca el valor de [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011). Este atributo usa un campo denominado "Name". El valor de "Name" es el nombre de la propiedad de esa clase que corresponde a la propiedad de contenido XAML. Por ejemplo, en la página de referencia de [**Border**](https://msdn.microsoft.com/library/windows/apps/br209250), verás lo siguiente: ContentProperty("Name=Child").

Debemos mencionar una regla de sintaxis XAML importante: no puedes mezclar la propiedad de contenido XAML y otros elementos de propiedad que establezcas en el elemento. La propiedad de contenido XAML debe establecerse completamente antes de cualquier elemento de propiedad, o bien completamente después. Por ejemplo, esto sería un ejemplo de XAML no válido:

``` syntax
<StackPanel>
  <Button>This example</Button>
  <StackPanel.Resources>
    <SolidColorBrush x:Key="BlueBrush" Color="Blue"/>
  </StackPanel.Resources>
  <Button>... is illegal XAML</Button>
</StackPanel>
```

## <a name="collection-syntax"></a>Sintaxis de colección

Todas las sintaxis mostradas hasta el momento son para establecer propiedades en objetos únicos. Sin embargo, muchos escenarios de interfaz de usuario requieren que un elemento principal determinado pueda tener varios elementos secundarios. Por ejemplo, la interfaz de usuario de un formulario de entrada necesita varios elementos de cuadro de texto, algunas etiquetas y quizás un botón "Enviar". Sin embargo, si usaras un modelo de objetos de programación para acceder a estos elementos, por lo general serían elementos de una única propiedad de colección en lugar de que cada elemento sea el valor de diferentes propiedades. XAML admite varios elementos secundarios y también un modelo de colección de respaldo típico; para ello, trata las propiedades que usan un tipo de colección como implícito y lleva a cabo un control especial de los elementos secundarios de un tipo de colección.

Muchas propiedades de colección también se identifican como la propiedad de contenido XAML para la clase. La combinación de procesamiento de colección implícito y sintaxis de contenido XAML suele verse en los tipos que se usan para componer controles, como controles de elementos, vistas o paneles. Por ejemplo, los siguientes ejemplos muestran el código XAML más simple posible para combinar dos elementos de interfaz de usuario del mismo nivel en un [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635).

```xml
<StackPanel>
  <TextBlock>Hello</TextBlock>
  <TextBlock>World</TextBlock>
</StackPanel>
```

### <a name="the-mechanism-of-xaml-collection-syntax"></a>Mecanismo de la sintaxis de colección XAML

Al principio podría parecer que XAML está habilitando un "conjunto" de la propiedad de colección de solo lectura. En realidad, lo que XAML habilita aquí es la incorporación de elementos a una colección existente. El lenguaje XAML y los procesadores XAML que implementan compatibilidad para XAML dependen de una convención en los tipos de colección de respaldo para habilitar esta sintaxis. Habitualmente, existe una propiedad de respaldo como un indizador o una propiedad **Items** que hace referencia a elementos específicos de la colección. Por lo general, esa propiedad no es explícita en la sintaxis XAML. En las colecciones, el mecanismo subyacente del análisis de XAML no es una propiedad sino un método; específicamente, el método **Add** en la mayoría de los casos. Cuando el procesador XAML encuentra uno o más elementos de objeto en una sintaxis de colección XAML, cada uno de esos objetos se crea primero a partir de un elemento, después se agrega cada uno de los objetos nuevos en orden a la colección contenedora llamando al método **Add** de la colección.

Cuando un analizador XAML agrega elementos a una colección, es la lógica del método **Add** la que determina si un elemento XAML determinado es un elemento secundario aceptable para el objeto de colección. Muchos tipos de colecciones son fuertemente tipados por la implementación de respaldo, lo que significa que el parámetro de entrada de **Add** espera que el tipo de cualquier elemento que se pase coincida con el tipo del parámetro de **Add**.

En las propiedades de colección, ten cuidado al intentar especificar la colección explícitamente como un elemento de objeto. Un analizador XAML creará un objeto nuevo cuando encuentre un elemento de objeto. Si la propiedad de colección que estás intentando usar es de solo lectura, podría lanzar una excepción de análisis XAML. Si usas simplemente la sintaxis de colección implícita no verás dicha excepción.

## <a name="when-to-use-attribute-or-property-element-syntax"></a>Cuándo usar una sintaxis de elemento de propiedad o atributo

Todas las propiedades que admiten ser establecidas en XAML admitirán una sintaxis de elemento de atributo o de propiedad para establecer el valor directamente, pero podrían no admitir ambas sintaxis indistintamente. Algunas propiedades admiten las dos sintaxis, y algunas propiedades admiten otras opciones de sintaxis como una propiedad de contenido XAML. El tipo de sintaxis XAML que admite una propiedad depende del tipo de objeto que la propiedad utiliza como tipo de propiedad. Si el tipo de propiedad es un tipo primitivo, como un doble (flotante o decimal), un entero, un valor booleano o una cadena, la propiedad siempre admite la sintaxis de atributo.

También puedes usar la sintaxis de atributo para establecer una propiedad si el tipo de objeto que usas para establecerla se puede crear mediante el procesamiento de una cadena. En el caso de tipos primitivos, la conversión de tipos siempre está integrada en el analizador. Sin embargo, también se pueden crear otros tipos de objeto usando una cadena especificada como un valor de atributo, en lugar de un elemento de objeto en un elemento de propiedad Para que funcione, debe haber una conversión de tipos subyacente admitida por una determinada propiedad o admitida en general para todos los valores que usan ese tipo de propiedad. El valor de la cadena del atributo se usa para establecer propiedades que son importantes para la inicialización del valor de objeto nuevo. Un convertidor de tipos específicos también podría crear diferentes subclases de un tipo de propiedad común, en función de la manera exclusiva que tenga de procesar la información de la cadena. Los tipos de objeto que admiten este comportamiento tendrán una gramática especial indicada en la sección relativa a la sintaxis de la documentación de referencia. Como ejemplo, la sintaxis XAML para [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) muestra cómo se puede usar una sintaxis de atributo para crear un valor de [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) nuevo para cualquier propiedad del tipo **Brush** (y hay muchas propiedades **Brush** en el XAML de Windows Runtime).

## <a name="xaml-parsing-logic-and-rules"></a>Lógica y reglas de análisis XAML

En ocasiones resulta revelador leer el XAML de manera similar al modo en que un analizador XAML debe leerlo: como un conjunto de tokens de cadena colocados en orden lineal. Un analizador XAML debe interpretar estos tokens bajo un conjunto de reglas que forman parte de la definición del funcionamiento de XAML.

Establecer un valor de atributo es el medio típico por el cual estableces un valor de propiedad en un lenguaje de marcado, por ejemplo XML o HTML. En la siguiente sintaxis, *objectName* es el objeto para el que deseas crear una instancia, *propertyName* es el nombre de la propiedad que deseas establecer en el objeto y *propertyValue* es el valor para establecer.

```xml
<objectName propertyName="propertyValue" .../>

-or-

<objectName propertyName="propertyValue">

...<!--element children -->

</objectName>
```

Cualquiera de las dos sintaxis te permite declarar un objeto y establecer una propiedad en ese objeto. Aunque el primer ejemplo sea de un solo elemento en el marcado, en realidad contiene pasos discretos sobre la manera en que un procesador XAML analiza este marcado.

En primer lugar, la presencia del elemento de objeto indica que se debe crear una instancia de un objeto *objectName* nuevo. Solamente cuando exista dicha instancia se puede establecer la propiedad de instancia *propertyName*.

Otra regla de XAML es que los atributos de un elemento deben poder establecerse en cualquier orden. Por ejemplo, no existe ninguna diferencia entre `<Rectangle Height="50" Width="100" />` y `<Rectangle Width="100"  Height="50" />`. El orden utilizado es una cuestión de estilo.

**Nota**diseñadores XAML suelen promoción para ordenar cuando se usan superficies de diseño que no sean el editor XML, pero puedes editar libremente ese código XAML más adelante, para reordenar los atributos e introducir otros nuevos.

## <a name="attached-properties"></a>Propiedades adjuntas

XAML amplía XML mediante la adición de un elemento de sintaxis conocido como *propiedad adjunta*. Al igual que la sintaxis de elemento de propiedad, la sintaxis de propiedad adjunta contiene un punto, y este tiene un significado especial en el análisis de XAML. Específicamente, el punto separa el proveedor de propietario de la propiedad adjunta y el nombre de la propiedad.

En XAML, las propiedades adjuntas se establecen con la sintaxis *AttachedPropertyProvider*.*PropertyName* .
Este es un ejemplo de cómo puede establecer la propiedad adjunta [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) en XAML:

```xml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

La propiedad adjunta se puede establecer para elementos que no tienen una propiedad con ese nombre en el tipo de respaldo, y de manera que funcionen de forma similar a una propiedad global, o un atributo definido por un espacio de nombres XML distinto, como el atributo **xml:space**.

En XAML de Windows Runtime verás propiedades adjuntas que admiten estos escenarios:

-   Elementos secundarios que pueden informar a los paneles del contenedor primario acerca de su comportamiento en el diseño: [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704), [**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/br227651).
-   Usos de controles que pueden influir en el comportamiento de una parte del control importante procedente de la plantilla de control: [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527), [**VirtualizingStackPanel**](https://msdn.microsoft.com/library/windows/apps/br227689).
-   Uso de un servicio disponible en una clase relacionada, donde el servicio y la clase que usa no comparten la herencia: [**Typography**](https://msdn.microsoft.com/library/windows/apps/hh702143), [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/br209021), [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/br209081), [**ToolTipService**](https://msdn.microsoft.com/library/windows/apps/br227609).
-   Selección del destino de animaciones: [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490).

Para obtener más información, consulta [Introducción a las propiedades adjuntas](attached-properties-overview.md).

## <a name="literal--values"></a>Valores "{" literales

Ya que la llave de apertura \{ es la apertura de la secuencia de la extensión de marcado, debes usar una secuencia de escape para especificar un valor de cadena literal que comienza con "\{". La secuencia de escape es "\{\}". Por ejemplo, para especificar un valor de cadena que es una única llave de apertura, especifica el valor de atributo como "\{\}\{". También puedes usar otras comillas (por ejemplo, **'** dentro de un valor de atributo delimitado por **""**) para proporcionar un valor "\{" como cadena.

**Nota**"{\\}" también funciona si se encuentra dentro de un atributo entre comillas.
 
## <a name="enumeration-values"></a>Valores de enumeración

Muchas propiedades de la API de Windows en tiempo de ejecución usan enumeraciones como valores. Si el miembro es una propiedad de solo lectura puedes establecer dicha propiedad proporcionando un valor de atributo. Para identificar el valor de enumeración que debes usar como valor de la propiedad, debes usar el nombre no completo del nombre de constante. Por ejemplo, [**UIElement.Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) se establecería en XAML así: `<Button Visibility="Visible"/>`. Aquí el "Visible" como una cadena se asigna directamente a una constante con nombre de la enumeración [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br209006), **Visible**.

-   No uses la forma completa, porque no funcionará. Por ejemplo, esto sería un ejemplo de XAML no válido: `<Button Visibility="Visibility.Visible"/>`
-   No uses el valor de la constante. Es decir, no utilices el valor entero de la enumeración, que se encuentra allí de forma explícita o implícita, en función de cómo se definió la enumeración. Aunque parezca que funcione, se trata de una mala práctica en XAML o en código, ya que estás confiando en un detalle de implementación que podría ser transitorio. Por ejemplo, no uses esto: `<Button Visibility="1"/>`.

**Nota**en los temas de referencia de API que usan XAML y enumeraciones, haz clic en el vínculo del tipo de enumeración en la sección de **valor de propiedad** de **sintaxis**. Este vínculo lleva a la página de la enumeración, donde puedes encontrar las constantes con nombre para esa enumeración.

Las enumeraciones pueden estar basadas en marcas, lo que quiere decir que tienen el atributo **FlagsAttribute**. Si necesitas especificar una combinación de valores para una enumeración basada en marcas como un valor de atributo XAML, usa el nombre de cada constante de enumeración, con una coma (,) entre cada nombre y sin caracteres de espacio entre ellos. Los atributos basados en marcas no son comunes en el vocabulario XAML de Windows Runtime, pero [**ManipulationModes**](https://msdn.microsoft.com/library/windows/apps/br227934) es un ejemplo que admite la configuración de un valor de enumeración basada en marcas en XAML.

## <a name="interfaces-in-xaml"></a>Interfaces en XAML

En raras ocasiones, verás una sintaxis XAML donde el tipo de una propiedad es una interfaz. En el sistema de tipos XAML, un tipo que implementa esa interfaz es aceptable como valor cuando se analiza. Debe haber una instancia creada de dicho tipo disponible para que sirva de valor. Verás una interfaz usada como tipo en la sintaxis XAML para las propiedades [**Command**](https://msdn.microsoft.com/library/windows/apps/br227740) y [**CommandParameter**](https://msdn.microsoft.com/library/windows/apps/br227741) de [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/br227736). Estas propiedades admiten los modelos de diseño Model-View-ViewModel (MVVM) en que la interfaz **ICommand** es el contrato para la forma de interacción de los modelos y las vistas.

## <a name="xaml-placeholder-conventions-in-windows-runtime-reference"></a>Convenciones de marcadores de posición XAML en la referencia de Windows en tiempo de ejecución

Si has consultado la sección **Sintaxis** de los temas de referencia sobre las API de Windows en tiempo de ejecución que usan XAML, probablemente hayas visto en la sintaxis bastantes marcadores de posición. La sintaxis XAML es diferente a las extensiones de componentes de C#, Microsoft Visual Basic o VisualC ++ (C++ / CX) sintaxis porque la sintaxis XAML es una sintaxis de uso. Indica el uso final en tus archivos XAML, pero sin ser excesivamente preceptiva sobre los valores que puedes usar. Por tanto, normalmente el uso describe un tipo de gramática que mezcla literales y marcadores de posición, y define algunos de los marcadores de posición en la sección **Valores de XAML**.

Cuando veas nombres de elementos o de tipos en la sintaxis XAML para una propiedad, el nombre que se muestra es para el tipo que define originalmente la propiedad. Sin embargo, el XAML de Windows Runtime admite un modelo de herencia de clases para las clases basadas en [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). Por tanto, a menudo puedes usar un atributo en una clase que no es literalmente la clase definitoria, sino que se deriva de una clase que definió en primer lugar la propiedad o atributo. Por ejemplo, puedes establecer [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) como atributo en cualquier clase derivada [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) mediante una herencia en profundidad. Por ejemplo: `<Button Visibility="Visible" />`. No tomes el nombre del elemento mostrado en cualquier sintaxis de uso de XAML de forma demasiado literal; puede que la sintaxis sea viable para elementos que representan esa clase y elementos que representan una clase derivada. En aquellos casos en que sea raro o imposible que el tipo mostrado como elemento definitorio se encuentre en un uso de la vida real, ese nombre de tipo se usa de forma deliberada en minúsculas en la sintaxis. Por ejemplo, la sintaxis que ves para **UIElement.Visibility** es:

``` syntax
<uiElement Visibility="Visible"/>
-or-
<uiElement Visibility="Collapsed"/>
```

Muchas secciones de la sintaxis XAML incluyen, en la sección "Uso", marcadores de posición que se definen después en una sección **Valores de XAML** inmediatamente posterior a la sección **Sintaxis**.

Las secciones de uso de XAML también utilizan diversos marcadores de posición generalizados. Estos marcadores de posición no se vuelven a definir cada vez en **Valores de XAML**, porque adivinarás o acabarás por aprender lo que representan. Creemos que la mayoría de nuestros lectores se cansarían de verlos en la sección **Valores de XAML** una y otra vez y, por eso, los hemos omitido de las definiciones. Como referencia, aquí tienes una lista de algunos de estos marcadores de posición y lo que significan en general:

-   *object*: en teoría representa cualquier valor de objeto, aunque en la práctica se suele limitar a determinados tipos de objetos, como una selección de cadena u objeto, así que te recomendamos que leas las observaciones de la página de referencia para obtener más información.
-   *object* *property*: la combinación *object* *property* se usa para aquellos casos en que la sintaxis que se muestra es la de un tipo que se puede usar como valor de atributo de muchas propiedades. Por ejemplo, la sección **Uso del atributo Xaml** que se muestra para [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) incluye: <*object* *property*= "*predefinedColorName*"/>
-   *eventhandler*: aparece como el valor de atributo de todas las sintaxis XAML mostradas de un atributo de evento. Aquí, lo que proporcionas es el nombre de función para una función de controlador de eventos. Dicha función se debe definir en el código subyacente de la página XAML. En el nivel de programación, esa función debe coincidir con la firma de delegado del evento que estés controlando; de lo contrario, el código de la aplicación no se compilará. De todos modos esto es algo a tener en cuenta para la programación, pero no para XAML, así que no estamos diciendo que se aplique al tipo delegado en la sintaxis XAML. Si quieres saber qué delegado deberías implementar para un evento, consulta la sección **Información del evento** del tema de referencia sobre el evento, en una fila de la tabla llamada **Delegado**.
-   *enumMemberName*: se muestra en la sintaxis de atributo de todas las enumeraciones. Existe un marcador de posición similar para las propiedades que usan un valor de enumeración, aunque suele prefijar el marcador de posición con una indicación del nombre de la enumeración. Por ejemplo, la sintaxis que se muestra para [**FrameworkElement.FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) es <*frameworkElement***FlowDirection**="* flowDirectionMemberName*"/>. Si te encuentras en una de esas páginas de referencia sobre propiedades, haz clic en el vínculo del tipo de enumeración que aparece en la sección **Valor de propiedad** , junto al texto **Tipo:**. Como valor de atributo de una propiedad que use esa enumeración, puedes usar cualquier cadena que aparezca en la columna **Miembro** de la tabla **Miembros** .
-   *double*, *int*, *string*, *bool*: se trata de tipos primitivos conocidos en el lenguaje XAML. Si estás programando con C# o Visual Basic, estos tipos se proyectan en tipos equivalentes de Microsoft .NET como [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx), [**Int32**](https://msdn.microsoft.com/library/windows/apps/xaml/system.int32.aspx), [**String**](https://msdn.microsoft.com/library/windows/apps/xaml/system.string.aspx) y [**Boolean**](https://msdn.microsoft.com/library/windows/apps/xaml/system.boolean.aspx), y puedes usar cualquier miembro de esos tipos .NET cuando trabajes con tus valores definidos con XAML en el código subyacente en .NET. Si programas con C++/CX, usarás los tipos primitivos de C++, pero también puedes considerarlos equivalentes a los tipos definidos por el espacio de nombres [**Platform**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710417.aspx), por ejemplo [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/xaml/hh755812.aspx). En algunos casos habrá restricciones de valores adicionales para determinadas propiedades. De todos modos, normalmente estarán indicadas en la sección **Valor de propiedad** u Observaciones, y no en la sección XAML, ya que ese tipo de restricciones se aplican tanto a los usos de código como a los de XAML.

## <a name="tips-and-tricks-notes-on-style"></a>Trucos, sugerencias y notas sobre el estilo

-   Las extensiones de marcado se suelen describir en la [Introducción a XAML](xaml-overview.md) principal. Sin embargo, la extensión de marcado que afecta en mayor medida a la orientación proporcionada en este tema es la extensión de marcado [StaticResource](staticresource-markup-extension.md) (y la extensión relacionada [ThemeResource](themeresource-markup-extension.md)). La función de la extensión de marcado StaticResource es permitir la factorización del XAML en recursos reutilizables procedentes de un [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) XAML. Casi siempre defines las plantillas de control y los estilos relacionados en un **ResourceDictionary**. A menudo también defines las partes más pequeñas de una plantilla de control o un estilo específico de la aplicación en un **ResourceDictionary**; por ejemplo, un [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/br242962) para un color que tu aplicación usa más de una vez en distintas partes de la interfaz de usuario. Mediante el uso de un StaticResource, cualquier propiedad que de otro modo requeriría el uso de un elemento de propiedad para establecerse, ahora puede establecerse mediante sintaxis de atributo. Pero las ventajas de la factorización de XAML para permitir la reutilización no se limitan a simplificar la sintaxis de nivel de página. Para obtener más información, consulta [Referencias a ResourceDictionary y a recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273).
-   Verás varias convenciones distintas sobre la aplicación de espacios en blanco y avances de línea en ejemplos de XAML. En concreto, hay distintas convenciones sobre la división de elementos de objeto que tienen muchos atributos distintos establecidos. Es solo una cuestión de estilo. El editor XML de Visual Studio aplica algunas reglas de estilo predeterminadas cuando editas XAML, pero puedes cambiarlas en la configuración. En un número reducido de casos, el espacio en blanco en un archivo XAML se considera importante; para obtener más información, consulta [XAML y espacio en blanco](xaml-and-whitespace.md).

## <a name="related-topics"></a>Temas relacionados

* [Introducción a XAML](xaml-overview.md)
* [Espacios de nombres XAML y asignación de espacios de nombres](xaml-namespaces-and-namespace-mapping.md)
* [Referencias a ResourceDictionary a recursos XAML](https://msdn.microsoft.com/library/windows/apps/mt187273)
 

