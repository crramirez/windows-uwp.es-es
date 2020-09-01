---
title: Procedimientos de entrada para juegos
description: Aprenda patrones y técnicas para usar de forma eficaz los dispositivos de entrada en juegos Plataforma universal de Windows (UWP).
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, entrada
ms.localizationpriority: medium
ms.openlocfilehash: d8ea74f7053cfdd0aeb6ef3dbe032afdb089f420
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173119"
---
# <a name="input-practices-for-games"></a>Procedimientos de entrada para juegos

En este tema se describen patrones y técnicas para usar de forma eficaz los dispositivos de entrada en juegos Plataforma universal de Windows (UWP).

Al leer este tema, aprenderá lo siguiente:

* Cómo hacer un seguimiento de los jugadores y de los dispositivos de entrada y navegación que están usando actualmente
* Cómo detectar las transiciones de botón (presionado a no presionado, sin presionar a presionado)
* Cómo detectar disposiciones de botones complejas con una sola prueba

## <a name="choosing-an-input-device-class"></a>Elección de una clase de dispositivo de entrada

Hay muchos tipos diferentes de API de entrada disponibles, como [ArcadeStick](/uwp/api/windows.gaming.input.arcadestick), [FlightStick](/uwp/api/windows.gaming.input.flightstick)y [controlador de juegos](/uwp/api/windows.gaming.input.gamepad). ¿Cómo se decide qué API usar para el juego?

Debe elegir cualquier API que le proporcione la entrada más adecuada para el juego. Por ejemplo, si va a crear un juego de plataforma 2D, probablemente solo puede usar la clase de **controlador para juegos** y no molestarse en la funcionalidad adicional disponible a través de otras clases. Esto restringe el juego para admitir solo los controladores de juegos y proporcionar una interfaz coherente que funcionará en muchos controladores de interfaz de juegos diferentes sin necesidad de código adicional.

Por otro lado, en el caso de simulaciones complejas de vuelos y carreras, es posible que desee enumerar todos los objetos de [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) como línea base para asegurarse de que admiten cualquier dispositivo de nicho que puedan tener los jugadores, incluidos los dispositivos como pedales o aceleradores independientes que siguen siendo utilizados por un solo jugador. 

Desde allí, puede usar el método **FromGameController** de una clase de entrada, como [controlador de juegos. FromGameController](/uwp/api/windows.gaming.input.gamepad.fromgamecontroller), para ver si cada dispositivo tiene una vista más seleccionada. Por ejemplo, si el dispositivo es también un **controlador de juegos**, puede que desee ajustar la interfaz de usuario de asignación de botones para reflejarlo y proporcionar algunas asignaciones de botones predeterminados razonables para elegir. (Esto contrasta con la exigencia de que el reproductor configure manualmente las entradas del controlador de juegos si solo usa **RawGameController**). 

Como alternativa, puede consultar el ID. de proveedor (VID) y el ID. de producto (PID) de un **RawGameController** (con [HardwareVendorId](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) y [HardwareProductId](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId), respectivamente) y proporcionar las asignaciones de botones sugeridos para los dispositivos populares, a la vez que sigue siendo compatible con dispositivos desconocidos que salen en el futuro a través de asignaciones manuales por parte del reproductor.

## <a name="keeping-track-of-connected-controllers"></a>Seguimiento de los controladores conectados

