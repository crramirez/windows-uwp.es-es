---
title: Agregar controles
description: Ahora, echemos un vistazo a cómo el juego de ejemplo implementa los controles de movimiento y desplazamiento en un juego 3D, y cómo desarrollar controles táctiles básicos, del mouse y del controlador de juegos.
ms.assetid: f9666abb-151a-74b4-ae0b-ef88f1f252f8
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, controles, entrada
ms.localizationpriority: medium
ms.openlocfilehash: dfe864f0b8c16cce9cc8d413c41a4e3324cf2e9b
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409664"
---
# <a name="add-controls"></a>Agregar controles

> [!NOTE]
> Este tema forma parte de la serie de tutoriales de [creación de una plataforma universal de Windows sencilla (UWP) con DirectX](tutorial--create-your-first-uwp-directx-game.md) . El tema de ese vínculo establece el contexto de la serie.

\[Se actualizó con aplicaciones para UWP en Windows 10. Para artículos de Windows 8. x, consulte el [archivo](/previous-versions/windows/apps/mt244353(v=win.10)?redirectedfrom=MSDN)\]

Una buena Plataforma universal de Windows juego (UWP) admite una amplia variedad de interfaces. Un posible reproductor podría tener Windows 10 en una tableta sin botones físicos, un equipo con una controladora Xbox conectada o la plataforma de juegos de escritorio más reciente con un mouse y un teclado de juegos de alto rendimiento. En nuestro juego, los controles se implementan en la clase [**MoveLookController**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp) . Esta clase agrega los tres tipos de entrada (mouse y teclado, táctil y controlador para juegos) a un solo controlador. El resultado final es un cierre de primera persona que usa los controles de movimiento y búsqueda estándar del género que funcionan con varios dispositivos.

> [!NOTE]
> Para obtener más información sobre los controles, vea [controles de movimiento y aspecto para juegos](tutorial--adding-move-look-controls-to-your-directx-game.md) y [controles táctiles para juegos](tutorial--adding-touch-controls-to-your-directx-game.md).


## <a name="objective"></a>Objetivo

Llegados a este punto, tenemos un juego que representa, pero no se puede mover el jugador o captar los objetivos. Echaremos un vistazo a cómo nuestro juego implementa los controles de movimiento de la primera persona con el dedo para los siguientes tipos de entrada en nuestro juego DirectX de UWP.
- Mouse y teclado
- Entrada táctil
- Controlador para juegos

