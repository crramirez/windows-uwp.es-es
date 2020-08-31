---
description: 'La extensión de marcado xBind es una alternativa de alto rendimiento al enlace. xBind-New para Windows 10: se ejecuta en menos tiempo y menos memoria que el enlace y admite una mejor depuración.'
title: Extensión de marcado xBind
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8f383174e0afccd9800a4fbf8e0db6c4f5ba9cf0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170789"
---
# <a name="xbind-markup-extension"></a>Extensión de marcado {x:Bind}

**Nota:**   Para obtener información general sobre el uso del enlace de datos en la aplicación con **{x:Bind}** (y para una comparación completa entre **{X:Bind}** y **{Binding}**), consulte [enlace de datos en profundidad](../data-binding/data-binding-in-depth.md).

La extensión de marcado **{x:Bind}** (nueva para Windows 10) es una alternativa a **{Binding}**. **{x:Bind}** se ejecuta en menos tiempo y menos memoria que **{Binding}** y admite una mejor depuración.

Durante el tiempo de compilación de XAML, **{x:Bind}** se convierte en un código que obtendrá un valor de la propiedad del origen de datos y lo establecerá en la propiedad especificada en el marcado. El objeto de enlace se puede configurar opcionalmente para observar los cambios en el valor de la propiedad de origen de datos y actualizarse en función de los cambios ( `Mode="OneWay"` ). También se puede configurar opcionalmente para insertar de nuevo los cambios en su propio valor en la propiedad de origen ( `Mode="TwoWay"` ).

