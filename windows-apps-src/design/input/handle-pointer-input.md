---
description: Reciba, procese y administre los datos de entrada desde dispositivos que apunten, como la entrada táctil, el mouse, el lápiz/lápiz y el panel táctil, en las aplicaciones Windows.
title: Controlar la entrada de puntero
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: lápiz, mouse, panel táctil, función táctil, puntero, entrada, interacción del usuario
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bae166c1671421c13302df0d2f85e505985d3f2e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030748"
---
# <a name="handle-pointer-input"></a>Controlar la entrada de puntero

Recibir, procesar y administrar los datos de entrada de los dispositivos señaladores (por ejemplo, táctil, Mouse, lápiz/lápiz y Touchpad) en las aplicaciones Windows.

> [!Important]
> Cree interacciones personalizadas solo si hay un requisito claro y bien definido y las interacciones que admiten los controles de plataforma no admiten su escenario.  
> Si personaliza las experiencias de interacción en la aplicación Windows, los usuarios esperan que sean coherentes, intuitivas y reconocibles. Por estos motivos, se recomienda modelar las interacciones personalizadas en las que admiten los [controles de plataforma](../controls-and-patterns/controls-by-function.md). Los controles de plataforma proporcionan la experiencia de interacción del usuario completa de la aplicación Windows, incluidas las interacciones estándar, los efectos físicos animados, los comentarios visuales y la accesibilidad. 

## <a name="important-apis"></a>API importantes
- [Windows.Devices.Input](/uwp/api/Windows.Devices.Input)
- [Windows.UI.Input](/uwp/api/Windows.UI.Core)
- [Windows.UI.Xaml.Input](/uwp/api/Windows.UI.Input)

## <a name="pointers"></a>Punteros
Normalmente, la mayoría de las experiencias de interacción implican al usuario que identifica el objeto con el que desea interactuar; para ello, apunta a ella a través de dispositivos de entrada como, por ejemplo, táctil, Mouse, lápiz/lápiz y Touchpad. Dado que los datos de HID sin procesar (HID) proporcionados por estos dispositivos de entrada incluyen muchas propiedades comunes, los datos se promueven y consolidan en una pila de entrada unificada y se exponen como datos de puntero independientes del dispositivo. Las aplicaciones de Windows pueden consumir estos datos sin preocuparse por el dispositivo de entrada que se está usando.

> [!NOTE]
> La información específica del dispositivo también se promueve de los datos de HID sin procesar, en caso de que la aplicación lo requiera.

Cada punto de entrada (o contacto) de la pila de entrada se representa mediante un objeto de [**puntero**](/uwp/api/Windows.UI.Xaml.Input.Pointer) expuesto a través del parámetro [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) en los diversos controladores de eventos de puntero. En el caso de una entrada de varios lápices o varias entradas táctiles, cada contacto se trata como un puntero de entrada único.

## <a name="pointer-events"></a>Eventos de puntero

Los eventos de puntero exponen información básica como el tipo de dispositivo de entrada y el estado de detección (en el intervalo o en contacto) y la información ampliada como la ubicación, la presión y la geometría de contacto. Además, también están disponibles las propiedades del dispositivo específico, como qué botón del mouse presionó un usuario o si un usuario está usando el extremo borrador del lápiz. Si la aplicación necesita diferenciar entre distintos dispositivos de entrada y sus funcionalidades, consulta el tema de [identificar dispositivos de entrada](identify-input-devices.md).

Las aplicaciones de Windows pueden escuchar los siguientes eventos de puntero:

