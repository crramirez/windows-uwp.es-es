---
Description: Recibe, procesa y administra los datos de entrada de dispositivos señaladores, como la función táctil, el mouse, la pluma o el lápiz y el panel táctil, en aplicaciones para la Plataforma universal de Windows (UWP).
title: Controlar la entrada de puntero
ms.assetid: BDBC9E33-4037-4671-9596-471DCF855C82
label: Handle pointer input
template: detail.hbs
---

# Controlar la entrada de puntero


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Recibe, procesa y administra los datos de entrada de dispositivos señaladores, como la función táctil, el mouse, la pluma o el lápiz y el panel táctil, en aplicaciones para la plataforma universal de Windows (UWP).

**API importantes**

-   [**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)
-   [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)


**Importante**  
Si implementas tu propia compatibilidad con la interacción, ten presente que los usuarios esperan una experiencia intuitiva que implica la interacción directa con los elementos de la interfaz de usuario de la aplicación. Es recomendable que modeles las interacciones personalizadas sobre la [lista de controles](https://msdn.microsoft.com/library/windows/apps/mt185406) para que todo sea coherente y pueda detectarse. Los controles de plataforma proporcionan una experiencia de interacción de usuario de la Plataforma universal de Windows (UWP) completa, incluidas interacciones estándar, efectos físicos animados, comentarios visuales y accesibilidad. Crea interacciones personalizadas solamente si existe un requisito claro y bien definido, y las interacciones básicas no admiten el escenario.


## <span id="Pointers"></span><span id="pointers"></span><span id="POINTERS"></span>Punteros


Muchas experiencias de interacción implican que el usuario identifique el objeto con el que quiere interactuar apuntando a él con dispositivos de entrada como la función táctil, mouse, pluma o lápiz y panel táctil. Dado que los datos de dispositivos de interfaz de usuario (HID) sin procesar proporcionados por estos dispositivos de entrada incluyen muchas propiedades comunes, se promueve la información en una pila de entrada unificada y expuesta como datos de puntero independiente del dispositivo consolidados. Las aplicaciones para UWP pueden entonces consumir estos datos sin que tengas que preocuparte por el dispositivo de entrada que se use.

**Nota:** Si la aplicación lo requiere, también se promueve la información específica del dispositivo desde los datos sin procesar de HID.

 

Cada punto de entrada (o contacto) de la pila de entrada se representa mediante un objeto [**Pointer**](https://msdn.microsoft.com/library/windows/apps/br227968) que se expone mediante el parámetro [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) proporcionado por diversos eventos de puntero. En el caso de lápices múltiples o entrada multitáctil, cada contacto se trata como un punto de entrada único.

## <span id="Pointer_events"></span><span id="pointer_events"></span><span id="POINTER_EVENTS"></span>Eventos de puntero


Los eventos de puntero exponen información básica, como el estado de detección (en un intervalo o un contacto) y el tipo de dispositivo, e información extendida, como la ubicación, la presión y la geometría de contacto. Además, también están disponibles las propiedades del dispositivo específico, como qué botón del mouse presionó un usuario o si un usuario está usando el extremo borrador del lápiz. Si la aplicación necesita diferenciar entre distintos dispositivos de entrada y sus funcionalidades, consulta el tema de [identificar dispositivos de entrada](identify-input-devices.md).

Las aplicaciones para UWP pueden escuchar los siguientes eventos de puntero:

**Nota:** Llama a [**CapturePointer**](https://msdn.microsoft.com/library/windows/apps/br208918) para limitar la entrada del puntero a un elemento específico de la interfaz de usuario. Cuando un elemento captura un puntero, solo ese objeto recibe los eventos de entrada del puntero, incluso aunque se mueva el puntero fuera del área límite del objeto. Normalmente capturas el puntero dentro de un controlador de eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) ya que [**IsInContact**](https://msdn.microsoft.com/library/windows/apps/br227976) (botón del mouse presionado, función táctil o pluma en contacto) debe ser verdadero para que **CapturePointer** se realice correctamente.

 

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
<td align="left"><p><span id="PointerCanceled"></span><span id="pointercanceled"></span><span id="POINTERCANCELED"></span>[<strong>PointerCanceled</strong>](https://msdn.microsoft.com/library/windows/apps/br208964)</p></td>
<td align="left"><p>Se produce cuando la plataforma cancela un puntero.</p>
<ul>
<li>Los punteros táctiles se cancelan cuando se detecta un lápiz dentro del alcance de la superficie de entrada.</li>
<li>No se detectan contactos activos de más de 100 ms.</li>
<li>Se modifica el monitor o la pantalla (resolución, configuración, configuración de multimonitor).</li>
<li>El escritorio está bloqueado o el usuario ha cerrado la sesión.</li>
<li>El número de contactos simultáneos superó el número admitido por el dispositivo.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerCaptureLost"></span><span id="pointercapturelost"></span><span id="POINTERCAPTURELOST"></span>[<strong>PointerCaptureLost</strong>](https://msdn.microsoft.com/library/windows/apps/br208965)</p></td>
<td align="left"><p>Se produce cuando otro elemento de la interfaz de usuario captura el puntero, si se libera el puntero o si se captura otro puntero mediante programación.</p>
<div class="alert">
<strong>Nota:</strong> No hay ningún evento de captura de puntero correspondiente.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="PointerEntered"></span><span id="pointerentered"></span><span id="POINTERENTERED"></span>[<strong>PointerEntered</strong>](https://msdn.microsoft.com/library/windows/apps/br208968)</p></td>
<td align="left"><p>Se produce cuando un puntero entra en el área límite de un elemento. Esto puede ocurrir de forma ligeramente distinta para la función táctil, de panel táctil, mouse y de pluma.</p>
<ul>
<li>La función táctil requiere un contacto del dedo para desencadenar este evento, bien sea de una pulsación directa hacia abajo o de movimiento en el área límite del elemento.</li>
<li>Tanto el mouse como el panel táctil tienen un cursor en pantalla que siempre está visible y desencadena este evento incluso si no se presiona ningún botón del mouse o del teclado táctil.</li>
<li>Al igual que la función táctil, la pluma desencadena este evento con un movimiento directo hacia abajo del lápiz sobre el elemento o un movimiento en el área límite del elemento. Sin embargo, la pluma también tiene un estado cuando se mantiene el puntero encima ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)) que, cuando tiene un valor true, desencadena este evento.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerExited"></span><span id="pointerexited"></span><span id="POINTEREXITED"></span>[<strong>PointerExited</strong>](https://msdn.microsoft.com/library/windows/apps/br208969)</p></td>
<td align="left"><p>Se produce cuando un puntero sale del área límite de un elemento. Esto puede ocurrir de forma ligeramente distinta para la función táctil, de panel táctil, mouse y de pluma.</p>
<ul>
<li>La función táctil requiere un contacto con el dedo y desencadena este evento cuando el puntero se mueve fuera del área límite del elemento.</li>
<li>Tanto el mouse como el panel táctil tienen un cursor en pantalla que siempre está visible y desencadena este evento incluso si no se presiona ningún botón del mouse o del teclado táctil.</li>
<li>Al igual que la función táctil, la pluma desencadena este evento cuando se mueve fuera del área límite del elemento. Sin embargo, la pluma también tiene un estado cuando se mantiene el puntero encima ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)) que desencadena este evento cuando cambia el estado de true a false.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="PointerMoved"></span><span id="pointermoved"></span><span id="POINTERMOVED"></span>[<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970)</p></td>
<td align="left"><p>Se produce cuando un puntero modifica sus coordenadas, el estado de los botones, la presión, la inclinación o la geometría de contacto (por ejemplo, el ancho y alto) dentro del área límite de un elemento. Esto puede ocurrir de forma ligeramente distinta para la función táctil, de panel táctil, mouse y de pluma.</p>
<ul>
<li>La función táctil requiere un contacto con el dedo y desencadena este evento solo cuando está en contacto dentro del área límite del elemento.</li>
<li>Tanto el mouse como el panel táctil tienen un cursor en pantalla que siempre está visible y desencadena este evento incluso si no se presiona ningún botón del mouse o del teclado táctil.</li>
<li>Al igual que la función táctil, la pluma desencadena este evento cuando está en contacto dentro del área límite del elemento. Sin embargo, la pluma también tiene un estado cuando se mantiene el puntero encima ([<strong>IsInRange</strong>](https://msdn.microsoft.com/library/windows/apps/br227977)) que, cuando tiene un valor true y está dentro del área límite del elemento, desencadena este evento.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerPressed"></span><span id="pointerpressed"></span><span id="POINTERPRESSED"></span>[<strong>PointerPressed</strong>](https://msdn.microsoft.com/library/windows/apps/br208971)</p></td>
<td align="left"><p>Se produce cuando el puntero indica una acción de presión (como un toque, presionar el botón del mouse, presionar con el lápiz o presionar el botón de panel táctil) dentro del área límite de un elemento.</p>
<p>Debe llamarse a [<strong>CapturePointer</strong>](https://msdn.microsoft.com/library/windows/apps/br208918) desde el controlador de este evento.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><span id="PointerReleased"></span><span id="pointerreleased"></span><span id="POINTERRELEASED"></span>[<strong>PointerReleased</strong>](https://msdn.microsoft.com/library/windows/apps/br208972)</p></td>
<td align="left"><p>Se produce cuando el puntero indica una acción de liberar (por ejemplo, finalizar un toque, soltar el botón del mouse, levantar el lápiz o soltar el botón del panel táctil) dentro del área límite de un elemento o, si se captura el puntero, fuera del área límite.</p></td>
</tr>
<tr class="even">
<td align="left"><p><span id="PointerWheelChanged"></span><span id="pointerwheelchanged"></span><span id="POINTERWHEELCHANGED"></span>[<strong>PointerWheelChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208973)</p></td>
<td align="left"><p>Se produce cuando se gira la rueda del mouse.</p>
<p>La entrada de mouse se asocia con un solo puntero que se asigna cuando se detecta por primera vez la entrada. Al hacer clic en un botón del mouse (izquierdo, rueda o derecho), se crea una asociación secundaria entre el puntero y ese botón a través del evento [<strong>PointerMoved</strong>](https://msdn.microsoft.com/library/windows/apps/br208970).</p></td>
</tr>
</tbody>
</table>

 

## <span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo


A continuación se muestran algunos ejemplos de código de una aplicación básica de seguimiento del puntero que muestra cómo escuchar y controlar eventos de puntero y obtener diversas propiedades de punteros activos.

### <span id="Create_the_UI"></span><span id="create_the_ui"></span><span id="CREATE_THE_UI"></span>Crear la interfaz de usuario

Para este ejemplo, usamos un rectángulo (`targetContainer`) como el objeto de destino de la entrada de puntero. El color del destino cambia cuando cambia el estado del puntero.

Los detalles de cada puntero se muestran en un bloque de texto flotante que se mueve con el puntero. Los eventos de puntero en sí se muestran a la izquierda del rectángulo (para informar de la secuencia de eventos).

Este es el lenguaje de marcado de aplicaciones extensible (XAML) para este ejemplo.

```XAML
<Page
    x:Class="PointerInput.MainPage"
    IsTabStop="false"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:PointerInput"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    Name="page">

    <Grid Background="{StaticResource ApplicationForegroundThemeBrush}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="69.458" />
            <ColumnDefinition Width="80.542"/>
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
        <Button Name="buttonClear"
                Foreground="White"
                Width="100"
                Height="100">
            clear
        </Button>
        <TextBox Name="eventLog" 
                 Grid.Column="1"
                 Grid.Row="0"
                 Grid.RowSpan="3" 
                 Background="#000000" 
                 TextWrapping="Wrap" 
                 Foreground="#FFFFFF" 
                 ScrollViewer.VerticalScrollBarVisibility="Visible" 
                 BorderThickness="0" Grid.ColumnSpan="2"/>
    </Grid>
</Page>
```

### <span id="Listen_for_pointer_events"></span><span id="listen_for_pointer_events"></span><span id="LISTEN_FOR_POINTER_EVENTS"></span>Escuchar eventos de puntero

En la mayoría de los casos, es recomendable que obtengas la información del puntero mediante el elemento [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076) del controlador de eventos.

Si el argumento del evento no expone los detalles de puntero necesarios, puedes acceder a la información extendida de [**PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) que se expone a través de los métodos [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) y [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) de [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076).

Para este ejemplo, se utiliza un rectángulo (`targetContainer`) como el objeto de destino de la entrada de puntero. El color del destino cambia cuando cambia el estado del puntero.

El siguiente código configura el objeto de destino, declara variables globales e identifica los diversos agentes de escucha de eventos de puntero para el destino.

```CSharp
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }

```

### <span id="Handle_pointer_events"></span><span id="handle_pointer_events"></span><span id="HANDLE_POINTER_EVENTS"></span>Controlar eventos de puntero

Después, se usan comentarios de interfaz de usuario para mostrar controladores de eventos de puntero básicos.

-   Este controlador administra un evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). Se agrega el evento al registro de eventos, se agrega el puntero a la matriz de punteros que se utiliza para realizar un seguimiento de los punteros de interés y se muestran los detalles de los punteros.

    **Nota:** Los eventos [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971) y [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) no siempre se producen en parejas. La aplicación debe escuchar y controlar cualquier evento que pueda concluir una acción de puntero hacia abajo (como [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969), [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964) y [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965)).

     

```    CSharp
        // PointerPressed and PointerReleased events do not always occur in pairs. 
            // Your app should listen for and handle any event that might conclude a pointer down action 
            // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
            // For this example, we track the number of contacts in case the 
            // number of contacts has reached the maximum supported by the device.
            // Depending on the device, additional contacts might be ignored 
            // (PointerPressed not fired). 
            void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nDown: " + ptr.PointerId;

                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Lock the pointer to the target.
                Target.CapturePointer(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer captured: " + ptr.PointerId;

                // Check if pointer already exists (for example, enter occurred prior to press).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }
                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   Este controlador administra un evento [**PointerEntered**](https://msdn.microsoft.com/library/windows/apps/br208968). Se agrega el evento al registro de eventos, se agrega el puntero a la colección de punteros y se muestran los detalles del puntero.

```    CSharp
        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
            {
                Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

                // Update event sequence.
                eventLog.Text += "\nEntered: " + ptr.PointerId;

                if (contacts.Count == 0)
                {
                    // Change background color of target when pointer contact detected.
                    Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
                }

                // Check if pointer already exists (if enter occurred prior to down).
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    return;
                }

                // Add contact to dictionary.
                contacts[ptr.PointerId] = ptr;
                ++numActiveContacts;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;

                // Display pointer details.
                createInfoPop(e);
            }
```

-   Este controlador administra un evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970). Se agrega el evento al registro de eventos y se actualizan los detalles del puntero.

    **Importante:** La entrada del mouse se asocia con un puntero único que se asigna cuando se detecta por primera vez la entrada del mouse. Al hacer clic en un botón del mouse (izquierdo, rueda o derecho), se crea una asociación secundaria entre el puntero y ese botón a través del evento [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208971). El evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) solo se desencadena cuando se libera ese mismo botón del mouse (no se puede asociar ningún otro botón con el puntero hasta que se complete este evento). Debido a esta asociación exclusiva, los clics de otros botones del mouse se enrutan a través del evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208970).

     

```    CSharp
private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

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
        if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // To get mouse state, we need extended pointer details.
            // We get the pointer info through the getCurrentPoint method
            // of the event argument. 
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            if (ptrPt.Properties.IsLeftButtonPressed)
            {
                eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsMiddleButtonPressed)
            {
                eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
            }
            if (ptrPt.Properties.IsRightButtonPressed)
            {
                eventLog.Text += "\nRight button: " + ptrPt.PointerId;
            }
        }

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        updateInfoPop(e);
    }
```

-   Este controlador administra un evento [**PointerWheelChanged**](https://msdn.microsoft.com/library/windows/apps/br208973). Se agrega el evento al registro de eventos, se agrega el puntero a la matriz de punteros (si es necesario) y se muestran los detalles del puntero.

```    CSharp
private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

        // Check if pointer already exists (for example, enter occurred prior to wheel).
        if (contacts.ContainsKey(ptr.PointerId))
        {
            return;
        }

        // Add contact to dictionary.
        contacts[ptr.PointerId] = ptr;
        ++numActiveContacts;

        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;

        // Display pointer details.
        createInfoPop(e);
    }
