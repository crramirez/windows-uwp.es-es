---
title: Crear un juego para UWP en MonoGame 2D
description: Un sencillo juego para UWP para Microsoft Store, escrito en C# y MonoGame
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5d5f7af2-41a9-4749-ad16-4503c64bb80c
ms.localizationpriority: medium
ms.openlocfilehash: 4300bdc0224a18874a7ff9153195f81f8bb8d101
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685048"
---
# <a name="create-a-uwp-game-in-monogame-2d"></a>Crear un juego para UWP en MonoGame 2D

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-c-and-monogame"></a>Un sencillo juego en 2D para UWP para Microsoft Store, escrito en C# y MonoGame


![Hoja sprite de un dinosaurio caminando](images/JS2D_0.png)

## <a name="introduction"></a>Introducción

MonoGame es un marco de desarrollo de juegos ligero. En este tutorial se le enseñará lo esencial del desarrollo de juegos en MonoGame, incluido cómo descargar contenido, dibujar sprites, animarlos, así como a gestionar las entradas de los usuarios. También se tratarán otros conceptos más avanzados, como la detección de colisiones y el escalado vertical para pantallas con valores altos de PPP. Este tutorial dura entre 30 y 60 minutos.

## <a name="prerequisites"></a>Requisitos previos
+   Windows 10 y Microsoft Visual Studio 2019.  [Haz clic aquí para obtener información sobre cómo prepararte para usar Visual Studio](https://docs.microsoft.com/windows/uwp/get-started/get-set-up).
+ El marco de desarrollo de escritorio .NET. Si aún no lo tiene instalado, puede obtenerlo volviendo a ejecutar el instalador de Visual Studio y modificando la instalación de Visual Studio 2019.
+   Conocimientos básicos de C# o algún lenguaje similar de programación orientado a objetos. [Haga clic aquí para obtener información acerca de cómo empezar a trabajar con C#](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).
+   Familiaridad con conceptos informáticos básicos como clases, métodos y variables es una ventaja.

## <a name="why-monogame"></a>¿Por qué MonoGame?
Hay diversas opciones cuando se trata de entornos de desarrollo de juegos. Desde motores completos como Unity a API de multimedia exhaustivas y complejas, como DirectX, puede ser difícil saber por dónde comenzar. MonoGame es un conjunto de herramientas, con un nivel de complejidad entre un motor de juego y una API resolutiva como DirectX. Proporciona una canalización de contenido fácil de usar y toda la funcionalidad necesaria para crear juegos ligeros que puedan ejecutarse en diferentes plataformas. Lo mejor de todo es que las aplicaciones de MonoGame están escritas en C# y podrá distribuirlas rápidamente a través de la Microsoft Store u otras plataformas similares de distribución.

## <a name="get-the-code"></a>Obtener el código
Si no le apetece seguir este tutorial paso a paso y solo quiere ver MonoGame en acción, [haga clic aquí para obtener la aplicación acabada](https://github.com/Microsoft/Windows-appsample-get-started-mg2d).

Abra el proyecto en Visual Studio 2019 y presione **F5** para ejecutar la muestra. La primera vez que haga esto, puede que tarde un poco, ya que Visual Studio debe capturar todos los paquetes de NuGet que faltan de la instalación.

Si ya lo hizo, omita la sección siguiente acerca de cómo configurar MonoGame para ver un tutorial paso a paso del código.

**Nota:** El juego creado en este ejemplo no tiene por qué estar completo (ni ser divertido). Su único propósito es demostrar todos los conceptos básicos del desarrollo en 2D en MonoGame. Siéntase libre para usar este código y hacer algo mucho mejor, o bien para empezar desde cero una vez que domine los conceptos básicos.

## <a name="set-up-monogame-project"></a>Configurar un proyecto de MonoGame
1. Instale **MonoGame 3.6** para Visual Studio desde [MonoGame.net](https://www.monogame.net/)

2. Inicie Visual Studio 2019.

3. Vaya a **Archivo -> Nuevo -> Proyecto**

4. En las plantillas de proyecto de Visual C#, seleccione **MonoGame** y **MonoGame Windows 10 Universal Project**

5. Asigne un nombre al proyecto “MonoGame2D" y seleccione Aceptar. Una vez creado el proyecto, probablemente parezca que tiene muchos errores, estos deberían desaparecer una vez que ejecute el proyecto por primera vez y se instalen todos los paquetes de NuGet que faltan.

6. Asegúrese de que **x86** y **Equipo local** están configurados como la plataforma de destino, y pulsa **F5** para compilar y ejecutar el proyecto vacío. Si siguió los pasos que se indican arriba, debería ver una ventana azul vacía una vez que el proyecto se ha compilado.

## <a name="method-overview"></a>Introducción del método
Una vez creado el proyecto, abra el archivo **Game1.cs** desde el **Explorador de soluciones**. Aquí es donde sucede la mayor parte de la lógica del juego. Muchos de los métodos fundamentales se generan automáticamente aquí cuando crea un nuevo proyecto de MonoGame. Vamos a revisarlos rápidamente:

**public Game1()** The constructor. No modificaremos este método en este tutorial.

**protected override void Initialize()** Aquí se inicializan las variables de clase que se usan. Este método se llama una vez al iniciar el juego

**protected override void LoadContent()** Este método carga contenido (p. ex. texturas, audio, fuentes) en la memoria antes de que comience el juego. Como sucede con Inicializar, se llama una vez cuando se inicia la aplicación.

**protected override void UnloadContent()** Este método se usa para descargar contenido que no es del Administrador de contenido. No utilizamos este método.

**protected override void Update(GameTime gameTIme)** Este método se llama una vez durante cada ciclo del bucle del juego. Aquí se actualizan los estados de cualquier objeto o variable que se usa en el juego. Incluye cosas como la posición, la velocidad o el color de un objeto. Aquí también se controla la entrada del usuario. En resumen, este método controla cada parte de la lógica del juego, excepto el dibujo de objetos en la pantalla.

**protected override void Draw(GameTime gameTime)** Aquí es donde se dibujan los objetos en la pantalla, mediante el uso de las posiciones que proporciona el método Update.

## <a name="draw-a-sprite"></a>Dibujar un sprite
Una vez ejecutado el nuevo proyecto de MonoGame y después de ver el hermoso cielo azul, agreguemos un poco de tierra.
En MonoGame, el arte en 2D se agrega a la aplicación en el formulario de “sprites”. Un sprite es simplemente un gráfico para PC que se manipula como una única entidad. Los sprites pueden moverse, escalarse, modificarse, animarse y combinarse para crear cualquier cosa que pueda imaginar en el espacio 2D.

### <a name="1-download-a-texture"></a>1. Descargar una textura
En este caso, el primer sprite va a ser muy aburrido. [Haga clic aquí para descargar este rectángulo verde uniforme](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/grass.png).

### <a name="2-add-the-texture-to-the-content-folder"></a>2. Agregar textura a la carpeta Contenido
- Abra el **Explorador de soluciones**
- Haga clic con el botón derecho en **Content.mgcb** en la carpeta **Contenido** y seleccione **Abrir con**. En el menú emergente seleccione **Canalización de Monogame** y después **Aceptar**.
- En la nueva ventana, haga clic con el botón derecho en el elemento **Contenido** y seleccione **Agregar -> Elemento existente**.
- Localice y seleccione el rectángulo verde en el explorador de archivos.
- Asígnele un nombre al elemento “grass.png” y seleccione **Agregar**.

### <a name="3-add-class-variables"></a>3. Agregar variables de clase
Para cargar esta imagen como una textura de sprite, abra **Game1.cs** y agregue las siguientes variables de clase.

```CSharp
const float SKYRATIO = 2f/3f;
float screenWidth;
float screenHeight;
Texture2D grass;
```

La variable SKYRATIO nos indica qué cantidad del escenario queremos que sea cielo y qué cantidad queremos que sea césped; en este caso, dos tercios. **screenWidth** y **screenHeight** llevarán un seguimiento del tamaño de la ventana de la aplicación, mientras que **grass** es dónde almacenaremos nuestro rectángulo verde.

### <a name="4-initialize-class-variables-and-set-window-size"></a>4. Inicializar variables de clase y configurar el tamaño de la ventana
Las variables **screenWidth** y **screenHeight** deberán inicializarse, así que debe agregar este código al método **Inicializar**:

```CSharp
ApplicationView.PreferredLaunchWindowingMode = ApplicationViewWindowingMode.FullScreen;

screenHeight = (float)ApplicationView.GetForCurrentView().VisibleBounds.Height;
screenWidth = (float)ApplicationView.GetForCurrentView().VisibleBounds.Width;

this.IsMouseVisible = false;
```

Al mismo tiempo que se obtiene el ancho y el alto de la pantalla, también configuramos el modo basado en ventanas de la aplicación a **Pantalla completa** y configuramos el mouse para que sea invisible.

### <a name="5-load-the-texture"></a>5. Cargar la textura
Para cargar la textura en la variable de césped, agregue lo siguiente al método **LoadContent**:

```CSharp
grass = Content.Load<Texture2D>("grass");
```

### <a name="6-draw-the-sprite"></a>6. Dibujar el sprite
Para dibujar el rectángulo, agregue las líneas siguientes al método **Draw**:

```CSharp
GraphicsDevice.Clear(Color.CornflowerBlue);
spriteBatch.Begin();
spriteBatch.Draw(grass, new Rectangle(0, (int)(screenHeight * SKYRATIO),
  (int)screenWidth, (int)screenHeight), Color.White);
spriteBatch.End();
```

Aquí se usa el método **spriteBatch.Draw** para colocar la textura determinada en los bordes de un objeto Rectangle. Un **Rectangle** por las coordenadas x e y de su esquina superior izquierda y de su esquina inferior derecha. Con las variables **screenWidth**, **screenHeight** y **SKYRATIO** que definimos antes, dibujamos la textura del rectángulo verde en el tercio inferior de la pantalla. Si ejecuta el programa ahora, verá que el fondo que antes era azul ahora está parcialmente cubierto por el rectángulo verde.

![Rectángulo verde](images/monogame-tutorial-1.png)

## <a name="scale-to-high-dpi-screens"></a>Escalar a pantallas con valores altos de PPP
Si está ejecutando Visual Studio en un monitor con una densidad de píxeles alta, como los que se encuentran en Surface Pro o Surface Studio, puede que vea que el rectángulo verde de los pasos anteriores no cubre completamente el tercio inferior de la pantalla. Es probable que flote sobre la esquina inferior izquierda de la pantalla. Para corregir esto y unificar la experiencia de nuestro juego en todos los dispositivos, deberemos crear un método que escale determinados valores relativos a la densidad de los píxeles de la pantalla.

```CSharp
public float ScaleToHighDPI(float f)
{
  DisplayInformation d = DisplayInformation.GetForCurrentView();
  f *= (float)d.RawPixelsPerViewPixel;
  return f;
}
```

A continuación sustituya las inicializaciones **screenHeight** y **screenWidth** en el método **Inicializar** con esto:

```CSharp
screenHeight = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Height);
screenWidth = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Width);
```

Si estás usando una pantalla con valores altos de PPP e intentas ejecutar la aplicación ahora, deberías ver el rectángulo verde que cubre el tercio inferior de la pantalla según lo previsto.

## <a name="build-the-spriteclass"></a>Compilar el SpriteClass
Antes de empezar a animar sprites, vamos a crear una nueva clase denominada “SpriteClass,” que nos permitirá reducir la complejidad del nivel de superficie de la manipulación de sprite.

### <a name="1-create-a-new-class"></a>1. Crear una clase nueva
En el **Explorador de soluciones**, haga clic con el botón derecho en **MonoGame2D (Universal Windows)** y seleccione **Agregar -> Clase**. Asigne un nombre a la clase “SpriteClass.cs” y a continuación seleccione **Agregar**.

### <a name="2-add-class-variables"></a>2. Agregar variables de clase
Agregue este código a la clase que acaba de crear:

```CSharp
public Texture2D texture
{
  get;
}

public float x
{
  get;
  set;
}

public float y
{
  get;
  set;
}

public float angle
{
  get;
  set;
}

public float dX
{
  get;
  set;
}

public float dY
{
  get;
  set;
}

public float dA
{
  get;
  set;
}

public float scale
{
  get;
  set;
}
```

Aquí configuramos las variables de clase que necesitamos dibujar y animamos un sprite. Las variables **x** e **y** representan la posición actual del sprite en el plano, mientras que la variable **angle** es el ángulo actual del sprite en grados (siendo 0 la posición vertical, 90 inclinado 90 grados en el sentido de las agujas del reloj). Es importante tener en cuenta que, para esta clase, **x** e **y** representan las coordenadas del **centro** del sprite (el origen predeterminado es la esquina superior izquierda). Esto hace que rotar los sprites sea más fácil, ya que rotarán alrededor de cualquier origen que se indique, además de que rotar alrededor del centro nos da un giro uniforme.

Una vez hecho esto, tenemos **dX**, **dY**, y **dA**, que son las tarifas por segundo de cambio para el **x**, **y**, y **angle**, respectivamente.

### <a name="3-create-a-constructor"></a>3. Crear un constructor
Al crear una instancia de **SpriteClass**, proporcionamos el constructor con el dispositivo de gráficas de **Game1.cs**, la ruta de acceso a la textura relativa a la carpeta del proyecto y la escala deseada de la textura relativa a su tamaño original. Configuraremos el resto de las variables de clase después del juego, en el método de actualización.

```CSharp
public SpriteClass (GraphicsDevice graphicsDevice, string textureName, float scale)
{
  this.scale = scale;
  if (texture == null)
  {
    using (var stream = TitleContainer.OpenStream(textureName))
    {
      texture = Texture2D.FromStream(graphicsDevice, stream);
    }
  }
}
```

### <a name="4-update-and-draw"></a>4. Actualizar y dibujar
Todavía hay algunos métodos que debemos agregar a la declaración SpriteClass:

```CSharp
public void Update (float elapsedTime)
{
  this.x += this.dX * elapsedTime;
  this.y += this.dY * elapsedTime;
  this.angle += this.dA * elapsedTime;
}

public void Draw (SpriteBatch spriteBatch)
{
  Vector2 spritePosition = new Vector2(this.x, this.y);
  spriteBatch.Draw(texture, spritePosition, null, Color.White, this.angle, new Vector2(texture.Width/2, texture.Height/2), new Vector2(scale, scale), SpriteEffects.None, 0f);
}
```

El método **Update** SpriteClass se llama en el método **Update** de Game1.cs, y se usa para actualizar los valores **x**, **y** y **angle** de los sprites según sus respectivas velocidades de cambio.

El método **Draw** se llama en el método **Draw** de Game1.cs, y se usa para dibujar el sprite en la ventana del juego.

## <a name="user-input-and-animation"></a>Entrada de usuario y animación
Ahora que ya está compilada la clase SpriteClass, la usaremos para crear dos nuevos objetos de juego. El primero es una avatar que el jugador puede controlar con las teclas de dirección y la barra espaciadora. El segundo es un objeto que el jugador debe evitar.

### <a name="1-get-the-textures"></a>1. Obtener las texturas
Para el avatar del jugador, vamos a usar el gato ninja de Microsoft a hombros de su fiel Tyrannosaurus rex. [Haga clic aquí para descargar la imagen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/ninja-cat-dino.png).

Ahora, en cuanto al obstáculo que el jugador debe esquivar. ¿Qué es lo que más odian los gatos ninja y los dinosaurios carnívoros? ¡Comer verduras! [Haga clic aquí para descargar la imagen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/broccoli.png).

Tal y como hicimos antes con el rectángulo verde, agregue estas imágenes a **Content.mgcb** a través de **Canalización de MonoGame**, y asígneles los nombres “ninja-cat-dino.png” y “broccoli.png”, respectivamente.

### <a name="2-add-class-variables"></a>2. Agregar variables de clase
Agregue el código siguiente a la lista de variables de clase en **Game1.cs**:

```CSharp
SpriteClass dino;
SpriteClass broccoli;

bool spaceDown;
bool gameStarted;

float broccoliSpeedMultiplier;
float gravitySpeed;
float dinoSpeedX;
float dinoJumpY;
float score;

Random random;
```

**dino** y **broccoli** son nuestras variables de SpriteClass. **dino** tiene el avatar del jugador, mientras que **broccoli** el brécol que funciona como obstáculo.

**spaceDown** realiza un seguimiento de si la barra espaciadora se mantiene presionada en lugar de presionarla y soltarla.

**gameStarted** nos indica si el usuario comenzó el juego por primera vez.

**broccoliSpeedMultiplier** determina la velocidad con la que se mueve el brécol que funciona como obstáculo a través de la pantalla.

**gravitySpeed** determina la velocidad con la que acelera el avatar del jugador al descender después de saltar.

**dinoSpeedX** y **dinoJumpY** determinan la velocidad a la que el avatar del jugador se mueve y salta.
La puntuación realiza un seguimiento de cuántos obstáculos esquivó el jugador con éxito.

Por último, **random** se usará para agregar algún comportamiento aleatorio al brécol que funciona como obstáculo.

### <a name="3-initialize-variables"></a>3. Inicializar variables
A continuación debemos inicializar estas variables. Agregue el código siguiente al método de Inicializar:

```CSharp
broccoliSpeedMultiplier = 0.5f;
spaceDown = false;
gameStarted = false;
score = 0;
random = new Random();
dinoSpeedX = ScaleToHighDPI(1000f);
dinoJumpY = ScaleToHighDPI(-1200f);
gravitySpeed = ScaleToHighDPI(30f);
```

Tenga en cuenta que las últimas tres variables deben escalarse para dispositivos con valores altos de PPP, porque especifican una velocidad de cambio en píxeles.

### <a name="4-construct-spriteclasses"></a>4. Construir SpriteClasses
Construiremos objetos SpriteClass en el método **LoadContent**. Agrega este código a lo que ya tienes:

```CSharp
dino = new SpriteClass(GraphicsDevice, "Content/ninja-cat-dino.png", ScaleToHighDPI(1f));
broccoli = new SpriteClass(GraphicsDevice, "Content/broccoli.png", ScaleToHighDPI(0.2f));
```

La imagen del brécol es un poco más grande de lo que queríamos, así que la reduciremos verticalmente a 0,2 veces su tamaño original.

### <a name="5-program-obstacle-behaviour"></a>5. Programar el comportamiento del obstáculo
Queremos que el brécol se genere fuera de pantalla y se dirija hacia el avatar del jugador, para que tenga que esquivarlo. Para lograrlo, agregue este método a la clase **Game1.cs**:

```CSharp
public void SpawnBroccoli()
{
  int direction = random.Next(1, 5);
  switch (direction)
  {
    case 1:
      broccoli.x = -100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 2:
      broccoli.y = -100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
    case 3:
      broccoli.x = screenWidth + 100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 4:
      broccoli.y = screenHeight + 100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
  }

  if (score % 5 == 0) broccoliSpeedMultiplier += 0.2f;

  broccoli.dX = (dino.x - broccoli.x) * broccoliSpeedMultiplier;
  broccoli.dY = (dino.y - broccoli.y) * broccoliSpeedMultiplier;
  broccoli.dA = 7f;
}
```

La primera parte del método determina desde qué punto de la pantalla se generará el brécol, mediante dos números aleatorios.

La segunda parte determina la velocidad a la que el brécol se moverá, lo que se determina mediante la puntuación actual. La velocidad aumentará por cada cinco brécoles que el jugador consiga esquivar.

La tercera parte configura la dirección del movimiento del sprite del brécol. Se dirige en dirección del avatar del jugador (dinosaurio) cuando se esquiva el brécol. También se le da un valor **dA** de 7f, que provocará que el brécol gire en el aire mientras persigue al jugador.

### <a name="6-program-game-starting-state"></a>6. Programar el estado de inicio del juego
Antes de pasar al control de entrada del teclado, es necesario un método que configure el estado inicial del juego de los dos objetos que creamos. En lugar de que el juego comience en cuanto se ejecute la aplicación, queremos que el usuario lo inicie manualmente al presionar la barra espaciadora. Agregue el código siguiente, que configura el estado inicial de los objetos animados y restablece la puntuación:

```CSharp
public void StartGame()
{
  dino.x = screenWidth / 2;
  dino.y = screenHeight * SKYRATIO;
  broccoliSpeedMultiplier = 0.5f;
  SpawnBroccoli();  
  score = 0;
}
```

### <a name="7-handle-keyboard-input"></a>7. Controlar la entrada de teclado
A continuación necesitamos un nuevo método para controlar la entrada de usuario mediante el teclado. Agregue este método a **Game1.cs**:

```CSharp
void KeyboardHandler()
{
  KeyboardState state = Keyboard.GetState();

  // Quit the game if Escape is pressed.
  if (state.IsKeyDown(Keys.Escape))
  {
    Exit();
  }

  // Start the game if Space is pressed.
  if (!gameStarted)
  {
    if (state.IsKeyDown(Keys.Space))
    {
      StartGame();
      gameStarted = true;
      spaceDown = true;
      gameOver = false;
    }
    return;
  }            
  // Jump if Space is pressed
  if (state.IsKeyDown(Keys.Space) || state.IsKeyDown(Keys.Up))
  {
    // Jump if the Space is pressed but not held and the dino is on the floor
    if (!spaceDown && dino.y >= screenHeight * SKYRATIO - 1) dino.dY = dinoJumpY;

    spaceDown = true;
  }
  else spaceDown = false;

  // Handle left and right
  if (state.IsKeyDown(Keys.Left)) dino.dX = dinoSpeedX * -1;

  else if (state.IsKeyDown(Keys.Right)) dino.dX = dinoSpeedX;
  else dino.dX = 0;
}
```

Arriba tenemos una serie de cuatro instrucciones condicionales:

La primera cierra el juego si se presiona la tecla **Escape**.

La segunda inicia el juego si se presiona la tecla **Barra espaciadora** y el juego no ha comenzado todavía.

La tercera hace que el avatar de dinosaurio salte si se presiona la tecla **Barra espaciadora**, al modificar su propiedad **dY**. Tenga en cuenta que el jugador no puede saltar a menos que esté en el “suelo” (dino.y = screenHeight * SKYRATIO), y tampoco saltará si la barra espaciadora se mantiene pulsada en lugar de pulsarla solo una vez. Esto impide que el dinosaurio salte tan pronto comienza el juego, aprovechándose de la misma pulsación que comienza el juego.

Por último, la última cláusula condicional permite comprobar si las flechas izquierda y derecha se están presionando y, si es el caso, cambia la propiedad **dX** del dinosaurio consecuentemente.

**Desafío:** ¿puedes hacer que el método de control de teclado anterior funcione con el esquema de entrada WASD así como las teclas de dirección?

### <a name="8-add-logic-to-the-update-method"></a>8. Agregar lógica al método Update
A continuación, tenemos que agregar lógica para todas estas partes en el método **Update** en **Game1.cs**:

```CSharp
float elapsedTime = (float)gameTime.ElapsedGameTime.TotalSeconds;
KeyboardHandler(); // Handle keyboard input
// Update animated SpriteClass objects based on their current rates of change
dino.Update(elapsedTime);
broccoli.Update(elapsedTime);

// Accelerate the dino downward each frame to simulate gravity.
dino.dY += gravitySpeed;

// Set game floor so the player does not fall through it
if (dino.y > screenHeight * SKYRATIO)
{
  dino.dY = 0;
  dino.y = screenHeight * SKYRATIO;
}

// Set game edges to prevent the player from moving offscreen
if (dino.x > screenWidth - dino.texture.Width/2)
{
  dino.x = screenWidth - dino.texture.Width/2;
  dino.dX = 0;
}
if (dino.x < 0 + dino.texture.Width/2)
{
  dino.x = 0 + dino.texture.Width/2;
  dino.dX = 0;
}

// If the broccoli goes offscreen, spawn a new one and iterate the score
if (broccoli.y > screenHeight+100 || broccoli.y < -100 || broccoli.x > screenWidth+100 || broccoli.x < -100)
{
  SpawnBroccoli();
  score++;
}
```

### <a name="9-draw-spriteclass-objects"></a>9. Dibujar objetos SpriteClass
Por último, agregue el siguiente código al método **Draw** de **Game1.cs**, justo después de la última llamada de **spriteBatch.Draw**:

```CSharp
broccoli.Draw(spriteBatch);
dino.Draw(spriteBatch);
```

En MonoGame, las nuevas llamadas de **spriteBatch.Draw** prevalecerán sobre cualquier llamada anterior. Esto significa que tanto el sprite del brécol como el del dinosaurio se dibujarán sobre el sprite del césped existente, para que nunca puedan esconderse independientemente de su posición.

Intente ejecutar el juego ahora y mover el dinosaurio con las teclas de dirección y la barra espaciadora. Si siguió los pasos anteriores, debería poder mover el avatar por la ventana del juego, y el brécol debería moverse a una velocidad creciente.

![Avatar del jugador y obstáculo](images/monogame-tutorial-2.png)

## <a name="render-text-with-spritefont"></a>Representar texto con SpriteFont
Con el código anterior, se podrá realizar un seguimiento de la puntuación del jugador de manera oculta, pero sin decirle cuál es. También hay una introducción poco intuitiva cuando la aplicación se inicia; el jugador ve una ventana azul y verde, pero es imposible que sepa que es necesario pulsar la barra espaciadora para que las cosas empiecen a moverse.

Para solucionar estos problemas, vamos a usar un nuevo tipo de objeto MonoGame denominado **SpriteFonts**.

### <a name="1-create-spritefont-description-files"></a>1. Crear archivos de descripción de SpriteFont
En el **Explorador de soluciones** busque la carpeta **Contenido**. En esta carpeta, haga clic con el botón derecho en el archivo **Content.mgcb** y seleccione **Abrir con**. En el menú emergente seleccione **Canalización MonoGame** y, después, pulse **Aceptar**. En la nueva ventana, haga clic con el botón derecho en el elemento **Contenido** y seleccione **Agregar -> Nuevo elemento**. Seleccione **SpriteFont Description**, asígnele el nombre “Score” y presione **Aceptar**. Después, agregue otra descripción de SpriteFont denominada “GameState” con el mismo procedimiento.

### <a name="2-edit-descriptions"></a>2. Editar descripciones
Haga clic con el botón derecho en la carpeta **Contenido** de **Canalización de MonoGame** y seleccione **Abrir ubicación del archivo**. Debería ver una carpeta con los archivos de descripción de SpriteFont que acaba de crear, así como cualquier imagen que haya agregado a la carpeta Contenido hasta ahora. Puede ahora cerrar y guardar la ventana de Canalización de MonoGame. En el **Explorador de archivos**, abra ambos archivos de descripción en un editor de texto (Visual Studio, NotePad++, Atom, etc).

Cada descripción contiene un número de valores que describen SpriteFont. Vamos a hacer algunos cambios:

En **Score.spritefont**, cambie el valor **<Size>** de 12 a 36.

En **GameState.spritefont**, cambie el valor **<Size>** de 12 a 72, y el valor **<FontName>** de Arial a Agency. Agency es otra fuente que se incluye con los ordenadores Windows 10, y que aportará algo de estilo nuestra pantalla de introducción.

### <a name="3-load-spritefonts"></a>3. Cargar SpriteFonts
En Visual Studio, primero vamos a agregar una nueva textura a la pantalla de presentación. [Haga clic aquí para descargar la imagen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/start-splash.png).

Al igual que antes, agregue la textura al proyecto haciendo clic con el botón derecho en Contenido y seleccionando **Agregar -> Elemento existente**. Asígnele un nombre nuevo al elemento “start-splash.png”.

A continuación, agregue las siguientes variables a **Game1.cs**:

```CSharp
Texture2D startGameSplash;
SpriteFont scoreFont;
SpriteFont stateFont;
```

Después agregue estas líneas al método **LoadContent**:

```CSharp
startGameSplash = Content.Load<Texture2D>("start-splash");
scoreFont = Content.Load<SpriteFont>("Score");
stateFont = Content.Load<SpriteFont>("GameState");
```

### <a name="4-draw-the-score"></a>4. Dibujar la puntuación
Vaya al método **Draw** de **Game1.cs** y agregue el siguiente código justo antes de **spriteBatch.End();**

```CSharp
spriteBatch.DrawString(scoreFont, score.ToString(),
new Vector2(screenWidth - 100, 50), Color.Black);
```

El código anterior usa la descripción sprite que creamos (Arial tamaño 36) para dibujar la puntuación actual del jugador en la esquina superior derecha de la pantalla.

### <a name="5-draw-horizontally-centered-text"></a>5. Dibujar el texto centrado horizontalmente
Al crear un juego, puede que quiera dibujar un texto centrado, ya sea horizontal o verticalmente. Para centrar horizontalmente el texto introductorio, agregue este código al método **Draw** justo antes de **spriteBatch.End();**

```CSharp
if (!gameStarted)
{
  // Fill the screen with black before the game starts
  spriteBatch.Draw(startGameSplash, new Rectangle(0, 0,
  (int)screenWidth, (int)screenHeight), Color.White);

  String title = "VEGGIE JUMP";
  String pressSpace = "Press Space to start";

  // Measure the size of text in the given font
  Vector2 titleSize = stateFont.MeasureString(title);
  Vector2 pressSpaceSize = stateFont.MeasureString(pressSpace);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, title,
  new Vector2(screenWidth / 2 - titleSize.X / 2, screenHeight / 3),
  Color.ForestGreen);
  spriteBatch.DrawString(stateFont, pressSpace,
  new Vector2(screenWidth / 2 - pressSpaceSize.X / 2,
  screenHeight / 2), Color.White);
  }
```

Primero creados dos Cadenas, una para cada línea de texto que queremos dibujar. Después, medimos el ancho y el alto de cada línea impresa, usando el método **SpriteFont.MeasureString(String)** . Esto nos da el tamaño como un objeto **Vector2**, con la propiedad **X** que contiene el ancho y la propiedad **Y**, la altura.

Por último, dibujamos cada línea. Para centrar el texto horizontalmente, igualamos el valor **X** del vector de su posición con **screenWidth / 2 - textSize.X / 2**.

**Desafío:** ¿cómo cambiaría el procedimiento anterior para centrar el texto vertical y horizontalmente?

Pruebe a ejecutar el juego. ¿Ve la pantalla de presentación? ¿La puntuación aumenta cada vez que se esquiva el brécol?

![Pantalla de presentación](images/monogame-tutorial-3.png)

## <a name="collision-detection"></a>Detección de colisión
Ya tenemos un brécol que lo persigue, así como la puntuación que aumenta cada vez que se esquiva uno, pero así como está, no hay manera de perder este juego. Necesitamos saber si los sprites del dino y del brécol colisionan y, si lo hacen, indicar el fin del juego.

### <a name="1-get-the-textures"></a>1. Obtener las texturas
La última imagen que necesitamos es una para “game over”. [Haga clic aquí para descargar la imagen](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/game-over.png).

Tal y como hicimos antes con el rectángulo verde, el gato ninja y el brécol, agregue esta imagen a **Content.mgcb** a través de **Canalización MonoGame** y asígnele el hombre “game-over.png”.

### <a name="2-rectangular-collision"></a>2. Colisión rectangular
Al detectar colisiones en un juego, los objetos suelen simplificarse para reducir la complejidad de las matemáticas que implican. Vamos a tratar al avatar del jugador y al brécol que funciona como obstáculo como rectángulos para detectar la colisión entre ellos.

Abra **SpriteClass.cs** y agregue una nueva variable de clase:

```CSharp
const float HITBOXSCALE = .5f;
```

Este valor representará el nivel de “indulgencia” a la hora de detectar las colisiones para el jugador. Con un valor de .5f, los bordes del rectángulo contra los cuales el dinosaurio puede colisionar contra el brécol (a menudo denominado “cuadro de colisión”) serán la mitad del tamaño completo de la textura. Esto provocará que sean pocos los casos en los que las esquinas de las dos texturas colisionen, sin que parezca que las imágenes se estén tocando. No dude en retocar este valor según le parezca.

A continuación, agregue un nuevo método a **SpriteClass.cs**:

```CSharp
public bool RectangleCollision(SpriteClass otherSprite)
{
  if (this.x + this.texture.Width * this.scale * HITBOXSCALE / 2 < otherSprite.x - otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y + this.texture.Height * this.scale * HITBOXSCALE / 2 < otherSprite.y - otherSprite.texture.Height * otherSprite.scale / 2) return false;
  if (this.x - this.texture.Width * this.scale * HITBOXSCALE / 2 > otherSprite.x + otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y - this.texture.Height * this.scale * HITBOXSCALE / 2 > otherSprite.y + otherSprite.texture.Height * otherSprite.scale / 2) return false;
  return true;
}
```

Este método detecta si han colisionado dos objetos rectangulares. El algoritmo funciona probando para ver si hay un espacio entre cualquiera de los lados de los rectángulos. Si hay algún espacio, no hay ninguna colisión; si no existe ningún intervalo, debe haber una colisión.

### <a name="3-load-new-textures"></a>3. Cargar nuevas texturas

A continuación, abra **Game1.cs** y agregue dos variables de clase nuevas, una para almacenar la textura de sprite para el fin del juego y una booleana para realizar un seguimiento del estado del juego:

```CSharp
Texture2D gameOverTexture;
bool gameOver;
```

Después, inicialice **gameOver** en el método **Inicializar**:

```CSharp
gameOver = false;
```

Por último, cargue la textura en **gameOverTexture** en el método **LoadContent**:

```CSharp
gameOverTexture = Content.Load<Texture2D>("game-over");
```

### <a name="4-implement-game-over-logic"></a>4. Implementar lógica de “game over”
Agregue este código al método **Update**, justo después de llamar al método **KeyboardHandler**:

```CSharp
if (gameOver)
{
  dino.dX = 0;
  dino.dY = 0;
  broccoli.dX = 0;
  broccoli.dY = 0;
  broccoli.dA = 0;
}
```

Esto hará que el movimiento se detenga una vez terminado el juego, y los sprites de dinosaurio y de brécol se inmovilizarán en sus posiciones.

A continuación, al final del método **Update**, justo antes de **base. Update(gameTime)** , agregue esta línea:

```CSharp
if (dino.RectangleCollision(broccoli)) gameOver = true;
```

Esto llama al método **RectangleCollision** que creamos en **SpriteClass** y marca el juego como terminado si devuelve true.

### <a name="5-add-user-input-for-resetting-the-game"></a>5. Agregar entrada de usuario para restablecer el juego
Agregue este código al método **KeyboardHandler** para permitir al usuario restablecer el juego si presiona ENTRAR:

```CSharp
if (gameOver && state.IsKeyDown(Keys.Enter))
{
  StartGame();
  gameOver = false;
}
```

### <a name="6-draw-game-over-splash-and-text"></a>6. Dibujar el juego sobre la presentación y el texto
Por último, agregue este código al método Draw, justo después de la primera llamada de **spriteBatch.Draw** (debería ser la llamada que dibuja la textura de césped).

```CSharp
if (gameOver)
{
  // Draw game over texture
  spriteBatch.Draw(gameOverTexture, new Vector2(screenWidth / 2 - gameOverTexture.Width / 2, screenHeight / 4 - gameOverTexture.Width / 2), Color.White);

  String pressEnter = "Press Enter to restart!";

  // Measure the size of text in the given font
  Vector2 pressEnterSize = stateFont.MeasureString(pressEnter);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, pressEnter, new Vector2(screenWidth / 2 - pressEnterSize.X / 2, screenHeight - 200), Color.White);
}
```

Aquí usamos el mismo método que antes para dibujar el texto centrado horizontalmente (volviendo a usar la fuente que usamos en la presentación), así como centrando **gameOverTexture** en la mitad superior de la ventana.

¡Y listo! Pruebe a ejecutar el juego otra vez. Si siguió los pasos anteriores, el juego debería finalizar ahora cuando el dinosaurio colisiona con el brécol y debería solicitarse al jugador que reinicie el juego presionando la tecla ENTRAR.

![Juego terminado](images/monogame-tutorial-4.png)

## <a name="publish-to-the-microsoft-store"></a>Publicación en Microsoft Store
Dado que compilamos este juego como una aplicación para UWP, es posible publicar este proyecto en la Microsoft Store. Este proceso tiene diferentes pasos.

Tiene que estar [registrado](https://developer.microsoft.com/store/register) como desarrollador de Windows.

Debe usar la [lista de comprobación del envío de la aplicación](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

La aplicación debe enviarse para su [certificación](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process).

Para obtener más información, consulte [Publicar su aplicación para UWP](https://docs.microsoft.com/windows/uwp/publish/).
