---
title: Hacer que los juegos sean accesibles
description: Aprende a hacer que los juegos sean accesibles. Usa el principio de diseño de juego inclusivo para lograr accesibilidad al juego.
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.date: 11/09/2017
ms.topic: article
keywords: Windows 10, UWP, accesibilidad, juegos
ms.localizationpriority: medium
ms.openlocfilehash: f90f976f696d5c49e7f772627bbb7d0e3e3d0908
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159439"
---
#  <a name="making-games-accessible"></a>Hacer que los juegos sean accesibles

La accesibilidad puede permitir que cada persona y cada organización del planeta consiga más, y esto se aplica a hacer que los juegos también sean más accesibles. Este artículo está escrito para desarrolladores de juegos, diseñadores de juegos y productores. Proporciona una introducción a las directrices de accesibilidad a juegos derivadas de varias organizaciones (enumeradas en la sección Referencia más adelante) y presenta el principio de diseño de juego inclusivo para crear juegos más accesibles.

## <a name="gaming-for-everyone"></a>Juegos para todas las edades

En Microsoft, creemos que los juegos deben ser divertidos para todos. Nos sentimos obligados a hacer más para jugar un entorno inclusivo que adoptó todo el mundo. Creemos fundamentalmente que lo que creamos para nuestros aficionados y el modo en que se muestran, dentro y fuera de las paredes de Microsoft, es un reflejo de quién somos. Diseñamos el programa para reflejar los valores principales que tenemos como organización y creemos que el programa podría producir un cambio positivo, no solo en nuestro lugar de trabajo, sino en los productos que creamos para los jugadores que ofrecemos. ([Entrada de blog](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone/) de Phil Spencer)

