---
title: Dispositivo de juego sin procesar
description: Use las API de dispositivo de juego Windows. Gaming. Input sin formato para leer la entrada de prácticamente cualquier tipo de controlador de juego.
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.date: 03/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, entrada, dispositivo de juego sin formato
ms.localizationpriority: medium
ms.openlocfilehash: d21b411965da874cfb324fc2ee867e39bdcd0ded
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171989"
---
# <a name="raw-game-controller"></a>Dispositivo de juego sin procesar

En esta página se describen los aspectos básicos de la programación de prácticamente cualquier tipo de controlador de juego mediante [Windows. Gaming. Input. RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) y las API relacionadas para el plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:

* cómo recopilar una lista de dispositivos de juego sin procesar conectados y sus usuarios
* Cómo detectar que se ha agregado o quitado un dispositivo de juego sin formato
* Cómo obtener las capacidades de un dispositivo de juego sin formato
* Cómo leer la entrada de un dispositivo de juego sin formato

## <a name="overview"></a>Información general

Un dispositivo de juego sin formato es una representación genérica de un dispositivo de juego, con entradas que se encuentran en muchos tipos diferentes de dispositivos de juego comunes. Estas entradas se exponen como matrices simples de botones, modificadores y ejes sin nombre. Con un dispositivo de juego sin procesar, puede permitir que los clientes creen asignaciones de entrada personalizadas independientemente del tipo de controlador que estén usando.

La clase [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) está realmente pensada para escenarios en los que las otras clases de entrada ([ArcadeStick](/uwp/api/windows.gaming.input.arcadestick), [FlightStick](/uwp/api/windows.gaming.input.flightstick), etc.) no satisfacen sus necesidades &mdash; si desea algo más genérico, anticipando que los clientes usarán muchos tipos diferentes de dispositivos de juego. esta clase es para usted.

## <a name="detect-and-track-raw-game-controllers"></a>Detección y seguimiento de dispositivos de juego sin formato

La detección y el seguimiento de los dispositivos de juego sin formato funcionan exactamente del mismo modo que para los controladores de juegos, excepto con la clase [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) en lugar de la clase de [controlador de juegos](/uwp/api/Windows.Gaming.Input.Gamepad) . Consulte [controlador de juegos y vibración](gamepad-and-vibration.md) para obtener más información.

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

The following example copies all connected raw game controllers into a new collection:

```cpp
auto myRawGameControllers = ref new Vector<RawGameController^>();

for (auto rawGameController : RawGameController::RawGameControllers)
{
    // This code assumes that you're interested in all raw game controllers.
    myRawGameControllers->Append(rawGameController);
}
```

### Adding and removing raw game controllers

