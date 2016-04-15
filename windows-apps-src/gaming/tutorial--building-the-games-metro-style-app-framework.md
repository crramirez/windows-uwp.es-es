---
title: Definir el marco de la aplicación para la Plataforma universal de Windows (UWP) del juego
description: La primera parte de codificar un juego de Plataforma universal de Windows (UWP) con DirectX es crear el marco que permite al objeto de juego interactuar con Windows.
ms.assetid: 7beac1eb-ba3d-e15c-44a1-da2f5a79bb3b
---

#  Definir el marco de la aplicación para la Plataforma universal de Windows (UWP) del juego


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

La primera parte de codificar un juego de Plataforma universal de Windows (UWP) con DirectX es crear el marco que permite al objeto de juego interactuar con Windows. Esto incluye propiedades de Windows Runtime como la suspensión o reanudación del control de eventos, el foco de la ventana y el acoplamiento, además de los eventos, interacciones y transiciones para la interfaz de usuario. Examinaremos cómo está estructurado el juego de muestra y cómo define la máquina de estados general para la interacción entre el jugador y el sistema.

## Objetivo


-   Configurar el marco de un juego DirectX de UWP e implementar la máquina de estados que define la experiencia de juego general.

## Inicialización e inicio del proveedor de vista


En cualquier juego DirectX de UWP, debes obtener un proveedor de vista que el singleton de la aplicación, el objeto de Windows Runtime que define una instancia de tu aplicación en ejecución, puede usar para acceder a los recursos gráficos que necesita. A través de Windows Runtime, tu aplicación tiene conexión directa con la interfaz gráfica, pero debes especificar los recursos que necesitas y cómo controlarlos.

Tal y como hablamos en [Configuración del proyecto de juego](tutorial--setting-up-the-games-infrastructure.md), Microsoft Visual Studio 2015 proporciona una implementación de un representador básico para DirectX en el archivo **Sample3DSceneRenderer.cpp**, que está disponible cuando seleccionas la plantilla **DirectX 11 App (Universal Windows)** .

