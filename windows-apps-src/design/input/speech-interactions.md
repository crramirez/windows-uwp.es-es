---
author: Karl-Bridge-Microsoft
Description: Incorporate speech into your apps using Cortana voice commands, speech recognition, and speech synthesis.
title: Interacciones de voz
ms.assetid: 646DB3CE-FA81-4727-8C21-936C81079439
label: Speech interactions
template: detail.hbs
keywords: voz, reconocimiento de voz, lenguaje natural, dictado, entrada, interacción del usuario
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4006cdedffdbc601b498ce64caddfdefcbf4877a
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6839428"
---
# <a name="speech-interactions"></a>Interacciones de voz

Integra el reconocimiento de voz y texto a voz (también denominado TTS o síntesis de voz) directamente en la experiencia del usuario de la aplicación.

**Reconocimiento de voz** Reconocimiento de voz convierte las palabras que pronuncia el usuario en texto para entrada de formulario, para dictado de texto, para especificar una acción o un comando y para llevar a cabo tareas. Las gramáticas predefinidas para el dictado de texto libre y la búsqueda web, y las gramáticas personalizadas creadas con la versión 1.0 de la Especificación de gramática de reconocimiento de voz (SRGS) son compatibles.

**TTS** TTS usa un motor de síntesis de voz (voz) para convertir una cadena de texto en palabras habladas. La cadena de entrada puede ser texto básico y sin adornos o Lenguaje de marcado de síntesis de voz (SSML) más complejo. SSML proporciona una forma estándar de controlar características de la salida de voz, como la pronunciación, el volumen, el tono, la velocidad o el énfasis.

