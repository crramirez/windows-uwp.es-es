---
title: Definir el objeto principal del juego
description: Vamos a ver los detalles del objeto principal de la muestra de juego y cómo las reglas que implementa se traducen en interacciones con el mundo del juego.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, objeto principal
ms.localizationpriority: medium
ms.openlocfilehash: 96aefc8b053dd7490f47910ca5bb79989855e1a3
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8481895"
---
# <a name="define-the-main-game-object"></a>Definir el objeto principal del juego

Una vez que hayas disponen de la estructura básica del juego de muestra e implementado una máquina de estado que controla los comportamientos de sistema y de alto nivel de usuarios, querrás examinar las reglas y las mecánicas que convierten la muestra de juego en un juego. Echemos un vistazo a los detalles del objeto principal de la muestra de juego y cómo reglas del juego se traducen en interacciones con el mundo del juego.

>[!Note]
>Si no has descargado el código del juego más reciente para esta muestra, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Ten en cuenta que este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Obtén información sobre cómo aplicar técnicas de desarrollo básicas para implementar las reglas del juego y las mecánicas de un juego DirectX de UWP.

## <a name="main-game-object"></a>Objeto principal del juego

En este juego de muestra, __Simple3DGame__ es la clase del objeto principal del juego. En el método __App:: Load__ , se crea una instancia del objeto __Simple3DGame__ .

El objeto de clase __Simple3DGame__ :
* Especifica la implementación de la lógica del juego.
* Contiene métodos que comunican:
    * Cambios en el estado del juego a la máquina de estado definida en el marco de la aplicación.
    * Cambios en el estado del juego desde la aplicación en el propio objeto de juego.
    * Detalles para actualizar la interfaz de usuario (superposición y visualización frontal) del juego, animaciones y efectos físicos (la dinámica).

    >[!Note]
    >La actualización de gráficos se controla mediante la clase __GameRenderer__ , que contiene métodos para obtener y usar recursos de dispositivo de gráficos usados por el juego. Para obtener más información, consulta [el marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md).

* Actúa como contenedor para los datos que define una sesión del juego, nivel, o la duración, en función de cómo definas tu juego en un nivel alto. En este caso, los datos de estado del juego es para el ciclo de vida del juego y se inicializan una vez que un usuario cuando inicia el juego.

