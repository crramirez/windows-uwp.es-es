---
author: eliotcowley
title: Controlador de navegación de la interfaz de usuario
description: Usa las API de controlador de navegación de la interfaz de usuario Windows.Gaming.Input para detectar y leer los distintos tipos de dispositivos de entrada destinados a la navegación de la interfaz de usuario.
ms.assetid: 5A14926D-8C2E-4DE8-AAFB-BEEB9BFE91A5
ms.author: elcowle
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, interfaz de usuario, navegación
ms.localizationpriority: medium
ms.openlocfilehash: a0ec2790f6dddf93959a8c826602c0ac622a7d23
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6665950"
---
# <a name="ui-navigation-controller"></a>Controlador de navegación de la interfaz de usuario

En esta página se describen los conceptos básicos de programación para dispositivos de navegación de la interfaz de usuario mediante la clase [Windows.Gaming.Input.UINavigationController][uinavigationcontroller] y las API relacionadas para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:
* Cómo obtener una lista de los dispositivos de navegación de la interfaz de usuario conectados y sus usuarios
* Cómo detectar que se ha agregado o quitado un dispositivo de navegación
* Cómo leer la entrada desde uno o varios dispositivos de navegación de la interfaz de usuario
* Cómo se comportan los controladores para juegos y los sticks arcade como dispositivos de navegación

## <a name="ui-navigation-controller-overview"></a>Información general sobre controladores de navegación de la interfaz de usuario

Casi todos los juegos tienen al menos alguna interfaz de usuario que es independiente del juego, aunque se trate simplemente de menús previos al juego o cuadros de diálogo en el juego. Los jugadores deben poder navegar por la interfaz de usuario con cualquier dispositivo de entrada que hayan elegido, pero esto resulta abrumador para los desarrolladores, ya que deben agregar compatibilidad específica para cada tipo de dispositivo de entrada y, además, puede producir incoherencias entre los juegos y los dispositivos de entrada que confunden a los jugadores. Por estas razones, se creó la API [UINavigationController][].

Los controladores de navegación de la interfaz de usuario son dispositivos de entrada _lógicos_ que existen para proporcionar un vocabulario general de comandos de navegación de la interfaz de usuario que puede ser compatible con diversos dispositivos de entrada _físicos_. Un _controlador de navegación de la interfaz de usuario_ es, simplemente, otra forma de denominar un dispositivo de entrada físico. Usamos el término _dispositivo de navegación_ para hacer referencia a cualquier dispositivo de entrada físico que se considera un controlador de navegación. Al programar con un dispositivo de navegación en lugar de dispositivos de entrada específicos, los desarrolladores evitan la carga de tener que agregar compatibilidad para diferentes dispositivos de entrada y logran uniformidad de manera predeterminada.

Dado que el número y la diversidad de controles compatibles con cada tipo de dispositivo de entrada pueden diferir mucho y que determinados dispositivos de entrada podrían querer admitir un conjunto más amplio de comandos de navegación, la interfaz de controladores de navegación divide el vocabulario de comandos en un _conjunto necesario_, que contiene los comandos más habituales y esenciales, y un _conjunto opcional_, que contiene comandos convenientes, pero no esenciales. Todos los dispositivos de navegación admiten todos los comandos del _conjunto necesario_ y pueden admitir la totalidad, algunos o ninguno de los comandos del _conjunto opcional_.

### <a name="required-set"></a>Conjunto necesario

Los dispositivos de navegación deben admitir todos los comandos de navegación del _conjunto necesario_. Se trata de los comandos direccionales (arriba, abajo, izquierda y derecha) y los comandos ver, menú, aceptar y cancelar.

