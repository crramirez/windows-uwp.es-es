---
description: Aquí describimos el concepto de programación de eventos en una aplicación de Windows Runtime cuando se usa C#, Visual Basic o extensiones de componentes de Visual C++ (C++/CX), como lenguaje de programación y XAML, para la definición de la interfaz de usuario.
title: Introducción a eventos y eventos enrutados
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 67ee1f31e1480e0aa805e161433976d5e789d00c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155099"
---
# <a name="events-and-routed-events-overview"></a>Introducción a eventos y eventos enrutados

**API importantes**
- [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)
- [**RoutedEventArgs**](/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

Aquí describimos el concepto de programación de eventos en una aplicación de Windows Runtime cuando se usa C#, Visual Basic o extensiones de componentes de Visual C++ (C++/CX), como lenguaje de programación y XAML, para la definición de la interfaz de usuario. Puedes asignar controladores para eventos como parte de las declaraciones de los elementos de la interfaz de usuario en XAML, o puedes agregar los controladores en el código. Windows Runtime admite *eventos enrutados*, lo que implica que ciertos eventos de entrada y eventos de datos puedan ser controlados por otros objetos distintos del objeto que originó el evento. Los eventos enrutados son útiles cuando tienes que definir plantillas de control o usar contenedores de páginas o de diseño.

## <a name="events-as-a-programming-concept"></a>Eventos como concepto de programación

En general, los conceptos de eventos para programar una aplicación de Windows Runtime son parecidos al modelo de eventos de los lenguajes de programación más populares. Si ya sabes cómo trabajar con eventos de Microsoft .NET o C++, ya cuentas con ventaja. Pero no tienes por qué saber tantas cosas sobre los conceptos del modelo de eventos para poder realizar tareas básicas, como adjuntar controladores.

Cuando usas C#, Visual Basic o C++/CX como lenguaje de programación, la interfaz de usuario se define en el marcado (XAML). En la sintaxis del marcado XAML, algunos de los principios para conectar eventos entre los elementos de marcado y las entidades de código en tiempo de ejecución, son similares a otras tecnologías web como ASP.NET o HTML5.

**Nota:**    El código que proporciona la lógica en tiempo de ejecución para una interfaz de usuario definida por XAML se conoce a menudo como *código subyacente* o archivo de código subyacente. En las vistas de la solución de Microsoft Visual Studio, esta relación se muestra gráficamente, con el archivo de código subyacente como archivo dependiente y anidado frente a la página XAML a la que hace referencia.

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>Button.Click: introducción a los eventos y a XAML

Una de las tareas de programación más comunes de una aplicación de Windows Runtime es capturar la entrada del usuario en la interfaz de usuario. Por ejemplo, la interfaz de usuario podría tener un botón en el que el usuario deba hacer clic para enviar información o cambiar el estado.

La interfaz de usuario de la aplicación de Windows Runtime se define al generar el código XAML. Este XAML suele ser el resultado de una superficie de diseño en Visual Studio. Asimismo, el código XAML también se puede escribir en un editor de texto sin formato o en un editor XAML de terceros. Mientras generas ese código XAML, puedes conectar controladores de eventos para elementos individuales de la interfaz de usuario y, al mismo tiempo, definir todos los demás atributos XAML que establecen los valores de propiedad de ese elemento de la interfaz de usuario.

Para conectar eventos en XAML, tienes que especificar el nombre en cadena del método del controlador que ya has definido o que vas a definir más tarde en el código subyacente. Por ejemplo, este XAML define un objeto [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) con otras propiedades ([x:Name attribute](x-name-attribute.md), [**Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)) asignadas como atributos, y conecta un controlador para el evento [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) del botón, mediante una referencia al método `ShowUpdatesButton_Click`:

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**Sugerencia** La   *conexión de eventos* es un término de programación. Hace referencia al proceso o al código que se usa para indicar que las instancias de un evento deben invocar a un método de controlador con nombre. En la mayoría de modelos de código de procedimiento, la conexión de eventos es el código "AddHandler", implícito o explícito, que da nombre tanto al evento como al método y que normalmente implica una instancia de objeto de destino. En XAML, el código "AddHandler" está implícito y la conexión de eventos consiste en su totalidad en utilizar el nombre de atributo de un elemento de objeto como nombre del evento y el valor de ese atributo como nombre del controlador.

Escribe el controlador real en el lenguaje de programación que estés usando para todo el código de la aplicación y el código subyacente. Con el atributo `Click="ShowUpdatesButton_Click"`, creaste un contrato por el cual cuando se compila y se analiza el código XAML para marcado, tanto el proceso de compilación del marcado XAML de la acción de compilación del IDE como el proceso de análisis del XAML final cuando se carga la aplicación, pueden encontrar un método `ShowUpdatesButton_Click` dentro del código de la aplicación. `ShowUpdatesButton_Click` debe ser un método que implemente una firma de método compatible (basada en un delegado) para cualquier controlador del evento [**click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) . Por ejemplo, este código define el controlador `ShowUpdatesButton_Click`.

```csharp
private void ShowUpdatesButton_Click (object sender, RoutedEventArgs e) 
{
    Button b = sender as Button;
    //more logic to do here...
}
```

```vb
Private Sub ShowUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```

```cppwinrt
void winrt::MyNamespace::implementation::BlankPage::ShowUpdatesButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e)
{
    auto b{ sender.as<Windows::UI::Xaml::Controls::Button>() };
    // More logic to do here.
}
```

```cpp
void MyNamespace::BlankPage::ShowUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e) 
{
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

En este ejemplo, el método `ShowUpdatesButton_Click` se basa en el delegado [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler). Sabrá que se trata del delegado que se va a usar porque verá el delegado denominado en la sintaxis del método [**click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) .

**Sugerencia**    Visual Studio proporciona una manera cómoda de asignar un nombre al controlador de eventos y definir el método de control mientras se edita XAML. Cuando proporciones el nombre de atributo del evento en el editor de texto XAML, espera un momento hasta que se muestre una lista de Microsoft IntelliSense. Si hace clic en ** &lt; nuevo controlador &gt; de eventos** de la lista, Microsoft Visual Studio sugerirá un nombre de método basado en el valor de **x:Name** (o nombre de tipo) del elemento, el nombre del evento y un sufijo numérico. Es entonces cuando puedes hacer clic en el nombre del controlador de eventos seleccionado y hacer clic en **Navegar al controlador de eventos**. De esta manera, irás directamente a la definición del controlador de eventos recién insertada, tal como se ve en la vista del editor de código del archivo de código subyacente de la página XAML. El controlador de eventos ya tiene la firma correcta, incluido el parámetro *sender* y la clase de datos de evento que el evento usa. Además, si ya existe un método de controlador con la firma correcta en el código subyacente, el nombre de ese método aparece en la lista desplegable de autocompletar junto con la opción ** &lt; nuevo controlador &gt; de eventos** . Asimismo, también puedes presionar la tecla Tab como método abreviado, en lugar de hacer clic en los elementos de la lista de IntelliSense.

## <a name="defining-an-event-handler"></a>Definición de un controlador de evento

En el caso de los objetos que son elementos de la interfaz de usuario y están declarados en XAML, el código de controlador de evento se define en la clase parcial que funciona como código subyacente de una página XAML. Los controladores de eventos son métodos que escribes como parte de la clase parcial que está asociada a tu código XAML. Estos controladores de eventos se basan en los delegados que un evento determinado usa. Los métodos de tu controlador de eventos pueden ser públicos o privados. El acceso privado funciona porque el controlador y la instancia que creó el código XAML se unen finalmente con la generación del código. La recomendación general es hacer que los métodos del controlador de eventos sean privados en la clase.

**Nota:**    Los controladores de eventos para C++ no se definen en las clases parciales, se declaran en el encabezado como un miembro de clase privada. Las acciones de compilación de un proyecto C++ se encargan de generar el código que admita el sistema de tipo XAML y el modelo de código subyacente para C++.

### <a name="the-sender-parameter-and-event-data"></a>Parámetro *sender* y datos del evento

El controlador que escribas para el evento podrá obtener acceso a los dos valores que están disponibles como entrada de cada caso en el que se invoca el controlador. El primero de estos valores es *sender*, que es una referencia al objeto donde se adjunta el controlador. El parámetro *sender* se escribe como el tipo base **Object**. Una técnica común es convertir *sender* a un tipo más preciso. Esta técnica resulta útil si esperas comprobar o cambiar el estado del mismo objeto *sender*. En función del diseño de la aplicación, ya sabes a qué tipo puedes convertir *sender* con seguridad, según la ubicación donde se adjuntó el controlador u otros aspectos específicos del diseño.

El segundo valor son los datos del evento, los cuales generalmente aparecen en las definiciones de sintaxis como el parámetro *e*. Puedes descubrir qué propiedades de los datos del evento se encuentran disponibles, examinando el parámetro *e* del delegado que está asignado al evento específico que estás controlando y, a continuación, mediante el uso de IntelliSense o el examinador de objetos de Visual Studio. También puedes usar la documentación de referencia de Windows Runtime.

Para algunos eventos, los valores de propiedad específicos de los datos del evento son tan importantes como saber que el evento ocurrió. Esto es especialmente cierto en los eventos de entrada. En cuanto a los eventos de puntero, podría ser importante la posición del puntero al producirse el evento. En el caso de los eventos de teclado, todas las posibles pulsaciones de tecla generan los eventos [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown) y [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup). Para determinar qué tecla presionó el usuario, debes obtener acceso a la clase [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs), que está disponible en el controlador de eventos. Para obtener más información sobre el control de eventos de entrada, consulta [Interacciones de teclado](../design/input/keyboard-interactions.md) y [Controlar la entrada de puntero](../design/input/handle-pointer-input.md). Los eventos y escenarios de entrada suelen contar con otros elementos a tener en cuenta que no se tratan en este tema como, por ejemplo, la captura del puntero de los eventos de puntero, las teclas modificadoras y los códigos de teclas de la plataforma para los eventos de teclado.

### <a name="event-handlers-that-use-the-async-pattern"></a>Controladores de eventos que usan el patrón **async**

En ocasiones, querrás usar las API que usan un patrón **async** en un controlador de eventos. Por ejemplo, podrías usar una clase [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) en una clase [**AppBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) para mostrar un selector de archivos e interactuar con él. Pero recuerda que muchas de las API del selector de archivos son asincrónicas. Por ello, es necesario llamarlas en un ámbito que admita **async**/await; además, el compilador aplicará esta acción. Así pues, lo que puedes hacer es agregar la palabra clave **async** al controlador de eventos, de modo que este pase a ser **async** **void**. Gracias a ello, el controlador de eventos podrá realizar llamadas a las que se puede aplicar **async**/await.

Para obtener un ejemplo de controladores de eventos que requieren la interacción del usuario mediante el patrón **async**, consulta el tema [Acceso a los archivos y selectores de archivos](/previous-versions/windows/apps/jj655411(v=win.10)) (que forma parte de la serie [Crear la primera aplicación de Windows Runtime con C# o Visual Basic](/previous-versions/windows/apps/hh974581(v=win.10))). Te recomendamos que también leas [Llamar a API asincrónicas en C).

## <a name="adding-event-handlers-in-code"></a>Agregar controladores de eventos en el código

XAML no es la única forma de asignar un controlador de evento a un objeto. Para agregar controladores de eventos a un objeto en particular del código, incluidos los objetos que no pueden usarse en XAML, puedes usar la sintaxis específica del lenguaje para agregar controladores de eventos.

En C#, la sintaxis es usar el operador `+=`. El controlador se registra haciendo referencia al nombre del método del controlador del evento que se encuentra a la derecha del operador.

Si usas un código para agregar controladores de eventos a los objetos que aparecen en la interfaz de usuario en tiempo de ejecución, se recomienda agregar esos controladores en respuesta a una devolución de llamada o evento de duración del objeto, como [**Loaded**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) o [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate), de manera que los controladores de eventos del objeto correspondiente estén listos para los eventos que inicie el usuario en tiempo de ejecución. En este ejemplo se muestra un esquema XAML de la estructura de página y se proporciona la sintaxis del lenguaje C# para agregar un controlador de eventos a un objeto.

```xaml
<Grid x:Name="LayoutRoot" Loaded="LayoutRoot_Loaded">
  <StackPanel>
    <TextBlock Name="textBlock1">Put the pointer over this text</TextBlock>
...
  </StackPanel>
</Grid>
```

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += textBlock1_PointerEntered;
    textBlock1.PointerExited += textBlock1_PointerExited;
}
```

**Nota:**    Existe una sintaxis más detallada. En 2005, C# incorporó una característica llamada inferencia de delegado, que permite a un compilador inferir una nueva instancia de delegado y, además, habilita la sintaxis anterior que es más simple. La sintaxis detallada es funcionalmente idéntica al ejemplo anterior, pero crea explícitamente una nueva instancia de delegado antes de registrarla y, por lo tanto, no aprovecha la inferencia de delegado. Esta sintaxis explícita es menos común, pero podrías verla en algunos ejemplos de código.

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Existen dos posibilidades para la sintaxis de Visual Basic. Una es cotejar la sintaxis de C# y adjuntar controladores directamente a las instancias. Esto requiere la palabra clave **AddHandler** y también el operador **AddressOf** que anula la referencia al nombre de método del controlador.

La otra opción de la sintaxis de Visual Basic es usar la palabra clave **Handles** en los controladores de eventos. Esta técnica es apropiada para casos en los que se espera que existan controladores en los objetos en tiempo de carga y que persistan durante la duración del objeto. Si usas **Handles** en un objeto que está definido en XAML, debes proporcionar un elemento **Name** / **x:Name**. Este nombre se convierte en el calificador de instancia necesario para la parte *Instance.Event* de la sintaxis de **Handles**. En este caso, no necesitas un controlador de eventos basado en la duración del objeto para iniciar la conexión de otros controladores de eventos; las conexiones **Handles** se crean cuando compilas la página XAML.

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**Nota:**    Visual Studio y su superficie de diseño XAML generalmente promueven la técnica de control de instancias en lugar de la palabra clave **Handles** . Esto se debe a que establecer la conexión del controlador de eventos en XAML forma parte del flujo de trabajo del desarrollador y diseñador, y la técnica de la palabra clave **Handles** es incompatible con la conexión de los controladores de eventos en XAML.

En C++/CX, también se usa la **+=** sintaxis, pero existen diferencias con el formato básico de C#:

- No existe inferencia de delegado, por lo que debes usar **ref new** en la instancia de delegado.
- El constructor delegado tiene dos parámetros y debe tener el objeto de destino como primer parámetro. Como norma general, especificas **this**.
- El constructor delegado requiere la dirección del método como segundo parámetro, por lo que el **&** operador de referencia precede al nombre del método.

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>Quitar controladores de eventos del código

Normalmente, no es necesario quitar controladores de eventos del código aunque se hayan agregado a él. El comportamiento de la duración del objeto en la mayoría de los objetos de Windows Runtime, como páginas y controles, destruirá los objetos cuando se desconecten de la clase [**Window**](/uwp/api/Windows.UI.Xaml.Window) principal y su árbol visual; igualmente, también se destruirán las referencias delegadas. Para ello, .NET usa la recolección de elementos no utilizados y Windows Runtime con C++/CX usa referencias débiles de manera predeterminada.

En ocasiones excepcionales, querrás quitar los controladores de eventos de forma explícita. Entre ellas se incluyen las siguientes:

- Controladores agregados para eventos estáticos, en los que no se puede usar la recolección de eventos no utilizados de manera convencional. Algunos ejemplos de eventos estáticos en la API de Windows Runtime, son los eventos de las clases [**CompositionTarget**](/uwp/api/Windows.UI.Xaml.Media.CompositionTarget) y [**Clipboard**](/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard).
- Código de prueba donde quieres que la eliminación de controladores sea inmediata, o el código donde quieres intercambiar controladores de eventos antiguos o nuevos para un evento en tiempo de ejecución.
- La implementación de un descriptor de acceso **remove** personalizado.
- Eventos estáticos personalizados.
- Controladores para las navegaciones de la página.

[**FrameworkElement.Unloaded**](/uwp/api/windows.ui.xaml.frameworkelement.unloaded) o [**Page.NavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) son los posibles desencadenadores de eventos que tienen las posiciones adecuadas en la administración del estado y la duración del objeto, de tal forma que puedes usarlos para quitar controladores de otros eventos.

Por ejemplo, puede quitar un controlador de eventos denominado **textBlock1 \_ PointerEntered** del objeto de destino **textBlock1** mediante este código.

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

Asimismo, también puedes quitar los controladores cuando el evento se haya agregado a través de un atributo XAML, lo que implica que el controlador se agregó en código generado. Esto te resultará más sencillo si proporcionaste un valor **Name** para el elemento al que se adjuntó el controlador, ya que proporciona una referencia de objeto del código posterior; sin embargo, también podrías recorrer el árbol de objetos para buscar la referencia de objeto necesaria, en aquellos casos en los que el objeto no tenga un valor **Name**.

Si necesitas quitar un controlador de eventos en C++/CX, necesitarás un token de registro, el cual deberías haber recibido por medio del valor que devolvió el registro del controlador de eventos `+=`. Esto se debe a que el valor utilizado para el lado derecho de la anulación de registro de `-=` en la sintaxis C++/CX es el token y no el nombre del método. En C++/CX, no puedes quitar controladores que se agregaron como un atributo XAML porque el código generado en C++/CX no guarda un token.

## <a name="routed-events"></a>Eventos enrutados

El entorno Windows Runtime con C#, Microsoft Visual Basic o C++/CX admite el concepto de evento enrutado, para un conjunto de eventos que están presentes en la mayoría de los elementos de la interfaz de usuario. Estos eventos se usan en escenarios de entrada e interacción del usuario, y se implementan en la clase base [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement). Esta es una lista de eventos de entrada que son eventos enrutados:

- [**BringIntoViewRequested**](/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Omisiones**](/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Holding**](/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**PreviewKeyUp**](/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped)

Un evento enrutado es un evento que posiblemente pasa (se *enruta*) de un objeto secundario, a cada uno de los sucesivos objetos principales de un árbol de objetos. La estructura XAML de tu interfaz de usuario se asemeja a este árbol, en el que la raíz es el elemento raíz en XAML. El verdadero árbol de objetos podría variar en cierta forma del anidamiento del elemento XAML, porque el primero no incluye características del lenguaje XAML, como las etiquetas de elementos de propiedad. Puedes considerar los eventos enrutados como una *propagación* desde el elemento de objeto XAML secundario que genera el evento hacia el elemento de objeto primario que los contiene. El evento y su evento de datos se pueden administrar en varios objetos junto con la ruta del evento. Si ningún elemento tiene controladores, la ruta probablemente siga avanzando hasta alcanzar el elemento raíz.

Si conoces las tecnologías web como Dynamic HTML (DHTML) o HTML5, quizás ya estés familiarizado con el concepto de evento de *propagación*.

Cuando un evento enrutado se propaga por su ruta de evento, todos los controladores de eventos adjuntos acceden a una instancia compartida de los datos del evento. Por lo tanto, si un controlador puede escribir cualquiera de los datos del evento, los cambios realizados en estos se pasarán al próximo controlador y puede que ya no representen los datos originales del evento. Cuando un evento tiene un comportamiento de evento enrutado, la documentación de referencia incluirá comentarios u otras anotaciones sobre el comportamiento enrutado.

### <a name="the-originalsource-property-of-routedeventargs"></a>Propiedad **OriginalSource** de **RoutedEventArgs**

Cuando un evento propaga una ruta de evento, el objeto *sender* ya no es el mismo objeto que generó el evento. En vez de eso, *sender* es el objeto donde se adjunta el controlador que se está invocando.

En algunos casos, *sender* ya no será el objeto que te interesa, sino que querrás obtener otro tipo de información como, por ejemplo, sobre cuál de los posibles objetos secundarios se encontraba el puntero al desencadenarse un evento de puntero o qué objeto de una interfaz de usuario más amplia tenía el foco cuando un usuario presionó una tecla. En estos casos, puedes usar el valor de la propiedad [**OriginalSource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource). En todos los puntos de la ruta, **OriginalSource** notifica el objeto original que generó el evento y no el objeto al que se adjuntó el controlador. Sin embargo, para los eventos de entrada [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement), ese objeto original suele ser uno que no es inmediatamente visible en el XAML de definición de la interfaz de usuario de la página. En cambio, podría ser una parte con plantilla de un control. Por ejemplo, si el usuario mantiene el puntero sobre el borde de una clase [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button), para la mayoría de los eventos de puntero la propiedad **OriginalSource** forma parte de la plantilla [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) de la propiedad [**Template**](/uwp/api/windows.ui.xaml.controls.control.template), no de la misma clase **Button**.

**Sugerencia**    La propagación de eventos de entrada es especialmente útil si va a crear un control con plantilla. En todos los controles que tienen una plantilla, su consumidor puede aplicar una nueva plantilla. Un consumidor que intenta recrear una plantilla de trabajo podría eliminar por accidente cierto código de control de eventos declarado en la plantilla predeterminada. Aún puedes proporcionar código de control de eventos en el nivel del control, adjuntando controladores como parte de la invalidación [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate) en la definición de la clase. A continuación, puedes capturar los eventos de entrada que se propagan en la raíz del control durante la creación de instancias.

### <a name="the-handled-property"></a>Propiedad **Handled**

Varias clases de datos de evento pertenecientes a eventos enrutados específicos contienen una propiedad llamada **Handled**. Si quieres ver algún ejemplo, consulta las propiedades [**PointerRoutedEventArgs.Handled**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled), [**KeyRoutedEventArgs.Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled), [**DragEventArgs.Handled**](/uwp/api/windows.ui.xaml.drageventargs.handled). En todos los casos, **Handled** es una propiedad booleana configurable.

Si configuras la propiedad **Handled** como **true**, esta influirá en el comportamiento del sistema de eventos. Si **Handled** se establece en **true**, el enrutamiento se detiene para la mayoría de los controladores de eventos; esto es, el evento no continúa a lo largo de la ruta para notificar a otros controladores adjuntos de ese caso de evento en particular. De ti dependen el significado de la acción "controlar" en el contexto del evento y el modo en que la aplicación responde a ella. Básicamente, **Handled** es un protocolo simple que permite que el código de la aplicación declare que la instancia de un evento no necesita propagarse en ningún contenedor, ya que la lógica de la aplicación se encarga de realizar las acciones necesarias. A la inversa, sin embargo, debe tener cuidado de no controlar eventos que probablemente deberían propagarse para que los comportamientos integrados del sistema o control puedan actuar. Por ejemplo, el control de eventos de bajo nivel dentro de las partes o los elementos de un control de selección puede ser perjudicial. El control de selección podría estar buscando eventos de entrada para determinar si la selección debe cambiar.

No todos los eventos enrutados pueden cancelar una ruta de esta forma; sabrás cuáles son porque no tendrán la propiedad **Handled**. Por ejemplo, [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) y [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus) se propagan, pero siempre lo hacen siguiendo todo el recorrido hasta la raíz; asimismo, sus clases de datos de evento no tienen una propiedad **Handled** que pueda influir en ese comportamiento.

##  <a name="input-event-handlers-in-controls"></a>Controladores de eventos de entrada en controles

Algunos controles de Windows Runtime usan a veces el concepto **Handled** de los eventos de entrada de forma interna. Esto puede hacer que parezca que un evento de entrada nunca se produce, porque el código de usuario no puede controlarlo. Por ejemplo, la clase [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) incluye una lógica que controla deliberadamente el evento de entrada general [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed). Lo hace de esa manera, porque los botones generan un evento [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) que se inicia no solo con una entrada de presión del puntero, sino también con otros modos de entrada (por ejemplo, el control teclas como Entrar), que pueden invocar al botón cuando recibe el foco. Para diseñar la clase **Button**, el evento de entrada sin procesar se controla conceptualmente y, en su lugar, los consumidores de clase como tu código de usuario, pueden interactuar con el evento **Click** que resulta ser relevante para el control. Los temas sobre las clases de control específicas que se encuentran en la referencia de la API de Windows Runtime, a menudo advierten acerca del comportamiento del control de eventos que la clase implementa. En algunos casos, puedes cambiar el comportamiento anulando los métodos **On**_Event_. Por ejemplo, puedes cambiar la forma en que tu clase derivada [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) reacciona a la entrada de teclado anulando [**Control.OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown).

##  <a name="registering-handlers-for-already-handled-routed-events"></a>Registrar controladores para eventos enrutados previamente controlados

Anteriormente dijimos que establecer **Handled** en **true** impide que se invoque la mayoría de los controladores. Sin embargo, el método [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) ofrece una técnica con la que puedes adjuntar un controlador que se invoca siempre para la ruta, incluso si otro controlador anterior de la ruta ha establecido **Handled** como **true** en los datos del evento compartido. Esta técnica resulta útil si un control que usas se dedica a controlar el evento en su composición interna, o para la lógica específica del control, pero aún quieres responder a él desde una instancia del control o desde la interfaz de usuario de la aplicación. Sin embargo, esta técnica se debe usar con precaución porque puede contradecir el propósito de **Handled** y, posiblemente, interrumpir las interacciones previstas del control.

Solo los eventos enrutados que tengan su correspondiente identificador de evento enrutado pueden usar la técnica de control de eventos [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler), ya que el identificador es una entrada obligatoria del método **AddHandler**. Consulta la documentación de referencia de [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) para obtener una lista de los eventos que tienen identificadores de eventos enrutados disponibles. La mayor parte de esta lista coincide con la lista de eventos enrutados que te mostramos anteriormente. La excepción son los dos últimos eventos de la lista: [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) y [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus) no tienen un identificador de evento enrutado, de modo que no puedes usar **AddHandler** con ellos.

## <a name="routed-events-outside-the-visual-tree"></a>Eventos enrutados fuera del árbol visual

Ciertos objetos participan en una relación con el árbol visual principal que conceptualmente es similar a tener una superposición de los elementos visuales principales. Estos objetos no forman parte de las relaciones habituales entre elementos principales y secundarios que conectan todos los elementos del árbol con la raíz visual. Este es el caso de cualquier clase [**Popup**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) o [**ToolTip**](/uwp/api/Windows.UI.Xaml.Controls.ToolTip) que se muestre. Si deseas controlar los eventos enrutados de una clase **Popup** o **ToolTip**, debes colocar los controladores en elementos específicos de la interfaz de usuario de **Popup** o **ToolTip** y no en los mismos elementos **Popup** o **ToolTip**. No dependas del enrutamiento que se realice en el interior de la composición que se lleva a cabo para el contenido de **Popup** o **ToolTip**. Esto se debe a que el enrutamiento de eventos enrutados solo funciona a lo largo del árbol visual principal. No se considera que las clases **Popup** o **ToolTip** sean un elemento principal de los elementos de la interfaz de usuario subsidiaria, por lo que nunca reciben el evento enrutado, aunque este esté tratando de usar algo similar al fondo predeterminado de **Popup** como área de captura de los eventos de entrada.

## <a name="hit-testing-and-input-events"></a>Prueba de posicionamiento y eventos de entrada

La determinación de si un elemento es visible para la entrada táctil, la entrada de ratón o la entrada de lápiz y de dónde está visible se denomina *prueba de acceso*. En el caso de las acciones táctiles y también de los eventos de manipulación o específicos de la interacción que son consecuencia de una acción táctil, un elemento debe ser visible en la prueba de acceso para poder ser origen de eventos y generar el evento que está asociado a la acción. De lo contrario, la acción pasa a través del elemento a cualquier elemento subyacente o elemento principal del árbol visual que pueda interaccionar con esos datos. Hay varios factores que afectan a la prueba de posicionamiento, pero puedes determinar si un elemento específico puede generar eventos de entrada comprobando la propiedad [**IsHitTestVisible**](/uwp/api/windows.ui.xaml.uielement.ishittestvisible). Esta propiedad solo devuelve **true** si el elemento cumple con los siguientes criterios:

- El valor de la propiedad [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) del elemento es [**Visible**](/uwp/api/Windows.UI.Xaml.Visibility).
- El valor de la propiedad **Background** o **Fill** del elemento no es **null**. Un valor **null de la clase ** [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) provoca transparencia e invisibilidad en la prueba de posicionamiento. (Para que un elemento sea transparente pero se pueda someter a la prueba de posicionamiento, usa un pincel cuya propiedad sea [**Transparent**](/uwp/api/windows.ui.colors.transparent) en lugar de **null**).

**Nota**            La clase [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) no define las propiedades   **Background** y **Fill**; estas las definen diferentes clases derivadas como [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) y [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape). Sin embargo, las implicaciones de los pinceles que uses en las propiedades de primer plano y segundo plano, son las mismas tanto para la prueba de posicionamiento como para los eventos de entrada, independientemente de qué subclase implemente las propiedades.

- Si el elemento es un control, el valor de la propiedad [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) debe ser **true**.
- El elemento debe tener dimensiones reales en el diseño. Un elemento cuyas propiedades [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) y [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) tengan un valor 0, no generarán eventos de entrada.

Algunos controles tienen reglas especiales para la prueba de posicionamiento. Por ejemplo, [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) no tiene ninguna propiedad **Background**, pero se puede someter a la prueba de posicionamiento en toda la región de sus dimensiones. [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) y [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement) se pueden someter a la prueba de posicionamiento según las dimensiones definidas de su rectángulo e independientemente del contenido transparente, como el canal alfa del archivo de origen multimedia que se esté mostrando. [              Los controles **WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView) tienen un comportamiento especial en la prueba de posicionamiento porque el HTML hospedado puede controlar la entrada y generar eventos de script.

La mayoría de las clases [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel) y [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border) no se pueden someter a la prueba de posicionamiento en su propio segundo plano, aunque pueden controlar los eventos de entrada de usuario que se enrutan desde los elementos que contienen.

Puedes determinar qué elementos están ubicados en la misma posición que un evento de entrada de usuario, independientemente de si se puede realizar la prueba de posicionamiento en los elementos o no. Para ello, llama al método [**FindElementsInHostCoordinates**](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates). Tal como indica su nombre, este método busca los elementos en una ubicación relativa a un elemento host especificado. Pero las transformaciones aplicadas y los cambios de diseño pueden ajustar el sistema de coordenadas relativo de un elemento y, por tanto, afectar a los elementos que se encuentran en una ubicación determinada.

## <a name="commanding"></a>Comandos

Algunos elementos de la interfaz de usuario admiten *comandos*. Los comandos usan eventos enrutados relacionados con la entrada en su implementación subyacente y permiten procesar la entrada de la interfaz de usuario relacionada (una acción de puntero determinada o una tecla aceleradora específica) mediante la invocación de un único controlador de comandos. Si hay comandos disponibles para un elemento de interfaz de usuario, te recomendamos que uses las API de comandos en lugar de eventos de entrada discretos. Las referencias **Binding** se suelen usar en propiedades de una clase que defina el modelo de vista de los datos. Las propiedades contienen comandos con nombre que implementan el patrón de comandos **ICommand** específico del lenguaje. Para obtener más información, consulta [**ButtonBase.Command**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).

## <a name="custom-events-in-the-windows-runtime"></a>Eventos personalizados en Windows Runtime

A la hora de definir eventos personalizados, el modo en que se agrega el evento y el significado que ello tiene para el diseño de la clase depende en gran medida del lenguaje de programación que uses.

- En C# y Visual Basic, defines un evento CLR. Puede usar el patrón de eventos estándar de .net, siempre y cuando no use descriptores de acceso personalizados (**Agregar** / **quitar**). Sugerencias adicionales:
    - Es buena idea usar [**System.EventHandler<TEventArgs>**](/dotnet/api/system.eventhandler-1) para el controlador de eventos, porque incorpora la conversión al delegado de eventos genéricos [**EventHandler<T>**](/uwp/api/windows.foundation.eventhandler) de Windows Runtime.
    - No bases la clase de datos de evento en [**System.EventArgs**](/dotnet/api/system.eventargs), porque no se convierte en un elemento compatible con Windows Runtime. Usa una clase de datos de evento existente o no uses ninguna clase base.
    - Si usa descriptores de acceso personalizados, consulte [eventos personalizados y descriptores de acceso de eventos en Windows Runtime componentes](/previous-versions/windows/apps/hh972883(v=vs.140)).
    - Si no conoces bien el patrón de eventos de .NET estándar, consulta el tema sobre la [definición de eventos para clases de Silverlight personalizadas](/previous-versions/windows/). Aunque se trata de documentación para Microsoft Silverlight, ofrece un buen resumen del código y de los conceptos relativos al patrón de eventos de .NET estándar.
- Para C++/CX, consulta [Eventos (C++/CX)](/cpp/cppcx/events-c-cx).
    - Usa referencias con nombre incluso en tus propios usos de eventos personalizados. No utilices lambda para eventos personalizados, ya que puede crear una referencia circular.

No puedes declarar un evento enrutado personalizado para Windows Runtime; los eventos enrutados se limitan al conjunto procedente de Windows Runtime.

La definición de un evento personalizado suele realizarse como parte del ejercicio de definir un control personalizado. Un patrón común consiste en tener una propiedad de dependencia con una devolución de llamada modificada por propiedades y definir un evento personalizado generado por la devolución de llamada modificada por propiedades en algunos casos o siempre. Los usuarios del control no tienen acceso a la devolución de llamada modificada por propiedades que hayas definido, pero sí disponen de un evento de notificación. Para obtener más información, consulta [Propiedades de dependencia personalizadas](custom-dependency-properties.md).

## <a name="related-topics"></a>Temas relacionados

* [Introducción a XAML](xaml-overview.md)
* [Inicio rápido: entrada táctil](/previous-versions/windows/apps/hh465387(v=win.10))
* [Interacciones de teclado](../design/input/keyboard-interactions.md)
* [Eventos y funciones delegadas de .NET](/previous-versions/17sde2xt(v=vs.110))
* [Crear componentes de Windows Runtime](/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler)