---
author: mithom
title: "Controlador para juegos y vibración"
description: "Usa las API de controlador para juegos Windows.Gaming.Input para detectar, leer y enviar comandos de impulso y vibración a controladores para juegos."
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, juegos, games, controlador para juegos, gamepad, vibración, vibration"
ms.openlocfilehash: b92ddbaadd7fd09b252d5c8842ed89daf53a6982
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="gamepad-and-vibration"></a>Controlador para juegos y vibración

En esta página se describen los conceptos básicos de programación para controladores para juegos de Xbox One usando [Windows.Gaming.Input.Gamepad][gamepad] y las API relacionadas para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:
* Cómo obtener una lista de los controladores para juegos conectados y sus usuarios
* Cómo detectar que se ha agregado o quitado un controlador para juegos
* Cómo leer la entrada de uno o más controladores para juegos
* Cómo enviar comandos de vibración e impulso
* Cómo se comportan los controladores para juegos como dispositivo de navegación


## <a name="gamepad-overview"></a>Información general del controlador para juegos

Los controladores para juegos como el Mando inalámbrico Xbox y el Mando inalámbrico Xbox S son dispositivos de entrada para juegos de propósito general. Son el dispositivo de entrada estándar en Xbox One y una opción habitual para los jugadores de Windows que no prefieran el teclado y el mouse. Los controladores para juegos son compatibles con aplicaciones para UWP de Xbox y Windows 10 en el espacio de nombres [Windows.Gaming.Input][].

Los controladores para juegos de Xbox One están equipados con un mando de dirección (o cruceta); los botones **A**, **B**, **X**, **Y**, de **vista** y **menú**; los sticks analógicos izquierdo y derecho, los botones superiores y los gatillos; y un total de cuatro motores de vibración. Ambos sticks analógicos proporcionan lecturas analógicas duales en los ejes X e Y y, además, actúan como botón cuando se presionan. Cada gatillo proporciona una lectura analógica que representa cuánto se retrocede.

> **Nota**    El Mando inalámbrico Xbox Elite está equipado con cuatro botones de **Palanca** adicionales en la parte inferior. Estos se pueden usar para proporcionar acceso redundante a comandos de juego que son difíciles de usar en combinación (como el stick analógico derecho junto con cualquiera de los botones **A**, **B**, **X** o **Y**) o para proporcionar acceso dedicado a comandos adicionales.

> **Nota**    `Windows.Gaming.Input.Gamepad` también es compatible con controladores para juegos de Xbox 360, que tienen el mismo diseño de control que los controladores para juegos estándar de Xbox One.

### <a name="vibration-and-impulse-triggers"></a>Gatillos de impulso y vibración

Los controladores para juegos de Xbox One proporcionan dos motores independientes para la vibración fuerte y sutil del controlador para juegos, así como dos motores dedicados que proporcionan una vibración nítida a cada gatillo (esta característica única es la razón por la que se hace referencia a los gatillos del controlador para juegos de Xbox One como _gatillos de impulso_).

> **Nota**    Los controladores para juegos de Xbox 360 no están equipados con _gatillos de impulso_.

