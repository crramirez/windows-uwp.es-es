---
title: Stick arcade
description: Usa las API de stick arcade Windows.Gaming.Input para detectar y leer los sticks arcade.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, Stick Arcade, entrada
ms.localizationpriority: medium
ms.openlocfilehash: e9a9a2b3622169b99a1b3a0c3607329c85c5785f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159419"
---
# <a name="arcade-stick"></a>Stick arcade

En esta página se describen los conceptos básicos de programación para sticks arcade de Xbox One usando [Windows.Gaming.Input.ArcadeStick][arcadestick] y las API relacionadas para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:

* Cómo obtener una lista de sticks arcade conectados y sus usuarios
* Cómo detectar que se ha agregado o quitado un stick arcade
* Cómo leer la entrada de uno o más sticks arcade
* Cómo se comportan los sticks Arcade como dispositivos de navegación de IU

## <a name="arcade-stick-overview"></a>Información general sobre el stick arcade

Los sticks arcade son dispositivos de entrada apreciados por reproducir la sensación de las máquinas arcade de pie y por sus controles de alta precisión digital. Los sticks arcade son el dispositivo de entrada perfecto para los combates cara a acara y otros juegos de estilo arcade, además de ser adecuados para cualquier juego que funcione con controles totalmente digitales. Los sticks arcade son compatibles con aplicaciones para UWP de Windows 10 y Xbox One en el espacio de nombres [Windows.Gaming.Input][].

Los sticks arcade de Xbox One están equipados con un joystick digital de 8 vías, seis botones de **acción** (representados como a1-A6 en la imagen siguiente) y dos botones **especiales** (representados como S1 y S2); todos son dispositivos de entrada digitales que no admiten controles analógicos ni vibraciones. Los sticks arcade de Xbox One también están equipados con botones de **vista** y **menú** que se usan para admitir la navegación de la interfaz de usuario, pero no están diseñados para admitir comandos de juegos y no se puede acceder a ellos fácilmente como botones del joystick.

![Stick Arcade con joystick 4 direccional, 6 botones de acción (a1-A6) y 2 botones especiales (S1 y S2)](images/arcade-stick-1.png)

### <a name="ui-navigation"></a>Navegación de la interfaz de usuario

Para aliviar la carga de la compatibilidad con muchos dispositivos de entrada diferentes para la navegación de la interfaz de usuario y fomentar la coherencia entre dispositivos y juegos, la mayoría de dispositivos de entrada _física_ actúan simultáneamente como dispositivo independiente de entrada _lógica_, llamado [controlador de navegación de la interfaz de usuario](ui-navigation-controller.md). El controlador de navegación de la interfaz de usuario proporciona un vocabulario común para los comandos de navegación de la interfaz de usuario entre los dispositivos de entrada.