Los comandos direccionales están destinados a la [navegación con foco XY](../design/devices/designing-for-tv.md#xy-focus-navigation-and-interaction) principal entre elementos únicos de la interfaz de usuario. Los comandos "ver" y "menú" están pensados para mostrar información de juego (suelen ser momentáneos, a veces de forma modal) y para cambiar entre los contextos de juego y menú, respectivamente. Los comandos "aceptar" y "cancelar" están destinados a las respuestas afirmativa (sí) y negativa (no), respectivamente.

En la siguiente tabla se muestra un resumen de estos comandos y sus usos previstos, con ejemplos.
| Comando | Uso previsto
| -------:| ---------------
|      Arriba | Navegación hacia arriba con foco XY
|    Abajo | Navegación hacia abajo con foco XY
|    Izquierda | Navegación hacia la izquierda con foco XY
|   Derecha | Navegación hacia la derecha con foco XY
|    Ver | Mostrar información de juego _(marcador, estadísticas de juegos, objetivos, mapa mundial o del área)_
|    Menú | Menú principal/pausa _(configuración, estado, equipos, inventario, pausa)_
|  Aceptar | Respuesta afirmativa _(aceptar, avanzar, confirmar, iniciar, sí)_
|  Cancelar | Respuesta negativa _(rechazar, revertir, rechazar, detener, no)_


### <a name="optional-set"></a>Conjunto opcional

Es posible que los dispositivos de navegación también admitan la totalidad, algunos o ninguno de los comandos de navegación del _conjunto opcional_. Se trata de comandos de paginación (arriba, abajo, izquierda y derecha), desplazamiento (arriba, abajo, izquierda y derecha) y contextuales (contexto de 1 a 4).

Los comandos contextuales están explícitamente pensados para los comandos específicos de la aplicación y los accesos directos de navegación. Los comandos de paginación y desplazamiento están destinados a navegar rápidamente entre páginas o grupos de elementos de la interfaz de usuario y para la navegación específica dentro de los elementos de la interfaz de usuario, respectivamente.

En la siguiente tabla se muestra un resumen de estos comandos y sus usos previstos.
|     Comando | Uso previsto
| -----------:| ------------
|      PageUp | Saltar hacia arriba (a la página o el grupo vertical superior o anterior)
|    PageDown | Saltar hacia abajo (a la página o el grupo vertical inferior o siguiente)
|    PageLeft | Saltar hacia la izquierda (a la página o el grupo horizontal a la izquierda o anterior)
|   PageRight | Saltar hacia la derecha (a la página o el grupo horizontal a la derecha o siguiente)
|    ScrollUp | Desplazar hacia arriba (dentro del elemento de interfaz de usuario o el grupo desplazable con foco)
|  ScrollDown | Desplazar hacia abajo (dentro del elemento de interfaz de usuario o el grupo desplazable con foco)
|  ScrollLeft | Desplazar hacia la izquierda (dentro del elemento de interfaz de usuario o el grupo desplazable con foco)
| ScrollRight | Desplazar hacia la derecha (dentro del elemento de interfaz de usuario o el grupo desplazable con foco)
|    Context1 | Acción de contexto principal
|    Context2 | Acción de contexto secundaria
|    Context3 | Tercera acción de contexto
|    Context4 | Cuarta acción de contexto

> **Nota**: Aunque un juego tiene la posibilidad de responder a cualquier comando con una función real que sea diferente de la que está destinado a usar, debe evitarse el comportamiento sorpresivo. En concreto, no cambies la función real de un comando si necesitas su uso previsto. Trata de asignar funciones nuevas para el comando que sea más acorde a esas funciones y asigna funciones equivalentes a comandos equivalentes como PageUp o PageDown. Por último, considera qué comandos son compatibles con cada tipo de dispositivo de entrada y a qué controles están asignados, para así asegurarte de que los comandos críticos sean accesibles desde todos los dispositivos.

## <a name="gamepad-arcade-stick-and-racing-wheel-navigation"></a>Navegación mediante controlador para juegos, stick arcade y volante

Todos los dispositivos de entrada admitidos por el espacio de nombres Windows.Gaming.Input son dispositivos de navegación de la interfaz de usuario.

En la siguiente tabla se resume cómo el _conjunto necesario_ de comandos de navegación se asigna a diversos dispositivos de entrada.

| Comando de navegación | Entrada del controlador para juegos                       | Entrada del stick arcade | Entrada del volante |
| ------------------:| ----------------------------------- | ------------------ | ------------------ |
|                 Arriba | Stick analógico izquierdo hacia arriba/cruceta hacia arriba       | Stick hacia arriba           | Cruceta hacia arriba           |
|               Abajo | Stick analógico izquierdo hacia abajo/cruceta hacia abajo   | Stick hacia abajo         | Cruceta hacia abajo         |
|               Izquierda | Stick analógico izquierdo hacia la izquierda/cruceta hacia la izquierda   | Stick hacia la izquierda         | Cruceta hacia la izquierda         |
|              Derecha | Stick analógico izquierdo hacia la derecha/cruceta hacia la derecha | Stick hacia la derecha        | Cruceta hacia la derecha        |
|               Ver | Botón de vista                         | Botón de vista        | Botón de vista        |
|               Menú | Botón de menú                         | Botón de menú        | Botón de menú        |
|             Aceptar | Botón A                            | Botón de acción 1    | Botón A           |
|             Cancelar | Botón B                            | Botón de acción 2    | Botón B           |

En la siguiente tabla se resume cómo el _conjunto opcional_ de comandos de navegación se asigna a diversos dispositivos de entrada.

| Comando de navegación | Entrada del controlador para juegos          | Entrada del stick arcade | Entrada del volante    |
| ------------------:| ---------------------- | ------------------ | --------------------- |
|             PageUp | Gatillo izquierdo           | _no compatible_    | _varía_              |
|           PageDown | Gatillo derecho          | _no compatible_    | _varía_              |
|           PageLeft | Botón superior izquierdo            | _no compatible_    | _varía_              |
|          PageRight | Botón superior derecho           | _no compatible_    | _varía_              |
|           ScrollUp | Stick analógico derecho hacia arriba    | _no compatible_    | _varía_              |
|         ScrollDown | Stick analógico derecho hacia abajo  | _no compatible_    | _varía_              |
|         ScrollLeft | Stick analógico derecho hacia la izquierda  | _no compatible_    | _varía_              |
|        ScrollRight | Stick analógico derecho hacia la derecha | _no compatible_    | _varía_              |
|           Context1 | Botón X               | _no compatible_    | Botón X (_generalmente_) |
|           Context2 | Botón Y               | _no compatible_    | Botón Y (_generalmente_) |
|           Context3 | Presión del stick analógico izquierdo  | _no compatible_    | _varía_              |
|           Context4 | Presión del stick analógico derecho | _no compatible_    | _varía_              |


## <a name="detect-and-track-ui-navigation-controllers"></a>Detectar y realizar un seguimiento de los controladores de navegación de la interfaz de usuario

Aunque los controladores de navegación de la interfaz de usuario son dispositivos de entrada lógicos, son una representación de un dispositivo físico y el sistema los administra de la misma forma. No tienes que crearlos o inicializarlos, ya que el sistema proporciona una lista de eventos y controladores de navegación de la interfaz de usuario conectados para notificarte cuando se agrega o se quita un controlador de navegación de la interfaz de usuario.

### <a name="the-ui-navigation-controllers-list"></a>Lista de controladores de navegación de la interfaz de usuario

La clase [UINavigationController][] proporciona una propiedad estática, [UINavigationControllers][], que es una lista de solo lectura de los dispositivos de navegación de la interfaz de usuario que están actualmente conectados. Dado que, probablemente, solo te interesen algunos de los dispositivos conectados, es conveniente mantener tu propia colección en lugar de acceder a ellos a través de la propiedad `UINavigationControllers`.

En el siguiente ejemplo, se copian todos los controladores de navegación de la interfaz de usuario conectados a una nueva colección.
```cpp
auto myNavigationControllers = ref new Vector<UINavigationController^>();

for (auto device : UINavigationController::UINavigationControllers)
{
    // This code assumes that you're interested in all navigation controllers.
    myNavigationControllers->Append(device);
}
```

### <a name="adding-and-removing-ui-navigation-controllers"></a>Agregar y quitar controladores de navegación de la interfaz de usuario

Cuando se agrega o se quita un controlador de navegación de la interfaz de usuario, se generan los eventos [UINavigationControllerAdded][] y [UINavigationControllerRemoved][]. Puedes registrar un controlador de eventos para estos eventos con el fin de hacer un seguimiento de los dispositivos de navegación que están actualmente conectados.

En el siguiente ejemplo, se inicia el seguimiento de un dispositivo de navegación de la interfaz de usuario que se ha agregado.
```cpp
UINavigationController::UINavigationControllerAdded += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    // This code assumes that you're interested in all new navigation controllers.
    myNavigationControllers->Append(args);
}
```

En el siguiente ejemplo, se detiene el seguimiento de un stick arcade que se ha quitado.
```cpp
UINavigationController::UINavigationControllerRemoved += ref new EventHandler<UINavigationController^>(Platform::Object^, UINavigationController^ args)
{
    unsigned int indexRemoved;

    if(myNavigationControllers->IndexOf(args, &indexRemoved))
    {
        myNavigationControllers->RemoveAt(indexRemoved);
    }
}
```

### <a name="users-and-headsets"></a>Usuarios y auriculares

Cada dispositivo de navegación puede asociarse con una cuenta de usuario para vincular su identidad a su entrada y puede tener conectados unos auriculares para facilitar las características de navegación o chat de voz. Para obtener más información sobre cómo trabajar con usuarios y auriculares, consulta [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) (Seguimiento de usuarios y sus dispositivos) y [Headset](headset.md) (Auriculares).

## <a name="reading-the-ui-navigation-controller"></a>Leer el controlador de navegación de la interfaz de usuario

Después de identificar el dispositivo de navegación de la interfaz de usuario que te interesa, puedes recopilar su entrada. Sin embargo, a diferencia de algunos otros tipos de entrada con los que puedas estar familiarizado, los dispositivos de navegación no comunican el cambio de estado mediante la generación de eventos. En cambio, tienes que realizar lecturas regulares de su estado actual mediante _sondeos_.

### <a name="polling-the-ui-navigation-controller"></a>Sondear el controlador de navegación de la interfaz de usuario

El sondeo captura una instantánea del dispositivo de navegación en un momento preciso en el tiempo. Este enfoque de recopilación de entrada es una buena opción para la mayoría de los juegos porque su lógica, normalmente, se ejecuta en un bucle determinista en lugar de controlarse mediante eventos. También suele ser más sencillo interpretar los comandos de juego de la entrada recopilada de una vez que de muchas entradas individuales recopiladas a lo largo del tiempo.

El sondeo de un dispositivo de navegación se realiza mediante una llamada a [UINavigationController.GetCurrentReading][getcurrentreading]. Esta función devuelve una estructura [UINavigationReading][] que contiene el estado del dispositivo de navegación.

```cpp
auto navigationController = myNavigationControllers[0];

UINavigationReading reading = navigationController->GetCurrentReading();
```

### <a name="reading-the-buttons"></a>Lectura de los botones

Cada uno de los botones de navegación de la interfaz de usuario proporciona una lectura booleana que corresponde a si se lo presiona (abajo) o libera (arriba). Por motivos de eficacia, las lecturas de botones no se representan como valores booleanos individuales, sino que todos se incorporan en uno de los dos campos de bits representados por las enumeraciones [RequiredUINavigationButtons][] y [OptionalUINavigationButtons][].

Los botones pertenecientes al _conjunto necesario_ se leen desde la propiedad `RequiredButtons` de la estructura [UINavigationReading][] y los botones pertenecientes al _conjunto opcional_ se leen desde la propiedad `OptionalButtons`. Dado que estas propiedades son campos de bits, se usa el enmascaramiento bit a bit para aislar el valor del botón que te interesa. El botón está presionado (abajo) cuando se establece el bit correspondiente; de lo contrario, no lo está (arriba).

En el ejemplo siguiente, se determina si el botón Aceptar del _conjunto necesario_ está presionado.
```cpp
if (RequiredUINavigationButtons::Accept == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is pressed
}
```

En el ejemplo siguiente, se determina si el botón Aceptar del _conjunto necesario_ está liberado.
```cpp
if (RequiredUINavigationButtons::None == (reading.RequiredButtons & RequiredUINavigationButtons::Accept))
{
    // Accept is released (not pressed)
}
```

Asegúrate de usar la propiedad `OptionalButtons` y la enumeración `OptionalUINavigationButtons` al leer botones del _conjunto opcional_.

En el ejemplo siguiente, se determina si el botón Contexto 1 del _conjunto opcional_ está presionado.
```cpp
if (OptionalUINavigationButtons::Context1 == (reading.OptionalButtons & OptionalUINavigationButtons::Context1))
{
    // Context 1 is pressed
}
```

A veces, tal vez quieras determinar si se libera un botón que está presionado o si se presiona un botón que no lo estaba, si se presionan o liberan varios botones o si un conjunto de botones tiene una disposición determinada (algunos presionados y otros no). Para obtener información sobre cómo detectar estas condiciones, consulta [Detección de transiciones de botón](input-practices-for-games.md#detecting-button-transitions) y [Detección disposiciones de botones complejas](input-practices-for-games.md#detecting-complex-button-arrangements).


## <a name="run-the-ui-navigation-controller-sample"></a>Ejecutar la muestra de controlador de navegación de la interfaz de usuario

La [muestra InputInterfacingUWP _(GitHub)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/InputInterfacingUWP) evidencia cómo los distintos dispositivos de entrada se comportan como controladores de navegación de la interfaz de usuario.

## <a name="see-also"></a>Consulta también
[Windows.Gaming.Input.Gamepad][]
[Windows.Gaming.Input.ArcadeStick][]
[Windows.Gaming.Input.RacingWheel][]
[Windows.Gaming.Input.IGameController][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.Gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[Windows.Gaming.Input.Arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[Windows.Gaming.Input.Racingwheel]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.racingwheel.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[uinavigationcontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[uinavigationcontrollers]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollers.aspx
[uinavigationcontrolleradded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrolleradded.aspx
[uinavigationcontrollerremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.uinavigationcontrollerremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.getcurrentreading.aspx
[uinavigationreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationreading.aspx
[requireduinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.requireduinavigationbuttons.aspx
[optionaluinavigationbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.optionaluinavigationbuttons.aspx
