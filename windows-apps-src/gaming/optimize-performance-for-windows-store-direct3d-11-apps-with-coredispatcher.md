---
author: mtoepke
title: Optimizar la latencia de entrada para juegos de DirectX para UWP
description: La latencia de entrada puede influir considerablemente en la experiencia de un juego, por lo que al optimizarla puedes conseguir una experiencia más fluida.
ms.assetid: e18cd1a8-860f-95fb-098d-29bf424de0c0
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, games, juegos, DirectX, input latency, latencia de entrada
ms.localizationpriority: medium
ms.openlocfilehash: a2e92dc10dbcdc3a511c1b1a1271ae759cc03c60
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5936604"
---
#  <a name="optimize-input-latency-for-universal-windows-platform-uwp-directx-games"></a>Optimización de la latencia de entrada en juegos DirectX de la Plataforma universal de Windows (UWP)



La latencia de entrada puede influir tremendamente en la experiencia de un juego, por lo que al optimizarla puedes conseguir una experiencia más fluida. Además, una correcta optimización de los eventos de entrada contribuye a mejorar la vida de la batería. Aprende a elegir las opciones adecuadas de procesamiento de eventos de entrada de CoreDispatcher, para garantizar que el juego controle las entradas de la forma más fluida posible.

## <a name="input-latency"></a>Latencia de entrada


La latencia de entrada es el tiempo que el sistema tarda en responder a una entrada del usuario. A menudo, esta respuesta consiste en un cambio de lo que se muestra en pantalla o la información de audio que se escucha.

Cada evento de entrada, que puede proceder de un puntero táctil, un puntero de ratón o un teclado, genera un mensaje que un controlador de eventos procesa. Los digitalizadores táctiles y periféricos de los juegos de hoy en día informan de los eventos de entrada a un mínimo de 100 Hz por puntero, lo que significa que tu aplicación puede recibir 100 o más eventos por segundo y por puntero (o por pulsación de tecla). Esta velocidad de actualizaciones se dispara si hay varios punteros funcionando al mismo tiempo o si se usa un dispositivo de entrada que tenga una precisión mayor (un mouse para juegos, por ejemplo). La cola de mensajes de eventos puede llenarse con mucha rapidez.

Es importante saber cuál va a ser la demanda de latencia de entrada del juego para que los eventos se procesen de la mejor forma según el escenario. No existe una única solución para todos los juegos.

## <a name="power-efficiency"></a>Eficiencia energética


En el contexto de la latencia de entrada, la "eficiencia energética" hace referencia a la cantidad de GPU que un juego usa. Un juego que use menos recursos de GPU será más eficiente energéticamente hablando y prolongará la vida de la batería. Esto es válido también en el caso de la CPU.

Si un juego es capaz de dibujar la pantalla completa en menos de 60fotogramas por segundo (la velocidad de representación máxima de la mayoría de las pantallas actualmente) sin perjudicar la experiencia del usuario, será más eficiente energéticamente si dibuja con menos frecuencia. Algunos juegos actualizan la pantalla únicamente como respuesta ante una entrada de usuario, por lo que no deberían dibujar el mismo contenido repetidamente a 60fotogramas por segundo.

## <a name="choosing-what-to-optimize-for"></a>Elección de lo que se debe optimizar


Cuando se diseña una aplicación de DirectX, es necesario decidir algunos aspectos. ¿Es necesario que la aplicación represente 60fotogramas por segundo para mostrar las animaciones fluidamente o solo debe representar como respuesta a una entrada? ¿Debe tener la menor latencia de entrada posible o es capaz de tolerar un cierto retraso? ¿Van a esperar mis usuarios que la aplicación haga un uso sensato de la batería?

Las respuestas a estas preguntas harán que la aplicación encaje en uno de los siguientes escenarios:

