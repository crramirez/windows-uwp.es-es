---
title: Agregar controles
description: Echemos un vistazo ahora al modo en que la muestra de juego implementa los controles de movimiento y vista en un juego 3-D, y cómo va a desarrollar controles básicos para mandos, función táctil y mouse.
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, controles, entrada
ms.localizationpriority: medium
ms.openlocfilehash: 0ff7088ec4062973d0b9d1ff6d20d7992e4135c3
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367955"
---
# <a name="add-controls"></a>Agregar controles


\[ Se actualizó con las aplicaciones para UWP en Windows 10. Para artículos de Windows 8.x, consulte el [archive](https://go.microsoft.com/fwlink/p/?linkid=619132) \]

Un buen juego de la plataforma universal de Windows (UWP) admite una amplia variedad de interfaces. Un reproductor posibles podría tener Windows 10 en una tableta con ningún botón físico, un equipo con un controlador Xbox asociado o la plataforma de juegos de escritorio más reciente con un mouse de alto rendimiento y el teclado de juegos. En nuestro juego los controles se implementan en la clase [**MoveLookController**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp). Esta clase concentra los tres tipos de entrada (mouse y teclado, táctil y controlador para juegos) en un único controlador. El resultado final es un tiros en primera persona que usa controles de movimiento y vista estándares en el género y que funcionan con varios dispositivos.

> [!NOTE]
> Para obtener más información acerca de los controles, consulta [Controles de movimiento y vista para juegos](tutorial--adding-move-look-controls-to-your-directx-game.md) y [Controles táctiles para juegos](tutorial--adding-touch-controls-to-your-directx-game.md).


## <a name="objective"></a>Objetivo

En este momento tenemos un juego que presenta contenido, pero no podemos mover nuestro jugador ni disparar a los objetivos. Echaremos un vistazo a cómo implementa nuestro juego los controles de movimiento y vista de tiros en primera persona y para los siguientes tipos de entrada en nuestro juego DirectX UWP.
- Mouse y teclado
- Función táctil
- Controlador para juegos

>[!Note]
>Si no has descargado el código del juego más reciente para esta muestra, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="common-control-behaviors"></a>Comportamientos de controles comunes


Los controles táctiles y los controles de mouse o teclado tienen una implementación principal muy parecida. En una aplicación para UWP, un puntero es simplemente un punto en la pantalla. Puedes moverlo desplazando el ratón o deslizando un dedo por la pantalla táctil. Como resultado, puedes registrar un único conjunto de eventos y despreocuparte de si el jugador usa un mouse o una pantalla táctil para mover y presionar el puntero.

Cuando la clase **MoveLookController** en la muestra de juego se inicializa, registra cuatro eventos específicos de puntero y uno específico de ratón:

Evento | Descripción
:------ | :-------
[**CoreWindow::PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerpressed) | Se presionó (o se mantuvo presionado) el botón izquierdo o derecho del mouse o se tocó la superficie táctil.
[**CoreWindow::PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved) |El mouse se movió, o se realizó una acción de arrastrar en la superficie táctil.
[**CoreWindow::PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerreleased) |Se soltó el botón izquierdo del mouse, o se retiró el objeto que estaba en contacto con la superficie táctil.
[**CoreWindow::PointerExited**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerexited) |El puntero se movió fuera de la ventana principal.
[**Windows::Devices::Input::MouseMoved**](https://docs.microsoft.com/uwp/api/windows.devices.input.mousedevice.mousemoved) | El mouse se desplazó una distancia determinada. Ten en cuenta que solo nos interesan los valores delta del movimiento de ratón, no la posición actual en las coordenadas X-Y.


Estos controladores de eventos se establecen para empezar a escuchar la entrada del usuario en cuanto **MoveLookController** se inicializa en la ventana de la aplicación.
```cpp
void MoveLookController::InitWindow(_In_ CoreWindow^ window)
{
    ResetState();

    window->PointerPressed +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->PointerExited +=
        ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerExited);

    // There is a separate handler for mouse only relative mouse movement events.
    MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);
    //
    // ...
    //
}
```

El código completo de [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92) se puede ver en GitHub.


Para determinar el momento en el que el juego debería estar escuchando una entrada determinada, la clase **MoveLookController** tiene tres estados específicos del controlador, independientemente del tipo de control:

Estado | Descripción
:----- | :-------
**Ninguno** | Es el estado inicializado para el mando. Se ignoran todas las entradas porque el juego no espera la entrada de ningún controlador.
**WaitForInput** | El controlador está esperando que el jugador confirme un mensaje desde el juego mediante un clic con el botón izquierdo del mouse, un evento táctil, o con el botón de menú en un controlador para juegos.
**Active** | El controlador está en modo de juego activo.



### <a name="waitforinput-state-and-pausing-the-game"></a>Estado WaitForInput y pausa en el juego

El juego entra en el estado **WaitForInput** cuando se ha pausado. Esto ocurre si el jugador mueve el puntero fuera de la ventana principal del juego o presiona el botón de pausa (la tecla P o el botón **Inicio** del controlador para juegos). **MoveLookController** registra dicha presión e informa al bucle de juego cuando llama al método [**IsPauseRequested**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127). En ese punto, si **IsPauseRequested** devuelve **verdadero**, el bucle de juego llama a **WaitForPress** en **MoveLookController** para mover el controlador al estado **WaitForInput**. 


Una vez en el estado **WaitForInput**, el juego detiene el procesamiento de casi todos los eventos de entrada de juego hasta que regrese al estado **Activo**. La excepción es el botón de pausa, al presionarlo, el juego vuelve al estado activo. Que no sea el botón de pausa, en orden del juego volver a la **Active** estado necesita el Reproductor seleccionar un elemento de menú. 



### <a name="the-active-state"></a>El estado activo

Durante el estado **Activo**, la instancia **MoveLookController** está procesando eventos desde todos los dispositivos de entrada habilitados e interpretando las intenciones del jugador. El resultado es que actualiza la velocidad y la dirección de la vista del jugador, y comparte los datos actualizados con el juego después de que se llame a **Actualizar** desde el bucle del juego.


Todas las entradas del puntero se registran en el estado **Activo**, con diferentes id. de puntero correspondientes a los distintas acciones de puntero.
Cuando se recibe un evento [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerpressed), **MoveLookController** obtiene el valor del identificador del puntero creado por la ventana. El id. de puntero represente un tipo específico de entrada. Por ejemplo, en un dispositivo multitoque, puede que haya varias entradas activas diferentes al mismo tiempo. Los identificadores se usan para llevar un seguimiento de la entrada que está usando el jugador. Si un evento se encuentra en el rectángulo de movimiento de la pantalla táctil, se asigna un id. de puntero para realizar un seguimiento de cualquier evento de puntero en el rectángulo de movimiento. Otros eventos de puntero en el rectángulo de disparo se siguen por separado, con id. de puntero diferentes.


> [!NOTE]
> Las entradas del mouse y de la palanca de control derecha de un controlador para juegos también tienen identificadores que se administran por separado.

Una vez que los eventos de puntero se han asignado a una acción específica del juego, es hora de actualizar los datos que el objeto **MoveLookController** comparte con el bucle de juego principal.

Cuando se llama, el [ **actualización** ](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) método en el ejemplo de juego procesa la entrada y actualiza las variables de dirección de velocidad y el aspecto (**m\_velocity** y **m\_lookdirection**), que, a continuación, recupera el bucle de juego mediante una llamada a público [ **Velocity** ](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909) y [ **LookDirection** ](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923) métodos.

> [!NOTE]
> Más detalles sobre el método [**Update**](#the-update-method) se pueden ver más adelante en esta página.




El bucle de juego puede probar si el jugador está disparando llamando al método **IsFiring** en la instancia **MoveLookController**. El **MoveLookController** comprueba para ver si el jugador ha presionado el botón de disparo en uno de los tres tipos de entrada.

```cpp
bool MoveLookController::IsFiring()
{
    if (m_state == MoveLookControllerState::Active)
    {
        if (m_autoFire)
        {
            return (m_fireInUse || (m_mouseInUse && m_mouseLeftInUse) || PollingFireInUse());
        }
        else
        {
            if (m_firePressed)
            {
                m_firePressed = false;
                return true;
            }
        }
    }
    return false;
}
```









Ahora, echemos un vistazo a la implementación de cada uno de los tres tipos de control con un poco más de detalle.

## <a name="adding-relative-mouse-controls"></a>Añadir controles de mouse relativos


Si se detecta un movimiento de mouse, queremos usarlo para averiguar la nueva rotación alrededor del eje 'x' y del eje 'y' de la cámara. Esto se logra implementando controles de ratón relativos, con los que controlamos la distancia que el ratón ha recorrido (el delta entre el inicio del movimiento hasta que el ratón se detiene ) en contraposición a grabar las coordenadas de píxel x-y absolutas del movimiento.

Para ello, obtenemos los cambios en las coordenadas X (movimiento horizontal) e Y (movimiento vertical) examinando los campos [**MouseDelta::X**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input.MouseDelta) y **MouseDelta::Y** del objeto de argumento [**Windows::Device::Input::MouseEventArgs::MouseDelta**](https://docs.microsoft.com/uwp/api/windows.devices.input.mouseeventargs.mousedelta) que el evento [**MouseMoved**](https://docs.microsoft.com/uwp/api/windows.devices.input.mousedevice.mousemoved) devuelve.

```cpp
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice^ /* mouseDevice */,
    _In_ MouseEventArgs^ args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args->MouseDelta.X);
        mouseDelta.y = static_cast<float>(args->MouseDelta.Y);

        XMFLOAT2 rotationDelta;
        rotationDelta.x  = mouseDelta.x * MoveLookConstants::RotationGain;   // scale for control sensitivity
        rotationDelta.y  = mouseDelta.y * MoveLookConstants::RotationGain;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in same range by wrapping.
        if (m_yaw >  XM_PI)
        {
            m_yaw -= XM_PI * 2.0f;
        }
        else if (m_yaw < -XM_PI)
        {
            m_yaw += XM_PI * 2.0f;
        }
        break;
    }
}
```

## <a name="adding-touch-support"></a>Añadir compatibilidad con la función táctil.

Los controles táctiles son geniales para los usuarios con tabletas. Este juego recopila entrada táctil desactivando ciertas áreas de la pantalla con cada alineación para realizar determinadas acciones incluidas en el juego.
La entrada táctil de este juego usa tres zonas.

![Diseño táctil de movimiento y vista](images/simple-dx-game-controls-touchzones.png)

Los siguientes comandos resumen el comportamiento de control táctil.
Entrada de usuario | Acción
:------- | :--------
Mover rectángulo | La entrada táctil se convierte en un joystick virtual donde el movimiento vertical se traducirá en movimiento hacia delante/atrás, y el movimiento horizontal se traducirá en movimiento a derecha/izquierda.
Disparar rectángulo | Disparar una esfera.
Toca fuera del movimiento y dispara a rectángulo | Cambia la rotación (inclinación y desvío) de la vista de cámara.

El **MoveLookController** comprueba el id. del puntero para determinar dónde se produjo el evento y realiza una de las siguientes acciones:

-   Si se produjo el evento [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved) en el rectángulo de disparo o movimiento, actualiza la posición de puntero para el controlador.
-   Si el evento [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved) se produjo en alguna otra parte de la pantalla (definida como los controles de apariencia), calcula el cambio en la rotación alrededor del eje x y del eje y del vector de dirección que corresponde a la vista.


Una vez que hemos implementado nuestros controles táctiles, los rectángulos que obtuvimos anteriormente con Direct2D indicarán a los jugadores dónde están las zonas de movimiento, disparo y vista.


![controles táctiles](images/simple-dx-game-controls-showzones.png)


Ahora echemos un vistazo a cómo implementamos cada control.


### <a name="move-and-fire-controller"></a>Controlador de movimiento y disparo
El rectángulo del controlador de movimiento en el cuadrante inferior izquierdo de la pantalla se utiliza como un controlador direccional. Al deslizar el pulgar a la izquierda y derecho dentro de este espacio, el jugador se mueve a la izquierda y derecha, y al deslizar arriba y abajo, la cámara se mueve hacia delante y atrás.
Después de configurar esto, al pulsar el controlador de disparo en el cuadrante inferior derecho de la pantalla, se dispara una esfera.

Los métodos [**SetMoveRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) y [**SetFireRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867) crearán nuestros rectángulos de entrada, con dos vectores 2D para especificar las posiciones de las esquinas superior izquierda e inferior derecha de cada rectángulos en la pantalla. 


Los parámetros se asignan a **m\_fireUpperLeft** y **m\_fireLowerRight** que nos ayudarán a determinar si el usuario toca dentro los rectángulos. 
```cpp
m_fireUpperLeft  = upperLeft;
m_fireLowerRight = lowerRight;
```

Si se cambia el tamaño de la pantalla, se vuelve a dibujar los rectángulos para el tamaño adecuado.


Ahora que hemos delimitado los controles, es el momento de determinar si hay un usuario que realmente los esté usando.
Para ello, debemos configurar algunos controladores de eventos en el método **MoveLookController::InitWindow** para cuando el usuario pulsa, mueve o libera el puntero.

```cpp
window->PointerPressed +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

window->PointerMoved +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

window->PointerReleased +=
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);
```


En primer lugar determinaremos lo que sucede cuando el usuario pulsa dentro de los rectángulos de disparo o movimiento por primera vez, usando el método [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313).
Aquí comprobamos dónde se está tocando un control y si un puntero ya está en ese controlador. Si este es el primer dedo que toca el control específico, hacemos lo siguiente.
- Store a la ubicación de la touchdown en **m\_moveFirstDown** o **m\_fireFirstDown** como un vector 2D.
- Asignar el identificador de puntero a **m\_movePointerID** o **m\_firePointerID**.
- Establecer adecuada **InUse** marca (**m\_moveInUse** o **m\_fireInUse**) a `true` puesto que ahora tenemos un puntero de activo para que control.


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;
    auto pointerDeviceType = pointerDevice->PointerDeviceType;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Check to see if this pointer is in the move control.
            if (position.x > m_moveUpperLeft.x &&
                position.x < m_moveLowerRight.x &&
                position.y > m_moveUpperLeft.y &&
                position.y < m_moveLowerRight.y)
            {
                if (!m_moveInUse)         // If no pointer is in this control yet:
                {
                    // Process a DPad touch down event.
                    m_moveFirstDown = position;                 // Save the location of the initial contact
                    m_movePointerID = pointerID;                // Store the pointer using this control
                    m_moveInUse = true;                         // Set InUse flag to signal there is an active move pointer
                }
            }
            // Check to see if this pointer is in the fire control.
            else if (position.x > m_fireUpperLeft.x &&
                position.x < m_fireLowerRight.x &&
                position.y > m_fireUpperLeft.y &&
                position.y < m_fireLowerRight.y)
            {
                if (!m_fireInUse)
                {
                    m_fireLastPoint = position;     // Save the location of the initial contact
                    m_firePointerID = pointerID;    // Store the pointer using this control
                    m_fireInUse = true;             // Set InUse flag to signal there is an active fire pointer
                }
            }
            ...
```


Ahora que hemos determinado si el usuario toca un control de disparo o movimiento, vemos si el jugador está realizando los movimientos con el dedo presionado.
Con el método [**MoveLookController::OnPointerMoved**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395), se comprueba qué puntero se mueve y, a continuación, se almacena su nueva posición como un vector 2D.  


```cpp
    PointerPoint^ point = args->CurrentPoint;
    uint32 pointerID = point->PointerId;
    Point pointerPosition = point->Position;
    PointerPointProperties^ pointProperties = point->Properties;
    auto pointerDevice = point->PointerDevice;

    XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);     // convert to allow math
    
    switch (m_state)
    {
    case MoveLookControllerState::Active:
        // Decide which control this pointer is operating.
        
        // Move control
        if (pointerID == m_movePointerID)
        {
            m_movePointerPosition = position;       // Save the current position.
        }
        // Look control
        else if (pointerID == m_lookPointerID)
        {
            // ...
        }
        // Fire control
        else if (pointerID == m_firePointerID)
        {
            m_fireLastPoint = position;
        }
```


Una vez que el usuario ha realizado sus gestos dentro de los controles, liberan el puntero. Con el método [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500), se determina qué puntero se ha liberado y se realizan una serie de restablecimientos.


Si se ha liberado el control de movimiento, hacemos lo siguiente.
- Establecer la velocidad del jugador a `0` en todas las direcciones para impedir que se muevan en el juego.
- Conmutador **m\_moveInUse** a `false` puesto que el usuario ya no esté tocando el controlador de movimiento.
- Establecer el identificador de puntero de movimiento a `0`, puesto que ya no hay un puntero en el controlador de movimiento.

```cpp
       if (pointerID == m_movePointerID)
        {
            m_velocity = XMFLOAT3(0, 0, 0);      // Stop on release.
            m_moveInUse = false;
            m_movePointerID = 0;
        }
```


Para el control de disparo, si se ha soltado, todo lo que hacemos es cambiar la marca **m_fireInUse** a `false` y el identificador de puntero de disparo a `0`, puesto que ya no hay un puntero en el control de disparo.
```cpp
        else if (pointerID == m_firePointerID)
        {
            m_fireInUse = false;
            m_firePointerID = 0;
        }
```

### <a name="look-controller"></a>Controlador de vista
Tratamos los eventos de puntero del dispositivo táctil para las regiones no usadas de la pantalla como el controlador de vista. Al deslizar el dedo por esta zona, cambian la inclinación y el desvío (rotación) de la cámara del jugador.


Si se produce un evento **MoveLookController::OnPointerPressed** en un dispositivo táctil en esta región y el estado del juego está establecido en **Activo**, se le asigna un identificador de puntero.

```cpp
    if (!m_lookInUse)   // If no pointer is in this control yet:
    {
        m_lookLastPoint = position;                   // Save the pointer for a later move.
        m_lookPointerID = pointerID;                  // Store the pointer using this control.
        m_lookLastDelta.x = m_lookLastDelta.y = 0;    // These are for smoothing.
        m_lookInUse = true;
    }
```
Puedes ver el código completo para el método **MoveLookController::OnPointerPressed** en [GitHub](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L252-L259).




Aquí, **MoveLookController** asigna el identificador de puntero para el puntero que provocó el evento en una variable que corresponda a la región de vista. En el caso de una entrada táctil que se producen en la región de aspecto, el **m\_lookPointerID** variable se establece en el identificador de puntero que desencadenó el evento. Una variable booleana, **m\_lookInUse**, también se establece para indicar que el control tiene no se han publicado.

Veamos ahora cómo la muestra de juego controla el evento de pantalla táctil [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointermoved).


Dentro del método **MoveLookController::OnPointerMoved**, se comprueba qué tipo de Id. de puntero se asignó al evento. Si es **m_lookPointerID**, calculamos el cambio de posición del puntero.
A continuación, usamos este delta para calcular cuánto debe cambiar la rotación. Finalmente estamos en un punto donde podemos actualizar el **m\_pitch** y **m\_guiñada** para usarse en el juego para cambiar la rotación del Reproductor. 

```cpp
    else if (pointerID == m_lookPointerID)     // This is the look pointer.
    {
        // Look control
        XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did the pointer move.
        pointerDelta.y = position.y - m_lookLastPoint.y;

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
        m_lookLastPoint = position;                             // Save for the next time through.

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw   += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);M_PI / 2.0f, m_pitch);
        }
```





Lo último que veremos es cómo maneja el ejemplo de juego el evento de pantalla táctil [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerreleased).
Una vez que el usuario haya terminado el gesto de toque y quite el dedo de la pantalla, [**MoveLookController::OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) se inicia.
Si el id. del puntero que desencadenó el evento [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.pointerreleased) es el id. del puntero de movimiento registrado anteriormente, **MoveLookController** establece la velocidad a `0` porque el jugador ha dejado de tocar el área de vista.

```cpp
    else if (pointerID == m_lookPointerID)
    {
        m_lookInUse = false;
        m_lookPointerID = 0;
    }
```





## <a name="adding-mouse-and-keyboard-support"></a>Agregar compatibilidad de teclado y mouse


Este juego tiene el siguiente diseño de control para teclado y mouse.

Entrada de usuario | Acción
:------- | :--------
W | El jugador se mueve hacia adelante
A | El jugador se mueve a la izquierda
S | El jugador se mueve hacia atrás
D | El jugador se mueve a la derecha
X | Subir la vista
Barra espaciadora | Bajar la vista
P | Pausar el juego
Movimiento del mouse | Cambia la rotación (inclinación y desvío) de la vista de cámara
Botón izquierdo del mouse | Disparar una esfera


Para usar el teclado, la muestra de juego registra dos eventos nuevos, [**CoreWindow::KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.keyup) y [**CoreWindow::KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.keydown), dentro del método [**MoveLookController::InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88). Estos eventos controlan la pulsación y liberación de una tecla.

```cpp
window->KeyDown +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

window->KeyUp +=
        ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);
```

El mouse se trata de una forma ligeramente distinta a los controles táctiles, a pesar de que usa un puntero. Para alinearse con nuestro diseño de control, el **MoveLookController** gira la cámara, siempre que se mueve el mouse, y dispara cuando se pulsa el botón izquierdo del ratón.


Esto se controla en el método [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) de **MoveLookController**.

En este método comprobamos qué tipo de dispositivo de puntero se utiliza con la enumeración [`Windows::Devices::Input::PointerDeviceType`](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Input.PointerDeviceType). Si el juego está **Activo** y **PointerDeviceType** no es **Táctil**, damos por hecho que es la entrada de mouse.

```cpp
    case MoveLookControllerState::Active:
        switch (pointerDeviceType)
        {
        case Windows::Devices::Input::PointerDeviceType::Touch:
            // Behavior for touch controls
        
        default:
        // Behavior for mouse controls
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            if (!m_autoFire && (!m_mouseLeftInUse && leftButton))
            {
                m_firePressed = true;
            }

            if (!m_mouseInUse)
            {
                m_mouseInUse = true;
                m_mouseLastPoint = position;
                m_mousePointerID = pointerID;
                m_mouseLeftInUse = leftButton;
                m_mouseRightInUse = rightButton;
                m_lookLastDelta.x = m_lookLastDelta.y = 0;          // These are for smoothing.
            }
            break;
        }
```

Cuando el jugador deja de presionar uno de los botones del mouse, se produce el evento [CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased), llamando al método [MoveLookController::OnPointerReleased](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500), y se completa la entrada. En este punto, dejarán de dispararse esferas si el botón izquierdo del mouse se presionó y ahora se libera. Puesto que la vista siempre está habilitada, el juego continúa usando el mismo puntero de mouse para realizar un seguimiento de los eventos de vista en curso.

```cpp
    case MoveLookControllerState::Active:
        // Touch points
        if (pointerID == m_movePointerID)
        {
            // Stop movement
        }
        else if (pointerID == m_lookPointerID)
        {
            // Stop look rotation
        }
        // Fire button has been released
        else if (pointerID == m_firePointerID)
        {
            // Stop firing
        }
        // Mouse point
        else if (pointerID == m_mousePointerID)
        {
            bool rightButton = pointProperties->IsRightButtonPressed;
            bool leftButton = pointProperties->IsLeftButtonPressed;

            // Mouse no longer in use so stop firing
            m_mouseInUse = false;

            // Don't clear the mouse pointer ID so that Move events still result in Look changes.
            // m_mousePointerID = 0;
            m_mouseLeftInUse = leftButton;
            m_mouseRightInUse = rightButton;
        }
        break;
    }
}
```



Ahora veamos el último tipo de control compatible: los controladores para juegos. Los controladores para juegos se manejan de forma independiente a los controles táctiles y de ratón, porque no usan el objeto de puntero. Por este motivo, unos nuevos controladores de eventos y métodos deben agregarse.

## <a name="adding-gamepad-support"></a>Añadir compatibilidad con el controlador para juegos


En este juego, se añade la compatibilidad con el controlador para juegos llamando a las API [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input). Este conjunto de API proporciona acceso entradas de dispositivos de juegos como volantes de carreras o palancas de vuelo. 


Nuestros controles de controlador para juegos serán los siguientes.

Entrada de usuario | Acción
:------- | :--------
Stick izquierdo analógico | El jugador se mueve
Stick derecho analógico | Cambia la rotación (inclinación y desvío) de la vista de cámara
Gatillo derecho | Disparar una esfera
Botón de menú/inicio | Pausar o reanudar el juego




En el método [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103), agregamos dos nuevos eventos para determinar si un controlador para juegos se ha [agregado](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105) o [quitado](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114). Estos eventos actualizan la propiedad **m_gamepadsChanged**. Esto se usa en el **UpdatePollingDevices** método para comprobar si ha cambiado la lista de configuración conocido de los controles. 

```cpp
    // Detect gamepad connection and disconnection events.
    Gamepad::GamepadAdded +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadAdded);

    Gamepad::GamepadRemoved +=
        ref new EventHandler<Gamepad^>(this, &MoveLookController::OnGamepadRemoved);
