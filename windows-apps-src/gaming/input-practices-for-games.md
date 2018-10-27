---
author: eliotcowley
title: Prácticas de entrada para juegos
description: Obtén información sobre patrones y técnicas para el uso eficaz de los dispositivos de entrada.
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.author: elcowle
ms.date: 11/20/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, entrada, games, input
ms.localizationpriority: medium
ms.openlocfilehash: ed0d611c761315e42decb89e1a5a5ad84f4b067a
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5696009"
---
# <a name="input-practices-for-games"></a>Procedimientos de entrada para juegos

En esta página se describen patrones y técnicas para usar con eficacia dispositivos de entrada de juegos para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:

* Cómo hacer un seguimiento de los jugadores y de los dispositivos de entrada y navegación que están usando actualmente
* Cómo detectar las transiciones de botón (presionado a no presionado, sin presionar a presionado)
* Cómo detectar disposiciones de botones complejas con una sola prueba

## <a name="choosing-an-input-device-class"></a>Elegir una clase de dispositivo de entrada

Tienes a tu disposición muchos tipos diferentes de API, por ejemplo, [ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) y [Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad). ¿Cómo decidir qué API usar para tu juego?

Deberías elegir la API que te ofrezca la entrada más adecuada para tu juego. Por ejemplo, si estás creando un juego para una plataforma 2D, probablemente puedes usar la clase **Gamepad** y no molestarte con la funcionalidad adicional disponible a través de otras clases. De este modo, el juego se restringiría a admitir solo mandos y proporcionar una interfaz coherente que funcione con muchos mandos diferentes, sin necesidad de código adicional.

Por otro lado, para simulaciones complejas de vuelo y carreras, es recomendable enumerar todos los objetos [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) como base de referencia para asegurarte de que admitan cualquier dispositivo nicho que puedan usar los jugadores entusiastas, por ejemplo, dispositivos, como pedales o aceleradores independientes que el modo de un solo jugador aún usa. 

A partir de ahí, puedes usar el método **FromGameController** de una clase entrada, como [Gamepad.FromGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.fromgamecontroller), para ver si cada dispositivo tiene una vista más protegida. Por ejemplo, si el dispositivo también es un objeto **Gamepad**, es recomendable ajustar la interfaz de usuario de asignación de botones para reflejar ello y proporcionan algunas asignaciones de botones predeterminadas razonables entre las que elegir. (Esto es lo opuesto a requerir que el jugador configure manualmente las entradas de mando si solo usas **RawGameController**). 

Como alternativa, puedes consultar el identificador de proveedor (VID) y el identificador de producto (PID) de un objeto **RawGameController** (mediante [HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) y [HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId), respectivamente) y proporcionar asignaciones de botones sugeridas para dispositivos populares y, al mismo tiempo, mantener la compatibilidad con dispositivos desconocidos que puedan lanzarse en el futuro a través de asignaciones manuales del jugador.

## <a name="keeping-track-of-connected-controllers"></a>Realizar un seguimiento de los controladores conectados

