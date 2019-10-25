---
description: Se describe el concepto de programación de eventos en una aplicación Windows Runtime, al C#usar, Visual Basic o C++ extensiones de componentesC++de Visual (/CX) como lenguaje de programación y XAML para la definición de la interfaz de usuario.
title: Introducción a eventos y eventos enrutados
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 759e47348198feedbf7b1e3ee2c0bfc2da1da671
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690396"
---
# <a name="events-and-routed-events-overview"></a>Introducción a eventos y eventos enrutados

**API importantes**
- [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)
- [**RoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

Se describe el concepto de programación de eventos en una aplicación Windows Runtime, al C#usar, Visual Basic o C++ extensiones de componentesC++de Visual (/CX) como lenguaje de programación y XAML para la definición de la interfaz de usuario. Puede asignar Controladores para eventos como parte de las declaraciones de los elementos de la interfaz de usuario en XAML, o puede Agregar los controladores en el código. Windows Runtime admite *eventos enrutados*: ciertos eventos de entrada y eventos de datos se pueden controlar mediante objetos más allá del objeto que desencadenó el evento. Los eventos enrutados son útiles cuando se definen plantillas de control o se utilizan contenedores de páginas o de diseño.

## <a name="events-as-a-programming-concept"></a>Eventos como un concepto de programación

En términos generales, los conceptos de eventos al programar una aplicación Windows Runtime son similares al modelo de eventos de los lenguajes de programación más populares. Si ya sabe cómo trabajar con eventos de Microsoft .NET C++ o, tiene un inicio principal. Sin embargo, no es necesario saber mucho sobre los conceptos del modelo de eventos para realizar algunas tareas básicas, como adjuntar controladores.

Al usar C#, Visual Basic o C++/CX como lenguaje de programación, la interfaz de usuario se define en marcado (XAML). En la sintaxis de marcado XAML, algunos de los principios de conexión de eventos entre elementos de marcado y entidades de código en tiempo de ejecución son similares a otras tecnologías Web, como ASP.NET o HTML5.

**Tenga en cuenta**  el código que proporciona la lógica en tiempo de ejecución para una interfaz de usuario definida por XAML se conoce a menudo como *código subyacente* o el archivo de código subyacente. En las vistas de soluciones de Microsoft Visual Studio, esta relación se muestra gráficamente, donde el archivo de código subyacente es un archivo dependiente y anidado en comparación con la página XAML a la que hace referencia.

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>Button. click: Introducción a los eventos y XAML

Una de las tareas de programación más comunes para una aplicación Windows Runtime es capturar la entrada del usuario en la interfaz de usuario. Por ejemplo, la interfaz de usuario podría tener un botón en el que el usuario debe hacer clic para enviar información o cambiar el estado.

Puede definir la interfaz de usuario para la aplicación Windows Runtime generando XAML. Este código XAML suele ser el resultado de una superficie de diseño en Visual Studio. También puede escribir el XAML en un editor de texto sin formato o en un editor XAML de terceros. Al generar ese XAML, puede conectar los controladores de eventos para los elementos individuales de la interfaz de usuario al mismo tiempo que define todos los demás atributos XAML que establecen los valores de propiedad de ese elemento de la interfaz de usuario.

Para conectar los eventos en XAML, especifique el nombre de formato de cadena del método de controlador que ya ha definido o que definirá más adelante en el código subyacente. Por ejemplo, este código XAML define un objeto de [**botón**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) con otras propiedades ([atributo x:Name](x-name-attribute.md), [**contenido**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content)) asignadas como atributos y conecta un controlador para el evento [**click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) del botón haciendo referencia a un método denominado `ShowUpdatesButton_Click`:

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**Sugerencia**: la  *conexión de eventos* es un término de programación. Hace referencia al proceso o código que indica que las apariciones de un evento deben invocar un método de controlador con nombre. En la mayoría de los modelos de código de procedimientos, la conexión de eventos es código implícito o explícito "AddHandler" que nombra el evento y el método, y normalmente implica una instancia de objeto de destino. En XAML, el "AddHandler" es implícito y la conexión de eventos consta en su totalidad de nombrar el evento como el nombre de atributo de un elemento de objeto y asignar un nombre al controlador como el valor de ese atributo.

El controlador real se escribe en el lenguaje de programación que se usa para todo el código de la aplicación y el código subyacente. Con el atributo `Click="ShowUpdatesButton_Click"`, ha creado un contrato que cuando el XAML se compila y analiza, tanto el paso de compilación de marcado XAML en la acción de compilación del IDE como el análisis de XAML eventual cuando la aplicación se carga puede encontrar un método denominado `ShowUpdatesButton_Click` como parte del co de la aplicación resguardo. `ShowUpdatesButton_Click` debe ser un método que implemente una firma de método compatible (basada en un delegado) para cualquier controlador del evento [**click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) . Por ejemplo, este código define el controlador de `ShowUpdatesButton_Click`.

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

En este ejemplo, el método `ShowUpdatesButton_Click` se basa en el delegado [**RoutedEventHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventhandler) . Sabrá que se trata del delegado que se va a usar porque verá el delegado denominado en la sintaxis del método [**click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) .

**Sugerencia**  Visual Studio proporciona una manera cómoda de asignar un nombre al controlador de eventos y definir el método de control mientras se edita XAML. Cuando proporcione el nombre de atributo del evento en el editor de texto XAML, espere un momento hasta que se muestre una lista de Microsoft IntelliSense. Si hace clic en **&lt;nuevo controlador de eventos&gt;** de la lista, Microsoft Visual Studio sugerirá un nombre de método basado en el valor de **x:Name** (o nombre de tipo) del elemento, el nombre del evento y un sufijo numérico. Después, puede hacer clic con el botón secundario en el nombre del controlador de eventos seleccionado y hacer clic en **navegar hasta el controlador de eventos**. Esto navegará directamente a la definición del controlador de eventos que se acaba de insertar, tal como se muestra en la vista del editor de código del archivo de código subyacente de la página XAML. El controlador de eventos ya tiene la firma correcta, incluido el parámetro *Sender* y la clase de datos de evento que usa el evento. Además, si ya existe un método de controlador con la firma correcta en el código subyacente, el nombre de ese método aparece en la lista desplegable de autocompletar junto con la opción **&lt;nuevo controlador de eventos&gt;** . También puede presionar la tecla TAB como un acceso directo en lugar de hacer clic en los elementos de lista de IntelliSense.

## <a name="defining-an-event-handler"></a>Definir un controlador de eventos

En el caso de los objetos que son elementos de la interfaz de usuario y se declaran en XAML, el código del controlador de eventos se define en la clase parcial que actúa como el código subyacente de una página XAML. Los controladores de eventos son métodos que se escriben como parte de la clase parcial que está asociada a su código XAML. Estos controladores de eventos se basan en los delegados que usa un evento determinado. Los métodos de control de eventos pueden ser públicos o privados. El acceso privado funciona porque el controlador y la instancia creados por el XAML se unen finalmente mediante la generación de código. En general, se recomienda que los métodos de control de eventos sean privados en la clase.

**Tenga en cuenta**  los controladores C++ de eventos para no se definen en las clases parciales, se declaran en el encabezado como un miembro de clase privada. Las acciones de compilación de C++ un proyecto se encargan de generar código que admita el sistema de tipos XAML y el C++modelo de código subyacente para.

### <a name="the-sender-parameter-and-event-data"></a>Parámetro de *remitente* y datos de evento

El controlador que escriba para el evento puede tener acceso a dos valores que están disponibles como entrada para cada caso en el que se invoca el controlador. El primer valor de este tipo es *Sender*, que es una referencia al objeto al que está asociado el controlador. El parámetro *Sender* se escribe como el tipo de **objeto** base. Una técnica común es convertir el *remitente* a un tipo más preciso. Esta técnica es útil si espera comprobar o cambiar el estado en el propio objeto de *remitente* . En función de su propio diseño de la aplicación, normalmente se conoce un tipo que es seguro convertir el *remitente* en, en función de dónde se adjunte el controlador o de otras características específicas del diseño.

El segundo valor son los datos de evento, que generalmente aparecen en las definiciones de sintaxis como el parámetro *e* . Puede detectar qué propiedades de los datos de eventos están disponibles si examina el parámetro *e* del delegado que se asigna para el evento específico que está controlando y, a continuación, usa IntelliSense o examinador de objetos en Visual Studio. También puede usar la documentación de referencia de Windows Runtime.

En algunos eventos, los valores de propiedad específicos de los datos de evento son tan importantes como saber que se ha producido el evento. Esto es especialmente cierto en los eventos de entrada. En el caso de los eventos de puntero, la posición del puntero en el momento en que se produjo el evento puede ser importante. En el caso de los eventos de teclado, todas las pulsaciones de tecla posibles desencadenan un evento [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) y [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) . Para determinar qué tecla presionó un usuario, debe tener acceso al [**KeyRoutedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs) que está disponible para el controlador de eventos. Para obtener más información sobre el control de eventos de entrada, consulte [interacciones de teclado](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions) y control de la [entrada de puntero](https://docs.microsoft.com/windows/uwp/input-and-devices/handle-pointer-input). Los eventos de entrada y los escenarios de entrada suelen tener consideraciones adicionales que no se tratan en este tema, como la captura de puntero para eventos de puntero, y las teclas modificadoras y los códigos de clave de plataforma para eventos de teclado.

### <a name="event-handlers-that-use-the-async-pattern"></a>Controladores de eventos que usan el patrón **Async**

En algunos casos, querrá usar las API que usan un patrón **Async** dentro de un controlador de eventos. Por ejemplo, puede usar un [**botón**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) en un [**AppBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.AppBar) para mostrar un selector de archivos e interactuar con él. Sin embargo, muchas de las API del selector de archivos son asincrónicas. Deben llamarse dentro de un ámbito de/awaitable **asincrónico**y el compilador lo exigirá. Lo que puede hacer es agregar la palabra clave **Async** al controlador de eventos de modo que el controlador sea ahora **Async** **void**. Ahora el controlador de eventos tiene permiso para realizar llamadas **asincrónicas**de/awaitable.

Para obtener un ejemplo de control de eventos de interacción de usuario mediante el patrón **Async** , vea [acceso y selectores de archivos](https://docs.microsoft.com/previous-versions/windows/apps/jj655411(v=win.10)) (parte de la[aplicación de creación de su primera Windows Runtime C# con o Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/hh974581(v=win.10)) series). Vea también [llamada a API asincrónicas en C].

## <a name="adding-event-handlers-in-code"></a>Agregar controladores de eventos en el código

XAML no es la única manera de asignar un controlador de eventos a un objeto. Para agregar controladores de eventos a cualquier objeto dado en el código, incluidos los objetos que no se pueden usar en XAML, puede usar la sintaxis específica del lenguaje para agregar controladores de eventos.

En C#, la sintaxis es usar el operador`+=`. El controlador se registra haciendo referencia al nombre del método de control de eventos en el lado derecho del operador.

Si usa código para agregar controladores de eventos a objetos que aparecen en la interfaz de usuario en tiempo de ejecución, una práctica común es agregar tales controladores en respuesta a un evento de duración de objeto o una devolución de llamada, como [**Loaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.loaded) o [**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate), para que los controladores de eventos del el objeto pertinente está listo para los eventos iniciados por el usuario en tiempo de ejecución. En este ejemplo se muestra un esquema XAML de la estructura de página y C# , a continuación, se proporciona la sintaxis del lenguaje para agregar un controlador de eventos a un objeto.

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

**Tenga en cuenta**  existe una sintaxis más detallada. En 2005, C# se ha agregado una característica denominada inferencia de delegado, que permite que un compilador infiera la nueva instancia de delegado y habilita la sintaxis anterior y más sencilla. La sintaxis detallada es funcionalmente idéntica al ejemplo anterior, pero crea explícitamente una nueva instancia de delegado antes de registrarla, por lo que no aprovecha la inferencia de delegado. Esta sintaxis explícita es menos común, pero podría verla en algunos ejemplos de código.

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Existen dos posibilidades de Visual Basic sintaxis. Uno consiste en paralelizar la C# sintaxis y adjuntar controladores directamente a las instancias de. Esto requiere la palabra clave **AddHandler** y también el operador **AddressOf** que desreferencia el nombre del método de controlador.

La otra opción de Visual Basic sintaxis es usar la palabra clave **Handles** en los controladores de eventos. Esta técnica es adecuada para los casos en los que se espera que los controladores existan en los objetos en tiempo de carga y se conserven durante la vigencia del objeto. El uso de **identificadores** en un objeto que se define en XAML requiere que proporcione un **nombre** / **x:Name**. Este nombre se convierte en el calificador de instancia necesario para la parte de la *instancia. event* de la sintaxis de **Handles** . En este caso, no necesita un controlador de eventos basado en la duración de un objeto para iniciar la Asociación de los demás controladores de eventos; los **identificadores** de las conexiones se crean al compilar la página XAML.

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**Nota**  Visual Studio y su superficie de diseño XAML promueven normalmente la técnica de control de instancias en lugar de la palabra clave **Handles** . Esto se debe a que el establecimiento del cableado del controlador de eventos en XAML forma parte del flujo de trabajo típico del desarrollador de diseñadores y la técnica de la palabra clave **Handles** es incompatible con el cableado de los controladores de eventos en XAML.

En C++/CX, también se usa la sintaxis de **+=** , pero hay diferencias en la forma C# básica:

- No existe ninguna inferencia de delegado, por lo que debe usar **ref New** para la instancia de delegado.
- El constructor delegado tiene dos parámetros y requiere el objeto de destino como primer parámetro. Normalmente **se especifica.**
- El constructor delegado requiere la dirección del método como segundo parámetro, por lo que el operador de referencia **&** precede al nombre del método.

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>Quitar controladores de eventos en el código

Normalmente no es necesario quitar los controladores de eventos en el código, aunque se hayan agregado en el código. El comportamiento de duración del objeto para la mayoría de los objetos de Windows Runtime como páginas y controles destruirá los objetos cuando estén desconectados de la [**ventana**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) principal y su árbol visual, y también se destruirán las referencias a los delegados. .NET hace esto a través de la recolección de C++elementos no utilizados y Windows Runtime con/CX usa referencias débiles de forma predeterminada.

Hay algunos casos raros en los que desea quitar los controladores de eventos explícitamente. Entre ellos se incluyen los siguientes:

- Controladores agregados para eventos estáticos, que no pueden recopilar elementos no utilizados de forma convencional. Ejemplos de eventos estáticos en la API de Windows Runtime son los eventos de las clases [**CompositionTarget**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CompositionTarget) y [**Clipboard**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard) .
- Código de prueba en el que desea que la eliminación del controlador sea inmediata, o código donde desea intercambiar los controladores de eventos antiguos o nuevos para un evento en tiempo de ejecución.
- Implementación de un descriptor de acceso **Remove** personalizado.
- Eventos estáticos personalizados.
- Controladores para las navegaciones de páginas.

[**FrameworkElement. Unloaded**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.unloaded) o [**Page. NavigatedFrom**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) son posibles desencadenadores de eventos que tienen posiciones adecuadas en la administración de estado y la duración de los objetos, de modo que se pueden usar para quitar controladores de otros eventos.

Por ejemplo, puede quitar un controlador de eventos denominado **textBlock1\_PointerEntered** del objeto de destino **textBlock1** mediante este código.

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

También puede quitar controladores para los casos en los que el evento se agregó a través de un atributo XAML, lo que significa que el controlador se agregó en el código generado. Esto es más fácil si proporcionó un valor de **nombre** para el elemento al que se adjuntó el controlador, ya que proporciona una referencia de objeto para el código más adelante. sin embargo, también podría recorrer el árbol de objetos para buscar la referencia de objeto necesaria en los casos en los que el objeto no tiene **nombre**.

Si necesita quitar un controlador de eventos en C++/CX, necesitará un token de registro, que debería haber recibido del valor devuelto del registro del controlador de eventos`+=`. Esto se debe a que el valor que se usa para el lado derecho de la `-=` anulación del registro en la C++sintaxis de/CX es el token, no el nombre del método. Para C++/CX, no se pueden quitar los controladores que se agregaron como atributo XAML porque C++el código generado por/CX no guarda un token.

## <a name="routed-events"></a>Eventos enrutados

El Windows Runtime con C#, Microsoft Visual Basic o C++/CX admite el concepto de un evento enrutado para un conjunto de eventos que están presentes en la mayoría de los elementos de la interfaz de usuario. Estos eventos son para escenarios de entrada y de interacción del usuario, y se implementan en la clase base [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) . Esta es una lista de eventos de entrada que son eventos enrutados:

- [**BringIntoViewRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**CharacterReceived**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**ContextCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**ContextRequested**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Omisiones**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.drop)
- [**DropCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**GettingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**Primero**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Esa**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup)
- [**LosingFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**NoFocusCandidateFound**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeydown.md)
- [**PreviewKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.previewkeyup.md)
- [**PointerWheelChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Derivados**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.tapped)

Un evento enrutado es un evento que se puede pasar (*enrutado*) de un objeto secundario a cada uno de sus objetos primarios sucesivos en un árbol de objetos. La estructura XAML de la interfaz de usuario se aproxima a este árbol, donde la raíz de ese árbol es el elemento raíz en XAML. El árbol de objetos verdadero puede variar ligeramente desde el anidamiento del elemento XAML, porque el árbol de objetos no incluye características del lenguaje XAML como etiquetas de elemento de propiedad. Puede concebir el evento enrutado como si se *propague* desde cualquier elemento secundario del elemento de objeto XAML que desencadene el evento, hacia el elemento de objeto primario que lo contiene. El evento y sus datos de evento se pueden controlar en varios objetos a lo largo de la ruta del evento. Si ningún elemento tiene controladores, la ruta puede continuar hasta que se alcanza el elemento raíz.

Si conoce tecnologías web como HTML dinámico (DHTML) o HTML5, es posible que ya esté familiarizado con el concepto de evento de *propagación* .

Cuando un evento enrutado se propaga a través de su ruta de eventos, todos los controladores de eventos adjuntos tienen acceso a una instancia compartida de datos de evento. Por consiguiente, si un controlador puede escribir en cualquiera de los datos de evento, cualquier cambio realizado en los datos de evento se pasará al controlador siguiente y es posible que ya no represente los datos de eventos originales del evento. Cuando un evento tiene un comportamiento de evento enrutado, la documentación de referencia incluirá comentarios u otras anotaciones sobre el comportamiento enrutado.

### <a name="the-originalsource-property-of-routedeventargs"></a>Propiedad **OriginalSource** de **RoutedEventArgs**

Cuando un evento propaga una ruta de evento, el *remitente* ya no es el mismo objeto que el objeto que provoca el evento. En su lugar, *Sender* es el objeto en el que se adjunta el controlador que se está invocando.

En algunos casos, el *remitente* no es interesante y, en su lugar, está interesado en información como cuál de los posibles objetos secundarios en los que se encuentra el puntero cuando se activa un evento de puntero o qué objeto en una interfaz de usuario más grande tiene el foco cuando un usuario presiona una tecla del teclado. En estos casos, puede usar el valor de la propiedad [**OriginalSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.routedeventargs.originalsource) . En todos los puntos de la ruta, **OriginalSource** informa del objeto original que desencadenó el evento, en lugar del objeto al que está asociado el controlador. Sin embargo, para los eventos de entrada de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) , ese objeto original suele ser un objeto que no está visible inmediatamente en el XAML de definición de la interfaz de usuario de nivel de página. En su lugar, ese objeto de origen original podría ser una parte de plantilla de un control. Por ejemplo, si el usuario mantiene el puntero sobre el borde de un [**botón**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button), para la mayoría de los eventos de puntero, el **OriginalSource** es una parte de la plantilla de [**borde**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) en la [**plantilla**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template), no el propio **botón** .

**Sugerencia**  la propagación de eventos de entrada es especialmente útil si va a crear un control con plantilla. Cualquier control que tenga una plantilla puede tener una nueva plantilla aplicada por su consumidor. El consumidor que está intentando volver a crear una plantilla de trabajo podría eliminar accidentalmente algún control de eventos declarado en la plantilla predeterminada. Todavía puede proporcionar control de eventos de nivel de control adjuntando controladores como parte de la invalidación de [**OnApplyTemplate**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate) en la definición de clase. Después, puede detectar los eventos de entrada que se propagan hasta la raíz del control en la creación de instancias.

### <a name="the-handled-property"></a>Propiedad **controlada**

Varias clases de datos de eventos para eventos enrutados específicos contienen una propiedad denominada **Handled**. Para obtener ejemplos, vea [**PointerRoutedEventArgs. Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled), [**KeyRoutedEventArgs. Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled), [**DragEventArgs. Handled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.drageventargs.handled). En todos los casos, **Handled** es una propiedad booleana configurable.

El establecimiento de la propiedad **Handled** en **true** influye en el comportamiento del sistema de eventos. Cuando **Handled** es **true**, el enrutamiento se detiene para la mayoría de los controladores de eventos; el evento no continúa a lo largo de la ruta para notificar a otros controladores adjuntos de ese caso de evento concreto. Lo que significa "controlado" significa en el contexto del evento y cómo responde su aplicación a usted. Básicamente, **Handled** es un protocolo simple que permite al código de la aplicación indicar que no es necesario que se produzca una aparición de un evento en ningún contenedor, la lógica de la aplicación se ocupa de las necesidades realizadas. A la inversa, sin embargo, debe tener cuidado de no controlar eventos que probablemente deberían propagarse para que los comportamientos integrados del sistema o control puedan actuar. Por ejemplo, el control de eventos de bajo nivel dentro de las partes o los elementos de un control de selección puede ser perjudicial. Es posible que el control de selección busque eventos de entrada para saber que la selección debería cambiar.

No todos los eventos enrutados pueden cancelar una ruta de este modo y puede indicarle que no tendrán una propiedad **controlada** . Por ejemplo, [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) y [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus) realizan burbujas, pero siempre propagan todo el recorrido a la raíz, y sus clases de datos de evento no tienen una propiedad **controlada** que pueda influir en ese comportamiento.

##  <a name="input-event-handlers-in-controls"></a>Controladores de eventos de entrada en controles

Los controles de Windows Runtime específicos a veces usan el concepto **controlado** para los eventos de entrada internamente. Esto puede hacer que parezca que un evento de entrada nunca se produce, porque el código de usuario no puede controlarlo. Por ejemplo, la clase [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) incluye lógica que controla deliberadamente el evento de entrada general [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed). Esto se debe a que los botones activan un evento de [**clic**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) que se inicia mediante la entrada de puntero presionado, así como con otros modos de entrada, como el control de teclas como la tecla entrar, que puede invocar el botón cuando está centrado. En lo que respecta al diseño de clase de **Button**, el evento de entrada sin formato se controla conceptualmente y los consumidores de clase, como el código de usuario, pueden interactuar con el evento **click** pertinente del control. Los temas de las clases de control específicas de la referencia de API de Windows Runtime suelen tener en cuenta el comportamiento de control de eventos que la clase implementa. En algunos casos, puede cambiar el comportamiento invalidando **en métodos de**_evento_ . Por ejemplo, puede cambiar el modo en que la clase derivada de [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) reacciona a la entrada de clave mediante la invalidación de [**control. onkeydown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown).

##  <a name="registering-handlers-for-already-handled-routed-events"></a>Registrar Controladores para eventos enrutados ya controlados

Antes dijimos que el valor **de establecido en** **true** impide que se llame a la mayoría de los controladores. Pero el método [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) proporciona una técnica en la que se puede adjuntar un controlador que siempre se invoca para la ruta, incluso si algún otro controlador anterior de la ruta **tiene establecido en** **true** en los datos de evento compartidos. Esta técnica es útil si un control que está usando ha controlado el evento en su composición interna o para la lógica específica del control. pero todavía desea responder a él desde una instancia del control o la interfaz de usuario de la aplicación. Pero use esta técnica con precaución, ya que puede contradicción del propósito de **controlado** y, posiblemente, interrumpir las interacciones de un control.

Solo los eventos enrutados que tienen un identificador de evento enrutado correspondiente pueden utilizar la técnica de control de eventos [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) , ya que el identificador es una entrada necesaria del método **AddHandler** . Consulte la documentación de referencia de [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler) para obtener una lista de eventos que tienen los identificadores de eventos enrutados disponibles. En su mayor parte, esta es la misma lista de eventos enrutados que le mostramos anteriormente. La excepción es que los últimos dos de la lista: [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus) y [**LostFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.lostfocus) no tienen un identificador de evento enrutado, por lo que no puede usar **AddHandler** para ellos.

## <a name="routed-events-outside-the-visual-tree"></a>Eventos enrutados fuera del árbol visual

Algunos objetos participan en una relación con el árbol visual principal que es conceptualmente como tener una superposición sobre los objetos visuales principales. Estos objetos no forman parte de las relaciones de elementos primarios y secundarios habituales que conectan todos los elementos de árbol a la raíz visual. Este es el caso de cualquier [**ventana emergente**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) o [**información sobre herramientas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToolTip)mostrada. Si desea controlar los eventos enrutados desde una **ventana emergente** o una **información sobre herramientas**, coloque los controladores en elementos de interfaz de usuario específicos que estén dentro de la **ventana emergente** o la **información** sobre herramientas, y no los propios elementos **emergentes** o de **información sobre herramientas** . No confíe en el enrutamiento dentro de cualquier composición que se realice para el contenido **emergente** o de **información sobre herramientas** . Esto se debe a que el enrutamiento de eventos para los eventos enrutados solo funciona a lo largo del árbol visual principal. Un **elemento emergente** o una **información sobre herramientas** no se considera un elemento primario de los elementos de la interfaz de usuario de la subsidiaria y nunca recibe el evento enrutado, aunque intente usar algo como el fondo predeterminado del **elemento emergente** como el área de captura de los eventos de entrada.

## <a name="hit-testing-and-input-events"></a>Pruebas de posicionamiento y eventos de entrada

Determinar si y dónde de la interfaz de usuario es visible un elemento para la entrada de mouse, táctil y lápiz se denomina *prueba de posicionamiento*. En el caso de las acciones táctiles y también para los eventos de manipulación o específicos de la interacción que son consecuencia de una acción táctil, un elemento debe estar visible para el origen del evento y desencadenar el evento asociado a la acción. De lo contrario, la acción pasa a través del elemento a los elementos subyacentes o elementos primarios del árbol visual que podrían interactuar con esa entrada. Hay varios factores que afectan a la prueba de posicionamiento, pero puede determinar si un determinado elemento puede desencadenar eventos de entrada comprobando su propiedad [**IsHitTestVisible**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.ishittestvisible) . Esta propiedad solo devuelve **true** si el elemento cumple estos criterios:

- El valor de la propiedad [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) del elemento es [**visible**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Visibility).
- El valor de la propiedad **background** o **Fill** del elemento no es **null**. Un valor de [**pincel**](/uwp/api/Windows.UI.Xaml.Media.Brush) **nulo** da como resultado una transparencia y una invisibilidad de la prueba de posicionamiento. (Para hacer que un elemento sea transparente, pero también para la prueba de posicionamiento, use un pincel [**transparente**](https://docs.microsoft.com/uwp/api/windows.ui.colors.transparent) en lugar de **null**).

**Nota:** el  **fondo** y el **relleno** no se definen mediante [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)y, en su lugar, se definen mediante distintas clases derivadas, como [**control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) y [**forma**](/uwp/api/Windows.UI.Xaml.Shapes.Shape). Sin embargo, las implicaciones de los pinceles que se usan para las propiedades de primer plano y de fondo son las mismas para las pruebas de posicionamiento y los eventos de entrada, con independencia de qué subclase implemente las propiedades.

- Si el elemento es un control, el valor de la propiedad [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) debe ser **true**.
- El elemento debe tener dimensiones reales en el diseño. Un elemento donde [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) y [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) son 0 no activarán los eventos de entrada.

Algunos controles tienen reglas especiales para la prueba de posicionamiento. Por ejemplo, [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) no tiene ninguna propiedad **background** , pero todavía se puede probar en toda la región de sus dimensiones. Los controles [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) y [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement) se pueden probar a través de sus dimensiones de rectángulo definidas, independientemente del contenido transparente como el canal alfa del archivo de origen multimedia que se muestra. Los controles [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) tienen un comportamiento especial de la prueba de posicionamiento porque la entrada se puede controlar mediante los eventos de script de desencadenamiento y HTML hospedados.

La mayoría de las clases y el [**borde**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) del [**Panel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel) no se pueden probar en su propio fondo, pero todavía pueden controlar los eventos de entrada del usuario que se enrutan desde los elementos que contienen.

Puede determinar qué elementos se encuentran en la misma posición que un evento de entrada de usuario, independientemente de si los elementos son comprobables. Para ello, llame al método [**FindElementsInHostCoordinates**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates) . Como su nombre implica, este método busca los elementos en una ubicación relativa a un elemento host especificado. Sin embargo, las transformaciones y los cambios de diseño aplicados pueden ajustar el sistema de coordenadas relativo de un elemento y, por tanto, afectar a los elementos que se encuentran en una ubicación determinada.

## <a name="commanding"></a>Comandos

Un pequeño número de elementos de la interfaz de usuario admite el *comando*. Los comandos usan eventos enrutados relacionados con la entrada en su implementación subyacente y permiten el procesamiento de la entrada de la interfaz de usuario relacionada (una determinada acción de puntero, una clave de acelerador concreta) al invocar un solo controlador de comandos. Si el comando está disponible para un elemento de la interfaz de usuario, considere la posibilidad de usar las API de comandos en lugar de los eventos de entrada discretos. Normalmente, se utiliza una referencia de **enlace** en las propiedades de una clase que define el modelo de vista para los datos. Las propiedades contienen comandos con nombre que implementan el patrón de comandos **ICommand** específico del lenguaje. Para obtener más información, vea [**ButtonBase. Command**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).

## <a name="custom-events-in-the-windows-runtime"></a>Eventos personalizados en el Windows Runtime

A efectos de la definición de eventos personalizados, el modo en que se agrega el evento y lo que significa para el diseño de clase depende en gran medida del lenguaje de programación que se use.

- Para C# y Visual Basic, está definiendo un evento CLR. Puede usar el patrón de eventos estándar de .NET, siempre y cuando no use descriptores de acceso personalizados (**agregar**/**quitar**). Sugerencias adicionales:
    - En el caso del controlador de eventos, se recomienda usar [**System. EventHandler<TEventArgs>** ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1) porque tiene una traducción integrada al Windows Runtime EventHandler del delegado de eventos genéricos [ **<T>** ](https://docs.microsoft.com/uwp/api/windows.foundation.eventhandler).
    - No base la clase de datos de evento en [**System. EventArgs**](https://docs.microsoft.com/dotnet/api/system.eventargs) porque no se convierte en el Windows Runtime. Usar una clase de datos de evento existente o ninguna clase base.
    - Si usa descriptores de acceso personalizados, consulte [eventos personalizados y descriptores de acceso de eventos en Windows Runtime componentes](https://docs.microsoft.com/previous-versions/windows/apps/hh972883(v=vs.140)).
    - Si no está claro cuál es el patrón de eventos estándar de .NET, consulte [definir eventos para clases de Silverlight personalizadas](https://docs.microsoft.com/previous-versions/windows/). Esto está escrito para Microsoft Silverlight, pero sigue siendo una buena suma del código y los conceptos del patrón de eventos estándar de .NET.
- Para C++/CX, consulte [eventos (C++/CX)](https://docs.microsoft.com/cpp/cppcx/events-c-cx).
    - Use referencias con nombre incluso para sus propios usos de eventos personalizados. No use lambda para eventos personalizados, sino que puede crear una referencia circular.

No se puede declarar un evento enrutado personalizado para Windows Runtime; los eventos enrutados se limitan al conjunto que procede de la Windows Runtime.

La definición de un evento personalizado se realiza normalmente como parte del ejercicio de definir un control personalizado. Es un patrón común tener una propiedad de dependencia que tiene una devolución de llamada de propiedad modificada, así como definir un evento personalizado desencadenado por la devolución de llamada de la propiedad de dependencia en algunos o todos los casos. Los consumidores del control no tienen acceso a la devolución de llamada de propiedad modificada que definió, pero tener un evento de notificación disponible es lo mejor. Para obtener más información, consulte [propiedades de dependencia personalizadas](custom-dependency-properties.md).

## <a name="related-topics"></a>Temas relacionados

* [Información general sobre XAML](xaml-overview.md)
* [Inicio rápido: entrada táctil](https://docs.microsoft.com/previous-versions/windows/apps/hh465387(v=win.10))
* [Interacciones de teclado](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)
* [Eventos y delegados de .NET](https://go.microsoft.com/fwlink/p/?linkid=214364)
* [Crear componentes de Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.addhandler)
