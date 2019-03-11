---
title: Hacer que los juegos sean accesibles
description: Aprende a hacer que los juegos sean accesibles. Usa el principio de diseño de juego inclusivo para lograr accesibilidad al juego.
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.date: 11/09/2017
ms.topic: article
keywords: Windows 10, UWP, accesibilidad, juegos
ms.localizationpriority: medium
ms.openlocfilehash: 0e1d9d25fc63dd2fbb8d258fdaee692ccdfdc911
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592940"
---
#  <a name="making-games-accessible"></a>Hacer que los juegos sean accesibles

La accesibilidad puede permitir que cada persona y cada organización del planeta consiga más, y esto se aplica a hacer que los juegos también sean más accesibles. Este artículo está diseñado para los desarrolladores, diseñadores y productores de juegos. Proporciona una introducción a las directrices de accesibilidad a juegos derivadas de varias organizaciones (enumeradas en la sección Referencia más adelante) y presenta el principio de diseño de juego inclusivo para crear juegos más accesibles.

## <a name="gaming-for-everyone"></a>Juegos para todas las edades

En Microsoft, creemos que los juegos deberían ser divertidos para todos. "Nos sentimos motivados a hacer de los juegos un entorno inclusivo para todo el mundo. Básicamente, creemos en lo que compilamos para nuestros seguidores y lo que hacemos (dentro y fuera de Microsoft) es el reflejo de lo que somos. Diseñamos el programa para reflejar los valores principales que tenemos como organización y creemos que el programa podría ocasionar cambios positivos, no solo en el área de trabajo, sino en los productos que creamos para los jugadores para los que trabajamos". ([Entrada de blog](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone) de Phil Spencer)

Queremos crear un entorno divertido, variado e inclusivo donde todo el mundo puede jugar. "Para lograr un impacto permanente es necesario un cambio cultural, uno que no se produzca de un día para el otro. Sin embargo, nuestro equipo se compromete a mejorar cada día, y a aprender juntos a detenernos al tomar decisiones y pensar en la increíble diversidad de necesidades, capacidades e intereses entre los jugadores de todo el mundo". ([Entrada de blog](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone) de Phil Spencer)