Para ver los datos definidos en este objeto de clase y métodos, ve al [objeto Simple3DGame](#simple3dgame-object).

## <a name="initialize-and-start-the-game"></a>Inicializar e iniciar el juego

Cuando un jugador inicia el juego, el objeto del juego debe inicializar su estado, crear y agregar la superposición, establecer las variables que realizan un seguimiento del rendimiento del jugador y crear una instancia de los objetos que usará para generar los niveles. En este ejemplo, esto se realiza cuando se crea la nueva instancia de __GameMain__ en [__App:: Load__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/App.cpp#L115-L123). 

El objeto del juego, __Simple3DGame__, se crea en el constructor de __GameMain__ . A continuación, se inicializa mediante el método de [__simple3dgame:: Initialize__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L54-L250) durante la [async crear tarea en el constructor de __GameMain__ ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L65-L74).

### <a name="simple3dgameinitialize-method"></a>Método simple3dgame:: Initialize

La muestra de juego configura los siguientes componentes en el objeto del juego:

* Se crea un objeto de reproducción de audio.
* Se crean matrices para los primitivos gráficos del juego, incluidas matrices para los primitivos de nivel, munición y obstáculos.
* Se crea una ubicación denominada *Game* donde guardar los datos de estado de las partidas, y se coloca en la ubicación de almacenamiento de configuración de datos de aplicación especificada por [**ApplicationData::Current**](https://msdn.microsoft.com/library/windows/apps/br241619).
* Se crea un reloj de juego y el mapa de bits de representación en la partida inicial.
* Se crea una nueva cámara con un conjunto específico de parámetros de vista y proyección.
* El dispositivo de entrada (el mando) se establece con la misma rotación alrededor del eje x (pitch) y la misma rotación alrededor del eje y (yaw) de inicio que la cámara, de modo que el jugador tiene una correspondencia 1 a 1 entre la posición de control inicial y la posición de la cámara.
* Se crea el objeto de jugador y se establece en activo. Usamos un objeto de esfera para detectar la proximidad del jugador para las paredes y obstáculos y evitan que la cámara Introducción colocado en una posición que podría interrumpir la inmersión.
* Se crea el primitivo del mundo de juego.
* Se crean los obstáculos cilíndricos.
* Se crean los objetivos (objetos **Face**) y se numeran.
* Se crean las esferas de munición.
* Se crean los niveles.
* Se carga la puntuación más alta.
* Se carga cualquier estado de juego guardado anteriormente.

Ahora, el juego tiene instancias de todos los componentes clave: el mundo, los obstáculos, los objetivos y las esferas de munición. También tiene instancias de los niveles, que representan configuraciones de todos los componentes anteriores y sus comportamientos en cada nivel específico. Veamos cómo genera el juego los niveles.

## <a name="build-and-load-game-levels"></a>Compilar y cargar los niveles del juego

La mayoría del trabajo pesado para la construcción de nivel se realiza en los archivos de __Level.h/.cpp__ se encuentran en la carpeta __GameLevels__ de la solución de ejemplo. Dado que se centra en una implementación muy específica, te no se protegerlas aquí. Lo importante es que el código de cada nivel se ejecuta como un objeto __LevelN__ independiente. Si quieres extender el juego, puedes crear un objeto de **nivel** que toma un número asignado como parámetro y coloca aleatoriamente los obstáculos y destinos. O bien, puedes hacer que cargue datos de configuración de nivel de un archivo de recursos, o incluso de Internet.

## <a name="define-the-game-play"></a>Definir el juego

En este punto, tenemos todos los componentes que necesitamos para ensamblar el juego. Los niveles se han generado en la memoria de los primitivos y están listos para el jugador empiece a interactuar con.

Tthe mejores juegos reaccionan de forma instantánea a la entrada del jugador y proporcionan respuestas inmediatas. Esto es así para cualquier tipo de un juego, desde juegos la acción, tirador de primera persona en tiempo real para juegos de estrategia basados en turnos.

### <a name="simple3dgamerungame-method"></a>Método Simple3DGame::RunGame

Al reproducir un nivel, el juego está en __el estado__ . 

[__Gamemain:: Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329) es el bucle de actualización principal que actualiza el estado de la aplicación una vez por fotograma, tal como se muestra a continuación. En el bucle de actualización, llama al método [__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) para controlar el trabajo si el juego está en __el estado__ .

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
          
[__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) controla el conjunto de datos que defina el estado actual del juego para la iteración actual del bucle del juego.

Lógica de juego en __RunGame__:
*  El método actualiza el temporizador que cuenta los segundos que quedan hasta que se complete el nivel y comprueba si el tiempo del nivel ha expirado. Esta es una de las reglas del juego: al tiempo que se ejecuta y no se ha disparado todos los destinos, es fin del juego.
*  Si se ha agotado el tiempo, el método establece el estado de juego **TimeExpired** y regresa al método **Update** en el código anterior.
*  Si queda tiempo, se pide al mando de movimiento y vista que actualice la posición de la cámara, concretamente, que actualice el ángulo de la proyección de vista normal desde el plano de la cámara (dónde mira el jugador) y la distancia que recorrió ese ángulo desde la última vez que se consultó al mando.
*  La cámara se actualiza basándose en los nuevos datos del mando de movimiento y vista.
*  Se actualiza la dinámica, es decir, la animación y los comportamientos de los objetos en el mundo del juego que son independientes del control del jugador. En esta muestra de juego, se llama al método [__UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) para actualizar el movimiento de las esferas de munición que se han disparado, la animación de los obstáculos de columna y el movimiento de los destinos. Para obtener más información, consulta [actualizar el mundo del juego](#update-the-game-world).
*  El método comprueba si se han cumplido los criterios para completar con éxito un nivel. En caso afirmativo, finaliza la puntuación del nivel y comprueba si era el último nivel (de seis posibles). Si se trata del último nivel, el método devuelve el estado de juego **GameComplete**; de lo contrario, devuelve el estado de juego __LevelComplete__.
*  Si el nivel no se completó, el método establece el estado de juego en __Active__ y regresa.

## <a name="update-the-game-world"></a>Actualizar el mundo del juego

En este ejemplo, cuando se ejecuta el juego, el método [__Simple3DGame::UpdateDynamics()__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L436-L856) se llama desde el método [__Simple3DGame::RunGame__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Simple3DGame.cpp#L337-L418) (que se llama desde [__gamemain:: Update__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L261-L329)) para actualizar los objetos que se representan en una escena de juego.

En el bucle __UpdateDynamics__ , llamar a métodos que se usan para establecer el mundo del juego en movimiento, con independencia del Reproductor de entrada, crea una experiencia de juego envolvente y el nivel de *cobre vida*. Esto incluye los gráficos que debe representarse y animación en ejecución recorre llevar sobre vida, respirar mundo, incluso cuando no hay ninguna entrada del jugador. Por ejemplo, los árboles influenciando el viento, ondas cresting a lo largo de líneas de tierra, fumar de máquinas y los monstruos alienígenas estiramiento y mover. También incluye la interacción entre objetos, incluidas las colisiones entre la esfera del jugador y el mundo, o entre la munición y los obstáculos y objetivos.

El bucle del juego siempre debe tener que actualizar el mundo del juego si se basa en la lógica del juego, algoritmos físicos, o si es simplemente aleatorias, excepto cuando el juego se pause específicamente. 

En la muestra de juego, este principio se denomina *dinámica*, y comprende el ascenso y caída de los obstáculos en forma de columna, así como el movimiento y comportamiento físico de las esferas de munición cuando se disparan. 

### <a name="simple3dgameupdatedynamics-method"></a>Método Simple3DGame::UpdateDynamics 

Este método aborda cuatro conjuntos de cálculos:

* Las posiciones de las esferas de munición disparadas en el mundo.
* La animación de los obstáculos con forma de columna.
* La intersección de los límites del jugador y el mundo.
* Las colisiones de las esferas de munición con los obstáculos, los destinos, otras esferas de munición y el mundo.

La animación de los obstáculos es un bucle definido en **Animate.h/.cpp**. El comportamiento de la munición y cualquier colisión se define mediante algoritmos físicos simplificados, proporcionados en el código y parametrizados por un conjunto de constantes globales para el mundo del juego, incluidas las propiedades de materiales y gravedad. Todo esto se calcula en el espacio coordinado del mundo de juego.

### <a name="review-the-flow"></a>Revisa el flujo

Ahora que hemos actualizado todos los objetos de la escena y calculado todas las colisiones, necesitamos usar esta información para dibujar los cambios visuales correspondientes. 

Una vez __GameMain::Update()__ se completa la iteración actual del bucle del juego, la muestra llama inmediatamente __Render()__ para tomar los datos actualizados del objeto y generar una nueva escena para presentar al jugador, como se muestra aquí. A continuación, echemos un vistazo a la representación.

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

## <a name="render-the-game-worlds-graphics"></a>Representar gráficos del mundo de juego

Te recomendamos que los gráficos de un juego se actualicen siempre que sea posible, lo que, como máximo, sería cada vez que haya una iteración del bucle principal del juego. A medida que se producen iteraciones del bucle, el juego se actualiza, con o sin entrada del jugador, lo que permite que las animaciones y los comportamientos calculados se muestren sin saltos. Imagina si tuviéramos una sencilla escena de agua que solo se moviera cuando el jugador pulsa un botón. Tendríamos unos efectos visuales terriblemente aburridos. El aspecto de un buen juego es fluido y sin saltos.

Recuerda el bucle del juego de muestra como se muestra anteriormente en [__gamemain:: Run__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameMain.cpp#L143-L202). Si la ventana principal del juego está visible y no está acoplada o desactivada, el juego continúa actualizando y representando los resultados de esa actualización. Ahora, el método [__Render__](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameRenderer.cpp#L474-L624) que estamos examinando representa una representación de ese estado. Esto se realiza inmediatamente después de una llamada a **Actualizar**, que incluye **RunGame** para actualizar los Estados, que se explicó en la sección anterior.

Este método dibuja la proyección del mundo en 3D y, a continuación, dibuja la superposición Direct2D encima. Una vez completado, presenta la cadena de intercambio final con los búferes combinados para su representación.

>[!Note]
>Hay dos Estados para la superposición de Direct2D del juego de muestra: uno que el juego muestra la superposición de información del juego que contiene el mapa de bits para el menú de pausa y otro donde el juego muestra el punto de mira con los rectángulos para el movimiento y vista de pantalla táctil controlador. El texto de puntuación aparece dibujado en ambos estados. Para obtener más información, consulta [Marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md).

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

## <a name="simple3dgame-object"></a>Objeto de Simple3DGame

Estos son los métodos y los datos que se definen en la clase de objeto __Simple3DGame__ .

### <a name="methods"></a>Métodos

Los métodos internos definidos en **Simple3DGame** incluyen:

-   **Inicializar**: establece los valores iniciales de las variables globales e inicializa los objetos del juego. Esto se explica en la sección de [inicializar e inicia el juego](#initialize-and-start-the-game) .
-   **LoadGame**: inicializa un nuevo nivel y comienza a cargarlo.
-   **LoadLevelAsync**: inicia una tarea asincrónica (si no está familiarizado con las tareas asincrónicas, consulta la [Biblioteca de modelos paralelos](https://docs.microsoft.com/cpp/parallel/concrt/parallel-patterns-library-ppl)) para inicializar el nivel y, a continuación, invocar una tarea asincrónica en el representador para cargar los recursos de nivel de dispositivo específico. Este método se ejecuta en otro subproceso; como consecuencia, solo se puede llamar a los métodos [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) (en contraposición a los métodos [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)) desde este subproceso. Los métodos de contexto de dispositivo se llaman en el método **FinalizeLoadLevel**.
-   **FinalizeLoadLevel**: finaliza cualquier tarea de carga de nivel que se debe hacer en el subproceso principal. Esto incluye cualquier llamada a métodos de contexto de dispositivo de Direct3D11 ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)).
-   **StartLevel**: inicia el juego para un nuevo nivel.
-   **PauseGame**: pausa el juego.
-   **RunGame**: se ejecuta una iteración del bucle del juego. Si el estado del juego es is **Active**, recibe una llamada de **App::Update** una vez cada iteración del bucle del juego.
-   **OnSuspending** y **OnResuming**: suspende y reanuda el audio del juego, respectivamente.

Y los métodos privados:

-   **LoadSavedState** y **SaveState**: carga y guarda el estado actual del juego, respectivamente.
-   **SaveHighScore** y **LoadHighScore**: guarda y carga la puntuación más alta entre partidas, respectivamente.
-   **InitializeAmmo**: restablece el estado de cada objeto de esfera usado como munición a su estado original al inicio de cada ronda.
-   **UpdateDynamics**: este es un método importante, ya que actualiza todos los objetos del juego en función de la entrada de controles, física y rutinas de animación preestablecidas. Es el corazón de la interactividad que define el juego. Esto se explica en la sección de [actualizar el mundo del juego](#update-the-game-world) .

Los otros métodos públicos son captadores de propiedades que devuelven información específica de juego y de superposición al marco de la aplicación para que la muestre.

### <a name="data"></a>Datos

Al principio del ejemplo de código hay cuatro objetos cuyas instancias se actualizan mientras se ejecuta el bucle del juego.

-   **MoveLookController** objeto: representa la entrada del jugador. Para más información, consulta [Agregar controles](tutorial--adding-controls.md).
-   Objeto de **GameRenderer** : representa el representador de Direct3D 11 derivado de la clase de **DirectXBase** que controla todos los objetos específicos del dispositivo y su representación. Para obtener más información, consulta [marco de representación I](tutorial--assembling-the-rendering-pipeline.md).
-   Objeto de **audio** : controla la reproducción de audio del juego. Para obtener más información, consulta [Agregar sonido](tutorial--adding-sound.md).

El resto de variables del juego contienen las listas de primitivos y sus cantidades en cada partida, así como datos y limitaciones específicas de las partidas.

## <a name="next-steps"></a>Pasos siguientes

En este momento, probablemente curiosidad sobre el motor de representación: cómo las llamadas a los métodos __de representación__ en los primitivos actualizados se convierten en píxeles en la pantalla. Esto se explica en dos partes &mdash; [marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md) y [marco de representación II: representación de juego](tutorial-game-rendering.md). Si estás más interesado en saber cómo actualizan el estado del juego los controles del jugador, puedes consultar el tema sobre cómo [agregar controles](tutorial--adding-controls.md).
