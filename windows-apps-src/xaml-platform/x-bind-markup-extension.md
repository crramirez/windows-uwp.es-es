---
author: jwmsft
description: "La extensión de marcado xBind es una alternativa a Binding. xBind carece de algunas de las características de Binding, pero se ejecuta en menos tiempo y usa menos memoria que Binding, además de admitir una mejor depuración."
title: "Extensión de marcado xBind"
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: ceb5562ae08d7cc966f80fdb7e23f12afe040430

---

# Extensión de marcado {x:Bind}

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Nota** Para obtener información general sobre el uso de enlace de datos en la aplicación con **{x:Bind}** (y para realizar una comparación total entre **{x:Bind}** y **{Binding}**), consulta el tema [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

La extensión de marcado **{x:Bind}** (nueva para Windows 10) es una alternativa a **{Binding}**. **{x:Bind}** carece de algunas de las características de **{Binding}**, pero se ejecuta en menos tiempo y usa menos memoria que **{Binding}**, además de admitir una mejor depuración.

En el tiempo de carga de XAML, **{x:Bind}** se convierte en lo que puedes interpretar como un objeto de enlace, que obtiene un valor de una propiedad en un origen de datos. Opcionalmente, el objeto de enlace puede configurarse para observar cambios en el valor de la propiedad de origen de datos y se actualiza en función de los cambios. Opcionalmente, también puede configurarse para insertar los cambios en su propio valor de nuevo en la propiedad de origen. Los objetos de enlace creados por **{x: enlace}** y **{Binding}** son prácticamente funcionalmente equivalentes. Pero **{x:Bind}** ejecuta el código con propósito especial, que genera en el momento de la compilación y **{Binding}** usa la inspección de objetos en el tiempo de ejecución con un propósito general. En consecuencia, los enlaces **{x:Bind}** (a menudo denominados enlaces compilados) tienen un rendimiento óptimo, proporcionan la validación de tiempo de compilación de las expresiones de enlace y admiten depuración al permitirte establecer puntos de interrupción en los archivos de código que se generan como la clase parcial de la página. Estos archivos pueden encontrarse en la carpeta `obj`, con nombres como (para C#) `<view name>.g.cs`.

**Aplicaciones de ejemplo que muestran {x:Bind}**

-   [Ejemplo de {x:Bind}](http://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame)
-   [Muestra de conceptos básicos de interfaz de usuario de XAML](http://go.microsoft.com/fwlink/p/?linkid=619992)

## Uso del atributo XAML

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
```

| Término | Descripción |
|------|-------------|
| _propertyPath_ | Una cadena que especifica la ruta de acceso de la propiedad para el enlace. Para obtener más información, consulta la sección [Ruta de acceso de propiedades](#property-path) que aparece más adelante. |
| _bindingProperties_ |
| _propName_
            =
            _value_\[, _propName_=_value_\]* | Una o más propiedades de enlace que se especifican con una sintaxis de par de nombre-valor. |
| _propName_ | El nombre de cadena de la propiedad que se establecerá en el objeto Binding. Por ejemplo, "Converter". | 
| _value_ | El valor en el que se establecerá la propiedad. La sintaxis del argumento depende de la propiedad que se establece. A continuación, se muestra un ejemplo de uso de _propName_=_value_ en el que el valor es en sí mismo una extensión de marcado: `Converter={StaticResource myConverterClass}`. Para obtener más información, consulta la sección [Propiedades que se pueden establecer con {x: enlace}](#properties-you-can-set) que se incluye más adelante. | 

## Ruta de acceso de propiedades

*PropertyPath* establece **Path** para una expresión **{x:Bind}**. **Path** es una ruta de acceso de propiedad que especifica el valor de la propiedad, una subpropiedad, un campo o un método al que estás enlazando (el origen). Puedes mencionar el nombre de la propiedad **Path** explícitamente: `{Binding Path=...}`. O puedes omitirlo: `{Binding ...}`.

**{x:Bind}** no usa **DataContext** como un origen predeterminado; en su lugar, usa el control de usuario o de la página. Por lo tanto, buscará las propiedades, campos y métodos en el código subyacente de la página o control de usuario. Para exponer el modelo de vista a **{x:Bind}**, normalmente se prefiere agregar nuevos campos o propiedades al código subyacente para el control de usuario o la página. Los pasos de las rutas de acceso de propiedad están delimitados por puntos (.) y puedes incluir varios delimitadores para desviar subpropiedades sucesivas. Usa puntos como delimitadores independientemente del lenguaje de programación que uses para implementar el objeto al que se está enlazando.

Por ejemplo: en una página, **Text="{x:Bind Employee.FirstName}"** buscará un miembro **Employee** de la página y, a continuación, un miembro **FirstName** en el objeto devuelto por **Employee**. Si estás enlazando un control de elemento a una propiedad que contiene las personas a cargo del empleado, la ruta de acceso de la propiedad podría ser "Employee.Dependents" y la plantilla del elemento del control de elementos se ocuparía de mostrar los elementos en "Dependents".

En el caso de C++/CX, **{x:Bind}** no se puede enlazar a propiedades y campos privados en el modelo de datos o página: debes tener una propiedad pública para que se pueda enlazar. El área de superficie para el enlace se debe exponer como clases / interfaces de CX para que podamos obtener los metadatos relevantes. El atributo **\[Bindable\]** no debería ser necesario.

Si el origen de datos es una colección, una ruta de acceso de propiedad puede especificar los elementos de la colección por su posición o índice. Por ejemplo, "Teams\[0\].Players", donde se escribe el literal "\[\]" alrededor del "0" que solicita el primer elemento en una colección con índice cero.

Para usar un indexador, el modelo debe implementar **IList&lt;T&gt;** o **IVector&lt;T&gt;** en el tipo de la propiedad que se va a indexar. Si el tipo de la propiedad indexada admite **INotifyCollectionChanged** o **IObservableVector** y el enlace es OneWay o TwoWay, registrará y escuchará notificaciones de cambio en esas interfaces. La lógica de detección de cambios se actualizará en función de todos los cambios de colección, incluso aunque no afecten al valor indizado específico. Esto es porque la lógica de escucha es común en todas las instancias de la colección.

Para enlazar a propiedades adjuntas, debes insertar el nombre de clase y la propiedad entre paréntesis después del punto. Por ejemplo, **Text="{x:Bind Button22.(Grid.Row)}"**. Si la propiedad no está declarada en un espacio de nombres Xaml, necesitarás agregar un prefijo al espacio de nombres xml, que debes asignar a un espacio de nombres de código al principio del documento.

El establecimiento de tipos de los enlaces compilados es inflexible y resuelve el tipo de cada paso en una ruta de acceso. Si el tipo devuelto no incluye el miembro, se producirá un error durante la compilación. Puedes especificar una conversión para indicar el tipo real del objeto al enlace. En el siguiente caso, **obj** es una propiedad del objeto de tipo, pero contiene un cuadro de texto, por lo que podemos usar **Text="{x:Bind obj.(TextBox.Text)}"**.

El campo **groups3** de **Text="{x:Bind groups3\[0\].(data:SampleDataGroup.Title)}"** es un diccionario de objetos, por lo que debes convertirlo a **data:SampleDataGroup**. Ten en cuenta el uso del prefijo del espacio de nombres **data:** para asignar el tipo de objeto a un espacio de nombres que no forma parte del espacio de nombres XAML predeterminado.

Con **x:Bind**, no necesitas usar **ElementName=xxx** como parte de la expresión de enlace. Con **x:Bind**, puedes usar el nombre del elemento como la primera parte de la ruta de acceso para el enlace, ya que los elementos con nombre se convierten en campos de la página o control de usuario que representan el origen del enlace raíz.

El enlace de eventos es una nueva característica del enlace compilado. Permite especificar el controlador para un evento mediante un enlace en lugar de un método en el código subyacente. Por ejemplo: **Click="{x:Bind rootFrame.GoForward}"**.

Para los eventos, el método de destino no debe estar sobrecargado y también debe:

-   Coincidir con la firma del evento.
-   O bien, no tener parámetros.
-   O bien, tener el mismo número de parámetros de tipos que se pueden asignar de los tipos de los parámetros de evento.

En el código subyacente generado, el enlace compilado controla el evento y lo enruta al método en el modelo, evaluando de la ruta de acceso de la expresión de enlace cuando se produce el evento. Esto significa que, a diferencia de los enlaces de propiedad, no controla los cambios en el modelo.

Para obtener más información sobre la sintaxis de cadena de una ruta de acceso de propiedades, consulta [Property-path syntax](property-path-syntax.md) y ten en cuenta las diferencias que se describen aquí para **{x:Bind}**.

##  Propiedades que se pueden establecer con {x: enlace}


**{x:Bind}** se explica a través de la sintaxis de marcadores de posición *bindingProperties* porque hay muchas propiedades de lectura y escritura que se pueden establecer en la extensión de marcado. Las propiedades pueden establecerse en cualquier orden con pares separados por comas *propName*=*value*. Ten en cuenta que no puedes incluir saltos de línea en la expresión de enlace. Algunas de las propiedades requieren tipos que no tienen una conversión de tipos, por lo que requieren sus propias extensiones de marcado anidadas dentro de **{x:Bind}**.

Estas propiedades funcionan de forma muy parecida a como lo hacen las propiedades de la clase [**Binding**](https://msdn.microsoft.com/library/windows/apps/br209820).

| Propiedad | Descripción |
|----------|-------------|
| **Ruta de acceso** | Consulta la sección [Ruta de acceso de propiedades](#property-path) anterior. |
| **Convertidor** | Especifica el objeto convertidor que llama el motor de enlace. El convertidor puede establecerse en XAML, pero solo si se hace referencia a una instancia de objeto que hayas asignado en una referencia de [extensión de marcado {StaticResource}](staticresource-markup-extension.md) a ese objeto en el diccionario de recursos. |
| **ConverterLanguage** | Especifica la referencia cultural que usará el convertidor. (Si estableces **ConverterLanguage**, también deberías establecer **Converter**). La cultura se establece como un identificador basado en estándares. Para obtener más información, consulta [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880). |
| **ConverterParameter** | Especifica el parámetro del convertidor que se puede usar en la lógica del convertidor. (Si estableces **ConverterParameter**, también deberías establecer **Converter**). La mayoría de convertidores usan una lógica simple que obtiene toda la información que necesitan del valor pasado para convertir, y no necesitan un valor **ConverterParameter**. El parámetro **ConverterParameter** es para las implementaciones de convertidor moderadamente avanzado con más de una lógica que controla lo que se pasa en **ConverterParameter**. Puedes escribir un convertidor que use valores que no sean cadenas, aunque no es lo habitual. Consulta las observaciones de [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827) para obtener más información. |
| **FallbackValue** | Especifica un valor que se mostrará cuando no se puede resolver el origen o la ruta de acceso. |
| **Modo** | Especifica el modo de enlace como una de las siguientes cadenas: "OneTime", "OneWay" o "TwoWay". El valor predeterminado es "OneTime". Ten en cuenta que esto difiere del valor predeterminado de **{Binding}**, que es "OneWay" en la mayoría de los casos. |
| **TargetNullValue** | Especifica un valor que se mostrará cuando se resuelve el valor de origen pero es explícitamente **null**. | 

**Nota** Si quieres convertir el marcado de **{Binding}** a **{x:Bind}**, ten en cuenta las diferencias en los valores predeterminados de la propiedad **Mode**.
 
## Observaciones

Dado que **{x:Bind}** usa código generado para lograr sus ventajas, necesita la información de tipo en el momento de compilación. Esto significa que no puedes enlazar a propiedades de las que no conoces el tipo antes de tiempo. Por este motivo, no puedes usar **{x:Bind}** con la propiedad **DataContext**, que es de tipo **Object** y también está sujeta a cambios en el tiempo de ejecución.

Al usar **{x:Bind}** con plantillas de datos, debes indicar el tipo al que se enlazan estableciendo un valor **x:DataType**, como se muestra en el siguiente ejemplo. También puedes establecer el tipo de una interfaz o de una clase base y luego usar conversiones si es necesario para formular una expresión completa.

Los enlaces compilados dependen de la generación de código. Por tanto, si usas **{x:Bind}** en un diccionario de recursos, entonces el diccionario de recursos debe tener una clase de código subyacente. Consulta [Diccionarios de recursos con {x: enlace}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) para ver un ejemplo de código.

**Importante** Si estableces un valor local para una propiedad que anteriormente tenía una extensión de marcado **{x:Bind}** para proporcionar un valor local, el enlace se elimina por completo.

**Sugerencia** Si tienes que especificar una llave para un valor, como en [**Path**](https://msdn.microsoft.com/library/windows/apps/br209830) o [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/br209827), debe ir precedida por una barra diagonal inversa: `\{`. Como alternativa, escribe la cadena completa que contiene las llaves que necesitan escape entre un conjunto de comillas secundario, por ejemplo, `ConverterParameter='{Mix}'`.

[
              **Converter**
            ](https://msdn.microsoft.com/library/windows/apps/br209826), [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/hh701880) y **ConverterLanguage** tienen relación con la conversión de un valor o tipo del origen de enlace en un valor o tipo que sea compatible con la propiedad de destino de enlace. Para más información y ejemplos, consulta la sección “Conversiones de datos” de [Enlaces de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

**{x:Bind}** es solo una extensión de marcado; no hay forma de crear o manipular estos enlaces mediante programación. Para obtener más información acerca de las extensiones de marcado, consulta [Introducción a XAML](xaml-overview.md).

## Ejemplos

```XML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

En este XAML de ejemplo se usa **{x:Bind}** con una propiedad **ListView.ItemTemplate**. Observa la declaración de un valor **x:DataType**.

```XML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```




<!--HONumber=Jun16_HO4-->