**Otros componentes relacionados con la voz:**
**Cortana** en las aplicaciones Windows usa comandos de voz personalizados (hablados o escritos) para lanzar la aplicación en primer plano (la aplicación toma el foco, como si se iniciara desde el menú Inicio) o activarla como un servicio en segundo plano (**Cortana** conserva el foco, pero proporciona los resultados de la aplicación). Consulta las [Directrices para el comando de voz (VCD) de Cortana](https://docs.microsoft.com/en-us/cortana/voice-commands/vcd) si expones la funcionalidad de la aplicación en la interfaz de usuario de **Cortana**.

## <a name="speech-interaction-design"></a>Diseño de la interacción mediante voz

Si se diseña y se implementa con cuidado, la voz puede ser una forma eficaz y divertida de que la gente interactúe con tu aplicación, que además complementa (llegando incluso a sustituir) al teclado, el mouse, la interacción táctil o los gestos.

En estas directrices y recomendaciones se describe cómo integrar reconocimiento de voz y TTS en la experiencia de interacción de la aplicación.

Si estás pensando en dar soporte a las interacciones de voz en la aplicación:

-   ¿Qué acciones pueden realizarse a través de la voz? ¿Puede un usuario navegar entre páginas, invocar comandos o escribir datos como campos de texto, notas breves o mensajes largos?
-   ¿La entrada de voz es una buena opción para completar una tarea?
-   ¿Cómo sabe un usuario cuándo está disponible la entrada de voz?
-   ¿Está la aplicación siempre escuchando o el usuario necesita realizar una acción para que la aplicación entre en modo de escucha?
-   ¿Qué frases inician una acción o comportamiento? ¿Las frases y las acciones deben enumerarse en pantalla?
-   ¿Son necesarias las pantallas de confirmación y desambiguación o TTS?
-   ¿Qué es el diálogo de interacción entre la aplicación y el usuario?
-   ¿Es necesario un vocabulario restringido o personalizado (por ejemplo, medicina, ciencia o configuración regional) para el contexto de la aplicación?
-   ¿Es necesaria la conectividad de red?

## <a name="text-input"></a>Entrada de texto

La voz para entrada de texto puede oscilar entre formato corto (una única palabra o frase) y formato largo (dictado continuo). La entrada de formato corto debe tener menos de 10 segundos de longitud, mientras que la sesión de entrada de formato largo puede tener un máximo de dos minutos de longitud. (La entrada de formato largo puede reiniciarse sin intervención del usuario para dar la impresión de dictado continuo).

Debes proporcionar una indicación visual para indicar que el reconocimiento de voz es compatible, que está disponible para el usuario y si el usuario necesita activarlo. Por ejemplo, un botón de la barra de comandos con un glifo de micrófono (consulta [Barras de comando](../controls-and-patterns/app-bars.md)) puede usarse para mostrar la disponibilidad y el estado.

Ofrece comentarios sobre reconocimiento continuos para reducir cualquier falta de respuesta aparente mientras se realiza el reconocimiento.

Permite que los usuarios revisen el texto de reconocimiento con la entrada de teclado, opciones de desambiguación, sugerencias o reconocimiento de voz adicionales.

Detén el reconocimiento si se detecta entrada desde un dispositivo que no sea el reconocimiento de voz, como entrada de teclado o táctil. Probablemente esto indica que el usuario se ha movido a otra tarea, como la corrección del texto de reconocimiento o la interacción con otros campos del formulario.

Especifica el intervalo de tiempo sin entrada de voz que indica que el reconocimiento ha finalizado. No reinicies automáticamente el reconocimiento después de este período de tiempo, ya que suele indicar que el usuario ha dejado de interactuar con la aplicación.

Deshabilita la interfaz de usuario de reconocimiento continuo y finaliza la sesión de reconocimiento si no hay disponible una conexión de red. El reconocimiento continuo requiere una conexión de red.

## <a name="commanding"></a>Comandos

La entrada de voz puede iniciar acciones, invocar comandos y realizar tareas.

Si el espacio lo permite, considera la posibilidad de mostrar las respuestas compatibles para el contexto actual de la aplicación, con ejemplos de entrada válidos. Esto reduce las posibles respuestas que la aplicación debe procesar y también elimina las confusión del usuario.

Intenta plantear preguntas cerradas para que den lugar a una respuesta lo más específica posible. Por ejemplo, "¿Qué quieres hacer hoy?" tiene un final muy abierto y requeriría una definición gramática muy grande debido a la gran variedad de respuestas que podría tener. Como alternativa, "¿Quieres jugar o escuchar música?" limita la respuesta a una de dos respuestas válidas con una definición gramática proporcionalmente pequeña. Una gramática pequeña es mucho más fácil de crear y da lugar a resultados de reconocimiento mucho más precisos.

Pide confirmación por parte del usuario cuando la confianza del reconocimiento de voz es baja. Si la intención del usuario no está clara, es mejor obtener una aclaración que iniciar una acción no intencionada.

Debes proporcionar una indicación visual para indicar que el reconocimiento de voz es compatible, que está disponible para el usuario y si el usuario necesita activarlo. Por ejemplo, un botón de la barra de comandos con un glifo de micrófono (consulta [Directrices para las barras de comandos](../controls-and-patterns/app-bars.md)) puede usarse para mostrar la disponibilidad y el estado.

Si el conmutador de reconocimiento de voz está generalmente visible, considera la posibilidad de mostrar un indicador de estado en el área de contenido de la aplicación.

Si el usuario inicia el reconocimiento, considera la posibilidad de usar la funcionalidad de reconocimiento integrada para mantener la coherencia. La funcionalidad integrada incluye pantallas personalizables con avisos, ejemplos, desambigüaciones, confirmaciones y errores.

Las pantallas varían en función de las restricciones especificadas:

-   Gramática predefinida (dictado o búsqueda web)

    -   La pantalla **Escucha**.
    -   La pantalla **Procesando**.
    -   La pantalla **Te he oído** o la pantalla de error.
-   Lista de palabras o frases o un archivo de gramática SRGS

    -   La pantalla **Escucha**.
    -   La pantalla **Has dicho**, si lo que el usuario ha dicho pudiera interpretarse como más de un posible resultado.
    -   La pantalla **Te he oído** o la pantalla de error.

En la pantalla **Escucha** puedes hacer lo siguiente:

-   Personalizar el texto del título.
-   Proporcionar un texto de ejemplo de lo que el usuario puede decir.
-   Especificar si se muestra la pantalla **Te he oído**.
-   Volver a leer la cadena reconocida al usuario en la pantalla **Te he oído**.

Este es un ejemplo del flujo de reconocimiento integrado para un reconocedor de voz que usa una restricción definida por SRGS. En este ejemplo, el reconocimiento de voz es correcto.

![initial reconocimiento screen for a constraint based on a sgrs grammar file](images/speech/speech-listening-initial.png)

![pantalla de reconocimiento intermedio para una restricción basada en un archivo de gramática SGRS](images/speech/speech-listening-intermediate.png)

![pantalla de reconocimiento final para una restricción basada en un archivo de gramática SGRS](images/speech/speech-listening-complete.png)

## <a name="always-listening"></a>Siempre escuchando

La aplicación puede escuchar y reconocer la entrada de voz en cuanto se inicia la aplicación, sin la intervención del usuario.

Se recomienda personalizar las restricciones de gramática basadas en el contexto de la aplicación. Esto mantiene la funcionalidad de reconocimiento de voz muy dirigida y relevante para la tarea actual y reduce los errores.

## <a name="what-can-i-say"></a>"¿Qué puedo decir?"

Cuando se habilita la entrada de voz, es importante ayudar a los usuarios a descubrir qué se puede entender exactamente y qué acciones se pueden realizar.

Si el reconocimiento de voz está habilitado por el usuario, considera la posibilidad de usar la barra de comandos o un comando de menú para mostrar todas las palabras y frases que se admiten en el contexto actual.

Si el reconocimiento de voz está siempre activado, considera la posibilidad de agregar la frase "¿Qué puedo decir?". en cada página. Cuando el usuario dice esta frase, muestra todas las palabras y frases que se admiten en el contexto actual. El uso de esta frase proporciona un modo coherente para los usuarios de descubrir las capacidades de voz en el sistema.

## <a name="recognition-failures"></a>Errores de reconocimiento

El reconocimiento de voz fallará. Los errores ocurren cuando la calidad de audio es deficiente, cuando solo se reconoce una parte de una frase o cuando no se detecta ninguna entrada.

Controla el error correctamente, ayuda al usuario a comprender el motivo del error de reconocimiento y soluciónalo.

La aplicación debe informar al usuario de que no se le comprendió y de que debe intentarlo de nuevo.

Considera la posibilidad de proporcionar ejemplos de una o más frases admitidas. Es probable que el usuario repita una frase sugerida, lo que aumenta el éxito de reconocimiento.

Debes mostrar una lista de posibles coincidencias para que el usuario pueda seleccionar una de entre las mismas. Esto puede ser mucho más eficiente que pasar por el proceso de reconocimiento de nuevo.

Siempre debes admitir tipos de entrada alternativos, lo que es especialmente útil para controlar los errores de reconocimiento repetidos. Por ejemplo, podrías sugerir que el usuario intente usar un teclado, o la entrada táctil o un mouse para seleccionar de una lista de posibles coincidencias.

Usa la funcionalidad de reconocimiento de voz integrada, ya que incluye pantallas que indican al usuario que el reconocimiento no fue correcto y le permiten volver a intentar el reconocimiento de nuevo.

Escucha e intenta subsanar los problemas en las entradas de audio. El reconocedor de voz puede detectar problemas con la calidad de audio que podrían afectar negativamente a la precisión del reconocimiento de voz. Puedes usar la información proporcionada por el reconocedor de voz para informar al usuario del problema y permitirle tomar medidas correctivas, si es posible. Por ejemplo, si la configuración de volumen del micrófono es demasiado baja, puedes pedir al usuario que hable más alto o que suba el volumen.

## <a name="constraints"></a>Restricciones

Las restricciones, o las gramáticas, definen las palabras y frases que el reconocedor de voz puede hallar. Puedes especificar una de las gramáticas de servicio web predefinidas o puedes crear una gramática personalizada que se instala con la aplicación.

### <a name="predefined-grammars"></a>Gramáticas predefinidas

Las gramáticas predefinidas de dictado y búsqueda en Internet proporcionan a tu aplicación la funcionalidad de reconocimiento de voz sin necesidad de crear una gramática. Al usar estas gramáticas, un servicio web remoto se encarga de llevar a cabo el reconocimiento de voz y los resultados se devuelven al dispositivo.

-   La gramática predeterminada de dictado de texto libre tiene la capacidad de reconocer la mayoría de las palabras y frases que un usuario puede decir en un idioma en particular y está optimizada para reconocer frases cortas. El dictado de texto libre es útil si no quieres limitar los tipos de términos que puede decir un usuario. Entre los usos típicos se incluyen la creación de notas o el dictado del contenido de un mensaje.
-   La gramática de búsqueda web, como una gramática de dictado, contiene un gran número de palabras y frases que puede decir un usuario. Sin embargo, está optimizada para reconocer los términos que suelen usar las personas cuando buscan en la web.

> [!NOTE]
> Debido a que las gramáticas predefinidas de dictado y búsqueda en Internet pueden ser grandes y se accede a ellas a través de Internet (no se encuentran en el dispositivo), su rendimiento puede no ser tan rápido como el de una gramática personalizada instalada en el dispositivo.

Estas gramáticas predefinidas pueden usarse para reconocer hasta 10 segundos de entrada de voz y no requieren ningún esfuerzo de edición por su parte. Sin embargo, sí requieren una conexión a una red.

### <a name="custom-grammars"></a>Gramáticas personalizadas

Una gramática personalizada se ha diseñado y creado por el usuario y se instala con la aplicación. El reconocimiento de voz con una restricción personalizada se realiza en el dispositivo.

-   Las restricciones de lista mediante programación ofrecen un enfoque ligero para la creación de gramáticas sencillas como, por ejemplo, una lista de palabras o frases. Una restricción de lista es efectiva para reconocer frases cortas y distintas. Especificar explícitamente todas las palabras en una gramática también mejora la precisión del reconocimiento, porque el motor de reconocimiento de voz debe procesar la voz únicamente para confirmar una coincidencia. La lista también se puede actualizar mediante programación.
-   Una gramáticaSRGS es un documento estático que, a diferencia de una restricción de lista mediante programación, usa el formato XML definido por [SRGS versión 1.0](http://go.microsoft.com/fwlink/p/?LinkID=262302). Una gramática SRGS proporciona el máximo control sobre la funcionalidad de reconocimiento de voz al permitir capturar varios significados semánticos en un solo reconocimiento.

    Estas son algunas sugerencias para crear gramáticas SRGS:

    -   Reduce cada gramática al mínimo. Las gramáticas que contienen pocas frases suelen proporcionar un reconocimiento más preciso que las gramáticas más extensas compuestas por más frases. Es preferible tener varias gramáticas escuetas para escenarios específicos que tener una sola gramática para toda la aplicación.
    -   Permite que los usuarios sepan qué decir para cada contexto de la aplicación y habilitar y deshabilitar gramáticas según sea necesario.
    -   Diseña cada gramática para que los usuarios puedan decir un comando de diferentes formas. Por ejemplo, puedes usar la regla **GARBAGE** para que coincida con la entrada de voz que la gramática no define. Esto permite a los usuarios decir más palabras que no tienen sentido para la aplicación. Por ejemplo, "dame", "y", "uh", "quizás", etc.
    -   Usa el elemento [sapi:subset](http://msdn.microsoft.com/library/windowsphone/design/jj572474.aspx) como ayuda para encontrar entradas de voz. Se trata de una extensión de Microsoft a la especificación de SRGS para ayudar a encontrar frases parciales.
    -   Intenta no definir frases en la gramática que contengan una sola sílaba. El reconocimiento tiende a ser más exacto con frases con dos o más sílabas.
    -   Evita usar frases que suenen parecido. Por ejemplo, frases como "hola", "cola" y "bola" pueden confundir al motor de reconocimiento y la precisión del reconocimiento puede no ser buena.

> [!NOTE]
> El tipo de restricción que uses depende de la complejidad de la funcionalidad de reconocimiento que desees crear. Cualquier enfoque puede ser la mejor opción para una tarea de reconocimiento determinada y puedes encontrar usos para todos los tipos de restricción en tu aplicación.

### <a name="custom-pronunciations"></a>Pronunciaciones personalizadas

Si la aplicación contiene vocabulario especializado con palabras inusuales o ficticias, o palabras con pronunciaciones poco comunes, mejorarás el rendimiento del reconocimiento de esas palabras si defines pronunciaciones personalizadas.

Para una pequeña lista de palabras y frases, o una lista de palabras o frases poco usadas, puedes crear pronunciaciones personalizadas en una gramática SRGS. Consulta [Elemento token](http://msdn.microsoft.com/library/windowsphone/design/hh361600.aspx) para obtener más información.

En el caso de listas de palabras y frases más largas, o palabras o frases usadas con frecuencia, puedes crear documentos de lexicón de pronunciación independiente. Consulta [Acerca de los lexicones y los alfabetos fonéticos](http://msdn.microsoft.com/library/windowsphone/design/hh361646.aspx) para obtener más información.

## <a name="testing"></a>Pruebas

Prueba la precisión del reconocimiento de voz y la interfaz de usuario compatible con el público objetivo de la aplicación. Esta es la mejor manera de determinar la eficacia de la funcionalidad de interacción de voz en la aplicación. Por ejemplo, ¿los usuarios obtienen resultados de reconocimiento inexactos porque la aplicación no puede escuchar una frase común?

Modifica la gramática para admitir esta frase o proporciona a los usuarios una lista de frases admitidas. Si ya proporcionó la lista de frases admitidas, asegúrate de que sea fácilmente detectable.

## <a name="text-to-speech-tts"></a>Texto a voz (TTS)

TTS genera salidas de voz a partir de texto sin formato o SSML.

Intenta diseñar mensajes educados y alentadores.

Ten en cuenta si debes leer cadenas largas de texto. Una cosa es escuchar un mensaje de texto, pero otra bastante diferente es escuchar una lista larga de resultados de búsqueda difíciles de recordar.

Debes proporcionar controles de medios para permitir a los usuarios pausar o detener TTS.

Debes escuchar todas las cadenas de TTS para garantizar que son inteligibles y suenan natural.

-   Encadenar una secuencia de palabras inusuales o dictar números o signos de puntuación puede provocar que una frase sea ininteligible.
-   La voz puede sonar forzada cuando la prosodia o cadencia es diferente a cómo un hablante nativo diría una frase.

Estos dos tipos de problema se pueden abordar usando SSML en lugar de texto sin formato como entrada en el sintetizador de voz. Para obtener más información sobre SSML, consulta [Usar SSML para controlar la voz sintetizada](http://msdn.microsoft.com/library/windowsphone/design/hh378454.aspx) y [Referencia de Lenguaje de marcado de síntesis de voz](http://msdn.microsoft.com/library/windowsphone/design/hh378377.aspx).

## <a name="other-articles-in-this-section"></a>Otros artículos de esta sección 

| Tema | Descripción |
| --- | --- |
| [Reconocimiento de voz](speech-recognition.md) | Usa el reconocimiento de voz para proporcionar datos de entrada, especificar una acción o un comando y realizar tareas. |
| [Especificar el idioma del reconocedor de voz](specify-the-speech-recognizer-language.md) | Obtén información sobre cómo seleccionar un idioma instalado para usarlo en el reconocimiento de voz. |
| [Definir restricciones de reconocimiento personalizadas](define-custom-recognition-constraints.md) | Aprende a definir y usar restricciones personalizadas para el reconocimiento de voz. |
| [Habilitar el dictado continuo](enable-continuous-dictation.md) |Obtén información sobre cómo capturar y reconocer la entrada de voz de dictado continuo de larga duración. |
| [Administrar los problemas con la entrada de audio](manage-issues-with-audio-input.md) | Aprende a administrar los problemas con la precisión del reconocimiento de voz causados por la calidad de la entrada de audio. |
| [Establecer tiempos de espera de reconocimiento de voz](set-speech-recognition-timeouts.md) | Establece durante cuánto tiempo un reconocedor de voz pasa por alto el silencio o los sonidos irreconocibles (balbuceo) y continúa escuchando la entrada de voz. |

## <a name="related-articles"></a>Artículos relacionados

* [Interacciones de voz](https://msdn.microsoft.com/library/windows/apps/mt185614)
* [Interacciones de Cortana](https://msdn.microsoft.com/library/windows/apps/mt185598)

 **Ejemplos**

* [Muestra de reconocimiento de voz y síntesis de voz](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 



