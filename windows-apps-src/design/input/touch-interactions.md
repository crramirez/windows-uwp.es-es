---
description: Cree aplicaciones de Windows con experiencias de interacción de usuario intuitivas y distintivas que estén optimizadas para Touch, pero que sean funcionalmente coherentes en todos los dispositivos de entrada.
title: Interacciones táctiles
ms.assetid: DA6EBC88-EB18-4418-A98A-457EA1DEA88A
label: Touch interactions
template: detail.hbs
keywords: táctil, función táctil,puntero,entrada,interacción del usuario
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f173c90d43b7be795b8a87fe14dd3de9d6284da1
ms.sourcegitcommit: da44cb95946440cd06ff36254d42ecefcdd87ce2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93063017"
---
# <a name="touch-interactions"></a>Interacciones táctiles


Diseña tu aplicación teniendo en mente que el principal método de entrada de los usuarios será táctil. Si usas los controles de UWP, la compatibilidad con el mouse, el panel táctil y el lápiz o la pluma no requiere programación adicional, ya que las aplicaciones para UWP la ofrecen de forma gratuita.

Pero ten presente que una interfaz de usuario optimizada para entrada táctil no es siempre mejor que una interfaz de usuario tradicional. Ambas ofrecen ventajas y desventajas exclusivas de la tecnología y la aplicación en cuestión. En el paso a una interfaz de usuario táctil, es importante comprender las diferencias principales entre el toque, el panel táctil, el lápiz/lápiz, el mouse y la entrada del teclado.

> **API importantes** : [**Windows. UI. Xaml. Input**](/uwp/api/Windows.UI.Xaml.Input), [**Windows. UI. Core**](/uwp/api/Windows.UI.Core), [**Windows. Devices. Input**](/uwp/api/Windows.Devices.Input)


Muchos dispositivos tienen pantallas multitoque que admiten el uso de uno o más dedos (o contactos táctiles) como entrada. Los contactos táctiles y su movimiento, se interpretan como gestos táctiles y manipulaciones para admitir distintas interacciones del usuario.

La aplicación de Windows incluye una serie de mecanismos distintos para controlar la entrada táctil, lo que le permite crear una experiencia envolvente que los usuarios pueden explorar con confianza. Aquí se describen los aspectos básicos del uso de la entrada táctil en una aplicación de Windows.

Las interacciones táctiles requieren tres cosas:

- Una pantalla táctil.
- El contacto directo (o la proximidad, si la pantalla dispone de sensores de proximidad y admite esta clase de detección) de uno o más dedos sobre dicha pantalla.
- Movimiento de los contactos táctiles (o la falta de movimiento, según un umbral de tiempo).

Los datos de entrada proporcionados por el sensor táctil pueden ser:

- Interpretados como un gesto físico para la manipulación directa de uno o más elementos de la interfaz de usuario (como el movimiento panorámico, girar, cambiar el tamaño o mover). Por el contrario, interactuar con un elemento a través de su ventana de propiedades, otro cuadro de diálogo u otra prestación de la interfaz de usuario se considera una manipulación indirecta.
- Reconocidos como un método de entrada alternativo, como un mouse o un lápiz.
- Usados para complementar o modificar aspectos de otros métodos de entrada, como difuminar un trazo de lápiz dibujado con un lápiz.

Normalmente, la entrada táctil implica la manipulación directa de un elemento en la pantalla. El elemento responde inmediatamente a cualquier contacto táctil dentro de su área de prueba de posicionamiento y reacciona correctamente a cualquier movimiento posterior de los contactos táctiles, incluso la eliminación.

Las interacciones y los gestos táctiles personalizados deben diseñarse cuidadosamente. Deben ser intuitivos, dinámicos y reconocibles, así como permitir a los usuarios explorar la aplicación con confianza.

Asegúrate de que la funcionalidad de la aplicación se exponga de manera coherente entre todos los tipos de dispositivos de entrada admitidos. Si es necesario, emplea algún método de entrada indirecto, como la escritura de texto en las interacciones con teclado o prestaciones de interfaz de usuario para mouse y lápiz.

Recuerda que los dispositivos de entrada tradicionales (como el mouse y el teclado) resultan familiares y muchos usuarios los prefieren. Pueden ofrecer una velocidad, precisión y respuesta que el control táctil no puede alcanzar.