> [!NOTE]
> Restrinja la entrada de puntero a un elemento específico de la interfaz de usuario mediante una llamada a  [**CapturePointer**](/uwp/api/windows.ui.xaml.uielement.capturepointer) en ese elemento dentro de un controlador de eventos de puntero. Cuando un elemento captura un puntero, solo dicho objeto recibe eventos de entrada de puntero, incluso cuando el puntero se mueve fuera del área de límite del objeto. El [**IsInContact**](/uwp/api/windows.ui.xaml.input.pointer.isincontact) (botón del mouse presionado, táctil o lápiz en contacto) debe ser true para que **CapturePointer** se realice correctamente.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Evento</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointercanceled"><strong>PointerCanceled</strong></a></p></td>
<td align="left"><p>Se produce cuando la plataforma cancela un puntero. Esto puede ocurrir en las siguientes circunstancias:</p>
<ul>
<li>Los punteros táctiles se cancelan cuando se detecta un lápiz dentro del alcance de la superficie de entrada.</li>
<li>No se detectan contactos activos de más de 100 ms.</li>
<li>Se modifica el monitor o la pantalla (resolución, configuración, configuración de multimonitor).</li>
<li>El escritorio está bloqueado o el usuario ha cerrado la sesión.</li>
<li>El número de contactos simultáneos superó el número admitido por el dispositivo.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointercapturelost"><strong>PointerCaptureLost</strong></a></p></td>
<td align="left"><p>Se produce cuando otro elemento de la interfaz de usuario captura el puntero, si se libera el puntero o si se captura otro puntero mediante programación.</p>
<div class="alert">
<strong>Nota:</strong>  No hay ningún evento de captura de puntero correspondiente.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointerentered"><strong>PointerEntered</strong></a></p></td>
<td align="left"><p>Se produce cuando un puntero entra en el área límite de un elemento. Esto puede ocurrir de forma ligeramente distinta para la función táctil, de panel táctil, mouse y de pluma.</p>
<ul>
<li>La función táctil requiere un contacto del dedo para desencadenar este evento, bien sea de una pulsación directa hacia abajo o de movimiento en el área límite del elemento.</li>
<li>Tanto el mouse como el panel táctil tienen un cursor en pantalla que siempre está visible y desencadena este evento incluso si no se presiona ningún botón del mouse o del teclado táctil.</li>
<li>Al igual que la función táctil, la pluma desencadena este evento con un movimiento directo hacia abajo del lápiz sobre el elemento o un movimiento en el área límite del elemento. Sin embargo, Pen también tiene un estado de desplazamiento (<a href="/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>) que, cuando es true, desencadena este evento.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointerexited"><strong>PointerExited</strong></a></p></td>
<td align="left"><p>Se produce cuando un puntero sale del área límite de un elemento. Esto puede ocurrir de forma ligeramente distinta para la función táctil, de panel táctil, mouse y de pluma.</p>
<ul>
<li>La función táctil requiere un contacto con el dedo y desencadena este evento cuando el puntero se mueve fuera del área límite del elemento.</li>
<li>Tanto el mouse como el panel táctil tienen un cursor en pantalla que siempre está visible y desencadena este evento incluso si no se presiona ningún botón del mouse o del teclado táctil.</li>
<li>Al igual que la función táctil, la pluma desencadena este evento cuando se mueve fuera del área límite del elemento. Sin embargo, Pen también tiene un estado de desplazamiento (<a href="/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>) que desencadena este evento cuando el estado cambia de true a false.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointermoved"><strong>PointerMoved</strong></a></p></td>
<td align="left"><p>Se produce cuando un puntero modifica sus coordenadas, el estado de los botones, la presión, la inclinación o la geometría de contacto (por ejemplo, el ancho y alto) dentro del área límite de un elemento. Esto puede ocurrir de forma ligeramente distinta para la función táctil, de panel táctil, mouse y de pluma.</p>
<ul>
<li>La función táctil requiere un contacto con el dedo y desencadena este evento solo cuando está en contacto dentro del área límite del elemento.</li>
<li>Tanto el mouse como el panel táctil tienen un cursor en pantalla que siempre está visible y desencadena este evento incluso si no se presiona ningún botón del mouse o del teclado táctil.</li>
<li>Al igual que la función táctil, la pluma desencadena este evento cuando está en contacto dentro del área límite del elemento. Sin embargo, Pen también tiene un estado de desplazamiento (<a href="/uwp/api/windows.ui.xaml.input.pointer.isinrange">IsInRange</a>) que, cuando es true y dentro del área de límite del elemento, desencadena este evento.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointerpressed"><strong>PointerPressed</strong></a></p></td>
<td align="left"><p>Se produce cuando el puntero indica una acción de presión (como un toque, presionar el botón del mouse, presionar con el lápiz o presionar el botón de panel táctil) dentro del área límite de un elemento.</p>
<p>Se debe llamar a <a href="/uwp/api/windows.ui.xaml.uielement.capturepointer">CapturePointer</a> desde el controlador para este evento.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointerreleased"><strong>PointerReleased</strong></a></p></td>
<td align="left"><p>Se produce cuando el puntero indica una acción de liberar (por ejemplo, finalizar un toque, soltar el botón del mouse, levantar el lápiz o soltar el botón del panel táctil) dentro del área límite de un elemento o, si se captura el puntero, fuera del área límite.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged"><strong>PointerWheelChanged</strong></a></p></td>
<td align="left"><p>Se produce cuando se gira la rueda del mouse.</p>
<p>La entrada de mouse se asocia con un solo puntero que se asigna cuando se detecta por primera vez la entrada. Al hacer clic en un botón del mouse (izquierda, rueda o derecha), se crea una asociación secundaria entre el puntero y ese botón a través del evento <a href="/uwp/api/windows.ui.xaml.uielement.pointermoved">PointerMoved</a> .</p></td>
</tr>
</tbody>
</table> 

## <a name="pointer-event-example"></a>Ejemplo de evento de puntero

Estos son algunos fragmentos de código de una aplicación de seguimiento de puntero básica que muestran cómo escuchar y controlar eventos de varios punteros, y cómo obtener varias propiedades para los punteros asociados.

![Interfaz de usuario de la aplicación de puntero](images/pointers/pointers1.gif)

**Descargar este ejemplo del [ejemplo de entrada de puntero (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)**

### <a name="create-the-ui"></a>Creación de la interfaz de usuario

En este ejemplo, usamos un [rectángulo](/uwp/api/windows.ui.xaml.shapes.rectangle) ( `Target` ) como el objeto que consume la entrada del puntero. El color del destino cambia cuando cambia el estado del puntero.

Los detalles de cada puntero se muestran en un [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) flotante que sigue el puntero a medida que se mueve. Los propios eventos de puntero se muestran en el [RichTextBlock](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) a la derecha del rectángulo.

Este es el lenguaje XAML (XAML) de la interfaz de usuario en este ejemplo. 

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="*" />
        <ColumnDefinition Width="250"/>
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="*" />
        <RowDefinition Height="320" />
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Canvas Name="Container" 
            Grid.Column="0"
            Grid.Row="1"
            HorizontalAlignment="Center" 
            VerticalAlignment="Center" 
            Margin="245,0" 
            Height="320"  Width="640">
        <Rectangle Name="Target" 
                    Fill="#FF0000" 
                    Stroke="Black" 
                    StrokeThickness="0"
                    Height="320" Width="640" />
    </Canvas>
    <Grid Grid.Column="1" Grid.Row="0" Grid.RowSpan="3">
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Button Name="buttonClear" 
                Grid.Row="0"
                Content="Clear"
                Foreground="White"
                HorizontalAlignment="Stretch" 
                VerticalAlignment="Stretch">
        </Button>
        <ScrollViewer Name="eventLogScrollViewer" Grid.Row="1" 
                        VerticalScrollMode="Auto" 
                        Background="Black">                
            <RichTextBlock Name="eventLog"  
                        TextWrapping="Wrap" 
                        Foreground="#FFFFFF" 
                        ScrollViewer.VerticalScrollBarVisibility="Visible" 
                        ScrollViewer.HorizontalScrollBarVisibility="Disabled"
                        Grid.ColumnSpan="2">
            </RichTextBlock>
        </ScrollViewer>
    </Grid>
</Grid>
```

### <a name="listen-for-pointer-events"></a>Escuchar eventos de puntero

En la mayoría de los casos, es recomendable que obtengas la información del puntero mediante el elemento [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs) del controlador de eventos.

Si el argumento del evento no expone los detalles de puntero necesarios, puedes acceder a la información extendida de [**PointerPoint**](/uwp/api/Windows.UI.Input.PointerPoint) que se expone a través de los métodos [**GetCurrentPoint**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) y [**GetIntermediatePoints**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) de [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs).

El código siguiente configura el objeto de diccionario global para realizar el seguimiento de cada puntero activo e identifica los distintos agentes de escucha de eventos de puntero para el objeto de destino.

```CSharp
// Dictionary to maintain information about each active pointer. 
// An entry is added during PointerPressed/PointerEntered events and removed 
// during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
Dictionary<uint, Windows.UI.Xaml.Input.Pointer> pointers;

public MainPage()
{
    this.InitializeComponent();

    // Initialize the dictionary.
    pointers = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>();

    // Declare the pointer event handlers.
    Target.PointerPressed += 
        new PointerEventHandler(Target_PointerPressed);
    Target.PointerEntered += 
        new PointerEventHandler(Target_PointerEntered);
    Target.PointerReleased += 
        new PointerEventHandler(Target_PointerReleased);
    Target.PointerExited += 
        new PointerEventHandler(Target_PointerExited);
    Target.PointerCanceled += 
        new PointerEventHandler(Target_PointerCanceled);
    Target.PointerCaptureLost += 
        new PointerEventHandler(Target_PointerCaptureLost);
    Target.PointerMoved += 
        new PointerEventHandler(Target_PointerMoved);
    Target.PointerWheelChanged += 
        new PointerEventHandler(Target_PointerWheelChanged);

    buttonClear.Click += 
        new RoutedEventHandler(ButtonClear_Click);
}
```

### <a name="handle-pointer-events"></a>Controlar eventos de puntero

Después, se usan comentarios de interfaz de usuario para mostrar controladores de eventos de puntero básicos.

-   Este controlador administra el evento [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) . Se agrega el evento al registro de eventos, se agrega el puntero al Diccionario de punteros activo y se muestran los detalles del puntero.

    > [!NOTE]
    > Los eventos [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) y [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) no siempre se producen en pares. La aplicación debe escuchar y controlar cualquier evento que pueda concluir un puntero hacia abajo (como [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited), [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)y [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)).
         

```csharp
/// <summary>
/// The pointer pressed event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Down: " + ptrPt.PointerId);

    // Lock the pointer to the target.
    Target.CapturePointer(e.Pointer);

    // Update event log.
    UpdateEventLog("Pointer captured: " + ptrPt.PointerId);

    // Check if pointer exists in dictionary (ie, enter occurred prior to press).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Change background color of target when pointer contact detected.
    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Este controlador administra el evento [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered) . Se agrega el evento al registro de eventos, se agrega el puntero a la colección de punteros y se muestran los detalles del puntero.

```csharp
/// <summary>
/// The pointer entered event handler.
/// We do not capture the pointer on this event.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Entered: " + ptrPt.PointerId);

    // Check if pointer already exists (if enter occurred prior to down).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    if (pointers.Count == 0)
    {
        // Change background color of target when pointer contact detected.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Este controlador administra el evento [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved) . Se agrega el evento al registro de eventos y se actualizan los detalles del puntero.

    > [!Important]
    > La entrada de mouse se asocia con un solo puntero que se asigna cuando se detecta por primera vez la entrada. Al hacer clic en un botón del mouse (izquierdo, rueda o derecho), se crea una asociación secundaria entre el puntero y ese botón a través del evento [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed). El evento [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) solo se desencadena cuando se libera ese mismo botón del mouse (no se puede asociar ningún otro botón con el puntero hasta que se complete este evento). Debido a esta asociación exclusiva, los clics de otros botones del mouse se enrutan a través del evento [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved).     

```csharp
/// <summary>
/// The pointer moved event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Multiple, simultaneous mouse button inputs are processed here.
    // Mouse input is associated with a single pointer assigned when 
    // mouse input is first detected. 
    // Clicking additional mouse buttons (left, wheel, or right) during 
    // the interaction creates secondary associations between those buttons 
    // and the pointer through the pointer pressed event. 
    // The pointer released event is fired only when the last mouse button 
    // associated with the interaction (not necessarily the initial button) 
    // is released. 
    // Because of this exclusive association, other mouse button clicks are 
    // routed through the pointer move event.          
    if (ptrPt.PointerDevice.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        if (ptrPt.Properties.IsLeftButtonPressed)
        {
            UpdateEventLog("Left button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsMiddleButtonPressed)
        {
            UpdateEventLog("Wheel button: " + ptrPt.PointerId);
        }
        if (ptrPt.Properties.IsRightButtonPressed)
        {
            UpdateEventLog("Right button: " + ptrPt.PointerId);
        }
    }

    // Display pointer details.
    UpdateInfoPop(ptrPt);
}
```

-   Este controlador administra el evento [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged) . Se agrega el evento al registro de eventos, se agrega el puntero a la matriz de punteros (si es necesario) y se muestran los detalles del puntero.

```csharp
/// <summary>
/// The pointer wheel event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Mouse wheel: " + ptrPt.PointerId);

    // Check if pointer already exists (for example, enter occurred prior to wheel).
    if (!pointers.ContainsKey(ptrPt.PointerId))
    {
        // Add contact to dictionary.
        pointers[ptrPt.PointerId] = e.Pointer;
    }

    // Display pointer details.
    CreateInfoPop(ptrPt);
}
```

-   Este controlador administra el evento [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased) en el que se termina el contacto con el digitalizador. Se agrega el evento al registro de eventos, se quita el puntero de la colección de punteros y se actualizan los detalles del puntero.

```csharp
/// <summary>
/// The pointer released event handler.
/// PointerPressed and PointerReleased don't always occur in pairs. 
/// Your app should listen for and handle any event that can conclude 
/// a pointer down (PointerExited, PointerCanceled, PointerCaptureLost).
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Up: " + ptrPt.PointerId);

    // If event source is mouse or touchpad and the pointer is still 
    // over the target, retain pointer and pointer details.
    // Return without removing pointer from pointers dictionary.
    // For this example, we assume a maximum of one mouse pointer.
    if (ptrPt.PointerDevice.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
    {
        // Update target UI.
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

        DestroyInfoPop(ptrPt);

        // Remove contact from dictionary.
        if (pointers.ContainsKey(ptrPt.PointerId))
        {
            pointers[ptrPt.PointerId] = null;
            pointers.Remove(ptrPt.PointerId);
        }

        // Release the pointer from the target.
        Target.ReleasePointerCapture(e.Pointer);

        // Update event log.
        UpdateEventLog("Pointer released: " + ptrPt.PointerId);
    }
    else
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
    }
}
```

-   Este controlador administra el evento [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited) (cuando se mantiene el contacto con el digitalizador). Se agrega el evento al registro de eventos, se quita el puntero de la matriz de punteros y se actualizan los detalles del puntero.

```csharp
/// <summary>
/// The pointer exited event handler.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer exited: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
    }

    // Update the UI and pointer details.
    DestroyInfoPop(ptrPt);
}
```

-   Este controlador administra el evento [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled) . Se agrega el evento al registro de eventos, se quita el puntero de la matriz de punteros y se actualizan los detalles del puntero.

```csharp
/// <summary>
/// The pointer canceled event handler.
/// Fires for various reasons, including: 
///    - Touch contact canceled by pen coming into range of the surface.
///    - The device doesn't report an active contact for more than 100ms.
///    - The desktop is locked or the user logged off. 
///    - The number of simultaneous contacts exceeded the number supported by the device.
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer canceled: " + ptrPt.PointerId);

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    DestroyInfoPop(ptrPt);
}
```

-   Este controlador administra el evento [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost) . Se agrega el evento al registro de eventos, se quita el puntero de la matriz de punteros y se actualizan los detalles del puntero.

    > [!NOTE]
    > [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost) puede producirse en lugar de [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased). La captura de puntero se puede perder por varias razones, como la interacción con el usuario, la captura mediante programación de otro puntero, la llamada a [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased).     

```csharp
/// <summary>
/// The pointer capture lost event handler.
/// Fires for various reasons, including: 
///    - User interactions
///    - Programmatic capture of another pointer
///    - Captured pointer was deliberately released
// PointerCaptureLost can fire instead of PointerReleased. 
/// </summary>
/// <param name="sender">Source of the pointer event.</param>
/// <param name="e">Event args for the pointer routed event.</param>
private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
{
    // Prevent most handlers along the event route from handling the same event again.
    e.Handled = true;

    PointerPoint ptrPt = e.GetCurrentPoint(Target);

    // Update event log.
    UpdateEventLog("Pointer capture lost: " + ptrPt.PointerId);

    if (pointers.Count == 0)
    {
        Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
    }

    // Remove contact from dictionary.
    if (pointers.ContainsKey(ptrPt.PointerId))
    {
        pointers[ptrPt.PointerId] = null;
        pointers.Remove(ptrPt.PointerId);
    }

    DestroyInfoPop(ptrPt);
}
```

### <a name="get-pointer-properties"></a>Obtener las propiedades del puntero

Como se indicó anteriormente, debes obtener la información más extendida del puntero desde un objeto [**Windows.UI.Input.PointerPoint**](/uwp/api/Windows.UI.Input.PointerPoint) obtenido a través de los métodos [**GetCurrentPoint**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getcurrentpoint) y [**GetIntermediatePoints**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.getintermediatepoints) de [**PointerRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.PointerRoutedEventArgs). En los fragmentos de código siguientes se muestra cómo.

-   Primero, se crea un nuevo elemento [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) para cada puntero.

```csharp
/// <summary>
/// Create the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void CreateInfoPop(PointerPoint ptrPt)
{
    TextBlock pointerDetails = new TextBlock();
    pointerDetails.Name = ptrPt.PointerId.ToString();
    pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
    pointerDetails.Text = QueryPointer(ptrPt);

    TranslateTransform x = new TranslateTransform();
    x.X = ptrPt.Position.X + 20;
    x.Y = ptrPt.Position.Y + 20;
    pointerDetails.RenderTransform = x;

    Container.Children.Add(pointerDetails);
}
```

-   Después, se proporciona una manera de actualizar la información del puntero en un elemento [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) existente asociado con ese puntero.

```csharp
/// <summary>
/// Update the pointer info popup.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
void UpdateInfoPop(PointerPoint ptrPt)
{
    foreach (var pointerDetails in Container.Children)
    {
        if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
        {
            TextBlock textBlock = (TextBlock)pointerDetails;
            if (textBlock.Name == ptrPt.PointerId.ToString())
            {
                // To get pointer location details, we need extended pointer info.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;
                textBlock.Text = QueryPointer(ptrPt);
            }
        }
    }
}
```

-   Por último, se consultan diversas propiedades del puntero.

```csharp
/// <summary>
/// Get pointer details.
/// </summary>
/// <param name="ptrPt">Reference to the input pointer.</param>
/// <returns>A string composed of pointer details.</returns>
String QueryPointer(PointerPoint ptrPt)
{
    String details = "";

    switch (ptrPt.PointerDevice.PointerDeviceType)
    {
        case Windows.Devices.Input.PointerDeviceType.Mouse:
            details += "\nPointer type: mouse";
            break;
        case Windows.Devices.Input.PointerDeviceType.Pen:
            details += "\nPointer type: pen";
            if (ptrPt.IsInContact)
            {
                details += "\nPressure: " + ptrPt.Properties.Pressure;
                details += "\nrotation: " + ptrPt.Properties.Orientation;
                details += "\nTilt X: " + ptrPt.Properties.XTilt;
                details += "\nTilt Y: " + ptrPt.Properties.YTilt;
                details += "\nBarrel button pressed: " + ptrPt.Properties.IsBarrelButtonPressed;
            }
            break;
        case Windows.Devices.Input.PointerDeviceType.Touch:
            details += "\nPointer type: touch";
            details += "\nrotation: " + ptrPt.Properties.Orientation;
            details += "\nTilt X: " + ptrPt.Properties.XTilt;
            details += "\nTilt Y: " + ptrPt.Properties.YTilt;
            break;
        default:
            details += "\nPointer type: n/a";
            break;
    }

    GeneralTransform gt = Target.TransformToVisual(this);
    Point screenPoint;

    screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
    details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
        "\nPointer location (target): " + Math.Round(ptrPt.Position.X) + ", " + Math.Round(ptrPt.Position.Y) +
        "\nPointer location (container): " + Math.Round(screenPoint.X) + ", " + Math.Round(screenPoint.Y);

    return details;
}
```

## <a name="primary-pointer"></a>Puntero primario
Algunos dispositivos de entrada, como un digitalizador táctil o un panel táctil, admiten más que el puntero único típico de un mouse o un lápiz (en la mayoría de los casos, ya que el Surface Hub admite dos entradas manuscritas). 

Use la propiedad **[IsPrimary](/uwp/api/windows.ui.input.pointerpointproperties.IsPrimary)** de solo lectura de la clase **[PointerPointerProperties](/uwp/api/windows.ui.input.pointerpointproperties)** para identificar y diferenciar un puntero primario único (el puntero primario siempre es el primer puntero detectado durante una secuencia de entrada). 

Al identificar el puntero principal, puede usarlo para emular la entrada del mouse o del lápiz, personalizar las interacciones o proporcionar alguna otra funcionalidad o interfaz de usuario específica.

> [!NOTE]
> Si el puntero principal se libera, se cancela o se pierde durante una secuencia de entrada, no se crea un puntero de entrada principal hasta que se inicia una nueva secuencia de entrada (una secuencia de entrada finaliza cuando todos los punteros se han liberado, cancelado o perdido).

## <a name="primary-pointer-animation-example"></a>Ejemplo de animación de puntero principal

Estos fragmentos de código muestran cómo puede proporcionar comentarios visuales especiales para ayudar a los usuarios a diferenciar entre las entradas de puntero en la aplicación.

Esta aplicación concreta usa el color y la animación para resaltar el puntero primario.

![Aplicación de puntero con comentarios visuales animados](images/pointers/pointers-usercontrol-animation.gif)

**Descargar este ejemplo del [ejemplo de entrada de puntero (UserControl con animación)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)**

### <a name="visual-feedback"></a>Información visual

Definimos un **[UserControl](/uwp/api/windows.ui.xaml.controls.usercontrol)** , basado en un objeto de **[elipse](/uwp/api/windows.ui.xaml.shapes.ellipse)** XAML, que resalta Dónde está cada puntero en el lienzo y usa un **[guion gráfico](/uwp/api/windows.ui.xaml.media.animation.storyboard)** para animar la elipse que corresponde al puntero primario.

**Este es el código XAML:**

```xaml
<UserControl
    x:Class="UWP_Pointers.PointerEllipse"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:UWP_Pointers"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="100"
    d:DesignWidth="100">

    <UserControl.Resources>
        <Style x:Key="EllipseStyle" TargetType="Ellipse">
            <Setter Property="Transitions">
                <Setter.Value>
                    <TransitionCollection>
                        <ContentThemeTransition/>
                    </TransitionCollection>
                </Setter.Value>
            </Setter>
        </Style>
        
        <Storyboard x:Name="myStoryboard">
            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleY)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Double property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <DoubleAnimation
              Storyboard.TargetName="ellipse"
              Storyboard.TargetProperty="(RenderTransform).(ScaleTransform.ScaleX)"  
              Duration="0:0:1" 
              AutoReverse="True" 
              RepeatBehavior="Forever" From="1.0" To="1.4">
            </DoubleAnimation>

            <!-- Animates the value of a Color property between 
            two target values using linear interpolation over the 
            specified Duration. -->
            <ColorAnimation 
                Storyboard.TargetName="ellipse" 
                EnableDependentAnimation="True" 
                Storyboard.TargetProperty="(Fill).(SolidColorBrush.Color)" 
                From="White" To="Red"  Duration="0:0:1" 
                AutoReverse="True" RepeatBehavior="Forever"/>
        </Storyboard>
    </UserControl.Resources>

    <Grid x:Name="CompositionContainer">
        <Ellipse Name="ellipse" 
        StrokeThickness="2" 
        Width="{x:Bind Diameter}" 
        Height="{x:Bind Diameter}"  
        Style="{StaticResource EllipseStyle}" />
    </Grid>
