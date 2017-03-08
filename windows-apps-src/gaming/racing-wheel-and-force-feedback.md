---
author: mithom
title: Volante y fuerza de respuesta
description: Usa las API de volante Windows.Gaming.Input para detectar y leer comandos de fuerza de respuesta, para enviarlos a los volantes y para determinar sus funcionalidades.
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, games, juegos, racing wheel, volante, force feedback, fuerza de respuesta
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ca8413a857fd4d8925a6767280a32a8336eeba19
ms.lasthandoff: 02/07/2017

---

# <a name="racing-wheel-and-force-feedback"></a>Volante y fuerza de respuesta

En esta página se describen los conceptos básicos de programación de los volantes de Xbox One usando [Windows.Gaming.Input.RacingWheel][racingwheel] y las API relacionadas para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:
* Cómo obtener una lista de los volantes conectados y sus usuarios.
* Cómo detectar que se ha agregado o quitado un volante.
* Cómo leer la entrada de uno o más volantes.
* Cómo enviar comandos de fuerza de respuesta
* El comportamiento de los volantes como dispositivos de navegación.


## <a name="racing-wheel-overview"></a>Información general sobre los volantes

Los volantes son dispositivos de entrada cuya apariencia se parece mucho a la cabina de un coche de carreras real. Los volantes son el dispositivo de entrada perfecto para los juegos de carreras de estilo Arcade y simulación que representan coches o camiones. Los volantes son compatibles con aplicaciones para UWP de Windows 10 y Xbox One en el espacio de nombres [Windows.Gaming.Input][].

Los volantes de Xbox One se ofrecen en distintas gamas de precios, por lo general con más y mejores funcionalidades de entrada y fuerza de respuesta cuanto más alto es el precio. Todos los volantes están equipados con un volante analógico, controles de acelerador y freno analógicos y algunos botones en el volante. Además, algunos volantes están equipados con controles de embrague y freno de mano analógicos, palancas de cambios y funcionalidades de fuerza de respuesta. No todos los volantes están equipados con los mismos conjuntos de características y también pueden variar en la compatibilidad de determinadas características; por ejemplo, los volantes pueden admitir distintos rangos de rotación y las palancas de cambios pueden admitir un número distinto de velocidades.

### <a name="device-capabilities"></a>Funcionalidades del dispositivo

Los distintos volantes para Xbox One ofrecen diferentes conjuntos de funcionalidades del dispositivo opcionales y distintos niveles de compatibilidad para esas funcionalidades; este nivel de variación entre un solo tipo de dispositivo de entrada es único entre los dispositivos compatibles con las API [Windows.Gaming.Input][]. Además, la mayoría de los dispositivos disponibles admiten como mínimo algunas de las funcionalidades opcionales u otras variaciones. Por este motivo, es importante determinar las funcionalidades de cada volante conectado por separado y admitir la variación completa de funcionalidades que tenga sentido para tu juego.

