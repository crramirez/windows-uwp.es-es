---
description: 'La extensión de marcado xBind es una alternativa de alto rendimiento al enlace. xBind-New para Windows 10: se ejecuta en menos tiempo y menos memoria que el enlace y admite una mejor depuración.'
title: extensión de marcado xBind
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a25797f50ee76542b8f9543cb76453d2916368ac
ms.sourcegitcommit: 82d202478ab4d3011c5ddd2e852958c34336830d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2019
ms.locfileid: "72715859"
---
# <a name="xbind-markup-extension"></a>{x:Bind} (extensión de marcado)

**Nota**  para obtener información general sobre el uso del enlace de datos en la aplicación con **{x:Bind}** (y para obtener una comparación completa entre **{x:Bind}** y **{Binding}** ), consulte [enlace de datos en profundidad](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

La extensión de marcado **{x:Bind}** , nueva para Windows 10, es una alternativa a **{Binding}** . **{x:Bind}** se ejecuta en menos tiempo y menos memoria que **{Binding}** y admite una mejor depuración.

En tiempo de compilación de XAML, **{x:Bind}** se convierte en código que obtendrá un valor de una propiedad en un origen de datos y lo establecerá en la propiedad especificada en el marcado. El objeto de enlace se puede configurar opcionalmente para observar los cambios en el valor de la propiedad de origen de datos y actualizarse en función de los cambios (`Mode="OneWay"`). También se puede configurar opcionalmente para insertar de nuevo los cambios en su propio valor en la propiedad de origen (`Mode="TwoWay"`).

Los objetos de enlace creados por **{x:Bind}** y **{Binding}** son en gran medida equivalentes funcionalmente. Pero **{x:Bind}** ejecuta código de propósito especial, que se genera en tiempo de compilación y **{Binding}** usa la inspección de objetos en tiempo de ejecución de uso general. Por consiguiente, los enlaces **{x:Bind}** (a menudo conocidos como enlaces compilados) tienen un gran rendimiento, proporcionan validación en tiempo de compilación de las expresiones de enlace y admiten la depuración, ya que permiten establecer puntos de interrupción en los archivos de código que son se genera como la clase parcial de la página. Estos archivos se pueden encontrar en la carpeta `obj`, con nombres como (para C#)`<view name>.g.cs`.

> [!TIP]
> **{x:Bind}** tiene un modo predeterminado de **onetime**, a diferencia de **{Binding}** , que tiene un modo predeterminado de **OneWay**. Esto se eligió por motivos de rendimiento, ya que el uso de **OneWay** hace que se genere más código para establecer el enlace y controlar la detección de cambios. Puede especificar explícitamente un modo para usar el enlace OneWay o TwoWay. También puede usar [x:DefaultBindMode](x-defaultbindmode-attribute.md) para cambiar el modo predeterminado de **{x:Bind}** para un segmento específico del árbol de marcado. El modo especificado se aplica a cualquier expresión **{x:Bind}** en ese elemento y sus elementos secundarios, que no especifican explícitamente un modo como parte del enlace.

**Aplicaciones de ejemplo que muestran {x:Bind}**

-   [{x:Bind} (ejemplo)](https://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [Ejemplo de conceptos básicos de la interfaz de usuario XAML](https://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>Uso de atributos XAML

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

| Término | Description |
|------|-------------|
| _propertyPath_ | Cadena que especifica la ruta de acceso de la propiedad para el enlace. Encontrará más información en la sección [ruta de acceso](#property-path) de la propiedad. |
| _bindingProperties_ |
| _propname_=_valor_\[, _propName_=_valor_\]* | Una o más propiedades de enlace que se especifican mediante una sintaxis de par de nombre y valor. |
| _propName_ | Nombre de cadena de la propiedad que se va a establecer en el objeto de enlace. Por ejemplo, "Converter". |
| _value_ | Valor en el que se va a establecer la propiedad. La sintaxis del argumento depende de la propiedad que se establece. A continuación se muestra un ejemplo de un uso de un _valor_ de _propName_=en el que el valor es una extensión de marcado: `Converter={StaticResource myConverterClass}`. Para obtener más información, vea [propiedades que puede establecer con la sección {x:Bind}](#properties-that-you-can-set-with-xbind) a continuación. |

## <a name="examples"></a>Ejemplos

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

En este ejemplo XAML se usa **{x:Bind}** con una propiedad **ListView. ItemTemplate** . Observe la declaración de un valor de **x:DataType** .

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>Ruta de acceso de propiedad

*PropertyPath* establece la **ruta de acceso** de una expresión **{x:Bind}** . **Path** es una ruta de acceso de propiedad que especifica el valor de la propiedad, subpropiedad, campo o método al que se está enlazando (el origen). Puede mencionar explícitamente el nombre de la propiedad de **ruta de acceso** : `{x:Bind Path=...}`. También puede omitirla: `{x:Bind ...}`.

### <a name="property-path-resolution"></a>Resolución de la ruta de acceso de propiedad

**{x:Bind}** no utiliza **DataContext** como origen predeterminado; en su lugar, utiliza la página o el propio control de usuario. Por lo tanto, buscará en el código subyacente de la página o en el control de usuario las propiedades, los campos y los métodos. Para exponer el modelo de vista a **{x:Bind}** , normalmente querrá agregar nuevos campos o propiedades al código subyacente para la página o el control de usuario. Los pasos de una ruta de acceso de propiedad están delimitados por puntos (.) y puede incluir varios delimitadores para atravesar subpropiedades sucesivas. Use el delimitador de puntos, independientemente del lenguaje de programación utilizado para implementar el objeto al que se enlaza.

Por ejemplo: en una página, **Text = "{X:Bind Employee. firstname}"** buscará un miembro de **empleado** en la página y, a continuación, un miembro **FirstName** en el objeto devuelto por **Employee**. Si enlaza un control de elementos a una propiedad que contiene los dependientes de un empleado, la ruta de acceso de la propiedad podría ser "Employee. Dependents" y la plantilla de elemento del control de elementos se encargaría de mostrar los elementos en "Dependents".

En C++el caso de/CX, **{x:Bind}** no se puede enlazar a campos y propiedades privados en la página o el modelo de datos: debe tener una propiedad pública para que se pueda enlazar. El área expuesta para el enlace debe exponerse como clases o interfaces de CX para que podamos obtener los metadatos pertinentes. No es necesario el **\[atributo\]enlazable** .

Con **x:Bind**, no es necesario usar **ElementName = XXX** como parte de la expresión de enlace. En su lugar, puede usar el nombre del elemento como la primera parte de la ruta de acceso para el enlace, ya que los elementos con nombre se convierten en campos dentro de la página o control de usuario que representa el origen de enlace raíz. 


### <a name="collections"></a>Colecciones

Si el origen de datos es una colección, una ruta de acceso de propiedad puede especificar los elementos de la colección por su posición o índice. Por ejemplo, "Teams\[0\]. Reproductores ", donde el literal"\[\]"incluye el" 0 "que solicita el primer elemento de una colección con índice cero.

Para usar un indizador, el modelo debe implementar **IList&lt;t&gt;** o **IVector&lt;t&gt;** en el tipo de la propiedad que se va a indizar. (Tenga en cuenta que IReadOnlyList&lt;T&gt; y IVectorView&lt;T&gt; no admiten la sintaxis del indexador). Si el tipo de la propiedad indizada admite **INotifyCollectionChanged** o **IObservableVector** y el enlace es OneWay o TwoWay, se registrará y escuchará las notificaciones de cambio en esas interfaces. La lógica de detección de cambios se actualizará en función de todos los cambios de recopilación, incluso si esto no afecta al valor indizado específico. Esto se debe a que la lógica de escucha es común en todas las instancias de la colección.

Si el origen de datos es un diccionario o un mapa, una ruta de acceso de propiedad puede especificar los elementos de la colección por su nombre de cadena. Por ejemplo **&lt;TextBlock Text = "{x:Bind Reproductores\[' John Smith '\]'/&gt;** buscará un elemento en el Diccionario denominado" John Smith ". El nombre debe incluirse entre comillas y se pueden usar comillas simples o dobles. Hat (^) se puede utilizar para usar comillas en las cadenas. Suele ser más fácil usar comillas alternativas de las utilizadas para el atributo XAML. (Tenga en cuenta que IReadOnlyDictionary&lt;T&gt; y IMapView&lt;T&gt; no admiten la sintaxis del indexador).

Para usar un indizador de cadena, el modelo debe implementar **IDictionary&lt;cadena, t&gt;** o **IMap&lt;cadena, t&gt;** en el tipo de la propiedad que se va a indizar. Si el tipo de la propiedad indizada es compatible con **IObservableMap** y el enlace es OneWay o TwoWay, se registrará y escuchará las notificaciones de cambio en esas interfaces. La lógica de detección de cambios se actualizará en función de todos los cambios de recopilación, incluso si esto no afecta al valor indizado específico. Esto se debe a que la lógica de escucha es común en todas las instancias de la colección.

### <a name="attached-properties"></a>Propiedades adjuntas

Para enlazar a [las propiedades adjuntas](./attached-properties-overview.md), debe colocar el nombre de la clase y la propiedad entre paréntesis después del punto. Por ejemplo **, Text = "{X:Bind Button22. ( Grid. Row)} "** . Si la propiedad no está declarada en un espacio de nombres XAML, tendrá que prefijarla con un espacio de nombres XML, que debe asignar a un espacio de nombres de código en el encabezado del documento.

### <a name="casting"></a>Conversión

Los enlaces compilados están fuertemente tipados y resolverán el tipo de cada paso en una ruta de acceso. Si el tipo devuelto no tiene el miembro, se producirá un error en tiempo de compilación. Puede especificar una conversión para indicar al enlace el tipo real del objeto. En el siguiente caso, **obj** es una propiedad de tipo Object, pero contiene un cuadro de texto, por lo que podemos usar **Text = "{X:BIND ((TextBox) obj). Text} "** o **Text =" {x:Bind obj. (TextBox. Text)} "** .
El campo **groups3** de **Text = "{x:BIND ((Data: SampleDataGroup) groups3\[0\]). Title} "** es un diccionario de objetos, por lo que debe convertirlo en **Data: SampleDataGroup**. Tenga en cuenta el uso del prefijo de espacio de nombres XML **Data:** para asignar el tipo de objeto a un espacio de nombres de código que no forma parte del espacio de nombres XAML predeterminado.

_Nota: la C#sintaxis de conversión de estilo es más flexible que la sintaxis de la propiedad adjunta y es la sintaxis recomendada en el futuro._

## <a name="functions-in-binding-paths"></a>Funciones en rutas de acceso de enlace

A partir de Windows 10, versión 1607, **{x:Bind}** admite el uso de una función como el paso hoja de la ruta de acceso de enlace. Se trata de una característica eficaz para el enlace de los enlaces que permite varios escenarios en el marcado. Vea [enlaces de función](../data-binding/function-bindings.md) para obtener más información.

## <a name="event-binding"></a>Enlace de eventos

El enlace de eventos es una característica única para el enlace compilado. Permite especificar el controlador de un evento mediante un enlace, en lugar de tener que ser un método en el código subyacente. Por ejemplo: **clic = "{X:Bind rootFrame. GoForward}"** .

En el caso de los eventos, el método de destino no debe sobrecargarse y también debe:

- Coincide con la firma del evento.
- O no tienen parámetros.
- O tienen el mismo número de parámetros de tipos que se pueden asignar desde los tipos de los parámetros de evento.

En el código subyacente generado, el enlace compilado controla el evento y lo enruta al método en el modelo, evaluando la ruta de acceso de la expresión de enlace cuando se produce el evento. Esto significa que, a diferencia de los enlaces de propiedad, no realiza el seguimiento de los cambios en el modelo.

Para obtener más información sobre la sintaxis de la cadena para una ruta de acceso de propiedad, vea [Sintaxis de ruta de acceso de propiedad](property-path-syntax.md), teniendo en cuenta las diferencias que se describen aquí para **{x:Bind}** .

## <a name="properties-that-you-can-set-with-xbind"></a>Propiedades que se pueden establecer con {x:Bind}

**{x:Bind}** se ilustra con la sintaxis del marcador de posición *bindingProperties* porque hay varias propiedades de lectura y escritura que se pueden establecer en la extensión de marcado. Las propiedades se pueden establecer en cualquier orden con pares de *valores*=*propName* separados por comas. Tenga en cuenta que no puede incluir saltos de línea en la expresión de enlace. Algunas de las propiedades requieren tipos que no tienen una conversión de tipos, por lo que requieren extensiones de marcado propias anidadas dentro de **{x:Bind}** .

Estas propiedades funcionan de forma muy similar a las propiedades de la clase de [**enlace**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) .

| Propiedad | Description |
|----------|-------------|
| **Path** | Vea la sección sobre la [ruta de acceso](#property-path) de la propiedad anterior. |
| **Convertidor** | Especifica el objeto de convertidor al que llama el motor de enlace. El convertidor se puede establecer en XAML, pero solo si hace referencia a una instancia de objeto que ha asignado en una referencia de [extensión de marcado {StaticResource}](staticresource-markup-extension.md) a ese objeto en el Diccionario de recursos. |
| **ConverterLanguage** | Especifica la referencia cultural que va a usar el convertidor. (Si está estableciendo **ConverterLanguage** , también debe establecer el **convertidor**). La referencia cultural se establece como un identificador basado en estándares. Para obtener más información, consulte [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| **ConverterParameter** | Especifica el parámetro de convertidor que se puede usar en la lógica del convertidor. (Si está estableciendo **ConverterParameter** , también debe establecer el **convertidor**). La mayoría de los convertidores usan una lógica simple que obtiene toda la información que necesitan del valor que se pasa a convertir y no necesita un valor de **ConverterParameter** . El parámetro **ConverterParameter** es para implementaciones de convertidor de moderadamente avanzadas que tienen más de una lógica que supera las claves que se pasan en **ConverterParameter**. Puede escribir un convertidor que use valores distintos de las cadenas pero esto no es habitual; vea la sección Comentarios en [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) para obtener más información. |
| **FallbackValue** | Especifica un valor que se va a mostrar cuando no se pueda resolver el origen o la ruta de acceso. |
| **Modo** | Especifica el modo de enlace, como una de estas cadenas: "OneTime", "OneWay" o "TwoWay". El valor predeterminado es "OneTime". Tenga en cuenta que esto difiere del valor predeterminado de **{Binding}** , que es "OneWay" en la mayoría de los casos. |
| **TargetNullValue** | Especifica un valor que se va a mostrar cuando el valor de origen resuelva pero sea explícitamente **null**. |
| **BindBack** | Especifica una función que se va a utilizar para la dirección inversa de un enlace bidireccional. |
| **UpdateSourceTrigger** | Especifica cuándo se deben volver a enviar los cambios del control al modelo en los enlaces de TwoWay. El valor predeterminado para todas las propiedades excepto TextBox. Text es PropertyChanged; TextBox. Text es LostFocus.|

> [!NOTE]
> Si está convirtiendo el marcado de **{Binding}** a **{x:Bind}** , tenga en cuenta las diferencias en los valores predeterminados de la propiedad **mode** .
> [**x:DefaultBindMode**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) se puede usar para cambiar el modo predeterminado para x:BIND para un segmento específico del árbol de marcado. El modo seleccionado aplicará las expresiones x:Bind en ese elemento y sus elementos secundarios, que no especifican explícitamente un modo como parte del enlace. OneTime tiene un mayor rendimiento que OneWay, ya que el uso de OneWay hará que se genere más código para establecer el enlace y controlar la detección de cambios.

## <a name="remarks"></a>Comentarios

Dado que **{x:Bind}** usa código generado para lograr sus ventajas, requiere información de tipo en tiempo de compilación. Esto significa que no se puede enlazar a propiedades en las que no se conoce el tipo de antemano. Por este motivo, no puede usar **{x:Bind}** con la propiedad **DataContext** , que es de tipo **Object**, y también está sujeta a cambios en tiempo de ejecución.

Al usar **{x:Bind}** con plantillas de datos, debe indicar el tipo al que se está enlazando estableciendo un valor de **x:DataType** , como se muestra en la sección [ejemplos](#examples) . También puede establecer el tipo en una interfaz o un tipo de clase base y, a continuación, utilizar las conversiones si es necesario para formular una expresión completa.

Los enlaces compilados dependen de la generación de código. Por lo tanto, si usa **{x:Bind}** en un diccionario de recursos, el Diccionario de recursos debe tener una clase de código subyacente. Vea [diccionarios de recursos con {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) para obtener un ejemplo de código.

Las páginas y los controles de usuario que incluyen enlaces compilados tendrán una propiedad "enlaces" en el código generado. Esto incluye los siguientes métodos:

- **Update ()** : se actualizarán los valores de todos los enlaces compilados. Los enlaces unidireccionales o bidireccionales tendrán los agentes de escucha enlazados para detectar cambios.
- **Initialize ()** : Si aún no se han inicializado los enlaces, llamará a Update () para inicializar los enlaces
- **StopTracking ()** : se desenlazarán todos los agentes de escucha creados para los enlaces unidireccionales y bidireccionales. Se pueden volver a inicializar con el método Update ().

> [!NOTE]
> A partir de Windows 10, versión 1607, el marco XAML proporciona un valor booleano integrado para el convertidor de visibilidad. El convertidor asigna **true** al valor de enumeración **visible** y **false** a **collapsed** , de modo que puede enlazar una propiedad Visibility a un valor booleano sin crear un convertidor. Tenga en cuenta que esto no es una característica del enlace de funciones, solo el enlace de propiedades. Para usar el convertidor integrado, la versión mínima del SDK de destino de la aplicación debe ser 14393 o posterior. No se puede usar cuando la aplicación tiene como destino versiones anteriores de Windows 10. Para obtener más información sobre las versiones de destino, consulte [código adaptable de versiones](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

**Sugerencia**   si tiene que especificar una sola llave para un valor, como en [**path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) o [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), debe ir precedida de una barra diagonal inversa: `\{`. Como alternativa, incluya la cadena completa que contenga las llaves que necesiten escapar en un conjunto de Comillas secundario, por ejemplo `ConverterParameter='{Mix}'`.

[**Converter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter), [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) y **ConverterLanguage** están relacionados con el escenario de conversión de un valor o tipo del origen de enlace en un tipo o valor que es compatible con la propiedad de destino de enlace. Para obtener más información y ejemplos, vea la sección "conversiones de datos" del [enlace de datos en profundidad](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

**{x:Bind}** es solo una extensión de marcado, sin forma de crear ni manipular estos enlaces mediante programación. Para obtener más información sobre las extensiones de marcado, vea [información general sobre XAML](xaml-overview.md).

