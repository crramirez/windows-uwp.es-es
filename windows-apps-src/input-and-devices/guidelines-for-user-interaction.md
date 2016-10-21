---
author: Karl-Bridge-Microsoft
Description: "Crea aplicaciones para la Plataforma universal de Windows (UWP) con experiencias de interacción del usuario intuitivas y distintivas que estén optimizadas para entrada táctil, pero que sean funcionalmente coherentes entre los distintos dispositivos de entrada."
title: "Directrices para el diseño de la función táctil"
ms.assetid: 3250F729-4FDD-4AD4-B856-B8BA575C3375
label: Touch design guidelines
template: detail.hbs
redirect_url: https://msdn.microsoft.com/windows/uwp/input-and-devices/touch-interactions
translationtype: Human Translation
ms.sourcegitcommit: 2db7aaccfd56b1bdfda099b197a695bad8a9cba1
ms.openlocfilehash: 28dfadf6010aed3fb2ed0d03b73f92631c17fcf4

---

# Directrices para el diseño de la función táctil





Crea aplicaciones para la Plataforma universal de Windows (UWP) con experiencias de interacción del usuario intuitivas y distintivas que estén optimizadas para entrada táctil, pero que sean funcionalmente coherentes entre los distintos dispositivos de entrada.

## <span id="Dos_and_don_ts"></span><span id="dos_and_don_ts"></span><span id="DOS_AND_DON_TS"></span>Qué hacer y qué no hacer


-   Diseña aplicaciones en las que la interacción táctil sea el principal método de entrada que se espera.
-   Incluye información visual sobre todos los tipos de interacción (táctil, lápiz, pluma, mouse, etc.).
-   Optimiza la selección de destinos ajustando el tamaño, la geometría de contacto, la limpieza y la inclinación del destino táctil.
-   Optimiza la precisión usando puntos de acoplamiento y "guías" direccionales.
-   Incorpora información sobre herramientas y controladores para mejorar la precisión táctil de los elementos de UI con espacio muy reducido.
-   No uses interacciones temporales en la medida de lo posible (ejemplo de uso correcto: mantener pulsado).
-   No uses la cantidad de dedos para distinguir la manipulación en la medida de lo posible.

## <span id="Additional_usage_guidance"></span><span id="additional_usage_guidance"></span><span id="ADDITIONAL_USAGE_GUIDANCE"></span>Instrucciones de uso adicionales


Ante todo, diseña tu aplicación teniendo en mente que el principal método de entrada de los usuarios será táctil. Si usas los controles de la plataforma, la compatibilidad con el mouse, el panel táctil y el lápiz o la pluma no requiere programación adicional, ya que Windows8 la ofrece de forma gratuita.

Pero ten presente que una interfaz de usuario optimizada para entrada táctil no es siempre mejor que una interfaz de usuario tradicional. Ambas ofrecen ventajas y desventajas exclusivas de la tecnología y la aplicación en cuestión. A la hora de diseñar una interfaz de usuario básicamente táctil, es importante entender las diferencias fundamentales entre entrada táctil (incluido el panel táctil), con pluma o lápiz, con mouse y con el teclado. No des por sentado las propiedades y los comportamientos de los dispositivos de entrada conocidos, ya que, en Windows 8, la entrada táctil no se limita a simular esas funciones.

Al leer estas directrices, descubrirás que la entrada táctil hace necesario un enfoque diferente del diseño de la interfaz de usuario.

**Compara los requisitos de la interacción táctil**

En la tabla siguiente, se muestran algunas de las diferencias entre los dispositivos de entrada que debes tener en cuenta al diseñar aplicaciones de la Tienda Windows optimizadas para entrada táctil.

