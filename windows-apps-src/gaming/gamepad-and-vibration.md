---
title: Controlador para juegos y vibración
description: Usa las API de controlador para juegos Windows.Gaming.Input para detectar, leer y enviar comandos de impulso y vibración a controladores para juegos.
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.date: 09/06/2018
ms.topic: article
keywords: Windows 10, UWP, juegos, controlador para juegos, vibración
ms.localizationpriority: medium
ms.openlocfilehash: 66844b78893ffa8cb92b6b17bd11d87c1d4b1aa0
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175329"
---
# <a name="gamepad-and-vibration"></a>Controlador para juegos y vibración

En esta página se describen los conceptos básicos de programación para controladores para juegos de Xbox One usando [Windows.Gaming.Input.Gamepad][gamepad] y las API relacionadas para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:

* Cómo obtener una lista de los controladores para juegos conectados y sus usuarios
* Cómo detectar que se ha agregado o quitado un controlador para juegos
* Cómo leer la entrada de uno o más controladores para juegos
* Cómo enviar comandos de vibración e impulso
* Cómo se comportan los controladores de juegos como dispositivos de navegación de IU

## <a name="gamepad-overview"></a>Información general del controlador para juegos

Los controladores para juegos como el Mando inalámbrico Xbox y el Mando inalámbrico Xbox S son dispositivos de entrada para juegos de propósito general. Son el dispositivo de entrada estándar en Xbox One y una opción habitual para los jugadores de Windows que no prefieran el teclado y el mouse. Los controladores para juegos son compatibles con aplicaciones para UWP de Xbox y Windows 10 en el espacio de nombres [Windows.Gaming.Input][].

Los controladores de juegos de Xbox One están equipados con un panel direccional (o el PAD D); **A**, **B**, **X**, **y**, **vista**y botones de **menú** ; thumbsticks izquierda y derecha, los reboteadores y los desencadenadores; y un total de cuatro motores de vibración. Ambos sticks analógicos proporcionan lecturas analógicas duales en los ejes X e Y y, además, actúan como botón cuando se presionan. Cada desencadenador proporciona una lectura analógica que representa hasta qué punto se vuelve a extraer.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **Paddle** buttons on its underside. These can be used to provide redundant access to game commands that are difficult to use together (such as the right thumbstick together with any of the **A**, **B**, **X**, or **Y** buttons) or to provide dedicated access to additional commands. -->

> [!NOTE]
> `Windows.Gaming.Input.Gamepad` también admite los controladores de juegos de Xbox 360, que tienen el mismo diseño de control que los controladores de juegos estándar de Xbox One.

### <a name="vibration-and-impulse-triggers"></a>Gatillos de impulso y vibración

Los controladores para juegos de Xbox One proporcionan dos motores independientes para la vibración fuerte y sutil del controlador para juegos, así como dos motores dedicados que proporcionan una vibración nítida a cada gatillo (esta característica única es la razón por la que se hace referencia a los gatillos del controlador para juegos de Xbox One como _gatillos de impulso_).

> [!NOTE]
> Los controladores de juegos de Xbox 360 no están equipados con _desencadenadores de impulsos_.

