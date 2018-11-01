---
author: GrantMeStrength
title: Creación de una aplicación compleja de plataforma Universal de Windows (UWP)
description: 'En los equipos de diseño de Microsoft, nuestro proceso para crear aplicaciones consta de cinco etapas diferentes: concepto, estructura, dinámica, elementos visuales y prototipo. Te animamos a que adoptes un método similar y te diviertas creando experiencias nuevas de las que todo el mundo pueda disfrutar.'
ms.assetid: 9A5189CD-3B97-4967-8E7D-36D25F04F244
ms.author: jken
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ce5e0d272e14b9c69f660c8c7de6a838fa7956ac
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "5922416"
---
#  <a name="building-a-complex-universal-windows-platform-uwp-app"></a>Creación de una aplicación compleja de plataforma Universal de Windows (UWP)

En los equipos de diseño de Microsoft, nuestro proceso para crear aplicaciones consta de cinco etapas diferentes: concepto, estructura, dinámica, elementos visuales y prototipo. Te animamos a que adoptes un método similar y te diviertas creando experiencias nuevas de las que todo el mundo pueda disfrutar.

## <a name="concept"></a>Concepto

**Concreta tu aplicación**

A la hora de planear tu aplicación para la Plataforma universal de Windows (UWP), no solo debes determinar qué es lo que hará tu aplicación y a quién estará destinada, sino también aquello en lo que tu aplicación va a ser genial. En el corazón de toda buena aplicación reside un concepto sólido que proporciona una base resistente.

Supongamos que deseas crear una aplicación de fotografía. Pensando en los motivos por los que los usuarios trabajan con sus fotografías, las guardan y las comparten, te das cuenta de que quieren revivir recuerdos, conectar con otras personas a través de las fotos y conservar las fotos en un lugar seguro. Por tanto, esas son las cosas que te interesa que sean los puntos fuertes de la aplicación y utilizas esos objetivos de experiencia del usuario para guiarte durante el resto del proceso de diseño.

**¿En qué consiste la aplicación?** Empieza con un concepto amplio y enumera todo lo que quieras que los usuarios puedan hacer con la aplicación.

Imagina que quieres crear una aplicación que ayude a las personas a planear sus viajes. Aquí te presentamos algunas ideas que bien podrías haber anotado en una servilleta:

-   Conseguir mapas de todos los lugares de un itinerario y llevarlos contigo al viaje.
-   Averiguar sobre eventos especiales que tengan lugar mientras estás en la ciudad.
-   Permitir que los compañeros de viaje creen listas con actividades y atracciones indispensables por separado, pero que puedan compartirlas.
-   Permitir que los compañeros de viaje recopilen sus fotografías y las compartan con familiares y amigos.
-   Obtener los destinos recomendados en función del precio de los vuelos.
-   Encontrar una lista consolidada de restaurantes, comercios y actividades dentro del área de destino.

![diseño de una aplicación de viajes](images/ux-triptracker-tab-phone-700.png)

**¿En qué se destaca tu aplicación?** Da un paso atrás y examina la lista de ideas para ver si algún escenario en particular te llama la atención. Acepta el reto de reducir la lista a un solo escenario en el que quieras centrarte. En el proceso puede que descartes muchas ideas buenas, pero hacerlo es fundamental a fin de lograr un solo escenario excepcional.

Después de elegir un solo escenario, decide cómo explicarías a una persona normal los motivos por los que tu aplicación es excelente, y escríbelos en una sola frase. Por ejemplo:

-   Mi aplicación para viajes es excelente para crear itinerarios de forma conjunta para viajes en grupo.
-   Mi aplicación de gimnasia es excelente para permitir a los amigos realizar un seguimiento de su progreso y compartir sus logros.
-   Mi aplicación de tiendas de comestibles es excelente para ayudar a las familias a coordinar sus compras semanales para que no se olviden de comprar algo o lo compren dos veces.

![diseño de una herramienta de colaboración](images/ux-collaboration-tabphone-700.png)

Este es el enunciado de puntos fuertes de la aplicación y puede orientar muchas decisiones y disyuntivas de diseño que surgen mientras se crea la aplicación. Concéntrate en los escenarios que quieres que los usuarios experimenten en tu aplicación y asegúrate de que esto no acaba convirtiéndose en una lista de funciones. Debe tratarse de lo que podrán hacer los usuarios y no de lo que podrá hacer la aplicación.