```

-   Este controlador administra un evento [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972) donde se termina el contacto con el digitalizador. Se agrega el evento al registro de eventos, se quita el puntero de la colección de punteros y se actualizan los detalles del puntero.

```    CSharp
void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nUp: " + ptr.PointerId;

        // If event source is mouse or touchpad and the pointer is still 
        // over the target, retain pointer and pointer details.
        // Return without removing pointer from pointers dictionary.
        // For this example, we assume a maximum of one mouse pointer.
        if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
        {
            // Update target UI.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

            destroyInfoPop(ptr);

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Release the pointer from the target.
            Target.ReleasePointerCapture(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer released: " + ptr.PointerId;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }
        else
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
        }

    }
```

-   Este controlador administra un evento [**PointerExited**](https://msdn.microsoft.com/library/windows/apps/br208969) donde el contacto con el digitalizador se mantiene. Se agrega el evento al registro de eventos, se quita el puntero de la matriz de punteros y se actualizan los detalles del puntero.

```    CSharp
private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer exited: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        // Update the UI and pointer details.
        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
        }
        
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   Este controlador administra un evento [**PointerCanceled**](https://msdn.microsoft.com/library/windows/apps/br208964). Se agrega el evento al registro de eventos, se quita el puntero de la matriz de punteros y se actualizan los detalles del puntero.

```    CSharp
// Fires for for various reasons, including: 
    //    - Touch contact canceled by pen coming into range of the surface.
    //    - The device doesn&#39;t report an active contact for more than 100ms.
    //    - The desktop is locked or the user logged off. 
    //    - The number of simultaneous contacts exceeded the number supported by the device.
    private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

-   Este controlador administra un evento [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965). Se agrega el evento al registro de eventos, se quita el puntero de la matriz de punteros y se actualizan los detalles del puntero.

    **Nota:** [**PointerCaptureLost**](https://msdn.microsoft.com/library/windows/apps/br208965) puede producirse en lugar de [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208972). La captura del puntero se puede perder por varios motivos.

     

```    CSharp
// Fires for for various reasons, including: 
    //    - User interactions
    //    - Programmatic capture of another pointer
    //    - Captured pointer was deliberately released
    // PointerCaptureLost can fire instead of PointerReleased. 
    private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
    {
        Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

        // Update event sequence.
        eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

        // Remove contact from dictionary.
        if (contacts.ContainsKey(ptr.PointerId))
        {
            contacts[ptr.PointerId] = null;
            contacts.Remove(ptr.PointerId);
            --numActiveContacts;
        }

        destroyInfoPop(ptr);

        if (contacts.Count == 0)
        {
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
        }
        // Prevent most handlers along the event route from handling the same event again.
        e.Handled = true;
    }
```

### <span id="Get_pointer_properties"></span><span id="get_pointer_properties"></span><span id="GET_POINTER_PROPERTIES"></span>Obtener las propiedades del puntero

Como se indicó anteriormente, debes obtener la información más extendida del puntero desde un objeto [**Windows.UI.Input.PointerPoint**](https://msdn.microsoft.com/library/windows/apps/br242038) obtenido a través de los métodos [**GetCurrentPoint**](https://msdn.microsoft.com/library/windows/apps/hh943077) y [**GetIntermediatePoints**](https://msdn.microsoft.com/library/windows/apps/hh943078) de [**PointerRoutedEventArgs**](https://msdn.microsoft.com/library/windows/apps/hh943076).

-   Primero, se crea un nuevo elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) para cada puntero.

```    CSharp
        void createInfoPop(PointerRoutedEventArgs e)
            {
                TextBlock pointerDetails = new TextBlock();
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                pointerDetails.Name = ptrPt.PointerId.ToString();
                pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
                pointerDetails.Text = queryPointer(ptrPt);

                TranslateTransform x = new TranslateTransform();
                x.X = ptrPt.Position.X + 20;
                x.Y = ptrPt.Position.Y + 20;
                pointerDetails.RenderTransform = x;

                Container.Children.Add(pointerDetails);
            }
```

-   Después, se proporciona una manera de actualizar la información del puntero en un elemento [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) existente asociado con ese puntero.

```    CSharp
        void updateInfoPop(PointerRoutedEventArgs e)
            {
                foreach (var pointerDetails in Container.Children)
                {
                    if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                    {
                        TextBlock _TextBlock = (TextBlock)pointerDetails;
                        if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                        {
                            // To get pointer location details, we need extended pointer info.
                            // We get the pointer info through the getCurrentPoint method
                            // of the event argument. 
                            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                            TranslateTransform x = new TranslateTransform();
                            x.X = ptrPt.Position.X + 20;
                            x.Y = ptrPt.Position.Y + 20;
                            pointerDetails.RenderTransform = x;
                            _TextBlock.Text = queryPointer(ptrPt);
                        }
                    }
                }
            }
```

-   Por último, se consultan diversas propiedades del puntero.

```    CSharp
         String queryPointer(PointerPoint ptrPt)
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

                 GeneralTransform gt = Target.TransformToVisual(page);
                 Point screenPoint;

                 screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
                 details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                     "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                     "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
                 return details;
             }
