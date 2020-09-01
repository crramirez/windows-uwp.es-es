---
title: Palanca de mandos
description: Use las API de lápiz de vuelo Windows. Gaming. Input para leer la entrada de los paquetes de vuelos.
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.date: 03/06/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, entrada, caja de vuelo
ms.localizationpriority: medium
ms.openlocfilehash: b9b4353a2736feb7cdfde9871c29f61de52e0c9a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156429"
---
# <a name="flight-stick"></a>Palanca de mandos

En esta página se describen los aspectos básicos de la programación de los paquetes de vuelos con certificación de Xbox mediante [Windows. Gaming. Input. FlightStick](/uwp/api/windows.gaming.input.flightstick) y las API relacionadas para el plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:

* cómo recopilar una lista de bastones de vuelo conectados y sus usuarios
* Cómo detectar que se ha agregado o quitado un stick de vuelo
* Cómo leer la entrada de uno o más palos
* Cómo se comportan los vuelos como dispositivos de navegación de la interfaz de usuario

## <a name="overview"></a>Información general

Los palos son dispositivos de entrada de juegos que se han valorado para reproducir el aspecto de los palos que se encontraban en un avión o una cabina de espacio. Son el dispositivo de entrada perfecto para un control rápido y preciso del vuelo. Los palos se admiten en aplicaciones de UWP de Windows 10 y Xbox One mediante el espacio de nombres [Windows. Gaming. Input](/uwp/api/windows.gaming.input) .

Los palos de Xbox One están equipados con los siguientes controles:

* Un joystick analógico distorsionable capaz de rollo, de brea y de guiñada
* Una limitación analógica
* Dos botones de activación
* Conmutador digital Hat de 8 vías
* Botones **Ver** y **menú**

> [!NOTE]
> Los botones **Ver** y **menú** se usan para admitir la navegación de la interfaz de usuario, no los comandos de juego y, por lo tanto, no se puede acceder fácilmente a ellos como botones del joystick.

### <a name="ui-navigation"></a>Navegación de la interfaz de usuario

Con el fin de facilitar la carga de admitir los distintos dispositivos de entrada para la navegación por la interfaz de usuario y fomentar la coherencia entre los juegos y los dispositivos, la mayoría de los dispositivos de entrada _físicos_ actúan simultáneamente como dispositivos de entrada _lógicos_ independientes denominados [controladores de navegación](ui-navigation-controller.md)de la interfaz de usuario. El controlador de navegación de la interfaz de usuario proporciona un vocabulario común para los comandos de navegación de la interfaz de usuario entre los dispositivos de entrada.

