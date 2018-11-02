---
author: mtoepke
title: Controles de movimiento y vista para juegos
description: Aprende a agregar controles de movimiento y vista tradicionales de mouse y teclado (también conocidos como controles mouselook) a un juego DirectX.
ms.assetid: 4b4d967c-3de9-8a97-ae68-0327f00cc933
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, controles, movimiento y vista
ms.localizationpriority: medium
ms.openlocfilehash: 219d014eb03803ace440dc1c1773043a9ecbc99f
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5939982"
---
# <a name="span-iddevgamingtutorialaddingmove-lookcontrolstoyourdirectxgamespanmove-look-controls-for-games"></a><span id="dev_gaming.tutorial__adding_move-look_controls_to_your_directx_game"></span>Controles de movimiento y vista para juegos



Aprende a agregar controles de movimiento y vista tradicionales de mouse y teclado (también conocidos como controles mouselook) a un juego DirectX.

También hablaremos de la compatibilidad de movimiento y vista para dispositivos táctiles, con el controlador de movimiento definido como la sección inferior izquierda de la pantalla que se comporta como una entrada direccional, y el controlador de vista definido para el recordatorio de la pantalla, con una cámara que se centra en el último lugar que el jugador tocó en esa área.

Si se trata de un concepto de control desconocido para ti, piensa en ello de esta forma: el teclado (o cuadro de entrada direccional basado en funciones táctiles) controla tus piernas en este espacio 3D y se comporta como si tus piernas solo fueran capaces de moverse hacia delante o hacia atrás o girar hacia la izquierda o la derecha. El mouse (o puntero táctil) controla tu cabeza. Usas la cabeza para mirar en una dirección: izquierda o derecha, arriba o abajo, o en algún lugar en ese plano. Si hay un objetivo a la vista, usarías el mouse para centrar la vista de cámara en el objetivo y, a continuación, presionarías la tecla adelante para moverte hacia él, o la tecla atrás para alejarte de él. Para hacer un círculo alrededor del objetivo, mantendrías la vista de cámara fija en el objetivo, y te moverías hacia la izquierda o hacia la derecha al mismo tiempo. Como puedes comprobar, este método de control es muy eficaz para navegar por entornos 3D.

En el mundo de los juegos, estos controles se conocen comúnmente como controles WASD, puesto que las teclas W, A, S y D se usan para el movimiento de cámara fija en el plano x-z y el mouse se usa para controlar la rotación de la cámara sobre los ejes x e y.

## <a name="objectives"></a>Objetivos


-   Agregar controles básicos de movimiento y vista a tu juego DirectX para mouse, teclado y pantallas táctiles.
-   Implementar una cámara en primera persona usada para navegar por el entorno en 3D.

## <a name="a-note-on-touch-control-implementations"></a>Nota sobre las implementaciones de controles táctiles


Para los controles táctiles, implementamos dos controladores: el controlador de movimiento, que se encarga del movimiento en el plano x-z relativo al punto de vista de la cámara, y el controlador de vista, que apunta el punto de vista de la cámara. Nuestro controlador de movimiento se asigna a los teclas WASD del teclado y el controlador de vista, al mouse. Sin embargo, para los controles táctiles es necesario definir una región de la pantalla que funcione como las entradas direccionales, o las teclas WASD virtuales, con el resto de la pantalla como espacio de entrada para los controles de vista.

Nuestra pantalla tiene esta apariencia.

![diseño del controlador de movimiento y vista](images/movelook-touch.png)

Cuando mueves el puntero táctil (¡no hablamos del mouse!) en la parte inferior izquierda de la pantalla, cualquier movimiento hacia arriba hará que la cámara se mueva hacia delante. Cualquier movimiento hacia abajo moverá la cámara hacia atrás. Lo mismo ocurre con el movimiento a izquierda y derecha dentro del espacio de puntero del controlador de movimiento. Fuera de ese espacio, y se convierte en un controlador de vista: basta con tocar o arrastrar la cámara al lugar hacia el que quieras mirar.

## <a name="set-up-the-basic-input-event-infrastructure"></a>Configuración de la infraestructura básica para eventos de entrada


