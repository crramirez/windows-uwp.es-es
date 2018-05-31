---
author: QuinnRadich
title: Estilo de escritura
description: Usar un tono y voz adecuados es fundamental para que el texto de la aplicación parezca una parte natural de su diseño.
keywords: UWP, Windows 10, texto, escritura, voz, tono, diseño, IU, UX
ms.author: quradic
ms.date: 1/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: d38f22f896e31a925f07c5d6dffd357c211b3336
ms.sourcegitcommit: d780e3a087ab5240ea643346480a1427bea9e29b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2018
ms.locfileid: "1573269"
---
# <a name="writing-style"></a>Estilo de escritura

La forma en la que elaboras un mensaje de error, la manera en que escribes la documentación de ayuda e incluso el texto que eliges para un botón tienen un gran impacto en la facilidad de uso de la aplicación. 

El estilo de escritura puede suponer una gran diferencia entre una experiencia de usuario terrible...

![Pantalla azul de la muerte de Windows 7](images/writing/bluescreen_old.png)

... y una mejor.

![Pantalla azul de la muerte de Windows 10](images/writing/bluescreen_new.png)


## <a name="writing-with-a-natural-voice-and-tone"></a>Escribir con una voz y un tono naturales

La investigación muestra que los usuarios responden mejor a un estilo de escritura descriptivo, útil y conciso. 

## <a name="be-friendly"></a>Sé amable

Por encima de todo, no quieres ahuyentar al usuario. Se informal y no usen términos que no se vayan a entender. Incluso cuando las cosas se rompen, no culpes al usuario de ningún problema. En vez de eso, tu aplicación debe asumir la responsabilidad y ofrecer instrucciones amables que sitúen las acciones del usuario en primer lugar.

![Antes: Se ha producido un error y no se ha podido cargar la imagen. Si también se produce un error en los intentos posteriores, es posible que se deba reinicializar la aplicación. Después: No se ha podido cargar la imagen porque se ha producido un error. Inténtalo de nuevo; si aún no funciona, prueba a reiniciar la aplicación.](images/writing/friendly_example.png)

## <a name="be-helpful"></a>Se útil

Céntrate siempre en explicar lo que está sucediendo y en proporcionar la información pertinente que el usuario necesita, sin sobrecargarla con información innecesaria.

![Antes: No se puede conectar a la red. Después: El dispositivo está conectado a una red, pero la red no está conectada a Internet. Intenta a reiniciar el enrutador o el módem. Si usas un punto de acceso público, asegúrate de haber iniciado sesión.](images/writing/helpful_example.png)

## <a name="be-clear-and-concise"></a>Sé claro y conciso

La mayoría de las veces, el texto no es el foco de una aplicación. Está ahí para guiar a los usuarios, para enseñarles qué está sucediendo y qué hacer a continuación. No pierdas esto de vista al escribir el texto en tu aplicación. Usa un lenguaje que resulte familiar para tu público. Por lo general, esto significa elegir palabras cotidianas y de conversación, pero las aplicaciones para usos especializados tendrán sus propios estándares. Los usuarios nunca deben preguntarse sobre lo que quieres decir en el texto, así que intenta usar un lenguaje sencillo si no estás seguro sobre qué tono usar.

![Antes: Algo fue mal y no pudimos guardar el archivo. Es posible que la ubicación no sea accesible para este dispositivo o que no haya espacio suficiente. Después: No hemos podido guardar el archivo en esa ubicación. Vuelve a intentarlo con una ubicación diferente.](images/writing/concise_example.png)

## <a name="writing-tips"></a>Sugerencias de escritura

Hay muchas maneras de poder implementar esos principios generales en el límite de espacio que tiene tu aplicación. A continuación, indicamos algunas estrategias que pensamos que son útiles para poner en acción esos principios:

### <a name="lead-with-whats-important"></a>Comienza con lo que es importante

Los usuarios deben poder leer y comprender el texto de un solo vistazo. No rellenes el texto con introducciones innecesarias. Da la máxima visibilidad a los puntos principales y siempre presenta el fundamento de una idea antes de agregarla.

**Correcto:** Selecciona "filtros" para agregar efectos a la imagen.

**Incorrecto:** Si quieres agregar modificaciones o efectos visuales a la imagen, selecciona "filtros".

### <a name="emphasize-action"></a>Enfatiza la acción

Las aplicaciones se definen mediante acciones. Los usuarios realizan una acción cuando usan la aplicación y esta realiza una acción cuando responde a los usuarios. Asegúrate de que el texto usa la *voz activa* en toda la aplicación. Los usuarios y las funciones deben describirse usando la forma activa, en lugar de la forma pasiva.

**Correcto:** Reinicia la aplicación para ver los cambios.

**Incorrecto:** Los cambios estarán aplicados cuando la aplicación esté reiniciada.

### <a name="short-and-sweet"></a>Breve y simple

