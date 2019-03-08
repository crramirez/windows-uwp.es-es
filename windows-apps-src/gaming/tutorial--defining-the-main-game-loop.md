---
title: Definir el objeto principal del juego
description: Vamos a ver los detalles del objeto principal de la muestra de juego y cómo las reglas que implementa se traducen en interacciones con el mundo del juego.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, objeto principal
ms.localizationpriority: medium
ms.openlocfilehash: 96aefc8b053dd7490f47910ca5bb79989855e1a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651500"
---
# <a name="define-the-main-game-object"></a>Definir el objeto principal del juego

Una vez haya disponen del marco de trabajo básico del juego de ejemplo e implementa una máquina de Estados que controla los comportamientos de sistema y usuario de nivel superior, desea examinar las reglas y los mecanismos que convierten el ejemplo de juego en un juego. Echemos un vistazo a los detalles del objeto principal del ejemplo de juego y cómo traducir reglas del juego en las interacciones con el mundo del juego.

>[!Note]
>Si no has descargado el código del juego más reciente para esta muestra, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Obtenga información sobre cómo aplicar técnicas de desarrollo básico para implementar las reglas del juego y mecanismos para un juego DirectX de UWP.

## <a name="main-game-object"></a>Objeto principal del juego

En este juego de ejemplo, __Simple3DGame__ es la clase principal objeto de juego. Una instancia de __Simple3DGame__ se construye el objeto en el __App::Load__ método.

El __Simple3DGame__ objeto de clase:
* Especifica la implementación de la lógica de juego
* Contiene métodos que se comunican:
    * Cambios en el estado del juego a la máquina de Estados definido en el marco de aplicación.
    * Cambios en el estado del juego desde la aplicación para el objeto de juego.
    * Detalles para actualizar la interfaz de usuario (superposición y pantallas de presentación) del juego, animaciones y física (la dinámica).

    >[!Note]
    >Actualización de los gráficos se controla mediante el __GameRenderer__ (clase), que contiene métodos para obtener y usar recursos de dispositivo de gráficos utilizados por el juego. Para obtener más información, consulte [marco de representación lo hago?: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md).

* Actúa como un contenedor para los datos que define una sesión de juego, nivel, o la duración, según cómo defina su juego en un nivel alto. En este caso, los datos de estado del juego son durante la vigencia del juego y se inicializan una vez cuando un usuario inicia el juego.

