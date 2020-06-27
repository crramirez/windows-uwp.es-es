---
title: Administración de flujo de juegos
description: Obtenga información sobre cómo inicializar los Estados de juego, controlar eventos y configurar el bucle de actualización de juegos.
ms.assetid: 6c33bf09-b46a-4bb5-8a59-ca83ce257eb3
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, games, directx
ms.localizationpriority: medium
ms.openlocfilehash: 181eca743a9ccdc76ebfc1302e8bb04d85a32269
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409634"
---
# <a name="game-flow-management"></a>Administración de flujo de juegos

> [!NOTE]
> Este tema forma parte de la serie de tutoriales de [creación de una plataforma universal de Windows sencilla (UWP) con DirectX](tutorial--create-your-first-uwp-directx-game.md) . El tema de ese vínculo establece el contexto de la serie.

El juego tiene ahora una ventana, ha registrado algunos controladores de eventos y ha cargado los recursos de forma asincrónica. En este tema se explica el uso de los Estados de juego, cómo administrar los Estados de juegos de claves específicos y cómo crear un bucle de actualización para el motor de juegos. A continuación, obteneremos información sobre el flujo de la interfaz de usuario y, por último, comprenderemos más sobre los controladores de eventos necesarios para un juego de UWP.

## <a name="game-states-used-to-manage-game-flow"></a>Estados de juego usados para administrar el flujo de juegos

Hacemos uso de los Estados de juego para administrar el flujo del juego.

Cuando el juego de ejemplo **Simple3DGameDX** se ejecuta por primera vez en un equipo, se encuentra en un estado en el que no se ha iniciado ningún juego. Las veces posteriores en las que se ejecuta el juego, pueden estar en cualquiera de estos Estados.

- No se ha iniciado ningún juego o el juego está entre niveles (la puntuación máxima es cero).
- El bucle del juego se está ejecutando y está en medio de un nivel.
- El bucle del juego no se está ejecutando porque se ha completado un juego (la puntuación alta tiene un valor distinto de cero).

El juego puede tener tantos Estados como sea necesario. Pero recuerde que puede terminar en cualquier momento. Y cuando se reanuda, el usuario espera que se reanude en el estado en que se encontraba cuando se cerró.

## <a name="game-state-management"></a>Administración de Estados de juegos

Por lo tanto, durante la inicialización del juego, deberá admitir el inicio en frío y reanudar el juego después de detenerlo en vuelo. El ejemplo **Simple3DGameDX** siempre guarda su estado de juego para dar la impresión de que nunca se detuvo.

En respuesta a un evento de suspensión, el juego se suspende, pero los recursos del juego todavía están en la memoria. Del mismo modo, se controla el evento de reanudación para asegurarse de que el juego de ejemplo recoge en el estado en que se encontraba cuando se suspendió o finalizó. Dependiendo del estado, al jugador se le presentan distintas opciones. 

- Si el juego reanuda el nivel medio, aparece en pausa y la superposición ofrece la opción de continuar.
- Si el juego se reanuda en un estado en el que se completa el juego, se muestran las puntuaciones altas y una opción para reproducir un juego nuevo.
- Por último, si el juego se reanuda antes de que se haya iniciado un nivel, la superposición presenta una opción de inicio al usuario.

El juego de ejemplo no distingue si el juego está en frío, se inicia por primera vez sin un evento de suspensión o se reanuda desde un estado suspendido. Este es el diseño adecuado para cualquier aplicación para UWP.

