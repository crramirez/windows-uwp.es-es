---
title: Estructura de la aplicación Marble Maze
description: La estructura de una aplicación DirectX para la Plataforma universal de Windows (UWP) es diferente de la de una aplicación de escritorio tradicional.
ms.assetid: 6080f0d3-478a-8bbe-d064-73fd3d432074
ms.date: 09/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, muestra, directx, estructura, games, sample, structure
ms.localizationpriority: medium
ms.openlocfilehash: d19fe1a81a193baf7fe6b7b86865dfb7ea65c00b
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "7831056"
---
# <a name="marble-maze-application-structure"></a>Estructura de la aplicación Marble Maze




La estructura de una aplicación DirectX para la Plataforma universal de Windows (UWP) es diferente de la de una aplicación de escritorio tradicional. En lugar de trabajar con tipos de identificadores como [HWND](https://msdn.microsoft.com/library/windows/desktop/aa383751) y funciones como [CreateWindow](https://msdn.microsoft.com/library/windows/desktop/ms632679), Windows Runtime proporciona interfaces como [Windows::UI::Core::ICoreWindow](https://msdn.microsoft.com/library/windows/apps/br208296) para que puedas desarrollar aplicaciones para UWP de una manera más moderna y orientada a objetos. En esta sección de la documentación se muestra cómo está estructurado el código de la aplicación Marble Maze.

> [!NOTE]
> El código de ejemplo correspondiente a este documento se encuentra en el [Ejemplo de juego de Marble Maze con DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
## 
Estos son algunos de los puntos principales que se tratan en este documento para cuando estructures tu código de juego:

-   En la fase de inicialización, configura los componentes de la biblioteca y el tiempo en ejecución que use tu juego, y carga los recursos específicos del juego.
-   Las aplicaciones para UWP deben iniciar el procesamiento de eventos antes de que transcurran 5 segundos desde el inicio. Por lo tanto, carga solo los recursos fundamentales al cargar la aplicación. Los juegos deberían cargar los recursos grandes en segundo plano y mostrar una pantalla de progreso.
-   En el bucle del juego, responde a los eventos de Windows, lee la entrada del usuario, actualiza los objetos de la escena y representa la escena.
-   Usa controladores de eventos para responder a eventos de ventana. (Estos sustituyen a los mensajes de ventana de las aplicaciones de escritorio de Windows).
-   Usa una máquina de estado para controlar el flujo y el orden de la lógica del juego.

##  <a name="file-organization"></a>Organización de archivos


Algunos de los componentes de Marble Maze se pueden volver a utilizar con cualquier juego casi sin modificaciones. Para tu propio juego, puedes adaptar la organización y las ideas que proporcionan estos archivos. En la siguiente tabla se describen brevemente los archivos de código fuente importantes.

| Archivos                                      | Descripción                                                                                                                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| App.h, App.cpp               | Define las clases **App** y **DirectXApplicationSource**, que encapsulan la vista (ventana, subproceso y eventos) de la aplicación.                                                     |
| Audio.h, Audio.cpp                         | Define la clase **Audio**, que administra los recursos de audio.                                                                                                                          |
| BasicLoader.h, BasicLoader.cpp             | Define la clase **BasicLoader**, que proporciona métodos de utilidades que te ayudan a cargar texturas, mallas y sombreadores.                                                                  |
| BasicMath.h                                | Define las estructuras y funciones que te ayudan a trabajar con cálculos y datos de vectores y matrices. Muchas de estas funciones son compatibles con los tipos de sombreadores HLSL.                     |
| BasicLeeerWriter.h, BasicLeeerWriter.cpp | Define la clase **BasicReaderWriter**, que usa Windows Runtime para leer y escribir datos de archivos en una aplicación para UWP.                                                                    |
| BasicShapes.h, BasicShapes.cpp             | Define la clase **BasicShapes**, que proporciona métodos de utilidades para crear formas básicas como cubos y esferas. (Estos archivos no se usan en la implementación de Marble Maze). |                                                                                  |
| Camera.h, Camera.cpp                       | Define la clase **Camera**, que proporciona la posición y la orientación de una cámara.                                                                                               |
| Collision.h, Collision.cpp                 | Administra la información de colisión entre la canica y otros objetos, como el laberinto.                                                                                                       |
| DDSTextureLoader.h, DDSTextureLoader.cpp   | Define la función **CreateDDSTextureFromMemory**, que carga texturas en formato .dds desde un búfer de memoria.                                                              |
| DirectXHelper.h             | Define las funciones auxiliares de DirectX que son útiles para muchas aplicaciones para UWP DirectX.                                                                            |
| LoadScreen.h, LoadScreen.cpp               | Define la clase **LoadScreen**, que muestra una pantalla de carga durante la inicialización de la aplicación.                                                                                         |
| MarbleMazeMain.h, MarbleMazeMain.cpp               | Define la clase **MarbleMazeMain**, que administra los recursos específicos del juego y define una gran parte de la lógica del juego.                                                                          |
| MediaStreamer.h, MediaStreamer.cpp         | Define la clase **MediaStreamer**, que usa Media Foundation para que el juego administre los recursos de audio.                                                                            |
| PersistentState.h, PersistentState.cpp     | Define la clase **PersistentState**, que lee y escribe tipos de datos primitivos en una memoria auxiliar.                                                                      |
| Physics.h, Physics.cpp                     | Define la clase **Physics**, que implementa la simulación de efectos físicos entre la canica y el laberinto.                                                                              |
| Primitives.h                               | Define los tipos geométricos usados por el juego.                                                                                                                                   |
| SampleOverlay.h, SampleOverlay.cpp         | Define la clase **SampleOverlay**, que proporciona datos y operaciones comunes 2D y de la interfaz de usuario.                                                                               |
| SDKMesh.h, SDKMesh.cpp                     | Define la clase **SDKMesh**, que carga y representa las mallas que tienen formato SDK Mesh (.sdkmesh).                                                                                |
| StepTimer.h               | Define la clase **StepTimer**, que proporciona una forma fácil de obtener los tiempos total y transcurrido.
| UserInterface.h, UserInterface.cpp         | Define la funcionalidad relacionada con la interfaz de usuario, como el sistema de menús y la tabla de puntuaciones máximas.                                                                        |

 

##  <a name="design-time-versus-run-time-resource-formats"></a>Formatos de recursos en tiempo de diseño y en tiempo de ejecución


Cuando puedas, usa los formatos en tiempo de ejecución en vez de los formatos en tiempo de diseño para lograr una carga más eficaz de los recursos del juego.

Un formato en *tiempo de diseño* es el formato que se usa al diseñar los recursos. Por los general, los diseñadores 3D trabajan con formatos en tiempo de diseño. Algunos formatos en tiempo de diseño también están basados en texto, por lo que podrás modificarlos en cualquier editor basado en texto. Los formatos en tiempo de diseño pueden ser detallados y contener más información de la que requiere el juego. Un formato en *tiempo de ejecución* es el formato binario leído por el juego. Los formatos en tiempo de ejecución suelen ser más compactos y tienen una carga más eficaz que los formatos en tiempo de diseño correspondientes. Esta es la razón por la que la mayoría de los juegos usan activos de tiempo de ejecución en tiempo de ejecución.

Aunque el juego se pueda leer directamente en un formato en tiempo de diseño, hay varias ventajas para usar un formato en tiempo de ejecución aparte. Como los formatos en tiempo de ejecución suelen ser más compactos, requieren menos espacio en disco y menos tiempo para transferirlos en una red. Además, los formatos en tiempo de ejecución suelen estar representados como estructuras de datos de mapas de memoria. Por lo tanto, pueden cargarse en la memoria mucho más rápido que, por ejemplo, un archivo de texto basado en XML. Por último, como los formatos en tiempo de ejecución separados suelen tener codificación binaria, son más difíciles de modificar por parte del usuario final.

Los sombreadores HLSL son un ejemplo de recursos que usan formato en tiempo de diseño y en tiempo de ejecución diferentes. Marble Maze usa .hlsl como formato en tiempo de diseño y .cso como formato en tiempo de ejecución. Un archivo .hlsl contiene código fuente para el sombreador; un archivo .cso tiene el código de bytes de sombreador correspondiente. Al poner los archivos .hlsl sin conexión y proporcionar archivos .cso con el juego, evitas la necesidad de convertir archivos de origen HLSL en código de bytes cuando se carga el juego.

Por razones instructivas, el proyecto Marble Maze incluye el formato en tiempo de diseño y el formato en tiempo de ejecución para muchos recursos, pero solo debes mantener los formatos en tiempo de diseño en el proyecto de origen para tu propio juego porque puedes convertirlos en formatos en tiempo de ejecución cuando los necesites. Esta documentación muestra cómo convertir los formatos en tiempo de diseño en formatos en tiempo de ejecución.

##  <a name="application-life-cycle"></a>Ciclo de vida de la aplicación


Marble Maze sigue el ciclo de vida de una aplicación para UWP típica. Para obtener más información sobre el ciclo de vida en una aplicación para UWP, consulta [Ciclo de vida de la aplicación](https://msdn.microsoft.com/library/windows/apps/mt243287).

Cuando se inicializa un juego para UWP, suele inicializar los componentes en tiempo de ejecución como Direct3D, Direct2D y las bibliotecas de métodos de entrada, audio o efectos físicos que usa. También carga recursos específicos del juego requeridos antes de que comience el juego. Esta inicialización se produce una vez durante una sesión del juego.

Después de la inicialización, los juegos suelen ejecutar el *bucle del juego*. En este bucle, los juegos suelen realizar cuatro acciones: procesar eventos de Windows, recopilar métodos de entrada, actualizar objetos de la escena y representar la escena. Cuando el juego actualiza la escena, puede aplicar el estado de entrada actual en los objetos de la escena and y simular eventos físicos, como las colisiones de objetos. El juego también puede realizar otras actividades como reproducir efectos de sonido o enviar datos por la red. Cuando el juego representa la escena, captura el estado actual de la escena y la representa en el dispositivo de pantalla. En las siguientes secciones se describen estas actividades con mayor detalle.

##  <a name="adding-to-the-template"></a>Agregar a la plantilla


La plantilla **Aplicación DirectX 11 (Windows universal)** crea una ventana principal en la que se puede representar con Direct3D. Esta plantilla incluye además la clase **DeviceResources**, responsable de crear todos los recursos de dispositivo de Direct3D que se necesitan para representar contenido en 3D en una aplicación para UWP.

La clase **App** crea el objeto de la clase **MarbleMazeMain**, inicia la carga de recursos, repite una serie de instrucciones para actualizar el temporizador y llama al método de representación **MarbleMazeMain::Render** en cada marco. Los métodos **App::OnWindowSizeChanged**, **App::OnDpiChanged** y **App::OnOrientationChanged** llaman todos ellos al método **MarbleMazeMain::CreateWindowSizeDependentResources** y el método **App:: Run** llama a los métodos **MarbleMazeMain::Update** y **MarbleMazeMain::Render**.

El siguiente ejemplo muestra el lugar en que el método **App::SetWindow** crea el objeto de la clase **MarbleMazeMain**. La clase **DeviceResources** se pasa al método, para que pueda usar los objetos Direct3D para la representación.

```cpp
    m_main = std::unique_ptr<MarbleMazeMain>(new MarbleMazeMain(m_deviceResources));
```

La clase **App** también inicia la carga de los recursos diferidos para el juego. Consulta la siguiente sección para obtener más detalles.

Además, la clase **App** configura los controladores de eventos para los eventos [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow). Cuando los controladores de estos eventos reciben la llamada, pasan la entrada a la clase **MarbleMazeMain**.

## <a name="loading-game-assets-in-the-background"></a>Cargar activos del juego en segundo plano


Para asegurase de que el juego responda a los eventos de ventana en menos de 5 segundos después de su inicio, recomendamos que cargues los activos del juego asincrónicamente en segundo plano. A medida que los activos se cargan en segundo plano, el juego puede responder a los eventos de ventana.

> [!NOTE]
> También puedes mostrar el menú principal cuando esté listo y permitir que los activos restantes sigan cargándose en segundo plano. Si el usuario selecciona una opción del menú antes de que se hayan cargado todos los recursos, puedes indicar que los recursos de la escena siguen cargándose mostrando una barra de progreso, por ejemplo.

 

Incluso si el juego contiene relativamente pocos activos, es recomendable cargarlos asincrónicamente por dos razones. Una razón es que es difícil garantizar que todos los recursos se cargarán rápidamente en todos los dispositivos y con todas las configuraciones. Además, al incorporar pronto la carga asíncrona, el código está listo para escalarse a medida que agregas funcionalidades.

La carga asíncrona de activos comienza con el método **App::Load**. Este método usa la clase [tarea](https://docs.microsoft.com/cpp/parallel/concrt/reference/task-class) para cargar activos del juego en segundo plano.

```cpp
    task<void>([=]()
    {
        m_main->LoadDeferredResources(true, false);
    });
```

La clase **MarbleMazeMain** define la marca *m\_deferredResourcesReady* para indicar que se ha completado la carga asíncrona. El método **MarbleMazeMain::LoadDeferredResources** carga los recursos del juego y después establece esta marca. Las fases de actualización (**MarbleMazeMain::Update**) y representación (**MarbleMazeMain::Render**) de la aplicación comprueban esta marca. Cuando se establece esta marca, el juego continúa de forma normal. Si la marca no está aún establecida, el juego muestra la pantalla de carga.

Para más información sobre la programación asíncronade las aplicaciones para UWP, consulta [Programación asíncrona en C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

> [!TIP]
> Si escribes código del juego que forma parte de una biblioteca C++ de Windows Runtime (es decir, una DLL), podrías leer [Creación de operaciones asíncronas en C++ para aplicaciones de UWP](https://docs.microsoft.com/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps) para aprender a crear operaciones asincrónicas que pueden ser consumidas por aplicaciones y otras bibliotecas.

 

## <a name="the-game-loop"></a>El bucle del juego


El método **App::Run** método ejecuta el bucle principal del juego (**MarbleMazeMain::Update**). Este método se llama en cada marco.

Para ayudar a separar el código de vista y de ventanas del código específico del juego, hemos implementado el método **App::Run** para reenviar las llamadas de actualización y representación al objeto **MarbleMazeMain**.

El siguiente ejemplo muestra el método **App::Run**, que incluye el bucle principal del juego. El bucle del juego actualiza el tiempo total y las variables de tiempo del marco, y después actualiza y representa la escena. Esto también garantiza que el contenido se represente únicamente cuando la ventana es visible.

```cpp
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->
                ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }

    // The app is exiting so do the same thing as if the app were being suspended.
    m_main->OnSuspending();

#ifdef _DEBUG
    // Dump debug info when exiting.
    DumpD3DDebug();
#endif //_DEGBUG
}
```

## <a name="the-state-machine"></a>La máquina de estado


Los juegos suelen contener una *máquina de estado* (también conocida como *máquina de estado finito* o FSM) para controlar el flujo y el orden de la lógica del juego. Una máquina de estado contiene un determinado número de estados y la capacidad para pasar de uno a otro. Una máquina de estado suele iniciarse desde un estado *inicial*, pasar a uno o más estados *intermedios* y posiblemente terminar en un estado *terminal*.

Los bucles del juego suelen usar una máquina de estado para que realice la lógica específica del estado actual del juego. Marble Maze define la enumeración **GameState**, que define cada estado posible del juego.

```cpp
enum class GameState
{
    Initial,
    MainMenu,
    HighScoreDisplay,
    PreGameCountdown,
    InGameActive,
    InGamePaused,
    PostGameResults,
};
```

Por ejemplo, el estado **MainMenu** define que aparezca el menú principal y que el juego no esté activo. Por el contrario, el estado **InGameActive** define que el juego esté activo y que no aparezca el menú. La clase **MarbleMazeMain** define la variable de miembro **m\_gameState** para mantener el estado activo del juego.

Los métodos **MarbleMazeMain::Update** y **MarbleMazeMain::Render** usan instrucciones de conmutador para ejecutar la lógica del estado actual. El siguiente ejemplo muestra el aspecto que podría tener una instrucción de conmutador para el método **MarbleMazeMain::Update** (los detalles se han quitado para resaltar la estructura).

```cpp
switch (m_gameState)
{
case GameState::MainMenu:
    // Do something with the main menu. 
    break;

case GameState::HighScoreDisplay:
    // Do something with the high-score table. 
    break;

case GameState::PostGameResults:
    // Do something with the game results. 
    break;

case GameState::InGamePaused:
    // Handle the paused state. 
    break;
}
```

Cuando la lógica del juego o la representación dependen de un estado específico del juego, lo enfatizamos en esta documentación.

## <a name="handling-app-and-window-events"></a>Control de los eventos de ventanas y de la aplicación


Windows Runtime proporciona un sistema de control de eventos orientado a los objetos para que puedas administrar más fácilmente los mensajes de Windows. Para consumir un evento en una aplicación, deberás proporcionar un controlador de eventos o un método de control de eventos que responda al evento. También deberás registrar el controlador de eventos con el origen del evento. Este proceso también se conoce como cableado de eventos.

### <a name="supporting-suspend-resume-and-restart"></a>Soporte de la suspensión, la reanudación y el reinicio

Marble Maze se puede suspender cuando el usuario cambia a otra aplicación o cuando Windows entra en modo de bajo consumo. El juego se reanuda cuando el usuario la pasa a primer plano o cuando Windows sale de un estado de bajo consumo. Por lo general, las aplicaciones no se cierran. Windows puede terminar la aplicación cuando está en estado de suspensión y Windows requiere los recursos (como la memoria) que está usando la aplicación. Windows notifica a una aplicación cuando está a punto de suspenderse o reanudarse, pero no lo hace cuando está a punto de terminarla. Por lo tanto, la aplicación debe poder guardar (en el punto en que Windows notifique a la aplicación que está a punto de ponerla en suspensión) los datos necesarios para restaurar el estado actual del usuario cuando se reinicie la aplicación. Si la aplicación tiene un estado de usuario importantes que resulta caro de guardar, es posible que también debas guardar el estado con regularidad, incluso antes de que la aplicación reciba la notificación de suspensión. Marble Maze responda a las notificaciones de suspensión y reanudación por dos motivos:

1.  Cuando se suspende la aplicación, el juego guarda el estado actual del juego y pausa la reproducción del audio. Cuando se reanuda la aplicación, el juego reanuda la reproducción de audio.
2.  Cuando se cierra la aplicación y después se reinicia, el juego se reanuda desde su estado anterior.

Marble Maze realiza las siguientes tareas para admitir la suspensión y la reanudación:

-   Guarda su estado en un almacenamiento persistente en puntos clave del juego, como cuando el usuario llega a un punto de control.
-   Responde a notificaciones de suspensión guardando su estado en almacenamiento persistente.
-   Responde a notificaciones de reanudación cargando su estado desde el almacenamiento persistente. También carga el estado anterior durante el inicio.

Para admitir la suspensión y la reanudación, Marble Maze define la clase **PersistentState**. (Consulta **PersistentState.h** y **PersistentState.cpp**). Esta clase usa la interfaz [Windows::Foundation::Collections::IPropertySet](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IPropertySet) para leer y escribir propiedades. La clase **PersistentState** proporciona métodos que leen y escriben tipos de datos primitivos (como **bool**, **int**, **float**, [XMFLOAT3](https://msdn.microsoft.com/library/windows/desktop/ee419475) y [Platform::String](https://docs.microsoft.com/cpp/cppcx/platform-string-class)) en una memoria auxiliar.

```cpp
ref class PersistentState
{
internal:
    void Initialize(
        _In_ Windows::Foundation::Collections::IPropertySet^ settingsValues,
        _In_ Platform::String^ key
        );

    void SaveBool(Platform::String^ key, bool value);
    void SaveInt32(Platform::String^ key, int value);
    void SaveSingle(Platform::String^ key, float value);
    void SaveXMFLOAT3(Platform::String^ key, DirectX::XMFLOAT3 value);
    void SaveString(Platform::String^ key, Platform::String^ string);

    bool LoadBool(Platform::String^ key, bool defaultValue);
    int  LoadInt32(Platform::String^ key, int defaultValue);
    float LoadSingle(Platform::String^ key, float defaultValue);

    DirectX::XMFLOAT3 LoadXMFLOAT3(
        Platform::String^ key, 
        DirectX::XMFLOAT3 defaultValue);

    Platform::String^ LoadString(
        Platform::String^ key, 
        Platform::String^ defaultValue);

private:
    Platform::String^ m_keyName;
    Windows::Foundation::Collections::IPropertySet^ m_settingsValues;
};
```

La clase **MarbleMazeMain** contiene un objeto **PersistentState**. El constructor **MarbleMazeMain** inicializa este objeto y proporciona el almacén de datos de aplicaciones locales como almacén de datos de respaldo.

```cpp
m_persistentState = ref new PersistentState();

m_persistentState->Initialize(
    Windows::Storage::ApplicationData::Current->LocalSettings->Values,
    "MarbleMaze");
```

Marble Maze guarda su estado cuando la canica pasa por un punto de control o por el objetivo (en el método **MarbleMazeMain::Update**) y cuando la ventana pierde el enfoque (en el método **MarbleMazeMain::OnFocusChange**). Si tu juego contiene una gran cantidad de datos de estado, recomendamos que guardes ocasionalmente el estado en el almacenamiento persistente de un modo similar porque solo tendrás unos pocos segundos para responder a la notificación de suspensión. Por lo tanto, cuando la aplicación reciba una notificación de suspensión, solo tiene que guardar los datos de estado que hayan cambiado.

Para responder a las notificaciones de suspensión y reanudación, la clase **MarbleMazeMain** define los métodos **SaveState** y **LoadState**, a los que se llama al suspender y reanudar. El método **MarbleMazeMain::OnSuspending** controla el evento de suspensión y el método **MarbleMazeMain::OnResuming** controla el evento de reanudación.

El método **MarbleMazeMain::OnSuspending** guarda el estado del juego y suspende el audio.

```cpp
void MarbleMazeMain::OnSuspending()
{
    SaveState();
    m_audio.SuspendAudio();
}
```

El método **MarbleMazeMain::SaveState** guarda los valores del estado del juego, como la velocidad y la posición actual de la canica, el punto de control más reciente y la tabla de puntuaciones máximas.

```cpp
void MarbleMazeMain::SaveState()
{
    m_persistentState->SaveXMFLOAT3(":Position", m_physics.GetPosition());
    m_persistentState->SaveXMFLOAT3(":Velocity", m_physics.GetVelocity());

    m_persistentState->SaveSingle(
        ":ElapsedTime", 
        m_inGameStopwatchTimer.GetElapsedTime());

    m_persistentState->SaveInt32(":GameState", static_cast<int>(m_gameState));
    m_persistentState->SaveInt32(":Checkpoint", static_cast<int>(m_currentCheckpoint));

    int i = 0;
    HighScoreEntries entries = m_highScoreTable.GetEntries();
    const int bufferLength = 16;
    char16 str[bufferLength];

    m_persistentState->SaveInt32(":ScoreCount", static_cast<int>(entries.size()));

    for (auto iter = entries.begin(); iter != entries.end(); ++iter)
    {
        int len = swprintf_s(str, bufferLength, L"%d", i++);
        Platform::String^ string = ref new Platform::String(str, len);

        m_persistentState->SaveSingle(
            Platform::String::Concat(":ScoreTime", string), 
            iter->elapsedTime);

        m_persistentState->SaveString(
            Platform::String::Concat(":ScoreTag", string), 
            iter->tag);
    }
}
```

Cuando el juego se reanuda, solo tiene que reanudar el audio. No tiene que cargar el estado de un almacenamiento persistente porque el estado ya está cargado en la memoria.

En el documento [Agregar audio a la muestra de Marble Maze](adding-audio-to-the-marble-maze-sample.md) se explica cómo el juego suspende y reanuda el audio.

Para admitir el reinicio, el constructor **MarbleMazeMain**, al que se llama durante el inicio, llama al método **MarbleMazeMain::LoadState**. El método **MarbleMazeMain::LoadState** lee el estado y lo aplica a los objetos del juego. Este método también establece el estado actual del juego en pausa si el juego estaba en pausa o activo cuando se suspendió. Tenemos que pausar el juego para que al usuario no le sorprenda una actividad inesperada. También pasa al menú principal si el juego noes estaba en estado de juego cuando se suspendió.

```cpp
void MarbleMazeMain::LoadState()
{
    XMFLOAT3 position = m_persistentState->LoadXMFLOAT3(
        ":Position", 
        m_physics.GetPosition());

    XMFLOAT3 velocity = m_persistentState->LoadXMFLOAT3(
        ":Velocity", 
        m_physics.GetVelocity());

    float elapsedTime = m_persistentState->LoadSingle(":ElapsedTime", 0.0f);

    int gameState = m_persistentState->LoadInt32(
        ":GameState", 
        static_cast<int>(m_gameState));

    int currentCheckpoint = m_persistentState->LoadInt32(
        ":Checkpoint", 
        static_cast<int>(m_currentCheckpoint));

    switch (static_cast<GameState>(gameState))
    {
    case GameState::Initial:
        break;

    case GameState::MainMenu:
    case GameState::HighScoreDisplay:
    case GameState::PreGameCountdown:
    case GameState::PostGameResults:
        SetGameState(GameState::MainMenu);
        break;

    case GameState::InGameActive:
    case GameState::InGamePaused:
        m_inGameStopwatchTimer.SetVisible(true);
        m_inGameStopwatchTimer.SetElapsedTime(elapsedTime);
        m_physics.SetPosition(position);
        m_physics.SetVelocity(velocity);
        m_currentCheckpoint = currentCheckpoint;
        SetGameState(GameState::InGamePaused);
        break;
    }

    int count = m_persistentState->LoadInt32(":ScoreCount", 0);

    const int bufferLength = 16;
    char16 str[bufferLength];

    for (int i = 0; i < count; i++)
    {
        HighScoreEntry entry;
        int len = swprintf_s(str, bufferLength, L"%d", i);
        Platform::String^ string = ref new Platform::String(str, len);

        entry.elapsedTime = m_persistentState->LoadSingle(
            Platform::String::Concat(":ScoreTime", string), 
            0.0f);

        entry.tag = m_persistentState->LoadString(
            Platform::String::Concat(":ScoreTag", string), 
            L"");

        m_highScoreTable.AddScoreToTable(entry);
    }
}
```

> [!IMPORTANT]
> Marble Maze no distingue entre el inicio en frío (es decir, iniciar por primera vez sin un evento de suspensión anterior) y la reanudación desde un estado suspendido. Este diseño es el recomendado para todas las aplicaciones para UWP.

Para más información sobre los datos de aplicación, consulta [Almacenar y recuperar la configuración y otros datos de aplicaciones](https://msdn.microsoft.com/library/windows/apps/mt299098).

##  <a name="next-steps"></a>Pasos siguientes


Lee [Agregar contenido visual a la muestra de Marble Maze](adding-visual-content-to-the-marble-maze-sample.md) para obtener información sobre algunas de las prácticas principales que se deben tener en cuenta al trabajar con recursos visuales.

## <a name="related-topics"></a>Temas relacionados

* [Agregar contenido visual a la muestra de Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Conceptos básicos sobre la muestra de Marble Maze](marble-maze-sample-fundamentals.md)
* [Desarrollo de Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