Al proporcionar experiencias de interacción únicas y distintivas para todos los dispositivos de entrada, darás cabida a la más amplia variedad de capacidades y preferencias, llegarás hasta un público más amplio y tu aplicación atraerá a más clientes.

## <a name="compare-touch-interaction-requirements"></a>Compara los requisitos de la interacción táctil

En la tabla siguiente se muestran algunas de las diferencias entre los dispositivos de entrada que se deben tener en cuenta al diseñar aplicaciones de Windows con optimización táctil.

<table>
<tbody><tr><th>Factor</th><th>Interacciones táctiles</th><th>Interacciones con mouse, teclado, pluma/lápiz</th><th>Panel táctil</th></tr>
<tr><td rowspan="3">Precision</td><td>El área de contacto de la punta de un dedo es mayor que la de una sola coordenada x-y, lo que aumenta las probabilidades de activación no intencional de comandos.</td><td>El mouse y la pluma/lápiz suministran una coordenada x-y precisa.</td><td>Es igual al mouse.</td></tr>
<tr><td>La forma del área de contacto cambia durante el movimiento.  </td><td>Los movimientos de mouse y los trazos de la pluma/lápiz suministran coordenadas x-y precisas. El foco del teclado es explícito.</td><td>Es igual al mouse.</td></tr>
<tr><td>No hay cursor de mouse para ayudar con la selección del destino.</td><td>El cursor del mouse, el cursor de la pluma/lápiz y el foco del teclado ayudan a seleccionar el destino.</td><td>Es igual al mouse.</td></tr>
<tr><td rowspan="3">Anatomía humana</td><td>Los movimientos de los dedos no son precisos, ya que es difícil realizar un movimiento en línea recta con uno o más dedos. Esto se debe a la curvatura de las articulaciones de la mano y a la cantidad de articulaciones involucradas en el movimiento.</td><td>Es más fácil ejecutar un movimiento en línea recta con mouse o pluma/lápiz porque la mano que los controla recorre una distancia física menor que el cursor en la pantalla.</td><td>Es igual al mouse.</td></tr>
<tr><td>Puede ser difícil llegar a algunas áreas de la superficie táctil de un dispositivo de pantalla debido a la posición de los dedos y la sujeción del dispositivo por parte del usuario.</td><td>El mouse y la pluma o el lápiz pueden llegar a cualquier parte de la pantalla. El teclado permite acceder a cualquier control con el orden de tabulación. </td><td>La posición de los dedos y la empuñadura pueden ser un problema.</td></tr>
<tr><td>Puede ocurrir que los objetos queden ocultos por la punta de uno o más dedos o la mano del usuario. Esto se conoce como oclusión.</td><td>Los dispositivos de entrada indirecta no provocan oclusión.</td><td>Es igual al mouse.</td></tr>
<tr><td>Estado de objeto</td><td>La interacción táctil emplea un modelo de dos estados: la superficie táctil de un dispositivo de pantalla se toca (activado) o no se toca (desactivado). No existe un estado de movimiento que pueda desencadenar una respuesta visual adicional.</td><td>
<p>El mouse, la pluma/lápiz y el teclado exponen un modelo de tres estados: arriba (desactivado), abajo (activado) y movimiento (foco).</p>
<p>El estado de movimiento permite que el usuario explore y aprenda mediante informaciones sobre herramientas asociadas con elementos de la interfaz de usuario. Los efectos de movimiento y foco pueden transmitir qué objetos son interactivos y ayudar, además, a seleccionar el destino. 
</p>
</td><td>Es igual al mouse.</td></tr>
<tr><td rowspan="2">Interacción enriquecida</td><td>Admite multitoque: múltiples puntos de entrada (puntas de los dedos) en una superficie táctil.</td><td>Admite un punto único de entrada.</td><td>Es igual a la entrada táctil.</td></tr>
<tr><td>Admite manipulación directa de objetos por medio de gestos como pulsar, arrastrar, deslizar, reducir y girar.</td><td>No admite manipulación directa porque el mouse, la pluma o el lápiz y el teclado son dispositivos de entrada indirecta.</td><td>Es igual al mouse.</td></tr>
</tbody></table>