Para obtener más detalles para comprender y crear un proveedor de vista y un representador, consulta el tema sobre cómo [configurar una aplicación para UWP con C++ y DirectX para mostrar una vista DirectX](https://msdn.microsoft.com/library/windows/apps/hh465077).

Basta con decir que debes proporcionar la implementación para cinco métodos a los que llama el singleton de la aplicación:

-   [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495)
-   [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509)
-   [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501)
-   [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505)
-   [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523)

En la plantilla DirectX11 App (Universal Windows), estos 5 métodos se definen en el objeto **App** en [App.h](#code_sample). Veamos la forma en que están implementados en este juego.

El método Initialize del proveedor de vista

```cpp
void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}
```

El singleton de la aplicación llama en primer lugar a **Initialize**. Por lo tanto, es crucial que este método controle los comportamientos más fundamentales de un juego de UWP, como controlar la activación de la ventana principal o asegurarse de que el juego puede enfrentarse a una suspensión repentina (y una posible reanudación más tarde).

Cuando la aplicación del juego se inicializa, asigna memoria específica para el controlador para permitir que el jugador comience a proporcionar entradas. Asimismo, crea una nueva instancia sin inicializar del representador y la máquina de estado del juego. Nos detendremos en esto en [Definición del objeto principal del juego](tutorial--defining-the-main-game-loop.md).

En este punto, la aplicación del juego puede controlar un mensaje de suspensión (o reanudación) y tiene asignada memoria para el controlador y para el propio juego. Sin embargo, no existe una ventana con la que trabajar, y el juego no está inicializado. Aún quedan varias cosas por hacer.

El método SetWindow del proveedor de vista

```cpp
void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}
```

Ahora, con una llamada a una implementación de [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), el singleton de la aplicación proporciona un objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que representa la ventana principal del juego, y pone a disposición del juego sus recursos y eventos. Dado que existe una ventana con la que trabajar, ahora el juego puede agregar los componentes y eventos básicos de la interfaz de usuario: un puntero (usado tanto por el mouse como por los controles táctiles) y los eventos básicos de cambio de tamaño de la ventana, cierre y cambios en los PPP (si el dispositivo de pantalla cambia).

La aplicación del juego también inicializa el controlador (ya que existe una ventana con la que interactuar) e inicializa el propio objeto de juego. Puede leer entradas del controlador (táctil, mouse o mando de la XBox 360).

Después de que el controlador se inicialice, la aplicación define dos áreas rectangulares en las esquinas inferiores izquierda y derecha de la pantalla para los controles táctiles de movimiento y cámara respectivamente. El jugador usa el rectángulo inferior izquierdo (definido con la llamada a **SetMoveRect**) como teclado de control virtual para mover la cámara hacia delante y hacia atrás y de un lado a otro. El rectángulo inferior derecho (definido con el método **SetFireRect**) se usa como botón virtual para disparar munición.

Todo empieza a tomar forma.

El método Load del proveedor de vista

```cpp
void App::Load(
    Platform::String^ entryPoint
    )
{
    task<void>([this]()
    {
        m_game->Initialize(m_controller, m_renderer);

        return m_renderer->CreateGameDeviceResourcesAsync(m_game);

    }).then([this]()
    {
        // The finalize code needs to run in the same thread context
        // in which the m_renderer object was created because the D3D device context
        // can be accessed only on a single thread.
        m_renderer->FinalizeCreateGameDeviceResources();

        InitializeGameState();

        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // the finalizer code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can  be accessed only on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}
```

Una vez establecida la ventana principal, el singleton de la aplicación llama a **Load**. En la muestra, este método usa un conjunto de tareas asincrónicas (cuya sintaxis se define en la [biblioteca de modelos paralelos](https://msdn.microsoft.com/library/windows/apps/dd492418.aspx)) para crear objetos del juego, cargar recursos gráficos e inicializar la máquina de estados del juego. Al usar el modelo de tarea asincrónica, el método Load finaliza rápidamente y permite que la aplicación empiece a procesar entradas. En este método, la aplicación también muestra una barra de progreso mientras los archivos de recursos se cargan.

Distinguimos dos fases dentro de la carga de recursos porque el acceso al contexto de dispositivo de Direct3D 11 está limitado al subproceso en el que el contexto de dispositivo se creó, mientras que el acceso al dispositivo de Direct3D 11 para crear objetos no está sujeto a ningún subproceso. La tarea **CreateGameDeviceResourcesAsync** se ejecuta en un subproceso distinto del de la tarea de finalización (*FinalizeCreateGameDeviceResources*), que se ejecuta en el subproceso original. Empleamos un modelo parecido para cargar los recursos de nivel con **LoadLevelAsync** y **FinalizeLoadLevel**.

Tras crear los objetos del juego y cargar los recursos gráficos, inicializamos la máquina de estado del juego en las condiciones de inicio (por ejemplo, definir la cantidad de munición inicial, el número de nivel y las posiciones de los objetos). Si el estado del juego indica que el jugador está reanudando un juego, cargamos el nivel actual (aquel en el que el jugador estaba cuando el juego se suspendió).

En el método **Load**, haremos todos los preparativos necesarios antes de que empiece el juego, como definir los estados de inicio o los valores globales. Si prefieres realizar una captura previa de datos o activos del juego, este es el mejor sitio para hacerlo, en vez de usar **SetWindow** o **Initialize**. Usa tareas asincrónicas en tu juego para cargar recursos, ya que Windows impone restricciones en cuanto al tiempo que el juego puede tardar en empezar a procesar entradas. Si la carga tarda por haber una gran cantidad de recursos, muestra a los usuarios una barra de progreso que se actualice periódicamente.

Al desarrollar tu propio juego, diseña el código de inicio en torno a estos métodos. A continuación encontrarás una lista con sugerencias básicas para cada método:

-   Usa **Initialize** para asignar las clases principales y conectar los controladores de eventos básicos.
-   Usa **SetWindow** para crear la ventana principal y conectar con cualquier evento específico de ventana.
-   Usa **Load** para controlar cualquier configuración restante y empezar a crear objetos asincrónicamente y cargar recursos. Si necesitas crear archivos o datos temporales, como activos generados en procedimientos, hazlo aquí también.

Por tanto, el juego de muestra crea una instancia de la máquina de estado del juego y la establece en la configuración de inicio. Controla todos los eventos de sistema y de entrada. Proporciona una ventana en la que mostrar el contenido. El código del juego está ahora listo para ejecutarse.

El método Run del proveedor de vista

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The app is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save the state.
}
```

Aquí es donde llegamos a la parte del juego de la aplicación. Una vez ejecutado los 3 métodos y establecido el escenario, la aplicación del juego ejecuta el método **Run** y empieza la diversión.

En la muestra de juego, iniciamos un bucle que finaliza cuando el jugador cierra la ventana del juego. El código de muestra realiza la transición a uno de estos dos estados en la máquina de estado del motor de juego:

-   La ventana de juego se desactiva (pierde el foco) o se acopla. Cuando esto sucede, el juego suspende el procesamiento de eventos y espera a que la ventana recupere el foco o se desacople.
-   De lo contrario, el juego actualiza su propio estado y representa los gráficos para mostrar.

Cuando el juego tiene el foco, debes controlar cada evento en la cola de mensajes tan pronto como llegue, y por tanto debes llamar a [**CoreWindowDispatch.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) con la opción **ProcessAllIfPresent**. Otras opciones pueden causar retrasos en el procesamiento de eventos de mensaje, lo que puede provocar que el juego no responda bien o que los comportamientos táctiles parezcan lentos e imprecisos.

No hace falta decir que, cuando la aplicación no está visible o está suspendida o acoplada, no conviene que consuma ningún recurso en ciclos de envío de mensajes que nunca llegarán a su destino. Por lo tanto, el juego debe usar **ProcessOneAndAllPending**, que lo bloquea hasta que recibe un evento y, a continuación, procesa dicho evento y cualquier otro que llegue a la cola de procesos durante el procesamiento del primer evento. A continuación, [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) regresa inmediatamente una vez procesada la cola.

El juego está en ejecución. Los eventos que usa para realizar la transición entre estados de juego se despachan y procesan. Los gráficos se actualizan mientras el bucle del juego cicla. Esperamos que el jugador se divierta. Sin embargo, antes o después la diversión debe acabar...

...y nosotros debemos limpiar la casa. Aquí es donde entra en acción la propiedad **Uninitialize**.

El método Uninitialize del proveedor de vista

```cpp
void App::Uninitialize()
{
}
```

En la muestra de juego, dejamos que el singleton de la aplicación del juego limpie todo una vez que el juego termine. En Windows 10, cerrar la ventana de la aplicación no acaba con el proceso de la aplicación, sino que en su lugar se escribe en la memoria el estado del singleton de la aplicación. Si hay algo especial que debe ocurrir cuando el sistema tiene que reclamar su memoria, cualquier limpieza de recursos, pon el código de esa limpieza en este método.

Volveremos a hacer referencia a estos 5 métodos en el tutorial, así que tenlos en cuenta. Ahora vamos a echar un vistazo a la estructura general del motor de juego y las máquinas de estado que la definen.

## Inicialización del estado del motor de juego


Puesto que un usuario puede reanudar una aplicación de juego de UWP desde el estado suspendido en cualquier momento, la aplicación puede tener cualquier número de estados posibles.

La muestra de juego puede estar en uno de estos tres estados cuando se inicia:

-   El bucle del juego estaba ejecutándose y estaba en mitad de un nivel.
-   El bucle del juego no estaba ejecutándose porque un juego acababa de completarse. (Se establece la puntuación alta).
-   No se ha iniciado ningún juego o el juego se encuentra entre niveles. (La puntuación alta es 0).

Obviamente, en tu propio juego podrías tener más o menos estados. De nuevo, ten siempre presente que tu juego de UWP puede finalizarse en cualquier momento y, cuando se reanuda, el jugador espera que el juego se comporte como si nunca hubiera dejado de jugarlo.

En la muestra del juego, el flujo de código presenta este aspecto.

```cpp
void App::InitializeGameState()
{
    //
    // Set up the initial state machine for handling game playing state.
    //
    if (m_game->GameActive() && m_game->LevelActive())
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...

    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        m_updateState = UpdateEngineState::WaitingForPress;
        // ...
    }
    else
    {
        m_updateState = UpdateEngineState::WaitingForResources;
        // ...
    }
    SetAction(GameInfoOverlayCommand::PleaseWait);
    ShowGameInfoOverlay();
}
```

La inicialización no consiste tanto en iniciar en frío la aplicación como en reiniciarla después de que haya terminado. El juego de muestra siempre guarda el estado, por lo que parece que la aplicación siempre está en ejecución. El estado suspendido es simplemente eso: el juego se suspende, pero los recursos del juego siguen en la memoria. De igual modo, el evento de reanudación indica que el juego de muestra lo retoma desde donde estaba cuando se suspendió o terminó por última vez. Cuando el juego de muestra se reinicia después de haber terminado, se inicia con normalidad y después determina cuál fue el último estado conocido para que el jugador pueda seguir progresando con el juego inmediatamente.

El diagrama de flujo muestra los estados iniciales y las transiciones para el proceso de inicialización de la muestra de juego.

![the process fo initializing y preparing our game befoe the main loop starts](images/simple3dgame-appstartup.png)

Dependiendo del estado, al jugador se le presentan distintas opciones. Si el juego se reanuda en mitad de un nivel, aparece como pausado, y la superposición muestra una opción de continuar. Si el juego se reanudó en un estado en que el juego estaba completado, muestra las puntuaciones más altas y una opción para jugar un nuevo juego. Por último, si el juego se reanuda antes de que se inicie un nivel, la superposición presenta al usuario una opción de inicio.

La muestra del juego no distingue entre que el propio juego se inicie en frío, es decir, que el juego se inicia por primera vez sin un evento de suspensión, y que el juego se reanude desde un estado suspendido. Este diseño es el adecuado para cualquier aplicación para UWP.

## Control de eventos


Nuestro código de muestra registró varios controladores para eventos específicos en **Initialize**, **SetWindow** y **Load**. Probablemente hayas adivinado que eran eventos importantes, porque la muestra de código realizó este trabajo antes de empezar con cualquier mecánica de juego o desarrollo de gráficos. Estabas en lo cierto. Estos eventos son fundamentales para lograr la experiencia adecuada con una aplicación para UWP. Dado que una aplicación para UWP se puede activar, desactivar, cambiar de tamaño, acoplar, desacoplar, suspender o reanudar en cualquier momento, el juego debe registrar esos mismos eventos tan pronto como pueda y controlarlos de modo que el jugador disfrute de una experiencia predecible y sin interrupciones.

A continuación tienes una lista de los controladores de eventos de la muestra y los eventos que controlan. Puedes ver el código completo para estos controladores de eventos en el [código completo para esta sección](#code_sample).

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
<td align="left">Controla [<strong>CoreApplicationView::Activated</strong>] (https://msdn.microsoft.com/library/windows/apps/br225018). La aplicación del juego se coloca en primer plano, de modo que la ventana principal está activa.</td>
</tr>
<tr class="even">
<td align="left">OnLogicalDpiChanged</td>
<td align="left">Controla [<strong>DisplayProperties::LogicalDpiChanged</strong>] (https://msdn.microsoft.com/library/windows/apps/br226150). Los PPP de la ventana de juego principal han cambiado, por lo que la aplicación del juego ajusta sus recursos en consonancia.
<div class="alert">
<strong>Nota</strong>  Las coordenadas de [<strong>CoreWindow</strong>](https://msdn.microsoft.com/library/windows/desktop/hh404559) están en PPP (píxeles independientes del dispositivo), como en [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370987). Como resultado, se debe notificar a Direct2D el cambio en PPP para mostrar cualquier primitivo o activo 2D correctamente.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">OnResuming</td>
<td align="left">Controla [<strong>CoreApplication::Resuming</strong>] (https://msdn.microsoft.com/library/windows/apps/br205859). La aplicación del juego restaura el juego desde un estado suspendido.</td>
</tr>
<tr class="even">
<td align="left">OnSuspending</td>
<td align="left">Controla [<strong>CoreApplication::Suspending</strong>](https://msdn.microsoft.com/library/windows/apps/br205860). La aplicación del juego guarda su estado en disco. Tiene 5 segundos para guardar el estado en el almacenamiento.</td>
</tr>
<tr class="odd">
<td align="left">OnVisibilityChanged</td>
<td align="left">Controla [<strong>CoreWindow::VisibilityChanged</strong>](https://msdn.microsoft.com/library/windows/apps/hh701591). La aplicación del juego ha cambiado la visibilidad y ha pasado a ser visible u otra aplicación visible la ha convertido en invisible.</td>
</tr>
<tr class="even">
<td align="left">OnWindowActivationChanged</td>
<td align="left">Controla [<strong>CoreWindow::Activated</strong>](https://msdn.microsoft.com/library/windows/apps/br208255). La ventana principal de la aplicación del juego se ha desactivado o activado, por lo que debe quitar el foco y pausar el juego, o recuperar el foco. En ambos casos, la superposición indica que el juego está en pausa.</td>
</tr>
<tr class="odd">
<td align="left">OnWindowClosed</td>
<td align="left">Controla [<strong>CoreWindow::Closed</strong>](https://msdn.microsoft.com/library/windows/apps/br208261). La aplicación del juego cierra la ventana principal y suspende el juego.</td>
</tr>
<tr class="even">
<td align="left">OnWindowSizeChanged</td>
<td align="left">Controla [<strong>CoreWindow::SizeChanged</strong>](https://msdn.microsoft.com/library/windows/apps/br208283). La aplicación del juego reasigna los recursos gráficos y la superposición para ajustarse al cambio de tamaño y después actualiza el destino de representación.</td>
</tr>
</tbody>
</table>

 

Tu propio juego debe controlar estos eventos, porque forman parte del diseño de una aplicación para UWP.

## Actualización del motor de juego


Dentro del bucle de juego en **Run**, la muestra ha implementado una máquina de estado básica para controlar las principales acciones que puede llevar a cabo un jugador. En el nivel más general, la máquina de estado se encarga de cargar un juego, jugar un nivel específico o continuar un nivel después de que el juego se haya pausado (por el sistema o el jugador).

En la muestra de juego, el juego puede encontrarse en 3 estados principales (UpdateEngineState):

-   **Waiting for resources**. El bucle del juego está ciclando, incapaz de realizar la transición hasta que disponga de recursos (en concreto recursos gráficos). Cuando las tareas asincrónicas para cargar recursos finalizan, el estado se actualiza a **ResourcesLoaded**. Esto suele ocurrir entre niveles cuando el nivel necesita cargar nuevos recursos del disco. En la muestra de juego, simulamos este comportamiento, dado que la muestra no necesita ningún recurso adicional por nivel en ese momento.
-   **Waiting for press**. El bucle del juego está ciclando, esperando a la entrada específica del usuario. Esta entrada es una acción del jugador para cargar un juego, iniciar un nivel o continuar un nivel. El código de muestra se refiere a estos subestados como valores de enumeración PressResultState.
-   **Dynamics**. El bucle del juego se está ejecutando mientras el usuario juega. Cuando el usuario está jugando, el juego comprueba las 3 condiciones con las que puede realizar una transición: que se acabe el tiempo establecido para un nivel, que el jugador complete un nivel o que el jugador complete todos los niveles.

Aquí podemos ver la estructura del código. El código completo se encuentra en el [código completo para esta sección](#code_sample).

La estructura de la máquina de estado usada para actualizar el motor de juego

```cpp
void App::Update()
{
    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
        switch (m_pressResult)
        {
        case PressResultState::LoadGame:
            // ...
            break;

        case PressResultState::PlayLevel:
            // ...
            break;

        case PressResultState::ContinueLevel:
            // ...
            break;
        }
        // ...
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete() || m_pressComplete)
        {
            m_pressComplete = false;

            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                // ...
                break;

            case PressResultState::PlayLevel:
                // ...
                break;

            case PressResultState::ContinueLevel:
                // ...
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested() || m_pauseRequested)
        {
            // ...
        }
        else 
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                // ...
                break;

            case GameState::LevelComplete:
                // ...
                break;

            case GameState::GameComplete:
                // ...
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // transitioning state, so enable waiting for the press event
            m_controller->WaitForPress(m_game->GameInfoOverlayUpperLeft(), m_game->GameInfoOverlayLowerRight());
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

Visualmente, la máquina de estado de juego principal tiene este aspecto:

![the main state machine fo our game](images/simple3dgame-mainstatemachine.png)

Hablamos con más detalle de la propia lógica del juego en el tema de [definición del objeto de juego principal](tutorial--defining-the-main-game-loop.md). Por ahora, lo que debe quedar claro es que el juego es una máquina de estado. Cada estado específico debe estar definido por criterios muy específicos, y las transiciones de un estado a otro deben basarse en entradas de usuario o en acciones del sistema discretas (como la carga de recursos gráficos). Cuando planees tu juego, dibuja un diagrama como el que usamos nosotros, asegurándote de responder a todas las posibles acciones que el usuario o el sistema puedan tomar en el nivel más general. Los juegos pueden ser muy complicados, y la máquina de estado es una eficaz herramienta para visualizar y hacer más sencilla esta complejidad.

Por supuesto, como has visto, existen máquinas de estado dentro de otras máquinas de estado. Hay una para el controlador, que controla todas las entradas aceptables que puede generar el jugador. En el diagrama, una presión es alguna forma de entrada de usuario. A esta máquina de estado no le preocupa qué es, porque funciona a nivel general; da por sentando que la máquina de estado para el controlador se ocupará de cualquier transición que afecte al movimiento y al comportamiento de los disparos, así como de las actualizaciones de la representación asociadas. Hablamos de la administración de los estados de entrada en el tema sobre cómo [agregar controles](tutorial--adding-controls.md).

## Actualización de la interfaz de usuario


Asimismo, debemos mantener informado al usuario del estado del sistema y permitirle cambiar el estado general de acuerdo con las reglas del juego. En la mayoría de los juegos, incluida esta muestra de juego, esto se hace mediante una pantalla de visualización frontal que contiene representaciones del estado del juego y otra información específica del juego, como la puntuación, la munición o el número de oportunidades restantes. A esto lo llamamos superposición, porque se representa independientemente de la canalización de gráfica principal y se coloca encima de la proyección en 3D. En el juego de muestra, creamos esta superposición con las API de Direct2D. También podemos crearla usando XAML, como se explica en el tema sobre cómo [extender la muestra de juego](tutorial-resources.md).

La interfaz de usuario consta de dos componentes:

-   La pantalla de visualización frontal, que contiene la puntuación y la información sobre el estado actual del juego.
-   El mapa de bits de pausa, que es un rectángulo negro con texto superpuesto durante el estado de pausa/suspensión del juego. Se trata de la superposición del juego, que analizaremos con más detalle en el tema sobre cómo [agregar una interfaz de usuario](tutorial--adding-a-user-interface.md).

Como era de prever, la superposición también cuenta con una máquina de estado. La superposición puede mostrar el inicio de un nivel o un mensaje de fin de juego. Es, básicamente, un Canvas donde incluir cualquier información sobre el estado del juego que se mostrará al jugador cuando el juego esté en pausa o suspendido.

La muestra de juego estructura la máquina de estado de la superposición de la siguiente manera:

```cpp
void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {

    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        // ...
        break;

    case GameInfoOverlayState::LevelStart:
        // ...
        break;

    case GameInfoOverlayState::GameOverCompleted:
        // ...
        break;

    case GameInfoOverlayState::GameOverExpired:
        // ...
        break;

    case GameInfoOverlayState::Pause:
        // ...
        break;
    }
}
```

Hay seis pantallas de estado que la superposición muestra en función del estado del propio juego: una pantalla de carga de recursos al inicio del juego; una pantalla de juego; una pantalla con un mensaje de inicio de nivel; una pantalla de final de juego cuando se completan todos los niveles sin quedarse sin tiempo; una pantalla de final de juego cuando el tiempo se agota; y una pantalla de menú de pausa.

Separar la interfaz de usuario de la canalización gráfica del juego te permite trabajar en ella con independencia del motor de representación gráfica del juego y reduce significativamente la complejidad del código del juego.

## Pasos siguientes


Hemos cubierto aquí la estructura básica de la muestra de juego y hemos presentado un buen modelo para el desarrollo de aplicaciones de juego para UWP con DirectX. Obviamente, esto es solo el principio, únicamente hemos visto el esqueleto del juego. Echemos ahora un vistazo en profundidad al juego y sus mecánicas, y cómo dichas mecánicas se implementan como el objeto de juego principal. Revisamos esa parte en el tema de la [definición del objeto de juego principal](tutorial--defining-the-main-game-loop.md).

También es el momento de considerar el motor gráfico del juego de muestra con más detalle. Esa parte se analiza en el tema sobre cómo [ensamblar la canalización de representación](tutorial--assembling-the-rendering-pipeline.md).

## Código de muestra completo para esta sección


App.h

```cpp
///// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include "Simple3DGame.h"

enum class UpdateEngineState
{
    WaitingForResources,
    ResourcesLoaded,
    WaitingForPress,
    Dynamics,
    Snapped,
    Suspended,
    Deactivated,
};

enum class PressResultState
{
    LoadGame,
    PlayLevel,
    ContinueLevel,
};

enum class GameInfoOverlayState
{
    Loading,
    GameStats,
    GameOverExpired,
    GameOverCompleted,
    LevelStart,
    Pause,
};

ref class App : public Windows::ApplicationModel::Core::IFrameworkView
{
internal:
    App();

public:
    // IFrameworkView Methods
    virtual void Initialize(_In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
    virtual void SetWindow(_In_ Windows::UI::Core::CoreWindow^ window);
    virtual void Load(_In_ Platform::String^ entryPoint);
    virtual void Run();
    virtual void Uninitialize();

private:
    void InitializeGameState();

    // Event Handlers
    void OnSuspending(
        _In_ Platform::Object^ sender,
        _In_ Windows::ApplicationModel::SuspendingEventArgs^ args
        );

    void OnResuming(
        _In_ Platform::Object^ sender,
        _In_ Platform::Object^ args
        );

    void UpdateViewState();

    void OnWindowActivationChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
        );

    void OnWindowSizeChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::WindowSizeChangedEventArgs^ args
        );

    void OnWindowClosed(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::CoreWindowEventArgs^ args
        );

    void OnLogicalDpiChanged(
        _In_ Platform::Object^ sender
        );

    void OnActivated(
        _In_ Windows::ApplicationModel::Core::CoreApplicationView^ applicationView,
        _In_ Windows::ApplicationModel::Activation::IActivatedEventArgs^ args
        );

    void OnVisibilityChanged(
        _In_ Windows::UI::Core::CoreWindow^ sender,
        _In_ Windows::UI::Core::VisibilityChangedEventArgs^ args
        );

    void Update();
    void SetGameInfoOverlay(GameInfoOverlayState state);
    void SetAction (GameInfoOverlayCommand command);
    void ShowGameInfoOverlay();
    void HideGameInfoOverlay();
    void SetSnapped();
    void HideSnapped();

    bool                                                m_windowClosed;
    bool                                                m_renderNeeded;
    bool                                                m_haveFocus;
    bool                                                m_visible;

    MoveLookController^                                 m_controller;
    GameRenderer^                                       m_renderer;
    Simple3DGame^                                       m_game;

    UpdateEngineState                                   m_updateState;
    UpdateEngineState                                   m_updateStateNext;
    PressResultState                                    m_pressResult;
    GameInfoOverlayState                                m_gameInfoOverlayState;
    GameInfoOverlayCommand                              m_gameInfoOverlayCommand;
    uint32                                              m_loadingCount;
};

ref class Direct3DApplicationSource : Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView();
};
```

App.cpp

```cpp
//--------------------------------------------------------------------------------------
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#include "pch.h"
#include "App.h"

using namespace concurrency;
using namespace DirectX;
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::Foundation;
using namespace Windows::Graphics::Display;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::ViewManagement;


App::App() :
    m_windowClosed(false),
    m_haveFocus(false),
    m_gameInfoOverlayCommand(GameInfoOverlayCommand::None),
    m_visible(true),
    m_loadingCount(0),
    m_updateState(UpdateEngineState::WaitingForResources)
{
}

//--------------------------------------------------------------------------------------

void App::Initialize(
    _In_ CoreApplicationView^ applicationView
    )
{
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);

    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);

    m_controller = ref new MoveLookController();
    m_renderer = ref new GameRenderer();
    m_game = ref new Simple3DGame();
}

//--------------------------------------------------------------------------------------

void App::SetWindow(
    _In_ CoreWindow^ window
    )
{
    window->PointerCursor = ref new CoreCursor(CoreCursorType::Arrow, 0);

    PointerVisualizationSettings^ visualizationSettings = PointerVisualizationSettings::GetForCurrentView();
    visualizationSettings->IsContactFeedbackEnabled = false;
    visualizationSettings->IsBarrelButtonFeedbackEnabled = false;

    window->SizeChanged +=
        ref new TypedEventHandler<CoreWindow^, WindowSizeChangedEventArgs^>(this, &App::OnWindowSizeChanged);

    window->Closed +=
        ref new TypedEventHandler<CoreWindow^, CoreWindowEventArgs^>(this, &App::OnWindowClosed);

    window->VisibilityChanged +=
        ref new TypedEventHandler<CoreWindow^, VisibilityChangedEventArgs^>(this, &App::OnVisibilityChanged);

    DisplayProperties::LogicalDpiChanged +=
        ref new DisplayPropertiesEventHandler(this, &App::OnLogicalDpiChanged);

    m_controller->Initialize(window);

    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    m_renderer->Initialize(window, DisplayProperties::LogicalDpi);
    SetGameInfoOverlay(GameInfoOverlayState::Loading);
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Load(
    _In_ Platform::String^ /* entryPoint */
    )
{
    create_task([this]()
    {
        // Asynchronously initialize the game class and load the renderer device resources.
        // By doing all this asynchronously, the game gets to its main loop more quickly
        // and loads all the necessary resources in parallel on other threads.
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
            // In the middle of a game, so spin up the async task to load the level.
            create_task([this]()
            {
                return m_game->LoadLevelAsync();

            }).then([this]()
            {
                // The m_game object may need to deal with D3D device context work, so
                // again the finalize code needs to run in the same thread
                // context as the m_renderer object was created because the D3D 
                // device context can ONLY be accessed on a single thread.
                m_game->FinalizeLoadLevel();
                m_updateState = UpdateEngineState::ResourcesLoaded;

            }, task_continuation_context::use_current());
        }
    }, task_continuation_context::use_current());
}

//--------------------------------------------------------------------------------------

void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible)
        {
            switch (m_updateState)
            {
            case UpdateEngineState::Deactivated:
            case UpdateEngineState::Snapped:
                if (!m_renderNeeded)
                {
                    // The App is not currently the active window, so just wait for events.
                    CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
                    break;
                }
                // Otherwise, fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close.  Make sure to save state.
}

//--------------------------------------------------------------------------------------

void App::Uninitialize()
{
}

//--------------------------------------------------------------------------------------

void App::OnWindowSizeChanged(
    _In_ CoreWindow^ window,
    _In_ WindowSizeChangedEventArgs^ /* args */
    )
{
    UpdateViewState();
    m_renderer->UpdateForWindowSizeChange();

    // The location of the GameInfoOverlay may have changed with the size change, so update the controller.
    m_controller->SetMoveRect(
        XMFLOAT2(0.0f, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(GameConstants::TouchRectangleSize, window->Bounds.Height)
        );
    m_controller->SetFireRect(
        XMFLOAT2(window->Bounds.Width - GameConstants::TouchRectangleSize, window->Bounds.Height - GameConstants::TouchRectangleSize),
        XMFLOAT2(window->Bounds.Width, window->Bounds.Height)
        );

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowClosed(
    _In_ CoreWindow^ /* sender */,
    _In_ CoreWindowEventArgs^ /* args */
    )
{
    m_windowClosed = true;
}

//--------------------------------------------------------------------------------------

void App::OnLogicalDpiChanged(
    _In_ Platform::Object^ /* sender */
    )
{
    m_renderer->SetDpi(DisplayProperties::LogicalDpi);

    // The GameInfoOverlay may have been recreated as a result of DPI changes, so
    // regenerate the data.
    SetGameInfoOverlay(m_gameInfoOverlayState);
    SetAction(m_gameInfoOverlayCommand);
}

//--------------------------------------------------------------------------------------

void App::OnActivated(
    _In_ CoreApplicationView^ /* applicationView */,
    _In_ IActivatedEventArgs^ /* args */
    )
{
    CoreWindow::GetForCurrentThread()->Activated +=
        ref new TypedEventHandler<CoreWindow^, WindowActivatedEventArgs^>(this, &App::OnWindowActivationChanged);
    CoreWindow::GetForCurrentThread()->Activate();
}

//--------------------------------------------------------------------------------------

void App::OnVisibilityChanged(
    _In_ CoreWindow^ /* sender */,
    _In_ VisibilityChangedEventArgs^ args
    )
{
    m_visible = args->Visible;
}

//--------------------------------------------------------------------------------------

void App::InitializeGameState()
{
    // Set up the initial state machine for handling the Game playing state.
    if (m_game->GameActive() && m_game->LevelActive())
    {
        // The last time the game terminated it was in the middle
        // of a level.
        // We are waiting for the user to continue the game.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::ContinueLevel;
        SetGameInfoOverlay(GameInfoOverlayState::Pause);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    else if (!m_game->GameActive() && (m_game->HighScore().totalHits > 0))
    {
        // The last time the game terminated the game had been completed.
        // Show the high score.
        // We are waiting for the user to acknowledge the high score and start a new game.
        // The level resources for the first level will be loaded later.
        m_updateState = UpdateEngineState::WaitingForPress;
        m_pressResult = PressResultState::LoadGame;
        SetGameInfoOverlay(GameInfoOverlayState::GameStats);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        SetAction(GameInfoOverlayCommand::TapToContinue);
    }
    else
    {
        // This is either the first time the game has run or
        // the last time the game terminated the level was completed.
        // We are waiting for the user to begin the next level.
        m_updateState = UpdateEngineState::WaitingForResources;
        m_pressResult = PressResultState::PlayLevel;
        SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
        SetAction(GameInfoOverlayCommand::PleaseWait);
    }
    ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::Update()
{
    static uint32 loadCount = 0;

    m_controller->Update();

    switch (m_updateState)
    {
    case UpdateEngineState::WaitingForResources:
        // Waiting for the initial load.  Display an update once per 60 updates.
        loadCount++;
        if ((loadCount % 60) == 0)
        {
            m_loadingCount++;
            SetGameInfoOverlay(m_gameInfoOverlayState);
        }
        break;

    case UpdateEngineState::ResourcesLoaded:
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
        m_updateState = UpdateEngineState::WaitingForPress;
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        ShowGameInfoOverlay();
        m_renderNeeded = true;
        break;

    case UpdateEngineState::WaitingForPress:
        if (m_controller->IsPressComplete())
        {
            switch (m_pressResult)
            {
            case PressResultState::LoadGame:
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;
                m_controller->Active(false);
                m_game->LoadGame();
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case PressResultState::PlayLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->StartLevel();
                break;

            case PressResultState::ContinueLevel:
                m_updateState = UpdateEngineState::Dynamics;
                HideGameInfoOverlay();
                m_controller->Active(true);
                m_game->ContinueGame();
                break;
            }
        }
        break;

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            m_game->PauseGame();
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_updateState = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            ShowGameInfoOverlay();
        }
        else
        {
            GameState runState = m_game->RunGame();
            switch (runState)
            {
            case GameState::TimeExpired:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverExpired);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;

            case GameState::LevelComplete:
                SetAction(GameInfoOverlayCommand::PleaseWait);
                SetGameInfoOverlay(GameInfoOverlayState::LevelStart);
                ShowGameInfoOverlay();
                m_updateState = UpdateEngineState::WaitingForResources;
                m_pressResult = PressResultState::PlayLevel;

                m_game->LoadLevelAsync().then([this]()
                {
                    m_game->FinalizeLoadLevel();
                    m_updateState = UpdateEngineState::ResourcesLoaded;

                }, task_continuation_context::use_current());
                break;

            case GameState::GameComplete:
                SetAction(GameInfoOverlayCommand::TapToContinue);
                SetGameInfoOverlay(GameInfoOverlayState::GameOverCompleted);
                ShowGameInfoOverlay();
                m_updateState  = UpdateEngineState::WaitingForPress;
                m_pressResult = PressResultState::LoadGame;
                break;
            }
        }

        if (m_updateState == UpdateEngineState::WaitingForPress)
        {
            // Transitioning state, so enable waiting for the press event.
            m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
        }
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            // Transitioning state, so shut down the input controller until resources are loaded.
            m_controller->Active(false);
        }
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::OnWindowActivationChanged(
    _In_ Windows::UI::Core::CoreWindow^ /* sender */,
    _In_ Windows::UI::Core::WindowActivatedEventArgs^ args
    )
{
    if (args->WindowActivationState == CoreWindowActivationState::Deactivated)
    {
        m_haveFocus = false;

        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of Deactivated rather than going directly back into game play
            // go to the paused state waiting for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            ShowGameInfoOverlay();
            m_game->PauseGame();
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            m_renderNeeded = true;
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            m_updateState = UpdateEngineState::Deactivated;
            SetAction(GameInfoOverlayCommand::None);
            ShowGameInfoOverlay();
            m_renderNeeded = true;
            break;
        }
        // Don't have focus, so shutdown input processing.
        m_controller->Active(false);
    }
    else if (args->WindowActivationState == CoreWindowActivationState::CodeActivated
        || args->WindowActivationState == CoreWindowActivationState::PointerActivated)
    {
        m_haveFocus = true;

        if (m_updateState == UpdateEngineState::Deactivated)
        {
            m_updateState = m_updateStateNext;

            if (m_updateState == UpdateEngineState::WaitingForPress)
            {
                SetAction(GameInfoOverlayCommand::TapToContinue);
                m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
            }
            else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
            {
                SetAction(GameInfoOverlayCommand::PleaseWait);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::OnSuspending(
    _In_ Platform::Object^ /* sender */,
    _In_ SuspendingEventArgs^ args
    )
{
    // Save application state.
    // If your application needs time to complete a lengthy operation, it can request a deferral.
    // The SuspendingOperation has a deadline time. Make sure all your operations are complete by that time!
    // If the app doesn't return from this handler within five seconds, it will be terminated.
    SuspendingOperation^ op = args->SuspendingOperation;
    SuspendingDeferral^ deferral = op->GetDeferral();

    create_task([=]()
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // Game is in the active game play state, Stop Game Timer and Pause play and save the state.
            SetAction(GameInfoOverlayCommand::None);
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            m_updateStateNext = m_updateState;
            break;

        default:
            // If it is any other state, don't save as the next state as they are transient states and have already set m_updateStateNext
            break;
        }
        m_updateState = UpdateEngineState::Suspended;

        m_controller->Active(false);
        m_game->OnSuspending();

        deferral->Complete();
    });
}

//--------------------------------------------------------------------------------------

void App::OnResuming(
    _In_ Platform::Object^ /* sender */,
    _In_ Platform::Object^ /* args */
    )
{
    if (m_haveFocus)
    {
        m_updateState = m_updateStateNext;
    }
    else
    {
        m_updateState = UpdateEngineState::Deactivated;
    }

    if (m_updateState == UpdateEngineState::WaitingForPress)
    {
        SetAction(GameInfoOverlayCommand::TapToContinue);
        m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
    }
    m_game->OnResuming();
    ShowGameInfoOverlay();
    m_renderNeeded = true;
}

//--------------------------------------------------------------------------------------

void App::UpdateViewState()
{
    m_renderNeeded = true;

    if (ApplicationView::Value == ApplicationViewState::Snapped)
    {
        switch (m_updateState)
        {
        case UpdateEngineState::Dynamics:
            // From Dynamic mode, when coming out of SNAPPED layout rather than going directly back into game play,
            // go to the paused state and wait for user input to continue.
            m_updateStateNext = UpdateEngineState::WaitingForPress;
            m_pressResult = PressResultState::ContinueLevel;
            SetGameInfoOverlay(GameInfoOverlayState::Pause);
            SetAction(GameInfoOverlayCommand::TapToContinue);
            m_game->PauseGame();
            break;

        case UpdateEngineState::WaitingForResources:
        case UpdateEngineState::WaitingForPress:
            // Avoid corrupting the m_updateStateNext on a transition from Snapped -> Snapped.
            // Otherwise, just cache the current state and return to it when leaving SNAPPED layout.

            m_updateStateNext = m_updateState;
            break;

        default:
            break;
        }

        m_updateState = UpdateEngineState::Snapped;
        m_controller->Active(false);
        HideGameInfoOverlay();
        SetSnapped();
    }
    else if (ApplicationView::Value == ApplicationViewState::Filled ||
        ApplicationView::Value == ApplicationViewState::FullScreenLandscape ||
        ApplicationView::Value == ApplicationViewState::FullScreenPortrait)
    {
        if (m_updateState == UpdateEngineState::Snapped)
        {

            HideSnapped();
            ShowGameInfoOverlay();
            m_renderNeeded = true;

            if (m_haveFocus)
            {
                if (m_updateStateNext == UpdateEngineState::WaitingForPress)
                {
                    SetAction(GameInfoOverlayCommand::TapToContinue);
                    m_controller->WaitForPress(m_renderer->GameInfoOverlayUpperLeft(), m_renderer->GameInfoOverlayLowerRight());
                }
                else if (m_updateStateNext == UpdateEngineState::WaitingForResources)
                {
                    SetAction(GameInfoOverlayCommand::PleaseWait);
                }

                m_updateState = m_updateStateNext;
            }
            else
            {
                m_updateState = UpdateEngineState::Deactivated;
                SetAction(GameInfoOverlayCommand::None);
            }
        }
    }
}

//--------------------------------------------------------------------------------------

void App::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_renderer->InfoOverlay()->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        m_renderer->InfoOverlay()->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_renderer->InfoOverlay()->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_renderer->InfoOverlay()->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_renderer->InfoOverlay()->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_renderer->InfoOverlay()->SetPause();
        break;
    }
}

//--------------------------------------------------------------------------------------

void App::SetAction (GameInfoOverlayCommand command)
{
    m_gameInfoOverlayCommand = command;
    m_renderer->InfoOverlay()->SetAction(command);
}

//--------------------------------------------------------------------------------------

void App::ShowGameInfoOverlay()
{
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideGameInfoOverlay()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::SetSnapped()
{
    m_renderer->InfoOverlay()->SetPause();
    m_renderer->InfoOverlay()->ShowGameInfoOverlay();
}

//--------------------------------------------------------------------------------------

void App::HideSnapped()
{
    m_renderer->InfoOverlay()->HideGameInfoOverlay();
    SetGameInfoOverlay(m_gameInfoOverlayState);
}

//--------------------------------------------------------------------------------------

IFrameworkView^ DirectXAppSource::CreateView()
{
    return ref new App();
}

//--------------------------------------------------------------------------------------

[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}

//--------------------------------------------------------------------------------------
```

 

 






<!--HONumber=Mar16_HO1-->