En primer lugar, debemos crear la clase de control que usaremos para controlar los eventos de entrada del mouse y del teclado, y actualizar la perspectiva de la cámara según dicha entrada. Debido a que vamos a implementar controles de movimiento y vista, recibe el nombre de **MoveLookController**.

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <DirectXMath.h>

// Methods to get input from the UI pointers
ref class MoveLookController
{
};  // class MoveLookController
```

Ahora vamos a crear un encabezado que defina el estado del controlador de movimiento y vista y su cámara en primera persona, más los métodos básicos y controladores de eventos que implementan los controles y que actualiza el estado de la cámara.

```cpp
#define ROTATION_GAIN 0.004f    // Sensitivity adjustment for the look controller
#define MOVEMENT_GAIN 0.1f      // Sensitivity adjustment for the move controller

ref class MoveLookController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // The position of the controller
    float m_pitch, m_yaw;           // Orientation euler angles in radians

    // Properties of the Move control
    bool m_moveInUse;               // Specifies whether the move control is in use
    uint32 m_movePointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_moveFirstDown;          // Point where initial contact occurred
    DirectX::XMFLOAT2 m_movePointerPosition;   // Point where the move pointer is currently located
    DirectX::XMFLOAT3 m_moveCommand;            // The net command from the move control

    // Properties of the Look control
    bool m_lookInUse;               // Specifies whether the look control is in use
    uint32 m_lookPointerID;         // Id of the pointer in this control
    DirectX::XMFLOAT2 m_lookLastPoint;          // Last point (from last frame)
    DirectX::XMFLOAT2 m_lookLastDelta;          // For smoothing

    bool m_forward, m_back;         // States for movement
    bool m_left, m_right;
    bool m_up, m_down;


