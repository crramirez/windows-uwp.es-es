---
title: Controles táctiles para juegos
description: Aprende a agregar controles táctiles básicos a tu juego C++ de la Plataforma universal de Windows (UWP) con DirectX.
ms.assetid: 9d40e6e4-46a9-97e9-b848-522d61e8e109
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, entrada táctil, controles, directx, entrada
ms.localizationpriority: medium
ms.openlocfilehash: e8892219b485d320bb77f90ac0d172e8e2403392
ms.sourcegitcommit: 231065c899d0de285584d41e6335251e0c2c4048
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8825235"
---
# <a name="touch-controls-for-games"></a>Controles táctiles para juegos



Aprende a agregar controles táctiles básicos a tu juego C++ de la Plataforma universal de Windows (UWP) con DirectX. Te enseñamos cómo agregar controles basados en el tacto para mover una cámara de plano fijo en un entorno de Direct3D, donde arrastrar con un dedo o un lápiz cambia la perspectiva de la cámara.

Puedes incorporar estos controles en juegos donde desees que el jugador arrastre para desplazarse o para obtener una panorámica de un entorno de 3D, como un mapa o un campo de juego. Por ejemplo, en un juego de estrategia o en un puzzle, puedes usar estos controles para que el jugador pueda ver un entorno de juego más grande que la pantalla desplazándose hacia la izquierda o la derecha.

> **Nota**nuestro código también funciona con controles de movimiento panorámico basados en el mouse. Las API de Windows Runtime abstraen los eventos relacionados con el puntero, de modo que pueden controlar eventos basados tanto en el toque como en el mouse.

 

## <a name="objectives"></a>Objetivos


-   Crear un sencillo control de tocar y arrastrar para obtener una vista panorámica de una cámara de plano fijo en un juego DirectX.

## <a name="set-up-the-basic-touch-event-infrastructure"></a>Configuración de la infraestructura básica para eventos de toque


Primero, definimos nuestro tipo de controlador básico, **CameraPanController** en este caso. Aquí definimos el controlador como una idea abstracta, el conjunto de comportamientos que el usuario puede llevar a cabo.

La clase **CameraPanController** es una colección de información, actualizada con frecuencia, sobre el estado del controlador de cámara y permite a nuestra aplicación obtener esa información de su bucle de actualización.

```cpp
using namespace Windows::UI::Core;
using namespace Windows::System;
using namespace Windows::Foundation;
using namespace Windows::Devices::Input;
#include <directxmath.h>

// Methods to get input from the UI pointers
ref class CameraPanController
{
}
```

Ahora vamos a crear un encabezado que defina el estado del controlador de cámara, y los métodos básicos y controladores de evento que implementan las interacciones del controlador de cámara.

```cpp
ref class CameraPanController
{
private:
    // Properties of the controller object
    DirectX::XMFLOAT3 m_position;               // the position of the camera

    // Properties of the camera pan control
    bool m_panInUse;                
    uint32 m_panPointerID;          
    DirectX::XMFLOAT2 m_panFirstDown;           
    DirectX::XMFLOAT2 m_panPointerPosition;   
    DirectX::XMFLOAT3 m_panCommand;         
    
internal:
    // Accessor to set the position of the controller
    void SetPosition( _In_ DirectX::XMFLOAT3 pos );

       // Accessor to set the fixed "look point" of the controller
       DirectX::XMFLOAT3 get_FixedLookPoint();

    // Returns the position of the controller object
    DirectX::XMFLOAT3 get_Position();

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

    // Set up the Controls supported by this controller
    void Initialize( _In_ Windows::UI::Core::CoreWindow^ window );

    void Update( Windows::UI::Core::CoreWindow ^window );

};  // Class CameraPanController
```

Los campos privados contienen el estado actual del controlador de cámara. Vamos a revisarlos.

