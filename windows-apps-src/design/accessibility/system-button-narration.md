---
description: 'Más información acerca de: lectores de pantalla y botones del sistema de hardware'
title: Eventos de botón de hardware y lectores de pantalla
label: Screen readers and hardware button events
template: detail.hbs
ms.date: 02/20/2020
ms.topic: article
keywords: Windows 10, UWP, accesibilidad, narrador, lector de pantalla
ms.localizationpriority: medium
ms.openlocfilehash: d4bf712c83b39a184247a4476db5a045f62a9482
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029858"
---
# <a name="screen-readers-and-hardware-system-buttons"></a>Botones del sistema de hardware y lectores de pantalla

Los lectores de pantalla, como [narrador](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator), deben ser capaces de reconocer y controlar los eventos de los botones del sistema de hardware y comunicar su estado a los usuarios. En algunos casos, es posible que el lector de pantalla tenga que controlar estos eventos de botón de hardware de forma exclusiva y no permitir que se propaguen a otros controladores.

A partir de la versión 2004 de Windows 10, las aplicaciones UWP pueden escuchar y controlar los eventos de botón del sistema de hardware **FN** de la misma manera que otros botones de hardware. Anteriormente, este botón del sistema actuaba solo como un modificador para el modo en que otros botones de hardware indicaban sus eventos y su estado.

> [!NOTE]
> La compatibilidad con el botón FN es específica del OEM y puede incluir características como la capacidad de activar o desactivar la alternancia y el bloqueo (frente a una combinación de teclas de presionar y mantener pulsada), junto con una luz del indicador de bloqueo correspondiente (que podría no ser útil para los usuarios ciegos o con discapacidad visual).

Los eventos de botón FN se exponen a través de una nueva [clase SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) en el espacio de nombres [Windows. UI. Input](/uwp/api/windows.ui.input) . El objeto SystemButtonEventController admite los siguientes eventos:

- [SystemFunctionButtonPressed](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonpressed)
- [SystemFunctionButtonReleased](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionbuttonreleased)
- [SystemFunctionLockChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockchanged)
- [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged)

> [!Important]
> SystemButtonEventController no puede recibir estos eventos si ya los ha controlado un controlador de prioridad más alto.

## <a name="examples"></a>Ejemplos

En los ejemplos siguientes, se muestra cómo crear un [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) basado en un DispatcherQueue y administrar los cuatro eventos admitidos por este objeto.

Es habitual que se active más de uno de los eventos compatibles cuando se presiona el botón FN. Por ejemplo, al presionar el botón FN en un teclado Surface se activan SystemFunctionButtonPressed, SystemFunctionLockChanged y SystemFunctionLockIndicatorChanged al mismo tiempo.

1. En este primer fragmento, simplemente incluimos los espacios de nombres necesarios y especificamos algunos objetos globales, incluidos el [DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) y los objetos [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller) para administrar el subproceso [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) .

   A continuación, se especifican los [tokens de evento](/uwp/cpp-ref-for-winrt/event-token) devueltos al registrar los delegados de control de eventos [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) .

    ```cppwinrt
    namespace winrt
    {
        using namespace Windows::System;
        using namespace Windows::UI::Input;
    }

    ...

    // Declare related members
    winrt::DispatcherQueueController _queueController;
    winrt::DispatcherQueue _queue;
    winrt::SystemButtonEventController _controller;
    winrt::event_token _fnKeyDownToken;
    winrt::event_token _fnKeyUpToken;
    winrt::event_token _fnLockToken;
    ```

2. También especificamos un token de evento para el evento [SystemFunctionLockIndicatorChanged](/uwp/api/windows.ui.input.systembuttoneventcontroller.systemfunctionlockindicatorchanged) junto con un booleano para indicar si la aplicación está en "modo de aprendizaje" (donde el usuario simplemente está intentando explorar el teclado sin realizar ninguna función).

    ```cppwinrt
    winrt::event_token _fnLockIndicatorToken;
    bool _isLearningMode = false;
    ```

3. Este tercer fragmento de código incluye los delegados de controlador de eventos correspondientes para cada evento admitido por el objeto [SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller) .

   Cada controlador de eventos anuncia el evento que se ha producido. Además, el controlador FunctionLockIndicatorChanged controla también si la aplicación está en modo de "aprendizaje" ( `_isLearningMode` = true), lo que impide que el evento se propague a otros controladores y permite al usuario explorar las características del teclado sin realizar realmente la acción.

    ```cppwinrt
    void SetupSystemButtonEventController()
    {
        // Create dispatcher queue controller and dispatcher queue
        _queueController = winrt::DispatcherQueueController::CreateOnDedicatedThread();
        _queue = _queueController.DispatcherQueue();

        // Create controller based on new created dispatcher queue
        _controller = winrt::SystemButtonEventController::CreateForDispatcherQueue(_queue);

        // Add Event Handler for each different event
        _fnKeyDownToken = _controller->FunctionButtonPressed(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
            {
                // Mock function to read the sentence "Fn button is pressed"
                PronounceFunctionButtonPressedMock();
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

            _fnKeyUpToken = _controller->FunctionButtonReleased(
                [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionButtonEventArgs& args)
                {
                    // Mock function to read the sentence "Fn button is up"
                    PronounceFunctionButtonReleasedMock();
                    // Set Handled as true means this event is consumed by this controller
                    // no more targets will receive this event
                    args.Handled(true);
                });

        _fnLockToken = _controller->FunctionLockChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn shift is locked/unlocked"
                PronounceFunctionLockMock(args.IsLocked());
                // Set Handled as true means this event is consumed by this controller
                // no more targets will receive this event
                args.Handled(true);
            });

        _fnLockIndicatorToken = _controller->FunctionLockIndicatorChanged(
            [](const winrt::SystemButtonEventController& /*sender*/, const winrt:: FunctionLockIndicatorChangedEventArgs& args)
            {
                // Mock function to read the sentence "Fn lock indicator is on/off"
                PronounceFunctionLockIndicatorMock(args.IsIndicatorOn());
                // In learning mode, the user is exploring the keyboard. They expect the program
                // to announce what the key they just pressed WOULD HAVE DONE, without actually
                // doing it. Therefore, handle the event when in learning mode so the key is ignored
                // by the system.
                args.Handled(_isLearningMode);
            });
    }
    ```

## <a name="see-also"></a>Consulte también

[Clase SystemButtonEventController](/uwp/api/windows.ui.input.systembuttoneventcontroller)
