---
author: mithom
title: "Prácticas de entrada para juegos"
description: "Obtén información sobre patrones y técnicas para el uso eficaz de los dispositivos de entrada."
ms.assetid: CBAD3345-3333-4924-B6D8-705279F52676
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, juegos, entrada, games, input
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 15d56a27ad914b258bb19b80b3498510d01105cd
ms.lasthandoff: 02/07/2017

---

# <a name="input-practices-for-games"></a>Procedimientos de entrada para juegos

En esta página se describen patrones y técnicas para usar con eficacia dispositivos de entrada de juegos para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:
* Cómo hacer un seguimiento de los jugadores y de los dispositivos de entrada y navegación que están usando actualmente
* Cómo detectar las transiciones de botón (presionado a no presionado, sin presionar a presionado)
* Cómo detectar disposiciones de botones complejas con una sola prueba

## <a name="tracking-users-and-their-devices"></a>Seguimiento de usuarios y sus dispositivos

Todos los dispositivos de entrada se asocian con un [usuario][Windows.System.User] para que su identidad pueda vincularse a su juego, logros, cambios de configuración y otras actividades. Los usuarios pueden iniciar o cerrar sesión cuando quieran y es habitual que un usuario diferente inicie sesión en un dispositivo de entrada que permanece conectado al sistema después de que el usuario anterior haya cerrado sesión. Cuando un usuario se conecta o desconecta, se genera el evento [IGameController.UserChanged][]. Puedes registrar un controlador de eventos para este evento a fin de realizar un seguimiento de los jugadores y de los dispositivos que usan.

La identidad del usuario también es la manera en que un dispositivo de entrada se asocia a su correspondiente controlador de navegación de la interfaz de usuario.

Por estas razones, debe realizarse un seguimiento de la entrada del jugador y correlacionarse con la propiedad [User][igamecontroller.user] de la clase de dispositivo (heredada de la interfaz [IGameController][]).

La muestra de [UserGamepadPairingUWP _(github)_](
https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/Samples/System/UserGamepadPairingUWP) indica cómo puedes hacer un seguimiento de los usuarios y los dispositivos que usan.

## <a name="detecting-button-transitions"></a>Detección de transiciones de botón

A veces quieres saber cuándo se presiona o se suelta un botón; es decir, cuándo cambia exactamente el estado del botón de no presionado a presionado o de presionado a no presionado. Para determinarlo, debes recordar la lectura de dispositivo anterior y compararla con la lectura actual para ver qué ha cambiado.

En el siguiente ejemplo se muestra un enfoque básico para recordar la lectura anterior; en él se muestran controladores para juegos, pero los principios son los mismos para sticks arcade, volantes y botones de navegación de la interfaz de usuario

```cpp
GamepadReading newReading();
GamepadReading oldReading();

// Game::Loop represents one iteration of a typical game loop
void Game::Loop()
{
    // move previous newReading into oldReading before getting next newReading
    oldReading = newReading, newReading = gamepad.CurrentReading();

    // process device readings using buttonJustPressed/buttonJustReleased
}
```

Antes de realizar alguna cosa, `Game::Loop` mueve el valor existente de `newReading` (la lectura del controlador para juegos de la iteración de bucle anterior) a `oldReading` i, después, rellena `newReading` con una nueva lectura de controlador para juegos para la iteración actual. Esto te ofrece la información que necesitas para detectar transiciones de botón.

En el siguiente ejemplo se muestra un enfoque básico para detectar transiciones de botón.

```cpp
bool buttonJustPressed(const gamepadButtons selection)
{
    bool newSelectionPressed = (selection == (newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (oldReading.Buttons & selection));

    return newSelectionPressed && !oldSelectionPressed;
}

bool buttonJustReleased(gamepadButtons selection)
{
    bool newSelectionReleased = (gamepadButtons.None == (newReading.Buttons & selection));
    bool oldSelectionReleased = (gamepadButtons.None == (oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Estas dos funciones primero derivan el estado booleano de la selección de botón de `newReading` y `oldReading` y, después, realizan una lógica booleana para determinar si se ha producido la transición de destino. Estas funciones devuelven _true_ solo si la nueva lectura contiene el estado de destino (presionado o liberado, respectivamente) *y* la lectura anterior tampoco contiene el estado de destino; de lo contrario, devuelven _false_.


## <a name="detecting-complex-button-arrangements"></a>Detección de disposiciones de botones complejas

Cada botón de un dispositivo de entrada proporciona una lectura digital que indica si está presionado (abajo) o liberado (arriba). Por motivos de eficacia, las lecturas de los botones no se representan como valores booleanos individuales; en su lugar, se empaquetan todas en campos de bits que se representan mediante enumeraciones específicas de dispositivo como [GamepadButtons][]. Para leer botones específicos, se usa el enmascaramiento bit a bit para aislar los valores que te interesan. Los botones están presionados (abajo) cuando se establece su bit correspondiente; de lo contrario no lo están (arriba).

Recuerda cómo se determina si un solo botón está presionado o no; aquí se muestran controladores para juegos pero los principios son los mismos para el stick arcade, el volante y los botones de navegación de la interfaz de usuario.

```cpp
// determines whether gamepad button A is pressed
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}

// determines whether gamepad button A is released
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released (not pressed)
}
```

Como puedes ver, la determinación del estado de un solo botón es directa, pero a veces, es posible que quieras determinar si varios botones están presionados o liberados, o si un conjunto de botones está dispuesto de un modo determinado, algunos presionados y otros no. Probar varios botones es más complejo que probar un solo botón, especialmente por la posibilidad de estado de botones combinados, pero hay una fórmula simple para estas pruebas que se aplica a un solo botón y a varios por igual.

En el ejemplo siguiente, se determina si los botones A y B del controlador para juegos están ambos presionados.

```cpp
if ((GamepadButtons::A | GamepadButtons::B) == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are pressed
}
```

En el ejemplo siguiente, se determina si los botones A y B del controlador para juegos están ambos liberados.

```cpp
if ((GamepadButtons::None == (reading.Buttons & GamepadButtons::A | GamepadButtons::B))
{
    // buttons A and B are released (not pressed)
}
```

En el ejemplo siguiente, se determina si el botón A del controlador para juegos está presionado mientras que el botón B está liberado.

```cpp
if (GamepadButtons::A == (reading.Buttons & (GamepadButtons::A | GamepadButtons::B))
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

La fórmula que los cinco ejemplos tienen en común es que la disposición de los botones que se van a probar se especifica mediante la expresión de la izquierda del operador de igualdad mientras que los botones que se deben tomar en consideración se seleccionan mediante la expresión de enmascaramiento de la derecha.

En el siguiente ejemplo se muestra esta fórmula más claramente, volviendo a escribir el ejemplo anterior.

```cpp
auto buttonArrangement = GamepadButtons::A;
auto buttonSelection = (reading.Buttons & (GamepadButtons::A | GamepadButtons::B));

if (buttonArrangement == buttonSelection)
{
    // button A is pressed and button B is released (button B is not pressed)
}
```

Esta fórmula puede aplicarse para probar cualquier número de botones en cualquier disposición de sus estados.



[Windows.System.User]: https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.user]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.user.aspx
[igamecontroller.userchanged]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.userchanged.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx

