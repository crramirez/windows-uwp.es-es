---
author: mithom
title: Stick arcade
description: Usa las API de stick arcade Windows.Gaming.Input para detectar y leer los sticks arcade.
ms.assetid: 2E52232F-3014-4C8C-B39D-FAC478BA3E01
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, juegos, stick arcade, entrada
ms.openlocfilehash: b0411dcf1fd75ec7dc31d29a39e95f5c26073953
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: es-ES
ms.locfileid: "238647"
---
# <a name="arcade-stick"></a>Stick arcade

En esta página se describen los conceptos básicos de programación para sticks arcade de Xbox One usando [Windows.Gaming.Input.ArcadeStick][arcadestick] y las API relacionadas para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:
* Cómo obtener una lista de sticks arcade conectados y sus usuarios
* Cómo detectar que se ha agregado o quitado un stick arcade
* Cómo leer la entrada de uno o más sticks arcade
* Cómo se comportan los sticks arcade como dispositivo de navegación


## <a name="arcade-stick-overview"></a>Información general sobre el stick arcade

Los sticks arcade son dispositivos de entrada apreciados por reproducir la sensación de las máquinas arcade de pie y por sus controles de alta precisión digital. Los sticks arcade son el dispositivo de entrada perfecto para los combates cara a acara y otros juegos de estilo arcade, además de ser adecuados para cualquier juego que funcione con controles totalmente digitales. Los sticks arcade son compatibles con aplicaciones para UWP de Windows 10 y Xbox One en el espacio de nombres [Windows.Gaming.Input][].

Los sticks arcade de Xbox One están equipados con un joystick digital de 8 vías, seis botones de **acción** y dos botones **especiales**; son dispositivos de entrada totalmente digital que no admiten controles analógicos o vibración. Los sticks arcade de Xbox One también están equipados con botones de **vista** y **menú** que se usan para la navegación de la interfaz de usuario, pero no están diseñados para admitir comandos de juego y su acceso no es tan fácil como el de los botones de joystick.

### <a name="ui-navigation"></a>Navegación de la interfaz de usuario

Para aliviar la carga de la compatibilidad con muchos dispositivos de entrada diferentes para la navegación de la interfaz de usuario y fomentar la coherencia entre dispositivos y juegos, la mayoría de dispositivos de entrada _física_ actúan simultáneamente como dispositivo independiente de entrada _lógica_, llamado [controlador de navegación de la interfaz de usuario](ui-navigation-controller.md). El controlador de navegación de la interfaz de usuario proporciona un vocabulario común para los comandos de navegación de la interfaz de usuario entre los dispositivos de entrada.

Como controlador de navegación de la interfaz de usuario, los sticks arcade asignan el [conjunto necesario](ui-navigation-controller.md#required-set) de comandos de navegación al joystick y a los botones de **vista**, **menú**, **acción 1** y **acción 2**.

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

El sistema administra los sticks arcade, por lo tanto, no tendrás que crearlos ni inicializarlos. El sistema proporciona una lista de sticks arcade conectados y eventos para notificarte cuándo se agrega o quita un stick arcade.

### <a name="the-arcade-sticks-list"></a>Lista de sticks arcade

La clase [ArcadeStick][] proporciona una propiedad estática, [ArcadeSticks][], que es una lista de solo lectura de los sticks arcade que están actualmente conectados. Como probablemente solo te interesen algunos de los sticks arcade conectados, se recomienda mantener tu propia colección en lugar de acceder a ellos a través de la propiedad `ArcadeSticks`.

En el siguiente ejemplo se copian todos los sticks arcade conectados en una nueva colección.
```cpp
auto myArcadeSticks = ref new Vector<ArcadeStick^>();

for (auto arcadestick : ArcadeStick::ArcadeSticks)
{
    // This code assumes that you're interested in all arcade sticks.
    myArcadeSticks->Append(arcadestick);
}
```

### <a name="adding-and-removing-arcade-sticks"></a>Agregar y quitar sticks arcade

Cuando se agrega o quita un stick arcade, se generan los eventos [ArcadeStickAdded][] y [ArcadeStickRemoved][]. Puedes registrar controladores de estos eventos para realizar un seguimiento de los sticks arcade que están conectados actualmente.

En el siguiente ejemplo se inicia el seguimiento de un stick arcade que se ha agregado.
```cpp
ArcadeStick::ArcadeStickAdded += ref new EventHandler<ArcadeStick^>(Platform::Object^, ArcadeStick^ args)
{
    // This code assumes that you're interested in all new arcade sticks.
    myArcadeSticks->Append(args);
}
```

En el siguiente ejemplo se detiene el seguimiento de un stick arcade que se ha quitado.
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

### <a name="users-and-headsets"></a>Usuarios y auriculares

Cada stick arcade puede asociarse con una cuenta de usuario para vincular su identidad al juego y puede tener conectados unos auriculares para facilitar el chat de voz o las funciones en el juego. Para obtener más información sobre cómo trabajar con usuarios y auriculares, consulta [Tracking users and their devices (Seguimiento de usuarios y sus dispositivos)](input-practices-for-games.md#tracking-users-and-their-devices) y [Headset (Auriculares)](headset.md).


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

Cada uno de los botones del stick arcade (las cuatro direcciones del joystick, seis botones de **acción** y dos botones **especiales**) proporcionan una lectura digital que indica si está presionado (abajo) o no (arriba). Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales; en su lugar, se empaquetan todas en un único campo de bits que se representa mediante la enumeración [ArcadeStickButtons][].

> **Nota**    Los sticks arcade están equipados con botones adicionales que se usan para la navegación de la interfaz de usuario, como los botones de **vista** y **menú**. Estos botones no forman parte de la enumeración `ArcadeStickButtons` y solo se pueden leer accediendo al stick arcade como dispositivo de navegación de la interfaz de usuario. Para obtener más información, consulta [UI Navigation Device (Dispositivo de navegación de la interfaz de usuario)](ui-navigation-controller.md).

Los valores de los botones se leen en la propiedad `Buttons` de la estructura [ArcadeStickReading][]. Dado que esta propiedad es un campo de bits, se usa el enmascaramiento bit a bit para aislar el valor del botón que te interesa. El botón está presionado (abajo) cuando se establece el bit correspondiente; de lo contrario no lo está (arriba).

En el ejemplo siguiente, se determina si el botón de acción 1 está presionado.
```cpp
if (ArcadeStickButtons::Action1 == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is pressed
}
```

En el ejemplo siguiente, se determina si el botón de acción 1 no está presionado.
```cpp
if (ArcadeStickButtons::None == (reading.Buttons & ArcadeStickButtons::Action1))
{
    // Action 1 is released (not pressed)
}
```

A veces, es posible que quieras determinar si se suelta un botón que está presionado o si se presiona un botón que no lo estaba, si se presionan o sueltan varios botones o si un conjunto de botones tiene una disposición determinada, algunos presionados y otros no. Para obtener información sobre cómo detectar estas condiciones, consulta [Detección de transiciones de botón](input-practices-for-games.md#detecting-button-transitions) y [Detección disposiciones de botones complejas](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-inputinterfacing-sample"></a>Ejecución de la muestra de InputInterfacing

La [muestra de InputInterfacingUWP _(github)_](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/InputInterfacingUWP) ilustra cómo usar los sticks arcade y distintos tipos de dispositivos de entrada conjuntamente, así como el comportamiento de estos dispositivos de entrada como controladores de navegación de la interfaz de usuario.


## <a name="see-also"></a>Consulta también
[Windows.Gaming.Input.UINavigationController][]
[Windows.Gaming.Input.IGameController][]

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