```

> [!NOTE]
> Las aplicaciones para UWP no pueden recibir entradas de un controlador de una Xbox mientras la aplicación no tiene el foco.

### <a name="the-updatepollingdevices-method"></a>El método UpdatePollingDevices

El método [**UpdatePollingDevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782) de la instancia **MoveLookController** comprueba inmediatamente si hay un controlador para juegos conectado. Si hay alguno, comenzaremos a leer su estado con [**Gamepad.GetCurrentReading**](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading). Esto devuelve la estructura [**GamepadReading**](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.GamepadReading), lo que nos permite comprobar en qué botones se ha hecho clic o qué palancas de control se han movido.


Si el estado del juego es **WaitForInput**, solo escuchamos al botón de menú/inicio del controlador, de modo que se puede reanudar el juego.


Si es **Activo**, se comprueba la entrada del usuario y se determina qué acción en el juego debe ocurrir.
Por ejemplo, si el usuario movió el stick analógico izquierdo en una dirección específica, el juego sabe que necesitamos mover el jugador en la dirección en la que se ha movido el stick. El movimiento del stick en una dirección específica debe registrarse como mayor que el radio de la **zona muerta**; de lo contrario, no ocurrirá nada. Este radio de la zona muerta es necesario para evitar "derivas", es decir, cuando el controlador capta pequeños movimientos del pulgar del jugador al dejarlo descansar sobre el stick. Sin zonas muertas, los mandos pueden parecer demasiado sensibles para el usuario.


La entrada de la palanca de control está entre -1 y 1 para los ejes x e y. La siguiente constante especifica el radio de la zona muerta de la palanca de control.

```cpp
#define THUMBSTICK_DEADZONE 0.25f
```

Con esta variable, empezaremos a procesar la entrada de la palanca de control accionable. El movimiento se produciría con un valor de [-1, -,26] o [,26, 1] en cualquier eje.

![zona muerta para palancas de control](images/simple-dx-game-controls-deadzone.png)

Este fragmento de método **UpdatePollingDevices** controla las palancas de control derecha e izquierda.
Cada valor de X e Y del stick se comprueba para ver si están fuera de la zona muerta. Si hay uno o ambos fuera, actualizaremos el componente correspondiente.
Por ejemplo, si se mueve la palanca de control analógica izquierdo izquierda a lo largo del eje X, agregaremos -1 al componente **x** del vector **m_moveCommand**. Este vector es lo que se usará para agregar todos los movimientos en todos los dispositivos y se usará más tarde para calcular dónde debe moverse el jugador. 


```cpp
        // Use the left thumbstick to control the eye point position (position of the player)

        // Check if left thumbstick is outside of dead zone on x axis
        if (reading.LeftThumbstickX > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickX < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on x axis
            float x = static_cast<float>(reading.LeftThumbstickX);
            // Set the x of the move vector to 1 if the stick is being moved right.
            // Set to -1 if moved left. 
            m_moveCommand.x -= (x > 0) ? 1 : -1;
        }

        // Check if left thumbstick is outside of dead zone on y axis
        if (reading.LeftThumbstickY > THUMBSTICK_DEADZONE ||
            reading.LeftThumbstickY < -THUMBSTICK_DEADZONE)
        {
            // Get value of left thumbstick's position on y axis
            float y = static_cast<float>(reading.LeftThumbstickY);
            // Set the y of the move vector to 1 if the stick is being moved forward.
            // Set to -1 if moved backwards. 
            m_moveCommand.y += (y > 0) ? 1 : -1;
        }