Esperamos que tu unas a nosotros en este viaje para hacer realidad los [Juegos para todas las edades](https://news.microsoft.com/gamingforeveryone). 

##  <a name="why-make-games-accessible"></a>¿Por qué hacer que los juegos sean accesibles?

### <a name="increased-gamer-base"></a>Mayor base de jugadores

En su nivel más básico, la justificación comercial de la accesibilidad es sencilla:

Número de usuarios que pueden jugar al juego x Genialidad del juego = Ventas de juegos

Si hicieras un juego increíble que es muy complicado o complejo y al que solo un puñado de personas pudiera jugar, limitarías tus ventas. De forma parecida, si crearas un juego al que no pudieran jugar los usuarios con impedimentos cognitivos, sensoriales o físicos, perderías ventas potenciales. Por ejemplo, el [19 % de las personas en los Estados Unidos tiene algún tipo de discapacidad](https://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), [un 14 % de los adultos en los Estados Unidos tienen dificultades para leer](https://nces.ed.gov/naal/estimates/overview.aspx) y [un 10 % de los hombres tiene alguna forma de discapacidad para distinguir colores](https://www.aao.org/eye-health/diseases/color-blindness-risk). Todo esto puede tener un gran impacto en los ingresos de tu título. 

Para más justificaciones empresariales, consulta [Hacer que los videojuegos sean accesibles](https://msdn.microsoft.com/library/windows/desktop/ee415219).

### <a name="better-games"></a>Mejores juegos

Crear un juego más accesible puede crear un juego mejor al final. 

Un ejemplo es los subtítulos en los juegos. Antes, los juegos apenas admitían subtítulos en sus diálogos. Actualmente, se espera que los juegos incluyan subtítulos. Este cambio no lo hicieron jugadores con discapacidades. En realidad, fue la localización, pero se popularizó entre una gran variedad de jugadores que simplemente prefirieron jugar con subtítulos porque mejoraba su experiencia de juego. Los jugadores activan los subtítulos cuando juegan con mucho ruido de fondo, tienen dificultades para oír voces con varios efectos de sonido o ruido ambiental a la vez o simplemente cuando necesitan tener el volumen bajo para evitar molestar a los demás. Los subtítulos no solo ayudaron a los jugadores a tener una mejor experiencia de juego, sino que también permiten que personas con discapacidades auditivas también jueguen.

La reasignación del mando es otra característica que lentamente se está convirtiendo en un estándar de la industria del juego por razones similares. Normalmente se ofrece como una de las ventajas para todos los jugadores. Algunos de los jugadores disfrutan personalizando sus experiencias de juego, y algunos simplemente prefieren algo diferente de lo que los diseñadores proponen. Lo que la mayoría de las personas no sabe es que la capacidad de reasignar botones en un dispositivo de entrada es también una característica de accesibilidad que fue diseñada para que un mismo juego lo pudieran usar personas con distintos tipos de discapacidades motrices y que no pueden utilizar (o les es muy difícil) ciertas áreas del mando.

Básicamente, el proceso de pensamiento usado para hacer que el juego fuera más accesible a menudo causará que el juego sea mejor porque habrás diseñado una experiencia más fácil de usar y personalizable para que los jugadores la disfruten.

### <a name="social-space-and-quality-of-life"></a>Espacio social y calidad de vida

Los videojuegos son una de las formas de entretenimiento que genera más ingresos y puede proporcionar horas de diversión. Para algunos, jugar no es solo una forma de entretenimiento, sino un escape de la cama del hospital, del dolor crónico o de una ansiedad social grave. Los jugadores se transportan a un mundo donde se convierten en los protagonistas del videojuego. A través de los juegos, pueden crear y participar en un espacio social por sí mismos que proporciona una distracción de las dificultades diarias originadas que les causan sus limitaciones y eso les da una oportunidad de comunicarse con personas con las que, de lo contrario, es posible que no interactuaran. 

Los juegos también son una cultura. Poder participar en la misma cosa de la que todos tus amigos están hablando es algo que puede ser muy valioso para la calidad de vida de una persona.

##  <a name="is-the-game-you-are-making-today-accessible"></a>¿El juego que estás haciendo hoy es accesible?

Si estás pensando por primera vez en hacer un juego accesible, estas son algunas preguntas que puedes hacerte:

* ¿Puedes completar el juego con una sola mano? 
* ¿Puede una persona normal escoger el juego y jugarlo?
* ¿Puedes jugar eficazmente al juego en un monitor o un televisor pequeño sentado a cierta distancia?
* ¿Es compatible con más de un tipo de dispositivo de entrada que puede usarse para jugar a todo el juego?
* ¿Puedes jugar al juego con el sonido desactivado?
* ¿Puedes jugar al juego con el monitor establecido en blanco y negro?
* Cuando se carga el último juego guardado después de un mes, ¿puedes averiguar fácilmente dónde estás en el juego y lo que necesitas hacer para continuar?

Si las respuestas son principalmente no, o no sabes responderlas, es el momento de dar el paso y aplicar la accesibilidad en el juego.

## <a name="defining-disability"></a>Definir discapacidad

La discapacidad se define como "un desajuste entre las necesidades de la persona y las del servicio, producto o entorno que se ofrece". ([Vídeo inclusivo](https://www.microsoft.com/design/inclusive), Microsoft.com.) Esto significa que cualquier usuario puede experimentar una discapacidad y que puede ser una condición a corto plazo o conocimiento. Imagina qué desafíos pueden encontrarse los jugadores con estos problemas cuando jueguen al juego y piensa en cómo se puede mejorar el juego pensando en ellos. A continuación se enumeran algunas discapacidades por considerar:

### <a name="vision"></a>Visual

*   Enfermedades médicas de larga duración como glaucoma, cataratas, daltonismo, miopía y retinopatía diabética.
*   Situaciones de corta duración y circunstanciales, como un monitor o pantalla de tamaño pequeño, una pantalla de baja resolución o un brillo sobre la pantalla debido a fuentes de luz como el sol en un monitor o pantalla móvil
        
### <a name="hearing"></a>Auditiva

* Enfermedades médicas de larga duración como sordera completa o pérdida parcial de la audición debido a enfermedades o a la genética
* Situaciones de corta duración y circunstanciales como ruido de fondo excesivo, baja calidad del sonido o volumen restringido para evitar molestar a los demás
        
### <a name="motor"></a>Motriz

* Enfermedades médicas de larga duración como la enfermedad de Parkinson, Esclerosis lateral amiotrófica (ELA), artritis y distrofia muscular
* Situaciones de corta duración y circunstanciales como una mano lesionada, sujetar una bebida o llevar un niño en un brazo
  
### <a name="cognitive"></a>Cognitiva

* Condiciones médicas de largo plazo como dislexia, epilepsia, trastorno por déficit de atención con hiperactividad (TDAH), demencia y amnesia
* Situaciones de corta duración y circunstanciales como el consumo de alcohol, la falta de sueño o distracciones temporales, como oír una sirena de un vehículo de emergencias que pasa cerca de la vivienda

### <a name="speech"></a>Voz

* Enfermedades médicas de larga duración como daños en las cuerdas vocales, disartria y apraxia
* Situaciones de corta duración y circunstanciales como trabajos dentales o comer y beber


## <a name="how-to-make-games-more-accessible"></a>¿Cómo hacer que los juegos sean más accesibles?

### <a name="design-shift-inclusive-game-design-approach"></a>Cambio de diseño: Enfoque de diseño de juegos inclusivo

El diseño inclusivo se centra en la creación de productos y servicios más accesible para una gama más amplia de consumidores, incluidas las personas con discapacidades.

Para tener éxito, los diseñadores de juegos de hoy necesitan pensar en más que simplemente crear los juegos que les gustan. Los diseñadores de juegos tienen que tener en cuenta cómo afectarán sus decisiones de diseño a la accesibilidad general del juego y a la jugabilidad del público general para los que están dirigidos, incluidas las personas con discapacidades.

Por tanto, los paradigmas tradicionales de diseño de juegos deben cambiar para aprovechar el concepto de diseño de juego inclusivo. El diseño de juego inclusivo significa que se intenta ir más allá del diseño de juego básico de crear diversión para un público de destino para crear personajes adicionales o modificados que incluyan una mayor variedad de jugadores. Debes conocer perfectamente las barreras de diseño en tu juego y asegurarte de que no agregas barreras innecesarias que estropearían la diversión de la experiencia.

Al identificar las diferencias, puedes optimizar el concepto de diseño original y mejorarlo. De esta manera, más personas experimentarán tu visión. Cuando, durante el proceso de diseño del juego, te tomas tu tiempo para ser más inclusivo, el +juego final se vuelve más accesible. Ningún juego puede funcionar para todos los usuarios, la definición de juego requiere que haya cierto grado de desafío, pero, si consideras la accesibilidad, puedes asegurarse de que nadie quede excluido innecesariamente.

### <a name="empower-gamers-give-gamers-options"></a>Capacite a jugadores: Conceda a jugadores opciones

Casi todas las soluciones de accesibilidad se basan en uno de estos dos principios. El primero es darles a los jugadores las opciones para personalizar su experiencia de juego. Si ya tienes una gran base de aficionados, puede que una parte importante de tu público no quiera que la experiencia cambie en alguna manera. No hay problema. Ofrece a los jugadores la capacidad de activar y desactivar esas características y haz que se puedan configurar individualmente. Tienes que permitir que la gente pueda experimentar el juego de la manera que mejor se adapte a sus propias necesidades y preferencias.

### <a name="reinforce-communicate-information-in-more-than-one-way"></a>Reforzar: Comunicar la información en más de una forma

El segundo principio es en lo que se basa el concepto de diseño universal, un enfoque único que, no solo atrae a más jugadores, sino que también mejora la experiencia de todos. Por ejemplo, una imagen vale lo mismo que un texto y un símbolo lo mismo que un color. Un daltónico no puede usar un mapa que se basa en una variedad de marcadores de colores diferentes y, además, es frustrante para todos los demás, ya que deben recordar las equivalencias correspondientes. Agregar símbolos mejora la experiencia para todos los usuarios.

### <a name="innovate-be-creative"></a>Innove: Sea creativo

Existen muchas maneras creativas de mejorar la accesibilidad de tu juego. Ponte tu traje de creatividad y aprende de otros juegos accesibles del mercado. Si ya tienes un juego existente, aprende a identificar las características actuales del juego que se podrían mejorar mientras mantienes la experiencia y la mecánica de juego principal como se diseñaron. Como se menciona anteriormente, la accesibilidad a juegos es proporcionar a los jugadores opciones para personalizar su experiencia de juego. Puede ser a través de refuerzo o comunicación de la información de más de una forma. 

Tener en cuenta la accesibilidad permite enfocar el diseño desde un ángulo de nuevo y, posiblemente, con ideas que no hubieras considerado antes. Este enfoque para diseñar ha producido conceptos interesantes y ha creado productos que tienen un gran éxito de aceptación o comercial. Algunos ejemplos son el texto predictivo, el reconocimiento de voz, las reducciones en las aceras, el altavoz, la máquina de escribir y el reconocimiento óptico de caracteres (OCR). Las ideas para estos productos proceden de aquellos que empezaron a pensar en soluciones de accesibilidad.

### <a name="adopt-quality-means-accessible-features"></a>Adoptar: Calidad significa características accesibles

La accesibilidad es una medida de calidad. Debe ser un requisito incluido y no una opción interesante. Por ejemplo, "Permitir la adaptación minimapa para los daltónicos" no se considera un elemento de trabajo de prioridad baja del que te encargarás si te sobra tiempo. Si no se realiza este elemento de trabajo, la opción minimapa está incompleta y no se puede enviar.

### <a name="evangelize-make-accessibility-a-priority-in-your-game-studio"></a>Dé a conocer: Hacer una prioridad de accesibilidad en la game studio

El desarrollo de juegos siempre se realiza en una línea de tiempo ajustada, así que dar prioridad a la accesibilidad ayudará a facilitar el proceso. Una forma de hacerlo es diseñar desde el principio teniendo en cuenta la accesibilidad. Cuanto antes consideres la accesibilidad, más fácil y barata será. 

Comparte tus conocimientos sobre la accesibilidad con tu equipo, habla de las ventajas comerciales y destruye los mitos (como que no beneficia a muchas personas, que atenúa tu método de trabajo y que es difícil y caro de implementar).

### <a name="review-constantly-evaluate-your-game"></a>Revisión: Evalúe constantemente su juego

Durante el desarrollo, puedes realizar un proceso de revisión para asegurarte de que se esté pensando en la accesibilidad en cada paso del camino. Haz una lista de comprobación como la siguiente para ayudar a tu equipo a evaluar constantemente si lo que se está creando es accesible o no.

| Checklist                                         | Características de accesibilidad                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Animaciones del juego                                | Se probaron los subtítulos y la fotosensibilidad                                                                           |
| Ilustraciones generales (gráficos 2D y 3D)              | Opciones y colores fáciles de usar para daltónicos, no se depende totalmente del color para identificar y se usan también formas y patrones|
| Pantalla de inicio, menú de configuración y otros menús       | Capacidad de leer opciones en voz alta y de recordar las opciones de la configuración, método de entrada de mando alternativo, tamaño de fuente de la interfaz de usuario ajustable  |
| Experiencia de juego                                          | Niveles de dificultad ajustables, subtítulos, buenos elementos visuales y comentarios de audio para el jugador                           |
| Pantalla HUD                                       | Posición de la pantalla ajustable, tamaño de fuente ajustable, opción fácil de usar para daltónicos                                                  |        
| Entrada del mando                                     | Controles que se puedan reasignar para dispositivo de entrada, compatibilidad con mandos personalizados, entrada simplificada permitida para el juego                               |        

### <a name="playtest-and-iterate-get-gamers-feedback"></a>Playtest y recorrer en iteración: Obtener comentarios de jugadores

Al organizar sesiones de prueba de juegos, invita a probadores con discapacidades para las que tu juego está diseñado y haz que jueguen. Recuerda que tienes que incluir preguntas de accesibilidad en los cuestionarios de pruebas Beta. Las asociaciones locales de discapacitados son una estupenda fuente de participantes. Observa cómo juegan y obtén comentarios de ellos. Averigua qué cambios deben realizarse para mejorar el juego.

Usa las redes sociales y el foro de tu juego para escuchar comentarios sobre las características de accesibilidad más importantes y cómo implementarlas. 

### <a name="shout-it-out-let-the-world-know-your-game-is-accessible"></a>Gritarlo: Informar del mundo a que su juego es accesible

Los usuarios querrán saber si los jugadores con discapacidades podrán jugar a tu juego. Expón claramente la accesibilidad del juego en el sitio web, en las notas de prensa y en la caja del juego, a fin de garantizar que los usuarios sepan qué esperar cuando compren tu juego. Recuerda hacer accesibles también tu sitio web y todos los canales de ventas del juego. Más importante aún, llega a la comunidad de juegos de accesibilidad e infórmales sobre tu juego.

## <a name="game-accessibility-features"></a>Características de accesibilidad a juegos

En esta sección se describen algunas características que pueden hacer que el juego sea más accesible. Estas características se derivan de instrucciones que se tomaron de las [Directrices de accesibilidad a juegos](https://gameaccessibilityguidelines.com/), que representan los resultados de un grupo de colaboración de estudios, especialistas y academias. Para obtener más información, consulta las [Directrices de accesibilidad a juegos](https://gameaccessibilityguidelines.com/). 

### <a name="colorblind-friendly-graphics-and-user-interface"></a>Gráficos e interfaz de usuario fáciles de usar para daltónicos

La retina del ojo tiene dos tipos de células sensible a la luz: los conos para ver donde está luz y los bastones para ver cuando hay poca luz. 

Hay tres tipos de conos (rojos, verdes y azules) que nos permiten ver colores correctamente. El daltonismo se produce cuando uno o varios de estos tres tipos de conos no funciona según lo esperado. El grado de daltonismo puede oscilar desde una percepción de color casi normal con una sensibilidad reducida hacia la luz roja, verde o azul a una completa incapacidad para percibir ningún color. 

Dado que es menos común tener sensibilidad reducida a la luz azul, al diseñar para daltónicos, la selección de colores está dirigida a personas con daltonismo al rojo o al verde:
 
  + Usa combinaciones de colores que puedan diferenciar las personas con daltonismo al rojo o al verde:
  
    * Colores que aparecen similar: Todas las sombras de rojo y verde como brown y naranja
    * Colores que destaquen: Azul y amarillo
    
  + No confíes únicamente en el color para comunicar o distinguir los objetos del juego. Usa también formas y patrones.
  + Si tienes que depender solo de colores, combina los preestablecidos con una selección libre de colores, de modo que se pueda personalizar por completo por los jugadores que lo necesiten y que no suponga trabajo adicional para los que no lo necesitan.
  + Usa un simulador de daltonismo a fin de probar los diseños y poder verlos a través de los ojos de un daltónico. Esto puede ayudarte a evitar problemas comunes de contraste. [Color Oracle](https://www.colororacle.org) es un simulador de daltonismo gratuito que puede emular los tres tipos más comunes de deficiencia en la visión del color: deuteranopía, protanopia y tritanopia.
  
### <a name="closed-captioning-and-subtitles"></a>Subtítulos

Al diseñar los subtítulos de tu juego, el objetivo es proporcionar subtítulos legibles como una opción para que también se pueda disfrutar de tu juego sin sonido. Debería ser posible tener componentes del juego como diálogos, audio y efectos de sonido que se muestren como texto en pantalla.

Estas son algunas directrices básicas a tener en cuenta cuando se diseñan los subtítulos:

*   Selecciona una fuente legible simple.
*   Selecciona el tamaño de fuente lo suficientemente grande o ten en cuenta la opción de un tamaño de fuente ajustable para obtener una mayor flexibilidad. (El tamaño de fuente ideal depende del tamaño de la pantalla o la distancia a la pantalla, entre otros).
*   Crea un alto contraste entre el color de fondo y el de la fuente. Usa un diseño fuerte y sombras para el texto. Usa una superposición de fondo oscura para los títulos y recuerda ofrecer opciones para activarlos o desactivarlos. (Para obtener más información, consulta [Información acerca de la relación de contraste](https://msdn.microsoft.com/windows/uwp/accessibility/accessible-text-requirements).)
* Muestra frases cortas en pantalla, como máximo 38 caracteres por línea y un máximo de 2 o 3 líneas en cada momento. (Recuerda no revelar contenido del juego mostrando el texto antes de que ocurra el evento.)
*   Distingue qué hace el sonido o quién habla. (Ejemplo: "Daniel: Hola!")
*   Proporciona la opción de activar y desactivar los subtítulos. (Característica adicional: Capacidad de seleccionar se muestra la cantidad de sonido información basada en importancia).

### <a name="game-chat-transcription"></a>Transcripción del chat de juego

Si el título permite a los jugadores comunicarse por voz y enviar mensajes de texto entre sí, las funciones de texto a voz y voz a texto deben estar disponibles como una opción.

Las personas que no tienen micrófonos conectados a su dispositivo de juegos aun pueden tener una conversación con alguien que está hablando. Pueden escribir texto en la ventana del chat y convertir esos mensajes en voz. También permite que alguien con dificultades auditivas pueda leer la transcripción de los mensajes de texto de la persona con la que están hablando.

Para desarrolladores ID@Xbox y asociados administrados del programa, las funciones de texto a voz y voz a texto están disponibles como parte de las [funciones de accesibilidad Game Chat 2](../xbox-live/multiplayer/chat/using-game-chat-2.md#accessibility) en el servicio de Xbox Live. Para obtener más información, consulta [Game Chat 2 Overview](../xbox-live/multiplayer/chat/game-chat-2-overview.md).

### <a name="sound-feedback"></a>Información de sonido

El sonido proporciona información al jugador, aparte de la información visual. Un buen diseño del audio del juego puede mejorar la accesibilidad para los jugadores con dificultades visuales. A continuación se enumeran otras indicaciones por considerar:

*   Usa pistas de audio 3D para proporcionar información espacial adicional.
* Separa los controles de volumen de la música, la voz y los efectos de sonido.
*   Diseña una voz que proporcione información significativa para los jugadores. (Ejemplo: "Se aproximan los enemigos" vs. "Enemigos entran desde la puerta trasera.")
*   Asegúrate de que la voz se habla a una velocidad razonable y proporciona un control de velocidad para obtener una mejor accesibilidad.

### <a name="fully-mappable-controls"></a>Controles completamente asignables

Hay empresas y organizaciones, como [Special Effect](https://www.specialeffect.org.uk/), que diseña mandos de juego personalizados que pueden usarse con distintos sistemas de juego como Windows y Xbox One. Esta personalización permite a los usuarios con distintas formas de discapacidad jugar a juegos a los que, de lo contrario, no podrían jugar. Para obtener más información sobre personas que pueden jugar a juegos de forma independiente gracias a los mandos personalizados, consulta [a quién ayudaron](https://www.specialeffect.org.uk/who-we-helped).

Como desarrollador de juegos, puedes hacer que tu juego sea más accesible permitiendo que los controles se puedan asignar por completo para que los jugadores tengan la opción de conectar sus mandos personalizados y asignar las teclas según sus necesidades.

Tener controles completamente asignables también beneficia a las personas que usan controladores estándar. Los jugadores pueden crear un diseño que se adapte a sus necesidades individuales.

Los mandos estándar de Xbox One y Xbox Elite ofrecen la personalización de los mandos para jugar con precisión. Para utilizar al máximo sus funcionalidades de asignación, __se recomienda que los desarrolladores incluyan la reasignación directamente en el juego__. Para obtener más información, consulta [Xbox One](https://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) y [Xbox Elite](https://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### <a name="wider-selection-of-difficulty-levels"></a>Selección más amplia de niveles de dificultad

Los videojuegos ofrecen entretenimiento. El desafío para los desarrolladores de juegos es ajustar el nivel de dificultad para que el jugador experimente el nivel de desafío correcto. En primer lugar, no todos los jugadores tienen el mismo nivel y capacidad, así que diseñar una selección más amplia de opciones de dificultad aumenta la posibilidad de proporcionar a los jugadores la dificultad adecuada del desafío. Al mismo tiempo, esta selección más amplia también hace que tu videojuego sea más accesible porque podría hacer que más personas con discapacidades jueguen al juego. Recuerda, los jugadores quieren superar desafíos en un juego y recibir una recompensa por ello. No quieren un juego al que no puedan ganar.

Retocar el nivel de dificultad del juego es un proceso delicado. Si está muy fácil, es posible que los jugadores se aburran. Si es demasiado difícil, es posible que los jugadores o lo dejen y no vuelvan a jugar a partir de un punto. El proceso de equilibrio es arte y ciencia. Existen muchas formas de realizar un nivel de juego que tiene la cantidad adecuada de desafío. Algunos juegos ofrecen entradas simplificadas, como una opción de juego de pulsar solo un botón, una opción rebobinar y reproducir para hacer que el juego sea más tolerable o menos enemigos y más débiles para que sea más fácil seguir adelante después de varios intentos.

### <a name="photosensitivity-epilepsy-testing"></a>Pruebas de epilepsia fotosensible

La epilepsia fotosensible es una enfermedad en la que se producen ataques a causa de estímulos visuales, incluyendo las exposiciones a luces parpadeantes o a determinadas formas y patrones visuales en movimiento. Esto ocurre en aproximadamente el tres por ciento de la población y es más común en niños y adolescentes. En números absolutos, esto supone [1 de cada 4000 personas con edades entre los 5 y los 24 años](https://www.epilepsy.com/information/professionals/about-epilepsy-seizures/reflex-seizures-and-related-epileptic-syndromes-3).

Hay muchos factores que pueden causar una reacción fotosensible al jugar a videojuegos, incluida la duración del juego, la frecuencia del parpadeo, la intensidad de la luz, el contraste del fondo y la luz, la distancia entre la pantalla y el jugador y la longitud de onda de la luz.

Muchas personas descubren que tienen epilepsia cuando sufren un ataque. Se dan casos de jugadores que tienen sus primeros ataques a través de los videojuegos y esto puede provocar daños físicos. Como desarrollador, te ofrecemos algunas sugerencias para el diseño de juegos que reduzcan el riesgo de sufrir ataques por epilepsia fotosensible.

Evita lo siguiente:
* Luces parpadeantes con una frecuencia de 5 a 30 parpadeos por segundo (hercios), porque es más probable que las luces parpadeantes en ese intervalo causen ataques.
* Todas las secuencias de imágenes parpadeantes que duren más de 5 segundos
* Más de tres parpadeos en un solo segundo que abarquen el 25 % o más de la pantalla
* Patrones repetidos en movimiento o texto uniforme, que abarque el 25 % o más de la pantalla
* Patrones repetidos estáticos o texto uniforme, que abarque el 40 % o más de la pantalla
* Un cambio grande y repentino en brillo/contraste (incluidos los cortes rápidos) o desde/hacia el color rojo
* Más de cinco bandas repetidas de contraste alto separadas uniformemente (filas o columnas como cuadrículas y dameros) que puedan estar compuestos de elementos normales más pequeños como polkadots
* Más de cinco líneas de texto con formato de solo mayúsculas, sin demasiado espacio entre las letras y con un interlineado de la misma altura que las propias líneas, ya que esto es, en efecto, una imagen de columnas de contraste alto que se alternan de forma regular.

Usa un sistema automatizado para buscar en el juego estímulos que pudieran causar epilepsia fotosensible. (Ejemplo: [La prueba de Harding](https://www.hardingtest.com/index.php?page=test) y [Harding Flash y analizador de patrón (FPA) G2](https://www.hardingfpa.com/harding-fpa-for-games/) desarrollado por Cambridge Research sistema Ltd y profesor Graham Harding.) 

Incluir la opción **Parpadeo On/Off** como una opción de configuración y establecer el **Parpadeo** en **Off** de manera predeterminada. Al hacer esto, protegerá a los jugadores que todavía no saben que pueden sufrir ataques.

Diseña pausas entre los niveles del juego y anima a los jugadores a tomar un descanso y a no jugar sin interrupciones.

## <a name="other-accessibility-resources"></a>Otros recursos de accesibilidad

Estos son algunos sitios externos que proporcionan información adicional sobre accesibilidad a juegos.

### <a name="game-accessibility-guidelines"></a>Directrices sobre accesibilidad a juegos
* [Directrices de accesibilidad de juegos](https://gameaccessibilityguidelines.com/)
* [Directrices de AbleGamers Foundation](https://www.includification.com/)
* [Diseñar juegos universalmente accesible (UA)](https://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### <a name="custom-input-controllers"></a>Mandos de entrada personalizados
* [Efecto especial](https://www.specialeffect.org.uk/)
* [Combate WAR activa](https://www.warfighterengaged.org/)

## <a name="references-used"></a>Referencias usadas
* [Directrices de accesibilidad de juegos](https://gameaccessibilityguidelines.com/)
* [Directrices de AbleGamers Foundation](https://www.includification.com/)
* [Reconocimiento de color ciegos, una empresa de interés de la Comunidad](https://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [Cómo hacer subtítulos bien de un artículo de blog sobre Gamasutra por Ian Hamilton](https://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Innovación para todos los programas](https://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)
* [Foundation epilepsia](https://www.epilepsy.com/)

## <a name="related-links"></a>Vínculos relacionados
* [Diseño inclusivo](https://www.microsoft.com/design/inclusive)
* [Centro de accesibilidad de Microsoft para desarrolladores](https://developer.microsoft.com/windows/accessible-apps)
* [Desarrollo de aplicaciones UWP accesible](https://msdn.microsoft.com/windows/uwp/accessibility/accessibility)
* [Libro electrónico de Software de accesibilidad de ingeniería](https://www.microsoft.com/download/details.aspx?id=19262)
