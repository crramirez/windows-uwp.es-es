---
author: msatranjr
title: Creación de un componente simple de Windows Runtime y llamada a este desde JavaScript
description: En este tutorial se muestra cómo puedes usar .NET Framework con Visual Basic o C# para crear tus propios tipos de Windows Runtime, empaquetados en un componente de Windows Runtime, y cómo llamar al componente de tu aplicación universal de Windows creada para Windows mediante JavaScript.
ms.assetid: 1565D86C-BF89-4EF3-81FE-35367DB8D671
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 49b9fe0833151155b11b7d7b796e395bb6a2ca7f
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7571975"
---
# <a name="walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript"></a>Tutorial: Creación de un componente simple de Windows Runtime y llamarlo desde JavaScript




En este tutorial se muestra cómo puedes usar .NET Framework con Visual Basic o C# para crear tus propios tipos de Windows Runtime, empaquetados en un componente de Windows Runtime, y cómo llamar al componente desde tu aplicación universal de Windows creada para Windows mediante JavaScript.

Visual Studio facilita el proceso de agregar un componente de Windows Runtime escrito con C# o Visual Basic a tu aplicación y crear tipos de Windows Runtime que puedas llamar desde JavaScript. Internamente, los tipos de Windows Runtime pueden usar cualquier funcionalidad de .NET Framework permitida en una aplicación universal de Windows. (Para obtener más información, consulta [Crear componentes de Windows en tiempo de ejecución en C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) y [.NET para Introducción a las aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)). Externamente, los miembros de tu tipo pueden exponer solo los tipos de Windows Runtime para sus parámetros y valores devueltos. Al compilar la solución, Visual Studio crea un proyecto de componente de Windows Runtime en .NET Framework y, a continuación, ejecuta un paso de compilación que crea un archivo de metadatos (.winmd) de Windows. Este es el componente de Windows Runtime que Visual Studio incluye en tu aplicación.