Factor Interacciones táctiles Interacciones con mouse, teclado, pluma/lápiz Panel táctil Precisión El área de contacto de la punta de un dedo es mayor que la de una sola coordenada x-y, lo que aumenta las probabilidades de activación no intencional de comandos.
El mouse y la pluma/lápiz suministran una coordenada x-y precisa.
Es igual al mouse.
La forma del área de contacto cambia durante el movimiento.
Los movimientos de mouse y los trazos de la pluma/lápiz suministran coordenadas x-y precisas. El foco del teclado es explícito.
Es igual al mouse.
No hay cursor de mouse para ayudar con la selección del destino.
El cursor del mouse, el cursor de la pluma/lápiz y el foco del teclado ayudan a seleccionar el destino.
Es igual al mouse.
Anatomía humana Los movimientos de los dedos no son precisos, ya que es difícil realizar un movimiento en línea recta con uno o más dedos. Esto se debe a la curvatura de las articulaciones de la mano y a la cantidad de articulaciones involucradas en el movimiento.
Es más fácil ejecutar un movimiento en línea recta con mouse o pluma/lápiz porque la mano que los controla recorre una distancia física menor que el cursor en la pantalla.
Es igual al mouse.
Puede ser difícil llegar a algunas áreas de la superficie táctil de un dispositivo de pantalla debido a la posición de los dedos y la sujeción del dispositivo por parte del usuario.
El mouse y la pluma o el lápiz pueden llegar a cualquier parte de la pantalla. El teclado permite acceder a cualquier control con el orden de tabulación.
La posición de los dedos y la empuñadura pueden ser un problema.
Puede ocurrir que los objetos queden ocultos por la punta de uno o más dedos o la mano del usuario. Esto se conoce como oclusión.
Los dispositivos de entrada indirecta no provocan oclusión.
Es igual al mouse.
Estado del objeto La interacción táctil emplea un modelo de dos estados: la superficie táctil de un dispositivo de pantalla se toca (activado) o no se toca (desactivado). No existe un estado de movimiento que pueda desencadenar una respuesta visual adicional.
El mouse, la pluma/lápiz y el teclado exponen un modelo de tres estados: arriba (desactivado), abajo (activado) y movimiento (foco).

Al mantener el puntero sobre un elemento, permite que el usuario explore y aprenda mediante informaciones sobre herramientas asociadas con elementos de la interfaz de usuario. Los efectos de foco y mantener el puntero pueden transmitir qué objetos son interactivos y también ayudar a seleccionar el destino.

Es igual al mouse.
Interacción enriquecida Admite multitoque: múltiples puntos de entrada (puntas de los dedos) en una superficie táctil.
Admite un punto único de entrada.
Es igual a la entrada táctil.
Admite manipulación directa de objetos por medio de gestos como pulsar, arrastrar, deslizar, reducir y girar.
No admite manipulación directa porque el mouse, la pluma o el lápiz y el teclado son dispositivos de entrada indirecta.
Es igual al mouse.
 

**Nota**  
La entrada indirecta tiene la ventaja de más de 25 años de perfeccionamiento. Algunas funciones, como la información sobre herramientas desencadenada al mantener el mouse sobre un elemento, se diseñaron para explorar la interfaz de usuario específicamente con entrada de panel táctil, mouse, pluma o lápiz y teclado. Funciones de UI como esta se han rediseñado para lograr la experiencia completa que reporta la entrada táctil, sin poner en riesgo la experiencia de usuario de estos otros dispositivos.

 

**Usar la información táctil**

La información visual adecuada durante las interacciones con la aplicación ayuda al usuario a reconocer y aprender cómo interpreta la aplicación y Windows 8 sus interacciones, y a adaptarse a esa interpretación. La información visual puede indicar interacciones satisfactorias y el estado del sistema relé, mejorar la sensación de control, reducir errores, ayudar a los usuarios a entender el sistema y el dispositivo de entrada, y alentar la interacción.

La información visual es esencial cuando el usuario usa la entrada táctil para llevar a cabo actividades que requieren exactitud y precisión en lo que respecta a ubicación. Muestra información siempre que se detecte entrada táctil para ayudar al usuario a entender cualquier regla personalizada de selección de destinos que defina la aplicación y los controles correspondientes.

**Crea una experiencia de interacción envolvente**

Las técnicas siguientes mejoran la experiencia envolvente de las aplicaciones de la Tienda Windows.

**Selección de destinos**

La selección de destinos se optimiza mediante:

-   Tamaños de destinos táctiles

    Unas directrices claras sobre el tamaño garantizan que las aplicaciones ofrecerán una interfaz de usuario cómoda que contenga objetos y controles cuyos destinos sean fáciles y seguros.

-   Geometría de contacto

    Toda el área de contacto del dedo determina el objeto de destino más probable.

-   Arrastrar

    La selección de destinos de los elementos dentro de un grupo se puede realizar fácilmente si se arrastra el dedo entre ellos (por ejemplo, los botones de radio). El elemento actual se activa cuando se libera el contacto táctil.

