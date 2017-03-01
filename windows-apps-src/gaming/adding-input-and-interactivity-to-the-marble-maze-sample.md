---
author: mtoepke
title: "Agregar métodos de entrada e interactividad en la muestra de Marble Maze"
description: "Los juegos de aplicación para Plataforma universal de Windows (UWP) se ejecutan en una variedad de dispositivos, como equipos de escritorio, portátiles y tabletas."
ms.assetid: b946bf62-c0ca-f9ec-1a87-8195b89a5ab4
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, juegos, entrada, ejemplo
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: dc667be326950151b08bbaded6d4e9a0b109523b
ms.lasthandoff: 02/07/2017

---

# <a name="adding-input-and-interactivity-to-the-marble-maze-sample"></a>Agregar métodos de entrada e interactividad al ejemplo de Marble Maze


\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Los juegos de aplicación para Plataforma universal de Windows (UWP) se ejecutan en una variedad de dispositivos, como equipos de escritorio, portátiles y tabletas. Un dispositivo puede tener varios mecanismos de entrada y de control. Al dar soporte a varios dispositivos de entrada, el juego podrá acomodarse a una amplia variedad de preferencias y habilidades entre los clientes. En este documento se describen las prácticas clave que se deben tener en cuenta al trabajar con dispositivos de entrada y se muestra cómo Marble Maze aplica estas prácticas.

