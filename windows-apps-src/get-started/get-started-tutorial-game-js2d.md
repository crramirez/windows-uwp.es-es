---
title: Crear un juego para UWP en JavaScript
description: Un sencillo juego para UWP para Microsoft Store, escrito en JavaScript y CreateJS
author: GrantMeStrength
ms.author: jken
ms.date: 02/09/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 01af8254-b073-445e-af4c-e474528f8aa3
ms.localizationpriority: medium
ms.openlocfilehash: 597451826958c355dad9f9380dbdc1264bc87883
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5865800"
---
# <a name="create-a-uwp-game-in-javascript"></a>Crear un juego para UWP en JavaScript

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-javascript-and-createjs"></a>Un sencillo juego en 2D para UWP para Microsoft Store, escrito en JavaScript y CreateJS


![Hoja de sprite de un dinosaurio caminando](images/JS2D_1.png)


## <a name="introduction"></a>Introducción


Publicación de una aplicación a los medios de Microsoft Store puede compartirla (o venderla a!) con millones de personas con muchos dispositivos diferentes.  

Para poder publicar la aplicación en Microsoft Store debe estar escrita como una aplicación para UWP (plataforma Universal de Windows). Sin embargo, la UWP es muy flexible y admite una amplia variedad de lenguajes y marcos. Para demostrar esto, en este ejemplo se muestra un juego sencillo escrito en JavaScript, haciendo uso de varias bibliotecas CreateJS, y se muestra cómo dibujar sprites, crear un bucle de juego, ofrecer compatibilidad con el teclado y el mouse, y adaptarse a pantallas de diferentes tamaños.

Este proyecto se crea con JavaScript mediante Visual Studio. Con algunos cambios menores, también puede hospedarse en un sitio web o adaptarse para otras plataformas. 

**Nota:** Esto no es un juego completo (ni necesariamente bueno); está diseñada para mostrar como usar JavaScript y un tercer biblioteca de terceros para preparar una aplicación lista para publicar en Microsoft Store.


## <a name="requirements"></a>Requisitos

Para jugar con este proyecto, necesitarás lo siguiente:

