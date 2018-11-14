---
author: joannaleecy
title: Definir el marco de la aplicación para UWP del juego
description: La primera parte de codificar un juego de Plataforma universal de Windows (UWP) con DirectX es crear el marco que permite al objeto de juego interactuar con Windows.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
ms.author: joanlee
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, directx
ms.localizationpriority: medium
ms.openlocfilehash: 3444c71b4e4c610be0b7d92ac6d761340c5dd5c2
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6656380"
---
#  <a name="define-the-uwp-app-framework"></a>Definir el marco de la aplicación para UWP

Crea un marco que permita que el objeto de tu juego interactúe con Windows, incluyendo las propiedades de Windows Runtime como suspender/reanudar el control de eventos, los cambios en el enfoque de ventanas y el ajuste.

Para configurar este marco, primero debes obtener un proveedor de vista, de manera que el singleton de la aplicación, el objeto de Windows Runtime que define una instancia de tu aplicación en ejecución, puede acceder a los recursos gráficos que necesita. A través de Windows Runtime, tu juego también tiene conexión directa con la interfaz gráfica,lo que te permite especificar los recursos que necesitas y cómo controlarlos.

El objeto proveedor de vista implementa la interfaz __IFrameworkView__, que consta de una serie de métodos que deben estar configurados para crear esta muestra de juego.