> **Nota** El código de ejemplo correspondiente a este documento se encuentra en la [muestra de Marble Maze con DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

 
A continuación se indican algunos de los puntos principales que se tratan en este documento para trabajar con métodos de entrada en el juego:

-   Cuando sea posible, da soporte a varios dispositivos de entrada para que el juego pueda acomodarse a una amplia variedad de preferencias y habilidades entre los clientes. Aunque los dispositivos de juego y el uso del sensor son opcionales, los recomendamos para mejorar la experiencia del jugador. Hemos diseñado la API de dispositivo de juego y sensor para que te resulte más fácil integrar estos dispositivos de entrada.
-   Para inicializar la función táctil, debes registrar los eventos de ventana, por ejemplo, la activación, la liberación o el movimiento del puntero. Para inicializar el acelerómetro, crea un objeto [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687) al inicializar la aplicación. El mando Xbox 360 no requiere inicialización.
-   Para los juegos de un solo jugador, considera combinar métodos de entrada de todos los mandos posibles de Xbox 360. De este modo, no tendrás que hacer un seguimiento de qué entrada procede de cada controlador.
-   Procesa los eventos de Windows antes de procesar los dispositivos de entrada.
-   El mando de la Xbox 360 y el acelerómetro admiten el sondeo. Es decir, puedes hacer un sondeo de datos cuando lo necesites. Para la entrada táctil, registra los eventos táctiles en estructuras de datos que estén disponibles para tu código de procesamiento de entrada.
-   Considera si deseas normalizar los valores de entrada en un formato común. Al hacerlo así, puedes simplificar la forma en que otros componentes interpretan la entrada de tu juego, como la simulación de efectos físicos, y se facilita la escritura de juegos que funcionan en varias resoluciones de pantalla.

## <a name="input-devices-supported-by-marble-maze"></a>Dispositivos de entrada compatibles con Marble Maze


Marble Maze es compatible con dispositivos de controladores comunes de Xbox 360, el mouse y entrada táctil para seleccionar elementos de menú y el mando de la Xbox 360, el mouse, la entrada táctil y el acelerómetro para controlar el juego. Marble Maze usa la API XInput para realizar un sondeo del controlador para la entrada. La entrada táctil permite que las aplicaciones puedan realizar un seguimiento de la entrada de las huellas de los dedos y responder a ella. El acelerómetro es un sensor que mide la fuerza que se aplica a lo largo de los ejes "x", "y" y "z". Al usar Windows Runtime, puedes realizar un sondeo del estado actual del acelerómetro, así como recibir eventos táctiles mediante el mecanismo de administración de eventos de Windows Runtime.

> **Nota** En este documento se usa el término "función táctil" para referirse tanto a las entradas táctiles como de mouse y el término "puntero" para hacer referencia a cualquier dispositivo que use eventos de puntero. Dado que la función táctil y el mouse usan eventos de puntero estándar, puedes usar cualquier dispositivo para seleccionar elementos de menú y controlar el juego.

 

> **Nota** El manifiesto del paquete define que la rotación admitida sea Horizontal para el juego, ya que así se impide que la orientación cambie al girar el dispositivo para hacer rodar la canica.

 

## <a name="initializing-input-devices"></a>Inicialización de dispositivos de entrada


El mando de la Xbox 360 no requiere inicialización. Para inicializar la función táctil, debes registrar los eventos de ventana, tales como la activación, la liberación o el movimiento del puntero (por ejemplo, el usuario presiona el botón del mouse o toca la pantalla). Para inicializar el acelerómetro, tienes que crear un objeto [**Windows::Devices::Sensors::Accelerometer**](https://msdn.microsoft.com/library/windows/apps/br225687) al inicializar la aplicación.

En el siguiente ejemplo se muestra cómo el constructor **DirectXPage** registra los eventos de puntero [**Windows::UI::Core::CoreIndependentInputSource::PointerPressed**](https://msdn.microsoft.com/library/windows/apps/dn298471), [**Windows::UI::Core::CoreIndependentInputSource::PointerReleased**](https://msdn.microsoft.com/library/windows/apps/dn298472) y [**Windows::UI::Core::CoreIndependentInputSource::PointerMoved**](https://msdn.microsoft.com/library/windows/apps/dn298469) para [**SwapChainPanel**](https://msdn.microsoft.com/library/windows/apps/dn252834). Estos eventos se registran durante la inicialización de la aplicación y antes del bucle del juego.

Estos eventos se controlan en un subproceso independiente que invoca a los controladores de eventos.

Para más información acerca de cómo se inicializa la aplicación, consulta [Estructura de la aplicación Marble Maze](marble-maze-application-structure.md).

```cpp
coreInput->PointerPressed += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerPressed);
coreInput->PointerMoved += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerMoved);
coreInput->PointerReleased += ref new TypedEventHandler<Object^, PointerEventArgs^>(this, &DirectXPage::OnPointerReleased);
```

La clase MarbleMaze también crea un objeto std::map para contener los eventos táctiles. La clave de este objeto map es un valor que identifica de manera exclusiva el puntero de entrada. Cada clave se asigna a la distancia entre cada punto táctil y el centro de la pantalla. Marble Maze después usa estos valores para calcular la cantidad de inclinación del laberinto.

```cpp
typedef std::map<int, XMFLOAT2> TouchMap;
TouchMap        m_touches;
```

La clase MarbleMaze contiene un objeto Accelerometer.

```cpp
Windows::Devices::Sensors::Accelerometer^           m_accelerometer;
```

El objeto Accelerometer se inicializa en el método MarbleMaze::Initialize, tal como se muestra en el siguiente ejemplo. El método Windows::Devices::Sensors::Accelerometer::GetDefault devuelve una instancia del acelerómetro predeterminado. Si no hay un acelerómetro predeterminado (Accelerometer::GetDefault), el valor de m\_accelerometer seguirá siendo nullptr.

```cpp
// Returns accelerometer ref if there is one; nullptr otherwise.
m_accelerometer = Windows::Devices::Sensors::Accelerometer::GetDefault();
```

##  <a name="navigating-the-menus"></a>Navegación por los menús


###  <a name="tracking-xbox-360-controller-input"></a>Seguimiento de la entrada del mando Xbox 360

Puedes usar el mouse, la función táctil o el mando de Xbox 360 para navegar por los menús, como se indica a continuación:

-   Usa el control de dirección para cambiar el elemento de menú activo.
-   Usa la entrada táctil, el botón A, o el botón Start para seleccionar un elemento de menú o cerrar el menú actual, como por ejemplo la tabla de máximas puntuaciones.
-   Usa el botón Start para pausar o reanudar el juego.
-   Haz clic en un elemento de menú con el mouse para elegir esa acción.

###  <a name="tracking-touch-and-mouse-input"></a>Seguimiento de la entrada táctil y de mouse

Para hacer el seguimiento de la entrada del mando Xbox 360, el método **MarbleMaze::Update** define una matriz de botones que definen los comportamientos de entrada. XInput solo proporciona el estado actual del mando. Por lo tanto, **MarbleMaze::Update** también define dos matrices que realizan seguimiento, para cada mando Xbox 360 posible, si cada botón se presionó durante el fotograma anterior y si cada botón está actualmente presionado.

```cpp
// This array contains the constants from XINPUT that map to the 
// particular buttons that are used by the game. 
// The index of the array is used to associate the state of that button in 
// the wasButtonDown, isButtonDown, and combinedButtonPressed variables. 

static const WORD buttons[] = {
    XINPUT_GAMEPAD_A,
    XINPUT_GAMEPAD_START,
    XINPUT_GAMEPAD_DPAD_UP,
    XINPUT_GAMEPAD_DPAD_DOWN,
    XINPUT_GAMEPAD_DPAD_LEFT,
    XINPUT_GAMEPAD_DPAD_RIGHT,
    XINPUT_GAMEPAD_BACK,
};

static const int buttonCount = ARRAYSIZE(buttons);
static bool wasButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
bool isButtonDown[XUSER_MAX_COUNT][buttonCount] = { false, };
```

Puedes conectar hasta cuatro mandos de Xbox 360 en un dispositivo de Windows. Para evitar tener que identificar cuál es el mando activo, el método **MarbleMaze::Update** combina métodos de entrada en todos los mandos.

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
```

Si el juego admite más de un jugador, se debe realizar el seguimiento de la entrada de cada jugador por separado.

En un bucle, el método **MarbleMaze::Update** hace un sondeo de cada mando para detectar la entrada y lee el estado de cada botón.

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

Una vez que el método **MarbleMaze::Update** sondea la entrada, actualiza la matriz de entradas combinadas. La matriz de entradas combinadas realiza un seguimiento solo de los botones que están ahora presionados pero que no estaban presionados previamente. Esto permite que el juego realice una acción solo en el momento en que se presiona el botón inicialmente y no cuando el botón se mantiene presionado.

```cpp
bool combinedButtonPressed[buttonCount] = { false, };
for (int i = 0; i < buttonCount; ++i)
{
    for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
    {
        bool pressed = !wasButtonDown[userIndex][i] && isButtonDown[userIndex][i];
        combinedButtonPressed[i] = combinedButtonPressed[i] || pressed;
    }
}
```

Una vez que el método **MarbleMaze::Update** recopila la entrada de botones, realiza todas las acciones que se deben producir. Por ejemplo, al presionar el botón Inicio (XINPUT\_GAMEPAD\_START), el estado del juego cambia de activo a en pausa o de en pausa a activo.

```cpp
// Check whether the user paused or resumed the game. 
// XINPUT_GAMEPAD_START  
if (combinedButtonPressed[1] || m_pauseKeyPressed)
{
    m_pauseKeyPressed = false;
    if (m_gameState == GameState::InGameActive)
        SetGameState(GameState::InGamePaused);
    else if (m_gameState == GameState::InGamePaused)
        SetGameState(GameState::InGameActive);
}
```

Si el menú principal está activo, el elemento de menú activo cambia cuando se presiona el control de dirección hacia arriba o hacia abajo. Si el usuario elige la selección actual, el elemento de interfaz de usuario se marca como elegido.

```cpp
// Handle menu navigation. 

// XINPUT_GAMEPAD_A or XINPUT_GAMEPAD_START 
bool chooseSelection = (combinedButtonPressed[0] || combinedButtonPressed[1]);

// XINPUT_GAMEPAD_DPAD_UP 
bool moveUp = combinedButtonPressed[2];

// XINPUT_GAMEPAD_DPAD_DOWN 
bool moveDown = combinedButtonPressed[3];                                           

switch (m_gameState)
{
case GameState::MainMenu:
    if (chooseSelection)
    {
        m_audio.PlaySoundEffect(MenuSelectedEvent);

        if (m_startGameButton.GetSelected())
            m_startGameButton.SetPressed(true);

        if (m_highScoreButton.GetSelected())
            m_highScoreButton.SetPressed(true);
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
        SetGameState(GameState::MainMenu);
    break;

case GameState::PostGameResults:
    if (chooseSelection || anyPoints)
        SetGameState(GameState::HighScoreDisplay);
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

Una vez que el método **MarbleMaze::Update** procesa la entrada del mando, guarda el estado de entrada actual para el próximo fotograma.

```cpp
// Update the button state for the next frame.
memcpy(wasButtonDown, isButtonDown, sizeof(wasButtonDown));
```

### <a name="tracking-touch-and-mouse-input"></a>Seguimiento de la entrada táctil y de mouse

Para la entrada táctil y de mouse, se elige un elemento de menú cuando el usuario lo toca o hace clic en él. En el siguiente ejemplo se muestra cómo el método **MarbleMaze::Update** procesa la entrada del puntero para seleccionar elementos de menú. La variable de miembro **m\_pointQueue** realiza un seguimiento de las ubicaciones en las que el usuario ha pulsado o ha hecho clic en la pantalla. La forma en que Marble Maze recopila la entrada del puntero se describe con más detalle más adelante en este documento en la sección Procesamiento de entrada de puntero.

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

Una vez que el método **MarbleMaze::Update** procesa la entrada táctil y del mando, actualiza el estado del juego si se presionó cualquier botón.

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


El bucle del juego y el método **MarbleMaze::Update** funcionan juntos para actualizar el estado de los objetos del juego. Si el juego acepta la entrada de varios dispositivos, puedes acumular la entrada desde todos los dispositivos en un conjunto de variables para que puedas escribir código que sea más fácil de mantener. El método **MarbleMaze::Update** define un conjunto de variables que acumula el movimiento de todos los dispositivos.

```cpp
float combinedTiltX = 0.0f;
float combinedTiltY = 0.0f;
```

El mecanismo de entrada puede variar entre los distintos dispositivos de entrada. Por ejemplo, la entrada del puntero está controlada mediante el modelo de administración de eventos de Windows Runtime. Por otra parte, se hace un sondeo de los datos de entrada desde el mando de la Xbox 360 cuando se necesite. Recomendamos que siempre sigas el mecanismo de entrada indicado para cada dispositivo determinado. En esta sección se describe cómo Marble Maze lee la entrada de cada dispositivo, cómo actualiza los valores de entrada combinados y cómo usa los valores de entrada combinados para actualizar el estado del juego.

###  <a name="processing-pointer-input"></a>Procesamiento de entrada de puntero

Cuando trabajes con la entrada de puntero, llama al método [**Windows::UI::Core::CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208217) para procesar eventos de ventana. Llama a este método en el bucle del juego antes de actualizar o representar la escena. Marble Maze pasa **CoreProcessEventsOption::ProcessAllIfPresent** a este método para procesar todos los eventos en cola y después regresa inmediatamente. Una vez que se han procesado los eventos, Marble Maze representa y presenta el siguiente marco.

```cpp
// Process windowing events.
CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
```

Windows en tiempo de ejecución llama al controlador registrado para cada evento que ha ocurrido. La clase **DirectXApp** registra eventos y reenvía la información del puntero a la clase **MarbleMaze**.

```cpp
void DirectXApp::OnPointerPressed(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->AddTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}

void DirectXApp::OnPointerReleased(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->RemoveTouch(args->CurrentPoint->PointerId);
}

void DirectXApp::OnPointerMoved(
    _In_ Windows::UI::Core::CoreWindow^ sender,
    _In_ Windows::UI::Core::PointerEventArgs^ args
    )
{
    m_renderer->UpdateTouch(args->CurrentPoint->PointerId, args->CurrentPoint->Position);
}
```

La clase **MarbleMaze** reacciona ante los eventos de puntero mediante la actualización del objeto map que contiene los eventos de entrada táctil. Se llama al método **MarbleMaze::AddTouch** cuando se presiona el puntero por primera vez, por ejemplo, si el usuario toca inicialmente la pantalla en un dispositivo con funcionalidad táctil. Se llama al método **MarbleMaze::AddTouch** cuando se mueve la posición del puntero. Se llama al método **MarbleMaze::RemoveTouch** cuando se libera el puntero, por ejemplo, si el usuario deja de tocar la pantalla.

```cpp
void MarbleMaze::AddTouch(int id, Windows::Foundation::Point point)
{
    m_touches[id] = PointToTouch(point, m_windowBounds);

    m_pointQueue.push(D2D1::Point2F(point.X, point.Y));
}

void MarbleMaze::UpdateTouch(int id, Windows::Foundation::Point point)
{
    if (m_touches.find(id) != m_touches.end())
        m_touches[id] = PointToTouch(point, m_windowBounds);
}

void MarbleMaze::RemoveTouch(int id)
{
    m_touches.erase(id);
}
```

La función PointToTouch traslada la posición actual del puntero de modo que el origen esté en el centro de la pantalla y después escala las coordenadas para que estén en un rango aproximado entre -1,0 y +1,0. Esto facilita el cálculo de la inclinación del laberinto de forma constante en todos los distintos métodos de entrada.

```cpp
inline XMFLOAT2 PointToTouch(Windows::Foundation::Point point, Windows::Foundation::Rect bounds)
{
    float touchRadius = min(bounds.Width, bounds.Height);
    float dx = (point.X - (bounds.Width / 2.0f)) / touchRadius;
    float dy = ((bounds.Height / 2.0f) - point.Y) / touchRadius;

    return XMFLOAT2(dx, dy);
}
```

El método **MarbleMaze::Update** actualiza los valores de entrada combinada incrementando el factor de inclinación en un valor de escala constante. Este valor de escala se determinó experimentando varios valores distintos.

```cpp
// Account for touch input. 
const float touchScalingFactor = 2.0f;
for (TouchMap::const_iterator iter = m_touches.cbegin(); iter != m_touches.cend(); ++iter)
{
    combinedTiltX += iter->second.x * touchScalingFactor;
    combinedTiltY += iter->second.y * touchScalingFactor;
}
```

### <a name="processing-accelerometer-input"></a>Procesamiento de la entrada del acelerómetro

Para procesar la entrada del acelerómetro, el método **MarbleMaze::Update** llama al método [**Windows::Devices::Sensors::Accelerometer::GetCurrentReading**](https://msdn.microsoft.com/library/windows/apps/br225699). Este método devuelve un objeto [**Windows::Devices::Sensors::AccelerometerReading**](https://msdn.microsoft.com/library/windows/apps/br225688), que representa una lectura del acelerómetro. Las propiedades **Windows::Devices::Sensors::AccelerometerReading::AccelerationX** y **Windows::Devices::Sensors::AccelerometerReading::AccelerationY** contienen la aceleración de fuerza G a lo largo de los ejes X e Y, respectivamente.

En el siguiente ejemplo se muestra cómo el método **MarbleMaze::Update** sondea el acelerómetro y actualiza los valores de entrada combinada. Al inclinar el dispositivo, la gravedad hace que la canica se mueva más rápido.

```cpp
// Account for sensors. 
const float acceleromterScalingFactor = 3.5f;
if (m_accelerometer != nullptr)
{
    Windows::Devices::Sensors::AccelerometerReading^ reading =
        m_accelerometer->GetCurrentReading();

    if (reading != nullptr)
    {
        combinedTiltX += static_cast<float>(reading->AccelerationX) * acceleromterScalingFactor;
        combinedTiltY += static_cast<float>(reading->AccelerationY) * acceleromterScalingFactor;
    }
}
```

Como no se puede estar seguro de que el equipo del usuario tenga un acelerómetro, asegúrate de tener siempre un objeto Accelerometer válido antes de sondear el acelerómetro.

### <a name="processing-xbox-360-controller-input"></a>Procesamiento de la entrada del mando de la Xbox 360

En el siguiente ejemplo se muestra cómo el método **MarbleMaze::Update** lee el mando Xbox 360 y actualiza los valores de entrada combinada. El método **MarbleMaze::Update** usa un bucle para permitir que la entrada se reciba desde cualquier mando conectado. El método **XInputGetState** rellena un objeto XINPUT\_STATE con el estado actual del mando. Los valores **combinedTiltX** y **combinedTiltY** se actualizan según los valores de X e Y del stick izquierdo.

```cpp
// Account for input on any connected controller.
XINPUT_STATE inputState = {0};
for (DWORD userIndex = 0; userIndex < XUSER_MAX_COUNT; ++userIndex)
{
    DWORD result = XInputGetState(userIndex, &inputState);
    if(result != ERROR_SUCCESS) 
        continue;

    SHORT thumbLeftX = inputState.Gamepad.sThumbLX;
    if (abs(thumbLeftX) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftX = 0;

    SHORT thumbLeftY = inputState.Gamepad.sThumbLY;
    if (abs(thumbLeftY) < XINPUT_GAMEPAD_LEFT_THUMB_DEADZONE) 
        thumbLeftY = 0;

    combinedTiltX += static_cast<float>(thumbLeftX) / 32768.0f;
    combinedTiltY += static_cast<float>(thumbLeftY) / 32768.0f;

    for (int i = 0; i < buttonCount; ++i)
        isButtonDown[userIndex][i] = (inputState.Gamepad.wButtons & buttons[i]) == buttons[i];
}
```

XInput define la constante **XINPUT\_GAMEPAD\_LEFT\_THUMB\_DEADZONE** del stick izquierdo. Esto es un umbral de zona muerta adecuado para la mayoría de los juegos.

> **Importante** Cuando trabajes con el mando de Xbox 360, ten siempre en cuenta la zona muerta. La zona muerta se refiere a la varianza entre mandos y sus sensibilidades para el movimiento inicial. En algunos mandos, es posible que un pequeño movimiento no genere ninguna lectura, pero en otros podría generar una lectura apreciable. Para tener esto en cuenta en tu juego, crea una zona sin movimiento para el movimiento inicial del stick analógico. Para más información acerca de la zona muerta, consulta [Introducción a XInput](https://msdn.microsoft.com/library/windows/desktop/ee417001).

 

###  <a name="applying-input-to-the-game-state"></a>Aplicación de la entrada al estado del juego

Los dispositivos notifican los valores de entrada de varias maneras. Por ejemplo, la entrada del puntero podría expresarse en coordenadas de la pantalla y la entrada del mando podría estar en un formato totalmente distinto. Uno de los retos al combinar la entrada de varios dispositivos en un único conjunto de valores de entrada es la normalización, es decir, la conversión de valores a un formato común. Marble Maze normaliza los valores escalándolos en el intervalo \[-1.0, 1.0\]. Para normalizar la entrada del mando Xbox 360, Marble Maze divide los valores de entrada por 32 768 porque los valores de entrada del stick siempre están entre -32 768 y 32 767. La función **PointToTouch**, que ya se ha descrito en esta sección, logra un resultado similar al convertir las coordenadas de pantalla en valores normalizados que están en un intervalo aproximado entre -1,0 y +1,0.

> **Sugerencia** Aunque la aplicación use un único método de entrada, te recomendamos normalizar siempre los valores de entrada. Al hacerlo así, puedes simplificar la forma en que otros componentes interpretan la entrada de tu juego, como la simulación de efectos físicos, y se facilita la escritura de juegos que funcionan en varias resoluciones de pantalla.

 

Una vez que el método **MarbleMaze::Update** procesa la entrada, crea un vector que representa el efecto de la inclinación del laberinto en la canica. En el siguiente ejemplo se muestra cómo Marble Maze usa la función **XMVector3Normalize** para crear un vector de gravedad normalizado. La variable *MaxTilt* limita la cantidad de inclinación del laberinto e impide que este se incline de lado.

```cpp
const float maxTilt = 1.0f / 8.0f;
XMVECTOR gravity = XMVectorSet(combinedTiltX * maxTilt, combinedTiltY * maxTilt, 1.0f, 0.0f);
gravity = XMVector3Normalize(gravity);
```

Para completar la actualización de los objetos de la escena, Marble Maze pasa el vector de gravedad actualizado a la simulación de efectos físicos, actualiza la simulación de efectos físicos durante el tiempo transcurrido desde el marco anterior y actualiza la posición y la orientación de la canica. Si la canica se sale del laberinto, el método **MarbleMaze::Update** vuelve a colocarla en el último punto de control que tocó la canica y restablece el estado de la simulación de efectos físicos.

```cpp
XMFLOAT3 g;
XMStoreFloat3(&g, gravity);
m_physics.SetGravity(g);
```

```cpp
// Only update physics when gameplay is active.
m_physics.UpdatePhysicsSimulation(timeDelta);
```

```cpp
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

En esta sección no se describe cómo funciona la simulación de efectos físicos. Para más información sobre esto, consulta Physics.h y Physics.cpp en los orígenes de Marble Maze.

## <a name="next-steps"></a>Pasos siguientes


Lee [Agregar audio a la muestra de Marble Maze](adding-audio-to-the-marble-maze-sample.md) para obtener información sobre algunas de las prácticas principales que se deben tener en cuenta al trabajar con audio. En el documento se trata la forma en que Marble Maze usa Microsoft Media Foundation y XAudio2 para cargar, mezclar y reproducir recursos de audio.

## <a name="related-topics"></a>Temas relacionados


* [Agregar audio a la muestra de Marble Maze](adding-audio-to-the-marble-maze-sample.md)
* [Agregar contenido visual a la muestra de Marble Maze](adding-visual-content-to-the-marble-maze-sample.md)
* [Desarrollar Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 