Como controlador de navegación de la interfaz de usuario, los sticks Arcade asignan el [conjunto necesario](ui-navigation-controller.md#required-set) de comandos de navegación a los botones joystick y **vista**, **menú**, **acción 1**y **acción 2** .

| Comando de navegación | Entrada del stick arcade  |
| ------------------:| ------------------- |
|                 Arriba | Stick hacia arriba            |
|               Bajar | Stick hacia abajo          |
|               Left | Stick hacia la izquierda          |
|              Right | Stick hacia la derecha         |
|               Ver | Botón de vista         |
|               Menú | Botón de menú         |
|             Accept | Botón de acción 1     |
|             Cancelar | Botón de acción 2     |

Los sticks arcade no se asignan a ninguno de los comandos del [conjunto opcional](ui-navigation-controller.md#optional-set) de navegación.

## <a name="detect-and-track-arcade-sticks"></a>Detección y seguimiento de los sticks arcade

La detección y el seguimiento de los sticks Arcade funcionan exactamente del mismo modo que para los controladores de juegos, excepto con la clase [ArcadeStick][] en lugar de la clase de [controlador de juegos](/uwp/api/Windows.Gaming.Input.Gamepad) . Consulte [controlador de juegos y vibración](gamepad-and-vibration.md) para obtener más información.

<!-- Arcade sticks are managed by the system, therefore you don't have to create or initialize them. The system provides a list of connected arcades sticks and events to notify you when an arcade stick is added or removed.

### The arcade sticks list

The [ArcadeStick][] class provides a static property, [ArcadeSticks][], which is a read-only list of arcade sticks that are currently connected. Because you might only be interested in some of the connected arcade sticks, it's recommended that you maintain your own collection instead of accessing them through the `ArcadeSticks` property.

The following example copies all connected arcade sticks into a new collection. Note that because other threads in the background will be accessing this collection (in the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events), you need to place a lock around any code that reads or updates the collection.

```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();
critical_section myLock{};

for (auto arcadeStick : ArcadeStick::ArcadeSticks)
{
    // Check if the arcade stick is already in myArcadeSticks; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myArcadeSticks), end(myArcadeSticks), arcadeStick);

    if (it == end(myArcadeSticks))
    {
        // This code assumes that you're interested in all arcade sticks.
        myArcadeSticks->Append(arcadeStick);
    }
}
```

### Adding and removing arcade sticks

When an arcade stick is added or removed the [ArcadeStickAdded][] and [ArcadeStickRemoved][] events are raised. You can register handlers for these events to keep track of the arcade sticks that are currently connected.

The following example starts tracking an arcade stick that's been added.

```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // Check if the just-added arcade stick is already in myArcadeSticks; if it
    // isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

The following example stops tracking an arcade stick that's been removed.

```cpp
ArcadeStick::ArcadeStickRemoved += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    unsigned int indexRemoved;

    if(myArcadeSticks->IndexOf(args, &indexRemoved))
    {
        myArcadeSticks->RemoveAt(indexRemoved);
    }
}
```

### Users and headsets

Each arcade stick can be associated with a user account to link their identity to their gameplay, and can have a headset attached to facilitate voice chat or in-game features. To learn more about working with users and headsets, see [Tracking users and their devices](input-practices-for-games.md#tracking-users-and-their-devices) and [Headset](headset.md). -->

## <a name="reading-the-arcade-stick"></a>Lectura del stick arcade

Después de identificar el stick arcade que te interesa, puedes recopilar datos de él. Sin embargo, a diferencia de algunos otros tipos de entrada con los que puedes estar familiarizado, los sticks arcade no comunican el cambio de estado mediante la generación de eventos. En cambio, tienes que realizar lecturas regulares de su estado actual mediante _sondeos_.

### <a name="polling-the-arcade-stick"></a>Sondeo del stick arcade

El sondeo captura una instantánea del stick arcade en un momento preciso en el tiempo. Este enfoque para la recopilación de entradas es una buena opción para la mayoría de los juegos porque su lógica normalmente se ejecuta en un bucle determinista en lugar de ser controlado por eventos. Normalmente, es más fácil interpretar los comandos del juego a partir de la entrada recopilada todos a la vez que desde muchas entradas únicas recopiladas a lo largo del tiempo.

El sondeo de un stick arcade se realiza llamando a [GetCurrentReading][]; esta función devuelve [ArcadeStickReading][] que contiene el estado del stick arcade.

En el siguiente ejemplo se sondea el estado actual de un stick arcade.

```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

Además del estado del stick arcade, cada lectura incluye una marca de tiempo que indica con precisión cuándo se recuperó el estado. La marca de tiempo es útil para establecer una relación con los intervalos de lecturas anteriores o la duración de la simulación de juego.

### <a name="reading-the-buttons"></a>Lectura de los botones

Cada uno de los botones del Stick Arcade &mdash; las cuatro direcciones del joystick, seis botones de **acción** y dos botones **especiales** &mdash; proporcionan una lectura digital que indica si se ha presionado (hacia abajo) o se ha soltado (arriba). Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales; en su lugar, se empaquetan en un solo campo de bits que se representa mediante la enumeración [ArcadeStickButtons][] .

> [!NOTE]
> Los sticks arcade están equipados con botones adicionales que se usan para la navegación de la interfaz de usuario, como los botones de **vista** y de **menú** . Estos botones no forman parte de la enumeración `ArcadeStickButtons` y solo se pueden leer accediendo al stick arcade como dispositivo de navegación de la interfaz de usuario. Para obtener más información, consulta [UI Navigation Device (Dispositivo de navegación de la interfaz de usuario)](ui-navigation-controller.md).

Los valores de los botones se leen en la propiedad `Buttons` de la estructura [ArcadeStickReading][]. Dado que esta propiedad es un campo de bits, se usa el enmascaramiento bit a bit para aislar el valor del botón que te interesa. El botón está presionado (abajo) cuando se establece el bit correspondiente; de lo contrario, se libera (up).

En el ejemplo siguiente se determina si se presiona el botón 1 de la **acción** .

```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

En el ejemplo siguiente se determina si se libera el botón de la **acción 1** .

```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

En ocasiones, es posible que desee determinar si un botón cambia de presionado a liberado o liberado a presionado, si se presionan o se liberan varios botones, o si un conjunto de botones se organizan de una manera determinada &mdash; , pero no. Para obtener información sobre cómo detectar estas condiciones, consulta [Detección de transiciones de botón](input-practices-for-games.md#detecting-button-transitions) y [Detección disposiciones de botones complejas](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-inputinterfacing-sample"></a>Ejecución de la muestra de InputInterfacing

La [muestra de InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) ilustra cómo usar los sticks arcade y distintos tipos de dispositivos de entrada conjuntamente, así como el comportamiento de estos dispositivos de entrada como controladores de navegación de la interfaz de usuario.

## <a name="see-also"></a>Vea también

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Procedimientos de entrada para juegos](input-practices-for-games.md)

[Windows. Gaming. Input]: /uwp/api/Windows.Gaming.Input
[Windows.Gaming.Input.IGameController]: /uwp/api/Windows.Gaming.Input.IGameController
[Windows.Gaming.Input.UINavigationController]: /uwp/api/Windows.Gaming.Input.UINavigationController
[arcadestick]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadesticks]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickadded]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickremoved]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[getcurrentreading]: /uwp/api/Windows.Gaming.Input.ArcadeStick
[arcadestickreading]: /uwp/api/Windows.Gaming.Input.ArcadeStickReading
[arcadestickbuttons]: /uwp/api/Windows.Gaming.Input.ArcadeStickButtons