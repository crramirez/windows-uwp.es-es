---
author: eliotcowley
title: Palanca de mandos
description: Usa las API de palanca de mandos Windows.Gaming.Input para leer entradas de las palancas de mando.
ms.assetid: DC633F6B-FDC9-4D6E-8401-305861F31192
ms.author: wdg-dev-content
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp, games, juegos, input, entrada, flight stick, palanca de mandos
ms.localizationpriority: medium
ms.openlocfilehash: ebe7695b3f16271f3adedae658c0d62d38d7c078
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7278726"
---
# <a name="flight-stick"></a>Palanca de mandos

En esta página se describen los conceptos básicos de programación para palancas de mandos certificadas para Xbox One mediante [Windows.Gaming.Input.FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) y las API relacionadas para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre lo siguiente:

* Cómo obtener una lista de palancas de mandos conectadas y sus usuarios
* Cómo detectar que se ha agregado o quitado una palanca de mandos
* Cómo leer la entrada de una o más palancas de mandos
* Cómo las palancas de mandos se comportan como dispositivos de navegación de interfaz de usuario

## <a name="overview"></a>Introducción

Las palancas de mandos son dispositivos de entrada de juegos que tienen un valor para reproducir la sensación de las palancas de mandos que se encontrarían en un avión o cabina de nave espacial. Son el dispositivo de entrada perfecto para un control de vuelo rápido y preciso. Las palancas de mandos se admiten en aplicaciones para UWP de Windows 10 y Xbox One en el espacio de nombres [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input).

Las palancas de mandos de Xbox One vienen equipadas con los siguientes controles:

* Un joystick analógico que se puede retorcer con capacidad para vueltas, inclinaciones y guiñadas
* Un acelerador analógico
* Dos botones de disparo
* Un botón de control digital de 8 vías
* Botones **Vista** y **Menú**

> [!NOTE]
> Los botones **Vista** y **Menú** se usan para la compatibilidad con la navegación de la interfaz de usuario, no los comandos de juego y, por tanto, no se pueden acceder a ellos fácilmente como botones de joystick.

### <a name="ui-navigation"></a>Navegación de la interfaz de usuario

Para aliviar la carga de la compatibilidad con los diferentes dispositivos de entrada para la navegación de la interfaz de usuario y fomentar la coherencia entre dispositivos y juegos, la mayoría de dispositivos de entrada _físicos_ actúan simultáneamente como dispositivos de entrada _lógica_ independientes llamados [controladores de navegación de la interfaz de usuario](ui-navigation-controller.md). El controlador de navegación de la interfaz de usuario proporciona un vocabulario común para los comandos de navegación de la interfaz de usuario entre los dispositivos de entrada.