1.  Representar a petición. Los juegos de esta categoría actualizan la pantalla solo como respuesta a determinados tipos de entrada. La eficiencia energética es óptima, ya que la aplicación no tiene que representar fotogramas idénticos de forma repetida, y la latencia de entrada es baja porque la aplicación emplea la mayor parte del tiempo en esperar a que haya una entrada. Los juegos de mesa y los lectores de noticias son ejemplos de aplicaciones que pertenecerían a esta categoría.
2.  Representar a petición con animaciones transitorias. Este escenario es parecido al primero, a excepción de que algunos tipos de entrada iniciarán una animación que no depende de las entradas posteriores del usuario. Se logra una buena eficiencia energética porque el juego no representa fotogramas idénticos de forma repetida y la latencia de entrada es baja cuando no hay animaciones en el juego. Los juegos interactivos para niños y los juegos de mesa que animan cada movimiento son ejemplos de aplicaciones que pertenecerían a esta categoría.
3.  Representar 60fotogramas por segundo. En este escenario, el juego actualiza la pantalla constantemente. La eficiencia energética es reducida, ya que se representa el máximo número de fotogramas que la pantalla es capaz de presentar. En cuanto a la latencia de entrada, es alta porque DirectX bloquea el subproceso mientras el contenido se está presentando; al hacerlo, impide que el subproceso envíe a la pantalla más fotogramas de los que puede mostrar al usuario. Los juegos de tirador en primera persona, los juegos de estrategia en tiempo real y los juegos físicos son ejemplos de aplicaciones que pertenecerían a esta categoría.
4.  Representar 60fotogramas por segundo y lograr la latencia de entrada más baja posible. Al igual que el escenario3, la aplicación actualiza la pantalla constantemente, de modo que la eficiencia energética no es buena. La diferencia estriba en que el juego responde a la entrada en un subproceso aparte, con lo cual el procesamiento de dicha entrada no se bloquea cuando se muestran gráficos en la pantalla. Los juegos multijugador en línea, los juegos de lucha y los juegos de ritmo o sincronización pertenecen a esta categoría, ya que admiten entradas de movimiento en ventanas de evento realmente ajustadas.

## <a name="implementation"></a>Implementación


La mayoría de los juegos de DirectX se controla por medio de lo que se conoce como bucle del juego. El algoritmo básico consiste en realizar los siguientes pasos hasta que el usuario cierra el juego o la aplicación:

1.  Procesar la entrada
2.  Actualizar el estado del juego
3.  Dibujar el contenido del juego

Cuando el contenido de un juego de DirectX se representa y está listo para aparecer en pantalla, el bucle del juego espera a que la GPU esté preparada para recibir un fotograma nuevo antes activarse para procesar la entrada de nuevo.

Realizaremos una iteración de un sencillo juego de puzzle para ilustrar la implementación del bucle del juego en cada uno de los escenarios mencionados anteriormente. En cada implementación se comentan las decisiones tomadas, los beneficios y el equilibrio obtenidos, que te servirán de guía a la hora de optimizar tus aplicaciones para lograr una latencia de entrada baja y la eficiencia energética.

## <a name="scenario-1-render-on-demand"></a>Escenario1: Representar a petición


La primera iteración del juego de puzzle actualiza la pantalla únicamente cuando un usuario mueve una pieza del puzzle. Un usuario puede arrastrar una pieza del puzzle hasta su sitio o bien ajustarla en su sitio seleccionándola y, luego, tocando el destino correspondiente. En el segundo caso, la pieza de puzzle irá al destino sin ninguna animación o efecto.

El código tiene un bucle de juego con un solo subproceso dentro del método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) que usa **CoreProcessEventsOption::ProcessOneAndAllPending**. Si se usa esta opción, todos los eventos que actualmente estén disponibles se distribuirán en la cola. Si no hay eventos pendientes, el bucle del juego espera hasta que surge uno.

``` syntax
void App::Run()
{
    
    while (!m_windowClosed)
    {
        // Wait for system events or input from the user.
        // ProcessOneAndAllPending will block the thread until events appear and are processed.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

        // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
        // scene and present it to the display.
        if (m_updateWindow || m_state->StateChanged())
        {
            m_main->Render();
            m_deviceResources->Present();

            m_updateWindow = false;
            m_state->Validate();
        }
    }
}
```

## <a name="scenario-2-render-on-demand-with-transient-animations"></a>Escenario2: Representar a petición con animaciones transitorias


En la segunda iteración, el juego se modifica de forma que, cuando un usuario selecciona una pieza del puzzle y después toca el destino correcto de dicha pieza, se produce una animación de la pieza en pantalla hasta que llega a su destino.

