---
title: Definir el objeto principal del juego
description: Ahora, veremos los detalles del objeto principal del juego de ejemplo y cómo las reglas que implementa se traducen en interacciones con el mundo de juegos.
ms.assetid: 6afeef84-39d0-cb78-aa2e-2e42aef936c9
ms.date: 06/24/2020
ms.topic: article
keywords: Windows 10, UWP, juegos, objeto principal
ms.localizationpriority: medium
ms.openlocfilehash: 9a6d087be6df93ee6798c29147f7fd1c820bd225
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409564"
---
# <a name="define-the-main-game-object"></a>Definir el objeto principal del juego

> [!NOTE]
> Este tema forma parte de la serie de tutoriales de [creación de una plataforma universal de Windows sencilla (UWP) con DirectX](tutorial--create-your-first-uwp-directx-game.md) . El tema de ese vínculo establece el contexto de la serie.

Una vez que haya diseñado el marco de trabajo básico del juego de ejemplo e implementado una máquina de Estados que controle los comportamientos de los usuarios y del sistema de alto nivel, querrá examinar las reglas y las mecánicas que convierten el juego de ejemplo en un juego. Echemos un vistazo a los detalles del objeto principal del juego de ejemplo y cómo traducir las reglas del juego en interacciones con el mundo de juego.

## <a name="objectives"></a>Objetivos

- Obtenga información sobre cómo aplicar las técnicas de desarrollo básicas para implementar las reglas y los mecanismos de juego para un juego DirectX de UWP.

## <a name="main-game-object"></a>Objeto principal del juego

En el juego de ejemplo **Simple3DGameDX** , **Simple3DGame** es la clase principal del juego de objetos. Una instancia de **Simple3DGame** se construye, indirectamente, a través del método **App:: Load** .

Estas son algunas de las características de la clase **Simple3DGame** .

- Contiene la implementación de la lógica de juego.
- Contiene métodos que comunican estos detalles.
  - Cambios en el estado del juego a la máquina de Estados definida en el marco de trabajo de la aplicación.
  - Cambios en el estado del juego desde la aplicación hasta el propio objeto de juego.
  - Detalles para actualizar la interfaz de usuario del juego (pantalla de superposición y cabeza arriba), animaciones y física (la dinámica).
  > [!NOTE]
  > La actualización de gráficos se controla mediante la clase **GameRenderer** , que contiene métodos para obtener y usar los recursos de dispositivo gráficos utilizados por el juego. Para obtener más información, vea [marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md).
- Actúa como contenedor de los datos que definen una sesión de juego, un nivel o una duración, en función de cómo se defina el juego en un nivel alto. En este caso, los datos de estado del juego son para la duración del juego y se inicializan una vez cuando un usuario inicia el juego.