> [!NOTE]
> La entrada indirecta tiene la ventaja de más de 25 años de perfeccionamiento. Algunas funciones, como la información sobre herramientas desencadenada al mantener el mouse sobre un elemento, se diseñaron para explorar la interfaz de usuario específicamente con entrada de panel táctil, mouse, pluma o lápiz y teclado. Funciones de UI como esta se han rediseñado para lograr la experiencia completa que reporta la entrada táctil, sin poner en riesgo la experiencia de usuario de estos otros dispositivos.

 

## <a name="use-touch-feedback"></a>Usar la información táctil

Los comentarios visuales adecuados durante las interacciones con la aplicación ayudan a los usuarios a reconocer, aprender y adaptarse a cómo se interpretan sus interacciones tanto en la aplicación como en la plataforma Windows. La información visual puede indicar interacciones satisfactorias y el estado del sistema relé, mejorar la sensación de control, reducir errores, ayudar a los usuarios a entender el sistema y el dispositivo de entrada, y alentar la interacción.

La información visual es esencial cuando el usuario usa la entrada táctil para llevar a cabo actividades que requieren exactitud y precisión en lo que respecta a ubicación. Muestra información siempre que se detecte entrada táctil para ayudar al usuario a entender cualquier regla personalizada de selección de destinos que defina la aplicación y los controles correspondientes.


## <a name="targeting"></a>Establecer destinos

La selección de destinos se optimiza mediante:

- Tamaños de destinos táctiles

    Unas directrices claras sobre el tamaño garantizan que las aplicaciones ofrecerán una interfaz de usuario cómoda que contenga objetos y controles cuyos destinos sean fáciles y seguros.

- Geometría de contacto

    Toda el área de contacto del dedo determina el objeto de destino más probable.

- Scrubbing

    La selección de destinos de los elementos dentro de un grupo se puede realizar fácilmente si se arrastra el dedo entre ellos (por ejemplo, los botones de radio). El elemento actual se activa cuando se libera el contacto táctil.

- Inclinación de un lado a otro

    Los elementos que se encuentra muy cerca unos de otros (por ejemplo, hipervínculos) se pueden seleccionar de nuevo presionando el dedo e inclinándolo de un lado a otro sobre los elementos, sin deslizarlo. Debido a la oclusión, el elemento actual se identifica mediante la información sobre herramientas o la barra de estado y se activa al liberar el contacto táctil.

## <a name="accuracy"></a>Precisión

Diseña teniendo en cuenta las interacciones descuidadas mediante:

- Puntos de acoplamiento que permitan detenerse en las ubicaciones deseadas con mayor facilidad cuando los usuarios interactúan con el contenido.
- "Raíles" direccional que pueden ayudar con el movimiento panorámico vertical u horizontal, incluso cuando la mano se mueve en un arco ligero. Para obtener más información, vea [instrucciones para la panorámica](guidelines-for-panning.md).

## <a name="occlusion"></a>Oclusión

La oclusión de dedos y manos se evita mediante:

- El tamaño y la posición de la interfaz de usuario

    Crea los elementos de la interfaz de usuario lo suficientemente grandes, para que no se cubran por completo con el área de contacto del dedo.

    Coloca los menús y las ventanas emergentes sobre el área de contacto, siempre que sea posible.

- Información sobre herramientas

    Muestra información sobre herramientas cuando un usuario mantiene el contacto sobre un objeto con el dedo. Sirve para describir las funciones de los objetos. El usuario puede retirar el dedo del objeto para evitar que se invoque la información sobre herramientas.

    Para objetos pequeños, desplaza la información de herramientas para que no quede cubierta por el área de contacto del dedo. Esto resulta útil para la selección de destinos.

- Controladores para mayor precisión

    Cuando necesites una mayor precisión (por ejemplo, para la selección de texto), ofrece controladores de selección que se desplacen para mejorar la precisión. Para obtener más información, vea [directrices para seleccionar texto e imágenes (aplicaciones Windows Runtime)](guidelines-for-textselection.md).

## <a name="timing"></a>Control de tiempo

Evita los cambios de modo cronometrado en favor de la manipulación directa. Esta simula el control físico, directo y en tiempo real de un objeto. El objeto responde a medida que se mueven los dedos.

Una interacción temporal, en cambio, tiene lugar después de la interacción táctil. Para determinar qué comando se ejecutará, las interacciones temporales suelen depender de umbrales invisibles, como el tiempo, la distancia o la velocidad. Las interacciones temporales no ofrecen una respuesta visual hasta que el sistema ejecuta la acción.