> **Nota**.NET Framework asigna automáticamente algunos tipos de .NET Framework usados frecuentemente, como los tipos de datos primitivos y tipos de colección, a sus equivalentes de Windows Runtime. Estos tipos de .NET Framework pueden usarse en la interfaz pública de un componente de Windows Runtime y se mostrarán a los usuarios del componente como los tipos correspondientes de Windows Runtime. Consulta [Creación de componentes de Windows Runtime en C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

En este tutorial se explican las tareas siguientes. Una vez completada la primera sección, que configura la aplicación de Windows con JavaScript, puedes completar el resto de las secciones en cualquier orden.

## <a name="prerequisites"></a>Requisitos previos:

-   Windows10
-   Microsoft Visual Studio2015 o Microsoft Visual Studio Community2015

## <a name="creating-a-simple-windows-runtime-class"></a>Creación de una clase simple de Windows Runtime


En esta sección se crea una aplicación universal de Windows compilada para Windows con JavaScript y se agrega un proyecto de componente de Windows Runtime en C# o Visual Basic. Se muestra cómo definir un tipo administrado de Windows Runtime, crear una instancia del tipo desde JavaScript y llamar a miembros estáticos y de instancia. La aplicación de ejemplo se ha representado deliberadamente en mate para centrar la atención en el componente. No dudes en mejorarla.

1.  En Visual Studio, crea un nuevo proyecto de JavaScript: en la barra de menús, selecciona **Archivo, Nuevo, Proyecto**. En la sección **Plantillas instaladas** del cuadro de diálogo **Nuevo proyecto**, selecciona **JavaScript**y, a continuación, elige **Windows** y después **Universal**. (Si Windows no está disponible, asegúrate de que estás usando Windows 8 o versiones posteriores). Elige la plantilla **Aplicación vacía** y escribe SampleApp como nombre del proyecto.
2.  Crea el proyecto de componente: en el Explorador de soluciones, abre el menú contextual para la solución SampleApp y selecciona **Agregar**y, a continuación, elige **Nuevo proyecto** para agregar un nuevo proyecto de C# o Visual Basic a la solución. En la sección **Plantillas instaladas** del cuadro de diálogo **Agregar nuevo proyecto**, selecciona **Visual Basic** o **Visual C#** y, a continuación, elige **Windows** y después **Universal**. Selecciona la plantilla **Componente de Windows Runtime** y escribe **SampleComponent** en el nombre del proyecto.
3.  Cambia el nombre de la clase por **Example**. Ten en cuenta que, de manera predeterminada, la clase se marca como **public sealed** (**Public NotInheritable** en Visual Basic). Todas las clases de Windows Runtime que expongas desde tu componente deben estar selladas.
4.  Agrega dos miembros simples a la clase, un método **static** (método **Shared** en Visual Basic) y una propiedad de instancia:

    > [!div class="tabbedCodeSnippets"]
    > ```csharp
    > namespace SampleComponent
    > {
    >     public sealed class Example
    >     {
    >         public static string GetAnswer()
    >         {
    >             return "The answer is 42.";
    >         }
    >
    >         public int SampleProperty { get; set; }
    >     }
    > }
    > ```
    > ```vb
    > Public NotInheritable Class Example
    >     Public Shared Function GetAnswer() As String
    >         Return "The answer is 42."
    >     End Function
    >
    >     Public Property SampleProperty As Integer
    > End Class
    > ```

5.  Opcional: para habilitar IntelliSense en los miembros recién agregados, en el Explorador de soluciones, abre el menú contextual del proyecto SampleComponent y, a continuación, elige **Compilación**.
6.  En el Explorador de soluciones, en el proyecto de JavaScript, abre el menú contextual de **Referencias** y, a continuación, elige **Agregar referencia** para abrir el **Administrador de referencias**. Elige **Proyectos** y, a continuación, **Solución**. Selecciona la casilla de verificación del proyecto SampleComponent y selecciona **Aceptar** para agregar una referencia.

## <a name="call-the-component-from-javascript"></a>Llamar al componente desde JavaScript


Para usar el tipo de Windows Runtime desde JavaScript, agrega el código siguiente a la función anónima del archivo default.js (en la carpeta js del proyecto) que se proporciona en la plantilla de Visual Studio. Debería hacerse después del controlador de eventos app.oncheckpoint y antes de la llamada a app.start.

```javascript
var ex;

function basics1() {
   document.getElementById('output').innerHTML =
        SampleComponent.Example.getAnswer();

    ex = new SampleComponent.Example();

   document.getElementById('output').innerHTML += "<br/>" +
       ex.sampleProperty;

}

function basics2() {
    ex.sampleProperty += 1;
    document.getElementById('output').innerHTML += "<br/>" +
        ex.sampleProperty;
}
```

Ten en cuenta que la primera letra del nombre de cada miembro pasa de mayúsculas a minúsculas. Esta transformación es parte de la compatibilidad que proporciona JavaScript para habilitar el uso natural de Windows Runtime. Los espacios de nombres y los nombres de clases utilizan la convención de mayúsculas y minúsculas de Pascal. Los nombres de miembros utilizan la convención de mayúsculas y minúsculas Camel, excepto para los nombres de eventos, que van todos en minúsculas. Consulta [Uso de Windows Runtime en JavaScript](https://msdn.microsoft.com/library/hh710230.aspx). Las reglas de la convención de mayúsculas y minúsculas Camel pueden resultar confusas. Una serie de letras mayúsculas iniciales aparece normalmente en minúsculas, pero si hay tres letras en mayúsculas seguidas de una letra en minúscula, solo las dos primeras letras aparecen en minúsculas: por ejemplo, un miembro denominado IDStringKind aparece como idStringKind. En Visual Studio, puedes compilar el proyecto de componente de Windows Runtime y después utilizar IntelliSense en tu proyecto de JavaScript para ver las mayúsculas y minúsculas correctamente.

De forma similar, .NET Framework proporciona compatibilidad para habilitar el uso natural de Windows Runtime en código administrado. Esto se explica en secciones posteriores de este artículo y en los artículos de [Creación de componentes de Windows Runtime en C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) y [Compatibilidad de .NET Framework con aplicaciones para UWP y en tiempo de ejecución de Windows](https://msdn.microsoft.com/library/hh694558.aspx).

## <a name="create-a-simple-user-interface"></a>Crear una interfaz de usuario sencilla


En el proyecto de JavaScript, abre el archivo default.html y actualiza el cuerpo tal como se muestra en el código siguiente. Este código incluye el conjunto completo de controles de la aplicación de ejemplo y especifica los nombres de función de los eventos de clic.

> **Nota**cuando se ejecute la aplicación por primera vez, el botón Basics1 y Basics 2 son compatibles.

```html
<body>
            <div id="buttons">
            <button id="button1" >Basics 1</button>
            <button id="button2" >Basics 2</button>

            <button id="runtimeButton1">Runtime 1</button>
            <button id="runtimeButton2">Runtime 2</button>

            <button id="returnsButton1">Returns 1</button>
            <button id="returnsButton2">Returns 2</button>

            <button id="events1Button">Events 1</button>

            <button id="btnAsync">Async</button>
            <button id="btnCancel" disabled="disabled">Cancel Async</button>
            <progress id="primeProg" value="25" max="100" style="color: yellow;"></progress>
        </div>
        <div id="output">
        </div>
</body>
```

En el proyecto de JavaScript, en la carpeta css, abre default.css. Modifica la sección del cuerpo, tal como se muestra, y agrega estilos para controlar el diseño de los botones y la colocación del texto de salida.

```css
body
{
    -ms-grid-columns: 1fr;
    -ms-grid-rows: 1fr 14fr;
    display: -ms-grid;
}

#buttons {
    -ms-grid-rows: 1fr;
    -ms-grid-columns: auto;
    -ms-grid-row-align: start;
}
#output {
    -ms-grid-row: 2;
    -ms-grid-column: 1;
}
```

Ahora, agrega el código de registro del agente de escucha de eventos agregando una cláusula "then" a la llamada processAll en app.onactivated del archivo default.js. Reemplaza la línea de código existente que llama a setPromise y cámbialo por el código siguiente:

```javascript
args.setPromise(WinJS.UI.processAll().then(function () {
    var button1 = document.getElementById("button1");
    button1.addEventListener("click", basics1, false);
    var button2 = document.getElementById("button2");
    button2.addEventListener("click", basics2, false);
}));
```

Esta es una mejor manera de agregar eventos a los controles HTML que agregando un controlador de eventos clic directamente en formato HTML. Consulta [Crear una aplicación "Hello, world" (JS)](https://msdn.microsoft.com/library/windows/apps/mt280216).

## <a name="build-and-run-the-app"></a>Compilar y ejecutar la aplicación


Antes de compilar, cambia la plataforma de destino de todos los proyectos por ARM, x64 o x86, según corresponda para tu equipo.

Para compilar y ejecutar la solución, presiona la tecla F5. (Si recibes un mensaje de error en tiempo de ejecución que indica que SampleComponent no está definido, falta la referencia al proyecto de la biblioteca de clases).

Visual Studio compila primero la biblioteca de clases y, a continuación, ejecuta una tarea de MSBuild que ejecuta [Winmdexp.exe (herramienta de exportación de metadatos de Windows Runtime)](https://msdn.microsoft.com/library/hh925576.aspx) para crear el componente de Windows Runtime. El componente se incluye en un archivo .winmd que contiene el código administrado y los metadatos de Windows que describen el código. WinMdExp.exe genera mensajes de error de compilación cuando escribes código que no es válido en un componente de Windows Runtime, y los mensajes de error se muestran en el IDE de Visual Studio. Visual Studio agrega el componente al paquete de la aplicación (archivo .appx) para la aplicación universal de Windows y genera el manifiesto apropiado.

Selecciona el botón Basics 1 para asignar el valor de devolución desde el método estático GetAnswer en el área de salida, crear una instancia de la clase Example y mostrar el valor de la propiedad SampleProperty en el área de salida. El resultado se muestra aquí:

``` syntax
"The answer is 42."
0
```

Selecciona el botón Basics 2 para aumentar el valor de la propiedad SampleProperty y mostrar el nuevo valor en el área de salida. Pueden utilizarse tipos primitivos, como cadenas y números, como tipos de parámetros y tipos devueltos y se pueden pasar entre código administrado y JavaScript. Como los números en JavaScript se almacenan en formato de punto flotante de precisión doble, se convierten en tipos numéricos de .NET Framework.

> **Nota**de manera predeterminada, puedes establecer puntos de interrupción solo en el código de JavaScript. Para depurar el código de Visual Basic o C#, consulta Creación de componentes de Windows Runtime en C# y Visual Basic.

 

Para detener la depuración y cerrar la aplicación, pasa de la aplicación a Visual Studio y presiona Mayús+F5.

## <a name="using-the-windows-runtime-from-javascript-and-managed-code"></a>Uso de Windows Runtime desde JavaScript y código administrado


Windows Runtime se puede llamar desde JavaScript o código administrado. Los objetos de Windows Runtime se pueden pasar del uno al otro, y los eventos se pueden controlar desde cualquier lado. Sin embargo, las formas de utilizar tipos de Windows Runtime en ambos entornos difieren en algunos detalles, ya que JavaScript y .NET Framework admiten Windows Runtime de manera diferente. En el ejemplo siguiente se muestran estas diferencias mediante la clase [Windows.Foundation.Collections.PropertySet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.propertyset.aspx). En este ejemplo, puedes crear una instancia de la colección PropertySet en código administrado y registrar un controlador de eventos para controlar los cambios en la colección. A continuación, puedes agregar el código de JavaScript que obtiene la colección, registra su propio controlador de eventos y utiliza la colección. Por último, puedes agregar un método que realiza cambios en la colección desde el código administrado y muestra cómo JavaScript controla una excepción administrada.

> **Importante**en este ejemplo, se desencadena el evento en el subproceso de interfaz de usuario. Si desencadenas el evento desde un subproceso en segundo plano, por ejemplo en una llamada asincrónica, deberás realizar algún proceso adicional para que JavaScript controle el evento. Para obtener más información, consulta [Generar eventos de componentes de Windows Runtime](raising-events-in-windows-runtime-components.md).

 

En el proyecto SampleComponent, agrega una nueva clase **public sealed** (clase **Public NotInheritable** en Visual Basic) denominada PropertySetStats. La clase encapsula una colección PropertySet y controla su evento MapChanged. El controlador de eventos realiza un seguimiento del número de cambios de cada tipo que se producen, y el método DisplayStats genera un informe con formato HTML. Ten en cuenta la instrucción adicional **using** (instrucción **Imports** en Visual Basic); asegúrate de agregarla a las instrucciones **using** existentes en lugar de sobrescribirlas.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using Windows.Foundation.Collections;
>
> namespace SampleComponent
> {
>     public sealed class PropertySetStats
>     {
>         private PropertySet _ps;
>         public PropertySetStats()
>         {
>             _ps = new PropertySet();
>             _ps.MapChanged += this.MapChangedHandler;
>         }
>
>         public PropertySet PropertySet { get { return _ps; } }
>
>         int[] counts = { 0, 0, 0, 0 };
>         private void MapChangedHandler(IObservableMap<string, object> sender,
>             IMapChangedEventArgs<string> args)
>         {
>             counts[(int)args.CollectionChange] += 1;
>         }
>
>         public string DisplayStats()
>         {
>             StringBuilder report = new StringBuilder("<br/>Number of changes:<ul>");
>             for (int i = 0; i < counts.Length; i++)
>             {
>                 report.Append("<li>" + (CollectionChange)i + ": " + counts[i] + "</li>");
>             }
>             return report.ToString() + "</ul>";
>         }
>     }
> }
> ```
> ```vb
> Imports System.Text
>
> Public NotInheritable Class PropertySetStats
>     Private _ps As PropertySet
>     Public Sub New()
>         _ps = New PropertySet()
>         AddHandler _ps.MapChanged, AddressOf Me.MapChangedHandler
>     End Sub
>
>     Public ReadOnly Property PropertySet As PropertySet
>         Get
>             Return _ps
>         End Get
>     End Property
>
>     Dim counts() As Integer = {0, 0, 0, 0}
>     Private Sub MapChangedHandler(ByVal sender As IObservableMap(Of String, Object),
>         ByVal args As IMapChangedEventArgs(Of String))
>
>         counts(CInt(args.CollectionChange)) += 1
>     End Sub
>
>     Public Function DisplayStats() As String
>         Dim report As New StringBuilder("<br/>Number of changes:<ul>")
>         For i As Integer = 0 To counts.Length - 1
>             report.Append("<li>" & CType(i, CollectionChange).ToString() &
>                           ": " & counts(i) & "</li>")
>         Next
>         Return report.ToString() & "</ul>"
>     End Function
> End Class
> ```

El controlador de eventos sigue el patrón de eventos de .NET Framework familiar, salvo que el remitente del evento (en este caso, el objeto PropertySet) se convierte en el IObservableMap&lt;de cadena, el objeto&gt; interfaz (IObservableMap (Of String, Object) en Visual Basic), que es una instancia de la interfaz de Windows Runtime [IObservableMap&lt;K, V&gt;](https://msdn.microsoft.com/library/windows/apps/br226050.aspx). (Se puede convertir el remitente a su tipo si es necesario). Además, los argumentos del evento se presentan como una interfaz en lugar de un objeto.

En el archivo default.js, agrega la función Runtime1 tal como se muestra. Este código crea un objeto PropertySetStats, obtiene su colección PropertySet y agrega su propio controlador de eventos, la función onMapChanged, para controlar el evento MapChanged. Después de realizar cambios en la colección, runtime1 llama al método DisplayStats para mostrar un resumen de los tipos de cambio.

```javascript
var propertysetstats;

function runtime1() {
    document.getElementById('output').innerHTML = "";

    propertysetstats = new SampleComponent.PropertySetStats();
    var propertyset = propertysetstats.propertySet;

    propertyset.addEventListener("mapchanged", onMapChanged);

    propertyset.insert("FirstProperty", "First property value");
    propertyset.insert("SuperfluousProperty", "Unnecessary property value");
    propertyset.insert("AnotherProperty", "A property value");

    propertyset.insert("SuperfluousProperty", "Altered property value")
    propertyset.remove("SuperfluousProperty");

    document.getElementById('output').innerHTML +=
        propertysetstats.displayStats();
}

function onMapChanged(change) {
    var result
    switch (change.collectionChange) {
        case Windows.Foundation.Collections.CollectionChange.reset:
            result = "All properties cleared";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemInserted:
            result = "Inserted " + change.key + ": '" +
                change.target.lookup(change.key) + "'";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemRemoved:
            result = "Removed " + change.key;
            break;
        case Windows.Foundation.Collections.CollectionChange.itemChanged:
            result = "Changed " + change.key + " to '" +
                change.target.lookup(change.key) + "'";
            break;
        default:
            break;
     }

     document.getElementById('output').innerHTML +=
         "<br/>" + result;
}
```

La forma de controlar los eventos de Windows Runtime en JavaScript es muy diferente de la forma de controlarlos en código de .NET Framework. El controlador de eventos de JavaScript solo utiliza un argumento. Cuando ves este objeto en el depurador de Visual Studio, la primera propiedad es el remitente. Los miembros de la interfaz del argumento de evento también aparecen directamente en este objeto.

Para ejecutar la aplicación, presiona la tecla F5. Si no se sella la clase, recibirás el mensaje de error "Exporting unsealed type 'SampleComponent.Example' is not currently supported. Márcalo como sellado."

Selecciona el botón **Runtime 1**. El controlador de eventos muestra los cambios a medida que se agregan o cambian los elementos y al final se llama al método DisplayStats para generar un resumen de recuentos. Para detener la depuración y cerrar la aplicación, vuelve a Visual Studio y presiona Mayús+F5.

Para agregar dos elementos más a la colección PropertySet desde el código administrado, agrega el código siguiente a la clase PropertySetStats:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public void AddMore()
> {
>     _ps.Add("NewProperty", "New property value");
>     _ps.Add("AnotherProperty", "A property value");
> }
> ```
> ```vb
> Public Sub AddMore()
>     _ps.Add("NewProperty", "New property value")
>     _ps.Add("AnotherProperty", "A property value")
> End Sub
> ```

Este código resalta otra diferencia en la manera de utilizar los tipos de Windows Runtime en los dos entornos. Si escribes este código tú mismo, te darás cuenta de que IntelliSense no muestra el método "insert" que has utilizado en el código de JavaScript. En cambio, muestra el método Add, que suele verse en las colecciones de .NET Framework. Esto se debe a que algunas interfaces de colección utilizadas con frecuencia tienen diferentes nombres pero una funcionalidad similar en Windows Runtime y .NET Framework. Cuando utilizas estas interfaces en código administrado, aparecen como sus equivalentes de .NET Framework. Esto se explica en [Creación de componentes de Windows Runtime en C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md). Cuando utilizas las mismas interfaces en JavaScript, el único cambio respecto a Windows Runtime es que las letras mayúsculas al principio de los nombres de los miembros se convierten en minúsculas.

Por último, para llamar al método AddMore con control de excepciones, agrega la función runtime2 a default.js.

```javascript
function runtime2() {
   try {
      propertysetstats.addMore();
    }
   catch(ex) {
       document.getElementById('output').innerHTML +=
          "<br/><b>" + ex + "<br/>";
   }

   document.getElementById('output').innerHTML +=
       propertysetstats.displayStats();
}
```

Agrega el código de registro del controlador de eventos tal como lo has hecho anteriormente.

```javascript
var runtimeButton1 = document.getElementById("runtimeButton1");
runtimeButton1.addEventListener("click", runtime1, false);
var runtimeButton2 = document.getElementById("runtimeButton2");
runtimeButton2.addEventListener("click", runtime2, false);
```

Para ejecutar la aplicación, presiona la tecla F5. Elige **Runtime 1** y después **Runtime 2**. El controlador de eventos de JavaScript notifica el primer cambio en la colección. El segundo cambio, sin embargo, tiene una clave duplicada. Los usuarios de los diccionarios de .NET Framework esperan que el método Add produzca una excepción, y esto es lo que sucede. JavaScript controla la excepción de .NET Framework.

> **Nota**no puede mostrar el mensaje de la excepción desde el código de JavaScript. El texto del mensaje se reemplaza por un seguimiento de la pila. Para obtener más información, consulta "Producir excepciones" en Creación de componentes de Windows Runtime en C# y Visual Basic.

Por el contrario, cuando JavaScript llama al método "insert" con una clave duplicada, se cambia el valor del elemento. Esta diferencia de comportamiento se debe a las distintas formas en que JavaScript y .NET Framework admiten Windows Runtime, tal como se explica en [Creación de componentes de Windows Runtime en C# y Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

## <a name="returning-managed-types-from-your-component"></a>Devolver tipos administrados desde el componente


Como se ha explicado anteriormente, puedes pasar tipos nativos de Windows Runtime libremente entre el código de JavaScript y el código de C# o Visual Basic. La mayoría de las veces, los nombres de tipos y los nombres de miembros serán los mismos en ambos casos (salvo que los nombres de miembros empiecen con letras en minúscula en JavaScript). Sin embargo, en la sección anterior, parecía que la clase PropertySet tiene diferentes miembros en código administrado. (Por ejemplo, en JavaScript llamaste al método de inserción y en el código de .NET Framework llamaste el método Add). Esta sección explora cómo afectan estas diferencias a los tipos de .NET Framework que se pasan a JavaScript.

Además de devolver los tipos de Windows Runtime que creaste en tu componente o que pasaste a tu componente desde JavaScript, puedes devolver un tipo administrado, creado en código administrado, a JavaScript como si fuera el tipo de Windows Runtime correspondiente. Incluso en el primer y sencillo ejemplo de una clase en tiempo de ejecución, los parámetros y los tipos devueltos de los miembros eran tipos primitivos de Visual Basic o C#, que son tipos de .NET Framework. Para demostrarlo en las colecciones, agrega el código siguiente a la clase Example para crear un método que devuelva un diccionario genérico de cadenas indexado por enteros:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IDictionary<int, string> GetMapOfNames()
> {
>     Dictionary<int, string> retval = new Dictionary<int, string>();
>     retval.Add(1, "one");
>     retval.Add(2, "two");
>     retval.Add(3, "three");
>     retval.Add(42, "forty-two");
>     retval.Add(100, "one hundred");
>     return retval;
> }
> ```
> ```vb
> Public Shared Function GetMapOfNames() As IDictionary(Of Integer, String)
>     Dim retval As New Dictionary(Of Integer, String)
>     retval.Add(1, "one")
>     retval.Add(2, "two")
>     retval.Add(3, "three")
>     retval.Add(42, "forty-two")
>     retval.Add(100, "one hundred")
>     Return retval
> End Function
> ```

Ten en cuenta que el diccionario se debe devolver como una interfaz que se implementa mediante [Dictionary&lt;TKey, TValue&gt;](https://msdn.microsoft.com/library/xfhwa508.aspx) y que se asigna a una interfaz de Windows Runtime. En este caso, la interfaz es IDictionary&lt;int, string&gt; [IDictionary(Of Integer, String) en Visual Basic]. Cuando se pasa el tipo de Windows Runtime IMap&lt;int, string&gt; a código administrado, aparece como IDictionary&lt;int, string&gt;, y sucede justo lo contrario cuando el tipo administrado se pasa a JavaScript.

**Importante**cuando un tipo administrado implementa varias interfaces, JavaScript usa la interfaz que aparece en primer lugar en la lista. Por ejemplo, si devuelves Dictionary&lt;int, string&gt; al código JavaScript, aparece como IDictionary&lt;int, string&gt; independientemente de qué interfaz especifiques como tipo devuelto. Esto significa que si la primera interfaz no incluye a un miembro que aparece en las últimas interfaces, ese miembro no es visible para JavaScript.

 

Para probar el nuevo método y utilizar el diccionario, agrega las funciones returns1 y returns2 a default.js:

```javascript
var names;

function returns1() {
    names = SampleComponent.Example.getMapOfNames();
    document.getElementById('output').innerHTML = showMap(names);
}

var ct = 7;

function returns2() {
    if (!names.hasKey(17)) {
        names.insert(43, "forty-three");
        names.insert(17, "seventeen");
    }
    else {
        var err = names.insert("7", ct++);
        names.insert("forty", "forty");
    }
    document.getElementById('output').innerHTML = showMap(names);
}

function showMap(map) {
    var item = map.first();
    var retval = "<ul>";

    for (var i = 0, len = map.size; i < len; i++) {
        retval += "<li>" + item.current.key + ": " + item.current.value + "</li>";
        item.moveNext();
    }
    return retval + "</ul>";
}
```

A continuación, agrega el código de registro de eventos al mismo bloque que el otro código de registro de eventos:

```javascript
var returnsButton1 = document.getElementById("returnsButton1");
returnsButton1.addEventListener("click", returns1, false);
var returnsButton2 = document.getElementById("returnsButton2");
returnsButton2.addEventListener("click", returns2, false);
```

Se deben tener en cuenta algunos aspectos interesantes sobre este código de JavaScript. En primer lugar, incluye una función showMap para mostrar el contenido del diccionario en HTML. En el código de showMap, observa el patrón de iteración. En .NET Framework, no hay ningún método First en la interfaz IDictionary genérica, y el tamaño lo devuelve una propiedad Count en lugar de un método Size. En JavaScript, IDictionary&lt;int, string&gt; parece que es el tipo de Windows Runtime IMap&lt;int, string&gt;. (Consulta la interfaz [IMap&lt;K,V&gt;](https://msdn.microsoft.com/library/windows/apps/br226042.aspx)).

En la función returns2, al igual que en ejemplos anteriores, JavaScript llama al método Insert ("insert" en JavaScript) para agregar elementos al diccionario.

Para ejecutar la aplicación, presiona la tecla F5. Para crear y mostrar el contenido inicial del diccionario, selecciona el botón **Returns 1**. Para agregar dos entradas más al diccionario, selecciona el botón **Returns 2**. Ten en cuenta que las entradas se muestran en orden de inserción, como cabría esperar de Dictionary&lt;TKey, TValue&gt;. Si deseas ordenarlas, puedes devolver un SortedDictionary&lt;int, string&gt; desde GetMapOfNames. (La clase PropertySet que se usa en ejemplos anteriores presenta una organización interna distinta de Dictionary&lt;TKey, TValue&gt;).

Por supuesto, JavaScript no es un lenguaje fuertemente tipado, por lo que el uso de colecciones genéricas fuertemente tipadas puede provocar resultados incoherentes. Vuelve a seleccionar el botón **Returns 2**. JavaScript convierte el número "7" en un 7 numérico, y el 7 numérico que se almacena en ct, en una cadena. Y convierte la cadena "cuarenta" en cero. Pero eso es solo el principio. Selecciona el botón **Returns 2** unas cuantas veces más. En código administrado, el método Add generaría excepciones de clave duplicada, incluso aunque los valores se convirtieran a los tipos correctos. En cambio, el método Insert actualiza el valor asociado a una clave existente y devuelve un valor booleano que indica si se ha agregado una nueva clave al diccionario. Es por eso que el valor asociado a la clave de 7 va cambiando.

Otro comportamiento inesperado: si pasas una variable sin asignar de JavaScript como un argumento de cadena, lo que obtienes es la cadena "undefined". En resumen, ten cuidado cuando pases tipos de la colección de .NET Framework a código JavaScript.

> **Nota**si tienes grandes cantidades de texto para concatenar, puedes hacerlo más eficacia si mueves el código a un método de .NET Framework y mediante la clase StringBuilder, como se muestra en la función showMap.

Aunque no puedas exponer tus propios tipos genéricos desde un componente de Windows Runtime, puedes devolver colecciones genéricas de .NET Framework para las clases de Windows Runtime usando código como el siguiente:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static object GetListOfThis(object obj)
> {
>     Type target = obj.GetType();
>     return Activator.CreateInstance(typeof(List<>).MakeGenericType(target));
> }
> ```
> ```vb
> Public Shared Function GetListOfThis(obj As Object) As Object
>     Dim target As Type = obj.GetType()
>     Return Activator.CreateInstance(GetType(List(Of )).MakeGenericType(target))
> End Function
> ```

List&lt;T&gt; implementa IList&lt;T&gt;, que aparece como el tipo de Windows Runtime IVector&lt;T&gt; en JavaScript.

## <a name="declaring-events"></a>Declarar eventos


Puedes declarar eventos con el patrón de eventos estándar de .NET Framework u otros patrones usados por Windows Runtime. .NET Framework admite la equivalencia entre el delegado System.EventHandler&lt;TEventArgs&gt; y el delegado EventHandler&lt;T&gt; de Windows Runtime, por lo que usar EventHandler&lt;TEventArgs&gt; es una buena forma de implementar el patrón estándar de .NET Framework. Para ver cómo funciona esto, agrega el siguiente par de clases al proyecto SampleComponent:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> namespace SampleComponent
> {
>     public sealed class Eventful
>     {
>         public event EventHandler<TestEventArgs> Test;
>         public void OnTest(string msg, long number)
>         {
>             EventHandler<TestEventArgs> temp = Test;
>             if (temp != null)
>             {
>                 temp(this, new TestEventArgs()
>                 {
>                     Value1 = msg,
>                     Value2 = number
>                 });
>             }
>         }
>     }
>
>     public sealed class TestEventArgs
>     {
>         public string Value1 { get; set; }
>         public long Value2 { get; set; }
>     }
> }
> ```
> ```vb
> Public NotInheritable Class Eventful
>     Public Event Test As EventHandler(Of TestEventArgs)
>     Public Sub OnTest(ByVal msg As String, ByVal number As Long)
>         RaiseEvent Test(Me, New TestEventArgs() With {
>                             .Value1 = msg,
>                             .Value2 = number
>                             })
>     End Sub
> End Class
>
> Public NotInheritable Class TestEventArgs
>     Public Property Value1 As String
>     Public Property Value2 As Long
> End Class
> ```

Cuando expongas un evento de Windows Runtime, la clase de argumento de evento se hereda de System.Object. No se hereda de System.EventArgs, como haría en .NET Framework, porque EventArgs no es un tipo de Windows Runtime.

Si declaras los descriptores de acceso de eventos personalizados para el evento (palabra clave **Custom** en Visual Basic), debes usar el patrón de eventos de Windows Runtime. Consulta [Eventos y descriptores de acceso de eventos personalizados en componentes de Windows Runtime](custom-events-and-event-accessors-in-windows-runtime-components.md).

Para controlar el evento Test, agrega la función events1 a default.js. La función events1 crea una función de controlador de eventos para el evento Test e invoca inmediatamente el método OnTest para generar el evento. Si colocas un punto de interrupción en el cuerpo del controlador de eventos, podrás ver que el objeto pasado al parámetro único incluye el objeto de origen y los dos miembros de TestEventArgs.

```javascript
var ev;

function events1() {
   ev = new SampleComponent.Eventful();
   ev.addEventListener("test", function (e) {
       document.getElementById('output').innerHTML = e.value1;
       document.getElementById('output').innerHTML += "<br/>" + e.value2;
   });
   ev.onTest("Number of feet in a mile:", 5280);
}
```

A continuación, agrega el código de registro de eventos al mismo bloque que el otro código de registro de eventos:

```javascript
var events1Button = document.getElementById("events1Button");
events1Button.addEventListener("click", events1, false);
```

## <a name="exposing-asynchronous-operations"></a>Exponer operaciones asincrónicas


.NET Framework cuenta con un amplio conjunto de herramientas para el procesamiento asincrónico y el procesamiento en paralelo, basado en la clase Task y la clase [Task&lt;TResult&gt;](https://msdn.microsoft.com/library/dd321424.aspx) genérica. Para exponer el procesamiento asincrónico basado en tareas en un componente de Windows Runtime, usa las interfaces de Windows Runtime [IAsyncAction](https://msdn.microsoft.com/library/br205781.aspx), [IAsyncActionWithProgress&lt;TProgress&gt;](https://msdn.microsoft.com/library/br205784.aspx), [IAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/br205802.aspx) e [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/br205807.aspx). (En Windows Runtime, las operaciones devuelven resultados, pero no las acciones).

En esta sección se muestra una operación asincrónica cancelable que notifica el progreso y devuelve resultados. El método GetPrimesInRangeAsync usa la clase [AsyncInfo](https://msdn.microsoft.com/library/system.runtime.interopservices.windowsruntime.asyncinfo.aspx) para generar una tarea y conectar sus características de cancelación y notificación del progreso con un objeto WinJS.Promise. Comienza agregando el método GetPrimesInRangeAsync a la clase de ejemplo:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using System.Runtime.InteropServices.WindowsRuntime;
> using Windows.Foundation;
>
> public static IAsyncOperationWithProgress<IList<long>, double>
> GetPrimesInRangeAsync(long start, long count)
> {
>     if (start < 2 || count < 1) throw new ArgumentException();
>
>     return AsyncInfo.Run<IList<long>, double>((token, progress) =>
>
>         Task.Run<IList<long>>(() =>
>         {
>             List<long> primes = new List<long>();
>             double onePercent = count / 100;
>             long ctProgress = 0;
>             double nextProgress = onePercent;
>
>             for (long candidate = start; candidate < start + count; candidate++)
>             {
>                 ctProgress += 1;
>                 if (ctProgress >= nextProgress)
>                 {
>                     progress.Report(ctProgress / onePercent);
>                     nextProgress += onePercent;
>                 }
>                 bool isPrime = true;
>                 for (long i = 2, limit = (long)Math.Sqrt(candidate); i <= limit; i++)
>                 {
>                     if (candidate % i == 0)
>                     {
>                         isPrime = false;
>                         break;
>                     }
>                 }
>                 if (isPrime) primes.Add(candidate);
>
>                 token.ThrowIfCancellationRequested();
>             }
>             progress.Report(100.0);
>             return primes;
>         }, token)
>     );
> }
> ```
> ```vb
> Imports System.Runtime.InteropServices.WindowsRuntime
>
> Public Shared Function GetPrimesInRangeAsync(ByVal start As Long, ByVal count As Long)
> As IAsyncOperationWithProgress(Of IList(Of Long), Double)
>
>     If (start < 2 Or count < 1) Then Throw New ArgumentException()
>
>     Return AsyncInfo.Run(Of IList(Of Long), Double)( _
>         Function(token, prog)
>             Return Task.Run(Of IList(Of Long))( _
>                 Function()
>                     Dim primes As New List(Of Long)
>                     Dim onePercent As Long = count / 100
>                     Dim ctProgress As Long = 0
>                     Dim nextProgress As Long = onePercent
>
>                     For candidate As Long = start To start + count - 1
>                         ctProgress += 1
>
>                         If ctProgress >= nextProgress Then
>                             prog.Report(ctProgress / onePercent)
>                             nextProgress += onePercent
>                         End If
>
>                         Dim isPrime As Boolean = True
>                         For i As Long = 2 To CLng(Math.Sqrt(candidate))
>                             If (candidate Mod i) = 0 Then
>                                 isPrime = False
>                                 Exit For
>                             End If
>                         Next
>
>                         If isPrime Then primes.Add(candidate)
>
>                         token.ThrowIfCancellationRequested()
>                     Next
>                     prog.Report(100.0)
>                     Return primes
>                 End Function, token)
>         End Function)
> End Function
> ```

GetPrimesInRangeAsync es un buscador de números primos muy sencillo gracias a su diseño. Aquí lo primordial es la implementación de una operación asincrónica, por lo que la simplicidad es importante, y una implementación lenta supone una ventaja cuando se quiere mostrar la cancelación. GetPrimesInRangeAsync busca números primos mediante fuerza bruta: divide un candidato por todos los enteros inferiores o iguales a su raíz cuadrada, en lugar de utilizar solamente los números primos. Ejecución paso a paso de este código:

-   Antes de iniciar una operación asincrónica, realiza actividades de mantenimiento, como validar parámetros e iniciar excepciones para las entradas no válidas.
-   La clave de esta implementación es el método [AsyncInfo.Run&lt;TResult, TProgress&gt;(Func&lt;CancellationToken, IProgress&lt;TProgress&gt;, Task&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779740.aspx)&gt;) y el delegado que es el único parámetro del método. El delegado debe aceptar un token de cancelación y una interfaz para informar del progreso y debe devolver una tarea iniciada que emplee esos parámetros. Cuando JavaScript llama al método GetPrimesInRangeAsync, se producen los pasos siguientes (no necesariamente en el orden indicado aquí):

    -   El objeto [WinJS.Promise](https://msdn.microsoft.com/library/windows/apps/br211867.aspx) proporciona funciones para procesar los resultados devueltos, reaccionar a la cancelación y controlar informes de progreso.
    -   El método AsyncInfo.Run crea un origen de cancelación y un objeto que implementa la interfaz IProgress&lt;T&gt;. Para el delegado, pasa un token [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) desde el origen de la cancelación y la interfaz [IProgress&lt;T&gt;](https://msdn.microsoft.com/library/hh138298.aspx).

        > **Nota**si el objeto Promise no proporciona una función para reaccionar a la cancelación, AsyncInfo.Run sigue pasa un token cancelable y cancelación puede producirse igualmente. Si el objeto Promise no proporciona una función para controlar las actualizaciones del progreso, AsyncInfo.Run sigue proporcionando un objeto que implementa IProgress&lt;T&gt;, pero se omiten sus informes.

    -   El delegado usa el método [Task.Run&lt;TResult&gt;(Func&lt;TResult&gt;, CancellationToken](https://msdn.microsoft.com/library/hh160376.aspx)) para crear una tarea iniciada que use el token y la interfaz de progreso. Una función lambda proporciona el delegado de la tarea iniciada, que calcula el resultado deseado. Más información al respecto en un momento.
    -   El método AsyncInfo.Run crea un objeto que implementa la interfaz [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://msdn.microsoft.com/library/windows/apps/br206594.aspx), conecta el mecanismo de cancelación de Windows Runtime con el origen del token y conecta la función de información del progreso del objeto Promise con la interfaz IProgress&lt;T&gt;.
    -   La interfaz IAsyncOperationWithProgress&lt;TResult, TProgress&gt; se devuelve a JavaScript.

-   La función lambda que se representa mediante la tarea iniciada no acepta ningún argumento. Dado que es una función lambda, tiene acceso al token y a la interfaz IProgress. Cada vez que se evalúa un número candidato, la función lambda:

    -   Comprueba si se ha llegado al siguiente punto de porcentaje del progreso. Si lo ha hecho, la función lambda llama al método IProgress&lt;T&gt;.Report y el porcentaje se pasa a la función que el objeto Promise especificó para informar del progreso.
    -   Utiliza el token de cancelación para producir una excepción si se ha cancelado la operación. Si se ha llamado al método [IAsyncInfo.Cancel](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncinfo.cancel.aspx) (que hereda la interfaz IAsyncOperationWithProgress&lt;TResult, TProgress&gt;), la conexión que configura el método AsyncInfo.Run garantiza que se notificará el token de cancelación.
-   Cuando la función lambda devuelve la lista de números primos, la lista se pasa a la función que especificó el objeto WinJS.Promise para procesar los resultados.

Para crear la promesa de JavaScript y configurar el mecanismo de cancelación, agrega las funciones asyncRun y asyncCancel a default.js.

```javascript
var resultAsync;
function asyncRun() {
    document.getElementById('output').innerHTML = "Retrieving prime numbers.";
    btnAsync.disabled = "disabled";
    btnCancel.disabled = "";

    resultAsync = SampleComponent.Example.getPrimesInRangeAsync(10000000000001, 2500).then(
        function (primes) {
            for (i = 0; i < primes.length; i++)
                document.getElementById('output').innerHTML += " " + primes[i];

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function () {
            document.getElementById('output').innerHTML += " -- getPrimesInRangeAsync was canceled. -- ";

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function (prog) {
            document.getElementById('primeProg').value = prog;
        }
    );
}

function asyncCancel() {    
    resultAsync.cancel();
}
```

No olvides que el código de registro de eventos es el mismo que antes.

```javascript
var btnAsync = document.getElementById("btnAsync");
btnAsync.addEventListener("click", asyncRun, false);
var btnCancel = document.getElementById("btnCancel");
btnCancel.addEventListener("click", asyncCancel, false);
```

Al llamar al método asincrónico GetPrimesInRangeAsync, la función asyncRun crea un objeto WinJS.Promise. A continuación, el método "then" del objeto utiliza tres funciones que procesan los resultados devueltos, reaccionan en caso de errores (incluida la cancelación) y controlan los informes de progreso. En este ejemplo, se imprimen los resultados devueltos en el área de salida. En caso de cancelación o finalización, se restablecen los botones que inician y cancelan la operación. La información del progreso actualiza el control del progreso.

La función asyncCancel simplemente llama al método cancel del objeto WinJS.Promise.

Para ejecutar la aplicación, presiona la tecla F5. Para iniciar la operación asincrónica, selecciona el botón **Async**. Lo que suceda después dependerá de la velocidad de tu equipo. Si la barra de progreso llega al final antes de que tengas tiempo de reaccionar, multiplica el tamaño del número inicial que se pasa a GetPrimesInRangeAsync una o varias veces por diez. Para ajustar la duración de la operación, puedes probar de aumentar o reducir la cantidad de números, pero es más eficaz agregar ceros en medio del número inicial. Para cancelar la operación, selecciona el botón **Cancel Async**.

## <a name="related-topics"></a>Temas relacionados

* [.NET para Introducción a las aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [.NET para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [Tutorial: Creación de un componente simple de Windows Runtime y llamada al mismo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