-   Inclinación de un lado a otro

    Los elementos que se encuentra muy cerca unos de otros (por ejemplo, hipervínculos) se pueden seleccionar de nuevo presionando el dedo e inclinándolo de un lado a otro sobre los elementos, sin deslizarlo. Debido a la oclusión, el elemento actual se identifica mediante la información sobre herramientas o la barra de estado y se activa al liberar el contacto táctil.

**Precisión**

Diseña teniendo en cuenta las interacciones descuidadas mediante:

-   Puntos de acoplamiento que permitan detenerse en las ubicaciones deseadas con mayor facilidad cuando los usuarios interactúan con el contenido.
-   "Guías" direccionales que ayuden con el movimiento panorámico vertical y horizontal, incluso cuando la mano se mueve ligeramente en arco. Si deseas obtener más información, consulta las [directrices para movimiento panorámico](guidelines-for-panning.md).

**Oclusión**

La oclusión de dedos y manos se evita mediante:

-   El tamaño y la posición de la interfaz de usuario

    Crea los elementos de la interfaz de usuario lo suficientemente grandes, para que no se cubran por completo con el área de contacto del dedo.

    Coloca los menús y las ventanas emergentes sobre el área de contacto, siempre que sea posible.

-   Información de herramientas

    Muestra información sobre herramientas cuando un usuario mantiene el contacto sobre un objeto con el dedo. Sirve para describir las funciones de los objetos. El usuario puede retirar el dedo del objeto para evitar que se invoque la información sobre herramientas.

    Para objetos pequeños, desplaza la información de herramientas para que no quede cubierta por el área de contacto del dedo. Esto resulta útil para la selección de destinos.

-   Controladores para mayor precisión

    Cuando necesites una mayor precisión (por ejemplo, para la selección de texto), ofrece controladores de selección que se desplacen para mejorar la precisión. Para obtener más información, consulta las [directrices para seleccionar texto e imágenes (aplicaciones de Windows Runtime)](guidelines-for-textselection.md).

**Intervalos**

Evita los cambios de modo cronometrado en favor de la manipulación directa. Esta simula el control físico, directo y en tiempo real de un objeto. El objeto responde a medida que se mueven los dedos.

Una interacción temporal, en cambio, tiene lugar después de la interacción táctil. Para determinar qué comando se ejecutará, las interacciones temporales suelen depender de umbrales invisibles, como el tiempo, la distancia o la velocidad. Las interacciones temporales no ofrecen una respuesta visual hasta que el sistema ejecuta la acción.

La manipulación directa ofrece una serie de ventajas sobre las interacciones temporales:

-   La información visual instantánea durante las interacciones hace que los usuarios se sientan más comprometidos, seguros de si mismos y con el control.
-   Las manipulaciones directas hacen que sea más seguro explorar un sistema porque son reversibles: los usuarios pueden deshacer fácilmente sus acciones de manera lógica e intuitiva.
-   Las interacciones que afectan directamente a los objetos y mimetizan las interacciones de la vida real son más intuitivas y fáciles de detectar y recordar. No dependen de interacciones oscuras o abstractas.
-   Las interacciones temporales pueden ser difíciles de realizar, ya que los usuarios deben alcanzar umbrales arbitrarios e invisibles.

Además, te recomendamos lo siguiente:

-   Las manipulaciones no deben distinguirse por el número de dedos usados.
-   Las interacciones deben admitir manipulaciones compuestas. Por ejemplo, alejar para ampliar mientras se arrastran los dedos para el movimiento panorámico.
-   Las interacciones no deben distinguirse temporalmente. La misma interacción debe tener el mismo resultado, independientemente del tiempo que se haya tardado en realizarla. Las activaciones temporales introducen retrasos obligatorios para los usuarios y reducen la naturaleza envolvente de la manipulación directa, así como la percepción de la respuesta del sistema.

    **Nota** Una excepción a esto se produce cuando se usan interacciones temporales específicas para favorecer el aprendizaje y la exploración (por ejemplo, mantener presionado).

     

-   Las descripciones y las indicaciones visuales adecuadas tienen un gran efecto sobre el uso de las interacciones avanzadas.

## <span id="related_topics"></span>Artículos relacionados

**Para desarrolladores (XAML)**
* [Interacciones táctiles](https://msdn.microsoft.com/library/windows/apps/mt185617)
* [Interacciones del usuario personalizadas](https://msdn.microsoft.com/library/windows/apps/mt185599)
 

 







<!--HONumber=Sep16_HO3-->