-   **m\_position** es la posición de la cámara en el espacio de la escena. En este ejemplo, el valor de la coordenada z está fijado en 0. Podríamos usar DirectX::XMFLOAT2 para representar este valor, pero para el objetivo de esta muestra y la futura extensibilidad, usaremos DirectX::XMFLOAT3. Pasamos este valor a través de la propiedad **get\_Position** a la propia aplicación para que pueda actualizar la ventanilla en consecuencia.
-   **m\_panInUse** es un valor booleano que indica si hay activa una operación de movimiento panorámico o, más específicamente, si el jugador está tocando la pantalla y moviendo la cámara.
-   **m\_panPointerID** es un identificador único para el puntero. No lo vamos a usar en esta muestra, pero es una práctica recomendada asociar la clase de estado del controlador a un puntero específico.
-   **m\_panFirstDown** es el punto de la pantalla que el jugador tocó o hizo clic con el mouse por primera vez durante el movimiento panorámico de la cámara. Usaremos este valor más adelante para establecer una zona muerta y evitar las vibraciones cuando se toca la pantalla o si el mouse vibra un poco.
-   **m\_panPointerPosition** es el punto de la pantalla al cual el jugador acaba de mover el puntero. Lo usamos para determinar la dirección a la que quería moverse el jugador examinándolo con relación a **m\_panFirstDown**.
-   **m\_panCommand** es el comando calculado final del controlador de cámara: arriba, abajo, izquierda o derecha. Puesto que estamos trabajando con una cámara fija en el plazo x-y, también podría ser un valor DirectX::XMFLOAT2.

Usamos estos 3 controladores de eventos para actualizar la información de estado del controlador de cámara.

-   **OnPointerPressed** es un controlador de eventos al que llama la aplicación cuando los jugadores presionan con un dedo en la superficie táctil y el puntero se mueve a las coordenadas del punto presionado.
-   **OnPointerMoved** es un controlador de eventos al que llama la aplicación cuando el jugador desliza el dedo por la superficie táctil. Se actualiza con las nuevas coordenadas de la ruta de arrastre.
-   **OnPointerReleased** es un controlador de eventos al que llama la aplicación cuando el jugador retira el dedo de la superficie táctil.

Por último, usamos estos métodos y propiedades para inicializar, acceder y actualizar la información de estado del controlador de cámara.

-   **Initialize** es un controlador de eventos al que llama la aplicación para inicializar los controles y adjuntarlos al objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que describe la ventana de la pantalla.
-   **SetPosition** es un método al que llama la aplicación para establecer las coordenadas (x, y y z) de nuestros controles en el espacio de la escena. Ten en cuenta que nuestra coordenada z es 0 a lo largo de este tutorial.
-   **get\_Position** es una propiedad a la que accede la aplicación para obtener la posición actual de la cámara en el espacio de la escena. Puedes usar esta propiedad como forma de comunicar la posición actual de la cámara a la aplicación.
-   **get\_FixedLookPoint** es una propiedad a la que accede la aplicación para obtener el punto actual hacia el que apunta la cámara del controlador. En este ejemplo, está bloqueada como normal en el plano x-y.
-   **Update** es un método que lee el estado del controlador y actualiza la posición de la cámara. Puedes llamar a este &lt;something&gt; continuamente desde el bucle principal de la aplicación para actualizar los datos del controlador de cámara y la posición de la cámara en el espacio de la escena.

Ahora ya tenemos todos los componentes necesarios para implementar controles táctiles. Puedes detectar cuándo y dónde se han producido los eventos de puntero  táctiles o de mouse, y qué acción es. Puedes establecer la posición y orientación de la cámara en relación al espacio de la escena y realizar un seguimiento de los cambios. Finalmente, puedes comunicar la nueva posición de la cámara a la aplicación que realiza la llamada.

Ahora, intentemos combinar todas las piezas.

## <a name="create-the-basic-touch-events"></a>Creación de los eventos táctiles básicos


El distribuidor de eventos de Windows Runtime proporciona 3 eventos que queremos que controle nuestra aplicación:

-   [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/br208278)
-   [**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/br208276)
-   [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279)