</UserControl>
```

Y este es el código subyacente:
```csharp
using Windows.Foundation;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;

// The User Control item template is documented at 
// https://go.microsoft.com/fwlink/?LinkId=234236

namespace UWP_Pointers
{
    /// <summary>
    /// Pointer feedback object.
    /// </summary>
    public sealed partial class PointerEllipse : UserControl
    {
        // Reference to the application canvas.
        Canvas canvas;

        /// <summary>
        /// Ellipse UI for pointer feedback.
        /// </summary>
        /// <param name="c">The drawing canvas.</param>
        public PointerEllipse(Canvas c)
        {
            this.InitializeComponent();
            canvas = c;
        }

        /// <summary>
        /// Gets or sets the pointer Id to associate with the PointerEllipse object.
        /// </summary>
        public uint PointerId
        {
            get { return (uint)GetValue(PointerIdProperty); }
            set { SetValue(PointerIdProperty, value); }
        }
        // Using a DependencyProperty as the backing store for PointerId.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PointerIdProperty =
            DependencyProperty.Register("PointerId", typeof(uint), 
                typeof(PointerEllipse), new PropertyMetadata(null));


        /// <summary>
        /// Gets or sets whether the associated pointer is Primary.
        /// </summary>
        public bool PrimaryPointer
        {
            get { return (bool)GetValue(PrimaryPointerProperty); }
            set
            {
                SetValue(PrimaryPointerProperty, value);
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryPointer.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryPointerProperty =
            DependencyProperty.Register("PrimaryPointer", typeof(bool), 
                typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the ellipse style based on whether the pointer is Primary.
        /// </summary>
        public bool PrimaryEllipse 
        {
            get { return (bool)GetValue(PrimaryEllipseProperty); }
            set
            {
                SetValue(PrimaryEllipseProperty, value);
                if (value)
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["PrimaryStrokeBrush"];

                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                    ellipse.RenderTransform = new CompositeTransform();
                    ellipse.RenderTransformOrigin = new Point(.5, .5);
                    myStoryboard.Begin();
                }
                else
                {
                    SolidColorBrush fillBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryFillBrush"];
                    SolidColorBrush strokeBrush = 
                        (SolidColorBrush)Application.Current.Resources["SecondaryStrokeBrush"];
                    ellipse.Fill = fillBrush;
                    ellipse.Stroke = strokeBrush;
                }
            }
        }
        // Using a DependencyProperty as the backing store for PrimaryEllipse.  
        // This enables animation, styling, binding, etc...
        public static readonly DependencyProperty PrimaryEllipseProperty =
            DependencyProperty.Register("PrimaryEllipse", 
                typeof(bool), typeof(PointerEllipse), new PropertyMetadata(false));


        /// <summary>
        /// Gets or sets the diameter of the PointerEllipse object.
        /// </summary>
        public int Diameter
        {
            get { return (int)GetValue(DiameterProperty); }
            set { SetValue(DiameterProperty, value); }
        }
        // Using a DependencyProperty as the backing store for Diameter.  This enables animation, styling, binding, etc...
        public static readonly DependencyProperty DiameterProperty =
            DependencyProperty.Register("Diameter", typeof(int), 
                typeof(PointerEllipse), new PropertyMetadata(120));
    }
}
```

### <a name="create-the-ui"></a>Creación de la interfaz de usuario
La interfaz de usuario de este ejemplo se limita al **[lienzo](/uwp/api/windows.ui.xaml.controls.canvas)** de entrada donde se realiza un seguimiento de los punteros y se representan los indicadores de puntero y la animación del puntero principal (si es aplicable), junto con una barra de encabezado que contiene un contador de puntero y un identificador de puntero principal.

Este es el código MainPage. XAML:

```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <StackPanel x:Name="HeaderPanel" 
                Orientation="Horizontal" 
                Grid.Row="0">
        <StackPanel.Transitions>
            <TransitionCollection>
                <AddDeleteThemeTransition/>
            </TransitionCollection>
        </StackPanel.Transitions>
        <TextBlock x:Name="Header" 
                    Text="Basic pointer tracking sample - IsPrimary" 
                    Style="{ThemeResource HeaderTextBlockStyle}" 
                    Margin="10,0,0,0" />
        <TextBlock x:Name="PointerCounterLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Number of pointers: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerCounter"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="0" 
                    Margin="10,0,0,0"/>
        <TextBlock x:Name="PointerPrimaryLabel"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="Primary: " 
                    Margin="50,0,0,0"/>
        <TextBlock x:Name="PointerPrimary"
                    VerticalAlignment="Center"                 
                    Style="{ThemeResource BodyTextBlockStyle}"
                    Text="n/a" 
                    Margin="10,0,0,0"/>
    </StackPanel>
    
    <Grid Grid.Row="1">
        <!--The canvas where we render the pointer UI.-->
        <Canvas x:Name="pointerCanvas"/>
    </Grid>
</Grid>
```

### <a name="handle-pointer-events"></a>Controlar eventos de puntero

Por último, definimos nuestros controladores de eventos de puntero básicos en el código subyacente de MainPage.xaml.cs. No reproduciremos el código aquí, ya que los aspectos básicos se trataron en el ejemplo anterior, pero puede descargar el ejemplo de trabajo desde el ejemplo de [entrada de puntero (UserControl con animación)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip).

## <a name="related-articles"></a>Artículos relacionados

### <a name="topic-samples"></a>Ejemplos del tema

- [Ejemplo de entrada de puntero (Basic)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)
- [Ejemplo de entrada de puntero (UserControl con animación)](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)

### <a name="other-samples"></a>Otras muestras

- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de latencia baja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Ejemplos de archivo

- [Entrada: muestra de eventos de entrada de usuario de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrada: muestra de funcionalidades del dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: ejemplo de manipulaciones y gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Entrada: muestra de prueba de acceso táctil](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: ejemplo de entrada de lápiz simplificada](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