Aunque cada tipo de controlador incluya una lista de controladores conectados (como [Gamepad.Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.Gamepads)), es una buena idea mantener tu propia lista de controladores. Consulta [Lista de controladores para juegos](gamepad-and-vibration.md#the-gamepads-list) para obtener más información (cada tipo de controlador tiene una sección con un nombre similar en su propio tema).

Sin embargo, ¿qué sucede cuando el jugador desconecta su controlador o conecta uno nuevo? Deberás controlar estos eventos y actualizar la lista según corresponda. Consulta [Agregar y quitar controladores para juegos](gamepad-and-vibration.md#adding-and-removing-gamepads) para obtener más información (cada tipo de controlador tiene una sección con un nombre similar en su propio tema).

Ya que se generan eventos agregados y eliminados asincrónicamente, puedes obtener resultados incorrectos cuando trabajas con tu lista de controladores. Por lo tanto, cada vez que obtienes acceso a la lista de controladores, debes colocar un bloqueo a su alrededor para que solo pueda acceder un subproceso a la vez. Esto puede hacerse con [Runtime de simultaneidad](https://docs.microsoft.com/cpp/parallel/concrt/concurrency-runtime), específicamente la [clase critical_section](https://docs.microsoft.com/cpp/parallel/concrt/reference/critical-section-class), en **&lt;ppl.h&gt;**.

Otra cosa en la que pensar es que la lista de controladores conectados inicialmente estará vacía y que tarda uno o dos segundos en rellenarse. Por lo tanto, si solo se asigna el controlador para juegos actual en el método de inicio, será **null**.

Para corregir esto, debes tener un método que "actualice" el controlador para juegos principal (en un juego de un solo jugador; los juegos multijugador requerirán soluciones más sofisticadas). A continuación, debes llamar a este método en los controladores de evento eliminado y agregado del controlador, o en tu método de actualización.

El siguiente método solo devuelve el primer controlador para juegos en la lista (o **nullptr** si la lista está vacía). Entonces, solo tienes que comprobar **nullptr** en cualquier momento que hagas algo con el controlador. Tú decides si quieres bloquear el juego cuando no hay ningún controlador conectado (por ejemplo, mediante la pausa del juego) o simplemente hacer que continúe y pasar por alto la entrada.

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

En resumen, presentamos un ejemplo de cómo controlar la entrada desde un controlador para juegos:

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

Todos los dispositivos de entrada se asocian con un [usuario](https://docs.microsoft.com/uwp/api/windows.system.user) para que su identidad pueda vincularse a su juego, logros, cambios de configuración y otras actividades. Los usuarios pueden iniciar o cerrar sesión cuando quieran, y es habitual que un usuario diferente inicie sesión en un dispositivo de entrada que permanece conectado al sistema después de que el usuario anterior haya cerrado sesión. Cuando un usuario inicia o cierra sesión, se genera el evento [IGameController.UserChanged](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.UserChanged). Puedes registrar un controlador de eventos para este evento a fin de realizar un seguimiento de los jugadores y de los dispositivos que usan.

La identidad del usuario también es la manera en que un dispositivo de entrada se asocia a su correspondiente [controlador de navegación de la interfaz de usuario](ui-navigation-controller.md).

Por estos motivos, debe realizarse un seguimiento de la entrada del jugador y correlacionarse con la propiedad [User](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller.User) de la clase de dispositivo (heredada de la interfaz [IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)).

En el ejemplo [UserGamepadPairingUWP (GitHub)](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) se indica cómo puedes hacer un seguimiento de los usuarios y los dispositivos que usan.

## <a name="detecting-button-transitions"></a>Detección de transiciones de botón

A veces quieres saber cuándo se presiona o se suelta un botón; es decir, cuándo cambia exactamente el estado del botón de no presionado a presionado o de presionado a no presionado. Para determinarlo, debes recordar la lectura de dispositivo anterior y compararla con la lectura actual para ver qué ha cambiado.

En el siguiente ejemplo se muestra un enfoque básico para recordar la lectura anterior; en él se muestran mandos para juegos, pero los principios son los mismos para sticks arcade, volantes los tipos de dispositivo de entrada.

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

En el siguiente ejemplo se muestra un enfoque básico para detectar transiciones de botón:

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

Estas dos funciones primero derivan el estado booleano de la selección de botón de `newReading` y `oldReading` y, después, realizan una lógica booleana para determinar si se produjo la transición de destino. Estas funciones devuelven **true** solo si la nueva lectura contiene el estado de destino (presionado o liberado, respectivamente) *y* la lectura anterior tampoco contiene el estado de destino; de lo contrario, devuelven **false**.

## <a name="detecting-complex-button-arrangements"></a>Detección de disposiciones de botones complejas

Cada botón de un dispositivo de entrada proporciona una lectura digital que indica si está presionado (abajo) o liberado (arriba). Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales; en su lugar, se empaquetan todas en campos de bits que se representan mediante enumeraciones específicas de dispositivo como [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons). Para leer botones específicos, se usa el enmascaramiento bit a bit para aislar los valores que te interesan. Un botón está presionado (abajo) cuando se establece su bit correspondiente; de lo contrario, está liberado (arriba).

Recuerda cómo se determina si un solo botón está presionado o no; aquí se muestran controladores para juegos, pero los principios son los mismos para el stick arcade, el volante y los otros tipos de dispositivo de entrada.

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

Como puedes ver, la determinación del estado de un solo botón es directa, pero a veces, es posible que quieras determinar si varios botones están presionados o liberados, o si un conjunto de botones está dispuesto de un modo determinado, algunos presionados y otros no. Probar varios botones es más complejo que probar un solo botón, especialmente por la posibilidad de estado de botones combinados, pero hay una fórmula simple para estas pruebas que se aplica a un solo botón y a varios por igual.

En el ejemplo siguiente, se determina si los botones A y B del controlador para juegos están ambos presionados:

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both pressed.
}
```

En el ejemplo siguiente, se determina si los botones A y B del controlador para juegos están ambos liberados:

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // The A and B buttons are both released (not pressed).
}
```

En el ejemplo siguiente, se determina si el botón A del controlador para juegos está presionado mientras que el botón B está liberado:

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

La fórmula que los cinco ejemplos tienen en común es que la disposición de los botones que se van a probar se especifica mediante la expresión de la izquierda del operador de igualdad mientras que los botones que se deben tomar en consideración se seleccionan mediante la expresión de enmascaramiento de la derecha.

En el siguiente ejemplo se muestra esta fórmula más claramente, volviendo a escribir el ejemplo anterior:

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // The A button is pressed and the B button is released (B is not pressed).
}
```

Esta fórmula puede aplicarse para probar cualquier número de botones en cualquier disposición de sus estados.

## <a name="get-the-state-of-the-battery"></a>Obtener el estado de la batería

Para cualquier dispositivo de juego que implemente la interfaz [IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo), puedes llamar a [TryGetBatteryReport](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo.TryGetBatteryReport) en la instancia del controlador para obtener un objeto [BatteryReport](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport) que ofrezca información sobre la batería en el controlador. Puedes obtener propiedades como el ritmo de carga de la batería ([ChargeRateInMilliwatts](https://docs.microsoft.com/uwp/api/windows.devices.power.batteryreport.ChargeRateInMilliwatts)), la capacidad de energía estimada de una batería nueva ([DesignCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.DesignCapacityInMilliwattHours)) y la capacidad de energía totalmente cargada de la batería actual ([FullChargeCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.FullChargeCapacityInMilliwattHours)).

Para los dispositivos de juego compatibles con el informe de batería detallado, puedes obtener esto y más información sobre la batería, como se detalla en [Obtener información sobre la batería](../devices-sensors/get-battery-info.md). Sin embargo, la mayoría de dispositivos de juegos no admite ese nivel de informes de batería y, en su lugar, usan hardware de bajo coste. Para estos controladores, deberás tener en cuenta las siguientes consideraciones:

* **ChargeRateInMilliwatts** y **DesignCapacityInMilliwattHours** siempre serán **NULL**.

* Puedes obtener el porcentaje de batería calculando [RemainingCapacityInMilliwattHours](https://docs.microsoft.com/en-us/uwp/api/windows.devices.power.batteryreport.RemainingCapacityInMilliwattHours) / **FullChargeCapacityInMilliwattHours**. Debes pasar por alto los valores de estas propiedades y solo tratar el porcentaje calculado.

* El porcentaje del punto anterior siempre será uno de los siguientes:

    * 100 % (completo)
    * 70 % (medio)
    * 40 % (bajo)
    * 10 % (crítico)

Si tu código realiza alguna acción (por ejemplo, dibujar la interfaz de usuario) en función del porcentaje de duración de batería restante, asegúrate de que cumpla con los valores anteriores. Por ejemplo, si deseas advertir al jugador cuando el controlador tenga poca batería, puedes hacerlo cuando llega al 10%.

## <a name="see-also"></a>Ver también

* [Windows.System.User class](https://docs.microsoft.com/uwp/api/windows.system.user)
* [Interfaz Windows.Gaming.Input.IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Enumeración Windows.Gaming.Input.GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons)
