---
author: scottmill
title: Movimiento de mouse relativo
description: Utiliza controles de mouse relativos, que no usan el cursor del sistema y no devuelven coordenadas de pantalla absolutas para el seguimiento del delta de píxeles entre los movimientos del mouse en los juegos.
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, games, juegos, mouse, input, entrada
ms.assetid: 08c35e05-2822-4a01-85b8-44edb9b6898f
ms.localizationpriority: medium
ms.openlocfilehash: adf3b629095f633521b99133ce1961e5c8408ef5
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7171824"
---
# <a name="relative-mouse-movement-and-corewindow"></a>Movimiento de mouse relativo y CoreWindow

En los juegos, el mouse es una opción de control común que es conocida por muchos de los jugadores, y es también esencial para muchos géneros de juegos, como los juegos de disparos en primera y tercera persona, y los juegos de estrategia en tiempo real. Aquí analizamos la implementación de controles de mouse relativos, que no usan el cursor del sistema y no devuelven coordenadas de pantalla absolutas. En su lugar, siguen el delta de píxeles entre los diferentes movimientos del mouse.

Algunas aplicaciones, como los juegos, usan el mouse como un dispositivo de entrada más general. Por ejemplo, un modelador 3D puede usar la entrada de mouse para orientar un objeto 3D simulando una bola de seguimiento virtual, o un juego puede usar el mouse para cambiar la dirección de la cámara de visualización mediante controles con apariencia de mouse. 

En estos escenarios, la aplicación requiere de datos de mouse relativos. Los valores de mouse relativos representan la distancia que recorrió el mouse desde el último fotograma, en lugar de los valores de coordenadas x e y absolutos dentro de una ventana o pantalla. Además, las aplicaciones a menudo ocultan el cursor del mouse dado que la posición del cursor con respecto a las coordenadas de pantalla no es relevante al manipular una escena o un objeto 3D. 

Cuando el usuario realiza una acción que mueve la aplicación dentro de un modo de manipulación de escena u objeto 3D relativo, la aplicación debe realizar lo siguiente: 
- Ignorar el control del mouse predeterminado
- Habilitar el control del mouse relativo
- Ocultar el cursor del mouse configurando un puntero nulo (nullptr) 

Cuando el usuario realiza una acción que mueve la aplicación fuera de un modo de manipulación de escena u objeto 3D relativo, la aplicación debe realizar lo siguiente: 
- Habilitar el control del mouse absoluto/predeterminado
- Desactivar el control del mouse relativo 
- Establecer el cursor del mouse en un valor no nulo (que lo hace visible)

> **Nota**  
Con este patrón, la ubicación del cursor del mouse absoluto se conserva al entrar en el modo relativo sin cursor. El cursor reaparece en la misma ubicación de coordenada de pantalla en la que estaba antes de habilitar el modo de movimiento de mouse relativo.

 

## <a name="handling-relative-mouse-movement"></a>Controlar el movimiento de mouse relativo


Para acceder a los valores delta de mouse relativo, regístrate para el evento [MouseDevice::MouseMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mousedevice.mousemoved.aspx) tal como se muestra aquí.


```cpp


// register handler for relative mouse movement events
Windows::Devices::Input::MouseDevice::GetForCurrentView()->MouseMoved +=
        ref new TypedEventHandler<MouseDevice^, MouseEventArgs^>(this, &MoveLookController::OnMouseMoved);


```

```cpp


void MoveLookController::OnMouseMoved(
    _In_ Windows::Devices::Input::MouseDevice^ mouseDevice,
    _In_ Windows::Devices::Input::MouseEventArgs^ args
    )
{
    float2 pointerDelta;
    pointerDelta.x = static_cast<float>(args->MouseDelta.X);
    pointerDelta.y = static_cast<float>(args->MouseDelta.Y);

    float2 rotationDelta;
    rotationDelta = pointerDelta * ROTATION_GAIN;   // scale for control sensitivity

    // update our orientation based on the command
    m_pitch -= rotationDelta.y;                     // mouse y increases down, but pitch increases up
    m_yaw   -= rotationDelta.x;                     // yaw defined as CCW around y-axis

    // limit pitch to straight up or straight down
    float limit = (float)(M_PI/2) - 0.01f;
    m_pitch = (float) __max( -limit, m_pitch );
    m_pitch = (float) __min( +limit, m_pitch );

    // keep longitude in useful range by wrapping
    if ( m_yaw >  M_PI )
        m_yaw -= (float)M_PI*2;
    else if ( m_yaw < -M_PI )
        m_yaw += (float)M_PI*2;
}

```

El controlador de eventos de este ejemplo de código, **OnMouseMoved**, representa la vista sobre la base de los movimientos del mouse. La posición del puntero del mouse se pasa al controlador como un objeto [MouseEventArgs](https://msdn.microsoft.com/library/windows/apps/xaml/windows.devices.input.mouseeventargs.aspx). 

Omite procesar en exceso los datos de mouse absoluto del evento [CoreWindow::PointerMoved](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointermoved.aspx) cuando la aplicación cambie al control de valores de movimiento de mouse relativo. No obstante, solamente omite esta entrada si el evento **CoreWindow::PointerMoved** se produjo como resultado de la entrada de mouse (en contraposición con la entrada táctil). El cursor se oculta estableciendo el valor de [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx) en **nullptr**. 

## <a name="returning-to-absolute-mouse-movement"></a>Volver al movimiento de mouse absoluto

Cuando la aplicación salga del modo de manipulación de escenas u objetos 3D, y ya no use el movimiento de mouse relativo (por ejemplo, cuando devuelve una pantalla de menú) vuelve al procesamiento normal del movimiento de mouse absoluto. En este punto, deja de leer los datos de mouse relativo, reinicia el procesamiento de eventos de mouse (y puntero) estándar y establece [CoreWindow::PointerCursor](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.core.corewindow.pointercursor.aspx) en un valor no nulo. 

> **Nota**  
Cuando tu aplicación se encuentra en el modo de manipulación de escenas u objetos 3D (procesamiento de movimientos de mouse relativo con el cursor desactivado), el mouse no puede invocar la interfaz de usuario perimetral como los accesos, la pila de retroceso o la barra de la aplicación. Por lo tanto, es importante proporcionar un mecanismo para salir de este modo específico, como la tecla **Esc** que se usa habitualmente.

## <a name="related-topics"></a>Temas relacionados

* [Controles de movimiento y vista para juegos](tutorial--adding-move-look-controls-to-your-directx-game.md) 
* [Controles táctiles para juegos](tutorial--adding-touch-controls-to-your-directx-game.md)