Los usuarios echan un vistazo al texto y a menudo pasarán por alto bloques más grandes de palabras. No sacrifiques información ni presentaciones necesarias, pero no uses más palabras de las que necesites. En ocasiones, esto significa usar numerosas frases o fragmentos más cortos. Otras veces, esto significa ser muy selectivo a la hora de elegir palabras y estructuras de frases más largas.

**Correcto:** Lo sentimos, no pudimos cargar la imagen. Si esto sucede de nuevo, prueba a reiniciar la aplicación. Pero no te preocupes, la imagen estará esperando hasta que vuelvas.

**Incorrecto:** Se ha producido un error y no hemos podido cargar la imagen. Inténtalo de nuevo y, si se vuelve a producir este problema, puede que tengas que reiniciar la aplicación. Pero no te preocupes, hemos guardado tu trabajo localmente y te estará esperando cuando vuelvas.

## <a name="style-conventions"></a>Convenciones de estilo

Si no te consideras escritor, puede que te resulte intimidante intentar implementar estos principios y recomendaciones. Pero no te preocupes, usar un lenguaje sencillo y directo es una manera fantástica de proporcionar una buena experiencia de usuario. Y si aun así, no estás seguro de cómo estructurar tus palabras, aquí te indicamos algunas instrucciones útiles.

### <a name="addressing-the-user"></a>Tratamiento del usuario

Habla directamente al usuario.

* Trate siempre al usuario de"tú".

* Para hacer referencia a tu propia perspectiva, usa "nosotros". Resulta amable y ayuda al usuario a sentirse parte de la experiencia.

* No uses "yo" para hacer referencia a la perspectiva de la aplicación, incluso si eres el único que la ha creado.

> No hemos podido guardar el archivo en esa ubicación.

### <a name="abbreviations"></a>Abreviaturas

Las abreviaturas pueden ser útiles cuando se necesita hacer referencia a productos, lugares o conceptos técnicos varias veces en toda la aplicación. Pueden ahorrar espacio y ser más naturales, siempre y cuando el usuario los comprenda.

* No asumas que los usuarios ya están familiarizados con las abreviaturas, incluso si crees que son habituales.

* Define siempre lo que significa una abreviatura nueva la primera vez que el usuario la vea.

* No uses abreviaturas muy parecidos entre sí.

> Las instrucciones sobre diseño de la Plataforma universal de Windows (UWP) son un recurso que te ayudan a diseñar y crear fantásticas aplicaciones. Con las características de diseño que se incluyen en cada aplicación para UWP, puedes crear interfaces de usuario (IU) que se adecúen a una gama de dispositivos.

### <a name="contractions"></a>Contracciones

Usa un tono informal para dirigirte a los usuarios. Si usas un tono formal, la aplicación parecerá demasiado formal o incluso forzada.

* Usa contracciones cuando resulten naturales en el texto.

* No uses contracciones forzadas solo para ahorrar espacio o cuando parezca que, al usarlas, el texto sea menos informal.

> Cuando estés satisfecho con la imagen, presiona "guardar" para agregarla a la galería. Desde allí, podrás compartirla con tus amigos.

### <a name="periods"></a>Puntos

Finalizar el texto con un punto implica que el texto es una frase completa. Usa un punto para bloques de texto más grandes y evítalos para texto que sea más corto que una frase completa.

* Usa puntos para terminar oraciones completas en información sobre herramientas, mensajes de error y cuadros de diálogo.

* No termines con un punto el texto de botones, botones de radio, etiquetas o casillas.

> **No estás conectado**
> * Comprueba que tus cables de red están conectados.
> * Asegúrate de que no te encuentras en el modo avión.
> * Consulta si el interruptor inalámbrico está activado.
> * Reinicia el enrutador.

### <a name="capitalization"></a>Uso de mayúsculas

Aunque las letras mayúsculas son importantes, es fácil caer en un uso excesivo de ellas.

* Escribe en mayúscula los nombres propios.

* Escribe en mayúsculas el comienzo de cada cadena de texto en tu aplicación: el inicio de cada frase, etiqueta y título.

> **¿Qué parte te da problemas?**
> * He olvidado mi contraseña
> * No acepta mi contraseña
> * Es posible que otra persona esté usando mi cuenta

## <a name="error-messages"></a>Mensajes de error

Cuando se produce un error en la aplicación, los usuarios prestan atención. Dado que los usuarios pueden estar confusos o sentirse frustrados cuando ven un mensaje de error, se encuentra en una zona donde usar un buen tono puede tener un impacto particularmente importante.

Sobre todo, es importante que el mensaje de error no culpe al usuario. Pero también es importante no abrumarles con información que no comprendes. La mayoría de las veces un usuario que encuentra un mensaje de error solo desea volver a lo que estaba haciendo de la manera más rápida y sencilla posible. Por lo tanto, cualquier mensaje de error que escribas debería:

* **Sé amable**; asume la responsabilidad de lo que ha ocurrido, y evita términos desconocidos y una jerga técnica.

* **Sé útil**; indica al usuario qué ha ocurrido de la manera más sencilla posible y qué sucederá a continuación, y ofrécele una solución realista que pueda realizar.

* **Sé claros y conciso**; elimina información superflua.

![Ejemplo de un mensaje de error correcto](images/writing/connection_error.png)

## <a name="buttons"></a>Botones

El texto de los botones debe ser lo suficientemente conciso como para que los usuarios puedan leerlo todo de un vistazo y lo suficientemente claro para que la función del botón se obvia. La longitud máxima del texto de un botón nunca debe ser superior a un par de palabras breves y muchos deben tener incluso menos.

Al escribir el texto de botones, recuerda que cada botón representa una acción. Asegúrate de usar la *voz activa* en el texto del botón y de usar palabras que representen acciones en lugar de reacciones.

![Ejemplo de un botón correcto](images/writing/install_button.png)

## <a name="dialogs"></a>Cuadros de diálogo

Para crear el texto de los cuadros de diálogo de la aplicación, se aplican muchos de los mismos consejos que para escribir mensajes de error. Aunque el usuario espera que aparezcan cuadros de diálogo, interrumpen el flujo normal de la aplicación y deben ser útiles y concisos para que el usuario pueda volver a lo que estaba haciendo.

Pero lo más importante es la "llamada y respuesta" entre el título de un diálogo y sus botones. Asegúrate de que tus botones sean respuestas claras a la pregunta planteada en el título y que su formato sea coherente en toda la aplicación.

![Ejemplo de un diálogo correcto](images/writing/password_dialog.png)

## <a name="spoken-experiences"></a>Experiencias con voz

Se aplican los mismos principios generales y recomendaciones al escribir texto para experiencias con voz, como Cortana. En esas características, los principios de la buena escritura son aún más importantes, porque no se puede proporcionar a los usuarios otros elementos de diseño visual para complementar las palabras habladas.

* **Sé descriptivo**; atrae al usuario con un tono informal. Más que en cualquier otra área, resulta fundamental que una experiencia con voz resulte amable y accesible, y que sea algo a lo que los usuarios no tengan miedo de hablar.

* **Sé útil**; proporciona sugerencias alternativas cuando el usuario pida lo imposible. De manera muy similar a un mensaje de error, si algo salió mal y tu aplicación no puede completar la solicitud, debes dar al usuario una alternativa realista que pueda intentar pedir, en su lugar.

* **Sé claro y conciso**; mantén un lenguaje sencillo. Las experiencias con voz no son adecuados para frases largas o palabras complicadas.

## <a name="accessibility-and-localization"></a>Accesibilidad y localización

Tu aplicación puede llegar a un público más amplio si se crea teniendo presentes la accesibilidad y la localización. Esto es algo que no solo se puede lograr a través del texto, aunque el lenguaje sencillo y amable es un buen comienzo. Para obtener más información, consulta nuestra [información general sobre accesibilidad](https://docs.microsoft.com/windows/uwp/design/accessibility/accessibility-overview) y las [instrucciones para localización](https://docs.microsoft.com/windows/uwp/design/globalizing/globalizing-portal).

* **Sé amable y servicial**; ten en cuenta diferentes experiencias. Evita las frases que podrían no tener sentido para un público internacional y no uses palabras que hagan suposiciones sobre lo que el usuario puede y no puede hacer.

* **Sé claro y conciso**; evita palabras inusuales y especializadas cuando no sean necesarias. Cuanto más sencillo sea el texto, más fácil será de localizar.


## <a name="techniques-for-non-writers"></a>Técnicas para no escritores

No es necesario ser un escritor con formación o experiencia para proporcionar a los usuarios una buena experiencia. Elige palabras que te resulten cómodas: el usuario también se sentirá cómodo con ellas. Pero a veces, no es tan sencillo como parece. Si sigues teniendo problemas, estas técnicas pueden ayudarte. 

* Imagina que estás hablando a un amigo acerca de la aplicación. ¿Cómo les explicarías en qué consiste la aplicación? ¿Cómo hablarías de sus características o les darías instrucciones? Aún mejor, explica la aplicación a una persona real que no la haya usado todavía. 

* Imagina cómo describías una aplicación totalmente diferente. Por ejemplo, si vas a crear un juego, piensa en lo que dirías o escribirías para describir una aplicación financiera o de noticias. El contraste en el lenguaje y la estructura que usas puede darte más información sobre las palabras adecuadas para aquello sobre los que escribes realmente.

* Echa un vistazo a aplicaciones similares para obtener inspiración. 

Encontrar las palabras adecuadas es un problema que muchas personas tratan de hacer frente, así que no te sientas mal si no te resulta fácil encontrar algo que suene natural.