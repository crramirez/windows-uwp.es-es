---
author: Karl-Bridge-Microsoft
Description: Receive, process, and manage input data from pointing devices such as touch, mouse, pen/stylus, and touchpad, in your Universal Windows Platform (UWP) applications.
title: Controlar la entrada de puntero
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
keywords: lápiz, mouse, panel táctil, función táctil, puntero, entrada, interacción del usuario
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: a0753081af4128cf2cad3eeff9d8c919c42eb596
ms.sourcegitcommit: 588171ea8cb629d2dd6aa2080e742dc8ce8584e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
ms.locfileid: "1895144"
---
# <a name="handle-pointer-input"></a>Controlar la entrada de puntero

Recibe, procesa y administra los datos de entrada de dispositivos señaladores (como la función táctil, el mouse, la pluma o el lápiz y el panel táctil) en aplicaciones para la Plataforma universal de Windows (UWP).

> [!Important]
> Crea interacciones personalizadas solamente si existe un requisito claro y bien definido, y las interacciones admitidas por los controles de plataforma no admiten el escenario.  
> Si personalizas las experiencias de interacción en tu aplicación Windows, los usuarios esperan que sean coherentes, intuitivas y reconocibles. Por estos motivos, recomendamos que modeles las interacciones personalizadas sobre las admitidas por los [controles de plataforma](../controls-and-patterns/controls-by-function.md). Los controles de plataforma proporcionan una experiencia de interacción de usuario de la Plataforma universal de Windows (UWP) completa, incluidas interacciones estándar, efectos físicos animados, comentarios visuales y accesibilidad. 