* Un equipo de Windows (o una máquina virtual) que ejecute la versión actual de Windows 10.
* Una copia de Visual Studio. Puedes descargar la copia gratuita de Visual Studio Community Edition desde la [página principal de Visual Studio](http://visualstudio.com).

En este proyecto se usa el marco de CreateJS JavaScript. CreateJS es un conjunto gratuito de herramientas, publicado bajo una licencia de MIT, diseñado para facilitar la tarea de crear juegos basados en sprite. Las bibliotecas CreateJS ya están presentes en el proyecto (busca *js/easeljs-0.8.2.min.js* y *js/preloadjs-0.6.2.min.js* en la vista Explorador de soluciones). Para obtener más información acerca de CreateJS, consulta la [página principal de CreateJS](http://www.createjs.com).


## <a name="getting-started"></a>Introducción

El código fuente completo de la aplicación se almacena en [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js2d).

La forma más sencilla de empezar es visitar GitHub, haz clic en el botón verde **Clonar o descargar** y selecciona **Abrir en Visual Studio**. 

![Clonar el repositorio](images/JS2D_2.png)

También puedes descargar el proyecto como un archivo zip, o usar cualquier otra manera estándar para que funcione con [proyectos de GitHub](https://msdn.microsoft.com/en-us/windows/uwp/get-started/get-uwp-app-samples).

Una vez que se ha cargado la solución en Visual Studio, verás varios archivos, incluidos:

* Images/: una carpeta que contiene diferentes iconos que requieren las aplicaciones para UWP, así como el SpriteSheet y otros mapas de bits del juego.
* js/: una carpeta que contiene archivos JavaScript. El archivo main.js es nuestro juego, los demás archivos son EaselJS y PreloadJS.
* index.html: la página web que contiene el objeto Canvas donde se hospedan los gráficos del juego.

Ya puedes ejecutar el juego.

Presiona **F5** para ejecutar la aplicación. Deberías ver una ventana abierta y a nuestro dinosaurio en un entorno idílico (aunque) horizontal. Ahora examinaremos la aplicación, explicaremos algunas partes importantes y desbloquearemos el resto de las características a medida que avancemos.

![Un simple dinosaurio con un gato ninja en el lomo](images/JS2D_3.png)

**Nota:** ¿Hubo algún problema? Asegúrate de haber instalado Visual Studio con el soporte web. Puedes crear un proyecto nuevo para comprobarlo; si no existe compatibilidad con JavaScript, tendrás que volver a instalar Visual Studio y marcar el cuadro *Microsoft Web Developer Tools*.

## <a name="walkthough"></a>Tutorial

Si iniciaste el juego con F5, probablemente te estarás preguntando qué está pasando. Y la respuesta es "no mucho", como una gran parte del código está comentado. Hasta ahora, todo lo verás es el dinosaurio y una solicitud ineficaz pulsar la barra espaciadora. 

### <a name="1-setting-the-stage"></a>1. Configurar el escenario

Si abres y examinas **index.html**, verás que está casi vacío. Este archivo es la página web predeterminada que contiene nuestra aplicación, y hace solo dos cosas importantes. En primer lugar, incluye el código fuente de JavaScript para las bibliotecas CreateJS **EaselJS** y **PreloadJS**, y también **main.js** (nuestro propio archivo de código fuente).
En segundo lugar, define una etiqueta &lt;canvas&gt;, que es donde aparecerán todos los gráficos. Un &lt;canvas&gt; es un componente de documento estándar HTML5. Le asignamos un nombre (gameCanvas) para que nuestro código en **main.js** puede hacer referencia a él. Por cierto, si vas a escribir tu propio juego en JavaScript desde cero, también deberás copiar los archivos **EaselJS** y **PreloadJS** en tu solución y, después, crea un objeto Canvas.

EaselJS nos ofrece un nuevo objeto denominado *Stage*. El objeto Stage está directamente relacionado con Canvas y se usa para mostrar imágenes y texto. Cualquier objeto que se quiera mostrar en el escenario deberá agregarse primero como un elemento secundario del objeto Stage, como este:

```
    stage.addChild(myObject);
```

Verás que la línea de código aparece varias veces en **main.js**

A propósito, ahora es un buen momento para abrir **main.js**.

### <a name="2-loading-the-bitmaps"></a>2. Cargar los mapas de bits

EaselJS nos proporciona distintos tipos de objetos gráficos. Podemos crear formas simples (por ejemplo, el rectángulo azul que se usa para el cielo), o mapas de bits (por ejemplo, las nubes que vamos a agregar), objetos de texto y sprites. Los sprites usan un (SpriteSheet) [http://createjs.com/docs/easeljs/classes/SpriteSheet.html]: un único mapa de bits que contiene varias imágenes. Por ejemplo, usamos este SpriteSheet para almacenar los distintos fotogramas de la animación de dinosaurio:

![Hoja de sprite de un dinosaurio caminando](images/JS2D_4.png)

Haremos que el dinosaurio camine, mediante la definición de diferentes fotogramas y la rapidez con la que se animarán en este código:

```
    // Define the animated dino walk using a spritesheet of images,
    // and also a standing-still state, and a knocked-over state.
    var data = {
        images: [loader.getResult("dino")],
        frames: { width: 373, height: 256 },
        animations: {
            stand: 0,
            lying: { 
                frames: [0, 1],
                speed: 0.1
            },
            walk: {
                frames: [0, 1, 2, 3, 2, 1],
                speed: 0.4
            }
        }
    }

    var spriteSheet = new createjs.SpriteSheet(data);
    dino_walk = new createjs.Sprite(spriteSheet, "walk");
    dino_stand = new createjs.Sprite(spriteSheet, "stand");
    dino_lying = new createjs.Sprite(spriteSheet, "lying");

```

Ahora, vamos a agregar algunas nubes esponjosas al escenario. Una vez que se ejecute el juego, se deslizarán por la pantalla. La imagen para la nube ya está en la solución, en la carpeta de *imágenes*.

Busca en **main.js** hasta que encuentres la función **init()**. Esta función se llama cuando se inicia el juego, y es dónde empezamos a configurar todos los objetos gráficos.

Busca el código siguiente y quita los comentarios (\\) de la línea que hace referencia a la imagen de la nube.

```
 manifest = [
        { src: "walkingDino-SpriteSheet.png", id: "dino" },
        { src: "barrel.png", id: "barrel" },
        { src: "fluffy-cloud-small.png", id: "cloud" },
    ];
```

JavaScript necesita un poco de ayuda para cargar recursos como imágenes, así que estamos usando una característica de la biblioteca CreateJS que permite precargar imágenes, que se denomina [LoadQueue](http://www.createjs.com/docs/preloadjs/classes/LoadQueue.html). No estamos seguros de cuánto tardarán las imágenes en cargarse, así que usamos LoadQueue para que se encargue de ello. Una vez que las imágenes estén disponibles, la cola nos indicará que están listas. Para ello, primero crearemos un nuevo objeto que enumera todas nuestras imágenes y, después, creamos un objeto LoadQueue. En el código siguiente verás cómo se configura una llamada a una función denominada **loadingComplete()** cuando todo esté listo.

```
    // Now we create a special queue, and finally a handler that is
    // called when they are loaded. The queue object is provided by preloadjs.

    loader = new createjs.LoadQueue(false);
    loader.addEventListener("complete", loadingComplete);
    loader.loadManifest(manifest, true, "../images/");
```    

Cuando se llama a la función **loadingComplete()**, las imágenes se cargan y están listas para usar. Podrás ver una sección comentada que crea las nubes, ahora su mapa de bits ya está disponible. Quita los comentarios, para que tenga este aspecto:

```
    // Create some clouds to drift by..
    for (var i = 0; i < 3; i++) {
        cloud[i] = new createjs.Bitmap(loader.getResult("cloud"));
        cloud[i].x = Math.random()*1024; // Random start location
        cloud[i].y = 64 + i * 48;
        stage.addChild(cloud[i]);
    }
```
Este código crea tres objetos de nube mediante nuestra imagen precargada, define su ubicación y, después, los agrega al escenario.

Vuelve a ejecutar la aplicación (presiona F5) y verás que aparecen las nubes.

### <a name="3-moving-the-clouds"></a>3. Mover las nubes

Ahora vamos a hacer que las nubes se muevan. El secreto para hacer que las nubes se muevan, así como para mover cualquier otra cosa, es configurar una función [ticker](http://www.createjs.com/docs/easeljs/classes/Ticker.html) que se llama repetidamente varias veces por segundo. Cada vez que se llama a esta función, los gráficos vuelven a dibujarse en un lugar ligeramente diferente.

<p data-height="500" data-theme-id="23761" data-slug-hash="vxZVRK" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="CreateJS - Animating clouds" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/vxZVRK/">CreateJS - Animating clouds</a> (CreateJS, animación de nubes) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
 
El código para esto ya está en el archivo **main.js**, que proporciona la biblioteca CreateJS, EaselJS. Tiene esta apariencia:

```
    // Set up the game loop and keyboard handler.
    // The keyword 'tick' is required to automatically animated the sprite.
    createjs.Ticker.timingMode = createjs.Ticker.RAF;
    createjs.Ticker.addEventListener("tick", gameLoop);
```

Este código llamará a una función denominada **gameLoop()** entre 30 y 60 fotogramas por segundo. La velocidad exacta depende de la velocidad del equipo.

Busca la función **gameLoop()** y hacia el final verás una función denominada **animateClouds()**. Modifícala para que no se comente.

```
    // Move clouds
    animateClouds();
```

Si consultas la definición de esta función, verás cuánto tarda cada nube y verás cómo cambia su coordenada x. Si la ordenada x está fuera de la pantalla, se restablece al extremo derecho. Cada nube también se mueve a una velocidad ligeramente diferente.

```
function animate_clouds()
{
    // Move the cloud sprites across the sky. If they get to the left edge, 
    // move them over to the right.

    for (var i = 0; i < 3; i++) {    
        cloud[i].x = cloud[i].x - (i+1);
        if (cloud[i].x < -128)
            cloud[i].x = width + 128;
    }
}
```

Si ejecutas la aplicación ahora, verás que las nubes han iniciado el desplazamiento. ¡Por fin tenemos movimiento!

### <a name="4-adding-keyboard-and-mouse-input"></a>4. Agregar entrada de teclado y mouse

Un juego con el que no se puede interactuar, no es un juego. Así que vamos a permitir que el jugador use el teclado o el mouse para hacer algo. En la función **loadingComplete()**, verás lo siguiente. Quita los comentarios.

```
    // This code will call the method 'keyboardPressed' is the user presses a key.
    this.document.onkeydown = keyboardPressed;

    // Add support for mouse clicks
    stage.on("stagemousedown", mouseClicked);
```

Ahora tenemos dos funciones que se llamarán cada vez que el jugador toque una tecla o haga clic con el mouse. Ambos eventos llamarán a **userDidSomething()**, una función que mira la variable gamestate para decidir qué está haciendo el juego, y qué debe ocurrir después como resultado.

Gamestate es un patrón de diseño común que se usa en juegos. Todo lo que sucede, sucede en la función **gameLoop()** que llama el temporizador ticker. gameLoop() realiza un seguimiento de si se está jugando al juego, o si está en un "estado de fin del juego" o en un "estado listo para jugar", o cualquier otro estado que defina el autor, con una variable. Esta variable de estado se comprueba en una instrucción switch y esto define a qué otras funciones se llama. Por lo tanto, si el estado se establece en "jugando", se llamará a las funciones para hacer que el dinosaurio salte y que los barriles se muevan. Si algo mata al dinosaurio, la variable gamestate se configurará en "estado de fin del juego" y se mostrará el mensaje "¡Fin del juego !". Si estás interesado en los patrones de diseño de juego, te recomendamos el libro [Game Programming Patterns](http://gameprogrammingpatterns.com/) (Modelos de programación de juegos).

Vuelva a intentar ejecutar la aplicación y, por último, podrás comenzar a jugar. Presiona la barra espaciadora (o haz clic en el mouse o toca la pantalla) para que pase algo. 

Verás un barril que rueda hacia ti: vuelve a presionar la barra espaciadora o pulsa otra vez en el momento justo para que el dinosaurio salte. Si no calculas bien, habrás perdido.

El barril está animado de la misma manera que las nubes (aunque cada vez va más rápido), y se comprueba la posición del dinosaurio y del barril para asegurar que no colisionaron:

```
 // Very simple check for collision between dino and barrel
                if ((barrel.x > 220 && barrel.x < 380)
                    &&
                    (!jumping))
                {
                    barrel.x = 380;
                    GameState = GameStateEnum.GameOver;
                }
```

Si el dinosaurio no salta y el barril está cerca, el código cambiar la variable de estado al estado que denominamos *GameOver*. Como puedes imaginar, *GameOver* hace que finalice el juego.

Estos son los mecanismos principales del juego.

### <a name="5-resizing-support"></a>5. Compatibilidad con cambio de tamaño

Ya casi hemos terminado. Pero antes de hacerlo, hay un problema molesto del que debemos ocuparnos. Cuando el juego se esté ejecutando, intenta cambiar el tamaño de la ventana. Verás que el juego se estropea rápidamente, y los objetos no están donde deberían. Podemos solucionar esto si creamos un controlador para el evento de cambio de tamaño de ventana que se genera cuando el jugador cambia el tamaño de la ventana, o cuando el dispositivo se gira de la vista horizontal a la vertical.

El código para hacer esto ya está presente (de hecho, lo llamamos cuando comenzamos el juego por primera vez, para asegurar que el tamaño predeterminado de la ventana funciona, porque cuando se inicia una aplicación para UWP no se puede asegurar el tamaño de la ventana).

Quita solo los comentarios de esta línea para llamar a la función cuando se desencadene el evento de tamaño de pantalla:

```
    // This code makes the app call the method 'resizeGameWindow' if the user resizes the current window.
     window.addEventListener('resize', resizeGameWindow);
```

Si vuelves a ejecutar la aplicación, deberías poder cambiar el tamaño de la ventana y obtener mejores resultados.

## <a name="publishing-to-the-microsoft-store"></a>Publicar en Microsoft Store

Ahora que tienes una aplicación para UWP, es posible publicar en Microsoft Store (siempre que la hayas mejorado). 

Este proceso tiene diferentes pasos.

1. Tienes que estar [registrado](https://developer.microsoft.com/en-us/store/register) como desarrollador de Windows.
2. Debes usar la [lista de comprobación](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) del envío de la aplicación.
3. La aplicación debe enviarse para su [certificación](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process).

Para obtener más información, consulta la [publicación de tu aplicación para UWP](https://developer.microsoft.com/en-us/store/publish-apps).

## <a name="suggestions-for-other-features"></a>Sugerencias para otras características.

¿Qué debo hacer a continuación? Estas son algunas sugerencias de características que podrías agregar a tu (próximamente) galardonada aplicación.

1. Efectos de sonido. La biblioteca CreateJS incluye compatibilidad con sonido, con una biblioteca denominada [SoundJS](http://www.createjs.com/soundjs).
2. Compatibilidad con el controlador para juegos. Hay una [API disponible](https://gamedevelopment.tutsplus.com/tutorials/using-the-html5-gamepad-api-to-add-controller-support-to-browser-games--cms-21345).
3. ¡Mejora al máximo tu juego! Esa parte depende de ti, pero hay muchos recursos disponibles en línea. 

## <a name="other-links"></a>Otros vínculos

* [Make a simple Windows game with JavaScript (Crear un juego sencillo de Windows con JavaScript)](https://www.sitepoint.com/creating-a-simple-windows-8-game-with-javascript-game-basics-createjseaseljs/)
* [Picking an HTML/JS game engine (Seleccionar un motor de juego de HTML/JS)](https://html5gameengine.com/)
* [Using CreateJS in your JS based game (Usar CreateJS en tu juego basado en JS)](https://blogs.msdn.microsoft.com/cbowen/2012/09/19/using-createjs-in-your-javascript-based-windows-8-game/)
* [Cursos de desarrollo de juegos de LinkedIn Learning](https://www.linkedin.com/learning/topics/game-development)
