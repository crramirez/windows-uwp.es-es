---
author: joannaleecy
title: Administración del flujo de los juegos
description: Obtén información sobre cómo inicializar estados de juegos, controlar eventos y establecer el bucle de actualización del juego.
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, directx
ms.localizationpriority: medium
ms.openlocfilehash: 610b794c0ded6791e93c14d8960366132afd973b
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7431880"
---
# <a name="game-flow-management"></a>Administración del flujo de los juegos

Ahora, el juego tiene una ventana, ha registrado un par de controladores de eventos y carga asíncronamente activos de carga. Esta sección trata sobre el uso de los estados del juego, cómo administrar estados de juego claves específicos y cómo crear un bucle de actualización para el motor del juego. A continuación, se tratará sobre el flujo de la interfaz de usuario y por último, sobre saber más acerca de los controladores de eventos y eventos que son necesarios para un juego UWP.

>[!Note]
>Si no has descargado el código del juego más reciente para esta muestra, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Ten en cuenta que este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="game-states-used-to-manage-game-flow"></a>Estados del juego usados para administrar el flujo del juego

Usamos estados del juego para administrar el flujo del juego. El juego puede tener cualquier número de estados posibles, puesto que un usuario puede reanudar en cualquier momento una aplicación de juego de UWP desde un estado suspendido.

Para esta muestra de juego, al comenzar podrá estar en uno de estos tres estados:
* El bucle del juego se está ejecutando y está en mitad de un nivel.
* El bucle del juego no se está ejecutando porque un juego acababa de completarse. (Se establece la puntuación alta)
* No se ha iniciado ningún juego o el juego se encuentra entre niveles. (La puntuación alta es 0)

Puedes definir el número de estados necesarios según las necesidades de tu juego. De nuevo, ten presente que tu juego de UWP puede finalizarse en cualquier momento y, cuando se reanude, el jugador espera que el juego se comporte como si nunca hubiera dejado de jugarlo.

## <a name="game-states-initialization"></a>Inicialización de estados de juego

En la inicialización del juego, no debes pensar solo en el comienzo del juego desde el principio, sino también en un reinicio después de que se haya pausado o finalizado. El juego de muestra siempre guarda el estado del juego, dándole la apariencia de que ha seguido en ejecución. 

En estado suspendido, el juego se suspende, pero los recursos del juego siguen en la memoria. 

De igual modo, el evento de reanudación debe asegurar que el juego de muestra se retome donde se suspendió o terminó por última vez. Cuando el juego de muestra se reinicia después de haber terminado, se inicia con normalidad y después determina cuál fue el último estado conocido para que el jugador pueda seguir progresando con el juego inmediatamente.

Dependiendo del estado, al jugador se le presentan distintas opciones. 

* Si el juego se reanuda en mitad de un nivel, aparece como pausado, y la superposición muestra una opción de continuar.
* Si el juego se reanuda en un estado en que el juego estaba completado, muestra las puntuaciones más altas y una opción de jugar un nuevo juego.
* Por último, si el juego se reanuda antes de que se inicie un nivel, la superposición presenta al usuario una opción de inicio.

La muestra de juego no distingue si el propio juego se inicia en frío, es decir, que se inicia por primera vez sin un evento de suspensión, o se reanuda desde un estado suspendido. Este es el diseño adecuado para cualquier aplicación para UWP.

En este ejemplo, se produce la inicialización del estado de juego en [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method).

Este es un diagrama de flujo para ayudarte a visualizar el flujo y trata tanto sobre la inicialización como sobre el bucle de actualización.