```

De forma similar a cómo el stick de control izquierdo controla el movimiento, el stick derecho controla la rotación de la cámara.


El comportamiento del stick de controlador derecho se alinea con el comportamiento del movimiento del mouse en nuestra configuración de control de mouse y teclado.
Si el stick está fuera de la zona muerta, calculamos la diferencia entre la posición del puntero actual y donde el usuario está intentando buscar.
Este cambio en la posición del puntero (**pointerDelta**), a continuación, se usa para actualizar la inclinación y el desvío de la rotación de la cámara que más adelante se aplica en nuestro método **Actualizar**.
El vector **pointerDelta** puede resultar familiar porque también se usa en el método [MoveLookController::OnPointerMoved](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395) para realizar un seguimiento del cambio de posición de puntero para la entrada de mouse y la entrada táctil.


```cpp
        // Use the right thumbstick to control the look at position

        XMFLOAT2 pointerDelta;

        // Check if right thumbstick is outside of deadzone on x axis
        if (reading.RightThumbstickX > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickX < -THUMBSTICK_DEADZONE)
        {
            float x = static_cast<float>(reading.RightThumbstickX);
            // Register the change in the pointer along the x axis
            pointerDelta.x = x * x * x; 
        }
        // No actionable thumbstick movement. Register no change in pointer.
        else
        {
            pointerDelta.x = 0.0f;
        }
        // Check if right thumbstick is outside of deadzone on y axis
        if (reading.RightThumbstickY > THUMBSTICK_DEADZONE ||
            reading.RightThumbstickY < -THUMBSTICK_DEADZONE)
        {
            float y = static_cast<float>(reading.RightThumbstickY);
            // Register the change in the pointer along the y axis
            pointerDelta.y = y * y * y;
        }
        else
        {
            pointerDelta.y = 0.0f;
        }

        XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x *  0.08f;       // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y *  0.08f;

        // Update our orientation based on the command.
        m_pitch += rotationDelta.y;
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        m_pitch = __max(-XM_PI / 2.0f, m_pitch);
        m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

