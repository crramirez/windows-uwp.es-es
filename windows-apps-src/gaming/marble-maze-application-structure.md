---
title: Estructura de la aplicación Marble Maze
description: La estructura de una aplicación DirectX para la Plataforma universal de Windows (UWP) es diferente de la de una aplicación de escritorio tradicional.
ms.assetid: 6080f0d3-478a-8bbe-d064-73fd3d432074
---

# Estructura de la aplicación Marble Maze


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


La estructura de una aplicación DirectX para la Plataforma universal de Windows (UWP) es diferente de la de una aplicación de escritorio tradicional. En lugar de trabajar con tipos de identificadores como **HWND** y funciones como **CreateWindow**, Windows Runtime proporciona interfaces como [**Windows::UI::Core::ICoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208296) para que puedas desarrollar aplicaciones para UWP de una manera más moderna y orientada a objetos. En esta sección de la documentación se muestra cómo está estructurado el código de la aplicación Marble Maze.

> **Nota**: El código de ejemplo correspondiente a este documento se encuentra en la [muestra de Marble Maze con DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
## 
Estos son algunos de los puntos principales que se tratan en este documento para cuando estructures tu código de juego:

-   En la fase de inicialización, configura los componentes de la biblioteca y el tiempo en ejecución que use tu juego. Carga también los recursos específicos del juego.
-   Las aplicaciones para UWP deben iniciar el procesamiento de eventos antes de que transcurran 5 segundos desde el inicio. Por lo tanto, carga solo los recursos fundamentales al cargar la aplicación. Los juegos deberían cargar los recursos grandes en segundo plano y mostrar una pantalla de progreso.
-   En el bucle del juego, responde a los eventos de Windows, lee la entrada del usuario, actualiza los objetos de la escena y representa la escena.
-   Usa controladores de eventos para responder a eventos de ventana. (Estos sustituyen a los mensajes de ventana de las aplicaciones de escritorio de Windows).
-   Usa una máquina de estado para controlar el flujo y el orden de la lógica del juego.

##  Organización de archivos


Algunos de los componentes de Marble Maze se pueden volver a utilizar con cualquier juego casi sin modificaciones. Para tu propio juego, puedes adaptar la organización y las ideas que proporcionan estos archivos. En la siguiente tabla se describen brevemente los archivos de código fuente importantes.

| Archivos                                      | Descripción                                                                                                                                                                          |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Audio.h, Audio.cpp                         | Define la clase **Audio**, que administra los recursos de audio.                                                                                                                          |
| BasicLoader.h, BasicLoader.cpp             | Define la clase **BasicLoader**, que proporciona métodos de utilidades que te ayudan a cargar texturas, mallas y sombreadores.                                                                  |
| BasicMath.h                                | Define las estructuras y funciones que te ayudan a trabajar con cálculos y datos de vectores y matrices. Muchas de estas funciones son compatibles con los tipos de sombreadores HLSL.                     |
| BasicLeeerWriter.h, BasicLeeerWriter.cpp | Define la clase **BasicReaderWriter**, que usa Windows Runtime para leer y escribir datos de archivos en una aplicación para UWP.                                                                    |
| BasicShapes.h, BasicShapes.cpp             | Define la clase **BasicShapes**, que proporciona métodos de utilidades para crear formas básicas como cubos y esferas. (Estos archivos no se usan en la implementación de Marble Maze). |
| BasicTimer.h, BasicTimer.cpp               | Define la clase **BasicTimer**, que proporciona una forma fácil de obtener tiempos totales y tiempos transcurridos.                                                                                         |
| Camera.h, Camera.cpp                       | Define la clase **Camera**, que proporciona la posición y la orientación de una cámara.                                                                                               |
| Collision.h, Collision.cpp                 | Administra la información de colisión entre la canica y otros objetos, como el laberinto.                                                                                                       |
| DDSTextureLoader.h, DDSTextureLoader.cpp   | Define la función **CreateDDSTextureFromMemory**, que carga texturas en formato .dds desde un búfer de memoria.                                                              |
| DirectXApp.h, DirectXApp.cpp               | Defines las clases **DirectXApp** y **DirectXAppSource**, que encapsulan la vista (ventana, subproceso y eventos) de la aplicación.                                                     |
| DirectXBase.h, DirectXBase.cpp             | Define la clase **DirectXBase**, que proporciona una infraestructura que es común a muchas aplicaciones DirectX para UWP.                                                                            |
| DirectXSample.h                            | Define las funciones de utilidad que pueden usar las aplicaciones para UWP con DirectX.                                                                                                                      |
| LoadScreen.h, LoadScreen.cpp               | Define la clase **LoadScreen**, que muestra una pantalla de carga durante la inicialización de la aplicación.                                                                                         |
| MarbleMaze.h, MarbleMaze.cpp               | Define la clase **MarbleMaze**, que administra los recursos específicos del juego y define una gran parte de la lógica del juego.                                                                          |
| MediaStreamer.h, MediaStreamer.cpp         | Define la clase **MediaStreamer**, que usa Media Foundation para que el juego administre los recursos de audio.                                                                            |
| PersistentState.h, PersistentState.cpp     | Define la clase **PersistentState**, que lee y escribe tipos de datos primitivos en una memoria auxiliar.                                                                      |
| Physics.h, Physics.cpp                     | Define la clase **Physics**, que implementa la simulación de efectos físicos entre la canica y el laberinto.                                                                              |
| Primitives.h                               | Define los tipos geométricos usados por el juego.                                                                                                                                   |
| SampleOverlay.h, SampleOverlay.cpp         | Define la clase **SampleOverlay**, que proporciona datos y operaciones comunes 2D y de la interfaz de usuario.                                                                               |
| SDKMesh.h, SDKMesh.cpp                     | Define la clase **SDKMesh**, que carga y representa las mallas que tienen formato SDK Mesh (.sdkmesh).                                                                                |
| UserInterface.h, UserInterface.cpp         | Define la funcionalidad relacionada con la interfaz de usuario, como el sistema de menús y la tabla de puntuaciones máximas.                                                                        |

 

##  Formatos de recursos en tiempo de diseño y en tiempo de ejecución


Cuando puedas, usa los formatos en tiempo de ejecución en vez de los formatos en tiempo de diseño para lograr una carga más eficaz de los recursos del juego.

Un formato en *tiempo de diseño* es el formato que se usa al diseñar los recursos. Por los general, los diseñadores 3D trabajan con formatos en tiempo de diseño. Algunos formatos en tiempo de diseño también están basados en texto, por lo que podrás modificarlos en cualquier editor basado en texto. Los formatos en tiempo de diseño pueden ser detallados y contener más información de la que requiere el juego. Un formato en *tiempo de ejecución* es el formato binario leído por el juego. Los formatos en tiempo de ejecución suelen ser más compactos y tienen una carga más eficaz que los formatos en tiempo de diseño correspondientes. Esta es la razón por la que la mayoría de los juegos usan activos de tiempo de ejecución en tiempo de ejecución.

Aunque el juego se pueda leer directamente en un formato en tiempo de diseño, hay varias ventajas para usar un formato en tiempo de ejecución aparte. Como los formatos en tiempo de ejecución suelen ser más compactos, requieren menos espacio en disco y menos tiempo para transferirlos en una red. Además, los formatos en tiempo de ejecución suelen estar representados como estructuras de datos de mapas de memoria. Por lo tanto, pueden cargarse en la memoria mucho más rápido que, por ejemplo, un archivo de texto basado en XML. Por último, como los formatos en tiempo de ejecución separados suelen tener codificación binaria, son más difíciles de modificar por parte del usuario final.

Los sombreadores HLSL son un ejemplo de recursos que usan formato en tiempo de diseño y en tiempo de ejecución diferentes. Marble Maze usa .hlsl como formato en tiempo de diseño y .cso como formato en tiempo de ejecución. Un archivo .hlsl contiene código fuente para el sombreador; un archivo .cso tiene el código de bytes de sombreador correspondiente. Al poner los archivos .hlsl sin conexión y proporcionar archivos .cso con el juego, evitas la necesidad de convertir archivos de origen HLSL en código de bytes cuando se carga el juego.

Por razones instructivas, el proyecto Marble Maze incluye el formato en tiempo de diseño y el formato en tiempo de ejecución para muchos recursos, pero solo debes mantener los formatos en tiempo de diseño en el proyecto de origen para tu propio juego porque puedes convertirlos en formatos en tiempo de ejecución cuando los necesites. Esta documentación muestra cómo convertir los formatos en tiempo de diseño en formatos en tiempo de ejecución.

##  Ciclo de vida de la aplicación


Marble Maze sigue el ciclo de vida de una aplicación para UWP típica. Para obtener más información sobre el ciclo de vida en una aplicación para UWP, consulta [Ciclo de vida de la aplicación](https://msdn.microsoft.com/library/windows/apps/mt243287).

Cuando se inicializa un juego para UWP, suele inicializar los componentes en tiempo de ejecución como Direct3D, Direct2D y las bibliotecas de métodos de entrada, audio o efectos físicos que usa. También carga recursos específicos del juego requeridos antes de que comience el juego. Esta inicialización se produce una vez durante una sesión del juego.

Después de la inicialización, los juegos suelen ejecutar el *bucle del juego*. En este bucle, los juegos suelen realizar cuatro acciones: procesar eventos de Windows, recopilar métodos de entrada, actualizar objetos de la escena y representar la escena. Cuando el juego actualiza la escena, puede aplicar el estado de entrada actual en los objetos de la escena and y simular eventos físicos, como las colisiones de objetos. El juego también puede realizar otras actividades como reproducir efectos de sonido o enviar datos por la red. Cuando el juego representa la escena, captura el estado actual de la escena y la representa en el dispositivo de pantalla. En las siguientes secciones se describen estas actividades con mayor detalle.

##  Agregar a la plantilla


La plantilla *Aplicación DirectX 11 (Windows universal)* crea una ventana principal que se puede representar con Direct3D. Esta plantilla incluye además la clase **DeviceResources**, responsable de crear todos los recursos de dispositivo de Direct3D que se necesitan para representar contenido en 3D en una aplicación para UWP. La clase **AppMain** crea el objeto de la clase **MarbleMaze**, inicia la carga de recursos, repite una serie de instrucciones para actualizar el temporizador y llama al método de representación **MarbleMaze** para cada marco. Los métodos **CreateWindowSizeDependentResources**, Update y Render para esta clase llaman a los métodos correspondientes en la clase **MarbleMaze**. El siguiente ejemplo muestra el lugar en que el constructor **AppMain** crea el objeto de la clase **MarbleMaze**. La clase de los recursos de dispositivo se pasa a esta clase para que pueda usar los objetos Direct3D en la representación.

```cpp
    m_marbleMaze = std::unique_ptr<MarbleMaze>(new MarbleMaze(m_deviceResources));
    m_marbleMaze->CreateWindowSizeDependentResources();
```

La clase **AppMain** también inicia la carga de recursos diferidos para el juego. Consulta la siguiente sección para obtener más detalles. El constructor **DirectXPage** configura los controladores de eventos, crea la clase **DeviceResources** y crea la clase **AppMain**.

Cuando los controladores de estos eventos reciben la llamada, pasan la entrada a la clase **MarbleMaze**.

## Cargar activos del juego en segundo plano


Para asegurase de que el juego responda a los eventos de ventana en menos de 5 segundos después de su inicio, recomendamos que cargues los activos del juego asincrónicamente en segundo plano. Mientras los activos se cargan en segundo plano, el juego puede responder a los eventos de la ventana.

> **Nota**  También puedes mostrar el menú principal cuando esté listo y permitir que los activos restantes sigan cargándose en segundo plano. Si el usuario selecciona una opción del menú antes de que se hayan cargado todos los recursos, puedes indicar que los recursos de la escena siguen cargándose mostrando una barra de progreso, por ejemplo.

 

Incluso si el juego contiene relativamente pocos activos, es recomendable cargarlos asincrónicamente por dos razones. Una razón es que es difícil garantizar que todos los recursos se cargarán rápidamente en todos los dispositivos y con todas las configuraciones. Además, al incorporar pronto la carga asincrónica, el código está listo para escalarse a medida que agregas funcionalidades.

La carga asincrónica de activos comienza con el método **AppMain::Load**. Este método usa la clase [**task Class (Concurrency Runtime)**](https://msdn.microsoft.com/library/windows/apps/hh750113.aspx) para cargar activos del juego en segundo plano.

```cpp
    task<void>([=]()
    {
        m_marbleMaze->LoadDeferredResources();
    });

```

La clase **MarbleMaze** define la marca *m\_deferredResourcesReady* para indicar que la carga asincrónica se completó. El método **MarbleMaze::LoadDeferredResources** carga los recursos del juego y después establece esta marca. Las fases de actualización (**MarbleMaze::Update**) y representación (**MarbleMaze::Render**) de la aplicación activan esta marca. Cuando se establece esta marca, el juego continúa de forma normal. Si la marca no está aún establecida, el juego muestra la pantalla de carga.

Para obtener más información sobre la programación asincrónica de las aplicaciones para UWP, consulta [Programación asincrónica en C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

>> > **Sugerencia**   Si escribes código del juego que forma parte de una biblioteca C++ de Windows Runtime (es decir, una DLL), podrías leer [Creación de operaciones asincrónicas en C++ para aplicaciones de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/hh750113.aspx) para aprender a crear operaciones asincrónicas que pueden consumir las aplicaciones y otras bibliotecas.

 

## El bucle del juego


El método **DirectPage::OnRendering** ejecuta el bucle del juego principal. Este método se llama en cada marco.

Para separar el código de vista y de ventanas del código específico del juego, hemos implementado el método **DirectXApp::Run** para reenviar las llamadas de actualización y representación al objeto **MarbleMaze**. El método **DirectPage::OnRendering** también define el temporizador del juego, que se usa para la animación y la simulación de efectos físicos.

El siguiente ejemplo muestra el método **DirectPage::OnRendering**, que incluye el bucle principal del juego. El bucle del juego actualiza el tiempo total y las variables de tiempo del marco, y después actualiza y representa la escena. Esto también garantiza que el contenido se represente únicamente cuando la ventana es visible.

```cpp

void DirectXPage::OnRendering(Object^ sender, Object^ args)
{
    if (m_windowVisible)
    {
        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
}
```

## La máquina de estado


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

Por ejemplo, el estado **MainMenu** define que aparezca el menú principal y que el juego no esté activo. Por el contrario, el estado **InGameActive** define que el juego esté activo y que no aparezca el menú. La clase **MarbleMaze** define la variable de miembro **m\_gameState** para mantener el estado activo del juego.

Los métodos **MarbleMaze::Update** y **MarbleMaze::Render** usan la instrucción de conmutador para realizar la lógica del estado actual. El siguiente ejemplo muestra el aspecto que podría tener esta instrucción de conmutador para el método **MarbleMaze::Update** (los detalles se quitaron para ilustrar la estructura).

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

## Control de los eventos de ventanas y de la aplicación


Windows Runtime proporciona un sistema de control de eventos orientado a los objetos para que puedas administrar más fácilmente los mensajes de Windows. Para consumir un evento en una aplicación, deberás proporcionar un controlador de eventos o un método de control de eventos que responda al evento. También deberás registrar el controlador de eventos con el origen del evento. Este proceso también se conoce como cableado de eventos.

### Soporte de la suspensión, la reanudación y el reinicio

Marble Maze se puede suspender cuando el usuario cambia a otra aplicación o cuando Windows entra en modo de bajo consumo. El juego se reanuda cuando el usuario la pasa a primer plano o cuando Windows sale de un estado de bajo consumo. Por lo general, las aplicaciones no se cierran. Windows puede terminar la aplicación cuando está en estado de suspensión y Windows requiere los recursos (como la memoria) que está usando la aplicación. Windows notifica a una aplicación cuando está a punto de suspenderse o reanudarse, pero no lo hace cuando está a punto de terminarla. Por lo tanto, la aplicación debe poder guardar (en el punto en que Windows notifique a la aplicación que está a punto de ponerla en suspensión) los datos necesarios para restaurar el estado actual del usuario cuando se reinicie la aplicación. Si la aplicación tiene un estado de usuario importantes que resulta caro de guardar, es posible que también debas guardar el estado con regularidad, incluso antes de que la aplicación reciba la notificación de suspensión. Marble Maze responda a las notificaciones de suspensión y reanudación por dos motivos:

1.  Cuando se suspende la aplicación, el juego guarda el estado actual del juego y pausa la reproducción del audio. Cuando se reanuda la aplicación, el juego reanuda la reproducción de audio.
2.  Cuando se cierra la aplicación y después se reinicia, el juego se reanuda desde su estado anterior.

Marble Maze realiza las siguientes tareas para admitir la suspensión y la reanudación:

-   Guarda su estado en un almacenamiento persistente en puntos clave del juego, como cuando el usuario llega a un punto de control.
-   Responde a notificaciones de suspensión guardando su estado en almacenamiento persistente.
-   Responde a notificaciones de reanudación cargando su estado desde el almacenamiento persistente. También carga el estado anterior durante el inicio.

Para admitir la suspensión y la reanudación, Marble Maze define la clase **PersistentState**. (Consulta PersistentState.h y PersistentState.cpp). Esta clase usa la interfaz [**Windows::Foundation::Collections::IPropertySet**](https://msdn.microsoft.com/library/windows/apps/br226054) para leer y escribir propiedades. La clase **PersistentState** proporciona métodos que leen y escriben tipos de datos primitivos (como **bool**, **int**, **float**, [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475) y [**Platform::String**](https://msdn.microsoft.com/library/windows/apps/hh755812.aspx)) en una memoria auxiliar.

```cpp
ref class PersistentState
{
public:
    void Initialize(
        _In_ Windows::Foundation::Collections::IPropertySet^ m_settingsValues,
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
    DirectX::XMFLOAT3 LoadXMFLOAT3(Platform::String^ key, DirectX::XMFLOAT3 defaultValue);
    Platform::String^ LoadString(Platform::String^ key, Platform::String^ defaultValue);

private:
    Platform::String^ m_keyName;
    Windows::Foundation::Collections::IPropertySet^ m_settingsValues;
};
```

La clase **MarbleMaze** contiene un objeto **PersistentState**. El constructor **MarbleMaze** inicializa este objeto y proporciona el almacén de datos de aplicaciones locales como el almacén de datos auxiliar.

```cpp
m_persistentState = ref new PersistentState();
m_persistentState->Initialize(ApplicationData::Current->LocalSettings->Values, "MarbleMaze");
```

Marble Maze guarda su estado cuando la canica pasa por un punto de control o por el objetivo (en el método **MarbleMaze::Update**) y cuando la ventana pierde el enfoque (en el método **MarbleMaze::OnFocusChange**). Si tu juego contiene una gran cantidad de datos de estado, recomendamos que guardes ocasionalmente el estado en el almacenamiento persistente de un modo similar porque solo tendrás unos pocos segundos para responder a la notificación de suspensión. Por lo tanto, cuando la aplicación reciba una notificación de suspensión, solo tiene que guardar los datos de estado que han cambiado.

Para responder con el objetivo de suspender y reanudar notificaciones, la clase **DirectXPage** define los métodos **SaveInternalState** y **LoadInternalState**, a los que se llama para suspender y reanudar. El método **MarbleMaze::OnSuspending** controla el evento de suspensión y el método **MarbleMaze::OnResuming** controla el evento de reanudación.

El método **MarbleMaze::OnSuspending** guarda el estado del juego y suspende el audio.

```cpp
void MarbleMaze::OnSuspending()
{
    SaveState();
    m_audio.SuspendAudio();
}
```

El método **MarbleMaze::SaveState** guarda los valores del estado del juego, como la velocidad y la posición actual de la canica, el punto de control más reciente y la tabla de puntuaciones máximas.

```cpp
void MarbleMaze::SaveState()
{
    m_persistentState->SaveXMFLOAT3(":Position", m_physics.GetPosition());
    m_persistentState->SaveXMFLOAT3(":Velocity", m_physics.GetVelocity());
    m_persistentState->SaveSingle(":ElapsedTime", m_inGameStopwatchTimer.GetElapsedTime());

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

        m_persistentState->SaveSingle(Platform::String::Concat(":ScoreTime", string), iter->elapsedTime);
        m_persistentState->SaveString(Platform::String::Concat(":ScoreTag", string), iter->tag);
    }
}
```

Cuando el juego se reanuda, solo tiene que reanudar el audio. No tiene que cargar el estado de un almacenamiento persistente porque el estado ya está cargado en la memoria.

En el documento [Agregar audio a la muestra de Marble Maze](adding-audio-to-the-marble-maze-sample.md) se explica cómo el juego suspende y reanuda el audio.

Para admitir el reinicio, el método **MarbleMaze::Initialize**, al que se llama durante el inicio, llama al método **MarbleMaze::LoadState**. El método **MarbleMaze::LoadState** lee el estado y lo aplica en los objetos del juego. Este método también establece el estado actual del juego en pausa si el juego estaba en pausa o activo cuando se suspendió. Tenemos que pausar el juego para que al usuario no le sorprenda una actividad inesperada. También pasa al menú principal si el juego noes estaba en estado de juego cuando se suspendió.

```cpp
void MarbleMaze::LoadState()
{
    XMFLOAT3 position = m_persistentState->LoadXMFLOAT3(":Position", m_physics.GetPosition());
    XMFLOAT3 velocity = m_persistentState->LoadXMFLOAT3(":Velocity", m_physics.GetVelocity());
    float elapsedTime = m_persistentState->LoadSingle(":ElapsedTime", 0.0f);

    int gameState = m_persistentState->LoadInt32(":GameState", static_cast<int>(m_gameState));
    int currentCheckpoint = m_persistentState->LoadInt32(":Checkpoint", static_cast<int>(m_currentCheckpoint));

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

        entry.elapsedTime = m_persistentState->LoadSingle(Platform::String::Concat(":ScoreTime", string), 0.0f);
        entry.tag = m_persistentState->LoadString(Platform::String::Concat(":ScoreTag", string), L"");
        m_highScoreTable.AddScoreToTable(entry);
    }
}
```

> **Important**  Marble Maze no distingue entre el inicio en frío (es decir, iniciar por primera vez sin un evento de suspensión anterior) y la reanudación desde un estado suspendido. Este diseño es el recomendado para todas las aplicaciones para UWP.

 

Para obtener más ejemplos que demuestren cómo almacenar y recuperar configuraciones y archivos desde el almacén de datos de aplicaciones locales, consulta [Inicio rápido: datos de aplicaciones locales](https://msdn.microsoft.com/library/windows/apps/hh465118). Para más información sobre los datos de aplicación, consulta [Almacenar y recuperar la configuración y otros datos de aplicación](https://msdn.microsoft.com/library/windows/apps/mt299098).

##  Pasos siguientes


Lee [Agregar contenido visual a la muestra de Marble Maze](adding-visual-content-to-the-marble-maze-sample.md) para obtener información sobre algunas de las prácticas principales que se deben tener en cuenta al trabajar con recursos visuales.

## Temas relacionados

* [Agregar contenido visual a la muestra de Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Conceptos básicos sobre la muestra de Marble Maze](marble-maze-sample-fundamentals.md)
* [Desarrollo de Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 






<!--HONumber=Mar16_HO1-->