Como dispositivo de navegación de la interfaz de usuario, una palanca de mandos asigna el [conjunto necesario](ui-navigation-controller.md#required-set) de comandos de navegación al joystick y los botones **Vista**, **Menú**, **FirePrimary** y **FireSecondary**.

| Comando de navegación | Entrada de la palanca de mandos                  |
| ------------------:| ----------------------------------- |
|                 Arriba | Joystick hacia arriba                         |
|               Abajo | Joystick hacia abajo                       |
|               Izquierda | Joystick hacia la izquierda                       |
|              Derecha | Joystick hacia la derecha                      |
|               Vista | Botón **Vista**                     |
|               Menú | Botón **Mené**                     |
|             Aceptar | Botón **FirePrimary**              |
|             Cancelar | Botón **FireSecondary**            |

Las palancas de mandos no asignan ninguno de los [conjuntos opcionales](ui-navigation-controller.md#optional-set) de comandos de navegación.

## <a name="detect-and-track-flight-sticks"></a>Detección y seguimiento de las palancas de mandos

La detección y el seguimiento de palancas de vuelo funciona exactamente del mismo modo que para los controladores para juegos, solo que con la clase [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) en lugar de la clase [Gamepad](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) clase. Para obtener más información, consulta [Controlador para juegos y vibración](gamepad-and-vibration.md).

<!-- Flight sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected flight sticks and events to notify you when a flight stick is added or removed.

### The flight stick list

The [FlightStick](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick) class provides a static property, [FlightSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightSticks), which is a read-only list of flight sticks that are currently connected. Because you might only be interested in some of the connected flight sticks, we recommend that you maintain your own collection instead of accessing them through the `FlightSticks` property.

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

When a flight stick is added or removed, the [FlightStickAdded](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickAdded) and [FlightStickRemoved](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick#Windows_Gaming_Input_FlightStick_FlightStickRemoved) events are raised. You can register handlers for these events to keep track of the flight sticks that are currently connected.

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

## <a name="reading-the-flight-stick"></a>Lectura de la palanca de mandos

Después de identificar la palanca de mandos que te interesa, puedes recopilar datos de ella. Sin embargo, a diferencia de algunos otros tipos de entrada con los que puedes estar familiarizado, las palancas de mandos no comunican el cambio de estado mediante la generación de eventos. En cambio, tienes que realizar lecturas periódicas de su estado actual mediante _sondeos_.

### <a name="polling-the-flight-stick"></a>Sondeo de la palanca de mandos

El sondeo captura una instantánea de la palanca de mandos en un momento preciso en el tiempo. Este enfoque de la recopilación de entrada es una buena opción para la mayoría de los juegos porque su lógica normalmente se ejecuta en un bucle determinista en lugar de controlarse mediante eventos También suele ser más sencillo interpretar comandos de juego de la entrada recopilada de una vez que de muchas entradas individuales recopiladas a lo largo del tiempo

Una palanca de mandos se sondea al llamar a [FlightStick.GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstick.GetCurrentReading). Esta función devuelve un objeto [FlightStickReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading) que contiene el estado de la palanca de mandos.

En el siguiente ejemplo se sondea el estado actual de una palanca de mandos:

```cpp
auto flightStick = myFlightSticks->GetAt(0);
FlightStickReading reading = flightStick->GetCurrentReading();
```

Además del estado de la palanca de mandos, cada lectura incluye una marca de tiempo que indica con precisión cuándo se recuperó el estado. La marca de tiempo es útil para establecer una relación con los intervalos de lecturas anteriores o la duración de la simulación de juego.

### <a name="reading-the-joystick-and-throttle-input"></a>Leer la entrada del joystick y el acelerador

El joystick proporciona una lectura analógica entre -1,0 y 1,0 en los ejes X, Y y Z (vuelta, inclinación y guiñada, respectivamente). Para una vuelta, un valor de -1,0 corresponde a la posición de la extrema izquierda de la palanca de mandos, mientras que un valor de 1,0 corresponde a la posición de la extrema derecha. Para la inclinación, un valor de -1,0 corresponde a la posición del extremo inferior de la palanca de mandos, mientras que un valor de 1,0 corresponde a la posición del extremo superior. Para la guiñada, un valor de -1,0 corresponde a la posición retorcida del extremo antihorario, mientras que un valor de 1,0 corresponde a la posición del extremo horario.

En todos los ejes, el valor es aproximadamente 0,0, incluso cuando el joystick se encuentra en la posición del centro, pero es normal que el valor preciso varíe, incluso entre lecturas posteriores. Las estrategias para mitigar esta variación se describen más adelante en esta sección.

El valor de vuelta del joystick se lee de la propiedad [FlightStickReading.Roll](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Roll), el valor de la inclinación se lee de la propiedad [FlightStickReading.Pitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Pitch) y el valor de la guiñada se lee de la propiedad [FlightStickReading.Yaw](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Yaw):

```cpp
// Each variable will contain a value between -1.0 and 1.0.
float roll = reading.Roll;
float pitch = reading.Pitch;
float yaw = reading.Yaw;
```

Al leer los valores del joystick, verás que no producen una lectura neutra confiable de 0,0 cuando el joystick está en reposo en la posición central. En su lugar, se producen diferentes valores próximos a 0,0 cada vez que se mueve el joystick y se devuelve a la posición central. Para mitigar estas variaciones, puedes implementar una pequeña _zona muerta_, que es un intervalo de valores cerca de la posición central ideal que se omiten.

Una manera de implementar una zona muerta es determinar la distancia que se ha desplazado el joystick desde el centro y pasar por alto las lecturas más próximas a una cierta distancia que elijas. Puedes calcular la distancia a grandes rasgos (no es exacta porque las lecturas del joystick son básicamente valores polares, no planos) simplemente con el teorema de Pitágoras. Esto genera una zona muerta radial.

En el siguiente ejemplo se muestra una zona muerta radial básica mediante el teorema de Pitágoras:

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

### <a name="reading-the-buttons-and-hat-switch"></a>Leer los botones y el botón de control

Cada uno de los dos botones de disparo de la palanca de mandos proporciona una lectura digital que indica si se presionó (abajo) o liberó (arriba). Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales. En su lugar, se empaquetan todas en un único campo de bits que se representa mediante la enumeración [FlightStickButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickbuttons). Además, el botón de control de 8 vías proporciona una dirección empaquetada en un campo de bits único que se representa mediante la enumeración [GameControllerSwitchPosition](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerswitchposition).

> [!NOTE]
> Las palancas de mandos están equipadas con botones adicionales que se usan para la navegación de la interfaz de usuario, como los botones **Vista** y **Menú**. Estos botones no forman parte de la enumeración `FlightStickButtons` y solo se pueden leer accediendo a la palanca de mandos como dispositivo de navegación de la interfaz de usuario. Para más información, consulta [Dispositivos de navegación de la interfaz de usuario](ui-navigation-controller.md).

Los valores del botón se leen de la propiedad [FlightStickReading.Buttons](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.Buttons). Dado que esta propiedad es un campo de bits, se usa el enmascaramiento bit a bit para aislar el valor del botón que te interesa. El botón está presionado (abajo) cuando se establece el bit correspondiente; de lo contrario no lo está (arriba).

En el ejemplo siguiente, se determina si el botón **FirePrimary** está presionado:

```cpp
if (FlightStickButtons::FirePrimary == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is pressed.
}
```

En el ejemplo siguiente, se determina si el botón **FirePrimary** está liberado:

```cpp
if (FlightStickButtons::None == (reading.Buttons & FlightStickButtons::FirePrimary))
{
    // FirePrimary is released (not pressed).
}
```

A veces, es posible que quieras determinar si se suelta un botón que está presionado o si se presiona un botón que no lo estaba, si se presionan o sueltan varios botones o si un conjunto de botones tiene una disposición determinada; algunos presionados y otros no. Para información sobre cómo detectar cada una de estas condiciones, consulta [Detección de transiciones de botón](input-practices-for-games.md#detecting-button-transitions) y [Detección de disposiciones de botones complejas](input-practices-for-games.md#detecting-complex-button-arrangements).

El valor del botón de control se lee de la propiedad [FlightStickReading.HatSwitch](https://docs.microsoft.com/uwp/api/windows.gaming.input.flightstickreading.HatSwitch). Dado que esta propiedad también es un campo de bits, las máscaras bit a bit se vuelven a usar aislar la posición del botón de control.

En el ejemplo siguiente se determina si el botón de control está en posición hacia arriba:

```cpp
if (GameControllerSwitchPosition::Up == (reading.HatSwitch & GameControllerSwitchPosition::Up))
{
    // The hat switch is in the up position.
}
```

En el ejemplo siguiente se determina si el botón de control está en posición central:

```cpp
if (GameControllerSwitchPosition::Center == (reading.HatSwitch & GameControllerSwitchPosition::Center))
{
    // The hat switch is in the center position.
}
```

<!--## Run the InputInterfacingUWP sample

The [InputInterfacingUWP sample _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) demonstrates how to use flight sticks and different kinds of input devices in tandem, as well as how these input devices behave as UI navigation controllers.-->

## <a name="see-also"></a>Consulta también

* [Clase Windows.Gaming.Input.UINavigationController](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Interfaz Windows.Gaming.Input.IGameController](https://docs.microsoft.com/uwp/api/windows.gaming.input.igamecontroller)
* [Prácticas de entrada para juegos](input-practices-for-games.md)