Para obtener más información, consulta [Información general sobre los gatillos de impulso y vibración](#vibration-and-impulse-triggers-overview).

### <a name="thumbstick-deadzones"></a>Zonas muertas del stick analógico

Lo ideal es que un stick analógico en reposo en la posición central produzca la misma lectura neutra en los ejes X e Y siempre. Sin embargo, debido a las fuerzas mecánicas y la sensibilidad del stick analógico, las lecturas reales en la posición central solo se aproximan al valor ideal neutro y pueden variar en lecturas posteriores. Por esta razón, siempre debe usar un pequeño _Deadzone_ &mdash; un intervalo de valores cerca de la posición de centro ideal que se omiten &mdash; para compensar las diferencias de fabricación, el desgaste mecánico u otros problemas del controlador de juegos.

Las zonas muertas mayores ofrecen una estrategia simple para separar la entrada intencionada de la entrada no intencionada.

Para obtener más información, consulta [Lectura de los sticks analógicos](#reading-the-thumbsticks).

### <a name="ui-navigation"></a>Navegación de la interfaz de usuario

Para aliviar la carga de la compatibilidad con los diferentes dispositivos de entrada para la navegación de la interfaz de usuario y fomentar la coherencia entre dispositivos y juegos, la mayoría de dispositivos de entrada _física_ actúan simultáneamente como dispositivo independiente de entrada _lógica_, llamado [controlador de navegación de la interfaz de usuario](ui-navigation-controller.md). El controlador de navegación de la interfaz de usuario proporciona un vocabulario común para los comandos de navegación de la interfaz de usuario entre los dispositivos de entrada.

Como controlador de navegación de la interfaz de usuario, los controladores de juegos asignan el [conjunto necesario](ui-navigation-controller.md#required-set) de comandos de navegación al Stick izquierdo, el panel D, la **vista**, el **menú**, **el**y el botón **B** .

| Comando de navegación | Entrada del controlador para juegos                       |
| ------------------:| ----------------------------------- |
|                 Arriba | Stick analógico izquierdo hacia arriba/cruceta hacia arriba       |
|               Bajar | Stick analógico izquierdo hacia abajo/cruceta hacia abajo   |
|               Left | Stick analógico izquierdo hacia la izquierda/cruceta hacia la izquierda   |
|              Right | Stick analógico izquierdo hacia la derecha/cruceta hacia la derecha |
|               Ver | Botón de vista                         |
|               Menú | Botón de menú                         |
|             Accept | Botón A                            |
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

La clase [Gamepad][] proporciona una propiedad estática, [Gamepads][], que es una lista de solo lectura de los controladores para juegos que están actualmente conectados. Dado que es posible que solo esté interesado en algunos de los controladores de juegos conectados, se recomienda que mantenga su propia colección en lugar de acceder a ellas a través de la `Gamepads` propiedad.

El siguiente ejemplo copia todos los controladores para juegos conectados en una nueva colección. Tenga en cuenta que, dado que otros subprocesos en segundo plano van a tener acceso a esta colección (en los eventos [GamepadAdded][] y [GamepadRemoved][] ), debe colocar un bloqueo en torno a cualquier código que lea o actualice la colección.

```cpp
auto myGamepads = ref new Vector<Gamepad^>();
critical_section myLock{};

for (auto gamepad : Gamepad::Gamepads)
{
    // Check if the gamepad is already in myGamepads; if it isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), gamepad);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all gamepads.
        myGamepads->Append(gamepad);
    }
}
```

```cs
private readonly object myLock = new object();
private List<Gamepad> myGamepads = new List<Gamepad>();
private Gamepad mainGamepad;

private void GetGamepads()
{
    lock (myLock)
    {
        foreach (var gamepad in Gamepad.Gamepads)
        {
            // Check if the gamepad is already in myGamepads; if it isn't, add it.
            bool gamepadInList = myGamepads.Contains(gamepad);

            if (!gamepadInList)
            {
                // This code assumes that you're interested in all gamepads.
                myGamepads.Add(gamepad);
            }
        }
    }   
}
```

### <a name="adding-and-removing-gamepads"></a>Agregar y quitar controladores para juegos

Cuando se agrega o se quita un controlador de juegos, se generan los eventos [GamepadAdded][] y [GamepadRemoved][] . Puedes registrar controladores de estos eventos para realizar un seguimiento de los controladores para juegos que están conectados actualmente.

En el siguiente ejemplo se inicia el seguimiento de un controlador para juegos que se ha agregado.

```cpp
Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all new gamepads.
        myGamepads->Append(args);
    }
}
```

```cs
Gamepad.GamepadAdded += (object sender, Gamepad e) =>
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    lock (myLock)
    {
        bool gamepadInList = myGamepads.Contains(e);

        if (!gamepadInList)
        {
            myGamepads.Add(e);
        }
    }
};
```

En el ejemplo siguiente se detiene el seguimiento de un controlador de juegos que se ha quitado. También deberá controlar lo que ocurre con los controladores de juegos a los que está realizando un seguimiento cuando se quitan. por ejemplo, este código solo realiza un seguimiento de la entrada de un controlador de juegos y simplemente lo establece en `nullptr` cuando se quita. Tendrá que comprobar cada fotograma si el controlador de juegos está activo y actualizar el controlador de juegos del que está recopilando datos cuando los controladores están conectados y desconectados.

```cpp
Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ myLock };

    if(myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        myGamepads->RemoveAt(indexRemoved);
    }
}
```

```cs
Gamepad.GamepadRemoved += (object sender, Gamepad e) =>
{
    lock (myLock)
    {
        int indexRemoved = myGamepads.IndexOf(e);

        if (indexRemoved > -1)
        {
            if (mainGamepad == myGamepads[indexRemoved])
            {
                mainGamepad = null;
            }

            myGamepads.RemoveAt(indexRemoved);
        }
    }
};
```

Consulte [prácticas de entrada para juegos](input-practices-for-games.md) para obtener más información.

### <a name="users-and-headsets"></a>Usuarios y auriculares

Cada controlador para juegos puede asociarse con una cuenta de usuario para vincular su identidad al juego y puede tener conectados unos auriculares para facilitar el chat de voz o las funciones en el juego. Para obtener más información sobre cómo trabajar con usuarios y auriculares, consulta [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) (Seguimiento de usuarios y sus dispositivos) y [Headset](headset.md) (Auriculares).

## <a name="reading-the-gamepad"></a>Lectura del controlador para juegos

Después de identificar el controlador para juegos que te interesa, puedes recopilar datos de él. Sin embargo, a diferencia de algunos otros tipos de entrada con los que puedes estar familiarizado, los controladores para juegos no comunican el cambio de estado mediante la generación de eventos. En cambio, tienes que realizar lecturas regulares de su estado actual mediante _sondeos_.

### <a name="polling-the-gamepad"></a>Sondeo del controlador para juegos

El sondeo captura una instantánea del dispositivo de navegación en un momento preciso en el tiempo. Este enfoque para la recopilación de entradas es una buena opción para la mayoría de los juegos porque su lógica normalmente se ejecuta en un bucle determinista en lugar de ser controlado por eventos. Normalmente, es más fácil interpretar los comandos del juego a partir de la entrada recopilada todos a la vez que desde muchas entradas únicas recopiladas a lo largo del tiempo.

El sondeo de un controlador para juegos se realiza llamando a [GetCurrentReading][]; esta función devuelve [GamepadReading][] que contiene el estado del controlador para juegos.

En el siguiente ejemplo se sondea el estado actual de un controlador para juegos.

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

```cs
Gamepad gamepad = myGamepads[0];

GamepadReading reading = gamepad.GetCurrentReading();
```

Además del estado del controlador para juegos, cada lectura incluye una marca de tiempo que indica con precisión cuándo se recuperó el estado. La marca de tiempo es útil para establecer una relación con los intervalos de lecturas anteriores o la duración de la simulación de juego.

### <a name="reading-the-thumbsticks"></a>Lectura de los sticks analógicos

Cada stick analógico proporciona una lectura analógica entre -1,0 y +1,0 en los ejes X e Y. En el eje X, un valor -1,0 corresponde a la posición más a la izquierda del stick analógico; un valor + 1,0 corresponde a la posición más a la derecha. En el eje Y, un valor -1,0 corresponde a la posición más abajo del stick analógico; un valor + 1,0 corresponde a la posición más arriba. En ambos ejes, el valor es aproximadamente 0,0 cuando el Stick está en la posición central, pero es normal que varíe el valor preciso, incluso entre lecturas posteriores. más adelante en esta sección se describen las estrategias para mitigar esta variación.

El valor del eje X del stick analógico izquierdo se lee en la propiedad `LeftThumbstickX` de la estructura [GamepadReading][]; el valor del eje Y se lee en la propiedad `LeftThumbstickY`. El valor del eje X del stick analógico derecho se lee en la propiedad `RightThumbstickX`; el valor del eje Y se lee en la propiedad `RightThumbstickY`.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
float rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
float rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
double rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
double rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

Al leer los valores del stick analógico, verás que no producen una lectura neutra confiable de 0,0 cuando el stick analógico está en reposo en la posición central; en su lugar, se producen diferentes valores próximos a 0,0 cada vez que se mueve el stick analógico y se devuelve a la posición central. Para mitigar estas variaciones, puedes implementar una pequeña _zona muerta_, que es un intervalo de valores cerca de la posición central ideal que se omiten. Una manera de implementar una zona muerta es determinar la distancia desde el centro que se ha movido el stick analógico y pasar por alto las lecturas más próximas a una cierta distancia que elijas. Puede calcular la distancia aproximadamente &mdash; no es exacto porque las lecturas del stick analógico son esencialmente valores polares, no planos, &mdash; simplemente mediante el teorema de pitagórica. Esto genera un zona muerta radial.

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
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const double deadzoneRadius = 0.1;
const double deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
double oppositeSquared = leftStickY * leftStickY;
double adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
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

```cs
double leftTrigger = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
double rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>Lectura de los botones

Cada uno de los botones del controlador de juegos &mdash; las cuatro direcciones del panel D, los reboteadores izquierdo y derecho, la presión de la palanca izquierda y derecha, **a**, **B**, **X**, **y**, **vista**y **menú** &mdash; proporcionan una lectura digital que indica si se ha presionado (hacia abajo) o se ha soltado (arriba). Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales; en su lugar, se empaquetan en un solo campo de bits que se representa mediante la enumeración [GamepadButtons][] .

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **paddle** buttons on its underside. These buttons are also represented in the `GamepadButtons` enumeration and their values are read in the same way as the standard gamepad buttons. -->

Los valores de los botones se leen en la propiedad `Buttons` de la estructura [GamepadReading][]. Dado que esta propiedad es un campo de bits, se usa el enmascaramiento bit a bit para aislar el valor del botón que te interesa. El botón está presionado (abajo) cuando se establece el bit correspondiente; de lo contrario, se libera (up).

En el ejemplo siguiente, se determina si el botón A está presionado.

```cpp
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

```cs
if (GamepadButtons.A == (reading.Buttons & GamepadButtons.A))
{
    // button A is pressed
}
```

En el ejemplo siguiente, se determina si el botón A no está presionado.

```cpp
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released
}
```

```cs
if (GamepadButtons.None == (reading.Buttons & GamepadButtons.A))
{
    // button A is released
}
```

En ocasiones, es posible que desee determinar si un botón cambia de presionado a liberado o liberado a presionado, si hay varios botones presionados o liberados, o si un conjunto de botones está organizado de una forma determinada &mdash; , pero no. Para obtener información sobre cómo detectar cada una de estas condiciones, consulta [Detecting button transitions (Detección de transiciones de botón)](input-practices-for-games.md#detecting-button-transitions) y [Detecting complex button arrangements (Detección de disposiciones de botones complejas)](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-gamepad-input-sample"></a>Ejecutar la muestra de entrada del controlador para juegos

La [muestra de GamepadUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadUWP) indica cómo conectarse a un controlador para juegos y leer su estado.

## <a name="vibration-and-impulse-triggers-overview"></a>Información general sobre los gatillos de impulso y vibración

Los motores de vibración dentro de un controlador para juegos se usan para proporcionar información táctil al usuario. Los juegos usan esta capacidad para crear una mayor sensación de inmersión, para ayudar a comunicar información de estado (por ejemplo, sufrir daños), para indicar la proximidad de objetos importantes o para otros usos creativos.

Los controladores para juegos de Xbox One están equipados con un total de cuatro motores de vibración independientes. Dos son motores grandes ubicados en el cuerpo del controlador para juegos; el motor de la izquierda proporciona vibraciones aproximadas de alta amplitud, mientras que el motor adecuado proporciona vibraciones más suave y sutil. Los otros dos son motores pequeños, dentro de cada gatillo, que proporcionan nítidas ráfagas de vibración directamente a los dedos del usuario del gatillo; esta capacidad única de los controladores para juegos de Xbox One es la razón por la que se hace referencia a sus gatillos como _gatillos de impulso_. Al orquestar estos motores, se puede producir una amplia gama de sensaciones táctiles.

## <a name="using-vibration-and-impulse"></a>Uso de vibración e impulso

La vibración del controlador para juegos se controla mediante la propiedad [Vibration][] de la clase [Gamepad][]. `Vibration` es una instancia de la estructura [GamepadVibration][] que se compone de cuatro valores de punto flotante. cada valor representa la intensidad de uno de los motores.

Aunque los miembros de la `Gamepad.Vibration` propiedad se pueden modificar directamente, se recomienda inicializar una `GamepadVibration` instancia independiente con los valores que desee y, a continuación, copiarla en la `Gamepad.Vibration` propiedad para cambiar las intensidades del motor reales todas a la vez.

En el siguiente ejemplo se muestra cómo cambiar todas las intensidades del motor a la vez.

```cpp
// get the first gamepad
Gamepad^ gamepad = Gamepad::Gamepads->GetAt(0);

// create an instance of GamepadVibration
GamepadVibration vibration;

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

```cs
// get the first gamepad
Gamepad gamepad = Gamepad.Gamepads[0];

// create an instance of GamepadVibration
GamepadVibration vibration = new GamepadVibration();

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
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

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
mainGamepad.Vibration = vibration;
```

Recuerda que estos dos motores no son idénticos; por lo tanto, si se establecen estas propiedades en el mismo valor, no se produce la misma vibración en cada uno de ellos. Para cualquier valor, el motor de la izquierda genera una vibración más fuerte con una frecuencia menor que el motor derecho que, &mdash; para el mismo valor, &mdash; genera una vibración más suave con una frecuencia mayor. Incluso en el valor máximo, el motor izquierdo no puede generar las altas frecuencias del motor derecho, ni el motor derecho puede producir las altas fuerzas del motor izquierdo. Aun así, debido a que los motores están conectados de forma rígida por el cuerpo del controlador para juegos, los jugadores no experimentan las vibraciones por separado totalmente, aunque los motores tengan diferentes características y puedan vibrar con intensidades diferentes. Este tipo de disposición permite producir una gama más amplia y expresiva de sensaciones que si los motores fueran idénticos.

### <a name="using-the-impulse-triggers"></a>Uso de los gatillos de impulso

Cada motor de gatillo de impulso toma un valor de punto flotante entre 0,0 (sin vibración) y 1,0 (la vibración más intensa). La intensidad del motor del gatillo izquierdo se establece mediante la propiedad `LeftTrigger` de la estructura [GamepadVibration][]; la intensidad del gatillo del motor derecho se establece mediante la propiedad `RightTrigger`.

En el siguiente ejemplo se establece la intensidad de ambos gatillos de impulso y se activan.

```cpp
GamepadVibration vibration;
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
mainGamepad.Vibration = vibration;
```

A diferencia de los otros, los dos motores de vibración dentro de los gatillos son idénticos, por lo que producen la misma vibración en los dos motores con el mismo valor. Sin embargo, como estos motores no están conectados de forma rígida, los jugadores experimentan las vibraciones independientemente. Este tipo de disposición permite dirigir sensaciones totalmente independientes a ambos gatillos al mismo tiempo y les ayuda a transmitir información más específica que los motores del cuerpo del controlador para juegos.

## <a name="run-the-gamepad-vibration-sample"></a>Ejecutar la muestra de vibración del controlador para juegos

La [muestra de GamepadVibrationUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadVibrationUWP) indica cómo se utilizan los motores de vibración del controlador para juegos y los gatillos de impulso para producir una variedad de efectos.

## <a name="see-also"></a>Vea también

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Procedimientos de entrada para juegos](input-practices-for-games.md)

[Windows. Gaming. Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[gamepad]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepads]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepadadded]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepadremoved]: /uwp/api/Windows.Gaming.Input.Gamepad
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.Gamepad
[vibration]: /uwp/api/Windows.Gaming.Input.Gamepad
[gamepadreading]: /uwp/api/Windows.Gaming.Input.GamepadReading
[gamepadbuttons]: /uwp/api/Windows.Gaming.Input.GamepadButtons
[gamepadvibration]: /uwp/api/Windows.Gaming.Input.GamepadVibration