**El embudo de diseño**

Después de tener una buena idea resulta muy tentador pasar a desarrollarla, incluso llevarla a la fase de producción en cierta medida. Pero supongamos que lo haces y que más adelante se te ocurre otra idea interesante. Es natural que tiendas a seguir con la idea a la que ya has dedicado tu tiempo y esfuerzo sin tener en cuenta los méritos relativos de las dos ideas. ¡Si se te hubiese ocurrido esa idea antes! Pues bien, el embudo de diseño es una técnica que ayuda a dar con las mejores ideas lo antes posible.

El término "embudo" se debe a su forma. En la parte ancha del embudo entran muchas ideas y cada una se muestra como un artefacto de diseño de baja fidelidad (como un boceto, quizás, o como un párrafo de texto). A medida que esta recopilación de ideas viaja hacia el extremo estrecho del embudo, el número de ideas se reduce y aumenta, al mismo tiempo, la fidelidad de los artefactos que representan las ideas. Cada artefacto solo debe capturar la información necesaria para comparar una idea con otra o para responder a una pregunta determinada, como "¿es útil o intuitivo?". *No debe emplearse más tiempo ni esfuerzo en cada uno de ellos*. Algunas ideas se quedarán en el camino al probarlas, lo cual te parecerá bien, porque no habrás invertido más de lo absolutamente necesario en juzgar dichas ideas. Las ideas que consigan avanzar a lo largo del embudo se tratarán sucesivamente como ideas de alta fidelidad. Al final tendrás un único artefacto de diseño que representará la idea ganadora. Esta es la idea que ha ganado por méritos propios, no simplemente porque se te ocurrió primero. De este modo, habrás diseñado la mejor aplicación posible.

## <a name="structure"></a>Estructura


**La organización hace que todo sea más sencillo**

![la organización hace que todo sea más sencillo](images/ux-vision-and-process-organization.png)

Cuando estés contento con tu concepto, estarás preparado para la siguiente etapa: crear el plano de la aplicación. La arquitectura de información (AI) proporciona al contenido la integridad estructural que necesita. Te ayudará a definir el modelo de navegación de la aplicación y, en última instancia, la identidad de la aplicación. Si planeas la organización de tu contenido y cómo descubrirán los usuarios dicho contenido, podrás hacerte una idea más aproximada del modo en que los usuarios usarán tu aplicación.