Para obtener más información, consulta [Información general sobre los gatillos de impulso y vibración](#vibration-and-impulse-triggers-overview).

### <a name="thumbstick-deadzones"></a>Zonas muertas del stick analógico

Lo ideal es que un stick analógico en reposo en la posición central produzca la misma lectura neutra en los ejes X e Y siempre. Sin embargo, debido a las fuerzas mecánicas y la sensibilidad del stick analógico, las lecturas reales en la posición central solo se aproximan al valor ideal neutro y pueden variar en lecturas posteriores. Por este motivo, siempre debes usar una pequeña _zona muerta_ (un intervalo de valores próximos a la posición ideal central que se omiten) para compensar las diferencias de fabricación, desgaste mecánico u otros problemas del controlador para juegos.

Las zonas muertas mayores ofrecen una estrategia simple para separar la entrada intencionada de la entrada no intencionada.

Para obtener más información, consulta [Lectura de los sticks analógicos](#reading-the-thumbsticks).

### <a name="ui-navigation"></a>Navegación de la interfaz de usuario

Para aliviar la carga de la compatibilidad con los diferentes dispositivos de entrada para la navegación de la interfaz de usuario y fomentar la coherencia entre dispositivos y juegos, la mayoría de dispositivos de entrada _física_ actúan simultáneamente como dispositivo independiente de entrada _lógica_, llamado [controlador de navegación de la interfaz de usuario](ui-navigation-controller.md). El controlador de navegación de la interfaz de usuario proporciona un vocabulario común para los comandos de navegación de la interfaz de usuario entre los dispositivos de entrada.

Como controlador de navegación de la interfaz de usuario, los controladores para juegos asignan el [conjunto necesario](ui-navigation-controller.md#required-set) de comandos de navegación al stick analógico izquierdo, la cruceta y los botones de **vista**, **menú**, **A** y **B**.

| Comando de navegación | Entrada del controlador para juegos                       |
| ------------------:| ----------------------------------- |
|                 Arriba | Stick analógico izquierdo hacia arriba/cruceta hacia arriba       |
|               Abajo | Stick analógico izquierdo hacia abajo/cruceta hacia abajo   |
|               Izquierda | Stick analógico izquierdo hacia la izquierda/cruceta hacia la izquierda   |
|              Derecha | Stick analógico izquierdo hacia la derecha/cruceta hacia la derecha |
|               Ver | Botón de vista                         |
|               Menú | Botón de menú                         |
|             Aceptar | Botón A                            |
|             Cancelar | Botón B                            |

Además, los controladores para juegos asignan todos los comandos del [conjunto opcional](ui-navigation-controller.md#optional-set) de navegación a las entradas restantes.

| Comando de navegación | Entrada del controlador para juegos          |
| ------------------:| ---------------------- |
|            Re Pág | Gatillo izquierdo           |
|          Av Pág | Gatillo derecho          |
|          Página a la izquierda | Botón superior izquierdo            |
|         Página a la derecha | Botón superior derecho           |
|          Desplazar hacia arriba | Stick analógico derecho hacia arriba    |
|        Desplazar hacia abajo | Stick analógico derecho hacia abajo  |
|        Desplazar a la izquierda | Stick analógico derecho hacia la izquierda  |
|       Desplazar a la derecha | Stick analógico derecho hacia la derecha |
|          Contexto 1 | Botón X               |
|          Contexto 2 | Botón Y               |
|          Contexto 3 | Presión del stick analógico izquierdo  |
|          Contexto 4 | Presión del stick analógico derecho |


## <a name="detect-and-track-gamepads"></a>Detectar y realizar un seguimiento de los controladores para juegos

El sistema administra los controladores para juegos, por lo tanto, no tendrás que crearlos ni inicializarlos. El sistema proporciona una lista de controladores para juegos conectados y eventos para notificarte cuándo se agrega o quita un controlador para juegos.

### <a name="the-gamepads-list"></a>Lista de controladores para juegos

La clase [Gamepad][] proporciona una propiedad estática, [Gamepads][], que es una lista de solo lectura de los controladores para juegos que están actualmente conectados. Como probablemente solo te interesen algunos de los controladores para juegos conectados, es recomendable que mantengas tu propia colección en lugar de acceder a ellos a través de la propiedad `Gamepads`.

El siguiente ejemplo copia todos los controladores para juegos conectados en una nueva colección.

```cpp
auto myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    // This code assumes that you're interested in all gamepads.
    myGamepads->Append(gamepad);
}
```

### <a name="adding-and-removing-gamepads"></a>Agregar y quitar controladores para juegos

Cuando se agrega o quita un controlador para juegos, se generan los eventos [GamepadAdded][] y [GamepadRemoved][]. Puedes registrar controladores de estos eventos para realizar un seguimiento de los controladores para juegos que están conectados actualmente.

En el siguiente ejemplo se inicia el seguimiento de un controlador para juegos que se ha agregado.

```cpp
Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    // This code assumes that you're interested in all new gamepads.
    myGamepads->Append(args);
}
```

En el siguiente ejemplo se detiene el seguimiento de un stick arcade que se ha quitado.

```cpp
Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if(myGamepads->IndexOf(args, &indexRemoved))
    {
        myGamepads->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>Usuarios y auriculares

Cada controlador para juegos puede asociarse con una cuenta de usuario para vincular su identidad al juego y puede tener conectados unos auriculares para facilitar el chat de voz o las funciones en el juego. Para obtener más información sobre cómo trabajar con usuarios y auriculares, consulta [Tracking users and their devices (Seguimiento de usuarios y sus dispositivos)](input-practices-for-games.md#tracking-users-and-their-devices) y [Headset (Auriculares)](headset.md).

## <a name="reading-the-gamepad"></a>Lectura del controlador para juegos

Después de identificar el controlador para juegos que te interesa, puedes recopilar datos de él. Sin embargo, a diferencia de algunos otros tipos de entrada con los que puedes estar familiarizado, los controladores para juegos no comunican el cambio de estado mediante la generación de eventos. En su lugar, tienes que realizar lecturas regulares de su estado actual mediante _sondeos_.

### <a name="polling-the-gamepad"></a>Sondeo del controlador para juegos

El sondeo captura una instantánea del dispositivo de navegación en un momento preciso en el tiempo. Este enfoque de la recopilación de entrada es una buena opción para la mayoría de los juegos porque su lógica normalmente se ejecuta en un bucle determinista en lugar de controlarse mediante eventos; también suele ser más sencillo interpretar comandos de juego de la entrada recopilada de una vez que de muchas entradas individuales recopiladas a lo largo del tiempo.

El sondeo de un controlador para juegos se realiza llamando a [GetCurrentReading][]; esta función devuelve [GamepadReading][] que contiene el estado del controlador para juegos.

En el siguiente ejemplo se sondea el estado actual de un controlador para juegos.

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

Además del estado del controlador para juegos, cada lectura incluye una marca de tiempo que indica con precisión cuándo se recuperó el estado. La marca de tiempo es útil para establecer una relación con los intervalos de lecturas anteriores o la duración de la simulación de juego.

### <a name="reading-the-thumbsticks"></a>Lectura de los sticks analógicos

Cada stick analógico proporciona una lectura analógica entre -1,0 y +1,0 en los ejes X e Y. En el eje X, un valor -1,0 corresponde a la posición más a la izquierda del stick analógico; un valor + 1,0 corresponde a la posición más a la derecha. En el eje Y, un valor -1,0 corresponde a la posición más abajo del stick analógico; un valor + 1,0 corresponde a la posición más arriba. En ambos ejes, el valor es aproximadamente 0,0 cuando el stick está en la posición central, pero es normal que el valor exacto varíe, incluso entre lecturas posteriores; las estrategias para mitigar esta variación se describen más adelante en esta sección.

El valor del eje X del stick analógico izquierdo se lee en la propiedad `LeftThumbstickX` de la estructura [GamepadReading][]; el valor del eje Y se lee en la propiedad `LeftThumbstickY`. El valor del eje X del stick analógico derecho se lee en la propiedad `RightThumbstickX`; el valor del eje Y se lee en la propiedad `RightThumbstickY`.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
float rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
float rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

Al leer los valores del stick analógico, verás que no producen una lectura neutra confiable de 0,0 cuando el stick analógico está en reposo en la posición central; en su lugar, se producen diferentes valores próximos a 0,0 cada vez que se mueve el stick analógico y se devuelve a la posición central. Para mitigar estas variaciones, puedes implementar una pequeña _zona muerta_, que es un intervalo de valores cerca de la posición central ideal que se omiten. Una manera de implementar una zona muerta es determinar la distancia desde el centro que se ha movido el stick analógico y pasar por alto las lecturas más próximas a una cierta distancia que elijas. Puedes calcular la distancia a grandes rasgos (no es exacta porque las lecturas del stick analógico son básicamente valores polares, no planos) simplemente con el teorema de Pitágoras. Esto genera un zona muerta radial.

En el siguiente ejemplo se muestra una zona muerta radial básica mediante el teorema de Pitágoras.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const float deadzoneRadius = 0.1;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if((oppositeSquared + adjacentSquared) < deadzoneSquared)
{
    // input accepted, process it
}
```

Cada stick analógico también actúa como un botón cuando se presiona; para obtener más información sobre la lectura de esta entrada, consulta [Lectura de los botones](#reading-the-buttons).

### <a name="reading-the-triggers"></a>Lectura de los gatillos

Los gatillos se representan como valores de punto flotante entre 0,0 (totalmente liberado) y 1,0 (totalmente presionado). El valor del gatillo izquierdo se lee en la propiedad `LeftTrigger` de la estructura [GamepadReading][]; el valor del gatillo derecho se lee en la propiedad `RightTrigger`.

```cpp
float leftTrigger  = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
float rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>Lectura de los botones

Cada uno de los botones del controlador para juegos (las cuatro direcciones de la cruceta, los botones superiores izquierdo y derecho, la presión del stick analógico izquierdo y derecho, **A**, **B**, **X**, **Y**, **vista** y **menú**) proporcionan una lectura digital que indica si está presionado (abajo) o no (arriba). Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales; en su lugar, se empaquetan todas en un único campo de bits que se representa mediante la enumeración [GamepadButtons][].

> **Nota**    El Mando inalámbrico Xbox Elite está equipado con cuatro botones de **Palanca** adicionales en la parte inferior. Estos botones también se representan en la enumeración `GamepadButtons` y sus valores se leen en la misma forma que los botones estándar del controlador para juegos.

Los valores de los botones se leen en la propiedad `Buttons` de la estructura [GamepadReading][]. Dado que esta propiedad es un campo de bits, se usa el enmascaramiento bit a bit para aislar el valor del botón que te interesa. El botón está presionado (abajo) cuando se establece el bit correspondiente; de lo contrario no lo está (arriba).

En el ejemplo siguiente, se determina si el botón A está presionado.

```cpp
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

En el ejemplo siguiente, se determina si el botón A no está presionado.

```cpp
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

A veces, es posible que quieras determinar si se suelta un botón que está presionado o si se presiona un botón que no lo estaba, si se presionan o sueltan varios botones o si un conjunto de botones tiene una disposición determinada, algunos presionados y otros no. Para obtener información sobre cómo detectar cada una de estas condiciones, consulta [Detecting button transitions (Detección de transiciones de botón)](input-practices-for-games.md#detecting-button-transitions) y [Detecting complex button arrangements (Detección de disposiciones de botones complejas)](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-gamepad-input-sample"></a>Ejecutar la muestra de entrada del controlador para juegos

La [muestra de GamepadUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/GamepadUWP) indica cómo conectarse a un controlador para juegos y leer su estado.


## <a name="vibration-and-impulse-triggers-overview"></a>Información general sobre los gatillos de impulso y vibración

Los motores de vibración dentro de un controlador para juegos se usan para proporcionar información táctil al usuario. Los juegos usan esta capacidad para crear una mayor sensación de inmersión, para ayudar a comunicar información de estado (por ejemplo, sufrir daños), para indicar la proximidad de objetos importantes o para otros usos creativos.

Los controladores para juegos de Xbox One están equipados con un total de cuatro motores de vibración independientes. Dos son motores grandes ubicados en el cuerpo del controlador para juegos; el motor izquierdo proporciona una vibración brusca, de gran amplitud, mientras que el motor derecho proporciona una vibración suave, más sutil. Los otros dos son motores pequeños, dentro de cada gatillo, que proporcionan nítidas ráfagas de vibración directamente a los dedos del usuario del gatillo; esta capacidad única de los controladores para juegos de Xbox One es la razón por la que se hace referencia a sus gatillos como _gatillos de impulso_. Al orquestar estos motores, se puede producir una amplia gama de sensaciones táctiles.


## <a name="using-vibration-and-impulse"></a>Uso de vibración e impulso

La vibración del controlador para juegos se controla mediante la propiedad [Vibration][] de la clase [Gamepad][]. `Vibration` Es una instancia de la estructura [GamepadVibration][] que se compone de cuatro valores de punto flotante. Cada valor representa la intensidad de uno de los motores.

Aunque los miembros de la propiedad `Gamepad.Vibration` pueden modificarse directamente, se recomienda inicializar una instancia de `GamepadVibration` separada con los valores que se quieran y, a continuación, copiarla en la propiedad `Gamepad.Vibration` para cambiar todas las intensidades del motor reales a la vez.

En el siguiente ejemplo se muestra cómo cambiar todas las intensidades del motor a la vez.

```cpp
// get the first gamepad
Gamepad^ gamepad = Gamepad::Gamepads->GetAt(0);

// create an instance of GamepadVibration
GamepadVibration vibration;

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration.
```

### <a name="using-the-vibration-motors"></a>Uso de los motores de vibración

Los motores de vibración izquierdo y derecho toman valores de punto flotante entre 0,0 (sin vibración) y 1,0 (la vibración más intensa). La intensidad del motor izquierdo se establece mediante la propiedad `LeftMotor` de la estructura [GamepadVibration][]; la intensidad del motor derecho se establece mediante la propiedad `RightMotor`.

El siguiente ejemplo establece la intensidad de ambos motores de vibración y activa la vibración del controlador para juegos.

```cpp
GamepadVibration vibration;
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
gamepad.Vibration = vibration;
```

Recuerda que estos dos motores no son idénticos; por lo tanto, si se establecen estas propiedades en el mismo valor, no se produce la misma vibración en cada uno de ellos. Con cualquier valor, el motor izquierdo genera una vibración más fuerte con una frecuencia inferior que el motor derecho que, con el mismo valor, produce una vibración más suave con una frecuencia superior. Incluso en el valor máximo, el motor izquierdo no puede generar las altas frecuencias del motor derecho, ni el motor derecho puede producir las altas fuerzas del motor izquierdo. Aun así, debido a que los motores están conectados de forma rígida por el cuerpo del controlador para juegos, los jugadores no experimentan las vibraciones por separado totalmente, aunque los motores tengan diferentes características y puedan vibrar con intensidades diferentes. Este tipo de disposición permite producir una gama más amplia y expresiva de sensaciones que si los motores fueran idénticos.

### <a name="using-the-impulse-triggers"></a>Uso de los gatillos de impulso

Cada motor de gatillo de impulso toma un valor de punto flotante entre 0,0 (sin vibración) y 1,0 (la vibración más intensa). La intensidad del motor del gatillo izquierdo se establece mediante la propiedad `LeftTrigger` de la estructura [GamepadVibration][]; la intensidad del gatillo del motor derecho se establece mediante la propiedad `RightTrigger`.

En el siguiente ejemplo se establece la intensidad de ambos gatillos de impulso y se activan.

```cpp
GamepadVibration vibration;
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
gamepad.Vibration = vibration;
```

A diferencia de los otros, los dos motores de vibración dentro de los gatillos son idénticos, por lo que producen la misma vibración en los dos motores con el mismo valor. Sin embargo, como estos motores no están conectados de forma rígida, los jugadores experimentan las vibraciones independientemente. Este tipo de disposición permite dirigir sensaciones totalmente independientes a ambos gatillos al mismo tiempo y les ayuda a transmitir información más específica que los motores del cuerpo del controlador para juegos.


## <a name="run-the-gamepad-vibration-sample"></a>Ejecutar la muestra de vibración del controlador para juegos

La [muestra de GamepadVibrationUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/GamepadVibrationUWP) indica cómo se utilizan los motores de vibración del controlador para juegos y los gatillos de impulso para producir una variedad de efectos.

## <a name="see-also"></a>Consulta también
[Windows.Gaming.Input.UINavigationController][]
[Windows.Gaming.Input.IGameController][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[controlador para juegos]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[controladores para juegos]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepads.aspx
[gamepadadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadadded.aspx
[gamepadremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.getcurrentreading.aspx
[vibración]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.vibration.aspx
[gamepadreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadreading.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
[gamepadvibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadvibration.aspx