* La inicialización comienza en el nodo __inicio__ al comprobar el estado actual del juego. Para el código del juego, ve a [__GameMain::InitializeGameState__](#gamemaininitializegamestate-method).
* Para obtener más información sobre el bucle de actualización, ve a [Motor de actualización de juego](#update-game-engine). Para el código del juego, ve a [__Active__](#appupdate-method).

![máquina de estado principal para el juego](images/simple-dx-game-flow-statemachine.png)

### <a name="gamemaininitializegamestate-method"></a>Método GameMain::InitializeGameState

El método __InitializeGameState__ se llama desde la clase de constructor [__GameMain__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L32-L131), que se llama cuando se crea el objeto de clase __GameMain__ en el método [__App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123).

```cpp

GameMain::GameMain(...)
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...

    create_task([this]()
    {
        ...

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState(); //Initialization of game states occurs here.
        
        ...
    
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
        
    }, task_continuation_context::use_current());
}

```

```cpp

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle of a level.
        // We are waiting for the user to continue the game.
        //...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        //...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        m_uiControl->SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    m_uiControl->ShowGameInfoOverlay();
}

```

## <a name="update-game-engine"></a>Motor de actualización de juego

En el método [__App:: Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L127-L130), llama a [__GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202). Dentro de este método, el ejemplo ha implementado una máquina de estado básica para controlar todas las acciones principales que puede llevar a cabo un jugador. En el nivel más general, la máquina de estado se encarga de cargar un juego, jugar un nivel específico o continuar un nivel después de que el juego se haya pausado (por el sistema o el jugador).

En la muestra de juego, existen 3 estados principales (__UpdateEngineState__) en los que puede estar el juego:

1. __En espera de recursos__: El bucle del juego es cíclico, incapaz de realizar una transición hasta que disponga de recursos (en concreto, recursos gráficos). Cuando las tareas asíncronas de carga de recursos finalizan, el estado se actualiza a __ResourcesLoaded__. Esto suele ocurrir entre niveles cuando el nivel está cargando nuevos recursos del disco, servidor de juegos o back-end de la nube. En la muestra de juego, simulamos este comportamiento, dado que la muestra no necesita ningún recurso adicional por nivel en ese momento.
2. __Waiting for press__: el bucle del juego está dando vueltas, esperando una entrada específica del usuario. Esta entrada es una acción del jugador para cargar un juego, iniciar un nivel o continuar un nivel. El código de muestra se refiere a estos subestados como valores de enumeración __PressResultState__.
3. En __Dynamics__: El bucle del juego se está ejecutando mientras el usuario juega. Cuando el usuario está jugando, el juego comprueba 3 condiciones con las que puede realizar una transición: 
    * __TimeExpired__: acaba el tiempo establecido para un nivel
    * __LevelComplete__: completado de un nivel por parte del jugador 
    * __GameComplete__: completado de todos los niveles por parte del jugador

Tu juego es simplemente una máquina de estado que contiene varias máquinas de estado más pequeñas. Para cada estado específico, debe estar definida por criterios muy específicos. La forma en que pasa de un estado a otro debe basarse en entradas de usuario o en acciones del sistema discretas (como la carga de recursos gráficos). Al diseñar para tu juego, considera la posibilidad de plantear el flujo de juego completo, para asegurarte de que hayas atendido a todas las posibles acciones que el usuario o el sistema pueden llevar a cabo. Los juegos pueden ser muy complicados, por lo que una máquina de estado es una eficaz herramienta para ayudar a visualizar y hacer más manejable esta complejidad.

Vamos a dar un vistazo a los códigos del bucle de actualización que viene a continuación.

### <a name="appupdate-method"></a>Método App::Update

Estructura de la máquina de estado usada para actualizar el motor de juego

```cpp
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        //...
        break;

    case UpdateEngineState::ResourcesLoaded:
        //...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            //...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when the player is playing, the work is handled by this Simple3DGame::RunGame method.
            switch (runState)
            {
            case GameState::TimeExpired:
                //...
                break;

            case GameState::LevelComplete:
                //...
                break;

            case GameState::GameComplete:
                //...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-user-interface"></a>Actualizar la interfaz de usuario

Es necesario mantener informado al usuario del estado del sistema y permitir que el estado del juego cambie dependiendo de las acciones del jugador y las reglas que definen el juego. Muchos juegos, incluyendo esta muestra de juego suelen utilizan elementos de interfaz de usuario (UI) para presentar esta información al jugador. La interfaz de usuario contiene representaciones del estado del juego y otra información de juego específica como la puntuación, la munición o el número de intentos restantes. La UI recibe también el nombre de superposición, porque se representa independientemente de la canalización de gráficos principal y se coloca por encima de la proyección en 3D. También se presenta información de la interfaz de usuario como una pantalla de visualización frontal (HUD) para permitir que los usuarios obtengan estas información sin quitar la mirada del área de juego principal. En el juego de muestra, creamos esta superposición con las API de Direct2D. También podemos crearla usando XAML, como se explica en el tema sobre cómo [extender la muestra de juego](tutorial-resources.md).

La interfaz de usuario consta de dos componentes:

-   La HUD que contiene la puntuación y la información sobre el estado actual del juego.
-   El mapa de bits de pausa, que es un rectángulo negro con texto superpuesto durante el estado de pausa/suspensión del juego. Se trata de la superposición del juego, que analizaremos con más detalle en el tema sobre cómo [agregar una interfaz de usuario](tutorial--adding-a-user-interface.md).

Como era de prever, la superposición también cuenta con una máquina de estado. La superposición puede mostrar el inicio de un nivel o un mensaje de fin de juego. Es, básicamente, un Canvas donde incluir cualquier información sobre el estado del juego que se mostrará al jugador cuando el juego esté en pausa o suspendido.

La superposición representada puede ser una de las seis pantallas, según el estado del juego: 
1. Pantalla de carga de recursos al inicio del juego
2. Pantalla de juego de estadísticas del juego
3. Pantalla de mensaje de inicio de nivel
4. Pantalla de final de juego cuando todos los niveles se completan sin tiempo máximo
5. Pantalla de final de juego cuando se agota el tiempo
6. Pantalla de menú de pausa

Separar la interfaz de usuario de la canalización gráfica del juego te permite trabajar en ella con independencia del motor de representación gráfica del juego y reduce significativamente la complejidad del código del juego.

La muestra de juego estructura la máquina de estado de la superposición de la siguiente manera:

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        //...
        break;

    case GameInfoOverlayState::LevelStart:
        //...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        //...
        break;

    case GameInfoOverlayState::GameOverExpired:
        //...
        break;

    case GameInfoOverlayState::Pause:
        //...
        break;
    }
}
```

## <a name="events-handling"></a>Control de eventos

Nuestro código de muestra registró varios controladores para eventos específicos en **Initialize**, **SetWindow** y **Load**, en App.cpp. Estos son los eventos importantes que hay que trabajar antes de que podamos agregar desarrollo de mecánicas de juego o gráficos de inicio. Estos eventos son fundamentales para una correcta experiencia de aplicación UWP. Dado que una aplicación para UWP se puede activar, desactivar, cambiar de tamaño, acoplar, desacoplar, suspender o reanudar en cualquier momento, el juego debe registrar estos eventos tan pronto como pueda y controlarlos de modo que el jugador disfrute de una experiencia predecible y sin interrupciones.

Estos son los controladores de eventos usados en esta muestra y los eventos que controlan.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Controlador de eventos</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">OnActivated</td>
<td align="left">Controla <a href="https://msdn.microsoft.com/library/windows/apps/br225018"><strong>CoreApplicationView::Activated</strong></a>. La aplicación del juego se coloca en primer plano, de modo que la ventana principal está activa.</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">Controla <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>Graphics::Display::DisplayInformation::DpiChanged</strong></a>. Los PPP de la pantalla han cambiado, por lo que la aplicación del juego ajusta sus recursos en consonancia.
<div class="alert">
<strong>Nota</strong>[<strong>CoreWindow</strong>] (https://msdn.microsoft.com/library/windows/desktop/hh404559) coordenadas son en DIP (píxeles independientes del dispositivo) para [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987). Como resultado, se debe notificar a Direct2D del cambio en PPP para mostrar cualquier primitivo o activo 2D correctamente.
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">Controla <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>Graphics::Display::DisplayInformation::OrientationChanged</strong></a>. La orientación de los cambios de pantalla y la representación debe actualizarse.</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">Controla <a href="https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>Graphics::Display::DisplayInformation::DisplayContentsInvalidated</strong></a>. La pantalla requiere volver a dibujar y tu juego necesita volver a representarse.</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Controla <a href="https://msdn.microsoft.com/library/windows/apps/br205859"><strong>CoreApplication::Resuming</strong></a>. La aplicación del juego restaura el juego desde un estado suspendido.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Controla <a href="https://msdn.microsoft.com/library/windows/apps/br205860"><strong>CoreApplication::Suspending</strong></a>. La aplicación del juego guarda su estado en disco. Tiene 5 segundos para guardar el estado en el almacenamiento.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Controla <a href="https://msdn.microsoft.com/library/windows/apps/hh701591"><strong>CoreWindow::VisibilityChanged</strong></a>. La aplicación del juego ha cambiado la visibilidad y ha pasado a ser visible u otra aplicación visible la ha convertido en invisible.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Controla <a href="https://msdn.microsoft.com/library/windows/apps/br208255"><strong>CoreWindow::Activated</strong></a>. La ventana principal de la aplicación del juego se ha desactivado o activado, por lo que debe quitar el foco y pausar el juego, o recuperar el foco. En ambos casos, la superposición indica que el juego está en pausa.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Controla <a href="https://msdn.microsoft.com/library/windows/apps/br208261"><strong>CoreWindow::Closed</strong></a>. La aplicación del juego cierra la ventana principal y suspende el juego.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Controla <a href="https://msdn.microsoft.com/library/windows/apps/br208283"><strong>CoreWindow::SizeChanged</strong></a>. La aplicación del juego reasigna los recursos gráficos y la superposición para ajustarse al cambio de tamaño y después actualiza el destino de representación.</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>Pasos siguientes

En este tema, hemos analizado cómo se administra el flujo de juego general, usando los estados del juego y que un juego se compone de varias máquinas de estado diferentes. También hemos aprendido cómo actualizar la interfaz de usuario y administrar los controladores principales de eventos de la aplicación. Ahora estamos listos para entrar en el bucle de representación, el juego y sus mecánicas.
 
Puedes ir viendo el resto de componentes que conforman el juego en cualquier orden:
* [Definir el objeto principal del juego](tutorial--defining-the-main-game-loop.md)
* [Marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md)
* [Marco de representación II: Representación de juego](tutorial-game-rendering.md)
* [Agregar una interfaz de usuario](tutorial--adding-a-user-interface.md)
* [Agregar controles](tutorial--adding-controls.md)
* [Agregar sonido](tutorial--adding-sound.md)