La manipulación directa ofrece una serie de ventajas sobre las interacciones temporales:

- La información visual instantánea durante las interacciones hace que los usuarios se sientan más comprometidos, seguros de si mismos y con el control.
- Las manipulaciones directas hacen que sea más seguro explorar un sistema porque son reversibles: los usuarios pueden deshacer fácilmente sus acciones de manera lógica e intuitiva.
- Las interacciones que afectan directamente a los objetos y mimetizan las interacciones de la vida real son más intuitivas y fáciles de detectar y recordar. No dependen de interacciones oscuras o abstractas.
- Las interacciones temporales pueden ser difíciles de realizar, ya que los usuarios deben alcanzar umbrales arbitrarios e invisibles.

Además, te recomendamos lo siguiente:

- Las manipulaciones no deben distinguirse por el número de dedos usados.
- Las interacciones deben admitir manipulaciones compuestas. Por ejemplo, alejar para ampliar mientras se arrastran los dedos para el movimiento panorámico.
- Las interacciones no deben distinguirse temporalmente. La misma interacción debe tener el mismo resultado, independientemente del tiempo que se haya tardado en realizarla. Las activaciones temporales introducen retrasos obligatorios para los usuarios y reducen la naturaleza envolvente de la manipulación directa, así como la percepción de la respuesta del sistema.

    > [!NOTE]
    > Una excepción a esto es el lugar en el que se usan interacciones de tiempo específicas para ayudar en el aprendizaje y la exploración (por ejemplo, mantener presionado).

- Las descripciones y las indicaciones visuales adecuadas tienen un gran efecto sobre el uso de las interacciones avanzadas.


## <a name="app-views"></a>Vistas de aplicación


Retoca la experiencia de interacción del usuario mediante la configuración de zoom y movimiento panorámico/desplazamiento de las vistas de tu aplicación. Una vista de aplicación determina cómo un usuario accede a tu aplicación y su contenido, y los manipula. Las vistas también proporcionan comportamientos, tales como inercia, puntos de acoplamiento y "devolución" de límite de contenido.

La configuración del movimiento panorámico y del desplazamiento del control [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) determina la forma en que los usuarios pueden navegar dentro de una vista única cuando el contenido de la vista no cabe dentro de la ventanilla. Una vista única puede ser, por ejemplo, una página de una revista o libro, la estructura de carpetas de un equipo, una biblioteca de documentos o un álbum de fotos.

La configuración de zoom se aplica tanto al zoom óptico (compatible con el control [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)) como al control [**Semantic Zoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom). El zoom semántico es una técnica optimizada para entrada táctil que permite presentar grandes conjuntos de contenido o datos relacionados dentro de una vista única y navegar por ellos. Funciona mediante dos modos distintos de clasificación (o niveles de zoom). Esto es similar al movimiento panorámico y al desplazamiento en una vista única. El movimiento panorámico y el desplazamiento pueden usarse junto con el zoom semántico.

Usa las vistas de aplicación y los eventos para modificar los comportamientos de movimiento panorámico/desplazamiento y zoom. Esto puede proporcionar una experiencia de interacción más uniforme que la que permite el control de los eventos de puntero y de gestos.

Para obtener más información sobre las vistas de aplicaciones, consulte [controles, diseños y texto](../basics/index.md).

## <a name="custom-touch-interactions"></a>Interacciones táctiles personalizadas


Si implementas tu propia compatibilidad con la interacción, ten presente que los usuarios esperan una experiencia intuitiva que implique la interacción directa con los elementos de la interfaz de usuario de tu aplicación. Te recomendamos que modeles tus interacciones personalizadas en las bibliotecas de control de plataforma para que todo sea coherente y reconocible. Los controles de estas bibliotecas proporcionan una experiencia de interacción de usuario completa, con interacciones estándar, efectos físicos animados, comentarios visuales y accesibilidad. Crea interacciones personalizadas solamente si existe un requisito claro y bien definido, y si no hay ninguna interacción básica que admita tu escenario.

Para proporcionar compatibilidad táctil personalizada, puedes controlar diversos eventos [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement). Estos eventos se agrupan en tres niveles de abstracción.

