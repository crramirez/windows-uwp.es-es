---
title: Definir el marco de la aplicación para UWP del juego
description: El primer paso para codificar un juego de Plataforma universal de Windows (UWP) es crear el marco de trabajo que permite que el objeto de aplicación interactúe con Windows.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.date: 06/24/2020
ms.topic: article
keywords: windows 10, uwp, games, directx
ms.localizationpriority: medium
ms.openlocfilehash: 2d762aebeaa5c97c1b23a91f0765cb5c09a91b46
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163059"
---
#  <a name="define-the-games-uwp-app-framework"></a>Definir el marco de la aplicación para UWP del juego

> [!NOTE]
> Este tema forma parte de la serie de tutoriales de [creación de una plataforma universal de Windows sencilla (UWP) con DirectX](tutorial--create-your-first-uwp-directx-game.md) . El tema de ese vínculo establece el contexto de la serie.

El primer paso para codificar un juego de Plataforma universal de Windows (UWP) es crear el marco de trabajo que permite que el objeto de aplicación interactúe con Windows, incluidas Windows Runtime características como el control de eventos de suspender y reanudar, los cambios en el foco de la ventana y el ajuste.

## <a name="objectives"></a>Objetivos

* Configure el marco de trabajo para un juego de DirectX Plataforma universal de Windows (UWP) e implemente el equipo de estado que define el flujo de juego global.

> [!NOTE]
> Para seguir este tema, busque en el código fuente del juego de ejemplo [Simple3DGameDX](/samples/microsoft/windows-universal-samples/simple3dgamedx/) que descargó.

## <a name="introduction"></a>Introducción

En el tema [configurar el proyecto de juego](tutorial--setting-up-the-games-infrastructure.md) , se presentó la función **wWinMain** , así como las interfaces [**IFrameworkViewSource**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) y [**IFrameworkView**](/uwp/api/windows.applicationmodel.core.iframeworkviewsource) . Hemos aprendido que la clase **App** (que puede ver definida en el `App.cpp` archivo de código fuente en el proyecto **Simple3DGameDX** ) actúa como generador de proveedores de *vistas* y *proveedor de vistas*.

En este tema se recoge a partir de ahí y se explican con mucha más detalle cómo la clase de **aplicación** de un juego debe implementar los métodos de **IFrameworkView**.

## <a name="the-appinitialize-method"></a>El método App:: Initialize

Al iniciarse la aplicación, el primer método que llama a Windows es nuestra implementación de [**IFrameworkView:: Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize).

La implementación debe controlar los comportamientos más fundamentales de un juego de UWP, como asegurarse de que el juego puede controlar un evento de suspensión (y un posible reanudación posterior) mediante la suscripción a esos eventos. Aquí también tenemos acceso al dispositivo del adaptador de pantalla, por lo que podemos crear recursos de gráficos que dependan del dispositivo.

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    applicationView.Activated({ this, &App::OnActivated });

    CoreApplication::Suspending({ this, &App::OnSuspending });

    CoreApplication::Resuming({ this, &App::OnResuming });

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

Evite los punteros sin formato siempre que sea posible (y casi siempre es posible).

- En el caso de los tipos de Windows Runtime, a menudo puede evitar los punteros de forma conjunta y simplemente construir un valor en la pila. Si necesita un puntero, use [**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) (en breve veremos un ejemplo).
- Para los punteros únicos, use [**STD:: unique_ptr**](/cpp/cpp/how-to-create-and-use-unique-ptr-instances) y **std:: make_unique**.
- Para los punteros compartidos, use [**STD:: shared_ptr**](/cpp/cpp/how-to-create-and-use-shared-ptr-instances) y **std:: make_shared**.

## <a name="the-appsetwindow-method"></a>El método App:: SetWindow

Después de la **inicialización**, Windows llama a nuestra implementación de [**IFrameworkView:: SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), pasando un objeto [**CoreWindow**](/uwp/api/windows.ui.core.corewindow) que representa la ventana principal del juego.

En **App:: SetWindow**, se suscribe a eventos relacionados con ventanas y se configuran algunos comportamientos de ventana y visualización. Por ejemplo, creamos un puntero del mouse (a través de la clase [**CoreCursor**](/uwp/api/windows.ui.core.corecursor) ), que puede ser utilizado por los controles de mouse y táctil. También pasamos el objeto Window a nuestro objeto de recursos dependientes del dispositivo.