Una buena AI no solo facilita los escenarios de usuario, sino que también te ayuda a prever las pantallas clave con las que empezar. La aplicación [Audible](http://go.microsoft.com/fwlink/p/?LinkID=268089) , por ejemplo, se inicia directamente en un concentrador que proporciona acceso a la biblioteca del usuario, la tienda, noticias y estadísticas. La experiencia se centra, por lo que los usuarios pueden obtener y disfrutar concretado rápidamente. Los niveles más profundos de la aplicación se centran en tareas más específicas.

Para obtener directrices relacionadas, consulta [Patrones de diseño básicos](../design/basics/navigation-basics.md).

## <a name="dynamics"></a>Dynamics

**Ejecutar el concepto**

Si la etapa de concepto consiste en definir el objetivo de tu aplicación, la etapa de dinámica consiste en cumplir dicho objetivo. Esto puede lograrse de muchas maneras como, por ejemplo, usando una representación esquemática para elaborar un boceto de los flujos de las páginas (esto es, cómo vas de un sitio a otro dentro de la aplicación para lograr sus objetivos) y planeando la voz de la aplicación y las palabras que se usarán en toda la interfaz de usuario de la misma. Las tramas de alambre son una herramienta rápida y de baja fidelidad que te ayuda a tomar decisiones críticas sobre el flujo de usuarios de tu aplicación.

El flujo de tu aplicación debe estar estrechamente relacionado con el enunciado de puntos fuertes y debe ayudar a los usuarios a alcanzar ese único escenario que quieres resaltar. Una aplicación excelente cuenta con flujos fáciles de aprender y que requieren un mínimo esfuerzo. Comienza pensando pantalla a pantalla (mira tu aplicación como si la estuvieses utilizando por primera vez). Cuando identifiques los escenarios de usuario de las páginas que crees, ofrecerás a las personas justo lo que quieren sin que tengan que tocar la pantalla innecesariamente. La dinámica también está relacionada con el movimiento. Las capacidades adecuadas de movimiento determinarán la fluidez y la facilidad de uso de una página a la siguiente.

Técnicas habituales para ayudarte con este paso:

-   Explicar el flujo: ¿Qué es lo primero, qué sigue?
-   Presentación gráfica del flujo: ¿Cómo deben desplazarse los usuarios por la interfaz de usuario para completar el flujo?
-   Prototipo: probar el flujo con un prototipo rápido.

**¿Qué deben poder hacer los usuarios?** Por ejemplo, la aplicación de viajes es "excelente para crear itinerarios de forma colaborativa para viajes en grupo". Confeccionemos una lista de los flujos que queremos habilitar:

-   Crear un viaje con información general.
-   Invitar a amigos a sumarse al viaje.
-   Unirse al viaje de un amigo.
-   Ver itinerarios recomendados por otros viajeros.
-   Agregar destinos y actividades a los viajes.
-   Modificar y comentar los destinos y las actividades que agregaron los amigos.
-   Compartir itinerarios para que puedan seguirlos amigos y familiares.

## <a name="visual"></a>Elementos visuales

**Habla sin pronunciar ni una palabra**

![diseño de una aplicación de creador de cócteles](images/ux-cocktailcreator-tab-phone.png)

Una vez que hayas establecido la dinámica de la aplicación, puedes hacer que tu aplicación deslumbre refinando los elementos visuales. Unos elementos visuales de gran calidad no solo determinan el aspecto de tu aplicación, sino también las sensaciones que esta transmite y cómo cobra vida mediante la animación y el movimiento. La paleta de colores, el icono y los gráficos que elijas son solo unos ejemplos de este idioma visual.

Todas las aplicaciones tienen su propia identidad única, de modo que explora las direcciones visuales que puedes tomar con tu aplicación. Deja que el contenido determine el aspecto y la sensación de tu aplicación y no permitas que el aspecto determine el contenido.

## <a name="prototype"></a>Prototipo

**Refina tu obra maestra**

La creación del prototipo es una etapa del *embudo de diseño*, que es una técnica sobre la que hablamos anteriormente, y en la que el artefacto que representa tu idea evoluciona hasta convertirse en algo más que un boceto, pero que es, al mismo tiempo, menos complicado que una aplicación completa. Un prototipo puede ser un flujo de pantallas dibujadas a mano mostradas a un usuario. La persona que realiza la prueba puede responder a indicaciones del usuario colocando diferentes pantallas o superponiendo o quitando pequeñas partes de la interfaz de usuario sobre las páginas con el fin de simular una aplicación en ejecución. Un prototipo también podría ser una aplicación muy sencilla que simule algunos flujos de trabajo, siempre que el operador se ciña a un guión establecido y pulse los botones adecuados. En esta etapa tus ideas comienzan realmente a cobrar vida y tu trabajo es sometido a pruebas de manera determinada. Cuando crees el prototipo de áreas de tu aplicación, toma el tiempo necesario para esculpir y refinar los componentes que más lo necesiten.

No nos cansamos de decírselo a los nuevos desarrolladores: la creación de aplicaciones de gran calidad es un proceso iterativo. Te recomendamos que crees los prototipos lo antes posible y de manera frecuente. Al igual que sucede con cualquier trabajo creativo, las mejores aplicaciones son el resultado de un proceso intensivo de ensayo y error.

## <a name="decide-what-features-to-include"></a>Decide qué funciones incluirás

Cuando sepas qué es lo que buscan los usuarios y cómo puedes ayudarlos a encontrarlo, puedes echar un vistazo a algunas herramientas de la caja de herramientas. Explora la Plataforma universal de Windows (UWP) y asocia funciones con las necesidades de la aplicación. Asegúrate de seguir las [directrices de experiencia del usuario](https://developer.microsoft.com/windows/apps/design) para cada característica.
<!--need URL for landing page -->

Técnicas comunes:

-   Investigación sobre la plataforma: descubre qué funciones ofrece la plataforma y cómo puedes usarlas.
-   Diagramas de asociación: conecta los flujos con las funciones.
-   Prototipo: prueba las funciones para asegurarte de que hacen lo que necesitas.

**Contratos entre aplicaciones**la aplicación puede participar en contratos entre aplicaciones que permiten flujos amplios entre aplicaciones cross-característica de usuarios.

-   **Compartir**permite que los usuarios compartan contenido desde la aplicación con otras personas a través de otras aplicaciones y también permite recibir contenido de otras personas y aplicaciones.
-   **Reproducir en**permite que los usuarios disfruten la transmisión de audio, vídeo o imágenes de la aplicación a otros dispositivos de la red doméstica.
-   **Selector de archivos y extensiones de selector de archivos** Permite que los usuarios carguen y guarden sus archivos desde el sistema de archivos local, dispositivos de almacenamiento conectados, grupo hogar o incluso otras aplicaciones. También puedes proporcionar una extensión de selector de archivos para que otras aplicaciones puedan cargar el contenido de la aplicación.

Para más información, consulta el tema sobre las [extensiones y los contratos entre aplicaciones](https://msdn.microsoft.com/library/windows/apps/hh464906).
<!-- Win 8 page. Should have replacement. -->

**Diferentes vistas, factores de forma y configuraciones de hardware**Windows pone a los usuarios en la aplicación en primer plano y cargos. Seguramente quieres que la interfaz de usuario de la aplicación se destaque en cualquier dispositivo, modo de entrada, orientación, configuración de hardware y en cualquier circunstancia en la que el usuario decida usarla.

**En primer lugar Touch**Windows ofrece una experiencia táctil única e incomparable que hace mucho más que simplemente emular la funcionalidad del mouse.

Por ejemplo, un zoom semántico es un modo optimizado para funcionalidad táctil que permite navegar por un conjunto extenso de contenido. Los usuarios pueden desplazarse de lado a lado o de arriba a abajo por las categorías del contenido y acercar la vista de las categorías para ver más información y con más detalle. Puedes usarlo para presentar el contenido de una manera más táctil, visual e informativa que con los modelos tradicionales de navegación y diseño, como las pestañas.

Por supuesto, puedes usar diferentes interacciones táctiles, como girar, pasar el dedo, etc. Obtén más información sobre [Función táctil y otras interacciones de usuario](../design/input/input-primer.md).

**Atractiva y original**Asegúrate de que la aplicación sea original y atraiga a los usuarios con estas experiencias estándar:

-   **Las animaciones**usa nuestra biblioteca de animaciones para que la aplicación rápida y fluida para los usuarios. Ayuda a los usuarios a comprender cambios contextuales y relaciona las experiencias entre sí con transiciones visuales. Obtén más información sobre [cómo animar la interfaz de usuario](../graphics/animations-overview.md).
-   **Notificaciones del sistema**informa a los usuarios saber sobre sujeto a limitación temporal contenido personalmente importante o a través de las notificaciones del sistema, atraerlos a la aplicación incluso cuando se cierra la aplicación. Obtén más información sobre los [iconos, las notificaciones y las notificaciones del sistema](../design/shell/tiles-and-notifications/index.md).
-   **Iconos de la aplicación**ofrece actualizaciones renovadas y relevantes para atraer a los usuarios volver a la aplicación. Hay más información en la siguiente sección. Obtén más información sobre los [iconos de la aplicación](../design/shell/tiles-and-notifications/creating-tiles.md).

**Personalización**

-   **Configuración de**permite que los usuarios crean la experiencia que quieran guardar la configuración de la aplicación. Consolida toda la configuración en una pantalla y a continuación los usuarios podrán configurar la aplicación con un mecanismo común con el que ya están familiarizados. Obtén más información sobre cómo [agregar la configuración de la aplicación](../design/app-settings/app-settings-and-data.md).
-   **Itinerancia**crea una experiencia continua entre dispositivos mediante la itinerancia de datos que permite al usuario retomar una tarea exactamente donde la dejó y preserva la experiencia de usuario que les interesa más, independientemente del dispositivo que están usando. Mantén la movilidad de la configuración y los estados, para que los usuarios puedan usar la aplicación en todas partes, ya sea en el equipo familiar de la cocina, en el del trabajo o en una tableta personal. Obtén más información acerca de cómo [Administrar datos de la aplicación](../design/app-settings/store-and-retrieve-app-data.md) y consulta las [Directrices de datos móviles de las aplicaciones](https://msdn.microsoft.com/library/windows/apps/hh465094).
-   **Los iconos de usuario**  hacer que la aplicación más personal a los usuarios: carga su imagen de icono de usuario, o permitir que los usuarios establezcan contenido de la aplicación como su icono personal en Windows.

**Funcionalidades del dispositivo**Asegúrate de que la aplicación puede beneficiarse de las funcionalidades de dispositivos de hoy en día.

-   **Gestos de proximidad**permite que los usuarios conecten sus dispositivos a otros usuarios que estén físicamente cercanos tocándolos físicamente a los dispositivos entre sí (juegos multijugador). Obtén más información sobre [proximidad y pulsación](https://msdn.microsoft.com/library/windows/apps/hh465229).
-   **Cámaras y dispositivos de almacenamiento externos**conecta a los usuarios con sus cámaras integradas o conversar para charlando y conferencias, grabar videoblogs, tomar fotografías del perfil, documentar hechos del mundo alrededor de ellos, o para cualquier actividad de la aplicación. Obtén más información sobre el [acceso a contenido en almacenamiento extraíble](https://msdn.microsoft.com/library/windows/apps/hh465189).
-   **Acelerómetros y otros sensores**   Los dispositivos vienen con diversos sensores hoy en día. La aplicación puede atenuar o iluminar la pantalla según la luz ambiental, redistribuir la interfaz de usuario si el usuario gira la pantalla o reaccionar ante un movimiento físico. Obtén más información sobre los [sensores](../devices-sensors/sensors.md).
-   **Ubicación geográfica**usa la información de ubicación geográfica datos web estándar o sensores de ubicación geográfica para ayudar a los usuarios desplazarse, encontrar su posición en un mapa u obtener avisos sobre personas, actividades y destinos cercanos. Obtén más información sobre la [ubicación geográfica](https://msdn.microsoft.com/library/windows/apps/hh465139).

Volvamos a tomar el ejemplo de la aplicación para viajes. Para ofrecer una excelente ayuda a un grupo de amigos que quieren crear de forma conjunta los itinerarios de viajes en grupo, podrías usar algunas de estas funciones, por mencionar algunas:

-   Compartir: los usuarios comparten sus próximos viajes e itinerarios en diferentes redes sociales para compartir el entusiasmo previo al viaje con amigos y familiares.
-   Buscar: los usuarios buscan y encuentran actividades o destinos en los itinerarios compartidos o públicos de otras personas que pueden incluir en sus propios viajes.
-   Notificaciones: los usuarios reciben una notificación cuando sus compañeros de viaje actualizan los itinerarios.
-   Configuración: los usuarios configuran la aplicación según su preferencia, por ejemplo, qué viaje debe generar notificaciones o qué grupos sociales tienen permitido buscar los itinerarios de los usuarios.
-   Zoom semántico: los usuarios se desplazan por la escala de tiempo del itinerario y acercan la vista para ver con más detalle la larga lista de actividades que planearon.
-   Imágenes de usuario: los usuarios eligen la imagen que quieren que aparezca cuando comparten su viaje con amigos.

## <a name="decide-how-to-monetize-your-app"></a>Decide cómo rentabilizar tu aplicación

Cuentas con muchas opciones para ganar dinero con tu aplicación. Si decides usar publicidad o ventas en la aplicación, querrás diseñar una interfaz de usuario que lo permita. Para más información, consulta [Planear la rentabilidad](../monetize/index.md).

## <a name="design-the-ux-for-your-app"></a>Diseña la experiencia de usuario de tu aplicación

Se trata de comprender correctamente los aspectos fundamentales. Ahora que sabes cuáles son los puntos fuertes de tu aplicación y ya pensaste en los flujos que quieres admitir, puedes comenzar a pensar en los aspectos fundamentales del diseño de la experiencia de usuario.

**¿Cómo se debe organizar el contenido de la interfaz de usuario?**   La mayor parte del contenido de la aplicación se puede organizar en algún tipo de grupos o jerarquías. Lo que elijas como grupo de nivel superior del contenido debe coincidir con el objetivo del enunciado de puntos fuertes.

Por ejemplo, en la aplicación para viajes existen varias formas de agrupar los itinerarios. Si el objetivo de la aplicación fuese descubrir destinos interesantes, podrías agruparlos según los intereses, como aventuras, diversión bajo el sol o escapadas románticas. Sin embargo, como el objetivo de la aplicación es planear viajes con amigos, tiene más sentido organizar los itinerarios en función de círculos sociales, como familia, amigos o trabajo.

Elegir cómo quieres agrupar el contenido te ayuda a decidir qué páginas o vistas necesitas en la aplicación. Para obtener más información, consulta los conceptos básicos de la interfaz de usuario.

**¿Cómo deberías presentar el contenido de la interfaz de usuario?** Una vez que hayas decidido cómo organizar la interfaz de usuario, puedes definir objetivos de experiencia de usuario que especifiquen la manera en que se crea y se presenta dicha interfaz al usuario. En cualquier escenario, lo que te interesa es asegurarte de que el usuario pueda seguir usando la aplicación y disfrutando de ella lo antes posible. Para ello decide qué partes de la interfaz de usuario deben presentarse primero y asegúrate de que esas partes estén completas antes de dedicar tiempo a crear partes que no sean esenciales.

En la aplicación de viaje, lo más probable es que lo primero que el usuario quiera hacer sea encontrar un itinerario de viaje específico. Para presentar esta información lo más rápido posible, deberías mostrar primero la lista de viajes mediante un control **ListView**.

![diseño para el selector itinerarios en una aplicación de viajes](images/ux-app-travel-cc-a-1-180.png)

Después de mostrar la lista de viajes, podrías empezar a cargar otras funciones como, por ejemplo, una fuente de noticias acerca de los viajes de los amigos del usuario.

**¿Qué comandos y superficies de interfaz de usuario necesitas?**   Revisa los flujos que identificaste anteriormente. Para cada flujo, crea una descripción general de los pasos que deben dar los usuarios.

Examinemos el flujo "Compartir itinerarios para que los sigan amigos y familiares". Vamos a suponer que el usuario ya ha creado un viaje. Compartir un itinerario de viaje podría requerir estos pasos:

1.  El usuario abre la aplicación y ve una lista con los viajes que creó.
2.  El usuario pulsa en el viaje que quiere compartir.
3.  Los detalles del viaje aparecen en pantalla.
4.  El usuario accede a una interfaz de usuario para empezar a compartir.
5.  El usuario selecciona o escribe la dirección de correo electrónico o el nombre del amigo con el que quiere compartir el viaje.
6.  El usuario accede a una interfaz de usuario para terminar de compartir.
7.  La aplicación actualiza los detalles del viaje con la lista de personas con las que compartió el viaje.

Durante este proceso, comienzas a ver qué interfaz de usuario necesitas crear y los detalles adicionales en los que debes pensar (por ejemplo, redactar un correo electrónico estándar reutilizable para los amigos que aún no usan la aplicación). También puedes comenzar a eliminar pasos innecesarios. Quizás, el usuario en realidad no necesite ver los detalles del viaje antes de compartirlo, por ejemplo. Cuanto más claro sea el flujo, más fácil será usarlo.

Para obtener información detallada sobre cómo usar superficies distintas, consulta <!--[Command design basics](../design/basics/commanding-basics.md)-->.

**¿Cómo debería ser el flujo?** Una vez que hayas definido los pasos que seguirá el usuario, puedes convertir ese flujo en objetivos de rendimiento. Para obtener más información, consulta [Planear el rendimiento](../debug-test-perf/planning-and-measuring-performance.md).

**¿Cómo deberías organizar los comandos?** Usa el esquema de los pasos del flujo para identificar posibles comandos que necesites diseñar. A continuación, piensa dónde se utilizarán esos comandos en la aplicación.

-   **Trata siempre de usar el contenido.** Siempre que sea posible, permite que los usuarios manipulen directamente el contenido en el lienzo de la aplicación, en lugar de agregar comandos que actúan sobre el contenido. Por ejemplo, en la aplicación para viajes, permite que los usuarios reorganicen el itinerario arrastrando y colocando las actividades de una lista en el Canvas, en lugar de seleccionar la actividad y usar los botones de comandos Arriba o Abajo.
-   **Si no puedes usar el contenido.** Coloca comandos en una de estas superficies de la interfaz de usuario si no puedes usar el contenido:

    -   En la [barra de comandos](https://msdn.microsoft.com/library/windows/apps/hh465302): debes colocar la mayoría de los comandos en la barra de comandos, que generalmente permanece oculta hasta que el usuario toca la pantalla para que se muestre.
    -   En el lienzo de la aplicación: si el usuario se encuentra en una página o vista que tiene un solo propósito, puedes ofrecerle comandos para ese propósito directamente en el lienzo. Debería haber muy pocos de estos comandos.
    -   En un [menú contextual](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/menus): puedes usar los menús contextuales para acciones del Portapapeles (como cortar, copiar y pegar) o para comandos que se aplican al contenido y que no se pueden seleccionar (como agregar un alfiler de anotación en una ubicación sobre el mapa).

**Decide cómo diseñar la aplicación en cada vista.** Windows admite orientaciones horizontales y verticales permite cambiar el tamaño de las aplicaciones a cualquier ancho: desde la pantalla completa hasta un ancho mínimo. Te interesa que tu aplicación tenga un funcionamiento y un aspecto perfectos en cualquier tamaño, pantalla y orientación, para ello tendrás que planear el diseño de los elementos de la interfaz de usuario para distintos tamaños y vistas. Al hacerlo, la interfaz de usuario de la aplicación cambia de forma fluida para satisfacer las necesidades y preferencias del usuario.

![Diseños de una aplicación para PC y móvil](images/ux-budgettracker1-md-notablet.png)

Para obtener más información sobre cómo diseñar para diferentes tamaños de pantalla, consulta [tamaños de pantalla y puntos de interrupción de diseño dinámico](https://docs.microsoft.com/en-us/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design).

## <a name="make-a-good-first-impression"></a>Causa una buena primera impresión

Piensa en lo que quieres que sientan, piensen o hagan los usuarios cuando inicien por primera vez la aplicación. Vuelve a consultar el enunciado de puntos fuertes. Aunque no tengas la oportunidad de decir a los usuarios en persona para qué es excelente la aplicación, puedes transmitirles el mensaje con tu primera impresión. Aprovecha lo siguiente:

**Iconos y notificaciones**  El icono es la cara de la aplicación. Entre todas las aplicaciones que el usuario pueda tener en su pantalla Inicio, ¿qué es lo que hará que quiera iniciar la tuya? Asegúrate de que el icono resalte la marca de tu aplicación y muestre sus puntos fuertes. Usa las notificaciones de icono para que la aplicación siempre tenga una apariencia renovada y relevante, lo que hará que el usuario vuelva a ella una y otra vez.

**Pantalla de presentación**la pantalla de presentación debe cargarse lo más rápido posible y permanecer en la pantalla solo, siempre debes inicializar el estado de la aplicación. Lo que muestres en la pantalla de presentación debe expresar la personalidad de la aplicación.

**Primer inicio**antes de que los usuarios registrarse en el servicio, iniciar sesión su cuenta o agregar su propio contenido, ¿qué verán? Intenta demostrar el valor de la aplicación antes de solicitar información a los usuarios. Considera la posibilidad de ofrecer una muestra del contenido que el usuario pueda manipular para que comprenda lo que hace tu aplicación antes de pedirle que se comprometa.

**Página principal**la página principal es que verán los usuarios cada vez que inicien la aplicación. El contenido debe tener un propósito claro y mostrar de inmediato para qué sirve la aplicación. Haz que esta página sea excelente en algo en particular y confía en que las personas explorarán el resto de la aplicación. Céntrate en eliminar las distracciones de la página de destino y no en su detectabilidad.

## <a name="validate-your-design"></a>Valida tu diseño

Antes de ir demasiado lejos con el desarrollo de tu aplicación, debes validar el diseño o crear un prototipo basado en directrices, impresiones del usuario y requisitos, para evitar tener que volver a trabajar en ello más adelante. Cada función tiene un conjunto de directrices de experiencia del usuario que te ayudan a pulir la aplicación y un conjunto de requisitos de la tienda que debes cumplir para publicar la aplicación en Microsoft Store. Puedes usar el [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) para comprobar si tu aplicación cumple técnicamente con los requisitos de la Tienda. También puedes usar las herramientas de rendimiento de Microsoft Visual Studio para asegurarte de ofrecer a los usuarios una gran experiencia en todos los escenarios.

Usa las [directrices detalladas de la experiencia de usuario para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/hh465424) para mantenerte centrado en las características importantes. Usa las herramientas que encontrarás en [Visual Studio performance tools (Herramientas de rendimiento de Visual Studio)](https://msdn.microsoft.com/library/windows/apps/hh696636.aspx) para analizar el rendimiento de cada uno de los escenarios de la aplicación.