- Los eventos de gestos estáticos se desencadenan una vez completada una interacción. Los eventos de gesto incluyen los de [**puntear**](/uwp/api/windows.ui.xaml.uielement.tapped), [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped), [**RightTapped**](/uwp/api/windows.ui.xaml.uielement.righttapped)y [**Holding**](/uwp/api/windows.ui.xaml.uielement.holding).

    Puede deshabilitar los eventos de gestos en elementos concretos estableciendo [**IsTapEnabled**](/uwp/api/windows.ui.xaml.uielement.istapenabled), [**IsDoubleTapEnabled**](/uwp/api/windows.ui.xaml.uielement.isdoubletapenabled), [**IsRightTapEnabled**](/uwp/api/windows.ui.xaml.uielement.isrighttapenabled)y [**IsHoldingEnabled**](/uwp/api/windows.ui.xaml.uielement.isholdingenabled) en **false** .

- Los eventos de puntero como [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) y [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved) proporcionan detalles de bajo nivel para cada contacto táctil, incluido el movimiento del puntero y la capacidad de distinguir los eventos de presionar y soltar.

    Un puntero es un tipo de entrada genérico con un mecanismo de eventos unificado. Expone información básica, como la posición de la pantalla, en el origen de entrada activo, que puede ser entrada táctil, panel táctil, mouse o lápiz.

- Los eventos de gestos de manipulación, tal como [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted), indican una interacción continua del usuario. Estos eventos comienzan a desencadenarse cuando el usuario toca un elemento y continúan hasta que el usuario levanta el dedo (o los dedos) o se cancela la manipulación.

    Entre los eventos de manipulación se incluyen las interacciones multitáctiles, tales como el zoom, el movimiento panorámico o la rotación, y las interacciones que usan datos de velocidad e inercia, como arrastrar. La información que proporcionan los eventos de manipulación no identifica la forma de la interacción que se hizo, sino que incluye datos sobre el contacto táctil, tales como posición, diferencia de traslación y velocidad. Puedes usar estos datos táctiles para determinar el tipo de interacción que se debería realizar.

Este es el conjunto básico de gestos táctiles que admite la UWP.

| Nombre           | Tipo                 | Description                                                                            |
|----------------|----------------------|----------------------------------------------------------------------------------------|
| Pulsar            | Gesto estático       | Un dedo toca la pantalla y se levanta.                                            |
| Pulsar y sostener | Gesto estático       | Un dedo toca la pantalla y se queda en el lugar.                                      |
| Diapositiva          | Gesto de manipulación | Uno o más dedos tocan la pantalla y se mueven en la misma dirección.                   |
| Deslizar rápidamente          | Gesto de manipulación | Uno o más dedos tocan la pantalla y se mueven una corta distancia en la misma dirección.  |
| Girar           | Gesto de manipulación | Dos o más dedos tocan la pantalla y se mueven describiendo un arco en el sentido de las agujas del reloj o en el sentido contrario a las agujas del reloj. |
| Reducir          | Gesto de manipulación | Dos o más dedos tocan la pantalla y se acercan entre sí.                         |
| Stretch        | Gesto de manipulación | Dos o más dedos tocan la pantalla y se alejan entre sí.                           |

 

<!-- mijacobs: Removing for now. We don't have a real page to link to yet. 
For more info about gestures, manipulations, and interactions, see [Custom user interactions](custom-user-input-portal.md).
-->

## <a name="gesture-events"></a>Eventos de gestos


Para obtener detalles sobre los controles individuales, consulta [Lista de controles](../controls-and-patterns/index.md).

## <a name="pointer-events"></a>Eventos de puntero


Los eventos de puntero los genera una gran variedad de orígenes de entrada activos como, por ejemplo, la entrada táctil, el panel táctil, el lápiz y el mouse (reemplazan los eventos tradicionales del mouse).

Los eventos de puntero se basan en un único punto de entrada (dedo, punta del lápiz, cursor del mouse) y no admiten interacciones basadas en velocidad.

Esta es una lista de eventos de puntero y su argumento de evento relacionado.