Queremos crear un entorno divertido, diverso y inclusivo en el que todos los usuarios puedan jugar. "Para tener realmente un impacto duradero, es necesario un cambio de referencia cultural, uno que no pasa por la noche. Sin embargo, nuestro equipo se compromete a ayudarnos a mejorar cada día, para enseñarlo a poner en pausa en nuestro proceso de toma de decisiones y pensar en la sorprendente diversidad de necesidades, capacidades e intereses de los jugadores de todo el mundo ". ([Entrada de blog](https://blogs.microsoft.com/blog/2016/06/13/gaming-for-everyone/) de Phil Spencer)

Esperamos que se una a nuestro viaje para que se produzcan [juegos para todo el mundo](https://news.microsoft.com/gamingforeveryone/) . 

##  <a name="why-make-games-accessible"></a>¿Por qué hacer que los juegos sean accesibles?

### <a name="increased-gamer-base"></a>Mayor base de jugadores

En su nivel más básico, la justificación comercial de la accesibilidad es sencilla:

Número de usuarios que pueden jugar al juego x Genialidad del juego = Ventas de juegos

Si hicieras un juego increíble que es muy complicado o complejo y al que solo un puñado de personas pudiera jugar, limitarías tus ventas. De forma parecida, si crearas un juego al que no pudieran jugar los usuarios con impedimentos cognitivos, sensoriales o físicos, perderías ventas potenciales. Teniendo en cuenta que, por ejemplo, [el 19% de las personas del Estados Unidos tienen alguna forma de discapacidad](https://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), [el 14% de los adultos en Estados Unidos tienen dificultades de lectura](https://nces.ed.gov/naal/estimates/overview.aspx)y el [10% de los hombres](https://www.aao.org/eye-health/diseases/color-blindness-risk)es un porcentaje de la deficiencia de la visión del color, lo que puede tener un gran impacto en los ingresos del título. 

Para más justificaciones empresariales, consulta [Hacer que los videojuegos sean accesibles](/windows/desktop/DxTechArts/accessibility-best-practices).

### <a name="better-games"></a>Mejores juegos

Crear un juego más accesible puede crear un juego mejor al final. 

Un ejemplo es los subtítulos en los juegos. Antes, los juegos apenas admitían subtítulos en sus diálogos. Actualmente, se espera que los juegos incluyan subtítulos. Este cambio no lo hicieron jugadores con discapacidades. En su lugar, se encargó de la localización, pero resultó popular con una amplia gama de jugadores que simplemente prefieren jugar con subtítulos, ya que mejoró la experiencia de juego. Los jugadores activan los subtítulos cuando juegan con mucho ruido de fondo, tienen dificultades para oír voces con varios efectos de sonido o ruido ambiental a la vez o simplemente cuando necesitan tener el volumen bajo para evitar molestar a los demás. Los subtítulos no solo ayudaron a los jugadores a tener una mejor experiencia de juego, sino que también permiten que personas con discapacidades auditivas también jueguen.

La reasignación del mando es otra característica que lentamente se está convirtiendo en un estándar de la industria del juego por razones similares. Se suele ofrecer como una ventaja a todos los reproductores. Algunos jugadores disfrutan de la personalización de sus experiencias de juego y otras simplemente prefieren algo diferente de lo que los diseñadores tenían en mente. Lo que la mayoría de los usuarios no se ha observado es que la capacidad de reasignar botones en un dispositivo de entrada también es una característica de accesibilidad que se diseñó para que un juego sea reproducible para personas con distintos tipos de discapacidades del motor, que no pueden o no resultan difíciles de manejar ciertas áreas del controlador.

Básicamente, el proceso de pensamiento usado para hacer que el juego fuera más accesible a menudo causará que el juego sea mejor porque habrás diseñado una experiencia más fácil de usar y personalizable para que los jugadores la disfruten.

### <a name="social-space-and-quality-of-life"></a>Espacio social y calidad de vida

Los juegos de vídeo son una de las formas más elevadas de entretenimiento y juegos que pueden proporcionar horas de alegría. Para algunos, jugar no es solo una forma de entretenimiento, sino un escape de la cama del hospital, del dolor crónico o de una ansiedad social grave. Los jugadores se transportan a un mundo donde se convierten en los protagonistas del videojuego. A través de los juegos, pueden crear y participar en un espacio social por sí mismos que proporciona una distracción de las dificultades diarias originadas que les causan sus limitaciones y eso les da una oportunidad de comunicarse con personas con las que, de lo contrario, es posible que no interactuaran. 

El juego también es una referencia cultural. Ser capaz de formar parte de lo mismo que todos sus amigos hablan es algo que puede ser enormemente valioso para la calidad de vida de alguien.

##  <a name="is-the-game-you-are-making-today-accessible"></a>¿El juego que estás haciendo hoy es accesible?

Si estás pensando por primera vez en hacer un juego accesible, estas son algunas preguntas que puedes hacerte:

* ¿Puedes completar el juego con una sola mano? 
* ¿Puede una persona normal escoger el juego y jugarlo?
* ¿Puedes jugar eficazmente al juego en un monitor o un televisor pequeño sentado a cierta distancia?
* ¿Es compatible con más de un tipo de dispositivo de entrada que puede usarse para jugar a todo el juego?
* ¿Puedes jugar al juego con el sonido desactivado?
* ¿Puedes jugar al juego con el monitor establecido en blanco y negro?
* Al cargar el último juego guardado después de un mes, ¿puede averiguar fácilmente Dónde está en el juego y saber lo que debe hacer para progresar?

Si las respuestas son principalmente no, o no sabes responderlas, es el momento de dar el paso y aplicar la accesibilidad en el juego.

## <a name="defining-disability"></a>Definir discapacidad

La discapacidad se define como "un desajuste entre las necesidades de la persona y las del servicio, producto o entorno que se ofrece". ([Vídeo inclusivo](https://www.microsoft.com/design/inclusive/), Microsoft.com.) Esto significa que cualquier persona puede experimentar una discapacidad y que puede ser un problema breve o circunstancial. Imagina qué desafíos pueden encontrarse los jugadores con estos problemas cuando jueguen al juego y piensa en cómo se puede mejorar el juego pensando en ellos. A continuación se enumeran algunas discapacidades por considerar:

### <a name="vision"></a>Visión

*   Enfermedades médicas de larga duración como glaucoma, cataratas, daltonismo, miopía y retinopatía diabética.
*   Condiciones de situación a corto plazo, como un pequeño monitor o tamaño de pantalla, una pantalla de baja resolución o un brillo de la pantalla debido a fuentes brillantes de luz como el sol en un monitor o una pantalla móvil
        
### <a name="hearing"></a>Oído

* Enfermedades médicas de larga duración como sordera completa o pérdida parcial de la audición debido a enfermedades o a la genética
* Condiciones de situación a corto plazo, como ruido de fondo excesivo, calidad de audio de baja calidad o volumen restringido para evitar molestar a otras personas
        
### <a name="motor"></a>Motriz

* Enfermedades médicas de larga duración como la enfermedad de Parkinson, Esclerosis lateral amiotrófica (ELA), artritis y distrofia muscular
* Situaciones de corta duración y circunstanciales como una mano lesionada, sujetar una bebida o llevar un niño en un brazo
  
### <a name="cognitive"></a>Cognitivo

* Condiciones médicas de largo plazo como dislexia, epilepsia, trastorno por déficit de atención con hiperactividad (TDAH), demencia y amnesia
* Condiciones de situación a corto plazo, como el consumo de alcohol, falta de suspensión o distracciones temporales como Siren desde un vehículo de emergencia que conduce por la casa

### <a name="speech"></a>Voz

* Enfermedades médicas de larga duración como daños en las cuerdas vocales, disartria y apraxia
* Situaciones de corta duración y circunstanciales como trabajos dentales o comer y beber


## <a name="how-to-make-games-more-accessible"></a>¿Cómo hacer que los juegos sean más accesibles?

### <a name="design-shift-inclusive-game-design-approach"></a>Cambio de diseño: enfoque de diseño de juego inclusivo

El diseño inclusivo se centra en la creación de productos y servicios más accesible para una gama más amplia de consumidores, incluidas las personas con discapacidades.

Para que se realicen correctamente, los diseñadores de juegos de hoy en día deben pensar más allá de crear juegos que disfrutan. Los diseñadores de juegos deben tener en cuenta la forma en que sus decisiones de diseño afectan a la accesibilidad general del juego. la capacidad de reproducción del juego para toda la audiencia de destino, incluidas las con discapacidades.

Por tanto, los paradigmas tradicionales de diseño de juegos deben cambiar para aprovechar el concepto de diseño de juego inclusivo. El diseño de juego inclusivo significa que se intenta ir más allá del diseño de juego básico de crear diversión para un público de destino para crear personajes adicionales o modificados que incluyan una mayor variedad de jugadores. Debe ser consciente del diseño de las barreras del juego y asegurarse de que no agregan barreras innecesarias que Diviértase de la experiencia deseada.

Mediante la identificación de brechas, puede optimizar, iterar en el concepto de diseño original y mejorarlo, lo que permite a más personas experimentar su visión. Cuando, durante el proceso de diseño del juego, te tomas tu tiempo para ser más inclusivo, el +juego final se vuelve más accesible. Ningún juego puede funcionar en todo el mundo, la definición del juego requiere que haya cierto grado de desafío, pero a través de la accesibilidad puede asegurarse de que nadie se excluya innecesariamente.

### <a name="empower-gamers-give-gamers-options"></a>Potencia a los jugadores: dales opciones

Casi todas las soluciones de accesibilidad se refieren a uno de dos principios. La primera es ofrecer a los jugadores las opciones para personalizar su experiencia de juego. Si ya tienes una gran base de aficionados, puede que una parte importante de tu público no quiera que la experiencia cambie en alguna manera. No hay problema. Ofrece a los jugadores la capacidad de activar y desactivar esas características y haz que se puedan configurar individualmente. Debe permitir que los usuarios experimenten el juego de la forma que mejor se adapte a sus propias necesidades y preferencias.

### <a name="reinforce-communicate-information-in-more-than-one-way"></a>Reforzar: comunicar información de más de una manera

El segundo principio es el lugar en el que entra el concepto de diseño universal, un enfoque único que no solo aporta más reproductores, sino que también mejora la experiencia de todos. Por ejemplo, una imagen, así como texto, un símbolo y color. Un mapa que se basa en un intervalo de distintos marcadores coloreados no es solo imposible para los jugadores de color ciego, sino que también es frustrante para todos los usuarios que deben recordar lo que es todo. Agregar símbolos hace que sea una mejor experiencia para todos.

### <a name="innovate-be-creative"></a>Innova: sé creativo

Existen muchas maneras creativas de mejorar la accesibilidad de tu juego. Ponte tu traje de creatividad y aprende de otros juegos accesibles del mercado. Si ya tienes un juego existente, aprende a identificar las características actuales del juego que se podrían mejorar mientras mantienes la experiencia y la mecánica de juego principal como se diseñaron. Como se menciona anteriormente, la accesibilidad a juegos es proporcionar a los jugadores opciones para personalizar su experiencia de juego. Podría ser a través del refuerzo o la comunicación de información de más de una manera. 

Tener en cuenta la accesibilidad le permite enfocar el diseño desde un nuevo ángulo y posiblemente las ideas que no hubiera imaginado de otra manera. Este enfoque de diseño no solo resultó en conceptos interesantes, sino que ha creado productos que tienen una adopción ampliada o un éxito comercial de mercado masivo. Algunos ejemplos son el texto predictivo, el reconocimiento de voz, los cortes de frenado, el altavoz, la mecanografía y el reconocimiento óptico de caracteres (OCR). Las ideas para estos productos provienen de aquellos que empezaron a pensar en soluciones de accesibilidad.

### <a name="adopt-quality-means-accessible-features"></a>Adopción: la calidad significa características accesibles

La accesibilidad es una medida de calidad. Debe ser un requisito de característica y no un elemento de trabajo adecuado. Por ejemplo, "adapte minimapa for colourblindness" no se considera un elemento de trabajo de prioridad baja al que se obtiene si tiene tiempo adicional. Si no se realiza este elemento de trabajo, simplemente significa que la característica minimapa completa está incompleta y no se puede enviar.

### <a name="evangelize-make-accessibility-a-priority-in-your-game-studio"></a>Evangeliza: haz que la accesibilidad sea una prioridad en tu estudio de juegos

El desarrollo de juegos siempre se realiza en una línea de tiempo ajustada, así que dar prioridad a la accesibilidad ayudará a facilitar el proceso. Una forma de hacerlo es diseñar desde el principio teniendo en cuenta la accesibilidad. Antes de considerar la accesibilidad, más fácil y más barato se convierte en. 

Comparta sus conocimientos sobre accesibilidad con su equipo, comparta las justificaciones empresariales y Dispel los malentendidos habituales, ya que no beneficia a muchas personas, diluye su mecánico y es difícil de implementar.

### <a name="review-constantly-evaluate-your-game"></a>Revisa: evalúa constantemente tu juego

Durante el desarrollo, puedes realizar un proceso de revisión para asegurarte de que se esté pensando en la accesibilidad en cada paso del camino. Haz una lista de comprobación como la siguiente para ayudar a tu equipo a evaluar constantemente si lo que se está creando es accesible o no.

| Lista de comprobación                                         | Características de accesibilidad                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Animaciones del juego                                | Se probaron los subtítulos y la fotosensibilidad                                                                           |
| Ilustraciones generales (gráficos 2D y 3D)              | Colores y opciones invidentes de color, no dependientes completamente del color para la identificación, sino también usar formas y patrones|
| Pantalla de inicio, menú de configuración y otros menús       | Capacidad de leer opciones en voz alta y de recordar las opciones de la configuración, método de entrada de mando alternativo, tamaño de fuente de la interfaz de usuario ajustable  |
| Experiencia de juego                                          | Niveles de dificultad ajustables, subtítulos, buenos elementos visuales y comentarios de audio para el jugador                           |
| Pantalla HUD                                       | Posición de pantalla ajustable, tamaño de fuente ajustable, opción compatible con daltonismo                                                  |        
| Entrada del mando                                     | Controles que se puedan reasignar para dispositivo de entrada, compatibilidad con mandos personalizados, entrada simplificada permitida para el juego                               |        

### <a name="playtest-and-iterate-get-gamers-feedback"></a>Probar e iterar: obtener comentarios de los jugadores

Al organizar sesiones de prueba de juegos, invita a probadores con discapacidades para las que tu juego está diseñado y haz que jueguen. No olvide incluir preguntas de accesibilidad en la prueba beta questionaires. Los grupos de discapacidad local son una buena fuente de participantes. Observa cómo juegan y obtén comentarios de ellos. Averigua qué cambios deben realizarse para mejorar el juego.

Use medios sociales y el foro del juego para escuchar información sobre las características de accesibilidad más importantes y cómo deben implementarse. 

### <a name="shout-it-out-let-the-world-know-your-game-is-accessible"></a>Anúncialo: haz saber al mundo que tu juego es accesible

Los usuarios querrán saber si los jugadores con discapacidades podrán jugar a tu juego. Indique claramente la accesibilidad del juego en el sitio web del juego, presione versiones y empaquete para asegurarse de que los consumidores sepan qué esperar cuando compren su juego. Recuerda hacer accesibles también tu sitio web y todos los canales de ventas del juego. Más importante aún, llega a la comunidad de juegos de accesibilidad e infórmales sobre tu juego.

## <a name="game-accessibility-features"></a>Características de accesibilidad a juegos

En esta sección se describen algunas características que pueden hacer que el juego sea más accesible. Estas características se derivan de las directrices tomadas en el sitio web de [directrices de accesibilidad de juegos](http://gameaccessibilityguidelines.com/) . Ese recurso representa las conclusiones de un grupo de estudios, especialistas y académicos.

### <a name="color-blind-friendly-graphics-and-user-interface"></a>Interfaz de usuario y gráficos descriptivos para colores ciegos

La retina del ojo tiene dos tipos de células sensible a la luz: los conos para ver donde está luz y los bastones para ver cuando hay poca luz. 

Hay tres tipos de conos (rojos, verdes y azules) que nos permiten ver colores correctamente. La persiana de color se produce cuando uno o varios de estos tres tipos de conos claros no funcionan según lo esperado. El grado de daltonización del color puede oscilar entre la claridad de color casi normal con una sensibilidad reducida hacia la luz roja, verde o azul, hasta una incapacidad total de percibir cualquier color. 

Dado que es menos común tener una sensibilidad reducida a la luz azul, al diseñar para el color ciego, la selección de colores está orientada hacia las personas que están desvidentes en rojo o verde:
 
  + Use combinaciones de colores que se pueden diferenciar por personas con daltonismo de color rojo/verde:
  
    * Colores que parecen similares: Todos los tonos de rojo y verde, incluidos el marrón y el naranja
    * Colores que destacan: Azul y amarillo
    
  + No confíe únicamente en el color para comunicar o distinguir objetos de juego. Use también formas y patrones.
  + Si solo tiene que basarse en colores, combine los valores preestablecidos con una selección libre de colores para que los reproductores que los necesiten y no creen trabajo adicional para los jugadores no los necesiten.
  + Use un simulador ciego de color para probar los diseños de modo que pueda ver los diseños a través de los ojos de color ciego. Esto puede ayudarle a evitar problemas de contraste comunes. [Color Oracle](https://www.colororacle.org) es un simulador ciego de color gratuito que puede simular los tres tipos más comunes de deficiencias de la visión de color: deuteranopia, Protanopia y tritanopia.
  
### <a name="closed-captioning-and-subtitles"></a>Subtítulos

Al diseñar los subtítulos de tu juego, el objetivo es proporcionar subtítulos legibles como una opción para que también se pueda disfrutar de tu juego sin sonido. Debería ser posible tener componentes del juego como diálogos, audio y efectos de sonido que se muestren como texto en pantalla.

Estas son algunas directrices básicas a tener en cuenta cuando se diseñan los subtítulos:

*   Selecciona una fuente legible simple.
*   Selecciona el tamaño de fuente lo suficientemente grande o ten en cuenta la opción de un tamaño de fuente ajustable para obtener una mayor flexibilidad. (El tamaño de fuente ideal depende del tamaño de la pantalla o la distancia a la pantalla, entre otros).
*   Crea un alto contraste entre el color de fondo y el de la fuente. Utilice contorno seguro y sombras para el texto. Use una superposición de fondo oscuro para las leyendas y recuerde proporcionar opciones para que se active o desactive. (Para obtener más información, consulta [Información acerca de la relación de contraste](../design/accessibility/accessible-text-requirements.md).)
* Muestra frases cortas en la pantalla, con un máximo de 38 caracteres por línea y un máximo de 2-3 líneas al mismo tiempo. (Recuerda no revelar contenido del juego mostrando el texto antes de que ocurra el evento.)
*   Distingue qué hace el sonido o quién habla. (Ejemplo: "Daniel: ¡Hola!")
*   Proporciona la opción de activar y desactivar los subtítulos. (Característica adicional: capacidad de seleccionar cuánta información de sonido se muestra en función de la importancia.)

### <a name="game-chat-transcription"></a>Transcripción de chat de juegos

Si su título permite a los jugadores comunicarse mediante voz y enviar mensajes de texto entre sí, las funcionalidades de texto a voz y de voz a texto deben estar disponibles como opción.

Las personas que no tienen micrófonos conectados a su dispositivo de juegos pueden seguir teniendo una conversación de voz con alguien que está hablando. Pueden escribir texto en la ventana de chat y hacer que los mensajes se conviertan en voz. También permite a alguien que no puede oír muy bien leer los mensajes de texto transformados de la persona con la que tiene un chat de voz.

En el caso de los desarrolladores de ID@Xbox y el programa de asociados administrados, las características de texto a voz y de voz a texto están disponibles como parte de las [características de accesibilidad de Game chat 2](/gaming/xbox-live/multiplayer/chat/using-game-chat-2.md#accessibility) en el servicio Xbox Live. Para obtener más información, consulte [Introducción a Game chat 2](/gaming/xbox-live/multiplayer/chat/game-chat-2-overview.md).

### <a name="sound-feedback"></a>Información de sonido

El sonido proporciona información al jugador, aparte de la información visual. Un buen diseño del audio del juego puede mejorar la accesibilidad para los jugadores con dificultades visuales. A continuación se enumeran otras indicaciones por considerar:

*   Usa pistas de audio 3D para proporcionar información espacial adicional.
* Separa los controles de volumen de la música, la voz y los efectos de sonido.
*   Diseña una voz que proporcione información significativa para los jugadores. (Ejemplo: "Los enemigos se acercan" frente a "Los enemigos entran por la puerta de atrás.")
*   Asegúrate de que la voz se habla a una velocidad razonable y proporciona un control de velocidad para obtener una mejor accesibilidad.

### <a name="fully-mappable-controls"></a>Controles completamente asignables

Hay empresas y organizaciones, como [Special Effect](https://www.specialeffect.org.uk/), que diseña mandos de juego personalizados que pueden usarse con distintos sistemas de juego como Windows y Xbox One. Esta personalización permite a los usuarios con distintas formas de discapacidad jugar a juegos a los que, de lo contrario, no podrían jugar. Para obtener más información sobre personas que pueden jugar a juegos de forma independiente gracias a los mandos personalizados, consulta [a quién ayudaron](https://www.specialeffect.org.uk/who-we-helped).

Como desarrollador de juegos, puedes hacer que tu juego sea más accesible permitiendo que los controles se puedan asignar por completo para que los jugadores tengan la opción de conectar sus mandos personalizados y asignar las teclas según sus necesidades.

Disponer de controles totalmente asignables también beneficia a los usuarios que usan controladores estándar. Los jugadores pueden diseñar un diseño que se ajuste a sus necesidades individuales particulares.

Los mandos estándar de Xbox One y Xbox Elite ofrecen la personalización de los mandos para jugar con precisión. Para utliizer completamente sus capacidades de reasignación, __se recomienda que los desarrolladores incluyan reasignación directamente en el juego__. Para obtener más información, consulta [Xbox One](https://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) y [Xbox Elite](https://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### <a name="wider-selection-of-difficulty-levels"></a>Selección más amplia de niveles de dificultad

Los videojuegos ofrecen entretenimiento. El desafío para los desarrolladores de juegos es ajustar el nivel de dificultad para que el jugador experimente el nivel de desafío correcto. En primer lugar, no todos los jugadores tienen el mismo nivel y capacidad, así que diseñar una selección más amplia de opciones de dificultad aumenta la posibilidad de proporcionar a los jugadores la dificultad adecuada del desafío. Al mismo tiempo, esta selección más amplia también hace que tu videojuego sea más accesible porque podría hacer que más personas con discapacidades jueguen al juego. Recuerda, los jugadores quieren superar desafíos en un juego y recibir una recompensa por ello. No quieren un juego al que no puedan ganar.

Retocar el nivel de dificultad del juego es un proceso delicado. Si está muy fácil, es posible que los jugadores se aburran. Si es demasiado difícil, es posible que los jugadores o lo dejen y no vuelvan a jugar a partir de un punto. El proceso de equilibrio es arte y ciencia. Existen muchas formas de realizar un nivel de juego que tiene la cantidad adecuada de desafío. Algunos juegos ofrecen entradas simplificadas, como una opción de juego de pulsar solo un botón, una opción rebobinar y reproducir para hacer que el juego sea más tolerable o menos enemigos y más débiles para que sea más fácil seguir adelante después de varios intentos.

### <a name="photosensitivity-epilepsy-testing"></a>Pruebas de epilepsia fotosensible

La epilepsia fotosensible (PSE) es una condición en la que los estímulos visuales desencadenan ataques, incluidas exposiciones a luces intermitentes o a ciertos patrones y formularios visuales móviles. Esto ocurre en aproximadamente el tres por ciento de la población y es más común en niños y adolescentes. En lo que respecta a los números, estamos observando aproximadamente [1 en 4000 personas con una antigüedad de 5-24](https://www.epilepsy.com/learn/professionals/about-epilepsy-seizures/reflex-seizures-and-related-epileptic-syndromes-0).

Hay muchos factores que pueden causar una reacción fotosensible al jugar a videojuegos, incluida la duración del juego, la frecuencia del parpadeo, la intensidad de la luz, el contraste del fondo y la luz, la distancia entre la pantalla y el jugador y la longitud de onda de la luz.

Muchas personas detectan que tienen epilepsia a través de una asunción. Los jugadores pueden y deben tener su primer secuestro a través de videojuegos, lo que puede provocar lesiones físicas. Como desarrollador, estas son algunas sugerencias para diseñar un juego con el fin de reducir el riesgo de ataques causados por la epilepsia fotosensible.

Evite lo siguiente:
* Tener luces intermitentes con una frecuencia de entre 5 y 30 parpadeos por segundo (hercios) porque las luces intermitentes en ese intervalo tienen más probabilidades de desencadenar ataques.
* Cualquier secuencia de imágenes intermitentes que dure más de 5 segundos
* Más de tres destellos en un solo segundo, cubriendo un 25% + de la pantalla
* Mover patrones repetidos o texto uniforme, cubriendo el 25% + de la pantalla
* Patrones repetidos estáticos o texto uniforme que cubren el 40% + de la pantalla
* Un cambio elevado instantáneo en el brillo y el contraste (incluidos los cortes rápidos), o en el color rojo
* Hay más de cinco franjas repetidas de contraste alto uniformemente: filas o columnas, como cuadrículas y checkerboards, que pueden estar compuestas de elementos normales más pequeños como Polkadots
* Hay más de cinco líneas de texto con el formato de mayúsculas y minúsculas, sin un espaciado entre las letras y el interlineado del mismo alto que las propias líneas, de forma que se convierten en filas alternas uniformemente en contraste alto.

Usa un sistema automatizado para buscar en el juego estímulos que pudieran causar epilepsia fotosensible. (Ejemplo: [la prueba de hardware y el](https://www.hardingtest.com/index.php?page=test) [analizador de patrón y Flash y el analizador de patrones (FPA) G2](https://www.hardingfpa.com/harding-fpa-for-games/) desarrollados por Cambridge Research System Ltd y profesor Graham Harding). 

Incluya el **parpadeo activado y desactivado** como opción de configuración y establezca **parpadeo** como **desactivado** de forma predeterminada. Al hacerlo, protege a los jugadores que todavía no saben que son susceptibles de sufrir ataques.

Diseña pausas entre los niveles del juego y anima a los jugadores a tomar un descanso y a no jugar sin interrupciones.

## <a name="other-accessibility-resources"></a>Otros recursos de accesibilidad

Estos son algunos sitios externos que proporcionan información adicional sobre accesibilidad a juegos.

### <a name="game-accessibility-guidelines"></a>Directrices sobre accesibilidad a juegos
* [Directrices de accesibilidad de juegos](http://gameaccessibilityguidelines.com/) (se usa como referencia en este tema)
* [Directrices de AbleGamers Foundation](https://accessible.games/accessible-player-experiences/) (se usa como referencia en este tema)
* [Diseñar juegos universalmente accesibles (UA)](https://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### <a name="custom-input-controllers"></a>Mandos de entrada personalizados
* [Special effect](https://www.specialeffect.org.uk/)
* [War fighter engaged](https://www.warfighterengaged.org/)

### <a name="other-references-used"></a>Otras referencias usadas
* [Color Blind Awareness, una compañía de interés social](https://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [Cómo hacer los subtítulos bien &mdash; en un artículo de blog sobre Gamasutra by Ian Hamilton](https://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Programa Innovation for All](https://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)
* [Epilepsia base](https://www.epilepsy.com/)

### <a name="related-links"></a>Vínculos relacionados
* [Inclusive Design](https://www.microsoft.com/design/inclusive/)
* [Concentrador del desarrollador de accesibilidad de Microsoft](https://developer.microsoft.com/windows/accessible-apps)
* [Desarrollar aplicaciones para UWP accesibles](../design/accessibility/accessibility.md)
* [Libro electrónico Creación de un software accesible](https://www.microsoft.com/download/details.aspx?id=19262)