Como ya se comentó antes, el código tiene un bucle de juego con un solo subproceso que usa **ProcessOneAndAllPending** para distribuir los eventos de entrada en la cola. La diferencia aquí reside en que, durante una animación, el bucle cambia para usar **CoreProcessEventsOption::ProcessAllIfPresent** y, así, no tener que esperar nuevos eventos de entrada. Si no hay eventos pendientes, [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) regresa inmediatamente y permite que la aplicación muestre el siguiente fotograma de la animación. Cuando la animación finaliza, el bucle cambia de nuevo a **ProcessOneAndAllPending** para restringir las actualizaciones de pantalla.

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        // 2. Switch to a continuous rendering loop during the animation.
        if (m_state->Animating())
        {
            // Process any system events or input from the user that is currently queued.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // you are trying to present a smooth animation to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // Wait for system events or input from the user.
            // ProcessOneAndAllPending will block the thread until events appear and are processed.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);

            // If any of the events processed resulted in a need to redraw the window contents, then we will re-render the
            // scene and present it to the display.
            if (m_updateWindow || m_state->StateChanged())
            {
                m_main->Render();
                m_deviceResources->Present();

                m_updateWindow = false;
                m_state->Validate();
            }
        }
    }
}
```

Para permitir la transición entre **ProcessOneAndAllPending** y **ProcessAllIfPresent**, la aplicación debe realizar un seguimiento del estado para saber si hay animación. En la aplicación de puzle, esto se logra agregando un nuevo método al que se puede llamar durante el bucle del juego en la clase GameState. La rama de animación del bucle del juego controla las actualizaciones en el estado de la animación llamando al nuevo método Update de GameState.

## <a name="scenario-3-render-60-frames-per-second"></a>Escenario3: Representar 60fotogramas por segundo


En la tercera iteración, la aplicación muestra un cronómetro que informa al usuario del tiempo que lleva manipulando el puzzle. Como refleja el tiempo transcurrido hasta en milisegundos, debe representar 60fotogramas por segundo para mantener la pantalla actualizada.

Al igual que sucede en los escenarios1 y2, la aplicación tiene un bucle del juego con un solo subproceso. La diferencia en este escenario es que, al ser la representación continua, no es necesario hacer un seguimiento de los cambios en el estado del juego, como ocurría en los dos primeros escenarios. En consecuencia, puede usar **ProcessAllIfPresent** (que es el procedimiento predeterminado) para procesar eventos. Si no hay eventos pendientes, **ProcessEvents** regresa inmediatamente y comienza a representar el siguiente fotograma.

``` syntax
void App::Run()
{

    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            // 3. Continuously render frames and process system events and input as they appear in the queue.
            // ProcessAllIfPresent will not block the thread to wait for events. This is the desired behavior when
            // trying to present smooth animations to the user.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_state->Update();
            m_main->Render();
            m_deviceResources->Present();
        }
        else
        {
            // 3. If the window isn't visible, there is no need to continuously render.
            // Process events as they appear until the window becomes visible again.
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

Este método constituye la forma más sencilla de escribir un juego, ya que no hay que realizar un seguimiento del estado para saber cuándo representar. Alcanza la mayor velocidad de representación posible y una capacidad de respuesta a entradas bastante razonable en un intervalo de cronómetro.

Sin embargo, esta facilidad de desarrollo tiene su precio, y es que representar a 60 fotogramas por segundo requiere más energía que hacerlo a petición. Lo mejor es usar **ProcessAllIfPresent** cuando el juego cambie lo que aparece en cada fotograma. Asimismo, la latencia de entrada también aumenta hasta llegar incluso a los 16,7 ms, ya que la aplicación bloquea el bucle del juego en el intervalo de sincronización de la pantalla en lugar de en **ProcessEvents**. Algunos eventos de entrada pueden quedar descartados, porque la cola se procesa solamente una vez por fotograma (60 Hz).

## <a name="scenario-4-render-60-frames-per-second-and-achieve-the-lowest-possible-input-latency"></a>Escenario4: Representar 60fotogramas por segundo y lograr la latencia de entrada más baja posible


Algunos juegos pueden ser capaces de ignorar o compensar el aumento de la latencia de entrada que hemos visto en el escenario3. No obstante, si una latencia de entrada baja es fundamental para la experiencia del juego y la sensibilidad de la respuesta del jugador, los juegos que se representan a 60fotogramas por segundo deberán procesar las entradas en un subproceso aparte.

La cuarta iteración del juego de puzzle se basa en el escenario3, y divide el procesamiento de entradas y la representación de gráficos del bucle del juego en subprocesos independientes. Al tener subprocesos independientes para cada evento, la salida de gráficos no provoca retrasos en la entrada, pero el código es más complejo. En el escenario 4, el subproceso de entrada llama a [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) con [**CoreProcessEventsOption::ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217), que espera nuevos eventos y distribuye todos los eventos disponibles. Este comportamiento se mantiene hasta que la ventana se cierra o el juego llama a [**CoreWindow::Close**](https://msdn.microsoft.com/library/windows/apps/br208260).

``` syntax
void App::Run()
{
    // 4. Start a thread dedicated to rendering and dedicate the UI thread to input processing.
    m_main->StartRenderThread();

    // ProcessUntilQuit will block the thread and process events as they appear until the App terminates.
    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
}

void JigsawPuzzleMain::StartRenderThread()
{
    // If the render thread is already running, then do not start another one.
    if (IsRendering())
    {
        return;
    }

    // Create a task that will be run on a background thread.
    auto workItemHandler = ref new WorkItemHandler([this](IAsyncAction^ action)
    {
        // Notify the swap chain that this app intends to render each frame faster
        // than the display's vertical refresh rate (typically 60 Hz). Apps that cannot
        // deliver frames this quickly should set this to 2.
        m_deviceResources->SetMaximumFrameLatency(1);

        // Calculate the updated frame and render once per vertical blanking interval.
        while (action->Status == AsyncStatus::Started)
        {
            // Execute any work items that have been queued by the input thread.
            ProcessPendingWork();

            // Take a snapshot of the current game state. This allows the renderers to work with a
            // set of values that won't be changed while the input thread continues to process events.
            m_state->SnapState();

            m_sceneRenderer->Render();
            m_deviceResources->Present();
        }

        // Ensure that all pending work items have been processed before terminating the thread.
        ProcessPendingWork();
    });

    // Run the task on a dedicated high priority background thread.
    m_renderLoopWorker = ThreadPool::RunAsync(workItemHandler, WorkItemPriority::High, WorkItemOptions::TimeSliced);
}
```

La plantilla **DirectX 11 and XAML App (Universal Windows)** en Microsoft Visual Studio2015 divide el bucle del juego en varios subprocesos de una forma similar. Emplea el objeto [**Windows::UI::Core::CoreIndependentInputSource**](https://msdn.microsoft.com/library/windows/apps/dn298460) para iniciar un subproceso dedicado a controlar la entrada y, de igual modo, crea un subproceso de representación independiente del subproceso de interfaz de usuario XAML. Para más información sobre estas plantillas, te recomendamos que leas [Crear un proyecto de juego para la Plataforma universal de Windows y DirectX a partir de una plantilla](user-interface.md).

## <a name="additional-ways-to-reduce-input-latency"></a>Otras formas de reducir la latencia de entrada


### <a name="use-waitable-swap-chains"></a>Usar cadenas de intercambio que pueden esperar

Los juegos de DirectX responden a las entradas de usuario actualizando lo que se ve en pantalla. En una pantalla de 60Hz, la actualización se produce cada 16,7ms (1 segundo/60fotogramas). En la figura1 se muestran el ciclo de vida aproximado y la respuesta a un evento de entrada relativo a la señal de actualización de 16,7ms (VBlank) de una aplicación que representa 60fotogramas por segundo:

Figura 1

![figura 1: latencia de entrada en directx ](images/input-latency1.png)

En Windows8.1, DXGI introdujo la marca **DXGI\_SWAP\_CHAIN\_FLAG\_FRAME\_LATENCY\_WAITABLE\_OBJECT** la cadena de intercambio, lo que permite a las aplicaciones reducir esta latencia fácilmente sin tener que implementar la heurística para mantener la cola vacía. Las cadenas de intercambio creadas con esta etiqueta se conocen como cadenas de intercambio que pueden esperar. En la figura2 se muestran el ciclo de vida y respuesta aproximados a un evento de entrada cuando se usan cadenas de intercambio que pueden esperar:

Figura 2

![figura 2: latencia de entrada en DirectX que puede esperar](images/input-latency2.png)

Lo que deducimos de estos diagramas es que existe la posibilidad de reducir la latencia de entrada de los juegos en hasta dos fotogramas completos si son capaces de representar cada fotograma dentro de los 16,7 ms previstos y que están definidos en la frecuencia de actualización de la pantalla. En el ejemplo del juego de puzle se usan cadenas de intercambio que pueden esperar y se controla el límite de la cola de eventos presentes llamando a ` m_deviceResources->SetMaximumFrameLatency(1);`

 

 




