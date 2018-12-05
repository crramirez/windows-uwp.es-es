---
title: Dispositivo de juego sin procesar
description: Usa las API de dispositivo de juego sin procesar Windows.Gaming.Input para leer la entrada desde casi cualquier tipo de dispositivo de juego.
ms.assetid: 2A466C16-1F51-4D8D-AD13-704B6D3C7BEC
ms.date: 03/08/2017
ms.topic: article
keywords: windows 10, uwp, games, juegos, input, entrada, raw game controller, dispositivo de juego sin procesar
ms.localizationpriority: medium
ms.openlocfilehash: 7b5f4d49ad49cf9f9065fe17788456e9dd2a4a4e
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8693714"
---
# <a name="raw-game-controller"></a>Dispositivo de juego sin procesar

En esta página se describen los conceptos básicos de programación para casi cualquier tipo de dispositivo de juego mediante [Windows.Gaming.Input.RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) y las API relacionadas para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre lo siguiente:

* Cómo obtener una lista de los dispositivos de juego sin procesar conectados y sus usuarios
* Cómo detectar que se ha agregado o quitado un dispositivo de juego sin procesar
* Cómo obtener las funcionalidades de un dispositivo de juego sin procesar
* Cómo leer la entrada desde un dispositivo de juego sin procesar

## <a name="overview"></a>Introducción

Un dispositivo de juego sin procesar es una representación genérica de un dispositivo de juego, con entradas que se encuentran en muchos tipos diferentes de dispositivos de juego comunes. Estas entradas se exponen como matrices simples de botones, modificadores y ejes sin nombre. Con un dispositivo de juego sin procesar, puedes permitir a los clientes crear asignaciones de entrada personalizadas independientemente de qué tipo de controlador estén usando.

La clase [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller), en realidad, está pensada para escenarios en los que las otras clases de entrada ([ArcadeStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) y así sucesivamente) no satisfacen tus necesidades&mdash;si quieres algo más genérico, porque sabes que los clientes usarán varios tipos diferentes de dispositivos de juego, entonces esta clase es para ti.

## <a name="detect-and-track-raw-game-controllers"></a>Detectar y hacer un seguimiento de los dispositivos de juego sin procesar

La detección y el seguimiento de los dispositivos de juego sin procesar funciona exactamente del mismo modo que para los controladores para juegos, solo que con la clase [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) en lugar de la clase [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad). Para obtener más información, consulta [Controlador para juegos y vibración](gamepad-and-vibration.md).

<!-- Raw game controllers are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected raw game controllers and events to notify you when a raw game controller is added or removed.

### The raw game controller list

The [RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller) class provides a static property, [RawGameControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllers), which is a read-only list of raw game controllers that are currently connected. Because you might only be interested in some of the connected raw game controllers, we recommend that you maintain your own collection instead of accessing them through the **RawGameControllers** property.

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

When a raw game controller is added or removed, the [RawGameControllerAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerAdded) and [RawGameControllerRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_RawGameControllerRemoved) events are raised. You can register handlers for these events to keep track of the raw game controllers that are currently connected.

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

## <a name="get-the-capabilities-of-a-raw-game-controller"></a>Obtener las funcionalidades de un dispositivo de juego sin procesar

Después de identificar el dispositivo de juego sin procesar que te interesa, puedes recopilar información sobre las funcionalidades del dispositivo. Puedes obtener el número de botones del dispositivo de juego sin procesar con [RawGameController.ButtonCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.ButtonCount), el número de ejes analógicos con [RawGameController.AxisCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.AxisCount) y el número de modificadores con [RawGameController.SwitchCount](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.SwitchCount). Además, puedes obtener el tipo de modificador mediante [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_).

En el siguiente ejemplo, se obtienen los recuentos de entrada de un dispositivo de juego sin procesar:

```cpp
auto rawGameController = myRawGameControllers->GetAt(0);
int buttonCount = rawGameController->ButtonCount;
int axisCount = rawGameController->AxisCount;
int switchCount = rawGameController->SwitchCount;
```

En el siguiente ejemplo, se determina el tipo de cada modificador:

```cpp
for (uint32_t i = 0; i < switchCount; i++)
{
    GameControllerSwitchKind mySwitch = rawGameController->GetSwitchKind(i);
}
```

## <a name="reading-the-raw-game-controller"></a>Leer el dispositivo de juego sin procesar

Después de obtener el número de entradas en un dispositivo de juego sin procesar, estás listo para recopilar datos de él. Sin embargo, a diferencia de algunos otros tipos de entrada con los que puedes estar familiarizado, un controlador de juego sin procesar no comunica el cambio de estado mediante la generación de eventos. En cambio, tienes que realizar lecturas periódicas de su estado actual mediante _sondeos_.

### <a name="polling-the-raw-game-controller"></a>Sondear el dispositivo de juego sin procesar

El sondeo captura una instantánea del dispositivo de juego sin procesar en un momento preciso en el tiempo. Este método para la recopilación de entradas es una buena opción para la mayoría de los juegos porque su lógica suele ejecutarse en un bucle determinista en lugar de ser impulsada por eventos. También suele ser más sencillo interpretar los comandos de juego desde las entradas recopiladas en una sola vez que lo que es de muchas entradas individuales recopiladas a medida que pasa el tiempo.

Un dispositivo de juego sin procesar se sondea mediante una llamada a [RawGameController.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetCurrentReading_System_Boolean___Windows_Gaming_Input_GameControllerSwitchPosition___System_Double___). Esta función rellena las matrices para los botones, modificadores y ejes que contienen el estado del dispositivo de juego sin procesar.

En el siguiente ejemplo se sondea el estado actual de un controlador de juego sin procesar:

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

