---
author: joannaleecy
title: Hacer que los juegos sean accesibles
description: "Aprende a hacer que los juegos sean accesibles. Usa el principio de diseño de juego inclusivo para lograr accesibilidad al juego."
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.sourcegitcommit: 2492ff5c8b3ba0331e831234943a1db124f8fa4f
ms.openlocfilehash: 2b78d767a7ac75e27f759c0eb06953e6158fb88b

---
#  Hacer que los juegos sean accesibles

La accesibilidad puede permitir que cada persona y cada organización del planeta consiga más, y esto se aplica a hacer que los juegos también sean más accesibles. Este artículo está diseñado para los desarrolladores de juegos; específicamente diseñadores, productores y administradores de juegos. Proporciona una introducción a las directrices de accesibilidad a juegos derivadas de varias organizaciones (enumeradas en la sección Referencia más adelante) y presenta el principio de diseño de juego inclusivo para crear juegos más accesibles.

##  ¿Por qué hacer que los juegos sean accesibles?

### Mayor base de jugadores

En su nivel más básico, la justificación comercial de la accesibilidad es sencilla:

Número de usuarios que pueden jugar al juego x Genialidad del juego = Ventas de juegos

Si hicieras un juego increíble que es muy complicado o complejo y al que solo un puñado de personas pudiera jugar, limitarías tus ventas. Del mismo modo, si hiciste un juego al que no pueden jugar los usuarios con impedimentos cognitivos, sensoriales o físicos, pierdes ventas potenciales. Teniendo en cuenta que [el 19% de la población solo de los Estados Unidos tiene algún tipo de discapacidad](http://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), esto puede tener un gran impacto en los ingresos de tu título. 

Para más justificaciones empresariales, consulta [Hacer que los videojuegos sean accesibles](https://msdn.microsoft.com/library/windows/desktop/ee415219).

### Mejores juegos

Crear un juego más accesible puede crear un juego mejor al final. 

Un ejemplo es los subtítulos en los juegos. Antes, los juegos apenas admitían subtítulos en sus diálogos. Actualmente, se espera que los juegos incluyan subtítulos. Este cambio no lo hicieron jugadores con discapacidades. Fueron los jugadores los que simplemente prefirieron jugar con subtítulos porque mejoraba la experiencia de juego. Los jugadores activan los subtítulos cuando juegan con mucho ruido de fondo, tienen dificultades para oír voces con varios efectos de sonido o ruido ambiental a la vez o simplemente cuando necesitan tener el volumen bajo para evitar molestar a los demás. Los subtítulos no solo ayudaron a los jugadores a tener una mejor experiencia de juego, sino que también permiten que personas con discapacidades auditivas también jueguen.

La reasignación del mando es otra característica que lentamente se está convirtiendo en un estándar de la industria del juego por razones similares. Los jugadores disfrutan personalizando sus experiencias de juego. La mayoría de usuarios no sabe que la capacidad de reasignar botones en un dispositivo de entrada es una característica de accesibilidad que fue diseñada para hacer que personas con distintos tipos de discapacidades motrices puedan jugar a un juego.

Básicamente, el proceso de pensamiento usado para hacer que el juego fuera más accesible a menudo causará que el juego sea mejor porque habrás diseñado una experiencia más fácil de usar y personalizable para que los jugadores la disfruten.

### Un espacio social

Jugar es una forma de entretenimiento y puede proporcionar horas de diversión. Para algunos, jugar no es solo una forma de entretenimiento, sino un escape de la cama del hospital, del dolor crónico o de una ansiedad social grave. Los jugadores se transportan a un mundo donde se convierten en los protagonistas del videojuego. A través de los juegos, pueden crear y participar en un espacio social por sí mismos que proporciona una distracción de las dificultades diarias originadas que les causan sus limitaciones y eso les da una oportunidad de comunicarse con personas con las que, de lo contrario, es posible que no interactuaran.

##  ¿El juego que estás haciendo hoy es accesible?

Si estás pensando por primera vez en hacer un juego accesible, estas son algunas preguntas que puedes hacerte:

* ¿Puedes completar el juego con una sola mano? 
* ¿Puede una persona normal escoger el juego y jugarlo?
* ¿Puedes jugar eficazmente al juego en un monitor o un televisor pequeño sentado a cierta distancia?
* ¿Es compatible con más de un tipo de dispositivo de entrada que puede usarse para jugar a todo el juego?
* ¿Puedes jugar al juego con el sonido desactivado?
* ¿Puedes jugar al juego con el monitor establecido en blanco y negro?

Si las respuestas son principalmente no, o no sabes responderlas, es el momento de dar el paso y aplicar la accesibilidad en el juego.

## Definir discapacidad

La discapacidad se define como "un desajuste entre las necesidades de la persona y las del servicio, producto o entorno que se ofrece". ([Vídeo inclusivo](https://www.microsoft.com/design/inclusive), Microsoft.com.) Esto significa que cualquier persona puede experimentar una discapacidad y que puede ser un problema breve o circunstancial. Imagina qué desafíos pueden encontrarse los jugadores con estos problemas cuando jueguen al juego y piensa en cómo se puede mejorar el juego pensando en ellos. A continuación se enumeran algunas discapacidades por considerar:

### Visual

*   Enfermedades médicas de larga duración como glaucoma, cataratas, daltonismo, miopía y retinopatía diabética.
*   Situaciones de corta duración y circunstanciales como un monitor o pantalla de tamaño pequeño, una pantalla de baja resolución o un alto brillo de la pantalla debido a fuentes de luz muy brillantes de un monitor
        
### Auditiva

* Enfermedades médicas de larga duración como sordera completa o pérdida parcial de la audición debido a enfermedades o a la genética
* Situaciones de corta duración y circunstanciales como ruido de fondo excesivo o volumen restringido para evitar molestar a los demás
        
### Motriz

* Enfermedades médicas de larga duración como la enfermedad de Parkinson, Esclerosis lateral amiotrófica (ELA), artritis y distrofia muscular
* Situaciones de corta duración y circunstanciales como una mano lesionada, sujetar una bebida o llevar un niño en un brazo
  
### Cognitiva

* Condiciones médicas de largo plazo como dislexia, epilepsia, trastorno por déficit de atención con hiperactividad (TDAH), demencia y amnesia
* Situaciones de corta duración y circunstanciales como falta de sueño o distracciones temporales como oír una sirena de un vehículo de emergencias que conduce cerca de la vivienda

### Comunicativas

* Enfermedades médicas de larga duración como daños en las cuerdas vocales, disartria y apraxia
* Situaciones de corta duración y circunstanciales como trabajos dentales o comer y beber


## ¿Cómo hacer que los juegos sean más accesibles?

### Cambio de diseño: enfoque de diseño de juego inclusivo

El diseño inclusivo se centra en la creación de productos y servicios más accesible para una gama más amplia de consumidores, incluidas las personas con discapacidades.

Para tener éxito, los diseñadores de juegos de hoy necesitan pensar más allá de crear juegos divertidos para un público pequeño y dirigido. Los diseñadores de juegos tienen que tener en cuenta cómo sus decisiones de diseño afectan a la accesibilidad general del juego y a la jugabilidad de su público potencial general, incluidas las personas con discapacidades.

Por tanto, los paradigmas tradicionales de diseño de juegos deben cambiar para aprovechar el concepto de diseño de juego inclusivo. El diseño de juego inclusivo significa algo más que el diseño de juego básico de crear diversión para un público de destino, es crear personajes adicionales o modificados para incluir una mayor variedad de personas. 

Este paso adicional ayuda a identificar diferencias en el diseño original. Al identificar las diferencias, se puede iterar en el concepto de diseño original y mejorarlo. Cuando, durante el proceso de diseño del juego, te tomas tu tiempo para ser más inclusivo, el +juego final se vuelve más accesible.

### Potencia a los jugadores: dales opciones

La accesibilidad consiste en opciones. Da a los jugadores las opciones para personalizar su experiencia de juego. Si ya tienes una gran base de aficionados, puede que una parte importante de tu público no quiera que la experiencia cambie en alguna manera. No hay problema. Ofrece a los jugadores la capacidad de activar y desactivar esas características y haz que se puedan configurar individualmente.

### Innova: sé creativo

Existen muchas maneras creativas de mejorar la accesibilidad de tu juego. Ponte tu traje de creatividad y aprende de otros juegos accesibles del mercado. Si ya tienes un juego existente, aprende a identificar las características actuales del juego que se podrían mejorar mientras mantienes la experiencia y la mecánica de juego principal como se diseñaron. Como se menciona anteriormente, la accesibilidad a juegos es proporcionar a los jugadores opciones para personalizar su experiencia de juego.

### Evangeliza: haz que la accesibilidad sea una prioridad en tu estudio de juegos

El desarrollo de juegos siempre se realiza en una línea de tiempo ajustada, así que dar prioridad a la accesibilidad ayudará a facilitar el proceso. Una forma de hacerlo es diseñar desde el principio teniendo en cuenta la accesibilidad. Comparte tus conocimientos sobre la accesibilidad con tu equipo y comparte la justificación empresarial.

### Revisa: evalúa constantemente tu juego

Durante el desarrollo, puedes realizar un proceso de revisión para asegurarte de que se esté pensando en la accesibilidad en cada paso del camino. Haz una lista de comprobación como la siguiente para ayudar a tu equipo a evaluar constantemente si lo que se está creando es accesible o no.

| Lista de comprobación                                         | Características de accesibilidad                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Animaciones del juego                                | Se probaron los subtítulos y la fotosensibilidad                                                                           |
| Ilustraciones generales (gráficos 2D y 3D)              | Opciones y colores fáciles de usar para daltónicos, no se depende totalmente del color para identificar y se usan también formas y patrones|
| Pantalla de inicio, menú de configuración y otros menús       | Capacidad de leer opciones en voz alta y de recordar las opciones de la configuración, método de entrada de mando alternativo, tamaño de fuente de la interfaz de usuario ajustable  |
| Experiencia de juego                                          | Niveles de dificultad ajustables, subtítulos, buenos elementos visuales y comentarios de audio para el jugador                           |
| Pantalla HUD                                       | Posición de la pantalla ajustable, tamaño de fuente ajustable, opción fácil de usar para daltónicos                                                  |        
| Entrada del mando                                     | Controles que se puedan reasignar para dispositivo de entrada, compatibilidad con mandos personalizados, entrada simplificada permitida para el juego                               |        

### Probar e iterar: obtener comentarios de los jugadores

Al organizar sesiones de prueba de juegos, invita a probadores con discapacidades para las que tu juego está diseñado y haz que jueguen. Observa cómo juegan y obtén comentarios de ellos. Averigua qué cambios deben realizarse para mejorar el juego.

### Anúncialo: haz saber al mundo que tu juego es accesible

Los usuarios querrán saber si los jugadores con discapacidades podrán jugar a tu juego. Expón claramente la accesibilidad al juego en el sitio web y el envase del juego para garantizar que los usuarios sepan qué esperar cuando compren tu juego. Recuerda hacer accesibles también tu sitio web y todos los canales de ventas del juego. Más importante aún, llega a la comunidad de juegos de accesibilidad e infórmales sobre tu juego.

## Características de accesibilidad a juegos

En esta sección se describen algunas características que pueden hacer que el juego sea más accesible. Estas características se derivan de instrucciones que se tomaron de las [Directrices de accesibilidad a juegos](http://gameaccessibilityguidelines.com/), que representan los resultados de un grupo de colaboración de estudios, especialistas y academias. Para obtener más información, consulta las [Directrices de accesibilidad a juegos](http://gameaccessibilityguidelines.com/). 

### Gráficos e interfaz de usuario fáciles de usar para daltónicos

La retina del ojo tiene dos tipos de células sensible a la luz: los conos para ver donde está luz y los bastones para ver cuando hay poca luz. Hay tres tipos de conos (rojos, verdes y azules) que nos permiten ver colores correctamente. El daltonismo se produce cuando uno o varios de estos tres tipos de conos no funciona según lo esperado. El grado de daltonismo puede oscilar desde una percepción de color casi normal con una sensibilidad reducida hacia la luz roja, verde o azul a una completa incapacidad para percibir el color rojo, verde o azul. Dado que es menos común tener sensibilidad reducida a la luz azul, al diseñar para daltónicos, la selección de colores está dirigida a personas con daltonismo al rojo o al verde:
 
  + Usa combinaciones de colores que puedan diferenciar las personas con daltonismo al rojo o al verde:
  
    * Colores que parecen similares: todos los tonos de rojo y verde incluidos marrón y naranja
    * Colores que destacan: azul y amarillo
    
  + No confíes únicamente en los colores para distinguir los objetos del juego: usa también formas y patrones
  
### Subtítulos

Al diseñar los subtítulos de tu juego, el objetivo es proporcionar subtítulos legibles como una opción para que también se pueda disfrutar de tu juego sin sonido. Debería ser posible tener componentes del juego como diálogos, audio y efectos de sonido que se muestren como texto en pantalla.

Estas son algunas directrices básicas a tener en cuenta cuando se diseñan los subtítulos:

*   Selecciona una fuente legible simple.
*   Selecciona el tamaño de fuente lo suficientemente grande o ten en cuenta la opción de un tamaño de fuente ajustable para obtener una mayor flexibilidad. (El tamaño de fuente ideal depende del tamaño de la pantalla o la distancia a la pantalla, entre otros).
*   Crea un alto contraste entre el color de fondo y el de la fuente. (Para obtener más información, consulta [Información acerca de la relación de contraste](https://msdn.microsoft.com/windows/uwp/accessibility/accessible-text-requirements).)
* Muestra frases cortas en pantalla. (Recuerda no revelar contenido del juego mostrando el texto antes de que ocurra el evento.)
*   Distingue qué hace el sonido o quién habla. (Ejemplo: "Daniel: ¡Hola!")
*   Proporciona la opción de activar y desactivar los subtítulos. (Característica adicional: capacidad de seleccionar cuánta información de sonido se muestra en función de la importancia.)

### Información de sonido

El sonido proporciona información al jugador, aparte de la información visual. Un buen diseño del audio del juego puede mejorar la accesibilidad para los jugadores con dificultades visuales. A continuación se enumeran otras indicaciones por considerar:

*   Usa pistas de audio 3D para proporcionar información espacial adicional.
* Separa los controles de volumen de la música, la voz y los efectos de sonido.
*   Diseña una voz que proporcione información significativa para los jugadores. (Ejemplo: "Los enemigos se acercan" frente a "Los enemigos entran por la puerta de atrás.")
*   Asegúrate de que la voz se habla a una velocidad razonable y proporciona un control de velocidad para obtener una mejor accesibilidad.

### Controles completamente asignables

Hay empresas y organizaciones, como [Special Effect](http://www.specialeffect.org.uk/), que diseña mandos de juego personalizados que pueden usarse con distintos sistemas de juego como Windows y Xbox One. Esta personalización permite a los usuarios con distintas formas de discapacidad jugar a juegos a los que, de lo contrario, no podrían jugar. Para obtener más información sobre personas que pueden jugar a juegos de forma independiente gracias a los mandos personalizados, consulta [a quién ayudaron](http://www.specialeffect.org.uk/who-we-helped).

Como desarrollador de juegos, puedes hacer que tu juego sea más accesible permitiendo que los controles se puedan asignar por completo para que los jugadores tengan la opción de conectar sus mandos personalizados y asignar las teclas según sus necesidades.

Los mandos estándar de Xbox One y Xbox Elite ofrecen la personalización de los mandos para jugar con precisión. Para obtener más información, consulta [Xbox One](http://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) y [Xbox Elite](http://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### Selección más amplia de niveles de dificultad

Los videojuegos ofrecen entretenimiento. El desafío para los desarrolladores de juegos es ajustar el nivel de dificultad para que el jugador experimente el nivel de desafío correcto. En primer lugar, no todos los jugadores tienen el mismo nivel y capacidad, así que diseñar una selección más amplia de opciones de dificultad aumenta la posibilidad de proporcionar a los jugadores la dificultad adecuada del desafío. Al mismo tiempo, esta selección más amplia también hace que tu videojuego sea más accesible porque podría hacer que más personas con discapacidades jueguen al juego. Recuerda, los jugadores quieren superar desafíos en un juego y recibir una recompensa por ello. No quieren un juego al que no puedan ganar.

Retocar el nivel de dificultad del juego es un proceso delicado. Si está muy fácil, es posible que los jugadores se aburran. Si es demasiado difícil, es posible que los jugadores o lo dejen y no vuelvan a jugar a partir de un punto. El proceso de equilibrio es arte y ciencia. Existen muchas formas de realizar un nivel de juego que tiene la cantidad adecuada de desafío. Algunos juegos ofrecen entradas simplificadas, como una opción de juego de pulsar solo un botón, una opción rebobinar y reproducir para hacer que el juego sea más tolerable o menos enemigos y más débiles para que sea más fácil seguir adelante después de varios intentos.

### Pruebas de epilepsia fotosensible

La epilepsia fotosensible es una enfermedad en la que se producen ataques a causa de estímulos visuales como exposiciones a luces parpadeantes o a determinadas formas y patrones visuales en movimiento. Esto ocurre en aproximadamente el tres por ciento de la población y es más común en niños y adolescentes.

Hay muchos factores que pueden causar una reacción fotosensible al jugar a videojuegos, incluida la duración del juego, la frecuencia del parpadeo, la intensidad de la luz, el contraste del fondo y la luz, la distancia entre la pantalla y el jugador y la longitud de onda de la luz.

Como desarrollador, estas son algunas sugerencias para el diseño de juego para incluir a jugadores que tienen tendencia a la epilepsia fotosensible:

*   Evita usar luces parpadeantes con una frecuencia de 5 a 30 parpadeos por segundo (hercios) porque las luces parpadeantes en ese intervalo son más probables de causar ataques.
*   Usa un sistema automatizado para buscar en el juego estímulos que pudieran causar epilepsia fotosensible. (Ejemplo: [Harding Flash and Pattern Analyzer (FPA) G2](http://www.hardingfpa.com/harding-fpa-for-games/) desarrollado por Cambridge Research System Ltd y el Profesor Graham Harding.) 
*   Diseña pausas entre los niveles del juego y anima a los jugadores a tomar un descanso y a no jugar sin interrupciones.

## Otros recursos de accesibilidad

Estos son algunos sitios externos que proporcionan información adicional sobre accesibilidad a juegos.

### Directrices sobre accesibilidad a juegos
* [Directrices sobre accesibilidad a juegos](http://gameaccessibilityguidelines.com/)
* [Directrices de la Fundación AbleGamers](http://www.includification.com/)
* [Diseñar juegos universalmente accesibles (UA)](http://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### Mandos de entrada personalizados
* [Special effect](http://www.specialeffect.org.uk/)
* [War fighter engaged](http://www.warfighterengaged.org/)

## Referencias usadas
* [Directrices sobre accesibilidad a juegos](http://gameaccessibilityguidelines.com/)
* [Directrices de la Fundación AbleGamers](http://www.includification.com/)
* [Color Blind Awareness, una compañía de interés social](http://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [How to do subtitles well: un artículo en el blog Gamasutra, de Ian Hamilton](http://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Programa Innovation for All](http://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)

## Vínculos relacionados
* [Inclusive Design](https://www.microsoft.com/design/inclusive)
* [Concentrador del desarrollador de accesibilidad de Microsoft](https://developer.microsoft.com/windows/accessible-apps)
* [Desarrollar aplicaciones para UWP accesibles](https://msdn.microsoft.com/windows/uwp/accessibility/accessibility)
* [Libro electrónico Creación de un software accesible](https://www.microsoft.com/download/details.aspx?id=19262)



<!--HONumber=Jun16_HO4-->