Estos eventos se implementan en el tipo [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Damos por sentado que tienes un objeto **CoreWindow** con el que trabajar. Para más información, consulta el tema sobre cómo [configurar una aplicación C++ para UWP para mostrar una vista DirectX](https://msdn.microsoft.com/library/windows/apps/hh465077).

Puesto que estos eventos se desencadenan mientras nuestra aplicación se ejecuta, los controladores actualizan la información del estado del controlador de cámara definida en nuestros campos privados.

En primer lugar, rellenemos los controladores de eventos de puntero táctiles. En el primer controlador de eventos, **OnPointerPressed**, obtenemos las coordenadas X-Y del puntero de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que administra nuestra pantalla cuando el usuario toca la pantalla o hace clic con el mouse.

**OnPointerPressed**

```cpp
void CameraPanController::OnPointerPressed(
                                           _In_ CoreWindow^ sender,
                                           _In_ PointerEventArgs^ args)
{
    // Get the current pointer position.
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    auto device = args->CurrentPoint->PointerDevice;
    auto deviceType = device->PointerDeviceType;
    

       if ( !m_panInUse )   // If no pointer is in this control yet.
    {
       m_panFirstDown = position;                   // Save the location of the initial contact.
       m_panPointerPosition = position;
       m_panPointerID = pointerID;              // Store the id of the pointer using this control.
       m_panInUse = TRUE;
    }
    
}
```

Usamos este controlador para que la instancia **CameraPanController** actual sepa que el controlador de cámara debe considerarse activo estableciendo el valor de configuración **m\_panInUse** en TRUE. De este modo, cuando la aplicación llama a **Update**, usará los datos de la posición actual para actualizar la ventanilla.

Ahora que hemos establecido los valores base para el movimiento de la cámara cuando el usuario toca la pantalla o hace clic/presiona la ventana, debemos determinar qué hacer cuando el usuario arrastre por la pantalla o mueva el mouse con un botón presionado.

El controlador de eventos **OnPointerMoved** se desencadena cada vez que el puntero se mueve, con cada tick que el jugador lo arrastra por la pantalla. Debemos asegurarnos de que la aplicación conozca la ubicación actual de puntero, y lo hacemos de este modo.

**OnPointerMoved**

```cpp
void CameraPanController::OnPointerMoved(
                                        _In_ CoreWindow ^sender,
                                        _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panPointerPosition = position;
}
```

Para terminar, debemos desactivar el comportamiento panorámico de la cámara cuando el jugador deje de tocar la pantalla. Usamos **OnPointerReleased**, que al que se llama cuando [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/br208279) se desencadena, para establecer **m\_panInUse** en FALSE, desactivar el movimiento panorámico de la cámara y establecer el identificador de puntero en 0.

**OnPointerReleased**

```cpp
void CameraPanController::OnPointerReleased(
                                             _In_ CoreWindow ^sender,
                                             _In_ PointerEventArgs ^args)
{
    uint32 pointerID = args->CurrentPoint->PointerId;
    DirectX::XMFLOAT2 position = DirectX::XMFLOAT2( args->CurrentPoint->Position.X, args->CurrentPoint->Position.Y );

    m_panInUse = FALSE;
    m_panPointerID = 0;
}
```

## <a name="initialize-the-touch-controls-and-the-controller-state"></a>Inicialización de los controles táctiles y del estado del controlador


Enlacemos los eventos e inicialicemos todos los campos de estado básicos del controlador de cámara.

**Initialize**

```cpp
void CameraPanController::Initialize( _In_ CoreWindow^ window )
{

    // Start recieving touch/mouse events.
    window->PointerPressed += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerPressed);

    window->PointerMoved += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerMoved);

    window->PointerReleased += 
    ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(this, &CameraPanController::OnPointerReleased);


    // Initialize the state of the controller.
    m_panInUse = FALSE;             
    m_panPointerID = 0;

    //  Initialize this as it is reset on every frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

**Initialize** hace referencia a la instancia [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de la aplicación como un parámetro y registra los controladores de eventos que desarrollamos en los eventos adecuados en **CoreWindow**.

## <a name="getting-and-setting-the-position-of-the-camera-controller"></a>Obtención y establecimiento de la posición del controlador de cámara


Definamos algunos métodos para obtener y establecer la posición del controlador de cámara en el espacio de la escena.

```cpp
void CameraPanController::SetPosition( _In_ DirectX::XMFLOAT3 pos )
{
    m_position = pos;
}

// Returns the position of the controller object
DirectX::XMFLOAT3 CameraPanController::get_Position()
{
    return m_position;
}

DirectX::XMFLOAT3 CameraPanController::get_FixedLookPoint()
{
    // For this sample, we don't need to use the trig functions because our
    // look point is fixed. 
    DirectX::XMFLOAT3 result= m_position;
    result.z += 1.0f;
    return result;    

}
```

**SetPosition** es un método público al que podemos llamar desde la aplicación si necesitamos establecer la posición del controlador de cámara en un punto específico.

**get\_Position** es la propiedad pública más importante: gracias a ella la aplicación obtiene la posición actual del controlador de cámara en el espacio de la escena, para poder actualizar la ventanilla en consonancia.

**get\_FixedLookPoint** es una propiedad pública que, en este ejemplo, obtiene un punto de vista que es normal al plano x-y. Puedes cambiar este método para usar las funciones trigonométricas, sin y cos, al calcular los valores de las coordenadas X, Y y Z si quieres crear ángulos más oblicuos para la cámara fija.

## <a name="updating-the-camera-controller-state-information"></a>Actualización de la información de estado del controlador de cámara


Ahora vamos a realizar los cálculos que convierten la información de coordenadas del puntero registrada en **m\_panPointerPosition** en nueva información de coordenadas con respecto al espacio de la escena en 3D. Nuestra aplicación llamará a este método cada vez que actualicemos el bucle de la aplicación principal. Aquí calculamos la información de la nueva posición que luego pasamos a la aplicación que se usa para actualizar la matriz de vista antes de proyectarla a la ventanilla.

```cpp

void CameraPanController::Update( CoreWindow ^window )
{
    if ( m_panInUse )
    {
        pointerDelta.x = m_panPointerPosition.x - m_panFirstDown.x;
        pointerDelta.y = m_panPointerPosition.y - m_panFirstDown.y;

        if ( pointerDelta.x > 16.0f )        // Leave 32 pixel-wide dead spot for being still.
            m_panCommand.x += 1.0f;
        else
            if ( pointerDelta.x < -16.0f )
                m_panCommand.x += -1.0f;

        if ( pointerDelta.y > 16.0f )        
            m_panCommand.y += 1.0f;
        else
            if (pointerDelta.y < -16.0f )
                m_panCommand.y += -1.0f;
    }

       DirectX::XMFLOAT3 command = m_panCommand;
   
    // Our velocity is based on the command.
    DirectX::XMFLOAT3 Velocity;
    Velocity.x =  command.x;
    Velocity.y =  command.y;
    Velocity.z =  0.0f;

    // Integrate
    m_position.x = m_position.x + Velocity.x;
    m_position.y = m_position.y + Velocity.y;
    m_position.z = m_position.z + Velocity.z;

    // Clear the movement input accumulator for use during the next frame.
    m_panCommand = DirectX::XMFLOAT3( 0.0f, 0.0f, 0.0f );

}
```

Puesto que no queremos que la vibración del toque o del mouse entrecorten la vista panorámica de la cámara, establecemos una zona muerta alrededor del puntero con un diámetro de 32 píxeles. Tenemos también un valor de velocidad, que en este caso es 1:1 con el cruce seguro de píxel del puntero pasada la zona muerta. Puedes ajustar este comportamiento para reducir o aumentar la velocidad del movimiento.

## <a name="updating-the-view-matrix-with-the-new-camera-position"></a>Actualización de la matriz de vista con la nueva posición de cámara


Ahora podemos obtener una coordenada del espacio de la escena en la que se centra nuestra cámara, y que se actualiza cada vez que se lo ordenamos a la aplicación (cada 60 segundos en el bucle principal de la aplicación, por ejemplo). Este pseudocódigo sugiere el comportamiento de llamada que puedes implementar:

```cpp
 myCameraPanController->Update( m_window ); 

 // Update the view matrix based on the camera position.
 myCamera->MyMethodToComputeViewMatrix(
        myController->get_Position(),        // The position in the 3D scene space.
        myController->get_FixedLookPoint(),      // The point in the space we are looking at.
        DirectX::XMFLOAT3( 0, 1, 0 )                    // The axis that is "up" in our space.
        );  
```

¡Enhorabuena! Has implementado un sencillo conjunto de controles táctiles de movimiento panorámico de cámara en tu juego.


 

 

 