public:

    // Methods to get input from the UI pointers
    void OnPointerPressed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnPointerMoved(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnPointerReleased(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::PointerEventArgs^ args
        );

    void OnKeyDown(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    void OnKeyUp(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::KeyEventArgs^ args
        );

    // Set up the Controls that this controller supports
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );
    
internal:
    // Accessor to set position of controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

    // Accessor to set position of controller
    void SetOrientation( _In_ float pitch, _In_ float yaw );

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

    // Returns the point  which the controller is facing
    DirectX::XMFLOAT3 get_LookPoint();


};  // class MoveLookController
```

Nuestro código contiene 4 grupos de campos privados. Revisemos la finalidad de cada uno de ellos.

En primer lugar, definimos algunos campos útiles que almacenan la información actualizada acerca da la vista de la cámara.

-   **m\_position** es la posición de la cámara (y por tanto del plano de visión) en la escena 3D mediante el uso coordenadas de escena.
-   **m\_pitch** es la rotación alrededor del eje X (pitch) de la cámara o su rotación arriba-abajo sobre el eje X del plano de visión (en radianes).
-   **m\_yaw** es la rotación alrededor del eje Y (yaw) de la cámara o su rotación izquierda-derecha sobre el eje Y del plano de visión (en radianes).

Ahora, vamos a definir los campos que usamos para almacenar información sobre el estado y la posición de nuestros controladores. Primero definiremos los campos que necesitamos para el controlador de movimiento táctil. (No se necesita nada especial para la implementación para teclado del controlador de movimiento. Los eventos de teclado simplemente se leen con controladores específicos).

-   **m\_moveInUse** indica si el controlador de movimiento está en uso.
-   **m\_movePointerID** es el identificador único del puntero de movimiento actual. Lo usamos para diferenciar entre el puntero de vista y el puntero de movimiento cuando comprobamos el valor del id. de puntero.
-   **m\_moveFirstDown** es el punto de la pantalla donde el jugador tocó por primera vez el área del puntero del controlador de movimiento. Usaremos este valor más adelante para establecer una zona muerte y evitar que movimientos minúsculos hagan vibrar la vista.
-   **m\_movePointerPosition** es el punto de la pantalla al cual el jugador acaba de mover el puntero. Lo usamos para determinar la dirección a la que quería moverse el jugador examinándolo con relación a **m\_moveFirstDown**.
-   **m\_moveCommand** es el comando calculado final del controlador de movimiento: arriba (adelante), abajo (atrás), izquierda o derecha.

Ahora vamos a definir los campos que usaremos para el controlador de vista, tanto las implementaciones táctiles como las de mouse.

-   **m\_lookInUse** indica si el control de vista está en uso.
-   **m\_lookPointerID** es el identificador único del puntero de vista actual. Lo usamos para diferenciar entre el puntero de vista y el puntero de movimiento cuando comprobamos el valor del id. de puntero.
-   **m\_lookLastPoint** es el último punto (en coordenadas de escena) que se capturó en el fotograma anterior.
-   **m\_lookLastDelta** es la diferencia calculada entre los valores actuales **m\_position** y **m\_lookLastPoint**.

Finalmente definimos 6 valores booleanos para los 6 grados de movimiento, que usamos para indicar el estado actual de cada acción de movimiento direccional (activa o inactiva):

-   **m\_forward**, **m\_back**, **m\_left**, **m\_right**, **m\_up** y **m\_down**.

Usamos los 6 controladores de eventos para capturar los datos de entrada con los que actualizamos el estado de los controladores:

-   **OnPointerPressed**. El jugador presionó el botón primario del mouse con el puntero en nuestra pantalla de juego o tocó la pantalla.
-   **OnPointerMoved**. El jugador movió el mouse con el puntero en nuestra pantalla de juego, o arrastró el puntero táctil en la pantalla.
-   **OnPointerReleased**. El jugador soltó el botón izquierdo del mouse con el puntero en nuestra pantalla de juego, o dejó de tocar la pantalla.
-   **OnKeyDown**. El jugador presionó una tecla.
-   **OnKeyUp**. El jugador liberó una tecla.

Y, por último, usamos estos métodos y propiedades para inicializar, acceder y actualizar la información de estado de los controladores.

-   **Initialize**. Nuestra aplicación llama a este controlador de eventos para inicializar los controles y adjuntarlos al objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que describe nuestra ventana.
-   **SetPosition**. Nuestra aplicación llama a este método para establecer las coordenadas (x, y y z) de nuestros controles en el espacio de la escena.
-   **SetOrientation**. Nuestra aplicación llama a este método para establecer la rotación alrededor de los ejes X e Y de la cámara.
-   **get\_Position**. Nuestra aplicación accede a esta propiedad para obtener la posición actual de la cámara en el espacio de la escena. Puedes usar esta propiedad como forma de comunicar la posición actual de la cámara a la aplicación.
-   **get\_LookPoint**. Nuestra aplicación accede a esta propiedad para obtener el punto actual hacia el que apunta la cámara del controlador.
-   **Update**. Lee el estado de los controladores de movimiento y vista y actualiza la posición de la cámara. Llamas a este método continuamente desde el bucle principal de la aplicación para actualizar los datos del controlador de cámara y la posición de la cámara en el espacio de la escena.

Ahora ya tenemos todos los componentes necesarios para implementar tus controles de movimiento y vista. Por tanto, intentemos combinar todas las piezas.

## <a name="create-the-basic-input-events"></a>Creación de los eventos de entrada básicos


El distribuidor de eventos de Windows Runtime proporciona 5 eventos que queremos que controlen las instancias de la clase **MoveLookController**:

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)
-   [**KeyUp**](https://msdn.microsoft.com/library/windows/apps/br208271)
-   [**KeyDown**](https://msdn.microsoft.com/library/windows/apps/br208270)

Estos eventos se implementan en el tipo [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Damos por sentado que tienes un objeto **CoreWindow** con el que trabajar. Si no sabes cómo obtener uno, consulta el tema sobre cómo [configurar una aplicación C++ para la Plataforma universal de Windows (UWP) para mostrar una vista DirectX](https://msdn.microsoft.com/library/windows/apps/hh465077).

Puesto que estos eventos se desencadenan mientras nuestra aplicación se ejecuta, los controladores actualizan la información del estado de los controladores definidos en nuestros campos privados.

En primer lugar, rellenemos los controladores de eventos de puntero táctiles y de mouse. En el primer controlador de eventos, **OnPointerPressed()**, obtenemos las coordenadas x-y del puntero del [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que administra nuestra pantalla cuando el usuario hace clic o toca la pantalla en la región del controlador de vista.

**OnPointerPressed**

```cpp
void MoveLookController::OnPointerPressed(
_In_ CoreWindow^ sender,
_In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    if ( deviceType == PointerDeviceType::Mouse )
    {
        // Action, Jump, or Fire
    }

    // Check  if this pointer is in the move control.
    // Change the values  to percentages of the preferred screen resolution.
    // You can set the x value to <preferred resolution> * <percentage of width>
    // for example, ( position.x < (screenResolution.x * 0.15) ).

    if (( position.x < 300 && position.y > 380 ) && ( deviceType != PointerDeviceType::Mouse ))
    {
        if ( !m_moveInUse ) // if no pointer is in this control yet
        {
            // Process a DPad touch down event.
            m_moveFirstDown = position;                 // Save the location of the initial contact.
            m_movePointerPosition = position;
            m_movePointerID = pointerID;                // Store the id of the pointer using this control.
            m_moveInUse = TRUE;
        }
    }
    else // This pointer must be in the look control.
    {
        if ( !m_lookInUse ) // If no pointer is in this control yet...
        {
            m_lookLastPoint = position;                         // save the point for later move
            m_lookPointerID = args->CurrentPoint->PointerId;  // store the id of pointer using this control
            m_lookLastDelta.x = m_lookLastDelta.y = 0;          // these are for smoothing
            m_lookInUse = TRUE;
        }
    }
}
```

Este controlador de eventos comprueba si el puntero es o no el mouse (esta muestra admite tanto mouse como toque) y si se encuentra en el área del controlador de movimiento. Si ambos criterios son verdaderos, comprueba si el puntero acaba de presionarse, más específicamente si este clic está relacionado con una entrada anterior de movimiento o vista), comprobando si **m\_moveInUse** es falso. Si lo es, el controlador captura el punto en el área del controlador de movimiento donde se produjo la presión y establece **m\_moveInUse** en verdadero, de modo que cuando se vuelva a llamar a este controlador no tenga que sobrescribir la posición de inicio de la interacción de entrada del controlador de movimiento. También actualiza el id. de puntero del controlador de movimiento a la id. del puntero actual.

Si el puntero es el mouse o si el puntero táctil no se encuentra en el área del controlador de movimiento, debe estar en el área del controlador de vista. Establece **m\_lookLastPoint** en la posición actual donde el usuario presionó el botón del mouse o tocó y presionó, restablece la diferencia y actualiza el identificador de puntero del controlador de vista al identificador del puntero actual. También establece el estado del controlador de vista en activo.

**OnPointerMoved**

```cpp
void MoveLookController::OnPointerMoved(
    _In_ CoreWindow ^sender,
    _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2(args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y);

    // Decide which control this pointer is operating.
    if (pointerID == m_movePointerID)           // This is the move pointer.
    {
        // Move control
        m_movePointerPosition = position;       // Save the current position.

    }
    else if (pointerID == m_lookPointerID)      // This is the look pointer.
    {
        // Look control

        DirectX::XMFLOAT2 pointerDelta;
        pointerDelta.x = position.x - m_lookLastPoint.x;        // How far did pointer move
        pointerDelta.y = position.y - m_lookLastPoint.y;

        DirectX::XMFLOAT2 rotationDelta;
        rotationDelta.x = pointerDelta.x * ROTATION_GAIN;   // Scale for control sensitivity.
        rotationDelta.y = pointerDelta.y * ROTATION_GAIN;

        m_lookLastPoint = position;                     // Save for the next time through.

                                                        // Update our orientation based on the command.
        m_pitch -= rotationDelta.y;                     // Mouse y increases down, but pitch increases up.
        m_yaw -= rotationDelta.x;                       // Yaw is defined as CCW around the y-axis.

                                                        // Limit the pitch to straight up or straight down.
        m_pitch = (float)__max(-DirectX::XM_PI / 2.0f, m_pitch);
        m_pitch = (float)__min(+DirectX::XM_PI / 2.0f, m_pitch);
    }
}
```

El controlador de eventos **OnPointerMoved** se desencadena cada vez que se mueve el puntero; es decir, si se arrastra un puntero de la pantalla táctil o si el puntero del mouse se mueve mientras se presiona el botón izquierdo. Si el identificador del puntero es el mismo que el identificador del puntero del controlador de movimiento, se trata de un puntero de movimiento; de lo contrario, comprobamos si el puntero activo es el controlador de vista.

Si es el controlador de movimiento, simplemente actualizamos la posición del puntero. Seguimos actualizándolo siempre que el evento [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276) se siga desencadenando, porque queremos comparar la posición final con la primera que capturamos con el controlador de eventos **OnPointerPressed**.

Si es el controlador de vista, las cosas se complican un poquito más. Necesitamos calcular un nuevo punto de vista y centrar la cámara en él, por lo que calculamos la diferencia entre el último punto de vista y la posición actual en pantalla y, a continuación, multiplicamos con nuestra escala de factores, que podemos ajustar para que los movimientos de vista parezcan más pequeños o más grandes en comparación con la distancia del movimiento en la pantalla. Usando este valor, calculamos el cabeceo y la guiñada.

Por último, necesitamos desactivar los comportamientos de movimiento o vista cuando el jugador para de mover el mouse o de tocar la pantalla. Usamos **OnPointerReleased**, al que llamamos cuando [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) se desencadena, para establecer **m\_moveInUse** o **m\_lookInUse** en FALSE y desactivar el movimiento panorámico de la cámara y para reducir a cero el identificador de puntero.

**OnPointerReleased**

```cpp
void MoveLookController::OnPointerReleased(
_In_ CoreWindow ^sender,
_In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );


    if ( pointerID == m_movePointerID )    // This was the move pointer.
    {
        m_moveInUse = FALSE;
        m_movePointerID = 0;
    }
    else if (pointerID == m_lookPointerID ) // This was the look pointer.
    {
        m_lookInUse = FALSE;
        m_lookPointerID = 0;
    }
}
```

Hasta ahora hemos tratado todos los eventos de pantalla táctil. Ahora vamos a controlar los eventos de entrada clave para un controlador de movimiento basado en teclado.

**OnKeyDown**

```cpp
void MoveLookController::OnKeyDown(
                                   __in CoreWindow^ sender,
                                   __in KeyEventArgs^ args )
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // Forward
        m_forward = true;
    if ( Key == VirtualKey::S )     // Back
        m_back = true;
    if ( Key == VirtualKey::A )     // Left
        m_left = true;
    if ( Key == VirtualKey::D )     // Right
        m_right = true;
}
```

Mientras una de las teclas esté presionada, este controlador de eventos establece el estado de movimiento direccional correspondiente en verdadero.

**OnKeyUp**

```cpp
void MoveLookController::OnKeyUp(
                                 __in CoreWindow^ sender,
                                 __in KeyEventArgs^ args)
{
    Windows::System::VirtualKey Key;
    Key = args->VirtualKey;

    // Figure out the command from the keyboard.
    if ( Key == VirtualKey::W )     // forward
        m_forward = false;
    if ( Key == VirtualKey::S )     // back
        m_back = false;
    if ( Key == VirtualKey::A )     // left
        m_left = false;
    if ( Key == VirtualKey::D )     // right
        m_right = false;
}
```

Cuando la tecla se suelta, el controlador de eventos lo revierte a falso. Cuando llamamos a **Update**, comprueba los estados de movimiento direccional y mueve la cámara en consonancia. Este proceso es mucho más simple que la implementación táctil.

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>Inicialización de los controles táctiles y del estado del controlador


Encadenemos ahora los eventos e inicialicemos todos los campos de estado de controladores.

**Initialize**

```cpp
void MoveLookController::Initialize( _In_ CoreWindow^ window )
{

    // Opt in to recieve touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &MoveLookController::OnPointerReleased);

    window->CharacterReceived +=
    ref new TypedEventHandler<CoreWindow^, CharacterReceivedEventArgs^>(this, &MoveLookController::OnCharacterReceived);

    window->KeyDown += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyDown);

    window->KeyUp += 
    ref new TypedEventHandler<CoreWindow^, KeyEventArgs^>(this, &MoveLookController::OnKeyUp);

    // Initialize the state of the controller.
    m_moveInUse = FALSE;                // No pointer is in the Move control.
    m_movePointerID = 0;

    m_lookInUse = FALSE;                // No pointer is in the Look control.
    m_lookPointerID = 0;

    //  Need to init this as it is reset every frame.
    m_moveCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

    SetOrientation( 0, 0 );             // Look straight ahead when the app starts.

}
```

**Initialize** hace referencia a la instancia [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de la aplicación como un parámetro y registra los controladores de eventos que desarrollamos en los eventos adecuados en **CoreWindow**. Inicializa los id. del puntero de movimiento y vista, establece el vector de comandos para nuestra implementación de movimiento en pantalla táctil en cero y establece que la cámara mire al frente cuando se inicia la aplicación.

## <a name="getting-and-setting-the-position-and-orientation-of-the-camera"></a>Obtención y configuración de la posición y la orientación de la cámara


Definamos algunos métodos para obtener y establecer la posición de la cámara en relación a la ventanilla.

```cpp
void MoveLookController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Accessor to set the position of the controller.
void MoveLookController::SetOrientation( _In_ float pitch, _In_ float yaw )
{
    m_pitch = pitch;
    m_yaw = yaw;
}

