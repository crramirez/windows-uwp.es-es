---
title: Volante y fuerza de respuesta
description: Usa las API de volante Windows.Gaming.Input para detectar y leer comandos de fuerza de respuesta, para enviarlos a los volantes y para determinar sus funcionalidades.
ms.assetid: 6287D87F-6F2E-4B67-9E82-3D6E51CBAFF9
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10, UWP, juegos, rueda de carreras, forzar comentarios
ms.localizationpriority: medium
ms.openlocfilehash: 0d92fe45a008ee0c3864355faf8133ab8e1ebefb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163069"
---
# <a name="racing-wheel-and-force-feedback"></a>Volante y retroalimentación de fuerza

En esta página se describen los conceptos básicos de la programación de volantes de Xbox One con [Windows. Gaming. Input. RacingWheel][racingwheel] y las API relacionadas para el plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:

* cómo recopilar una lista de ruedas de carreras conectadas y sus usuarios
* Cómo detectar que se ha agregado o quitado una rueda de carreras
* Cómo leer datos de una o varias ruedas de carrera
* Cómo enviar comandos de fuerza de respuesta
* Cómo se comportan los volantes como dispositivos de navegación

## <a name="racing-wheel-overview"></a>Información general sobre los volantes

Los volantes son dispositivos de entrada cuya apariencia se parece mucho a la cabina de un coche de carreras real. Los volantes son el dispositivo de entrada perfecto para los juegos de carreras de estilo Arcade y simulación que representan coches o camiones. Los volantes son compatibles con aplicaciones para UWP de Windows 10 y Xbox One en el espacio de nombres [Windows.Gaming.Input](/uwp/api/windows.gaming.input).

Las ruedas de las carreras de Xbox One se ofrecen en una gran variedad de puntos de precios, por lo general, con una mayor y mejor funcionalidad de comentarios de entrada y fuerza a medida que aumentan sus puntos de precios. Todos los volantes están equipados con una rueda de gobierno analógica, controles de aceleración y frenos analógicos y algunos botones de rueda. Algunos volantes están también equipados con los controles HandBrake y de embrague analógicos, los desfasadors de patrón y las capacidades de fuerza de comentarios. No todos los volantes están equipados con los mismos conjuntos de características, y también pueden variar en su compatibilidad con determinadas características; &mdash; por ejemplo, las ruedas de gobierno pueden admitir diferentes intervalos de rotación y los separadores de patrones pueden admitir diferentes números de engranajes.

### <a name="device-capabilities"></a>Capacidades del dispositivo

Diferentes volantes de Xbox One ofrecen diferentes conjuntos de funcionalidades de dispositivo opcionales y distintos niveles de soporte técnico para esas capacidades. este nivel de variación entre un solo tipo de dispositivo de entrada es único entre los dispositivos admitidos por la API de [Windows. Gaming. Input](/uwp/api/windows.gaming.input) . Además, la mayoría de los dispositivos disponibles admiten como mínimo algunas de las funcionalidades opcionales u otras variaciones. Por este motivo, es importante determinar las capacidades de cada rueda de carreras conectada individualmente y admitir la variación completa de las funcionalidades que tiene sentido para su juego.