| Evento o clase                                                       | Description                                                   |
|----------------------------------------------------------------------|---------------------------------------------------------------|
| [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)             | Se produce cuando un dedo toca la pantalla.               |
| [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)           | Se produce cuando ese mismo contacto táctil se levanta.                |
| [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)                 | Se produce cuando el puntero se arrastra por la pantalla.         |
| [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)             | Se produce cuando un puntero entra en el área de prueba de posicionamiento de un elemento. |
| [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)               | Se produce cuando un puntero sale del área de prueba de posicionamiento de un elemento.  |
| [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)           | Se produce cuando un contacto táctil se pierde de manera inesperada.               |
| [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)     | Se produce cuando otro elemento hace una captura del puntero.    |
| [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)   | Se produce cuando cambia el valor Delta de una rueda del mouse y cuando se gira el Touchpad.         |
| [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) | Proporciona datos para todos los eventos de puntero.                         |

 

En el ejemplo siguiente se muestra cómo usar los eventos [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) y [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) para controlar una interacción de presionar en un objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle).

En primer lugar, se crea un [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) denominado `touchRectangle` en el lenguaje XAML.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
           Height="100" Width="200" Fill="Blue" />
</Grid>
```
Luego, se especifican los agentes de escucha para los eventos [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) y [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited).

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Pointer event listeners.
    touchRectangle->PointerPressed += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerPressed);
    touchRectangle->PointerReleased += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerReleased);
    touchRectangle->PointerExited += ref new PointerEventHandler(this, &MainPage::touchRectangle_PointerExited);
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Pointer event listeners.
    touchRectangle.PointerPressed += touchRectangle_PointerPressed;
    touchRectangle.PointerReleased += touchRectangle_PointerReleased;
    touchRectangle.PointerExited += touchRectangle_PointerExited;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Pointer event listeners.
    AddHandler touchRectangle.PointerPressed, AddressOf touchRectangle_PointerPressed
    AddHandler touchRectangle.PointerReleased, AddressOf Me.touchRectangle_PointerReleased
    AddHandler touchRectangle.PointerExited, AddressOf touchRectangle_PointerExited

End Sub
```

Por último, el controlador de eventos [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) aumenta los valores [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) y [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) del objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle), mientras que los controladores de eventos [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) y [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) establecen los objetos **Height** y **Width** en sus valores iniciales.

```cpp
// Handler for pointer exited event.
void MainPage::touchRectangle_PointerExited(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer released event.
void MainPage::touchRectangle_PointerReleased(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Reset the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 200;
        rect->Height = 100;
    }
}

// Handler for pointer pressed event.
void MainPage::touchRectangle_PointerPressed(Object^ sender, PointerRoutedEventArgs^ e)
{
    Rectangle^ rect = (Rectangle^)sender;

    // Change the dimensions of the Rectangle.
    if (nullptr != rect)
    {
        rect->Width = 250;
        rect->Height = 150;
    }
}
```

```cs
// Handler for pointer exited event.
private void touchRectangle_PointerExited(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Pointer moved outside Rectangle hit test area.
    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}
// Handler for pointer released event.
private void touchRectangle_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Reset the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 200;
        rect.Height = 100;
    }
}

// Handler for pointer pressed event.
private void touchRectangle_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    Rectangle rect = sender as Rectangle;

    // Change the dimensions of the Rectangle.
    if (null != rect)
    {
        rect.Width = 250;
        rect.Height = 150;
    }
}
```

```vb
' Handler for pointer exited event.
Private Sub touchRectangle_PointerExited(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Pointer moved outside Rectangle hit test area.
    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer released event.
Private Sub touchRectangle_PointerReleased(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Reset the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 200
        rect.Height = 100
    End If
End Sub

' Handler for pointer pressed event.
Private Sub touchRectangle_PointerPressed(sender As Object, e As PointerRoutedEventArgs)
    Dim rect As Rectangle = CType(sender, Rectangle)

    ' Change the dimensions of the Rectangle.
    If (rect IsNot Nothing) Then
        rect.Width = 250
        rect.Height = 150
    End If
End Sub
```

## <a name="manipulation-events"></a>Eventos de manipulación


Usa eventos de manipulación si necesitas incluir en tu aplicación compatibilidad para interacciones con varios dedos o interacciones que requieren datos de velocidad.

Puedes usar los eventos de manipulación para detectar interacciones como, por ejemplo, arrastrar, zoom y sostener.

> [!NOTE]
> Touchpad no genera eventos de manipulación. En su lugar, se producirán eventos de puntero para la entrada de Touchpad.