Aunque cada tipo de controlador incluye una lista de controladores conectados (por ejemplo, [controlador de juegos.](/uwp/api/windows.gaming.input.gamepad.Gamepads)controladores de juegos), es una buena idea mantener su propia lista de controladores. Consulte [la lista](gamepad-and-vibration.md#the-gamepads-list) de controladores de juegos para obtener más información (cada tipo de controlador tiene una sección con el mismo nombre en su propio tema).

Sin embargo, ¿qué ocurre cuando el reproductor desconecta el controlador o se conecta a uno nuevo? Debe controlar estos eventos y actualizar la lista en consecuencia. Consulte [Agregar y quitar controladores de juegos](gamepad-and-vibration.md#adding-and-removing-gamepads) para obtener más información (de nuevo, cada tipo de controlador tiene una sección con el mismo nombre en su propio tema).

Dado que los eventos agregados y quitados se generan de forma asincrónica, puede obtener resultados incorrectos cuando se trabaja con la lista de controladores. Por lo tanto, siempre que tenga acceso a la lista de controladores, debe colocar un bloqueo en torno a él para que solo un subproceso pueda acceder a él a la vez. Esto puede hacerse con el [Runtime de simultaneidad](/cpp/parallel/concrt/concurrency-runtime), en concreto la [clase critical_section](/cpp/parallel/concrt/reference/critical-section-class), en ** &lt; PPL. h &gt; **.

Otro aspecto que hay que tener en cuenta es que la lista de controladores conectados estará inicialmente vacía y tarda dos o dos segundos en rellenarse. Por lo tanto, si solo asigna el controlador de juegos actual en el método de inicio, será **null**.

Para rectificar esto, debe tener un método que "actualice" el controlador de juegos principal (en un juego de un solo jugador; los juegos multijugador requerirán soluciones más sofisticadas). A continuación, debe llamar a este método en los controladores de eventos del controlador agregado y del controlador quitado, o en el método de actualización.

El siguiente método simplemente devuelve el primer controlador de juegos de la lista (o **nullptr** si la lista está vacía). Después, solo tiene que recordar comprobar **nullptr** en cualquier momento con el controlador. Depende de usted si desea bloquear el juego cuando no hay ningún controlador conectado (por ejemplo, al pausar el juego) o simplemente hacer que el juego continúe, y omitir la entrada.

```cpp
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Gaming::Input;
using namespace concurrency;

Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();

Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}
```

Juntos, aquí se muestra un ejemplo de cómo controlar la entrada desde un controlador de juegos:

```cpp
#include <algorithm>
#include <ppl.h>

using namespace Platform::Collections;
using namespace Windows::Foundation;
using namespace Windows::Gaming::Input;
using namespace concurrency;

static Vector<Gamepad^>^ m_myGamepads = ref new Vector<Gamepad^>();
static Gamepad^          m_gamepad = nullptr;
static critical_section  m_lock{};

void Start()
{
    // Register for gamepad added and removed events.
    Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(&OnGamepadAdded);
    Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(&OnGamepadRemoved);

    // Add connected gamepads to m_myGamepads.
    for (auto gamepad : Gamepad::Gamepads)
    {
        OnGamepadAdded(nullptr, gamepad);
    }
}

void Update()
{
    // Update the current gamepad if necessary.
    if (m_gamepad == nullptr)
    {
        auto gamepad = GetFirstGamepad();

        if (m_gamepad != gamepad)
        {
            m_gamepad = gamepad;
        }
    }

    if (m_gamepad != nullptr)
    {
        // Gather gamepad reading.
    }
}

// Get the first gamepad in the list.
Gamepad^ GetFirstGamepad()
{
    Gamepad^ gamepad = nullptr;
    critical_section::scoped_lock{ m_lock };

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(0);
    }

    return gamepad;
}

void OnGamepadAdded(Platform::Object^ sender, Gamepad^ args)
{
    // Check if the just-added gamepad is already in m_myGamepads; if it isn't, 
    // add it.
    critical_section::scoped_lock lock{ m_lock };
    auto it = std::find(begin(m_myGamepads), end(m_myGamepads), args);

    if (it == end(m_myGamepads))
    {
        m_myGamepads->Append(args);
    }
}

void OnGamepadRemoved(Platform::Object^ sender, Gamepad^ args)
{
    // Remove the gamepad that was just disconnected from m_myGamepads.
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ m_lock };

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == m_myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        m_myGamepads->RemoveAt(indexRemoved);
    }
}
```

## <a name="tracking-users-and-their-devices"></a>Seguimiento de usuarios y sus dispositivos

Todos los dispositivos de entrada se asocian con un [usuario](/uwp/api/windows.system.user) para que su identidad pueda vincularse a su juego, logros, cambios de configuración y otras actividades. Los usuarios pueden iniciar sesión o cerrar sesión en y es habitual que un usuario diferente inicie sesión en un dispositivo de entrada que permanezca conectado al sistema después de que el usuario anterior haya cerrado la sesión. Cuando un usuario inicia o cierra sesión, se genera el evento [IGameController. UserChanged](/uwp/api/windows.gaming.input.igamecontroller.UserChanged) . Puedes registrar un controlador de eventos para este evento a fin de realizar un seguimiento de los jugadores y de los dispositivos que usan.

La identidad del usuario también es la manera en que un dispositivo de entrada está asociado a su [controlador de navegación de IU](ui-navigation-controller.md)correspondiente.

Por estos motivos, se debe realizar un seguimiento de la entrada del reproductor y correlacionarla con la propiedad de [usuario](/uwp/api/windows.gaming.input.igamecontroller.User) de la clase Device (heredada de la interfaz [IGameController](/uwp/api/windows.gaming.input.igamecontroller) ).

En el ejemplo [UserGamepadPairingUWP](/samples/microsoft/xbox-atg-samples/usergamepadpairinguwp/) se muestra cómo se puede realizar un seguimiento de los usuarios y los dispositivos que están usando.

## <a name="detecting-button-transitions"></a>Detección de transiciones de botón

A veces quieres saber cuándo se presiona o se suelta un botón; es decir, cuándo cambia exactamente el estado del botón de no presionado a presionado o de presionado a no presionado. Para determinarlo, debes recordar la lectura de dispositivo anterior y compararla con la lectura actual para ver qué ha cambiado.

En el ejemplo siguiente se muestra un enfoque básico para recordar la lectura anterior; Aquí se muestran los controladores de juegos, pero los principios son los mismos para el Stick Arcade, la rueda de carreras y los otros tipos de dispositivos de entrada.

```cpp
Gamepad gamepad;
GamepadReading newReading();
GamepadReading oldReading();

// Called at the start of the game.
void Game::Start()
{
    gamepad = Gamepad::Gamepads[0];
}

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.GetCurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased (see below)
}
```

Antes de realizar alguna cosa, `Game::Loop` mueve el valor existente de `newReading` (la lectura del controlador para juegos de la iteración de bucle anterior) a `oldReading` i, después, rellena `newReading` con una nueva lectura de controlador para juegos para la iteración actual. Esto te ofrece la información que necesitas para detectar transiciones de botón.

En el ejemplo siguiente se muestra un enfoque básico para detectar transiciones de botón:

```cpp
bool ButtonJustPressed(const GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased =
        (GamepadButtons.None == (newReading.Buttons & selection));

    bool oldSelectionReleased =
        (GamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Estas dos funciones primero derivan el estado booleano de la selección del botón de `newReading` y `oldReading` , a continuación, realizan una lógica booleana para determinar si se ha producido la transición de destino. Estas funciones devuelven **true** solo si la nueva lectura contiene el estado de destino (presionado o liberado, respectivamente) *y* la lectura anterior tampoco contiene el estado de destino; de lo contrario, devuelven **false**.

## <a name="detecting-complex-button-arrangements"></a>Detección de disposiciones de botones complejas

Cada botón de un dispositivo de entrada proporciona una lectura digital que indica si está presionado (inactivo) o liberado (arriba). Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales; en su lugar, se empaquetan todas en campos de bits que se representan mediante enumeraciones específicas de dispositivo como [GamepadButtons](/uwp/api/windows.gaming.input.gamepadbuttons). Para leer botones específicos, se usa el enmascaramiento bit a bit para aislar los valores que te interesan. Se presiona un botón (abajo) cuando se establece su bit correspondiente; de lo contrario, se libera (up).

Recordar cómo se determinan los botones individuales para que se presionen o se liberen; Aquí se muestran los controladores de juegos, pero los principios son los mismos para el Stick Arcade, la rueda de carreras y los otros tipos de dispositivos de entrada.

```cpp
GamepadReading reading = gamepad.GetCurrentReading();

// Determines whether gamepad button A is pressed.
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // The A button is pressed.
}

// Determines whether gamepad button A is released.
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // The A button is released (not pressed).
}
```

Como puede ver, la determinación del estado de un solo botón es directa, pero a veces es posible que desee determinar si se presionan o se liberan varios botones, o si un conjunto de botones se organizan de una manera determinada &mdash; , algunos no. Probar varios botones es más complejo que probar botones individuales &mdash; , especialmente con el potencial del estado de los botones mixtos &mdash; , pero hay una fórmula simple para estas pruebas que se aplica a las pruebas de botón único y múltiple.

En el ejemplo siguiente se determina si se presionan los botones A y B del controlador de juegos:

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

En el ejemplo siguiente se determina si se liberan los botones A y B del controlador de juegos:

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

En el ejemplo siguiente se determina si se presiona el botón de controlador de juegos A mientras se suelta el botón B:

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

La fórmula que los cinco ejemplos tienen en común es que la disposición de los botones que se van a probar se especifica mediante la expresión de la izquierda del operador de igualdad mientras que los botones que se deben tomar en consideración se seleccionan mediante la expresión de enmascaramiento de la derecha.

En el ejemplo siguiente se muestra esta fórmula más claramente rescribiendo el ejemplo anterior:

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Esta fórmula puede aplicarse para probar cualquier número de botones en cualquier disposición de sus estados.

## <a name="get-the-state-of-the-battery"></a>Obtención del estado de la batería

Para cualquier dispositivo de juego que implementa la interfaz [IGameControllerBatteryInfo](/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo) , puede llamar a [TryGetBatteryReport](/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport) en la instancia del controlador para obtener un objeto [BatteryReport](/uwp/api/windows.devices.power.batteryreport) que proporcione información sobre la batería en el controlador. Puede obtener propiedades como la velocidad a la que se carga la batería ([ChargeRateInMilliwatts](/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts)), la capacidad de energía estimada de una nueva batería ([DesignCapacityInMilliwattHours](/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours)) y la capacidad energética totalmente cargada de la batería actual ([FullChargeCapacityInMilliwattHours](/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours)).

En el caso de los dispositivos de juego que admiten informes de batería detallados, puede obtener este y más información acerca de la batería, tal y como se detalla en [obtener información](../devices-sensors/get-battery-info.md)de la batería. Sin embargo, la mayoría de los controladores de juegos no admiten ese nivel de informes de batería y, en su lugar, usan Hardware de bajo costo. En el caso de estos controladores, tendrá que tener en cuenta las consideraciones siguientes:

* **ChargeRateInMilliwatts** y **DesignCapacityInMilliwattHours** siempre serán **null**.

* Puede obtener el porcentaje de la batería mediante el cálculo de [RemainingCapacityInMilliwattHours](/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours)  /  **FullChargeCapacityInMilliwattHours**. Debe omitir los valores de estas propiedades y solo tratar el porcentaje calculado.

* El porcentaje de la viñeta anterior siempre será uno de los siguientes:

    * 100% (completo)
    * 70% (medio)
    * 40% (bajo)
    * 10% (crítico)

Si el código realiza alguna acción (como dibujar la interfaz de usuario) en función del porcentaje de duración de la batería restante, asegúrese de que se ajusta a los valores anteriores. Por ejemplo, si desea advertir al jugador de que la batería del controlador es baja, hágalo cuando llegue al 10%.

## <a name="see-also"></a>Vea también

* [Windows.SysTEM. Clase de usuario](/uwp/api/windows.system.user)
* [Interfaz Windows. Gaming. Input. IGameController](/uwp/api/windows.gaming.input.igamecontroller)
* [Enumeración Windows. Gaming. Input. GamepadButtons](/uwp/api/windows.gaming.input.gamepadbuttons)
* [Ejemplo de UserGamepadPairingUWP](/samples/microsoft/xbox-atg-samples/usergamepadpairinguwp/)