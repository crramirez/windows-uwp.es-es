---
description: 'La extensión de marcado xBind es una alternativa de alto rendimiento para el enlace. xBind - Novedades de Windows 10: se ejecuta en menos tiempo y menos memoria que admite mejor depuración y de enlace.'
title: Extensión de marcado xBind
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 625c48e2f0fc57a4e9fd3a98acc505e01e2eb42c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658620"
---
# <a name="xbind-markup-extension"></a>Extensión de marcado {x:Bind}

**Tenga en cuenta**  para obtener información general sobre el uso de enlace de datos en su aplicación con **{x: Bind}** (y para obtener una comparación total entre **{x: Bind}** y **{Binding}**), consulte [enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

El **{x: Bind}** extensión de marcado, nuevo para Windows 10, es una alternativa a **{Binding}**. **{x: Bind}**  se ejecuta en menos tiempo y menos memoria que **{Binding}** y admite la depuración mejor.

Durante el tiempo de compilación de XAML, **{x:Bind}** se convierte en un código que obtendrá un valor de la propiedad del origen de datos y lo establecerá en la propiedad especificada en el marcado. Opcionalmente, el objeto de enlace puede configurarse para observar cambios en el valor de la propiedad del origen de datos y se actualiza en función de los cambios (`Mode="OneWay"`). Opcionalmente, también puede configurarse para insertar los cambios en su propio valor de nuevo en la propiedad de origen (`Mode="TwoWay"`).

Los objetos de enlace creados por **{x: enlace}** y **{Binding}** son prácticamente funcionalmente equivalentes. Pero **{x:Bind}** ejecuta el código con propósito especial, que genera en el momento de la compilación y **{Binding}** usa la inspección de objetos en el tiempo de ejecución con un propósito general. En consecuencia, los enlaces **{x:Bind}** (a menudo denominados enlaces compilados) tienen un rendimiento óptimo, proporcionan la validación de tiempo de compilación de las expresiones de enlace y admiten depuración al permitirte establecer puntos de interrupción en los archivos de código que se generan como la clase parcial de la página. Estos archivos pueden encontrarse en la carpeta `obj`, con nombres como (para C#) `<view name>.g.cs`.

> [!TIP]
> **{x: Bind}** tiene un modo predeterminado de **OneTime**, a diferencia de **{Binding}**, que tiene un modo predeterminado de **OneWay**. Esto se escogió por motivos de rendimiento, ya que usar **OneWay** provocará que se genere más código para enlazar y gestionar la detección del cambio. Puedes especificar explícitamente un modo para utilizar el enlace OneWay o TwoWay. También puedes usar [x:DefaultBindMode](x-defaultbindmode-attribute.md) para cambiar el modo predeterminado de **{x:Bind}** para un segmento específico del árbol de marcado. El modo especificado ser aplica a las expresiones de **{x:Bind}** en ese elemento y sus elementos secundarios, que no especifican explícitamente un modo como parte del enlace.

**Aplicaciones de ejemplo que muestran {x: Bind}**

-   [{x:Bind} sample](https://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [Ejemplo de conceptos básicos de la interfaz de usuario de XAML](https://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
-or-
<object property="{x:Bind pathToFunction.functionName(functionParameter1, functionParameter2, ...), bindingProperties}" .../>
```

| Término | Descripción |
|------|-------------|
| _propertyPath_ | Una cadena que especifica la ruta de acceso de la propiedad para el enlace. Para obtener más información, consulta la sección [Ruta de acceso de propiedades](#property-path) que aparece más adelante. |
| _bindingProperties_ |
| _propName_=_value_\[, _propName_=_value_\]* | Una o más propiedades de enlace que se especifican con una sintaxis de par de nombre-valor. |
| _propName_ | El nombre de cadena de la propiedad que se establecerá en el objeto Binding. Por ejemplo, "Converter". |
| _value_ | El valor en el que se establecerá la propiedad. La sintaxis del argumento depende de la propiedad que se establece. A continuación, se muestra un ejemplo de uso de _propName_=_value_ en el que el valor es en sí mismo una extensión de marcado: `Converter={StaticResource myConverterClass}`. Para obtener más información, consulta la sección [Propiedades que se pueden establecer con {x: enlace}](#properties-that-you-can-set-with-xbind) que se incluye más adelante. |

## <a name="examples"></a>Ejemplos

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

En este XAML de ejemplo se usa **{x:Bind}** con una propiedad **ListView.ItemTemplate**. Observa la declaración de un valor **x:DataType**.

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>Ruta de acceso de propiedades

*PropertyPath* establece el elemento **Path** de una expresión **{x:Bind}**. **Path** es una ruta de acceso de propiedad que especifica el valor de la propiedad, la subpropiedad, el campo o el método al cual estas realizando el enlace (del origen). Puedes mencionar el nombre de la propiedad **Path** explícitamente: `{Binding Path=...}`. O puedes omitirlo: `{Binding ...}`.

### <a name="property-path-resolution"></a>Resolución de la ruta de acceso de propiedad

**{x:Bind}** no usa **DataContext** como un origen predeterminado; en su lugar, usa el control de usuario o de la página. Por lo tanto, buscará las propiedades, campos y métodos en el código subyacente de la página o control de usuario. Para exponer el modelo de vista a **{x:Bind}**, normalmente se prefiere agregar nuevos campos o propiedades al código subyacente para el control de usuario o la página. Los pasos de las rutas de acceso de propiedad están delimitados por puntos (.) y puedes incluir varios delimitadores para desviar subpropiedades sucesivas. Usa puntos como delimitadores independientemente del lenguaje de programación que uses para implementar el objeto al que se está enlazando.

Por ejemplo: en una página, **Text="{x:Bind Employee.FirstName}"** buscará un miembro **Employee** de la página y, a continuación, un miembro **FirstName** en el objeto devuelto por **Employee**. Si estás enlazando un control de elemento a una propiedad que contiene las personas a cargo del empleado, la ruta de acceso de la propiedad podría ser "Employee.Dependents" y la plantilla del elemento del control de elementos se ocuparía de mostrar los elementos en "Dependents".

En el caso de C++/CX, **{x:Bind}** no se puede enlazar a propiedades y campos privados en el modelo de datos o página: debes tener una propiedad pública para que se pueda enlazar. El área de superficie del enlace se debe exponer como clases o interfaces de CX para que podamos obtener los metadatos relevantes. El **\[enlazable\]** atributo no debería ser necesario.

Con **x:Bind**, no necesitas usar **ElementName=xxx** como parte de la expresión de enlace. En su lugar, puede usar el nombre del elemento como la primera parte de la ruta de acceso para el enlace porque los elementos con nombre se convierten en campos dentro de la página o control de usuario que representa el origen de enlace de raíz. 


### <a name="collections"></a>Colecciones

Si el origen de datos es una colección, una ruta de acceso de propiedad puede especificar los elementos de la colección por su posición o índice. Por ejemplo, "equipos\[0\]. Reproductores", donde el literal"\[\]"incluye"0"que solicita el primer elemento de una colección indizado cero.

Para usar un indexador, el modelo debe implementar **IList&lt;T&gt;** o **IVector&lt;T&gt;** en el tipo de la propiedad que se va a indexar. Si el tipo de la propiedad indexada admite **INotifyCollectionChanged** o **IObservableVector** y el enlace es OneWay o TwoWay, registrará y escuchará notificaciones de cambio en esas interfaces. La lógica de detección de cambios se actualizará en función de todos los cambios de colección, incluso aunque no afecten al valor indizado específico. Esto es porque la lógica de escucha es común en todas las instancias de la colección.

Si el origen de datos es un diccionario o un mapa, una ruta de acceso de propiedad puede especificar los elementos de la colección según el nombre de la cadena. Por ejemplo **&lt;texto de TextBlock = "{x: Bind reproductores\["John Smith"\]" /&gt;** va a buscar un elemento en el diccionario denominado "John Smith". Ten en cuenta que el nombre debe estar entrecomillado y que puedes usar comillas simples o dobles. Asimismo, puedes usar el acento circunflejo (^) para evitar las comillas de las cadenas. De todos modos, suele ser más fácil usar comillas alternativas a aquellas que se usan en el atributo XAML.

Para usar un indexador de cadenas, el modelo debe implementar**IDictionary&lt;string, T&gt;** o **IMap&lt;string, T&gt;** en el tipo de la propiedad que se va a indexar. Si el tipo de propiedad indexada admite **IObservableMap** y el enlace es OneWay o TwoWay, registrará y escuchará notificaciones de cambio en esas interfaces. La lógica de detección de cambios se actualizará en función de todos los cambios de colección, incluso aunque no afecten al valor indizado específico. Esto es porque la lógica de escucha es común en todas las instancias de la colección.

### <a name="attached-properties"></a>Propiedades adjuntas

Para enlazar las propiedades adjuntas, debes escribir el nombre de clase y la propiedad entre paréntesis después del punto. Por ejemplo, **Text="{x:Bind Button22.(Grid.Row)}"**. Si la propiedad no está declarada en un espacio de nombres XAML, necesitarás agregar un prefijo al espacio de nombres XML, que debes asignar a un espacio de nombres de código al principio del documento.

### <a name="casting"></a>Conversión

El establecimiento de tipos de los enlaces compilados es inflexible y resuelve el tipo de cada paso en una ruta de acceso. Si el tipo devuelto no incluye el miembro, se producirá un error durante la compilación. Puedes especificar una conversión para indicar el tipo real del objeto al enlace. En el siguiente caso, **obj** es una propiedad del objeto de tipo, pero contiene un cuadro de texto, por lo que podemos usar **Text="{x:Bind ((TextBox)obj).Text}"** o **Text="{x:Bind obj.(TextBox.Text)}"**.
El **groups3** campo **texto = "{x: Bind ((data:SampleDataGroup) groups3\[0\]). Título} "** es un diccionario de objetos, por lo que debe convertirlo a **datos: SampleDataGroup**. Recuerda que puedes usar el prefijo del espacio de nombres XML **data:** para asignar el tipo de objeto a un espacio de nombres de código que no forme parte del espacio de nombres XAML predeterminado.

_Nota: El C#: sintaxis de conversión de estilo es más flexible que la sintaxis de la propiedad adjunta, y es la sintaxis recomendada a partir de ahora._

## <a name="functions-in-binding-paths"></a>Funciones en rutas de acceso de enlace

A partir de la versión 1607 de Windows 10, **{x: Bind}** admite el uso de una función como el paso hoja de la ruta de acceso de enlace. Se trata de una característica muy eficaz para el enlace de datos que permite varios escenarios en el marcado. Consulte [función enlaces](../data-binding/function-bindings.md) para obtener más información.

## <a name="event-binding"></a>Enlace de eventos

El enlace de eventos es una característica única del enlace compilado. Te permite especificar el controlador de un evento mediante un enlace, en lugar de tener que especificar un método en el código subyacente. Por ejemplo: **Haga clic en = "{x: Bind rootFrame.GoForward}"**.

Para los eventos, el método de destino no debe estar sobrecargado y también debe:

- Coincidir con la firma del evento.
- O bien, no tener parámetros.
- O bien, tener el mismo número de parámetros de tipos que se pueden asignar de los tipos de los parámetros de evento.

En el código subyacente generado, el enlace compilado controla el evento y lo enruta al método en el modelo, evaluando de la ruta de acceso de la expresión de enlace cuando se produce el evento. Esto significa que, a diferencia de los enlaces de propiedad, no controla los cambios en el modelo.

Para obtener más información sobre la sintaxis de cadena de una ruta de acceso de propiedades, consulta [Property-path syntax](property-path-syntax.md) y ten en cuenta las diferencias que se describen aquí para **{x:Bind}**.

## <a name="properties-that-you-can-set-with-xbind"></a> Propiedades que se pueden establecer con {x: enlace}

**{x:Bind}** se explica a través de la sintaxis de marcadores de posición *bindingProperties* porque hay muchas propiedades de lectura y escritura que se pueden establecer en la extensión de marcado. Las propiedades pueden establecerse en cualquier orden con pares separados por comas *propName*=*value*. Ten en cuenta que no puedes incluir saltos de línea en la expresión de enlace. Algunas de las propiedades requieren tipos que no tienen una conversión de tipos, por lo que requieren sus propias extensiones de marcado anidadas dentro de **{x:Bind}**.

Estas propiedades funcionan de forma muy parecida a como lo hacen las propiedades de la clase [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820).

| Propiedad | Descripción |
|----------|-------------|
| **Path** | Consulta la sección [Ruta de acceso de propiedades](#property-path) anteriormente. |
| **Convertidor de tipos** | Especifica el objeto convertidor que llama el motor de enlace. El convertidor puede establecerse en XAML, pero solo si se hace referencia a una instancia de objeto que hayas asignado en una referencia de [extensión de marcado {StaticResource}](staticresource-markup-extension.md) a ese objeto en el diccionario de recursos. |
| **ConverterLanguage** | Especifica la referencia cultural que usará el convertidor. (Si está configurando **ConverterLanguage** también deben establecer **convertidor**.) La referencia cultural se establece como un identificador basado en estándares. Para obtener más información, consulta [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880). |
| **ConverterParameter** | Especifica el parámetro del convertidor que se puede usar en la lógica del convertidor. (Si está configurando **ConverterParameter** también deben establecer **convertidor**.) La mayoría de los convertidores de tipos usar lógica sencilla que obtener toda la información que necesitan desde el valor pasado a convertir, y no es necesario un **ConverterParameter** valor. El parámetro **ConverterParameter** es para las implementaciones de convertidor moderadamente avanzado con más de una lógica que controla lo que se pasa en **ConverterParameter**. Puedes escribir un convertidor que use valores que no sean cadenas, aunque no es lo habitual. Consulta las observaciones de [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) para obtener más información. |
| **FallbackValue** | Especifica un valor que se mostrará cuando no se pueda resolver el origen o la ruta de acceso. |
| **Modo** | Especifica el modo de enlace como una de estas cadenas: "Una vez", "OneWay" o "TwoWay". El valor predeterminado es "OneTime". Ten en cuenta que esto difiere del valor predeterminado de **{Binding}**, que es "OneWay" en la mayoría de los casos. |
| **TargetNullValue** | Especifica el valor que se mostrará cuando se resuelva el valor de origen, pero es explícitamente **null**. |
| **BindBack** | Especifica una función que se usará en la dirección inversa de un enlace bidireccional. |
| **UpdateSourceTrigger** | Especifica cuándo volver a insertar los cambios desde el control al modelo en los enlaces de TwoWay. El valor predeterminado para todas las propiedades excepto TextBox.Text es PropertyChanged; TextBox.Text es LostFocus.|

> [!NOTE]
> Si quieres convertir el marcado de **{Binding}** a **{x:Bind}**, debes tener en cuenta las diferencias en los valores predeterminados de la propiedad **Mode**.
 
> [**x: DefaultBindMode** ](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) puede usarse para cambiar el modo predeterminado para x: Bind para un segmento específico del árbol de marcado. El modo seleccionado aplicará las expresiones de x:Bind en ese elemento y sus elementos secundarios, que no especifican explícitamente un modo como parte del enlace. OneTime tiene un mejor rendimiento que OneWay ya que utilizar OneWay hará que se genere más código para el enlace y gestión de la detección de cambios.

## <a name="remarks"></a>Observaciones

Dado que **{x:Bind}** usa código generado para lograr sus ventajas, necesita la información de tipo en el momento de compilación. Esto significa que no puedes enlazar a propiedades de las que no conoces el tipo antes de tiempo. Por este motivo, no puedes usar **{x:Bind}** con la propiedad **DataContext**, que es de tipo **Object** y también está sujeta a cambios en el tiempo de ejecución.

Cuando se usa **{x: Bind}** con plantillas de datos, debe indicar el tipo que se está enlazando a estableciendo un **: tipo de datos x** valor, tal como se muestra en el [ejemplos](#examples) sección. También puedes establecer el tipo de una interfaz o de una clase base y luego usar conversiones si es necesario para formular una expresión completa.

Los enlaces compilados dependen de la generación de código. Por tanto, si usas **{x:Bind}** en un diccionario de recursos, entonces el diccionario de recursos debe tener una clase de código subyacente. Consulta [Diccionarios de recursos con {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) para ver un ejemplo de código.

Las páginas y los controles de usuario que incluyen enlaces de tipo Compiled, tendrán una propiedad "Bindings" en el código generado. Esto incluye los siguientes métodos:

- **Update()**: este método actualizará los valores de todos los enlaces compilados. Cualquier enlace unidireccional o bidireccional tendrá agentes de escucha conectados para detectar cualquier cambio.
- **Initialize()**: si los enlaces no se han inicializado, se llamará al método Update() para inicializar esos enlaces.
- **StopTracking()**: este método desconectará todos los agentes de escucha creados para los enlaces tanto unidireccionales como bidireccionales. Recuerda que puedes volver a inicializarlos mediante el método Update().

> [!NOTE]
> A partir de la versión 1607 de Windows 10, el marco XAML proporciona un valor booleano integrado para el convertidor Visibility. El convertidor asigna **true** al valor de la enumeración **Visible** y **false** a **Collapsed**, para que puedas enlazar una propiedad Visibility a un valor booleano sin necesidad de crear un convertidor. Ten en cuenta que esta no es una característica de enlace de función, solo enlace de propiedad. Para usar el convertidor integrado, la versión mínima del SDK de destino de la aplicación debe ser 14393 o posterior. No puedes usarlo si la aplicación está destinada a versiones anteriores de Windows 10. Para obtener más información sobre las versiones de destino, consulta [Version adaptive code (Código adaptativo para versiones)](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

**Sugerencia**    si tiene que especificar una única llave para un valor, como en [ **ruta** ](https://msdn.microsoft.com/library/windows/apps/br209830) o [ **ConverterParameter** ](https://msdn.microsoft.com/library/windows/apps/br209827), precedidos por una barra diagonal inversa: `\{`. Como alternativa, escribe la cadena completa que contiene las llaves que necesitan escape entre un conjunto de comillas secundario, por ejemplo, `ConverterParameter='{Mix}'`.

[**Convertidor**](https://msdn.microsoft.com/library/windows/apps/br209826), [ **ConverterLanguage** ](https://msdn.microsoft.com/library/windows/apps/hh701880) y **ConverterLanguage** están relacionadas con el escenario de convertir un valor o escriba desde el origen de enlace en un tipo o un valor que sea compatible con la propiedad de destino de enlace. Para obtener más información y ejemplos, consulta la sección “Conversiones de datos” de [Enlaces de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

**{x:Bind}** es solo una extensión de marcado; no hay forma de crear o manipular estos enlaces mediante programación. Para obtener más información acerca de las extensiones de marcado, consulta [Introducción a XAML](xaml-overview.md).