Deberás implementar estos cinco métodos que el singleton de la aplicación llama:
* [__Initialize__](#initialize-the-view-provider)
* [__SetWindow__](#configure-the-window-and-display-behavior)
* [__Load__](#load-method-of-the-view-provider)
* [__Run__](#run-method-of-the-view-provider)
* [__Uninitialize__](#uninitialize-method-of-the-view-provider)

El método __Initialize__ se llama en el inicio de la aplicación. El método __SetWindow__ se llama después de __Initialize__. Y, después, se llama al método __Load__. El método __Run__ es el que ocurre cuando el juego se está ejecutando. Cuando el juego finaliza, se llama al método __Uninitialize__. Para obtener más información, consulta la [referencia de API __IFrameworkView__](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview). 

>[!Note]
>Si no has descargado el código del juego más reciente para esta muestra, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Ten en cuenta que este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Configurar el marco de un juego DirectX par la plataforma universal de Windows (UWP) e implementar la máquina de estados que define la experiencia de juego general.

## <a name="define-the-view-provider-factory-and-view-provider-object"></a>Definir la fábrica del proveedor de vista y el objeto del proveedor de vista

Vamos a examinar el bucle __principal__ en __App.cpp__. 

En este paso, se crea una fábrica para la vista (implementa __IFrameworkViewSource__), que a su vez crea instancias del objeto proveedor de vista (implementa __IFrameworkView__) que define la vista.

### <a name="main-method"></a>Método principal

Crea un nuevo __DirectXApplicationSource__ si tienes cargado el código de muestra de GitHub. (Usa __Direct3DApplicationSource__ si estás usando la plantilla original de DirectX) Se trata de la fábrica del proveedor de vista que implementa __IFrameworkViewSource__. La interfaz de __IFrameworkViewSource__ de la fábrica del proveedor de vista tiene un método único, __CreateView__, definido.

En __CoreApplication::Run__, el singleton de la aplicación llama al método __CreateView__ cuando se pasa __Direct3DApplicationSource__ o __DirectXApplicationSource__.

__CreateView__ devuelve una referencia a una nueva instancia del objeto de tu aplicación que implementa __IFrameworkView__, que es el objeto de clase de la __aplicación__ en este ejemplo. El objeto de clase de la __aplicación__ es el objeto proveedor de vista.

```cpp
// The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto directXApplicationSource = ref new DirectXApplicationSource();
    CoreApplication::Run(directXApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXApplicationSource::CreateView()
{
    return ref new App();
}
```

## <a name="initialize-the-view-provider"></a>Inicializar el proveedor de vista

Después de crear el objeto del proveedor de vista, el singleton de la aplicación llama al método [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) en el inicio de la aplicación. Por lo tanto, es crucial que este método controle los comportamientos más fundamentales de un juego de UWP, como controlar la activación de la ventana principal o asegurarse de que el juego puede enfrentarse a una suspensión repentina (y una posible reanudación más tarde).

En este punto, la aplicación del juego puede controlar un mensaje de suspensión (o reanudación). Sin embargo, aún no existe una ventana con la que trabajar, y el juego no está inicializado. Aún quedan varias cosas por hacer.

### <a name="appinitialize-method"></a>Método App::Initialize

En este método, se crean varios controladores de eventos para activar, suspender y reanudar el juego.

Obtén acceso a los recursos del dispositivo. La función __make_shared__ se usa para crear un __shared_ptr__ cuando el recurso de memoria se crea por primera vez. Una ventaja de usar __make_shared__ es que está a prueba de excepciones. También usa la misma llamada para asignar la memoria para el bloque de control y el recurso y, por tanto, reduce la sobrecarga de construcción.

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    // Register event handlers for app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="configure-the-window-and-display-behaviors"></a>Configura los comportamientos de la ventana y la pantalla.

Ahora, echemos un vistazo a la implementación de [__SetWindow__](https://msdn.microsoft.com/library/windows/apps/hh700509). En el método __SetWindow__, se configura el comportamiento de la ventana y de la pantalla.

### <a name="appsetwindow-method"></a>Método App::SetWindow

El singleton de la aplicación proporciona un objeto [__CoreWindow__](https://msdn.microsoft.com/library/windows/apps/br208225) que representa la ventana principal del juego, y pone a disposición del juego sus recursos y eventos. Ahora que existe una ventana con la que trabajar, el juego puede iniciarse agregando los componentes y eventos básicos de interfaz de usuario.

A continuación, crea un puntero usando el método __CoreCursor__, que pueden usar tanto los controles táctiles como el mouse.

Por último, crea eventos básicos de cierre y cambio de tamaño de ventana, así como cambios de PPP (si el dispositivo de presentación cambia). Para obtener más información sobre los eventos, ve a [Control de eventos](tutorial-game-flow-management.md#events-handling).

```cpp
void App::SetWindow(
    CoreWindow^ window
    )
{
    // Codes added to modify the original DirectX template project
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;
    // --end of codes added
    
    m_deviceResources->SetWindow(window);

    window->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);
        
    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    DisplayInformation^ currentDisplayInformation = DisplayInformation::GetForCurrentView();

    currentDisplayInformation->DpiChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDpiChanged);

    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);
    
    // Codes added to modify the original DirectX template project
    currentDisplayInformation->StereoEnabledChanged +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnStereoEnabledChanged);
    // --end of codes added
    
    DisplayInformation::DisplayContentsInvalidated +=
        ref new TypedEventHandler<DisplayInformation^, Platform::Object^>(this, &App::OnDisplayContentsInvalidated);
}
```

## <a name="load-method-of-the-view-provider"></a>Método Load del proveedor de vista

Una vez establecida la ventana principal, el singleton de la aplicación llama a [__Load__](https://msdn.microsoft.com/library/windows/apps/hh700501). Este método usa un conjunto de tareas asincrónicas para crear objetos del juego, cargar recursos gráficos e inicializar la máquina de estados del juego. Si prefieres realizar una captura previa de datos o activos del juego, este es el mejor sitio para hacerlo, en vez de usar **SetWindow** o **Initialize**. 

Puesto que Windows impone restricciones en el tiempo que tu juego puede tardar antes de iniciar el procesamiento de la entrada, con el modelo de tarea asincrónica, necesitas diseñar que el método __Load__ finalice rápidamente para que pueda empezar a procesar la entrada. Si la carga tarda un rato o si hay una gran cantidad de recursos, muestra a los usuarios una barra de progreso que se actualice periódicamente. Este método también se usa para hacer todos los preparativos necesarios antes de que empiece el juego, como definir los estados de inicio o los valores globales.

Si no estás familiarizado con la programación asincrónica y los conceptos de paralelismo de tareas, ve a [Programación asincrónica en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

### <a name="appload-method"></a>Método App::Load

Crear la clase __GameMain__ que contenga las tareas de carga.

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
        if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
````

### <a name="gamemain-constructor"></a>Constructor de GameMain

* Crear e inicializar el representador de juego. Para obtener más información, consulta [Marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md).
* Crear el objeto initialize the Simple3Dgame. Para obtener más información, consulta la sección [Definir el objeto principal del juego](tutorial--defining-the-main-game-loop.md).    
* Crear el objeto de control de interfaz de usuario del juego y mostrar la superposición de información del juego para mostrar una barra de progreso como la carga de archivos de recursos. Para más información, consulta [Agregar una interfaz de usuario](tutorial--adding-a-user-interface.md).
* Crear el controlador de manera que pueda leer entradas del controlador (táctil, mouse o mando inalámbrico de la XBox). Para más información, consulta [Agregar controles](tutorial--adding-controls.md).
* Después de que el controlador se inicialice, definimos dos áreas rectangulares en las esquinas inferiores izquierda y derecha de la pantalla para los controles táctiles de movimiento y cámara respectivamente. El jugador usa el rectángulo inferior izquierdo (definido con la llamada a **SetMoveRect**) como teclado de control virtual para mover la cámara hacia delante y hacia atrás y de un lado a otro. El rectángulo inferior derecho (definido con el método **SetFireRect**) se usa como botón virtual para disparar munición.
* Usa __create_task__ y __create_task::then__ para dividir la carga de recursos en dos fases separadas. Puesto que el acceso al contexto de dispositivo Direct3D 11 está limitado al subproceso en el que se creó el contexto del dispositivo mientras el acceso al dispositivo Direct3D 11 para crear objetos es de subprocesamiento libre, la tarea **CreateGameDeviceResourcesAsync** se puede ejecutar en un subproceso independiente de la tarea de finalización (*FinalizeCreateGameDeviceResources*), que se ejecuta en el subproceso original. Empleamos un modelo parecido para cargar los recursos de nivel con **LoadLevelAsync** y **FinalizeLoadLevel**.

```cpp
GameMain::GameMain(const std::shared_ptr<DX::DeviceResources>& deviceResources) :
    m_deviceResources(deviceResources),
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
    m_deviceResources->RegisterDeviceNotify(this);

    m_renderer = ref new GameRenderer(m_deviceResources);
    m_game = ref new Simple3DGame();

    m_uiControl = m_renderer->GameUIControl();

    m_controller = ref new MoveLookController(CoreWindow::GetForCurrentThread());

    auto bounds = m_deviceResources->GetLogicalSize();

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(bounds.Width - GameConstants::TouchRectangleSize, bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(bounds.Width, bounds.Height)
        );

    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    m_uiControl->SetAction(GameInfoOverlayCommand::None);
    m_uiControl->ShowGameInfoOverlay();

    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and in parallel all the necessary resources are loaded on other threads.
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // as the m_renderer object was created because the D3D device context
        // can ONLY be accessed on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game so spin up the async task to load the level.
            return m_game->LoadLevelAsync().then([this]()
            {
                // The m_game object may need to deal with D3D device context work so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_game->SetCurrentLevelToSavedState();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
        else
        {
            // The game is not in the middle of a level so there aren't any level
            // resources to load.  Creating a no-op task so that in both cases
            // the same continuation logic is used.
            return create_task([]()
            {
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        // Since Game loading is an async task, the app visual state
        // may be too small or not have focus.  Put the state machine
        // into the correct state to reflect these cases.

        if (m_deviceResources->GetLogicalSize().Width < GameConstants::MinPlayableWidth)
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
    }, task_continuation_context::use_current());
}
```

## <a name="run-method-of-the-view-provider"></a>El método Run del proveedor de vista

Los tres métodos anteriores: __Initialize__, __SetWindow__ y __Load__ han preparado el camino. Ahora el juego puede avanzar al método **Run** y ¡ya puede empezar la diversión! Los eventos que usa para realizar la transición entre estados de juego se despachan y procesan. Los gráficos se actualizan durante el ciclo del bucle del juego.

### <a name="apprun"></a>App::Run

Empieza un bucle __while__ que finaliza cuando el jugador cierra la ventana del juego.

El código de muestra realiza la transición a uno de estos dos estados en la máquina de estado del motor de juego:
    * __Deactivated__: La ventana de juego se desactiva (pierde el foco) o se acopla. Cuando esto sucede, el juego suspende el procesamiento de eventos y espera a que la ventana recupere el foco o se desacople.
    * __TooSmall__: El juego actualiza su propio estado y representa los gráficos para mostrar.

Cuando el juego tiene el foco, debes controlar cada evento en la cola de mensajes tan pronto como llegue, y por tanto debes llamar a [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) con la opción **ProcessAllIfPresent**. Otras opciones pueden causar retrasos en el procesamiento de eventos de mensaje, lo que puede provocar que el juego no responda bien o que los comportamientos táctiles parezcan lentos e imprecisos.

Cuando el juego no está visible o está suspendido o acoplado, no conviene que consuma ningún recurso en ciclos de envío de mensajes que nunca llegarán a su destino. En este caso, el juego debe usar **ProcessOneAndAllPending**, que lo bloquea hasta que recibe un evento y, a continuación, procesa dicho evento y cualquier otro que llegue a la cola de procesos durante el procesamiento del primer evento. A continuación, [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) regresa inmediatamente una vez procesada la cola.

```cpp
void App::Run()
{
    m_main->Run();
}

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
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // exiting due to window close.  Make sure to save state.
}
```

## <a name="uninitialize-method-of-the-view-provider"></a>El método Uninitialize del proveedor de vista

Cuando el usuario finalmente finaliza la sesión de juego, debemos limpiar. Aquí es donde entra en acción **Uninitialize**.

En Windows 10, cerrar la ventana de la aplicación no acaba con el proceso de la aplicación, pero en su lugar escribe el estado del singleton de la aplicación en la memoria. Si hay algo especial que debe ocurrir cuando el sistema tiene que reclamar su memoria, incluyendo cualquier limpieza de recursos, pon el código de esa limpieza en este método.

### <a name="app-uninitialize"></a>App:: Uninitialize

```cpp
void App::Uninitialize()
{
}
```

## <a name="tips"></a>Sugerencias

Al desarrollar tu propio juego, diseña el código de inicio en torno a estos métodos. A continuación encontrarás una lista con sugerencias básicas para cada método:

-   Usa **Initialize** para asignar las clases principales y conectar los controladores de eventos básicos.
-   Usa **SetWindow** para crear la ventana principal y conectar con cualquier evento específico de ventana.
-   Usa **Load** para controlar cualquier configuración restante y empezar a crear objetos asincrónicamente y cargar recursos. Si necesitas crear archivos o datos temporales, como activos generados en procedimientos, hazlo aquí también.


## <a name="next-steps"></a>Pasos siguientes

Esto abarca la estructura básica de un juego para UWP con DirectX. Recuerda estos cinco métodos, ya que haremos referencia a ellos en otras partes de este tutorial. A continuación, vamos a echar un vistazo a cómo administrar los estados de juego y los eventos de control para mantener el juego bajo la [Administración del flujo de los juegos](tutorial-game-flow-management.md).