Esta es una lista de eventos de manipulación y los argumentos de evento relacionados.

| Evento o clase                                                                                               | Description                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| [**Evento ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)                                   | Se produce cuando se crea por primera vez el procesador de manipulación.                                                                                  |
| [**Evento ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)                                     | Se produce cuando un dispositivo de entrada comienza una manipulación en el [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement).                                            |
| [**Evento ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)                                         | Se produce cuando el dispositivo de entrada cambia de posición durante una manipulación.                                                                      |
| [**Evento ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastartingevent)                | Ocurre cuando el dispositivo de entrada pierde contacto con el objeto [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) durante una manipulación y el inicio de la inercia. |
| [**Evento ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)                                 | Ocurre cuando finaliza la manipulación y la inercia sobre el [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement).                                          |
| [**ManipulationStartingRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationStartingRoutedEventArgs)               | Proporciona datos para el evento [**ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting).                                         |
| [**ManipulationStartedRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationStartedRoutedEventArgs)                 | Proporciona datos para el evento [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted).                                           |
| [**ManipulationDeltaRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationDeltaRoutedEventArgs)                     | Proporciona datos para el evento [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta).                                               |
| [**ManipulationInertiaStartingRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationInertiaStartingRoutedEventArgs) | Proporciona datos para el evento [**ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting).                           |
| [**ManipulationVelocities**](/uwp/api/Windows.UI.Input.ManipulationVelocities)                                              | Describe la velocidad a la que tienen lugar las manipulaciones.                                                                                         |
| [**ManipulationCompletedRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.ManipulationCompletedRoutedEventArgs)             | Proporciona datos para el evento [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted).                                       |

 

Un gesto consiste en una serie de eventos de manipulación. Cada gesto se inicia con un evento [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted), por ejemplo, cuando un usuario toca la pantalla.

A continuación, se desencadenan uno o más eventos [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta). Por ejemplo, si tocas la pantalla y luego arrastras el dedo por la pantalla. Por último, cuando termina la interacción, tiene lugar un evento [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted).

> [!NOTE]
> Si no dispone de un monitor de pantalla táctil, puede probar el código de evento de manipulación en el simulador mediante una interfaz de mouse y rueda de mouse.

 

En el ejemplo siguiente se muestra cómo usar los eventos [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) para controlar una interacción de deslizamiento en un objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) y moverlo por la pantalla.

Primero, se crea un objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) denominado `touchRectangle` en XAML, con un valor de [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) y [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) de 200.

```XAML
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Rectangle Name="touchRectangle"
               Width="200" Height="200" Fill="Blue" 
               ManipulationMode="All"/>
</Grid>
```

A continuación, se crea un objeto [**TranslateTransform**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) global denominado `dragTranslation` para traducir el objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle). Un agente de escucha de eventos de [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) se especifica en el **rectángulo** y `dragTranslation` se agrega al [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) del **rectángulo** .

```cpp
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
Windows::UI::Xaml::Media::TranslateTransform^ dragTranslation;
```

```cs
// Global translation transform used for changing the position of 
// the Rectangle based on input data from the touch contact.
private TranslateTransform dragTranslation;
```

```vb
' Global translation transform used for changing the position of 
' the Rectangle based on input data from the touch contact.
Private dragTranslation As TranslateTransform
```

```cpp
MainPage::MainPage()
{
    InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle->ManipulationDelta += 
        ref new ManipulationDeltaEventHandler(
            this, 
            &MainPage::touchRectangle_ManipulationDelta);
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = ref new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle->RenderTransform = dragTranslation;
}
```

```cs
public MainPage()
{
    this.InitializeComponent();

    // Listener for the ManipulationDelta event.
    touchRectangle.ManipulationDelta += touchRectangle_ManipulationDelta;
    // New translation transform populated in 
    // the ManipulationDelta handler.
    dragTranslation = new TranslateTransform();
    // Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = this.dragTranslation;
}
```

```vb
Public Sub New()

    ' This call is required by the designer.
    InitializeComponent()

    ' Listener for the ManipulationDelta event.
    AddHandler touchRectangle.ManipulationDelta,
        AddressOf testRectangle_ManipulationDelta
    ' New translation transform populated in 
    ' the ManipulationDelta handler.
    dragTranslation = New TranslateTransform()
    ' Apply the translation to the Rectangle.
    touchRectangle.RenderTransform = dragTranslation

End Sub
```