Para ver los métodos y los datos definidos por esta clase, vea [la clase Simple3DGame](#the-simple3dgame-class) a continuación.

## <a name="initialize-and-start-the-game"></a>Inicializar e iniciar el juego

Cuando un jugador inicia el juego, el objeto del juego debe inicializar su estado, crear y agregar la superposición, establecer las variables que realizan un seguimiento del rendimiento del jugador y crear una instancia de los objetos que usará para generar los niveles. En este ejemplo, esto se hace cuando se crea la instancia de **GameMain** en **App:: Load**.

El objeto Game, de tipo **Simple3DGame**, se crea en el constructor **GameMain:: GameMain** . Después, se inicializa con el método **Simple3DGame:: Initialize** durante la corutina **GameMain:: ConstructInBackground** Fire-and-olvidó, a la que se llama desde **GameMain:: GameMain**.

### <a name="the-simple3dgameinitialize-method"></a>El método Simple3DGame:: Initialize

En el juego de ejemplo se configuran estos componentes en el objeto Game.

- Se crea un objeto de reproducción de audio.
- Se crean matrices para los primitivos gráficos del juego, incluidas matrices para los primitivos de nivel, munición y obstáculos.
- Se crea una ubicación denominada *Game* donde guardar los datos de estado de las partidas, y se coloca en la ubicación de almacenamiento de configuración de datos de aplicación especificada por [**ApplicationData::Current**](/uwp/api/windows.storage.applicationdata.current).
- Se crea un reloj de juego y el mapa de bits de representación en la partida inicial.
- Se crea una nueva cámara con un conjunto específico de parámetros de vista y proyección.
- El dispositivo de entrada (el mando) se establece con la misma rotación alrededor del eje x (pitch) y la misma rotación alrededor del eje y (yaw) de inicio que la cámara, de modo que el jugador tiene una correspondencia 1 a 1 entre la posición de control inicial y la posición de la cámara.
- Se crea el objeto de jugador y se establece en activo. Usamos un objeto sphere para detectar la proximidad del jugador con las paredes y los obstáculos y para evitar que la cámara se coloque en una posición que pueda romper la inmersión.
- Se crea el primitivo del mundo de juego.
- Se crean los obstáculos cilíndricos.
- Se crean los objetivos (objetos **Face**) y se numeran.
- Se crean las esferas de munición.
- Se crean los niveles.
- Se carga la puntuación más alta.
- Se carga cualquier estado de juego guardado anteriormente.

Ahora, el juego tiene instancias de todos los componentes clave &mdash; del mundo, el jugador, los obstáculos, los objetivos y las esferas de munición. También tiene instancias de los niveles, que representan configuraciones de todos los componentes anteriores y sus comportamientos en cada nivel específico. Ahora veamos cómo el juego compila los niveles.

## <a name="build-and-load-game-levels"></a>Compilar y cargar niveles de juego

La mayor parte del trabajo pesado para la construcción de nivel se realiza en los `Level[N].h/.cpp` archivos que se encuentran en la carpeta **GameLevels** de la solución de ejemplo. Dado que se centra en una implementación muy específica, no los cubriremos aquí. Lo importante es que el código de cada nivel se ejecute como un objeto de **nivel independiente [N]** . Si desea ampliar el juego, puede crear un objeto de **nivel [N]** que toma un número asignado como parámetro y coloca aleatoriamente los obstáculos y los destinos. O bien, puede hacer que se carguen datos de configuración de nivel de un archivo de recursos o incluso Internet.

## <a name="define-the-gameplay"></a>Definir el juego

En este momento, tenemos todos los componentes necesarios para desarrollar el juego. Los niveles se han construido en la memoria a partir de los primitivos y están listos para que el reproductor empiece a interactuar con.

Los mejores juegos reaccionan al instante a la entrada del jugador y proporcionan comentarios inmediatos. Esto se aplica a cualquier tipo de juego, desde Twitch-Action, los captadores de primera persona en tiempo real a juegos de estrategia meditados y basados en turnos.

### <a name="the-simple3dgamerungame-method"></a>El método Simple3DGame:: RunGame

Mientras un nivel de juego está en curso, el juego está en el estado de **Dynamics** . 

**GameMain:: Update** es el bucle de actualización principal que actualiza el estado de la aplicación una vez por cada fotograma, como se muestra a continuación. El bucle de actualización llama al método **Simple3DGame:: RunGame** para controlar el trabajo si el juego está en el estado de **Dynamics** .

```cppwinrt
// Updates the application state once per frame.
void GameMain::Update()
{
    // The controller object has its own update loop.
    m_controller->Update();

    switch (m_updateState)
    {
    ...
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
                ...
```
          
**Simple3DGame:: RunGame** controla el conjunto de datos que define el estado actual de la reproducción del juego para la iteración actual del bucle del juego.

Esta es la lógica de flujo de juegos en **Simple3DGame:: RunGame**.

- El método actualiza el temporizador que cuenta los segundos hasta que se completa el nivel y comprueba si ha expirado la hora del nivel. Esta es una de las reglas del juego &mdash; cuando se agota el tiempo, si no se han captado todos los destinos, se trata de un juego.
- Si se agota el tiempo, el método establece el estado del juego **TimeExpired** y vuelve al método **Update** en el código anterior.
- Si se mantiene el tiempo, el controlador de movimiento y búsqueda se sondea en busca de una actualización en la posición de la cámara. en concreto, una actualización en el ángulo de la vista normal proyectando desde el plano de la cámara (donde está buscando el reproductor) y la distancia que el ángulo ha cambiado desde que se sondeó el controlador por última vez.
- La cámara se actualiza basándose en los nuevos datos del mando de movimiento y vista.
- Se actualiza la dinámica, es decir, la animación y los comportamientos de los objetos en el mundo del juego que son independientes del control del jugador. En este juego de ejemplo, se llama al método **Simple3DGame:: UpdateDynamics** para actualizar el movimiento de las esferas de munición que se han desencadenado, la animación de los obstáculos del Pilar y el movimiento de los destinos. Para obtener más información, consulte [actualización del mundo de juegos](#update-the-game-world).
- El método comprueba si se han cumplido los criterios de la finalización correcta de un nivel. Si es así, finaliza la puntuación para el nivel y comprueba si este es el último nivel (de 6). Si es el último nivel, el método devuelve el estado de juego **GameState:: GameComplete** . de lo contrario, devuelve el estado de juego **GameState:: LevelComplete** .
- Si el nivel no está completo, el método establece el estado del juego en **GameState:: Active**y devuelve.

## <a name="update-the-game-world"></a>Actualizar el mundo del juego

En este ejemplo, cuando se ejecuta el juego, se llama al método **Simple3DGame:: UpdateDynamics** desde el método **Simple3DGame:: RunGame** (al que se llama desde **GameMain:: Update**) para actualizar los objetos que se representan en una escena de juego.

Un bucle como **UpdateDynamics** llama a los métodos que se usan para establecer el mundo del juego en movimiento, independientemente de la entrada del jugador, para crear una experiencia de juego envolvente y hacer que el nivel esté activo. Esto incluye los gráficos que se deben representar y la ejecución de bucles de animación para traer un mundo dinámico, incluso cuando no hay ninguna entrada en el reproductor. En el juego, eso podría incluir árboles que se encuentran en el viento, la cresta de las ondas a lo largo de las líneas de tierra, el fumar de la maquinaria y la Monsters y el desplazamiento. También incluye la interacción entre objetos, incluidas las colisiones entre la esfera del jugador y el mundo, o entre la munición y los obstáculos y objetivos.

Excepto cuando el juego está en pausa, el bucle del juego debe seguir actualizando el mundo del juego; Si se basa en la lógica del juego, en los algoritmos físicos o en si es simplemente aleatorio simple.

En el juego de ejemplo, este principio se denomina *Dynamics*y engloba el aumento y el descenso de los obstáculos del pilar, y el movimiento y los comportamientos físicos de las esferas de munición a medida que se activan y están en movimiento.

### <a name="the-simple3dgameupdatedynamics-method"></a>El método Simple3DGame:: UpdateDynamics 

Este método trata los cuatro conjuntos de cálculos.

- Las posiciones de las esferas de munición disparadas en el mundo.
- La animación de los obstáculos con forma de columna.
- La intersección de los límites del jugador y el mundo.
- Las colisiones de las esferas de munición con los obstáculos, los destinos, otras esferas de munición y el mundo.

La animación de los obstáculos tiene lugar en un bucle definido en los archivos de código fuente **Animate. h/. cpp** . El comportamiento de la munición y las colisiones se definen mediante algoritmos de física simplificada, suministrados en el código y parametrizados por un conjunto de constantes globales para el mundo de juego, incluidas las propiedades de la gravedad y del material. Todo esto se calcula en el espacio coordinado del mundo de juego.

### <a name="review-the-flow"></a>Revisar el flujo

Ahora que hemos actualizado todos los objetos de la escena y se han calculado las colisiones, es necesario usar esta información para dibujar los cambios visuales correspondientes. 

Después de que **GameMain:: Update** haya completado la iteración actual del bucle del juego, el ejemplo llama inmediatamente a **GameRenderer:: Render** para tomar los datos del objeto actualizado y generar una nueva escena para presentar al reproductor, como se muestra a continuación.

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
                ...
                // Otherwise, fall through and do normal processing to perform rendering.
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                    CoreProcessEventsOption::ProcessAllIfPresent);
                // GameMain::Update calls Simple3DGame::RunGame. If game is in Dynamics
                // state, uses Simple3DGame::UpdateDynamics to update game world.
                Update();
                // Render is called immediately after the Update loop.
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(
                CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

## <a name="render-the-game-worlds-graphics"></a>Presentar los gráficos del mundo de juego

Se recomienda que los gráficos de un juego se actualicen a menudo, idealmente exactamente como se repite el bucle principal del juego. A medida que el bucle se repite, se actualiza el estado del mundo del juego, con o sin la entrada del jugador. Esto permite que las animaciones calculadas y los comportamientos se muestren sin problemas. Imagine que tuvimos una escena simple de agua que se movía solo cuando el jugador presionó un botón. Eso no sería realista; un buen juego parece suave y fluido todo el tiempo.

Recuerde el bucle del juego de ejemplo tal y como se muestra anteriormente en **GameMain:: Run**. Si la ventana principal del juego está visible y no está ajustada o desactivada, el juego continúa actualizando y procesando los resultados de esa actualización. El método **GameRenderer:: Render** que examinamos a continuación representa una representación de ese estado. Esto se realiza inmediatamente después de una llamada a **GameMain:: Update**, que incluye **Simple3DGame:: RunGame** para actualizar los Estados, como se describe en la sección anterior.

**GameRenderer:: Render** dibuja la proyección del mundo 3D y, a continuación, dibuja la superposición de Direct2D en la parte superior. Una vez completado, presenta la cadena de intercambio final con los búferes combinados para su representación.

> [!NOTE]
> Hay dos Estados para la superposición de Direct2D del juego de ejemplo &mdash; , donde el juego muestra la superposición de la información del juego que contiene el mapa de bits para el menú de pausa y otro en el que el juego muestra las cruces junto con los rectángulos para el controlador de movimiento y búsqueda de la pantalla táctil. El texto de puntuación aparece dibujado en ambos estados. Para obtener más información, vea [marco de representación I: Introducción a la representación](tutorial--assembling-the-rendering-pipeline.md).

### <a name="the-gamerendererrender-method"></a>El método GameRenderer:: Render

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    ...
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            ...
            for (auto&& object : m_game->RenderObjects())
            {
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // To handle the swapchain being pre-rotated, set the D2D transformation to include it.
        d2dContext->SetTransform(m_deviceResources->GetOrientationTransform2D());

        if (m_game != nullptr && m_gameResourcesLoaded)
        {
            // This is only used after the game state has been initialized.
            m_gameHud.Render(m_game);
        }

        if (m_gameInfoOverlay.Visible())
        {
            d2dContext->DrawBitmap(
                m_gameInfoOverlay.Bitmap(),
                m_gameInfoOverlayRect
                );
        }
        ...
    }
}
```

## <a name="the-simple3dgame-class"></a>La clase Simple3DGame

Estos son los métodos y miembros de datos que se definen mediante la clase **Simple3DGame** .

### <a name="member-functions"></a>Funciones miembro

Entre las funciones miembro públicas definidas por **Simple3DGame** se incluyen las siguientes.

- **Inicializar**. Establece los valores iniciales de las variables globales e inicializa los objetos de juego. Esto se trata en la sección [inicialización e inicio del juego](#initialize-and-start-the-game) .
- **LoadGame**. Inicializa un nuevo nivel y comienza a cargarlo.
- **LoadLevelAsync**. Una corutina que inicializa el nivel y, a continuación, invoca otra corutina en el representador para cargar los recursos del nivel específico del dispositivo. Este método se ejecuta en otro subproceso; como consecuencia, solo se puede llamar a los métodos [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) (en contraposición a los métodos [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)) desde este subproceso. Los métodos de contexto de dispositivo se llaman en el método **FinalizeLoadLevel**. Si no está familiarizado con la programación asincrónica, vea [simultaneidad y operaciones asincrónicas con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).
- **FinalizeLoadLevel**. Finaliza cualquier tarea de carga de nivel que sea necesario realizar en el subproceso principal. Esto incluye cualquier llamada a métodos de contexto de dispositivo de Direct3D 11 ([**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)).
- **StartLevel**. Inicia el juego para un nuevo nivel.
- **PauseGame**. Pausa el juego.
- **RunGame**. Ejecuta una iteración del bucle del juego. Si el estado del juego es is **Active**, recibe una llamada de **App::Update** una vez cada iteración del bucle del juego.
- **OnSuspending** y **OnResuming**. Suspender o reanudar el audio del juego, respectivamente.

Estas son las funciones miembro privadas.

- **LoadSavedState** and **SaveState**. Carga y guarda el estado actual del juego, respectivamente.
- **LoadHighScore** y **SaveHighScore**. Carga y guarda la puntuación alta en los juegos, respectivamente.
- **InitializeAmmo**. Restablece el estado de cada objeto de esfera usado como munición a su estado original al inicio de cada ronda.
- **UpdateDynamics**. Se trata de un método importante porque actualiza todos los objetos de juego basados en rutinas de animación predefinidas, física y entrada de control. Es el corazón de la interactividad que define el juego. Esto se trata en la sección [actualización del mundo de juegos](#update-the-game-world) .

Los otros métodos públicos son los descriptores de acceso de propiedades que devuelven información específica de juegos y superposición al marco de trabajo de la aplicación para su presentación.

### <a name="data-members"></a>Miembros de datos

Estos objetos se actualizan cuando se ejecuta el bucle de juego.

- Objeto **MoveLookController** . Representa la entrada del reproductor. Para obtener más información, vea [Agregar controles](tutorial--adding-controls.md).
- Objeto **GameRenderer** . Representa un representador de Direct3D 11, que controla todos los objetos específicos del dispositivo y su representación. Para obtener más información, consulte el [marco de representación I](tutorial--assembling-the-rendering-pipeline.md).
- Objeto de **audio** . Controla la reproducción de audio del juego. Para obtener más información, consulte [Agregar sonido](tutorial--adding-sound.md).

El resto de las variables del juego contienen las listas de los primitivos, así como sus respectivas cantidades en el juego, y los datos y las restricciones específicos de la reproducción del juego.

## <a name="next-steps"></a>Pasos siguientes

Todavía hemos hablado sobre el motor de representación real sobre &mdash; cómo las llamadas a los métodos de **representación** en los primitivos actualizados se convierten en píxeles en la pantalla. Estos aspectos se describen en el marco de trabajo de representación de dos partes &mdash; [I: Introducción a](tutorial--assembling-the-rendering-pipeline.md) la representación y representación del [marco II: representación de juegos](tutorial-game-rendering.md). Si está más interesado en la forma en que los controles del reproductor actualizan el estado del juego, vea [Agregar controles](tutorial--adding-controls.md).