No hay ninguna garantía de qué posición de cada matriz tendrá qué valor de entrada entre los distintos tipos de dispositivos. Por lo tanto, tendrás que comprobar qué entrada es qué con los métodos [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) y [RawGameController.GetSwitchKind](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetSwitchKind_System_Int32_).

El objeto **GetButtonLabel** te indicará el texto o símbolo impreso en el botón físico en lugar de la función del botón. Por lo tanto, sirve mejor como ayuda para la interfaz de usuario en los casos en los que quieras dar sugerencias al jugador sobre qué botones realizan qué las funciones. El objeto **GetSwitchKind** te indicará el tipo de modificador (es decir, cuántas posiciones tiene), pero no el nombre.

No hay ninguna manera estandarizada de obtener la etiqueta de un eje o modificador, por lo que tendrás que probar estas tú mismo para determinar qué entrada es qué.

Si tienes un dispositivo específico que quieras admitir, puedes obtener los objetos [RawGameController.HardwareProductId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareProductId) y [RawGameController.HardwareVendorId](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller.HardwareVendorId), y comprobar si coinciden con ese dispositivo. La posición de cada entrada en cada matriz es la misma para todos los dispositivos con el mismo objeto **HardwareProductId** y **HardwareVendorId**, de modo que no tienes que preocuparte por que la lógica fuera incoherente entre los distintos dispositivos del mismo tipo.

Además del estado del dispositivo de juego sin procesar, cada lectura devuelve una marca de tiempo que indica con precisión cuándo se recuperó el estado. La marca de tiempo es útil para establecer una relación con los intervalos de lecturas anteriores o la duración de la simulación de juego.

### <a name="reading-the-buttons-and-switches"></a>Leer botones y modificadores

Cada uno de los botones del dispositivo juego sin procesar proporciona una lectura digital que indica si se presiona (abajo) o libera (arriba). Las lecturas de botón se representan como valores booleanos individuales en una única matriz. La etiqueta para cada botón se puede encontrar mediante [RawGameController.GetButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller#Windows_Gaming_Input_RawGameController_GetButtonLabel_System_Int32_) con el índice del valor booleano en la matriz. Cada valor se representa como un objeto [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel).

En el ejemplo siguiente, se determina si el botón **XboxA** está presionado:

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

A veces, es posible que quieras determinar si se suelta un botón que está presionado o si se presiona un botón que no lo estaba, si se presionan o sueltan varios botones o si un conjunto de botones tiene una disposición determinada; algunos presionados y otros no. Para obtener información sobre cómo detectar cada una de estas condiciones, consulta [Detección de transiciones de botón](input-practices-for-games.md#detecting-button-transitions) y [Detección de disposiciones de botones complejas](input-practices-for-games.md#detecting-complex-button-arrangements).

Los valores de modificador se proporcionan como una matriz de [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition). Dado que esta propiedad es un campo de bits, se usan máscaras bit a bit para aislar la dirección del modificador.

En el ejemplo siguiente se determina si cada modificador está en la posición arriba:

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

### <a name="reading-the-analog-inputs-sticks-triggers-throttles-and-so-on"></a>Leer las entradas analógicas (mandos, disparadores, aceleradores, etc.)

Un eje analógico proporciona una lectura entre 0,0 y 1,0. Esto incluye cada dimensión en un mando, como X e Y para los mandos estándar o incluso los ejes X, Y y Z (vuelta, inclinación y guiñada, respectivamente) para las palancas de mando.

Los valores pueden representar disparadores analógicos, aceleradores o cualquier otro tipo de entrada analógica. Estos valores no se suministran con etiquetas, por lo que sugerimos que el código se pruebe con una variedad de dispositivos de entrada para asegurarte de que coincidan correctamente con tus suposiciones.

En todos los ejes, el valor es aproximadamente 0,5 cuando el mando está en la posición central, pero es normal que el valor exacto varíe, incluso entre lecturas posteriores; las estrategias para mitigar esta variación se describen más adelante en esta sección.

En el siguiente ejemplo se muestra cómo leer los valores analógicos desde un mando de Xbox:

```cpp
// Xbox controllers have 6 axes: 2 for each stick and one for each trigger.
float leftStickX = currentAxisReading[0];
float leftStickY = currentAxisReading[1];
float rightStickX = currentAxisReading[2];
float rightStickY = currentAxisReading[3];
float leftTrigger = currentAxisReading[4];
float rightTrigger = currentAxisReading[5];
```

Al leer los valores del mando analógico, verás que no producen una lectura neutra confiable de 0,5 cuando el mando está en reposo en la posición central; en su lugar, se producen diferentes valores próximos a 0,5 cada vez que se mueve el mando analógico y se devuelve a la posición central. Para mitigar estas variaciones, puedes implementar una pequeña _zona muerta_, que es un intervalo de valores cerca de la posición central ideal que se omiten.

Una manera de implementar una zona muerta es determinar la distancia que se ha desplazado el mando desde el centro y pasar por alto las lecturas más próximas a una cierta distancia que elijas. Puedes calcular la distancia a grandes rasgos (no es exacta porque las lecturas del mando analógico son básicamente valores polares, no planos) simplemente con el teorema de Pitágoras. Esto genera una zona muerta radial.

En el siguiente ejemplo se muestra una zona muerta radial básica mediante el teorema de Pitágoras:

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

## <a name="see-also"></a>Ver también

* [Entrada para juegos](input-for-games.md)
* [Prácticas de entrada para juegos](input-practices-for-games.md)
* [Espacio de nombres Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Clase Windows.Gaming.Input.RawGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.rawgamecontroller)
* [Interfaz Windows.Gaming.Input.IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Interfaz Windows.Gaming.Input.IGameControllerBatteryInfo](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontrollerbatteryinfo)