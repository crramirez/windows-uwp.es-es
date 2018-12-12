---
title: Stick arcade
description: Usa las API de stick arcade Windows.Gaming.Input para detectar y leer los sticks arcade.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, stick arcade, entrada
ms.localizationpriority: medium
ms.openlocfilehash: 6f9e3ff29dfb17b6e2a07df52153013b5266206e
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933820"
---
# <a name="arcade-stick"></a>Stick arcade

En esta página se describen los conceptos básicos de programación para sticks arcade de Xbox One usando [Windows.Gaming.Input.ArcadeStick][arcadestick] y las API relacionadas para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:

* Cómo obtener una lista de sticks arcade conectados y sus usuarios
* Cómo detectar que se ha agregado o quitado un stick arcade
* Cómo leer la entrada de uno o más sticks arcade
* cómo se comportan los sticks arcade como dispositivos de navegación de la interfaz de usuario

## <a name="arcade-stick-overview"></a>Información general sobre el stick arcade

Los sticks arcade son dispositivos de entrada apreciados por reproducir la sensación de las máquinas arcade de pie y por sus controles de alta precisión digital. Los sticks arcade son el dispositivo de entrada perfecto para los combates cara a acara y otros juegos de estilo arcade, además de ser adecuados para cualquier juego que funcione con controles totalmente digitales. Los sticks arcade son compatibles con aplicaciones para UWP de Windows 10 y Xbox One en el espacio de nombres [Windows.Gaming.Input][].

Sticks arcade de Xbox One están equipados con un joystick digital de 8 vías, seis botones de **acción** (representados como A1 A6 en la imagen siguiente) y dos botones **especial** (representados como S1 y S2); se encuentra en los dispositivos de entrada totalmente digital que no admiten controles analógicos o vibración. Sticks arcade de Xbox One también están equipados con botones de **vista** y el **menú** que se usan para admitir la navegación de la interfaz de usuario, pero no está pensadas para admitir comandos de juego y no se puede acceder a ellos fácilmente como botones de joystick.

![Stick con 4 direccional joystick, Arcade 6 botones de acción (A1 A6) y 2 botones especiales (S1 y S2)](images/arcade-stick-1.png)

### <a name="ui-navigation"></a>Navegación de la interfaz de usuario

Para aliviar la carga de la compatibilidad con muchos dispositivos de entrada diferentes para la navegación de la interfaz de usuario y fomentar la coherencia entre dispositivos y juegos, la mayoría de dispositivos de entrada _física_ actúan simultáneamente como dispositivo independiente de entrada _lógica_, llamado [controlador de navegación de la interfaz de usuario](ui-navigation-controller.md). El controlador de navegación de la interfaz de usuario proporciona un vocabulario común para los comandos de navegación de la interfaz de usuario entre los dispositivos de entrada.

Como un controlador de navegación de la interfaz de usuario, los sticks arcade asignan el [conjunto necesario](ui-navigation-controller.md#required-set) de comandos de navegación al joystick y los botones de **vista**, **menú**, **acción 1**y **2 de la acción** .

| Comando de navegación | Entrada del stick arcade  |
| ------------------:| ------------------- |
|                 Arriba | Stick hacia arriba            |
|               Abajo | Stick hacia abajo          |
|               Izquierda | Stick hacia la izquierda          |
|              Derecha | Stick hacia la derecha         |
|               Ver | Botón de vista         |
|               Menú | Botón de menú         |
|             Aceptar | Botón de acción 1     |
|             Cancelar | Botón de acción 2     |

Los sticks arcade no se asignan a ninguno de los comandos del [conjunto opcional](ui-navigation-controller.md#optional-set) de navegación.

## <a name="detect-and-track-arcade-sticks"></a>Detección y seguimiento de los sticks arcade

Detección y seguimiento sticks arcade funciona exactamente del mismo modo que lo hace para los controladores para juegos, excepto con la clase [ArcadeStick][] en lugar de la clase del [controlador para juegos](https://docs.microsoft.com/uwp/api/Windows.Gaming.Input.Gamepad) . Para obtener más información, consulta [Controlador para juegos y vibración](gamepad-and-vibration.md).

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

Después de identificar el stick arcade que te interesa, puedes recopilar datos de él. Sin embargo, a diferencia de algunos otros tipos de entrada con los que puedes estar familiarizado, los sticks arcade no comunican el cambio de estado mediante la generación de eventos. En su lugar, tienes que realizar lecturas regulares de su estado actual mediante _sondeos_.

### <a name="polling-the-arcade-stick"></a>Sondeo del stick arcade

El sondeo captura una instantánea del stick arcade en un momento preciso en el tiempo. Este enfoque de la recopilación de entrada es una buena opción para la mayoría de los juegos porque su lógica normalmente se ejecuta en un bucle determinista en lugar de controlarse mediante eventos; también suele ser más sencillo interpretar comandos de juego de la entrada recopilada de una vez que de muchas entradas individuales recopiladas a lo largo del tiempo.

El sondeo de un stick arcade se realiza llamando a [GetCurrentReading][]; esta función devuelve [ArcadeStickReading][] que contiene el estado del stick arcade.

En el siguiente ejemplo se sondea el estado actual de un stick arcade.

```cpp
auto arcadestick = myArcadeSticks[0];

ArcadeStickReading reading = arcadestick->GetCurrentReading();
```

Además del estado del stick arcade, cada lectura incluye una marca de tiempo que indica con precisión cuándo se recuperó el estado. La marca de tiempo es útil para establecer una relación con los intervalos de lecturas anteriores o la duración de la simulación de juego.

### <a name="reading-the-buttons"></a>Lectura de los botones

Cada uno de los botones del stick arcade&mdash;las cuatro direcciones del joystick, seis botones de **acción** y dos botones **especiales** &mdash;proporciona una lectura digital que indica si está presionado (abajo) o liberado (arriba). Por motivos de eficacia, las lecturas de botones no se representan como valores booleanos individuales; en su lugar, se empaquetan todas en un único campo de bits que se representa mediante la enumeración de [ArcadeStickButtons][] .

> [!NOTE]
> Los sticks Arcade están equipados con botones adicionales que se usan para la navegación de la interfaz de usuario, como los botones de **vista** y de **menú** . Estos botones no forman parte de la enumeración `ArcadeStickButtons` y solo se pueden leer accediendo al stick arcade como dispositivo de navegación de la interfaz de usuario. Para obtener más información, consulta [UI Navigation Device (Dispositivo de navegación de la interfaz de usuario)](ui-navigation-controller.md).

Los valores de los botones se leen en la propiedad `Buttons` de la estructura [ArcadeStickReading][]. Dado que esta propiedad es un campo de bits, se usa el enmascaramiento bit a bit para aislar el valor del botón que te interesa. El botón está presionado (abajo) cuando se establece el bit correspondiente; de lo contrario no lo está (arriba).

En el ejemplo siguiente se determina si se presiona el botón de **acción 1** .

```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

En el ejemplo siguiente se determina si el botón de **acción 1** no está presionado.

```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

A veces, es posible que quieras determinar si se suelta un botón que está presionado o si se presiona un botón que no lo estaba, si se presionan o sueltan varios botones o si un conjunto de botones tiene una disposición determinada; algunos presionados y otros no. Para obtener información sobre cómo detectar estas condiciones, consulta [Detección de transiciones de botón](input-practices-for-games.md#detecting-button-transitions) y [Detección disposiciones de botones complejas](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-inputinterfacing-sample"></a>Ejecución de la muestra de InputInterfacing

La [muestra de InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) ilustra cómo usar los sticks arcade y distintos tipos de dispositivos de entrada conjuntamente, así como el comportamiento de estos dispositivos de entrada como controladores de navegación de la interfaz de usuario.

## <a name="see-also"></a>Consulta también

* [Windows.Gaming.Input.UINavigationController][]
* [Windows.Gaming.Input.IGameController][]
* [Prácticas de entrada para juegos](input-practices-for-games.md)

[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[arcadestick]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.aspx
[arcadesticks]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadesticks.aspx
[arcadestickadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickadded.aspx
[arcadestickremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.arcadestickremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestick.getcurrentreading.aspx
[arcadestickreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickreading.aspx
[arcadestickbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.arcadestickbuttons.aspx