En este ejemplo, la inicialización de los Estados de juego aparece en [**GameMain:: InitializeGameState**](#the-gamemaininitializegamestate-method) (se muestra un esquema de ese método en la sección siguiente).

Este es un diagrama de flujo que le ayudará a visualizar el flujo. Abarca la inicialización y el bucle de actualización.

- La inicialización comienza en el nodo de **Inicio** cuando se comprueba el estado actual del juego. Para ver el código de juego, consulte [**GameMain:: InitializeGameState**](#the-gamemaininitializegamestate-method) en la sección siguiente.
* Para obtener más información sobre el bucle de actualización, vaya a [actualizar el motor de juegos](#update-game-engine). Para el código de juego, vaya a [**GameMain:: Update**](#the-gamemainupdate-method).

![the main state machine fo our game](images/simple-dx-game-flow-statemachine.png)

### <a name="the-gamemaininitializegamestate-method"></a>El método GameMain:: InitializeGameState

Se llama indirectamente al método **GameMain:: InitializeGameState** mediante el constructor de la clase **GameMain** , que es el resultado de crear una instancia de **GameMain** en **App:: Load**.

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) : ...
{
    m_deviceResources->RegisterDeviceNotify(this);
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();
    ...
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        ...
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        ...
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        ...
    }
    m_uiControl->ShowGameInfoOverlay();
}
```

## <a name="update-game-engine"></a>Actualizar el motor de juegos

El método **App:: Run** llama a **GameMain:: Run**. Dentro de **GameMain:: Run** es una máquina de Estados básica para controlar todas las acciones principales que puede realizar un usuario. El nivel más alto de esta máquina de Estados trata con la carga de un juego, la reproducción de un nivel específico o la continuación de un nivel después de que el juego esté en pausa (por el sistema o por el usuario).

En el juego de ejemplo, hay 3 Estados principales (representados por la enumeración **UpdateEngineState** ) en los que puede estar el juego.

1. **UpdateEngineState:: WaitingForResources**. El bucle del juego está ciclando, incapaz de realizar la transición hasta que disponga de recursos (en concreto recursos gráficos). Cuando se completan las tareas de carga de recursos asincrónicos, se actualiza el estado a **UpdateEngineState:: ResourcesLoaded**. Esto suele ocurrir entre niveles cuando el nivel carga recursos nuevos desde el disco, desde un servidor de juegos o desde un back-end en la nube. En el juego de ejemplo, simulamos este comportamiento, ya que el ejemplo no necesita ningún recurso adicional por nivel en ese momento.
2. **UpdateEngineState:: WaitingForPress**. El bucle del juego está ciclando, esperando a la entrada específica del usuario. Esta entrada es una acción del reproductor para cargar un juego, para iniciar un nivel o para continuar un nivel. El código de ejemplo hace referencia a estos subestados a través de la enumeración **PressResultState** .
3. **UpdateEngineState::D ynamics**. El bucle del juego se está ejecutando mientras el usuario juega. Mientras se reproduce el usuario, el juego comprueba tres condiciones en las que se puede realizar la transición: 
 - **GameState:: TimeExpired**. Expiración del límite de tiempo de un nivel.
 - **GameState:: LevelComplete**. Finalización de un nivel por parte del reproductor.
 - **GameState:: GameComplete**. Finalización de todos los niveles del reproductor.

Un juego es simplemente una máquina de Estados que contiene varias máquinas de estado más pequeñas. Cada estado específico debe definirse mediante criterios muy específicos. Las transiciones de un estado a otro deben estar basadas en entradas de usuario discretas o acciones del sistema (como la carga de recursos de gráficos).

Mientras planea el juego, considere la posibilidad de dibujar todo el flujo de juego para asegurarse de que ha solucionado todas las acciones posibles que puede realizar el usuario o el sistema. Un juego puede ser muy complicado, por lo que una máquina de Estados es una herramienta eficaz para ayudarle a visualizar esta complejidad y hacer que sea más fácil de administrar.

Echemos un vistazo al código para el bucle de actualización.

### <a name="the-gamemainupdate-method"></a>El método GameMain:: Update

Esta es la estructura de la máquina de Estados utilizada para actualizar el motor de juegos.

```cppwinrt
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update(); 

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        ...
        break;

    case UpdateEngineState::ResourcesLoaded:
        ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            ...
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            ...
        }
        else
        {
            // When the player is playing, work is done by Simple3DGame::RunGame.
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                ...
                break;

            case GameState::LevelComplete:
                ...
                break;

            case GameState::GameComplete:
                ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(
                m_renderer->GameInfoOverlayUpperLeft(),
                m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller
            // until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}
```

## <a name="update-the-user-interface"></a>Actualizar la interfaz de usuario

Es necesario mantener informado al jugador del estado del sistema y permitir que el estado del juego cambie en función de las acciones del jugador y las reglas que definen el juego. Muchos juegos, incluido este juego de ejemplo, suelen usar elementos de la interfaz de usuario (UI) para presentar esta información al reproductor. La interfaz de usuario contiene representaciones del estado del juego y de otra información específica del juego, como la puntuación, la munición o el número de posibilidades que quedan. La interfaz de usuario también se denomina superposición porque se representa de forma independiente de la canalización de gráficos principal y se coloca en la parte superior de la proyección 3D.

Parte de la información de la interfaz de usuario también se presenta como una pantalla emergente (HUD) que permite al usuario ver esa información sin tener que desplegables completamente del área principal del juego. En el juego de ejemplo, creamos esta superposición mediante las API de Direct2D. Como alternativa, podríamos crear esta superposición mediante XAML, que analizaremos en [la extensión del juego de ejemplo](tutorial-resources.md).

Hay dos componentes en la interfaz de usuario.

- El HUD que contiene la puntuación y la información sobre el estado actual del juego.
- El mapa de bits de pausa, que es un rectángulo negro con texto superpuesto durante el estado de pausa/suspensión del juego. Se trata de la superposición del juego, que analizaremos con más detalle en el tema sobre cómo [agregar una interfaz de usuario](tutorial--adding-a-user-interface.md).

Como era de prever, la superposición también cuenta con una máquina de estado. La superposición puede mostrar un mensaje de inicio de nivel o de juego. Es esencialmente un lienzo en el que se puede enviar cualquier información sobre el estado del juego que queremos mostrar al jugador mientras el juego está pausado o suspendido.

La superposición presentada puede ser una de estas seis pantallas, dependiendo del estado del juego.

1. Pantalla de progreso de carga de recursos al principio del juego.
2. Pantalla de estadísticas de juegos.
3. Pantalla de mensaje de inicio de nivel.
4. Pantalla de juego cuando todos los niveles se completan sin que se agote el tiempo de espera.
5. Pantalla de juego en exceso cuando se agota el tiempo.
6. Pantalla del menú pausar.

Separar la interfaz de usuario de la canalización de gráficos del juego permite trabajar en ella independientemente del motor de representación de gráficos del juego y reduce la complejidad del código del juego de forma significativa.

Este es el modo en que el juego de ejemplo estructura el equipo de estado de la superposición.

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        ...
        break;

    case GameInfoOverlayState::LevelStart:
        ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        ...
        break;

    case GameInfoOverlayState::Pause:
        ...
        break;
    }
}
```

## <a name="event-handling"></a>Control de eventos

Como vimos en el tema [definir el marco de trabajo de la aplicación para UWP del juego](tutorial--building-the-games-uwp-app-framework.md) , muchos de los métodos del proveedor de vistas de la clase de **aplicación** registran los controladores de eventos. Estos métodos deben controlar correctamente estos eventos importantes antes de agregar mecánicas de juego o iniciar el desarrollo de gráficos.

El control adecuado de los eventos en cuestión es fundamental para la experiencia de la aplicación para UWP. Dado que una aplicación para UWP puede activarse, desactivarse, cambiar de tamaño, ajustarse, desacoplarse, suspenderse o reanudarse, el juego debe registrarse para estos eventos tan pronto como sea posible y controlarlos de forma que se mantenga la experiencia fluida y predecible para el jugador.

Estos son los controladores de eventos que se usan en este ejemplo y los eventos que controlan.

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
<td align="left">Controla <a href="/uwp/api/windows.applicationmodel.core.coreapplicationview.activated"><strong>CoreApplicationView:: Activated</strong></a>. La aplicación del juego se coloca en primer plano, de modo que la ventana principal está activa.</td>
</tr>
<tr class="even">
<td align="left">OnDpiChanged</td>
<td align="left">Controla los <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DpiChanged"><strong>gráficos::dble::D isplayinformation::D pichanged</strong></a>. El PPP de la pantalla ha cambiado y el juego ajusta sus recursos según corresponda.
<div class="alert">
<strong>Nota</strong>las coordenadas de <a href="/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow"><strong>CoreWindow</strong></a> están en píxeles independientes del dispositivo (DIP) para <a href="/windows/desktop/Direct2D/direct2d-overview">Direct2D</a>. Como resultado, se debe notificar a Direct2D el cambio en PPP para mostrar cualquier primitivo o activo 2D correctamente.
</div>
<div>
</div></td>
</tr>
<tr class="odd">
<td align="left">OnOrientationChanged</td>
<td align="left">Controla los <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_OrientationChanged"><strong>gráficos::D::D isplayinformation:: OrientationChanged</strong></a>. La orientación de los cambios de presentación y la representación deben actualizarse.</td>
</tr>
<tr class="even">
<td align="left">OnDisplayContentsInvalidated</td>
<td align="left">Controla los <a href="/uwp/api/windows.graphics.display.displayinformation#Windows_Graphics_Display_DisplayInformation_DisplayContentsInvalidated"><strong>gráficos::dble::D isplayinformation::D isplaycontentsinvalidated</strong></a>. La presentación requiere volver a dibujar y el juego debe representarse de nuevo.</td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Controla <a href="/uwp/api/windows.applicationmodel.core.coreapplication.resuming"><strong>CoreApplication:: RESUMING</strong></a>. La aplicación del juego restaura el juego desde un estado suspendido.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Controla <a href="/uwp/api/windows.applicationmodel.core.coreapplication.suspending"><strong>CoreApplication:: Suspending</strong></a>. La aplicación del juego guarda su estado en disco. Tiene 5 segundos para guardar el estado en el almacenamiento.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Controla <a href="/uwp/api/windows.ui.core.corewindow.visibilitychanged"><strong>CoreWindow:: VisibilityChanged</strong></a>. La aplicación del juego ha cambiado la visibilidad y ha pasado a ser visible u otra aplicación visible la ha convertido en invisible.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Controla <a href="/uwp/api/windows.ui.core.corewindow.activated"><strong>CoreWindow:: Activated</strong></a>. La ventana principal de la aplicación del juego se ha desactivado o activado, por lo que debe quitar el foco y pausar el juego, o recuperar el foco. En ambos casos, la superposición indica que el juego está en pausa.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Controla <a href="/uwp/api/windows.ui.core.corewindow.closed"><strong>CoreWindow:: Closed</strong></a>. La aplicación del juego cierra la ventana principal y suspende el juego.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Controla <a href="/uwp/api/windows.ui.core.corewindow.sizechanged"><strong>CoreWindow:: SizeChanged</strong></a>. La aplicación del juego reasigna los recursos gráficos y la superposición para ajustarse al cambio de tamaño y después actualiza el destino de representación.</td>
</tr>
</tbody>
</table>

## <a name="next-steps"></a>Pasos siguientes

En este tema, hemos visto cómo se administra el flujo de juegos general mediante los Estados de juegos y que un juego se compone de varias máquinas de Estados diferentes. También hemos visto cómo actualizar la interfaz de usuario y administrar los controladores de eventos de la aplicación clave. Ahora estamos preparados para profundizar en el bucle de representación, el juego y su mecánica.
 
Puede revisar los temas restantes que documentan este juego en cualquier orden.

- [Definir el objeto principal del juego](tutorial--defining-the-main-game-loop.md)
- [Marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md)
- [Plataforma de representación II: representación de juego](tutorial-game-rendering.md)
- [Agregar una interfaz de usuario](tutorial--adding-a-user-interface.md)
- [Agregar controles](tutorial--adding-controls.md)
- [Agregar sonido](tutorial--adding-sound.md)