¡Los mandos del juego no estarían completos sin la capacidad para disparar esferas!


Este método **UpdatePollingDevices** también comprueba si se presiona el gatillo correcto. Si es así, nuestra propiedad **m_firePressed** pasa a "verdadero", lo que le señala al juego que las esferas deben empezar a dispararse.
```cpp
    if (reading.RightTrigger > TRIGGER_DEADZONE)
    {
        if (!m_autoFire && !m_gamepadTriggerInUse)
        {
            m_firePressed = true;
        }

        m_gamepadTriggerInUse = true;
    }
    else
    {
        m_gamepadTriggerInUse = false;
    }
```


## <a name="the-update-method"></a>El método Actualizar

En resumen, vamos a profundizar en el método [**Actualizar**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096).
Este método combina cualquier movimiento o rotación realizados por el jugador con cualquier entrada compatible para generar un vector de velocidad y actualizar los valores de inclinación y desvío para que nuestro bucle de juego tenga acceso.


El método **Actualizar** empieza llamado a [**UpdatePollingDevices**](#the-updatepollingdevices-method) para actualizar el estado del controlador. Este método también recopila cualquier entrada de un controlador para juegos y agrega sus movimientos al vector **m_moveCommand**. 

En nuestro método **Actualizar**, a continuación, realizamos las siguientes comprobaciones de entrada.
- Si el jugador usa el rectángulo de controlador de movimiento, determinaremos el cambio de posición de puntero y lo usaremos para calcular si el usuario movió el puntero fuera de la zona muerta del controlador. Si está fuera de la zona muerta, la propiedad de vector **m_moveCommand** se actualiza con el valor de joystick virtual.
- Si cualquiera de las entradas de teclado de movimiento se presionan, un valor de `1.0f` o `-1.0f` se agregan en el componente correspondiente de la **m_moveCommand** vector &mdash; `1.0f` para reenviar y `-1.0f`para versiones anteriores.


Una vez que todas las entradas de movimiento se han tenido en cuenta, ejecutamos el vector **m_moveCommand** a través de algunos cálculos para generar un nuevo vector que representa la dirección del jugador en relación con el mundo del juego.
A continuación, tomamos nuestra movimientos en relación con el mundo y las aplicamos al jugador como velocidad en esa dirección.
Por último, restablecemos el vector **m_moveCommand** a `(0.0f, 0.0f, 0.0f)` para que todo esté listo para el siguiente fotograma de juego.


```cpp
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    // Get change in 
    if (m_moveInUse)
    {
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        if (fabsf(pointerDelta.x) > 16.0f)         // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x -= pointerDelta.x/fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y/fabsf(pointerDelta.y);
    }

    // Poll our state bits set by the keyboard input events.
    if (m_forward)
    {
        m_moveCommand.y += 1.0f;
    }
    if (m_back)
    {
        m_moveCommand.y -= 1.0f;
    }
    if (m_left)
    {
        m_moveCommand.x += 1.0f;
    }
    if (m_right)
    {
        m_moveCommand.x -= 1.0f;
    }
    if (m_up)
    {
        m_moveCommand.z += 1.0f;
    }
    if (m_down)
    {
        m_moveCommand.z -= 1.0f;
    }

    // Make sure that 45deg cases are not faster.
    if (fabsf(m_moveCommand.x) > 0.1f ||
        fabsf(m_moveCommand.y) > 0.1f ||
        fabsf(m_moveCommand.z) > 0.1f)
    {
        XMStoreFloat3(&m_moveCommand, XMVector3Normalize(XMLoadFloat3(&m_moveCommand)));
    }

    // Rotate command to align with our direction (world coordinates).
    XMFLOAT3 wCommand;
    wCommand.x =  m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y =  m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z =  m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z =  wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y =  wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```


## <a name="next-steps"></a>Pasos siguientes

Ahora que hemos agregado nuestros controles, hay otra característica que es necesario agregar para crear un juego envolvente: ¡sonido!
La música y los efectos sonoros son importantes en cualquier juego. Hablemos, por tanto, de cómo [agregar sonido](tutorial--adding-sound.md) a continuación.
 

 

 