Para obtener más información, consulta [Determinar las funcionalidades del volante](#determining-racing-wheel-capabilities).

### <a name="force-feedback"></a>Fuerza de respuesta

Algunos volantes de Xbox One ofrecen fuerza de respuesta real; es decir, pueden aplicar fuerzas reales en el eje de control, como el volante, no tan solo vibración. Los juegos usan esta capacidad para crear una sensación de inmersión mayor (_daños de impacto simulado_, _"sensación de la carretera"_) y para incrementar el reto de conducir bien.

Para obtener más información, consulta [Información general sobre la fuerza de respuesta](#force-feedback-overview).

### <a name="ui-navigation"></a>Navegación de la interfaz de usuario

Para aliviar la carga de la compatibilidad con los diferentes dispositivos de entrada para la navegación de la interfaz de usuario y fomentar la coherencia entre dispositivos y juegos, la mayoría de dispositivos de entrada _física_ actúan simultáneamente como dispositivo independiente de entrada _lógica_, llamado [controlador de navegación de la interfaz de usuario](ui-navigation-controller.md). El controlador de navegación de la interfaz de usuario proporciona un vocabulario común para los comandos de navegación de la interfaz de usuario entre los dispositivos de entrada.

Debido a su enfoque único en los controles analógicos y en el grado de variación entre los distintos volantes, suelen estar equipados con una cruceta digital, y con los botones de **vista**, **menú**, **A**, **B**, **X** e **Y**, parecidos a los de un [controlador para juegos](gamepad-and-vibration.md); estos botones no están destinados a admitir comandos de juego y no se puede acceder a ellos tan fácilmente como a los botones del volante.

Como controladores de navegación de la interfaz de usuario, los volantes asignan el [conjunto necesario](ui-navigation-controller.md#required-set) de comandos de navegación al stick analógico izquierdo, la cruceta y los botones de **vista**, **menú**, **A** y **B**.

| Comando de navegación | Entrada del volante |
| ------------------:| ------------------ |
|                 Arriba | Cruceta hacia arriba           |
|               Abajo | Cruceta hacia abajo         |
|               Izquierda | Cruceta hacia la izquierda         |
|              Derecha | Cruceta hacia la derecha        |
|               Vista | Botón de vista        |
|               Menú | Botón de menú        |
|             Aceptar | Botón A           |
|             Cancelar | Botón B           |

Además, algunos volantes pueden asignar algún [conjunto opcional](ui-navigation-controller.md#optional-set) de comandos de navegación a otras entradas que admiten, pero las asignaciones de comandos pueden variar de un dispositivo a otro. Considera la posibilidad de admitir también estos comandos, pero asegúrate de que no son esenciales para navegar por la interfaz del juego.

| Comando de navegación | Entrada del volante    |
| ------------------:| --------------------- |
|            Re Pág | _varía_              |
|          Av Pág | _varía_              |
|          Página a la izquierda | _varía_              |
|         Página a la derecha | _varía_              |
|          Desplazar hacia arriba | _varía_              |
|        Desplazar hacia abajo | _varía_              |
|        Desplazar a la izquierda | _varía_              |
|       Desplazar a la derecha | _varía_              |
|          Contexto 1 | Botón X (_habitualmente_) |
|          Contexto 2 | Botón Y (_habitualmente_) |
|          Contexto 3 | _varía_              |
|          Contexto 4 | _varía_              |


## <a name="detect-and-track-racing-wheels"></a>Detectar y realizar un seguimiento de los volantes

El sistema administra los volantes, por lo que no tendrás que crearlos ni inicializarlos. El sistema proporciona una lista de volantes conectados y eventos para informarte cuando se agrega o quita un volante.

### <a name="the-racing-wheels-list"></a>Lista de volantes

La clase [RacingWheel][] proporciona una propiedad estática, [RacingWheels][], que es una lista de solo lectura de los volantes que están actualmente conectados. Dado que probablemente solo te interesen algunos de los volantes conectados, se recomienda mantener la propia colección en lugar de acceder a ellos a través de la propiedad `RacingWheels`.

En el siguiente ejemplo se copian todos los volantes conectados en una nueva colección.
```cpp
auto myRacingWheels = ref new Vector<RacingWheel^>();

for (auto racingwheel : RacingWheel::RacingWheels)
{
    // This code assumes that you're interested in all racing wheels.
    myRacingWheels->Append(racingwheel);
}
```

### <a name="adding-and-removing-racing-wheels"></a>Agregar y quitar volantes

Cuando se agrega o quita un volante, se generan los eventos [RacingWheelAdded][] y [RacingWheelRemoved][]. Puedes registrar controladores de estos eventos para realizar un seguimiento de los volantes que están conectados actualmente.

En el siguiente ejemplo se inicia el seguimiento de un volante que se ha agregado.
```cpp
RacingWheel::RacingWheelAdded += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    // This code assumes that you're interested in all new racing wheels.
    myRacingWheels->Append(args);
}
```

En el siguiente ejemplo se deja de realizar el seguimiento de un volante que se ha quitado.
```cpp
RacingWheel::RacingWheelRemoved += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    unsigned int indexRemoved;

    if(myRacingWheels->IndexOf(args, &indexRemoved))
    {
        myRacingWheels->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>Usuarios y auriculares

Cada volante puede asociarse con una cuenta de usuario para vincular su identidad al juego y puede tener conectados unos auriculares para facilitar el chat de voz o las funciones en el juego. Para obtener más información sobre cómo trabajar con usuarios y auriculares, consulta [Seguimiento de usuarios y sus dispositivos](input-practices-for-games.md#tracking-users-and-their-devices) y [Auriculares](headset.md).

## <a name="reading-the-racing-wheel"></a>Lectura del volante

Después de identificar los volantes que te interesan, puedes recopilar datos de ellos. Sin embargo, a diferencia de algunos otros tipos de entrada con los que puedes estar familiarizado, los volantes no comunican el cambio de estado mediante la generación de eventos. En su lugar, tienes que realizar lecturas regulares de su estado actual mediante _sondeos_.

### <a name="polling-the-racing-wheel"></a>Sondeo del volante

El sondeo captura una instantánea del volante en un momento preciso en el tiempo. Este enfoque de recopilación de entrada es una buena opción para la mayoría de los juegos porque su lógica normalmente se ejecuta en un bucle determinista en lugar de controlarse mediante eventos. También suele ser más sencillo interpretar los comandos de juego de la entrada recopilada de una vez que de muchas entradas individuales recopiladas a lo largo del tiempo.

El sondeo de un volante se realiza llamando a [GetCurrentReading][]; esta función devuelve una estructura [RacingWheelReading][] que contiene el estado del volante.

En el siguiente ejemplo se realiza el sondeo del estado actual de un volante.
```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

Además del estado del volante, cada lectura incluye una marca de tiempo que indica con precisión cuándo se recuperó el estado. La marca de tiempo es útil para establecer una relación con los intervalos de lecturas anteriores o la duración de la simulación de juego.

### <a name="determining-racing-wheel-capabilities"></a>Determinar las funcionalidades del volante

Muchos de los controles del volante son opcionales o admiten diferentes variaciones incluso en los controles necesarios, por lo que tienes que determinar individualmente las funcionalidades de cada volante para poder procesar la entrada que se recopila en cada lectura del volante.

Los controles opcionales son el freno de mano, el embrague y las palancas de cambios; puedes determinar si un volante conectado admite estos controles mediante la lectura de las propiedades [HasHandbrake][], [HasClutch][] y [HasPatternShifter][] del volante, respectivamente. El control se admite si el valor de la propiedad es **true**; de lo contrario, no se admite.

```cpp
if (racingwheel->HasHandbrake)
{
    // the handbrake is supported
}

if (racingwheel->HasClutch)
{
    // the clutch is supported
}

if (racingwheel->HasPatternShifter)
{
    // the pattern shifter is supported
}
```

Además, los controles que pueden variar son el volante y la palanca de cambios. El volante pueden variar según el grado de giro físico que puede admitir el volante real, mientras que la palanca de cambios puede variar según el número de velocidades que admita. Para determinar el máximo ángulo de rotación que admite el volante real, lee la propiedad `MaxWheelAngle` del volante; su valor es el ángulo físico máximo que se admite en grados positivos, igual al valor en grados negativos. Puedes determinar la máxima velocidad que admite la palanca de cambios mediante la lectura de la propiedad `MaxPatternShifterGear` del volante; su valor incluye la máxima velocidad admitida, es decir, si el valor es el 4, la palanca de cambios admite la marcha atrás, el punto muerto y las marchas primera, segunda, tercera y cuarta.

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

Por último, algunos volantes admiten la fuerza de respuesta a través del volante. Puedes determinar si un volante conectado admite la fuerza de respuesta mediante la lectura de la propiedad [WheelMotor][] del volante. La fuerza de respuesta se admite si `WheelMotor` no es **null**; de lo contrario, no se admite.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

Para obtener información sobre cómo usar la funcionalidad de fuerza de respuesta de los volantes que la admiten, consulta [Información general sobre la fuerza de respuesta](#force-feedback-overview).

### <a name="reading-the-buttons"></a>Lectura de los botones

Cada uno de los botones del volante (las cuatro direcciones de la cruceta, los botones **Previous Gear** y **Next Gear** y los 16 botones adicionales) proporcionan una lectura digital que indica si está presionado o sin presionar. Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales; en su lugar, se empaquetan todas en un único campo de bits que se representa mediante la enumeración [RacingWheelButtons][].

> **Nota**    Los volantes están equipados con botones adicionales que se usan para la navegación de la interfaz de usuario, como los botones de **vista** y **menú**. Estos botones no forman parte de la enumeración `RacingWheelButtons` y solo se pueden leer accediendo al volante como dispositivo de navegación de la interfaz de usuario. Para obtener más información, consulta [UI Navigation Device (Dispositivo de navegación de la interfaz de usuario)](ui-navigation-controller.md).

Los valores de los botones se leen en la propiedad `Buttons` de la estructura [RacingWheelReading][]. Dado que esta propiedad es un campo de bits, se usa el enmascaramiento bit a bit para aislar el valor del botón que te interesa. El botón está presionado (abajo) cuando se establece el bit correspondiente; de lo contrario, no lo está (arriba).

En el ejemplo siguiente, se determina si el botón **Next Gear** está presionado.
```cpp
if (RacingWheelButtons::NextGear == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is pressed
}
```

En el ejemplo siguiente, se determina si el botón Next Gear está liberado.
```cpp
if (RacingWheelButtons::None == (reading.Buttons & RacingWheelButtons::NextGear))
{
    // Next Gear is released (not pressed)
}
```

A veces, tal vez quieras determinar si se libera un botón que está presionado o si se presiona un botón que no lo estaba, si se presionan o liberan varios botones o si un conjunto de botones tiene una disposición determinada (algunos presionados y otros no). Para obtener información sobre cómo detectar estas condiciones, consulta [Detección de transiciones de botón](input-practices-for-games.md#detecting-button-transitions) y [Detección disposiciones de botones complejas](input-practices-for-games.md#detecting-complex-button-arrangements).

### <a name="reading-the-wheel"></a>Lectura del volante

El volante es un control necesario que proporciona una lectura analógica entre -1,0 y + 1,0. Un valor de -1,0 corresponde a la posición más a la izquierda del volante; un valor de + 1,0 corresponde a la posición más a la derecha. El valor del volante se lee en la propiedad `Wheel` de la estructura [RacingWheelReading][].

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

Aunque las lecturas del volante corresponden a los distintos grados de rotación física del volante real según el rango de rotación que admite el volante físico, lo habitual no es escalar las lecturas del volante; los volantes que admiten mayores grados de rotación simplemente proporcionan una mayor precisión.

### <a name="reading-the-throttle-and-brake"></a>Lectura del acelerador y el freno

El acelerador y el freno son controles necesarios, cada uno de los cuales proporciona lecturas analógicas entre 0,0 (totalmente liberado) y 1,0 (totalmente presionado), que se representan como valores de punto flotante. El valor del control del acelerador se lee en la propiedad `Throttle` de la estructura [RacingWheelReading][]; el valor del control del freno se lee en la propiedad `Brake`.

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>Lectura del freno de mano y el embrague

El freno de mano y el embrague son controles opcionales, cada uno de los cuales proporciona lecturas analógicas entre 0,0 (totalmente liberado) y 1,0 (totalmente accionado), que se representan como valores de punto flotante. El valor del control del freno de mano se lee en la propiedad `Handbrake` de la estructura [RacingWheelReading][]; el valor del control del embrague se lee en la propiedad `Clutch`.

```cpp
float handbrake = 0.0;
float clutch = 0.0;

if(racingwheel->HasHandbrake)
{
    handbrake = reading.Handbrake;  // returns a value between 0.0 and 1.0
}

if(racingwheel->HasClutch)
{
    clutch = reading.Clutch;        // returns a value between 0.0 and 1.0
}
```

### <a name="reading-the-pattern-shifter"></a>Lectura de la palanca de cambios

La palanca de cambios es un control opcional que proporciona una lectura digital entre -1 y [MaxPatternShifterGear][], que se representa como un valor entero con signo. Un valor de -1 o 0 corresponde a la _marcha atrás_ y el _punto muerto_, respectivamente; los valores más altos positivos corresponden a velocidades superiores hasta [MaxPatternShifterGear][], inclusive. El valor de la palanca de cambios se lee en la propiedad `PatternShifterGear` de la estructura [RacingWheelReading][].

```cpp
if(racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear
}
```

> **Nota**    La palanca de cambios, si se admite, se encuentra junto a los botones necesarios de marcha anterior y marcha siguiente que también afectan a la velocidad actual del vehículo del jugador. Una estrategia simple para unificar estas entradas en que ambos elementos están presentes es ignorar la palanca de cambios (y el embrague) cuando un jugador elige el cambio automático para su vehículo, o bien ignorar los botones de marcha anterior y marcha siguiente cuando un jugador elige el cambio manual si su volante está equipado con un control de palanca de cambios. Puedes implementar una estrategia de unificación diferente si esta no te resulta adecuada para el juego.

## <a name="run-the-inputinterfacing-sample"></a>Ejecución de la muestra de InputInterfacing

La [muestra InputInterfacingUWP _(GitHub)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) ilustra cómo usar los volantes y los distintos tipos de dispositivos de entrada conjuntamente, así como el comportamiento de estos dispositivos de entrada como controladores de navegación de la interfaz de usuario.

## <a name="force-feedback-overview"></a>Información general sobre la fuerza de respuesta

Muchos volantes tienen funcionalidad de fuerza de respuesta para proporcionar una experiencia de conducción más envolvente y estimulante. Los volantes que admiten la fuerza de respuesta suelen estar equipados con un motor único que aplica fuerza al volante a lo largo de un solo eje: el eje de rotación del volante. La fuerza de respuesta se admite en Windows 10 y en las aplicaciones para UWP de Xbox One a través del espacio de nombres [Windows.Gaming.Input.ForceFeedback][].

> **Nota**    Las API de fuerza de respuesta son capaces de admitir varios ejes de fuerza, pero ningún volante de Xbox One admite actualmente ningún eje de respuesta que no sea el de rotación del volante.

## <a name="using-force-feedback"></a>Uso de la fuerza de respuesta

En estas secciones se describen los conceptos básicos de programación de los efectos de la fuerza de respuesta para los volantes de Xbox One. La respuesta se aplica mediante efectos, que se cargan por primera vez en el dispositivo de fuerza de respuesta y, luego, se pueden iniciar, pausar, reanudar y detener de manera similar a los efectos de sonido; sin embargo, primero debes determinar las funcionalidades de respuesta del volante.

### <a name="determining-force-feedback-capabilities"></a>Determinar las funcionalidades de fuerza de respuesta

Puedes determinar si un volante conectado admite la fuerza de respuesta mediante la lectura de la propiedad [WheelMotor][] del volante. La fuerza de respuesta no se admite si `WheelMotor` es **null**; de lo contrario, la fuerza de respuesta se admite y puedes determinar las funcionalidades de respuesta específicas del motor, como los ejes a los que puede afectar.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    auto axes = racingwheel->WheelMotor->SupportedAxes;

    if(ForceFeedbackEffectAxes::X == (axes & ForceFeedbackEffectAxes::X))
    {
        // Force can be applied through the X axis
    }

    if(ForceFeedbackEffectAxes::Y == (axes & ForceFeedbackEffectAxes::Y))
    {
        // Force can be applied through the Y axis
    }

    if(ForceFeedbackEffectAxes::Z == (axes & ForceFeedbackEffectAxes::Z))
    {
        // Force can be applied through the Z axis
    }
}
```

### <a name="loading-force-feedback-effects"></a>Carga de los efectos de la fuerza de respuesta

Los efectos de la fuerza de respuesta se cargan en el dispositivo de respuesta donde se "reproducen" de forma autónoma en el comando del juego. Se incluyen varios efectos básicos; se pueden crear efectos personalizados a través de una clase que implementa la interfaz [IForceFeedbackEffect][].

| Clase de efecto         | Descripción del efecto                                                                     |
| -------------------- | -------------------------------------------------------------------------------------- |
| ConditionForceEffect | Efecto que aplica una fuerza variable en respuesta a un sensor actual dentro del dispositivo. |
| ConstantForceEffect  | Efecto que aplica una fuerza constante a lo largo de un vector.                                  |
| PeriodicForceEffect  | Efecto que aplica una fuerza variable definida por una forma de onda, a lo largo de un vector.           |
| RampForceEffect      | Efecto que aplica una fuerza linealmente creciente o decreciente a lo largo de un vector.          |


```cpp
using FFLoadEffectResult = ForceFeedback::ForceFeedbackLoadEffectResult;

auto effect = ref new Windows.Gaming::Input::ForceFeedback::ConstantForceEffect();
auto time = TimeSpan(10000);

effect->SetParameters(Windows::Foundation::Numerics::float3(1.0f, 0.0f, 0.0f), time);

// Here, we assume 'racingwheel' is valid and supports force feedback

IAsyncOperation<FFLoadEffectResult>^ request
    = racingwheel->WheelMotor->LoadEffectAsync(effect);

auto loadEffectTask = Concurrency::create_task(request);

loadEffectTask.then([this](FFLoadEffectResult result)
{
    if (FFLoadEffectResult::Succeeded == result)
    {
        // effect successfully loaded
    }
    else
    {
        // effect failed to load
    }
}).wait();
```

### <a name="using-force-feedback-effects"></a>Uso de los efectos de la fuerza de respuesta

Una vez cargados, los efectos se pueden iniciar, pausar, reanudar y detener de forma sincrónica llamando a funciones en la propiedad `WheelMotor` del volante, o bien individualmente llamando a las funciones del propio efecto de respuesta. Por lo general, se deben cargar todos los efectos que se quieren usar en el dispositivo de respuesta antes de que comience el juego y, luego, usar sus respectivas funciones `SetParameters` para actualizar los efectos a medida que el juego avanza.

```cpp
if (ForceFeedbackEffectState::Running == effect->State)
{
    effect->Stop();
}
else
{
    effect->Start();
}
```

Por último, se puede habilitar, deshabilitar o restablecer de forma asincrónica todo el sistema de fuerza de respuesta en un volante concreto cuando sea necesario.

## <a name="see-also"></a>Consulta también
[Windows.Gaming.Input.UINavigationController][]
[Windows.Gaming.Input.IGameController][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[racingwheels]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheels.aspx
[racingwheeladded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheeladded.aspx
[racingwheelremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.racingwheelremoved.aspx
[haspatternshifter]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.haspatternshifter.aspx
[hashandbrake]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hashandbrake.aspx
[hasclutch]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.hasclutch.aspx
[maxpatternshiftergear]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxpatternshiftergear.aspx
[maxwheelangle]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.maxwheelangle.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.getcurrentreading.aspx
[wheelmotor]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.wheelmotor.aspx
[racingwheelreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelreading.aspx
[racingwheelbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheelbuttons.aspx

