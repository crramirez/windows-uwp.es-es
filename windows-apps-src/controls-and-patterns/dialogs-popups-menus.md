---
Description: Un control flotante es un elemento emergente ligero que se usa para mostrar temporalmente opciones de la interfaz de usuario relacionadas con lo que el usuario esté haciendo en ese momento.
title: Menús contextuales y cuadros de diálogo
ms.assetid: 7CA2600C-A1DB-46AE-8F72-24C25E224417
label: Menus, dialogs, and popups
template: detail.hbs
---
# Menús, cuadros de diálogo y elementos emergentes

Los menús, cuadros de diálogo y elementos emergentes mostrarán los elementos de interfaz de usuario transitorios que aparecen cuando el usuario los solicita o cuando sucede algo que requiere una notificación o aprobación. 

<span class="sidebar_heading" style="font-weight: bold;">API importantes</span>

-   [Clase MenuFlyout](https://msdn.microsoft.com/library/windows/apps/dn299030)
-   [Clase Flyout](https://msdn.microsoft.com/library/windows/apps/dn279496)

Un menú contextual proporciona al usuario acciones instantáneas. Se puede rellenar con comandos personalizados. Para descartar los menús contextuales, puedes tocar o hacer clic en algún lugar fuera del menú.

Los cuadros de diálogo son superposiciones modales en la interfaz de usuario que proporcionan información contextual sobre la aplicación. En la mayoría de casos, los cuadros de diálogo bloquean las interacciones con la ventana de la aplicación hasta que se descartan de forma explícita y a menudo solicitan algún tipo de acción por parte del usuario.

Un elemento emergente, también conocido como control flotante, es un elemento emergente contextual ligero que muestra la interfaz de usuario relacionada con lo que está haciendo el usuario. Incluye lógica de colocación y tamaño, y se puede usar para mostrar un menú, revelar un control oculto, mostrar más detalles sobre un elemento o pedirle al usuario que confirme una acción. Los elementos emergentes se pueden descartar tocando o haciendo clic en algún lugar fuera del elemento.


## ¿Es este el control adecuado?

Los menús contextuales se puede usar para:

-   Acciones contextuales en selecciones de texto, como Copiar, Cortar, Pegar, Revisar la ortografía, etc.
-   Comandos del Portapapeles.
-   Comandos personalizados.
-   Comandos para un objeto sobre el que se debe actuar, pero que no se puede seleccionar ni indicar de otra forma.

Los cuadros de diálogo se pueden usar para:

- Expresar información importante que el usuario debe leer y confirmar antes de continuar.
- Solicitar una acción clara del usuario o comunicar un mensaje importante que el usuario debe confirmar. Algunos ejemplos son:
  - Los casos en que se puede ver comprometida la seguridad del usuario
  - Los casos en que el usuario está a punto de modificar un activo valioso de manera permanente
  - Los casos en que el usuario está a punto de eliminar un activo valioso
  - Para confirmar una compra desde la aplicación
- Mensajes de error que se aplican al contexto general de la aplicación, como un error de conectividad.
- Preguntas cuando la aplicación deba hacer al usuario una pregunta de bloqueo, por ejemplo, cuando la aplicación no puede elegir en nombre del usuario. Una pregunta de bloqueo no se puede ignorar ni posponer, y debe ofrecer al usuario opciones bien definidas.

Los elementos emergentes (controles flotantes) se pueden usar para:

-   interfaces de usuario contextuales y transitorias;
-   advertencias y confirmaciones, incluidas las que están relacionadas con acciones potencialmente destructivas;
-   mostrar más información, como los detalles sobre un elemento en la pantalla;
-   menús de segundo nivel;
-   comandos personalizados.


## Ejemplos

A continuación se muestra un menú contextual de un solo panel. Se usaría para obtener una lista más corta de comandos sencillos. Usa separadores según sea necesario para agrupar comandos similares.

![Ejemplo de un menú contextual típico](images/controls_contextmenu_singlepane.png)

Un menú contextual en cascada se usaría para una colección más completa de comandos. Cuenta con un nivel de control flotante y se puede desplazar. Usa separadores según sea necesario para agrupar comandos similares.

![Ejemplo de un menú contextual en cascada](images/controls_contextmenu_cascading.png)

Este es un ejemplo de un cuadro de diálogo de confirmación con solo botón y en pantalla completa. Con este tipo de cuadro de diálogo, se proporciona al usuario una gran cantidad de información que esperaba leer antes de presionar el botón para continuar.

![Ejemplo de un cuadro de diálogo de pantalla completa con un solo botón](images/controls_dialog_singlebutton.png)

Este es un ejemplo de un cuadro de diálogo de dos botones que se presenta al usuario la opción A/B. Por lo general, la cantidad de información que se presenta en este cuadro de diálogo es breve.

![Ejemplo de un cuadro de diálogo con todos los botones](images/controls_dialog_twobutton.png)


## Guía de uso

**Menús contextuales**
- Usa un separador entre grupos de comandos en un menú contextual para:
  - Distinguir entre grupos de comandos relacionados.
  - Agrupar los conjuntos de comandos.
  - Dividir un conjunto de comandos predecibles, como comandos del Portapapeles (Cortar/Copiar/Pegar) de comandos específicos de la aplicación o de una vista.
-   En los equipos portátiles y de escritorio, los menús contextuales y las informaciones sobre herramientas no están limitados a la ventana de la aplicación y pueden pintar fuera de ella. Si la aplicación intenta representar un menú contextual completamente fuera de su ventana, se generará una excepción.

**Cuadros de diálogo**
-   Identifica claramente el problema o el objetivo del usuario en la primera línea del texto del cuadro de diálogo.
-   El título del cuadro de diálogo es la instrucción principal y es opcional.
    -   Usa un título corto para explicar lo que se necesita hacer con el cuadro de diálogo. Los títulos largos no se ajustan y se truncan.
    -   Puedes omitir el título si vas a usar el cuadro de diálogo para transmitir un mensaje sencillo, un error o una pregunta. Deja que el texto del contenido transmita esta información esencial.
    -   Asegúrate de que el título esté relacionado directamente con las opciones de botón.
-   El contenido del cuadro de diálogo contiene el texto descriptivo y es obligatorio.
    -   Presenta el mensaje, el error o la pregunta de bloqueo de la manera más sencilla posible.
    -   Si se usa un cuadro de diálogo, usa el área de contenido para proporcionar más detalles o definir terminología. No repitas el título con otras palabras ligeramente distintas.
-   Al menos debe aparecer un botón de cuadro de diálogo.
    -   Usa botones con texto que identifique respuestas específicas a la instrucción principal o el contenido. Un ejemplo sería: "¿Quieres permitir que nombreDeAplicación acceda a tu ubicación?", seguido de los botones "Permitir" y "Bloquear". Cuando las respuestas son específicas, se comprenden con mayor rapidez y la toma de decisiones resulta eficaz.
-   Los cuadros de diálogo de error muestran el mensaje de error en el cuadro de diálogo, junto con la información pertinente. El único botón que se usa en un cuadro de diálogo de error debe ser "Cerrar" o una acción similar.
-   No uses cuadros de diálogo en el caso de los errores que son contextuales para un lugar específico de la página, como los errores de validación (en los campos de contraseña, por ejemplo); usa el lienzo de la aplicación para mostrar errores en línea.

**Elementos emergentes**

-   Al igual que con otras interfaces de usuario contextuales, coloca un elemento emergente junto al punto desde el que se llama.
    -   Especifica el objeto al que quieres anclar el elemento emergente y el lado del objeto en el que se mostrará el elemento emergente.
    -   Prueba a colocar el elemento emergente para que no bloquee elementos importantes de la interfaz de usuario.
-   El elemento emergente se debería descartar en cuanto se selecciona una parte de él.
-   Los menús de elemento emergente funcionan mejor con un solo nivel. Los menús de elemento emergente con varios niveles son de difícil navegación y proporcionan una experiencia del usuario deficiente.

## Cosas que hacer y cosas que evitar

-   Mantén los comandos del menú contextual cortos. Los comandos más largos acaban por truncarse.
-   Usa mayúscula inicial para todos los nombres de comandos.
-   En cualquier menú contextual, muestra el menor número posible de comandos.
-   Si la manipulación directa de un elemento de interfaz de usuario es posible, evita colocar ese comando dentro de un menú contextual. Un menú contextual debe reservarse para los comandos contextuales que, de lo contrario, no son reconocibles en pantalla.



## Artículos relacionados

**Para desarrolladores**
- [**Clase MenuFlyout**](https://msdn.microsoft.com/library/windows/apps/dn299030)
- [**Clase Flyout**](https://msdn.microsoft.com/library/windows/apps/dn279496)


<!--HONumber=Mar16_HO4-->