// Returns the position of the controller object.
DirectX::XMFLOAT3 MoveLookController::get_Position()
{
    return m_position;
}

// Returns the point at which the camera controller is facing.
DirectX::XMFLOAT3 MoveLookController::get_LookPoint()
{
    float y = sinf(m_pitch);        // Vertical
    float r = cosf(m_pitch);        // In the plane
    float z = r*cosf(m_yaw);        // Fwd-back
    float x = r*sinf(m_yaw);        // Left-right
    DirectX::XMFLOAT3 result(x,y,z);
    result.x += m_position.x;
    result.y += m_position.y;
    result.z += m_position.z;

    // Return m_position + DirectX::XMFLOAT3(x, y, z);
    return result;
}
```

## <a name="updating-the-controller-state-info"></a>Actualización de la información de estado del controlador


Ahora vamos a realizar los cálculos que convierten la información de coordenadas del puntero registrada en **m\_movePointerPosition** en nueva información de coordenadas, con respecto al sistema de coordenadas del mundo. Nuestra aplicación llamará a este método cada vez que actualicemos el bucle de la aplicación principal. Es aquí, por tanto, donde calculamos la información de la nueva posición del punto de vista que luego pasamos a la aplicación para actualizar la matriz de vista antes de proyectarla a la ventanilla.

```cpp
void MoveLookController::Update(CoreWindow ^window)
{
    // Check for input from the Move control.
    if (m_moveInUse)
    {
        DirectX::XMFLOAT2 pointerDelta(m_movePointerPosition);
        pointerDelta.x -= m_moveFirstDown.x;
        pointerDelta.y -= m_moveFirstDown.y;

        // Figure out the command from the touch-based virtual joystick.
        if (pointerDelta.x > 16.0f)      // Leave 32 pixel-wide dead spot for being still.
            m_moveCommand.x = 1.0f;
        else
            if (pointerDelta.x < -16.0f)
            m_moveCommand.x = -1.0f;

        if (pointerDelta.y > 16.0f)      // Joystick y is up, so change sign.
            m_moveCommand.y = -1.0f;
        else
            if (pointerDelta.y < -16.0f)
            m_moveCommand.y = 1.0f;
    }

    // Poll our state bits that are set by the keyboard input events.
    if (m_forward)
        m_moveCommand.y += 1.0f;
    if (m_back)
        m_moveCommand.y -= 1.0f;

    if (m_left)
        m_moveCommand.x -= 1.0f;
    if (m_right)
        m_moveCommand.x += 1.0f;

    if (m_up)
        m_moveCommand.z += 1.0f;
    if (m_down)
        m_moveCommand.z -= 1.0f;

    // Make sure that 45 degree cases are not faster.
    DirectX::XMFLOAT3 command = m_moveCommand;
    DirectX::XMVECTOR vector;
    vector = DirectX::XMLoadFloat3(&command);

    if (fabsf(command.x) > 0.1f || fabsf(command.y) > 0.1f || fabsf(command.z) > 0.1f)
    {
        vector = DirectX::XMVector3Normalize(vector);
        DirectX::XMStoreFloat3(&command, vector);
    }
    

    // Rotate command to align with our direction (world coordinates).
    DirectX::XMFLOAT3 wCommand;
    wCommand.x = command.x*cosf(m_yaw) - command.y*sinf(m_yaw);
    wCommand.y = command.x*sinf(m_yaw) + command.y*cosf(m_yaw);
    wCommand.z = command.z;

    // Scale for sensitivity adjustment.
    wCommand.x = wCommand.x * MOVEMENT_GAIN;
    wCommand.y = wCommand.y * MOVEMENT_GAIN;
    wCommand.z = wCommand.z * MOVEMENT_GAIN;

    // Our velocity is based on the command.
    // Also note that y is the up-down axis. 
    DirectX::XMFLOAT3 Velocity;
    Velocity.x = -wCommand.x;
    Velocity.z = wCommand.y;
    Velocity.y = wCommand.z;

    // Integrate
    m_position.x += Velocity.x;
    m_position.y += Velocity.y;
    m_position.z += Velocity.z;

    // Clear movement input accumulator for use during the next frame.
    m_moveCommand = DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f);

}
```

Puesto que no queremos vibraciones cuando el jugador use nuestro controlador de movimiento táctil, establecemos una zona muerta virtual alrededor del puntero con un diámetro de 32 píxeles. También agregamos velocidad, que es el valor del comando más una frecuencia de ganancia de velocidad. (Puedes ajustar este comportamiento a tu gusto, para frenar o acelerar la velocidad del movimiento basándote en la distancia que se mueve el puntero en el área del controlador de movimiento).

Cuando calculamos la velocidad, también traducimos las coordenadas recibidas de los controladores de movimiento y vista en el movimiento del punto de vista real que enviamos al método que calcula nuestra matriz de vista para la escena. En primer lugar, invertimos la coordenada x, porque si hacemos movemos haciendo clic o arrastramos hacia la izquierda o la derecha con el controlador de vista, el punto de vista rotará en la dirección opuesta en la escena, como una cámara giraría sobre su eje central. Luego, intercambiamos los ejes Y y Z, porque presionar una tecla arriba/abajo o realizar un movimiento táctil de arrastre (interpretado como un comportamiento en el eje Y) en el controlador de movimiento se traduciría en una acción de cámara que mueve el punto de vista dentro o fuera de la pantalla (eje Z).

La posición final del punto de vista para el jugador es la última posición más la velocidad calculada y esto es lo que el representador lee cuando llama al método **get\_Position** (probablemente durante la configuración de cada fotograma). Después, restablecemos el comando de movimiento a cero.

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>Actualización de la matriz de vista con la nueva posición de cámara


Podemos obtener una coordenada del espacio de la escena en la que se centra nuestra cámara, y que se actualiza cada vez que se lo ordenamos a la aplicación (cada 60 segundos en el bucle principal de la aplicación, por ejemplo). Este pseudocódigo sugiere el comportamiento de llamada que puedes implementar:

```cpp
myMoveLookController->Update( m_window );   

// Update the view matrix based on the camera position.
myFirstPersonCamera->SetViewParameters(
                 myMoveLookController->get_Position(),       // Point we are at
                 myMoveLookController->get_LookPoint(),      // Point to look towards
                 DirectX::XMFLOAT3( 0, 1, 0 )                   // Up-vector
                 ); 
```

¡Enhorabuena! Has implementado controles básicos de movimiento y vista tanto para pantallas táctiles como para controles táctiles de entrada de teclado/mouse en tu juego.



 

 