Los objetos de enlace creados por **{x: enlace}** y **{Binding}** son prácticamente funcionalmente equivalentes. Pero **{x:Bind}** ejecuta el código con propósito especial, que genera en el momento de la compilación y **{Binding}** usa la inspección de objetos en el tiempo de ejecución con un propósito general. En consecuencia, los enlaces **{x:Bind}** (a menudo denominados enlaces compilados) tienen un rendimiento óptimo, proporcionan la validación de tiempo de compilación de las expresiones de enlace y admiten depuración al permitirte establecer puntos de interrupción en los archivos de código que se generan como la clase parcial de la página. Estos archivos pueden encontrarse en la carpeta `obj`, con nombres como (para C#) `<view name>.g.cs`.

> [!TIP]
> **{x:Bind}** tiene un modo predeterminado de **onetime**, a diferencia de **{Binding}**, que tiene un modo predeterminado de **OneWay**. Esto se eligió por motivos de rendimiento, ya que el uso de **OneWay** hace que se genere más código para establecer el enlace y controlar la detección de cambios. Puede especificar explícitamente un modo para usar el enlace OneWay o TwoWay. También puede usar [x:DefaultBindMode](x-defaultbindmode-attribute.md) para cambiar el modo predeterminado de **{x:Bind}** para un segmento específico del árbol de marcado. El modo especificado se aplica a cualquier expresión **{x:Bind}** en ese elemento y sus elementos secundarios, que no especifican explícitamente un modo como parte del enlace.

**Aplicaciones de ejemplo que muestran {x:Bind}**

-   [{x:Bind} (ejemplo)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery)

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
| _propName_ = _Value_ \[ , _propName_ = _valor_ de propName\]* | Una o más propiedades de enlace que se especifican con una sintaxis de par de nombre-valor. |
| _propName_ | El nombre de cadena de la propiedad que se establecerá en el objeto Binding. Por ejemplo, "Converter". |
| _value_ | El valor en el que se establecerá la propiedad. La sintaxis del argumento depende de la propiedad que se establece. A continuación se muestra un ejemplo de uso de un valor de _propName_ = _value_ en el que el valor es una extensión de marcado: `Converter={StaticResource myConverterClass}` . Para obtener más información, consulta la sección [Propiedades que se pueden establecer con {x: enlace}](#properties-that-you-can-set-with-xbind) que se incluye más adelante. |

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

## <a name="property-path"></a>Ruta de acceso de la propiedad

*PropertyPath* establece el elemento **Path** de una expresión **{x:Bind}**. **Path** es una ruta de acceso de propiedad que especifica el valor de la propiedad, la subpropiedad, el campo o el método al cual estas realizando el enlace (del origen). Puedes mencionar el nombre de la propiedad **Path** explícitamente: `{x:Bind Path=...}`. O puedes omitirlo: `{x:Bind ...}`.

### <a name="property-path-resolution"></a>Resolución de la ruta de acceso de propiedad

**{x:Bind}** no usa **DataContext** como un origen predeterminado; en su lugar, usa el control de usuario o de la página. Por lo tanto, buscará las propiedades, campos y métodos en el código subyacente de la página o control de usuario. Para exponer el modelo de vista a **{x:Bind}**, normalmente se prefiere agregar nuevos campos o propiedades al código subyacente para el control de usuario o la página. Los pasos de las rutas de acceso de propiedad están delimitados por puntos (.) y puedes incluir varios delimitadores para desviar subpropiedades sucesivas. Usa puntos como delimitadores independientemente del lenguaje de programación que uses para implementar el objeto al que se está enlazando.

Por ejemplo: en una página, **Text="{x:Bind Employee.FirstName}"** buscará un miembro **Employee** de la página y, a continuación, un miembro **FirstName** en el objeto devuelto por **Employee**. Si estás enlazando un control de elemento a una propiedad que contiene las personas a cargo del empleado, la ruta de acceso de la propiedad podría ser "Employee.Dependents" y la plantilla del elemento del control de elementos se ocuparía de mostrar los elementos en "Dependents".

En el caso de C++/CX, **{x:Bind}** no se puede enlazar a propiedades y campos privados en el modelo de datos o página: debes tener una propiedad pública para que se pueda enlazar. El área de superficie del enlace se debe exponer como clases o interfaces de CX para que podamos obtener los metadatos relevantes. No debe ser necesario el atributo ** \[ enlazable \] ** .

Con **x:Bind**, no necesitas usar **ElementName=xxx** como parte de la expresión de enlace. En su lugar, puede usar el nombre del elemento como la primera parte de la ruta de acceso para el enlace, ya que los elementos con nombre se convierten en campos dentro de la página o control de usuario que representa el origen de enlace raíz.

### <a name="collections"></a>Colecciones

Si el origen de datos es una colección, una ruta de acceso de propiedad puede especificar los elementos de la colección por su posición o índice. Por ejemplo, "equipos \[ 0 \] . Reproductores ", donde el literal" \[ \] "incluye el" 0 "que solicita el primer elemento de una colección con índice cero.

Para usar un indexador, el modelo debe implementar **IList&lt;T&gt;** o **IVector&lt;T&gt;** en el tipo de la propiedad que se va a indexar. (Tenga en cuenta que IReadOnlyList &lt; T &gt; y IVectorView &lt; t &gt; no admiten la sintaxis del indexador). Si el tipo de la propiedad indizada admite **INotifyCollectionChanged** o **IObservableVector** y el enlace es OneWay o TwoWay, se registrará y escuchará las notificaciones de cambio en esas interfaces. La lógica de detección de cambios se actualizará en función de todos los cambios de colección, incluso aunque no afecten al valor indizado específico. Esto es porque la lógica de escucha es común en todas las instancias de la colección.

Si el origen de datos es un diccionario o un mapa, una ruta de acceso de propiedad puede especificar los elementos de la colección según el nombre de la cadena. Por ejemplo, ** &lt; TextBlock Text = "{x:Bind Players \[ ' John Smith ' \] } &gt; "/** buscará un elemento en el Diccionario denominado "John Smith". Ten en cuenta que el nombre debe estar entrecomillado y que puedes usar comillas simples o dobles. Asimismo, puedes usar el acento circunflejo (^) para evitar las comillas de las cadenas. Normalmente, es más fácil usar comillas alternativas de las utilizadas para el atributo XAML. (Tenga en cuenta que IReadOnlyDictionary &lt; T &gt; y IMapView &lt; t &gt; no admiten la sintaxis del indexador).

Para usar un indexador de cadenas, el modelo debe implementar**IDictionary&lt;string, T&gt;** o **IMap&lt;string, T&gt;** en el tipo de la propiedad que se va a indexar. Si el tipo de propiedad indexada admite **IObservableMap** y el enlace es OneWay o TwoWay, registrará y escuchará notificaciones de cambio en esas interfaces. La lógica de detección de cambios se actualizará en función de todos los cambios de colección, incluso aunque no afecten al valor indizado específico. Esto es porque la lógica de escucha es común en todas las instancias de la colección.

### <a name="attached-properties"></a>Propiedades adjuntas

Para enlazar a [las propiedades adjuntas](./attached-properties-overview.md), debe colocar el nombre de la clase y la propiedad entre paréntesis después del punto. Por ejemplo, **Text="{x:Bind Button22.(Grid.Row)}"**. Si la propiedad no está declarada en un espacio de nombres XAML, necesitarás agregar un prefijo al espacio de nombres XML, que debes asignar a un espacio de nombres de código al principio del documento.

### <a name="casting"></a>Conversión

El establecimiento de tipos de los enlaces compilados es inflexible y resuelve el tipo de cada paso en una ruta de acceso. Si el tipo devuelto no incluye el miembro, se producirá un error durante la compilación. Puedes especificar una conversión para indicar el tipo real del objeto al enlace.

En el siguiente caso, **obj** es una propiedad del objeto de tipo, pero contiene un cuadro de texto, por lo que podemos usar **Text="{x:Bind ((TextBox)obj).Text}"** o **Text="{x:Bind obj.(TextBox.Text)}"**.

El campo **groups3** en **Text = "{x:BIND ((Data: SampleDataGroup) groups3 \[ 0 \] ). Title} "** es un diccionario de objetos, por lo que debe convertirlo en **Data: SampleDataGroup**. Recuerda que puedes usar el prefijo del espacio de nombres XML **data:** para asignar el tipo de objeto a un espacio de nombres de código que no forme parte del espacio de nombres XAML predeterminado.

_Nota: la sintaxis de conversión de estilo C# es más flexible que la sintaxis de propiedad adjunta; en un futuro esta será la sintaxis recomendada._

#### <a name="pathless-casting"></a>Conversión de rutas de acceso

El analizador de enlace nativo no proporciona una palabra clave que se represente `this` como parámetro de función, pero admite la conversión sin rutas (por ejemplo, `{x:Bind (x:String)}` ), que se puede usar como un parámetro de función. Por lo tanto, `{x:Bind MethodName((namespace:TypeOfThis))}` es una manera válida de realizar lo que es conceptualmente equivalente a `{x:Bind MethodName(this)}` .

Ejemplo:

`Text="{x:Bind local:MainPage.GenerateSongTitle((local:SongItem))}"`

```xaml
<Page
    x:Class="AppSample.MainPage"
    ...
    xmlns:local="using:AppSample">

    <Grid>
        <ListView ItemsSource="{x:Bind Songs}">
            <ListView.ItemTemplate>
                <DataTemplate x:DataType="local:SongItem">
                    <TextBlock
                        Margin="12"
                        FontSize="40"
                        Text="{x:Bind local:MainPage.GenerateSongTitle((local:SongItem))}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </Grid>
</Page>
```

```csharp
namespace AppSample
{
    public class SongItem
    {
        public string TrackName { get; private set; }
        public string ArtistName { get; private set; }

        public SongItem(string trackName, string artistName)
        {
            ArtistName = artistName;
            TrackName = trackName;
        }
    }

    public sealed partial class MainPage : Page
    {
        public List<SongItem> Songs { get; }
        public MainPage()
        {
            Songs = new List<SongItem>()
            {
                new SongItem("Track 1", "Artist 1"),
                new SongItem("Track 2", "Artist 2"),
                new SongItem("Track 3", "Artist 3")
            };

            this.InitializeComponent();
        }

        public static string GenerateSongTitle(SongItem song)
        {
            return $"{song.TrackName} - {song.ArtistName}";
        }
    }
}
```

## <a name="functions-in-binding-paths"></a>Funciones en rutas de acceso de enlace

A partir de la versión 1607 de Windows 10, **{x: Bind}** admite el uso de una función como el paso hoja de la ruta de acceso de enlace. Se trata de una característica eficaz para el enlace de los enlaces que permite varios escenarios en el marcado. Vea [enlaces de función](../data-binding/function-bindings.md) para obtener más información.

## <a name="event-binding"></a>Enlace de eventos

El enlace de eventos es una característica única del enlace compilado. Te permite especificar el controlador de un evento mediante un enlace, en lugar de tener que especificar un método en el código subyacente. Por ejemplo: **Click="{x:Bind rootFrame.GoForward}"**.

Para los eventos, el método de destino no debe estar sobrecargado y también debe:

- Coincidir con la firma del evento.
- O bien, no tener parámetros.
- O bien, tener el mismo número de parámetros de tipos que se pueden asignar de los tipos de los parámetros de evento.

En el código subyacente generado, el enlace compilado controla el evento y lo enruta al método en el modelo, evaluando de la ruta de acceso de la expresión de enlace cuando se produce el evento. Esto significa que, a diferencia de los enlaces de propiedad, no controla los cambios en el modelo.

Para obtener más información sobre la sintaxis de cadena de una ruta de acceso de propiedades, consulta [Property-path syntax](property-path-syntax.md) y ten en cuenta las diferencias que se describen aquí para **{x:Bind}**.

## <a name="properties-that-you-can-set-with-xbind"></a> Propiedades que se pueden establecer con {x: enlace}

**{x:Bind}** se explica a través de la sintaxis de marcadores de posición *bindingProperties* porque hay muchas propiedades de lectura y escritura que se pueden establecer en la extensión de marcado. Las propiedades se pueden establecer en cualquier orden con pares de valores de *propName*separados por comas = *value* . Ten en cuenta que no puedes incluir saltos de línea en la expresión de enlace. Algunas de las propiedades requieren tipos que no tienen una conversión de tipos, por lo que requieren sus propias extensiones de marcado anidadas dentro de **{x:Bind}**.

Estas propiedades funcionan de forma muy parecida a como lo hacen las propiedades de la clase [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding).

| Propiedad | Descripción |
|----------|-------------|
| **Ruta de acceso** | Consulta la sección [Ruta de acceso de propiedades](#property-path) anteriormente. |
| **Converter** | Especifica el objeto convertidor que llama el motor de enlace. El convertidor puede establecerse en XAML, pero solo si se hace referencia a una instancia de objeto que hayas asignado en una referencia de [extensión de marcado {StaticResource}](staticresource-markup-extension.md) a ese objeto en el diccionario de recursos. |
| **ConverterLanguage** | Especifica la referencia cultural que usará el convertidor. (Si estableces **ConverterLanguage**, también deberías establecer **Converter**). La cultura se establece como un identificador basado en estándares. Para obtener más información, consulte [**ConverterLanguage**](/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| **ConverterParameter** | Especifica el parámetro del convertidor que se puede usar en la lógica del convertidor. (Si estableces **ConverterParameter**, también deberías establecer **Converter**). La mayoría de convertidores usan una lógica simple que obtiene toda la información que necesitan del valor pasado para convertir, y no necesitan un valor **ConverterParameter**. El parámetro **ConverterParameter** es para las implementaciones de convertidor moderadamente avanzado con más de una lógica que controla lo que se pasa en **ConverterParameter**. Puede escribir un convertidor que use valores distintos de las cadenas pero esto no es habitual; vea la sección Comentarios en [**ConverterParameter**](/uwp/api/windows.ui.xaml.data.binding.converterparameter) para obtener más información. |
| **FallbackValue** | Especifica un valor que se mostrará cuando no se pueda resolver el origen o la ruta de acceso. |
| **Modo** | Especifica el modo de enlace como una de las siguientes cadenas: "OneTime", "OneWay" o "TwoWay". El valor predeterminado es "OneTime". Ten en cuenta que esto difiere del valor predeterminado de **{Binding}**, que es "OneWay" en la mayoría de los casos. |
| **TargetNullValue** | Especifica el valor que se mostrará cuando se resuelva el valor de origen, pero es explícitamente **null**. |
| **BindBack** | Especifica una función que se usará en la dirección inversa de un enlace bidireccional. |
| **UpdateSourceTrigger** | Especifica cuándo se deben volver a enviar los cambios del control al modelo en los enlaces de TwoWay. El valor predeterminado para todas las propiedades excepto TextBox. Text es PropertyChanged; TextBox. Text es LostFocus.|

> [!NOTE]
> Si está convirtiendo el marcado de **{Binding}** a **{x:Bind}**, tenga en cuenta las diferencias en los valores predeterminados de la propiedad **mode** .
> [**x:DefaultBindMode**](./x-defaultbindmode-attribute.md) puede usarse para cambiar el modo predeterminado para x: Bind para un segmento específico del árbol de marcado. El modo seleccionado aplicará las expresiones x:Bind en ese elemento y sus elementos secundarios, que no especifican explícitamente un modo como parte del enlace. OneTime tiene un mayor rendimiento que OneWay, ya que el uso de OneWay hará que se genere más código para establecer el enlace y controlar la detección de cambios.

## <a name="remarks"></a>Observaciones

Dado que **{x:Bind}** usa código generado para lograr sus ventajas, necesita la información de tipo en el momento de compilación. Esto significa que no puedes enlazar a propiedades de las que no conoces el tipo antes de tiempo. Por este motivo, no puedes usar **{x:Bind}** con la propiedad **DataContext**, que es de tipo **Object** y también está sujeta a cambios en el tiempo de ejecución.

Al usar **{x:Bind}** con plantillas de datos, debe indicar el tipo al que se está enlazando estableciendo un valor de **x:DataType** , como se muestra en la sección [ejemplos](#examples) . También puedes establecer el tipo de una interfaz o de una clase base y luego usar conversiones si es necesario para formular una expresión completa.

Los enlaces compilados dependen de la generación de código. Por tanto, si usas **{x:Bind}** en un diccionario de recursos, entonces el diccionario de recursos debe tener una clase de código subyacente. Consulta [Diccionarios de recursos con {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) para ver un ejemplo de código.

Las páginas y los controles de usuario que incluyen enlaces de tipo Compiled, tendrán una propiedad "Bindings" en el código generado. Esto incluye los siguientes métodos:

- **Update()**: este método actualizará los valores de todos los enlaces compilados. Cualquier enlace unidireccional o bidireccional tendrá agentes de escucha conectados para detectar cualquier cambio.
- **Initialize()**: si los enlaces no se han inicializado, se llamará al método Update() para inicializar esos enlaces.
- **StopTracking()**: este método desconectará todos los agentes de escucha creados para los enlaces tanto unidireccionales como bidireccionales. Recuerda que puedes volver a inicializarlos mediante el método Update().

> [!NOTE]
> A partir de la versión 1607 de Windows 10, el marco XAML proporciona un valor booleano integrado para el convertidor Visibility. El convertidor asigna **true** al valor de enumeración **visible** y **false** a **collapsed** , de modo que puede enlazar una propiedad Visibility a un valor booleano sin crear un convertidor. Tenga en cuenta que esto no es una característica del enlace de funciones, solo el enlace de propiedades. Para usar el convertidor integrado, la versión mínima del SDK de destino de la aplicación debe ser 14393 o posterior. No puedes usarlo si la aplicación está destinada a versiones anteriores de Windows 10. Para obtener más información sobre las versiones de destino, consulta [Version adaptive code (Código adaptativo para versiones)](../debug-test-perf/version-adaptive-code.md).

**Sugerencia**   Si tiene que especificar una sola llave para un valor, como en [**ruta de acceso**](/uwp/api/windows.ui.xaml.data.binding.path) o [**ConverterParameter**](/uwp/api/windows.ui.xaml.data.binding.converterparameter), debe anteponer una barra diagonal inversa: `\{` . Como alternativa, escribe la cadena completa que contiene las llaves que necesitan escape entre un conjunto de comillas secundario, por ejemplo, `ConverterParameter='{Mix}'`.

[**Converter**](/uwp/api/windows.ui.xaml.data.binding.converter), [**ConverterLanguage**](/uwp/api/windows.ui.xaml.data.binding.converterlanguage) y **ConverterLanguage** tienen relación con la conversión de un valor o tipo del origen de enlace en un valor o tipo que sea compatible con la propiedad de destino de enlace. Para obtener más información y ejemplos, consulta la sección “Conversiones de datos” de [Enlaces de datos en profundidad](../data-binding/data-binding-in-depth.md).

**{x:Bind}** es solo una extensión de marcado; no hay forma de crear o manipular estos enlaces mediante programación. Para obtener más información acerca de las extensiones de marcado, consulta [Introducción a XAML](xaml-overview.md).