Como controlador de navegación de la interfaz de usuario, un stick de vuelo asigna el [conjunto necesario](ui-navigation-controller.md#required-set) de comandos de navegación a los botones joystick y **vista**, **menú**, **FirePrimary**y **FireSecondary** .

| Comando de navegación | Entrada de vuelo                  |
| ------------------:| ----------------------------------- |
|                 Arriba | Joystick up                         |
|               Bajar | Joystick inactivo                       |
|               Left | Joystick a la izquierda                       |
|              Right | Joystick a la derecha                      |
|               Ver | Botón **Ver**                     |
|               Menú | Botón de **menú**                     |
|             Accept | Botón **FirePrimary**              |
|             Cancelar | Botón **FireSecondary**            |

Los palos de vuelos no asignan ninguno de los [conjuntos opcionales](ui-navigation-controller.md#optional-set) de comandos de navegación.

## <a name="detect-and-track-flight-sticks"></a>Detección y seguimiento de los palos de vuelos

La detección y el seguimiento de los paquetes de vuelos funcionan exactamente de la misma manera que para los controladores de juegos, excepto con la clase [FlightStick](/uwp/api/windows.gaming.input.flightstick) en lugar de la clase de [controlador de juegos](/uwp/api/Windows.Gaming.Input.Gamepad) . Consulte [controlador de juegos y vibración](gamepad-and-vibration.md) para obtener más información.

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

The following example copies all connected flight sticks into a new collection:

```cpp
auto myFlightSticks = ref new Vector<FlightStick^>();

for (auto flightStick : FlightStick::FlightSticks)
{
    // This code assumes that you're interested in all flight sticks.
    myFlightSticks->Append(flightStick);
}
```

### Adding and removing flight sticks

When a flight stick is added or removed, the [FlightStickAdded](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

The following example starts tracking a flight stick that's been added:

```cpp
FlightStick::FlightStickAdded += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    // This code assumes that you're interested in all new flight sticks.
    myFlightSticks->Append(args);
});
```

The following example stops tracking a flight stick that's been removed:

```cpp
FlightStick::FlightStickRemoved += 
    ref new EventHandler<FlightStick^>([] (Platform::Object^, FlightStick^ args)
{
    unsigned int indexRemoved;

    if (myFlightSticks->IndexOf(args, &indexRemoved))
    {
        myFlightSticks->RemoveAt(indexRemoved);
    }
});
```

### Users and headsets

Each flight stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-flight-stick"></a>Leer el stick de vuelo

Después de identificar el palo de vuelos que le interesa, está listo para recopilar datos de este. Sin embargo, a diferencia de otros tipos de entrada que se pueden usar, los paquetes de vuelo no comunican el cambio de estado mediante la generación de eventos. En cambio, tienes que realizar lecturas regulares de su estado actual mediante _sondeos_.

### <a name="polling-the-flight-stick"></a>Sondeo del palo de vuelos

El sondeo captura una instantánea del palo de vuelos en un momento concreto. Este enfoque para la recopilación de entradas es una buena opción para la mayoría de los juegos porque su lógica normalmente se ejecuta en un bucle determinista en lugar de ser controlado por eventos. Normalmente, es más fácil interpretar los comandos del juego a partir de la entrada recopilada todos a la vez que desde muchas entradas únicas recopiladas a lo largo del tiempo.

Sondeo un stick de vuelo llamando a [FlightStick. GetCurrentReading](/uwp/api/windows.gaming.input.flightstick.GetCurrentReading). Esta función devuelve un [FlightStickReading](/uwp/api/windows.gaming.input.flightstickreading) que contiene el estado del palo de vuelos.

En el siguiente ejemplo se sondea el estado actual de un stick de vuelo:

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

Además del estado de la caja de vuelo, cada lectura incluye una marca de tiempo que indica exactamente cuándo se recuperó el estado. La marca de tiempo es útil para establecer una relación con los intervalos de lecturas anteriores o la duración de la simulación de juego.

### <a name="reading-the-joystick-and-throttle-input"></a>Leer el joystick y la entrada del acelerador

El joystick proporciona una lectura analógica entre-1,0 y 1,0 en los ejes X, Y y Z (rollo, tono e guiñada, respectivamente). En el caso de roll, el valor-1,0 corresponde a la posición del joystick en el extremo izquierdo, mientras que un valor de 1,0 corresponde a la posición más a la derecha. Para el paso, el valor-1,0 corresponde a la posición del joystick de nivel inferior, mientras que un valor de 1,0 corresponde a la posición superior. En el caso de guiñada, un valor de-1,0 corresponde a la posición retorcida más en sentido contrario a las agujas del reloj, mientras que un valor de 1,0 corresponde a la posición más a la derecha.

En todos los ejes, el valor es aproximadamente 0,0 cuando el joystick está en la posición central, pero es normal que varíe el valor preciso, incluso entre las lecturas posteriores. Más adelante en esta sección se describen las estrategias para mitigar esta variación.

El valor del rollo del joystick se lee desde la propiedad [FlightStickReading. Roll](/uwp/api/windows.gaming.input.flightstickreading.Roll) , el valor del tono se lee desde la propiedad [FlightStickReading. Pitch](/uwp/api/windows.gaming.input.flightstickreading.Pitch) y el valor de guiñada se lee desde la propiedad [FlightStickReading. guiñada](/uwp/api/windows.gaming.input.flightstickreading.Yaw) :

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

Al leer los valores del joystick, observará que no producen una lectura neutra de 0,0 cuando el joystick está en reposo en la posición central; en su lugar, generarán valores distintos cerca de 0,0 cada vez que el joystick se mueva y se devuelva a la posición central. Para mitigar estas variaciones, puedes implementar una pequeña _zona muerta_, que es un intervalo de valores cerca de la posición central ideal que se omiten.

Una manera de implementar un Deadzone es determinar hasta qué punto se ha despasado el joystick del centro y omitir las lecturas más cercanas a la distancia que elija. Puede calcular la distancia aproximadamente &mdash; no es exacto porque las lecturas del joystick son esencialmente valores polares, no planos, &mdash; simplemente mediante el teorema de pitagórica. Esto genera un zona muerta radial.

En el ejemplo siguiente se muestra un Deadzone radial básico con pitagórica teorema:

```cpp
// Choose a deadzone. Readings inside this radius are ignored.
const float deadzoneRadius = 0.1f;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem: For a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
float oppositeSquared = pitch * pitch;
float adjacentSquared = roll * roll;

// Accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // Input accepted, process it.
}
```

### <a name="reading-the-buttons-and-hat-switch"></a>Lectura de los botones y el conmutador Hat

Cada uno de los botones de activación de la caja de vuelo proporciona una lectura digital que indica si se ha presionado (hacia abajo) o se ha soltado (arriba). Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales &mdash; , sino que se empaquetan en un solo campo de bits que está representado por la enumeración [FlightStickButtons](/uwp/api/windows.gaming.input.flightstickbuttons) . Además, el conmutador Hat de 8-Way proporciona una dirección empaquetada en un solo campo de bits que se representa mediante la enumeración [GameControllerSwitchPosition](/uwp/api/windows.gaming.input.gamecontrollerswitchposition) .

> [!NOTE]
> Los palos están equipados con botones adicionales que se usan para la navegación de la interfaz de usuario, como los botones de **vista** y de **menú** . Estos botones no forman parte de la `FlightStickButtons` enumeración y solo se pueden leer accediendo al dispositivo de vuelo como un dispositivo de navegación de la interfaz de usuario. Para obtener más información, vea controlador de navegación de la [interfaz de usuario](ui-navigation-controller.md).

Los valores de botón se leen de la propiedad [FlightStickReading. Buttons](/uwp/api/windows.gaming.input.flightstickreading.Buttons) . Dado que esta propiedad es un campo de bits, se usa el enmascaramiento bit a bit para aislar el valor del botón que te interesa. El botón está presionado (abajo) cuando se establece el bit correspondiente; de lo contrario, se libera (up).

En el ejemplo siguiente se determina si se presiona el botón **FirePrimary** :

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

En el ejemplo siguiente se determina si se libera el botón **FirePrimary** :

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

En ocasiones, es posible que desee determinar si un botón cambia de presionado a liberado o liberado a presionado, si se presionan o se liberan varios botones, o si un conjunto de botones se organizan de una manera determinada &mdash; , pero no. Para obtener información sobre cómo detectar cada una de estas condiciones, consulta [Detecting button transitions (Detección de transiciones de botón)](input-practices-for-games.md#detecting-button-transitions) y [Detecting complex button arrangements (Detección de disposiciones de botones complejas)](input-practices-for-games.md#detecting-complex-button-arrangements).

El valor del modificador Hat se lee de la propiedad [FlightStickReading. HatSwitch](/uwp/api/windows.gaming.input.flightstickreading.HatSwitch) . Dado que esta propiedad también es un campo de bits, se vuelve a usar el enmascaramiento bit a bit para aislar la posición del conmutador Hat.

En el ejemplo siguiente se determina si el conmutador Hat está en la posición superior:

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

En el ejemplo siguiente se determina si el conmutador Hat está en la posición central:

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>Vea también

* [Clase Windows. Gaming. Input. UINavigationController](/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Interfaz Windows. Gaming. Input. IGameController](/uwp/api/windows.gaming.input.igamecontroller)
* [Procedimientos de entrada para juegos](input-practices-for-games.md)