Para ver los métodos y datos definidos en este objeto de clase, vaya a [Simple3DGame objeto](#simple3dgame-object).

## <a name="initialize-and-start-the-game"></a>Inicializar e iniciar el juego

Cuando un jugador inicia el juego, el objeto del juego debe inicializar su estado, crear y agregar la superposición, establecer las variables que realizan un seguimiento del rendimiento del jugador y crear una instancia de los objetos que usará para generar los niveles. En este ejemplo, esto se hace cuando la nueva __GameMain__ se crea una instancia en [ __App::Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123). 

El objeto de juego, __Simple3DGame__, se crea en el __GameMain__ constructor. A continuación, se inicializa con el [ __Simple3DGame::Initialize__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250) método durante el [async crear tarea en el __GameMain__ constructor](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74).

### <a name="simple3dgameinitialize-method"></a>Método Simple3DGame::Initialize

El ejemplo de juego configura los componentes siguientes en el objeto de juego:

* Se crea un objeto de reproducción de audio.
* Se crean matrices para los primitivos gráficos del juego, incluidas matrices para los primitivos de nivel, munición y obstáculos.
* Se crea una ubicación denominada *Game* donde guardar los datos de estado de las partidas, y se coloca en la ubicación de almacenamiento de configuración de datos de aplicación especificada por [**ApplicationData::Current**](https://msdn.microsoft.com/library/windows/apps/br241619).
* Se crea un reloj de juego y el mapa de bits de representación en la partida inicial.
* Se crea una nueva cámara con un conjunto específico de parámetros de vista y proyección.
* El dispositivo de entrada (el mando) se establece con la misma rotación alrededor del eje x (pitch) y la misma rotación alrededor del eje y (yaw) de inicio que la cámara, de modo que el jugador tiene una correspondencia 1 a 1 entre la posición de control inicial y la posición de la cámara.
* Se crea el objeto de jugador y se establece en activo. Se usa un objeto de la esfera para detectar la proximidad del jugador a las paredes y obstáculos y para impedir que la cámara Introducción coloca en una posición que pueden provocar errores inmersión.
* Se crea el primitivo del mundo de juego.
* Se crean los obstáculos cilíndricos.
* Se crean los objetivos (objetos **Face**) y se numeran.
* Se crean las esferas de munición.
* Se crean los niveles.
* Se carga la puntuación más alta.
* Se carga cualquier estado de juego guardado anteriormente.

Ahora, el juego tiene instancias de todos los componentes clave: el mundo, los obstáculos, los objetivos y las esferas de munición. También tiene instancias de los niveles, que representan configuraciones de todos los componentes anteriores y sus comportamientos en cada nivel específico. Veamos cómo genera el juego los niveles.

## <a name="build-and-load-game-levels"></a>Creación y carga de los niveles de juego

La mayoría del trabajo pesado para la construcción de nivel se realiza en el __Level.h/.cpp__ archivos se encuentran en el __GameLevels__ carpeta de la solución de ejemplo. Dado que se centra en una implementación muy específica, no nos referiremos ellos aquí. Lo importante es que el código de cada nivel se ejecuta como un objeto __LevelN__ independiente. Si desea ampliar el juego, puede crear un **nivel** objeto que toma un número asignado como un parámetro y aleatoriamente coloca los objetivos y obstáculos. O bien, puede hacer que los datos de configuración de nivel de carga desde un archivo de recursos o incluso Internet.

## <a name="define-the-game-play"></a>Definir el juego.

En este punto, tenemos todos los componentes que necesitamos para ensamblar el juego. Los niveles se ha creado en la memoria de los tipos primitivos y están listos para empezar a interactuar con el Reproductor.

Sección mejores juegos reaccionan al instante a la entrada del Reproductor y proporcionan una respuesta inmediata. Esto es cierto para cualquier tipo de un juego, desde twitch-acción, en tiempo real tiro para juegos de estrategia bien meditada, basada en turnos.

### <a name="simple3dgamerungame-method"></a>Método Simple3DGame::RunGame

Al reproducir un nivel, el juego está en el __Dynamics__ estado. 

[__GameMain::Update__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329) es el bucle principal de actualización que actualiza el estado de la aplicación una vez por fotograma, tal como se muestra a continuación. En el bucle de actualización, llama a la [ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) método para controlar el trabajo si el juego en el __Dynamics__ estado.

```cpp
// Updates the application state once per frame.
void GameMain::Update()
{
    m_controller->Update(); //the controller instance has its own update loop.

    switch (m_updateState)
    {
        //...

    case UpdateEngineState::Dynamics:
        if (m_controller->IsPauseRequested())
        {
            //...
        }
        else
        {
            GameState runState = m_game->RunGame(); //when playing a level, the game is in the Dynamics state. Work is handled by Simple3DGame::RunGame method.
            switch (runState)
            {
                
      //...
```
          
[__Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) controla el conjunto de datos que define el estado actual del juego de la iteración actual del bucle de juego.

En la lógica del flujo de juego __RunGame__:
*  El método actualiza el temporizador que cuenta los segundos que quedan hasta que se complete el nivel y comprueba si el tiempo del nivel ha expirado. Esta es una de las reglas del juego: cuando expira el tiempo y no se ha disparado todos los destinos, está perdido.
*  Si se ha agotado el tiempo, el método establece el estado de juego **TimeExpired** y regresa al método **Update** en el código anterior.
*  Si queda tiempo, se pide al mando de movimiento y vista que actualice la posición de la cámara, concretamente, que actualice el ángulo de la proyección de vista normal desde el plano de la cámara (dónde mira el jugador) y la distancia que recorrió ese ángulo desde la última vez que se consultó al mando.
*  La cámara se actualiza basándose en los nuevos datos del mando de movimiento y vista.
*  Se actualiza la dinámica, es decir, la animación y los comportamientos de los objetos en el mundo del juego que son independientes del control del jugador. En este ejemplo de juego, la [ __UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) método se llama para actualizar el movimiento de las esferas munición desencadenado, la animación de los obstáculos de Pilar y el movimiento de los destinos. Para obtener más información, consulte [actualizar el mundo del juego](#update-the-game-world).
*  El método comprueba si se han cumplido los criterios para completar con éxito un nivel. En caso afirmativo, finaliza la puntuación del nivel y comprueba si era el último nivel (de seis posibles). Si se trata del último nivel, el método devuelve el estado de juego **GameComplete**; de lo contrario, devuelve el estado de juego __LevelComplete__.
*  Si el nivel no se completó, el método establece el estado de juego en __Active__ y regresa.

## <a name="update-the-game-world"></a>Actualizar el mundo del juego

En este ejemplo, cuando se ejecuta el juego, el [ __Simple3DGame::UpdateDynamics()__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) se llama al método desde el [ __Simple3DGame::RunGame__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418)método (que se llama desde [ __GameMain::Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)) para actualizar los objetos que se representan en una escena de juego.

En el __UpdateDynamics__ bucle, llame a métodos que se utilizan para establecer el mundo del juego en movimiento, independientemente de la entrada de jugadores, crean una experiencia de juego envolvente y hacer que el nivel vienen *alive*. Esto incluye gráficos que es necesario representar y bucles de animación en ejecución para conseguir una vida, respirar mundo, incluso cuando no hay ninguna entrada del Reproductor. Por ejemplo, árboles propiciando en el viento, ondas cresting a lo largo de líneas de tierra, fumar máquinas y alienígenas monstruos ajuste y se mueven por el. También incluye la interacción entre objetos, incluidas las colisiones entre la esfera del jugador y el mundo, o entre la munición y los obstáculos y objetivos.

El bucle de juego siempre debe mantener actualizando el mundo del juego si se basa en la lógica del juego, algoritmos físicos, o si se trata sencillamente aleatorio, excepto cuando el juego específicamente en pausa. 

En la muestra de juego, este principio se denomina *dinámica*, y comprende el ascenso y caída de los obstáculos en forma de columna, así como el movimiento y comportamiento físico de las esferas de munición cuando se disparan. 

### <a name="simple3dgameupdatedynamics-method"></a>Método Simple3DGame::UpdateDynamics 

Este método aborda cuatro conjuntos de cálculos:

* Las posiciones de las esferas de munición disparadas en el mundo.
* La animación de los obstáculos con forma de columna.
* La intersección de los límites del jugador y el mundo.
* Las colisiones de las esferas de munición con los obstáculos, los destinos, otras esferas de munición y el mundo.

La animación de los obstáculos es un bucle definido en **Animate.h/.cpp**. El comportamiento de la munición y los conflictos se definen los algoritmos de leyes físicas simplificada, proporcionado en el código y parametrizados por un conjunto de constantes globales para el mundo de juegos, incluidos la gravedad y material de las propiedades. Todo esto se calcula en el espacio coordinado del mundo de juego.

### <a name="review-the-flow"></a>Revisar el flujo

Ahora que hemos actualizado todos los objetos de la escena y calcula las colisiones, es necesario usar esta información se va a dibujar el objeto visual cambia correspondiente. 

Después de __GameMain::Update()__ finaliza la iteración actual del bucle de juego, el ejemplo se llama inmediatamente __Render()__ para tomar los datos del objeto actualizado y generar una nueva escena para presentar al Reproductor, como se muestra aquí. A continuación, echemos un vistazo a la representación.

```cpp
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
                // ...
                // otherwise fall through and do normal processing to get the rendering handled.
            default:
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update(); // GameMain::Update calls Simple3DGame::RunGame() if game is in Dynamics state, uses Simple3DGame::UpdateDynamics() to update game world.
                m_renderer->Render(); //Render() is called immediately after the Update() loop
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

## <a name="render-the-game-worlds-graphics"></a>Representar gráficos de juegos del mundo

Te recomendamos que los gráficos de un juego se actualicen siempre que sea posible, lo que, como máximo, sería cada vez que haya una iteración del bucle principal del juego. A medida que se producen iteraciones del bucle, el juego se actualiza, con o sin entrada del jugador, lo que permite que las animaciones y los comportamientos calculados se muestren sin saltos. Imagina si tuviéramos una sencilla escena de agua que solo se moviera cuando el jugador pulsa un botón. Tendríamos unos efectos visuales terriblemente aburridos. El aspecto de un buen juego es fluido y sin saltos.

Recuerde el bucle del juego de ejemplo como se indicó anteriormente en [ __GameMain::Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202). Si la ventana principal del juego está visible y no está acoplada o desactivada, el juego continúa actualizando y representando los resultados de esa actualización. El [ __representar__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624) estamos examinando el método presenta ahora una representación de ese estado. Esto se realiza inmediatamente después de llamar a **actualizar**, que incluye **RunGame** a Estados de las actualizaciones, que se ha explicado en la sección anterior.

Este método dibuja la proyección del mundo en 3D y, a continuación, dibuja la superposición Direct2D encima. Una vez completado, presenta la cadena de intercambio final con los búferes combinados para su representación.

>[!Note]
>Hay dos Estados de superposición de Direct2D del juego de ejemplo: uno donde el juego muestra la superposición de información de juego que contiene el mapa de bits para el menú de pausa y otra donde muestra el juego las cruces junto con los rectángulos para el movimiento con aspecto de pantalla táctil controlador. El texto de puntuación aparece dibujado en ambos estados. Para obtener más información, consulte [marco de representación lo hago?: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md).

### <a name="gamerendererrender-method"></a>Método GameRenderer::Render

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
   
        // ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            //...
            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); // Renders the 3D objects
            }

        //...
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw(); //Start drawing the overlays
        
        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud->Render(m_game); // Renders number of hits, shots, and time
        }

        if (m_gameInfoOverlay->Visible())
        {
            d2dContext->DrawBitmap(     // Renders the game overlay
                m_gameInfoOverlay->Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        //...
    }
}
```

## <a name="simple3dgame-object"></a>Objeto Simple3DGame

Estos son los métodos y los datos que se definen en el __Simple3DGame__ clase de objeto.

### <a name="methods"></a>Métodos

Los métodos internos definidos en **Simple3DGame** incluyen:

-   **Inicializar**: Establece los valores iniciales de las variables globales e inicializa los objetos del juego. Este tema se trata en el [inicializar e iniciar el juego](#initialize-and-start-the-game) sección.
-   **LoadGame**: Inicializa un nuevo nivel y comienza a cargarlo.
-   **LoadLevelAsync**: Se inicia una tarea asincrónica (si no está familiarizado con tareas asincrónicas, vea [Parallel Patterns Library](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) para inicializar el nivel y, a continuación, invocar una tarea asincrónica en el representador para cargar los recursos de nivel de dispositivo específicos. Este método se ejecuta en otro subproceso; como consecuencia, solo se puede llamar a los métodos [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) (en contraposición a los métodos [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) desde este subproceso. Los métodos de contexto de dispositivo se llaman en el método **FinalizeLoadLevel**.
-   **FinalizeLoadLevel**: Finaliza cualquier tarea de carga de nivel que sea necesario realizar en el subproceso principal. Esto incluye cualquier llamada a métodos de contexto de dispositivo de Direct3D 11 ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)).
-   **StartLevel**: Inicia el juego para un nuevo nivel.
-   **PauseGame**: Pausa el juego.
-   **RunGame**: Ejecuta una iteración del bucle del juego. Si el estado del juego es is **Active**, recibe una llamada de **App::Update** una vez cada iteración del bucle del juego.
-   **OnSuspending** y **OnResuming**: Suspende y reanuda el audio del juego, respectivamente.

Y los métodos privados:

-   **LoadSavedState** y **SaveState**: Carga y guarda el estado actual del juego, respectivamente.
-   **SaveHighScore** y **LoadHighScore**: Guarda y carga la puntuación más alta entre partidas, respectivamente.
-   **InitializeAmmo**: Restablece el estado de cada objeto de esfera usado como munición a su estado original al inicio de cada ronda.
-   **UpdateDynamics**: Se trata de un método importante, ya que actualiza todos los objetos del juego basándose en entrada de controles, física y rutinas de animación preestablecidas. Es el corazón de la interactividad que define el juego. Este tema se trata en el [actualizar el mundo del juego](#update-the-game-world) sección.

Los otros métodos públicos son captadores de propiedades que devuelven información específica de juego y de superposición al marco de la aplicación para que la muestre.

### <a name="data"></a>Datos

Al principio del ejemplo de código hay cuatro objetos cuyas instancias se actualizan mientras se ejecuta el bucle del juego.

-   **MoveLookController** objeto: Representa la entrada del Reproductor. Para más información, consulta [Agregar controles](tutorial--adding-controls.md).
-   **GameRenderer** objeto: Representa el representador de Direct3D 11 derivado el **DirectXBase** clase que controla todos los objetos específicos del dispositivo y su representación. Para obtener más información, consulte [marco de representación](tutorial--assembling-the-rendering-pipeline.md).
-   **Audio** objeto: Controla la reproducción de audio para el juego. Para obtener más información, consulte [agregar sonido](tutorial--adding-sound.md).

El resto de variables del juego contienen las listas de primitivos y sus cantidades en cada partida, así como datos y limitaciones específicas de las partidas.

## <a name="next-steps"></a>Pasos siguientes

En este momento, tiene probablemente curiosidad sobre el motor de representación real: cómo las llamadas a la __representar__ métodos en las primitivas actualizadas obtengan convierten en píxeles en la pantalla. Este tema se trata en dos partes &mdash; [marco de representación lo hago?: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md) y [II del marco de representación: Representación de juegos](tutorial-game-rendering.md). Si estás más interesado en saber cómo actualizan el estado del juego los controles del jugador, puedes consultar el tema sobre cómo [agregar controles](tutorial--adding-controls.md).
