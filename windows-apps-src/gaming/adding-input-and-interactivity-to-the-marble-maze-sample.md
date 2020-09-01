---
title: Agregar métodos de entrada e interactividad en la muestra de Marble Maze
description: Obtenga información sobre las prácticas clave que hay que tener en cuenta al trabajar con dispositivos de entrada.
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, entrada, ejemplo
ms.localizationpriority: medium
ms.openlocfilehash: d4c3742ed843deca9d7d8edba033addd2e4888fe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172079"
---
# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Agregar métodos de entrada e interactividad en la muestra de Marble Maze




Los juegos de Plataforma universal de Windows (UWP) se ejecutan en una amplia variedad de dispositivos, como equipos de escritorio, equipos portátiles y tabletas. Un dispositivo puede tener una gran cantidad de mecanismos de entrada y control. En este documento se describen las prácticas clave que se deben tener en cuenta al trabajar con dispositivos de entrada y se muestra cómo Marble Maze aplica estas prácticas.

> [!NOTE]
> El código de ejemplo que corresponde a este documento se encuentra en el [ejemplo Game Marble Maze de DirectX](https://github.com/microsoft/Windows-appsample-marble-maze).

 
A continuación se indican algunos de los puntos principales que se tratan en este documento para trabajar con métodos de entrada en el juego:

-   Cuando sea posible, da soporte a varios dispositivos de entrada para que el juego pueda acomodarse a una amplia variedad de preferencias y habilidades entre los clientes. Aunque los dispositivos de juego y el uso del sensor son opcionales, los recomendamos para mejorar la experiencia del jugador. Hemos diseñado el dispositivo de juego y las API de sensor para facilitar la integración de estos dispositivos de entrada.

-   Para inicializar la función táctil, debes registrar los eventos de ventana, por ejemplo, la activación, la liberación o el movimiento del puntero. Para inicializar el acelerómetro, crea un objeto [Windows::Devices::Sensors::Accelerometer](/uwp/api/Windows.Devices.Sensors.Accelerometer) al inicializar la aplicación. La controladora Xbox no requiere inicialización.

-   En el caso de los juegos de un solo jugador, considere la posibilidad de combinar la entrada de todos los controladores de Xbox posibles. De este modo, no tendrás que hacer un seguimiento de qué entrada procede de cada controlador. O bien, solo tiene que realizar un seguimiento de la entrada desde el último controlador agregado, como hacemos en este ejemplo.

-   Procesa los eventos de Windows antes de procesar los dispositivos de entrada.

-   La controladora Xbox y el acelerómetro admiten el sondeo. Es decir, puedes hacer un sondeo de datos cuando lo necesites. Para la entrada táctil, registra los eventos táctiles en estructuras de datos que estén disponibles para tu código de procesamiento de entrada.

-   Considera si deseas normalizar los valores de entrada en un formato común. Al hacerlo así, puedes simplificar la forma en que otros componentes interpretan la entrada de tu juego, como la simulación de efectos físicos, y se facilita la escritura de juegos que funcionan en varias resoluciones de pantalla.

## <a name="input-devices-supported-by-marble-maze"></a>Dispositivos de entrada compatibles con Marble Maze


Marble Maze es compatible con el controlador Xbox, el mouse y el toque para seleccionar elementos de menú y el controlador Xbox, el mouse, el toque y el acelerómetro para controlar la reproducción de juegos. Marble Maze usa las API [Windows:: Gaming:: Input](/uwp/api/windows.gaming.input) para sondear el controlador en busca de datos de entrada. La entrada táctil permite que las aplicaciones puedan realizar un seguimiento de la entrada de las huellas de los dedos y responder a ella. Un acelerómetro es un sensor que mide la fuerza que se aplica a lo largo de los ejes X, Y y Z. Al usar Windows Runtime, puedes realizar un sondeo del estado actual del acelerómetro, así como recibir eventos táctiles mediante el mecanismo de administración de eventos de Windows Runtime.

> [!NOTE]
> En este documento se usa la función táctil para hacer referencia tanto a la entrada táctil como a la entrada del mouse y al puntero para hacer referencia a cualquier dispositivo que utilice eventos de puntero. Dado que la función táctil y el mouse usan eventos de puntero estándar, puedes usar cualquier dispositivo para seleccionar elementos de menú y controlar el juego.

 

> [!NOTE]
> El manifiesto del paquete establece el **panorama** como el único giro admitido para que el juego Evite que la orientación cambie al girar el dispositivo para revertir la canica. Para ver el manifiesto del paquete, Abra **Package. appxmanifest** en el **Explorador de soluciones** en Visual Studio.

 

## <a name="initializing-input-devices"></a>Inicialización de dispositivos de entrada


La controladora Xbox no requiere inicialización. Para inicializar Touch, debe registrarse para eventos de ventana como cuando se activa el puntero (por ejemplo, el reproductor presiona el botón del mouse o toca la pantalla), se suelta y se mueve. Para inicializar el acelerómetro, tienes que crear un objeto [Windows::Devices::Sensors::Accelerometer](/uwp/api/Windows.Devices.Sensors.Accelerometer) al inicializar la aplicación.

En el ejemplo siguiente se muestra cómo el método **App:: SetWindow** se registra para los eventos de puntero [Windows:: UI:: Core:: CoreWindow::P ointerpressed](/uwp/api/windows.ui.core.corewindow.PointerPressed), [Windows:: UI:: Core:: CoreWindow::P OINTERRELEASED](/uwp/api/windows.ui.core.corewindow.PointerReleased)y [Windows:: UI:: Core:: CoreWindow::P ointermoved](/uwp/api/windows.ui.core.corewindow.PointerMoved) . Estos eventos se registran durante la inicialización de la aplicación y antes del bucle del juego.

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

La clase **MarbleMazeMain** también crea un objeto **STD:: Map** para contener los eventos Touch. La clave de este objeto map es un valor que identifica de manera exclusiva el puntero de entrada. Cada clave se asigna a la distancia entre cada punto táctil y el centro de la pantalla. Marble Maze después usa estos valores para calcular la cantidad de inclinación del laberinto.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

La clase **MarbleMazeMain** también contiene un objeto [Accelerometer](/uwp/api/Windows.Devices.Sensors.Accelerometer) .

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

El objeto **Accelerometer** se inicializa en el constructor **MarbleMazeMain** , tal y como se muestra en el ejemplo siguiente. El método [Windows::D evices:: sensors:: Accelerometer:: GetDefault](/uwp/api/Windows.Devices.Sensors.Accelerometer.GetDefault) devuelve una instancia del acelerómetro predeterminado. Si no hay ningún acelerómetro predeterminado, **Accelerometer:: GetDefault** devuelve **nullptr**.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>Navegación por los menús

Puede usar el mouse, el toque o el controlador Xbox para navegar por los menús, como se indica a continuación:

-   Usa el control de dirección para cambiar el elemento de menú activo.
-   Use la función táctil, el botón a o el botón de menú para seleccionar un elemento de menú o cerrar el menú actual, como la tabla de puntuación alta.
-   Use el botón de menú para pausar o reanudar el juego.
-   Haz clic en un elemento de menú con el mouse para elegir esa acción.

###  <a name="tracking-xbox-controller-input"></a>Seguimiento de la entrada del controlador Xbox

Para realizar un seguimiento de los controladores de juegos actualmente conectados al dispositivo, **MarbleMazeMain** define una variable miembro, **m_myGamepads**, que es una colección de objetos [Windows:: Gaming:: Input:: controlador de juegos](/uwp/api/windows.gaming.input.gamepad) . Se inicializa en el constructor de la siguiente manera:

```cpp
m_myGamepads = ref new Vector<Gamepad^>();

for (auto gamepad : Gamepad::Gamepads)
{
    m_myGamepads->Append(gamepad);
}
```

Además, el constructor **MarbleMazeMain** registra eventos para cuando se agregan o quitan controladores de juegos:

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

Cuando se agrega un controlador de juegos, se agrega a **m_myGamepads**; Cuando se quita un controlador de juegos, se comprueba si el controlador de juegos está en **m_myGamepads**y, si es así, se quita. En ambos casos, se establece **m_currentGamepadNeedsRefresh** en **true**, lo que indica que es necesario reasignar **m_gamepad**.

Por último, se asigna un controlador de juegos a **m_gamepad** y se establece **m_currentGamepadNeedsRefresh** en **false**:

```cpp
m_gamepad = GetLastGamepad();
m_currentGamepadNeedsRefresh = false;
```

En el método de **actualización** , se comprueba si es necesario reasignar **m_gamepad** :

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

Si es necesario reasignar **m_gamepad** , se le asigna el controlador de juegos agregado más recientemente, con **GetLastGamepad**, que se define de la manera siguiente:

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

Este método simplemente devuelve el último controlador de juegos en **m_myGamepads**.

Puede conectar hasta cuatro controladores de Xbox a un dispositivo de Windows 10. Para evitar tener que averiguar qué controlador es el activo, solo tiene que realizar un seguimiento de los controladores de juegos agregados más recientemente. Si el juego admite más de un jugador, se debe realizar el seguimiento de la entrada de cada jugador por separado.

El método **MarbleMazeMain:: Update** sondea la entrada del controlador para juegos:

```cpp
if (m_gamepad != nullptr)
{
    m_oldReading = m_newReading;
    m_newReading = m_gamepad->GetCurrentReading();
}
```

Realizamos un seguimiento de la lectura de entrada que obtuvimos en el último fotograma con **m_oldReading**, y la última lectura de entrada con **m_newReading**, que obtenemos llamando a un [controlador para juegos:: GetCurrentReading](/uwp/api/windows.gaming.input.gamepad.GetCurrentReading). Esto devuelve un objeto [GamepadReading](/uwp/api/windows.gaming.input.gamepadreading) , que contiene información sobre el estado actual del controlador de juegos.

Para comprobar si un botón se acaba de presionar o soltar, definimos **MarbleMazeMain:: ButtonJustPressed** y **MarbleMazeMain:: ButtonJustReleased**, que comparan las lecturas de los botones desde este fotograma y el último fotograma. De este modo, podemos realizar una acción solo en el momento en que un botón se presiona o se suelta inicialmente, y no cuando se mantiene:

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

Las lecturas de [GamepadButtons](/uwp/api/windows.gaming.input.gamepadbuttons) se comparan mediante las operaciones bit a bit que se &mdash; comprueban si se presiona un botón mediante el *operador and bit a bit* (&). Determinamos si un botón se acaba de presionar o soltar comparando la lectura antigua y la nueva lectura.

Con los métodos anteriores, se comprueba si se han presionado determinados botones y se realizan las acciones correspondientes que se deben producir. Por ejemplo, cuando se presiona el botón de menú (**GamepadButtons:: MENU**), el estado del juego cambia de activo a en pausa o en pausa a activo.

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

También comprobamos si el reproductor presiona el botón ver, en cuyo caso se reiniciará el juego o se borrará la tabla de puntuaciones máximas:

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

Para la entrada táctil y de mouse, se elige un elemento de menú cuando el usuario lo toca o hace clic en él. En el ejemplo siguiente se muestra cómo el método **MarbleMazeMain:: Update** procesa la entrada de puntero para seleccionar los elementos de menú. La variable miembro **m \_ pointQueue** realiza un seguimiento de las ubicaciones en las que el usuario toca o hizo clic en la pantalla. La forma en que Marble Maze recopila la entrada de puntero se describe con más detalle más adelante en este documento en la sección [procesar la entrada de puntero](#processing-pointer-input).

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

Después de que el método **MarbleMazeMain:: Update** procese el controlador y la entrada táctil, actualiza el estado del juego si se presionó cualquier botón.

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


El bucle de juego y el método **MarbleMazeMain:: Update** funcionan juntos para actualizar el estado de los objetos de juego. Si el juego acepta la entrada de varios dispositivos, puedes acumular la entrada desde todos los dispositivos en un conjunto de variables para que puedas escribir código que sea más fácil de mantener. El método **MarbleMazeMain:: Update** define un conjunto de variables que acumula el movimiento de todos los dispositivos.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

El mecanismo de entrada puede variar entre los distintos dispositivos de entrada. Por ejemplo, la entrada del puntero está controlada mediante el modelo de administración de eventos de Windows Runtime. Por el contrario, sondea los datos de entrada desde el controlador Xbox cuando lo necesite. Recomendamos que siempre sigas el mecanismo de entrada indicado para cada dispositivo determinado. En esta sección se describe cómo Marble Maze lee la entrada de cada dispositivo, cómo actualiza los valores de entrada combinados y cómo usa los valores de entrada combinados para actualizar el estado del juego.

###  <a name="processing-pointer-input"></a>Procesamiento de entrada de puntero

Cuando trabajes con la entrada de puntero, llama al método [Windows::UI::Core::CoreDispatcher::ProcessEvents](/uwp/api/windows.ui.core.coredispatcher.processevents) para procesar eventos de ventana. Llama a este método en el bucle del juego antes de actualizar o representar la escena. Marble Maze lo llama en el método **App:: Run** : 

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

Si la ventana está visible, pasamos **CoreProcessEventsOption::P rocessallifpresent** a **ProcessEvents** para procesar todos los eventos en cola y volver inmediatamente. de lo contrario, pasamos **CoreProcessEventsOption::P rocessoneandallpending** para procesar todos los eventos en cola y esperar al siguiente evento nuevo. Una vez que se han procesado los eventos, Marble Maze representa y presenta el siguiente marco.

Windows en tiempo de ejecución llama al controlador registrado para cada evento que ha ocurrido. El método **App:: SetWindow** se registra para los eventos y reenvía la información de puntero a la clase **MarbleMazeMain** .

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

La clase **MarbleMazeMain** reacciona a los eventos de puntero mediante la actualización del objeto de mapa que contiene los eventos de toque. Se llama al método **MarbleMazeMain:: AddTouch** cuando se presiona el puntero por primera vez, por ejemplo, cuando el usuario toca inicialmente la pantalla en un dispositivo habilitado para tocar. Se llama al método **MarbleMazeMain:: UpdateTouch** cuando se mueve la posición del puntero. Se llama al método **MarbleMazeMain:: RemoveTouch** cuando se libera el puntero, por ejemplo, cuando el usuario deja de tocar la pantalla.

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

La función **PointToTouch** traduce la posición actual del puntero para que el origen se encuentra en el centro de la pantalla y, a continuación, escala las coordenadas para que alcancen aproximadamente entre-1,0 y + 1,0. Esto facilita el cálculo de la inclinación del laberinto de forma constante en todos los distintos métodos de entrada.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Size bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

El método **MarbleMazeMain:: Update** actualiza los valores de entrada combinados incrementando el factor de inclinación mediante un valor de escala constante. Este valor de escala se determinó experimentando varios valores distintos.

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

Para procesar la entrada de acelerómetro, el método **MarbleMazeMain:: Update** llama al método [Windows::D Evices:: sensors:: Accelerometer:: GetCurrentReading](/uwp/api/windows.devices.sensors.accelerometer.getcurrentreading) . Este método devuelve un objeto [Windows::Devices::Sensors::AccelerometerReading](/uwp/api/Windows.Devices.Sensors.AccelerometerReading), que representa una lectura del acelerómetro. Las propiedades **Windows::D evices:: sensors:: AccelerometerReading:: AccelerationX** y **Windows::D Evices:: sensors:: AccelerometerReading:: accelerationy** contienen la aceleración g-Force a lo largo de los ejes X e y, respectivamente.

En el ejemplo siguiente se muestra cómo el método **MarbleMazeMain:: Update** sondea el acelerómetro y actualiza los valores de entrada combinados. Al inclinar el dispositivo, la gravedad hace que la canica se mueva más rápido.

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

Como no puede estar seguro de que un acelerómetro está presente en el equipo del usuario, asegúrese siempre de que tiene un objeto de [acelerómetro](/uwp/api/Windows.Devices.Sensors.Accelerometer) válido antes de sondear el acelerómetro.

### <a name="processing-xbox-controller-input"></a>Procesar la entrada del controlador Xbox

En el método **MarbleMazeMain:: Update** , usamos **m_newReading** para procesar la entrada desde el stick analógico izquierdo:

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

Comprobamos si la entrada del stick analógico izquierdo está fuera de la zona muerta y, si es así, la agregamos a **combinedTiltX** y **combinedTiltY** (multiplicado por un factor de escala) para inclinar la fase.

> [!IMPORTANT]
> Cuando trabaje con el controlador Xbox, siempre debe tener en cuenta la zona muerta. La zona muerta se refiere a la varianza entre mandos y sus sensibilidades para el movimiento inicial. En algunos mandos, es posible que un pequeño movimiento no genere ninguna lectura, pero en otros podría generar una lectura apreciable. Para tener esto en cuenta en tu juego, crea una zona sin movimiento para el movimiento inicial del stick analógico. Para obtener más información acerca de la zona muerta, consulte [lectura de thumbsticks](gamepad-and-vibration.md#reading-the-thumbsticks).

 

###  <a name="applying-input-to-the-game-state"></a>Aplicación de la entrada al estado del juego

Los dispositivos notifican los valores de entrada de varias maneras. Por ejemplo, la entrada del puntero podría expresarse en coordenadas de la pantalla y la entrada del mando podría estar en un formato totalmente distinto. Uno de los retos al combinar la entrada de varios dispositivos en un único conjunto de valores de entrada es la normalización, es decir, la conversión de valores a un formato común. Marble Maze normaliza los valores mediante su escalado al intervalo \[ -1,0, 1,0 \] . La función **PointToTouch** , que se describe anteriormente en esta sección, convierte las coordenadas de pantalla en valores normalizados que van aproximadamente entre-1,0 y + 1,0.

> [!TIP]
> Incluso si la aplicación usa un método de entrada, recomendamos que siempre normalice los valores de entrada. Al hacerlo así, puedes simplificar la forma en que otros componentes interpretan la entrada de tu juego, como la simulación de efectos físicos, y se facilita la escritura de juegos que funcionan en varias resoluciones de pantalla.

 

Después de que el método **MarbleMazeMain:: Update** procese la entrada, crea un vector que representa el efecto de la inclinación del laberinto en la canica. En el siguiente ejemplo se muestra cómo Marble Maze usa la función [XMVector3Normalize](/windows/desktop/api/directxmath/nf-directxmath-xmvector3normalize) para crear un vector de gravedad normalizado. La variable **maxTilt** restringe la cantidad por la que el laberinto inclina y evita que el laberinto se incline en su lado.

```cpp
const float maxTilt = 1.0f / 8.0f;

XMVECTOR gravity = XMVectorSet(
    combinedTiltX * maxTilt, 
    combinedTiltY * maxTilt, 
    1.0f, 
    0.0f);

gravity = XMVector3Normalize(gravity);
```

Para completar la actualización de los objetos de la escena, Marble Maze pasa el vector de gravedad actualizado a la simulación de efectos físicos, actualiza la simulación de efectos físicos durante el tiempo transcurrido desde el marco anterior y actualiza la posición y la orientación de la canica. Si la canica ha caído a través del laberinto, el método **MarbleMazeMain:: Update** vuelve a colocar la canica en el último punto de control que la canica toca y restablece el estado de la simulación física.

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

En esta sección no se describe cómo funciona la simulación de efectos físicos. Para obtener más información sobre esto, vea **física. h** y **física. cpp** en los orígenes de Marble Maze.

## <a name="next-steps"></a>Pasos siguientes


Lee [Agregar audio a la muestra de Marble Maze](adding-audio-to-the-marble-maze-sample.md) para obtener información sobre algunas de las prácticas principales que se deben tener en cuenta al trabajar con audio. En el documento se trata la forma en que Marble Maze usa Microsoft Media Foundation y XAudio2 para cargar, mezclar y reproducir recursos de audio.

## <a name="related-topics"></a>Temas relacionados


* [Agregar audio a la muestra de Marble Maze](adding-audio-to-the-marble-maze-sample.md)
* [Agregar contenido visual a la muestra de Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Desarrollo de Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 