## <a name="important-apis"></a>API importantes
- [Windows.Devices.Input](https://msdn.microsoft.com/library/windows/apps/br225648)
- [Windows.UI.Input](https://msdn.microsoft.com/library/windows/apps/br208383)
- [Windows.UI.Xaml.Input](https://msdn.microsoft.com/library/windows/apps/br242084)

## <a name="pointers"></a>Punteros
La mayoría de las experiencias de interacción suelen implicar que el usuario identifique el objeto con el que quiere interactuar apuntando a él mediante dispositivos de entrada como la función táctil, mouse, pluma o lápiz y panel táctil. Dado que los datos de dispositivos de interfaz de usuario (HID) sin procesar proporcionados por estos dispositivos de entrada incluyen muchas propiedades comunes, se promueven y consolidan los datos en una pila de entrada unificada y expuesta como datos de puntero independiente del dispositivo. Las aplicaciones para UWP pueden entonces consumir estos datos sin que tengas que preocuparte por el dispositivo de entrada que se use.

> [!NOTE]
> Si la aplicación lo requiere, también se promueve la información específica del dispositivo desde los datos sin procesar de HID.
 

Cada punto de entrada (o contacto) de la pila de entrada se representa mediante un objeto [**Pointer**](https://msdn.microsoft.com/library/windows/apps/br227968) que se expone mediante el parámetro [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) en los diversos controladores de eventos de puntero. En el caso de lápices múltiples o entrada multitáctil, cada contacto se trata como un puntero de entrada único.

## <a name="pointer-events"></a>Eventos de puntero


Los eventos de puntero exponen información básica, como el tipo de dispositivo de entrada y el estado de detección (en un intervalo o un contacto), e información extendida, como la ubicación, la presión y la geometría de contacto. Además, también están disponibles las propiedades del dispositivo específico, como qué botón del mouse presionó un usuario o si un usuario está usando el extremo borrador del lápiz. Si la aplicación necesita diferenciar entre distintos dispositivos de entrada y sus funcionalidades, consulta el tema de [identificar dispositivos de entrada](identify-input-devices.md).

Las aplicaciones para UWP pueden escuchar los siguientes eventos de puntero:

> [!NOTE]
> Limita la entrada del puntero a un elemento específico de la interfaz de usuario llamando a [**CapturePointer**](https://msdn.microsoft.com/library/windows/apps/br208918) en ese elemento dentro de un controlador de eventos de puntero. Cuando un elemento captura un puntero, solo ese objeto recibe eventos de entrada del puntero, incluso aunque se mueva el puntero fuera del área límite del objeto. [**IsInContact**](https://msdn.microsoft.com/library/windows/apps/br227976) (botón del mouse presionado, función táctil o pluma en contacto) debe ser verdadero para que **CapturePointer** se realice correctamente.
 

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208964"><strong>PointerCanceled</strong></a></p></td>
<td align="left"><p>Se produce cuando la plataforma cancela un puntero. Esto puede producirse en las siguientes circunstancias:</p>
<ul>
<li>Los punteros táctiles se cancelan cuando se detecta un lápiz dentro del alcance de la superficie de entrada.</li>
<li>No se detectan contactos activos de más de 100ms.</li>
<li>Se modifica el monitor o la pantalla (resolución, configuración, configuración de multimonitor).</li>
<li>El escritorio está bloqueado o el usuario ha cerrado la sesión.</li>
<li>El número de contactos simultáneos superó el número admitido por el dispositivo.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208965"><strong>PointerCaptureLost</strong></a></p></td>
<td align="left"><p>Se produce cuando otro elemento de la interfaz de usuario captura el puntero, si se libera el puntero o si se captura otro puntero mediante programación.</p>
<div class="alert">
<strong>Nota</strong>  No hay ningún evento de captura de puntero correspondiente.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208968"><strong>PointerEntered</strong></a></p></td>
<td align="left"><p>Se produce cuando un puntero entra en el área límite de un elemento. Esto puede ocurrir de forma ligeramente distinta para la función táctil, de panel táctil, mouse y de pluma.</p>
<ul>
<li>La función táctil requiere un contacto del dedo para desencadenar este evento, bien sea de una pulsación directa hacia abajo o de movimiento en el área límite del elemento.</li>
<li>Tanto el mouse como el panel táctil tienen un cursor en pantalla que siempre está visible y desencadena este evento incluso si no se presiona ningún botón del mouse o del teclado táctil.</li>
<li>Al igual que la función táctil, la pluma desencadena este evento con un movimiento directo hacia abajo del lápiz sobre el elemento o un movimiento en el área límite del elemento. Sin embargo, la pluma también tiene un estado de movimiento ([IsInRange](https://msdn.microsoft.com/library/windows/apps/br227977)) que, cuando es true, desencadena este evento.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208969"><strong>PointerExited</strong></a></p></td>
<td align="left"><p>Se produce cuando un puntero sale del área límite de un elemento. Esto puede ocurrir de forma ligeramente distinta para la función táctil, de panel táctil, mouse y de pluma.</p>
<ul>
<li>La función táctil requiere un contacto con el dedo y desencadena este evento cuando el puntero se mueve fuera del área límite del elemento.</li>
<li>Tanto el mouse como el panel táctil tienen un cursor en pantalla que siempre está visible y desencadena este evento incluso si no se presiona ningún botón del mouse o del teclado táctil.</li>
<li>Al igual que la función táctil, la pluma desencadena este evento cuando se mueve fuera del área límite del elemento. Sin embargo, la pluma también tiene un estado de movimiento ([IsInRange](https://msdn.microsoft.com/library/windows/apps/br227977)) que desencadena este evento cuando el estado cambia de true a false.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208970"><strong>PointerMoved</strong></a></p></td>
<td align="left"><p>Se produce cuando un puntero modifica sus coordenadas, el estado de los botones, la presión, la inclinación o la geometría de contacto (por ejemplo, el ancho y alto) dentro del área límite de un elemento. Esto puede ocurrir de forma ligeramente distinta para la función táctil, de panel táctil, mouse y de pluma.</p>
<ul>
<li>La función táctil requiere un contacto con el dedo y desencadena este evento solo cuando está en contacto dentro del área límite del elemento.</li>
<li>Tanto el mouse como el panel táctil tienen un cursor en pantalla que siempre está visible y desencadena este evento incluso si no se presiona ningún botón del mouse o del teclado táctil.</li>
<li>Al igual que la función táctil, la pluma desencadena este evento cuando está en contacto dentro del área límite del elemento. Sin embargo, la pluma también tiene un estado de movimiento ([IsInRange](https://msdn.microsoft.com/library/windows/apps/br227977)) que desencadena este evento cuando es true y está dentro del área límite del elemento.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208971"><strong>PointerPressed</strong></a></p></td>
<td align="left"><p>Se produce cuando el puntero indica una acción de presión (como un toque, presionar el botón del mouse, presionar con el lápiz o presionar el botón de panel táctil) dentro del área límite de un elemento.</p>
<p>Debe llamarse a [CapturePointer](https://msdn.microsoft.com/library/windows/apps/br208918) desde el controlador para este evento.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208972"><strong>PointerReleased</strong></a></p></td>
<td align="left"><p>Se produce cuando el puntero indica una acción de liberar (por ejemplo, finalizar un toque, soltar el botón del mouse, levantar el lápiz o soltar el botón del panel táctil) dentro del área límite de un elemento o, si se captura el puntero, fuera del área límite.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br208973"><strong>PointerWheelChanged</strong></a></p></td>
<td align="left"><p>Se produce cuando se gira la rueda del mouse.</p>
<p>La entrada de mouse se asocia con un solo puntero que se asigna cuando se detecta por primera vez la entrada. Al hacer clic en un botón del mouse (izquierdo, rueda o derecho), se crea una asociación secundaria entre el puntero y ese botón a través del evento [PointerMoved](https://msdn.microsoft.com/library/windows/apps/br208970).</p></td>
</tr>
</tbody>
</table> 

## <a name="pointer-event-example"></a>Ejemplo de evento de puntero

A continuación se muestran algunos fragmentos de código de una aplicación básica de seguimiento del puntero que muestra cómo escuchar y controlar eventos para varios punteros, y obtener diversas propiedades de los punteros asociados.

![Interfaz de usuario de la aplicación de puntero](images/pointers/pointers1.gif)

**Descargar este ejemplo de [Ejemplo de entrada (básica) de puntero](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)**

### <a name="create-the-ui"></a>Crear la interfaz de usuario

En este ejemplo, usamos un [Rectángulo](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.shapes.rectangle) (`Target`) como el objeto que consume la entrada de puntero. El color del destino cambia cuando cambia el estado del puntero.

Los detalles de cada puntero se muestran en un [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) flotante que sigue el puntero al moverse. Los propios eventos de puntero se notifican en el [RichTextBlock](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) a la derecha del rectángulo.

Este es el lenguaje de marcado de aplicaciones extensible (XAML) para la interfaz de usuario en este ejemplo. 

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

En la mayoría de los casos, es recomendable que obtengas la información del puntero mediante el elemento [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) del controlador de eventos.

Si el argumento del evento no expone los detalles de puntero necesarios, puedes acceder a la información extendida de [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) que se expone a través de los métodos [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) y [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) de [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076).

El siguiente código configura el objeto de diccionario global para hacer un seguimiento de cada puntero activo e identifica los diversas escuchas de eventos de puntero para el objeto de destino.

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

-   Este controlador administra el evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). Se agrega el evento al registro de eventos, se agrega el puntero al diccionario de puntero activo y se muestran los detalles del puntero.

    > [!NOTE]
    > Los eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) y [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) no siempre se producen en parejas. La aplicación debe escuchar y controlar cualquier evento que pueda concluir una acción de puntero hacia abajo (como [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969), [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) y [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)).
         

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

-   Este controlador administra el evento [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968). Se agrega el evento al registro de eventos, se agrega el puntero a la colección de punteros y se muestran los detalles del puntero.

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

-   Este controlador administra el evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970). Se agrega el evento al registro de eventos y se actualizan los detalles del puntero.

    > [!Important]
    > La entrada de mouse se asocia con un solo puntero asignado al detectar por primera vez la entrada. Al hacer clic en un botón del mouse (izquierdo, rueda o derecho), se crea una asociación secundaria entre el puntero y ese botón a través del evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). El evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) solo se desencadena cuando se libera ese mismo botón del mouse (no se puede asociar ningún otro botón con el puntero hasta que se complete este evento). Debido a esta asociación exclusiva, los clics de otros botones del mouse se enrutan a través del evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970).     

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

-   Este controlador administra el evento [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973). Se agrega el evento al registro de eventos, se agrega el puntero a la matriz de punteros (si es necesario) y se muestran los detalles del puntero.

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

-   Este controlador administra el evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) donde se termina el contacto con el digitalizador. Se agrega el evento al registro de eventos, se quita el puntero de la colección de punteros y se actualizan los detalles del puntero.

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

-   Este controlador administra el evento [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) (cuando el contacto con el digitalizador se mantiene). Se agrega el evento al registro de eventos, se quita el puntero de la matriz de punteros y se actualizan los detalles del puntero.

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

-   Este controlador administra el evento [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964). Se agrega el evento al registro de eventos, se quita el puntero de la matriz de punteros y se actualizan los detalles del puntero.

```csharp
/// <summary>
/// The pointer canceled event handler.
/// Fires for for various reasons, including: 
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

-   Este controlador administra el evento [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965). Se agrega el evento al registro de eventos, se quita el puntero de la matriz de punteros y se actualizan los detalles del puntero.

    > [!NOTE]
    > [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965) puede producirse en lugar de [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972). La captura del puntero se puede perder por varios motivos, entre los que se incluyen la interacción del usuario, la captura mediante programación de otro puntero o la llamada a [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972).     

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

Como se indicó anteriormente, debes obtener la información más extendida del puntero desde un objeto [**Windows.UI.Input.PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) obtenido a través de los métodos [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) y [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) de [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076). Los siguientes fragmentos de código muestran cómo.

-   Primero, se crea un nuevo elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) para cada puntero.

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

-   Después, se proporciona una manera de actualizar la información del puntero en un elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) existente asociado con ese puntero.

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

## <a name="primary-pointer"></a>Puntero principal
Algunos dispositivos de entrada, como un digitalizador táctil o un panel táctil, admiten más del típico puntero único de un mouse o un lápiz (en la mayoría de los casos al admitir Surface Hub dos entradas de lápiz). 

Usa la propiedad **[IsPrimary](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties.IsPrimary)** de solo lectura de la clase **[PointerPointerProperties](https://docs.microsoft.com/uwp/api/windows.ui.input.pointerpointproperties)** para identificar y diferenciar un solo puntero principal (el puntero principal siempre es el primer puntero eliminado durante una secuencia de entrada). 

Al identificar el puntero principal, puedes usarlo para emular la entrada de lápiz o mouse, personalizar interacciones o proporcionar otra funcionalidad o interfaz de usuario específica.

> [!NOTE]
> Si el puntero principal se libera, cancela o pierde durante una secuencia de entrada, no se creará un puntero de entrada principal hasta que se inicie una nueva secuencia de entrada (una secuencia de entrada finaliza cuando todos los punteros se han liberado, cancelado o perdido).

## <a name="primary-pointer-animation-example"></a>Ejemplo de animación de puntero principal

Estos fragmentos de código muestran cómo puedes proporcionar comentarios visuales para ayudar a un usuario a diferenciar entre las entradas de puntero en tu aplicación.

Esta aplicación particular usa color y animación para resaltar el puntero principal.

![Aplicación de puntero con comentarios visuales animados](images/pointers/pointers-usercontrol-animation.gif)

**Descargar este ejemplo de [Ejemplo de entrada (UserControl con animación) de puntero](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)**

### <a name="visual-feedback"></a>Comentarios visuales

Definimos un **[UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol)**, basado en un objeto **[Ellipse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.ellipse)** de XAML, que resalta dónde está cada puntero en el lienzo y usa un **[Storyboard](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.animation.storyboard)** para animar la elipse que corresponde al puntero principal.

**Este es el XAML:**

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

### <a name="create-the-ui"></a>Crear la interfaz de usuario
La interfaz de usuario en este ejemplo se limita a la entrada **[Lienzo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas)** donde hacemos un seguimiento de los punteros y representamos los indicadores de puntero y la animación de puntero principal (si procede), junto con una barra de encabezado que contiene un contador de puntero y un identificador de puntero principal.

Aquí te mostramos MainPage.xaml:

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

Por último, definimos nuestros controladores de eventos de puntero básicos en el código subyacente MainPage.xaml.cs. No reproduciremos el código aquí, ya que los conceptos básicos quedaron cubiertos en el ejemplo anterior, pero puedes descargar el ejemplo de trabajo de [Ejemplo de entrada (UserControl con animación) de puntero](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip).

## <a name="related-articles"></a>Artículos relacionados

**Ejemplos del tema**
* [Ejemplo de entrada (básica) de puntero](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers.zip)
* [Ejemplo de entrada (UserControl con animación) de puntero](https://github.com/MicrosoftDocs/windows-topic-specific-samples/archive/uwp-pointers-animation.zip)

**Otras muestras**
* [Ejemplo de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Muestra de entrada de latencia baja](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Muestra de modo de interacción del usuario](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Muestra de elementos visuales de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)

**Muestras de archivo**
* [Entrada: muestra de eventos de entrada de usuario de XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: muestra de funcionalidades del dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: muestra de manipulaciones y gestos (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Entrada: muestra de prueba de acceso táctil](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Muestra de desplazamiento, movimiento panorámico y zoom XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: ejemplo de entrada de lápiz simplificada](http://go.microsoft.com/fwlink/p/?linkid=246570)