Hablaremos más sobre el control de eventos en el tema [Administración del flujo de juegos](tutorial-game-flow-management.md#event-handling) .

```cppwinrt
void SetWindow(CoreWindow const& window)
{
    //CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();

    window.PointerCursor(CoreCursor(CoreCursorType::Arrow, 0));

    PointerVisualizationSettings visualizationSettings{ PointerVisualizationSettings::GetForCurrentView() };
    visualizationSettings.IsContactFeedbackEnabled(false);
    visualizationSettings.IsBarrelButtonFeedbackEnabled(false);

    m_deviceResources->SetWindow(window);

    window.Activated({ this, &App::OnWindowActivationChanged });

    window.SizeChanged({ this, &App::OnWindowSizeChanged });

    window.Closed({ this, &App::OnWindowClosed });

    window.VisibilityChanged({ this, &App::OnVisibilityChanged });

    DisplayInformation currentDisplayInformation{ DisplayInformation::GetForCurrentView() };

    currentDisplayInformation.DpiChanged({ this, &App::OnDpiChanged });

    currentDisplayInformation.OrientationChanged({ this, &App::OnOrientationChanged });

    currentDisplayInformation.StereoEnabledChanged({ this, &App::OnStereoEnabledChanged });

    DisplayInformation::DisplayContentsInvalidated({ this, &App::OnDisplayContentsInvalidated });
}
```

## <a name="the-appload-method"></a>El método App:: Load

Ahora que se ha establecido la ventana principal, se llama a la implementación de [**IFrameworkView:: Load**](/uwp/api/windows.applicationmodel.core.iframeworkview.load) . La **carga** es un lugar mejor para realizar una captura previa de datos de juegos o recursos que **inicializar** y **SetWindow**.

```cppwinrt
void Load(winrt::hstring const& /* entryPoint */)
{
    if (!m_main)
    {
        m_main = winrt::make_self<GameMain>(m_deviceResources);
    }
}
```

Como puede ver, el trabajo real se delega en el constructor del objeto **GameMain** que se realiza aquí. La clase **GameMain** se define en `GameMain.h` y `GameMain.cpp` .

### <a name="the-gamemaingamemain-constructor"></a>Constructor GameMain:: GameMain

El constructor **GameMain** (y las demás funciones miembro a las que llama) comienza un conjunto de operaciones de carga asincrónicas para crear los objetos de juego, cargar los recursos de gráficos e inicializar el equipo de estado del juego. También realizamos los preparativos necesarios antes de comenzar el juego, como establecer los Estados iniciales o los valores globales.

Windows impone un límite en el tiempo que puede tardar el juego antes de comenzar a procesar la entrada. Por tanto, el uso de asincrónicos, como hacemos aquí, significa que la **carga** puede volver rápidamente mientras el trabajo que ha comenzado continúa en segundo plano. Si la carga tarda mucho tiempo, o si hay muchos recursos, proporcionar a los usuarios una barra de progreso actualizada con frecuencia es una buena idea. 

Si no está familiarizado con la programación asincrónica, vea [simultaneidad y operaciones asincrónicas con C++/WinRT](../cpp-and-winrt-apis/concurrency.md).

```cppwinrt
GameMain::GameMain(std::shared_ptr<DX::DeviceResources> const& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = std::make_shared<GameRenderer>(m_deviceResources);
    m_game = std::make_shared<Simple3DGame>();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = std::make_shared<MoveLookController>(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(GameUIConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameUIConstants::TouchRectangleSize, bounds.Height - GameUIConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    // Asynchronously initialize the game class and load the renderer device resources.
    // By doing all this asynchronously, the game gets to its main loop more quickly
    // and in parallel all the necessary resources are loaded on other threads.
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    auto lifetime = get_strong();

    m_game->Initialize(m_controller, m_renderer);

    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);

    // The finalize code needs to run in the same thread context
    // as the m_renderer object was created because the D3D device context
    // can ONLY be accessed on a single thread.
    // co_await of an IAsyncAction resumes in the same thread context.
    m_renderer->FinalizeCreateGameDeviceResources();

    InitializeGameState();

    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        // In the middle of a game so spin up the async task to load the level.
        co_await m_game->LoadLevelAsync();

        // The m_game object may need to deal with D3D device context work so
        // again the finalize code needs to run in the same thread
        // context as the m_renderer object was created because the D3D
        // device context can ONLY be accessed on a single thread.
        m_game->FinalizeLoadLevel();
        m_game->SetCurrentLevelToSavedState();
        m_updateState = UpdateEngineState::ResourcesLoaded;
    }
    else
    {
        // The game is not in the middle of a level so there aren't any level
        // resources to load.
    }

    // Since Game loading is an async task, the app visual state
    // may be too small or not have focus. Put the state machine
    // into the correct state to reflect these cases.

    if (m_deviceResources->GetLogicalSize().Width < GameUIConstants::MinPlayableWidth)
    {
        m_updateStateNext = m_updateState;
        m_updateState = UpdateEngineState::TooSmall;
        m_controller->Active(false);
        m_uiControl->HideGameInfoOverlay();
        m_uiControl->ShowTooSmall();
        m_renderNeeded = true;
    }
    else if (!m_haveFocus)
    {
        m_updateStateNext = m_updateState;
        m_updateState = UpdateEngineState::Deactivated;
        m_controller->Active(false);
        m_uiControl->SetAction(GameInfoOverlayCommand::None);
        m_renderNeeded = true;
    }
}

void GameMain::InitializeGameState()
{
    // Set up the initial state machine for handling Game playing state.
    ...
}
```

A continuación se muestra un esquema de la secuencia de trabajo que el constructor inicia.

- Cree e inicialice un objeto de tipo **GameRenderer**. Para obtener más información, vea [marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md).
- Cree e inicialice un objeto de tipo **Simple3DGame**. Para obtener más información, vea [definir el objeto principal del juego](tutorial--defining-the-main-game-loop.md).
- Cree el objeto Game UI control y muestre la superposición de la información del juego para mostrar una barra de progreso a medida que se cargan los archivos de recursos. Para obtener más información, consulte [Agregar una interfaz de usuario](tutorial--adding-a-user-interface.md).
- Cree un objeto de controlador para leer la entrada del controlador (toque, mouse o controlador inalámbrico de Xbox). Para obtener más información, vea [Agregar controles](tutorial--adding-controls.md).
- Defina dos áreas rectangulares en las esquinas inferior izquierda e inferior derecha de la pantalla para los controles de movimiento y toque de cámara, respectivamente. El reproductor usa el rectángulo inferior izquierdo (definido en la llamada a **SetMoveRect**) como un panel de control virtual para mover la cámara hacia delante y hacia atrás, y de lado a lado. El rectángulo inferior derecho (definido por el método **SetFireRect** ) se usa como un botón virtual para activar la munición.
- Use corrutinas para interrumpir la carga de recursos en fases independientes. El acceso al contexto de dispositivo de Direct3D está restringido al subproceso en el que se creó el contexto de dispositivo. Aunque el acceso al dispositivo Direct3D para la creación de objetos es libre de subprocesos. Por consiguiente, la corutina **GameRenderer:: CreateGameDeviceResourcesAsync** puede ejecutarse en un subproceso independiente de la tarea de finalización (**GameRenderer:: FinalizeCreateGameDeviceResources**), que se ejecuta en el subproceso original.
- Usamos un patrón similar para cargar recursos de nivel con **Simple3DGame:: LoadLevelAsync** y **Simple3DGame:: FinalizeLoadLevel**.

Veremos más de **GameMain:: InitializeGameState** en el tema siguiente ([Administración del flujo de juegos](tutorial-game-flow-management.md)).

## <a name="the-apponactivated-method"></a>El método App:: Onactivated

A continuación, se genera el evento [**CoreApplicationView:: Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) . Por lo tanto, se llama a cualquier controlador de eventos **onactivated** que tenga (como nuestro método **App:: onactivated** ).

```cppwinrt
void OnActivated(CoreApplicationView const& /* applicationView */, IActivatedEventArgs const& /* args */)
{
    CoreWindow window = CoreWindow::GetForCurrentThread();
    window.Activate();
}
```

El único trabajo que hacemos aquí es activar el [**CoreWindow**](/uwp/api/windows.ui.core.corewindow)principal. Como alternativa, puede elegir hacerlo en **App:: SetWindow**.

## <a name="the-apprun-method"></a>El método App:: Run

**Initialize**, **SetWindow**y **Load** tienen establecida la fase. Ahora que el juego está en funcionamiento, se llama a la implementación de [**IFrameworkView:: Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) .

```cppwinrt
void Run()
{
    m_main->Run();
}
```

De nuevo, el trabajo se delega a **GameMain**.

### <a name="the-gamemainrun-method"></a>El método GameMain:: Run

**GameMain:: Run** es el bucle principal del juego; puede encontrarlo en `GameMain.cpp` . La lógica básica es que mientras la ventana del juego siga abierta, envíe todos los eventos, actualice el temporizador y, a continuación, represente y presente los resultados de la canalización de gráficos. Además, los eventos usados para realizar la transición entre Estados de juego se envían y procesan.

El código aquí también está relacionado con dos de los Estados de la máquina de Estados del motor de juego.

- **UpdateEngineState::D eactivated**. Esto especifica que la ventana de juego está desactivada (ha perdido el foco) o está ajustada. 
- **UpdateEngineState:: TooSmall**. Esto especifica que el área cliente es demasiado pequeña para representar el juego.

En cualquiera de estos Estados, el juego suspende el procesamiento de eventos y espera a que la ventana se Centre, se desajuste o cambie de tamaño.

Cuando el juego tiene el foco, debes controlar cada evento en la cola de mensajes tan pronto como llegue, y por tanto debes llamar a [**CoreWindowDispatch.ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) con la opción **ProcessAllIfPresent**. Otras opciones pueden producir retrasos en el procesamiento de eventos de mensajes, lo que puede hacer que el juego no responda o que los comportamientos táctiles parezcan lentos.

Cuando el juego no está visible, suspendido o ajustado, no desea que consuma ningún recurso en el ciclo para enviar mensajes que nunca llegarán. En este caso, el juego debe usar la opción **ProcessOneAndAllPending** . Esa opción se bloquea hasta que recibe un evento y, a continuación, procesa ese evento (así como cualquier otro que llegue a la cola de procesos durante el procesamiento de la primera). Después, **CoreWindowDispatch. ProcessEvents** vuelve inmediatamente después de que se haya procesado la cola.

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::TooSmall:
                if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    WaitingForResourceLoading();
                    m_renderNeeded = true;
                }
                else if (m_updateStateNext == UpdateEngineState::ResourcesLoaded)
                {
                    // In the device lost case, we transition to the final waiting state
                    // and make sure the display is updated.
                    switch (m_pressResult)
                    {
                    case PressResultState::LoadGame:
                        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
                        break;

                    case PressResultState::PlayLevel:
                        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                        break;

                    case PressResultState::ContinueLevel:
                        SetGameInfoOverlay(GameInfoOverlayState::Pause);
                        break;
                    }
                    m_updateStateNext = UpdateEngineState::WaitingForPress;
                    m_uiControl->ShowGameInfoOverlay();
                    m_renderNeeded = true;
                }

                if (!m_renderNeeded)
                {
                    // The App is not currently the active window and not in a transient state so just wait for events.
                    CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="the-appuninitialize-method"></a>El método App:: UnInitialize

Cuando finaliza el juego, se llama a la implementación de [**IFrameworkView:: UnInitialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize) . Esta es la oportunidad de realizar una limpieza. Al cerrar la ventana de la aplicación no se elimina el proceso de la aplicación. pero, en su lugar, escribe el estado del singleton de la aplicación en la memoria. Si se debe producir algo especial cuando el sistema reclama esta memoria, incluida cualquier limpieza especial de recursos, coloque el código para esa limpieza en la anulación de la **inicialización**.

En nuestro caso, **App:: UnInitialize** no es operativo.

```cpp
void Uninitialize()
{
}
```

## <a name="tips"></a>Sugerencias

Al desarrollar su propio juego, diseñe el código de inicio en torno a los métodos descritos en este tema. Esta es una lista sencilla de sugerencias básicas para cada método.

- Utilice **Initialize** para asignar las clases principales y conecte los controladores de eventos básicos.
- Use **SetWindow** para suscribirse a cualquier evento específico de la ventana y para pasar la ventana principal al objeto de recursos dependientes del dispositivo para que pueda usar esa ventana al crear una cadena de intercambio.
- Utilice **Load** para controlar el resto de la configuración y para iniciar la creación asincrónica de objetos y la carga de recursos. Si necesita crear archivos o datos temporales, como activos generados por procedimientos, hágalo aquí también.

## <a name="next-steps"></a>Pasos siguientes

En este tema se ha tratado parte de la estructura básica de un juego de UWP que usa DirectX. Es una buena idea tener en cuenta estos métodos, ya que haremos referencia a algunos de ellos en temas posteriores.

En el siguiente tema &mdash; [Administración del flujo de juegos](tutorial-game-flow-management.md) &mdash; , veremos una visión detallada de cómo administrar los Estados de los juegos y el control de eventos para mantener el flujo del juego.