Para obtener más información, consulta [Determinar las funcionalidades del volante](#determining-racing-wheel-capabilities).

### <a name="force-feedback"></a>Fuerza de respuesta

Algunos volantes de Xbox One ofrecen verdaderas comentarios sobre &mdash; la fuerza, por lo que pueden aplicar fuerzas reales en un eje de control, por ejemplo, la rueda de gobierno, &mdash; no solo una vibración simple. Los juegos usan esta capacidad para crear una mayor sensación de inmersión (daños en los_bloqueos simulados_, "sensación de carreteras") y aumentar el reto de impulsar bien.

Para obtener más información, consulta [Información general sobre la fuerza de respuesta](#force-feedback-overview).

### <a name="ui-navigation"></a>Navegación de la interfaz de usuario

Para aliviar la carga de la compatibilidad con los diferentes dispositivos de entrada para la navegación de la interfaz de usuario y fomentar la coherencia entre dispositivos y juegos, la mayoría de dispositivos de entrada _física_ actúan simultáneamente como dispositivo independiente de entrada _lógica_, llamado [controlador de navegación de la interfaz de usuario](ui-navigation-controller.md). El controlador de navegación de la interfaz de usuario proporciona un vocabulario común para los comandos de navegación de la interfaz de usuario entre los dispositivos de entrada.

Debido a su focalización exclusiva en los controles analógicos y el grado de variación entre los diferentes volantes, normalmente están equipados con un teclado digital D, **vista**, **menú**, **a**, **B**, **X**e **y** que se parecen a los de un [controlador de juegos](gamepad-and-vibration.md). Estos botones no están diseñados para admitir comandos de juego y no se puede acceder a ellos fácilmente como botones de la rueda de carreras.

Como controlador de navegación de la interfaz de usuario, los volantes asignan el [conjunto necesario](ui-navigation-controller.md#required-set) de comandos de navegación al Stick izquierdo, el panel D, la **vista**, el **menú**, **el**y el botón **B** .

| Comando de navegación | Entrada del volante |
| ------------------:| ------------------ |
|                 Arriba | Cruceta hacia arriba           |
|               Bajar | Cruceta hacia abajo         |
|               Left | Cruceta hacia la izquierda         |
|              Right | Cruceta hacia la derecha        |
|               Ver | Botón de vista        |
|               Menú | Botón de menú        |
|             Accept | Botón A           |
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
|          Contexto 1 | Botón X (_normalmente_) |
|          Contexto 2 | Botón Y (_normalmente_) |
|          Contexto 3 | _varía_              |
|          Contexto 4 | _varía_              |

## <a name="detect-and-track-racing-wheels"></a>Detectar y realizar un seguimiento de los volantes

La detección y el seguimiento de los volantes funcionan exactamente de la misma manera que para los controladores de juegos, excepto con la clase [RacingWheel](/uwp/api/windows.gaming.input.racingwheel) en lugar de la clase de [controlador de juegos](/uwp/api/Windows.Gaming.Input.Gamepad) . Consulte [controlador de juegos y vibración](gamepad-and-vibration.md) para obtener más información.

<!-- Racing wheels are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected racing wheels and events to notify you when a racing wheel is added or removed.

### The racing wheels list

The [RacingWheel](/uwp/api/windows.gaming.input.racingwheel) class provides a static property, [RacingWheels](/uwp/api/windows.gaming.input.racingwheel.racingwheels#Windows_Gaming_Input_RacingWheel_RacingWheels), which is a read-only list of racing wheels that are currently connected. Because you might only be interested in some of the connected racing wheels, it's recommended that you maintain your own collection instead of accessing them through the `RacingWheels` property.

The following example copies all connected racing wheels into a new collection.
```cpp
auto myRacingWheels = ref new Vector<RacingWheel^>();

for (auto racingwheel : RacingWheel::RacingWheels)
{
    // This code assumes that you're interested in all racing wheels.
    myRacingWheels->Append(racingwheel);
}
```

### Adding and removing racing wheels

When a racing wheel is added or removed the [RacingWheelAdded](/uwp/api/windows.gaming.input.racingwheel.racingwheeladded) and [RacingWheelRemoved](/uwp/api/windows.gaming.input.racingwheel.racingwheelremoved) events are raised. You can register handlers for these events to keep track of the racing wheels that are currently connected.

The following example starts tracking an racing wheels that's been added.
```cpp
RacingWheel::RacingWheelAdded += ref new EventHandler<RacingWheel^>(Platform::Object^, RacingWheel^ args)
{
    // This code assumes that you're interested in all new racing wheels.
    myRacingWheels->Append(args);
}
```

The following example stops tracking a racing wheel that's been removed.
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

### Users and headsets

Each racing wheel can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-racing-wheel"></a>Lectura del volante

Después de identificar los volantes que le interesan, está listo para recopilar datos de los mismos. Sin embargo, a diferencia de algunos otros tipos de entrada con los que puedes estar familiarizado, los volantes no comunican el cambio de estado mediante la generación de eventos. En su lugar, puede realizar lecturas regulares de sus Estados actuales mediante su _sondeo_ .

### <a name="polling-the-racing-wheel"></a>Sondeo del volante

El sondeo captura una instantánea del volante en un momento preciso en el tiempo. Este enfoque para la recopilación de entradas es una buena opción para la mayoría de los juegos porque su lógica normalmente se ejecuta en un bucle determinista en lugar de ser controlado por eventos. Normalmente, es más fácil interpretar los comandos del juego a partir de la entrada recopilada todos a la vez que desde muchas entradas únicas recopiladas a lo largo del tiempo.

Sondear una rueda de carreras llamando a [GetCurrentReading](/uwp/api/windows.gaming.input.racingwheel.getcurrentreading#Windows_Gaming_Input_RacingWheel_GetCurrentReading); Esta función devuelve un [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) que contiene el estado de la rueda de carreras.

En el siguiente ejemplo se realiza el sondeo del estado actual de un volante.

```cpp
auto racingwheel = myRacingWheels[0];

RacingWheelReading reading = racingwheel->GetCurrentReading();
```

Además del estado del volante, cada lectura incluye una marca de tiempo que indica con precisión cuándo se recuperó el estado. La marca de tiempo es útil para establecer una relación con los intervalos de lecturas anteriores o la duración de la simulación de juego.

### <a name="determining-racing-wheel-capabilities"></a>Determinar las funcionalidades del volante

Muchos de los controles del volante son opcionales o admiten diferentes variaciones incluso en los controles necesarios, por lo que tienes que determinar individualmente las funcionalidades de cada volante para poder procesar la entrada que se recopila en cada lectura del volante.

Los controles opcionales son el freno de mano, el embrague y las palancas de cambios; puedes determinar si un volante conectado admite estos controles mediante la lectura de las propiedades [HasHandbrake](/uwp/api/windows.gaming.input.racingwheel.hashandbrake#Windows_Gaming_Input_RacingWheel_HasHandbrake), [HasClutch](/uwp/api/windows.gaming.input.racingwheel.hasclutch#Windows_Gaming_Input_RacingWheel_HasClutch) y [HasPatternShifter](/uwp/api/windows.gaming.input.racingwheel.haspatternshifter#Windows_Gaming_Input_RacingWheel_HasPatternShifter) del volante, respectivamente. Se admite el control si el valor de la propiedad es **true**; de lo contrario, no se admite.

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

Además, los controles que pueden variar son el volante y la palanca de cambios. El volante pueden variar según el grado de giro físico que puede admitir el volante real, mientras que la palanca de cambios puede variar según el número de velocidades que admita. Para determinar el máximo ángulo de rotación que admite el volante real, lee la propiedad `MaxWheelAngle` del volante; su valor es el ángulo físico máximo que se admite en grados positivos, igual al valor en grados negativos. Puede determinar el mayor engranaje de reenvío que admite el Desfasador de patrones leyendo la `MaxPatternShifterGear` propiedad de la rueda de carreras; su valor es el mayor soporte de avance compatible, ambos inclusive, &mdash; es decir, si su valor es 4, el Desfasador de patrones admite los engranajes invertido, neutro, primero, segundo, tercero y cuarto.

```cpp
auto maxWheelDegrees = racingwheel->MaxWheelAngle;
auto maxShifterGears = racingwheel->MaxPatternShifterGear;
```

Por último, algunos volantes admiten la fuerza de respuesta a través del volante. Puedes determinar si un volante conectado admite la fuerza de respuesta mediante la lectura de la propiedad [WheelMotor](/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) del volante. Se admite Force Feedback si `WheelMotor` no es **null**; de lo contrario, no se admite.

```cpp
if (racingwheel->WheelMotor != nullptr)
{
    // force feedback is supported
}
```

Para obtener información sobre cómo usar la funcionalidad de fuerza de respuesta de los volantes que la admiten, consulta [Información general sobre la fuerza de respuesta](#force-feedback-overview).

### <a name="reading-the-buttons"></a>Lectura de los botones

Cada uno de los botones de la rueda &mdash; de carreras las cuatro direcciones del panel D, los botones de **engranaje anterior** y **siguiente** , y 16 botones adicionales &mdash; proporcionan una lectura digital que indica si está presionado (inactivo) o liberado (arriba). Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales; en su lugar, se empaquetan todas en un único campo de bits que se representa mediante la enumeración [RacingWheelButtons](/uwp/api/windows.gaming.input.racingwheelbuttons).

> [!NOTE]
> Los volantes están equipados con botones adicionales que se usan para la navegación de la interfaz de usuario, como los botones de **vista** y de **menú** . Estos botones no forman parte de la enumeración `RacingWheelButtons` y solo se pueden leer accediendo al volante como dispositivo de navegación de la interfaz de usuario. Para obtener más información, consulta [UI Navigation Device (Dispositivo de navegación de la interfaz de usuario)](ui-navigation-controller.md).

Los valores de los botones se leen en la propiedad `Buttons` de la estructura [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading). Dado que esta propiedad es un campo de bits, se usa el enmascaramiento bit a bit para aislar el valor del botón que te interesa. El botón está presionado (abajo) cuando se establece el bit correspondiente; de lo contrario, se libera (up).

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

En ocasiones, es posible que desee determinar si un botón cambia de presionado a liberado o liberado a presionado, si se presionan o se liberan varios botones, o si un conjunto de botones se organizan de una manera determinada &mdash; , pero no. Para obtener información sobre cómo detectar estas condiciones, consulta [Detección de transiciones de botón](input-practices-for-games.md#detecting-button-transitions) y [Detección disposiciones de botones complejas](input-practices-for-games.md#detecting-complex-button-arrangements).

### <a name="reading-the-wheel"></a>Lectura del volante

El volante es un control necesario que proporciona una lectura analógica entre -1,0 y + 1,0. Un valor de -1,0 corresponde a la posición más a la izquierda del volante; un valor de + 1,0 corresponde a la posición más a la derecha. El valor del volante se lee en la propiedad `Wheel` de la estructura [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading).

```cpp
float wheel = reading.Wheel;  // returns a value between -1.0 and +1.0.
```

Aunque las lecturas del volante corresponden a los distintos grados de rotación física del volante real según el rango de rotación que admite el volante físico, lo habitual no es escalar las lecturas del volante; los volantes que admiten mayores grados de rotación simplemente proporcionan una mayor precisión.

### <a name="reading-the-throttle-and-brake"></a>Lectura del acelerador y el freno

El acelerador y el freno son controles obligatorios, cada uno de los cuales proporciona lecturas analógicas entre 0,0 (completamente liberado) y 1,0 (totalmente presionado) representados como valores de punto flotante. El valor del control del acelerador se lee en la propiedad `Throttle` de la estructura [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading); el valor del control del freno se lee en la propiedad `Brake`.

```cpp
float throttle = reading.Throttle;  // returns a value between 0.0 and 1.0
float brake    = reading.Brake;     // returns a value between 0.0 and 1.0
```

### <a name="reading-the-handbrake-and-clutch"></a>Lectura del freno de mano y el embrague

El freno de mano y el embrague son controles opcionales, cada uno de los cuales proporciona lecturas analógicas entre 0,0 (totalmente liberado) y 1,0 (totalmente accionado), que se representan como valores de punto flotante. El valor del control del freno de mano se lee en la propiedad `Handbrake` de la estructura [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading); el valor del control del embrague se lee en la propiedad `Clutch`.

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

La palanca de cambios es un control opcional que proporciona una lectura digital entre -1 y [MaxPatternShifterGear](/uwp/api/windows.gaming.input.racingwheel.maxpatternshiftergear), que se representa como un valor entero con signo. Un valor de-1 o 0 corresponde a los engranajes _inversos_ y _neutros_ , respectivamente; los valores cada vez más positivos se corresponden con un mayor avance hasta **MaxPatternShifterGear**, ambos inclusive. El valor del Desfasador de patrón se lee de la propiedad [PatternShifterGear](/uwp/api/windows.gaming.input.racingwheelreading.patternshiftergear) del struct [RacingWheelReading](/uwp/api/windows.gaming.input.racingwheelreading) .

```cpp
if (racingwheel->HasPatternShifter)
{
    gear = reading.PatternShifterGear;
}
```

> [!NOTE]
> El Desfasador de patrones, donde se admite, existe junto a los botones de **engranaje** y **siguiente** engranaje necesarios, que también afectan al engranaje actual del coche del jugador. Una estrategia sencilla para unificar estas entradas cuando ambos están presentes es pasar por alto el Desfasador de patrón (y el embrague) cuando un jugador elige una transmisión automática para su coche y omitir los botones de **engranaje** **anterior** y siguiente cuando un jugador elige una transmisión manual para su automóvil solo si su rueda de carreras está equipada con un control de Desfasador de patrón. Puedes implementar una estrategia de unificación diferente si esta no te resulta adecuada para el juego.

## <a name="run-the-inputinterfacing-sample"></a>Ejecución de la muestra de InputInterfacing

La [muestra InputInterfacingUWP _(GitHub)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) ilustra cómo usar los volantes y los distintos tipos de dispositivos de entrada conjuntamente, así como el comportamiento de estos dispositivos de entrada como controladores de navegación de la interfaz de usuario.

## <a name="force-feedback-overview"></a>Información general sobre la fuerza de respuesta

Muchos volantes tienen capacidad de fuerza de respuesta para proporcionar una experiencia de conducción más envolvente y desafiante. Los volantes que admiten la fuerza de respuesta suelen estar equipados con un motor único que aplica fuerza al volante a lo largo de un solo eje: el eje de rotación del volante. Los comentarios de fuerza se admiten en aplicaciones de UWP de Windows 10 y Xbox One mediante el espacio de nombres [Windows. Gaming. Input. ForceFeedback](/uwp/api/windows.gaming.input.forcefeedback) .

> [!NOTE]
> Las API de Force Feedback son capaces de admitir varios ejes de fuerza, pero ninguna rueda de carreras de Xbox One es compatible actualmente con ningún eje de comentarios que no sea el de rotación de la rueda.

## <a name="using-force-feedback"></a>Uso de la fuerza de respuesta

En estas secciones se describen los conceptos básicos de programación de los efectos de la fuerza de respuesta para los volantes de Xbox One. La respuesta se aplica mediante efectos, que se cargan por primera vez en el dispositivo de fuerza de respuesta y, luego, se pueden iniciar, pausar, reanudar y detener de manera similar a los efectos de sonido; sin embargo, primero debes determinar las funcionalidades de respuesta del volante.

### <a name="determining-force-feedback-capabilities"></a>Determinar las funcionalidades de fuerza de respuesta

Puedes determinar si un volante conectado admite la fuerza de respuesta mediante la lectura de la propiedad [WheelMotor](/uwp/api/windows.gaming.input.racingwheel.wheelmotor#Windows_Gaming_Input_RacingWheel_WheelMotor) del volante. Los comentarios de fuerza no se admiten si `WheelMotor` es **null**; de lo contrario, se admite la fuerza de comentarios y puede continuar para determinar las capacidades de comentarios específicas del motor, como los ejes a los que puede afectar.

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

Los efectos de la fuerza de respuesta se cargan en el dispositivo de respuesta donde se "reproducen" de forma autónoma en el comando del juego. Se proporcionan varios efectos básicos; los efectos personalizados se pueden crear a través de una clase que implementa la interfaz [IForceFeedbackEffect](/uwp/api/windows.gaming.input.forcefeedback.iforcefeedbackeffect) .

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

## <a name="see-also"></a>Vea también

* [Windows.Gaming.Input.UINavigationController](/uwp/api/windows.gaming.input.uinavigationcontroller)
* [Windows.Gaming.Input.IGameController](/uwp/api/windows.gaming.input.igamecontroller)
* [Procedimientos de entrada para juegos](input-practices-for-games.md)

[Windows.Gaming.Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[racingwheel]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheels]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheeladded]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheelremoved]: /uwp/api/Windows.Gaming.Input.RacingWheel
[haspatternshifter]: /uwp/api/Windows.Gaming.Input.RacingWheel
[hashandbrake]: /uwp/api/Windows.Gaming.Input.RacingWheel
[hasclutch]: /uwp/api/Windows.Gaming.Input.RacingWheel
[maxpatternshiftergear]: /uwp/api/Windows.Gaming.Input.RacingWheel
[maxwheelangle]: /uwp/api/Windows.Gaming.Input.RacingWheel
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.RacingWheel
[wheelmotor]: /uwp/api/Windows.Gaming.Input.RacingWheel
[racingwheelreading]: /uwp/api/Windows.Gaming.Input.RacingWheelReading
[racingwheelbuttons]: /uwp/api/Windows.Gaming.Input.RacingWheelButtons