```

### <span id="Complete_example"></span><span id="complete_example"></span><span id="COMPLETE_EXAMPLE"></span>Ejemplo completo

A continuación se muestra el código de C\# de este ejemplo. Para obtener vínculos a ejemplos más complejos, consulta los artículos relacionados en la parte inferior de esta página.

```CSharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Input;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/?LinkId=234238

namespace PointerInput
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // For this example, we track simultaneous contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        uint numActiveContacts;
        Windows.Devices.Input.TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();

        // Dictionary to maintain information about each active contact. 
        // An entry is added during PointerPressed/PointerEntered events and removed 
        // during PointerReleased/PointerCaptureLost/PointerCanceled/PointerExited events.
        Dictionary<uint, Windows.UI.Xaml.Input.Pointer> contacts;

        public MainPage()
        {
            this.InitializeComponent();
            numActiveContacts = 0;
            // Initialize the dictionary.
            contacts = new Dictionary<uint, Windows.UI.Xaml.Input.Pointer>((int)touchCapabilities.Contacts);
            // Declare the pointer event handlers.
            Target.PointerPressed += new PointerEventHandler(Target_PointerPressed);
            Target.PointerEntered += new PointerEventHandler(Target_PointerEntered);
            Target.PointerReleased += new PointerEventHandler(Target_PointerReleased);
            Target.PointerExited += new PointerEventHandler(Target_PointerExited);
            Target.PointerCanceled += new PointerEventHandler(Target_PointerCanceled);
            Target.PointerCaptureLost += new PointerEventHandler(Target_PointerCaptureLost);
            Target.PointerMoved += new PointerEventHandler(Target_PointerMoved);
            Target.PointerWheelChanged += new PointerEventHandler(Target_PointerWheelChanged);

            buttonClear.Click += new RoutedEventHandler(ButtonClear_Click);
        }

        private void ButtonClear_Click(object sender, RoutedEventArgs e)
        {
            eventLog.Text = "";
        }


        // PointerPressed and PointerReleased events do not always occur in pairs. 
        // Your app should listen for and handle any event that might conclude a pointer down action 
        // (such as PointerExited, PointerCanceled, and PointerCaptureLost).
        // For this example, we track the number of contacts in case the 
        // number of contacts has reached the maximum supported by the device.
        // Depending on the device, additional contacts might be ignored 
        // (PointerPressed not fired). 
        void Target_PointerPressed(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nDown: " + ptr.PointerId;

            // Change background color of target when pointer contact detected.
            Target.Fill = new SolidColorBrush(Windows.UI.Colors.Green);

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Lock the pointer to the target.
            Target.CapturePointer(e.Pointer);

            // Update event sequence.
            eventLog.Text += "\nPointer captured: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to press).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }
            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Display pointer details.
            createInfoPop(e);
        }

        void Target_PointerReleased(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nUp: " + ptr.PointerId;

            // If event source is mouse or touchpad and the pointer is still 
            // over the target, retain pointer and pointer details.
            // Return without removing pointer from pointers dictionary.
            // For this example, we assume a maximum of one mouse pointer.
            if (ptr.PointerDeviceType != Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // Update target UI.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);

                destroyInfoPop(ptr);

                // Remove contact from dictionary.
                if (contacts.ContainsKey(ptr.PointerId))
                {
                    contacts[ptr.PointerId] = null;
                    contacts.Remove(ptr.PointerId);
                    --numActiveContacts;
                }

                // Release the pointer from the target.
                Target.ReleasePointerCapture(e.Pointer);

                // Update event sequence.
                eventLog.Text += "\nPointer released: " + ptr.PointerId;

                // Prevent most handlers along the event route from handling the same event again.
                e.Handled = true;
            }
            else
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

        }

        private void Target_PointerMoved(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

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
            if (ptr.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse)
            {
                // To get mouse state, we need extended pointer details.
                // We get the pointer info through the getCurrentPoint method
                // of the event argument. 
                Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                if (ptrPt.Properties.IsLeftButtonPressed)
                {
                    eventLog.Text += "\nLeft button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsMiddleButtonPressed)
                {
                    eventLog.Text += "\nWheel button: " + ptrPt.PointerId;
                }
                if (ptrPt.Properties.IsRightButtonPressed)
                {
                    eventLog.Text += "\nRight button: " + ptrPt.PointerId;
                }
            }

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            updateInfoPop(e);
        }

        private void Target_PointerEntered(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nEntered: " + ptr.PointerId;

            if (contacts.Count == 0)
            {
                // Change background color of target when pointer contact detected.
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Blue);
            }

            // Check if pointer already exists (if enter occurred prior to down).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        private void Target_PointerWheelChanged(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nMouse wheel: " + ptr.PointerId;

            // Check if pointer already exists (for example, enter occurred prior to wheel).
            if (contacts.ContainsKey(ptr.PointerId))
            {
                return;
            }

            // Add contact to dictionary.
            contacts[ptr.PointerId] = ptr;
            ++numActiveContacts;

            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;

            // Display pointer details.
            createInfoPop(e);
        }

        // Fires for for various reasons, including: 
        //    - User interactions
        //    - Programmatic capture of another pointer
        //    - Captured pointer was deliberately released
        // PointerCaptureLost can fire instead of PointerReleased. 
        private void Target_PointerCaptureLost(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer capture lost: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        // Fires for for various reasons, including: 
        //    - Touch contact canceled by pen coming into range of the surface.
        //    - The device doesn&#39;t report an active contact for more than 100ms.
        //    - The desktop is locked or the user logged off. 
        //    - The number of simultaneous contacts exceeded the number supported by the device.
        private void Target_PointerCanceled(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer canceled: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Black);
            }
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        private void Target_PointerExited(object sender, PointerRoutedEventArgs e)
        {
            Windows.UI.Xaml.Input.Pointer ptr = e.Pointer;

            // Update event sequence.
            eventLog.Text += "\nPointer exited: " + ptr.PointerId;

            // Remove contact from dictionary.
            if (contacts.ContainsKey(ptr.PointerId))
            {
                contacts[ptr.PointerId] = null;
                contacts.Remove(ptr.PointerId);
                --numActiveContacts;
            }

            // Update the UI and pointer details.
            destroyInfoPop(ptr);

            if (contacts.Count == 0)
            {
                Target.Fill = new SolidColorBrush(Windows.UI.Colors.Red);
            }
            
            // Prevent most handlers along the event route from handling the same event again.
            e.Handled = true;
        }

        void createInfoPop(PointerRoutedEventArgs e)
        {
            TextBlock pointerDetails = new TextBlock();
            Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
            pointerDetails.Name = ptrPt.PointerId.ToString();
            pointerDetails.Foreground = new SolidColorBrush(Windows.UI.Colors.White);
            pointerDetails.Text = queryPointer(ptrPt);

            TranslateTransform x = new TranslateTransform();
            x.X = ptrPt.Position.X + 20;
            x.Y = ptrPt.Position.Y + 20;
            pointerDetails.RenderTransform = x;

            Container.Children.Add(pointerDetails);
        }

        void destroyInfoPop(Windows.UI.Xaml.Input.Pointer ptr)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == ptr.PointerId.ToString())
                    {
                        Container.Children.Remove(pointerDetails);
                    }
                }
            }
        }

        void updateInfoPop(PointerRoutedEventArgs e)
        {
            foreach (var pointerDetails in Container.Children)
            {
                if (pointerDetails.GetType().ToString() == "Windows.UI.Xaml.Controls.TextBlock")
                {
                    TextBlock _TextBlock = (TextBlock)pointerDetails;
                    if (_TextBlock.Name == e.Pointer.PointerId.ToString())
                    {
                        // To get pointer location details, we need extended pointer info.
                        // We get the pointer info through the getCurrentPoint method
                        // of the event argument. 
                        Windows.UI.Input.PointerPoint ptrPt = e.GetCurrentPoint(Target);
                        TranslateTransform x = new TranslateTransform();
                        x.X = ptrPt.Position.X + 20;
                        x.Y = ptrPt.Position.Y + 20;
                        pointerDetails.RenderTransform = x;
                        _TextBlock.Text = queryPointer(ptrPt);
                    }
                }
            }
        }

         String queryPointer(PointerPoint ptrPt)
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

             GeneralTransform gt = Target.TransformToVisual(page);
             Point screenPoint;

             screenPoint = gt.TransformPoint(new Point(ptrPt.Position.X, ptrPt.Position.Y));
             details += "\nPointer Id: " + ptrPt.PointerId.ToString() +
                 "\nPointer location (parent): " + ptrPt.Position.X + ", " + ptrPt.Position.Y +
                 "\nPointer location (screen): " + screenPoint.X + ", " + screenPoint.Y;
             return details;
         }
    }
}
```

## <span id="related_topics"></span>Artículos relacionados


**Ejemplos**
* [Ejemplo de entrada básica](http://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Ejemplo de entrada de latencia baja](http://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Ejemplo de modo de interacción del usuario](http://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Muestra de elementos visuales de foco](http://go.microsoft.com/fwlink/p/?LinkID=619895)
**Muestras de archivo**
* [Entrada: muestra de eventos de entrada de usuario de XAML](http://go.microsoft.com/fwlink/p/?linkid=226855)
* [Entrada: muestra de funcionalidades del dispositivo](http://go.microsoft.com/fwlink/p/?linkid=231530)
* [Entrada: muestra de manipulaciones y gestos (C++)](http://go.microsoft.com/fwlink/p/?linkid=231605)
* [Entrada: muestra de prueba de acceso táctil](http://go.microsoft.com/fwlink/p/?linkid=231590)
* [Muestra de desplazamiento, movimiento panorámico y zoom XAML](http://go.microsoft.com/fwlink/p/?linkid=251717)
* [Entrada: ejemplo de entrada de lápiz simplificada](http://go.microsoft.com/fwlink/p/?linkid=246570)
 

 






<!--HONumber=Mar16_HO4-->