>[!Note]
>Si no ha descargado el código de juego más reciente para este ejemplo, vaya a [juego de ejemplo de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este ejemplo forma parte de una gran colección de ejemplos de características de UWP. Para obtener instrucciones sobre cómo descargar el ejemplo, consulte [obtener los ejemplos de UWP en github](/windows/uwp/get-started/get-uwp-app-samples).

## <a name="common-control-behaviors"></a>Comportamientos de controles comunes


Los controles táctiles y los controles de mouse o teclado tienen una implementación principal muy parecida. En una aplicación para UWP, un puntero es simplemente un punto en la pantalla. Puedes moverlo desplazando el ratón o deslizando un dedo por la pantalla táctil. Como resultado, puedes registrar un único conjunto de eventos y despreocuparte de si el jugador usa un mouse o una pantalla táctil para mover y presionar el puntero.

Cuando se inicializa la clase **MoveLookController** en el juego de ejemplo, se registra para cuatro eventos específicos del puntero y un evento específico del mouse:

Evento | Descripción
:------ | :-------
[**CoreWindow::P ointerPressed**](/uwp/api/windows.ui.core.corewindow.pointerpressed) | Se presionó (o se mantuvo presionado) el botón izquierdo o derecho del mouse o se tocó la superficie táctil.
[**CoreWindow::P ointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) |El mouse se movió, o se realizó una acción de arrastrar en la superficie táctil.
[**CoreWindow::P ointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) |Se soltó el botón izquierdo del mouse, o se retiró el objeto que estaba en contacto con la superficie táctil.
[**CoreWindow::P ointerExited**](/uwp/api/windows.ui.core.corewindow.pointerexited) |El puntero se movió fuera de la ventana principal.
[**Windows::D evices:: Input:: MouseMoved**](/uwp/api/windows.devices.input.mousedevice.mousemoved) | El mouse se desplazó una distancia determinada. Tenga en cuenta que solo nos interesan los valores Delta de movimiento del mouse, y no la posición X-Y actual.


Estos controladores de eventos se establecen para iniciar la escucha de datos proporcionados por el usuario en cuanto se inicializa **MoveLookController** en la ventana de la aplicación.
```cppwinrt
void MoveLookController::InitWindow(_In_ CoreWindow const& window)
{
    ResetState();

    window.PointerPressed({ this, &MoveLookController::OnPointerPressed });

    window.PointerMoved({ this, &MoveLookController::OnPointerMoved });

    window.PointerReleased({ this, &MoveLookController::OnPointerReleased });

    window.PointerExited({ this, &MoveLookController::OnPointerExited });

    ...

    // There is a separate handler for mouse-only relative mouse movement events.
    MouseDevice::GetForCurrentView().MouseMoved({ this, &MoveLookController::OnMouseMoved });

    ...
}
```

El código completo de [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L92) puede verse en github.


Para determinar el momento en que el juego debe estar escuchando una entrada determinada, la clase **MoveLookController** tiene tres Estados específicos del controlador, independientemente del tipo de controlador:

State | Descripción
:----- | :-------
**None** | Es el estado inicializado para el mando. Se omiten todas las entradas, ya que el juego no prevé ninguna entrada del controlador.
**WaitForInput** | El controlador está esperando a que el jugador confirme un mensaje del juego mediante un clic del mouse izquierdo, un evento Touch, y el botón de menú en un controlador de juegos.
**Activo** | El controlador está en modo de juego activo.



### <a name="waitforinput-state-and-pausing-the-game"></a>WaitForInput estado y pausa del juego

El juego entra en el estado **WaitForInput** cuando el juego se ha pausado. Esto sucede cuando el reproductor mueve el puntero fuera de la ventana principal del juego, o presiona el botón de pausa (tecla P o el botón **Inicio** del controlador de juegos). El **MoveLookController** registra la prensa e informa al bucle del juego cuando llama al método [**IsPauseRequested**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L107-L127) . En ese punto, si **IsPauseRequested** devuelve **true**, el bucle de juego llama entonces a **WaitForPress** en **MoveLookController** para pasar el controlador al estado **WaitForInput** . 


Una vez en el estado **WaitForInput** , el juego detiene el procesamiento de casi todos los eventos de entrada de juego hasta que vuelve al estado **activo** . La excepción es el botón pausa, con una prensa que hace que el juego vuelva al estado activo. Aparte del botón pausa, para que el juego vuelva al estado **activo** , el reproductor debe seleccionar un elemento de menú. 



### <a name="the-active-state"></a>Estado activo

Durante el estado **activo** , la instancia de **MoveLookController** procesa eventos de todos los dispositivos de entrada habilitados e interpreta las intenciones del jugador. Como resultado, actualiza la velocidad y la dirección de la apariencia de la vista del jugador y comparte los datos actualizados con el juego una vez que se llama a **Update** desde el bucle del juego.


Se realiza un seguimiento de todas las entradas de puntero en estado **activo** , con distintos identificadores de puntero correspondientes a las diferentes acciones de puntero.
Cuando se recibe un evento [**PointerPressed**](/uwp/api/windows.ui.core.corewindow.pointerpressed), **MoveLookController** obtiene el valor del identificador del puntero creado por la ventana. El id. de puntero represente un tipo específico de entrada. Por ejemplo, en un dispositivo multitoque, puede que haya varias entradas activas diferentes al mismo tiempo. Los identificadores se usan para llevar un seguimiento de la entrada que está usando el jugador. Si un evento está en el rectángulo de movimiento de la pantalla táctil, se asigna un identificador de puntero para realizar el seguimiento de los eventos de puntero en el rectángulo de movimiento. Otros eventos de puntero en el rectángulo de disparo se siguen por separado, con id. de puntero diferentes.


> [!NOTE]
> La entrada del mouse y el stick derecho de un controlador de juegos también tienen identificadores que se administran por separado.

Una vez que los eventos de puntero se han asignado a una acción específica del juego, es hora de actualizar los datos que el objeto **MoveLookController** comparte con el bucle de juego principal.

Cuando se llama, el método [**Update**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) del juego de ejemplo procesa la entrada y actualiza las variables de velocidad y apariencia **( \_ velocidad m** y **m \_ LookDirection**), que el bucle del juego recupera a continuación mediante una llamada a los métodos de [**velocidad**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L906-L909) pública y [**LookDirection**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L913-L923) .

> [!NOTE]
> Puede ver más detalles sobre el método de [**actualización**](#the-update-method) más adelante en esta página.




El bucle de juego puede probar si el jugador está disparando llamando al método **IsFiring** en la instancia **MoveLookController**. El **MoveLookController** comprueba para ver si el jugador ha presionado el botón de disparo en uno de los tres tipos de entrada.

```cppwinrt
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

Veamos ahora la implementación de cada uno de los tres tipos de control con un poco más detalle.

## <a name="adding-relative-mouse-controls"></a>Agregar controles de mouse relativos


Si se detecta el movimiento del mouse, queremos usar ese movimiento para determinar el nuevo tono y el guiñante de la cámara. Esto se logra implementando controles de ratón relativos, con los que controlamos la distancia que el ratón ha recorrido (el delta entre el inicio del movimiento hasta que el ratón se detiene ) en contraposición a grabar las coordenadas de píxel x-y absolutas del movimiento.

Para ello, obtenemos los cambios en las coordenadas X (movimiento horizontal) e Y (movimiento vertical) examinando los campos [**MouseDelta::X**](/uwp/api/Windows.Devices.Input.MouseDelta) y **MouseDelta::Y** del objeto de argumento [**Windows::Device::Input::MouseEventArgs::MouseDelta**](/uwp/api/windows.devices.input.mouseeventargs.mousedelta) que el evento [**MouseMoved**](/uwp/api/windows.devices.input.mousedevice.mousemoved) devuelve.

```cppwinrt
void MoveLookController::OnMouseMoved(
    _In_ MouseDevice const& /* mouseDevice */,
    _In_ MouseEventArgs const& args
    )
{
    // Handle Mouse Input via dedicated relative movement handler.

    switch (m_state)
    {
    case MoveLookControllerState::Active:
        XMFLOAT2 mouseDelta;
        mouseDelta.x = static_cast<float>(args.MouseDelta().X);
        mouseDelta.y = static_cast<float>(args.MouseDelta().Y);

        XMFLOAT2 rotationDelta;
        // Scale for control sensitivity.
        rotationDelta.x = mouseDelta.x * MoveLookConstants::RotationGain;
        rotationDelta.y = mouseDelta.y * MoveLookConstants::RotationGain;

        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;
        m_yaw += rotationDelta.x;

        // Limit pitch to straight up or straight down.
        float limit = XM_PI / 2.0f - 0.01f;
        m_pitch = __max(-limit, m_pitch);
        m_pitch = __min(+limit, m_pitch);

        // Keep longitude in sane range by wrapping.
        if (m_yaw > XM_PI)
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

## <a name="adding-touch-support"></a>Incorporación de compatibilidad táctil

Los controles táctiles son excelentes para admitir usuarios con tabletas. En este juego se recopilan entradas táctiles mediante el zoning de ciertas áreas de la pantalla con cada alineación con las acciones específicas del juego.
La entrada táctil de este juego usa tres zonas.

![cambiar el diseño de la vista táctil](images/simple-dx-game-controls-touchzones.png)

Los siguientes comandos Resumen el comportamiento del control Touch.
Datos proporcionados por el usuario | Acción
:------- | :--------
Cambiar rectángulo | La entrada táctil se convierte en un joystick virtual en el que el movimiento vertical se traducirá en el movimiento de posición hacia delante o hacia atrás y el movimiento horizontal se traducirá en el movimiento de la posición izquierda/derecha.
Desencadenar rectángulo | Desencadene una esfera.
Toque fuera del rectángulo de movimiento y fuego | Cambie la rotación (el tono y la guiñada) de la vista de cámara.

El **MoveLookController** comprueba el id. del puntero para determinar dónde se produjo el evento y realiza una de las siguientes acciones:

-   Si se produjo el evento [**PointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) en el rectángulo de disparo o movimiento, actualiza la posición de puntero para el controlador.
-   Si el evento [**PointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) se produjo en alguna otra parte de la pantalla (definida como los controles de apariencia), calcula el cambio en la rotación alrededor del eje x y del eje y del vector de dirección que corresponde a la vista.


Una vez que hemos implementado nuestros controles táctiles, los rectángulos dibujados anteriormente con Direct2D indicarán a los jugadores dónde están las zonas de movimiento, incendio y búsqueda.


![Touch (controles)](images/simple-dx-game-controls-showzones.png)


Ahora, echemos un vistazo a cómo se implementa cada control.


### <a name="move-and-fire-controller"></a>Controlador de movimiento y activación
El rectángulo del controlador de movimiento en el cuadrante inferior izquierdo de la pantalla se utiliza como Panel direccional. Deslizar el control de posición a la izquierda y a la derecha dentro de este espacio mueve el jugador hacia la izquierda y la derecha, mientras que hacia arriba y hacia abajo se mueve la cámara hacia delante y hacia atrás
Después de configurar esta opción, al pulsar el controlador de incendios en el cuadrante inferior derecho de la pantalla se activa una esfera.

Los métodos [**SetMoveRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L843-L853) y [**SetFireRect**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L857-L867) crean nuestros rectángulos de entrada, tomando dos vectores de 2D para especificar las posiciones de las esquinas superior izquierda e inferior derecha de cada rectángulo en la pantalla. 


Después, los parámetros se asignan a **m \_ fireUpperLeft** y **m \_ fireLowerRight** que nos ayudarán a determinar si el usuario está tocando dentro de los rectángulos. 
```cppwinrt
m_fireUpperLeft = upperLeft;
m_fireLowerRight = lowerRight;
```

Si se cambia el tamaño de la pantalla, estos rectángulos se vuelven a dibujar en el tamaño de approperiate.

Ahora que hemos dividido en zonas los controles, es el momento de determinar cuándo un usuario los está usando.
Para ello, configuramos algunos controladores de eventos en el método **MoveLookController:: InitWindow** para cuando el usuario presiona, mueve o suelta su puntero.

```cppwinrt
window.PointerPressed({ this, &MoveLookController::OnPointerPressed });

window.PointerMoved({ this, &MoveLookController::OnPointerMoved });

window.PointerReleased({ this, &MoveLookController::OnPointerReleased });
```

En primer lugar, determinamos lo que sucede cuando el usuario presiona en los rectángulos de movimiento o activación con el método [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) .
Aquí comprobamos el lugar en el que tocan un control y si un puntero ya está en ese controlador. Si este es el primer dedo para tocar el control específico, hacemos lo siguiente.
- Almacene la ubicación de touchdown en **m \_ moveFirstDown** o **m \_ fireFirstDown** como vector 2D.
- Asigne el ID. de puntero a **m \_ movePointerID** o **m \_ firePointerID**.
- Establezca la marca **inuse** adecuada (**m \_ moveInUse** o **m \_ fireInUse**) en `true` , ya que ahora tenemos un puntero activo para ese control.

```cppwinrt
PointerPoint point = args.CurrentPoint();
uint32_t pointerID = point.PointerId();
Point pointerPosition = point.Position();
PointerPointProperties pointProperties = point.Properties();
auto pointerDevice = point.PointerDevice();
auto pointerDeviceType = pointerDevice.PointerDeviceType();

XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

...
case MoveLookControllerState::Active:
    switch (pointerDeviceType)
    {
    case winrt::Windows::Devices::Input::PointerDeviceType::Touch:
        // Check to see if this pointer is in the move control.
        if (position.x > m_moveUpperLeft.x &&
            position.x < m_moveLowerRight.x &&
            position.y > m_moveUpperLeft.y &&
            position.y < m_moveLowerRight.y)
        {
            // If no pointer is in this control yet.
            if (!m_moveInUse)
            {
                // Process a DPad touch down event.
                // Save the location of the initial contact
                m_moveFirstDown = position;
                // Store the pointer using this control
                m_movePointerID = pointerID;
                // Set InUse flag to signal there is an active move pointer
                m_moveInUse = true;
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
                // Save the location of the initial contact
                m_fireLastPoint = position;
                // Store the pointer using this control
                m_firePointerID = pointerID;
                // Set InUse flag to signal there is an active fire pointer
                m_fireInUse = true;
                ...
            }
        }
        ...
```

Ahora que hemos determinado si el usuario está tocando un control de movimiento o fuego, vemos si el jugador está realizando movimientos con el dedo presionado.
Con el método [**MoveLookController:: OnPointerMoved**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L317-L395) , comprobamos qué puntero se ha desactivado y, a continuación, almacenamos su nueva posición como vector 2D.  

```cppwinrt
PointerPoint point = args.CurrentPoint();
uint32_t pointerID = point.PointerId();
Point pointerPosition = point.Position();
PointerPointProperties pointProperties = point.Properties();
auto pointerDevice = point.PointerDevice();

// convert to allow math
XMFLOAT2 position = XMFLOAT2(pointerPosition.X, pointerPosition.Y);

switch (m_state)
{
case MoveLookControllerState::Active:
    // Decide which control this pointer is operating.

    // Move control
    if (pointerID == m_movePointerID)
    {
        // Save the current position.
        m_movePointerPosition = position;
    }
    // Look control
    else if (pointerID == m_lookPointerID)
    {
        ...
    }
    // Fire control
    else if (pointerID == m_firePointerID)
    {
        m_fireLastPoint = position;
    }
    ...
```

Una vez que el usuario ha realizado sus gestos dentro de los controles, liberará el puntero. Con el método [**MoveLookController:: OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) , determinamos qué puntero se ha liberado y realizan una serie de restablecimientos.


Si se ha lanzado el control de movimiento, hacemos lo siguiente.
- Establezca la velocidad del reproductor `0` en en todas las direcciones para evitar que se muevan en el juego.
- Cambie **m \_ moveInUse** a `false` porque el usuario ya no toca el controlador de movimiento.
- Establezca el identificador de puntero de movimiento en porque ya no `0` hay un puntero en el controlador de movimiento.

```cppwinrt
if (pointerID == m_movePointerID)
{
    // Stop on release.
    m_velocity = XMFLOAT3(0, 0, 0);
    m_moveInUse = false;
    m_movePointerID = 0;
}
```

En el caso del control Fire, si se ha lanzado todo lo que hacemos es cambiar el indicador de **m_fireInUse** a `false` y el identificador del puntero de activación a `0` , ya que ya no hay un puntero en el control de incendios.
```cppwinrt
else if (pointerID == m_firePointerID)
{
    m_fireInUse = false;
    m_firePointerID = 0;
}
```

### <a name="look-controller"></a>Buscar controlador
Trataremos los eventos de puntero de dispositivo táctil para las regiones sin usar de la pantalla como controlador de búsqueda. Deslizar el dedo alrededor de esta zona cambia el tono y la guiñada (rotación) de la cámara del jugador.

Si el evento **MoveLookController:: OnPointerPressed** se genera en un dispositivo táctil en esta región y el estado del juego se establece en **activo**, se le asigna un identificador de puntero.

```cppwinrt
// If no pointer is in this control yet.
if (!m_lookInUse)
{
    // Save point for later move.
    m_lookLastPoint = position;
    // Store the pointer using this control.
    m_lookPointerID = pointerID;
    // These are for smoothing.
    m_lookLastDelta.x = m_lookLastDelta.y = 0;
    m_lookInUse = true;
}
```

Aquí, el **MoveLookController** asigna el identificador de puntero para el puntero que desencadenó el evento a una variable específica que corresponde a la región de búsqueda. En el caso de que se produzca una entrada táctil en la región de búsqueda, la variable **m \_ lookPointerID** se establece en el identificador de puntero que desencadenó el evento. También se establece una variable booleana, **m \_ lookInUse**, para indicar que aún no se ha liberado el control.

Ahora, echemos un vistazo a cómo el juego de ejemplo controla el evento de pantalla táctil de [**PointerMoved**](/uwp/api/windows.ui.core.corewindow.pointermoved) .

Dentro del método **MoveLookController:: OnPointerMoved** , comprobamos qué tipo de identificador de puntero se ha asignado al evento. Si es **m_lookPointerID**, se calcula el cambio en la posición del puntero.
A continuación, usamos este delta para calcular cuánto debe cambiar la rotación. Por último, vamos a un punto en el que podemos actualizar el ** \_ tono m** y **m \_ guiñada** para que se use en el juego para cambiar la rotación del jugador. 

```cppwinrt
// This is the look pointer.
else if (pointerID == m_lookPointerID)
{
    // Look control.
    XMFLOAT2 pointerDelta;
    // How far did the pointer move?
    pointerDelta.x = position.x - m_lookLastPoint.x;
    pointerDelta.y = position.y - m_lookLastPoint.y;

    XMFLOAT2 rotationDelta;
    // Scale for control sensitivity.
    rotationDelta.x = pointerDelta.x * MoveLookConstants::RotationGain;
    rotationDelta.y = pointerDelta.y * MoveLookConstants::RotationGain;
    // Save for next time through.
    m_lookLastPoint = position;

    // Update our orientation based on the command.
    m_pitch -= rotationDelta.y;
    m_yaw += rotationDelta.x;

    // Limit pitch to straight up or straight down.
    float limit = XM_PI / 2.0f - 0.01f;
    m_pitch = __max(-limit, m_pitch);
    m_pitch = __min(+limit, m_pitch);
    ...
}
```

La última parte que veremos es cómo el juego de ejemplo controla el evento de pantalla táctil de [**PointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) .
Una vez que el usuario ha terminado el gesto de toque y quitado el dedo de la pantalla, se inicia [**MoveLookController:: OnPointerReleased**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) .
Si el identificador del puntero que desencadenó el evento [**PointerReleased**](/uwp/api/windows.ui.core.corewindow.pointerreleased) es el identificador del puntero de movimiento grabado anteriormente, **MoveLookController** establece la velocidad en `0` porque el reproductor dejó de tocar el área de búsqueda.

```cppwinrt
else if (pointerID == m_lookPointerID)
{
    m_lookInUse = false;
    m_lookPointerID = 0;
}
```

## <a name="adding-mouse-and-keyboard-support"></a>Agregar compatibilidad con el mouse y el teclado

Este juego tiene el siguiente diseño de control para el teclado y el mouse.

Datos proporcionados por el usuario | Acción
:------- | :--------
W | Avanzar el reproductor
A | Mueve el jugador a la izquierda
S | Bajar reproductor hacia atrás
D | Movimiento del jugador a la derecha
X | Subir vista
Barra espaciadora | Bajar vista
P | Pausar el juego
Movimiento del mouse | Cambiar la rotación (el tono y la guiñada) de la vista de cámara
Botón primario del mouse | Activar una esfera


Para usar el teclado, el juego de ejemplo registra dos nuevos eventos, [**CoreWindow:: KeyUp**](/uwp/api/windows.ui.core.corewindow.keyup) y [**CoreWindow:: KeyDown**](/uwp/api/windows.ui.core.corewindow.keydown), dentro del método [**MoveLookController:: InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L84-L88) . Estos eventos controlan la prensa y la liberación de una clave.

```cppwinrt
window.KeyDown({ this, &MoveLookController::OnKeyDown });

window.KeyUp({ this, &MoveLookController::OnKeyUp });
```

El mouse se trata de forma ligeramente diferente a los controles táctiles aunque use un puntero. Para alinear con nuestro diseño de control, el **MoveLookController** gira la cámara cada vez que se mueve el mouse y se activa cuando se presiona el botón primario del mouse.

Esto se controla en el método [**OnPointerPressed**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L179-L313) de **MoveLookController**.

En este método, comprobamos qué tipo de dispositivo de puntero se usa con la [`Windows::Devices::Input::PointerDeviceType`](/uwp/api/Windows.Devices.Input.PointerDeviceType) enumeración. Si el juego está **activo** y el **PointerDeviceType** no está en **contacto**, se supone que es la entrada del mouse.

```cppwinrt
case MoveLookControllerState::Active:
    switch (pointerDeviceType)
    {
    case winrt::Windows::Devices::Input::PointerDeviceType::Touch:
        // Behavior for touch controls
        ...

    default:
        // Behavior for mouse controls
        bool rightButton = pointProperties.IsRightButtonPressed();
        bool leftButton = pointProperties.IsLeftButtonPressed();

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
            // These are for smoothing.
            m_lookLastDelta.x = m_lookLastDelta.y = 0;
        }
        break;
    }
    break;
```

Cuando el reproductor deja de presionar uno de los botones del mouse, se genera el evento [CoreWindow::P ointerreleased](/uwp/api/Windows.UI.Core.CoreWindow.PointerReleased) del mouse, llamando al método [MoveLookController:: OnPointerReleased](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L441-L500) y se completa la entrada. En este momento, las esferas dejarán de activarse si se presiona el botón primario del mouse y ahora se libera. Dado que la opción Buscar siempre está habilitada, el juego sigue usando el mismo puntero del mouse para realizar el seguimiento de los eventos de búsqueda en curso.

```cppwinrt
case MoveLookControllerState::Active:
    // Touch points
    if (pointerID == m_movePointerID)
    {
        // Stop movement
        ...
    }
    else if (pointerID == m_lookPointerID)
    {
        // Stop look rotation
        ...
    }
    // Fire button has been released
    else if (pointerID == m_firePointerID)
    {
        // Stop firing
        ...
    }
    // Mouse point
    else if (pointerID == m_mousePointerID)
    {
        bool rightButton = pointProperties.IsRightButtonPressed();
        bool leftButton = pointProperties.IsLeftButtonPressed();

        // Mouse no longer in use so stop firing
        m_mouseInUse = false;

        // Don't clear the mouse pointer ID so that Move events still result in Look changes.
        // m_mousePointerID = 0;
        m_mouseLeftInUse = leftButton;
        m_mouseRightInUse = rightButton;
    }
    break;
```

Ahora veamos el último tipo de control que vamos a admitir: los controladores de juegos. Los controladores de juegos se controlan de forma independiente de los controles táctiles y del mouse, ya que no usan el objeto de puntero. Por este motivo, es necesario agregar algunos nuevos métodos y controladores de eventos.

## <a name="adding-gamepad-support"></a>Agregar compatibilidad con el controlador para juegos

Para este juego, se agrega compatibilidad con el controlador de juegos mediante llamadas a las API de [Windows. Gaming. Input](/uwp/api/windows.gaming.input) . Este conjunto de API proporciona acceso a entradas del controlador de juego, como ruedas de carreras y bastones de vuelo. 

Los siguientes son los controles del controlador para juegos.

Datos proporcionados por el usuario | Acción
:------- | :--------
Stick analógico izquierdo | Movimiento del reproductor
Stick analógico derecho | Cambiar la rotación (el tono y la guiñada) de la vista de cámara
Gatillo derecho | Activar una esfera
Botón Inicio/menú | Pausar o reanudar el juego

En el método [**InitWindow**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L68-L103) , se agregan dos nuevos eventos para determinar si se ha [agregado](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1100-L1105) o [quitado](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1109-L1114)un controlador de juegos. Estos eventos actualizan la propiedad **m_gamepadsChanged** . Se usa en el método **UpdatePollingDevices** para comprobar si la lista de controladores de mandos conocidos ha cambiado. 

```cppwinrt
// Detect gamepad connection and disconnection events.
Gamepad::GamepadAdded({ this, &MoveLookController::OnGamepadAdded });

Gamepad::GamepadRemoved({ this, &MoveLookController::OnGamepadRemoved });
```

> [!NOTE]
> Las aplicaciones UWP no pueden recibir la entrada desde un controlador de Xbox One mientras la aplicación no está en el foco.

### <a name="the-updatepollingdevices-method"></a>El método UpdatePollingDevices

El método [**UpdatePollingDevices**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L654-L782) de la instancia de **MoveLookController** comprueba inmediatamente si hay un controlador de juegos conectado. Si es así, comenzaremos a leer su estado con el [**controlador para juegos. GetCurrentReading**](/uwp/api/windows.gaming.input.gamepad.GetCurrentReading). Esto devuelve el struct [**GamepadReading**](/uwp/api/Windows.Gaming.Input.GamepadReading) , lo que nos permite comprobar qué botones se han pulsado o thumbsticks.

Si el estado del juego es **WaitForInput**, solo escuchamos el botón de inicio o menú del controlador para que se pueda reanudar el juego.

Si está **activo**, comprobamos la entrada del usuario y determinamos qué acción del juego debe ocurrir.
Por ejemplo, si el usuario moviera el stick analógico izquierdo en una dirección específica, esto permite que el juego sepa que necesitamos trasladar el jugador en la dirección en la que se mueve el stick. El movimiento del palo en una dirección específica debe registrarse como mayor que el radio de la **zona muerta**; de lo contrario, no se producirá nada. Este radio de zona muerta es necesario para evitar la "derivación", que es cuando el controlador recoge movimientos pequeños desde el pulgar del jugador a medida que se sitúa sobre el palo. Sin zonas muertas, los controles pueden parecer demasiado confidenciales para el usuario.

La entrada del stick analógico está entre-1 y 1 para el eje x e y. El siguiente consant especifica el radio de la zona muerta del Stick.

```cppwinrt
#define THUMBSTICK_DEADZONE 0.25f
```

Con esta variable, comenzaremos a procesar la entrada de interbloqueo accionable. El movimiento se produciría con un valor de [-1,-.26] o [. 26, 1] en cualquiera de los dos ejes.

![zona muerta para thumbsticks](images/simple-dx-game-controls-deadzone.png)

Esta parte del método **UpdatePollingDevices** controla las thumbsticks izquierda y derecha.
Se comprueban los valores X e y de cada Stick para ver si están fuera de la zona muerta. Si uno o ambos son, actualizaremos el componente correspondiente.
Por ejemplo, si el stick izquierdo se mueve a la izquierda a lo largo del eje X, agregaremos-1 al componente **X** del vector de **m_moveCommand** . Este vector es lo que se usará para agregar todos los movimientos en todos los dispositivos y se usará más adelante para calcular dónde debe moverse el reproductor. 

```cppwinrt
// Use the left thumbstick to control the eye point position
// (position of the player).

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

De forma similar a cómo se mueve el stick izquierdo, el stick derecho controla el giro de la cámara.

El comportamiento del stick de control deslizante derecho se alinea con el comportamiento del movimiento del mouse en la configuración del mouse y del control del teclado.
Si el palo está fuera de la zona muerta, se calcula la diferencia entre la posición actual del puntero y el lugar en el que el usuario está intentando buscar.
Este cambio en la posición del puntero (**pointerDelta**) se usa para actualizar el tono y la guiñada de la rotación de la cámara que posteriormente se aplica en nuestro método de **actualización** .
El vector **pointerDelta** puede resultar familiar porque también se usa en el método [MoveLookController:: OnPointerMoved](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L318-L395) para realizar un seguimiento del cambio en la posición del puntero para nuestras entradas táctiles y del mouse.

```cppwinrt
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
// Scale for control sensitivity.
rotationDelta.x = pointerDelta.x * 0.08f;
rotationDelta.y = pointerDelta.y * 0.08f;

// Update our orientation based on the command.
m_pitch += rotationDelta.y;
m_yaw += rotationDelta.x;

// Limit pitch to straight up or straight down.
m_pitch = __max(-XM_PI / 2.0f, m_pitch);
m_pitch = __min(+XM_PI / 2.0f, m_pitch);
```

Los controles del juego no se completarán sin la capacidad de activar los esferas.

Este método **UpdatePollingDevices** también comprueba si se está presionando el desencadenador derecho. Si es así, la propiedad **m_firePressed** se voltea a true, señalando al Juego de que las esferas deben empezar a desencadenarse.
```cppwinrt
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

## <a name="the-update-method"></a>El método Update

Para envolver cosas, vamos a profundizar en el método de [**actualización**](https://github.com/Microsoft/Windows-universal-samples/blob/ef073ed8a2007d113af1d88eddace479e3bf0e07/SharedContent/cpp/GameContent/MoveLookController.cpp#L1005-L1096) .
Este método combina los movimientos o los giros que el jugador realizó con cualquier entrada admitida para generar un vector de velocidad y actualizar los valores de paso y guiñada para que el bucle del juego tenga acceso.

El método **Update** inicia las cosas mediante una llamada a [**UpdatePollingDevices**](#the-updatepollingdevices-method) para actualizar el estado del controlador. Este método también recopila cualquier entrada de un controlador de juegos y agrega sus movimientos al vector **m_moveCommand** . 

En nuestro método de **actualización** , realizamos las siguientes comprobaciones de entrada.
- Si el reproductor usa el rectángulo del controlador de movimiento, determinaremos el cambio en la posición del puntero y lo usaremos para calcular si el usuario ha movido el puntero fuera de la zona muerta del controlador. Si está fuera de la zona muerta, la propiedad vector de **m_moveCommand** se actualiza con el valor del joystick virtual.
- Si se presiona cualquiera de las entradas de teclado de movimiento, se agrega un valor de `1.0f` o `-1.0f` en el componente correspondiente del vector de **m_moveCommand** &mdash; `1.0f` para reenviar y `-1.0f` hacia atrás.

Una vez que se ha tenido en cuenta toda la entrada de movimiento, se ejecuta el vector de **m_moveCommand** a través de algunos cálculos para generar un nuevo vector que representa la dirección del jugador con respecto al mundo de juego.
A continuación, tomamos nuestros movimientos en relación con el mundo y los aplicamos al reproductor como velocidad en esa dirección.
Por último, se restablece el vector de **m_moveCommand** en para `(0.0f, 0.0f, 0.0f)` que todo esté listo para el siguiente fotograma del juego.

```cppwinrt
void MoveLookController::Update()
{
    // Get any gamepad input and update state
    UpdatePollingDevices();

    if (m_moveInUse)
    {
        // Move control.
        XMFLOAT2 pointerDelta;

        pointerDelta.x = m_movePointerPosition.x - m_moveFirstDown.x;
        pointerDelta.y = m_movePointerPosition.y - m_moveFirstDown.y;

        // Figure out the command from the virtual joystick.
        XMFLOAT3 commandDirection = XMFLOAT3(0.0f, 0.0f, 0.0f);
        // Leave 32 pixel-wide dead spot for being still.
        if (fabsf(pointerDelta.x) > 16.0f)
            m_moveCommand.x -= pointerDelta.x / fabsf(pointerDelta.x);

        if (fabsf(pointerDelta.y) > 16.0f)
            m_moveCommand.y -= pointerDelta.y / fabsf(pointerDelta.y);
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
    wCommand.x = m_moveCommand.x * cosf(m_yaw) - m_moveCommand.y * sinf(m_yaw);
    wCommand.y = m_moveCommand.x * sinf(m_yaw) + m_moveCommand.y * cosf(m_yaw);
    wCommand.z = m_moveCommand.z;

    // Scale for sensitivity adjustment.
    // Our velocity is based on the command. Y is up.
    m_velocity.x = -wCommand.x * MoveLookConstants::MovementGain;
    m_velocity.z = wCommand.y * MoveLookConstants::MovementGain;
    m_velocity.y = wCommand.z * MoveLookConstants::MovementGain;

    // Clear movement input accumulator for use during next frame.
    m_moveCommand = XMFLOAT3(0.0f, 0.0f, 0.0f);
}
```

## <a name="next-steps"></a>Pasos siguientes

Ahora que hemos agregado nuestros controles, hay otra característica que necesitamos agregar para crear un juego envolvente: sonido.
La música y los efectos sonoros son importantes para cualquier juego, así que vamos a analizar [Cómo agregar el sonido](tutorial--adding-sound.md) siguiente.