Por último, en el controlador de eventos [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta), se actualiza la posición del objeto [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) al usar el objeto [**TranslateTransform**](/uwp/api/Windows.UI.Xaml.Media.TranslateTransform) en la propiedad [**Delta**](/uwp/api/windows.ui.xaml.input.manipulationdeltaroutedeventargs.delta).

```cpp
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void MainPage::touchRectangle_ManipulationDelta(Object^ sender,
    ManipulationDeltaRoutedEventArgs^ e)
{
    // Move the rectangle.
    dragTranslation->X += e->Delta.Translation.X;
    dragTranslation->Y += e->Delta.Translation.Y;
    
}
```

```cs
// Handler for the ManipulationDelta event.
// ManipulationDelta data is loaded into the
// translation transform and applied to the Rectangle.
void touchRectangle_ManipulationDelta(object sender,
    ManipulationDeltaRoutedEventArgs e)
{
    // Move the rectangle.
    dragTranslation.X += e.Delta.Translation.X;
    dragTranslation.Y += e.Delta.Translation.Y;
}
```

```vb
' Handler for the ManipulationDelta event.
' ManipulationDelta data Is loaded into the
' translation transform And applied to the Rectangle.
Private Sub testRectangle_ManipulationDelta(
    sender As Object,
    e As ManipulationDeltaRoutedEventArgs)

    ' Move the rectangle.
    dragTranslation.X = (dragTranslation.X + e.Delta.Translation.X)
    dragTranslation.Y = (dragTranslation.Y + e.Delta.Translation.Y)

End Sub
```

## <a name="routed-events"></a>Eventos enrutados

Todos los eventos de puntero, los eventos de gestos y los eventos de manipulación mencionados aquí se implementan como *eventos enrutados* . Esto significa que el evento podría estar controlado por objetos distintos que el que generó el evento. Los elementos principales sucesivos de un árbol de objetos, como los contenedores principales de un objeto [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) o la raíz [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page) de tu aplicación, pueden elegir controlar estos eventos aunque el elemento original no lo haga. Por el contrario, cualquier objeto que no controle el evento puede marcar el evento controlado para que nunca llegue a un elemento principal. Para obtener más información sobre el concepto de evento enrutado y cómo afecta a cómo se escriben los controladores para los eventos enrutados, vea [Introducción a eventos y eventos enrutados](/previous-versions/windows/apps/hh758286(v=win.10)).

> [!Important]
> Si necesita controlar los eventos de puntero para un elemento [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) en una vista desplazable (como ScrollViewer o ListView), debe deshabilitar explícitamente la compatibilidad con los eventos de manipulación en el elemento en la vista llamando a [UIElement. CancelDirectmanipulation ()](/uwp/api/windows.ui.xaml.uielement.canceldirectmanipulations). Para volver a habilitar los eventos de manipulación en la vista, llame a [UIElement. TryStartDirectManipulation ()](/uwp/api/windows.ui.xaml.uielement.trystartdirectmanipulation).

## <a name="dos-and-donts"></a>Dos y don't's

- Diseña aplicaciones en las que la interacción táctil sea el principal método de entrada que se espera.
- Incluye información visual sobre todos los tipos de interacción (táctil, lápiz, pluma, mouse, etc.).
- Optimiza la selección de destinos ajustando el tamaño, la geometría de contacto, la limpieza y la inclinación del destino táctil.
- Optimiza la precisión usando puntos de acoplamiento y "guías" direccionales.
- Incorpora información sobre herramientas y controladores para mejorar la precisión táctil de los elementos de UI con espacio muy reducido.
- No uses interacciones temporales en la medida de lo posible (ejemplo de uso correcto: mantener pulsado).
- No uses la cantidad de dedos para distinguir la manipulación en la medida de lo posible.

## <a name="related-articles"></a>Artículos relacionados

- [Control de la entrada con puntero](handle-pointer-input.md)
- [Identificación de dispositivos de entrada](identify-input-devices.md)

### <a name="samples"></a>Ejemplos

- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de latencia baja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Muestras de archivo

- [Entrada: muestra de funcionalidades del dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: muestra de eventos de entrada de usuario de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: gestos y manipulaciones con GestureRecognizer](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