When a raw game controller is added or removed, the [RawGameControllerAdded](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

The following example starts tracking a raw game controller that's been added:

```cpp
RawGameController::RawGameControllerAdded += 
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    // This code assumes that you're interested in all new raw game controllers.
    myRawGameControllers->Append(args);
});
```

The following example stops tracking a raw game controller that's been removed:

```cpp
RawGameController::RawGameControllerRemoved +=
    ref new EventHandler<RawGameController^>(
        [] (Platform::Object^, RawGameController^ args)
{
    unsigned int indexRemoved;

    if (myRawGameControllers->IndexOf(args, &indexRemoved))
    {
        myRawGameControllers->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each raw game controller can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>Obtener las capacidades de un dispositivo de juego sin formato

Después de identificar el dispositivo de juego sin procesar que le interesa, puede recopilar información sobre las capacidades del controlador. Puede obtener el número de botones en el dispositivo de juego sin formato con [RawGameController. ButtonCount](/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount), el número de ejes analógicos con [RawGameController. AxisCount](/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount)y el número de conmutadores con [RawGameController. SwitchCount](/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount). Además, puede obtener el tipo de un modificador mediante [RawGameController. GetSwitchKind](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_).

En el ejemplo siguiente se obtienen los recuentos de entrada de un dispositivo de juego sin formato:

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

En el ejemplo siguiente se determina el tipo de cada modificador:

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>Leer el dispositivo de juego sin formato

Una vez que conozca el número de entradas en un dispositivo de juego sin procesar, estará listo para recopilar datos de la misma. Sin embargo, a diferencia de otros tipos de entrada a los que se podría usar, un dispositivo de juego sin procesar no comunica el cambio de estado mediante la generación de eventos. En su lugar, puede realizar lecturas regulares de su estado actual mediante un _sondeo_ .

### <a name="polling-the-raw-game-controller"></a>Sondear el dispositivo de juego sin procesar

El sondeo captura una instantánea del dispositivo de juego sin procesar en un momento concreto. Este enfoque para la recopilación de entradas es una buena opción para la mayoría de los juegos porque su lógica normalmente se ejecuta en un bucle determinista en lugar de ser controlado por eventos. Normalmente, es más fácil interpretar los comandos del juego a partir de la entrada recopilada todos a la vez que desde muchas entradas únicas recopiladas a lo largo del tiempo.

Sondea un dispositivo de juego sin procesar llamando a [RawGameController. GetCurrentReading](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___). Esta función rellena matrices para botones, modificadores y ejes que contienen el estado del dispositivo de juego sin formato.

En el siguiente ejemplo se sondea un dispositivo de juego sin formato para su estado actual:

```cpp
Platform::Array<bool>^ currentButtonReading =
    ref new Platform::Array<bool>(buttonCount);

Platform::Array<GameControllerSwitchPosition>^ currentSwitchReading =
    ref new Platform::Array<GameControllerSwitchPosition>(switchCount);

Platform::Array<double>^ currentAxisReading = ref new Platform::Array<double>(axisCount);

rawGameController->GetCurrentReading(
    currentButtonReading,
    currentSwitchReading,
    currentAxisReading);
```

No hay ninguna garantía de qué posición en cada matriz contendrá el valor de entrada entre distintos tipos de controladores, por lo que deberá comprobar qué entrada es la que usa los métodos [RawGameController. GetButtonLabel](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) y [RawGameController. GetSwitchKind](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_).

**GetButtonLabel** le indicará el texto o el símbolo que se imprime en el botón físico, en lugar de la función del botón &mdash; por lo tanto, es mejor usar como ayuda para la interfaz de usuario en los casos en los que desea proporcionar sugerencias al reproductor sobre qué botones realizan las funciones. **GetSwitchKind** le indicará el tipo de conmutador (es decir, cuántas posiciones tiene), pero no el nombre.

No hay ninguna forma estandarizada de obtener la etiqueta de un eje o conmutador, por lo que deberá probarla para determinar qué entrada es la que.

Si tiene un controlador específico que desea admitir, puede obtener [RawGameController. HardwareProductId](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId) y [RawGameController. HardwareVendorId](/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId) y comprobar si coinciden con el controlador. La posición de cada entrada en cada matriz es la misma para cada controlador con el mismo **HardwareProductId** y **HardwareVendorId**, por lo que no tiene que preocuparse de que la lógica no sea coherente entre los distintos controladores del mismo tipo.

Además del estado de la controladora de juegos sin procesar, cada lectura devuelve una marca de tiempo que indica exactamente cuándo se recuperó el estado. La marca de tiempo es útil para establecer una relación con los intervalos de lecturas anteriores o la duración de la simulación de juego.

### <a name="reading-the-buttons-and-switches"></a>Lectura de los botones y modificadores

Cada uno de los botones del dispositivo de juego sin formato proporciona una lectura digital que indica si está presionado (inactivo) o liberado (arriba). Las lecturas de los botones se representan como valores booleanos individuales en una sola matriz. La etiqueta de cada botón puede encontrarse mediante [RawGameController. GetButtonLabel](/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) con el índice del valor booleano en la matriz. Cada valor se representa como [GameControllerButtonLabel](/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel).

En el ejemplo siguiente se determina si se presiona el botón **xboxa** :

```cpp
for (uint32_t i = 0; i < buttonCount; i++)
{
    if (currentButtonReading[i])
    {
        GameControllerButtonLabel buttonLabel = rawGameController->GetButtonLabel(i);

        if (buttonLabel == GameControllerButtonLabel::XboxA)
        {
            // XboxA is pressed.
        }
    }
}
```

En ocasiones, es posible que desee determinar si un botón cambia de presionado a liberado o liberado a presionado, si se presionan o se liberan varios botones, o si un conjunto de botones se organizan de una manera determinada &mdash; , pero no. Para obtener información sobre cómo detectar cada una de estas condiciones, consulta [Detecting button transitions (Detección de transiciones de botón)](input-practices-for-games.md#detecting-button-transitions) y [Detecting complex button arrangements (Detección de disposiciones de botones complejas)](input-practices-for-games.md#detecting-complex-button-arrangements).

Los valores de modificador se proporcionan como una matriz de [GameControllerSwitchPosition](/uwp/api/windows.gaming.input.gamecontrollerswitchposition). Dado que esta propiedad es un campo de bits, el enmascaramiento bit a bit se utiliza para aislar la dirección del modificador.

En el ejemplo siguiente se determina si cada modificador está en la posición de arriba:

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    if (GameControllerSwitchPosition::Up ==
        (currentSwitchReading[i] & GameControllerSwitchPosition::Up))
    {
        // The switch is in the up position.
    }
}
```

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>Lectura de las entradas analógicas (palos, desencadenadores, aceleradores, etc.)

Un eje analógico proporciona una lectura entre 0,0 y 1,0. Esto incluye cada dimensión en un Stick como X e y para los palos estándar, o incluso los ejes X, Y y Z (rollo, tono y guiñada, respectivamente) para los sticks de vuelos.

Los valores pueden representar desencadenadores analógicos, limitaciones o cualquier otro tipo de entrada analógica. Estos valores no se proporcionan con etiquetas, por lo que se recomienda probar el código con una variedad de dispositivos de entrada para asegurarse de que coinciden correctamente con sus suposiciones.

En todos los ejes, el valor es aproximadamente 0,5 para un palo cuando está en la posición central, pero es normal que varíe el valor preciso, incluso entre lecturas posteriores. más adelante en esta sección se describen las estrategias para mitigar esta variación.

En el ejemplo siguiente se muestra cómo leer los valores analógicos de un controlador Xbox:

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

Al leer los valores de los palos, observará que no producen una lectura neutra de 0,5 cuando están en reposo en la posición central; en su lugar, generarán valores distintos cerca de 0,5 cada vez que se mueva el stick y se devuelva a la posición central. Para mitigar estas variaciones, puedes implementar una pequeña _zona muerta_, que es un intervalo de valores cerca de la posición central ideal que se omiten.

Una manera de implementar un Deadzone es determinar hasta qué punto se ha despasado el palo del centro y omitir las lecturas más cercanas a la distancia que elija. Puede calcular la distancia aproximadamente, &mdash; ya que las lecturas de palo son esencialmente valores polares, no planos, &mdash; solo con el teorema de pitagórica. Esto genera un zona muerta radial.

En el ejemplo siguiente se muestra un Deadzone radial básico con pitagórica teorema:

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = leftStickY * leftStickY;
float adjacentSquared = leftStickX * leftStickX;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

<!--## Run the RawGameControllerUWP sample

The [RawGameControllerUWP sample (GitHub)](TODO: Link) demonstrates how to use raw game controllers. TODO: More information-->

## <a name="see-also"></a>Vea también

* [Entrada para juegos](input-for-games.md)
* [Procedimientos de entrada para juegos](input-practices-for-games.md)
* [Espacio de nombres Windows. Gaming. Input](/uwp/api/windows.gaming.input)
* [Clase Windows. Gaming. Input. RawGameController](/uwp/api/windows.gaming.input.rawgamecontroller)
* [Interfaz Windows. Gaming. Input. IGameController](/uwp/api/windows.gaming.input.igamecontroller)
* [Interfaz Windows. Gaming. Input. IGameControllerBatteryInfo](/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)