---
title: Agregar métodos de entrada e interactividad en la muestra de Marble Maze
description: Aprende acerca de procedimientos clave a tener en cuenta cuando trabajas con dispositivos de entrada.
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, entrada, ejemplo
ms.localizationpriority: medium
ms.openlocfilehash: aee239f76c3d4431426f0bc9fe519a59f8f48838
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8923445"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Agregar métodos de entrada e interactividad al ejemplo de Marble Maze




Los juegos de la plataforma universal de Windows (UWP) se ejecutan en una variedad de dispositivos, como equipos de escritorio, portátiles y tabletas. Un dispositivo puede tener muchos mecanismos de entrada y de control. En este documento se describen las prácticas clave que se deben tener en cuenta al trabajar con dispositivos de entrada y se muestra cómo Marble Maze aplica estas prácticas.

> [!NOTE]
> El código de ejemplo correspondiente a este documento se encuentra en el [Ejemplo de juego de Marble Maze con DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
A continuación se indican algunos de los puntos principales que se tratan en este documento para trabajar con métodos de entrada en el juego:

-   Cuando sea posible, da soporte a varios dispositivos de entrada para que el juego pueda acomodarse a una amplia variedad de preferencias y habilidades entre los clientes. Aunque los dispositivos de juego y el uso del sensor son opcionales, los recomendamos para mejorar la experiencia del jugador. Hemos diseñado API de dispositivo de juego y sensor para que te resulte más fácil integrar estos dispositivos de entrada.

-   Para inicializar la función táctil, debes registrar los eventos de ventana, por ejemplo, la activación, la liberación o el movimiento del puntero. Para inicializar el acelerómetro, crea un objeto [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687) al inicializar la aplicación. El mando Xbox no requiere inicialización.

-   Para los juegos de un solo jugador, considera la posibilidad de combinar entrada de todos los mandos posibles de Xbox. De este modo, no tendrás que hacer un seguimiento de qué entrada procede de cada controlador. O, simplemente, realizar un seguimiento de la entrada solo desde el controlador agregado más recientemente, tal y como hacemos en este ejemplo.

-   Procesa los eventos de Windows antes de procesar los dispositivos de entrada.

-   El mando de la Xbox y el acelerómetro admiten el sondeo. Es decir, puedes hacer un sondeo de datos cuando lo necesites. Para la entrada táctil, registra los eventos táctiles en estructuras de datos que estén disponibles para tu código de procesamiento de entrada.

-   Considera si deseas normalizar los valores de entrada en un formato común. Al hacerlo así, puedes simplificar la forma en que otros componentes interpretan la entrada de tu juego, como la simulación de efectos físicos, y se facilita la escritura de juegos que funcionan en varias resoluciones de pantalla.

## <a name="input-devices-supported-by-marble-maze"></a>Dispositivos de entrada compatibles con Marble Maze


Marble Maze es compatible con el mando de la Xbox, el mouse y la función táctil para seleccionar elementos de menú, mientras que para controlar el desarrollo del juego es compatible con el mando de la Xbox, el mouse, la función táctil y el acelerómetro. Marble Maze usa las API [Windows::Gaming::Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) para realizar un sondeo del controlador para la entrada. La entrada táctil permite que las aplicaciones puedan realizar un seguimiento de la entrada de las huellas de los dedos y responder a ella. El acelerómetro es un sensor que mide la fuerza que se aplica en los ejes X, Y y Z. Al usar Windows Runtime, puedes realizar un sondeo del estado actual del acelerómetro, así como recibir eventos táctiles mediante el mecanismo de administración de eventos de Windows Runtime.

> [!NOTE]
> Este documento usa el término táctil para referirse tanto a la entrada táctil como de mouse y el término puntero para referirse a cualquier dispositivo que use eventos de puntero. Dado que la función táctil y el mouse usan eventos de puntero estándar, puedes usar cualquier dispositivo para seleccionar elementos de menú y controlar el juego.

 

> [!NOTE]
> El manifiesto de paquete define que la única rotación admitida sea **Landscape** para el juego, ya que así se impide que la orientación cambie al girar el dispositivo para hacer rodar la canica. Para ver el manifiesto de paquete, abre **Package.appxmanifest** en el **Explorador de soluciones** de Visual Studio.

 

## <a name="initializing-input-devices"></a>Inicialización de dispositivos de entrada


El mando de la Xbox no requiere inicialización. Para inicializar la función táctil, debes registrar los eventos de ventana, tales como la activación, la liberación o el movimiento del puntero (por ejemplo, el usuario pulsa el botón del mouse o toca la pantalla). Para inicializar el acelerómetro, tienes que crear un objeto [Windows::Devices::Sensors::Accelerometer](https://msdn.microsoft.com/library/windows/apps/br225687) al inicializar la aplicación.

El siguiente ejemplo muestra cómo se registra el método **App::SetWindow** para los eventos de puntero [Windows::UI::Core::CoreWindow::PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerPressed), [Windows::UI::Core::CoreWindow::PointerReleased](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerReleased) y [Windows::UI::Core::CoreWindow::PointerMoved](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow.PointerMoved). Estos eventos se registran durante la inicialización de la aplicación y antes del bucle del juego.

Estos eventos se controlan en un subproceso independiente que invoca a los controladores de eventos.

Para más información acerca de cómo se inicializa la aplicación, consulta [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md).

```cpp
window->PointerPressed += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerPressed);

window->PointerReleased += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerReleased);

window->PointerMoved += ref new TypedEventHandler<CoreWindow^, PointerEventArgs^>(
    this, 
    &App::OnPointerMoved);
```

La clase **MarbleMazeMain** también crea un objeto **std::map** para eventos táctiles. La clave de este objeto map es un valor que identifica de manera exclusiva el puntero de entrada. Cada clave se asigna a la distancia entre cada punto táctil y el centro de la pantalla. Marble Maze después usa estos valores para calcular la cantidad de inclinación del laberinto.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

La clase **MarbleMazeMain** también contiene un objeto [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer).

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

El objeto **Accelerometer** se inicializa en el constructor **MarbleMazeMain**, tal como se muestra en el siguiente ejemplo. El método [Windows::Devices::Sensors::Accelerometer::GetDefault](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault) devuelve una instancia del acelerómetro predeterminado. Si no hay un acelerómetro predeterminado, **Accelerometer::GetDefault** devolverá **nullptr**.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>Navegación por los menús

Puedes usar el mouse, la función táctil o el mando de Xbox para navegar por los menús, como se indica a continuación:

-   Usa el control de dirección para cambiar el elemento de menú activo.
-   Usa la entrada táctil, el botón A, o el botón Menú para seleccionar un elemento de menú o cerrar el menú actual, como por ejemplo la tabla de máximas puntuaciones.
-   Usa el botón Menú para pausar o reanudar el juego.
-   Haz clic en un elemento de menú con el mouse para elegir esa acción.

###  <a name="tracking-xbox-controller-input"></a>Seguimiento de la entrada del mando de la Xbox

Para realizar un seguimiento de los controladores para juegos actualmente conectados al dispositivo, **MarbleMazeMain** define una variable de miembro, **m_myGamepads**, que es una colección de objetos [Windows::Gaming::Input::Gamepad](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad). Esto se inicializa en el constructor de este modo:

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

Además, el constructor **MarbleMazeMain** registra eventos para cuando se agregan o eliminan controladores para juegos:

```cpp
Gamepad::GamepadAdded += 
    ref new EventHandler<Gamepad^>([=](Platform::Object^, Gamepad^ args)
{
    m_myGamepads->Append(args);
    m_currentGamepadNeedsRefresh = true;
});

Gamepad::GamepadRemoved += 
    ref new EventHandler<Gamepad ^>([=](Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;

    if (m_myGamepads->IndexOf(args, &indexRemoved))
    {
        m_myGamepads->RemoveAt(indexRemoved);
        m_currentGamepadNeedsRefresh = true;
    }
});
```

Cuando se agrega un controlador para juegos, se agrega a **m_myGamepads**; cuando se quita un controlador para juegos, comprobamos si el controlador para juegos está en **m_myGamepads**, y si es así, se quita. En ambos casos, establecemos **m_currentGamepadNeedsRefresh** a **verdadero**, lo que indica que hay que volver a asignar **m_gamepad**.

Por último, asignamos un controlador para juegos a **m_gamepad** y ponemos **m_currentGamepadNeedsRefresh** en **falso**:

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

En el método **Update**, comprobamos si **m_gamepad** debe reasignarse:

```cpp
if (m_currentGamepadNeedsRefresh)
{
    auto mostRecentGamepad = GetLastGamepad();

    if (m_gamepad != mostRecentGamepad)
    {
        m_gamepad = mostRecentGamepad;
    }

    m_currentGamepadNeedsRefresh = false;
}
```

Si es necesario reasignar **m_gamepad**, le asignamos el controlador para juegos agregado más recientemente usando **GetLastGamepad**, que se define de este modo:

```cpp
Gamepad^ MarbleMaze::MarbleMazeMain::GetLastGamepad()
{
    Gamepad^ gamepad = nullptr;

    if (m_myGamepads->Size > 0)
    {
        gamepad = m_myGamepads->GetAt(m_myGamepads->Size - 1);
    }

    return gamepad;
}
```

Este método simplemente devuelve el último controlador para juegos en **m_myGamepads**.

Puedes conectar hasta cuatro mandos de Xbox en un dispositivo Windows 10. Para evitar tener que saber cuál es el mando activo, solo seguimos el controlador para juegos agregado más recientemente. Si el juego admite más de un jugador, se debe realizar el seguimiento de la entrada de cada jugador por separado.

El método **MarbleMazeMain::Update** sondea el controlador para juegos para la entrada:

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

Hacemos un seguimiento de la lectura de la entrada que obtuvimos en la última trama con **m_oldReading**, y la última lectura de entrada con **m_newReading**, que obtenemos llamando a [Gamepad::GetCurrentReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad.GetCurrentReading). Esto devuelve un objeto [GamepadReading](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadreading), que contiene información sobre el estado actual del controlador para juegos.

Para comprobar si se acaba de pulsar o liberar un botón, definimos **MarbleMazeMain::ButtonJustPressed** y **MarbleMazeMain::ButtonJustReleased**, que comparan las lecturas del botón de esta trama y la última. Esto permite que podamos realizar una acción solo en el momento en que se presiona o libera el botón inicialmente, y no cuando el botón se está pulsando:

```cpp
bool MarbleMaze::MarbleMazeMain::ButtonJustPressed(GamepadButtons selection)
{
    bool newSelectionPressed = (selection == (m_newReading.Buttons & selection));
    bool oldSelectionPressed = (selection == (m_oldReading.Buttons & selection));
    return newSelectionPressed && !oldSelectionPressed;
}

bool MarbleMaze::MarbleMazeMain::ButtonJustReleased(GamepadButtons selection)
{
    bool newSelectionReleased = 
        (GamepadButtons::None == (m_newReading.Buttons & selection));

    bool oldSelectionReleased = 
        (GamepadButtons::None == (m_oldReading.Buttons & selection));

    return newSelectionReleased && !oldSelectionReleased;
}
```

Las lecturas de los [GamepadButtons](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepadbuttons) se comparan usando operaciones bit a bit&mdash;comprobamos si se está pulsando un botón con *bit a bit y* (&). Podemos determinar si un botón se acaba de pulsar o liberar comparando la lectura antigua con la nueva.

Con los métodos anteriores, comprobamos si determinados botones se han pulsado y si realizan todas las acciones correspondientes que se deben producir. Por ejemplo, al pulsar el botón Menú (**GamepadButtons::Menu**), el estado del juego cambia de activo a pausa o de pausa a activo.

```cpp
if (ButtonJustPressed(GamepadButtons::Menu) || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;

    if (m_gameState == GameState::InGameActive)
    {
        SetGameState(GameState::InGamePaused);
    }  
    else if (m_gameState == GameState::InGamePaused)
    {
        SetGameState(GameState::InGameActive);
    }
}
```

También comprobamos si el jugador pulsa el botón Vista, en cuyo caso se reinicia el juego o se borra la tabla de puntuaciones máximas:

```cpp
if (ButtonJustPressed(GamepadButtons::View) || m_homeKeyPressed)
{
    m_homeKeyPressed = false;

    if (m_gameState == GameState::InGameActive ||
        m_gameState == GameState::InGamePaused ||
        m_gameState == GameState::PreGameCountdown)
    {
        SetGameState(GameState::MainMenu);
        m_inGameStopwatchTimer.SetVisible(false);
        m_preGameCountdownTimer.SetVisible(false);
    }
    else if (m_gameState == GameState::HighScoreDisplay)
    {
        m_highScoreTable.Reset();
    }
}
```

Si el menú principal está activo, el elemento de menú activo cambia cuando se presiona el control de dirección hacia arriba o hacia abajo. Si el usuario elige la selección actual, el elemento de interfaz de usuario se marca como elegido.

```cpp
// Handle menu navigation.
bool chooseSelection = 
    (ButtonJustPressed(GamepadButtons::A) 
    || ButtonJustPressed(GamepadButtons::Menu));

bool moveUp = ButtonJustPressed(GamepadButtons::DPadUp);
bool moveDown = ButtonJustPressed(GamepadButtons::DPadDown);

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);
        if (m_startGameButton.GetSelected())
        {
            m_startGameButton.SetPressed(true);
        }
        if (m_highScoreButton.GetSelected())
        {
            m_highScoreButton.SetPressed(true);
        }
    }
    if (moveUp || moveDown)
    {
        m_startGameButton.SetSelected(!m_startGameButton.GetSelected());
        m_highScoreButton.SetSelected(!m_startGameButton.GetSelected());
        m_audio.PlaySoundEffect(MenuChangeEvent);
    }
    break;

case GameState::HighScoreDisplay:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::MainMenu);
    }
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
    {
        SetGameState(GameState::HighScoreDisplay);
    }
    break;

case GameState::InGamePaused:
    if (m_pausedText.IsPressed())
    {
        m_pausedText.SetPressed(false);
        SetGameState(GameState::InGameActive);
    }
    break;
}
```

### <a name="tracking-touch-and-mouse-input"></a>Seguimiento de la entrada táctil y de mouse

Para la entrada táctil y de mouse, se elige un elemento de menú cuando el usuario lo toca o hace clic en él. En el siguiente ejemplo se muestra cómo el método **MarbleMazeMain::Update** procesa la entrada del puntero para seleccionar elementos de menú. La variable de miembro **m\_pointQueue** realiza un seguimiento de las ubicaciones en las que el usuario ha pulsado o ha hecho clic en la pantalla. La forma en que Marble Maze recopila la entrada del puntero se describe con más detalle más adelante en este documento, en la sección [Procesamiento de entrada de puntero](#processing-pointer-input).

```cpp
// Check whether the user chose a button from the UI. 
bool anyPoints = !m_pointQueue.empty();
while (!m_pointQueue.empty())
{
    UserInterface::GetInstance().HitTest(m_pointQueue.front());
    m_pointQueue.pop();
}
```

El método **UserInterface::HitTest** determina si el punto proporcionado se encuentra dentro de los límites de cualquier elemento de la interfaz de usuario. Los elementos de la interfaz de usuario que pasen esta prueba se marcan como tocados. Este método usa la función auxiliar **PointInRect** para determinar si el punto proporcionado se encuentra dentro de los límites de cada elemento de la interfaz de usuario.

```cpp
void UserInterface::HitTest(D2D1_POINT_2F point)
{
    for (auto iter = m_elements.begin(); iter != m_elements.end(); ++iter)
    {
        if (!(*iter)->IsVisible())
            continue;

        TextButton* textButton = dynamic_cast<TextButton*>(*iter);
        if (textButton != nullptr)
        {
            D2D1_RECT_F bounds = (*iter)->GetBounds();
            textButton->SetPressed(PointInRect(point, bounds));
        }
    }
}
```

### <a name="updating-the-game-state"></a>Actualización del estado de juego

Una vez que el método **MarbleMazeMain::Update** procesa la entrada táctil y del mando, actualiza el estado del juego si se pulsó cualquier botón.

```cpp
// Update the game state if the user chose a menu option. 
if (m_startGameButton.IsPressed())
{
    SetGameState(GameState::PreGameCountdown);
    m_startGameButton.SetPressed(false);
}
if (m_highScoreButton.IsPressed())
{
    SetGameState(GameState::HighScoreDisplay);
    m_highScoreButton.SetPressed(false);
}
```

##  <a name="controlling-game-play"></a>Control del juego


El bucle del juego y el método **MarbleMazeMain::Update** funcionan juntos para actualizar el estado de los objetos del juego. Si el juego acepta la entrada de varios dispositivos, puedes acumular la entrada desde todos los dispositivos en un conjunto de variables para que puedas escribir código que sea más fácil de mantener. El método **MarbleMazeMain::Update** define un conjunto de variables que acumula el movimiento de todos los dispositivos.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

El mecanismo de entrada puede variar entre los distintos dispositivos de entrada. Por ejemplo, la entrada del puntero está controlada mediante el modelo de administración de eventos de Windows Runtime. Por otra parte, se hace un sondeo de los datos de entrada desde el mando de la Xbox cuando se necesite. Recomendamos que siempre sigas el mecanismo de entrada indicado para cada dispositivo determinado. En esta sección se describe cómo Marble Maze lee la entrada de cada dispositivo, cómo actualiza los valores de entrada combinados y cómo usa los valores de entrada combinados para actualizar el estado del juego.

###  <a name="processing-pointer-input"></a>Procesamiento de entrada de puntero

Cuando trabajes con la entrada de puntero, llama al método [Windows::UI::Core::CoreDispatcher::ProcessEvents](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) para procesar eventos de ventana. Llama a este método en el bucle del juego antes de actualizar o representar la escena. Marble Maze lo llama en el método **App:: Run**: 

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        m_main->Update();

        if (m_main->Render())
        {
            m_deviceResources->Present();
        }
    }
    else
    {
        CoreWindow::GetForCurrentThread()->
            Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

Si la ventana es visible, pasamos **CoreProcessEventsOption::ProcessAllIfPresent** a **ProcessEvents** para procesar todos los eventos en cola y regresar inmediatamente; de lo contrario, pasamos **CoreProcessEventsOption::ProcessOneAndAllPending** para procesar todos los eventos en cola y esperar al siguiente evento nuevo. Una vez que se han procesado los eventos, Marble Maze representa y presenta la siguiente trama.

La llamada Windows Runtime llama al controlador registrado para cada evento que haya ocurrido. El método **App::SetWindow** registra eventos y reenvía la información del puntero a la clase **MarbleMazeMain**.

```cpp
void App::OnPointerPressed(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void App::OnPointerReleased(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->RemoveTouch(args->CurrentPoint->PointerId);
}

void App::OnPointerMoved(
    Windows::UI::Core::CoreWindow^ sender, 
    Windows::UI::Core::PointerEventArgs^ args)
{
    m_main->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

La clase **MarbleMazeMain** reacciona ante los eventos de puntero mediante la actualización del objeto map que contiene los eventos de entrada táctil. Se llama al método **MarbleMazeMain::AddTouch** cuando se presiona el puntero por primera vez, por ejemplo, si el usuario toca inicialmente la pantalla en un dispositivo con funcionalidad táctil. Se llama al método **MarbleMazeMain::UpdateTouch** cuando se mueve la posición del puntero. Se llama al método **MarbleMazeMain::RemoveTouch** cuando se libera el puntero, por ejemplo, si el usuario deja de tocar la pantalla.

```cpp
void MarbleMazeMain::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMazeMain::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_deviceResources->GetLogicalSize());
}

void MarbleMazeMain::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

La función **PointToTouch** traslada la posición actual del puntero de modo que el origen esté en el centro de la pantalla y después escala las coordenadas para que estén en un rango aproximado entre -1,0 y +1,0. Esto facilita el cálculo de la inclinación del laberinto de forma constante en todos los distintos métodos de entrada.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

El método **MarbleMazeMain::Update** actualiza los valores de entrada combinada incrementando el factor de inclinación en un valor de escala constante. Este valor de escala se determinó experimentando varios valores distintos.

```cpp
// Account for touch input.
for (TouchMap::const_iterator iter = m_touches.cbegin(); 
    iter != m_touches.cend(); 
    ++iter)
{
    combinedTiltX += iter->second.x * m_touchScaleFactor;
    combinedTiltY += iter->second.y * m_touchScaleFactor;
}
```

### <a name="processing-accelerometer-input"></a>Procesamiento de la entrada del acelerómetro

Para procesar la entrada del acelerómetro, el método **MarbleMazeMain::Update** llama al método [Windows::Devices::Sensors::Accelerometer::GetCurrentReading](https://msdn.microsoft.com/library/windows/apps/br225699). Este método devuelve un objeto [Windows::Devices::Sensors::AccelerometerReading](https://msdn.microsoft.com/library/windows/apps/br225688), que representa una lectura del acelerómetro. Las propiedades **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** y **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** contienen la aceleración de fuerza G a lo largo de los ejes X e Y, respectivamente.

En el siguiente ejemplo se muestra cómo el método **MarbleMazeMain::Update** sondea el acelerómetro y actualiza los valores de entrada combinada. Al inclinar el dispositivo, la gravedad hace que la canica se mueva más rápido.

```cpp
// Account for sensors.
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += 
            static_cast<float>(reading->AccelerationX) * m_accelerometerScaleFactor;

        combinedTiltY += 
            static_cast<float>(reading->AccelerationY) * m_accelerometerScaleFactor;
    }
}
```

Como no se puede estar seguro de que el equipo del usuario tenga un acelerómetro, asegúrate de tener siempre un objeto [Accelerometer](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Accelerometer) válido antes de sondear el acelerómetro.

### <a name="processing-xbox-controller-input"></a>Procesamiento de la entrada del mando de la Xbox

En el método **MarbleMazeMain::Update**, usamos **m_newReading** para procesar la entrada desde el stick analógico izquierdo:

```cpp
float leftStickX = static_cast<float>(m_newReading.LeftThumbstickX);
float leftStickY = static_cast<float>(m_newReading.LeftThumbstickY);

auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

if ((oppositeSquared + adjacentSquared) > m_deadzoneSquared)
{
    combinedTiltX += leftStickX * m_controllerScaleFactor;
    combinedTiltY += leftStickY * m_controllerScaleFactor;
}
```

Comprobamos si la entrada desde el stick analógico izquierdo está fuera de la zona muerta, y si es así, la agregamos a **combinedTiltX** y **combinedTiltY** (multiplicada por un factor de escala) a fin de inclinar la escena.

> [!IMPORTANT]
> Cuando trabajes con el mando de la Xbox, siempre ten en cuenta la zona muerta. La zona muerta se refiere a la varianza entre mandos y sus sensibilidades para el movimiento inicial. En algunos mandos, es posible que un pequeño movimiento no genere ninguna lectura, pero en otros podría generar una lectura apreciable. Para tener esto en cuenta en tu juego, crea una zona sin movimiento para el movimiento inicial del stick analógico. Para obtener más información sobre la zona muerta, consulta [Lectura de las palancas de control](gamepad-and-vibration.md#reading-the-thumbsticks).

 

###  <a name="applying-input-to-the-game-state"></a>Aplicación de la entrada al estado del juego

Los dispositivos notifican los valores de entrada de varias maneras. Por ejemplo, la entrada del puntero podría expresarse en coordenadas de la pantalla y la entrada del mando podría estar en un formato totalmente distinto. Uno de los retos al combinar la entrada de varios dispositivos en un único conjunto de valores de entrada es la normalización, es decir, la conversión de valores a un formato común. Marble Maze normaliza los valores escalándolos en el intervalo \[-1.0, 1.0\]. La función **PointToTouch**, que ya se ha descrito en esta sección, convierte las coordenadas de pantalla en valores normalizados que están en un intervalo aproximado entre -1,0 y +1,0.

> [!TIP]
> Incluso si la aplicación usa un método de entrada, recomendamos que siempre normalices los valores de entrada. Al hacerlo así, puedes simplificar la forma en que otros componentes interpretan la entrada de tu juego, como la simulación de efectos físicos, y se facilita la escritura de juegos que funcionan en varias resoluciones de pantalla.

 

Una vez que el método **MarbleMazeMain::Update** procesa la entrada, crea un vector que representa el efecto de la inclinación del laberinto en la canica. En el siguiente ejemplo se muestra cómo Marble Maze usa la función [XMVector3Normalize](https://msdn.microsoft.com/library/windows/desktop/microsoft.directx_sdk.geometric.xmvector3normalize) para crear un vector de gravedad normalizado. La variable **maxTilt** limita la cantidad de inclinación del laberinto e impide que este se incline de lado.

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

Para completar la actualización de los objetos de la escena, Marble Maze pasa el vector de gravedad actualizado a la simulación de efectos físicos, actualiza la simulación de efectos físicos durante el tiempo transcurrido desde el marco anterior y actualiza la posición y la orientación de la canica. Si la canica se sale del laberinto, el método **MarbleMazeMain::Update** vuelve a colocarla en el último punto de control que tocó la canica y restablece el estado de la simulación de efectos físicos.

```cpp
XMFLOAT3A g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);

if (m_gameState == GameState::InGameActive)
{
    // Only update physics when gameplay is active.
    m_physics.UpdatePhysicsSimulation(static_cast<float>(m_timer.GetElapsedSeconds()));

    // ...Code omitted for simplicity...

}

// ...Code omitted for simplicity...

// Check whether the marble fell off of the maze. 
const float fadeOutDepth = 0.0f;
const float resetDepth = 80.0f;
if (marblePosition.z >= fadeOutDepth)
{
    m_targetLightStrength = 0.0f;
}
if (marblePosition.z >= resetDepth)
{
    // Reset marble.
    memcpy(&marblePosition, &m_checkpoints[m_currentCheckpoint], sizeof(XMFLOAT3));
    oldMarblePosition = marblePosition;
    m_physics.SetPosition((const XMFLOAT3&)marblePosition);
    m_physics.SetVelocity(XMFLOAT3(0, 0, 0));
    m_lightStrength = 0.0f;
    m_targetLightStrength = 1.0f;

    m_resetCamera = true;
    m_resetMarbleRotation = true;
    m_audio.PlaySoundEffect(FallingEvent);
}
```

En esta sección no se describe cómo funciona la simulación de efectos físicos. Para más información sobre esto, consulta **Physics.h** and **Physics.cpp**, en los orígenes de Marble Maze.

## <a name="next-steps"></a>Pasos siguientes


Lee [Agregar audio a la muestra de Marble Maze](adding-audio-to-the-marble-maze-sample.md) para obtener información sobre algunas de las prácticas principales que se deben tener en cuenta al trabajar con audio. En el documento se trata la forma en que Marble Maze usa Microsoft Media Foundation y XAudio2 para cargar, mezclar y reproducir recursos de audio.

## <a name="related-topics"></a>Temas relacionados


* [Agregar audio a la muestra de Marble Maze](adding-audio-to-the-marble-maze-sample.md)
* [Agregar contenido visual a la muestra de Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Desarrollar Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 




